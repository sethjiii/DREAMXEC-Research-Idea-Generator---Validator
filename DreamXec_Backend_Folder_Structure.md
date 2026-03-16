# DreamXec Backend - Scalable Folder Structure

## Overview

This is a **monorepo structure** designed for:
- 10+ microservices
- Team of 5 engineers
- Independent service deployments
- Shared code reusability
- Clear service boundaries

## Complete Folder Structure

```
dreamxec-backend/
│
├── README.md
├── .gitignore
├── .env.example
├── docker-compose.yml              # Local development
├── docker-compose.prod.yml         # Production reference
├── Makefile                        # Common commands
├── pyproject.toml                  # Python dependencies (root)
├── setup.py                        # Package setup
│
├── .github/                        # GitHub Actions CI/CD
│   ├── workflows/
│   │   ├── ci.yml                 # Run tests on PR
│   │   ├── deploy-staging.yml     # Deploy to staging
│   │   ├── deploy-production.yml  # Deploy to production
│   │   └── security-scan.yml      # Security scanning
│   └── CODEOWNERS                 # Code ownership
│
├── docs/                          # Documentation
│   ├── architecture/
│   │   ├── system-overview.md
│   │   ├── service-boundaries.md
│   │   └── data-flow.md
│   ├── api/
│   │   ├── openapi.yaml          # OpenAPI spec
│   │   └── postman-collection.json
│   ├── deployment/
│   │   ├── kubernetes.md
│   │   └── aws-setup.md
│   └── development/
│       ├── getting-started.md
│       ├── coding-standards.md
│       └── testing-guide.md
│
├── services/                      # Microservices (each independently deployable)
│   │
│   ├── api-gateway/              # Kong/Nginx gateway
│   │   ├── config/
│   │   │   ├── kong.yml
│   │   │   └── nginx.conf
│   │   └── Dockerfile
│   │
│   ├── orchestrator/             # Main pipeline orchestrator
│   │   ├── src/
│   │   │   ├── __init__.py
│   │   │   ├── main.py           # FastAPI entry point
│   │   │   ├── api/              # API endpoints
│   │   │   │   ├── __init__.py
│   │   │   │   ├── v1/
│   │   │   │   │   ├── __init__.py
│   │   │   │   │   ├── sessions.py
│   │   │   │   │   ├── messages.py
│   │   │   │   │   ├── checkpoints.py
│   │   │   │   │   └── health.py
│   │   │   │   └── dependencies.py
│   │   │   ├── core/             # Core business logic
│   │   │   │   ├── __init__.py
│   │   │   │   ├── state_machine.py
│   │   │   │   ├── pipeline.py
│   │   │   │   ├── checkpoint_manager.py
│   │   │   │   └── backtrack_handler.py
│   │   │   ├── models/           # Pydantic models
│   │   │   │   ├── __init__.py
│   │   │   │   ├── session.py
│   │   │   │   ├── stage.py
│   │   │   │   ├── checkpoint.py
│   │   │   │   └── response.py
│   │   │   ├── services/         # External service clients
│   │   │   │   ├── __init__.py
│   │   │   │   ├── agent_engine_client.py
│   │   │   │   ├── memory_keeper_client.py
│   │   │   │   └── validator_client.py
│   │   │   └── config.py         # Configuration
│   │   ├── tests/
│   │   │   ├── __init__.py
│   │   │   ├── unit/
│   │   │   │   ├── test_state_machine.py
│   │   │   │   └── test_pipeline.py
│   │   │   ├── integration/
│   │   │   │   └── test_api.py
│   │   │   └── conftest.py       # Pytest fixtures
│   │   ├── alembic/              # Database migrations
│   │   │   ├── versions/
│   │   │   └── env.py
│   │   ├── Dockerfile
│   │   ├── requirements.txt
│   │   └── README.md
│   │
│   ├── agent-engine/             # Agent execution service
│   │   ├── src/
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── v1/
│   │   │   │       ├── agents.py
│   │   │   │       └── execute.py
│   │   │   ├── agents/           # Agent implementations
│   │   │   │   ├── __init__.py
│   │   │   │   ├── base.py       # Base agent class
│   │   │   │   ├── agent_0_gateway.py
│   │   │   │   ├── agent_1_profiler.py
│   │   │   │   ├── agent_2_interviewer.py
│   │   │   │   ├── agent_3_extractor.py
│   │   │   │   ├── agent_4_validator.py
│   │   │   │   ├── agent_5_analyst.py
│   │   │   │   ├── agent_6_feasibility.py
│   │   │   │   ├── agent_7_scorer.py
│   │   │   │   └── agent_8_packager.py
│   │   │   ├── prompts/          # Agent prompts
│   │   │   │   ├── __init__.py
│   │   │   │   ├── templates/
│   │   │   │   │   ├── agent_0.txt
│   │   │   │   │   ├── agent_1.txt
│   │   │   │   │   └── ...
│   │   │   │   └── loader.py
│   │   │   ├── llm/              # LLM integration
│   │   │   │   ├── __init__.py
│   │   │   │   ├── anthropic_client.py
│   │   │   │   ├── openai_client.py
│   │   │   │   ├── router.py     # Model routing
│   │   │   │   └── cache.py      # Prompt caching
│   │   │   ├── validators/       # Output validators
│   │   │   │   ├── __init__.py
│   │   │   │   ├── json_schema.py
│   │   │   │   └── semantic.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   │   ├── unit/
│   │   │   │   ├── test_agents/
│   │   │   │   └── test_llm/
│   │   │   └── integration/
│   │   ├── Dockerfile
│   │   ├── requirements.txt
│   │   └── README.md
│   │
│   ├── memory-keeper/            # Session state & memory service
│   │   ├── src/
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── v1/
│   │   │   │       ├── memory.py
│   │   │   │       ├── checkpoints.py
│   │   │   │       └── branches.py
│   │   │   ├── core/
│   │   │   │   ├── memory_manager.py
│   │   │   │   ├── checkpoint_manager.py
│   │   │   │   ├── branch_manager.py
│   │   │   │   └── context_retrieval.py
│   │   │   ├── models/
│   │   │   │   ├── memory_fragment.py
│   │   │   │   ├── checkpoint.py
│   │   │   │   └── branch.py
│   │   │   ├── storage/          # Storage adapters
│   │   │   │   ├── postgres.py
│   │   │   │   ├── redis.py
│   │   │   │   └── s3.py
│   │   │   └── embeddings/       # Vector embeddings
│   │   │       ├── sentence_transformer.py
│   │   │       └── semantic_search.py
│   │   ├── tests/
│   │   ├── Dockerfile
│   │   └── requirements.txt
│   │
│   ├── validation-scanner/       # External validation service
│   │   ├── src/
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── v1/
│   │   │   │       └── scan.py
│   │   │   ├── scanners/         # Individual scanners
│   │   │   │   ├── __init__.py
│   │   │   │   ├── base.py
│   │   │   │   ├── reddit_scanner.py
│   │   │   │   ├── news_scanner.py
│   │   │   │   ├── crunchbase_scanner.py
│   │   │   │   └── scholar_scanner.py
│   │   │   ├── aggregator/       # Result aggregation
│   │   │   │   ├── __init__.py
│   │   │   │   ├── signal_scorer.py
│   │   │   │   └── report_generator.py
│   │   │   ├── cache/            # Caching layer
│   │   │   │   └── redis_cache.py
│   │   │   └── rate_limiter/
│   │   │       └── token_bucket.py
│   │   ├── tests/
│   │   ├── Dockerfile
│   │   └── requirements.txt
│   │
│   ├── document-generator/       # PDF/document generation
│   │   ├── src/
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── v1/
│   │   │   │       └── generate.py
│   │   │   ├── generators/
│   │   │   │   ├── campaign_package.py
│   │   │   │   ├── scorecard.py
│   │   │   │   ├── cost_table.py
│   │   │   │   └── risk_matrix.py
│   │   │   ├── templates/        # Jinja2 templates
│   │   │   │   ├── campaign/
│   │   │   │   │   ├── base.html
│   │   │   │   │   ├── executive_summary.html
│   │   │   │   │   └── ...
│   │   │   │   └── components/
│   │   │   │       ├── header.html
│   │   │   │       └── footer.html
│   │   │   ├── renderers/
│   │   │   │   ├── html_renderer.py
│   │   │   │   ├── pdf_renderer.py
│   │   │   │   └── chart_generator.py
│   │   │   └── storage/
│   │   │       └── s3_uploader.py
│   │   ├── tests/
│   │   ├── Dockerfile
│   │   └── requirements.txt
│   │
│   ├── metamcp-server/           # MetaMCP aggregator
│   │   ├── config/
│   │   │   └── metamcp-config.yaml
│   │   ├── mcp-servers/          # Individual MCP server configs
│   │   │   ├── reddit/
│   │   │   ├── newsapi/
│   │   │   ├── crunchbase/
│   │   │   └── scholar/
│   │   ├── Dockerfile
│   │   └── README.md
│   │
│   ├── auth-service/             # Authentication & authorization
│   │   ├── src/
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── v1/
│   │   │   │       ├── auth.py
│   │   │   │       ├── tokens.py
│   │   │   │       └── users.py
│   │   │   ├── core/
│   │   │   │   ├── jwt.py
│   │   │   │   ├── password.py
│   │   │   │   └── magic_link.py
│   │   │   └── models/
│   │   │       └── user.py
│   │   ├── tests/
│   │   └── Dockerfile
│   │
│   └── analytics-service/        # Metrics & logging
│       ├── src/
│       │   ├── __init__.py
│       │   ├── main.py
│       │   ├── collectors/
│       │   │   ├── prometheus.py
│       │   │   └── events.py
│       │   └── exporters/
│       │       ├── elasticsearch.py
│       │       └── cloudwatch.py
│       └── Dockerfile
│
├── shared/                       # Shared libraries (Python packages)
│   │
│   ├── common/                   # Common utilities
│   │   ├── src/
│   │   │   └── dreamxec_common/
│   │   │       ├── __init__.py
│   │   │       ├── logging.py    # Structured logging
│   │   │       ├── exceptions.py # Custom exceptions
│   │   │       ├── constants.py  # Global constants
│   │   │       └── utils/
│   │   │           ├── time.py
│   │   │           ├── string.py
│   │   │           └── validation.py
│   │   ├── tests/
│   │   ├── setup.py
│   │   └── README.md
│   │
│   ├── models/                   # Shared Pydantic models
│   │   ├── src/
│   │   │   └── dreamxec_models/
│   │   │       ├── __init__.py
│   │   │       ├── session.py
│   │   │       ├── student.py
│   │   │       ├── problem.py
│   │   │       ├── idea.py
│   │   │       └── api/
│   │   │           ├── requests.py
│   │   │           └── responses.py
│   │   ├── tests/
│   │   └── setup.py
│   │
│   ├── database/                 # Database utilities
│   │   ├── src/
│   │   │   └── dreamxec_db/
│   │   │       ├── __init__.py
│   │   │       ├── connection.py # Connection pooling
│   │   │       ├── base.py       # Base models
│   │   │       ├── session.py    # Session factory
│   │   │       └── migrations/   # Shared migrations
│   │   ├── tests/
│   │   └── setup.py
│   │
│   └── clients/                  # Internal service clients
│       ├── src/
│       │   └── dreamxec_clients/
│       │       ├── __init__.py
│       │       ├── base_client.py
│       │       ├── orchestrator_client.py
│       │       ├── agent_engine_client.py
│       │       ├── memory_keeper_client.py
│       │       └── validation_client.py
│       ├── tests/
│       └── setup.py
│
├── infrastructure/               # Infrastructure as Code
│   │
│   ├── terraform/               # Terraform configs
│   │   ├── environments/
│   │   │   ├── dev/
│   │   │   │   ├── main.tf
│   │   │   │   ├── variables.tf
│   │   │   │   └── outputs.tf
│   │   │   ├── staging/
│   │   │   └── production/
│   │   ├── modules/
│   │   │   ├── vpc/
│   │   │   ├── eks/
│   │   │   ├── rds/
│   │   │   ├── elasticache/
│   │   │   └── s3/
│   │   └── README.md
│   │
│   ├── kubernetes/              # K8s manifests
│   │   ├── base/                # Base configs
│   │   │   ├── namespace.yaml
│   │   │   ├── configmaps/
│   │   │   └── secrets/
│   │   ├── services/            # Service deployments
│   │   │   ├── orchestrator/
│   │   │   │   ├── deployment.yaml
│   │   │   │   ├── service.yaml
│   │   │   │   ├── hpa.yaml
│   │   │   │   └── configmap.yaml
│   │   │   ├── agent-engine/
│   │   │   ├── memory-keeper/
│   │   │   └── ...
│   │   ├── ingress/
│   │   │   └── ingress.yaml
│   │   ├── monitoring/
│   │   │   ├── prometheus/
│   │   │   └── grafana/
│   │   └── kustomization.yaml
│   │
│   ├── helm/                    # Helm charts
│   │   └── dreamxec/
│   │       ├── Chart.yaml
│   │       ├── values.yaml
│   │       ├── values-dev.yaml
│   │       ├── values-prod.yaml
│   │       └── templates/
│   │
│   └── ansible/                 # Server provisioning
│       ├── playbooks/
│       ├── roles/
│       └── inventory/
│
├── config/                      # Configuration files
│   ├── development.env
│   ├── staging.env
│   ├── production.env
│   └── agent-prompts/           # Centralized prompts
│       ├── agent_0.txt
│       ├── agent_1.txt
│       └── ...
│
├── scripts/                     # Utility scripts
│   ├── setup/
│   │   ├── install_dependencies.sh
│   │   └── setup_local_db.sh
│   ├── deploy/
│   │   ├── deploy_staging.sh
│   │   └── deploy_production.sh
│   ├── database/
│   │   ├── migrate.sh
│   │   ├── seed.sh
│   │   └── backup.sh
│   ├── testing/
│   │   ├── run_unit_tests.sh
│   │   ├── run_integration_tests.sh
│   │   └── run_load_tests.sh
│   └── utilities/
│       ├── generate_api_docs.sh
│       └── create_service.sh    # Template for new service
│
├── tests/                       # End-to-end tests
│   ├── e2e/
│   │   ├── test_complete_pipeline.py
│   │   ├── test_backtrack_flow.py
│   │   └── test_checkpoint_recovery.py
│   ├── load/
│   │   ├── locustfile.py        # Load testing with Locust
│   │   └── scenarios/
│   ├── fixtures/
│   │   └── test_data.py
│   └── conftest.py
│
├── monitoring/                  # Monitoring configs
│   ├── prometheus/
│   │   ├── prometheus.yml
│   │   └── alerts.yml
│   ├── grafana/
│   │   ├── dashboards/
│   │   │   ├── platform_overview.json
│   │   │   ├── agent_performance.json
│   │   │   └── llm_usage.json
│   │   └── datasources.yml
│   └── elk/
│       ├── logstash.conf
│       └── elasticsearch.yml
│
└── tools/                       # Development tools
    ├── api-client/              # Python API client
    │   └── dreamxec_client/
    ├── cli/                     # CLI tool for developers
    │   └── dreamxec_cli/
    └── postman/
        └── collections/
```

---

## Key Design Decisions

### 1. Monorepo vs Polyrepo
**Choice: Monorepo**

**Reasons:**
- Easier code sharing (shared libraries)
- Atomic commits across services
- Unified versioning
- Simpler CI/CD
- Better for team size (10 engineers)

### 2. Service Structure
Each service follows the same pattern:
```
service-name/
├── src/               # Source code
│   ├── api/          # API endpoints (versioned)
│   ├── core/         # Business logic
│   ├── models/       # Data models
│   ├── services/     # External clients
│   └── config.py     # Configuration
├── tests/            # Tests
├── Dockerfile        # Container definition
├── requirements.txt  # Dependencies
└── README.md         # Service docs
```

### 3. Shared Code Strategy
Shared code is packaged as installable Python packages:

```python
# In requirements.txt of any service
dreamxec-common==1.0.0
dreamxec-models==1.0.0
dreamxec-db==1.0.0
dreamxec-clients==1.0.0
```

Install from local:
```bash
pip install -e ../shared/common
```

Or from private PyPI:
```bash
pip install dreamxec-common
```

### 4. Configuration Management
**Environment-based config:**
```python
# services/orchestrator/src/config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    # Database
    POSTGRES_HOST: str
    POSTGRES_PORT: int = 5432
    POSTGRES_DB: str
    POSTGRES_USER: str
    POSTGRES_PASSWORD: str
    
    # Redis
    REDIS_HOST: str
    REDIS_PORT: int = 6379
    
    # Services
    AGENT_ENGINE_URL: str
    MEMORY_KEEPER_URL: str
    METAMCP_URL: str
    
    # LLM
    ANTHROPIC_API_KEY: str
    OPENAI_API_KEY: str
    
    # Environment
    ENVIRONMENT: str = "development"
    DEBUG: bool = False
    
    class Config:
        env_file = f".env.{os.getenv('ENVIRONMENT', 'development')}"
        case_sensitive = True

settings = Settings()
```

---

## Development Workflow

### Initial Setup
```bash
# Clone repository
git clone https://github.com/your-org/dreamxec-backend.git
cd dreamxec-backend

# Install dependencies
make install

# Setup local databases
make setup-db

# Run all services locally
make dev
```

### Working on a Service
```bash
# Navigate to service
cd services/orchestrator

# Install dependencies
pip install -r requirements.txt

# Install shared packages in editable mode
pip install -e ../../shared/common
pip install -e ../../shared/models

# Run tests
pytest tests/

# Run service locally
uvicorn src.main:app --reload --port 8000
```

### Running Tests
```bash
# Unit tests for all services
make test-unit

# Integration tests
make test-integration

# E2E tests
make test-e2e

# Specific service
cd services/orchestrator && pytest
```

### Deployment
```bash
# Build all Docker images
make build

# Deploy to staging
make deploy-staging

# Deploy to production
make deploy-production SERVICE=orchestrator
```

---

## Makefile Commands

```makefile
# Makefile at root

.PHONY: help install dev test build deploy

help:
	@echo "DreamXec Backend - Available Commands"
	@echo "  make install          - Install all dependencies"
	@echo "  make dev              - Start all services locally"
	@echo "  make test             - Run all tests"
	@echo "  make build            - Build Docker images"
	@echo "  make deploy-staging   - Deploy to staging"

install:
	@echo "Installing shared packages..."
	pip install -e shared/common
	pip install -e shared/models
	pip install -e shared/database
	pip install -e shared/clients
	@echo "Installing service dependencies..."
	cd services/orchestrator && pip install -r requirements.txt
	cd services/agent-engine && pip install -r requirements.txt

dev:
	docker-compose up

test-unit:
	@echo "Running unit tests..."
	pytest services/*/tests/unit -v

test-integration:
	@echo "Running integration tests..."
	pytest services/*/tests/integration -v

test-e2e:
	@echo "Running E2E tests..."
	pytest tests/e2e -v

build:
	@echo "Building Docker images..."
	docker-compose build

deploy-staging:
	./scripts/deploy/deploy_staging.sh

deploy-production:
	./scripts/deploy/deploy_production.sh

clean:
	find . -type d -name __pycache__ -exec rm -r {} +
	find . -type f -name "*.pyc" -delete
```

---

## Environment Variables

### `.env.example`
```bash
# Environment
ENVIRONMENT=development
DEBUG=true

# Database
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=dreamxec
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# Services
ORCHESTRATOR_URL=http://localhost:8000
AGENT_ENGINE_URL=http://localhost:8001
MEMORY_KEEPER_URL=http://localhost:8002
VALIDATION_SCANNER_URL=http://localhost:8003
DOCUMENT_GENERATOR_URL=http://localhost:8004
METAMCP_URL=http://localhost:8005

# LLM
ANTHROPIC_API_KEY=sk-ant-xxx
OPENAI_API_KEY=sk-xxx

# External APIs
REDDIT_CLIENT_ID=xxx
REDDIT_CLIENT_SECRET=xxx
NEWSAPI_KEY=xxx
CRUNCHBASE_API_KEY=xxx

# Storage
AWS_ACCESS_KEY_ID=xxx
AWS_SECRET_ACCESS_KEY=xxx
S3_BUCKET=dreamxec-documents

# Monitoring
SENTRY_DSN=https://xxx@sentry.io/xxx
```

---

## Service Communication

### Internal Service Calls
```python
# Using shared client library
from dreamxec_clients import AgentEngineClient

# In orchestrator service
agent_client = AgentEngineClient(base_url=settings.AGENT_ENGINE_URL)

result = await agent_client.execute_agent(
    agent_id="agent_0",
    inputs={"message": "Hello"}
)
```

### API Versioning
```python
# services/orchestrator/src/api/v1/sessions.py
from fastapi import APIRouter

router = APIRouter(prefix="/api/v1")

@router.post("/sessions")
async def create_session():
    pass

# services/orchestrator/src/main.py
from fastapi import FastAPI
from src.api.v1 import sessions

app = FastAPI()
app.include_router(sessions.router)

# URL: POST /api/v1/sessions
```

---

## Database Migrations

### Using Alembic
```bash
# In each service with database access
cd services/orchestrator

# Create migration
alembic revision --autogenerate -m "Add sessions table"

# Run migrations
alembic upgrade head

# Rollback
alembic downgrade -1
```

### Shared Migrations
```python
# shared/database/src/dreamxec_db/migrations/
# Common tables used by multiple services
```

---

## Testing Strategy

### Unit Tests
```python
# services/orchestrator/tests/unit/test_state_machine.py
import pytest
from src.core.state_machine import PipelineStateMachine

def test_stage_transition():
    sm = PipelineStateMachine(session_id="test-123")
    assert sm.current_stage == 0
    
    sm.transition_to_next()
    assert sm.current_stage == 1
```

### Integration Tests
```python
# services/orchestrator/tests/integration/test_api.py
from fastapi.testclient import TestClient
from src.main import app

client = TestClient(app)

def test_create_session():
    response = client.post(
        "/api/v1/sessions",
        json={"student_id": "test-456"}
    )
    assert response.status_code == 200
    assert "session_id" in response.json()
```

### E2E Tests
```python
# tests/e2e/test_complete_pipeline.py
import pytest

@pytest.mark.e2e
async def test_complete_student_journey():
    # Test entire pipeline from login to campaign generation
    session = await create_session()
    
    # Stage 0-1: Onboarding
    await complete_questionnaire(session)
    
    # Stage 2-3: Problem extraction
    problems = await get_problems(session)
    await select_problem(session, problems[0])
    
    # ... test all 15 stages
    
    # Verify campaign package generated
    package = await get_campaign_package(session)
    assert package.sections == 10
```

---

## CI/CD Pipeline

### GitHub Actions Workflow
```yaml
# .github/workflows/ci.yml
name: CI

on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Install dependencies
        run: |
          make install
      
      - name: Lint
        run: |
          flake8 services/
          black --check services/
          mypy services/
      
      - name: Unit tests
        run: make test-unit
      
      - name: Integration tests
        run: make test-integration
        env:
          POSTGRES_HOST: localhost
          REDIS_HOST: localhost
  
  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker images
        run: make build
      
      - name: Push to registry
        run: |
          docker tag orchestrator:latest registry/orchestrator:${{ github.sha }}
          docker push registry/orchestrator:${{ github.sha }}
```

---

## Security & Best Practices

### Secrets Management
```bash
# Never commit .env files
echo ".env*" >> .gitignore

# Use environment-specific files
.env.development
.env.staging
.env.production

# In production, use AWS Secrets Manager / Vault
```

### Code Quality
```bash
# Pre-commit hooks
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/psf/black
    hooks:
      - id: black
  
  - repo: https://github.com/PyCQA/flake8
    hooks:
      - id: flake8
  
  - repo: https://github.com/pre-commit/mirrors-mypy
    hooks:
      - id: mypy
```

---

## Team Collaboration

### Code Ownership
```
# .github/CODEOWNERS

# Overall platform
* @platform-team

# Services
/services/orchestrator/ @orchestrator-team
/services/agent-engine/ @agents-team
/services/memory-keeper/ @data-team

# Shared libraries
/shared/ @platform-team

# Infrastructure
/infrastructure/ @devops-team
```

### Branch Strategy
```
main                    # Production
  └── develop          # Integration
      ├── feature/orchestrator-checkpoints
      ├── feature/agent-4-validation
      └── fix/memory-leak-redis
```

---

## Monitoring & Observability

### Structured Logging
```python
# Using shared logging utility
from dreamxec_common.logging import get_logger

logger = get_logger(__name__)

logger.info(
    "agent_execution_started",
    session_id=session_id,
    agent_id=agent_id,
    stage=stage
)
```

### Metrics
```python
# Prometheus metrics
from prometheus_client import Counter, Histogram

agent_calls = Counter(
    'dreamxec_agent_calls_total',
    'Total agent invocations',
    ['agent_id', 'status']
)

agent_duration = Histogram(
    'dreamxec_agent_duration_seconds',
    'Agent execution time',
    ['agent_id']
)
```

---

## Documentation Standards

### Service README Template
```markdown
# Service Name

## Overview
Brief description

## API Endpoints
- POST /api/v1/endpoint - Description

## Configuration
Required environment variables

## Development
How to run locally

## Testing
How to run tests

## Deployment
Deployment notes
```

---

This folder structure scales from 1 developer to 100+ developers while maintaining clarity and supporting independent service deployments. Each service is isolated yet shares common code through well-defined packages.
