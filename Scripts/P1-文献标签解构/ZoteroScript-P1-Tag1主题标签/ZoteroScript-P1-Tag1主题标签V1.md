---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag1主题标签V1
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: [Zotero, 代码, 标签, 脚本]
RelatedNote:
CardRecord:
date: 2024-06-20
related:
  - - zotero_code/zotero_script_最终优化版本
---

## ZoteroScript-P1-Tag1 主题标签 V1

```javascript
#AddTags[color=#66dc79][trigger=]
${
(async ()=>{
  const items = ZoteroPane.getSelectedItems();
  for (let item of items) {
    let abs = item.getField("abstractNote");
    if (!abs) {
      const pdfItem = await item.getBestAttachment();
      if (pdfItem) {
        abs = (await Zotero.PDFWorker.getFullText(pdfItem.id, 1)).text;
      } else {
        continue;
      }
    }

    const res = await Meet.Global.views.ask(
      "Read the abstract of the article below:\n" + abs +
      "\n---\nReturns 3 tags that fit this abstract as a JSON list. Each tag should be in simplified Chinese and as concise as possible. The result can be parsed by JSON.parse()."
    );

    try {
      const tags = JSON.parse(res.match(/\[[\s\S]+\]/)[0]);
      tags.forEach(tag => item.addTag("Item/" + tag));
      await item.saveTx();
    } catch {}
  }
})()
}$
请直接输出：完成。
