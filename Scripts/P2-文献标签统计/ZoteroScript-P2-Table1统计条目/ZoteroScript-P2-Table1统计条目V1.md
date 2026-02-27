---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P2-Table1统计条目V1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 12:15
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


## ZoteroScript-P2-Table1 条目表格 V1

```javascript
#🏷️Titems[color=#2196F3][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  // 用户选择输出模式
  const modeInput = prompt(
    "请选择输出模式（输入数字）：\n\n" +
    "1 - 变量(6列) + 理论(1列)\n" +
    "2 - 变量(6列) + 理论(1列) + 样本(3列)\n" +
    "3 - 变量(6列) + 理论(1列) + 结果(1列)\n" +
    "4 - 变量(6列) + 理论(1列) + 结果(1列) + 样本(3列)\n" +
    "5 - 结果(1列) + 样本(3列) + 方法(1列)\n" +
    "6 - 全列(所有18列)"
  );
  
  // 检查用户输入
  if (!modeInput || !["1", "2", "3", "4", "5", "6"].includes(modeInput.trim())) {
    return "操作取消：未选择有效的输出模式";
  }
  
  const mode = modeInput.trim();
  
  // 定义标签前缀
  const tagPrefixes = {
    iv: "IV/",
    dv: "DV/",
    mv: "MV/",
    mod: "Mod/",
    med: "Med/",
    cv: "CV/",
    other: "OV/",
    item: "Item/",
    result: "Result/",
    theory: "Theory/",
    sample1: "Sample/1",
    sample2: "Sample/2",
    sample3: "Sample/3",
    method: "sMeth/"
  };
  
  // 收集所有条目信息
  const itemsData = [];
  
  for (let item of items) {
    try {
      // 获取基本元数据
      let author = "";
      let year = "";
      let journal = "";
      
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
      
      // 获取标签并分类
      const tags = item.getTags();
      const categorizedTags = {
        iv: [],
        dv: [],
        mv: [],
        mod: [],
        med: [],
        cv: [],
        other: [],
        item: [],
        result: [],
        theory: [],
        sample1: [],
        sample2: [],
        sample3: [],
        method: []
      };
      
      if (tags && tags.length > 0) {
        for (let tagObj of tags) {
          const tag = tagObj.tag;
          
          // 检查并分类标签
          if (tag.startsWith(tagPrefixes.iv)) {
            categorizedTags.iv.push(tag.substring(tagPrefixes.iv.length));
          } else if (tag.startsWith(tagPrefixes.dv)) {
            categorizedTags.dv.push(tag.substring(tagPrefixes.dv.length));
          } else if (tag.startsWith(tagPrefixes.mv)) {
            categorizedTags.mv.push(tag.substring(tagPrefixes.mv.length));
          } else if (tag.startsWith(tagPrefixes.mod)) {
            categorizedTags.mod.push(tag.substring(tagPrefixes.mod.length));
          } else if (tag.startsWith(tagPrefixes.med)) {
            categorizedTags.med.push(tag.substring(tagPrefixes.med.length));
          } else if (tag.startsWith(tagPrefixes.cv)) {
            categorizedTags.cv.push(tag.substring(tagPrefixes.cv.length));
          } else if (tag.startsWith(tagPrefixes.other)) {
            categorizedTags.other.push(tag.substring(tagPrefixes.other.length));
          } else if (tag.startsWith(tagPrefixes.item)) {
            categorizedTags.item.push(tag.substring(tagPrefixes.item.length));
          } else if (tag.startsWith(tagPrefixes.result)) {
            categorizedTags.result.push(tag.substring(tagPrefixes.result.length));
          } else if (tag.startsWith(tagPrefixes.theory)) {
            categorizedTags.theory.push(tag.substring(tagPrefixes.theory.length));
          } else if (tag.startsWith(tagPrefixes.sample1)) {
            categorizedTags.sample1.push(tag.substring(tagPrefixes.sample1.length));
          } else if (tag.startsWith(tagPrefixes.sample2)) {
            categorizedTags.sample2.push(tag.substring(tagPrefixes.sample2.length));
          } else if (tag.startsWith(tagPrefixes.sample3)) {
            categorizedTags.sample3.push(tag.substring(tagPrefixes.sample3.length));
          } else if (tag.startsWith(tagPrefixes.method)) {
            categorizedTags.method.push(tag.substring(tagPrefixes.method.length));
          }
        }
      }
      
      // 添加到结果数组
      itemsData.push({
        author: author || "-",
        year: year || "-",
        journal: journal || "-",
        iv: categorizedTags.iv.length > 0 ? categorizedTags.iv.join('; ') : "-",
        dv: categorizedTags.dv.length > 0 ? categorizedTags.dv.join('; ') : "-",
        mv: categorizedTags.mv.length > 0 ? categorizedTags.mv.join('; ') : "-",
        mod: categorizedTags.mod.length > 0 ? categorizedTags.mod.join('; ') : "-",
        med: categorizedTags.med.length > 0 ? categorizedTags.med.join('; ') : "-",
        cv: categorizedTags.cv.length > 0 ? categorizedTags.cv.join('; ') : "-",
        other: categorizedTags.other.length > 0 ? categorizedTags.other.join('; ') : "-",
        item: categorizedTags.item.length > 0 ? categorizedTags.item.join('; ') : "-",
        result: categorizedTags.result.length > 0 ? categorizedTags.result.join('; ') : "-",
        theory: categorizedTags.theory.length > 0 ? categorizedTags.theory.join('; ') : "-",
        sample1: categorizedTags.sample1.length > 0 ? categorizedTags.sample1.join('; ') : "-",
        sample2: categorizedTags.sample2.length > 0 ? categorizedTags.sample2.join('; ') : "-",
        sample3: categorizedTags.sample3.length > 0 ? categorizedTags.sample3.join('; ') : "-",
        method: categorizedTags.method.length > 0 ? categorizedTags.method.join('; ') : "-"
      });
      
    } catch (error) {
      console.error("处理条目时出错:", error);
    }
  }
  
  // 根据模式构建输出结果
  let result = '## 文献条目详情表\n\n';
  let headerRow = '';
  let separatorRow = '';
  
  // 基础列（所有模式都包含）
  const baseColumns = ['序号', '第一作者', '年份', '期刊'];
  
  // 根据模式定义列
  let columns = […baseColumns];
  
  switch(mode) {
    case "1": // 变量(6列) + 理论(1列)
      columns.push('IV', 'DV', 'MV', 'Mod', 'Med', 'CV', 'Theory');
      break;
    case "2": // 变量(6列) + 理论(1列) + 样本(3列)
      columns.push('IV', 'DV', 'MV', 'Mod', 'Med', 'CV', 'Theory', 'Sample1', 'Sample2', 'Sample3');
      break;
    case "3": // 变量(6列) + 理论(1列) + 结果(1列)
      columns.push('IV', 'DV', 'MV', 'Mod', 'Med', 'CV', 'Theory', 'Result');
      break;
    case "4": // 变量(6列) + 理论(1列) + 结果(1列) + 样本(3列)
      columns.push('IV', 'DV', 'MV', 'Mod', 'Med', 'CV', 'Theory', 'Result', 'Sample1', 'Sample2', 'Sample3');
      break;
    case "5": // 结果(1列) + 样本(3列) + 方法(1列)
      columns.push('Result', 'Sample1', 'Sample2', 'Sample3', 'Method');
      break;
    case "6": // 全列
      columns.push('IV', 'DV', 'MV', 'Mod', 'Med', 'CV', 'OV', 'Item', 'Result', 'Theory', 'Sample1', 'Sample2', 'Sample3', 'Method');
      break;
  }
  
  // 构建表头
  headerRow = '| ' + columns.join(' | ') + ' |\n';
  separatorRow = '|' + columns.map(() => '------').join('|') + '|\n';
  
  result += headerRow + separatorRow;
  
  // 构建数据行
  itemsData.forEach((data, index) => {
    let row = `| ${index + 1} | ${data.author} | ${data.year} | ${data.journal}`;
    
    // 根据模式添加相应的列
    switch(mode) {
      case "1":
        row += ` | ${data.iv} | ${data.dv} | ${data.mv} | ${data.mod} | ${data.med} | ${data.cv} | ${data.theory}`;
        break;
      case "2":
        row += ` | ${data.iv} | ${data.dv} | ${data.mv} | ${data.mod} | ${data.med} | ${data.cv} | ${data.theory} | ${data.sample1} | ${data.sample2} | ${data.sample3}`;
        break;
      case "3":
        row += ` | ${data.iv} | ${data.dv} | ${data.mv} | ${data.mod} | ${data.med} | ${data.cv} | ${data.theory} | ${data.result}`;
        break;
      case "4":
        row += ` | ${data.iv} | ${data.dv} | ${data.mv} | ${data.mod} | ${data.med} | ${data.cv} | ${data.theory} | ${data.result} | ${data.sample1} | ${data.sample2} | ${data.sample3}`;
        break;
      case "5":
        row += ` | ${data.result} | ${data.sample1} | ${data.sample2} | ${data.sample3} | ${data.method}`;
        break;
      case "6":
        row += ` | ${data.iv} | ${data.dv} | ${data.mv} | ${data.mod} | ${data.med} | ${data.cv} | ${data.other} | ${data.item} | ${data.result} | ${data.theory} | ${data.sample1} | ${data.sample2} | ${data.sample3} | ${data.method}`;
        break;
    }
    
    row += ' |\n';
    result += row;
  });
  
  console.log(result);
  return result;
})()
}$
/**
 * 输出要求说明：
 * 
 * 1. 功能：按条目统计，每行代表一个文献条目，支持多种输出模式
 * 
 * 2. 输出模式选择：
 *    模式1 - 变量(6列) + 理论(1列)：基础列 + IV、DV、MV、Mod、Med、CV、Theory
 *    模式2 - 变量(6列) + 理论(1列) + 样本(3列)：模式1 + Sample1、Sample2、Sample3
 *    模式3 - 变量(6列) + 理论(1列) + 结果(1列)：模式1 + Result
 *    模式4 - 变量(6列) + 理论(1列) + 结果(1列) + 样本(3列)：模式1 + Result + Sample1、Sample2、Sample3
 *    模式5 - 结果(1列) + 样本(3列) + 方法(1列)：基础列 + Result、Sample1、Sample2、Sample3、Method
 *    模式6 - 全列：所有18列
 * 
 * 3. 基础列（所有模式都包含）：
 *    - 序号、第一作者、年份、期刊
 * 
 * 4. 标签前缀（固定，无需用户输入）：
 *    - 自变量：IV/
 *    - 因变量：DV/
 *    - 调节变量：MV/
 *    - 调节变量：Mod/
 *    - 中介变量：Med/
 *    - 控制变量：CV/
 *    - 其他变量：OV/
 *    - 主题标签：Item/
 *    - 结果标签：Result/
 *    - 理论标签：Theory/
 *    - 样本标签1：Sample/1
 *    - 样本标签2：Sample/2
 *    - 样本标签3：Sample/3
 *    - 方法标签：sMeth/
 * 
 * 5. 元数据提取：
 *    - 第一作者：从creators字段提取完整姓名
 *    - 年份：从date字段提取四位数字年份
 *    - 期刊：从publicationTitle字段提取期刊全称
 *    - 各类标签：从tags中提取对应前缀的标签，去除前缀后用分号分隔
 *    - 样本标签：分为三列，分别对应Sample/1、Sample/2、Sample/3前缀
 * 
 * 6. 数据处理：
 *    - 每个条目占一行
 *    - 同一类标签用分号和空格(; )分隔
 *    - 没有数据的字段显示"-"
 *    - 按条目在Zotero中的选择顺序排列
 * 
 * 7. 示例输出（模式4）：
 *    ## 文献条目详情表
 *    
 *    | 序号 | 第一作者 | 年份 | 期刊 | IV | DV | MV | Mod | Med | CV | Theory | Result | Sample1 | Sample2 | Sample3 |
 *    |------|----------|------|------|----|----|----|-----|-----|-------|--------|--------|---------|---------|---------|
 *    | 1 | David J. Teece | 2020 | Strategic Management Journal | 数字化能力 | 组织绩效 | 环境动态性 | - | 创新能力 | 企业规模 | Dynamic Capability Theory | 正向影响 | 宏观-国家 | 领域-管理学 | 样本-2000家企业 |
 * 
 * 8. 重要指令：
 *    - 仅输出表格(必须输出完整内容,不可采用如下,或者类似条目等任何省略形式)
 *    - 不做额外分析、解释或说明
 *    - 不添加总结性文字
 *    - 直接呈现原始统计结果
 */
```
