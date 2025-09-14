# AGENTS.md
This file provides guidance to various AI agents when working with code in this repository.

## Common Commands

### Backend Development
- Build backend: `go build -o main`
- Run development server: `go run main.go`
- Run with specific port: `go run main.go --port 3001`
- Enable debug mode: `GIN_MODE=debug go run main.go`

### Frontend Development
- Install dependencies: `cd web && npm install` or `cd web && pnpm install`
- Build frontend: `cd web && npm run build` or `cd web && pnpm build`
- Start development server: `cd web && npm run dev`
- Lint frontend: `cd web && npm run lint`

### Docker Deployment
- Build and start: `docker-compose up -d`
- View logs: `docker-compose logs -f`
- Stop services: `docker-compose down`

### Database Operations
- The project supports SQLite (default), MySQL, and PostgreSQL
- Database migration happens automatically on startup
- For SQLite: Data is stored in `./data/veloera.db` or `./veloera.db`

## High-Level Architecture

Veloera is an AI API gateway system that provides unified access to multiple AI model providers through a single OpenAI-compatible API interface.

### Core Architecture
- **Backend**: Go-based API server using Gin web framework
- **Frontend**: React-based admin interface using Semi UI components
- **Database**: Multi-database support (SQLite, MySQL, PostgreSQL) with GORM ORM
- **Cache**: Redis support with in-memory fallback option
- **Architecture**: Monolithic with clear separation of concerns

### Directory Structure

#### Root Level
- `main.go` - Application entry point
- `go.mod` - Go module dependencies
- `docker-compose.yml` - Docker containerization setup
- `Makefile` - Build automation
- `VELOERA_PROJ` - Project identification file (must not be modified)

#### Core Packages
- `common/` - Shared utilities, logging, configuration
- `controller/` - HTTP request handlers and business logic
- `model/` - Database models and data access layer
- `dto/` - Data transfer objects for API requests/responses
- `relay/` - Request routing and provider adapter pattern
- `router/` - HTTP route definitions
- `middleware/` - HTTP middleware (CORS, authentication, rate limiting)
- `service/` - Business logic services
- `setting/` - Configuration management
- `constant/` - Application constants

#### Provider Adapters (`relay/channel/`)
Each AI provider has its own subdirectory with adapter implementation:
- `openai/` - OpenAI API compatibility
- `claude/` - Anthropic Claude models
- `gemini/` - Google Gemini models
- `azure/` - Microsoft Azure OpenAI
- `baidu/` - Baidu AI services
- `ali/` - Alibaba Cloud AI
- And many others...

#### Frontend (`web/`)
- React-based admin interface
- Semi UI component library
- Vite build system
- Multi-language support

### Key Components

#### Channel Management
- **Channels**: Individual AI provider configurations with API keys, base URLs, model mappings
- **Model Mapping**: Virtual model names mapped to actual provider models
- **Load Balancing**: Weighted random selection across multiple channels
- **Status Monitoring**: Automatic health checking and failover

#### Authentication & Authorization
- **Token-based**: API keys for authentication
- **User Management**: Role-based access control
- **Rate Limiting**: Per-user and per-token rate limiting
- **OAuth Support**: Telegram, GitHub, OIDC integration

#### Request Processing Pipeline
1. **Authentication**: Token validation and user identification
2. **Rate Limiting**: Request quota validation
3. **Channel Selection**: Load balancing based on priority and availability
4. **Request Transformation**: Convert OpenAI format to provider-specific format
5. **Response Processing**: Convert provider response back to OpenAI format
6. **Usage Tracking**: Token counting and quota management

### Database Schema

#### Core Tables
- `users` - User accounts and settings
- `channels` - AI provider configurations
- `tokens` - API keys for authentication
- `options` - System configuration settings
- `logs` - Request/response logging
- `abilities` - Channel capability mapping

#### Specialized Tables
- `redemptions` - Gift code management
- `midjourney` - Midjourney task tracking
- `quota_data` - Usage analytics
- `global_model_mapping` - Virtual model mappings

### Configuration

#### Environment Variables
- `SQL_DSN` - Database connection string
- `REDIS_CONN_STRING` - Redis connection
- `SESSION_SECRET` - Session encryption key
- `FRONTEND_BASE_URL` - Frontend URL for multi-node deployments
- `NODE_TYPE` - Node type (master/slave) for multi-node setups

#### Channel Types
- OpenAI-compatible providers
- Claude (Anthropic)
- Gemini (Google)
- Azure OpenAI
- Baidu, Ali, Tencent (Chinese providers)
- And 20+ other providers

### Development Guidelines

#### Go Code Style
- Follow standard Go conventions
- Use GORM for database operations
- Implement proper error handling with `common.LogError` and `common.SysError`
- Use structured logging with context
- Follow the adapter pattern for provider integrations

#### Frontend Guidelines
- Use Semi UI components consistently
- Single quotes for strings and JSX
- Implement proper error handling and loading states
- Follow the established routing patterns

#### API Compatibility
- Maintain OpenAI API compatibility
- Use standardized request/response formats
- Implement proper streaming support
- Include usage statistics in responses

### License Compliance
This project is licensed under GPL v3 with additional terms:
- Must retain "Powered by Veloera" attribution
- Must preserve the VELOERA_PROJ file unchanged
- Must maintain the `/veloera` route functionality