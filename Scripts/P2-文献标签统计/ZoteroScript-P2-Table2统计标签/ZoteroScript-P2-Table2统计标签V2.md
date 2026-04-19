---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P2-Table2统计标签V2
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

## ZoteroScript-P2-Table2 统计标签 V2

```javascript
#🏷️Ttags[color=#2196F3][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  // 定义标签映射
  const tagMapping = {
    // 1. 变量标签
    "1-1": "IV/",
    "1-2": "DV/",
    "1-3": "MV/",
    "1-4": "Mod/",
    "1-5": "Med/",
    "1-6": "CV/",
    "1": ["IV/", "DV/", "MV/", "Mod/", "Med/", "CV/"],
    
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
    "6": ["sMeth/1", "sMeth/2", "sMeth/3", "sMeth/4", "sMeth/5", "sMeth/6"]
  };
  
  // 用户输入选择
  const userInput = prompt(
    "请输入要统计的标签类型（多个用逗号分隔）：\n\n" +
    "1 - 变量标签（全部6个）\n" +
    "  1-1 自变量(IV)  1-2 因变量(DV)  1-3 调节变量(MV)\n" +
    "  1-4 调节变量(Mod)  1-5 中介变量(Med)  1-6 控制变量(CV)\n\n" +
    "2 - 主题标签(Item)\n" +
    "3 - 理论标签(Theory)\n" +
    "4 - 结果标签(Result)\n\n" +
    "5 - 样本标签（全部3个）\n" +
    "  5-1 样本1  5-2 样本2  5-3 样本3\n\n" +
    "6 - 方法标签（全部6个）\n" +
    "  6-1 方法1  6-2 方法2  6-3 方法3\n" +
    "  6-4 方法4  6-5 方法5  6-6 方法6\n\n" +
    "示例：1,3,4 或 1-1,1-2,3,5-1"
  );
  
  // 检查用户输入
  if (!userInput || userInput.trim() === "") {
    return "操作取消：未输入标签类型";
  }
  
  // 解析用户输入
  const userChoices = userInput.split(',').map(choice => choice.trim()).filter(choice => choice !== "");
  
  if (userChoices.length === 0) {
    return "操作取消：未输入有效的标签类型";
  }
  
  // 根据用户选择构建标签前缀列表
  const tagPrefixes = [];
  for (let choice of userChoices) {
    if (tagMapping[choice]) {
      if (Array.isArray(tagMapping[choice])) {
        // 如果是数组（如"1"对应多个前缀），添加所有前缀
        tagPrefixes.push(…tagMapping[choice]);
      } else {
        // 如果是单个前缀
        tagPrefixes.push(tagMapping[choice]);
      }
    } else {
      return `错误：无效的选择 "${choice}"`;
    }
  }
  
  // 去重
  const uniquePrefixes = […new Set(tagPrefixes)];
  
  // 使用Map记录每个前缀下的标签及其详细信息
  const prefixGroups = new Map();
  
  // 初始化每个前缀的Map
  for (let prefix of uniquePrefixes) {
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
        for (let prefix of uniquePrefixes) {
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
  
  for (let prefix of uniquePrefixes) {
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
 * 1. 用户输入：支持数字编码选择标签类型，可组合多个
 *    - 1 = 所有变量标签 (1-1到1-6)
 *      1-1=IV/, 1-2=DV/, 1-3=MV/, 1-4=Mod/, 1-5=Med/, 1-6=CV/
 *    - 2 = 主题标签 (Item/)
 *    - 3 = 理论标签 (Theory/)
 *    - 4 = 结果标签 (Result/)
 *    - 5 = 所有样本标签 (5-1到5-3)
 *      5-1=Sample/1, 5-2=Sample/2, 5-3=Sample/3
 *    - 6 = 所有方法标签 (6-1到6-6)
 *      6-1到6-6=sMeth/1到sMeth/6
 *    
 *    示例输入：
 *    - "1,3" = 所有变量标签 + 理论标签
 *    - "1-1,1-2,3" = IV + DV + 理论标签
 *    - "3,4,5" = 理论 + 结果 + 所有样本标签
 * 
 * 2. 输出格式：按前缀分组的 Markdown 表格
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
 * 5. 标签映射关系：
 *    变量标签：1-1(IV/), 1-2(DV/), 1-3(MV/), 1-4(Mod/), 1-5(Med/), 1-6(CV/)
 *    主题标签：2(Item/)
 *    理论标签：3(Theory/)
 *    结果标签：4(Result/)
 *    样本标签：5-1(Sample/1), 5-2(Sample/2), 5-3(Sample/3)
 *    方法标签：6-1到6-6(sMeth/1到sMeth/6)
 * 
 * 6. 重要指令：
 *    - 仅输出分组表格(必须完整输出,不可采用省略形式)
 *    - 不做额外分析、解释或说明
 *    - 不添加总结性文字
 *    - 直接呈现原始统计结果
 */
```
