# Project JARVIS — Technology Stack & Standards

This document specifies the planned technical foundation, dependencies, protocols, and architectural standards for Project JARVIS across all development phases.

---

## 1. Core Runtime & Architecture Standards

- **Primary Language**: Python 3.11+ (leveraging modern asyncio, type annotations, and performance enhancements).
- **Execution Model**: Fully asynchronous (`asyncio`) core event loop enabling non-blocking tool execution, voice streaming, and background scheduling.
- **Data Validation & Schemas**: `pydantic` (v2+) for strict data models, tool definitions, configuration management, and JSON-schema generation.
- **Packaging & Environment**: Modern Python packaging standards (`pyproject.toml`, virtual environment isolation via `uv` / `poetry` / `venv`).

---

## 2. Layer-by-Layer Technology Choices

### 2.1 Interface & Presentation Layer
- **CLI & REPL**: `typer` / `click` for command parsing; `rich` for formatted terminal output, syntax highlighting, spinners, and tables.
- **Terminal UI (TUI)**: `textual` for interactive multi-pane dashboard and live monitoring.
- **Local Web Console (Optional/Companion)**: `fastapi` + lightweight modern web UI (HTML5/Tailwind or WebSockets) for browser-based monitoring and remote control.

### 2.2 AI Brain & LLM Orchestration
- **Provider Abstraction**: Decoupled LLM client layer supporting:
  - Google Gemini API (`google-genai` / REST)
  - Anthropic Claude API (`anthropic`)
  - OpenAI API (`openai`)
  - Local Inference (`ollama`, `vLLM`, `llama.cpp`)
- **Reasoning Framework**: Custom lightweight ReAct / Plan-and-Solve orchestrator built directly on standard async primitives to avoid heavy framework lock-in.

### 2.3 Memory & Knowledge Store
- **Working Memory**: In-memory ring buffer and structured session state.
- **Vector Database**: Embedded, local-first vector store (`chromadb` / `qdrant-client` / `lancedb`).
- **Relational / Entity Database**: `sqlite3` (with `aiosqlite` or `sqlmodel`) for structured user facts, conversation history, audit logs, and schedule definitions.
- **Embeddings**: Fast local embeddings (`sentence-transformers` / `onnxruntime`) or hosted embeddings (Gemini/OpenAI).

### 2.4 System Operations & Computer Control
- **OS & Process Tools**: Native Python `subprocess` (async), `psutil` for system/hardware telemetry, `pathlib` for filesystem operations.
- **Screen & GUI Automation**: `mss` (high-performance screen capture), `pyautogui` / `pynput` for mouse/keyboard synthesis, `Pillow` for image preprocessing.
- **Web Automation**: `playwright` (async) for headless and headed browser automation and content extraction.

### 2.5 Voice & Multimodal Audio
- **Wake Word Detection**: `openWakeWord` (local, lightweight, zero cloud dependency).
- **Speech-to-Text (STT)**: `faster-whisper` (local GPU/CPU acceleration) or cloud streaming endpoints.
- **Text-to-Speech (TTS)**: `piper-tts` / `kokoro-onnx` / `edge-tts` for high-speed, local or low-latency neural synthesis.
- **Audio Capture & VAD**: `sounddevice` / `pyaudio` + `webrtcvad` / `silero-vad` for voice activity and interruption detection.

### 2.6 Integrations & Services
- **Google Services**: `google-api-python-client`, `google-auth-httplib2`, `google-auth-oauthlib` for Gmail, Google Calendar, and Google Drive.
- **Secrets Management**: `keyring` (OS-level secure credential store) and `cryptography` (AES-256 encryption for vault storage).

### 2.7 Testing & Quality Assurance
- **Unit & Integration Tests**: `pytest`, `pytest-asyncio`, `pytest-mock`.
- **Linting & Formatting**: `ruff` (fast linter and formatter), `mypy` (static type checking).
- **Evaluation & Benchmarks**: Custom scenario harness evaluating LLM tool selection accuracy, hallucination rate, and execution safety.

---

## 3. Communication & Data Protocols

1. **Tool Calling Protocol**: Standardized JSON-Schema function signatures with strongly typed Pydantic parameter definitions.
2. **Event Bus**: In-process Pub/Sub event dispatcher for cross-module signaling (e.g., voice detected, tool execution started, user confirmation requested).
3. **Audit Format**: Append-only JSONL / SQLite records capturing timestamp, actor, action, parameters, and outcome.

---

## 4. Portability & Platform Support

- **Primary Target**: Windows 11 (PowerShell / Command Prompt, Win32 automation hooks).
- **Secondary Targets**: macOS and Linux (designed with OS-agnostic abstractions for filesystem and process layers).
