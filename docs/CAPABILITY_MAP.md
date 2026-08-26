# Project JARVIS — Capability Map

This document establishes the structured capability matrix across all development phases of Project JARVIS, mapping functional modules to target phases and milestone features.

---

## Capability Matrix Overview

| Domain | Core Capability | Target Phase | Status / Milestone |
| :--- | :--- | :--- | :--- |
| **Command Centre** | CLI REPL & Interactive Shell | Phase 1 | Foundation |
| **Command Centre** | Structured Logging & Terminal TUI | Phase 1 | Foundation |
| **Command Centre** | Web / Dashboard Interface | Phase 1 | Foundation |
| **AI Brain** | Multi-Model LLM Adapter (OpenAI / Anthropic / Gemini / Local) | Phase 2 | Core Reasoning |
| **AI Brain** | Dynamic Context Assembler & Prompt Orchestration | Phase 2 | Core Reasoning |
| **AI Brain** | Plan-and-Solve Task Decomposition Engine | Phase 2 | Core Reasoning |
| **Tools** | Filesystem Reader / Writer / Searcher | Phase 3 | Tooling |
| **Tools** | Shell / Process Execution Sandboxing | Phase 3 | Tooling |
| **Tools** | System Information & Resource Monitoring | Phase 3 | Tooling |
| **Tools** | Web Browsing & Content Scraping | Phase 3 | Tooling |
| **Google Services** | Gmail Integration (Draft, Search, Send with Approval) | Phase 4 | Ecosystem |
| **Google Services** | Google Calendar (Events, Reminders, Scheduling) | Phase 4 | Ecosystem |
| **Google Services** | Google Drive (Doc Search, Sync, Upload/Download) | Phase 4 | Ecosystem |
| **Memory** | Ephemeral Working Memory & Scratchpad | Phase 5 | Knowledge |
| **Memory** | Long-Term Vector Memory (Semantic Search & RAG) | Phase 5 | Knowledge |
| **Memory** | Relational Entity Graph & User Preference Store | Phase 5 | Knowledge |
| **Computer Control** | Screen Capture & Visual Grounding / Multimodal Vision | Phase 6 | Automation |
| **Computer Control** | Mouse / Keyboard Synthetic Input & Window Management | Phase 6 | Automation |
| **Computer Control** | Application Launching & OS Automation Hooks | Phase 6 | Automation |
| **Voice** | Local Wake-Word Engine ("Jarvis") | Phase 7 | Multimodal |
| **Voice** | Speech-to-Text (STT) Transcription Pipeline | Phase 7 | Multimodal |
| **Voice** | Text-to-Speech (TTS) Low-Latency Voice Synthesis | Phase 7 | Multimodal |
| **Voice** | Full-Duplex Audio Conversation & Interruption Handling | Phase 7 | Multimodal |
| **Phone Control** | Mobile Companion Gateway & Remote Command Dispatch | Phase 8 | Mobile |
| **Phone Control** | Push Notifications & Urgent Alerts Relay | Phase 8 | Mobile |
| **Phone Control** | Mobile Telemetry & Location-Aware Triggers | Phase 8 | Mobile |
| **Multi-Agents** | Hierarchical Agent Coordinator (Orchestrator-Worker) | Phase 9 | Swarm |
| **Multi-Agents** | Specialist Agents (Coder, Researcher, Critic, Planner) | Phase 9 | Swarm |
| **Multi-Agents** | Inter-Agent Communication Bus & Shared Blackboard | Phase 9 | Swarm |
| **Autonomy** | Cron & Event-Driven Background Schedulers | Phase 10 | Autonomous |
| **Autonomy** | Proactive Suggestions & Daily Briefing Generation | Phase 10 | Autonomous |
| **Autonomy** | Self-Directed Goal Execution & Progress Tracking | Phase 10 | Autonomous |
| **Security** | Risk-Tiered Action Approval Gateway (Human-in-the-Loop) | Phase 11 | Guardrails |
| **Security** | Encrypted Vault for API Keys, Tokens & Secrets | Phase 11 | Guardrails |
| **Security** | Immutable Activity & Decision Audit Logger | Phase 11 | Guardrails |
| **Testing** | Unit & Integration Test Suite | Phase 12 | Quality |
| **Testing** | Agent Evals, Safety Benchmarks & Mock Sandboxes | Phase 12 | Quality |
| **JARVIS V1.0** | Full System Integration, Release Package & Setup Wizard | Release | Production V1.0 |

---

## Detailed Capability Breakdown

### 1. Command Centre & User Interaction (Phase 1)
- **Input Channels**: Interactive CLI shell, single-shot CLI invocation, optional lightweight local web control panel.
- **Output Streams**: Rich formatted markdown, streaming token responses, status badges, progress indicators.
- **Session Management**: Session persistence, conversation branching, historical query replay.

### 2. AI Brain & Cognitive Reasoning (Phase 2)
- **LLM Abstraction**: Unified provider interface supporting streaming, function/tool calling, and structured JSON schemas.
- **Planner / Reasoner**: Multi-step chain-of-thought, ReAct (Reason + Act) loop, self-reflection, and error recovery.
- **Dynamic Context**: Automatic injection of active workspace state, recent conversation history, and relevant memories.

### 3. Core Tools & System Operations (Phase 3)
- **Filesystem Tools**: Read, write, patch, find files, inspect directories, grep search with safety limits.
- **Process & Shell Tools**: Execution of system commands in controlled subprocesses with timeout and output capture.
- **Network / Web Tools**: HTTP requests, URL content fetching, web search integration.

### 4. Google Services Integration (Phase 4)
- **Authentication**: Secure OAuth2 PKCE flow with local token caching and automatic refresh.
- **Workspace Services**:
  - *Gmail*: Read unread emails, thread summarization, draft composition, sending (subject to confirmation).
  - *Google Calendar*: Conflict-free event scheduling, agenda overview, reminders.
  - *Google Drive*: File search, downloading docs, syncing reference documents into memory.

### 5. Memory Model (Phase 5)
- **Working Memory**: In-memory context buffer preserving current turn and intermediate tool step traces.
- **Vector Memory**: Embedding-based vector database for semantic recall of past interactions and indexed documents.
- **Entity & Preference Store**: Key-value / relational graph tracking persistent facts (e.g., user preferences, project paths, personal preferences).

### 6. Computer Control & GUI Automation (Phase 6)
- **Visual Grounding**: Desktop screenshot capture with VLM (Vision Language Model) element detection.
- **Input Synthesis**: Mouse navigation, click/double-click, drag, keyboard typing, hotkey triggers.
- **Application Orchestration**: Process spawning, active window focus management, UI automation fallback.

### 7. Voice Interface (Phase 7)
- **Wake Word**: Offline, lightweight keyword spotting engine running locally.
- **Speech-to-Text (STT)**: High-accuracy streaming transcription.
- **Text-to-Speech (TTS)**: Expressive, low-latency neural speech generation.
- **Duplex Audio Pipeline**: Acoustic echo cancellation, voice activity detection (VAD), and barge-in / interruption detection.

### 8. Mobile / Phone Control Bridge (Phase 8)
- **Remote Gateway**: Secure local/cloud tunnel or relay for remote instruction dispatch.
- **Notification Engine**: High-priority alert dispatching to user mobile devices.
- **Mobile Actions**: Device state querying, SMS/notification read/relay (where supported by client bridge).

### 9. Multi-Agent Swarm (Phase 9)
- **Orchestrator Agent**: Breaks user objectives into dependency graphs and assigns subtasks.
- **Specialist Personas**: Research Agent, Coding Agent, Reviewer / Critic Agent, Tool Worker.
- **Consensus & Evaluation**: Multi-perspective validation before finalizing critical outputs.

### 10. Proactivity & Autonomy (Phase 10)
- **Scheduled Jobs**: Persistent cron-like triggers (daily digests, periodic file cleanups, repo monitoring).
- **Proactive Insights**: Noticing upcoming calendar conflicts, deadlines, or actionable inbox items.
- **Long-Running Autonomous Tasks**: Resumable multi-step goal execution with periodic checkpointing.

### 11. Security & Guardrails (Phase 11)
- **Approval Gate**: UI prompt requiring user confirmation for destructive or external actions.
- **Secret Management**: Encrypted storage for API keys and tokens (OS keyring or AES-256 encrypted vault).
- **Audit Logging**: Append-only structured logs capturing timestamps, prompts, tool calls, and model decisions.

### 12. Testing, Evaluation & Hardening (Phase 12)
- **Test Automation**: Pytest suite for unit, integration, and mocking harnesses.
- **Agent Evaluation**: Deterministic benchmark test cases evaluating planning accuracy, tool selection, and hallucination resistance.
- **Chaos & Failure Testing**: Network failure resilience, invalid tool response handling, and token limit recovery.
