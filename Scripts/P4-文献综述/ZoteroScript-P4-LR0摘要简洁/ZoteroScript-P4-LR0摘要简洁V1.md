---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P4-LR0摘要简洁V1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:45
Type:
Status:
Version:
CardStatus:
CardType:
tags: []
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P4-LR0 摘要简洁 V1

```javascript
#🔄Cleanabstract[color=#FF6B6B][trigger=]
${
(async function() {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "❌ 未选择任何条目";
  
  let cleanedCount = 0;
  let skipCount = 0;
  
  for (let item of items) {
    const abstract = item.getField('abstractNote') || '';
    
    if (abstract.includes('【摘要翻译】')) {
      const cleanAbstract = abstract.split('【摘要翻译】')[0].trim();
      item.setField('abstractNote', cleanAbstract);
      await item.saveTx();
      cleanedCount++;
    } else {
      skipCount++;
    }
  }
  
  return `✅ 已清理 ${cleanedCount} 个 | ✓ 跳过 ${skipCount} 个`;
})()
}$

---
**重要指令**：请直接输出上述结果，不要添加任何额外的解释、分析或说明文字。

```
