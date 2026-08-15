# **InterCog Engine — 系统架构说明（Architecture Documentation）**

> **跨学科推理引擎（Interdisciplinary Reasoning Engine）核心技术文档**  
> 本文档用于说明 InterCog 的内部结构、模块关系、推理流程、路由逻辑与系统级行为。  
> 面向开发者与贡献者，而非普通用户。

---

# ## 1. 系统总览（System Overview）

InterCog 是一个结构化的跨学科推理引擎，通过模块化 JSON 配置实现：

- 自动场景识别  
- 自动学科调度  
- 自动证据权重排序  
- 自动神经系统适配  
- 自动合规检查  
- 自动局限性声明  
- 自动结构化输出  

它不是一个 Prompt，而是一个 **完整的 reasoning framework**，由多个独立模块协同工作。

---

# ## 2. 系统模块（System Modules）

InterCog 的所有模块均以 JSON 文件形式存在，便于扩展、版本管理与自动调度。

| 模块 | 文件路径 | 功能说明 |
|------|----------|----------|
| 学科索引 | `/disciplines/disciplines_index.json` | 定义学科属性：触发词、优先级、冲突、关联 |
| 学科图谱 | `/disciplines/disciplines_graph.json` | 定义学科之间的结构关系 |
| 场景框架 | `/frameworks/scenarios.json` | 定义不同分析场景的维度、步骤、输出结构 |
| 证据权重 | `/frameworks/evidence_weights.json` | 定义证据等级与排序规则 |
| 合规规则 | `/frameworks/compliance_rules.json` | 定义版权、法律、隐私、安全边界 |
| 局限性声明 | `/frameworks/limitations.json` | 定义每个场景的局限性与适用边界 |
| 神经系统特征 | `/frameworks/neuro_profile.json` | 定义用户认知风格与神经多样性适配规则 |
| 输出模板 | `/frameworks/output_templates.json` | 定义每个场景的最终呈现结构 |
| 主控制器 | `/intercog/intercog_config.json` | 定义系统加载顺序、路由逻辑、推理流程 |

---

# ## 3. 推理流程（Reasoning Pipeline）

InterCog 的推理流程由 `intercog_config.json` 控制，包含以下步骤：

### **Step 1：合规检查（Compliance Check）**
加载 `compliance_rules.json`  
- 拦截违规、付费、敏感内容  
- 替换为 summary 或 general guidance  
- 添加合规声明  

### **Step 2：场景识别（Scenario Detection）**
加载 `scenarios.json`  
- 关键词匹配  
- 语义 embedding 匹配  
- 上下文推断  
- 匹配度不足时 fallback 到 `complex_event_analysis`

### **Step 3：学科调度（Discipline Orchestration）**
加载 `disciplines_index.json` + `disciplines_graph.json`  
- 根据场景维度选择核心学科  
- 图谱扩展（max_depth=2）  
- 移除冲突学科  
- 按优先级排序  

### **Step 4：证据收集与排序（Evidence Weighting）**
加载 `evidence_weights.json`  
- 对引用资料进行权重排序  
- 标注可靠性与偏差来源  

### **Step 5：神经系统适配（Neuro Adaptation）**
加载 `neuro_profile.json`  
- 根据用户认知风格选择表达方式  
- 根据神经多样性调整建议  
- 根据生活节奏调整方案复杂度  

### **Step 6：结构化输出渲染（Output Rendering）**
加载 `output_templates.json`  
- 按场景选择模板  
- 按用户神经特征选择表达风格  
- 插入证据权重  
- 插入局限性声明  
- 插入合规声明  

### **Step 7：最终输出（Final Output）**
生成一个：

- 多学科  
- 多维度  
- 个体化  
- 合规  
- 有证据权重  
- 有局限性声明  
- 有结构化段落  

的专业分析。

---

# ## 4. 场景路由（Scenario Routing）

InterCog 使用三层路由机制：

### **1. 关键词匹配**
来自 `scenarios.json` 的 `trigger_keywords`

### **2. 语义 embedding 匹配**
用于识别隐含场景，例如：

- “我未来该往哪里走？” → 职业规划  
- “我想做点自己的东西” → 创业  
- “我想研究一个方向” → 科研  

### **3. 上下文推断**
用于多轮对话场景。

### **Fallback**
匹配度 < 0.45 时自动使用：

> **complex_event_analysis（复杂事件分析）**

作为通用框架。

---

# ## 5. 学科调度（Discipline Orchestration）

学科调度由两个文件控制：

- `disciplines_index.json`  
- `disciplines_graph.json`  

调度逻辑：

1. 根据场景维度选择核心学科  
2. 根据图谱扩展关联学科（max_depth=2）  
3. 移除冲突学科  
4. 按 priority 排序  
5. 输出最终学科列表  

这是 InterCog 的“知识层大脑”。

---

# ## 6. 证据权重系统（Evidence Weighting）

由 `evidence_weights.json` 控制：

| 证据类型 | 权重 |
|---------|------|
| 权威学术文献 | 1.0 |
| 官方公开数据 | 0.9 |
| 一手史料 | 0.8 |
| 行业共识 | 0.6 |
| 专家观点 | 0.5 |
| 二手史料 | 0.45 |
| 普通网络信息 | 0.3 |
| 未证实传闻 | 0.1 |

AI 会自动：

- 按权重排序证据  
- 标注可靠性  
- 标注偏差来源  
- 限制低等级证据的使用  

---

# ## 7. 神经系统适配（Neuro Adaptation）

由 `neuro_profile.json` 控制：

### **感官主导**
- 视觉型 → 图表式结构  
- 听觉型 → 叙事式结构  
- 触觉型 → 体验式结构  

### **神经多样性**
- ADHD → 短任务块 + 优先级  
- ASD → 结构化 + 减少隐喻  
- Gifted → 抽象推理 + 可行性校验  
- CPTSD → 非侵入性表达 + 安全提示  

### **生活节奏**
- 高压 → 简化方案  
- 深度工作 → 复杂推理  
- 碎片化 → 要点式总结  

这是 InterCog 的“个体化大脑”。

---

# ## 8. 合规系统（Compliance Engine）

由 `compliance_rules.json` 控制：

- 版权  
- 付费内容  
- 法律  
- 医疗  
- 投资  
- 隐私  
- 安全  
- 极端风险  

AI 会自动：

- 拒绝违规内容  
- 替换为 summary  
- 添加合规声明  
- 避免触发性语言  

---

# ## 9. 局限性系统（Limitations Engine）

由 `limitations.json` 控制：

- 通用局限性  
- 场景局限性  
- 不确定性来源  
- 风险盲区  
- 适用边界  

AI 会自动在输出末尾添加。

---

# ## 10. 输出渲染系统（Output Rendering Engine）

由 `output_templates.json` 控制：

- 场景模板  
- 段落结构  
- 表达风格  
- 证据权重  
- 局限性声明  
- 合规声明  

这是 InterCog 的“呈现层大脑”。

---

# ## 11. 主控制器（Master Orchestrator）

由 `intercog_config.json` 控制：

- 模块加载顺序  
- 场景路由规则  
- 学科调度规则  
- 神经系统适配优先级  
- 合规检查开关  
- 输出渲染规则  
- 系统级日志开关  

这是整个系统的“总控中心”。

---

# ## 12. 系统特性总结（Key Features）

- **跨学科推理**  
- **个体化认知适配**  
- **证据权重排序**  
- **结构化输出模板**  
- **合规与安全边界**  
- **自动局限性声明**  
- **模块化 JSON 架构**  
- **可扩展、可维护、可协作**

