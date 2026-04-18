---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P3-TagM1自定义变量定义与衡量分类V2
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
  - 变量测量
  - 变量定义
  - 标签体系
  - 标签维护
  - 代码
  - 多样性
  - 韧性
  - 冗余
  - 智能清理
  - 自动化标签
  - JavaScript
  - V2版本
  - Zotero
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P3-TagM1 - 变量定义与衡量分类 V2

```javascript
#🏷️MD[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processCount = 0;
  let skipCount = 0;
  let errorCount = 0;
  let addedTags = {
    diversity_def: 0, diversity_mea: 0,
    resilience_def: 0, resilience_mea: 0,
    redundancy_def: 0, redundancy_mea: 0
  };
  let removedTags = {
    diversity_def: 0, diversity_mea: 0,
    resilience_def: 0, resilience_mea: 0,
    redundancy_def: 0, redundancy_mea: 0
  };
  
  // 定义检测的标签前缀
  const prefixes = ["V1-def/", "V2-mea/"];
  
  // 定义主题和标签映射
  const themeMapping = [
    {
      theme: "多样性",
      keywords: ["多样", "多元"],
      tagMapping: {
        "V1-def/": { tag: "/@🔧/多样", key: "diversity_def" },
        "V2-mea/": { tag: "/@📐/多样", key: "diversity_mea" }
      }
    },
    {
      theme: "韧性",
      keywords: ["韧性"],
      tagMapping: {
        "V1-def/": { tag: "/@🔧/韧性", key: "resilience_def" },
        "V2-mea/": { tag: "/@📐/韧性", key: "resilience_mea" }
      }
    },
    {
      theme: "冗余",
      keywords: ["冗余"],
      tagMapping: {
        "V1-def/": { tag: "/@🔧/冗余", key: "redundancy_def" },
        "V2-mea/": { tag: "/@📐/冗余", key: "redundancy_mea" }
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
  let stats = `MD主题标签处理完成（V2 智能清理版）\n`;
  stats += `\n【处理统计】`;
  stats += `\n- 处理成功: ${processCount}`;
  stats += `\n- 无变化: ${skipCount}`;
  stats += `\n- 错误: ${errorCount}`;
  
  stats += `\n\n【添加标签统计】`;
  stats += `\n【多样性主题】`;
  stats += `\n- /@🔧/多样: ${addedTags.diversity_def}`;
  stats += `\n- /@📐/多样: ${addedTags.diversity_mea}`;
  stats += `\n【韧性主题】`;
  stats += `\n- /@🔧/韧性: ${addedTags.resilience_def}`;
  stats += `\n- /@📐/韧性: ${addedTags.resilience_mea}`;
  stats += `\n【冗余主题】`;
  stats += `\n- /@🔧/冗余: ${addedTags.redundancy_def}`;
  stats += `\n- /@📐/冗余: ${addedTags.redundancy_mea}`;
  
  stats += `\n\n【删除标签统计】`;
  stats += `\n【多样性主题】`;
  stats += `\n- /@🔧/多样: ${removedTags.diversity_def}`;
  stats += `\n- /@📐/多样: ${removedTags.diversity_mea}`;
  stats += `\n【韧性主题】`;
  stats += `\n- /@🔧/韧性: ${removedTags.resilience_def}`;
  stats += `\n- /@📐/韧性: ${removedTags.resilience_mea}`;
  stats += `\n【冗余主题】`;
  stats += `\n- /@🔧/冗余: ${removedTags.redundancy_def}`;
  stats += `\n- /@📐/冗余: ${removedTags.redundancy_mea}`;
  
  return stats;
})()
}$
```
