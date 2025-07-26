# GPT-Load

A high-performance, enterprise-grade AI API transparent proxy service designed for developers and businesses integrating multiple AI services. Built with Go, it features intelligent key management, load balancing, and comprehensive monitoring for high-concurrency production environments.

## Features

- **Transparent Proxy**: Preserves native API formats, supporting services like OpenAI, Google Gemini, and Anthropic Claude.
- **Intelligent Key Management**: A high-performance key pool with features like automatic rotation and failure recovery.
- **Load Balancing**: Weighted load balancing across multiple upstream endpoints.
- **Modern Management**: An intuitive web interface built with Vue 3.

## Quick Start

Using Docker Compose is the recommended way to deploy.

```bash
# Create a directory
mkdir -p gpt-load && cd gpt-load

# Download configuration files
wget https://raw.githubusercontent.com/tbphp/gpt-load/refs/heads/main/docker-compose.yml
wget -O .env https://raw.githubusercontent.com/tbphp/gpt-load/refs/heads/main/.env.example

# Start the service
docker compose up -d
```

After deployment, access the admin panel at `http://<YOUR_SERVER_IP>:3001`.
The default admin key is `sk-123456`, which can be changed by editing `AUTH_KEY` in the `.env` file.