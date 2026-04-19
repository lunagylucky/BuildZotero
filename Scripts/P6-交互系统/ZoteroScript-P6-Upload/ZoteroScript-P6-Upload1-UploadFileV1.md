---
System:
- Project
Process:
- 4-WorkProjects
Class:
- 02TS
Project:
- BuildZotero
Title: ZoteroScript-P6-Upload1-UploadFileV1
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
- 代码
- 批量处理
- 文件上传
- 学术工具
- 自动化脚本
- JavaScript
- OpenAI
- Zotero
- Pattern/Method
RelatedNote:
RelatedProjects:
CardRecord: ''
---

## ZoteroScript-P 6-Upload1-UploadFileV1

### 第一部分：完整代码

```javascript
#📑UploadFile[color=#f9c23c][trigger=]
${
(async () => {
  const attItems = []
  for (let item of ZoteroPane.getSelectedItems()) {
    if (item.isRegularItem() && item.isTopLevelItem()) {
      const pdfItem = await item.getBestAttachment()
      pdfItem && attItems.push(pdfItem)
    } else if (item.isPDFAttachment()) {
      attItems.push(item)
    } else if (item.isNote()) {
      attItems.push(item)
    }
  }
  await window.Meet.OpenAI.uploadFile(attItems)
  return ""
})()
}$
```


### 第二部分：代码逻辑图

#### 主流程图（Mermaid 语法）

```mermaid
flowchart TD
    A["🚀 开始执行脚本"] --> B["📋 获取选中的Zotero条目"]
    B --> C["📂 初始化附件数组<br/>attItems = []"]
    
    C --> D["🔄 遍历每个选中条目"]
    D --> E{"🔍 条目类型判断"}
    
    E -->|📄 常规条目 + 顶级条目| F["📎 获取最佳PDF附件<br/>getBestAttachment()"]
    E -->|📑 PDF附件| G["➕ 直接添加到数组"]
    E -->|📝 笔记条目| H["➕ 直接添加到数组"]
    E -->|❌ 其他类型| I["⏭️ 跳过该条目"]
    
    F --> J{"📎 PDF附件存在?"}
    J -->|✅ 是| K["➕ 添加PDF到数组"]
    J -->|❌ 否| L["⏭️ 跳过该条目"]
    
    G --> M{"🔄 还有更多条目?"}
    H --> M
    I --> M
    K --> M
    L --> M
    
    M -->|✅ 是| D
    M -->|❌ 否| N["☁️ 调用OpenAI上传接口<br/>window.Meet.OpenAI.uploadFile()"]
    
    N --> O["✅ 返回空字符串<br/>处理完成"]

    classDef startEnd fill:#e1f5fe
    classDef process fill:#f3e5f5
    classDef decision fill:#fff3e0
    classDef success fill:#e8f5e8
    classDef skip fill:#f5f5f5
    classDef api fill:#e8eaf6
    
    class A,O startEnd
    class B,C,D,F,G,H,K process
    class E,J,M decision
    class N api
    class I,L skip
```


#### 条目类型处理流程图

```mermaid
flowchart LR
    A["📋 选中条目"] --> B{"🔍 条目类型"}
    
    B -->|Type 1| C["📄 常规条目<br/>+ 顶级条目"]
    B -->|Type 2| D["📑 PDF附件"]
    B -->|Type 3| E["📝 笔记条目"]
    B -->|Type 4| F["❌ 其他类型"]
    
    C --> G["🔍 查找PDF附件"]
    G --> H{"📎 找到PDF?"}
    H -->|✅| I["➕ 添加PDF"]
    H -->|❌| J["⏭️ 跳过"]
    
    D --> K["➕ 直接添加"]
    E --> L["➕ 直接添加"]
    F --> M["⏭️ 跳过"]
    
    I --> N["📤 准备上传"]
    K --> N
    L --> N
    J --> O["❌ 不上传"]
    M --> O

    classDef regular fill:#e3f2fd
    classDef pdf fill:#fff3e0
    classDef note fill:#e8f5e8
    classDef other fill:#fce4ec
    classDef upload fill:#f3e5f5
    classDef skip fill:#f5f5f5
    
    class C,G,H,I regular
    class D,K pdf
    class E,L note
    class F,M other
    class N upload
    class J,O skip
```


#### 数据流程图

```mermaid
graph LR
    subgraph "输入数据"
        A1["📋 选中条目列表"]
    end
    
    subgraph "处理过程"
        B1["🔍 条目分类"]
        B2["📎 附件提取"]
        B3["📂 数组收集"]
    end
    
    subgraph "输出结果"
        C1["📤 上传文件列表"]
        C2["☁️ OpenAI接口"]
    end
    
    A1 --> B1
    B1 --> B2
    B2 --> B3
    B3 --> C1
    C1 --> C2

    classDef input fill:#e1f5fe
    classDef process fill:#f3e5f5
    classDef output fill:#e8f5e8
    
    class A1 input
    class B1,B2,B3 process
    class C1,C2 output
```
