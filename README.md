# Local-GPT-setup

## 1. The Setup

My local AI system consists of three main components.

1. **Ollama:** Runs the AI models on my PC.
2. **Open WebUI:** Provides the ChatGPT-like browser interface.
3. **Docker:** Runs Open WebUI in a container.

The connection works like this:

`Browser → Open WebUI → Ollama → Local AI model`

My browser interface is available at:

http://localhost:3001

The Ollama API is available locally on port `11434`. Open WebUI needs to reach that API through the Docker host gateway.