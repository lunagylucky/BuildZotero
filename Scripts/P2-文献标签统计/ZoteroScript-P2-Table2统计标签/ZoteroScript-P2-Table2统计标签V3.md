---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P2-Table2统计标签V3
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
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P2-Table2 统计标签 V3

```javascript
# Zotero-Script-P1-Tag8 表格标签 4
#🏷️Ttags[color=#2196F3][trigger=]  
${  
(async () => {  
  const items = ZoteroPane.getSelectedItems();  
  if (!items || items.length === 0) return " 未选择任何条目 ";
  
  // 定义标签映射  
  const tagMapping = {  
    // 1. 变量标签（A 系列）  
    "1-1": "A1-DV/",  
    "1-2": "A2-IV/",  
    "1-3": "A3-MO/",  
    "1-4": "A4-ME/",  
    "1-5": "A5-INV/",  
    "1-6": "A6-CV/",  
    "1": ["A1-DV/", "A2-IV/", "A3-MO/", "A4-ME/", "A5-INV/", "A6-CV/"],
    
    // 2. 主题标签
    "2": "Item/",
    
    // 3. 理论标签
    "3": "Theory/",
    
    // 4. 结果标签
    "4": "Result/",
    
    // 5. 样本标签
    "5-1": "Sample/1",
    "5-2": "Sample/2",
    "5-3": "Sample/3",
    "5": ["Sample/1", "Sample/2", "Sample/3"],
    
    // 6. 方法标签
    "6-1": "sMeth/1",
    "6-2": "sMeth/2",
    "6-3": "sMeth/3",
    "6-4": "sMeth/4",
    "6-5": "sMeth/5",
    "6-6": "sMeth/6",
    "6": ["sMeth/1", "sMeth/2", "sMeth/3", "sMeth/4", "sMeth/5", "sMeth/6"],
    
    // 7. 条目细节标签（研究要素）
    "7-1": "V1-def/",
    "7-2": "V2-mea/",
    "7-3": "V3-con/",
    "7-4": "V4-fut/",
    "7": ["V1-def/", "V2-mea/", "V3-con/", "V4-fut/"]
  };
  
  // 用户输入选择  
  const userInput = prompt(  
    " 请输入要统计的标签类型（多个用逗号分隔）：\n\n" +  
    "【数字选项】\n" +  
    "1 - 变量标签（全部 6 个）\n" +  
    "  1-1 因变量 (A1-DV)  1-2 自变量 (A2-IV)  1-3 调节变量 (A3-MO)\n" +  
    "  1-4 中介变量 (A4-ME)  1-5 工具变量 (A5-INV)  1-6 控制变量 (A6-CV)\n\n" +  
    "2 - 主题标签 (Item)\n" +  
    "3 - 理论标签 (Theory)\n" +  
    "4 - 结果标签 (Result)\n\n" +  
    "5 - 样本标签（全部 3 个）\n" +  
    "  5-1 样本 1  5-2 样本 2  5-3 样本 3\n\n" +  
    "6 - 方法标签（全部 6 个）\n" +  
    "  6-1 方法 1  6-2 方法 2  6-3 方法 3\n" +  
    "  6-4 方法 4  6-5 方法 5  6-6 方法 6\n\n" +  
    "7 - 条目细节标签（研究要素，全部 4 个）\n" +  
    "  7-1 变量定义 (V1-def)  7-2 测量方法 (V2-mea)\n" +  
    "  7-3 研究贡献 (V3-con)  7-4 研究展望 (V4-fut)\n\n" +  
    "【自定义前缀】\n" +  
    " 也可直接输入前缀，如：sMeth/, Theory/, Sample/1 宏观\n\n" +  
    " 示例：1,3,4 或 1-1,1-2 或 Theory/, Item/ 或 混合使用: 1,Theory/"  
  );
  
  // 检查用户输入  
  if (!userInput || userInput.trim() === "") {  
    return " 操作取消：未输入标签类型 ";  
  }
  
  // 解析用户输入  
  const userChoices = userInput.split(',').map(choice => choice.trim()).filter(choice => choice !== "");
  
  if (userChoices.length === 0) {  
    return " 操作取消：未输入有效的标签类型 ";  
  }
  
  // 根据用户选择构建标签前缀列表  
  const tagPrefixes = [];  
  const selectedDescriptions = []; // 用于显示选择的描述
  
  for (let choice of userChoices) {  
    if (tagMapping[choice]) {  
      // 数字选项  
      if (Array.isArray(tagMapping[choice])) {  
        tagPrefixes.push(...tagMapping[choice]);  
        selectedDescriptions.push(`选项${choice}(${tagMapping[choice].join(', ')})`);  
      } else {  
        tagPrefixes.push(tagMapping[choice]);  
        selectedDescriptions.push(`选项${choice}(${tagMapping[choice]})`);  
      }  
    } else {  
      // 自定义前缀  
      tagPrefixes.push(choice);  
      selectedDescriptions.push(`自定义前缀(${choice})`);  
    }  
  }
  
  // 去重  
  const uniquePrefixes = [...new Set(tagPrefixes)];
  
  // 使用 Map 记录每个前缀下的标签及其详细信息  
  const prefixGroups = new Map();
  
  // 初始化每个前缀的 Map  
  for (let prefix of uniquePrefixes) {  
    prefixGroups.set(prefix, new Map());  
  }
  
  // 使用 items2documents 获取文档信息并插入到可视化辅助视图  
  const docs = Meet.Zotero.items2documents(items);  
  Meet.Global.views.insertAuxiliary(docs);
  
  // 创建 item 到索引的映射  
  const itemIndexMap = new Map();  
  items.forEach((item, index) => {  
    itemIndexMap.set(item.id, index);  
  });
  
  // 遍历所有选中的条目  
  for (let item of items) {  
    try {  
      const itemIndex = itemIndexMap.get(item.id);  
      const tags = item.getTags();  
      if (!tags || tags.length === 0) continue;
      
      // 获取条目的元数据
      let year = "";
      let journal = "";
      let author = "";
      
      try {
        // 获取年份
        const date = item.getField("date");
        if (date) {
          const yearMatch = date.match(/\d{4}/);
          if (yearMatch) {
            year = yearMatch[0];
          }
        }
      } catch {}
      
      try {
        // 获取期刊名称
        journal = item.getField("publicationTitle") || "";
      } catch {}
      
      try {
        // 获取第一作者完整姓名
        const creators = item.getCreators();
        if (creators && creators.length > 0) {
          const firstAuthor = creators[0];
          const firstName = firstAuthor.firstName || "";
          const lastName = firstAuthor.lastName || firstAuthor.name || "";
          author = firstName ? `${firstName} ${lastName}` : lastName;
        }
      } catch {}
      
      for (let tagObj of tags) {
        const tag = tagObj.tag;
        
        // 检查标签是否匹配任何一个前缀
        for (let prefix of uniquePrefixes) {
          if (tag && tag.startsWith(prefix)) {
            // 去掉前缀
            let tagContent = tag.substring(prefix.length);
            
            // 对所有前缀的标签内容移除起始序号（忽略如 "1"、"２" 及常见分隔符）
            let normalizedContent = tagContent.replace(/^[0-9\uFF10-\uFF19]+[\-_.、\s]*/u, "");
            
            // 获取该前缀的Map
            let tagsMap = prefixGroups.get(prefix);
            
            // 如果标签内容已存在，更新信息
            if (tagsMap.has(normalizedContent)) {
              let existingData = tagsMap.get(normalizedContent);
              existingData.count++;
              
              // 添加年份（如果有且不重复）
              if (year && !existingData.years.includes(year)) {
                existingData.years.push(year);
              }
              
              // 添加期刊（如果有且不重复）
              if (journal && !existingData.journals.includes(journal)) {
                existingData.journals.push(journal);
              }
              
              // 添加作者（如果有且不重复）
              if (author && !existingData.authors.includes(author)) {
                existingData.authors.push(author);
              }
              
              // 添加文献编号（如果有且不重复）
              if (!existingData.itemIndices.includes(itemIndex)) {
                existingData.itemIndices.push(itemIndex);
              }
            } else {
              // 新标签，创建记录
              tagsMap.set(normalizedContent, {
                count: 1,
                years: year ? [year] : [],
                journals: journal ? [journal] : [],
                authors: author ? [author] : [],
                itemIndices: [itemIndex]
              });
            }
          }
        }
      }
    } catch (error) {
      console.error("处理条目时出错:", error);
    }
  }
  
  // 构建输出结果  
  let result = '';
  
  // 统计信息：条目总数与各分组的不同标签数量  
  let summary = '### 统计信息\n\n';  
  summary += `- 条目总数：${items.length}\n`;  
  summary += `- 分组不同标签数：\n`;  
  for (let prefix of uniquePrefixes) {  
    const tagsMap = prefixGroups.get(prefix);  
    const distinctCount = tagsMap ? tagsMap.size : 0;  
    summary += `  - ${prefix} ${distinctCount}\n`;  
  }  
  summary += `\n`;  
  result += summary;
  
  for (let prefix of uniquePrefixes) {  
    const tagsMap = prefixGroups.get(prefix);
    
    if (tagsMap.size === 0) continue;
    
    // 按出现次数排序(从高到低)
    const tagsList = Array.from(tagsMap.entries()).sort((a, b) => b[1].count - a[1].count);
    
    // 添加分组标题
    result += `## ${prefix}\n\n`;
    result += `| 序号 | 标签内容 | 次数 | 年份 | 文献编号 |\n|------|----------|------|------|------|\n`;
    
    tagsList.forEach(([tagContent, data], index) => {
      // 年份排序并用逗号连接
      const yearsStr = data.years.length > 0 
        ? data.years.sort().join(', ') 
        : '-';
      
      // 将索引转换为引用格式 [1], [2], [3]
      const citations = data.itemIndices
        .sort((a, b) => a - b)
        .map(idx => `[${idx + 1}]`)
        .join(', ');
      
      result += `| ${index + 1} | ${tagContent} | ${data.count} | ${yearsStr} | ${citations} |\n`;
    });
    
    result += `\n`;
  }
  
  if (result === '') {  
    result = ' 未找到匹配的标签 ';  
  }
  
  console.log(result);  
  return result;  
})()  
}$  
/**
 * 输出要求说明：  
 请直接输出包含| 序号 | 标签内容 | 次数 | 年份 | 文献编号 | 表格结果，不可有省略。标签内容不要做更改  
 文献编号与右侧可视化界面中的编号对应，点击右侧文献可查看详细信息

```
