---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P3-TagM1自定义变量定义与衡量分类V1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:41
Type:
Status:
Version:
CardStatus:
CardType:
tags: [变量测量, 变量定义, 标签体系, 代码, 多样性, 韧性, 冗余, 自动化标签, JavaScript, Zotero]
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P3-TagM1- 变量定义与衡量分类 V 1

### 脚本功能概述

```javascript
#🏷️MD[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noMatchCount = 0;
  let errorCount = 0;
  let addedTags = {
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
      keywords: ["多样性", "多元"],
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
      
      // 检查是否有任何目标标签
      const hasTargetTags = prefixes.some(prefix => tagsByPrefix[prefix].length > 0);
      if (!hasTargetTags) {
        noMatchCount++;
        continue;
      }
      
      let hasAddedTag = false;
      
      // 遍历每个主题
      for (let theme of themeMapping) {
        // 遍历每个前缀
        for (let prefix of prefixes) {
          const tags = tagsByPrefix[prefix];
          if (tags.length === 0) continue;
          
          // 检查是否包含该主题的关键词
          const hasKeyword = tags.some(tag => 
            theme.keywords.some(keyword => tag.tag.includes(keyword))
          );
          
          if (hasKeyword) {
            const mapping = theme.tagMapping[prefix];
            // 检查是否已存在该标签
            if (!existingTags.some(tag => tag.tag === mapping.tag)) {
              item.addTag(mapping.tag);
              hasAddedTag = true;
              addedTags[mapping.key]++;
            }
          }
        }
      }
      
      // 如果添加了标签则保存
      if (hasAddedTag) {
        try {
          await item.saveTx();
          successCount++;
        } catch {
          errorCount++;
        }
      } else {
        // 有目标标签但没有匹配的关键词
        skipCount++;
      }
      
    } catch (e) {
      errorCount++;
    }
  }
  
  // 构建统计信息
  let stats = `MD主题标签处理完成\n处理成功: ${successCount}\n跳过: ${skipCount}\n无匹配: ${noMatchCount}\n错误: ${errorCount}\n\n添加统计:`;
  stats += `\n【多样性主题】`;
  stats += `\n- /@🔧/多样: ${addedTags.diversity_def}`;
  stats += `\n- /@📐/多样: ${addedTags.diversity_mea}`;
  stats += `\n【韧性主题】`;
  stats += `\n- /@🔧/韧性: ${addedTags.resilience_def}`;
  stats += `\n- /@📐/韧性: ${addedTags.resilience_mea}`;
  stats += `\n【冗余主题】`;
  stats += `\n- /@🔧/冗余: ${addedTags.redundancy_def}`;
  stats += `\n- /@📐/冗余: ${addedTags.redundancy_mea}`;
  
  return stats;
})()
}$
```
