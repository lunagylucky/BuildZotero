---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P3-TagM4-地区固定效应V1
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
  - 标签体系
  - 代码
  - 地区标签
  - 固定效应
  - 面板数据
  - 年份标签
  - 实证方法
  - 行业标签
  - 学术研究
  - JavaScript
  - Zotero
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P3-TagM4- 地区固定效应 V 1

```javascript
#🏷️FE[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noMatchCount = 0;
  let errorCount = 0;
  let addedTags = {
    location: 0,
    year: 0,
    industry: 0
  };
  
  // 定义关键词列表
  const locationKeywords = ["省份", "城市", "地区"];
  const yearKeywords = ["年份", "年度"];
  const industryKeywords = ["行业", "产业"];
  
  for (let item of items) {
    try {
      // 获取条目的所有标签
      const existingTags = item.getTags();
      
      // 筛选A2-IV/和A6-CV/开头的标签
      const targetTags = existingTags.filter(tag => 
        tag.tag && (tag.tag.startsWith("A2-IV/") || tag.tag.startsWith("A6-CV/"))
      );
      
      if (targetTags.length === 0) {
        noMatchCount++;
        continue;
      }
      
      let hasAddedTag = false;
      let tagTexts = targetTags.map(t => t.tag);
      
      // 检查地区关键词 - 添加/@省份/FE标签
      const hasLocationKeyword = targetTags.some(tag => 
        locationKeywords.some(keyword => tag.tag.includes(keyword))
      );
      
      if (hasLocationKeyword) {
        const locationFETag = "/@省份/FE";
        // 检查是否已存在该标签
        if (!existingTags.some(tag => tag.tag === locationFETag)) {
          item.addTag(locationFETag);
          hasAddedTag = true;
          addedTags.location++;
        }
      }
      
      // 检查年份关键词 - 添加/@年份/FE标签
      const hasYearKeyword = targetTags.some(tag => 
        yearKeywords.some(keyword => tag.tag.includes(keyword))
      );
      
      if (hasYearKeyword) {
        const yearFETag = "/@年份/FE";
        // 检查是否已存在该标签
        if (!existingTags.some(tag => tag.tag === yearFETag)) {
          item.addTag(yearFETag);
          hasAddedTag = true;
          addedTags.year++;
        }
      }
      
      // 检查行业关键词 - 添加/@行业/FE标签
      const hasIndustryKeyword = targetTags.some(tag => 
        industryKeywords.some(keyword => tag.tag.includes(keyword))
      );
      
      if (hasIndustryKeyword) {
        const industryFETag = "/@行业/FE";
        // 检查是否已存在该标签
        if (!existingTags.some(tag => tag.tag === industryFETag)) {
          item.addTag(industryFETag);
          hasAddedTag = true;
          addedTags.industry++;
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
  
  return `固定效应标签处理完成\n处理成功: ${successCount}\n跳过: ${skipCount}\n无匹配: ${noMatchCount}\n错误: ${errorCount}\n\n添加统计:\n- /@省份/FE: ${addedTags.location}\n- /@年份/FE: ${addedTags.year}\n- /@行业/FE: ${addedTags.industry}`;
})()
}$
```
