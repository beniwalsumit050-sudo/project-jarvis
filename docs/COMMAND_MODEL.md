# Project JARVIS — Command & Execution Model

This document specifies the command processing pipeline, input routing, slash command system, and execution safety lifecycle in Project JARVIS.

---

## 1. Command Lifecycle Overview

Every user input or system event undergoes a standardized execution pipeline:

```
[ User Input (CLI / Voice / Web / Scheduler) ]
                       │
                       ▼
         ┌───────────────────────────┐
         │  1. Input Normalizer      │
         └─────────────┬─────────────┘
                       │
         ┌─────────────▼─────────────┐
         │  2. Dispatch Router       │
         └──────┬─────────────┬──────┘
                │             │
   (Slash / Meta Command)     (Natural Language / Intent)
                │             │
                ▼             ▼
  ┌──────────────────┐   ┌───────────────────────────────────┐
  │ System Handler   │   │ AI Brain Orchestrator             │
  │ (/help, /status) │   │ (Context Assembly -> LLM Reasoner)│
  └────────┬─────────┘   └─────────────────┬─────────────────┘
           │                               │
           │                               ▼
           │                     ┌───────────────────┐
           │                     │ Tool Call Plan    │
           │                     └─────────┬─────────┘
           │                               │
           │                               ▼
           │                     ┌───────────────────┐
           │                     │ Safety Gate       │
           │                     │ (Risk Assessment) │
           │                     └─────────┬─────────┘
           │                               │
           │                     ┌─────────┴─────────┐
           │                     │ Requires Confirm? │
           │                     └──┬─────────────┬──┘
           │                    YES │             │ NO
           │                        ▼             │
           │                [ User Prompt ]       │
           │                        │             │
           │                        └──────┬──────┘
           │                               │
           ▼                               ▼
  ┌──────────────────────────────────────────────────┐
  │  3. Execution & Streaming Engine                 │
  └────────────────────────┬─────────────────────────┘
                           │
                           ▼
  ┌──────────────────────────────────────────────────┐
  │  4. Response Formatter & Audit Logger            │
  └──────────────────────────────────────────────────┘
```

---

## 2. Command Types & Channels

### 2.1 Meta / Slash Commands (Direct Execution)
Direct system commands that bypass LLM inference for deterministic speed and configuration:

| Command | Description | Example |
| :--- | :--- | :--- |
| `/help` | Display available commands and active capabilities | `/help` |
| `/status` | View system status, active provider, and memory health | `/status` |
| `/model` | Inspect or switch the active LLM provider/model | `/model gemini-2.5-pro` |
| `/clear` | Clear active working memory / reset conversation | `/clear` |
| `/tools` | List registered tools and their permission levels | `/tools` |
| `/memory` | Query, inspect, or manage long-term memories | `/memory search "API key"` |
| `/plan` | Force explicit plan generation before executing a goal | `/plan "Organize downloads"` |
| `/exit` / `/quit` | Gracefully shut down JARVIS processes and bridges | `/exit` |

### 2.2 Natural Language Invocations
- Conversational queries and goal-oriented instructions parsed by the AI Brain.
- May result in direct textual responses or multi-step tool execution loops.

### 2.3 Scheduled & Background Triggers
- Cron expressions or event hooks (e.g., file change, incoming webhook, system alarm) generating autonomous background tasks.

---

## 3. Tool Execution & Safety Classification

Every tool registered in Project JARVIS is categorized into an explicit risk tier:

```
+-------------------------------------------------------------------------+
| Risk Tier 0: READ-ONLY (Auto-Approved)                                  |
| Examples: file_read, directory_list, web_search, system_info            |
+-------------------------------------------------------------------------+
| Risk Tier 1: SAFE WRITE (Auto-Approved in Workspace)                    |
| Examples: temp_file_create, memory_store, scratchpad_write              |
+-------------------------------------------------------------------------+
| Risk Tier 2: SENSITIVE OPERATION (Requires Confirmation or Safe-Mode)   |
| Examples: shell_exec, git_push, calendar_create_event, send_draft_email |
+-------------------------------------------------------------------------+
| Risk Tier 3: DESTRUCTIVE OPERATION (Strict Human-in-the-Loop Gate)      |
| Examples: file_delete, system_shutdown, format_disk, send_live_email    |
+-------------------------------------------------------------------------+
```

---

## 4. Execution Loop (ReAct / Plan-and-Solve)

1. **Step Assessment**: The AI Brain analyzes the user prompt, workspace context, and available tools.
2. **Tool Selection**: Emits a structured tool call schema with typed parameters.
3. **Safety Verification**: The Safety Gate checks the tool risk tier and user policy.
4. **Execution**: If approved, executes the tool asynchronously with timeout guards.
5. **Observation Ingestion**: The tool's output is captured, truncated if excessive, and fed back into Working Memory.
6. **Continuation / Termination**: Repeats until the final answer or goal condition is reached.

---

## 5. Structured Output & Feedback

- **Streaming Text**: Real-time token streaming to the active interface (CLI/TUI/Voice).
- **Tool Status Indicators**: Visual markers indicating tool execution progress (e.g., `[Running: search_files...]`, `[Success]`, `[Blocked: Awaiting Confirmation]`).
- **Audit Persistence**: Every executed command and tool invocation writes an immutable record to the audit log.
