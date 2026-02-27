---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P6-UploadS2-UploadPDFV1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:58
Type:
Status:
Version:
CardStatus:
CardType:
tags: [代码, 批量处理, 三元运算符, 文件上传, 学术工具, 异步编程, JavaScript, OpenAI, PDF, Zotero]
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P 6-UploadS2-UploadPDFV1

### 第一部分：完整代码

```javascript
#📑UploadPDF[color=#f9c23c][trigger=]
${
(async () => {
  const pdfItems = []
  for (let item of ZoteroPane.getSelectedItems()) {
    const pdfItem = item.isPDFAttachment() ? item : await item.getBestAttachment()
    pdfItem && pdfItems.push(pdfItem)
  }
  await window.Meet.OpenAI.uploadFile(pdfItems)
  return ""
})()
}$
```
