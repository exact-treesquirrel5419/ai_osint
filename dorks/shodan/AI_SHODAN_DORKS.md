# Shodan Dorks for AI/ML Infrastructure

> All ports verified against official documentation. Exposure counts from SentinelOne, Cisco Talos, and Censys research (2025-2026).

---

## Self-Hosted LLMs

### Ollama — 175,000+ exposed (SentinelOne/Censys Jan 2026)
```
"Ollama is running" port:11434
port:11434 http.html:"Ollama"
port:11434 "api/tags"
port:11434 http.status:200
```

### vLLM / OpenAI-Compatible (Port 8000)
```
port:8000 "openai" "model"
http.title:"FastAPI" port:8000 "v1/models"
"/v1/chat/completions" port:8000
"/v1/models" port:8000
```

### llama.cpp (Port 8080)
```
port:8080 "llama" "completion"
http.html:"llama.cpp" port:8080
```

### LM Studio (Port 1234)
```
port:1234 "/v1/models"
port:1234 http.html:"lm studio"
```

### GPT4All (Port 4891)
```
port:4891 "/v1/models"
```

### LocalAI (Port 8080)
```
http.title:"LocalAI" port:8080
```

### KoboldCpp (Port 5001)
```
port:5001 "kobold"
http.title:"KoboldAI"
```

---

## AI Agent Gateways — CRITICAL

### OpenClaw / Clawdbot — 4,000+ on Shodan
```
http.title:"Clawdbot Control" port:18789
http.title:"OpenClaw" port:18789
port:18789 "api/v1/status"
port:18789 "auth_mode"
http.html:"tools_loaded" port:18789
port:18789 "agent_id"
```

### Open WebUI / LobeChat / LibreChat
```
http.title:"Open WebUI"
http.title:"LobeChat"
http.title:"LibreChat"
```

### LiteLLM Proxy
```
http.title:"LiteLLM"
port:4000 "litellm"
```

---

## Gradio & Streamlit

### Gradio — 470,000+ apps on HuggingFace Spaces
```
http.title:"Gradio" port:7860
"gradio" port:7860 http.html:"queue"
port:7860 http.status:200
```

### Streamlit
```
http.title:"Streamlit" port:8501
port:8501 "streamlit"
```

### Stable Diffusion WebUI
```
http.title:"Stable Diffusion" port:7860
http.title:"AUTOMATIC1111"
```

### ComfyUI / Fooocus / InvokeAI
```
http.title:"ComfyUI"
http.title:"Fooocus"
http.title:"InvokeAI"
```

---

## Vector Databases

### Qdrant — No auth by default
```
port:6333 "qdrant"
port:6333 "/collections"
http.html:"qdrant" port:6333
port:6334 "qdrant"
```

### Weaviate
```
port:8080 "weaviate"
http.html:"weaviate" port:8080
```

### Milvus
```
port:19530 "milvus"
port:9091 "milvus"
```

### ChromaDB
```
port:8000 "chroma"
http.html:"chromadb" port:8000
```

---

## MLOps

### MLflow — No auth by default, CVE-2026-0545 (CVSS 9.1)
```
http.title:"MLflow" port:5000
port:5000 "/mlflow"
```

### Jupyter — ~10,000+ on Shodan, targeted by botnets
```
http.title:"Jupyter Notebook" port:8888
http.title:"JupyterLab" port:8888
port:8888 "jupyter" -"Authenticated"
```

### Kubeflow / Label Studio / Airflow / TensorBoard
```
http.title:"Kubeflow" port:8080
http.title:"Label Studio"
http.title:"Airflow" "DAGs"
http.title:"TensorBoard" port:6006
```

---

## Filters

### By Country
```
country:"US"
country:"CN"
country:"DE"
country:"IN"
country:"GB"
country:"JP"
country:"KR"
country:"CO"
```

### By Cloud Provider
```
org:"Amazon"
org:"Google"
org:"Microsoft"
org:"DigitalOcean"
org:"Hetzner"
org:"OVH"
```

### Unauthenticated
```
-"401" -"403" -"login" -"password" http.status:200
```
---

## 🆕 MCP Servers & AI Agent Gateways (v1.2.0)

> MCP architecture has systemic RCE — 200,000 vulnerable instances estimated (Ox Security, April 2026).

```
# MCP HTTP/SSE endpoints
http.html:"mcp" "tools" port:3000
http.html:"Model Context Protocol" port:8080
http.html:"mcp_message" port:443

# nginx-ui with MCP (CVE-2026-33032 — CVSS 9.8, actively exploited)
http.title:"Nginx UI" port:443
http.title:"nginx ui" port:9000
http.title:"Nginx UI" "/mcp"
```

---

## 🆕 AI Coding Assistants & IDE Backends (v1.2.0)

```
# Open WebUI (popular Ollama frontend)
http.title:"Open WebUI" port:3000
http.title:"Open WebUI" port:8080

# Flowise (CVE-2026-40933 — RCE via MCP adapters)
http.title:"Flowise" port:3000
http.title:"FlowiseAI"

# LiteLLM Proxy (OpenAI-compatible gateway, LLMjacking target)
http.title:"LiteLLM" port:4000
http.html:"litellm" "proxy" port:4000

# GPT Researcher
http.title:"GPT Researcher"

# Langflow / LangChain backends
http.title:"Langflow" port:7860
```

---

## 🆕 vLLM Servers — LLMjacking Targets (v1.2.0)

> Operation Bizarre Bazaar actively targets vLLM servers and OpenAI-compatible APIs for commercial resale (Pillar Security).

```
http.html:"vLLM" port:8000
"vllm" port:8000 "model"
http.html:"vllm" "/v1/models" port:8000
```

---

## 🆕 DeepSeek-Style ClickHouse Exposure (v1.2.0)

> Wiz Research found DeepSeek's ClickHouse DB exposed with 1M+ log entries including plaintext chat histories and API keys.

```
product:"ClickHouse" port:8123
product:"ClickHouse" port:9000 -"Login"
http.html:"ClickHouse" "play" port:8123
```

---

## 🆕 Additional LLM Inference Servers (v1.2.0)

```
# Text Generation Inference (HuggingFace TGI)
http.html:"text-generation-inference" port:8080
http.html:"tgi" "/generate" port:80

# TabbyAPI / Tabby (self-hosted coding assistant)
http.title:"Tabby" port:8080
http.html:"tabbyAPI" port:5000

# LocalAI
http.title:"LocalAI" port:8080
```
