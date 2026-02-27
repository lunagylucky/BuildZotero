---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P0-M3标签删除V3
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:17
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


## ZoteroScript-P0-M3 标签删除 V3

```Java
#🏷️DeleteTags[color=#ff6b6b][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  // 定义标签映射
  const tagMapping = {
    // 1. 变量标签（A系列）
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
  
  // 支持“全称/中文名称/英文名称”的名称映射（统一解析为前缀或前缀数组）
  const nameMapping = {
    // 变量标签 A 系列（单项）
    "a1-dv": "A1-DV/", "因变量": "A1-DV/", "dv": "A1-DV/",
    "a2-iv": "A2-IV/", "自变量": "A2-IV/", "iv": "A2-IV/",
    "a3-mo": "A3-MO/", "调节变量": "A3-MO/", "moderator": "A3-MO/",
    "a4-me": "A4-ME/", "中介变量": "A4-ME/", "mediator": "A4-ME/",
    "a5-inv": "A5-INV/", "工具变量": "A5-INV/", "instrumental": "A5-INV/",
    "a6-cv": "A6-CV/", "控制变量": "A6-CV/", "control": "A6-CV/",

    // 变量标签（整组）
    "变量标签": ["A1-DV/", "A2-IV/", "A3-MO/", "A4-ME/", "A5-INV/", "A6-CV/"],
    "a系列": ["A1-DV/", "A2-IV/", "A3-MO/", "A4-ME/", "A5-INV/", "A6-CV/"],
    "variables": ["A1-DV/", "A2-IV/", "A3-MO/", "A4-ME/", "A5-INV/", "A6-CV/"],

    // 主题/理论/结果（单组）
    "item": "Item/", "主题": "Item/", "主题标签": "Item/",
    "theory": "Theory/", "理论": "Theory/", "理论标签": "Theory/",
    "result": "Result/", "结果": "Result/", "结果标签": "Result/",

    // 样本（整组和单项）
    "sample": ["Sample/1", "Sample/2", "Sample/3"], "样本": ["Sample/1", "Sample/2", "Sample/3"], "样本标签": ["Sample/1", "Sample/2", "Sample/3"],
    "样本1": "Sample/1", "样本2": "Sample/2", "样本3": "Sample/3",
    "sample/1": "Sample/1", "sample/2": "Sample/2", "sample/3": "Sample/3",

    // 方法（整组和单项）
    "方法": ["sMeth/1", "sMeth/2", "sMeth/3", "sMeth/4", "sMeth/5", "sMeth/6"],
    "方法标签": ["sMeth/1", "sMeth/2", "sMeth/3", "sMeth/4", "sMeth/5", "sMeth/6"],
    "smeth": ["sMeth/1", "sMeth/2", "sMeth/3", "sMeth/4", "sMeth/5", "sMeth/6"],
    "方法1": "sMeth/1", "方法2": "sMeth/2", "方法3": "sMeth/3", "方法4": "sMeth/4", "方法5": "sMeth/5", "方法6": "sMeth/6",
    "smeth/1": "sMeth/1", "smeth/2": "sMeth/2", "smeth/3": "sMeth/3", "smeth/4": "sMeth/4", "smeth/5": "sMeth/5", "smeth/6": "sMeth/6",

    // 条目细节/研究要素（整组和单项）
    "条目细节": ["V1-def/", "V2-mea/", "V3-con/", "V4-fut/"],
    "研究要素": ["V1-def/", "V2-mea/", "V3-con/", "V4-fut/"],
    "v系列": ["V1-def/", "V2-mea/", "V3-con/", "V4-fut/"],
    "v1-def": "V1-def/", "变量定义": "V1-def/",
    "v2-mea": "V2-mea/", "测量方法": "V2-mea/",
    "v3-con": "V3-con/", "研究贡献": "V3-con/",
    "v4-fut": "V4-fut/", "研究展望": "V4-fut/"
  };
  
  // 标准化核心名称：去除前缀（取最后一级）、去除起始序号、去除末尾 -new
  const normalizeCore = (input) => {
    if (!input || typeof input !== "string") return "";
    const noSuffix = input.replace(/-new$/i, "");
    const withoutTrailingSlash = noSuffix.replace(/\/$/, "");
    const lastSegment = withoutTrailingSlash.split('/').pop() || "";
    // 去掉起始序号和连接符，如 1- 或 12-3- 等
    const dropLeadingNumbers = lastSegment.replace(/^\d+(?:-\d+)*-?\s*/, "");
    return dropLeadingNumbers.trim();
  };
  
  // 用户输入选择
  const userInput = prompt(
    "请输入要删除的标签类型（多个用逗号分隔）：\n\n" +
    "【数字选项】\n" +
    "1 - 变量标签（全部6个）\n" +
    "  1-1 因变量(A1-DV)  1-2 自变量(A2-IV)  1-3 调节变量(A3-MO)\n" +
    "  1-4 中介变量(A4-ME)  1-5 工具变量(A5-INV)  1-6 控制变量(A6-CV)\n\n" +
    "2 - 主题标签(Item)\n" +
    "3 - 理论标签(Theory)\n" +
    "4 - 结果标签(Result)\n\n" +
    "5 - 样本标签（全部3个）\n" +
    "  5-1 样本1  5-2 样本2  5-3 样本3\n\n" +
    "6 - 方法标签（全部6个）\n" +
    "  6-1 方法1  6-2 方法2  6-3 方法3\n" +
    "  6-4 方法4  6-5 方法5  6-6 方法6\n\n" +
    "7 - 条目细节标签（研究要素，全部4个）\n" +
    "  7-1 变量定义(V1-def)  7-2 测量方法(V2-mea)\n" +
    "  7-3 研究贡献(V3-con)  7-4 研究展望(V4-fut)\n\n" +
    "【前缀 / 全称 / 完整标签名】\n" +
    "也可直接输入：\n- 前缀（如 sMeth/, Theory/）\n- 全称（如 因变量, 理论标签, 变量定义）\n- 完整标签名（如 Theory/自我决定理论, Sample/1宏观）\n\n" +
    "示例：1,3,4 或 1-1,1-2 或 Theory/, Item/ 或 因变量, 理论标签 或 完整标签: Theory/自我决定理论, Sample/1宏观 或 混合: 1,Theory/,变量定义,Sample/1宏观"
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
  
  // 根据用户选择构建：前缀列表 + 精确匹配标签列表
  const tagPrefixes = [];
  const exactTags = [];
  const selectedDescriptions = []; // 用于显示选择的描述
  
  for (let choice of userChoices) {
    // 统一规格化（去空格、小写、去除结尾斜杠用于匹配名称）
    const normalized = choice.replace(/\s+/g, "").toLowerCase().replace(/\/$/, "");

    if (tagMapping[choice]) {
      // 数字选项
      if (Array.isArray(tagMapping[choice])) {
        tagPrefixes.push(...tagMapping[choice]);
        selectedDescriptions.push(`选项${choice}(${tagMapping[choice].join(', ')})`);
      } else {
        tagPrefixes.push(tagMapping[choice]);
        selectedDescriptions.push(`选项${choice}(${tagMapping[choice]})`);
      }
    } else if (nameMapping[normalized]) {
      // 全称/中文/英文映射
      const mapped = nameMapping[normalized];
      if (Array.isArray(mapped)) {
        tagPrefixes.push(...mapped);
        selectedDescriptions.push(`名称(${choice}) → (${mapped.join(', ')})`);
      } else {
        // 确保标准化前缀：除 Sample/ 与 sMeth/ 数字项外，常规类目以 / 结尾
        const needsSlash = /^(item|theory|result|a[1-6]-[a-z]+|v[1-4]-[a-z]+)$/i.test(normalized);
        const finalPrefix = needsSlash && !mapped.endsWith('/') ? `${mapped}/`.replace(/\/\/+$/, "/") : mapped;
        tagPrefixes.push(finalPrefix);
        selectedDescriptions.push(`名称(${choice}) → (${finalPrefix})`);
      }
    } else {
      // 其他任意输入：若以 / 结尾视为前缀；若包含 / 但不以 / 结尾视为完整标签；否则也视为完整标签
      const trimmed = choice.trim();
      if (/\/$/.test(trimmed)) {
        tagPrefixes.push(trimmed);
        selectedDescriptions.push(`自定义前缀(${trimmed})`);
      } else if (trimmed.includes('/')) {
        exactTags.push(trimmed);
        selectedDescriptions.push(`完整标签(${trimmed})`);
      } else {
        exactTags.push(trimmed);
        selectedDescriptions.push(`完整标签(${trimmed})`);
      }
    }
  }
  
  // 去重
  const uniquePrefixes = [...new Set(tagPrefixes)];
  const uniqueExactTags = [...new Set(exactTags)];
  const uniqueExactCores = new Set(uniqueExactTags.map(normalizeCore).filter(Boolean));
  
  // 确认删除操作 / 查询模式
  const prefixList = uniquePrefixes.map(prefix => `"${prefix}"`).join(", ");
  const exactList = uniqueExactTags.map(t => `"${t}"`).join(", ");
  const descriptionList = selectedDescriptions.join("\n");
  const isQueryOnly = confirm("是否仅查询匹配的标签而不执行删除？\n\n选择“确定”=仅查询；选择“取消”=继续删除流程");
  const confirmDelete = isQueryOnly ? true : confirm(
    `确认删除以下标签吗？\n\n` +
    `【选择的内容】\n${descriptionList}\n\n` +
    `【实际前缀】\n${prefixList || '(无)'}\n\n` +
    `【精确标签】\n${exactList || '(无)'}\n\n` +
    `⚠️ 此操作不可撤销，请谨慎操作！`
  );
  
  if (!confirmDelete) {
    return "操作取消：用户选择不删除";
  }
  
  let processedCount = 0;    // 处理的条目数
  let deletedTagsCount = 0;  // 删除的标签总数
  let skipCount = 0;         // 跳过的条目数（没有匹配标签）
  let errorCount = 0;        // 保存失败的条目数
  let previewTags = [];      // 预览将要删除的标签
  
  // 首先预览将要删除的标签
  for (let item of items) {
    try {
      const existingTags = item.getTags();
      // 检查是否有标签匹配任何一个前缀或精确标签（按核心名匹配忽略前缀/序号/-new）
      const tagsToDelete = existingTags.filter(tag => {
        const tagName = tag.tag || '';
        const matchPrefix = uniquePrefixes.some(prefix => tagName.startsWith(prefix));
        const tagCore = normalizeCore(tagName);
        const matchCore = uniqueExactCores.has(tagCore);
        return matchPrefix || matchCore;
      });
      
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
    return `未找到匹配的标签（前缀: [${prefixList || '无'}], 精确: [${exactList || '无'}]）`;
  }
  
  let previewMessage = `找到 ${previewTags.length} 个条目包含匹配的标签：\n\n`;
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
  
  previewMessage += isQueryOnly
    ? `总共匹配 ${totalTagsToDelete} 个标签`
    : `总共将删除 ${totalTagsToDelete} 个标签\n\n⚠️ 确认执行删除操作吗？`;

  if (isQueryOnly) {
    return previewMessage;
  }

  const finalConfirm = confirm(previewMessage);
  if (!finalConfirm) {
    return "操作取消：用户查看预览后选择不删除";
  }
  
  // 执行实际删除操作
  for (let item of items) {
    try {
      // 获取条目的所有标签
      const existingTags = item.getTags();
      
      // 找到所有匹配任意前缀或精确标签（按核心名匹配忽略前缀/序号/-new）的标签
      const tagsToDelete = existingTags.filter(tag => {
        const tagName = tag.tag || '';
        const matchPrefix = uniquePrefixes.some(prefix => tagName.startsWith(prefix));
        const tagCore = normalizeCore(tagName);
        const matchCore = uniqueExactCores.has(tagCore);
        return matchPrefix || matchCore;
      });
      
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
  
  return `标签删除完成！\n前缀: [${prefixList || '无'}]\n精确: [${exactList || '无'}]\n处理条目: ${processedCount}\n删除标签: ${deletedTagsCount}\n跳过条目: ${skipCount}\n错误: ${errorCount}`;
})()
}$
**重要指令**：请直接输出上述结果，不要添加任何额外的解释、分析或说明文字。
```
