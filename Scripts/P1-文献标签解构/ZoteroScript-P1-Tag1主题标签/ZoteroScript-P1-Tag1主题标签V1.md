---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag1主题标签V1
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
- Zotero
- 代码
- 标签
- 脚本
- Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord: ''
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
