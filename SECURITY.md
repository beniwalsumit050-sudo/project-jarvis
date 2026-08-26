# Project JARVIS — Security & Safety Model

This document outlines the security architecture, risk mitigation strategies, human-in-the-loop policies, and vulnerability disclosure guidelines for Project JARVIS.

---

## 1. Security Philosophy

Project JARVIS operates with broad system and integration privileges on the user's behalf. Consequently, security is built into the architecture as an active, preventative guardrail rather than an afterthought.

Core principles:
1. **Least Privilege**: Components and sub-agents operate strictly with the minimum necessary capabilities.
2. **Explicit Human Authorization**: Irreversible, sensitive, or external-facing actions require direct human confirmation.
3. **Local Secret Isolation**: Credentials, OAuth tokens, and private keys never leave local storage unencrypted.
4. **Defense Against Prompt Injection**: Input from untrusted sources (e.g., scraped web content, emails, files) is treated as potentially hostile data.
5. **Auditable Traceability**: Every autonomous decision and tool execution is recorded in an immutable local log.

---

## 2. Risk Classification & Authorization Matrix

Actions within Project JARVIS are assigned one of four risk tiers:

| Risk Tier | Level | Action Types | Default Policy |
| :--- | :--- | :--- | :--- |
| **Tier 0** | Read-Only / Safe Query | Reading local files within workspace, fetching system stats, web searching, querying local memory | **Auto-Approved** |
| **Tier 1** | Controlled Write | Writing to workspace scratchpads, creating temporary files, updating internal memory indices | **Auto-Approved** within designated sandbox |
| **Tier 2** | Sensitive Modification | Executing shell commands, writing/overwriting non-workspace files, creating calendar events, drafting emails | **Confirmation Recommended** / Configurable by user |
| **Tier 3** | Destructive / External | Deleting files, modifying system registry/OS settings, sending live emails, executing unsigned binaries, financial transactions | **Strict Confirmation Required** (Never auto-executed) |

---

## 3. Threat Model & Mitigation Strategies

### 3.1 Prompt Injection & Indirect Injection
- **Threat**: Malicious instructions embedded in emails, web pages, or local files attempting to hijack agent behavior.
- **Mitigations**:
  - Strict separation of system instructions and untrusted content blocks using structured framing.
  - Data-mode sanitization: content fetched from web/email is tagged as untrusted context.
  - Secondary verification: High-risk tool calls triggered during external data processing require explicit re-confirmation.

### 3.2 Path Traversal & Filesystem Tampering
- **Threat**: Erroneous or malicious tool arguments accessing sensitive system directories (e.g., `C:\Windows\System32`, `~/.ssh/`).
- **Mitigations**:
  - Canonical path resolution and boundary verification against an allowed workspace whitelist.
  - Blacklisting protected operating system folders and private credential files (`.env`, `id_rsa`, `*.pem`, `credentials.json`).

### 3.3 Credential & Secret Management
- **Threat**: Exposure of API keys, Google OAuth tokens, or passwords in logs, prompts, or commits.
- **Mitigations**:
  - Secure credential storage using OS keyring mechanisms (Windows Credential Manager / macOS Keychain / SecretService) or AES-256 encrypted vaults.
  - Zero-logging policy: Logging sinks automatically filter and redact recognized token patterns.
  - Environment variables loaded exclusively via secure configuration loaders.

### 3.4 Uncontrolled Autonomous Loops
- **Threat**: Agent entering an infinite recursive tool-calling loop, exhausting API credits or consuming high system resources.
- **Mitigations**:
  - Hard loop limits (maximum turn depth per task).
  - Configurable rate-limiting on tool invocations and API calls.
  - Global emergency stop shortcut / interrupt signal (`Ctrl+C` or voice phrase `"Jarvis Stop"`).

---

## 4. Audit Logging & Compliance

- **Storage**: Append-only local SQLite database and JSONL log files.
- **Payload**: Captures timestamp, session ID, model ID, action risk tier, input parameters (redacted), and outcome status.
- **Integrity**: Log entries are written synchronously before tool execution begins.

---

## 5. Vulnerability Reporting

If you discover a security vulnerability within Project JARVIS:
- **Do not open a public GitHub issue.**
- Submit details privately to the project maintainers via the security channel or designated repository contact.
- Please include steps to reproduce, potential impact, and suggested remediations.
