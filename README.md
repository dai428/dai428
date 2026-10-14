# 代益泉 · AI 应用开发

光学工程硕士在读（西安邮电大学），主动转向 **大模型应用开发**。

我的做法是「**先建评测、再谈优化**」——每个优化结论都配可复现的数字和代价说明，
而不是只说「效果更好了」。两个项目都开源，代码和评测脚本可直接查看。

---

## 🔬 光学科研文献智能体（Agentic RAG）

> 面向光学 / 液晶偏振测量（LCVR）垂直领域的科研文献问答系统。
> **不是「能跑通」的玩具——每一层优化都有可复现的评测数字兜底。**

[![repo](https://img.shields.io/badge/GitHub-optics--rag--agent-blue)](https://github.com/dai428/optics-rag-agent)
`Python` `LangGraph` `LangChain` `ChromaDB` `BGE` `FastAPI` `React + TypeScript`

**核心结果**（20 题自建评测集，14 篇真实光学语料）

| 指标 | 旧版（纯向量） | 新版（混合+重排） | 提升 |
|---|---|---|---|
| **Recall@5** | 80.0% | **100.0%** | **+20.0 pt** |
| Recall@1 | 50.0% | 65.0% | +15.0 pt |
| MRR@10 | 0.616 | **0.783** | **+27.1%** |
| NDCG@10 | 0.647 | 0.805 | +24.4% |
| 关键词命中@5 | 85.0% | 95.0% | +10.0 pt |

**做了什么：**
- **检索链路**：向量 + BM25 双路召回 → RRF 融合 → `bge-reranker-base` 精排 → 父块聚合 → 来源加权
- **领域适配**：Markdown 参数表格展开（横竖两种平铺）让数字可检索；论文体/实验记录来源加权，解决叙述性文字挤占一手数据
- **父子分层切块**：父块 2000–4000 字符 / 子块 500 字符，命中子块后回溯父块
- **六项 Agentic 能力**：查询改写、澄清反问、问题拆分、`Send` 动态并行检索、长对话滚动摘要、工具调用预算上限
- **可复现**：`eval_rag.py` 支持「纯向量 / 混合不重排 / 混合+重排」三模式对比，一条命令跑出来

> 生成层用 RAGAS 抽样评测：faithfulness **0.889**、answer_relevancy **0.818**
> （评测集无标准答案，且裁判模型为自评，绝对分值仅供参考）

---

## 🗺 行迹 · 智能行程规划系统

> 输入「去哪儿、玩几天」，得到一份**带攻略出处、可拖拽调整、可导出手机日历、可一键分享**的行程。

[![repo](https://img.shields.io/badge/GitHub-ai--trip--planner-blue)](https://github.com/dai428/ai-trip-planner)
`Python` `FastAPI` `Pydantic v2` `SQLite` `高德 Web API` `Vue 3 + TypeScript`

**核心设计：防幻觉靠核对，不靠提示词**

- **候选池约束**：自研 BM25 + 字符 n-gram 双路召回从本地攻略库选候选，叠加高德 POI 宽召回（一城约 130 条，SQLite 落盘 + 30 天 TTL）。**大模型只能在候选池内选点，地名必须原样照抄**
- **回池核对**：模型返回后逐条比对候选池，**池外一律丢弃**——这是防幻觉的唯一有效手段
- **合规率门槛**：低于 60% 整份作废、退回规则生成
- **降级显式化**：响应带 `generator` 字段（`llm` / `rule`）与降级原因，**不把规则生成伪装成大模型出品**

**其他能力：** 多城市串联（最多 6 城）、路线走廊算法判断中途停靠、ICS 日历导出（RFC 5545 合规）、只读分享链接（token 可撤销）、打包清单（纯规则生成）

**质量：** **92 个测试函数（展开 106 个用例）** + CDP 驱动真实浏览器的端到端 UI 冒烟测试

---

## 🛠 技术栈

| 方向 | 使用 |
|---|---|
| **语言** | Python（主）、TypeScript / JavaScript、HTML5 / CSS3 |
| **大模型应用** | RAG 全链路、Agent 编排（LangGraph 手写图）、Function Calling、Structured Output、Prompt Engineering |
| **检索与评测** | BM25、RRF 融合、CrossEncoder 重排；自建评测集与 Recall@K / MRR / NDCG / RAGAS |
| **框架与存储** | LangChain、LangGraph、ChromaDB、FastAPI（SSE 流式）、SQLite |
| **前端** | React + TypeScript、Vue 3 + Vite；原生 JS 手写 SSE 流式解析 |
| **工程** | Docker、Git、Linux、Langfuse（链路可观测） |

---

## 📫 联系

- 📧 450644094@qq.com
- 📱 19924394281
- 📍 西安（随时到岗 · 每周 4 天 · 可实习 3 个月以上）

---

<sub>另在研：硕士课题为 **宽波段宽温域双 LCVR 斯托克斯偏振计的双目标优化设计**（CN × BCPN 双目标 Pareto + 免权重膝点，温度纳入自变量），把「先用模型量化问题、再优化」的习惯从科研带到了工程。</sub>
