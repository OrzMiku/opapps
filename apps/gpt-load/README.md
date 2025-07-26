# GPT-Load

一个高性能、企业级的 AI 接口透明代理服务，专门为需要集成多种 AI 服务的企业和开发者设计。采用 Go 语言开发，具备智能密钥管理、负载均衡和完善的监控功能，专为高并发生产环境而设计。

## 功能特性

- **透明代理**: 完全保留原生 API 格式，支持 OpenAI、Google Gemini 和 Anthropic Claude 等多种格式。
- **智能密钥管理**: 高性能密钥池，支持分组管理、自动轮换和故障恢复。
- **负载均衡**: 支持多上游端点的加权负载均衡，提升服务可用性。
- **现代化管理**: 基于 Vue 3 的 Web 管理界面，直观易用。

## 快速开始

通过 Docker Compose 是最推荐的部署方式。

```bash
# 创建目录
mkdir -p gpt-load && cd gpt-load

# 下载配置文件
wget https://raw.githubusercontent.com/tbphp/gpt-load/refs/heads/main/docker-compose.yml
wget -O .env https://raw.githubusercontent.com/tbphp/gpt-load/refs/heads/main/.env.example

# 启动服务
docker compose up -d
```

部署完成后，请访问 `http://<YOUR_SERVER_IP>:3001`。
默认管理密钥为 `sk-123456`，可在 `.env` 文件中修改 `AUTH_KEY`。