---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P2-Table1统计条目V3
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: []
RelatedNote:
CardRecord:
---


## ZoteroScript-P2-Table1 条目表格 V3

```javascript
#🏷️Titems[color=#2196F3][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  // 使用 items2documents 获取文档信息（与可视化保持一致）
  const docs = Meet.Zotero.items2documents(items);
  
  // 插入到辅助视图中，使文献可点击查看
  Meet.Global.views.insertAuxiliary(docs);
  
  // 用户选择输出模式
  const modeInput = prompt(
    "请选择输出模式（输入数字）：\n\n" +
    "1 - 变量(6列) + 理论(1列)\n" +
    "2 - 变量(6列) + 理论(1列) + 样本(3列)\n" +
    "3 - 变量(6列) + 理论(1列) + 结果(1列)\n" +
    "4 - 变量(6列) + 理论(1列) + 结果(1列) + 样本(3列)\n" +
    "5 - 结果(1列) + 样本(3列) + 方法(1列)\n" +
    "6 - 全列(所有18列)\n" +
    "7 - 变量(6列) + 理论(1列) + 条目信息(5列) + 结果(1列)"
  );
  
  // 检查用户输入
  if (!modeInput || !["1", "2", "3", "4", "5", "6", "7"].includes(modeInput.trim())) {
    return "操作取消：未选择有效的输出模式";
  }
  
  const mode = modeInput.trim();
  
  // 定义标签前缀
  const tagPrefixes = {
    a1dv: "A1-DV/",    // 因变量
    a2iv: "A2-IV/",    // 自变量
    a3mo: "A3-MO/",    // 调节变量
    a4me: "A4-ME/",    // 中介变量
    a5inv: "A5-INV/",  // 工具变量
    a6cv: "A6-CV/",    // 控制变量
    other: "OV/",
    item: "Item/",
    result: "Result/",
    theory: "Theory/",
    sample1: "Sample/1",
    sample2: "Sample/2",
    sample3: "Sample/3",
    method: "sMeth/",
    v1def: "V1-def/",
    v2mea: "V2-mea/",
    v3con: "V3-con/",
    v4fut: "V4-fut/",
    v5sor: "V5-sor/"
  };
  
  // 收集所有条目信息
  const itemsData = [];
  
  for (let [index, item] of items.entries()) {
    try {
      // 从 docs 中获取格式化的引用信息
      const doc = docs[index];
      let citation = "";
      let author = "";
      let year = "";
      
      // 从 pageContent 中提取作者和年份信息
      // pageContent 格式通常为: "Title\nAuthor et al., Year\n…"
      const lines = doc.pageContent.split('\n');
      
      // 尝试从第二行提取作者和年份
      if (lines.length > 1) {
        const authorYearLine = lines[1];
        // 匹配 "Author et al., Year" 或 "Author, Year" 格式
        const match = authorYearLine.match(/^([^,]+?)(?:\s+et al\.)?,\s*(\d{4})/);
        if (match) {
          author = match[1].trim();
          year = match[2];
        }
      }
      
      // 如果上述方法失败，使用原始方法提取
      if (!author || !year) {
        try {
          const creators = item.getCreators();
          if (creators && creators.length > 0) {
            const firstAuthor = creators[0];
            author = firstAuthor.lastName || firstAuthor.name || "";
          }
        } catch {}
        
        try {
          const date = item.getField("date");
          if (date) {
            const yearMatch = date.match(/\d{4}/);
            if (yearMatch) {
              year = yearMatch[0];
            }
          }
        } catch {}
      }
      
      // 构建引用格式：author(year)[number]
      // 与 docs.map((doc, index) => "[" + String(index + 1) + "]" + " " + doc.pageContent) 保持一致
      if (author && year) {
        citation = `${author} (${year}) [${index + 1}]`;
      } else if (author) {
        citation = `${author} [${index + 1}]`;
      } else {
        citation = `[${index + 1}]`;
      }
      
      // 获取年份和期刊
      let journalYear = year || "-";
      let journal = "";
      
      try {
        journal = item.getField("publicationTitle") || "";
      } catch {}
      
      // 获取标签并分类
      const tags = item.getTags();
      const categorizedTags = {
        a1dv: [],
        a2iv: [],
        a3mo: [],
        a4me: [],
        a5inv: [],
        a6cv: [],
        other: [],
        item: [],
        result: [],
        theory: [],
        sample1: [],
        sample2: [],
        sample3: [],
        method: [],
        v1def: [],
        v2mea: [],
        v3con: [],
        v4fut: [],
        v5sor: []
      };
      
      if (tags && tags.length > 0) {
        for (let tagObj of tags) {
          const tag = tagObj.tag;
          
          // 检查并分类标签
          if (tag.startsWith(tagPrefixes.a1dv)) {
            categorizedTags.a1dv.push(tag.substring(tagPrefixes.a1dv.length));
          } else if (tag.startsWith(tagPrefixes.a2iv)) {
            categorizedTags.a2iv.push(tag.substring(tagPrefixes.a2iv.length));
          } else if (tag.startsWith(tagPrefixes.a3mo)) {
            categorizedTags.a3mo.push(tag.substring(tagPrefixes.a3mo.length));
          } else if (tag.startsWith(tagPrefixes.a4me)) {
            categorizedTags.a4me.push(tag.substring(tagPrefixes.a4me.length));
          } else if (tag.startsWith(tagPrefixes.a5inv)) {
            categorizedTags.a5inv.push(tag.substring(tagPrefixes.a5inv.length));
          } else if (tag.startsWith(tagPrefixes.a6cv)) {
            categorizedTags.a6cv.push(tag.substring(tagPrefixes.a6cv.length));
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
          } else if (tag.startsWith(tagPrefixes.v1def)) {
            categorizedTags.v1def.push(tag.substring(tagPrefixes.v1def.length));
          } else if (tag.startsWith(tagPrefixes.v2mea)) {
            categorizedTags.v2mea.push(tag.substring(tagPrefixes.v2mea.length));
          } else if (tag.startsWith(tagPrefixes.v3con)) {
            categorizedTags.v3con.push(tag.substring(tagPrefixes.v3con.length));
          } else if (tag.startsWith(tagPrefixes.v4fut)) {
            categorizedTags.v4fut.push(tag.substring(tagPrefixes.v4fut.length));
          } else if (tag.startsWith(tagPrefixes.v5sor)) {
            categorizedTags.v5sor.push(tag.substring(tagPrefixes.v5sor.length));
          }
        }
      }
      
      // 添加到结果数组
      itemsData.push({
        citation: citation,
        year: journalYear,
        journal: journal || "-",
        a1dv: categorizedTags.a1dv.length > 0 ? categorizedTags.a1dv.join('; ') : "-",
        a2iv: categorizedTags.a2iv.length > 0 ? categorizedTags.a2iv.join('; ') : "-",
        a3mo: categorizedTags.a3mo.length > 0 ? categorizedTags.a3mo.join('; ') : "-",
        a4me: categorizedTags.a4me.length > 0 ? categorizedTags.a4me.join('; ') : "-",
        a5inv: categorizedTags.a5inv.length > 0 ? categorizedTags.a5inv.join('; ') : "-",
        a6cv: categorizedTags.a6cv.length > 0 ? categorizedTags.a6cv.join('; ') : "-",
        other: categorizedTags.other.length > 0 ? categorizedTags.other.join('; ') : "-",
        item: categorizedTags.item.length > 0 ? categorizedTags.item.join('; ') : "-",
        result: categorizedTags.result.length > 0 ? categorizedTags.result.join('; ') : "-",
        theory: categorizedTags.theory.length > 0 ? categorizedTags.theory.join('; ') : "-",
        sample1: categorizedTags.sample1.length > 0 ? categorizedTags.sample1.join('; ') : "-",
        sample2: categorizedTags.sample2.length > 0 ? categorizedTags.sample2.join('; ') : "-",
        sample3: categorizedTags.sample3.length > 0 ? categorizedTags.sample3.join('; ') : "-",
        method: categorizedTags.method.length > 0 ? categorizedTags.method.join('; ') : "-",
        v1def: categorizedTags.v1def.length > 0 ? categorizedTags.v1def.join('; ') : "-",
        v2mea: categorizedTags.v2mea.length > 0 ? categorizedTags.v2mea.join('; ') : "-",
        v3con: categorizedTags.v3con.length > 0 ? categorizedTags.v3con.join('; ') : "-",
        v4fut: categorizedTags.v4fut.length > 0 ? categorizedTags.v4fut.join('; ') : "-",
        v5sor: categorizedTags.v5sor.length > 0 ? categorizedTags.v5sor.join('; ') : "-"
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
  const baseColumns = ['文献', '年份', '期刊'];
  
  // 根据模式定义列
  let columns = [...baseColumns];
  
  switch(mode) {
    case "1": // 变量(6列) + 理论(1列)
      columns.push('A1-DV', 'A2-IV', 'A3-MO', 'A4-ME', 'A5-INV', 'A6-CV', 'Theory');
      break;
    case "2": // 变量(6列) + 理论(1列) + 样本(3列)
      columns.push('A1-DV', 'A2-IV', 'A3-MO', 'A4-ME', 'A5-INV', 'A6-CV', 'Theory', 'Sample1', 'Sample2', 'Sample3');
      break;
    case "3": // 变量(6列) + 理论(1列) + 结果(1列)
      columns.push('A1-DV', 'A2-IV', 'A3-MO', 'A4-ME', 'A5-INV', 'A6-CV', 'Theory', 'Result');
      break;
    case "4": // 变量(6列) + 理论(1列) + 结果(1列) + 样本(3列)
      columns.push('A1-DV', 'A2-IV', 'A3-MO', 'A4-ME', 'A5-INV', 'A6-CV', 'Theory', 'Result', 'Sample1', 'Sample2', 'Sample3');
      break;
    case "5": // 结果(1列) + 样本(3列) + 方法(1列)
      columns.push('Result', 'Sample1', 'Sample2', 'Sample3', 'Method');
      break;
    case "6": // 全列
      columns.push('A1-DV', 'A2-IV', 'A3-MO', 'A4-ME', 'A5-INV', 'A6-CV', 'OV', 'Item', 'Result', 'Theory', 'Sample1', 'Sample2', 'Sample3', 'Method');
      break;
    case "7": // 变量(6列) + 理论(1列) + 条目信息(5列) + 结果(1列)
      columns.push('A1-DV', 'A2-IV', 'A3-MO', 'A4-ME', 'A5-INV', 'A6-CV', 'Theory', 'V1-变量定义', 'V2-测量方法', 'V3-研究贡献', 'V4-研究展望', 'V5-文献来源', 'Result');
      break;
  }
  
  // 构建表头
  headerRow = '| ' + columns.join(' | ') + ' |\n';
  separatorRow = '|' + columns.map(() => '------').join('|') + '|\n';
  
  result += headerRow + separatorRow;
  
  // 构建数据行
  itemsData.forEach((data, index) => {
    let row = `| ${data.citation} | ${data.year} | ${data.journal}`;
    
    // 根据模式添加相应的列
    switch(mode) {
      case "1":
        row += ` | ${data.a1dv} | ${data.a2iv} | ${data.a3mo} | ${data.a4me} | ${data.a5inv} | ${data.a6cv} | ${data.theory}`;
        break;
      case "2":
        row += ` | ${data.a1dv} | ${data.a2iv} | ${data.a3mo} | ${data.a4me} | ${data.a5inv} | ${data.a6cv} | ${data.theory} | ${data.sample1} | ${data.sample2} | ${data.sample3}`;
        break;
      case "3":
        row += ` | ${data.a1dv} | ${data.a2iv} | ${data.a3mo} | ${data.a4me} | ${data.a5inv} | ${data.a6cv} | ${data.theory} | ${data.result}`;
        break;
      case "4":
        row += ` | ${data.a1dv} | ${data.a2iv} | ${data.a3mo} | ${data.a4me} | ${data.a5inv} | ${data.a6cv} | ${data.theory} | ${data.result} | ${data.sample1} | ${data.sample2} | ${data.sample3}`;
        break;
      case "5":
        row += ` | ${data.result} | ${data.sample1} | ${data.sample2} | ${data.sample3} | ${data.method}`;
        break;
      case "6":
        row += ` | ${data.a1dv} | ${data.a2iv} | ${data.a3mo} | ${data.a4me} | ${data.a5inv} | ${data.a6cv} | ${data.other} | ${data.item} | ${data.result} | ${data.theory} | ${data.sample1} | ${data.sample2} | ${data.sample3} | ${data.method}`;
        break;
      case "7":
        row += ` | ${data.a1dv} | ${data.a2iv} | ${data.a3mo} | ${data.a4me} | ${data.a5inv} | ${data.a6cv} | ${data.theory} | ${data.v1def} | ${data.v2mea} | ${data.v3con} | ${data.v4fut} | ${data.v5sor} | ${data.result}`;
        break;
    }
    
    row += ' |\n';
    result += row;
  });
  
  console.log(result);
  return result;
})()
}$

```

/**
 * 输出要求说明：
 * 1. 请直接输出完整的 Markdown 表格，不可有省略任何行或列
 * 2. 标签内容请保持原样，不要做任何更改或简化
 * 3. 请勿添加额外的分析、解释、总结或评论性文字
 * 4. 仅输出表格本身及表格标题，保持格式规范整齐
 * 5. 文献编号格式（如 [1]）与右侧可视化界面中的编号对应，点击右侧文献可查看详细信息
 * 6. 所有列的数据都必须完整输出，包括：文献、年份、期刊以及所选模式下的所有变量、理论、样本、结果等标签列  
 */
