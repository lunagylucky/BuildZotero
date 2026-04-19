---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: Tag5-理论标签-版本迭代说明
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

## Tag4- 理论标签 版本迭代说明

### 概述
Tag4- 理论标签模块用于从学术文献中自动提取和生成理论标签，通过 AI 分析文献内容（摘要、PDF、笔记）识别核心理论框架。标签体系包括：标准理论库匹配、新理论识别（`-new` 后缀）、序号标签（支持多理论）、错误状态标签（无内容、无理论关键词、解析失败等）。理论库包含 140+ 种标准理论，支持别名匹配和关键词匹配。

---


### 版本号体系说明
理论标签脚本采用统一的版本号体系，便于理解版本关系和功能特性：

#### 版本号格式：`V[major].[variant]`
- **V[major]（主版本号）**：表示核心功能版本，决定内容获取策略、PDF 字符数、标签格式、理论库完善程度和 Prompt 优化
  - V1-V6：主要版本迭代

- **[variant]（变体标识）**：表示功能变体，在基础版本上增加特殊功能
  - 无标识：标准版本（跳过已有 Theory/ 标签）
  - **-R**：重新处理版本（删除旧标签后重新生成，基于对应的 V 版本）
  - **-Rnote**：笔记优先版本（删除旧标签后重新生成，优先使用笔记内容，需要 📒 标签，基于对应的 V 版本）

**R 系列与 Rnote 系列的区别：**
- **相同点**：都会删除现有的 Theory/ 标签后重新生成
- **不同点**：
  - **R 系列**：内容获取策略与对应的 V 版本相同（如 V6-R 使用 PDF 前 22000 字符 → 摘要）
  - **Rnote 系列**：优先使用笔记内容，备选 PDF 或摘要（如 V6-Rnote 使用 笔记 → 摘要）


#### 版本对应关系
| 基础版本 | 重新处理版本（R 系列） | 笔记优先版本（Rnote 系列） | 功能特性 |
|---------|---------------------|-------------------------|---------|
| V1 | - | - | theoriesList，英文理论名，摘要优先或 PDF 全文，Theory/前缀，返回 1 个理论，无跳过机制 |
| V2 | - | - | theoriesDatabase（别名），快速匹配，摘要优先或 PDF 2000 字符，Theory/前缀，返回 1 个理论，有跳过机制 |
| V3 | - | - | PDF 优先策略（PDF 前 3000 字符，备选摘要），PDF 字符数从 2000 增加到 3000，其余同 V2 |
| V4 | - | V4-Rnote | PDF 优先策略（PDF 前 3000 字符，备选摘要），理论关键词检查，Prompt 优化，返回 1 个理论，无序号 |
| V5 | V5-R | - | saveItemWithTags 函数，理论名称 "（中文）英文名称" 格式，Theory/1tag 格式（序号），返回 1-3 个理论，英文 Prompt |
| V6 | V6-R | V6-Rnote | PDF 22000 字符，扩展别名列表，Theory/1tag 格式（序号），返回 1-3 个理论，英文 Prompt |

**命名逻辑说明：**
- **V4-Rnote**：基于 V4 的笔记优先版本（在 V4 基础上增加笔记优先逻辑）
- **V6-Rnote**：基于 V6 的笔记优先版本（在 V6 基础上增加笔记优先逻辑）

---


### V 系列（标准版本）迭代历史

#### V1 - 初始版本（2024-06-20）
**核心特性：**
- 基础的理论标签生成功能
- 使用 `theoriesList`（简单数组结构）
- 优先使用摘要，备选 PDF 全文
- 英文理论名称（无中文标注）
- 无跳过机制（处理所有条目）

**内容获取策略：**

```javascript
// 优先摘要
let abs = item.getField("abstractNote");
if (!abs) {
  // 备选PDF全文
  const pdfText = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
  abs = pdfText.text;
}
```

**理论库结构：**
- 简单数组：`[{name: "Ability-motivation-opportunity (AMO) model"}, ...]`
- 理论数量：约 140 个
- 无别名支持

**标签前缀：**
- `Theory/` 前缀

**标签格式：**
- `Theory/理论名` 或 `Theory/理论名-new`
- 例如：`Theory/Social Exchange Theory` 或 `Theory/New Theory-new`
- 返回 1 个理论

**匹配逻辑：**
- 宽松匹配（完全一致、包含、关键词交集 ≥2 个词）

**AI Prompt：**
- 简单的英文 Prompt，要求返回 1 个理论名称

**特点：**
- ✅ 基础功能完整
- ✅ 支持新理论识别（`-new` 后缀）
- ❌ 无跳过机制（会重复处理已有标签的条目）
- ❌ 理论库无别名支持，匹配精度较低
- ❌ PDF 全文可能导致 AI 处理负担过重

---


#### V2 - 别名系统与快速匹配（2025-07-31）
**核心改进：**
- 理论库升级为 `theoriesDatabase`（支持别名）
- 快速匹配函数 `findTheory`（完全匹配、包含匹配、别名匹配、关键词匹配）
- 添加跳过机制（已存在 Theory/ 标签的条目跳过）
- 理论关键词检查（提前过滤无理论内容的条目）
- 摘要优先，备选 PDF 前 2000 字符（减少 AI 处理负担）

**新增功能：**

1. **理论库结构升级**

```javascript
const theoriesDatabase = [
  {name: "Ability-motivation-opportunity (AMO) model", aliases: ["AMO model", "AMO theory", "AMO framework"]},
  ...
];
```

2. **快速匹配函数**

```javascript
const findTheory = (searchTerm) => {
  // 完全匹配和包含匹配
  // 别名匹配
  // 关键词匹配（≥2个词重合）
  return theory.name || null;
};
```

3. **理论关键词检查**

```javascript
const triggerKeywords = ["theory", "theories", "theoretical", "framework", "model", "view", "lens", "理论", "视角"];
const hasTheoryKeywords = (text) => {
  return triggerKeywords.some(keyword => lowerText.includes(keyword));
};
```

**内容获取策略：**

```javascript
// 优先摘要（50字符阈值）
const abstract = item.getField("abstractNote");
if (abstract && abstract.trim().length > 50) {
  content = abstract.trim();
} else {
  // 备选PDF前2000字符
  content = fullTextData.text.substring(0, 2000).trim();
}
```

**标签格式：**
- `Theory/理论名` 或 `Theory/理论名-new`
- 返回 1 个理论

**错误状态标签：**
- `Theory/无摘要`：既没有摘要也没有附件
- `Theory/无理论关键词`：内容中不包含理论相关关键词
- `Theory/无明确理论`：AI 分析后没有识别到明确理论
- `Theory/解析失败`：JSON 解析失败或 AI 调用失败

**AI Prompt：**
- 中文 Prompt，要求返回 1 个理论名称，格式：`["理论名"]` 或 `["无明确理论"]`

**特点：**
- ✅ 别名系统大幅提高匹配精度
- ✅ 快速匹配函数优化性能
- ✅ 跳过机制避免重复处理
- ✅ 理论关键词检查提前过滤，节省 AI 调用
- ✅ PDF 字符数限制（2000）减少处理负担
- ❌ 错误状态标签命名不一致（" 无摘要 " 而非 " 无内容 "）

---


#### V3 - PDF 优先策略与字符数调整（2025-08-08）
**核心改进：**
- **改变内容获取优先级**：从 " 摘要优先 " 改为 "PDF 优先 "（这是 V3 的重要改进）
- PDF 内容长度从 2000 字符扩展到 3000 字符
- 错误状态标签从 " 无摘要 " 改为 " 无内容 "（更准确）
- 其余功能与 V2 相同

**内容获取策略变化：**

```javascript
// V2: 摘要优先，备选PDF前2000字符
const abstract = item.getField("abstractNote");
if (abstract && abstract.trim().length > 50) {
  content = abstract.trim();
} else {
  content = fullTextData.text.substring(0, 2000).trim();
}

// V3: PDF优先，备选摘要（改变优先级）
const pdfItem = await item.getBestAttachment();
if (pdfItem && pdfItem.isPDFAttachment()) {
  const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
  // PDF前3000字符（从2000增加到3000）
  content = fullTextData.text.substring(0, 3000).trim();
}

// 如果PDF获取失败，再使用摘要
if (!content) {
  const abstract = item.getField("abstractNote");
  if (abstract && abstract.trim().length > 50) {
    content = abstract.trim();
  }
}
```

**特点：**
- ✅ **PDF 优先策略更符合理论通常在引言部分的规律**（这是 V3 的重要改进）
- ✅ PDF 字符数从 2000 增加到 3000，覆盖更多内容
- ✅ 错误状态标签更准确（" 无内容 " 而非 " 无摘要 "）
- ✅ 其余功能保持不变，稳定性高

---


#### V4 - Prompt 优化与标签格式标准化（2025-10-12）
**核心改进：**
- 理论关键词检查优化
- Prompt 优化（更明确的理论识别要求）
- 无序号标签（返回 1 个理论，标签格式：`Theory/理论名`）
- 内容获取策略与 V3 相同（PDF 优先，PDF 前 3000 字符，备选摘要）

**内容获取策略：**

```javascript
// PDF优先（与V3相同，已在V3中引入）
const pdfItem = await item.getBestAttachment();
if (pdfItem && pdfItem.isPDFAttachment()) {
  const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
  content = fullTextData.text.substring(0, 3000).trim();
}

// 如果PDF获取失败，再使用摘要
if (!content) {
  const abstract = item.getField("abstractNote");
  if (abstract && abstract.trim().length > 50) {
    content = abstract.trim();
  }
}
```

**理论关键词检查：**

```javascript
const triggerKeywords = ["theory", "theories", "theoretical", "framework", "model", "view", "lens", "理论", "视角"];
```

**标签格式：**
- `Theory/理论名` 或 `Theory/理论名-new` 或 `Theory/无明确理论`
- 返回 1 个理论，无序号

**AI Prompt：**
- 中文 Prompt，更明确的要求：
  - 仅识别明确的理论、模型或框架名称
  - 英文文献用英文，中文文献用中文
  - 使用原文表述
  - 返回最主要的 1 个理论
  - 格式：`["理论名"]` 或 `["无明确理论"]`

**错误状态标签：**
- `Theory/无内容`：既没有摘要也没有附件
- `Theory/无理论关键词`：内容中不包含理论相关关键词
- `Theory/无明确理论`：AI 分析后没有识别到明确理论
- `Theory/解析失败`：JSON 解析失败或 AI 调用失败

**特点：**
- ✅ PDF 优先策略更符合理论通常在引言部分的规律
- ✅ Prompt 优化提高识别准确性
- ✅ 错误状态标签标准化
- ❌ 仍返回 1 个理论，不支持多理论识别

---


#### V5 - 序号标签与理论名称中文化（2025-10-12）
**核心改进：**
- 添加统一的保存函数 `saveItemWithTags`（事务保存，错误处理）
- 理论名称格式升级：`"（中文）英文名称"` 格式
- 标签格式升级：序号标签 `Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`
- 支持返回 1-3 个理论
- 英文 Prompt（更精确的理论名称识别）

**新增功能：**

1. **统一的保存函数**

```javascript
const saveItemWithTags = async (item, tags, countType) => {
  try {
    await Zotero.DB.executeTransaction(async function () {
      for (let tag of tags) {
        item.addTag(tag);
      }
      await item.save();
    });
    // 更新计数器
    switch(countType) {
      case 'success': successCount++; break;
      case 'noTheory': noTheoryCount++; break;
      // ...
    }
    return true;
  } catch (error) {
    console.error('保存标签失败:', error);
    errorCount++;
    return false;
  }
};
```

2. **理论名称格式升级**

```javascript
// V4: "Ability-motivation-opportunity (AMO) model"
// V5: "(能力-动机-机会模型)Ability-Motivation-Opportunity (AMO) Model"
const theoriesDatabase = [
  {name: "(能力-动机-机会模型)Ability-Motivation-Opportunity (AMO) Model", aliases: [...]},
  ...
];
```

3. **序号标签格式**

```javascript
// 标签格式：Theory/1理论名, Theory/2理论名, Theory/3理论名
tagsToAdd.push('Theory/' + (i + 1) + tagToAdd);
```

**内容获取策略：**
- PDF 前 3000 字符优先，备选摘要（同 V4）

**标签数量：**
- 支持 1-3 个理论（AI 根据相关性返回）
- 按相关性排序（最重要的理论为 `Theory/1理论名`）

**AI Prompt：**
- 英文 Prompt，要求：
  - Extract ONLY explicit theory/model/framework names
  - Return English names ONLY (maximum 50 characters per theory)
  - Use precise, standard academic terminology
  - Return 1-3 theories based on relevance and explicitness
  - Order by relevance: most important theory first
  - Format: `["Theory Name 1", "Theory Name 2", "Theory Name 3"]`

**错误状态标签：**
- `Theory/无内容`：既没有摘要也没有附件
- `Theory/无理论关键词`：内容中不包含理论相关关键词
- `Theory/无明确理论`：AI 分析后没有识别到明确理论
- `Theory/解析失败`：JSON 解析失败
- `Theory/JSON解析失败`：JSON 解析失败（更具体）
- `Theory/AI调用失败`：AI 调用失败（更具体）

**特点：**
- ✅ 序号标签支持多理论，便于排序和查找
- ✅ 理论名称中文化，便于中文用户理解
- ✅ 统一的保存函数提高代码可维护性
- ✅ 英文 Prompt 提高理论名称识别准确性
- ✅ 支持 1-3 个理论，更灵活
- ❌ PDF 字符数仍为 3000，可能不足以识别深层的理论引用

---


#### V6 - PDF 字符数扩展与别名列表扩展（2025-10-13）
**核心改进：**
- PDF 内容长度从 3000 字符扩展到 22000 字符
- 理论库别名列表大幅扩展（提高匹配精度）
- 其余功能与 V5 相同

**内容获取策略：**

```javascript
// 优先PDF前22000字符（从3000扩展到22000）
const pdfItem = await item.getBestAttachment();
if (pdfItem && pdfItem.isPDFAttachment()) {
  const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
  content = fullTextData.text.substring(0, 22000).trim();
}
```

**别名列表扩展示例：**

```javascript
// V5示例
{name: "(资源基础观)Resource-Based View (RBV)", aliases: ["RBV", "resource based view", "resource-based theory"]}

// V6扩展后
{name: "(资源基础观)Resource-Based View (RBV)", aliases: [
  "RBV", "resource based view", "resource-based theory",
  "Operational Slack Theory", "Slack resources theory", "Slack Resources Theory",
  "Organizational Slack", "Resource Management View", "Ricardian rents",
  "Monopoly rents", "Financial Slack Theory"
]}
```

**标签格式：**
- 同 V5：`Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`
- 支持 1-3 个理论

**AI Prompt：**
- 同 V5：英文 Prompt，返回 1-3 个理论

**特点：**
- ✅ PDF 22000 字符覆盖引言、理论背景、文献综述等关键部分，提高理论识别准确性
- ✅ 别名列表扩展大幅提高理论匹配精度（特别是关联理论的识别）
- ✅ 其余功能保持不变，稳定性高
- ❌ PDF 内容更长，AI 处理时间可能增加

---


### R 系列（重新处理版本）迭代历史

#### V5-R - 基于 V5 的重新处理版本（2025-10-12）
**核心特性：**
- 基于 V5 的代码逻辑
- **删除现有 Theory/ 标签后重新生成**
- PDF 前 3000 字符，备选摘要
- 理论名称格式：`"（中文）英文名称"`
- 序号标签：`Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`
- 支持返回 1-3 个理论

**删除旧标签逻辑：**

```javascript
const removeExistingTheoryTags = async (item) => {
  const existingTags = item.getTags();
  const theoryTags = existingTags.filter(tag => tag.tag && tag.tag.startsWith("Theory/"));
  for (let theoryTag of theoryTags) {
    item.removeTag(theoryTag.tag);
  }
};
```

**使用场景：**
- 理论库更新后，需要重新匹配已有文献
- 理论标签格式升级后（如从无序号到有序号），需要批量更新
- 修正错误的理论标签

**特点：**
- ✅ 删除旧标签，避免标签累积
- ✅ 基于 V5，功能稳定
- ❌ PDF 字符数仍为 3000，可能不足以识别深层的理论引用

---


#### V6-R - 基于 V6 的重新处理版本（2025-10-13）
**核心特性：**
- 基于 V6 的代码逻辑
- **删除现有 Theory/ 标签后重新生成**
- PDF 前 22000 字符，备选摘要
- 理论名称格式：`"（中文）英文名称"`
- 序号标签：`Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`
- 支持返回 1-3 个理论
- 扩展的别名列表

**使用场景：**
- 理论库更新后（特别是别名扩展后），需要重新匹配已有文献
- 理论标签格式升级后，需要批量更新
- 修正错误的理论标签

**特点：**
- ✅ PDF 22000 字符，覆盖更多内容，提高识别准确性
- ✅ 扩展的别名列表，提高匹配精度
- ✅ 删除旧标签，避免标签累积
- ✅ 基于 V6，功能最完整

---


### Rnote 系列（笔记优先版本）迭代历史
**系列说明：**
- **Rnote** = Re-process（重新处理）+ Note-priority（笔记优先）
- Rnote 系列版本会：
  1. **删除现有的 Theory/ 标签后重新生成**（与 R 系列相同）
  2. **检查 📒 标签**，仅处理带有 📒 标签的条目
  3. **优先使用笔记内容**，备选 PDF 或摘要
- **与 R 系列的区别**：
  - **相同点**：都会删除旧标签后重新生成
  - **不同点**：Rnote 系列优先使用笔记内容，而 R 系列使用与对应 V 版本相同的内容获取策略

---

#### V4-Rnote - 基于 V4 的笔记优先版本（2025-08-21）
**命名说明：** `V4-Rnote` 表示这是**基于 V4 版本的笔记优先版本**。在 V4 的基础上增加了笔记优先逻辑（检查 📒 标签，优先获取笔记内容），同时会删除旧标签后重新生成（与 R 系列相同）。

**核心特性：**
- **删除现有 Theory/ 标签后重新生成**（与 R 系列相同）
- **仅处理带有 📒 标签的条目**
- **优先使用笔记内容**（Zotero Notes，转换为 Markdown）
- 备选摘要
- **❌ 无 PDF 内容获取**（基于 V4 的版本限制）
- 英文理论名称（无中文标注）— 与 V4 相同
- 无序号标签：`Theory/理论名` 或 `Theory/理论名-new` — 与 V4 相同
- **支持返回多个理论**（不限制数量，AI 识别多少返回多少）

**笔记优先逻辑：**

```javascript
// 检查📒标签
const hasNotebookTag = (item) => {
  const tags = item.getTags();
  return tags && tags.some(tag => tag.tag && tag.tag.includes("📒"));
};

if (!hasNotebookTag(item)) {
  skipCount++;
  continue;
}

// 获取笔记内容
const noteIds = item.getNotes();
if (noteIds && noteIds.length > 0) {
  const notesResult = await getAllNotesContent(noteIds);
  if (notesResult.content && notesResult.content.trim().length > 50) {
    content = notesResult.content;
    isNoteUpdate = true;
    removeExistingTheoryTags(item); // 删除旧标签
  }
}

// 如果没有笔记内容，使用摘要作为备选
if (!content) {
  const abstract = safeGetField(item, "abstractNote");
  if (abstract && abstract.trim().length > 50) {
    content = abstract.trim();
    removeExistingTheoryTags(item);
  }
}
```

**内容获取策略：**
1. 第一优先：笔记内容（Zotero Notes，转换为 Markdown）
2. 第二优先：摘要
3. ❌ 无 PDF 内容获取（基于 V4 的版本限制）

**标签格式：**
- `Theory/理论名` 或 `Theory/理论名-new`
- **不限制理论数量**（AI 识别多少返回多少）

**AI Prompt：**
- 英文 Prompt，要求：
  - Only identify clear theory, model, or framework names
  - Use English names for theories
  - **Return all theories found across all content, no quantity limit**
  - Format: `["Theory 1", "Theory 2", "Theory 3", …]` or `["No clear theories"]`

**与 V4-R 的区别：**
| 特性 | V4-R | V4-Rnote |
|------|------|----------|
| 删除旧标签 | ✅ 是 | ✅ 是（相同） |
| 内容获取策略 | PDF 前 3000 字符 → 摘要 | 笔记 → 摘要（优先笔记） |
| 📒 标签要求 | ❌ 无 | ✅ 必须有 |
| 适用场景 | 所有条目 | 仅已阅读并做笔记的条目 |

**特点：**
- ✅ 笔记优先，充分利用用户阅读笔记
- ✅ 支持多理论识别（不限制数量）
- ✅ 删除旧标签，避免累积（与 R 系列相同）
- ❌ 无 PDF 内容获取，如果笔记内容不足，只能使用摘要
- ❌ 英文理论名称，无中文标注
- ❌ 无序号标签，理论顺序不明确

**使用场景：**
- 用户已经阅读并标记了笔记的文献
- 笔记中包含了理论分析内容
- 需要充分利用用户阅读笔记识别理论

**基于的 V 版本特性：**
- 内容获取策略：PDF 优先（PDF 前 3000 字符，备选摘要）— 与 V4 相同（但 Rnote 版本改为笔记优先）
- 理论库：英文理论名称（无中文标注）— 与 V4 相同
- 标签格式：无序号标签（`Theory/理论名` 或 `Theory/理论名-new`）— 与 V4 相同
- 返回理论数量：V4 返回 1 个理论，但 V4-Rnote 支持多个理论（不限制数量）
- Prompt：中文 Prompt— 与 V4 相同

---


#### V6-Rnote - 基于 V6 的笔记优先版本（2025-10-13）
**命名说明：** `V6-Rnote` 表示这是**基于 V6 版本的笔记优先版本**。在 V6 的基础上增加了笔记优先逻辑（检查 📒 标签，优先获取笔记内容），同时会删除旧标签后重新生成（与 R 系列相同）。

**核心特性：**
- **删除现有 Theory/ 标签后重新生成**（与 R 系列相同）
- **仅处理带有 📒 标签的条目**
- **优先使用笔记内容**（Zotero Notes，转换为 Markdown）
- 备选摘要
- **❌ 无 PDF 内容获取**（基于 V6 的版本限制）
- 理论名称格式：`"（中文）英文名称"` — 与 V6 相同
- **序号标签：`Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`** — 与 V6 相同
- **支持返回最多 3 个理论**（与 V6 相同）

**笔记优先逻辑：**

```javascript
// 检查📒标签
const hasNotebookTag = (item) => {
  const tags = item.getTags();
  return tags && tags.some(tag => tag.tag && tag.tag.includes("📒"));
};

if (!hasNotebookTag(item)) {
  skipCount++;
  continue;
}

// 删除旧标签
removeExistingTheoryTags(item);

// 第一优先：笔记内容
const noteIds = item.getNotes();
if (noteIds && noteIds.length > 0) {
  const notesResult = await getAllNotesContent(noteIds);
  if (notesResult.content && notesResult.content.trim().length > 50) {
    content = notesResult.content;
    isNoteUpdate = true;
  }
}

// 第二优先：摘要（无PDF备选）
if (!content) {
  const abstract = safeGetField(item, "abstractNote");
  if (abstract && abstract.trim().length > 50) {
    content = abstract.trim();
  }
}
```

**内容获取策略：**
1. 第一优先：笔记内容（Zotero Notes，转换为 Markdown）
2. 第二优先：摘要
3. ❌ 无 PDF 内容获取（基于 V6 的版本限制）

**标签格式：**
- **序号标签：`Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`** — 与 V6 相同
- **返回最多 3 个理论**（与 V6 相同）

**AI Prompt：**
- 英文 Prompt，要求：
  - Only identify clear theory, model, or framework names
  - Use English names for theories
  - **Return at most three theories found across all content**
  - Order by relevance: most important theory first
  - Format: `["Theory 1", "Theory 2", "Theory 3"]` or `["No clear theories"]`

**基于的 V 版本特性：**
- 理论库：扩展别名列表（V6 级别）— 与 V6 相同
- 理论名称格式：`"（中文）英文名称"` — 与 V6 相同
- 标签格式：序号标签 — 与 V6 相同
- 返回理论数量：最多 3 个理论 — 与 V6 相同
- Prompt：英文 Prompt — 与 V6 相同

**与 V6-R 的区别：**
| 特性 | V6-R | V6-Rnote |
|------|------|----------|
| 删除旧标签 | ✅ 是 | ✅ 是（相同） |
| 内容获取策略 | PDF 前 22000 字符 → 摘要 | 笔记 → 摘要（优先笔记） |
| 📒 标签要求 | ❌ 无 | ✅ 必须有 |
| 适用场景 | 所有条目 | 仅已阅读并做笔记的条目 |

**特点：**
- ✅ 笔记优先，充分利用用户阅读笔记
- ✅ 序号标签，理论顺序明确（最重要的理论为 `Theory/1理论名`）
- ✅ 删除旧标签，避免累积（与 R 系列相同）
- ✅ 理论库扩展别名列表，提高匹配精度（V6 级别）
- ✅ 理论名称中文化，便于中文用户理解（V6 级别）
- ❌ 无 PDF 内容获取，如果笔记内容不足，只能使用摘要

**使用场景：**
- 用户已经阅读并标记了笔记的文献
- 笔记中包含了理论分析内容
- 需要充分利用用户阅读笔记识别理论
- 需要识别最多 3 个理论（与 V6 相同）

---


### 特殊工具文件说明

#### Tag4-1 系列：提取新理论工具
**功能：** 从已处理的文献中提取所有 `Theory/*-new` 标签，用于识别新理论，便于后续添加到理论库。

**版本：**
- **V1（理论标签提取新理论 1）**：基础版本，提取所有 `Theory/*-new` 标签，按字母排序输出
- **V2（理论标签提取新理论 2）**：改进版本，优化输出格式
- **V3（理论标签提取新理论 3）**：进一步优化，添加统计信息

**使用场景：**
- 批量提取新理论，准备添加到理论库
- 审查 AI 识别的新理论，判断是否需要标准化

---


#### Tag4-2 系列：更新新增理论标签
**功能：** 只更新 `Theory/*-new` 标签，尝试匹配理论库，如果匹配成功则替换为标准理论名称。

**版本：**
- **V1（理论标签更新新增 V1）**：基础版本，更新 `Theory/*-new` 标签
- **V2（理论标签更新新增 V2）**：改进版本，优化匹配逻辑

**使用场景：**
- 理论库更新后，将之前的 `-new` 标签匹配到新的理论库
- 标准化理论标签，将临时标签替换为正式标签

---


#### Tag4-3 系列：更新全部理论标签
**功能：** 更新所有 `Theory/` 标签（包括标准理论和新理论），尝试匹配理论库，统一标签格式。

**版本：**
- **V1（理论标签更新全部 V1）**：基础版本，更新所有 Theory/ 标签
- **V2（理论标签更新全部 V2）**：改进版本，优化匹配逻辑

**使用场景：**
- 理论库更新后，批量更新已有标签
- 统一标签格式（如从无序号到有序号）
- 修正错误的理论标签

---


### 技术演进路径

#### 版本演进时间线

```
V1 (2024-06-20) → V2 (2025-07-31) → V3 (2025-08-08) → V4 (2025-10-12)
                                                                  ↓
                                                          V5 (2025-10-12) → V6 (2025-10-13)
                                                                  ↓              ↓
                                                            V5-R (2025-10-12)  V6-R (2025-10-13)
                                                                  ↓              ↓
                                                          V4-Rnote (2025-08-21) V6-Rnote (2025-10-13)
                                                          [基于V4的笔记优先版本] [基于V6的笔记优先版本]
```


#### 关键改进里程碑
1. **V1 → V2**：别名系统、快速匹配、跳过机制、理论关键词检查
2. **V2 → V3**：PDF 优先策略（改变内容获取优先级）、PDF 字符数从 2000 增加到 3000、错误状态标签标准化
3. **V3 → V4**：Prompt 优化、理论关键词检查优化
4. **V4 → V5**：序号标签、理论名称中文化、多理论支持（1-3 个）、英文 Prompt、统一保存函数
5. **V5 → V6**：PDF 字符数从 3000 扩展到 22000、别名列表大幅扩展


#### 内容获取策略演进

```
V1: 摘要优先 → PDF全文
V2: 摘要优先 → PDF前2000字符
V3-V5: PDF优先 → PDF前3000字符 → 摘要（V3引入PDF优先策略）
V6: PDF优先 → PDF前22000字符 → 摘要

V4-Rnote: 笔记优先 → 笔记 → 摘要（无PDF备选，基于V4）
V6-Rnote: 笔记优先 → 笔记 → 摘要（无PDF备选，基于V6）
```


#### 标签格式演进

```
V1-V4: Theory/理论名 或 Theory/理论名-new (无序号，返回1个理论)
V5-V6: Theory/1理论名, Theory/2理论名, Theory/3理论名 (有序号，返回1-3个理论)
V4-Rnote: Theory/理论名 或 Theory/理论名-new (无序号，返回多个理论，基于V4)
V6-Rnote: Theory/1理论名, Theory/2理论名, Theory/3理论名 (有序号，返回最多3个理论，基于V6)
```


#### Prompt 语言演进

```
V1-V4: 中文 Prompt
V5-V6: 英文 Prompt (更精确的理论名称识别)
```

---


### 使用建议

#### 选择合适版本的指南

##### 标准处理（首次标注）
- **推荐：V6**（最新版本，PDF 22000 字符，别名列表扩展，序号标签，支持多理论）
- **备选：V5**（PDF 3000 字符，功能完整，处理速度更快）


##### 重新处理（批量更新）
- **推荐：V6-R**（基于 V6，PDF 22000 字符，扩展别名列表）
- **备选：V5-R**（基于 V5，PDF 3000 字符，处理速度更快）


##### 笔记优先（已阅读并标记笔记的文献）
- **推荐：V6-Rnote**（基于 V6，笔记优先，序号标签，理论库扩展别名列表，返回最多 3 个理论）
- **备选：V4-Rnote**（基于 V4，笔记优先，仅笔记和摘要，无 PDF 备选，无序号标签，返回多个理论）


##### 维护工具
- **提取新理论：Tag4-1 系列**（从已处理文献中提取 `Theory/*-new` 标签）
- **更新新增标签：Tag4-2 系列**（只更新 `Theory/*-new` 标签）
- **更新全部标签：Tag4-3 系列**（更新所有 Theory/ 标签）

---


### 注意事项
1. **版本命名问题**：
   - ~~`V6-Rnote`~~ **已删除**（2026-01-11）。该文件虽然文件名包含 "Rnote"，但实际上**没有笔记优先逻辑**，它只是一个基于 V6 的重新处理版本，且功能与 V6-R.md 重复，已被删除。

2. **理论库更新**：
   - 理论库更新后（特别是别名扩展），建议使用 R 系列版本重新处理已有文献，以提高匹配精度。

3. **PDF 字符数选择**：
   - PDF 3000 字符：处理速度快，适合理论通常在引言部分的情况
   - PDF 22000 字符：处理速度较慢，但覆盖更多内容，适合理论引用较深的文献

4. **笔记优先版本使用**：
   - 笔记优先版本（V4-Rnote、V6-Rnote）**仅处理带有 📒 标签的条目**，使用前请确保条目已标记 📒 标签。
   - **V4-Rnote**：基于 V4，功能相对简单（笔记 → 摘要，无 PDF 备选，无序号标签）
   - **V6-Rnote**：基于 V6，功能更完整（笔记 → 摘要，无 PDF 备选，有序号标签，理论库扩展别名列表）

5. **序号标签**：
   - V5 及以后版本支持序号标签（`Theory/1理论名`, `Theory/2理论名`, `Theory/3理论名`），理论按重要性排序，`Theory/1理论名` 为最重要的理论。

---


### 文件列表

#### V 系列（标准版本）
- `ZoteroScript-P1-Tag4理论标签V1.md`
- `ZoteroScript-P1-Tag4理论标签V2.md`
- `ZoteroScript-P1-Tag4理论标签V3.md`
- `ZoteroScript-P1-Tag4理论标签V4.md`
- `ZoteroScript-P1-Tag4理论标签V5.md`
- `ZoteroScript-P1-Tag4理论标签V6.md`


#### R 系列（重新处理版本）
- `ZoteroScript-P1-Tag4理论标签V5-R.md`
- `ZoteroScript-P1-Tag4理论标签V6-R.md`


#### Rnote 系列（笔记优先版本）
- `ZoteroScript-P1-Tag4理论标签V4-Rnote.md`（基于 V4 的笔记优先版本）
- `ZoteroScript-P1-Tag4理论标签V6-Rnote.md`（基于 V6 的笔记优先版本）


#### 特殊工具文件
- `ZoteroScript-P1-Tag4-1理论标签提取新理论1.md`
- `ZoteroScript-P1-Tag4-1理论标签提取新理论2.md`
- `ZoteroScript-P1-Tag4-1理论标签提取新理论3.md`
- `ZoteroScript-P1-Tag4-2理论标签更新新增V1.md`
- `ZoteroScript-P1-Tag4-2理论标签更新新增V2.md`
- `ZoteroScript-P1-Tag4-3理论标签更新全部V1.md`
- `ZoteroScript-P1-Tag4-3理论标签更新全部V2.md`

---

**文档创建日期：** 2026-01-11  
**最后更新日期：** 2026-01-11  
**文档版本：** 1.0
