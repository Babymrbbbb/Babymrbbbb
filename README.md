# AI 系统工程方法论笔记

> 本账号公开内容只描述方法论、技术方案与工程判断。所有示例数据均为合成演示数据，与任何真实企业经营数据无关。

## 一、检索与知识库

- **双路召回**：中文二元组 BM25 负责精确词面命中，稠密向量负责语义改写；两路结果用 RRF 融合，而不是二选一。
- **聚合层编译**：原始资料编译为实体页、概念页与综述页，多源合并后标注来源列表，问答可追溯。
- **事实护栏**：关键事实只允许一个真相源，且在编译层做确定性覆盖。提示词只提升概率，不保证正确；权威结论必须由代码注入。
- **索引自愈**：聚合索引路径会漂移，读取函数必须校验存在性并兜底；空输入比报错更危险，需加「有效源数 ≥ 1」前置校验。

## 二、评测与护栏

- **离线评测**：32 条带标注查询对照 BM25 与 ONNX MiniLM，报告 Recall@5 与结果集 Jaccard。中文垂域小库上 BM25 `0.953` 明显领先英文预训练向量 `0.594`，说明模型与语种/领域的匹配度比「是否用向量」更关键。
- **回归门禁**：凡改检索链路必跑全量评测；基础设施失败（限流、5xx）需与质量失败分开计数，否则门禁会持续误报。
- **测试绿 ≠ 线上对**：验证必须做检索探针或直接查线上索引，不能只看把语料重灌进临时库的测试结果。
- **多入口一致性**：一个系统有多套编译入口时，护栏必须放进共享模块并让每个入口复用；只加一处等于没加。

## 三、模型路由与降级

- 按任务所需能力划分能力池（推理深度、上下文长度、视觉、吞吐），池与池之间额度隔离。
- 降级链按「能力接近程度」排序，每级降级记日志；频繁降到末级说明主模型选型需要调整。
- 无凭证或依赖缺失时自动降级到零依赖实现，保证端到端可跑。

## 四、双模型验收

- 执行方与验收方必须是两个独立实例，验收方要真实跑链路并留下调用痕迹。
- 任务单写清目标、素材、输入输出与验收标准；蓝图终审通过后才开工。
- 「无异议即通过」占比过高是危险信号，需要反勾结指标与成件验证。

## 五、AI 边界与数据治理

- 数据边界：敏感字段不进上下文；动作边界：模型只建议不执行；表达边界：生成内容必须标来源；失败边界：检索不到就拒答，不用常识补全。
- 边界必须做成系统约束，写在提示词里的「请不要」不算边界，代码里拦住才算。
- 公开仓只保留方法论与可复跑实验，业务实体与经营数据一律不进入公开文本。

## 公开项目索引

> 与站点项目地图逐字一致；各仓 description 同步使用同一句话定位。

| 分组 | 仓库 | 主题 |
|---|---|---|
| 检索与知识库 | [rag-vector-vs-keyword](https://github.com/Babymrbbbb/rag-vector-vs-keyword) | BM25 与稠密向量的同口径对照评测 |
| 检索与知识库 | [langchain-kb-qa](https://github.com/Babymrbbbb/langchain-kb-qa) | LangChain + chromadb 的可部署检索增强问答服务 |
| 检索与知识库 | [hunkon_rag](https://github.com/Babymrbbbb/hunkon_rag) | 混合检索、事实护栏与 CI 评测门禁的 RAG 服务 |
| 检索与知识库 | [llm-wiki](https://github.com/Babymrbbbb/llm-wiki) | 原始资料编译为可导航 wiki 知识层的方法论 |
| 检索与知识库 | [kb-cli](https://github.com/Babymrbbbb/kb-cli) | 零依赖本地知识库命令行与多层检索 |
| 检索与知识库 | [swan_kb](https://github.com/Babymrbbbb/swan_kb) | 后端底座 + RAG 服务的早期归档实现 |
| 模型与 Agent | [sensenova-gateway](https://github.com/Babymrbbbb/sensenova-gateway) | OpenAI 兼容的多账号模型网关与降级路由 |
| 模型与 Agent | [ai-native-team](https://github.com/Babymrbbbb/ai-native-team) | Agent 职责拆分与双模型隔离校验 |
| 模型与 Agent | [langgraph-agent-demo](https://github.com/Babymrbbbb/langgraph-agent-demo) | LangGraph 有状态多步任务编排示例 |
| 工程与评测 | [lora-finetune-demo](https://github.com/Babymrbbbb/lora-finetune-demo) | 手写 LoRA 前向/反向与参数量对照 |
| 工程与评测 | [prompt-craft](https://github.com/Babymrbbbb/prompt-craft) | 提示词结构与效果评测方法 |
| 工程与评测 | [store_tool](https://github.com/Babymrbbbb/store_tool) | FastAPI + SQLModel + Alembic 后端服务参考实现 |
| 工程与评测 | [swan-electron-suite](https://github.com/Babymrbbbb/swan-electron-suite) | Electron 桌面端工具的设计说明 |

- 站点：https://Babymrbbbb.github.io
- 项目地图：https://Babymrbbbb.github.io/#all-projects
