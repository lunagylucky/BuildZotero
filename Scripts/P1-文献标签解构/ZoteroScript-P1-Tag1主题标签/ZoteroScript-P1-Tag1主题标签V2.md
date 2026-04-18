---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag1主题标签V2
DateCreated: 2026-01-17 17:37
DateModified: 2026-04-18 17:38
Type:
  - doc
Status:
  - doing
Version: v1.0
CardStatus: false
CardType:
  - card-fleeting
tags:
  - Topic/工具技能/工作笔记
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P1-Tag1 主题标签 V2

### 完整代码

```javascript
#Item Tags Abstract[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";

  let successCount = 0;
  let skipCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;

  for (let item of items) {
    try {
      // 跳过已处理的条目
      const existingTags = item.getTags();
      if (existingTags.some(tag => tag.tag && tag.tag.startsWith("Item/"))) {
        skipCount++;
        continue;
      }

      let content = "";

      // 内容获取 - 优先摘要，备选PDF
      try {
        const abstract = item.getField("abstractNote");
        if (abstract && abstract.trim().length > 50) {
          content = abstract.trim();
        } else {
          try {
            const pdfItem = await item.getBestAttachment();
            if (pdfItem && pdfItem.isPDFAttachment()) {
              const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
              if (fullTextData && fullTextData.text) {
                // 取前3000字符，包含更多主题信息
                content = fullTextData.text.substring(0, 3000).trim();
              }
            }
          } catch {}
        }
      } catch {}

      if (!content) {
        // 无内容，标注便于识别
        try {
          item.addTag('Item/无摘要');
          await item.saveTx();
          noContentCount++;
        } catch {
          errorCount++;
        }
        continue;
      }

      // AI分析生成主题标签
      try {
        const prompt = `分析以下学术文献内容，生成3个最能代表其核心内容的标签：

${content}

要求：
1. 标签应简洁精确，3-6个字符
2. 使用简体中文
3. 重点关注：核心变量、研究主题、研究结论、核心概念
4. 避免研究方法、理论名称、研究对象（这些有单独标签）
5. 聚焦文献的核心贡献和发现
6. 避免过于宽泛的词汇，要具体明确
7. 返回JSON数组格式

请直接返回JSON数组，格式：["标签1", "标签2", "标签3"]`;

        const response = await Meet.Global.views.ask(prompt);

        // 解析AI响应
        try {
          const jsonMatch = response.match(/\[[\s\S]*?\]/);
          if (!jsonMatch) {
            failCount++;
            continue;
          }

          const tags = JSON.parse(jsonMatch[0]);
          if (!tags || tags.length === 0) {
            failCount++;
            continue;
          }

          // 验证和清理标签
          const validTags = tags
            .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
            .map(tag => tag.trim())
            .slice(0, 3); // 确保最多3个标签

          if (validTags.length === 0) {
            failCount++;
            continue;
          }

          // 添加标签
          for (let tag of validTags) {
            item.addTag('Item/' + tag);
          }

          try {
            await item.saveTx();
            successCount++;
          } catch {
            errorCount++;
          }

        } catch {
          // JSON解析失败
          failCount++;
        }

      } catch {
        // AI调用失败
        failCount++;
      }

    } catch {
      errorCount++;
    }
  }

  return `主题标签处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
})()
}$
```


### 模块 1：初始化和统计变量定义

```javascript
const items = ZoteroPane.getSelectedItems();
if (!items || items.length === 0) return "未选择任何条目";

let successCount = 0;      // 成功添加主题标签的条目
let skipCount = 0;         // 已有Item标签被跳过的条目
let noContentCount = 0;    // 无摘要也无PDF的条目
let failCount = 0;         // AI调用或解析失败的条目
let errorCount = 0;        // 标签保存失败的条目
```

**功能说明：**

- 获取用户在 Zotero 中选中的文献条目
- 验证选中条目的有效性，确保有可处理的文献
- 初始化五种专门针对主题标签的统计计数器

**统计变量的特殊意义：**

- `successCount`: 核心成果 - 成功提取核心变量、研究主题、研究结论的条目数
- `skipCount`: 效率指标 - 已有 Item/标签被跳过的条目数
- `noContentCount`: 内容问题 - 需要补充内容以提取主题信息的条目数
- `failCount`: 技术问题 - AI 无法识别核心主题内容的条目数
- `errorCount`: 系统问题 - 标签保存失败的条目数

---


### 模块 2：重复处理检查与 Item/命名空间

```javascript
for (let item of items) {
  try {
    // 跳过已处理的条目
    const existingTags = item.getTags();
    if (existingTags.some(tag => tag.tag && tag.tag.startsWith("Item/"))) {
      skipCount++;
      continue;
    }
  } catch {
    errorCount++;
  }
}
```

**Item/命名空间的设计逻辑：**

- **专门标识**: Item/专门用于标识文献的核心主题内容
- **功能定位**: 与其他标签体系形成清晰分工
- **避重机制**: 检查 Item/前缀，避免重复处理主题标签

**标签体系的功能分工：**

```
Theory/     - 理论框架标签
Method/     - 研究方法标签  
Sample/     - 研究对象标签
Item/       - 核心主题标签（核心变量、主题、结论）
```

---


### 模块 3：主题导向的内容获取策略

```javascript
let content = "";

try {
  const abstract = item.getField("abstractNote");
  if (abstract && abstract.trim().length > 50) {
    content = abstract.trim();
  } else {
    try {
      const pdfItem = await item.getBestAttachment();
      if (pdfItem && pdfItem.isPDFAttachment()) {
        const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
        if (fullTextData && fullTextData.text) {
          // 取前3000字符，包含更多主题信息
          content = fullTextData.text.substring(0, 3000).trim();
        }
      }
    } catch {}
  }
} catch {}
```

**主题标签的内容获取特点：**

#### 第一优先级：摘要内容
- **核心信息集中**: 摘要包含研究的核心变量、主要发现、关键结论
- **主题密度高**: 摘要是作者对研究核心内容的高度浓缩
- **质量控制**: 50 字符阈值确保有足够信息进行主题提取


#### 第二优先级：PDF 前 3000 字符
- **覆盖关键部分**: 包含摘要、引言、部分结果和结论
- **主题信息丰富**: 引言阐述研究问题，结论部分包含核心发现
- **平衡设计**: 3000 字符既能获取丰富主题信息，又不过度增加 AI 处理负担

**主题标签与其他标签的内容需求差异：**

```
Theory/标签: 需要识别明确的理论名称 → 相对简单
Method/标签: 需要识别研究方法描述 → 通常在方法部分
Sample/标签: 需要识别研究对象信息 → 通常在方法和摘要
Item/标签:   需要提取核心概念和发现 → 需要更全面的内容理解
```

---


### 模块 4：无内容的专门处理

```javascript
if (!content) {
  // 无内容，标注便于识别
  try {
    item.addTag('Item/无摘要');
    await item.saveTx();
    noContentCount++;
  } catch {
    errorCount++;
  }
  continue;
}
```

**主题标签无内容处理的特殊性：**

- **Item/无摘要标记**: 在 Item/命名空间下标记无内容情况
- **主题提取困难**: 没有内容就无法提取核心变量和研究主题
- **后续处理优先**: 主题标签对内容质量要求较高，无内容的条目需要优先补充

**与其他标签体系的区别：**

```
Theory/无摘要  - 无法识别理论框架
Sample/无摘要  - 无法识别研究对象  
Item/无摘要    - 无法提取核心主题和变量
Method/无摘要  - 无法识别研究方法
```

---


### 模块 5：优化的 AI 分析与核心内容提取

```javascript
try {
  const prompt = `分析以下学术文献内容，生成3个最能代表其核心内容的标签：

${content}

要求：
1. 标签应简洁精确，3-6个字符
2. 使用简体中文
3. 重点关注：核心变量、研究主题、研究结论、核心概念
4. 避免研究方法、理论名称、研究对象（这些有单独标签）
5. 聚焦文献的核心贡献和发现
6. 避免过于宽泛的词汇，要具体明确
7. 返回JSON数组格式

请直接返回JSON数组，格式：["标签1", "标签2", "标签3"]`;

  const response = await Meet.Global.views.ask(prompt);
} catch {
  // AI调用失败处理
}
```

**优化后的 Prompt 核心设计：**

#### 聚焦四大核心内容
1. **核心变量**: 研究中的关键变量和测量指标
    - 如：创新绩效、工作满意度、组织承诺、客户忠诚度
2. **研究主题**: 文献探讨的核心议题和问题
    - 如：数字化转型、可持续发展、知识管理、战略联盟
3. **研究结论**: 文献的主要发现和核心贡献
    - 如：正向影响、中介作用、调节效应、非线性关系
4. **核心概念**: 文献中的关键概念和现象
    - 如：组织学习、战略柔性、顾客价值、创新氛围


#### 明确排除内容（避免标签重复）
- **研究方法**: 如案例研究、问卷调查、实验设计（有 Method/标签）
- **理论名称**: 如社会交换理论、资源基础观（有 Theory/标签）
- **研究对象**: 如制造企业、高管、员工（有 Sample/标签）


#### 质量控制要求
- **长度控制**: 3-6 个字符，确保标签简洁实用
- **具体明确**: 避免 " 管理 "、" 组织 " 等过于宽泛的词汇
- **中文表达**: 使用准确的简体中文学术术语

**标签示例对比：**

**优化前可能生成（问题标签）：**

- 制造企业（这是研究对象，应该在 Sample/）
- 案例研究（这是研究方法，应该在 Method/）
- 资源基础观（这是理论，应该在 Theory/）

**优化后会生成（正确的主题标签）：**

- 创新绩效（核心变量）
- 数字化转型（研究主题）
- 正向影响（研究结论）

**另一个例子：**

- 工作满意度（核心变量）
- 组织承诺（核心概念）
- 中介作用（研究发现）

---


### 模块 6：AI 响应解析与主题标签验证

```javascript
try {
  const jsonMatch = response.match(/\[[\s\S]*?\]/);
  if (!jsonMatch) {
    failCount++;
    continue;
  }

  const tags = JSON.parse(jsonMatch[0]);
  if (!tags || tags.length === 0) {
    failCount++;
    continue;
  }

  // 验证和清理标签
  const validTags = tags
    .filter(tag => typeof tag === 'string' && tag.trim().length > 0)
    .map(tag => tag.trim())
    .slice(0, 3); // 确保最多3个标签

  if (validTags.length === 0) {
    failCount++;
    continue;
  }
} catch {
  // JSON解析失败
  failCount++;
}
```

**主题标签的验证特点：**

#### 格式验证
- **JSON 提取**: 使用正则表达式准确提取 JSON 数组
- **解析验证**: 确保 AI 返回的是有效 JSON 格式
- **容错处理**: 处理 AI 返回格式的微小变化


#### 内容验证
- **类型检查**: 确保每个标签都是有效字符串
- **空值处理**: 过滤空字符串和无效内容
- **数量控制**: 最多保留 3 个标签，平衡信息量与简洁性
- **数据清理**: 标准化标签格式


#### 主题标签的特殊验证需求
- **灵活性要求**: 与样本标签不同，主题标签不需要严格的预定义列表验证
- **内容导向**: 重点验证标签是否真正反映文献的核心内容
- **质量优先**: 宁可失败也不生成低质量的主题标签

**验证逻辑的设计考虑：**

```
Theory/标签: 需要严格匹配预定义理论库
Sample/标签: 需要严格匹配预定义层次选项
Item/标签:   相对灵活，重点关注内容质量和相关性
Method/标签: 需要匹配预定义方法类别
```

---


### 模块 7：主题标签的批量添加与保存

```javascript
// 添加标签
for (let tag of validTags) {
  item.addTag('Item/' + tag);
}

try {
  await item.saveTx();
  successCount++;
} catch {
  errorCount++;
}
```

**主题标签添加的特点：**

#### Item/前缀的意义
- **功能标识**: Item/明确标识这些是核心主题内容标签
- **检索便利**: 可以通过 Item/前缀快速筛选主题相关标签
- **体系完整**: 与 Theory/、Sample/、Method/形成完整标签生态


#### 批量操作设计
- **循环添加**: 依次添加所有有效的主题标签
- **前缀统一**: 确保所有主题标签都有 Item/前缀
- **原子保存**: 所有标签添加完毕后统一保存


#### 主题标签的结构示例

```
某个关于企业数字化转型的研究可能生成：
├── Item/创新绩效        (核心变量)
├── Item/数字化转型      (研究主题)
└── Item/正向影响        (研究结论)

某个关于员工行为的研究可能生成：
├── Item/工作满意度      (核心变量)
├── Item/组织承诺        (核心概念)
└── Item/中介作用        (研究发现)
```

---


### 模块 8：统计反馈与主题标签质量指导

```javascript
return `主题标签处理完成 - 成功: ${successCount}, 跳过: ${skipCount}, 无内容: ${noContentCount}, 失败: ${failCount}, 保存失败: ${errorCount}`;
```

**主题标签统计的特殊意义：**

#### 成果价值指标
- **成功 (successCount)**: 成功提取核心主题内容的文献数量
- **质量体现**: 反映 AI 对文献核心贡献的理解和提取能力


#### 处理效率指标
- **跳过 (skipCount)**: 已有 Item/标签的文献数量
- **避重效果**: 体现主题标签处理的效率


#### 内容质量指标
- **无内容 (noContentCount)**: 需要补充内容以提取主题的文献数量
- **质量要求**: 主题提取对内容质量要求较高


#### 技术质量指标
- **失败 (failCount)**: AI 无法准确识别核心主题的文献数量
- **技术挑战**: 反映主题提取的技术难度


#### 系统稳定指标
- **保存失败 (errorCount)**: 系统级错误数量

**后续处理的主题标签专门建议：**

1. **内容质量提升**:

    ```
    优先处理: Item/无摘要的文献
    补充策略: 添加详细摘要，确保包含核心变量和发现
    质量验证: 确保摘要能够体现研究的核心贡献
    ```

2. **主题标签质量检查**:

    ```
    抽查标准: 标签是否真正反映文献核心内容
    避免问题: 检查是否混入了方法、理论、对象类标签
    优化方向: 根据检查结果调整prompt的聚焦重点
    ```

3. **标签体系协同应用**:

    ```
    组合查询: Item/ + Theory/ + Sample/ + Method/
    研究发现: 通过Item/标签识别研究热点和核心变量
    空白识别: 发现缺乏研究的核心主题和变量组合
    ```

4. **主题标签的特殊价值**:

    ```
    核心变量库: 建立学科领域的核心变量词典
    研究主题图谱: 绘制研究主题的演进和热点
    研究发现总结: 汇总领域内的主要研究结论
    ```
