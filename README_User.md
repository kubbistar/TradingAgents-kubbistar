# TradingAgents-kubbistar 项目说明文档

本项目的核心是一个基于多智能体（Multi-Agent）协作的金融交易分析系统。通过协调不同角色的智能体（如分析师、风险控管、决策者等），对股票市场数据进行深度分析并生成交易决策。

## 1. 项目结构概览

主要的目录结构如下：

*   **`tradingagents/`**: 核心逻辑代码
    *   `agents/`: 定义各种智能体（Agent）的行为和提示词 (Prompts)。
    *   `llm_adapters/`: 大语言模型（LLM）的适配器，负责对接不同厂家的 API (OpenAI, DeepSeek, Gemini 等)。
    *   `dataflows/`: 数据流处理，负责获取和处理金融数据。
    *   `graph/`: 定义智能体之间的协作流程（Graph）。
*   **`app/`**: 后端应用逻辑 (Web API 或应用服务)。
*   **`frontend/`**: 前端用户界面代码。
*   **`scripts/`**: 各种辅助脚本，用于安装、启动、测试和数据维护。
*   **`main.py`**: 简单的命令行入口示例，展示如何初始化和运行交易代理图。

## 2. 操作流程

系统主要的工作流程如下：

1.  **配置**: 通过 `.env` 文件或 Web 界面配置 API Key 和模型参数。
2.  **启动**: 运行主程序或 Web 服务。
3.  **输入**: 用户输入股票代码（如 NVDA, 000001.SZ）和日期。
4.  **智能体协作**:
    *   **Data Agent**: 获取相关行情、新闻、财报数据。
    *   **Analyst Agents**: 多位分析师（如基本面分析、技术面分析、情绪分析）并行分析数据。
    *   **Debate/Discussion**: 分析师之间进行辩论和讨论，修正观点。
    *   **Risk Agent**: 评估潜在风险。
5.  **决策**: **Decision Agent** 综合所有信息，给出买入/卖出/持有建议。
6.  **输出**: 生成详细的分析报告。

## 3. 模型适配与配置

本项目支持多种主流大模型。针对最新模型的适配说明如下：

### 3.1 DeepSeek (深度求索)
*   **支持模型**: `deepseek-chat` (V3), `deepseek-reasoner` (R1)
*   **配置**:
    *   在 `.env` 中设置 `DEEPSEEK_API_KEY`。
    *   由于 `deepseek-reasoner` 是推理模型，系统会自动适配其特定参数。

### 3.2 OpenAI & ChatGPT
*   **支持模型**: `gpt-4o`, `gpt-4o-mini`, `o1`, `o3-mini` (及后续版本)
*   **配置**:
    *   在 `.env` 中设置 `OPENAI_API_KEY`。
    *   **注意**: 对于 `o1/o3` 等推理模型，系统会自动将 `max_tokens` 参数转换为 `max_completion_tokens` 以避免 API 报错。

### 3.3 Google Gemini
*   **支持模型**: `gemini-2.0-flash`, `gemini-1.5-pro` 等
*   **配置**:
    *   在 `.env` 中设置 `GOOGLE_API_KEY`。
    *   系统使用 Google 官方 SDK 或 OpenAI 兼容模式连接。

### 3.4 Claude (Anthropic)
*   **支持模型**: `claude-3-5-sonnet` 等
*   **配置**:
    *   推荐通过 "自定义 OpenAI" (Custom OpenAI) 模式接入（例如使用 OpenRouter 等聚合服务，或者本地转发）。
    *   配置 `CUSTOM_OPENAI_BASE_URL` 和 `CUSTOM_OPENAI_API_KEY`。

## 4. 常见问题

*   **API 响应为空**: 通常是因为模型参数不兼容（如 DeepSeek R1 的不当调用）或网络连接问题。请确保使用最新修复后的代码。
*   **`max_tokens` 报错**: OpenAI 新推理模型不再支持 `max_tokens`，请更新代码以支持自动转换。

## 5. 如何运行

推荐使用 `scripts/` 下的脚本进行快速操作：

*   **启动调试**: `python main.py`
*   **一键启动**: 使用 `scripts/smart_start.ps1` (Windows)
