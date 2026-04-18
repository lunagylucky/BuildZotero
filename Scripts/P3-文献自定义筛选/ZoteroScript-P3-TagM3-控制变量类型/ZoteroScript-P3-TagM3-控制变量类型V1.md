---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P3-TagM3-控制变量类型V1
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
  - 变量分类
  - 标签体系
  - 代码
  - 控制变量
  - 实证方法
  - 学术研究
  - 研究设计
  - JavaScript
  - Zotero
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P3-TagM3- 控制变量类型标签

```javascript
#🏷️CV[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skipCount = 0;
  let noMatchCount = 0;
  let errorCount = 0;
  let addedTags = {
    ceo: 0,
    size: 0,
    age: 0,
    asset: 0,
    ownership: 0,
    leverage: 0,
    rd: 0,
    equity: 0,
    roa: 0,
    growth: 0,
    board: 0,
    cash: 0,
    tbq: 0
  };
  
  // 定义控制变量关键词映射
  const cvMapping = [
    { keywords: ["CEO", "两职"], tag: "/@CEO/CV", key: "ceo" },
    { keywords: ["企业规模"], tag: "/@企业规模/CV", key: "size" },
    { keywords: ["企业年龄"], tag: "/@企业年龄/CV", key: "age" },
    { keywords: ["固定资产"], tag: "/@固资/CV", key: "asset" },
    { keywords: ["所有权", "国企", "国有企业", "国有"], tag: "/@所有权/CV", key: "ownership" },
    { keywords: ["杠杆", "资产负债率"], tag: "/@杠杆/CV", key: "leverage" },
    { keywords: ["研发"], tag: "/@研发/CV", key: "rd" },
    { keywords: ["股权"], tag: "/@股权/CV", key: "equity" },
    { keywords: ["资产回报率", "ROA"], tag: "/@资产回报率/CV", key: "roa" },
    { keywords: ["企业增长", "销售增长率", "营收增长率", "收入增长率"], tag: "/@销售增长率/CV", key: "growth" },
    { keywords: ["董事会规模"], tag: "/@董事会规模/CV", key: "board" },
    { keywords: ["现金"], tag: "/@现金/CV", key: "cash" },
    { keywords: ["托宾", "托宾Q", "Tobin"], tag: "/@TBQ/CV", key: "tbq" }
  ];
  
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
      
      // 遍历每种控制变量类型
      for (let cvType of cvMapping) {
        // 检查是否包含该类型的关键词
        const hasKeyword = targetTags.some(tag => 
          cvType.keywords.some(keyword => tag.tag.includes(keyword))
        );
        
        if (hasKeyword) {
          // 检查是否已存在该标签
          if (!existingTags.some(tag => tag.tag === cvType.tag)) {
            item.addTag(cvType.tag);
            hasAddedTag = true;
            addedTags[cvType.key]++;
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
  let stats = `控制变量类型标签处理完成\n处理成功: ${successCount}\n跳过: ${skipCount}\n无匹配: ${noMatchCount}\n错误: ${errorCount}\n\n添加统计:`;
  stats += `\n- /@CEO/CV: ${addedTags.ceo}`;
  stats += `\n- /@企业规模/CV: ${addedTags.size}`;
  stats += `\n- /@企业年龄/CV: ${addedTags.age}`;
  stats += `\n- /@固资/CV: ${addedTags.asset}`;
  stats += `\n- /@所有权/CV: ${addedTags.ownership}`;
  stats += `\n- /@杠杆/CV: ${addedTags.leverage}`;
  stats += `\n- /@研发/CV: ${addedTags.rd}`;
  stats += `\n- /@股权/CV: ${addedTags.equity}`;
  stats += `\n- /@资产回报率/CV: ${addedTags.roa}`;
  stats += `\n- /@销售增长率/CV: ${addedTags.growth}`;
  stats += `\n- /@董事会规模/CV: ${addedTags.board}`;
  stats += `\n- /@现金/CV: ${addedTags.cash}`;
  stats += `\n- /@TBQ/CV: ${addedTags.tbq}`;
  
  return stats;
})()
}$
```
