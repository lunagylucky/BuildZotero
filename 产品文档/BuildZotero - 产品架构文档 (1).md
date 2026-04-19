---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: BuildZotero - 产品架构文档
DateCreated: 2026-01-17 17:37
DateModified: 2026-04-18 17:38
Type:
  - publish
Status:
  - doing
Version: v1.0
CardStatus: true
CardType:
  - card-project
tags:
  - Topic/工具技能/工作笔记
  - Pattern/Memo
RelatedNote:
RelatedProjects:
CardRecord:
围绕 BuildZotero 下的 BuildZotero - 产品架构文档 沉淀可复用经验，支持检索、复盘与迭代。
---

> [!summary] 项目概览｜BuildZotero 产品架构文档
> 描述 BuildZotero 的架构原则、系统架构、模块/数据/技术/集成与安全架构，便于开发与协作对齐。

# <span style="color:#3B8650;">文档目录</span>

> [!info] 文档目录
> 1. [[#1. 架构概述|架构概述]]
> 2. [[#2. 系统架构|系统架构]]
> 3. [[#3. 模块架构|模块架构]]
> 4. [[#4. 数据架构|数据架构]]
> 5. [[#5. 技术架构|技术架构]]
> 6. [[#6. 集成架构|集成架构]]
> 7. [[#7. 安全架构|安全架构]]

# <span style="color:#3B8650;">1. 架构概述</span>

## <span style="color:#43973D;">1.1 架构原则</span>

> [!info] 架构原则
> BuildZotero 采用以下架构原则：
> 1. <span style="color:#048562; font-weight:bold;">多维标注体系驱动</span>：通过 8 维标签体系（标准化标注规范）实现结构化信息提取
> 2. <span style="color:#048562; font-weight:bold;">模块化设计</span>：每个模块独立运行，可灵活组合
> 3. <span style="color:#048562; font-weight:bold;">版本化管理</span>：支持多版本并存，便于迭代和回滚
> 4. <span style="color:#048562; font-weight:bold;">AI 驱动</span>：核心能力基于大语言模型和 Prompt 工程，多模型配置策略
> 5. <span style="color:#048562; font-weight:bold;">白盒化维护</span>：标签在 Zotero 中完全可见、可改，用户可随时手动核准
> 6. <span style="color:#048562; font-weight:bold;">工作流深度集成</span>：与 Zotero、Obsidian、Cursor 双向链接，打通科研工作流
> 7. <span style="color:#048562; font-weight:bold;">可扩展性</span>：支持功能扩展和定制
> 8. <span style="color:#048562; font-weight:bold;">知识资产化</span>：标签体系是永久的知识资产，可复用、可扩展
> 9. <span style="color:#048562; font-weight:bold;">理论库循环更新</span>：标准理论库匹配 + 新理论标记，理论库持续增值
> 10. <span style="color:#048562; font-weight:bold;">模型配置策略</span>：不同任务配置不同的大模型以节省 Tokens 费用

## <span style="color:#43973D;">1.2 架构层次</span>

```mermaid
graph TB
    A[用户界面层] --> B[业务逻辑层]
    B --> C[数据访问层]
    B --> D[AI 服务层]
    C --> E[数据存储层]
    D --> F[外部 API]
    
    A1[Zotero UI] --> A
    A2[脚本触发] --> A
    
    B1[P0-基础操作] --> B
    B2[P1-标签解构] --> B
    B3[P2-标签统计] --> B
    B4[P3-自定义筛选] --> B
    B5[P4-文献综述] --> B
    B6[P5-文献引用] --> B
    B7[P6-交互系统] --> B
    
    C1[Zotero API] --> C
    C2[SQLite] --> C
    
    D1[DeepSeekV3 API] --> D
    D2[DeepSeekR1 API] --> D
    D3[Gemini 2.5 API] --> D
    D4[Gemini 3.0 API] --> D
    
    E1[Zotero 数据库] --> E
    E2[标签数据] --> E
    E3[元数据] --> E
```

---

# <span style="color:#3B8650;">2. 系统架构</span>

## <span style="color:#43973D;">2.1 整体架构（多维标注体系驱动）</span>

```mermaid
graph TD
    A[非结构化PDF] --> B{AI 语义解构器<br/>标注规范协议}
    B -->|8维标签体系| C[结构化标签云]
    C --> D[(Zotero 关系型数据库)]
    D --> E[多维组合检索<br/>P3模块]
    D --> F[自动化矩阵生成<br/>P4模块]
    D --> G[零幻觉对话<br/>P6模块]
    E --> H[精准文献定位]
    F --> I[学术产出成果]
    G --> J[证据溯源回答]
    
    style C fill:#f9c23c,stroke:#333
    style D fill:#0EA293,stroke:#333,color:#fff
    style B fill:#fff3e0,stroke:#333
```

<span style="color:#048562; font-weight:bold;">核心流程</span>：
1. <span style="color:#048562; font-weight:bold;">内容获取层</span>：从 Zotero 提取 PDF/笔记/摘要
2. <span style="color:#048562; font-weight:bold;">Prompt 构建层</span>：基于 8 维标签体系（标注规范）构建结构化 Prompt
3. <span style="color:#048562; font-weight:bold;">AI 生成层</span>：调用大语言模型生成结构化标签
4. <span style="color:#048562; font-weight:bold;">数据存储层</span>：标签写入 Zotero 数据库（永久资产）
5. <span style="color:#048562; font-weight:bold;">应用层</span>：多维检索、矩阵生成、对话查询

## <span style="color:#43973D;">2.2 多维标注体系驱动的架构组件</span>

| 组件 | 职责 | 技术栈 | 标注规范设计 |
|------|------|--------|------------|
| <span style="color:#048562; font-weight:bold;">脚本引擎</span> | 执行 JavaScript 脚本 | Zotero Script API | - |
| <span style="color:#048562; font-weight:bold;">标注规范设计器</span> | 定义 8 维标签体系 | Prompt 工程 | 8 维标签体系（标准化标注规范） |
| <span style="color:#048562; font-weight:bold;">语义解构器</span> | AI 驱动的标签提取 | 大语言模型 API | 11 维分析框架 |
| <span style="color:#048562; font-weight:bold;">数据存储</span> | 标签数据持久化 | Zotero SQLite | 关系型数据库 |
| <span style="color:#048562; font-weight:bold;">多维检索</span> | 基于标签的逻辑检索 | Zotero API | 标签组合查询 |
| <span style="color:#048562; font-weight:bold;">模块路由</span> | 路由请求到对应模块 | JavaScript |
| <span style="color:#048562; font-weight:bold;">AI 服务</span> | 调用大语言模型 API | HTTP API |
| <span style="color:#048562; font-weight:bold;">数据访问</span> | 访问 Zotero 数据库 | Zotero API |
| <span style="color:#048562; font-weight:bold;">输出处理</span> | 格式化输出结果 | Markdown, JSON |

---

# <span style="color:#3B8650;">3. 模块架构</span>

## <span style="color:#43973D;">3.1 模块依赖关系</span>

```mermaid
graph TD
    P0[P0-基础操作] --> P1[P1-标签解构]
    P1 --> P2[P2-标签统计]
    P1 --> P3[P3-自定义筛选]
    P1 --> P4[P4-文献综述]
    P1 --> P5[P5-文献引用]
    P6[P6-交互系统] --> P1
    
    style P0 fill:#e3f2fd
    style P1 fill:#fff3e0
    style P2 fill:#e8f5e8
    style P3 fill:#f3e5f5
    style P4 fill:#fce4ec
    style P5 fill:#fff8e1
    style P6 fill:#f0f4ff
```

## <span style="color:#43973D;">3.2 模块内部架构</span>

### <span style="color:#7CA531;">P1- 文献标签解构模块</span>

```mermaid
graph LR
    A[输入: 选中文献] --> B[内容获取策略]
    B --> C[PDF 全文]
    B --> D[摘要]
    B --> E[用户笔记]
    
    C --> F[AI 分析引擎]
    D --> F
    E --> F
    
    F --> G[标签提取]
    G --> H[Tag1-主题]
    G --> I[Tag3-方法]
    G --> J[Tag4-样本]
    G --> K[Tag5-理论]
    G --> L[Tag6-结论]
    G --> M[Tag7-变量]
    G --> N[Tag8-条目细节]
    
    H --> O[标签存储]
    I --> O
    J --> O
    K --> O
    L --> O
    M --> O
    N --> O
    
    O --> P[输出: 标签列表]
```

### <span style="color:#7CA531;">P4- 文献综述模块</span>

```mermaid
graph TD
    A[输入: 选中文献] --> B[标签提取]
    B --> C[11 维分析]
    
    C --> D[维度 1: 研究背景]
    C --> E[维度 2: 构念定义]
    C --> F[维度 3: 测量方法]
    C --> G[维度 4: 理论视角]
    C --> H[维度 5: 对象层次]
    C --> I[维度 6: 研究情境]
    C --> J[维度 7: 研究方法]
    C --> K[维度 8: 前因因素]
    C --> L[维度 9: 作用后果]
    C --> M[维度 10: 中介机制]
    C --> N[维度 11: 调节因素]
    
    D --> O[AI 综合分析]
    E --> O
    F --> O
    G --> O
    H --> O
    I --> O
    J --> O
    K --> O
    L --> O
    M --> O
    N --> O
    
    O --> P[格式化输出]
    P --> Q[5 列表格]
    P --> R[2 列表格]
```

---

# <span style="color:#3B8650;">4. 数据架构</span>

## <span style="color:#43973D;">4.1 数据模型</span>

### <span style="color:#7CA531;">标签数据模型</span>

```mermaid
erDiagram
    ITEM ||--o{ TAG : has
    TAG ||--o{ TAG_VALUE : contains
    
    ITEM {
        int id
        string title
        string abstract
        string pdf_path
        date created_date
    }
    
    TAG {
        int id
        string prefix
        string type
        string description
    }
    
    TAG_VALUE {
        int id
        int tag_id
        int item_id
        string value
        int sequence
        date created_date
    }
```

### <span style="color:#7CA531;">标签体系结构</span>

```
标签体系
├── 变量标签 (A 系列)
│   ├── A1-DV/ (因变量)
│   ├── A2-IV/ (自变量)
│   ├── A3-MO/ (调节变量)
│   ├── A4-ME/ (中介变量)
│   ├── A5-INV/ (工具变量)
│   └── A6-CV/ (控制变量)
├── 内容标签 (V 系列)
│   ├── V1-def/ (变量定义)
│   ├── V2-mea/ (测量方法)
│   ├── V3-con/ (研究贡献)
│   ├── V4-fut/ (研究展望)
│   └── V5-sor/ (样本数据来源)
└── 分类标签
    ├── Item/ (主题)
    ├── Theory/ (理论)
    ├── Result/ (结论)
    ├── Sample/ (样本)
    └── sMeth/ (方法)
```

## <span style="color:#43973D;">4.2 数据流</span>

```mermaid
sequenceDiagram
    participant U as 用户
    participant Z as Zotero
    participant B as BuildZotero
    participant A as AI API
    participant D as 数据库
    
    U->>Z: 选择文献
    Z->>B: 触发脚本
    B->>Z: 获取文献数据
    Z->>B: 返回文献信息
    B->>A: 发送分析请求
    A->>B: 返回分析结果
    B->>D: 存储标签
    D->>B: 确认存储
    B->>Z: 更新文献标签
    Z->>U: 显示结果
```

---

# <span style="color:#3B8650;">5. 技术架构</span>

## <span style="color:#43973D;">5.1 技术栈</span>

| 层级                                                          | 技术             | 版本     | 说明                 |
| ----------------------------------------------------------- | -------------- | ------ | ------------------ |
| <span style="color:#048562; font-weight:bold;">平台</span>    | Zotero         | 6.0+   | 文献管理平台             |
| <span style="color:#048562; font-weight:bold;">脚本语言</span>  | JavaScript     | ES6+   | Zotero 脚本语言        |
| <span style="color:#048562; font-weight:bold;">AI 服务</span> | DeepSeekV3 API | Latest | 轻量级任务（P0/P1/P2/P3） |
| <span style="color:#048562; font-weight:bold;">AI 服务</span> | DeepSeekR1 API | Latest | 复杂推理任务（P4/P5）      |
| <span style="color:#048562; font-weight:bold;">AI 服务</span> | Gemini 2.5 API | Latest | 复杂推理任务（P4/P5）      |
| <span style="color:#048562; font-weight:bold;">AI 服务</span> | Gemini 3.0 API | Latest | 复杂推理任务（P4/P5）      |
| <span style="color:#048562; font-weight:bold;">AI 服务</span> | 自定义模型          | -      | P6 交互系统（根据任务选择）    |
| <span style="color:#048562; font-weight:bold;">数据存储</span>  | SQLite         | 3.x    | Zotero 本地数据库       |
| <span style="color:#048562; font-weight:bold;">集成工具</span>  | Obsidian       | Latest | 知识管理工具             |
| <span style="color:#048562; font-weight:bold;">输出格式</span>  | Markdown       | -      | 文档格式               |

## <span style="color:#43973D;">5.2 API 设计</span>

### <span style="color:#7CA531;">AI API 调用模式</span>

```javascript
// 标准 AI API 调用模式
async function callAI(prompt, content) {
  const request = {
    model: "gpt-4",
    messages: [
      { role: "system", content: prompt },
      { role: "user", content: content }
    ],
    temperature: 0.7,
    max_tokens: 2000
  };
  
  const response = await fetch(API_ENDPOINT, {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${API_KEY}`,
      "Content-Type": "application/json"
    },
    body: JSON.stringify(request)
  });
  
  return await response.json();
}
```

### <span style="color:#7CA531;">Zotero API 使用模式</span>

```javascript
// Zotero API 使用模式
function getSelectedItems() {
  return ZoteroPane.getSelectedItems();
}

function getItemTags(item) {
  return item.getTags();
}

function addTag(item, tag) {
  item.addTag(tag);
  item.saveTx();
}
```

---

# <span style="color:#3B8650;">6. 集成架构</span>

## <span style="color:#43973D;">6.1 Zotero 集成</span>

```mermaid
graph LR
    A[BuildZotero] --> B[Zotero Script API]
    B --> C[Zotero Core]
    C --> D[数据库]
    C --> E[UI 组件]
    
    B --> F[事件监听]
    B --> G[数据访问]
    B --> H[UI 操作]
```

## <span style="color:#43973D;">6.2 Obsidian 集成</span>

```mermaid
graph LR
    A[BuildZotero] --> B[生成 Markdown]
    B --> C[Obsidian 文件]
    C --> D[双向链接]
    C --> E[知识图谱]
    
    D --> F[文献关联]
    E --> G[可视化]
    
    H[P6 交互笔记] --> C
    I[Zotero 内容] --> C
```

<span style="color:#048562; font-weight:bold;">核心特性</span>:
- ✅ P6 的交互笔记以及任意的 Zotero 中的任何内容与 Obsidian 都是双向链接的，同步更新
- ✅ 充分利用 Zotero 与 Obsidian 的特性：Zotero 作为自己的文献库以及图书馆，Obsidian 则是笔记看板以及笔记整理

---

## <span style="color:#43973D;">6.3 Cursor 终端集成</span>

```mermaid
graph LR
    A[BuildZotero] --> B[笔记 Agent]
    B --> C[Cursor 终端]
    C --> D[笔记管理]
    C --> E[笔记操作]
    
    D --> F[笔记整理]
    E --> G[笔记编辑]
```

<span style="color:#048562; font-weight:bold;">核心特性</span>:
- ✅ 利用 Cursor 进行笔记管理以及操作
- ✅ 将整个科研工作流都打通了，让 AI 真正地赋能科研工作

---

## <span style="color:#43973D;">6.4 工作流集成架构</span>

```mermaid
graph LR
    A[Zotero<br/>文献库与图书馆] <--> B[Obsidian<br/>笔记看板与笔记整理]
    B <--> C[Cursor 终端<br/>笔记 Agent 管理]
    A --> D[P6 交互笔记]
    D --> B
    D --> C
```

<span style="color:#048562; font-weight:bold;">核心特性</span>:
- ✅ Zotero ↔ Obsidian ↔ Cursor 双向链接
- ✅ 打通整个科研工作流

---

## <span style="color:#43973D;">6.5 AI 服务集成（多模型配置策略）</span>

```mermaid
graph TD
    A[BuildZotero] --> B[AI 服务适配器]
    B --> C[DeepSeekV3]
    B --> D[DeepSeekR1]
    B --> E[Gemini 2.5]
    B --> F[Gemini 3.0]
    B --> G[自定义模型]
    
    C --> H[P0/P1/P2/P3 模块]
    D --> I[P4/P5 模块]
    E --> I
    F --> I
    G --> J[P6 交互系统]
    
    B --> K[错误处理]
    B --> L[重试机制]
    B --> M[缓存机制]
    B --> N[模型切换]
```

<span style="color:#048562; font-weight:bold;">模型配置策略</span>:
- <span style="color:#048562; font-weight:bold;">P0/P1/P2/P3</span>: 默认使用 DeepSeekV3（轻量级任务）
- <span style="color:#048562; font-weight:bold;">P4/P5</span>: 默认使用 DeepSeekR1 / Gemini 2.5 / Gemini 3.0（复杂推理任务）
- <span style="color:#048562; font-weight:bold;">P6</span>: 根据交互任务自定义模型
- <span style="color:#048562; font-weight:bold;">所有任务</span>: 都可以自由切换模型

---

# <span style="color:#3B8650;">7. 安全架构</span>

## <span style="color:#43973D;">7.1 安全考虑</span>

| 安全领域 | 措施 | 说明 |
|---------|------|------|
| <span style="color:#048562; font-weight:bold;">数据安全</span> | 本地存储 | 所有数据存储在本地 Zotero 数据库 |
| <span style="color:#048562; font-weight:bold;">API 安全</span> | API Key 管理 | 用户自行管理 AI API Key |
| <span style="color:#048562; font-weight:bold;">隐私保护</span> | 数据不上传 | 文献内容不上传到第三方服务器 |
| <span style="color:#048562; font-weight:bold;">访问控制</span> | 本地权限 | 遵循 Zotero 本地权限设置 |

## <span style="color:#43973D;">7.2 数据隐私</span>

- ✅ 所有数据处理在本地完成
- ✅ AI API 调用仅发送必要的文本内容
- ✅ 不存储用户的 API Key
- ✅ 支持离线模式（部分功能）

---

# <span style="color:#3B8650;">8. 性能架构</span>

## <span style="color:#43973D;">8.1 性能优化策略</span>

| 优化策略 | 实现方式 | 效果 |
|---------|---------|------|
| <span style="color:#048562; font-weight:bold;">批量处理</span> | 异步处理多篇文献 | 提升 50%+ 处理速度 |
| <span style="color:#048562; font-weight:bold;">缓存机制</span> | 缓存 AI 分析结果 | 减少 API 调用 |
| <span style="color:#048562; font-weight:bold;">并发控制</span> | 限制并发请求数 | 避免 API 限流 |
| <span style="color:#048562; font-weight:bold;">增量更新</span> | 仅处理新文献 | 节省处理时间 |

## <span style="color:#43973D;">8.2 性能指标</span>

| 指标 | 目标值 | 当前值 |
|------|--------|--------|
| 单篇文献处理时间 | < 30 秒 | 20-30 秒 |
| 批量处理能力 | 50 篇/批次 | 50 篇/批次 |
| API 响应时间 | < 5 秒 | 3-5 秒 |
| 系统可用性 | > 99% | 99%+ |

---

# <span style="color:#3B8650;">9. 扩展架构</span>

## <span style="color:#43973D;">9.1 插件系统（规划中）</span>

```mermaid
graph TD
    A[BuildZotero 核心] --> B[插件接口]
    B --> C[自定义标签提取器]
    B --> D[自定义输出格式]
    B --> E[第三方 AI 服务]
    B --> F[数据导出插件]
```

## <span style="color:#43973D;">9.2 API 开放（规划中）</span>

- RESTful API 接口
- Webhook 支持
- 第三方应用集成
- 移动端 API

---

<span style="color:#048562; font-weight:bold;">文档状态</span>: ✅ 已完成  
<span style="color:#048562; font-weight:bold;">最后更新</span>: 2026-01-12

