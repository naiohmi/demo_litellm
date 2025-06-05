# LiteLLM Demo Project

This project demonstrates the setup of a LiteLLM proxy server with multiple model configurations, semantic caching using Qdrant, and monitoring with Langfuse.

## Features

- Multiple model support (Azure GPT-4, Azure Embedding, Ollama)
- Semantic caching with Qdrant
- Request monitoring and logging with Langfuse
- Authentication with master key
- Web UI for management
- Docker containerization

## Project Structure

```
.
├── .env.example          # Example environment variables
├── config.yaml           # LiteLLM configuration
├── docker-compose.yml    # Docker compose configuration
└── docker/              # Docker-specific configurations
    ├── .env.example
    ├── config.yaml
    └── docker-compose.yml
```

## Environment Setup

1. Copy `.env.example` to `.env` and configure the following variables:

### Required Environment Variables

```env
# Azure OpenAI Configuration
AZURE_API_BASE=your-azure-endpoint
AZURE_OPENAI_API_KEY=your-gpt4-key
AZURE_API_KEY_TEXT_EMBEDDING_ADA_002=your-embedding-key

# Authentication
LITELLM_MASTER_KEY=your-master-key
UI_USERNAME=your-username
UI_PASSWORD=your-password

# Database Configuration
DATABASE_URL=postgresql://user:pass@host:5432/dbname
```

### Optional Environment Variables

```env
# Qdrant Configuration
QDRANT_API_BASE=http://qdrant:6333

# Langfuse Monitoring
LANGFUSE_PUBLIC_KEY=your-key
LANGFUSE_SECRET_KEY=your-secret
LANGFUSE_HOST=your-host
```

## Configuration

The `config.yaml` file contains:

- Model configurations (Azure GPT-4, Embedding model, Ollama)
- Caching settings with Qdrant
- Logging callbacks (Langfuse)
- Network and timeout settings
- Fallback configurations

## Deployment

### Using Docker Compose

1. Ensure Docker and Docker Compose are installed
2. Set up your environment variables in `.env`
3. Run the stack:

```bash
docker-compose up -d
```

This will start:
- LiteLLM proxy server
- Qdrant vector database
- PostgreSQL database
- Ollama instance (if configured)

### Health Checks

The system performs background health checks every 300 seconds on configured models.

## Security

- API authentication using master key
- User API key redaction in logs
- Web UI authentication
- Optional IPv4 enforcement

## Monitoring

- Request success/failure logging
- Semantic cache hit tracking
- Custom tags for Langfuse monitoring
- Request timeout monitoring

## Cache Configuration

The system uses Qdrant for semantic caching with:
- Similarity threshold: 0.7
- Default cache TTL: 300 seconds
- Cache can be enabled/disabled per request