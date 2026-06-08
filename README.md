# Multi-Agent Conversation for Disease Diagnosis

[![Research Preprint](https://img.shields.io/badge/Preprint-Research%20Square-blue?style=for-the-badge&logo=researchgate&logoColor=white)](https://www.researchsquare.com/article/rs-3757148/v1)
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Autogen](https://img.shields.io/badge/Autogen-0.2.32-orange?style=for-the-badge)](https://microsoft.github.io/autogen/)
[![GPT-4](https://img.shields.io/badge/GPT--4-OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> **One is Not Enough** — A multi-agent LLM conversation framework that significantly enhances rare disease diagnostic capabilities by orchestrating structured debates among specialist AI agents.

---

## Abstract

This repository presents a novel multi-agent conversation framework designed to enhance the capabilities of Large Language Models (LLMs) in diagnosing complex and rare diseases. Structured under Microsoft's **Autogen** framework, the system orchestrates in-depth conversations among multiple LLM-based specialist agents, enabling collaborative reasoning that mimics a real-world clinical consultation panel.

Our approach was validated on **302 curated disease cases** (primary and follow-up consultations), demonstrating measurable improvements in diagnostic accuracy over single-agent LLM baselines — particularly for rare conditions where a single model tends to anchor on common diagnoses.

The full study is published as a preprint on Research Square:
**[One is Not Enough: Multi-Agent Conversation Framework Enhances Rare Disease Diagnostic Capabilities of Large Language Models](https://www.researchsquare.com/article/rs-3757148/v1)**

---

## How It Works

The framework simulates a clinical consultation panel where multiple AI "doctor" agents collaborate to reach a diagnosis:

1. **Case Intake** — A patient case (symptoms, history, test results) is presented to the multi-agent system.
2. **Specialist Agents** — Multiple LLM-based doctor agents, each optionally assigned a clinical specialty, independently analyze the case.
3. **Structured Debate** — Agents engage in multi-turn conversation, proposing, challenging, and refining differential diagnoses.
4. **Supervisor Agent** — An optional supervisor agent moderates the discussion, resolves conflicts, and synthesizes the final diagnosis.
5. **Output** — The system produces a final diagnosis along with the full reasoning trail saved to `outputs/`.

**Key configurable parameters:**
- Number of doctor agents (scale the panel size)
- Whether to include a supervisor agent
- Case-specific clinical specialty assignments per agent
- Underlying LLM model (GPT-4, Claude, Gemini, LLaMA 3.1, etc.)

---

## Architecture

```
Patient Case Input
       │
       ▼
┌─────────────────────────────────────────┐
│           Autogen Orchestrator          │
│                                         │
│  ┌──────────┐  ┌──────────┐  ┌───────┐ │
│  │ Doctor 1 │  │ Doctor 2 │  │  ...  │ │
│  │(Specialty│  │(Specialty│  │       │ │
│  │    A)    │  │    B)    │  │       │ │
│  └────┬─────┘  └────┬─────┘  └───┬───┘ │
│       │             │            │      │
│       └─────────────┼────────────┘      │
│                     │                   │
│              ┌──────▼──────┐            │
│              │  Supervisor │ (optional) │
│              │    Agent    │            │
│              └──────┬──────┘            │
└─────────────────────┼───────────────────┘
                      │
                      ▼
              Final Diagnosis + Reasoning
```

Multi-Agent Conversation Flow:

![Conversation Flow](https://github.com/satapathyPro/multi-agent-conversation-for-disease-diagnosis/assets/148701415/35758db3-30b8-487d-83f6-1d8640e9ec38)

---

## Test Dataset

302 disease cases were retrieved and curated as primary and follow-up consultations to test the effectiveness of LLMs in actual clinical scenarios.

![Figure 2](https://github.com/satapathyPro/multi-agent-conversation-for-disease-diagnosis/assets/148701415/8762cb39-adaf-42a9-b123-9aef73e578bc)

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Agent Orchestration | [Microsoft Autogen](https://microsoft.github.io/autogen/) v0.2.32 |
| Primary LLM | GPT-4o (OpenAI) |
| Alternative LLMs | Claude, Gemini, LLaMA 3.1 (via LiteLLM/Ollama) |
| Language | Python 3.8+ |
| Environment | Anaconda |

---

## Setup

### Prerequisites

- [Anaconda](https://www.anaconda.com/distribution/) or Miniconda
- Python 3.8+
- An OpenAI API key (or compatible provider)

### Installation

```bash
# 1. Create and activate a conda environment
conda create --name mac python=3.8
conda activate mac

# 2. Install Autogen
pip install pyautogen==0.2.32
```

### Configuration

Set your API credentials in **`configs/config_list.json`**:

```json
[
  {
    "model": "gpt-4o",
    "api_key": "<your-api-key>",
    "base_url": "",
    "tags": ["x_gpt4o"]
  }
]
```

To use locally deployed models via [LiteLLM](https://microsoft.github.io/autogen/docs/topics/non-openai-models/local-litellm-ollama) + [Ollama](https://ollama.com/):

```json
[
  {
    "model": "llama3.1",
    "api_key": "NotRequired",
    "base_url": "http://0.0.0.0:4000",
    "tags": ["llama3.1"]
  }
]
```

### Running

All commands should be run from the project root directory.

**Run inference (training/inference):**
```bash
sh scripts/train.sh
```

**Run evaluation:**
```bash
bash scripts/eval.sh
```

Results are saved to `outputs/`.

### Runtime Estimate

A single case takes approximately **5–10 minutes** to process, depending on system specifications, network conditions, and the number of agents configured.

---

## Updates (2024-08-26)

- You may vary the number of doctor agents in the panel.
- You may exclude the supervisor agent for lightweight runs.
- You may assign case-specific clinical specialties to individual doctor agents.
- You may swap the base model of the framework to any supported LLM.

---

## Research Citation

If you use this framework in your research, please cite the preprint:

```bibtex
@article{satapathy2024multiagent,
  title     = {One is Not Enough: Multi-Agent Conversation Framework Enhances
               Rare Disease Diagnostic Capabilities of Large Language Models},
  author    = {Satapathy, Subham and others},
  journal   = {Research Square},
  year      = {2024},
  doi       = {10.21203/rs.3.rs-3757148/v1},
  url       = {https://www.researchsquare.com/article/rs-3757148/v1},
  note      = {Preprint}
}
```

Preprint link: [https://www.researchsquare.com/article/rs-3757148/v1](https://www.researchsquare.com/article/rs-3757148/v1)

---

## Contributing

Contributions are welcome and greatly appreciated. If you have suggestions for improvements, encounter issues, or wish to contribute, please feel free to open an issue or submit a pull request. This project is actively maintained.

---

## About the Maintainer

This project is actively maintained by **Subham Satapathy**, a Software Engineer with extensive experience building cloud-scale distributed systems and developing agentic AI automation, including LLM-orchestrated workflows. Subham's expertise spans Python, Scala, Java, and various AI/ML frameworks including PyTorch and TensorFlow, with a strong focus on robust architecture and production-ready systems.

- **GitHub:** [https://github.com/satapathyPro](https://github.com/satapathyPro)
- **LinkedIn:** [https://www.linkedin.com/in/subhamUMD/](https://www.linkedin.com/in/subhamumd/)
- **Email:** satapathypro@gmail.com
