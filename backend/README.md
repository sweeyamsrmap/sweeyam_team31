# Agentic AI Backend - Python FastAPI

A high-performance Python FastAPI backend for the Agentic AI Platform, providing intelligent goal planning, task execution, memory management, and AI-driven reflection capabilities.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Server](#running-the-server)
- [API Routes](#api-routes)
- [Agent Components](#agent-components)
- [Database Schema](#database-schema)
- [Development Guide](#development-guide)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)

## Overview

The Agentic AI Backend provides a REST API for an intelligent automation system that:
- Understands and decomposes user goals into actionable tasks
- Generates intelligent, prioritized task plans
- Executes tasks through n8n workflow integration
- Manages multiple memory types (short-term, long-term, episodic)
- Reflects on execution results and generates improvement insights
- Continuously learns and improves from past executions

Built with modern async Python best practices, the backend handles concurrent requests efficiently while maintaining data integrity through MongoDB transactions.

## 🎯 Features

- **Goal Decomposition**: Break complex goals into manageable sub-goals and tasks
- **Intelligent Planning**: Create structured, prioritized task plans with dependencies
- **Async Task Execution**: Execute tasks via n8n workflows with progress tracking
- **Memory Management**: 
  - Short-term (working memory)
  - Long-term (historical data with semantic search)
  - Episodic (event-based memories with timestamps)
- **Self-Reflection**: AI-powered analysis of execution results
- **User Authentication**: JWT-based secure user management
- **Real-time Status**: Track task execution progress
- **Error Handling**: Robust error recovery and logging
- **CORS Support**: Secure cross-origin requests

## Tech Stack

### Core Framework
- **FastAPI 0.104.1** - Modern, fast web framework with automatic docs
- **Uvicorn 0.24.0** - ASGI server for production deployment
- **Pydantic 2.5.0** - Data validation and serialization
- **Pydantic Settings 2.1.0** - Configuration management

### Database
- **Motor 3.3.2** - Async MongoDB driver
- **PyMongo 4.6.0** - MongoDB Python driver

### HTTP & Async
- **HTTPX 0.25.2** - Async HTTP client for external APIs

### Security & Authentication
- **PyJWT 2.8.1** - JWT token generation and validation
- **Bcrypt 4.1.1** - Password hashing with salt

### Utilities
- **Python-dotenv 1.0.0** - Environment variable management
- **Python-multipart 0.0.6** - Form data parsing

## Project Structure

```
backend/
├── app/
│   ├── __init__.py                    # Package initialization
│   ├── main.py                        # FastAPI application entry point
│   │
│   ├── api/                           # API route handlers
│   │   ├── __init__.py
│   │   ├── agent.py                   # Agent endpoints (goals, plans, tasks)
│   │   └── health.py                  # Health check endpoint
│   │
│   ├── agents/                        # Core agent components
│   │   ├── __init__.py
│   │   ├── planner.py                 # Goal planner (goals → tasks)
│   │   ├── executor.py                # Task executor (n8n integration)
│   │   ├── reflector.py               # Reflection engine (analysis & insights)
│   │   ├── memory.py                  # Memory management system
│   │   └── __pycache__/
│   │
│   ├── auth/                          # Authentication & authorization
│   │   ├── __init__.py
│   │   ├── controller.py              # Auth business logic
│   │   ├── middleware.py              # JWT verification middleware
│   │   ├── routes.py                  # Auth endpoints (login, signup)
│   │   ├── utils.py                   # Helper utilities
│   │   └── __pycache__/
│   │
│   ├── core/                          # Core configuration & database
│   │   ├── __init__.py
│   │   ├── config.py                  # Settings and configuration
│   │   ├── database.py                # MongoDB connection and initialization
│   │   └── __pycache__/
│   │
│   ├── models/                        # Data models & schemas
│   │   ├── __init__.py
│   │   ├── schemas.py                 # Pydantic request/response schemas
│   │   ├── user.py                    # User model
│   │   └── __pycache__/
│   │
│   ├── services/                      # External service integrations
│   │   ├── __init__.py
│   │   ├── huggingface.py             # Hugging Face LLM API client
│   │   ├── n8n.py                     # n8n workflow integration
│   │   └── __pycache__/
│   │
│   └── __pycache__/
│
├── requirements.txt                   # Python dependencies
├── .env.example                       # Example environment variables
├── .gitignore                         # Git ignore rules
├── README.md                          # This file
└── agents/                            # (Legacy/future use)
```

## Prerequisites

### Required Software
- **Python 3.10+** (3.11 or 3.12 recommended)
- **MongoDB 5.0+** (local or cloud instance)
- **Hugging Face Account** with API key
- **pip** (Python package manager)

### Required Services (External)
- **n8n instance** - For workflow execution
- **Hugging Face Inference API** - For LLM capabilities

### Optional Tools
- **Virtual Environment Tool**: `venv` (built-in) or `poetry`/`conda`
- **Database Management**: MongoDB Compass, mongosh
- **API Testing**: Postman, Thunder Client

## Installation

### Step 1: Navigate to Backend Directory

```bash
cd backend
```

### Step 2: Create Virtual Environment (Recommended)

```bash
# Using built-in venv
python -m venv venv

# Activate virtual environment
# On Windows (Command Prompt)
venv\Scripts\activate

# On Windows (PowerShell)
venv\Scripts\Activate.ps1

# On macOS/Linux
source venv/bin/activate
```

You should see `(venv)` prefix in your terminal.

### Step 3: Upgrade pip and setuptools

```bash
pip install --upgrade pip setuptools wheel
```

### Step 4: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 5: Verify Installation

```bash
# Check Python version
python --version

# List installed packages
pip list

# Test imports
python -c "import fastapi, motor, httpx; print('✅ All imports successful')"
```

## Configuration

### Environment Variables

Create `.env` file in the backend directory:

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

```env
# ==========================================
# FastAPI & Server Configuration
# ==========================================
PORT=5000                               # Server port
HOST=0.0.0.0                           # Bind to all interfaces
ENVIRONMENT=development                 # development|staging|production
DEBUG=true                             # Enable debug mode (false in production)

# ==========================================
# Database Configuration (MongoDB)
# ==========================================
# Local MongoDB
MONGODB_URI=mongodb://localhost:27017/agentic_ai

# OR MongoDB Atlas Cloud (recommended)
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/agentic_ai

# Optional: Separate read/write endpoints
# MONGODB_READ_URI=...
# MONGODB_WRITE_URI=...

# ==========================================
# Authentication & JWT
# ==========================================
JWT_SECRET=your_super_secret_key_change_this_in_production
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24

# ==========================================
# Hugging Face LLM Configuration
# ==========================================
# Get from: https://huggingface.co/settings/tokens
HF_API_KEY=hf_your_token_here

# Alternative name (some code may use this)
HUGGINGFACE_API_KEY=hf_your_token_here

# LLM Model selection
# Recommended: meta-llama/Llama-2-7b-chat
# Others: mistralai/Mistral-7B-Instruct-v0.1, tiiuae/falcon-7b-instruct
HF_MODEL=meta-llama/Llama-2-7b-chat

# Inference parameters
HF_TEMPERATURE=0.7
HF_MAX_TOKENS=512
HF_TOP_P=0.95

# ==========================================
# n8n Integration
# ==========================================
# Local n8n webhook
N8N_WEBHOOK_URL=http://localhost:5678/webhook/agent

# n8n API base URL
N8N_BASE_URL=http://localhost:5678

# n8n API key (if required)
# N8N_API_KEY=your_n8n_api_key

# ==========================================
# CORS Configuration
# ==========================================
# Comma-separated list of allowed origins
CORS_ORIGINS=http://localhost:3000,http://localhost:5173,http://localhost:8000

# ==========================================
# Logging Configuration
# ==========================================
LOG_LEVEL=INFO                         # DEBUG|INFO|WARNING|ERROR|CRITICAL
LOG_FORMAT=json                        # json|text
```

### Environment Profiles

#### Development
```env
ENVIRONMENT=development
DEBUG=true
LOG_LEVEL=DEBUG
```

#### Staging
```env
ENVIRONMENT=staging
DEBUG=false
LOG_LEVEL=INFO
```

#### Production
```env
ENVIRONMENT=production
DEBUG=false
LOG_LEVEL=WARNING
JWT_SECRET=<very_strong_random_secret>
MONGODB_URI=<secure_production_uri>
```

## Running the Server

### Development Mode (with auto-reload)

```bash
# Method 1: Using uvicorn with reload
uvicorn app.main:app --reload --host 0.0.0.0 --port 5000

# Method 2: Using uvicorn with custom config
uvicorn app.main:app --reload --port 5000 --log-level info

# Method 3: From root directory
cd ..
npm run dev:backend
```

The server will be available at:
- **API**: http://localhost:5000
- **Docs (Swagger UI)**: http://localhost:5000/docs
- **ReDoc**: http://localhost:5000/redoc

### Production Mode

```bash
# Using uvicorn without reload
uvicorn app.main:app --host 0.0.0.0 --port 5000 --workers 4

# With gunicorn (higher performance)
pip install gunicorn
gunicorn app.main:app --workers 4 --worker-class uvicorn.workers.UvicornWorker
```

### Production with Docker

```bash
# Build image
docker build -t agentic-ai-backend .

# Run container
docker run -p 5000:5000 --env-file .env agentic-ai-backend
```

## API Routes

### Health Check

```
GET /api/health
```

Check system health status.

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2026-01-25T10:30:00Z",
  "services": {
    "database": "connected",
    "llm": "available",
    "n8n": "connected"
  }
}
```

### Authentication Routes

#### Sign Up
```
POST /api/auth/signup
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword123",
  "name": "John Doe"
}
```

**Response:** `201 Created`
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "token_type": "bearer",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

#### Login
```
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword123"
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "token_type": "bearer"
}
```

### Agent Routes

#### Create Goal
```
POST /api/agent/goals
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Learn React Fundamentals",
  "description": "Master React basics including hooks and state management",
  "targetDate": "2026-02-25",
  "priority": "high"
}
```

**Response:** `201 Created`
```json
{
  "id": "507f1f77bcf86cd799439011",
  "userId": "507f1f77bcf86cd799439010",
  "title": "Learn React Fundamentals",
  "description": "Master React basics...",
  "status": "planning",
  "priority": "high",
  "targetDate": "2026-02-25T00:00:00Z",
  "createdAt": "2026-01-25T10:30:00Z",
  "updatedAt": "2026-01-25T10:30:00Z",
  "plan": {
    "subGoals": [...],
    "tasks": [...]
  }
}
```

#### Get All Goals
```
GET /api/agent/goals
Authorization: Bearer <token>
```

**Query Parameters:**
- `skip`: Number of results to skip (default: 0)
- `limit`: Number of results to return (default: 10)
- `status`: Filter by status (planning|in_progress|completed|failed)

**Response:** `200 OK`
```json
{
  "goals": [...],
  "total": 5,
  "skip": 0,
  "limit": 10
}
```

#### Get Goal Details
```
GET /api/agent/goals/{goalId}
Authorization: Bearer <token>
```

**Response:** `200 OK`
```json
{
  "id": "507f1f77bcf86cd799439011",
  "title": "Learn React Fundamentals",
  "status": "in_progress",
  "progress": 35,
  "plan": {
    "subGoals": [...],
    "tasks": [...]
  },
  "executionHistory": [...]
}
```

#### Update Goal Status
```
PATCH /api/agent/goals/{goalId}
Authorization: Bearer <token>
Content-Type: application/json

{
  "status": "in_progress",
  "notes": "Started learning"
}
```

#### Get Memory
```
GET /api/agent/memory
Authorization: Bearer <token>
```

**Query Parameters:**
- `type`: "short_term" | "long_term" | "episodic"
- `limit`: Number of results (default: 10)
- `query`: Search term for semantic search

**Response:** `200 OK`
```json
{
  "memories": [
    {
      "id": "...",
      "type": "episodic",
      "content": "Successfully completed React tutorial",
      "timestamp": "2026-01-25T10:30:00Z",
      "metadata": {
        "relatedGoal": "507f...",
        "importance": "high"
      }
    }
  ],
  "total": 25
}
```

#### Store Memory
```
POST /api/agent/memory
Authorization: Bearer <token>
Content-Type: application/json

{
  "type": "episodic",
  "content": "Completed React hooks tutorial successfully",
  "metadata": {
    "relatedGoal": "507f1f77bcf86cd799439011",
    "importance": "high"
  }
}
```

#### Trigger Reflection
```
POST /api/agent/reflect
Authorization: Bearer <token>
Content-Type: application/json

{
  "goalId": "507f1f77bcf86cd799439011",
  "focusArea": "execution_efficiency"
}
```

**Response:** `200 OK`
```json
{
  "insights": [
    "Tasks estimated at 2 hours took 3 hours on average",
    "Sequential tasks can be parallelized for better efficiency",
    "Pattern: Complex tasks have 40% higher failure rate"
  ],
  "suggestions": [
    "Add 30% time buffer to task estimates",
    "Review task dependencies for parallelization opportunities",
    "Add pre-execution validation for complex tasks"
  ],
  "generatedAt": "2026-01-25T10:30:00Z"
}
```

## Agent Components

### 1. Planner (`agents/planner.py`)

Converts user goals into structured task plans.

**Responsibilities:**
- Analyze goal description and requirements
- Generate sub-goals with hierarchical structure
- Create detailed task list
- Assign priorities based on dependencies
- Estimate task duration and resource requirements

**Key Functions:**
```python
async def plan_goal(goal: GoalSchema) -> PlanSchema
async def update_plan(goal_id: str, feedback: str) -> PlanSchema
async def estimate_task_duration(task: TaskSchema) -> float
```

### 2. Executor (`agents/executor.py`)

Manages task execution through n8n workflows.

**Responsibilities:**
- Select appropriate n8n workflows
- Trigger task execution with parameters
- Monitor execution status and progress
- Handle task retries on failure
- Collect execution results

**Key Functions:**
```python
async def execute_task(task: TaskSchema) -> ExecutionResultSchema
async def get_execution_status(execution_id: str) -> StatusSchema
async def handle_task_failure(task_id: str, error: str) -> RecoverySchema
```

### 3. Reflector (`agents/reflector.py`)

Analyzes execution outcomes and generates insights.

**Responsibilities:**
- Analyze task execution results
- Compare estimates vs actual performance
- Identify patterns and bottlenecks
- Generate improvement suggestions
- Update memory with learnings

**Key Functions:**
```python
async def reflect_on_execution(goal_id: str) -> InsightsSchema
async def generate_suggestions(analysis: AnalysisSchema) -> List[str]
async def identify_patterns(executions: List[ExecutionSchema]) -> PatternsSchema
```

### 4. Memory Manager (`agents/memory.py`)

Manages multiple types of memories for learning.

**Memory Types:**

- **Short-term Memory**: Current working session data
  - Immediate task context
  - Current goal information
  - Active execution state
  - Expires: End of session

- **Long-term Memory**: Historical learning data
  - Past goal outcomes
  - Task patterns and durations
  - Learned heuristics
  - Never expires (searchable)

- **Episodic Memory**: Event-based memories
  - Specific execution events
  - Significant achievements
  - Critical failures
  - Timestamped with context

**Key Functions:**
```python
async def store_memory(memory: MemorySchema) -> str
async def retrieve_memories(type: str, limit: int) -> List[MemorySchema]
async def search_memory(query: str) -> List[MemorySchema]
async def update_memory_relevance(memory_id: str, relevance: float) -> None
```

## Database Schema

### Collections

#### users
```json
{
  "_id": ObjectId,
  "email": "string",
  "password_hash": "string (bcrypt)",
  "name": "string",
  "created_at": "datetime",
  "updated_at": "datetime",
  "is_active": "boolean"
}
```

#### goals
```json
{
  "_id": ObjectId,
  "user_id": ObjectId,
  "title": "string",
  "description": "string",
  "status": "planning|in_progress|completed|failed",
  "priority": "low|medium|high",
  "target_date": "datetime",
  "created_at": "datetime",
  "updated_at": "datetime",
  "plan": {
    "sub_goals": [
      {
        "id": "string",
        "title": "string",
        "tasks": [
          {
            "id": "string",
            "title": "string",
            "description": "string",
            "estimated_duration": "float",
            "priority": "integer",
            "dependencies": ["string"],
            "status": "pending|in_progress|completed|failed"
          }
        ]
      }
    ]
  },
  "execution_history": ["ObjectId"]
}
```

#### tasks_execution
```json
{
  "_id": ObjectId,
  "goal_id": ObjectId,
  "task_id": "string",
  "user_id": ObjectId,
  "n8n_execution_id": "string",
  "status": "pending|running|completed|failed",
  "started_at": "datetime",
  "completed_at": "datetime",
  "result": "object",
  "error": "string",
  "retry_count": "integer"
}
```

#### memories
```json
{
  "_id": ObjectId,
  "user_id": ObjectId,
  "type": "short_term|long_term|episodic",
  "content": "string",
  "metadata": {
    "related_goal": "ObjectId",
    "related_task": "string",
    "importance": "low|medium|high",
    "tags": ["string"]
  },
  "created_at": "datetime",
  "updated_at": "datetime",
  "relevance_score": "float"
}
```

## Development Guide

### Adding New Agent Components

1. **Create new module** in `app/agents/`:
   ```python
   # app/agents/new_component.py
   from app.core.config import settings
   from app.core.database import get_db
   
   class NewComponent:
       def __init__(self, db):
           self.db = db
       
       async def process(self, data):
           # Implementation
           return result
   ```

2. **Register in main.py**:
   ```python
   from app.agents.new_component import NewComponent
   new_component = NewComponent(db)
   ```

3. **Create API endpoint** in `app/api/agent.py`:
   ```python
   @router.post("/new-endpoint")
   async def new_endpoint(data: dict, token: str = Depends(verify_token)):
       result = await agent.process(data)
       return result
   ```

### Testing

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_planner.py

# Run with coverage
pytest --cov=app tests/

# Run specific test
pytest tests/test_planner.py::test_goal_decomposition
```

### Code Style

- **PEP 8** compliance
- **Type hints** on all functions
- **Docstrings** for classes and modules
- **Black formatter** for consistency

```bash
# Format code
black app/

# Check style
flake8 app/

# Type checking
mypy app/
```

## Deployment

### Docker Deployment

```dockerfile
FROM python:3.10-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY app/ ./app/

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import requests; requests.get('http://localhost:5000/api/health')"

# Run application
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "5000"]
```

### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: agentic-ai-backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: agentic-ai-backend
  template:
    metadata:
      labels:
        app: agentic-ai-backend
    spec:
      containers:
      - name: backend
        image: agentic-ai-backend:latest
        ports:
        - containerPort: 5000
        env:
        - name: MONGODB_URI
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: uri
        - name: HF_API_KEY
          valueFrom:
            secretKeyRef:
              name: api-keys
              key: huggingface
        livenessProbe:
          httpGet:
            path: /api/health
            port: 5000
          initialDelaySeconds: 10
          periodSeconds: 30
```

## Troubleshooting

### Port Already in Use

```bash
# Windows PowerShell
Get-Process -Id (Get-NetTCPConnection -LocalPort 5000).OwningProcess | Stop-Process -Force

# macOS/Linux
lsof -ti:5000 | xargs kill -9
```

### ModuleNotFoundError

```bash
# Ensure virtual environment is activated
# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate

# Reinstall dependencies
pip install -r requirements.txt
```

### MongoDB Connection Error

```bash
# Check MongoDB is running
mongosh

# Verify MONGODB_URI in .env is correct
# Test connection manually
python -c "from motor.motor_asyncio import AsyncMongoClient; print('Connection OK')"
```

### Hugging Face API Errors

```bash
# Verify API key is valid
# Check model name is correct
# Monitor quota at https://huggingface.co/settings/tokens
```

### n8n Webhook Issues

```bash
# Check n8n is running
curl http://localhost:5678/api/health

# Verify webhook URL in .env matches n8n configuration
# Check n8n logs for webhook trigger details
```

## Performance Tuning

- **Connection Pooling**: Motor handles this automatically
- **Indexing**: Add MongoDB indexes on frequently queried fields
- **Caching**: Implement Redis for frequently accessed data
- **Async/Await**: Use throughout for non-blocking I/O

## Security Best Practices

- Never commit `.env` files
- Use strong JWT secrets (32+ chars)
- Enable HTTPS in production
- Rate limit API endpoints
- Validate all user inputs with Pydantic
- Use environment-specific configurations
- Regularly update dependencies

## Monitoring and Logging

- All requests logged with timestamps
- Error tracking with full stack traces
- Performance metrics for API endpoints
- Database operation logging
- n8n webhook integration logging

## Support & Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Motor Documentation](https://motor.readthedocs.io/)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Hugging Face API](https://huggingface.co/inference-api)
- [n8n Documentation](https://docs.n8n.io/)

---

**Last Updated:** January 2026
**Version:** 1.0.0
**Maintainer:** Development Team
```bash
uvicorn app.main:app --host 0.0.0.0 --port 5000
```

Or using Python directly:
```bash
python -m app.main
```

The server will start at `http://localhost:5000`

## API Endpoints

### Base URL
```
http://localhost:5000/api
```

### 1. Health Check

**GET** `/health`

Check if the backend is running.

**Response:**
```json
{
  "status": "ok",
  "timestamp": "2024-01-15T10:00:00.000Z"
}
```

### 2. Execute Full Agent Loop

**POST** `/agent/execute`

Execute a complete agent cycle: planning, execution, reflection, and re-planning.

**Request:**
```json
{
  "goal": "Learn React by building a todo app",
  "userId": "user-123"
}
```

**Response:**
```json
{
  "goalId": "550e8400-e29b-41d4-a716-446655440000",
  "goal": "Learn React by building a todo app",
  "plan": {
    "goalId": "...",
    "goal": "...",
    "plan": {
      "goal": "...",
      "subGoals": [...],
      "tasks": [...],
      "timeline": "...",
      "successCriteria": [...]
    },
    "createdAt": "..."
  },
  "executionResults": [
    {
      "taskId": "task-1",
      "success": true,
      "result": {...},
      "tool": "task-executor",
      "executedAt": "..."
    }
  ],
  "reflection": {
    "goalId": "...",
    "reflection": {
      "overallSuccess": true,
      "successRate": 85.5,
      "keyInsights": [...],
      "failures": [],
      "improvements": [...],
      "recommendations": [...]
    },
    "reflectedAt": "..."
  },
  "replan": null,
  "executionLog": [...],
  "summary": {
    "totalTasks": 8,
    "successCount": 7,
    "failureCount": 1,
    "successRate": "87.5%"
  }
}
```

### 3. Get Plan Only

**POST** `/agent/plan`

Generate a plan without executing tasks.

**Request:**
```json
{
  "goal": "Create a study plan for machine learning fundamentals",
  "userId": "user-123"
}
```

**Response:**
```json
{
  "goalId": "660e8400-e29b-41d4-a716-446655440001",
  "plan": {
    "goalId": "...",
    "goal": "...",
    "plan": {
      "goal": "...",
      "subGoals": [...],
      "tasks": [...],
      "timeline": "...",
      "successCriteria": [...]
    },
    "createdAt": "..."
  }
}
```

### 4. Execute Specific Task

**POST** `/agent/task/execute`

Execute a single task with custom parameters.

**Request:**
```json
{
  "task": {
    "id": "task-123",
    "title": "Send daily reminder",
    "description": "Send reminder for workout",
    "tool": "notification"
  },
  "userId": "user-123",
  "goalId": "550e8400-e29b-41d4-a716-446655440000"
}
```

**Response:**
```json
{
  "taskId": "task-123",
  "success": true,
  "result": {
    "success": true,
    "data": {...}
  },
  "tool": "notification",
  "executedAt": "2024-01-15T18:00:05Z"
}
```

### 5. Get Memories

**GET** `/agent/memories/{goalId}`

Retrieve stored memories for a goal.

**Query Parameters:**
- `type` (optional): Filter by memory type (`short-term`, `long-term`, `episodic`)
- `limit` (optional): Maximum number of memories (default: 50)

**Example:**
```
GET /api/agent/memories/550e8400-e29b-41d4-a716-446655440000?type=episodic&limit=20
```

**Response:**
```json
{
  "goalId": "550e8400-e29b-41d4-a716-446655440000",
  "memories": [
    {
      "_id": "mem-123",
      "userId": "user-123",
      "goalId": "550e8400-e29b-41d4-a716-446655440000",
      "memoryType": "episodic",
      "content": {
        "task": "Install Node.js and npm",
        "action": "task-executor",
        "result": {...}
      },
      "outcome": "success",
      "timestamp": "2024-01-15T10:30:00Z"
    }
  ],
  "count": 1
}
```

### 6. Get Reflection

**POST** `/agent/reflect/{goalId}`

Analyze execution results and get insights.

**Request:**
```json
{
  "executionResults": [
    {
      "taskId": "task-1",
      "success": true,
      "result": {...}
    },
    {
      "taskId": "task-2",
      "success": false,
      "error": "Dependency not met"
    }
  ],
  "userId": "user-123"
}
```

**Response:**
```json
{
  "goalId": "550e8400-e29b-41d4-a716-446655440000",
  "reflection": {
    "overallSuccess": false,
    "successRate": 50.0,
    "keyInsights": [...],
    "failures": [
      {
        "task": "task-2",
        "reason": "Dependency not met",
        "suggestion": "..."
      }
    ],
    "improvements": [...],
    "recommendations": [...]
  },
  "reflectedAt": "2024-01-15T11:00:00Z"
}
```

### 7. Get User Patterns

**GET** `/agent/patterns/{userId}`

Analyze patterns across multiple goal executions for a user.

**Response:**
```json
{
  "userId": "user-123",
  "analysis": {
    "commonFailures": [...],
    "successfulPatterns": [...],
    "userPreferences": [...],
    "recommendations": [...]
  },
  "analyzedAt": "2024-01-15T12:00:00Z"
}
```

## Error Responses

### 400 Bad Request
```json
{
  "error": "Goal is required"
}
```

### 500 Internal Server Error
```json
{
  "error": "LLM generation failed: Model is loading",
  "stack": "..."
}
```

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Server port | `5000` |
| `HOST` | Server host | `0.0.0.0` |
| `NODE_ENV` | Environment mode | `development` |
| `HF_API_KEY` | Hugging Face API key | Required |
| `HUGGINGFACE_API_KEY` | Alternative name for HF_API_KEY | Required |
| `HF_MODEL` | Hugging Face model name | `meta-llama/Meta-Llama-3-8B-Instruct` |
| `MONGODB_URI` | MongoDB connection string | `mongodb://localhost:27017/agentic_ai` |
| `N8N_WEBHOOK_URL` | n8n webhook URL | `http://localhost:5678/webhook/agent` |
| `N8N_WEBHOOK_BASE_URL` | Alternative name for N8N_WEBHOOK_URL | Same as above |

## Development

### Code Structure

- **Async/Await**: All database and HTTP operations use async/await
- **Error Handling**: Comprehensive error handling with proper HTTP status codes
- **Logging**: Structured logging throughout the application
- **Type Hints**: Full type hints for better code quality

### Testing API Endpoints

You can test the API using curl, Postman, or the frontend application.

**Example with curl:**
```bash
# Health check
curl http://localhost:5000/health

# Execute agent
curl -X POST http://localhost:5000/api/agent/execute \
  -H "Content-Type: application/json" \
  -d '{
    "goal": "Learn React by building a todo app",
    "userId": "user-123"
  }'
```

**Example with Python:**
```python
import requests

response = requests.post(
    'http://localhost:5000/api/agent/execute',
    json={
        'goal': 'Learn React by building a todo app',
        'userId': 'user-123'
    }
)
print(response.json())
```

## Migration Notes

This backend is a direct migration from Node.js + Express to Python + FastAPI:

- ✅ All API endpoints match exactly
- ✅ Request/response formats are identical
- ✅ Same environment variable names
- ✅ Same MongoDB schema
- ✅ Same n8n webhook integration
- ✅ Same error handling behavior

The frontend should work without any modifications.

## Troubleshooting

### MongoDB Connection Issues

If MongoDB connection fails:
1. Ensure MongoDB is running
2. Check `MONGODB_URI` in `.env`
3. Verify network connectivity

### Hugging Face API Errors

If LLM calls fail:
1. Verify `HF_API_KEY` is set correctly
2. Check API quota/limits
3. Ensure model name is correct

### n8n Webhook Errors

If n8n webhooks fail:
1. Ensure n8n is running
2. Verify webhook URL is correct
3. Check that workflow is activated in n8n
4. See n8n logs for details

## License

Same as the main project.
