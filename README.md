# my-agent

A lightweight conversational AI agent built with Google Agent Development Kit (ADK) and `agents-cli`, deployed on Google Cloud Agent Platform (Agent Runtime) using the Agent2Agent (A2A) protocol.

---

## What This Agent Does

The agent is powered by **Gemini 3.6 Flash** (`gemini-3.6-flash`) via **Vertex AI**. It processes user prompts and uses custom Python function tools to retrieve simulated real-time data:

- **Weather Inquiry (`get_weather`)**: Returns current weather conditions for queried locations (e.g. San Francisco, New York).
- **Time Retrieval (`get_current_time`)**: Computes and formats the current local time with timezone awareness for queried cities.

### Wired Components & Services

Based on the codebase implementation:
- **LLM / Inference**: Google Vertex AI Gemini (`gemini-3.6-flash`).
- **Framework**: Google Agent Development Kit (ADK) with FastAPI runner and A2A protocol endpoint support (`/a2a/app`).
- **Session Management**: In-memory session service (`InMemorySessionService`).
- **Artifact Handling**: In-memory artifact storage (`InMemoryArtifactService`).

*(Note: Advanced services like Vertex AI Memory Bank, Firestore databases, Cloud Storage buckets, Imagen image generation, and A2UI rich cards are not wired into this initial agent build.)*

---

## Getting Started Locally

### Prerequisites

- Python 3.10+
- [`uv`](https://docs.astral.sh/uv/) or `python3 -m venv`
- [`agents-cli`](https://google.github.io/agents-cli/)
- Google Cloud SDK (`gcloud`) authenticated:
  ```bash
  gcloud auth login
  gcloud auth application-default login
  ```

### Installation

1. Navigate to the project directory:
   ```bash
   cd my-agent
   ```

2. Install dependencies:
   ```bash
   agents-cli install
   ```

### Configuration

Copy or verify `.env`:
```bash
GOOGLE_GENAI_USE_VERTEXAI=true
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=global
```

---

## Running & Testing Locally

### 1. Single Command Prompt Test
Run queries directly from the terminal without keeping a server open:
```bash
agents-cli run "What is the weather in SF?"
```

### 2. Multi-turn Session Test
Keep the local background server running to preserve conversation state across queries:
```bash
# Start server and run initial prompt
agents-cli run "What is the weather in New York?" --start-server

# Follow up using the session ID printed in the previous output
agents-cli run "What about San Francisco?" --session-id <SESSION_ID>

# Stop the background server when finished
agents-cli run --stop-server
```

### 3. Interactive Web Playground
Launch the local ADK developer playground interface:
```bash
agents-cli playground
```
This serves the interactive web chat interface locally for real-time prompt testing and trace inspection.

---

## Deployment

Deploy directly to Agent Platform (Agent Runtime) using `agents-cli`:

```bash
agents-cli deploy -d agent_runtime --no-confirm-project
```

Once deployed, the agent communicates via standard A2A JSON-RPC protocols.
