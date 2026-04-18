---
System:
  - Project
Process:
  - 4-WorkProjects
Class:
  - 02TS
Project:
  - BuildZotero
Title: ZoteroScript-P6-AskS4-AskPDFV1
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
  - Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord:
---

## ZoteroScript-P6-AskS4-AskPDFV1

### 🎯 核心作用
AskPDF 智能触发问答系统是一个基于自然语言触发的智能文献问答工具，通过正则表达式自动识别用户输入中的文献相关关键词（如 " 本文 "、" 这篇文章 "、" 论文 "），自动激活 PDF 文献分析功能。该系统结合了智能触发机制和上下文相关检索技术，为用户提供更加自然、便捷的文献问答体验，大大降低了学术研究中的操作门槛。

---


### 第一部分：完整代码

```javascript
#📗AskPDF[color=#9C27B0][trigger=/^(本文|这篇文章|论文)/]
You are a helpful assistant. Context information is below.
${
Meet.Zotero.getRelatedText(Meet.Global.input || "概括本文")
}$
Using the provided context information, write a comprehensive reply to the given query. Make sure to cite results using [number] notation after the reference. If the provided context information refer to multiple subjects with the same name, write separate answers for each subject. Use prior knowledge only if the given context didn't provide enough information.

Answer the question: ${Meet.Global.input || "概括本文"}$

Reply in zh-CN.
```

---


### 第二部分：代码逻辑图

```mermaid
flowchart TD
    A["用户输入文本"] --> B["正则表达式检测"]
    B --> C{"匹配触发词？"}
    
    C -->|匹配| D["自动激活AskPDF"]
    C -->|不匹配| E["等待用户操作"]
    
    D --> F["获取用户完整输入"]
    F --> G["调用Meet.Zotero.getRelatedText"]
    G --> H["智能检索相关文献内容"]
    
    H --> I["基于输入关键词<br/>匹配文献片段"]
    I --> J["提取最相关的<br/>文献段落"]
    J --> K["构建上下文信息"]
    
    K --> L["AI角色激活<br/>学术助手模式"]
    L --> M["分析用户问题意图"]
    M --> N{"多主题检测"}
    
    N -->|单一主题| O["生成综合性回答"]
    N -->|多个主题| P["分别回答各主题"]
    
    O --> Q["添加引用标记<br/>数字格式"]
    P --> Q
    Q --> R["中文格式输出"]
    
    R --> S["返回最终答案"]
    
    E --> T["用户手动触发其他功能"]
    
    style A fill:#e1f5fe
    style D fill:#fff3e0
    style S fill:#c8e6c9
    style B fill:#f3e5f5
    style C fill:#f3e5f5
```

---
