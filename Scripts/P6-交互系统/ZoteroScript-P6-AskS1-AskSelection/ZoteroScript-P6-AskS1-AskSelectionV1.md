---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P6-AskS1-AskSelectionV1
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
  - 代码实现
  - 即时分析
  - 文本选择
  - 学习工具
  - 正则表达式
  - 智能阅读
  - 智能助手
  - AskSelection
  - PDF分析
  - Zotero插件
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P 6-AskS1-AskSelectionV1

### 🎯 核心作用
AskSelection 文本选择分析系统是一个专门针对用户选中文本内容进行智能分析的工具。该系统通过正则表达式自动识别用户输入中的文本选择相关关键词（如 " 这段文本 "、" 选中文字 " 等），智能获取用户在 PDF 阅读器中选中的文本内容或对话历史中的文本片段，并通过 AI 进行精准的文本分析和问答。作为即时文本分析的核心工具，AskSelection 将用户的瞬时选择转化为深度洞察，为精准阅读、重点分析和问题解答提供即时智能支持。

---


### 第一部分：完整代码

```javascript
#📖AskSelection[color=#2196F3][trigger=/(这段|选中)(文本|话|文字|描述)/]
Read these content:
${
Meet.Zotero.getPDFSelection() ||
Meet.Global.views.messages[0].content
}$
---
Answer me in the language of my question. This is my question: ${Meet.Global.input}$
```

---


### 第二部分：代码逻辑图

```mermaid
flowchart TD
    A[用户输入包含选择关键词] --> B[正则表达式匹配检测]
    B --> C{匹配选择触发词}
    
    C -->|匹配成功| D[激活Selection分析模式]
    C -->|匹配失败| E[等待其他功能触发]
    
    D --> F[尝试获取选中内容]
    F --> G[调用getPDFSelection方法]
    G --> H{PDF中有选中文本}
    
    H -->|有选中内容| I[获取PDF选中文本]
    H -->|无选中内容| J[获取对话历史首条消息]
    
    I --> K[文本内容验证]
    J --> K
    K --> L{内容是否有效}
    
    L -->|内容有效| M[构建分析上下文]
    L -->|内容无效| N[返回无内容提示]
    
    M --> O[设置语言适应模式]
    O --> P[检测用户问题语言]
    P --> Q[配置同语言回答]
    
    Q --> R[AI接收选中内容]
    R --> S[分析用户问题意图]
    S --> T{问题类型识别}
    
    T -->|解释说明| U[解释选中内容含义]
    T -->|分析评价| V[分析内容特点价值]
    T -->|翻译转换| W[进行语言转换处理]
    T -->|总结概括| X[概括内容要点]
    
    U --> Y[生成针对性回答]
    V --> Y
    W --> Y
    X --> Y
    
    Y --> Z[输出最终分析结果]
    
    E --> AA[使用其他功能模块]
    N --> BB[提示用户选择内容]
    
    style A fill:#ffe6ec
    style D fill:#ffb3c1
    style Z fill:#74b9ff
    style B fill:#fd79a8
    style C fill:#fd79a8
    style G fill:#a29bfe
    style R fill:#ffeaa7
```

---
