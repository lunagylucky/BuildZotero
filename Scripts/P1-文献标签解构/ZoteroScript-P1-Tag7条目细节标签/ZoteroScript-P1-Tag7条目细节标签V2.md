---
System:
Process:
Class:
Project:
  - BuildZotero
Title: ZoteroScript-P1-Tag7条目细节标签V2
DateCreated: 2026-01-17 17:37
DateModified: 2026-01-28 13:03
Status:
Version:
Type:
CardStatus:
CardType:
tags: []
RelatedNote:
CardRecord:
---


## ZoteroScript-P1-Tag7 条目细节标签 V2

```javascript

#🏷️ItemTag1[color=#66dc79][trigger=]
${
(async () => {
  const items = ZoteroPane.getSelectedItems();
  if (!items || items.length === 0) return "未选择任何条目";
  
  let successCount = 0;
  let skippedCount = 0;
  let noContentCount = 0;
  let failCount = 0;
  let errorCount = 0;
  
  // 检查是否存在研究要素标签
  const hasResearchTags = (item) => {
    const existingTags = item.getTags();
    const tagPrefixes = ['V3-con/', 'V4-fut/', 'V1-def/', 'V2-mea/', 'V5-sor/'];
    return tagPrefixes.every(prefix => 
      existingTags.some(tag => tag.tag && tag.tag.startsWith(prefix))
    );
  };
  
  // 删除现有研究要素标签(V开头)
  const removeExistingResearchTags = async (item) => {
    const existingTags = item.getTags();
    const researchTags = existingTags.filter(tag => 
      tag.tag && /^V[1-5]-(def|mea|con|fut|sor)\//.test(tag.tag)
    );
    for (let researchTag of researchTags) {
      item.removeTag(researchTag.tag);
    }
  };
  
  // 保存函数
  const saveItemWithTags = async (item, tags, countType) => {
    try {
      await Zotero.DB.executeTransaction(async function () {
        for (let tag of tags) {
          item.addTag(tag);
        }
        await item.save();
      });
      
      if (countType === 'success') successCount++;
      else if (countType === 'noContent') noContentCount++;
      else if (countType === 'fail') failCount++;
      
      return true;
    } catch {
      errorCount++;
      return false;
    }
  };
  
  for (let item of items) {
    try {
      // 检查是否已有完整的五类标签
      if (hasResearchTags(item)) {
        skippedCount++;
        continue;
      }
      
      // 删除原有研究要素标签
      await removeExistingResearchTags(item);
      
      let content = "";
      
      // 内容获取:优先PDF全文,其次摘要
      try {
        const pdfItem = await item.getBestAttachment();
        if (pdfItem && pdfItem.isPDFAttachment()) {
          try {
            const fullTextData = await Zotero.PDFWorker.getFullText(pdfItem.id, 1);
            if (fullTextData && fullTextData.text && fullTextData.text.trim().length > 100) {
              content = fullTextData.text.substring(0, 45000).trim();
            }
          } catch {}
        }
        
        if (!content) {
          const abstract = item.getField("abstractNote");
          if (abstract && abstract.trim().length > 50) {
            content = abstract.trim();
          }
        }
      } catch {}
      
      if (!content) {
        await saveItemWithTags(item, ['V3-con/无内容'], 'noContent');
        continue;
      }
      
      // AI分析识别研究要素
      try {
        const prompt = `你是一位学术文献分析专家。请仔细分析以下学术文献内容,识别研究的关键要素。

文献内容:
${content}

任务说明:
请识别文献中的以下5类信息:

1. **研究贡献与创新点**(1-4个):
   - 识别文献的主要学术贡献、理论创新、实证发现
   - 通常在"研究贡献"、"理论贡献"、"实证贡献"、"创新点"、"主要发现"等部分
   - 也可能在摘要结论部分体现
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 按重要性排序
   - 正确示例: "揭示数字化对创新的非线性影响"
   - 错误示例: "本研究揭示了企业数字化转型对创新绩效的影响机制"(过长)

2. **研究展望**(1-3个):
   - 未来研究方向、研究建议、研究局限
   - 通常在"研究展望"、"未来研究"、"研究局限"、"讨论"等部分
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 按重要性排序
   - 正确示例: "探索跨层次动态交互机制"
   - 错误示例: "未来研究可以探索组织层面和个体层面的跨层次动态交互机制"(过长)

3. **变量定义**(数量不限):
   - 识别文献中主要变量的学术定义或概念界定
   - 通常在"变量测量"、"概念界定"、"理论框架"等部分
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 格式: "变量名:定义"
   - 正确示例: "数字化转型:企业利用数字技术重塑运营"
   - 错误示例: "数字化转型是指企业系统性地利用数字技术对业务模式进行根本性重塑"(过长)

4. **测量方法**(数量不限):
   - 识别文献中变量的具体测量方式或操作化方法
   - 通常在"变量测量"、"数据与方法"等部分
   - **要求精简,只保留核心内容,去掉主语和无关表达**
   - **字符限制: 15-25个字符**
   - 格式: "变量名:测量方式"
   - 正确示例: "创新绩效:专利申请数对数值"
   - 错误示例: "创新绩效采用企业当年专利申请数量的自然对数进行衡量"(过长)

5. **样本与数据来源**(1-3个):
   - 识别文献使用的具体数据库名称、数据平台、文本来源渠道
   - 通常在"数据与方法"、"样本选择"、"数据来源"等部分
   - **必须是可直接访问或查找的数据库/平台/文本来源名称**
   - **重要：区分"数据来源"与"研究样本"**
     * 数据来源：数据库、网站、平台等数据获取渠道（这才是要识别的）
     * 研究样本：研究对象的描述，如"沪深A股上市公司"、"中国制造业企业"（不是数据来源）
   - **字符限制: 10-20个字符**
   - 按数据重要性排序
   - **数值型数据源示例**: "CSMAR数据库"、"Wind数据库"、"国泰安数据库"、"USPTO专利库"、"Compustat"、"中国工业企业数据库"
   - **文本型数据源示例**: 
     * 具体网站: "巨潮资讯网"、"新浪财经网"、"东方财富网"、"证监会官网"、"企业官方网站"、"LinkedIn"、"Twitter"、"Facebook"
     * 文本+网站结合: "巨潮资讯网年报"、"新浪财经新闻"、"公司官网公告"、"证监会公告"、"Twitter推文"
     * 文本类型(次优): "企业年度报告"、"财经新闻报道"、"管理层讨论与分析"、"分析师报告"
   - 错误示例: 
     * "上市公司数据"(过于笼统)
     * "面板数据"(数据类型，非来源)
     * "公开数据库"(无法定位)
     * "文本数据"(过于模糊)
     * "网络数据"(无法定位)
     * "沪深A股上市公司"(这是样本，不是数据来源)
     * "中国制造业企业"(这是样本，不是数据来源)
     * "2010-2020年数据"(这是时间范围，不是数据来源)
   - **对于文本来源，优先标注具体平台/网站，次选标注文本类型**
   - 如果文献明确提到多个数据源，可列出多个

识别要求:
- **所有内容必须精简到规定字符以内**
- **去掉"本研究"、"我们"、"文章"等主语**
- **去掉"发现"、"表明"、"认为"等冗余动词**
- **只保留核心学术内容**
- **数据来源必须具体到可查找的程度**
- **数据来源要识别的是数据库/平台/网站，不是研究对象或样本描述**
- 使用简体中文标准术语
- 忠实于原文,不要编造内容
- 如果某类信息在文献中不存在或未明确提及,则标记为"无"
- 按重要性排序

请严格按照以下JSON格式返回(仅返回JSON,无其他文字):
{
  "CON": ["贡献1", "贡献2", "贡献3", "贡献4"],
  "LIM": ["展望1", "展望2", "展望3"],
  "DEF": ["变量名1:定义1", "变量名2:定义2"],
  "MEA": ["变量名1:测量1", "变量名2:测量2"],
  "SOR": ["数据源1", "数据源2", "数据源3"]
}

示例输出1(数值型数据-正确格式):
{
  "CON": ["构建数字化转型三维框架", "发现倒U型影响关系", "识别组织学习中介机制"],
  "LIM": ["扩展跨行业样本研究", "纳入动态时间维度"],
  "DEF": ["数字化转型:利用数字技术重塑运营", "组织学习:知识获取与应用能力"],
  "MEA": ["数字化转型:信息化投入占比", "创新绩效:专利申请数对数值"],
  "SOR": ["CSMAR数据库", "Wind数据库", "国泰安专利数据库"]
}

示例输出2(文本型数据-具体网站):
{
  "CON": ["揭示CEO语调影响决策", "发现情绪传染机制"],
  "LIM": ["扩展多语言文本分析"],
  "DEF": ["管理语调:高管表达的情绪倾向"],
  "MEA": ["管理语调:文本情感得分", "决策激进度:投资强度"],
  "SOR": ["巨潮资讯网年报", "新浪财经网", "公司官方网站"]
}

示例输出3(混合型数据):
{
  "CON": ["验证环境规制波特假说", "发现技术创新中介效应"],
  "LIM": ["无"],
  "DEF": ["环境规制:政府环保政策强度"],
  "MEA": ["环境规制:排污费用对数值", "绿色创新:绿色专利数量"],
  "SOR": ["中国工业企业数据库", "CNRDS数据库"]
}

示例输出4(社交媒体网站数据):
{
  "CON": ["分析社交媒体对品牌影响"],
  "LIM": ["扩展多平台对比研究"],
  "DEF": ["品牌关注度:用户讨论热度"],
  "MEA": ["品牌关注度:提及次数对数"],
  "SOR": ["Twitter", "LinkedIn", "Facebook"]
}

示例输出5(仅有文本类型-次优格式):
{
  "CON": ["分析媒体关注对股价影响"],
  "LIM": ["考察社交媒体作用"],
  "DEF": ["媒体关注:新闻报道频次"],
  "MEA": ["媒体关注:月度报道数量对数"],
  "SOR": ["财经新闻报道", "企业年度报告"]
}`;
        const response = await Meet.Global.views.ask(prompt);
        
        // 解析AI响应
        try {
          const jsonMatch = response.match(/\{[\s\S]*?\}/);
          if (!jsonMatch) {
            await saveItemWithTags(item, ['V3-con/解析失败'], 'fail');
            continue;
          }
          
          const elements = JSON.parse(jsonMatch[0]);
          
          const tagsToAdd = [];
          
          // 处理研究贡献(contribution) -> V3-con
          if (elements.CON && Array.isArray(elements.CON)) {
            const contributions = elements.CON.filter(c => c && c.trim() !== '' && c.trim() !== '无');
            if (contributions.length > 0) {
              contributions.slice(0, 4).forEach((con, index) => {
                tagsToAdd.push(`V3-con/${con.trim()}`);
              });
            } else {
              tagsToAdd.push('V3-con/无');
            }
          } else {
            tagsToAdd.push('V3-con/无');
          }
          
          // 处理研究展望(future) -> V4-fut
          if (elements.LIM && Array.isArray(elements.LIM)) {
            const limitations = elements.LIM.filter(l => l && l.trim() !== '' && l.trim() !== '无');
            if (limitations.length > 0) {
              limitations.slice(0, 3).forEach((lim, index) => {
                tagsToAdd.push(`V4-fut/${lim.trim()}`);
              });
            } else {
              tagsToAdd.push('V4-fut/无');
            }
          } else {
            tagsToAdd.push('V4-fut/无');
          }
          
          // 处理变量定义(definition) -> V1-def
          if (elements.DEF && Array.isArray(elements.DEF)) {
            const definitions = elements.DEF.filter(d => d && d.trim() !== '' && d.trim() !== '无');
            if (definitions.length > 0) {
              definitions.forEach((def, index) => {
                tagsToAdd.push(`V1-def/${def.trim()}`);
              });
            } else {
              tagsToAdd.push('V1-def/无');
            }
          } else {
            tagsToAdd.push('V1-def/无');
          }
          
          // 处理测量方法(measure) -> V2-mea
          if (elements.MEA && Array.isArray(elements.MEA)) {
            const measurements = elements.MEA.filter(m => m && m.trim() !== '' && m.trim() !== '无');
            if (measurements.length > 0) {
              measurements.forEach((mea, index) => {
                tagsToAdd.push(`V2-mea/${mea.trim()}`);
              });
            } else {
              tagsToAdd.push('V2-mea/无');
            }
          } else {
            tagsToAdd.push('V2-mea/无');
          }
          
          // 处理样本与数据来源(source) -> V5-sor
          if (elements.SOR && Array.isArray(elements.SOR)) {
            const sources = elements.SOR.filter(s => s && s.trim() !== '' && s.trim() !== '无');
            if (sources.length > 0) {
              sources.slice(0, 3).forEach((sor, index) => {
                tagsToAdd.push(`V5-sor/${sor.trim()}`);
              });
            } else {
              tagsToAdd.push('V5-sor/无');
            }
          } else {
            tagsToAdd.push('V5-sor/无');
          }
          
          // 至少要有五类标签
          if (tagsToAdd.length < 5) {
            await saveItemWithTags(item, ['V3-con/标签不完整'], 'fail');
            continue;
          }
          
          await saveItemWithTags(item, tagsToAdd, 'success');
          
        } catch {
          await saveItemWithTags(item, ['V3-con/JSON解析失败'], 'fail');
        }
        
      } catch {
        await saveItemWithTags(item, ['V3-con/AI调用失败'], 'fail');
      }
      
    } catch {
      errorCount++;
    }
  }
  
  return `研究要素标签更新完成\n成功: ${successCount} | 跳过(已有标签): ${skippedCount} | 无内容: ${noContentCount} | 失败: ${failCount} | 错误: ${errorCount}`;
})()
}$
不需要提供任何额外说明，直接按照指令输出结果即可。
```
