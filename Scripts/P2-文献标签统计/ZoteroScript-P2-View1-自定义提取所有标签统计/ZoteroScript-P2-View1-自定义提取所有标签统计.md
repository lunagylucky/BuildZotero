---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P2-View1-自定义提取所有标签统计
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
- 代码
- 批量处理
- 文献管理
- 自动化脚本
- HTML清理
- JavaScript
- Zotero
- Pattern/Method
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P2-View1- 自定义提取所有标签统计

```javascript
#🏷️FindCustomTags[color=#2196F3][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  // 通过用户输入获取要查找的标签前缀，支持多个前缀
  const userInput = prompt("请输入要查找的标签前缀，多个前缀用逗号分隔\n例如：Theory/, sMeth/, Sample/, Item/, Result/\n或者：项目A, 类别B, 标签C");
  
  // 检查用户输入
  if (!userInput || userInput.trim() === "") {
    return "操作取消：未输入标签前缀";
  }
  
  // 解析多个标签前缀
  const tagPrefixes = userInput.split(',').map(prefix => prefix.trim()).filter(prefix => prefix !== "");
  
  if (tagPrefixes.length === 0) {
    return "操作取消：未输入有效的标签前缀";
  }
  
  // 使用Map记录每个前缀下的标签及其出现次数
  const prefixGroups = new Map();
  
  // 初始化每个前缀的Map
  for (let prefix of tagPrefixes) {
    prefixGroups.set(prefix, new Map());
  }
  
  // 遍历所有选中的条目
  for (let item of items) {
    try {
      const tags = item.getTags();
      if (!tags || tags.length === 0) continue;
      
      for (let tagObj of tags) {
        const tag = tagObj.tag;
        
        // 检查标签是否匹配任何一个前缀
        for (let prefix of tagPrefixes) {
          if (tag && tag.startsWith(prefix)) {
            // 去掉前缀
            let tagContent = tag.substring(prefix.length);
            
            // 获取该前缀的Map
            let tagsMap = prefixGroups.get(prefix);
            tagsMap.set(tagContent, (tagsMap.get(tagContent) || 0) + 1);
          }
        }
      }
    } catch (error) {
      console.error("处理条目时出错:", error);
    }
  }
  
  // 构建输出结果
  let result = '';
  
  for (let prefix of tagPrefixes) {
    const tagsMap = prefixGroups.get(prefix);
    
    if (tagsMap.size === 0) continue;
    
    // 按出现次数排序(从高到低)
    const tagsList = Array.from(tagsMap.entries()).sort((a, b) => b[1] - a[1]);
    
    // 添加分组标题
    result += `## ${prefix}\n\n`;
    result += `| 序号 | 标签内容 | 次数 |\n|------|----------|------|\n`;
    
    tagsList.forEach(([tagContent, count], index) => {
      result += `| ${index + 1} | ${tagContent} | ${count} |\n`;
    });
    
    result += `\n`;
  }
  
  if (result === '') {
    result = '未找到匹配的标签';
  }
  
  console.log(result);
  return result;
})()
}$

/**
 * 输出要求说明：
 * 
 * 1. 用户输入：支持多个标签前缀，用逗号分隔
 *    示例：Theory/, sMeth/, Sample/
 * 
 * 2. 输出格式：按前缀分组的 Markdown 表格
 *    - 每个前缀使用二级标题（## 前缀名）
 *    - 表格包含三列：序号、标签内容、次数
 *    - 标签内容不包含前缀部分
 * 
 * 3. 排序规则：每组内按出现次数从高到低排序
 * 
 * 4. 输出内容：
 *    - 提取所有匹配前缀的标签
 *    - 去除标签前缀，仅保留内容部分
 *    - 统计每个标签内容的出现次数
 *    - 按前缀分组展示
 * 
 * 5. 示例输出：
 *    ## Theory/
 *    | 序号 | 标签内容 | 次数 |
 *    |------|----------|------|
 *    | 1 | Dynamic Capability | 5 |
 *    | 2 | Resource-Based View | 3 |
 *    
 *    ## sMeth/
 *    | 序号 | 标签内容 | 次数 |
 *    |------|----------|------|
 *    | 1 | Case Study | 8 |
 *    | 2 | Survey | 4 |
 * 
 * 6. 重要指令：
 *    - 仅输出分组表格
 *    - 不做额外分析、解释或说明
 *    - 不添加总结性文字
 *    - 直接呈现原始统计结果
 */
```
