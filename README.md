# Azure-Foundry-RAG-Agent
# Orion Digital Labs — Foundry Agent Python Client

A Python-based knowledge assistant built with **Microsoft Foundry Agents**, **Foundry IQ**, and **Azure AI Search**.

The project demonstrates how to connect a Python application to a Microsoft Foundry Agent and enable grounded question answering over internal company documents using a Retrieval-Augmented Generation (RAG) architecture.

---

## Overview

**Orion Digital Labs** is a fictional company created for this project.

The assistant allows users to ask questions about internal company policies, IT security guidelines, and AI project guidelines.

Instead of relying only on the language model's general knowledge, the agent retrieves relevant information from a connected knowledge base and generates grounded responses with source citations when available.

### Example

**User:**

> What are the mandatory stages of an AI project at Orion Digital Labs?

**Agent:**

> Every AI/ML project must pass through seven mandatory stages:
>
> 1. Requirements definition
> 2. Data collection & sourcing
> 3. Model development
> 4. Rigorous evaluation
> 5. Security review
> 6. Production deployment
> 7. Continuous monitoring

The response is grounded in the connected Orion Digital Labs knowledge sources.

---

## Architecture

```text
┌──────────────────────┐
│   Python Terminal    │
│      User Query      │
└──────────┬───────────┘
           │
           ▼
┌────────────────────────────┐
│   Microsoft Foundry        │
│          Agent             │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│        Foundry IQ          │
│      Knowledge Base        │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│     Azure AI Search        │
│  Vectorized Search Index   │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│      Orion Documents       │
│                            │
│ • Company Policies         │
│ • IT Security Guidelines   │
│ • AI Project Guidelines    │
└────────────────────────────┘

             ▲
             │
      Grounded Answer
      + Source Citations
```

---

## Key Features

* **Microsoft Foundry Agent integration**
* **Foundry IQ Knowledge Base**
* **Azure AI Search integration**
* Retrieval-Augmented Generation (RAG)
* Vector-based document retrieval
* Grounded responses using internal knowledge sources
* Source citations in generated responses
* Multi-turn conversations
* Python terminal interface
* Azure authentication using `DefaultAzureCredential`
* Environment-based configuration
* Hallucination-aware agent instructions

---

## Technology Stack

| Technology                    | Purpose                               |
| ----------------------------- | ------------------------------------- |
| Python                        | Application development               |
| Microsoft Foundry             | Agent development and hosting         |
| Foundry IQ                    | Knowledge base and grounded retrieval |
| Azure AI Search               | Document retrieval and vector search  |
| Azure OpenAI / Foundry Models | Language model inference              |
| `text-embedding-3-small`      | Text embeddings                       |
| Azure Identity                | Authentication                        |
| `azure-ai-projects`           | Foundry Python SDK                    |
| `python-dotenv`               | Environment configuration             |

---

## Knowledge Sources

The knowledge base contains three fictional Orion Digital Labs documents:

### 1. Company Policies

Contains general company policies and internal procedures.

### 2. IT Security Guidelines

Contains internal security requirements and guidelines.

### 3. AI & Machine Learning Project Guidelines

Contains the organization's AI/ML project lifecycle and required project stages.

These documents are stored in Azure Blob Storage and indexed through Azure AI Search.

---

## How It Works

### 1. User submits a question

The user enters a question through the Python terminal.

```text
You: What are the mandatory stages of an AI project?
```

### 2. Python connects to the Foundry Agent

The application uses `AIProjectClient` and Azure identity authentication to connect to the Microsoft Foundry project.

### 3. The Agent processes the request

The Foundry Agent receives the question and uses its connected Knowledge Base when company-specific information is required.

### 4. Foundry IQ retrieves relevant information

Foundry IQ uses the connected Azure AI Search index to retrieve relevant document content.

### 5. The Agent generates a grounded answer

The retrieved information is provided to the agent so it can generate an answer based on the available Orion Digital Labs knowledge sources.

### 6. Sources are cited

When available, the generated response includes references to the source documents used to support the answer.

---

## Agent Grounding

The agent is configured to prioritize the connected Knowledge Base when answering company-specific questions.

It is explicitly instructed to:

* Use the Knowledge Base as the primary source for Orion-specific information.
* Avoid inventing company policies or procedures.
* Clearly state when requested information cannot be found.
* Distinguish company-specific information from general model knowledge.
* Provide source citations when available.

This helps reduce unsupported answers and demonstrates a basic approach to **grounded AI applications**.

---

## Example: Supported Question

```text
You: What are the mandatory stages of an AI project at Orion Digital Labs?
```

The agent retrieves the relevant information from:

```text
Orion_AI_Project_Guidelines.pdf
```

and returns a grounded response with the relevant source citation.

---

## Example: Unsupported Question

```text
You: What is Orion Digital Labs' salary increase policy?
```

If the connected knowledge sources do not contain a salary increase policy, the agent responds that the information could not be found instead of fabricating a policy.

This demonstrates how the application handles information that is not available in the knowledge base.

---

## Project Structure

```text
orion-foundry-agent/
│
├── src/
│   └── main.py
│
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

### `src/main.py`

Contains the Python application responsible for:

* Loading configuration
* Authenticating with Azure
* Connecting to Microsoft Foundry
* Connecting to the Foundry Agent
* Creating conversations
* Sending user questions
* Displaying agent responses

### `.env.example`

Provides the required environment variable structure without exposing credentials or project-specific secrets.

### `requirements.txt`

Contains the Python dependencies required to run the project.

---

## Setup

### Prerequisites

Before running the project, make sure you have:

* Python 3.10+
* An Azure subscription
* A Microsoft Foundry project
* A configured Foundry Agent
* A Foundry IQ Knowledge Base
* An Azure AI Search resource
* Azure authentication configured locally

---

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/orion-foundry-agent.git

cd orion-foundry-agent
```

---

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

**macOS / Linux**

```bash
source .venv/bin/activate
```

**Windows**

```bash
.venv\Scripts\activate
```

---

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

### 4. Configure environment variables

Create a `.env` file based on `.env.example`.

```env
PROJECT_ENDPOINT=your-foundry-project-endpoint
AGENT_NAME=your-agent-name
```

Do not commit `.env` to GitHub.

---

### 5. Authenticate with Azure

This project uses:

```python
DefaultAzureCredential()
```

You can authenticate locally using the Azure CLI:

```bash
az login
```

Your Azure identity must have the required permissions to access the Foundry project and Agent.

---

### 6. Run the application

```bash
python src/main.py
```

The application will start a terminal-based conversation:

```text
============================================================
ORION DIGITAL LABS
Knowledge Assistant
============================================================

Commands:
  quit   → Exit the application
  new    → Start a new conversation

Ask a question about Orion Digital Labs.

You:
```

---

## Security

Sensitive configuration is stored in environment variables.

The following files and directories should **not** be committed:

```text
.env
.venv/
__pycache__/
*.pyc
.DS_Store
```

The repository only contains the application code and configuration templates required to reproduce the project.

---

## What This Project Demonstrates

This project demonstrates practical experience with:

* AI agent development
* Retrieval-Augmented Generation (RAG)
* Knowledge-grounded AI
* Vector search
* Azure AI Search
* Microsoft Foundry
* Foundry IQ
* Python SDK integration
* Azure authentication
* Multi-turn conversations
* Source attribution
* Environment-based application configuration

It also demonstrates the integration of multiple Azure AI services into a working application rather than using an LLM API as a standalone chatbot.

---

## Future Improvements

Potential extensions include:

* Structured citation display
* Conversation history management
* Streaming responses
* Additional knowledge sources
* Document upload workflows
* Authentication and user access control
* Web-based interface
* Evaluation of retrieval and answer quality
* Monitoring and observability

---

## Disclaimer

Orion Digital Labs and all associated documents used in this project are fictional and were created for demonstration and learning purposes.

The project is not affiliated with or endorsed by Microsoft.

---

## Author

**Haneen Hasan Alamri**

AI Engineer | Artificial Intelligence

GitHub: `haneenhasan10`

LinkedIn: `haneen-alamri-298290280`
