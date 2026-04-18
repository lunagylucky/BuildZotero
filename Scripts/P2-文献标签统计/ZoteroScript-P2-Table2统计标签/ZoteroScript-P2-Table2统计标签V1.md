---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P2-Table2统计标签V1
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
  - 代码
  - 批量处理
  - 文献管理
  - 自动化脚本
  - HTML清理
  - JavaScript
  - Zotero
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P2-Table2 统计标签 V1

```javascript
#🏷️Ttags[color=#2196F3][trigger=]
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
  
  // 使用Map记录每个前缀下的标签及其详细信息
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
        for (let prefix of tagPrefixes) {
          if (tag && tag.startsWith(prefix)) {
            // 去掉前缀
            let tagContent = tag.substring(prefix.length);
            
            // 获取该前缀的Map
            let tagsMap = prefixGroups.get(prefix);
            
            // 如果标签内容已存在，更新信息
            if (tagsMap.has(tagContent)) {
              let existingData = tagsMap.get(tagContent);
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
            } else {
              // 新标签，创建记录
              tagsMap.set(tagContent, {
                count: 1,
                years: year ? [year] : [],
                journals: journal ? [journal] : [],
                authors: author ? [author] : []
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
  
  for (let prefix of tagPrefixes) {
    const tagsMap = prefixGroups.get(prefix);
    
    if (tagsMap.size === 0) continue;
    
    // 按出现次数排序(从高到低)
    const tagsList = Array.from(tagsMap.entries()).sort((a, b) => b[1].count - a[1].count);
    
    // 添加分组标题
    result += `## ${prefix}\n\n`;
    result += `| 序号 | 标签内容 | 次数 | 年份 | 期刊 | 作者 |\n|------|----------|------|------|------|------|\n`;
    
    tagsList.forEach(([tagContent, data], index) => {
      // 年份排序并用逗号连接
      const yearsStr = data.years.length > 0 
        ? data.years.sort().join(', ') 
        : '-';
      
      // 构建期刊-年份映射和作者-年份映射
      const journalYearMap = new Map();
      const authorYearMap = new Map();
      
      // 重新遍历所有条目以建立期刊-年份和作者-年份映射
      for (let item of items) {
        try {
          const tags = item.getTags();
          if (!tags || tags.length === 0) continue;
          
          let year = "";
          let journal = "";
          let author = "";
          
          try {
            const date = item.getField("date");
            if (date) {
              const yearMatch = date.match(/\d{4}/);
              if (yearMatch) year = yearMatch[0];
            }
          } catch {}
          
          try {
            journal = item.getField("publicationTitle") || "";
          } catch {}
          
          try {
            const creators = item.getCreators();
            if (creators && creators.length > 0) {
              const firstAuthor = creators[0];
              // 构建完整姓名
              const firstName = firstAuthor.firstName || "";
              const lastName = firstAuthor.lastName || firstAuthor.name || "";
              author = firstName ? `${firstName} ${lastName}` : lastName;
            }
          } catch {}
          
          // 检查该条目是否有当前标签
          const hasCurrentTag = tags.some(t => t.tag === prefix + tagContent);
          
          if (hasCurrentTag && journal && year) {
            if (!journalYearMap.has(journal)) {
              journalYearMap.set(journal, []);
            }
            if (!journalYearMap.get(journal).includes(year)) {
              journalYearMap.get(journal).push(year);
            }
          }
          
          if (hasCurrentTag && author && year) {
            if (!authorYearMap.has(author)) {
              authorYearMap.set(author, []);
            }
            if (!authorYearMap.get(author).includes(year)) {
              authorYearMap.get(author).push(year);
            }
          }
        } catch {}
      }
      
      // 格式化期刊信息为 "期刊名称(年份1,年份2)"
      const journalsStr = journalYearMap.size > 0
        ? Array.from(journalYearMap.entries())
            .map(([journal, years]) => `${journal}(${years.sort().join(',')})`)
            .join('; ')
        : '-';
      
      // 格式化作者信息为 "作者全名(年份1,年份2)"
      const authorsStr = authorYearMap.size > 0
        ? Array.from(authorYearMap.entries())
            .map(([author, years]) => `${author}(${years.sort().join(',')})`)
            .join('; ')
        : '-';
      
      result += `| ${index + 1} | ${tagContent} | ${data.count} | ${yearsStr} | ${journalsStr} | ${authorsStr} |\n`;
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
 * 2. 输出格式：按前缀分组的 Markdown 表格（包含年份、期刊和作者列）
 *    - 每个前缀使用二级标题（## 前缀名）
 *    - 表格包含六列：序号、标签内容、次数、年份、期刊、作者
 *    - 标签内容不包含前缀部分
 * 
 * 3. 排序规则：每组内按出现次数从高到低排序
 * 
 * 4. 元数据提取：
 *    - 年份：从date字段提取，自动排序，用逗号分隔
 *    - 期刊：从publicationTitle字段提取，格式为"期刊名称(年份1,年份2)"
 *    - 作者：从creators字段提取第一作者完整姓名，格式为"作者全名(年份1,年份2)"
 *    - 自动去重，同一标签的不同条目会合并年份、期刊和作者信息
 * 
 * 5. 输出内容：
 *    - 提取所有匹配前缀的标签
 *    - 去除标签前缀，仅保留内容部分
 *    - 统计每个标签内容的出现次数
 *    - 收集该标签所有相关条目的年份、期刊和作者
 *    - 按前缀分组展示
 * 
 * 6. 示例输出：
 *    ## Theory/
 *    | 序号 | 标签内容 | 次数 | 年份 | 期刊 | 作者 |
 *    |------|----------|------|------|------|------|
 *    | 1 | Dynamic Capability Theory | 4 | 2020, 2021, 2022 | Strategic Management Journal(2020,2021); Academy of Management Journal(2022) | David J. Teece(2020); Kathleen M. Eisenhardt(2021); Catherine L. Wang(2022) |
 *    | 2 | Resource-Based View | 3 | 2019, 2021 | Journal of Management(2019); Organization Science(2021) | Jay B. Barney(2019); Robert M. Grant(2021) |
 *    
 *    ## sMeth/
 *    | 序号 | 标签内容 | 次数 | 年份 | 期刊 | 作者 |
 *    |------|----------|------|------|------|------|
 *    | 1 | Case Study | 8 | 2018, 2019, 2020, 2021 | Management Science(2018,2020); Administrative Science Quarterly(2019,2021) | Robert K. Yin(2018); Kathleen M. Eisenhardt(2019,2020); Ann Langley(2021) |
 *    | 2 | Survey | 4 | 2020, 2022 | Journal of Applied Psychology(2020,2022) | Philip M. Podsakoff(2020); Joseph F. Hair(2022) |
 * 
 * 7. 重要指令：
 *    - 仅输出分组表格
 *    - 不做额外分析、解释或说明
 *    - 不添加总结性文字
 *    - 直接呈现原始统计结果
 */
```
