![](https://fastly.jsdelivr.net/gh/bucketio/img11@main/2024/10/21/1729466068183-23134fce-3131-4262-b18c-f378d71af4f6.gif)

![](https://fastly.jsdelivr.net/gh/bucketio/img9@main/2024/10/20/1729465031968-b3c8959e-1d37-4b8a-91b1-b0b0dfe25143.png)

## FinRobot：基于大语言模型的股票研究与估值框架

在当今日益复杂的金融市场中，想要实现高效的卖方证券研究，往往需要自动化工具的支持。然而，许多现有的AI方案只关注技术指标，且缺乏灵活的主观分析能力，难以满足实时适应新数据或准确评估风险的要求，也因此在投资实务中价值有限。


![](https://fastly.jsdelivr.net/gh/bucketio/img9@main/2025/01/29/1738120002722-a681f3a4-f2b3-4c73-91d7-24fe80ded644.png)


**本文介绍的FinRobot**，是首个专为**股票研究**设计的AI智能体框架。它采用**多智能体Chain of Thought (CoT)系统**，将定量与定性分析相结合，模拟人类分析师的全面推理过程。整体结构包括以下三大功能代理：

1. **Data-CoT Agent**：整合多源数据，实现对财务报表、公司公告、第三方数据库等信息的全面抓取和中间摘要。
2. **Concept-CoT Agent**：对关键财务指标与行业环境进行深入剖析，模拟人类分析师的研究思路，形成可执行的分析结论。
3. **Thesis-CoT Agent**：最终将分析结果整合为投资建议报告，提供数据信息、估值指标及风险评估等综合判断。

与现有自动化研究平台（如CapitalCube、Wright Reports）不同，FinRobot兼具**真实交易价值**与**机构级水准**，对市场动向具备持续更新能力，并在风险评估上更贴近现实。该框架现已开源，地址为：https://github.com/AI4Finance-Foundation/FinRobot

![](https://fastly.jsdelivr.net/gh/bucketio/img15@main/2025/01/22/1737589546535-0f621a57-2c2f-492d-ae3d-d9ef8b0a98d1.png)

> **关键词**：AI-agent，Large Language Models，Equity Research，Financial Analysis，Chain of Thought

---

## 1. 引言

**财务分析**是金融服务行业的核心内容，影响着投资者的各类决策（Abarbanell and Bushee, 1997; Greenwald et al., 2004; Penman, 2010; Berman and Knight, 2013; Subramanyam, 2014）。在此之中，**股票研究（equity research）** 尤其重要，特别是在大型投行和券商的卖方研究部门中。传统研究报告通常需要分析师具备深度的量化建模与行业认知，但此过程往往费时费力。

随着**人工智能（AI）**和**大语言模型（LLM）** 的兴起（Medhat et al., 2014; Brown et al., 2020; Wu et al., 2023; Yang et al., 2023; Kim et al., 2024），金融领域开始探索自动化证券研究。然而，现有工具多侧重技术面或简单模型，忽略了专家判断与实质性定性分析的结合。本研究推出的**FinRobot**，通过多智能体和Chain of Thought机制，既结合了量化分析，又保留了主观判断的灵活性，可满足机构级的研究深度。

**主要贡献**：

- **首个应用多Agent Chain of Thought(CoT)的AI证券研究框架**：FinRobot将研究过程拆解为数据处理（Data-CoT）、概念分析（Concept-CoT）及研究报告（Thesis-CoT）三个层次，模拟人类分析师的思维链条。
- **结合主观判断、实时数据与新评估指标**：FinRobot具备实时数据接入与多维度的报告质量评估（Accuracy、Logicality、Storytelling）。
- **开源平台推动金融AI民主化**：FinRobot开放源代码，鼓励金融领域与AI社区的交流与协作。

---

## 2. 相关研究

### 2.1 LLM与金融应用
大语言模型（LLMs）因其对自然语言的强大理解和表达能力，在**金融分析**中逐渐发挥重要作用，包括**情感分析**（Medhat et al., 2014; Huang et al., 2023; Zhang et al., 2023）和**市场预测**（Henrique et al., 2019; Nabipour et al., 2020; Kumar et al., 2022; Jiang, 2021）等任务。然而，LLM往往缺乏实时数据与行业专门知识，难以应对高要求的实时证券研究。本项目则针对这一痛点展开研究。

### 2.2 AI智能体及Chain of Thought在金融分析中的应用
多智能体协作框架为金融分析带来了更高效的决策过程，如FinAgent (Zhang et al., 2024)与FinMem (Yu et al., 2023)等可利用实时行情数据辅助交易策略。与此同时，**Chain-of-Thought**（CoT）提示可以模拟人类的思考步骤，显著提升分析质量（Wei et al., 2022; Kim et al., 2024）。FinRobot正是在此基础上，针对卖方研究的需求设计了专门的CoT框架，实现了更全面的财务分析深度和灵活适应性。



## 3. 方法论

### 3.1 总体框架
下图1展示了FinRobot的多层CoT结构，使整个财务研究过程分为依次衔接的三个Agent层级，既增强了分析的专业度，也便于最终报告的逻辑和可读性。


![](https://fastly.jsdelivr.net/gh/bucketio/img17@main/2025/01/29/1738120339772-d962baa5-31f2-475a-8079-03411ce161c0.png)


1. **Data Processing Layer（Data-CoT Agent）**  
   - 负责从SEC文件、财报电话会议、公司公告等多渠道抓取信息，并进行清洗、格式化与关键财务指标提炼。  
   - 该Agent可同时获取定量与定性数据，为后续的概念分析打下基础。

2. **Financial Concept Layer（Concept-CoT Agent）**  
   - 将处理后的数据转化为可操作的财务概念和预测，包括**营收增速**、**EBITDA趋势**、**市场定位**等。  
   - 通过类似人类分析师的思考方式，来评估竞争格局、情绪因素及潜在风险。

3. **Equity Research Template Layer（Thesis-CoT Agent）**  
   - 整合上述分析结果，输出完整的**投资研究报告**，包括投资论点、风险评估、估值模型及结论性投资评级（如买入/持有/卖出）。  
   - 该Agent采用卖方报告的专业模板，确保最终报告符合行业标准。

### 3.2 数据处理层
**Data-CoT Agent**聚合多渠道数据并进行预处理，保证信息的**准确性**和**全面性**。主要数据来源包括：

- **数据库**：Oceanbase、PostgreSQL等，用于存储结构化财务数据。
- **非结构化文档**：如PDF、DOCX、图片等，提取文本和关键信息。
- **第三方接口**：图表可视化配置、API实时数据获取等。
- **互联网搜索**：整合多维度市场资讯与行业动态。
- **分布式文件存储**：DFS、Minio等保证海量数据的高可用与鲁棒性。

此外，FinRobot针对**SEC文件**（10-K、10-Q等）和**财报电话会议**做精细的要点提取，包括**营收、运营成本、SG&A**等关键数值，以进一步计算**营收增速、贡献利润、EBITDA及其利润率**等（见下表）。这些指标是开展后续分析和估值的重要基石。

**常见财务公式举例**：

| **Formula** | **Description** |
| --- | --- |
| **Revenue Growth** = $(\text{Revenue}_{current} - \text{Revenue}_{previous}) / \text{Revenue}_{previous}$ | 计算营收相对上期增长幅度 |
| **Contribution Profit** = Revenue $-$ Operating Expense | 用于衡量运营成本扣除后的盈利能力 |
| **EBITDA Margin** = $ \text{EBITDA} / \text{Revenue} $ | EBITDA占营收比例，衡量运营效率 |
| **CAGR** = $((\text{EV}/\text{BV})^{1/n}-1)\times 100$ | 复合年化增长率 |
| **Enterprise Multiple** = $ \text{EV} / \text{EBITDA} $ | 评估公司价值的常用估值倍数 |

### 3.3 Fin-Concept层
在**Concept-CoT Agent**中，FinRobot对上一步生成的财务指标进行进一步剖析，回答与投资逻辑密切相关的深层问题：

- **营收预测**：考虑历史增长、在手订单（backlog）、通胀及市场定价等要素，为企业设定乐观与保守两种增长情境。
- **EBITDA及利润率**：通过扣除一次性项目，获得更真实的盈利水平，并与行业基准对比利润率趋势。
- **ROIC、WACC**等关键指标：用于评估公司资本使用效率与资本成本，为DCF等估值模型提供支撑。
- **财务问答**：如公司相较同行的优势、外部环境对其盈利影响、以及后续年度的利润扩张潜力等。

### 3.4 研究报告层
**Thesis-CoT Agent**将上述分析结果以**卖方研究报告**的格式呈现，包括以下关键内容：

- **投资论点**：结合财务预测、行业态势，明确地给出买/卖/持有等投资评级。  
- **风险分析**：罗列影响企业估值与增长的主要风险因素，帮助投资者平衡机会与挑战。
- **估值与财务预测**：包含未来数年的营收、EBITDA及利润率预期，使用DCF或P/E等模型给出目标价。
- **竞争对手分析**：通过与同行对比营收增速、毛利率、EBITDA与SG&A开支比等，判断目标公司在市场中的相对地位。


![](https://fastly.jsdelivr.net/gh/bucketio/img0@main/2025/01/29/1738121121008-bf853eb2-10f3-4495-8f66-ff4e1c8c618f.png)


## 4. 实验

### 4.1 任务描述
FinRobot可适用于多行业的股票报告，本次以**Waste Management, Inc.**（北美废物管理与环保服务巨头）为示例。它对该公司财务表现、估值方法、风险点以及行业比较进行全面整理，并提供清晰的表格与图表辅助说明。更多详情见附录中完整的研究报告。

### 4.2 实现细节
1. **数据处理（Data-CoT层）**：收集了SEC文件、公司公告及历史财务数据等。  
2. **概念分析（Concept-CoT层）**：采用分析师思路，回答关于盈利驱动、竞争格局和潜在风险的核心问题。  
3. **报告整合（Thesis-CoT层）**：将上述要点写成正式研究报告风格，包含图表与摘要，便于投资者快速理解。

### 4.3 评估

#### 4.3.1 专家评审
我们邀请了多位投行分析师对FinRobot生成的报告从**准确度(Accuracy)**、**逻辑性(Logicality)** 以及**叙事能力(Storytelling)** 三个维度进行0-10打分（表3提供详细标准）。在表2的结果可见，大多数评审对准确性均给出了**9或10**的高分，说明数字与分析十分可靠。逻辑性也获得了较高评价，但在少数评审眼中有些细微改进空间。叙事性整体表现良好，部分评审希望在可读性或故事性上再加强。

#### 4.3.2 大模型评测
除专家外，我们也使用**GPT-4**来对报告进行相同维度的评分（见图3），结果与专家评审基本一致（图4展示其具体点评），也再度证明报告在**数据精准、结构清晰与内容可读**等方面获得多方认可。

#### 4.3.3 稳定性测试
为验证FinRobot的稳定性，我们针对相同公司多次生成报告，并用GPT-4对每份报告做一致性评分，并与零样本、少样本及纯CoT提示下的结果对比，发现FinRobot在**Accuracy、Logicality、Storytelling**三方面表现始终优于其它提示模式（见图5），且波动较小，显示出报告输出的稳定可靠。


## 5. 结论

本文提出的**FinRobot**，利用多智能体Chain of Thought体系，将定量和定性分析完美融合，极大提升了AI在卖方证券研究中的实用价值。它拥有**实时数据管线**和**专业风险评估**能力，可输出准确详实且便于决策的研报。

展望未来，我们将进一步扩展FinRobot在不同行业与资产类型上的应用，例如覆盖更多标的、强化强化学习与情感分析等功能，以期为金融业提供更具多样化与创新性的研究工具。


## 关于LLMQuant

LLMQuant是由一群来自世界顶尖高校和量化金融从业人员组成的前沿社区，致力于探索人工智能（AI）与量化（Quant）领域的无限可能。我们的团队成员来自剑桥大学、牛津大学、哈佛大学、苏黎世联邦理工学院、北京大学、中科大等世界知名高校，外部顾问来自Microsoft、HSBC、Citadel、Man Group、Citi、Jump Trading、国内顶尖私募等一流企业。欢迎加入**知识星球**获取**内部资料**。


![](https://fastly.jsdelivr.net/gh/bucketio/img13@main/2025/01/29/1738120164157-32d67f79-b006-4f4e-bb7b-13db27759e2b.JPG)

