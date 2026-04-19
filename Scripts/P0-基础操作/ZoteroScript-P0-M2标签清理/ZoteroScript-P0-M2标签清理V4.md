---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P0-M2标签清理V4
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
- Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord: ''
---

## ZoteroScript-P0-M2 标签清理 V4

```Java
#🏷️ClearTags[color=#ff6b6b][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let processedItemsCount = 0;    // 处理的条目数
  let totalRemovedTags = 0;       // 删除的标签总数
  let errorCount = 0;             // 处理失败的条目数
  
  // 定义允许保留的标签前缀
  const allowedPrefixes = [
    "/", 
    "Item/", 
    "sMeth/",           // 研究方法标签
    "Sample/", 
    "Result/", 
    "Theory/",
    "Scholar/",         // 学者标签
    "A1-DV/",           // 因变量标签
    "A2-IV/",           // 自变量标签
    "A3-MO/",           // 调节变量标签
    "A4-ME/",           // 中介变量标签
    "A5-INV/",          // 工具变量标签
    "A6-CV/",           // 控制变量标签
    "V1-def/",          // 变量定义标签
    "V2-mea/",          // 测量方法标签
    "V3-con/",          // 研究贡献标签
    "V4-fut/"           // 研究展望标签
  ];
  
  for (let item of items) {
    try {
      const existingTags = item.getTags();
      let removedTagsCount = 0;
      let hasChanges = false;
      
      // 检查每个标签
      for (let tagObj of existingTags) {
        const tagName = tagObj.tag;
        
        // 检查标签是否以允许的前缀开头
        const isAllowedPrefix = allowedPrefixes.some(prefix => tagName.startsWith(prefix));
        
        // 检查标签是否以字母或数字开头（需要删除的）
        const startsWithAlphanumeric = /^[a-zA-Z0-9]/.test(tagName);
        
        // 只删除以字母或数字开头且不符合允许前缀的标签
        if (startsWithAlphanumeric && !isAllowedPrefix) {
          // 删除不符合要求的标签
          try {
            item.removeTag(tagName);
            removedTagsCount++;
            hasChanges = true;
          } catch {
            // 单个标签删除失败不影响其他标签
          }
        }
      }
      
      // 如果有变更，保存条目
      if (hasChanges) {
        try {
          await item.saveTx();
          processedItemsCount++;
          totalRemovedTags += removedTagsCount;
        } catch {
          errorCount++;
        }
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `标签清理完成 - 处理条目: ${processedItemsCount}, 删除标签: ${totalRemovedTags}, 失败条目: ${errorCount}`;
})()
}$
/**
 * 标签清理工具说明：
 * 
 * 1. 功能：清理条目中不符合规范的标签
 * 
 * 2. 保留的标签类型（共14类前缀）：
 *    - "/" : 特殊标记标签
 *    - "Item/" : 主题标签
 *    - "sMeth/" : 研究方法标签
 *    - "Sample/" : 样本标签
 *    - "Result/" : 研究结果标签
 *    - "Theory/" : 理论标签
 *    - "Scholar/" : 学者标签
 *    
 *    A系列（变量标签，6个）：
 *    - "A1-DV/" : 因变量标签
 *    - "A2-IV/" : 自变量标签
 *    - "A3-MO/" : 调节变量标签
 *    - "A4-ME/" : 中介变量标签
 *    - "A5-INV/" : 工具变量标签
 *    - "A6-CV/" : 控制变量标签
 *    
 *    V系列（条目信息标签，4个）：
 *    - "V1-def/" : 变量定义标签
 *    - "V2-mea/" : 测量方法标签
 *    - "V3-con/" : 研究贡献标签
 *    - "V4-fut/" : 研究展望标签
 * 
 * 3. 删除规则：
 *    - 删除以字母或数字开头的标签
 *    - 但保留符合上述14类前缀的标签
 *    - 不删除以特殊符号（如 emoji、中文等）开头的标签
 * 
 * 4. 处理逻辑：
 *    - 遍历所有选中的条目
 *    - 检查每个条目的所有标签
 *    - 按规则删除不符合要求的标签
 *    - 保存修改后的条目
 * 
 * 5. 统计信息：
 *    - 处理条目数：成功修改的条目数量
 *    - 删除标签数：总共删除的标签数量
 *    - 失败条目数：处理失败的条目数量
 * 
 * 6. 使用场景：
 *    - 批量导入文献后清理自动生成的标签
 *    - 删除不规范的标签，保留结构化标签
 *    - 维护标签系统的一致性和规范性
 * 
 * 7. 安全特性：
 *    - 只删除明确符合规则的标签
 *    - 保留所有规范的标签系统
 *    - 单个标签删除失败不影响其他标签
 *    - 提供详细的处理统计信息
 * 
 * 8. 标签系统完整性：
 *    现在保护了完整的标签体系：
 *    - 基础标签（7类）
 *    - A系列变量标签（6类）
 *    - V系列条目信息标签（4类）
 *    共14类标签前缀受到保护
 */
```
