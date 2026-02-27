---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag6变量标签V2-R
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: []
RelatedNote:
CardRecord:
---


## ZoteroScript-P1-Tag6 变量标签 V2-R

```javascript
#🏷️R-VariableTag2[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
  // 删除现有变量标签(A1-A6)
  const removeExistingVariableTags = async (item) => {
    const existingTags = item.getTags();
    const variableTags = existingTags.filter(tag => 
      tag.tag && /^A[1-6]-(DV|IV|MO|ME|INV|CV)\//.test(tag.tag)
    );
    for (let variableTag of variableTags) {
      item.removeTag(variableTag.tag);
    }
  };
  
  // 保存函数
  const saveItemWithTags = async (item, tags, countType) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        for (let tag of tags) {
          item.addTag(tag);
        }
        await item.save();
      });
      
      if (countType === 'success') successCount++;
      else if (countType === 'noContent') noContentCount++;
      else if (countType === 'fail') failCount++;
      
      return true;
    } catch {
      errorCount++;
      return false;
    }
  };
  
  for (let item of items) {
    try {
      // 删除原有变量标签
      await removeExistingVariableTags(item);
      
      let content = "";
      
      // 内容获取:优先PDF,其次摘要
      try {
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text && fullTextData.text.trim().length > 100) {
              content = fullTextData.text.substring(0, 30000).trim();
            }
          } catch {}
        }
        
        if (!content) {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        }
      } catch {}
      
      if (!content) {
        await saveItemWithTags(item, ['A1-DV/无内容'], 'noContent');
        continue;
      }
      
      // AI分析识别变量
      try {
        const prompt = `你是一位实证研究专家。请仔细分析以下学术文献内容,识别研究中的关键变量。
文献内容:
${content}
任务说明:
请识别文献中的以下6类变量,并提取变量的具体名称:
1. **因变量(DV - Dependent Variable)**:
   - 研究要解释或预测的结果变量
   - 通常在"被解释变量"、"结果变量"、"因变量"等部分提及
   - 示例:企业绩效、创新绩效、碳排放、经济增长
2. **自变量(IV - Independent Variable)**:
   - 研究的核心解释变量或预测变量,是研究关注的主要因果关系中的原因变量
   - 通常在"核心解释变量"、"自变量"、"关键变量"等部分提及
   - 示例:数字化转型、环境规制、金融科技、政策冲击
   - **注意:不要与控制变量混淆,自变量是研究的核心关注对象**
3. **调节变量(MO - Moderator)**:
   - 影响自变量与因变量之间关系强度或方向的变量
   - 通常在"调节效应"、"异质性分析"、"交互项"等部分提及
   - 示例:企业规模、产权性质、市场竞争、制度环境
4. **中介变量(ME - Mediator)**:
   - 解释自变量如何影响因变量的中间传导机制变量
   - 通常在"中介效应"、"机制检验"、"传导路径"等部分提及
   - 示例:技术创新、融资约束、信息不对称、资源配置效率
5. **工具变量(INV - Instrumental Variable)**:
   - 用于处理内生性问题的外生变量
   - 通常在"工具变量"、"内生性处理"、"IV估计"等部分提及
   - 示例:地理距离、历史事件、政策外生冲击、滞后变量
6. **控制变量(CV - Control Variable)**:
   - 为了排除干扰、提高估计精度而纳入模型的其他变量
   - 通常在"控制变量"、"其他变量"部分提及
   - 示例:企业年龄、行业、地区、时间固定效应
   - **注意:控制变量不是研究的核心关注点,只是为了控制其他因素的影响**
识别要求:
- **变量名称必须简洁准确,使用2-6个字的核心术语**
- **禁止使用描述性语句或完整句子作为变量名**
- **正确示例**: "企业绩效"、"数字化转型"、"环境规制"、"融资约束"
- **错误示例**: "企业的创新绩效表现"、"数字化转型程度的提升"、"环境规制政策的强度"
- 提取变量的具体学术名称(使用简体中文)
- 如果同一类别有多个变量,用加号(+)连接,例如:"探索性创新+利用式创新"
- 如果某类变量在文献中不存在或未明确提及,则该类别标记为"无"
- 优先提取文献中明确标注的变量名称
- **重要:严格区分自变量(IV)和控制变量(CV)**
  - 自变量是研究的核心解释变量,是假设检验的对象
  - 控制变量只是为了排除干扰因素,不是研究的核心关注点
请严格按照以下JSON格式返回(仅返回JSON,无其他文字):
{
  "DV": "因变量名称或多个用+连接",
  "IV": "自变量名称或多个用+连接",
  "MO": "调节变量名称或多个用+连接",
  "ME": "中介变量名称或多个用+连接",
  "INV": "工具变量名称或多个用+连接",
  "CV": "控制变量名称或多个用+连接"
}
示例输出1(正确格式):
{
  "DV": "企业绩效",
  "IV": "数字化转型",
  "MO": "企业规模+产权性质",
  "ME": "组织学习",
  "INV": "宽带普及率",
  "CV": "企业年龄+行业+地区"
}
示例输出2(正确格式):
{
  "DV": "碳排放强度",
  "IV": "环境规制",
  "MO": "技术水平",
  "ME": "无",
  "INV": "无",
  "CV": "企业规模+所有制+年份"
}
示例输出3(错误格式-名称过长):
{
  "DV": "企业的全要素生产率和利润率表现",  ❌ 应改为: "全要素生产率+利润率"
  "IV": "数字金融发展水平的提升",  ❌ 应改为: "数字金融"
  "MO": "无",
  "ME": "企业面临的融资约束程度和资源配置效率",  ❌ 应改为: "融资约束+资源配置"
  "INV": "地区互联网普及率",
  "CV": "企业的基本特征和行业特征以及地区特征"  ❌ 应改为: "企业特征+行业+地区"
}`;
        const response = await Meet.Global.views.ask(prompt);
        
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\{[\s\S]*?\}/);
          if (!jsonMatch) {
            await saveItemWithTags(item, ['A1-DV/解析失败'], 'fail');
            continue;
          }
          
          const variables = JSON.parse(jsonMatch[0]);
          
          // 验证必须包含DV和IV
          if (!variables.DV || !variables.IV) {
            await saveItemWithTags(item, ['A1-DV/格式错误'], 'fail');
            continue;
          }
          
          const tagsToAdd = [];
          
          // 添加标签的映射关系
          const tagMapping = {
            'DV': 'A1-DV',
            'IV': 'A2-IV',
            'MO': 'A3-MO',
            'ME': 'A4-ME',
            'INV': 'A5-INV',
            'CV': 'A6-CV'
          };
          
          // 处理每个变量类型
          for (let [key, prefix] of Object.entries(tagMapping)) {
            const value = variables[key];
            if (value && typeof value === 'string' && value.trim() !== '') {
              const trimmedValue = value.trim();
              
              // 只有DV为"无"时才添加标记,其他变量为"无"时跳过
              if (trimmedValue === '无') {
                if (key === 'DV') {
                  tagsToAdd.push(`${prefix}/无`);
                }
                // 其他变量为"无"时不添加标签,直接跳过
              } else {
                // 值不为"无"时,正常添加标签
                tagsToAdd.push(`${prefix}/${trimmedValue}`);
              }
            }
          }
          
          // 至少要有1个标签(DV,即使是DV/无)
          if (tagsToAdd.length < 1) {
            await saveItemWithTags(item, ['A1-DV/标签不完整'], 'fail');
            continue;
          }
          
          await saveItemWithTags(item, tagsToAdd, 'success');
          
        } catch {
          await saveItemWithTags(item, ['A1-DV/JSON解析失败'], 'fail');
        }
        
      } catch {
        await saveItemWithTags(item, ['A1-DV/AI调用失败'], 'fail');
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `变量标签更新完成\n成功: ${successCount} | 无内容: ${noContentCount} | 失败: ${failCount} | 错误: ${errorCount}`;
})()
}$
```
