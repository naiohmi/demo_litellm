# LiteLLM Demo Project

This project demonstrates a simple setup of a LiteLLM proxy server with Docker support, semantic caching, and UI integration.

## Project Structure

```
.
├── .gitignore           # Git ignore configuration
├── README.MD           # Project documentation
└── docker/            # Docker configurations
    ├── .env.example   # Example environment variables
    ├── config.yaml    # LiteLLM configuration
    ├── docker-compose.yml # Docker compose setup
    └── note.txt      # Additional notes
```

## Configuration

The project uses Docker for containerization with the following key configurations:

### Environment Variables

Copy `docker/.env.example` to `docker/.env` and configure:

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

# Optional: Qdrant and Langfuse Configuration
QDRANT_API_BASE=http://qdrant:6333
LANGFUSE_PUBLIC_KEY=your-key
LANGFUSE_SECRET_KEY=your-secret
LANGFUSE_HOST=your-host
```

### Features

The LiteLLM configuration (`docker/config.yaml`) includes:

- Multiple model support (Azure GPT-4, Azure Embedding, Ollama)
- Semantic caching with Qdrant
- Request monitoring with Langfuse
- Authentication and API key management
- Health checks and monitoring

## User Interfaces

### LiteLLM UI

The built-in LiteLLM UI provides monitoring and management capabilities:

- Access at: `http://localhost:4000/ui`
- Features:
  - Model performance monitoring
  - Request/response logging
  - Cost tracking
  - User management
  - API key creation and management

[Documentation Reference](https://docs.litellm.ai/docs/proxy/ui)

### OpenWebUI Integration

OpenWebUI provides a chat interface for interacting with LLM models:

1. Configuration:
   ```yaml
   # Add to docker-compose.yml
   openwebui:
     image: openwebui/core
     environment:
       - ENDPOINTS=http://litellm:4000
     ports:
       - "3000:3000"
   ```

2. Access at: `http://localhost:3000`
3. Features:
   - Chat interface for all configured models
   - Conversation history
   - Model switching
   - Prompt templates

[OpenWebUI Documentation](https://docs.openwebui.com/getting-started/quick-start)  
[Integration Guide](https://docs.litellm.ai/docs/tutorials/openweb_ui#quickstart)

## Semantic Caching with Qdrant

Qdrant provides vector-based semantic caching for improved response times and reduced API costs:

### Configuration

```yaml
litellm_settings:
  cache: true
  cache_params:
    type: qdrant-semantic
    qdrant_semantic_cache_embedding_model: azure-text-embedding-ada-002
    qdrant_collection_name: semantic_collection
    qdrant_quantization_config: product
    similarity_threshold: 0.7
    mode: default_off
    ttl: 300
```

### Features
- Vector-based similarity search
- Configurable similarity thresholds
- Time-to-live (TTL) for cache entries
- Quantization support for efficiency
- Optional per-request cache control

[Qdrant Configuration Guide](https://docs.litellm.ai/docs/caching/all_caches#initialize-cache---in-memory-redis-s3-bucket-redis-semantic-disk-cache-qdrant-semantic)  
[Qdrant Documentation](https://qdrant.tech/documentation/quickstart/)

## Deployment

### Prerequisites

- Docker
- Docker Compose

### Steps

1. Clone the repository:
```bash
git clone https://github.com/naiohmi/demo_litellm.git
cd demo_litellm
```

2. Set up environment variables:
```bash
cp docker/.env.example docker/.env
# Edit docker/.env with your configuration
```

3. Deploy with Docker Compose:
```bash
cd docker
docker-compose up -d
```

This will start:
- LiteLLM proxy server (`:4000`)
- Qdrant vector database (`:6333`)
- OpenWebUI (`:3000`)
- PostgreSQL database (`:5432`)

## Security Notes

- Always keep your `.env` file secure and never commit it to version control
- The `.gitignore` is configured to exclude sensitive files
- Use strong passwords for UI and API authentication
- Regularly update your API keys and master keys

## Monitoring and Maintenance

- Access the LiteLLM UI at `http://localhost:4000/ui` for monitoring
- Background health checks run every 300 seconds
- Monitor cache performance through Qdrant UI at `http://localhost:6333/dashboard`
- Track API usage and performance via Langfuse
- View logs using `docker-compose logs`