# Minty Desktop AI

A local-first desktop automation agent that runs AI models on your own machine — including quantised models served by [LM Studio](https://lmstudio.ai) — to control Windows applications without sending your data to a cloud API.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Python Version](https://img.shields.io/badge/Python-3.10%20%7C%203.11-blue)
[![Local LLM](https://img.shields.io/badge/LLM-LM%20Studio-green)](https://lmstudio.ai)

---

## What is Minty Desktop AI?

Minty Desktop AI is a desktop automation agent for Windows. It understands your task in plain language, inspects the screen, and drives applications (Word, Excel, PowerPoint, web browsers, shell, and more) to complete it — all using a language model running locally on your PC.

Because the model runs locally (for example, a quantised GGUF model in LM Studio), your screen content, prompts, and task data never leave your machine unless you choose to connect to a cloud provider.

## Key Features

- **Local LLM support**: Run quantised models through LM Studio's OpenAI-compatible server (default `http://localhost:1234/v1`).
- **Cloud provider support**: OpenAI, Azure OpenAI, and other OpenAI-compatible endpoints.
- **Windows automation**: Native UI Automation (UIA), Win32, and WinCOM control of Word, Excel, PowerPoint, browsers, and the shell.
- **Hybrid actions**: Combine GUI clicks with direct API calls for efficient, reliable execution.
- **Multi-app workflows**: Plan and execute tasks across multiple applications in one session.
- **Extensible**: Add custom actions, automators, and device agents.

## Quick Start

### 1. Install

```powershell
# Clone the repository
git clone https://github.com/parshborganve23-collab/minty-desktop-ai.git
cd minty-desktop-ai

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure the LLM

Copy the agent config template and fill in your settings:

```powershell
copy config\ufo\agents.yaml.template config\ufo\agents.yaml
```

Then edit `config\ufo\agents.yaml` and set the `API_BASE`, `API_KEY`, and `API_MODEL` for your chosen provider.

### 3. Run

```powershell
python -m ufo --task "your task description here"
```

## Running with a local quantised model (LM Studio)

Minty Desktop AI works out of the box with models served locally by LM Studio. LM Studio exposes an OpenAI-compatible API, so no code changes are needed — just point the agent at `http://localhost:1234/v1`.

### Setup steps

1. **Install LM Studio** from [lmstudio.ai](https://lmstudio.ai).
2. **Load a quantised model** (e.g. `Qwen2.5-7B-Instruct-Q4_K_M`, `Llama-3.1-8B-Instruct-Q4_K_M`, or similar GGUF).
3. **Start the local server**: In LM Studio, go to **Local Server**, select your model, and click **Start Server**. Note the endpoint (default `http://localhost:1234/v1`).
4. **Configure Minty Desktop AI**: Edit `config\ufo\agents.yaml`:

```yaml
API_TYPE: "openai"
API_BASE: "http://localhost:1234/v1/chat/completions"
API_KEY: "lm-studio"
API_MODEL: "Qwen2.5-7B-Instruct-Q4_K_M"   # match the model name in LM Studio
API_VERSION: "v1"
```

5. **Verify the connection** with the included helper script:

```powershell
python scripts/lm_studio_check.py --model "Qwen2.5-7B-Instruct-Q4_K_M"
```

A full configuration template is available at `config\ufo\lm_studio.yaml.template`.

### Choosing a quantised model

Pick a quantisation that fits your available memory:

| Model | Quantisation | Approx. VRAM | Best for |
|-------|--------------|--------------|----------|
| Qwen2.5-7B-Instruct | Q4_K_M | ~5 GB | General desktop tasks |
| Llama-3.1-8B-Instruct | Q4_K_M | ~5.5 GB | Strong reasoning |
| Qwen2.5-3B-Instruct | Q4_K_M | ~2.5 GB | Lightweight, fast |

Smaller quantisations (Q4_K_M, Q5_K_M) offer a good balance of speed and intelligence for desktop automation.

## Configuration

Configuration lives under `config/ufo/`:

- `agents.yaml.template` — LLM provider settings for each agent (host, app, backup, evaluation).
- `system.yaml` — Execution limits, timeouts, safety, and logging.
- `lm_studio.yaml.template` — Example local LM Studio configuration.
- `mcp.yaml` — Model Context Protocol server definitions.

Never commit `agents.yaml` (it contains API keys). It is git-ignored; always edit the local copy.

## Project Structure

```
minty-desktop-ai/
├── ufo/                # Desktop AgentOS core (agents, automator, LLM clients)
├── galaxy/             # Multi-device orchestration (Galaxy framework)
├── config/             # YAML configuration templates
├── model_worker/       # Vision-language model workers (LLaVA, CogAgent)
├── learner/            # Experience and knowledge substrate
├── record_processor/   # Demonstration record parsing
├── tests/              # Unit and integration tests
├── scripts/            # Helper scripts (e.g. lm_studio_check.py)
├── documents/          # Guides and API documentation
├── assets/             # Logos and diagrams
└── README.md
```

## Documentation

- [`ufo/README.md`](./ufo/README.md) — Desktop AgentOS usage guide
- [`galaxy/README.md`](./galaxy/README.md) — Multi-device Galaxy framework
- `documents/` — Detailed guides (MCP servers, device setup, advanced features)

## Contributing

We welcome contributions! See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for guidelines on reporting bugs, suggesting features, and submitting pull requests.

## License

This project is licensed under the [MIT License](LICENSE).

## Disclaimer

See [`DISCLAIMER.md`](./DISCLAIMER.md). By using this software you acknowledge and agree to its terms.

---

Built for local, private desktop automation. Run your own model. Keep your data on your machine.
