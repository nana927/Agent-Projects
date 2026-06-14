# AI-Native 工业视觉算法评测 Agent 系统

基于 **LangGraph** 构建的多模块 Agent 系统，面向工业视觉算法评测场景，涵盖知识库问答（RAG）、记忆管理（Memory）、测试任务自动化（Workflow）等核心能力。


## 功能完成度

### ✅ 已完成

| 模块 | 功能 | 说明 |
|---|---|---|
| **QA 问答** | 文档上传 & 解析入库 | PDF/DOC/DOCX/PPT/HTML/MD/图片 + OCR |
| | 语义分块 + 滑动窗口重叠 | Sentence-aware Chunking |
| | FAISS / Milvus 向量存储 | 双后端可切换 |
| | 意图识别 | technical_qa / general_chat / history_query / comparison |
| | Query 重写 & 扩写 | 指代消解 + 术语标准化 + 多变体并行召回 |
| | 预置 QA 对快速匹配 | 高频问题毫秒级响应 |
| | 多路召回融合 | 向量检索 + 关键词检索 + 历史对话复用 |
| | LLM Judge 答案自评 | 完整性 / 准确性 / 相关性三维打分 |
| | 追问闭环 | 信息缺口检测 → 追问生成 → 二次检索 |
| | 流式问答 (SSE) | 实时 token 推送 |
| **Memory** | 三层记忆架构 | 短期(会话缓存) / 长期(SQLite) / 语义(FAISS/Milvus) |
| | Dreaming 离线记忆整理 | 聚类 → 去重 → 矛盾检测 → 归档 → 画像抽取 |
| | 会话记忆召回 | 上下文注入 + 历史答案复用检测 |
| | 对话后记忆持久化 | extract → update → save |
| | User Profile 抽取 | 稳定偏好 & 长期信息持久化 |
| **测试任务** | 测试任务 CRUD + 状态管理 | 计划 → 确认 → 执行 → 完成 |
| | 资源驱动逆向规划 | WorkflowPlanner，从目标资源反推工具链 |
| | 测试用例自动生成 | xlsx 模板解析 → LLM ToolList (SKILL.md) → write_cases_to_template.py |
| | Excel 模板智能解析 | 冻结窗格定位(置信度 0.95) + 关键词 fallback + 多行合并 + 样式提取 |
| | 新建 Workbook 写入 | 非模板副本，样式复用，字段归一化，case_type → sheet 分配 |
| | 版本隔离 | 主版本 / 子版本树 + visible_version_ids 范围过滤 |
| | 产物归档 | TestArtifact 落库 + 任务详情携带产物 |
| **前端** | 测试任务执行看板 | 项目/版本选择 → 计划 → 确认 → 执行 → 产物 |
| | QA 问答页面 | 流式对话 + 记忆管理 |
| | 版本 & 测试包管理 | CRUD + 资源绑定 |
| | 知识库管理 | 上传 / 解析 / 文件列表 |
| | 模板管理 | 上传 / 解析结果预览 |
| | 产物列表 & 下载 | 版本过滤 + 文件下载 |

### 🚧 已有框架 / 待完善

| 模块 | 功能 | 当前状态 |
|---|---|---|
| **GT 重构** | extract_gt_format_node | 节点已注册，executor bridge 已接入，缺少完整 GT 解析/重构逻辑 |
| **Pred 重构** | extract_pred_format_node | 节点已注册，executor bridge 已接入，缺少完整 Pred 解析/重构逻辑 |
| **引擎执行** | run_engine_node | 节点已注册，缺少实际算法执行 Harness 对接 |
| **指标计算** | metric_runner_node | 节点已有部分实现，缺少完整 metric_config → 计算 → 输出链路 |
| **可视化** | visualize_node | 节点已注册，缺少图表生成 & 前端渲染对接 |
| **报告生成** | report_analysis / test_report_generation Skill | SKILL.md 已定义，缺少完整 LLM 生成链路 & 模板填充 |
| **Bug 分析** | bug_analysis_graph | 子图已注册，缺少完整 historical_bugs → 分析 → 归档链路 |

---

## TODO

### 近期
- [ ] GT 格式解析 & 标准化重构（extract_gt_format_node 完整实现）
- [ ] Pred 格式解析 & 标准化重构（extract_pred_format_node 完整实现）
- [ ] 可视化图表生成（visualize_node 完整实现 + 前端图表渲染）
- [ ] 测试报告自动生成（report_analysis / test_report_generation Skill 完整链路）
- [ ] 引擎执行 Harness 对接（run_engine_node 对接实际算法服务）
- [ ] 指标计算完整链路（metric_runner_node 对接 metric_config → 多指标输出）
- [ ] 测试任务执行进度实时推送（WebSocket）

### 中期
- [ ] 测试报告导出为 Word/PDF
- [ ] Bug 分析完整链路（historical_bugs → 分析 → 归档）
- [ ] 多 Agent 协作（QA Agent + Test Agent 联合推理）
- [ ] 评测结果历史趋势分析 & 基线对比
- [ ] 模板字段 LLM 精炼（字段类型推断 / required 检测 / 示例值提取）
- [ ] 测试用例覆盖率分析 & 缺失场景推荐
- [ ] 知识库自动更新（新文档 → 增量索引 → 过期文档清理）
- [ ] RAG 检索质量自动化评测（Recall / Precision / MRR）

### 远期
- [ ] 模型评测结果自动对比 & 回归检测
- [ ] 自然语言 → 测试计划端到端生成
- [ ] Memory 矛盾解决的人机协同确认流程
- [ ] Dreaming 定时任务调度（cron / scheduler）
- [ ] 多租户项目隔离
- [ ] 分布式任务执行（Celery / Ray）
- [ ] 自定义评测指标 DSL
- [ ] 开放 API 插件市场




## 技术栈

| 层级 | 技术 |
|---|---|
| 编排框架 | **LangGraph**（StateGraph + 条件路由 + Send 并行分发） |
| Web 框架 | **FastAPI**（RESTful API + 流式 SSE） |
| LLM 基座 | OpenAI-compatible API（支持多模型切换） |
| 向量检索 | **FAISS**（默认）/ **Milvus**（大规模生产） |
| Embedding | OpenAI Embedding / Sentence-Transformers（本地） |
| 数据库 | SQLite（开发）/ PostgreSQL（生产） + **SQLAlchemy** ORM |
| 文档解析 | DeepDoc（PDF/DOC/DOCX/PPT/HTML/Markdown/图片 OCR） |
| 模板解析 | **openpyxl**（Excel 模板结构提取 + 样式检测 + 表头智能识别） |

---

## 项目架构

```
                         ┌─────────────────────────────┐
                         │       Frontend (React)      │
                         └─────────────┬───────────────┘
                                       │ HTTP / SSE
                         ┌─────────────▼───────────────┐
                         │    FastAPI (api/v1/)        │
                         │  路由层：请求接收 & 参数校验  │
                         └─────────────┬───────────────┘
                                       │
              ┌────────────────────────┼────────────────────────┐
              │                        │                        │
    ┌─────────▼─────────┐  ┌──────────▼──────────┐  ┌─────────▼─────────┐
    │   QA Graph        │  │   Main Graph        │  │   Memory Pipeline │
    │  (QA 问答 Agent)   │  │  LangGraph 编       │  │  (记忆管理 Pipeline)│
    │  LangGraph 编排    │  │                     │  │  离线异步任务       │
    └─────────┬─────────┘  └──────────┬──────────┘   └─────────┬─────────┘
              │                       │                       │
    ┌─────────▼─────────┐  ┌──────────▼──────────┐  ┌─────────▼─────────┐
    │   Services        │  │   Tools & Skills    │  │   Memory Store    │
    │  业务逻辑层        │  │  可编排工具节点      │  │  FAISS / Milvus   │
    └─────────┬─────────┘  └──────────┬──────────┘  │  / SQLite         │
              │                       │             └───────────────────┘
    ┌─────────▼─────────┐  ┌──────────▼──────────┐
    │   RAG Engine      │  │   Workflow Engine   │
    │  向量检索 + 生成   │   │  自动规划 + 执行     │
    └───────────────────┘  └─────────────────────┘
```

### 目录结构

```
backend/
├── app/
│   ├── agent/                    # 🔵 LangGraph Agent 编排
│   │   ├── main_graph.py         # 测试任务主图（StateGraph）
│   │   ├── router.py             # 路由分发（条件路由 + Send 并行）
│   │   ├── state.py              # 全局状态定义（MainGraphState）
│   │   ├── qa_graph/             # QA 问答子图
│   │   │   ├── graph.py          # QA Graph 构建（16个节点）
│   │   │   ├── state.py          # QA 状态机
│   │   │   ├── runner.py         # QA Agent 入口
│   │   │   └── nodes/            # 16 个功能节点
│   │   ├── test_case_generator_graph/  # 测试用例生成子图
│   │   ├── test_task_chain_plan/       # 测试任务执行链
│   │   │   └── nodes/            # 10 个功能节点
│   │   ├── report_analysis_graph/      # 报告分析子图
│   │   └── bug_analysis_graph/         # Bug 分析子图
│   │
│   ├── RAG/                      # 🟢 RAG 知识引擎
│   │   ├── rag_service.py        # RAG 服务入口
│   │   ├── ingestion.py          # 文档入库 Pipeline
│   │   ├── chunking.py           # 文档切分策略
│   │   ├── intent_router.py      # 意图识别 & 路由
│   │   ├── embeddings.py         # Embedding 编码
│   │   ├── storage/              # 向量存储（FAISS/Milvus）
│   │   ├── deepdoc/              # DeepDoc 解析器
│   │   └── service/              # 检索 & 生成服务
│   │
│   ├── memory/                   # 🟡 记忆管理
│   │   ├── memory_pipeline.py    # 记忆管线入口
│   │   ├── dreaming_service.py   # Dreaming 离线整理
│   │   ├── memory_store.py       # Memory 抽象基类
│   │   ├── faiss_memory_store.py # FAISS 向量记忆
│   │   ├── milvus_memory_store.py# Milvus 向量记忆
│   │   ├── sqlite_memory_store.py# SQLite 结构化记忆
│   │   └── schemas.py            # 记忆数据模型
│   │
│   ├── tools/                    # 🟣 可编排工具集
│   │   ├── evaluation/           # 指标计算 / 报告生成 / 可视化
│   │   ├── template_parser/      # Excel 模板结构解析器
│   │   │   ├── excel_parser.py   # 工作簿解析
│   │   │   ├── header_detector.py# 表头智能检测（冻结窗格+关键词）
│   │   │   ├── table_extractor.py# 单元格矩阵提取
│   │   │   └── style_extractor.py# 样式信息提取
│   │   └── executors/            # 执行器（脚本 / API / Python）
│   │
│   ├── skills/                   # 🟠 Skill 沉淀复用
│   │   ├── test_case_generation/ # 测试用例生成 Skill
│   │   │   ├── SKILL.md          # Skill 定义（LLM 可读）
│   │   │   └── scripts/          # 白名单子进程脚本
│   │   ├── bug_analysis/         # Bug 分析 Skill
│   │   ├── report_analysis/      # 报告分析 Skill
│   │   └── configs/              # 配置文件（tool/resource/task_goal）
│   │
│   ├── workflow/                 # ⚙️ Workflow 引擎
│   │   ├── planner.py            # 资源驱动逆向规划器
│   │   ├── resource_mapper.py    # 资源类型映射 & 版本隔离
│   │   └── config_loader.py      # YAML 配置加载
│   │
│   ├── services/                 # 业务逻辑层
│   ├── api/v1/                   # RESTful API 路由
│   ├── domain/                   # 数据模型（Pydantic + SQLAlchemy）
│   └── db/                       # 数据库连接 & 模型
│
├── data/                         # 数据目录（模板/向量库/测试产物）
├── configs/                      # 全局配置
└── requirements.txt
```

---

## QA 模块
基于 **LangGraph StateGraph** 构建，包含 **16 个节点**的 ReAct 推理链路，覆盖意图识别 → 查询重写 → 检索规划 → 多路召回 → 答案生成 → 质量评估 → 追问闭环的完整流程。

```
用户问题
  │
  ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ ① 上下文准备  │───▶│ ② 记忆召回    │───▶│ ③ Query 重写  │
│ prepare_ctx  │    │ memory_ctx   │    │ rewrite_query│
└──────────────┘    └──────────────┘    └──────┬───────┘
                                               │
                    ┌──────────────────────────┘
                    ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ ④ 意图分类    │───▶│ ⑤ 历史复用检测 │───▶│ ⑥ 检索规划    │
│ intent_cls   │    │ history_reuse│    │ plan_retrieval│
└──────────────┘    └──────────────┘    └──────┬───────┘
                                               │
                        ┌──────────────────────┘
                        ▼
┌──────────────┐     ┌──────────────┐      ┌──────────────┐
│ ⑦ Query 增强  │───▶│ ⑧ 多路检索    │───▶│ ⑨ 答案生成    │
│ query_enhance│     │ retrieve     │      │ answer       │
└──────────────┘     └──────────────┘      └──────┬───────┘
                                                  │
                    ┌─────────────────────────────┘
                    ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ ⑩ 答案评估    │───▶│ ⑪ 追问生成    │───▶│ ⑫ 记忆提取    │
│ evaluate     │    │ followup     │    │ extract_mem  │
└──────────────┘    └──────────────┘    └──────────────┘
```

### 核心优化

| 优化项 | 实现 |
|---|---|
| **文档切分** | 语义分块（Sentence-aware Chunking）+ 滑动窗口重叠，保留上下文边界 |
| **意图识别** | 基于 LLM 的多分类器：`technical_qa` / `general_chat` / `history_query` / `comparison`，不同意图走不同检索策略 |
| **Query 重写** | 多轮对话上下文融合 + 指代消解 + 术语标准化，将口语化问题转为检索友好格式 |
| **Query 扩写** | 对简短 Query 进行语义扩展，生成多个检索变体并行召回 |
| **预置 QA 对** | 高频业务问题直接匹配预置答案，绕过检索链路，毫秒级响应 |
| **检索规划** | LLM 分析问题所需知识域 → 自动选择检索源（KB文档 / 历史对话 / 测试报告）|
| **多路召回** | 向量检索（语义相似）+ 关键词检索（精确匹配）+ 历史对话复用，三路融合 |
| **答案评估** | LLM Judge 自评：完整性 / 准确性 / 相关性三维打分，低于阈值触发重检索 |
| **追问闭环** | 检测答案中的信息缺口 → 自动生成追问 → 二次检索补充 |
| **记忆融合** | 会话开始时从 Memory Store 召回用户 Profile 和历史上下文（节点②）；回答前检测历史相似问题复用（节点⑤）；回答后将关键信息提取为记忆持久化（节点⑫），形成"召回 → 复用 → 沉淀"的闭环 |

### 与 Memory 模块的集成
```
会话开始                     问答结束
   │                           │
   ▼                           ▼
retrieve_memory_context    extract_memory → update_memory → save_memory
   │                           │
   │  从 FAISS/Milvus 召回      │  从对话中提取关键信息
   │  用户 Profile + 历史摘要    │  聚类去重 + 矛盾检测
   │                           │  持久化到三层记忆存储
   ▼                           ▼
 check_history_answer_reuse   (触发 Dreaming 整理)
   │
   │  语义相似度匹配
   │  历史答案 > 阈值 → 直接复用
   ▼
 (进入检索或直接回答)
```

### 效果展示
<img width="2454" height="1321" alt="image" src="https://github.com/user-attachments/assets/5e955c4e-27b2-41fd-a3f2-5290899cef6e" />
<img width="1251" height="168" alt="image" src="https://github.com/user-attachments/assets/bf19dbcb-63aa-4165-b74b-9f57fb1b200d" />
<img width="1307" height="508" alt="image" src="https://github.com/user-attachments/assets/22a8df2f-b672-42b2-8a18-6323a5dd14ee" />
<img width="1255" height="513" alt="image" src="https://github.com/user-attachments/assets/6d9ba58c-57d2-40ba-bf5e-84deba3b8e66" />
<img width="1250" height="501" alt="image" src="https://github.com/user-attachments/assets/0cf47c87-6aa9-4de1-a075-651ec093fc5a" />
<img width="1313" height="562" alt="image" src="https://github.com/user-attachments/assets/6e733e3d-0d7a-4c41-82f1-ebf05db3ce76" />
<img width="1307" height="462" alt="image" src="https://github.com/user-attachments/assets/40267db2-ee29-43e7-9800-1a70ed3582b6" />




### 文档入库 Pipeline

```
文档上传 (PDF/DOC/PPT/HTML/MD/图片)
  │
  ▼
DeepDoc 解析器 → 结构化文本提取 + OCR
  │
  ▼
语义分块 (Sentence-aware Chunking)
  │
  ▼
Embedding 编码 (OpenAI / Sentence-Transformers)
  │
  ▼
向量存储 (FAISS / Milvus) + 全文索引
```

### 向量数据库

| 方案 | 适用场景 |
|---|---|
| **FAISS** (默认) | 开发环境 / 小规模知识库，零依赖，本地运行 |
| **Milvus** | 生产环境 / 大规模知识库，支持分布式、增量更新、混合检索 |

---

### 效果展示
<img width="2422" height="802" alt="image" src="https://github.com/user-attachments/assets/fbe4fe32-2d2a-4f7a-bc91-754cfcab70fc" />


## 二、Memory 记忆管理模块

### 架构设计

Memory 模块采用**短期 / 长期 / 语义三层记忆架构**，支持会话级上下文的实时存取 + 离线的 Dreaming 记忆整理。

```
┌─────────────────────────────────────────────────────┐
│                   Memory Pipeline                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────────┐   ┌──────────┐   ┌──────────────────┐ │
│  │ 短期记忆  │   │ 长期记忆  │   │   语义记忆        │ │
│  │(会话缓存) │   │(SQLite)  │   │ (FAISS/Milvus)   │ │
│  │ TTL: 24h │   │ 结构化存储│   │  向量化 + 聚类    │ │
│  └──────────┘   └──────────┘   └──────────────────┘ │
│                                                     │
│  ┌──────────────────────────────────────────────┐   │
│  │          Dreaming Service（离线整理）          │   │
│  │  • 记忆聚类 (Cluster)                         │   │
│  │  • 去重合并 (Dedup)                           │   │
│  │  • 矛盾检测 (Contradiction Detection)         │   │
│  │  • 过期归档 (TTL-based Archive)               │   │
│  │  • 用户画像提取 (Profile Extraction)          │   │
│  └──────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

### 核心功能

| 功能 | 描述 |
|---|---|
| **短期记忆** | 会话级缓存，基于时间窗口滑动，自动过期 |
| **长期记忆** | 结构化存储于 SQLite/PostgreSQL，持久化用户偏好和关键信息 |
| **语义记忆** | 向量化存储（FAISS/Milvus），支持语义相似召回 |
| **记忆聚类** | 对近期记忆进行语义聚类，合并相似片段，生成高层摘要 |
| **去重合并** | 检测重复/高度相似的记忆条目，保留最新版本，删除冗余 |
| **矛盾检测** | LLM 驱动的语义矛盾识别：当新旧记忆存在冲突时，标记并提示用户确认 |
| **过期归档** | 基于 TTL 自动归档过期记忆，保留关键信息摘要 |
| **用户画像** | 从交互历史抽取稳定偏好和长期有效信息，持久化为 User Profile |

### Dreaming 整理流程

```
触发条件：定时 / 会话结束 / 记忆量超阈值
  │
  ▼
① 拉取近 N 小时记忆
  │
  ▼
② 语义聚类 → 按 Topic 分组
  │
  ▼
③ 重复检测 → 合并 / 删除
  │
  ▼
④ 矛盾检测 → LLM 判断冲突 → 标记 / 解决
  │
  ▼
⑤ 过期归档 → TTL 过期 → 移到归档区
  │
  ▼
⑥ 画像更新 → 抽取稳定偏好 → 更新 Profile
```

---

## 三、测试任务模块

### 编排架构

测试任务模块以 **LangGraph MainGraph** 为主体，通过 **资源驱动逆向规划** 自动编排测试流程，将测试用例生成、指标计算、报告分析、Bug 分析等能力拆解为可编排、可复用的工具节点。

```
用户创建测试任务
  │
  ▼
┌────────────────┐
│ ① TestTaskUpdate │  上下文加载 & 资源继承
│   资源状态检查    │  (PRD / 模板 / GT / 历史数据)
└───────┬────────┘
        │
        │  Send 并行分发 ──────────────────────┐
        ▼                        ▼             ▼
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│② TestCase Gen │   │③ Extract GT  │   │④ Run Engine  │
│  测试用例生成  │   │  GT 格式提取  │   │  算法执行     │
└──────┬───────┘   └──────┬───────┘   └──────┬───────┘
       │                  │                  │
       └──────────────────┼──────────────────┘
                          │
                          ▼
              ┌─────────────────────┐
              │⑤ MergeResourceUpdates│  资源路径增量合并
              │   共享状态同步        │
              └─────────┬───────────┘
                        │
              ┌─────────▼───────────┐
              │⑥ Metric Runner      │  指标计算
              │   class_metrics     │  分类别指标
              │   confusion_matrix  │  混淆矩阵
              │   bad_cases         │  Bad Case 分析
              └─────────┬───────────┘
                        │
           ┌────────────┼────────────┐
           ▼            ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐
   │⑦ Visualize│ │⑧ Report  │ │⑨ Bug     │
   │  可视化    │ │  报告分析 │ │  Bug分析  │
   └─────┬────┘ └─────┬────┘ └─────┬────┘
         │            │            │
         └────────────┼────────────┘
                      ▼
           ┌──────────────────┐
           │⑩ ArchiveArtifacts│  资源归档 → TestArtifact 表
           └────────┬─────────┘
                    ▼
           ┌──────────────────┐
           │⑪ UpdateTaskStatus│  任务状态更新 → 完成
           └──────────────────┘
```

### 资源驱动逆向规划

Workflow Planner 从目标资源出发，沿依赖链反推所需工具链：

```
目标：output_report（测试报告）
  │
  ▼
resource_config.yaml → 定义每个资源的位置和类型
  │
  ▼
tool_config.yaml → 定义哪个工具生产哪个资源
  │
  ▼
逆向推导 → [generate_test_cases → run_engine → metric_runner → generate_report]
  │
  ▼
自动排步 → Tool Plan JSON → 分发给 Executor 执行
```

### 以测试用例生成子系统为例

```
模板上传 (xlsx)
  │
  ▼
模板解析器 (template_parser)
  ├── 表头智能检测：冻结窗格定位（置信度 0.95）+ 关键词打分 fallback
  ├── 多行表头合并：父子表头 "-" 拼接
  ├── 样式提取：列宽 / 冻结窗格 / 字体 / 填充
  └── 示例行提取 + 填写要求识别
  │
  ▼
模板 Schema → {sheets, headers, fields, style_summary, ...}
  │
  ▼
LLM ToolList 模式（SKILL.md 定义 Step 流程）
  ├── Step 1: 从 PRD 提取缺陷名称
  ├── Step 2: 上下文分类 (effect / efficiency / stability)
  ├── Step 3-5: 按 case_type 生成测试用例 ← LLM per-defect 循环
  └── 每 Step: LLM → validate → retry (最多 N 次)
  │
  ▼
write_cases_to_template.py（白名单子进程）
  ├── 新建 Workbook（非模板副本）
  ├── 按 sheet 独立表头映射
  ├── case_type → sheet 自动分配
  ├── 样式复用（列宽/冻结/表头字体/填充）
  └── 字段归一化匹配（去空格/全角空格）
  │
  ▼
generated_test_cases.xlsx（干净输出）
```
### 展示
#### 测试模板抽取
<img width="2510" height="1346" alt="image" src="https://github.com/user-attachments/assets/eeceb562-55c4-4b1e-9e56-2640c72dd496" />
<img width="2559" height="1013" alt="image" src="https://github.com/user-attachments/assets/8d54a551-8ff7-4196-91f3-3ef6a9b69836" />
<img width="2524" height="1348" alt="image" src="https://github.com/user-attachments/assets/974781fb-dff8-4ef4-bf5e-0907f9427d0e" />

#### skill展示



#### 测试用例生成
<img width="1741" height="1019" alt="image" src="https://github.com/user-attachments/assets/c50e2860-1768-4947-a8c0-17f729906e8d" />
<img width="2000" height="1097" alt="image" src="https://github.com/user-attachments/assets/cc2fadc1-3030-4a12-a6f3-2dc55425c026" />
<img width="1332" height="1082" alt="image" src="https://github.com/user-attachments/assets/fbc3e187-edc3-4e55-89b9-420481b99aed" />
<img width="1493" height="1104" alt="image" src="https://github.com/user-attachments/assets/9f1e272c-eb05-4ca6-bf03-3912c14ebdb5" />


### 版本隔离

```
Project
  └── Version（主版本, type='major'）
        ├── TestPackage（主版本包：PRD / 模板 / GT）
        └── Version（子版本, type='test', parent_version_id）
              ├── TestPackage（子版本包：继承 + 覆盖）
              └── TestTask（绑定子版本，资源隔离）

get_visible_version_ids(project_id, version_id)
  ├── 子版本 → [version_id]
  └── 主版本 → [version_id] + child_ids
```

所有任务、产物、候选资源查询均通过 `visible_version_ids` 过滤，确保不同主版本间数据严格隔离。


### 展示
<img width="2494" height="1416" alt="image" src="https://github.com/user-attachments/assets/e45d1726-0823-483d-aff9-fee42d4db184" />
<img width="1516" height="1029" alt="image" src="https://github.com/user-attachments/assets/0142d2d2-0f0f-46d8-af3e-b55324f30f76" />
<img width="1512" height="954" alt="image" src="https://github.com/user-attachments/assets/498d0dfc-2f9d-49ad-b52e-c4a22322c50c" />
<img width="1428" height="1356" alt="image" src="https://github.com/user-attachments/assets/d42686af-44d1-4a29-9c5c-0092c0d09cab" />


### Harness 执行调度

```
Tool Plan JSON
  │
  ▼
Workflow Planner → executor 路由
  ├── agent: 进入 LangGraph 子图（QA / TestCase / Report / Bug）
  ├── skill: 调用 Skill 白名单脚本（子进程隔离）
  ├── python: 直接调用 Python 函数
  └── api: HTTP 调用外部服务
  │
  ▼
ScriptToolRunner（安全沙箱）
  ├── tool_manifest.json 白名单校验
  ├── subprocess 子进程隔离
  └── 输入/输出 JSON 文件桥接
```

---

## 四、API 接口

| 模块 | 端点 | 方法 | 说明 |
|---|---|---|---|
| **知识库** | `/api/v1/kb/files` | GET | 知识库文件列表 |
| | `/api/v1/kb/upload` | POST | 上传文档入库 |
| | `/api/v1/kb/{id}/parse` | POST | 文档解析 + 切分 + 向量化 |
| **QA 问答** | `/api/v1/qa/chat` | POST | 单轮问答 |
| | `/api/v1/qa/chat/stream` | POST | 流式问答（SSE） |
| | `/api/v1/qa/memory/{session_id}` | GET | 查询会话记忆 |
| **版本管理** | `/api/v1/projects/{id}/versions` | GET/POST | 版本 CRUD |
| | `/api/v1/versions/{id}/children` | GET | 子版本列表 |
| **测试包** | `/api/v1/versions/{id}/test-package` | GET | 获取测试包 |
| **测试模板** | `/api/v1/templates/upload` | POST | 上传测试用例模板 |
| | `/api/v1/templates/{id}` | GET | 模板详情（含解析结果） |
| **测试任务** | `/api/v1/test-tasks` | GET/POST | 任务列表 / 创建 |
| | `/api/v1/test-tasks/plan` | POST | 生成执行计划 |
| | `/api/v1/test-tasks/{id}/confirm` | POST | 确认计划 |
| | `/api/v1/test-tasks/{id}/execute` | POST | 执行任务 |
| | `/api/v1/test-tasks/{id}/result` | GET | 任务结果（含产物） |
| **产物** | `/api/v1/artifacts` | GET | 产物列表（支持版本过滤） |
| | `/api/v1/artifacts/{id}/download` | GET | 下载产物文件 |
| **报告** | `/api/v1/reports/{id}` | GET | 测试报告详情 |
| | `/api/v1/visualization/{id}` | GET | 可视化数据 |

---


## 项目亮点

1. **LangGraph 深度应用**：16 节点 QA Graph + 11 节点 MainGraph + 4 个子图，利用条件路由、Send 并行分发、状态 reducer 实现复杂 Agent 编排
2. **RAG 全链路优化**：从文档解析 → 语义切分 → 向量化 → 意图识别 → Query 重写 → 多路召回 → 答案评估 → 追问闭环，每个环节可独立优化
3. **Dreaming 记忆整理**：离线语义聚类 + 去重 + 矛盾检测 + 画像提取，让 Agent 在长期对话中持续学习用户偏好
4. **资源驱动逆向规划**：从目标资源自动推导工具链，无需手写 Workflow，新增工具只需更新 YAML 配置
5. **版本隔离机制**：主版本 / 子版本树形结构 + `visible_version_ids` 范围过滤，确保多版本并行评测数据不串
6. **模板智能解析**：冻结窗格定位表头（置信度 0.95）+ 关键词打分 fallback + 多行表头合并 + 样式提取
7. **Skill 沉淀复用**：白名单子进程沙箱执行 + SKILL.md 定义 LLM 可读流程 + Tool Manifest 安全校验
