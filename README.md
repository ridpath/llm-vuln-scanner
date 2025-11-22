# LLM Vulnerability Scanner

A comprehensive security assessment framework for testing local Large Language Models (LLMs) against 16 categories of adversarial attack vectors, including prompt injections, jailbreaks, training data leakage, and more.

## Features

- Tests 16 known classes of LLM vulnerabilities
- Designed for local LLM APIs (e.g., LM Studio, Ollama)
- Automatic fallback logic for API compatibility
- Confidence scoring per vulnerability
- Severity classification (LOW, MEDIUM, HIGH, CRITICAL)
- Evidence collection for each test
- JSON and HTML reporting
- Modular and extensible architecture

## Attack Categories

| # | Category | Description |
|---|----------|-------------|
| 1 | Prompt Injection | Contextual and direct prompt override |
| 2 | Jailbreak | DAN prompts, developer modes, roleplay attacks |
| 3 | Training Data Extraction | Memorization, key leakage, and pattern probing |
| 4 | Model Inversion | Reconstructing sensitive or representative data |
| 5 | Membership Inference | Probing whether specific data was part of training |
| 6 | Prompt Leakage | Revealing system and internal instructions |
| 7 | Parameter Extraction | Disclosing model architecture and internals |
| 8 | Context Poisoning | State manipulation through multi-turn input |
| 9 | Token Manipulation | Bypasses using homoglyphs, zero-width chars, etc. |
| 10 | Encoding Attacks | Obfuscated attacks using Base64, ROT13, hex, etc. |
| 11 | Roleplay Escape | Escaping constraints via persona simulation |
| 12 | Multimodal Attacks | Simulated instructions in non-textual context |
| 13 | Function Calling Abuse | Tooling misuse and code execution attempts |
| 14 | Self-Replication | Requests for architecture cloning or reproduction |
| 15 | Adversarial Examples | Misclassification or prompt-specific attacks |
| 16 | Data Exfiltration | Strategies to leak sensitive information |

## Usage

```bash
python llm_vuln_scanner.py --host localhost --port 1234 --path /v1
This will initiate a full scan across all 16 categories. Reports are automatically generated in:

llm_pentest_report_<timestamp>.json (machine-readable)

llm_pentest_report_<timestamp>.html (human-readable)
```
## Output Structure

Each finding includes:

Vulnerability category

Test name

Severity level

Model confidence score

Evidence strings

Prompt and response previews

Timestamp

## Requirements

Python 3.8 or higher

A locally hosted LLM API compatible with:

/v1/chat/completions

/api/chat

/api/v1/generate


Install dependencies:

pip install -r requirements.txt

## TODO / Roadmap

The following features are planned or under consideration for future versions:

 Selective category scanning via CLI
Example usage:

python llm_vuln_scanner.py --category prompt_injection,jailbreak


 Parallel execution support
Utilize concurrent.futures.ThreadPoolExecutor or asyncio to run attacks concurrently, reducing scan time.

 Unit test suite with pytest integration
Test category logic and result classification mechanisms.

 Automatic raw prompt/response logging
Save all prompt-response pairs in conversations/ directory for traceability and forensic use.

 Report schema validation
Define and validate report output using JSON Schema for programmatic parsing and API integration.

 Docker support
Provide a containerized version with LM Studio and scanner pre-installed for isolated test environments.

 Modular plugin interface
Add support for plugin-based vulnerability categories, allowing researchers to contribute new attack modules.

 Support for commercial or remote LLM APIs (with caution flags)
Add a --remote flag that disables certain attack categories for compliance.

 Replay mode
Allow previously failed prompts to be re-tested in a standalone mode.

## License

This project is licensed under the MIT License.

## Disclaimer

This tool is intended strictly for local LLM testing and research purposes. Do not use this against third-party APIs or hosted models without explicit permission. Unauthorized use may violate Terms of Service or legal boundaries.
