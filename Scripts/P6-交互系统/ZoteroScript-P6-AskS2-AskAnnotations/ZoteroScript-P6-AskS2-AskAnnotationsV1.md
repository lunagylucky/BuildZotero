---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P6-AskS2-AskAnnotationsV1
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
  - 高亮标注
  - 阅读痕迹
  - PDF注释
  - Zotero插件
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P 6-AskS2-AskAnnotationsV1

### 🎯 核心作用
AskAnnotations PDF 注释分析系统是一个专门针对 PDF 文档中用户注释和高亮内容的智能分析工具。通过正则表达式自动识别用户输入中的注释相关关键词（如 " 注释 "、" 高亮 "、" 标注 "），智能提取用户在 PDF 中标记的重要内容，包括高亮文本、注释备注、标注说明等，并通过 AI 进行深度分析和问答。该系统专注于用户个人阅读痕迹的智能化利用，将被动的阅读标记转化为主动的知识问答，为个性化学习、重点内容回顾和深度思考提供强有力的支持。

---


### 第一部分：完整代码

```javascript
#📖AskAnnotations[color=#2196F3][trigger=/(选中|选择的|选择|所选)?(注释|高亮|标注)/]
These are PDF Annotation contents:
${
Meet.Zotero.getPDFAnnotations(Meet.Global.input.match(/(选中|选择的|选择|所选)/))
}$

Please answer me in the language of my question. Make sure to cite results using [number] notation after the reference. 
My question is: ${Meet.Global.input}$
```

---


### 第二部分：代码逻辑图

```mermaid
flowchart TD
    A[用户输入文本] --> B[正则表达式检测]
    B --> C{匹配注释关键词}
    
    C -->|匹配| D[激活Annotations分析]
    C -->|不匹配| E[等待其他操作]
    
    D --> F[解析输入文本]
    F --> G{检测选择范围标识}
    
    G -->|有选择标识| H[获取选中注释]
    G -->|无选择标识| I[获取所有注释]
    
    H --> J[调用getPDFAnnotations选择模式]
    I --> K[调用getPDFAnnotations全部模式]
    
    J --> L[提取指定范围注释内容]
    K --> M[提取全部注释内容]
    
    L --> N[处理注释数据]
    M --> N
    N --> O[格式化注释信息]
    
    O --> P[包含高亮文本]
    O --> Q[包含用户备注]
    O --> R[包含标注位置]
    
    P --> S[组装注释内容集合]
    Q --> S
    R --> S
    
    S --> T[AI接收注释内容]
    T --> U[检测问题语言]
    U --> V[设置同语言回答]
    
    V --> W[分析用户问题]
    W --> X{问题类型识别}
    
    X -->|总结归纳| Y[生成注释内容摘要]
    X -->|主题分析| Z[分析注释主题分布]
    X -->|深度思考| AA[基于注释深度分析]
    
    Y --> BB[添加引用标记]
    Z --> BB
    AA --> BB
    BB --> CC[输出最终答案]
    
    E --> DD[使用其他功能]
    
    style A fill:#fff3e0
    style D fill:#ffeaa7
    style CC fill:#74b9ff
    style B fill:#fd79a8
    style C fill:#fd79a8
    style N fill:#a29bfe
    style S fill:#a29bfe
```

---
