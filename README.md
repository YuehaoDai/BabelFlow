# BabelFlow (巴别流)

> Rebuild the tower, frame by frame.

BabelFlow is a **self-hosted, enterprise-oriented video translation & dubbing engine** that focuses on one core idea:

> **Perfect Synchronization** – Smart semantic splitting + duration-aware TTS.

它不是简单的“给视频套一层翻译 + 合成音频”，而是围绕时间轴去重构整条音轨，让译制片尽可能接近“原生配音”的体验。

---

## ✨ Key Features

- 🧠 **Smart Semantic Split**  
  Whisper 全量 ASR（单词级时间戳）+ VAD 静音检测 + 规则引擎，按语义和停顿拆分片段，而不是机械按 5 秒一刀。

- ⏱ **Duration-Aware TTS (IndexTTS2)**  
  使用原片段时长作为硬约束，结合 IndexTTS2 时长控制能力，让生成语音长度 ≈ 原片段长度，尽量做到“音画对齐”。

- 👥 **Speaker → Voice Profile 映射**  
  对原视频做说话人聚类（SPK_01 / SPK_02 ...），再为不同角色绑定不同音色配置（样本克隆 / 预训练 checkpoint）。

- 🏢 **Fully Self-Hosted, Enterprise-Ready**  
  Go 负责调度与状态机（控制面），Python + PyTorch 负责 GPU 推理（数据面），通过 Docker Compose 在本地一键部署：
  - Go + Gin + GORM
  - FastAPI + PyTorch (Whisper, Demucs, IndexTTS2, etc.)
  - PostgreSQL + Redis
  - Shared `/data` volume

---

## 🧱 High-Level Architecture

- **Control Plane (Go)**
  - Task orchestration & state machine
  - Job & segment management (PostgreSQL)
  - Redis-based task queue
  - LLM-based translation (Qwen / DeepSeek, pluggable)

- **Data Plane (Python / GPU)**
  - Demucs / UVR5: vocal & BGM separation
  - Faster-Whisper: ASR with word-level timestamps
  - Pyannote: VAD & speaker diarization
  - IndexTTS2: duration-aware TTS
  - FastAPI: typed ML HTTP endpoints

---

## 📦 Status

Early design & prototyping stage.

- [x] Project blueprint & architecture design  
- [ ] Initial Go control plane skeleton  
- [ ] Python ML service skeleton (FastAPI)  
- [ ] Smart split implementation (ASR + VAD)  
- [ ] Duration-aware TTS integration  
- [ ] End-to-end demo pipeline

---

## 📜 License

Apache 2.0
