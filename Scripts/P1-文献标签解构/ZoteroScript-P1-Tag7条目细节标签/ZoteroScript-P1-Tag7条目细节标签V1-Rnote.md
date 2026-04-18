---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag7条目细节标签V1-Rnote
DateCreated: 2026-01-17 17:37
DateModified: 2026-04-18 17:38
Type:
- doc
Status:
- doing
Version:
- v1.0
CardStatus: false
CardType:
- card-fleeting
tags:
- Topic/工具技能/工作笔记
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P1-Tag7 条目细节标签 V1-Rnote

```javascript
#🏷️Rnote-ItemTag3[color=#66dc79][trigger=]
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
  // 检查是否有📒标签
  const hasNotebookTag = (item) => {
    const tags = item.getTags();
    return tags.some(tag => tag.tag && tag.tag.includes("📒"));
  };
  
  // 获取笔记内容 - 使用BetterNotes转换为markdown
  const getNoteContent = async (noteItem) => {
    try {
      return await Zotero.BetterNotes.api.convert.html2md(noteItem.getNote());
    } catch {
      return "";
    }
  };
  
  // 删除现有研究要素标签(V开头)
  const removeExistingResearchTags = async (item) => {
    const existingTags = item.getTags();
    const researchTags = existingTags.filter(tag => 
      tag.tag && /^V[1-4]-(def|mea|con|fut)\//.test(tag.tag)
    );
    for (let researchTag of researchTags) {
      item.removeTag(researchTag.tag);
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
      // 检查📒标签,没有则跳过
      if (!hasNotebookTag(item)) {
        skipCount++;
        continue;
      }
      
      // 有📒标签则处理,删除原有研究要素标签
      await removeExistingResearchTags(item);
      
      let content = "";
      
      // 第一优先:获取笔记内容
      const noteIds = item.getNotes();
      
      if (noteIds.length > 0) {
        // 使用第一个笔记
        const noteItem = Zotero.Items.get(noteIds[0]);
        if (noteItem) {
          const noteContent = await getNoteContent(noteItem);
          if (noteContent && noteContent.trim().length > 50) {
            content = noteContent;
          }
        }
      }
      
      // 第二优先:如果没有笔记内容,尝试获取PDF内容
      if (!content) {
        try {
          const pdfItem = await item.getBestAttachment();
          if (pdfItem && pdfItem.isPDFAttachment()) {
            try {
              const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
              if (fullTextData && fullTextData.text && fullTextData.text.trim().length > 100) {
                // 使用PDF前45000字符（与R-ItemTag2保持一致）
                content = fullTextData.text.substring(0, 45000).trim();
              }
            } catch {}
          }
        } catch {}
      }
      
      // 第三优先:如果没有PDF内容,使用摘要
      if (!content) {
        try {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        } catch {}
      }
      
      if (!content) {
        await saveItemWithTags(item, ['V3-con/无内容'], 'noContent');
        continue;
      }
      
      // AI分析识别研究要素
      try {
        const prompt = `你是一位学术文献分析专家。请仔细分析以下学术文献内容,识别研究的关键要素。

文献内容:
${content}

任务说明:
请识别文献中的以下4类信息:

1. **研究贡献与创新点**(1-4个):
   - 识别文献的主要学术贡献、理论创新、实证发现
   - 通常在"研究贡献"、"理论贡献"、"实证贡献"、"创新点"、"主要发现"等部分
   - 也可能在摘要结论部分体现
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 按重要性排序
   - 正确示例: "揭示数字化对创新的非线性影响"
   - 错误示例: "本研究揭示了企业数字化转型对创新绩效的影响机制"(过长)

2. **研究展望**(1-3个):
   - 未来研究方向、研究建议、研究局限
   - 通常在"研究展望"、"未来研究"、"研究局限"、"讨论"等部分
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 按重要性排序
   - 正确示例: "探索跨层次动态交互机制"
   - 错误示例: "未来研究可以探索组织层面和个体层面的跨层次动态交互机制"(过长)

3. **变量定义**(数量不限):
   - 识别文献中主要变量的学术定义或概念界定
   - 通常在"变量测量"、"概念界定"、"理论框架"等部分
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 格式: "变量名:定义"
   - 正确示例: "数字化转型:企业利用数字技术重塑运营"
   - 错误示例: "数字化转型是指企业系统性地利用数字技术对业务模式进行根本性重塑"(过长)

4. **测量方法**(数量不限):
   - 识别文献中变量的具体测量方式或操作化方法
   - 通常在"变量测量"、"数据与方法"等部分
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 格式: "变量名:测量方式"
   - 正确示例: "创新绩效:专利申请数对数值"
   - 错误示例: "创新绩效采用企业当年专利申请数量的自然对数进行衡量"(过长)

识别要求:
- **所有内容必须精简到15-25个字符以内**
- **去掉"本研究"、"我们"、"文章"等主语**
- **去掉"发现"、"表明"、"认为"等冗余动词**
- **只保留核心学术内容**
- 使用简体中文标准术语
- 忠实于原文,不要编造内容
- 如果某类信息在文献中不存在或未明确提及,则标记为"无"
- 按重要性排序

请严格按照以下JSON格式返回(仅返回JSON,无其他文字):
{
  "CON": ["贡献1", "贡献2", "贡献3", "贡献4"],
  "LIM": ["展望1", "展望2", "展望3"],
  "DEF": ["变量名1:定义1", "变量名2:定义2"],
  "MEA": ["变量名1:测量1", "变量名2:测量2"]
}

示例输出1(正确格式):
{
  "CON": ["构建数字化转型三维框架", "发现倒U型影响关系", "识别组织学习中介机制"],
  "LIM": ["扩展跨行业样本研究", "纳入动态时间维度"],
  "DEF": ["数字化转型:利用数字技术重塑运营", "组织学习:知识获取与应用能力"],
  "MEA": ["数字化转型:信息化投入占比", "创新绩效:专利申请数对数值"]
}

示例输出2(某些为空):
{
  "CON": ["验证环境规制波特假说", "发现技术创新中介效应"],
  "LIM": ["无"],
  "DEF": ["环境规制:政府环保政策强度"],
  "MEA": ["环境规制:排污费用对数值", "绿色创新:绿色专利数量"]
}`;
        const response = await Meet.Global.views.ask(prompt);
        
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\{[\s\S]*?\}/);
          if (!jsonMatch) {
            await saveItemWithTags(item, ['V3-con/解析失败'], 'fail');
            continue;
          }
          
          const elements = JSON.parse(jsonMatch[0]);
          
          const tagsToAdd = [];
          
          // 处理研究贡献(contribution) -> V3-con
          if (elements.CON && Array.isArray(elements.CON)) {
            const contributions = elements.CON.filter(c => c && c.trim() !== '' && c.trim() !== '无');
            if (contributions.length > 0) {
              contributions.slice(0, 4).forEach((con, index) => {
                tagsToAdd.push(`V3-con/${con.trim()}`);
              });
            } else {
              tagsToAdd.push('V3-con/无');
            }
          } else {
            tagsToAdd.push('V3-con/无');
          }
          
          // 处理研究展望(future) -> V4-fut
          if (elements.LIM && Array.isArray(elements.LIM)) {
            const limitations = elements.LIM.filter(l => l && l.trim() !== '' && l.trim() !== '无');
            if (limitations.length > 0) {
              limitations.slice(0, 3).forEach((lim, index) => {
                tagsToAdd.push(`V4-fut/${lim.trim()}`);
              });
            } else {
              tagsToAdd.push('V4-fut/无');
            }
          } else {
            tagsToAdd.push('V4-fut/无');
          }
          
          // 处理变量定义(definition) -> V1-def
          if (elements.DEF && Array.isArray(elements.DEF)) {
            const definitions = elements.DEF.filter(d => d && d.trim() !== '' && d.trim() !== '无');
            if (definitions.length > 0) {
              definitions.forEach((def, index) => {
                tagsToAdd.push(`V1-def/${def.trim()}`);
              });
            } else {
              tagsToAdd.push('V1-def/无');
            }
          } else {
            tagsToAdd.push('V1-def/无');
          }
          
          // 处理测量方法(measure) -> V2-mea
          if (elements.MEA && Array.isArray(elements.MEA)) {
            const measurements = elements.MEA.filter(m => m && m.trim() !== '' && m.trim() !== '无');
            if (measurements.length > 0) {
              measurements.forEach((mea, index) => {
                tagsToAdd.push(`V2-mea/${mea.trim()}`);
              });
            } else {
              tagsToAdd.push('V2-mea/无');
            }
          } else {
            tagsToAdd.push('V2-mea/无');
          }
          
          // 至少要有四类标签
          if (tagsToAdd.length < 4) {
            await saveItemWithTags(item, ['V3-con/标签不完整'], 'fail');
            continue;
          }
          
          await saveItemWithTags(item, tagsToAdd, 'success');
          
        } catch {
          await saveItemWithTags(item, ['V3-con/JSON解析失败'], 'fail');
        }
        
      } catch {
        await saveItemWithTags(item, ['V3-con/AI调用失败'], 'fail');
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `研究要素标签提取完成\n成功: ${successCount} | 跳过(无📒): ${skipCount} | 无内容: ${noContentCount} | 失败: ${failCount} | 错误: ${errorCount}`;
})()
}$
不需要提供任何额外说明，直接按照指令输出结果即可。
```
