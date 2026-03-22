# Experience

## AI Engineer @ Industrial Technology Research Institute (ITRI), ICL

- **Period:** Sep 2024 - Present
- **Location:** Hsinchu, Taiwan
- **Tech Stack:** LangChain, LangGraph, PGVector, LiteLLM, Arize Phoenix, FastAPI

### Contributions (zh)

- 建置並落地 Agent-powered 的企業教育訓練平台，從 PoC 驗證到實際產品上線
- 基於 LangGraph 設計 multi-node Agent，支援教學問答、模擬對練、平台導覽與評分診斷等功能
- 開發 Dynamic Node Config 模組，支援即時調整 Prompt、模型與參數配置，有效平衡使用者體驗與成本開銷
- 結合 LLM-as-a-Judge 建立評估驅動的優化流程，提升節點穩定性，並在 SLM 情境下降低 80% 以上 hallucination 導致的誤判
- 設計 Context Engineering 流程，透過檢索式範例（基於 PGVector）與 prompt template 優化，系統性改善 edge cases 與推論穩定性
- 設計並開發 Agent 專案的核心模組，提供可重用且可擴展的元件，統一並加速團隊開發 Agent 專案
- 基於 LangChain 與 LangGraph 建立模組化抽象層，封裝 context engineering、RAG、memory、tool 整合與 HITL 等核心模組
- 使用 LiteLLM 作為 LLM Gateway，實現 load balancing 與 fallback 機制，支援效能最佳化
- 使用 Arize Phoenix 進行 tracing，提升系統可觀測性，以利後續問題追蹤與成本分析
- 設計直播電商 AI 客服系統核心 Workflow，支援自動化客服與即時留言分析

### Contributions (en)

- Built and deployed an agent-powered enterprise training platform, delivering from PoC to production
- Designed multi-node agents using LangGraph, supporting QA, simulation roleplay training, platform navigation, and evaluation
- Developed a dynamic node configuration module to enable real-time adjustment of prompts, models, and parameters, balancing user experience and cost
- Implemented an evaluation-driven optimization pipeline (LLM-as-a-Judge), improving node robustness and reducing hallucination-related false positives by 80%+ under SLM scenarios
- Designed a context engineering pipeline, leveraging retrieval-based examples (PGVector) and prompt template optimization to systematically handle edge cases and improve stability
- Designed and developed core modules for the Agent platform, providing reusable and extensible components to standardize and accelerate team development of Agent-based projects
- Built a modular abstraction layer based on LangChain and LangGraph, encapsulating context engineering, RAG, memory, tool integration, and human-in-the-loop (HITL)
- Utilized LiteLLM as an LLM gateway to enable load balancing and fallback strategies for performance optimization
- Utilized Arize Phoenix for LLM tracing, improving observability for debugging, evaluation, and cost analysis
- Designed core AI workflow for livestream e-commerce, delivering automated customer service and real-time comment analysis

## AI Research Intern @ Gamania Digital Entertainment

- **Period:** Jul 2023 - Jun 2024
- **Location:** Taipei, Taiwan
- **Tech Stack:** LangChain, HuggingFace, PyTorch

### Contributions (zh)

- 建置多文件新聞資訊整合系統，針對大量新聞自動進行事件聚合與摘要生成
- 設計開發 map-reduce framework 以解決 context window 不足的情況
- 採用 P-tuning v2 對 ChatGLM 進行 PEFT，達成顯著效果提升 ROUGE-1 +13%、ROUGE-L +4%、BLEU +20%

### Contributions (en)

- Built a multi-document news information integration system that automatically performs event clustering and summary generation on large-scale news data
- Designed and developed a MapReduce framework to address context window limitations
- Applied P-Tuning v2 for PEFT of ChatGLM, achieving significant performance improvements: ROUGE-1 +13%, ROUGE-L +4%, BLEU +20%
