---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P1-Tag0-文献标签管理体系设计主题架构
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
- 结构化数据
- 科研工具
- 文献管理
- 效率工具
- 学术研究
- 知识管理
- 自动化
- AI标签
- JavaScript
- Zotero
- Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord: ''
---

## 🏗️ 文献数据结构化系统 - 科研知识管理核心环节

### 📍知识输入流程定位

```mermaid
graph TD
    A[📚 文献查找获取<br/>输入归档] --> B[🎯 复利效应思维<br/>查理·芒格启发]
    B --> C[📡 让文献找你<br/>检索式订阅]
    C --> D[🤖 八维标签<br/>AI自动分解]
    D --> E[📊 文献矩阵<br/>Obsidian双链]
    E --> F[🎯 双重价值效应]
    F --> G[📖 知识吸收<br/>AI驱动理解]
    G -.-> A
    
    %% 添加核心细节
    C -.-> C1[1期刊池主题跟踪<br/>2关心作者跟踪<br/>3重点期刊订阅]
    C -.-> C2[WOS检索式订阅<br/>ResearchGate邮件跟踪<br/>Zotero核心期刊订阅]
    D -.-> D1[主题+方法+样本<br/>+理论+结论+变量<br/>+条目细节<br/>理论库规范化]
    E -.-> E1[快速概览+精准定位<br/>+智能调用<br/>Dataview高级检索]
    F -.-> F1[大规模文献处理<br/>+复利时间累积<br/>→穷尽所有未必不可能]
    F -.-> F2[解放更多时间<br/>+知识网络构建<br/>→助力深度思考探索]
    
    %% 突出第二环节
    classDef highlight fill:#ff6b6b,stroke:#d63384,stroke-width:4px,color:#fff
    class D highlight
    
    style A fill:#e3f2fd,stroke:#1976d2,stroke-width:3px
    style B fill:#fff3e0,stroke:#f57f17,stroke-width:2px
    style C fill:#e8f5e8,stroke:#388e3c,stroke-width:3px
    style D fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style E fill:#f0f4ff,stroke:#303f9f,stroke-width:2px
    style F fill:#fff8e1,stroke:#f9a825,stroke-width:3px
    style G fill:#fce4ec,stroke:#e91e63,stroke-width:2px
    
    style C1 fill:#f1f8e9,stroke:#689f38,stroke-width:2px
    style C2 fill:#e8f5e8,stroke:#4caf50,stroke-width:2px
    style D1 fill:#fdf5e6,stroke:#ef6c00
    style E1 fill:#f8f9fa,stroke:#6c757d
    style F1 fill:#e8f5e8,stroke:#4caf50,stroke-width:2px
    style F2 fill:#fff3e0,stroke:#ff9800,stroke-width:2px
```

> 🎯 **核心定位**：文献数据结构化是连接 " 原始文献输入 " 与 " 知识网络构建 " 的关键桥梁，通过 AI 驱动的八维标签体系，将海量文献转化为结构化知识单元。

---


### 🏛️ 系统设计全局图

#### 🏷️ 八维标签系统 + 标签维护

##### 🔅简要版

```mermaid
mindmap
 root((🏷️ 文献标签体系))
   📌 ItemTags
     🔑 关键变量
     🎯 研究主题
     💡 核心概念
   🔬 MethodTags
     📋 方法类型
       📊 定性-案例·📚 定性-理论构建
       📈 定量-计量实证·🔬 定量-自然实验·⚗️ 定量-准实验·📝 定量-问卷调查
       📖 综述-文献综述·📋 综述-方法指导·📰 综述-期刊社评
       🔧 方法-建模·💻 方法-软件·⚙️ 方法-程序
     🎯 主要分析
     🛠️ 辅助分析
   👥 SampleTags
     📊 层次
         🌍 宏观-国家·🗺️ 宏观-地区·🏭 宏观-行业·📜 宏观-政策 
         🏢 中观-企业·🏬 中观-部门·🔗 中观-网络 
         👤 微观-员工·👔 微观-高管·🛒 微观-消费者
     📚 领域
     📋 样本：⏰ 时间+📊规模+🎯 具体对象
   📚 TheoryTags
     📖 标准理论
   📈 ResultTags
     🎯 主要
     📋 次要
     📊 其他
   🔢 VariableTags
     📊 A1-DV/ 因变量
     📈 A2-IV/ 自变量
     🔄 A3-MO/ 调节变量
     🔗 A4-ME/ 中介变量
     🔧 A5-INV/ 工具变量
     📋 A6-CV/ 控制变量
   📝 ItemDetailTags
     🔧 V1-def/ 变量定义
     📐 V2-mea/ 测量方法
     💎 V3-con/ 研究贡献
     🔮 V4-fut/ 研究展望
     📊 V5-sor/ 样本数据来源
   🛠️ 标签维护
     🧹 ClearTags
     🗑️ DeleteTags
     ✨ CleanTitle
```


##### 🔆详细版

```mermaid
mindmap
 root((🏷️ 文献标签体系))
   📌 ItemTags
     🔑 关键变量
     🎯 研究主题
     💡 核心概念
   🔬 MethodTags
     📋 方法类型
       📊 定性-案例·📚 定性-理论构建
       📈 定量-计量实证·🔬 定量-自然实验·⚗️ 定量-准实验·📝 定量-问卷调查
       📖 综述-文献综述·📋 综述-方法指导·📰 综述-期刊社评
       🔧 方法-建模·💻 方法-软件·⚙️ 方法-程序
     🎯 主要分析
       📊 统计模型·📈 计量经济·🤖 机器学习·🧪 实验方法·📚 定性方法
     🛠️ 辅助分析
       ✅ 检验方法·🔍 验证方法·📊 数据处理
   👥 SampleTags
     📊 研究层次
         🌍 宏观-国家·🗺️ 宏观-地区·🏭 宏观-行业·📜 宏观-政策 
         🏢 中观-企业·🏬 中观-部门·🔗 中观-网络 
         👤 微观-员工·👔 微观-高管·🛒 微观-消费者
     📚 研究领域
       📋 预设学科领域
       ✏️ 自定义学科领域
     📋 研究样本
       ⏰ 时间范围
       📊 样本规模
       🎯 具体对象
   📚 TheoryTags
     📖 标准理论
       🤖 AI识别·📚 理论库·✅ 匹配验证·🔧 标准化
           🔄 循环更新:🆕 识别-new标签·🔍 理论库匹配·🔄 标签替换·✨ 标准化
   📈 ResultTags
     🎯 主要
     📋 次要
     📊 其他
   🔢 VariableTags
     📊 A1-DV/ 因变量
       🎯 核心研究变量
       📈 被解释变量
     📈 A2-IV/ 自变量
       🔍 解释变量
       💡 核心影响因素
     🔄 A3-MO/ 调节变量
       ⚖️ 条件效应
       🔀 交互作用
     🔗 A4-ME/ 中介变量
       🌉 中介机制
       🔄 间接效应
     🔧 A5-INV/ 工具变量
       🎲 内生性解决
       📊 因果识别
     📋 A6-CV/ 控制变量
       🛡️ 混杂因素控制
       📊 标准控制变量
   📝 ItemDetailTags
     🔧 V1-def/ 变量定义
       📖 概念界定
       🎯 学术定义
     📐 V2-mea/ 测量方法
       🔢 操作化方法
       📊 测量指标
     💎 V3-con/ 研究贡献
       ✨ 理论贡献
       📊 实证发现
       💡 实践启示
     🔮 V4-fut/ 研究展望
       🚀 未来方向
       🔍 研究局限
       💭 研究建议
     📊 V5-sor/ 样本数据来源
       📅 数据时间范围
       🌍 数据地理范围
       📋 数据来源描述
   🛠️ 标签维护
     🧹 ClearTags
       🗑️ 清理非结构化标签
     🗑️ DeleteTags
       ⌨️ 用户输入前缀·👁️ 预览确认·🎯 精确删除·🔒 安全机制
     ✨ CleanTitle
       🧹 清理HTML字符
```


#### 📊 系统架构概览

```mermaid
graph TB
    subgraph "📥 输入层"
        A1[📖 Zotero文献库] --> A2[📄 PDF文档]
        A1 --> A3[📝 摘要数据]
        A1 --> A4[📋 元数据]
        A1 --> A5[📒 笔记内容]
    end
    
    subgraph "🤖 AI处理引擎"
        B1[🧠 Meet.Global.views.ask]
        B2[🔍 内容提取算法]
        B3[⚡ 智能分析模块]
    end
    
    subgraph "🏷️ 八维标签体系"
        C1[📌 Item Tags<br/>主题标签]
        C2[🔬 Method Tags<br/>方法标签] 
        C3[👥 Sample Tags<br/>样本标签]
        C4[📚 Theory Tags<br/>理论标签]
        C5[📈 Result Tags<br/>结论标签]
        C6[🔢 Variable Tags<br/>变量标签]
        C7[📝 ItemDetail Tags<br/>条目细节标签]
    end
    
    subgraph "🛠️ 维护工具"
        D1[🧹 Clear Tags<br/>标签清理]
        D2[🗑️ Delete Tags<br/>精确删除]
        D3[🔄 Update Theory<br/>理论标准化]
        D4[✨ Clean Title<br/>标题清理]
    end
    
    subgraph "📤 输出层"
        E1[📊 结构化文献库]
        E2[🔍 标签展示系统] 
        E3[➡️ 文献矩阵构建]
    end
    
    A1 --> B2
    A2 --> B2
    A3 --> B2
    A5 --> B2
    B2 --> B1
    B1 --> B3
    B3 --> C1
    B3 --> C2
    B3 --> C3
    B3 --> C4
    B3 --> C5
    B3 --> C6
    B3 --> C7
    
    C1 --> E1
    C2 --> E1
    C3 --> E1
    C4 --> E1
    C5 --> E1
    C6 --> E1
    C7 --> E1
    
    D1 --> E1
    D2 --> E1
    D3 --> C4
    D4 --> E1
    
    E1 --> E2
    E2 --> E3
    
    style C1 fill:#e8f5e8,stroke:#4caf50
    style C2 fill:#fff3e0,stroke:#ff9800
    style C3 fill:#e3f2fd,stroke:#2196f3
    style C4 fill:#f3e5f5,stroke:#9c27b0
    style C5 fill:#fce4ec,stroke:#e91e63
    style C6 fill:#e0f2f1,stroke:#00695c
    style C7 fill:#fff9c4,stroke:#f57f17
```


#### ⚡ 标签标记工作流程图

```mermaid
flowchart TD
    Start([🚀 开始处理]) --> Input[📥 选择文献条目]
    Input --> Check{🔍 检查现有标签}
    
    Check -->|无标签| Extract[📖 内容提取]
    Check -->|有标签| Skip[⏭️ 跳过处理]
    
    Extract --> Content{📄 内容类型}
    Content -->|笔记优先| Notes[📒 笔记内容]
    Content -->|PDF优先| PDF[📋 PDF前N字符]
    Content -->|摘要备选| Abstract[📝 摘要内容]
    Content -->|无内容| NoContent[❌ 标记无内容]
    
    Notes --> AI[🤖 AI分析处理]
    PDF --> AI
    Abstract --> AI
    
    AI --> Parse{🔧 解析结果}
    Parse -->|成功| Tags[🏷️ 生成标签]
    Parse -->|失败| Error[❗ 标记失败]
    
    Tags --> Save[💾 保存条目]
    Error --> Save
    NoContent --> Save
    Skip --> Next
    Save --> Next{➡️ 下一条目?}
    
    Next -->|是| Input
    Next -->|否| Report[📊 生成报告]
    Report --> End([✅ 完成])
    
    style Start fill:#4caf50,color:#fff
    style AI fill:#9c27b0,color:#fff
    style Tags fill:#2196f3,color:#fff
    style End fill:#4caf50,color:#fff
```

---


### 🌟文献标签管理系统工作流

#### 🔅文献标签全局工作流图简要版

```mermaid
flowchart TD
    %% 开始节点
    A[📚 文献导入] --> B{选择处理轮次}
    
    %% 第一轮：快速分类
    B -->|第一轮| C[🚀 摘要快速分类]
    
    %% 第一轮标签矩阵（横向）
    C --> D1[🏷️ 主题<br/>Item/xxx]
    C --> D2[📊 结论<br/>Result/xxx]
    C --> D3[👥 样本<br/>Sample/1-2-3]
    C --> D4[🔬 方法<br/>sMeth/1-2-3]
    C --> D5[🧠 理论<br/>Theory/xxx或-new]
    
    %% 清理汇聚
    D1 --> E[🧹 标签清理]
    D2 --> E
    D3 --> E
    D4 --> E
    D5 --> E
    
    %% 分支：第二轮或可视化
    E --> F{需要精准分类？}
    F -->|是| G[🎯 PDF精准分类]
    F -->|否| M[👁️ 标签可视化]
    
    %% 第二轮处理
    G --> H[🗑️ 删除特定标签]
    
    %% 第二轮标签矩阵（横向）
    H --> I1[🏷️ 主题深度<br/>PDF前8000字符]
    H --> I2[👥 样本精确<br/>详细对象描述]
    H --> I3[🔬 方法完整<br/>全方法学分析]
    H --> I4[🧠 理论全面<br/>全文理论扫描]
    
    %% 第三轮：深度变量分析
    I1 --> J[🔢 变量标签提取]
    I2 --> J
    I3 --> J
    I4 --> J
    
    J --> J1[📊 A1-DV/ 因变量]
    J --> J2[📈 A2-IV/ 自变量]
    J --> J3[🔄 A3-MO/ 调节变量]
    J --> J4[🔗 A4-ME/ 中介变量]
    J --> J5[🔧 A5-INV/ 工具变量]
    J --> J6[📋 A6-CV/ 控制变量]
    
    %% 第四轮：条目细节提取
    J1 --> K[📝 条目细节标签]
    J2 --> K
    J3 --> K
    J4 --> K
    J5 --> K
    J6 --> K
    
    K --> K1[🔧 V1-def/ 变量定义]
    K --> K2[📐 V2-mea/ 测量方法]
    K --> K3[💎 V3-con/ 研究贡献]
    K --> K4[🔮 V4-fut/ 研究展望]
    K --> K5[📊 V5-sor/ 样本数据来源]
    
    %% 理论特殊处理循环
    I1 --> L[🔄 理论循环处理]
    I2 --> L
    I3 --> L
    I4 --> L
    
    L --> L1[🔍 识别-new理论]
    L1 --> L2[👨‍💼 人工审核]
    L2 --> N{确认结果}
    N -->|新理论| O[📝 扩展理论库]
    N -->|已有理论| P[🔗 映射现有理论]
    O --> Q[🔄 批量更新标签]
    P --> Q
    Q --> R[✅ 标准化完成]
    
    %% 理论循环
    R -.->|持续优化| L1
    
    %% 可视化和应用
    R --> M
    K5 --> M
    M --> S[🎯 核心文献分析]
    S --> T[🕸️ 知识图谱构建]
    T --> U[🎓 完成知识输入]
    
    %% 循环回到开始
    U --> V{新文献导入？}
    V -->|是| A
    V -->|否| W[📖 进入下一阶段]
    
    %% 快速通道（跳过第二轮）
    M --> S
    
    %% 样式定义
    style A fill:#e3f2fd,stroke:#1976d2,stroke-width:3px
    style C fill:#fff3e0,stroke:#f57f17,stroke-width:2px
    style G fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    style J fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    style K fill:#fff9c4,stroke:#f57f17,stroke-width:2px
    style L fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style M fill:#f0f4ff,stroke:#303f9f,stroke-width:2px
    style S fill:#fff8e1,stroke:#f9a825,stroke-width:2px
    style T fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    style U fill:#f1f8e9,stroke:#689f38,stroke-width:3px
    style W fill:#fce4ec,stroke:#e91e63,stroke-width:3px
    
    %% 标签矩阵样式
    style D1 fill:#e1f5fe,stroke:#0277bd
    style D2 fill:#f3e5f5,stroke:#8e24aa
    style D3 fill:#e8f5e8,stroke:#4caf50
    style D4 fill:#fff3e0,stroke:#f57f17
    style D5 fill:#ffebee,stroke:#c62828
    
    style I1 fill:#e1f5fe,stroke:#0277bd
    style I2 fill:#e8f5e8,stroke:#4caf50
    style I3 fill:#fff3e0,stroke:#f57f17
    style I4 fill:#ffebee,stroke:#c62828
    
    style J1 fill:#e0f2f1,stroke:#00695c
    style J2 fill:#e0f2f1,stroke:#00897b
    style J3 fill:#e0f2f1,stroke:#00796b
    style J4 fill:#e0f2f1,stroke:#00695c
    style J5 fill:#e0f2f1,stroke:#004d40
    style J6 fill:#e0f2f1,stroke:#004d40
    
    style K1 fill:#fff9c4,stroke:#f57f17
    style K2 fill:#fff9c4,stroke:#fbc02d
    style K3 fill:#fff9c4,stroke:#f9a825
    style K4 fill:#fff9c4,stroke:#f57c00
    style K5 fill:#fff9c4,stroke:#ef6c00
    
    %% 决策节点样式
    style B fill:#fff9c4,stroke:#ff9800
    style F fill:#fff9c4,stroke:#ff9800
    style N fill:#fff9c4,stroke:#ff9800
    style V fill:#fff9c4,stroke:#ff9800
```


#### 🔆文献标签全局工作流图详细版

```mermaid
flowchart TD
    %% 文献导入
    Start[📚 文献库导入<br/>WOS/Scopus/CNKI] --> Phase1[🚀 第一轮：摘要快速分类]
    
    %% 第一轮标签生成矩阵
    Phase1 --> Round1{第一轮标签生成}
    
    Round1 --> Item1[🏷️ Item Tags<br/>• 核心主题识别<br/>• 3个标签<br/>• 摘要优先]
    Round1 --> Result1[📊 Result Tags<br/>• 研究发现<br/>• 实证结果<br/>• 理论贡献]
    Round1 --> Sample1[👥 Sample Tags<br/>• 宏观/中观/微观<br/>• 学科领域<br/>• 具体样本]
    Round1 --> Method1[🔬 Method Tags<br/>• 方法类型<br/>• 主要技术<br/>• 辅助方法]
    Round1 --> Theory1[🧠 Theory Tags<br/>• 关键词扫描<br/>• 理论库匹配<br/>• -new标记]
    
    %% 汇聚到清理
    Item1 --> Clean[🧹 标签清理]
    Result1 --> Clean
    Sample1 --> Clean
    Method1 --> Clean
    Theory1 --> Clean
    
    Clean --> CleanDetail[清理内容：<br/>• 删除原始关键词<br/>• 移除不规范标签<br/>• 保留系统生成标签]
    
    %% 第二轮准备
    CleanDetail --> Phase2[🎯 第二轮：PDF精准分类]
    Phase2 --> Delete[🗑️ 删除特定标签<br/>准备重新分类]
    
    %% 第二轮标签重建
    Delete --> Round2{第二轮标签重建}
    
    Round2 --> Item2[🏷️ Item Tags PDF<br/>• PDF前8000字符<br/>• 深度主题挖掘<br/>• 核心概念识别]
    Round2 --> Sample2[👥 Sample Tags PDF<br/>• 详细样本描述<br/>• 研究对象特征<br/>• 地域行业信息]
    Round2 --> Method2[🔬 Method Tags PDF<br/>• 完整方法学描述<br/>• 统计技术细节<br/>• 实验设计要素]
    Round2 --> Theory2[🧠 Theory Tags PDF<br/>• 全文理论扫描<br/>• 隐含理论发现<br/>• 理论关系分析]
    
    %% 第三轮：变量标签提取
    Item2 --> Phase3[🔢 第三轮：变量标签提取]
    Sample2 --> Phase3
    Method2 --> Phase3
    Theory2 --> Phase3
    
    Phase3 --> Variable[变量标签生成]
    
    Variable --> Var1[📊 A1-DV/ 因变量<br/>• 核心研究变量<br/>• 被解释变量]
    Variable --> Var2[📈 A2-IV/ 自变量<br/>• 解释变量<br/>• 核心影响因素]
    Variable --> Var3[🔄 A3-MO/ 调节变量<br/>• 条件效应<br/>• 交互作用]
    Variable --> Var4[🔗 A4-ME/ 中介变量<br/>• 中介机制<br/>• 间接效应]
    Variable --> Var5[🔧 A5-INV/ 工具变量<br/>• 内生性解决<br/>• 因果识别]
    Variable --> Var6[📋 A6-CV/ 控制变量<br/>• 混杂因素控制<br/>• 标准控制变量]
    
    %% 第四轮：条目细节标签提取
    Var1 --> Phase4[📝 第四轮：条目细节标签提取]
    Var2 --> Phase4
    Var3 --> Phase4
    Var4 --> Phase4
    Var5 --> Phase4
    Var6 --> Phase4
    
    Phase4 --> Detail[条目细节标签生成]
    
    Detail --> Det1[🔧 V1-def/ 变量定义<br/>• 概念界定<br/>• 学术定义]
    Detail --> Det2[📐 V2-mea/ 测量方法<br/>• 操作化方法<br/>• 测量指标]
    Detail --> Det3[💎 V3-con/ 研究贡献<br/>• 理论贡献<br/>• 实证发现<br/>• 实践启示]
    Detail --> Det4[🔮 V4-fut/ 研究展望<br/>• 未来方向<br/>• 研究局限<br/>• 研究建议]
    Detail --> Det5[📊 V5-sor/ 样本数据来源<br/>• 数据时间范围<br/>• 数据地理范围<br/>• 数据来源描述]
    
    %% 理论特殊处理
    Item2 --> TheoryProcess[🔄 理论标签特殊处理]
    Sample2 --> TheoryProcess
    Method2 --> TheoryProcess
    Theory2 --> TheoryProcess
    
    %% 理论循环更新详细流程
    TheoryProcess --> TheoryFlow{理论更新循环}
    
    TheoryFlow --> Scan[🔍 扫描-new标签<br/>• 识别新理论候选<br/>• 提取理论名称<br/>• 统计数量分布]
    
    Scan --> Review[👨‍💼 人工审核<br/>• 验证理论有效性<br/>• 检查重复性<br/>• 确认学术价值]
    
    Review --> Decision{理论确认}
    Decision -->|新理论| AddTheory[📝 理论库扩展<br/>• 添加理论条目<br/>• 设置标准别名<br/>• 更新版本号]
    Decision -->|已有理论| MapTheory[🔗 理论映射<br/>• 添加新别名<br/>• 完善理论条目<br/>• 增强匹配度]
    
    AddTheory --> UpdateTool[🔄 运行更新工具<br/>• 批量扫描文献库<br/>• 重新匹配理论<br/>• 标准化标签名称]
    MapTheory --> UpdateTool
    
    UpdateTool --> Standard[✅ 理论标准化完成<br/>• -new标签清理<br/>• 统一理论命名<br/>• 更新统计报告]
    
    %% 可视化和应用
    Standard --> Display[👁️ 标签可视化]
    Det5 --> Display
    Display --> DisplayDetail[可视化功能：<br/>• 正则表达式筛选<br/>• 多维度展示<br/>• 快速检索定位]
    
    DisplayDetail --> Phase5[🎯 第五轮：核心文献分析]
    Phase5 --> Matrix[🕸️ 文献矩阵构建<br/>• 全文深度分析<br/>• 知识关系挖掘<br/>• 概念网络构建]
    
    Matrix --> Integration[🔗 工具链集成<br/>• Obsidian Dataview<br/>• 动态文献表格<br/>• 知识图谱生成]
    
    Integration --> Complete[🎓 知识输入完成]
    
    %% 循环判断
    Complete --> NewLit{新文献？}
    NewLit -->|是| Start
    NewLit -->|否| NextStage[📖 下一学术阶段<br/>知识分析与应用]
    
    %% 持续优化循环
    Standard -.->|理论库持续更新| Scan
    DisplayDetail -.->|发现问题反馈| Review
    
    %% 样式定义
    style Start fill:#e3f2fd,stroke:#1976d2,stroke-width:3px
    style Phase1 fill:#fff3e0,stroke:#f57f17,stroke-width:2px
    style Phase2 fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    style Phase3 fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    style Phase4 fill:#fff9c4,stroke:#f57f17,stroke-width:2px
    style Phase5 fill:#fff8e1,stroke:#f9a825,stroke-width:2px
    style Clean fill:#ffebee,stroke:#c62828,stroke-width:2px
    style TheoryProcess fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style Complete fill:#f1f8e9,stroke:#689f38,stroke-width:3px
    style NextStage fill:#fce4ec,stroke:#e91e63,stroke-width:3px
    
    %% 特殊节点样式
    style Round1 fill:#fff9c4,stroke:#f57f17
    style Round2 fill:#e8f5e8,stroke:#4caf50
    style TheoryFlow fill:#f3e5f5,stroke:#9c27b0
    style Decision fill:#ffecb3,stroke:#ff9800
```


#### 🚀 五轮工作流程

##### 1️⃣第一轮：快速分类（基于摘要）

```mermaid
flowchart LR
    A[导入文献] --> B[主题标签]
    B --> C[结论标签]
    C --> D[样本标签]
    D --> E[方法标签]
    E --> F[理论标签]
    F --> G[标签清理]
    
    style A fill:#e1f5fe
    style G fill:#ffebee
```

**特点**：

- **处理量**：近万条文献
- **数据源**：文献摘要（>50 字符）
- **目标**：建立基础标签体系
- **效率**：高速批量处理

**操作顺序**：

1. 从 WOS 等数据库导入文献
2. 依次运行 5 个摘要版本标签工具
3. 运行清理标签工具，移除不规范标签（如原始关键词）

**处理逻辑**：

- 优先获取摘要内容进行分析
- 摘要不可用时备选 PDF 前 3000-8000 字符（根据模块不同）
- 无内容文献标记为 " 无内容 " 便于后续处理


##### 2️⃣第二轮：精准分类（基于 PDF）

```mermaid
flowchart LR
    A[选择目标文献] --> B[删除特定标签]
    B --> C[PDF版本重新标记]
    C --> D[理论标签更新]
    
    style A fill:#fff3e0
    style D fill:#e8f5e8
```

**特点**：

- **处理量**：项目相关文献子集
- **数据源**：PDF 全文内容前 8000-45000 字符（根据模块不同）
- **目标**：精准分类，深度信息提取
- **质量**：高精度标签标注

**操作顺序**：

1. 筛选项目相关文献
2. 使用删除特定标签工具清理旧标签
3. 运行 PDF 版本标签工具重新分类
4. 运行理论标签更新工具标准化理论

**优势分析**：

- 基于更丰富的全文信息
- 能识别摘要中未体现的细节
- 适用于核心文献的深度分析


##### 3️⃣第三轮：变量标签提取

```mermaid
flowchart LR
    A[核心文献选择] --> B[变量标签提取]
    B --> C[A1-DV 因变量]
    B --> D[A2-IV 自变量]
    B --> E[A3-MO 调节变量]
    B --> F[A4-ME 中介变量]
    B --> G[A5-INV 工具变量]
    B --> H[A6-CV 控制变量]
    
    style A fill:#e0f2f1
    style B fill:#00695c,color:#fff
```

**特点**：

- **处理量**：核心研究文献
- **数据源**：PDF 前 30000 字符或笔记
- **目标**：识别研究变量关系
- **应用**：变量关系分析、模型构建

**实现方式**：

- 使用变量标签提取工具（Tag7）
- 识别 6 类变量：DV、IV、MO、ME、INV、CV
- 支持 " 无 " 值处理（根据版本不同）


##### 4️⃣第四轮：条目细节标签提取

```mermaid
flowchart LR
    A[核心文献选择] --> B[条目细节标签提取]
    B --> C[V1-def 变量定义]
    B --> D[V2-mea 测量方法]
    B --> E[V3-con 研究贡献]
    B --> F[V4-fut 研究展望]
    B --> G[V5-sor 样本数据来源]
    
    style A fill:#fff9c4
    style B fill:#f57f17,color:#fff
```

**特点**：

- **处理量**：核心研究文献
- **数据源**：PDF 前 45000 字符或笔记
- **目标**：提取研究详细要素
- **应用**：研究贡献分析、未来方向识别

**实现方式**：

- 使用条目细节标签提取工具（Tag8）
- 识别 5 类信息：变量定义、测量方法、研究贡献、研究展望、样本数据来源


##### 5️⃣第五轮：深度分析（全文矩阵）

```mermaid
flowchart LR
    A[核心文献选择] --> B[全文分析]
    B --> C[文献矩阵构建]
    C --> D[Obsidian集成]
    
    style A fill:#fff8e1
    style D fill:#e8f5e8
```

**特点**：

- **处理量**：核心研究文献
- **数据源**：完整文献内容
- **目标**：知识图谱构建
- **应用**：配合 Dataview 生成文献表格

**实现方式**：

- 利用已标记的结构化标签数据
- 通过 Obsidian Dataview 插件生成动态表格
- 实现文献的多维度交叉分析

---


### 📚 模块解读

#### 🏷️ 核心标签生成模块（8 维度）
| 模块                | 功能定位   | 核心特征                         | 标签前缀 | 数量  |
| ----------------- | ------ | ---------------------------- | ------ | --- |
| 📌 **ItemTags**   | 主题标签提取 | 3-6 字符简洁标签，聚焦核心变量与概念         | Item/  | 3   |
| 🔬 **MethodTags** | 研究方法识别 | 三层结构：方法类型 + 主要分析 + 辅助分析      | sMeth/ | 2-5 |
| 👥 **SampleTags** | 样本维度分析 | 三维标签：研究层次 + 学科领域 + 样本描述      | Sample/| 3   |
| 📚 **TheoryTags** | 理论框架匹配 | 140+ 理论库智能匹配，支持新理论识别，理论库循环更新 | Theory/| 1-3 |
| 📈 **ResultTags** | 结论发现提取 | 15-20 字符内精确结论，专注研究发现         | Result/| 3   |
| 🔢 **VariableTags** | 变量关系识别 | 六类变量：DV、IV、MO、ME、INV、CV       | A1-DV/ 等 | 1-6 |
| 📝 **ItemDetailTags** | 研究要素提取 | 五类信息：定义、测量、贡献、展望、数据来源     | V1-def/ 等 | 5   |


#### 🛠️ 系统维护模块

```mermaid
graph LR
    A[🔄 UpdateTheoryTags] -->|标准化| B[📚 理论库统一]
    C[🧹 ClearTags] -->|批量清理| D[🏷️ 标签规范化]
    E[🗑️ DeleteTags] -->|精确删除| F[🎯 特定标签清理]
    G[✨ CleanTitle] -->|格式清理| H[📝 标题标准化]
    
    style A fill:#ff9800,color:#fff
    style C fill:#ff6b6b,color:#fff
    style E fill:#ff6b6b,color:#fff
    style G fill:#ff6b6b,color:#fff
```


#### 🎨 标签显示配置

```mermaid
graph LR
   A[📋 标签显示需求] --> B[🔍 通过正则表达式及文本活跃<br/>显示标识模式]
   
   B --> C[📊 全量显示]
   B --> D[🔬 单类别]
   B --> E[🔧 组合]
   B --> F[❌ 排除]
   
   style A fill:#e3f2fd,stroke:#1976d2
   style B fill:#fff3e0,stroke:#ff9800
   style C fill:#e8f5e8,stroke:#4caf50
   style D fill:#e8f5e8,stroke:#4caf50
   style E fill:#e8f5e8,stroke:#4caf50
   style F fill:#e8f5e8,stroke:#4caf50
```

**例：全量正则表达式**：`^((?:Item|Theory|Sample|sMeth|Result|A[1-6]-(?:DV|IV|MO|ME|INV|CV)|V[1-5]-(?:def|mea|con|fut|sor))\/)(.+)$`

|原始标签|显示效果|
|---|---|
|`Item/数字化转型`|数字化转型|
|`Theory/Resource-based view (RBV)`|Resource-based view (RBV)|
|`sMeth/1定量-计量实证`|1 定量 - 计量实证|
|`A1-DV/创新绩效`|创新绩效|
|`V1-def/技术多样性:...`|技术多样性:...|

---


### 💎 系统价值与创新点

#### ✨核心价值

```mermaid
mindmap
  root((🎯 系统价值))
    🚀 效率革命
      批量处理能力
      AI驱动自动化
      复利时间累积
    🧠 认知增强
      八维知识拆解
      结构化思维
      知识网络构建
    🔗 生态连接
      上游文献获取
      下游矩阵构建
      闭环知识管理
    📊 数据驱动
      量化分析基础
      精准检索支持
      智能推荐准备
```


#### ⚡创新特征
- **🎯 八维解构**：将复杂文献分解为主题、方法、样本、理论、结论、变量、条目细节八个标准维度
- **🤖 AI 驱动**：结合大语言模型与专业理论库的混合智能系统
- **🔄 自我进化**：理论库持续更新，标签体系不断优化
- **🏗️ 生态整合**：无缝连接文献获取与知识网络构建

---


### 🔮 未来发展方向

```mermaid
timeline
    title 🚀 系统演进路线图
    
    section 当前阶段
        八维标签体系 : 核心功能完备
        理论库建设 : 140+标准理论
        AI分析引擎 : 基础自动化
        变量关系分析 : 六类变量识别
        条目细节提取 : 五类信息识别
    
    section 近期优化
        智能推荐 : 相关文献发现
        关联分析 : 标签间关系挖掘
        质量评估 : 标签准确性提升
        自定义筛选 : 基于标签的智能筛选
    
    section 长期愿景
        知识图谱 : 全域知识网络
        个性化 : 用户偏好学习
        协同创新 : 团队知识共享
```

---

**💡 核心价值**：该系统通过 AI 驱动的八维标签体系，将文献转化为结构化知识，为科研工作者提供高效、智能的知识管理工具。

---

#Zotero #知识管理 #AI标签 #文献管理 #科研工具 #自动化 #结构化数据 #JavaScript #学术研究 #效率工具
