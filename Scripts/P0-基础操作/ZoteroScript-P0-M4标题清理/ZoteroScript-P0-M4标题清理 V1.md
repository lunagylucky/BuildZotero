---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P0-M4标题清理 V1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:17
Type:
Status:
Version:
CardStatus:
CardType:
tags: [代码, 批量处理, 文献管理, 自动化脚本, HTML清理, JavaScript, Zotero]
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P0-M4 标题清理 V1

```javascript
#🪄CleanTitle[color=#ff6b6b][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processedItemsCount = 0;    // 处理的条目数
  let totalChangedTitles = 0;     // 修改的标题总数
  let errorCount = 0;             // 处理失败的条目数
  
  // 定义需要清理的HTML标签
  const tagsToRemove = [
    /<i>/gi,           // 斜体开始标签
    /<\/i>/gi,         // 斜体结束标签
    /<scp>/gi,         // scp开始标签
    /<\/scp>/gi        // scp结束标签
  ];
  
  for (let item of items) {
    try {
      const originalTitle = item.getField('title');
      if (!originalTitle) continue;
      
      let cleanedTitle = originalTitle;
      let hasChanges = false;
      
      // 逐个清理指定的HTML标签
      for (let tagPattern of tagsToRemove) {
        const beforeClean = cleanedTitle;
        cleanedTitle = cleanedTitle.replace(tagPattern, '');
        if (beforeClean !== cleanedTitle) {
          hasChanges = true;
        }
      }
      
      // 清理可能产生的多余空格
      cleanedTitle = cleanedTitle.replace(/\s+/g, ' ').trim();
      
      // 如果标题有变化，则更新
      if (hasChanges && cleanedTitle !== originalTitle) {
        try {
          item.setField('title', cleanedTitle);
          await item.saveTx();
          processedItemsCount++;
          totalChangedTitles++;
        } catch {
          errorCount++;
        }
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `标题清理完成 - 处理条目: ${processedItemsCount}, 修改标题: ${totalChangedTitles}, 失败条目: ${errorCount}`;
})()
}$
```
