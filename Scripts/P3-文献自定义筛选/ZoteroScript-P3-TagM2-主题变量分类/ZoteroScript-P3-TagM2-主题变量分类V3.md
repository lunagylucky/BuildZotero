---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P3-TagM2-主题变量分类V3
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
  - AI
  - AI研究
  - JavaScript
  - V3版本
  - Zotero
  - 主题分类
  - 代码
  - 创新
  - 创新研究
  - 变量分类
  - 学术研究
  - 智能清理
  - 标签体系
  - 标签清理
  - 标签维护
  - 自动化标签
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P3-TagM2- 主题变量分类 V3

```javascript
#🏷️KEY[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processCount = 0;
  let skipCount = 0;
  let errorCount = 0;
  let addedTags = {
    ai_def: 0, ai_mea: 0, ai_dv: 0, ai_iv: 0, ai_mo: 0, ai_me: 0,
    innovation_def: 0, innovation_mea: 0, innovation_dv: 0, 
    innovation_iv: 0, innovation_mo: 0, innovation_me: 0
  };
  let removedTags = {
    ai_def: 0, ai_mea: 0, ai_dv: 0, ai_iv: 0, ai_mo: 0, ai_me: 0,
    innovation_def: 0, innovation_mea: 0, innovation_dv: 0, 
    innovation_iv: 0, innovation_mo: 0, innovation_me: 0
  };
  
  // 定义标签前缀
  const prefixes = ["V1-def/", "V2-mea/", "A1-DV/", "A2-IV/", "A3-MO/", "A4-ME/"];
  
  // 定义主题和标签映射
  const themeMapping = [
    {
      theme: "AI",
      keywords: ["AI", "人工智能", "机器学习", "ML", "人机", "大语言模型"],
      tagMapping: {
        "V1-def/": { tag: "/@🔧/AI", key: "ai_def" },
        "V2-mea/": { tag: "/@📐/AI", key: "ai_mea" },
        "A1-DV/": { tag: "/@AI/DV", key: "ai_dv" },
        "A2-IV/": { tag: "/@AI/IV", key: "ai_iv" },
        "A3-MO/": { tag: "/@AI/MO", key: "ai_mo" },
        "A4-ME/": { tag: "/@AI/ME", key: "ai_me" }
      }
    },
    {
      theme: "创新",
      keywords: ["创新"],
      tagMapping: {
        "V1-def/": { tag: "/@🔧/创新", key: "innovation_def" },
        "V2-mea/": { tag: "/@📐/创新", key: "innovation_mea" },
        "A1-DV/": { tag: "/@创新/DV", key: "innovation_dv" },
        "A2-IV/": { tag: "/@创新/IV", key: "innovation_iv" },
        "A3-MO/": { tag: "/@创新/MO", key: "innovation_mo" },
        "A4-ME/": { tag: "/@创新/ME", key: "innovation_me" }
      }
    }
  ];
  
  for (let item of items) {
    try {
      // 获取条目的所有标签
      const existingTags = item.getTags();
      
      // 按前缀分组标签
      const tagsByPrefix = {};
      for (let prefix of prefixes) {
        tagsByPrefix[prefix] = existingTags.filter(tag => 
          tag.tag && tag.tag.startsWith(prefix)
        );
      }
      
      let hasModified = false;
      
      // 遍历每个主题
      for (let theme of themeMapping) {
        // 遍历每个前缀
        for (let prefix of prefixes) {
          const tags = tagsByPrefix[prefix];
          const mapping = theme.tagMapping[prefix];
          
          // 检查该前缀的标签中是否包含该主题的关键词
          const hasKeyword = tags.length > 0 && tags.some(tag => 
            theme.keywords.some(keyword => tag.tag.includes(keyword))
          );
          
          // 检查目标标签是否已存在
          const hasTargetTag = existingTags.some(tag => tag.tag === mapping.tag);
          
          if (hasKeyword) {
            // 有关键词，应该添加标签
            if (!hasTargetTag) {
              item.addTag(mapping.tag);
              hasModified = true;
              addedTags[mapping.key]++;
            }
          } else {
            // 无关键词，应该删除标签（如果存在）
            if (hasTargetTag) {
              item.removeTag(mapping.tag);
              hasModified = true;
              removedTags[mapping.key]++;
            }
          }
        }
      }
      
      // 如果有修改则保存
      if (hasModified) {
        try {
          await item.saveTx();
          processCount++;
        } catch {
          errorCount++;
        }
      } else {
        skipCount++;
      }
      
    } catch (e) {
      errorCount++;
    }
  }
  
  // 构建统计信息
  let stats = `主题变量分类标签处理完成（V3 智能清理版）\n`;
  stats += `\n【处理统计】`;
  stats += `\n- 处理成功: ${processCount}`;
  stats += `\n- 无变化: ${skipCount}`;
  stats += `\n- 错误: ${errorCount}`;
  
  stats += `\n\n【添加标签统计】`;
  stats += `\n【AI主题】`;
  stats += `\n- /@🔧/AI: ${addedTags.ai_def}`;
  stats += `\n- /@📐/AI: ${addedTags.ai_mea}`;
  stats += `\n- /@AI/DV: ${addedTags.ai_dv}`;
  stats += `\n- /@AI/IV: ${addedTags.ai_iv}`;
  stats += `\n- /@AI/MO: ${addedTags.ai_mo}`;
  stats += `\n- /@AI/ME: ${addedTags.ai_me}`;
  stats += `\n【创新主题】`;
  stats += `\n- /@🔧/创新: ${addedTags.innovation_def}`;
  stats += `\n- /@📐/创新: ${addedTags.innovation_mea}`;
  stats += `\n- /@创新/DV: ${addedTags.innovation_dv}`;
  stats += `\n- /@创新/IV: ${addedTags.innovation_iv}`;
  stats += `\n- /@创新/MO: ${addedTags.innovation_mo}`;
  stats += `\n- /@创新/ME: ${addedTags.innovation_me}`;
  
  stats += `\n\n【删除标签统计】`;
  stats += `\n【AI主题】`;
  stats += `\n- /@🔧/AI: ${removedTags.ai_def}`;
  stats += `\n- /@📐/AI: ${removedTags.ai_mea}`;
  stats += `\n- /@AI/DV: ${removedTags.ai_dv}`;
  stats += `\n- /@AI/IV: ${removedTags.ai_iv}`;
  stats += `\n- /@AI/MO: ${removedTags.ai_mo}`;
  stats += `\n- /@AI/ME: ${removedTags.ai_me}`;
  stats += `\n【创新主题】`;
  stats += `\n- /@🔧/创新: ${removedTags.innovation_def}`;
  stats += `\n- /@📐/创新: ${removedTags.innovation_mea}`;
  stats += `\n- /@创新/DV: ${removedTags.innovation_dv}`;
  stats += `\n- /@创新/IV: ${removedTags.innovation_iv}`;
  stats += `\n- /@创新/MO: ${removedTags.innovation_mo}`;
  stats += `\n- /@创新/ME: ${removedTags.innovation_me}`;
  
  return stats;
})()
}$
```
