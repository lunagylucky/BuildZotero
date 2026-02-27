---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P0-M3标签删除V2
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:17
Type:
Status:
Version:
CardStatus:
CardType:
tags: [安全机制, 八脚本生态, 标签删除, 代码, 工作流工具, 批量操作, 数据管理, 用户交互, JavaScript, Zotero]
RelatedNote:
RelatedProjects:
CardRecord:
---


## ZoteroScript-P0-M3 标签删除 V2

```javascript
#🏷️DeleteTags[color=#ff6b6b][trigger=]
${
(async () => {
  // 通过用户输入获取要删除的标签前缀，支持多个前缀
  const userInput = prompt("请输入要删除的标签前缀，多个前缀用逗号分隔\n例如：sMeth/, Theory/, Sample/,  Item/, Result/,Sample/1宏观\n或者：项目A, 类别B, 标签C");
  
  // 检查用户输入
  if (!userInput || userInput.trim() === "") {
    return "操作取消：未输入标签前缀";
  }
  
  // 解析多个标签前缀
  const tagPrefixes = userInput.split(',').map(prefix => prefix.trim()).filter(prefix => prefix !== "");
  
  if (tagPrefixes.length === 0) {
    return "操作取消：未输入有效的标签前缀";
  }
  
  // 确认删除操作
  const prefixList = tagPrefixes.map(prefix => `"${prefix}"`).join(", ");
  const confirmDelete = confirm(`确认删除所有以下列前缀开头的标签吗？\n${prefixList}\n\n此操作不可撤销，请谨慎操作！`);
  if (!confirmDelete) {
    return "操作取消：用户选择不删除";
  }
  
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processedCount = 0;    // 处理的条目数
  let deletedTagsCount = 0;  // 删除的标签总数
  let skipCount = 0;         // 跳过的条目数（没有匹配标签）
  let errorCount = 0;        // 保存失败的条目数
  let previewTags = [];      // 预览将要删除的标签
  
  // 首先预览将要删除的标签
  for (let item of items) {
    try {
      const existingTags = item.getTags();
      // 检查是否有标签匹配任何一个前缀
      const tagsToDelete = existingTags.filter(tag => 
        tag.tag && tagPrefixes.some(prefix => tag.tag.startsWith(prefix))
      );
      
      if (tagsToDelete.length > 0) {
        const itemTitle = item.getField("title") || "无标题";
        previewTags.push({
          title: itemTitle.substring(0, 50) + (itemTitle.length > 50 ? "…" : ""),
          tags: tagsToDelete.map(t => t.tag)
        });
      }
    } catch (e) {
      console.error("预览处理失败:", e);
    }
  }
  
  // 显示预览信息
  if (previewTags.length === 0) {
    return `未找到以 [${prefixList}] 开头的标签`;
  }
  
  let previewMessage = `找到 ${previewTags.length} 个条目包含以 [${prefixList}] 开头的标签：\n\n`;
  let totalTagsToDelete = 0;
  
  for (let preview of previewTags.slice(0, 5)) { // 只显示前5个条目
    previewMessage += `📄 ${preview.title}\n`;
    for (let tag of preview.tags) {
      previewMessage += `   🏷️ ${tag}\n`;
      totalTagsToDelete++;
    }
    previewMessage += "\n";
  }
  
  if (previewTags.length > 5) {
    previewMessage += `… 还有 ${previewTags.length - 5} 个条目\n\n`;
    for (let i = 5; i < previewTags.length; i++) {
      totalTagsToDelete += previewTags[i].tags.length;
    }
  }
  
  previewMessage += `总共将删除 ${totalTagsToDelete} 个标签\n\n确认执行删除操作吗？`;
  
  const finalConfirm = confirm(previewMessage);
  if (!finalConfirm) {
    return "操作取消：用户查看预览后选择不删除";
  }
  
  // 执行实际删除操作
  for (let item of items) {
    try {
      // 获取条目的所有标签
      const existingTags = item.getTags();
      
      // 找到所有匹配任意前缀的标签
      const tagsToDelete = existingTags.filter(tag => 
        tag.tag && tagPrefixes.some(prefix => tag.tag.startsWith(prefix))
      );
      
      if (tagsToDelete.length === 0) {
        // 没有找到匹配的标签，跳过
        skipCount++;
        continue;
      }
      
      // 删除匹配的标签
      let itemDeletedCount = 0;
      for (let tagObj of tagsToDelete) {
        try {
          item.removeTag(tagObj.tag);
          itemDeletedCount++;
          deletedTagsCount++;
        } catch (e) {
          console.error(`删除标签失败: ${tagObj.tag}`, e);
        }
      }
      
      if (itemDeletedCount > 0) {
        try {
          await item.saveTx();
          processedCount++;
        } catch (e) {
          errorCount++;
          console.error("保存条目失败:", e);
        }
      }
      
    } catch (e) {
      errorCount++;
      console.error("处理条目失败:", e);
    }
  }
  
  return `标签删除完成！\n前缀: [${prefixList}]\n处理条目: ${processedCount}\n删除标签: ${deletedTagsCount}\n跳过条目: ${skipCount}\n错误: ${errorCount}`;
})()
}$
```
