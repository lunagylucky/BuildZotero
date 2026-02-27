---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P6-AskS8-AskZoteroV1
DateCreated: 2026-01-17 17:37
DateModified: 2026-02-27 11:59
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


## ZoteroScript-P 6-AskS9-AskZoteroV1

### 🎯 核心作用
AskZotero 是一个基于本地文献库的智能学术问答系统，它能够根据用户输入的学术问题，自动从 Zotero 文献库中检索相关内容，并通过 AI 分析整合多篇文献的信息，生成符合学术规范的综合性回答。该系统将个人文献库转化为智能知识库，为学术研究、文献综述和理论梳理提供强有力的 AI 辅助支持。

---


### 第一部分：完整代码

```javascript
#📚AskZotero[color=#0EA293][trigger=]
You are a helpful assistant. Context informations are below from different papers.
${
Meet.Zotero.getLocalRelatedText(Meet.Global.input)
}$

Using the provided context information come from different paragraphs in different papers, write a comprehensive reply to the given question. Make sure to cite results using [number] notation after the reference. Use the first author and year to represent the article, for example, "Sun et al. (2020)". If the provided context information refer to multiple subjects with the same name, write separate answers for each subject. Use prior knowledge only if the given context didn't provide enough information.

Answer this question in an unbiased, comprehensive, and scholarly tone. 
Reply in zh-CN.

Question: ${Meet.Global.input}$

Answer:
```

---


### 第二部分：代码逻辑图

```mermaid
flowchart TD
    A["用户输入学术问题"] --> B["系统接收问题"]
    B --> C["调用Meet.Zotero.getLocalRelatedText"]
    C --> D["扫描本地Zotero文献库"]
    D --> E["智能匹配相关文献内容"]
    E --> F["提取相关段落和上下文"]
    F --> G["构建AI提示词模板"]
    
    G --> H["AI角色定义<br/>学术助手"]
    H --> I["设置回答要求<br/>学术规范"]
    I --> J["指定引用格式<br/>数字标记"]
    J --> K["设置输出语言<br/>中文回答"]
    
    K --> L["AI分析文献内容"]
    L --> M["整合多源信息"]
    M --> N["生成综合性回答"]
    N --> O["添加标准学术引用"]
    O --> P["输出最终答案"]
    
    P --> Q{"用户满意？"}
    Q -->|否| R["调整问题重新提问"]
    Q -->|是| S["完成问答过程"]
    
    R --> A
    
    style A fill:#e1f5fe
    style P fill:#c8e6c9
    style L fill:#fff3e0
    style M fill:#fff3e0
    style N fill:#fff3e0
```

---
