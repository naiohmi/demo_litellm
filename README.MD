# LiteLLM Demo Project

This project demonstrates a simple setup of a LiteLLM proxy server with Docker support.

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

This will start the LiteLLM proxy server along with its required services (Qdrant, PostgreSQL, etc.).

## Security Notes

- Always keep your `.env` file secure and never commit it to version control
- The `.gitignore` is configured to exclude sensitive files
- Use strong passwords for UI and API authentication
- Regularly update your API keys and master keys

## Monitoring and Maintenance

- Background health checks run every 300 seconds
- Logs are available through Docker logs
- Monitor cache performance through Qdrant
- Track API usage and performance via Langfuse