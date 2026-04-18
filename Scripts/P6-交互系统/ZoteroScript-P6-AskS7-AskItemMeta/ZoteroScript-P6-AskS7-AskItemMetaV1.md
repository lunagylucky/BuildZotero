---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P6-AskS7-AskItemMetaV1
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
RelatedNote: []
RelatedProjects: []
CardRecord: null
---

## ZoteroScript-P 6-AskS8-AskItemMetaV1

### 🎯 核心作用
Items 文献条目分析系统是一个基于 Zotero 文献条目元数据的智能分析工具，通过正则表达式自动识别用户输入中的文献指代词（如 " 这个文献 "、" 本篇论文 "、" 这些文章 "），自动提取用户选中文献条目的核心信息，包括标题、作者、年份、期刊、摘要等元数据，并通过 AI 进行综合分析和问答。该系统专注于文献条目级别的快速分析，为文献管理、研究规划和学术写作提供高效的元数据驱动支持。

---


### 第一部分：完整代码

```javascript
#📚AskItemMeta[color=#0EA293][trigger=/(这|本)(个|些|篇)(文献|论文|文章|条目)/][trigger=/(这|本)(个|些|篇)(文献|论文|文章|条目)/]
These are Zotero item informations:
${
(async function() {
  let items = ZoteroPane.getSelectedItems()
  const docs = Meet.Zotero.items2documents(items)
  Meet.Global.views.insertAuxiliary(docs)
  return docs.map(
    (doc, index) => "[" + String(index + 1) + "]" + " " +  doc.pageContent
  ).join("\n\n")
})()
}$

---

Please answer me using the lanaguage as same as my question. Make sure to cite results using [number] notation after the reference. 
My question is: ${Meet.Global.input || "summary these papers"}$
```

---


### 第二部分：代码逻辑图

```mermaid
flowchart TD
    A["用户输入文本"] --> B["正则表达式检测"]
    B --> C{"匹配文献指代词？"}
    
    C -->|匹配| D["自动激活Items分析"]
    C -->|不匹配| E["等待其他操作"]
    
    D --> F["获取用户选中的文献条目"]
    F --> G["调用ZoteroPane.getSelectedItems"]
    G --> H["遍历所有选中条目"]
    
    H --> I["转换为文档格式<br/>Meet.Zotero.items2documents"]
    I --> J["提取条目元数据"]
    J --> K["构建标准化文档结构"]
    
    K --> L["插入辅助文档数据<br/>Meet.Global.views.insertAuxiliary"]
    L --> M["生成编号引用格式<br/>[1][2][3]…"]
    M --> N["组装所有条目信息"]
    
    N --> O["AI接收条目信息列表"]
    O --> P["检测用户问题语言"]
    P --> Q["设置相同语言回答"]
    
    Q --> R["分析用户问题意图"]
    R --> S{"问题类型判断"}
    
    S -->|概括总结| T["生成文献集合摘要"]
    S -->|特定问题| U["基于元数据回答问题"]
    S -->|比较分析| V["进行多文献对比"]
    
    T --> W["添加数字引用标记"]
    U --> W
    V --> W
    W --> X["输出最终答案"]
    
    E --> Y["用户使用其他功能"]
    
    style A fill:#e1f5fe
    style D fill:#fff3e0
    style X fill:#c8e6c9
    style B fill:#f3e5f5
    style C fill:#f3e5f5
    style I fill:#e8f5e8
    style J fill:#e8f5e8
```

---
