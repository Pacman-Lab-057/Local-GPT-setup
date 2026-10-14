# Local-GPT-setup

## 1. The Setup

My local AI system consists of three main components.

1. **Ollama:** Runs the AI models on my PC.
2. **Open WebUI:** Provides the ChatGPT-like browser interface.
3. **Docker:** Runs Open WebUI in a container.

The connection works like this:

`Browser → Open WebUI → Ollama → Local AI model`

My browser interface is available at:

http://localhost:3000

The Ollama API is available locally on port `11434`. Open WebUI needs to reach that API through the Docker host gateway.


## 2. Install Ollama

Install Ollama using the official Linux installer:

```
curl -fsSL https://ollama.com/install.sh | sh
```

Verify the installation:

```
ollama --version
```

Check the Ollama service:

```
sudo systemctl status ollama --no-pager
```

To enable automatic startup after reboot:

```
sudo systemctl enable ollama
```

To start Ollama immediately:

```
sudo systemctl start ollama
```

Official website:

https://ollama.com/

## 3. Install and Download AI Models

Download local models using Ollama.

To download Qwen:

```
ollama pull qwen3.5:latest
```

To download Llama:

```
ollama pull llama3.1:latest
```

List the installed models with:

```
ollama list
```

Run a model directly in the terminal:

```
ollama run qwen3.5:latest
```

Or:

```
ollama run llama3.1:latest
```

To exit a model's interactive chat:

```
/bye
```

---

## 4. Install and Start Docker

After install check Docker:

```
docker --version
```

IF encountered an error saying Docker could not connect to:

```
/var/run/docker.sock
```

Fix this by starting Docker:

```
sudo systemctl enable --now docker
```

This enables Docker to start automatically after reboot and starts it immediately.

Verify that Docker is running:

```
sudo systemctl status docker --no-pager
```

Check the containers:

```
sudo docker ps -a
```

Official installation instructions:

https://docs.docker.com/engine/install/

---

## 5. Install Open WebUI

Use this Docker image:

```
ghcr.io/open-webui/open-webui:main
```

I generated a secret key:

```
openssl rand -hex 32
```

Save this key securely.

run:

```
sudo docker run -d \
  --name open-webui \
  --restart unless-stopped \
  -p 127.0.0.1:3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  -e WEBUI_SECRET_KEY='MY_SAVED_SECRET_KEY' \
  ghcr.io/open-webui/open-webui:main
```

Replace `MY_SAVED_SECRET_KEY` with your actual saved key.


Verify Open WebUI

```
sudo docker ps --filter name=open-webui
```

Check the port mapping:

```
sudo docker port open-webui
```

Expected output:

```
8080/tcp -> 127.0.0.1:3000
```

I then open:

http://localhost:3000