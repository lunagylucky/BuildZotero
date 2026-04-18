---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P3-TagM2-主题变量分类V1
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
- 变量分类
- 标签体系
- 创新研究
- 代码
- 文献管理
- 学术研究
- 主题分类
- AI研究
- JavaScript
- Zotero
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P3-TagM2- 主题变量分类 V 1

### 第一部分: 完整代码

```javascript
#🏷️KEY[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noMatchCount = 0;
  let errorCount = 0;
  let addedTags = {
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
      keywords: ["AI", "人工智能", "机器学习", "ML"],
      tagMapping: {
        "V1-def/": { tag: "/@AI/def", key: "ai_def" },
        "V2-mea/": { tag: "/@AI/mea", key: "ai_mea" },
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
        "V1-def/": { tag: "/@创新/def", key: "innovation_def" },
        "V2-mea/": { tag: "/@创新/mea", key: "innovation_mea" },
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
  let stats = `主题变量分类标签处理完成\n处理成功: ${successCount}\n跳过: ${skipCount}\n无匹配: ${noMatchCount}\n错误: ${errorCount}\n\n添加统计:`;
  stats += `\n【AI主题】`;
  stats += `\n- /@AI/def: ${addedTags.ai_def}`;
  stats += `\n- /@AI/mea: ${addedTags.ai_mea}`;
  stats += `\n- /@AI/DV: ${addedTags.ai_dv}`;
  stats += `\n- /@AI/IV: ${addedTags.ai_iv}`;
  stats += `\n- /@AI/MO: ${addedTags.ai_mo}`;
  stats += `\n- /@AI/ME: ${addedTags.ai_me}`;
  stats += `\n【创新主题】`;
  stats += `\n- /@创新/def: ${addedTags.innovation_def}`;
  stats += `\n- /@创新/mea: ${addedTags.innovation_mea}`;
  stats += `\n- /@创新/DV: ${addedTags.innovation_dv}`;
  stats += `\n- /@创新/IV: ${addedTags.innovation_iv}`;
  stats += `\n- /@创新/MO: ${addedTags.innovation_mo}`;
  stats += `\n- /@创新/ME: ${addedTags.innovation_me}`;
  
  return stats;
})()
}$
```
