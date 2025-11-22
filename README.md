# LLM Vulnerability Scanner

A comprehensive security assessment framework for testing local Large Language Models (LLMs) against over 25 categories of adversarial attack vectors, including prompt injection, jailbreaks, data leakage, model inversion, encoding attacks, and more.

> **Status:** Alpha — under active development. Core functionality is stable, but interfaces and outputs may change. Contributions and feedback welcome.


## Features

- Tests 25+ known classes of LLM vulnerabilities
- Targets local LLM APIs (LM Studio, Ollama, custom OpenAI-compatible endpoints)
- Genetic algorithm for jailbreak optimization
- Automatic fallback logic for API compatibility detection
- Confidence scoring per test
- Severity classification (LOW, MEDIUM, HIGH, CRITICAL)
- HTML and JSON report output
- Resume mode with checkpoint saving
- Plugin loader for community attack modules
- Modular, extensible architecture
- CVSS-like scoring logic per vulnerability

## Attack Categories

| #  | Category                   | Description                                                             |
|----|----------------------------|-------------------------------------------------------------------------|
| 1  | Prompt Injection           | Contextual and direct prompt override                                   |
| 2  | Jailbreak                  | DAN prompts, developer modes, roleplay attacks                          |
| 3  | Training Data Extraction   | Memorization, key leakage, and pattern probing                          |
| 4  | Model Inversion            | Reconstructing sensitive or representative data                         |
| 5  | Membership Inference       | Probing whether specific data was part of training                      |
| 6  | Prompt Leakage             | Revealing system and internal instructions                              |
| 7  | Parameter Extraction       | Disclosing model architecture and internals                             |
| 8  | Context Poisoning          | State manipulation through multi-turn input                             |
| 9  | Token Manipulation         | Bypasses using homoglyphs, zero-width chars, and Unicode manipulation   |
| 10 | Encoding Attacks           | Obfuscated attacks using Base64, ROT13, hex, binary, etc.               |
| 11 | Roleplay Escape            | Escaping constraints via character simulation                           |
| 12 | Multimodal Attack          | Simulated non-text input attacks (image/audio contexts)                 |
| 13 | Function Calling Abuse     | Misusing structured tool-calling or function interfaces                 |
| 14 | Self-Replication           | LLM guidance on reproducing itself or similar models                    |
| 15 | Adversarial Examples       | Input patterns designed to mislead classification or output             |
| 16 | Data Exfiltration          | Prompt techniques to leak data from model memory/context                |
| 17 | Safety Alignment           | Testing whether model adheres to ethical constraints                    |
| 18 | Guardrail Effectiveness    | Bypassing built-in model safety mechanisms                              |
| 19 | Adversarial Robustness     | Variants of malicious prompts with obfuscation (e.g. "H4t3")            |
| 20 | Compliance Testing         | GDPR/HIPAA/data protection law violations                               |
| 21 | Toxicity Generation        | Generating hate speech, threats, or offensive content                   |
| 22 | Hallucination              | Making up incorrect or future “facts”                                   |
| 23 | Disinformation             | Generating falsehoods without correcting them                           |
| 24 | PII Disclosure             | Revealing real or synthetic personal identifiable information           |


--------------------
Usage
--------------------
Run a full scan across all categories:
python llm_vuln_scanner.py --host localhost --port 1234 --path /v1

Reports are automatically generated as:
llm_pentest_report_<timestamp>.json    (machine-readable)
llm_pentest_report_<timestamp>.html    (human-readable)

--------------------
Output Format
--------------------
Each vulnerability result includes:
- Vulnerability category
- Test name
- Severity level (LOW, MEDIUM, HIGH, CRITICAL)
- Confidence score (0.0 to 1.0)
- Evidence strings (indicators, matches)
- Prompt and response preview
- Timestamp

--------------------
Requirements
--------------------
- Python 3.8 or higher
- A locally hosted LLM with one or more of these API endpoints:
  /v1/chat/completions
  /api/chat
  /api/v1/generate

Install requirements:
pip install -r requirements.txt

--------------------
Modular Design
--------------------
The scanner is fully modular:
- Each attack category is implemented as a separate method
- New categories can be added by extending `AdvancedLLMVulnerabilityScanner` and registering via plugin loader
- Plugins must implement a callable `run()` and return `TestResult` objects
- Analysis functions (e.g., leakage detection, scoring) are isolated for reuse and expansion

--------------------
Resume Capability
--------------------
If a scan is interrupted or stopped, rerun it with:
python llm_vuln_scanner.py --resume

Checkpoints are saved automatically to `checkpoint.pkl`

--------------------
Plugin System
--------------------
Drop plugin modules into `plugins/` folder.
Each must implement a function or class with:
- category (AttackCategory Enum)
- run_attack(model_client) -> List[TestResult]

Plugins can be loaded with:
python llm_vuln_scanner.py --plugins

--------------------
TODO / Roadmap
--------------------

Selective category scanning via CLI:
python llm_vuln_scanner.py --category prompt_injection,jailbreak

Parallel execution support using ThreadPoolExecutor or asyncio

Unit test suite with pytest integration

Automatic raw prompt/response logging to conversations/ directory

JSON schema validation of report structure

Docker support for isolated environments with scanner + LLM preinstalled

Plugin system for community-contributed attack modules

Replay mode for failed prompts or repeated testing

Support for commercial/remote LLMs with `--remote` flag (disables high-risk attacks)

Adversarial fuzzing mode for prompt mutation

CLI-based attack toggles: `--no-encoding`, `--only-high`, etc.

Hallucination classification integration

Prompt reflection detection analysis

Adversarial pattern corpus loading from external files

Multi-model comparative analysis

Prompt obfuscation testing via spacing, delimiter abuse, malformed structure

Automated scoring report export to CVSS-like structure

Chain-of-Thought consistency testing under stress prompts

Long-context boundary tests (context overflow behavior)

Translation-based jailbreak probes

Tool-Use specific safety bypass testing

Data persistence over multi-turn test cases

Safety refusal consistency tests across repeated phrasing

Cross-turn state leakage analysis

Instruction injection via multi-modal context simulation

--------------------------------
License
--------------------------------
This project is licensed under the MIT License.

--------------------------------
Disclaimer
--------------------------------
This tool is designed exclusively for **ethical research and local model testing**. Do not use this against commercial APIs or externally hosted LLMs without explicit permission.  
Violating Terms of Service or testing boundaries on third-party systems may carry legal risk or ethical concerns.
