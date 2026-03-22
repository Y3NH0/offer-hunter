# ByteBERT: A Pre-trained Language Model for IoT Malware Detection Using Byte Sequences

*2024-03 - 2024-06 | 碩士論文*

> 當然前期也有更多實驗跟 survey，不過實際上是 2024-03 定好題目開始安排實驗、寫論文之類的

- **Tech Stack:** HuggingFace, PyTorch, mlflow

## Contributions (zh)

- 設計並實作 ByteBERT，建立可直接處理 raw byte 的預訓練模型，降低對 reverse engineering 的依賴
- 建立基於 MLM 的預訓練與微調流程，應用於 IoT malware detection，達到 **99.74% 準確率**，優於既有方法
- 透過模型架構調整（如 byte-level vocabulary 與 encoder 簡化）提升模型在 byte sequence 上的表現

## Contributions (en)

- Designed and implemented **ByteBERT**, a byte-level pre-trained model that reduces reliance on reverse engineering for malware analysis
- Built an **MLM-based pretraining and fine-tuning pipeline**, achieving **99.7% accuracy** on IoT malware detection and outperforming existing approaches
- Improved model performance on byte sequences through architectural adaptations (e.g., byte-level vocabulary and reduced encoder layers)
