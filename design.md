# Design Document: AI-Powered Structured Skill Progression Platform

## Overview

The AI-Powered Structured Skill Progression Platform is a web-based system that helps engineering students and developers track their learning journey through verified task completion, social engagement, and AI-powered recommendations. The platform integrates with external coding platforms via Model Context Protocol (MCP) to verify achievements, calculates user compatibility through a deterministic synergy engine, and positions users within validated learning paths.

The architecture emphasizes strict separation of concerns with deterministic progression mechanics isolated from AI recommendations. All user progression (learning levels, synergy scores, learning path positions) is computed through deterministic algorithms based on verified events, while AI operates in a stateless advisory capacity for task generation and content summarization.

## Project Structure

```
skill-progression-platform/
├── frontend/                          # React/Next.js application
│   ├── src/
│   │   ├── components/
│   │   │   ├── auth/
│   │   │   │   ├── LoginForm.tsx
│   │   │   │   ├── RegisterForm.tsx
│   │   │   │   └── OAuthConnect.tsx
│   │   │   ├── dashboard/
│   │   │   │   ├── Dashboard.tsx
│   │   │   │   ├── UserStats.tsx
│   │   │   │   └── RecentActivity.tsx
│   │   │   ├── tasks/
│   │   │   │   ├── TaskList.tsx
│   │   │   │   ├── TaskCard.tsx
│   │   │   │   ├── TaskForm.tsx
│   │   │   │   └── TaskCompletion.tsx
│   │   │   ├── feed/
│   │   │   │   ├── ScratchFeed.tsx
│   │   │   │   ├── ScratchCard.tsx
│   │   │   │   └── ScratchCanvas.tsx
│   │   │   ├── synergy/
│   │   │   │   ├── SynergyList.tsx
│   │   │   │   ├── SynergyCard.tsx
│   │   │   │   └── SynergyBreakdown.tsx
│   │   │   ├── mindmap/
│   │   │   │   ├── LearningMindmap.tsx
│   │   │   │   ├── TopicNode.tsx
│   │   │   │   └── PathEdge.tsx
│   │   │   ├── analytics/
│   │   │   │   ├── CodingProfile.tsx
│   │   │   │   ├── ProgressChart.tsx
│   │   │   │   └── SkillDistribution.tsx
│   │   │   └── common/
│   │   │       ├── Button.tsx
│   │   │       ├── Modal.tsx
│   │   │       └── Spinner.tsx
│   │   ├── hooks/
│   │   │   ├── useAuth.ts
│   │   │   ├── useTasks.ts
│   │   │   ├── useSynergy.ts
│   │   │   └── useWebSocket.ts
│   │   ├── services/
│   │   │   ├── api.ts              # Axios client with interceptors
│   │   │   ├── auth.service.ts
│   │   │   ├── task.service.ts
│   │   │   ├── synergy.service.ts
│   │   │   └── analytics.service.ts
│   │   ├── store/
│   │   │   ├── index.ts            # Redux store configuration
│   │   │   ├── slices/
│   │   │   │   ├── authSlice.ts
│   │   │   │   ├── taskSlice.ts
│   │   │   │   └── synergySlice.ts
│   │   │   └── middleware/
│   │   │       └── apiMiddleware.ts
│   │   ├── types/
│   │   │   ├── user.types.ts
│   │   │   ├── task.types.ts
│   │   │   └── synergy.types.ts
│   │   ├── utils/
│   │   │   ├── validation.ts
│   │   │   ├── formatting.ts
│   │   │   └── constants.ts
│   │   ├── pages/
│   │   │   ├── index.tsx           # Home/Dashboard
│   │   │   ├── login.tsx
│   │   │   ├── register.tsx
│   │   │   ├── tasks.tsx
│   │   │   ├── feed.tsx
│   │   │   ├── synergy.tsx
│   │   │   ├── mindmap.tsx
│   │   │   └── profile.tsx
│   │   └── styles/
│   │       ├── globals.css
│   │       └── components/
│   ├── public/
│   │   ├── images/
│   │   └── icons/
│   ├── package.json
│   ├── tsconfig.json
│   ├── next.config.js
│   └── .env.example
│
├── services/                          # Backend microservices
│   ├── auth-service/                  # Authentication service
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   ├── routes/
│   │   │   │   │   ├── auth.py
│   │   │   │   │   └── oauth.py
│   │   │   │   └── dependencies.py
│   │   │   ├── models/
│   │   │   │   ├── user.py
│   │   │   │   └── oauth_connection.py
│   │   │   ├── services/
│   │   │   │   ├── auth_service.py
│   │   │   │   ├── jwt_service.py
│   │   │   │   └── oauth_service.py
│   │   │   ├── utils/
│   │   │   │   ├── encryption.py
│   │   │   │   └── validation.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env.example
│   │
│   ├── core-api/                      # Core API service
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   ├── routes/
│   │   │   │   │   ├── tasks.py
│   │   │   │   │   ├── posts.py
│   │   │   │   │   └── scratch.py
│   │   │   │   └── dependencies.py
│   │   │   ├── models/
│   │   │   │   ├── task.py
│   │   │   │   ├── reflection_post.py
│   │   │   │   └── scratch_event.py
│   │   │   ├── services/
│   │   │   │   ├── task_service.py
│   │   │   │   ├── feed_service.py
│   │   │   │   └── scratch_service.py
│   │   │   ├── schemas/
│   │   │   │   ├── task_schema.py
│   │   │   │   └── post_schema.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env.example
│   │
│   ├── verification-service/          # MCP verification orchestrator
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── routes/
│   │   │   │       └── verification.py
│   │   │   ├── mcp_clients/
│   │   │   │   ├── base_client.py
│   │   │   │   ├── leetcode_client.py
│   │   │   │   ├── codeforces_client.py
│   │   │   │   ├── github_client.py
│   │   │   │   └── context7_client.py
│   │   │   ├── services/
│   │   │   │   ├── verification_service.py
│   │   │   │   └── circuit_breaker.py
│   │   │   ├── models/
│   │   │   │   └── mcp_event.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env.example
│   │
│   ├── synergy-engine/                # Synergy calculation service
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── routes/
│   │   │   │       └── synergy.py
│   │   │   ├── services/
│   │   │   │   ├── synergy_calculator.py
│   │   │   │   ├── component_calculators.py
│   │   │   │   └── chat_unlock_service.py
│   │   │   ├── models/
│   │   │   │   └── synergy_score.py
│   │   │   ├── workers/
│   │   │   │   └── synergy_worker.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env.example
│   │
│   ├── learning-path-service/         # Learning path management
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── routes/
│   │   │   │       └── paths.py
│   │   │   ├── services/
│   │   │   │   ├── path_service.py
│   │   │   │   ├── position_calculator.py
│   │   │   │   └── prerequisite_enforcer.py
│   │   │   ├── models/
│   │   │   │   ├── learning_path.py
│   │   │   │   └── path_position.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env.example
│   │
│   ├── analytics-service/             # Analytics aggregation
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── api/
│   │   │   │   └── routes/
│   │   │   │       └── analytics.py
│   │   │   ├── services/
│   │   │   │   ├── profile_aggregator.py
│   │   │   │   ├── progress_analyzer.py
│   │   │   │   └── trend_calculator.py
│   │   │   ├── models/
│   │   │   │   └── unified_profile.py
│   │   │   ├── workers/
│   │   │   │   └── analytics_worker.py
│   │   │   └── config.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── .env.example
│   │
│   └── ai-service/                    # AI orchestration
│       ├── src/
│       │   ├── main.py
│       │   ├── api/
│       │   │   └── routes/
│       │   │       └── ai.py
│       │   ├── services/
│       │   │   ├── ai_service.py
│       │   │   ├── task_generator.py
│       │   │   ├── reflection_summarizer.py
│       │   │   └── skill_extractor.py
│       │   ├── prompts/
│       │   │   ├── task_generation.txt
│       │   │   ├── reflection_summary.txt
│       │   │   └── skill_extraction.txt
│       │   ├── guardrails/
│       │   │   ├── task_validator.py
│       │   │   └── reflection_validator.py
│       │   ├── models/
│       │   │   └── ai_log.py
│       │   ├── workers/
│       │   │   └── ai_worker.py
│       │   └── config.py
│       ├── tests/
│       ├── requirements.txt
│       ├── Dockerfile
│       └── .env.example
│
├── workers/                           # Background workers
│   ├── notification-worker/
│   │   ├── src/
│   │   │   ├── main.py
│   │   │   ├── handlers/
│   │   │   │   ├── email_handler.py
│   │   │   │   └── push_handler.py
│   │   │   └── config.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   └── shared/
│       ├── queue_client.py
│       └── event_bus.py
│
├── infrastructure/                    # Infrastructure as Code
│   ├── terraform/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── modules/
│   │   │   ├── eks/
│   │   │   ├── rds/
│   │   │   ├── elasticache/
│   │   │   ├── sqs/
│   │   │   └── s3/
│   │   └── environments/
│   │       ├── dev/
│   │       ├── staging/
│   │       └── production/
│   ├── kubernetes/
│   │   ├── base/
│   │   │   ├── namespace.yaml
│   │   │   ├── configmaps/
│   │   │   └── secrets/
│   │   ├── services/
│   │   │   ├── auth-service.yaml
│   │   │   ├── core-api.yaml
│   │   │   ├── verification-service.yaml
│   │   │   ├── synergy-engine.yaml
│   │   │   ├── learning-path-service.yaml
│   │   │   ├── analytics-service.yaml
│   │   │   └── ai-service.yaml
│   │   ├── ingress/
│   │   │   └── ingress.yaml
│   │   └── monitoring/
│   │       ├── prometheus.yaml
│   │       └── grafana.yaml
│   └── docker-compose/
│       ├── docker-compose.yml         # Local development
│       ├── docker-compose.test.yml    # Testing
│       └── docker-compose.prod.yml    # Production-like local
│
├── database/                          # Database management
│   ├── migrations/                    # Alembic migrations
│   │   ├── versions/
│   │   │   ├── 001_initial_schema.py
│   │   │   ├── 002_add_oauth.py
│   │   │   └── 003_add_synergy.py
│   │   ├── env.py
│   │   └── alembic.ini
│   ├── seeds/                         # Seed data
│   │   ├── users.sql
│   │   └── learning_paths.sql
│   └── schema/
│       └── schema.sql                 # Full schema for reference
│
├── shared/                            # Shared libraries
│   ├── python/
│   │   ├── common/
│   │   │   ├── database.py
│   │   │   ├── cache.py
│   │   │   ├── queue.py
│   │   │   ├── logging.py
│   │   │   └── metrics.py
│   │   └── setup.py
│   └── typescript/
│       ├── types/
│       │   └── index.ts
│       └── package.json
│
├── docs/                              # Documentation
│   ├── api/
│   │   ├── openapi.yaml
│   │   └── postman_collection.json
│   ├── architecture/
│   │   ├── system_design.md
│   │   ├── data_flow.md
│   │   └── security.md
│   ├── deployment/
│   │   ├── aws_setup.md
│   │   ├── kubernetes_guide.md
│   │   └── monitoring.md
│   └── development/
│       ├── setup.md
│       ├── contributing.md
│       └── testing.md
│
├── scripts/                           # Utility scripts
│   ├── setup/
│   │   ├── init_db.sh
│   │   └── create_secrets.sh
│   ├── deployment/
│   │   ├── deploy.sh
│   │   └── rollback.sh
│   └── maintenance/
│       ├── backup_db.sh
│       └── cleanup_logs.sh
│
├── tests/                             # Integration tests
│   ├── e2e/
│   │   ├── auth.test.ts
│   │   ├── task_flow.test.ts
│   │   └── synergy.test.ts
│   ├── load/
│   │   └── k6_scripts/
│   └── fixtures/
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── cd.yml
│   │   └── security_scan.yml
│   └── CODEOWNERS
│
├── .gitignore
├── README.md
├── LICENSE
└── Makefile                           # Common commands
```

## Architecture

### High-Level Architecture

The system follows a microservices-inspired layered architecture with clear boundaries, event-driven communication, and horizontal scalability:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                          Frontend Layer                                   │
│                   (React/Next.js + TypeScript)                           │
│                                                                           │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────┐  ┌──────────────┐ │
│  │  Dashboard  │  │ Task Manager │  │ Scratch Feed│  │   Mindmap    │ │
│  └─────────────┘  └──────────────┘  └─────────────┘  └──────────────┘ │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────┐  ┌──────────────┐ │
│  │ Synergy Viz │  │   Analytics  │  │  Chat UI    │  │  Auth Flow   │ │
│  └─────────────┘  └──────────────┘  └─────────────┘  └──────────────┘ │
│                                                                           │
│  State Management: Redux Toolkit / Zustand                               │
│  API Client: Axios with interceptors, retry logic, caching              │
└──────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ HTTPS/REST + WebSocket
                                    │ (API Gateway / Load Balancer)
                                    ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                         API Gateway Layer                                 │
│                    (Kong / AWS API Gateway / Nginx)                      │
│                                                                           │
│  - Rate Limiting (per user/IP)    - Request Validation                  │
│  - JWT Verification               - CORS Handling                        │
│  - Request Routing                - Response Caching                     │
│  - Circuit Breaking               - Metrics Collection                   │
└──────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
                    ▼               ▼               ▼
┌──────────────────────┐  ┌──────────────────┐  ┌──────────────────────┐
│   Auth Service       │  │  Core API        │  │  Analytics Service   │
│   (Node.js/FastAPI)  │  │  (FastAPI)       │  │  (Python/FastAPI)    │
│                      │  │                  │  │                      │
│ - User Registration  │  │ - Task CRUD      │  │ - Data Aggregation   │
│ - Login/Logout       │  │ - Task Lifecycle │  │ - Metrics Compute    │
│ - OAuth Management   │  │ - Scratch Events │  │ - Profile Analytics  │
│ - JWT Issuance       │  │ - Feed Retrieval │  │ - Trend Analysis     │
│ - Session Mgmt       │  │ - Path Queries   │  │                      │
└──────────────────────┘  └──────────────────┘  └──────────────────────┘
         │                         │                        │
         └─────────────────────────┼────────────────────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    │              │              │
                    ▼              ▼              ▼
┌──────────────────────┐  ┌──────────────────┐  ┌──────────────────────┐
│  Synergy Engine      │  │  Learning Path   │  │  Verification        │
│  Service (Python)    │  │  Service (Python)│  │  Service (Python)    │
│                      │  │                  │  │                      │
│ - Score Calculation  │  │ - Path Validation│  │ - MCP Orchestration  │
│ - Component Compute  │  │ - Position Calc  │  │ - Retry Logic        │
│ - Chat Unlock Logic  │  │ - Prerequisite   │  │ - Result Caching     │
│ - Batch Processing   │  │   Enforcement    │  │ - Event Publishing   │
└──────────────────────┘  └──────────────────┘  └──────────────────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    │              │              │
                    ▼              ▼              ▼
┌──────────────────────┐  ┌──────────────────┐  ┌──────────────────────┐
│  Message Queue       │  │  Event Bus       │  │  Cache Layer         │
│  (RabbitMQ/SQS)      │  │  (Redis Pub/Sub) │  │  (Redis)             │
│                      │  │                  │  │                      │
│ - Async Task Queue   │  │ - Real-time      │  │ - Session Store      │
│ - AI Job Queue       │  │   Events         │  │ - API Response Cache │
│ - MCP Retry Queue    │  │ - WebSocket Msgs │  │ - Synergy Cache      │
│ - Email Queue        │  │ - Notifications  │  │ - MCP Result Cache   │
└──────────────────────┘  └──────────────────┘  └──────────────────────┘
         │                                                │
         ▼                                                ▼
┌──────────────────────┐                    ┌──────────────────────┐
│  Background Workers  │                    │  MCP Adapter Layer   │
│  (Celery/Bull)       │                    │  (Abstraction)       │
│                      │                    │                      │
│ - AI Orchestration   │◄───────────────────│ - LeetCode Client    │
│ - Synergy Recalc     │                    │ - Codeforces Client  │
│ - Analytics Update   │                    │ - GitHub Client      │
│ - Notification Send  │                    │ - Context7 Client    │
└──────────────────────┘                    │ - Circuit Breakers   │
         │                                  │ - Fallback Handlers  │
         ▼                                  └──────────────────────┘
┌──────────────────────┐                               │
│  AI Service          │                               │
│  (Bedrock/OpenAI)    │                               │
│                      │                               │
│ - Task Generation    │                               │
│ - Reflection Summary │                               │
│ - Skill Extraction   │                               │
│ - Prompt Templates   │                               │
│ - Response Parsing   │                               │
│ - Guardrails         │                               │
└──────────────────────┘                               │
                                                        │
         ┌──────────────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                         Data Layer                                        │
│                                                                           │
│  ┌────────────────────────┐  ┌────────────────────────┐                │
│  │  Primary Database      │  │  Read Replicas         │                │
│  │  (PostgreSQL)          │  │  (PostgreSQL)          │                │
│  │                        │  │                        │                │
│  │ - Users & Auth         │  │ - Analytics Queries    │                │
│  │ - Tasks & Completions  │  │ - Feed Generation      │                │
│  │ - Raw Events           │  │ - Report Generation    │                │
│  │ - Derived State        │  │                        │                │
│  │ - Write Operations     │  │ - Read Operations      │                │
│  └────────────────────────┘  └────────────────────────┘                │
│                                                                           │
│  ┌────────────────────────┐  ┌────────────────────────┐                │
│  │  Time-Series DB        │  │  Object Storage        │                │
│  │  (TimescaleDB/         │  │  (S3/MinIO)            │                │
│  │   InfluxDB)            │  │                        │                │
│  │                        │  │ - User Uploads         │                │
│  │ - Scratch Events       │  │ - AI Logs (archive)    │                │
│  │ - MCP Events           │  │ - Backups              │                │
│  │ - Metrics              │  │ - Static Assets        │                │
│  └────────────────────────┘  └────────────────────────┘                │
└──────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                    Observability Layer                                    │
│                                                                           │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐            │
│  │  Logging       │  │  Metrics       │  │  Tracing       │            │
│  │  (ELK/Loki)    │  │  (Prometheus)  │  │  (Jaeger)      │            │
│  └────────────────┘  └────────────────┘  └────────────────┘            │
│                                                                           │
│  ┌────────────────────────────────────────────────────────┐             │
│  │  Monitoring Dashboard (Grafana)                        │             │
│  │  - Service Health  - API Latency  - Error Rates       │             │
│  │  - MCP Status      - Queue Depth   - Cache Hit Rate   │             │
│  └────────────────────────────────────────────────────────┘             │
└──────────────────────────────────────────────────────────────────────────┘
```

### Architectural Principles

1. **Deterministic Core**: All progression mechanics (levels, synergy, path position) use deterministic algorithms
2. **Event Sourcing**: Raw MCP events stored separately from derived state for auditability and replay
3. **AI Isolation**: AI has no direct write access to progression state; operates through validated API endpoints
4. **MCP Verification**: All task completions verified through external MCP servers before state changes
5. **Stateless AI**: AI orchestrator maintains no conversation history; context provided per request
6. **Service Isolation**: Each service has a single responsibility and can scale independently
7. **Async Processing**: Long-running operations (AI, MCP, synergy) processed asynchronously via message queues
8. **Caching Strategy**: Multi-layer caching (API gateway, Redis, application) for performance
9. **Circuit Breaking**: MCP clients implement circuit breakers to handle external service failures gracefully
10. **Read/Write Separation**: Read replicas for analytics and feed queries; primary DB for writes
11. **Idempotency**: All state-changing operations are idempotent to handle retries safely
12. **Observability First**: Comprehensive logging, metrics, and tracing built into every service

## Components and Interfaces

### Service Communication Patterns

#### Synchronous Communication (REST)
- Frontend ↔ API Gateway: HTTPS/REST for user-initiated requests
- API Gateway ↔ Services: Internal HTTP for immediate responses
- Services ↔ Database: Direct connection pooling

#### Asynchronous Communication (Message Queue)
- Task Completion → AI Job Queue → Background Worker → AI Service
- MCP Verification → Retry Queue (on failure) → Verification Service
- Synergy Recalculation → Compute Queue → Synergy Engine
- Notification Triggers → Email Queue → Notification Worker

#### Event-Driven Communication (Pub/Sub)
- Task Completed Event → [Synergy Engine, Analytics Service, Notification Service]
- User Level Updated Event → [Learning Path Service, Analytics Service]
- Chat Unlocked Event → [Notification Service, WebSocket Server]

### API Gateway Configuration

The API Gateway serves as the single entry point for all client requests:

**Rate Limiting Rules:**
```yaml
rate_limits:
  authenticated_user:
    requests_per_minute: 100
    burst: 20
  anonymous:
    requests_per_minute: 10
    burst: 5
  ai_endpoints:
    requests_per_minute: 10  # Expensive operations
```

**Circuit Breaker Configuration:**
```yaml
circuit_breaker:
  failure_threshold: 5
  timeout_seconds: 30
  half_open_requests: 3
```

**Caching Rules:**
```yaml
cache_rules:
  - path: /api/learning-paths/*
    ttl: 3600  # 1 hour
  - path: /api/analytics/profile
    ttl: 300   # 5 minutes
  - path: /api/posts
    ttl: 60    # 1 minute
```

### Frontend Components

#### Dashboard Component
- Displays user overview: current level, synergy connections, active tasks
- Shows recent activity feed and recommendations
- Provides navigation to all major features

#### Task Tracking Component
- Lists user tasks with status (pending, in-progress, completed)
- Allows task creation with description and verification source selection
- Initiates task completion flow with MCP verification
- Displays verification status and error messages

#### Scratch-to-Reveal Feed Component
- Renders reflection posts with scratch mask overlay
- Captures scratch interactions (touch/mouse events with coordinates)
- Sends scratch events to backend for synergy calculation
- Progressively reveals content based on scratch coverage
- Does not auto-reveal on passive scroll

#### Synergy Visualization Component
- Displays user connections with synergy scores
- Shows chat unlock status for each connection
- Visualizes synergy score breakdown (mutual scratch, level proximity, topic similarity, consistency)
- Provides access to unlocked chats

#### Learning Mindmap Component
- Renders interactive graph of learning topics
- Nodes represent topics (colored by completion status)
- Edges represent prerequisite relationships
- Highlights current user position in learning path
- Shows task details on node click

#### Unified Coding Profile Component
- Aggregates statistics from all connected MCP sources
- Displays: total problems solved, contest ratings, GitHub contributions, skill distribution
- Shows source attribution for each metric
- Updates when new MCP events received

### Backend API Components

#### Auth Service (Node.js/Express or FastAPI)

**Responsibilities:**
- User registration and login
- JWT token issuance and validation
- OAuth 2.0 flow management (GitHub, LeetCode, Codeforces)
- Token encryption and secure storage
- Session management with Redis

**Key Endpoints:**
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Authenticate user (returns JWT)
- `POST /api/auth/refresh` - Refresh JWT token
- `POST /api/auth/logout` - Invalidate session
- `GET /api/auth/profile` - Get user profile
- `POST /api/auth/oauth/connect` - Initiate OAuth flow
- `GET /api/auth/oauth/callback` - OAuth callback handler
- `DELETE /api/auth/oauth/disconnect` - Disconnect OAuth provider

**Security Features:**
- Password hashing with bcrypt (cost factor: 12)
- JWT with RS256 signing (asymmetric keys)
- Token rotation on refresh
- OAuth token encryption at rest (AES-256-GCM)
- Rate limiting: 5 login attempts per 15 minutes per IP
- CSRF protection for OAuth flows

**Database Access:**
- Primary: users, oauth_connections tables
- Cache: Session tokens in Redis (TTL: 24 hours)

#### Core API Service (FastAPI)

**Responsibilities:**
- Task CRUD operations
- Task lifecycle state machine
- Scratch event logging
- Reflection post retrieval
- Feed generation with pagination

**Key Endpoints:**
- `POST /api/tasks` - Create new task
- `GET /api/tasks` - List user tasks (paginated, filtered)
- `GET /api/tasks/:id` - Get task details
- `PUT /api/tasks/:id` - Update task
- `POST /api/tasks/:id/complete` - Initiate completion flow
- `DELETE /api/tasks/:id` - Delete task
- `GET /api/posts` - Get reflection post feed
- `GET /api/posts/:id` - Get specific post
- `POST /api/posts/:id/scratch` - Log scratch event

**Task Completion Flow:**
1. Validate task exists and belongs to user
2. Publish `task.completion.requested` event to message queue
3. Return 202 Accepted with job ID
4. Background worker picks up job
5. Verification Service calls appropriate MCP
6. On success: Update task state, trigger AI reflection generation
7. On failure: Update task with error, allow retry

**Database Access:**
- Primary: tasks, task_completions, reflection_posts, scratch_events
- Read Replica: Feed queries, task listings
- Cache: Recent posts (Redis, TTL: 5 minutes)

#### Verification Service (Python/FastAPI)

**Responsibilities:**
- Orchestrate MCP verification requests
- Implement retry logic with exponential backoff
- Cache verification results
- Publish verification events

**MCP Verification Flow:**
```python
async def verify_task_completion(task_id: str, user_id: str):
    task = await get_task(task_id)
    mcp_client = get_mcp_client(task.verification_source)
    
    try:
        # Attempt verification with circuit breaker
        result = await mcp_client.verify(
            user_id=user_id,
            requirements=task.verification_requirements
        )
        
        # Store raw MCP event
        await store_raw_mcp_event(user_id, task.verification_source, result)
        
        # Cache result
        await cache.set(f"verification:{task_id}", result, ttl=3600)
        
        # Publish success event
        await event_bus.publish("task.verified", {
            "task_id": task_id,
            "user_id": user_id,
            "verification_data": result
        })
        
        return result
        
    except MCPServiceUnavailable:
        # Queue for retry
        await retry_queue.enqueue(
            "verify_task",
            {"task_id": task_id, "user_id": user_id},
            retry_count=0,
            max_retries=5,
            backoff_base=2  # 2^retry_count seconds
        )
        raise
```

**Circuit Breaker Configuration:**
- Failure threshold: 5 consecutive failures
- Timeout: 30 seconds
- Half-open state: Allow 3 test requests
- Reset timeout: 60 seconds

**Database Access:**
- Primary: raw_mcp_events, task_completions
- Cache: Verification results (Redis, TTL: 1 hour)

#### Synergy Engine Service (Python)
#### Synergy Engine Service (Python)

**Responsibilities:**
- Calculate synergy scores using deterministic formula
- Compute individual synergy components
- Manage chat unlock state
- Batch process synergy updates

**Synergy Calculation Algorithm:**
```python
def calculate_synergy(user_a_id: str, user_b_id: str) -> SynergyScore:
    # Component 1: Mutual Scratch Weight (40%)
    mutual_scratches = count_bidirectional_scratches(user_a_id, user_b_id)
    max_scratches = 100  # Normalization constant
    mutual_scratch_weight = min(mutual_scratches / max_scratches, 1.0)
    
    # Component 2: Level Proximity Score (30%)
    level_a = get_user_level(user_a_id)
    level_b = get_user_level(user_b_id)
    level_proximity_score = 1.0 / (1.0 + abs(level_a - level_b))
    
    # Component 3: Topic Similarity Score (20%)
    skills_a = get_user_skills(user_a_id)
    skills_b = get_user_skills(user_b_id)
    topic_similarity_score = cosine_similarity(skills_a, skills_b)
    
    # Component 4: Consistency Factor (10%)
    interaction_days = count_interaction_days(user_a_id, user_b_id, window_days=30)
    consistency_factor = min(interaction_days / 15, 1.0)  # 15 days = max
    
    # Final score
    score = (
        mutual_scratch_weight * 0.4 +
        level_proximity_score * 0.3 +
        topic_similarity_score * 0.2 +
        consistency_factor * 0.1
    )
    
    # Chat unlock threshold
    chat_unlocked = score >= 0.6
    
    return SynergyScore(
        score=score,
        mutual_scratch_weight=mutual_scratch_weight,
        level_proximity_score=level_proximity_score,
        topic_similarity_score=topic_similarity_score,
        consistency_factor=consistency_factor,
        chat_unlocked=chat_unlocked
    )
```

**Batch Processing:**
- Triggered by events: new scratch, task completion, level update
- Processes affected user pairs only (not full recalculation)
- Uses job queue to prevent blocking
- Debounces rapid updates (max 1 recalc per user pair per minute)

**Database Access:**
- Primary: synergy_scores (upsert operations)
- Read: scratch_events, learning_levels, reflection_posts
- Cache: Recent synergy scores (Redis, TTL: 10 minutes)

**Event Subscriptions:**
- `scratch.logged` → Recalculate synergy for post author and scratcher
- `task.completed` → Recalculate synergy for user and all connections
- `level.updated` → Recalculate synergy for user and all connections

#### Learning Path Service (Python/FastAPI)

**Responsibilities:**
- Validate learning paths via Context7 MCP
- Compute user position in learning paths
- Enforce prerequisite requirements
- Provide topic recommendations

**Path Validation Flow:**
```python
async def validate_learning_path(path_definition: dict) -> ValidationResult:
    # Call Context7 MCP for validation
    context7_client = get_context7_client()
    
    validation = await context7_client.validate_path(
        topics=path_definition["topics"],
        prerequisites=path_definition["prerequisites"]
    )
    
    if not validation.is_valid:
        return ValidationResult(
            valid=False,
            errors=validation.errors,
            suggestions=validation.suggestions
        )
    
    # Store validated path
    await store_learning_path(path_definition, validated=True)
    
    return ValidationResult(valid=True)
```

**Position Calculation:**
```python
def calculate_path_position(user_id: str, path_name: str) -> PathPosition:
    path = get_learning_path(path_name)
    completed_tasks = get_user_completed_tasks(user_id)
    
    # Map completed tasks to topics
    completed_topics = set()
    for task in completed_tasks:
        if task.topic_area in path.topics:
            completed_topics.add(task.topic_area)
    
    # Find available topics (prerequisites met)
    available_topics = []
    for topic in path.topics:
        if topic.name in completed_topics:
            continue
        
        prerequisites_met = all(
            prereq in completed_topics 
            for prereq in topic.prerequisites
        )
        
        if prerequisites_met:
            available_topics.append(topic.name)
    
    # Determine current topic (first available)
    current_topic = available_topics[0] if available_topics else None
    
    return PathPosition(
        path_name=path_name,
        current_topic=current_topic,
        completed_topics=list(completed_topics),
        available_topics=available_topics,
        progress_percentage=len(completed_topics) / len(path.topics) * 100
    )
```

**Database Access:**
- Primary: learning_path_positions, learning_path_definitions
- Read: tasks, task_completions
- Cache: Path definitions (Redis, TTL: 1 hour)

#### Analytics Service (Python/FastAPI)

**Responsibilities:**
- Aggregate data from multiple MCP sources
- Compute unified coding profile metrics
- Generate progress reports
- Provide trend analysis

**Analytics Aggregation:**
```python
async def get_unified_profile(user_id: str) -> UnifiedProfile:
    # Fetch from cache first
    cached = await cache.get(f"profile:{user_id}")
    if cached:
        return cached
    
    # Aggregate from MCP events
    leetcode_stats = await aggregate_leetcode_stats(user_id)
    codeforces_stats = await aggregate_codeforces_stats(user_id)
    github_stats = await aggregate_github_stats(user_id)
    
    profile = UnifiedProfile(
        total_problems_solved=leetcode_stats.problems_solved,
        contest_rating=codeforces_stats.rating,
        github_contributions=github_stats.contributions,
        skill_distribution=merge_skill_distributions([
            leetcode_stats.topics,
            codeforces_stats.tags,
            github_stats.languages
        ]),
        sources={
            "leetcode": leetcode_stats,
            "codeforces": codeforces_stats,
            "github": github_stats
        }
    )
    
    # Cache for 5 minutes
    await cache.set(f"profile:{user_id}", profile, ttl=300)
    
    return profile
```

**Database Access:**
- Read Replica: raw_mcp_events, task_completions, learning_levels
- Cache: Aggregated profiles (Redis, TTL: 5 minutes)

#### Background Workers (Celery/Bull)

**Worker Types:**

1. **AI Worker**
   - Processes AI job queue
   - Generates task recommendations
   - Summarizes reflections
   - Extracts skills
   - Rate limited to prevent API overuse

2. **Synergy Worker**
   - Processes synergy calculation queue
   - Batch processes multiple user pairs
   - Updates chat unlock states
   - Publishes unlock events

3. **Analytics Worker**
   - Processes analytics update queue
   - Aggregates MCP events
   - Updates derived metrics
   - Generates periodic reports

4. **Notification Worker**
   - Processes notification queue
   - Sends emails (task completed, chat unlocked)
   - Sends push notifications
   - Manages notification preferences

**Job Queue Configuration:**
```yaml
queues:
  ai_jobs:
    concurrency: 5
    priority: high
    timeout: 60s
  synergy_jobs:
    concurrency: 10
    priority: medium
    timeout: 30s
  analytics_jobs:
    concurrency: 3
    priority: low
    timeout: 120s
  notification_jobs:
    concurrency: 20
    priority: high
    timeout: 10s
```

### MCP Integration Layer

#### MCP Adapter Pattern

All MCP clients implement a common interface:

```python
class MCPClient(ABC):
    @abstractmethod
    async def verify(self, user_id: str, requirements: dict) -> VerificationResult:
        pass
    
    @abstractmethod
    async def get_user_stats(self, user_id: str) -> dict:
        pass
    
    @abstractmethod
    async def health_check(self) -> bool:
        pass
```

#### LeetCode MCP Client

**Verification Logic:**
```python
async def verify(self, user_id: str, requirements: dict) -> VerificationResult:
    oauth_token = await get_oauth_token(user_id, "leetcode")
    
    # Requirements: {"problem_slug": "two-sum", "min_difficulty": "easy"}
    problem_slug = requirements["problem_slug"]
    
    # Call LeetCode API
    submissions = await self.api_client.get_submissions(
        token=oauth_token,
        problem_slug=problem_slug
    )
    
    # Check for accepted submission
    accepted = any(sub.status == "Accepted" for sub in submissions)
    
    return VerificationResult(
        verified=accepted,
        metadata={
            "problem": problem_slug,
            "submissions": len(submissions),
            "accepted_at": submissions[0].timestamp if accepted else None
        }
    )
```

#### Codeforces MCP Client

**Verification Logic:**
```python
async def verify(self, user_id: str, requirements: dict) -> VerificationResult:
    oauth_token = await get_oauth_token(user_id, "codeforces")
    
    # Requirements: {"contest_id": 1234, "problem_index": "A"}
    contest_id = requirements["contest_id"]
    problem_index = requirements["problem_index"]
    
    # Call Codeforces API
    submissions = await self.api_client.get_user_status(
        token=oauth_token,
        contest_id=contest_id
    )
    
    # Check for accepted submission
    accepted = any(
        sub.problem.index == problem_index and sub.verdict == "OK"
        for sub in submissions
    )
    
    return VerificationResult(
        verified=accepted,
        metadata={
            "contest_id": contest_id,
            "problem_index": problem_index,
            "rating_change": await self.get_rating_change(contest_id)
        }
    )
```

#### GitHub MCP Client

**Verification Logic:**
```python
async def verify(self, user_id: str, requirements: dict) -> VerificationResult:
    oauth_token = await get_oauth_token(user_id, "github")
    
    # Requirements: {"repo": "username/repo", "min_commits": 5}
    repo = requirements["repo"]
    min_commits = requirements.get("min_commits", 1)
    
    # Call GitHub API
    commits = await self.api_client.get_commits(
        token=oauth_token,
        repo=repo
    )
    
    verified = len(commits) >= min_commits
    
    return VerificationResult(
        verified=verified,
        metadata={
            "repo": repo,
            "commit_count": len(commits),
            "latest_commit": commits[0].sha if commits else None
        }
    )
```

#### Context7 MCP Client

**Path Validation Logic:**
```python
async def validate_path(self, topics: list, prerequisites: dict) -> ValidationResult:
    # Call Context7 API
    response = await self.api_client.validate_learning_path(
        topics=topics,
        prerequisites=prerequisites
    )
    
    return ValidationResult(
        is_valid=response.valid,
        errors=response.errors,
        suggestions=response.suggestions,
        validated_order=response.recommended_order
    )
```

### AI Orchestration Layer

#### AI Service Architecture

**Components:**
- Prompt Template Manager
- Context Builder
- Response Parser
- Guardrail Validator
- Token Usage Tracker

**AI Service Interface:**
```python
class AIService:
    def __init__(self, model_provider: str = "bedrock"):
        self.provider = get_provider(model_provider)
        self.prompt_manager = PromptTemplateManager()
        self.guardrails = GuardrailValidator()
    
    async def generate_task(
        self, 
        user_level: float, 
        topic: str, 
        path_position: dict
    ) -> TaskRecommendation:
        # Build context
        context = {
            "user_level": user_level,
            "topic": topic,
            "path_position": path_position,
            "recent_completions": await self.get_recent_completions()
        }
        
        # Get prompt template
        prompt = self.prompt_manager.render(
            "task_generation",
            context
        )
        
        # Call AI model
        response = await self.provider.complete(
            prompt=prompt,
            max_tokens=500,
            temperature=0.7
        )
        
        # Parse response
        task = self.parse_task_response(response)
        
        # Validate with guardrails
        if not self.guardrails.validate_task(task):
            raise InvalidTaskError("Task failed guardrail validation")
        
        # Log inference
        await self.log_inference(
            operation="task_generation",
            request=context,
            response=task,
            tokens=response.usage.total_tokens
        )
        
        return task
```

#### Prompt Templates

**Task Generation Template:**
```
You are a learning task generator for an engineering education platform.

User Context:
- Current Level: {{user_level}} (scale 1-10)
- Topic Area: {{topic}}
- Learning Path Position: {{path_position.current_topic}}
- Recently Completed: {{recent_completions}}

Generate a single learning task that:
1. Matches the user's current level (difficulty appropriate)
2. Aligns with their learning path position
3. Can be verified via {{verification_source}}
4. Builds on recently completed tasks

Output Format (JSON):
{
  "description": "Clear task description",
  "difficulty": 1-10,
  "verification_source": "leetcode|codeforces|github",
  "verification_requirements": {
    // Source-specific requirements
  },
  "estimated_time_minutes": 30-240,
  "skills": ["skill1", "skill2"]
}

Constraints:
- Task must be verifiable through the specified source
- Difficulty must be within ±1 of user level
- Description must be clear and actionable
```

**Reflection Summarization Template:**
```
You are a learning reflection summarizer.

Task Completed:
- Description: {{task.description}}
- Difficulty: {{task.difficulty}}
- Verification: {{verification_result}}

Generate a structured reflection post:

Output Format (JSON):
{
  "summary": "2-3 sentence reflection on what was learned",
  "key_skills": ["skill1", "skill2", "skill3"],
  "concepts_applied": ["concept1", "concept2"],
  "difficulty_rating": "easier|as-expected|harder",
  "next_steps": "Brief suggestion for next learning step"
}

Constraints:
- Summary must be 2-3 sentences maximum
- Extract 3-5 key skills
- Identify 2-4 concepts applied
- Keep next steps brief (1 sentence)
```

#### Guardrail Validation

**Task Guardrails:**
```python
def validate_task(self, task: dict) -> bool:
    # Check required fields
    required_fields = ["description", "difficulty", "verification_source"]
    if not all(field in task for field in required_fields):
        return False
    
    # Validate difficulty range
    if not (1 <= task["difficulty"] <= 10):
        return False
    
    # Validate verification source
    valid_sources = ["leetcode", "codeforces", "github"]
    if task["verification_source"] not in valid_sources:
        return False
    
    # Check description length
    if len(task["description"]) < 20 or len(task["description"]) > 500:
        return False
    
    # Validate verification requirements exist
    if "verification_requirements" not in task:
        return False
    
    return True
```

**Reflection Guardrails:**
```python
def validate_reflection(self, reflection: dict) -> bool:
    # Check required fields
    required_fields = ["summary", "key_skills", "concepts_applied"]
    if not all(field in reflection for field in required_fields):
        return False
    
    # Validate summary length (2-3 sentences)
    sentences = reflection["summary"].split(".")
    if not (2 <= len(sentences) <= 4):
        return False
    
    # Validate skill count
    if not (3 <= len(reflection["key_skills"]) <= 5):
        return False
    
    # Validate concepts count
    if not (2 <= len(reflection["concepts_applied"]) <= 4):
        return False
    
    return True
```
- Creates tasks in pending state
- Initiates MCP verification on completion request
- Updates task state based on verification result
- Triggers reflection post generation on successful completion
- Maintains audit log of all state transitions

#### MCP Integration Service
- Abstracts MCP server communication
- Routes verification requests to appropriate MCP server (LeetCode, Codeforces, GitHub)
- Stores raw MCP responses in database
- Implements retry logic with exponential backoff for failures
- Logs all MCP requests and responses

#### Synergy Engine Service
- Calculates synergy scores using deterministic formula:
  ```
  Synergy_Score = (Mutual_Scratch_Weight × 0.4) + 
                  (Level_Proximity_Score × 0.3) + 
                  (Topic_Similarity_Score × 0.2) + 
                  (Consistency_Factor × 0.1)
  ```
- Components:
  - **Mutual_Scratch_Weight**: Count of bidirectional scratch events between users
  - **Level_Proximity_Score**: Inverse distance between learning levels (1 / (1 + |level_a - level_b|))
  - **Topic_Similarity_Score**: Cosine similarity of skill vectors
  - **Consistency_Factor**: Interaction frequency over rolling 30-day window
- Recalculates on new scratch events or task completions
- Updates chat unlock state when threshold crossed

#### Learning Path Service
- Validates AI-proposed learning paths using Context7 MCP
- Enforces prerequisite completion before topic access
- Computes user position in learning paths based on completed tasks
- Stores position as derived state (recomputed from task completions)
- Provides prerequisite map for frontend visualization

#### Learning Level Service
- Computes learning level using deterministic algorithm:
  ```
  Level = Σ(task_difficulty × completion_weight) / normalization_factor
  ```
- Recalculates on task completion
- Provides level breakdowns by topic area
- Stores level as derived state

#### AI Orchestration Service
- Generates task recommendations based on user level and path position
- Summarizes completed tasks into reflection posts
- Extracts skills from task descriptions and completions
- Operates statelessly (no conversation memory)
- Uses structured prompts with context provided per request
- All operations logged for audit

#### Analytics Aggregation Service
- Aggregates data from multiple MCP sources
- Computes: total problems solved, contest ratings, GitHub contributions, skill distribution
- Updates analytics on new MCP events
- Filters out disconnected sources
- Provides source attribution for all metrics

### MCP Integration Layer

#### LeetCode MCP Client
- Verifies problem submissions
- Returns: submission status, problem difficulty, topics, submission timestamp
- Handles authentication via stored OAuth token

#### Codeforces MCP Client
- Verifies contest participation and problem solves
- Returns: contest rating, problems solved, submission timestamp
- Handles authentication via stored OAuth token

#### GitHub MCP Client
- Verifies commits, pull requests, repository creation
- Returns: commit history, PR metadata, repository info
- Handles authentication via stored OAuth token

#### Context7 MCP Client
- Validates learning path topic ordering
- Returns: prerequisite relationships, topic dependencies, recommended sequence
- Ensures no hallucinated or misordered paths

### AI Orchestration Layer

#### Task Generation Module
- Input: user learning level, current path position, topic area
- Output: task description, difficulty estimate, verification source, MCP requirements
- Prompt structure:
  ```
  Generate a learning task for:
  - User Level: {level}
  - Current Topic: {topic}
  - Path Position: {position}
  - Previous Tasks: {recent_completions}
  
  Requirements:
  - Appropriate difficulty for level
  - Verifiable via {mcp_source}
  - Aligned with learning path prerequisites
  ```

#### Reflection Summarization Module
- Input: task description, completion data, verification result
- Output: structured reflection post with skill extractions
- Prompt structure:
  ```
  Summarize this completed learning task:
  - Task: {description}
  - Completion Data: {completion_info}
  - Verification: {mcp_result}
  
  Generate:
  - Brief reflection (2-3 sentences)
  - Key skills learned (list)
  - Concepts applied (list)
  ```

#### Skill Extraction Module
- Input: task description, reflection post
- Output: structured skill tags
- Extracts: programming languages, frameworks, algorithms, concepts
- Normalizes skill names for consistency

## Data Models

### Database Schema

#### Users Table
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
```

#### OAuth Connections Table
```sql
CREATE TABLE oauth_connections (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    provider VARCHAR(50) NOT NULL, -- 'github', 'leetcode', 'codeforces'
    encrypted_token TEXT NOT NULL,
    token_expires_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, provider)
);

CREATE INDEX idx_oauth_user ON oauth_connections(user_id);
```

#### Tasks Table
```sql
CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    description TEXT NOT NULL,
    verification_source VARCHAR(50) NOT NULL, -- 'leetcode', 'codeforces', 'github'
    verification_requirements JSONB NOT NULL, -- MCP-specific requirements
    state VARCHAR(20) NOT NULL DEFAULT 'pending', -- 'pending', 'in_progress', 'completed', 'failed'
    difficulty INTEGER, -- 1-10 scale
    topic_area VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_tasks_user ON tasks(user_id);
CREATE INDEX idx_tasks_state ON tasks(state);
```

#### Task Completions Table
```sql
CREATE TABLE task_completions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    completed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    verification_data JSONB NOT NULL, -- Raw MCP verification response
    UNIQUE(task_id, user_id)
);

CREATE INDEX idx_completions_user ON task_completions(user_id);
CREATE INDEX idx_completions_task ON task_completions(task_id);
```

#### Task State Audit Log Table
```sql
CREATE TABLE task_state_audit (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    from_state VARCHAR(20),
    to_state VARCHAR(20) NOT NULL,
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    metadata JSONB
);

CREATE INDEX idx_audit_task ON task_state_audit(task_id);
CREATE INDEX idx_audit_timestamp ON task_state_audit(changed_at);
```

#### Raw MCP Events Table
```sql
CREATE TABLE raw_mcp_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    mcp_source VARCHAR(50) NOT NULL, -- 'leetcode', 'codeforces', 'github', 'context7'
    event_type VARCHAR(100) NOT NULL,
    event_data JSONB NOT NULL,
    received_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_mcp_events_user ON raw_mcp_events(user_id);
CREATE INDEX idx_mcp_events_source ON raw_mcp_events(mcp_source);
CREATE INDEX idx_mcp_events_timestamp ON raw_mcp_events(received_at);
```

#### Reflection Posts Table
```sql
CREATE TABLE reflection_posts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    task_id UUID NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    extracted_skills JSONB NOT NULL, -- Array of skill tags
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(task_id)
);

CREATE INDEX idx_posts_user ON reflection_posts(user_id);
CREATE INDEX idx_posts_created ON reflection_posts(created_at);
```

#### Scratch Events Table
```sql
CREATE TABLE scratch_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    post_id UUID NOT NULL REFERENCES reflection_posts(id) ON DELETE CASCADE,
    scratch_x INTEGER NOT NULL,
    scratch_y INTEGER NOT NULL,
    scratched_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_scratch_user ON scratch_events(user_id);
CREATE INDEX idx_scratch_post ON scratch_events(post_id);
CREATE INDEX idx_scratch_timestamp ON scratch_events(scratched_at);
```

#### Synergy Scores Table (Derived State)
```sql
CREATE TABLE synergy_scores (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_a_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    user_b_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    score DECIMAL(5,4) NOT NULL, -- 0.0000 to 1.0000
    mutual_scratch_weight DECIMAL(5,4) NOT NULL,
    level_proximity_score DECIMAL(5,4) NOT NULL,
    topic_similarity_score DECIMAL(5,4) NOT NULL,
    consistency_factor DECIMAL(5,4) NOT NULL,
    chat_unlocked BOOLEAN DEFAULT FALSE,
    calculated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_a_id, user_b_id),
    CHECK (user_a_id < user_b_id) -- Ensure canonical ordering
);

CREATE INDEX idx_synergy_users ON synergy_scores(user_a_id, user_b_id);
CREATE INDEX idx_synergy_score ON synergy_scores(score);
```

#### Learning Levels Table (Derived State)
```sql
CREATE TABLE learning_levels (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    topic_area VARCHAR(100) NOT NULL,
    level DECIMAL(5,2) NOT NULL, -- Computed level value
    tasks_completed INTEGER NOT NULL,
    calculated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, topic_area)
);

CREATE INDEX idx_levels_user ON learning_levels(user_id);
```

#### Learning Path Positions Table (Derived State)
```sql
CREATE TABLE learning_path_positions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    path_name VARCHAR(100) NOT NULL,
    current_topic VARCHAR(100) NOT NULL,
    completed_topics JSONB NOT NULL, -- Array of completed topic names
    available_topics JSONB NOT NULL, -- Array of topics with prerequisites met
    calculated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, path_name)
);

CREATE INDEX idx_path_positions_user ON learning_path_positions(user_id);
```

#### Learning Path Definitions Table
```sql
CREATE TABLE learning_path_definitions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    path_name VARCHAR(100) UNIQUE NOT NULL,
    topics JSONB NOT NULL, -- Array of topic objects with prerequisites
    validated_by_context7 BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### AI Inference Logs Table
```sql
CREATE TABLE ai_inference_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE SET NULL,
    operation_type VARCHAR(50) NOT NULL, -- 'task_generation', 'reflection_summary', 'skill_extraction'
    request_data JSONB NOT NULL,
    response_data JSONB NOT NULL,
    model_used VARCHAR(100),
    tokens_used INTEGER,
    inference_time_ms INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_ai_logs_user ON ai_inference_logs(user_id);
CREATE INDEX idx_ai_logs_operation ON ai_inference_logs(operation_type);
CREATE INDEX idx_ai_logs_timestamp ON ai_inference_logs(created_at);
```

### API Endpoints

#### Authentication Endpoints
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Authenticate user
- `POST /api/auth/logout` - Invalidate session
- `GET /api/auth/profile` - Get user profile
- `POST /api/auth/oauth/connect` - Connect OAuth provider
- `DELETE /api/auth/oauth/disconnect` - Disconnect OAuth provider

#### Task Endpoints
- `POST /api/tasks` - Create new task
- `GET /api/tasks` - List user tasks (with filters)
- `GET /api/tasks/:id` - Get task details
- `PUT /api/tasks/:id` - Update task
- `POST /api/tasks/:id/complete` - Mark task complete (triggers MCP verification)
- `DELETE /api/tasks/:id` - Delete task

#### Reflection Post Endpoints
- `GET /api/posts` - Get feed of reflection posts
- `GET /api/posts/:id` - Get specific post
- `POST /api/posts/:id/scratch` - Log scratch event

#### Synergy Endpoints
- `GET /api/synergy` - Get user's synergy connections
- `GET /api/synergy/:userId` - Get synergy score with specific user

#### Learning Path Endpoints
- `GET /api/learning-paths` - List available learning paths
- `GET /api/learning-paths/:pathName` - Get path details and user position
- `POST /api/learning-paths/validate` - Validate proposed learning path via Context7

#### Analytics Endpoints
- `GET /api/analytics/profile` - Get unified coding profile analytics
- `GET /api/analytics/progress` - Get learning progress over time

#### AI Endpoints (Internal)
- `POST /api/ai/generate-task` - Generate task recommendation
- `POST /api/ai/summarize-reflection` - Generate reflection post
- `POST /api/ai/extract-skills` - Extract skills from text

#### Health Check Endpoints
- `GET /api/health` - Service health status
- `GET /api/health/db` - Database connection status
- `GET /api/health/mcp` - MCP server connectivity status


## Deployment Architecture

### Container Strategy

Each service is containerized using Docker with multi-stage builds:

**Example Dockerfile (FastAPI Service):**
```dockerfile
# Build stage
FROM python:3.11-slim as builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# Runtime stage
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Kubernetes Deployment

**Service Deployment Example (Core API):**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: core-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: core-api
  template:
    metadata:
      labels:
        app: core-api
    spec:
      containers:
      - name: core-api
        image: skill-platform/core-api:latest
        ports:
        - containerPort: 8000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: url
        - name: REDIS_URL
          valueFrom:
            configMapKeyRef:
              name: redis-config
              key: url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /health/ready
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: core-api-service
spec:
  selector:
    app: core-api
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8000
  type: ClusterIP
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: core-api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: core-api
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

### AWS Deployment Architecture

**Infrastructure Components:**

1. **Compute:**
   - EKS (Elastic Kubernetes Service) for container orchestration
   - EC2 instances (t3.medium for workers)
   - Auto Scaling Groups for dynamic scaling

2. **Database:**
   - RDS PostgreSQL (Multi-AZ for HA)
   - Instance: db.r6g.xlarge (4 vCPU, 32 GB RAM)
   - Read Replicas: 2 instances for analytics queries
   - Automated backups with 7-day retention

3. **Caching:**
   - ElastiCache Redis (cluster mode enabled)
   - Instance: cache.r6g.large (2 vCPU, 13 GB RAM)
   - 3 shards with 1 replica each

4. **Message Queue:**
   - Amazon SQS for job queues
   - Separate queues: ai-jobs, synergy-jobs, analytics-jobs, notifications
   - Dead Letter Queues (DLQ) for failed messages

5. **Object Storage:**
   - S3 for static assets, backups, AI logs
   - Lifecycle policies: Archive to Glacier after 90 days

6. **Load Balancing:**
   - Application Load Balancer (ALB) for HTTP/HTTPS traffic
   - Network Load Balancer (NLB) for WebSocket connections

7. **CDN:**
   - CloudFront for static asset delivery
   - Edge caching for API responses (selected endpoints)

8. **Monitoring:**
   - CloudWatch for logs and metrics
   - X-Ray for distributed tracing
   - SNS for alerting

**Network Architecture:**
```
┌─────────────────────────────────────────────────────────────┐
│                      CloudFront CDN                          │
│                   (Static Assets + API Cache)                │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   Application Load Balancer                  │
│                      (Public Subnet)                         │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│  EKS Cluster │  │  EKS Cluster │  │  EKS Cluster │
│   (AZ-1a)    │  │   (AZ-1b)    │  │   (AZ-1c)    │
│              │  │              │  │              │
│ - Auth Svc   │  │ - Auth Svc   │  │ - Auth Svc   │
│ - Core API   │  │ - Core API   │  │ - Core API   │
│ - Analytics  │  │ - Analytics  │  │ - Analytics  │
│ - Workers    │  │ - Workers    │  │ - Workers    │
└──────────────┘  └──────────────┘  └──────────────┘
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│ RDS Primary  │  │ ElastiCache  │  │     SQS      │
│  (AZ-1a)     │  │   Cluster    │  │   Queues     │
│              │  │              │  │              │
│ Sync Replica │  │ 3 Shards     │  │ - AI Jobs    │
│  (AZ-1b)     │  │ 1 Replica/   │  │ - Synergy    │
│              │  │   Shard      │  │ - Analytics  │
│ Read Replica │  │              │  │ - Notify     │
│  (AZ-1c)     │  │              │  │              │
└──────────────┘  └──────────────┘  └──────────────┘
```

### Environment Configuration

**Environment Variables (per service):**
```bash
# Database
DATABASE_URL=postgresql://user:pass@host:5432/dbname
DATABASE_POOL_SIZE=20
DATABASE_MAX_OVERFLOW=10

# Redis
REDIS_URL=redis://host:6379/0
REDIS_POOL_SIZE=10

# Message Queue
SQS_AI_QUEUE_URL=https://sqs.region.amazonaws.com/account/ai-jobs
SQS_SYNERGY_QUEUE_URL=https://sqs.region.amazonaws.com/account/synergy-jobs

# AI Service
AI_PROVIDER=bedrock
AI_MODEL=anthropic.claude-v2
AI_MAX_TOKENS=1000
AI_TEMPERATURE=0.7

# MCP Clients
LEETCODE_MCP_URL=https://leetcode-mcp.example.com
CODEFORCES_MCP_URL=https://codeforces-mcp.example.com
GITHUB_MCP_URL=https://github-mcp.example.com
CONTEXT7_MCP_URL=https://context7-mcp.example.com

# Security
JWT_SECRET_KEY=<from-secrets-manager>
JWT_ALGORITHM=RS256
JWT_EXPIRATION_MINUTES=1440
OAUTH_ENCRYPTION_KEY=<from-secrets-manager>

# Observability
LOG_LEVEL=INFO
JAEGER_AGENT_HOST=jaeger-agent
JAEGER_AGENT_PORT=6831
```

### CI/CD Pipeline

**GitHub Actions Workflow:**
```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: |
          docker-compose -f docker-compose.test.yml up --abort-on-container-exit
      - name: Run linting
        run: |
          docker run --rm -v $(pwd):/app skill-platform/linter

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      - name: Login to ECR
        run: |
          aws ecr get-login-password | docker login --username AWS --password-stdin $ECR_REGISTRY
      - name: Build and push images
        run: |
          docker build -t $ECR_REGISTRY/core-api:$GITHUB_SHA ./services/core-api
          docker push $ECR_REGISTRY/core-api:$GITHUB_SHA
          # Repeat for other services

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Update Kubernetes manifests
        run: |
          kubectl set image deployment/core-api core-api=$ECR_REGISTRY/core-api:$GITHUB_SHA
          kubectl rollout status deployment/core-api
      - name: Run database migrations
        run: |
          kubectl run migration --image=$ECR_REGISTRY/core-api:$GITHUB_SHA --command -- alembic upgrade head
```

### Database Migration Strategy

**Alembic Configuration:**
```python
# alembic/env.py
from alembic import context
from sqlalchemy import engine_from_config, pool
from app.models import Base

def run_migrations_online():
    connectable = engine_from_config(
        config.get_section(config.config_ini_section),
        prefix="sqlalchemy.",
        poolclass=pool.NullPool,
    )

    with connectable.connect() as connection:
        context.configure(
            connection=connection,
            target_metadata=Base.metadata,
            compare_type=True,
            compare_server_default=True
        )

        with context.begin_transaction():
            context.run_migrations()
```

**Migration Workflow:**
1. Generate migration: `alembic revision --autogenerate -m "description"`
2. Review generated migration file
3. Test migration on staging database
4. Apply to production during deployment
5. Rollback plan: `alembic downgrade -1`

### Monitoring and Alerting

**Prometheus Metrics:**
```python
from prometheus_client import Counter, Histogram, Gauge

# Request metrics
http_requests_total = Counter(
    'http_requests_total',
    'Total HTTP requests',
    ['method', 'endpoint', 'status']
)

http_request_duration_seconds = Histogram(
    'http_request_duration_seconds',
    'HTTP request latency',
    ['method', 'endpoint']
)

# Business metrics
tasks_completed_total = Counter(
    'tasks_completed_total',
    'Total tasks completed',
    ['verification_source']
)

synergy_calculations_total = Counter(
    'synergy_calculations_total',
    'Total synergy calculations'
)

active_users = Gauge(
    'active_users',
    'Number of active users'
)

# MCP metrics
mcp_requests_total = Counter(
    'mcp_requests_total',
    'Total MCP requests',
    ['source', 'status']
)

mcp_request_duration_seconds = Histogram(
    'mcp_request_duration_seconds',
    'MCP request latency',
    ['source']
)
```

**Grafana Dashboards:**
1. **System Health Dashboard:**
   - Service uptime
   - Request rate and latency
   - Error rate
   - Database connection pool usage
   - Cache hit rate

2. **Business Metrics Dashboard:**
   - Daily active users
   - Tasks completed per day
   - Synergy calculations per hour
   - Chat unlocks per day
   - MCP verification success rate

3. **Performance Dashboard:**
   - API endpoint latency (p50, p95, p99)
   - Database query performance
   - Cache performance
   - Queue depth and processing time

**Alert Rules:**
```yaml
groups:
  - name: service_alerts
    rules:
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected"
          
      - alert: HighLatency
        expr: histogram_quantile(0.95, http_request_duration_seconds) > 2
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High API latency detected"
          
      - alert: DatabaseConnectionPoolExhausted
        expr: database_connections_in_use / database_connections_max > 0.9
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Database connection pool near capacity"
          
      - alert: MCPServiceDown
        expr: up{job="mcp-client"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "MCP service is down"
```

### Security Measures

**1. Network Security:**
- VPC with private subnets for services
- Security groups restricting inbound traffic
- NAT Gateway for outbound internet access
- VPC endpoints for AWS services (no internet routing)

**2. Application Security:**
- JWT with RS256 (asymmetric keys)
- OAuth tokens encrypted at rest (AES-256-GCM)
- Input validation on all endpoints
- SQL injection prevention (parameterized queries)
- XSS prevention (output encoding)
- CSRF tokens for state-changing operations
- Rate limiting per user and IP

**3. Data Security:**
- Database encryption at rest (AWS KMS)
- TLS 1.3 for all connections
- Secrets stored in AWS Secrets Manager
- Regular security audits
- Automated vulnerability scanning

**4. Compliance:**
- GDPR compliance (data export, deletion)
- Audit logs for all data access
- Data retention policies
- Privacy policy enforcement

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property 1: User Registration Creates Unique Accounts
*For any* valid registration credentials, creating a user account should result in a unique user identifier being assigned and the account being stored in the database.
**Validates: Requirements 1.1**

### Property 2: Authentication Issues Valid Toke
