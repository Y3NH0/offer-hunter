# AI Mentor Platform

*Sep 2024 - Present | ITRI Project*

- **Overview:** 建置並落地 Agent 驅動的企業內訓平台，從 PoC 驗證到實際產品落地。
- 參與 PoC 到實際產品落地。負責設計開發以及優化維護系統中的 Agent 功能，E.g. 教學問答、模擬對練、課綱生成、導覽系統等。
- **Tech Stack:** LangChain, LangGraph, PGVector, LiteLLM, Arize Phoenix, FastAPI

## Contributions (zh)

- 建置並落地 Agent-powered 的企業教育訓練平台，從 PoC 驗證到實際產品上線
- 基於 LangGraph 設計 multi-node Agent，支援教學問答、模擬對練、平台導覽與評分診斷等功能
- 開發 Dynamic Node Config 模組，支援即時調整 Prompt、模型與參數配置，有效平衡使用者體驗與成本開銷
- 結合 LLM-as-a-Judge 建立評估驅動的優化流程，提升節點穩定性，並在 SLM 情境下降低 80% 以上 hallucination 導致的誤判
- 設計 Context Engineering 流程，透過檢索式範例（基於 PGVector）與 prompt template 優化，系統性改善 edge cases 與推論穩定性
- 使用 LiteLLM 作為 LLM Gateway，實現 load balancing 與 fallback 機制，支援效能最佳化
- 使用 Arize Phoenix 進行 tracing，提升系統可觀測性，以利後續問題追蹤與成本分析

## Contributions (en)

- Built and deployed an agent-powered enterprise training platform, delivering from PoC to production
- Designed multi-node agent using LangGraph, supporting QA, simulation roleplay training, platform navigation, and evaluation.
- Developed a dynamic node configuration module to enable real-time adjustment of prompts, models, and parameters, balancing user experience and cost
- Implemented an evaluation-driven optimization pipeline (LLM-as-a-Judge), improving node robustness and reducing hallucination-related false positives by 80%+ under SLM scenarios
- Designed a context engineering pipeline, leveraging retrieval-based examples (PGVector) and prompt template optimization to systematically handle edge cases and improve stability
- Utilized LiteLLM as an LLM gateway to enable load balancing and fallback strategies for performance optimization
- Utilized Arize Phoenix for LLM tracing, improving observability for debugging, evaluation, and cost analysis
