# 🤖 Agentic AI Platform

A production-ready Autonomous Agentic AI system that helps users achieve personalized learning or workflow goals through intelligent goal decomposition, task planning, automated execution, and continuous learning.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Development Guide](#development-guide)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Overview

The Agentic AI Platform is an intelligent automation system designed to help users achieve their goals through AI-powered planning, execution, and continuous learning. Whether you're looking to learn new skills, streamline workflows, or automate repetitive tasks, this platform decomposes your goals into actionable tasks, executes them intelligently, and learns from the results to improve future executions.

### Key Capabilities

- **Intelligent Goal Decomposition**: Break down complex goals into manageable sub-goals and tasks
- **Adaptive Planning**: Create prioritized, dependency-aware task plans
- **Automated Execution**: Execute tasks through integrated n8n workflows
- **Persistent Learning**: Store and retrieve memories from past executions
- **Self-Reflection**: Analyze outcomes and generate insights for improvement
- **Dynamic Re-planning**: Adjust plans based on execution results and feedback

## 🎯 Features

- **Goal Understanding**: Interprets user goals and breaks them into executable tasks using LLM analysis
- **Intelligent Planning**: Creates structured plans with priorities, dependencies, and time estimates
- **Multi-Tool Execution**: Integrates with n8n workflows for flexible task automation
- **Persistent Memory**: Stores short-term (working), long-term (historical), and episodic (event-based) memories
- **Self-Reflection**: Analyzes outcomes and generates insights for future improvements
- **Dynamic Re-planning**: Adjusts plans based on execution results and user feedback
- **Real-time Monitoring**: Track execution progress with detailed logs and status updates
- **User Authentication**: Secure login and signup with JWT tokens
- **Ethical Safeguards**: Built-in safety limits and transparency features

## 🏗️ Architecture

The platform follows a modular, layered architecture:

```
┌─────────────────────────────────────────┐
│      Frontend (React + Vite)            │
│  - Dashboard, Goal Input, Task Manager  │
└──────────────────┬──────────────────────┘
                   │ REST API
┌──────────────────▼──────────────────────┐
│    Backend (FastAPI + Python)           │
│  - Agent Orchestration                  │
│  - API Routes & Authentication          │
└──────────────────┬──────────────────────┘
       ┌───────────┬───────────┬────────────┐
       │           │           │            │
   ┌───▼──┐  ┌────▼───┐  ┌───▼───┐  ┌───▼────┐
   │Memory│  │Planner │  │Executor│  │Reflector│
   └──────┘  └────────┘  └────────┘  └────────┘
       │           │           │            │
   ┌───▼──────────▼───────────▼───────────▼──┐
   │  Services Layer                          │
   │  - Hugging Face LLM API                  │
   │  - n8n Webhook Integration               │
   │  - MongoDB Database                      │
   └─────────────────────────────────────────┘
```

### Components

#### Frontend (React)
- **React 18** with Vite for fast development
- **Tailwind CSS** for responsive UI styling
- **Zustand** for lightweight state management
- **React Router** for navigation
- **Framer Motion** for animations
- **Recharts** for data visualization
- Real-time execution logs and memory insights

#### Backend (Python FastAPI)
- **FastAPI** - Modern, high-performance web framework
- **Motor** - Async MongoDB driver
- **Hugging Face Inference API** - LLM integration (LLaMA models)
- **MongoDB** - Memory and data persistence
- **n8n** - Workflow automation and execution

#### Agent Components

1. **Planner** (`agents/planner.py`)
   - Converts goals into structured sub-goals
   - Breaks sub-goals into executable tasks
   - Assigns priorities and dependencies
   - Estimates task duration

2. **Executor** (`agents/executor.py`)
   - Selects appropriate n8n workflows
   - Triggers task execution
   - Monitors execution status
   - Handles errors and retries

3. **Reflector** (`agents/reflector.py`)
   - Analyzes execution outcomes
   - Generates improvement insights
   - Identifies patterns and bottlenecks
   - Suggests plan adjustments

4. **Memory Manager** (`agents/memory.py`)
   - Manages short-term (working memory)
   - Stores long-term (historical data)
   - Indexes episodic (event-based) memories
   - Enables semantic search across memories

## Tech Stack

### Frontend
- **React 18.2.0** - UI framework
- **Vite 5.0.0** - Build tool and dev server
- **Tailwind CSS 3.3.6** - Utility-first CSS
- **React Router 6.20.0** - Client-side routing
- **Zustand 4.4.7** - State management
- **Axios 1.6.0** - HTTP client
- **Framer Motion 10.16.16** - Animation library
- **Lucide React 0.292.0** - Icon library
- **Recharts 2.10.3** - Chart library

### Backend
- **Python 3.10+** - Programming language
- **FastAPI 0.104.1** - Web framework
- **Uvicorn 0.24.0** - ASGI server
- **Motor 3.3.2** - Async MongoDB driver
- **PyMongo 4.6.0** - MongoDB driver
- **Pydantic 2.5.0** - Data validation
- **HTTPX 0.25.2** - Async HTTP client
- **PyJWT 2.8.1** - JWT authentication
- **Bcrypt 4.1.1** - Password hashing

### Infrastructure
- **MongoDB** - NoSQL database
- **n8n** - Workflow automation platform
- **Hugging Face** - LLM inference API

## 📋 Prerequisites

Before starting, ensure you have:

- **Node.js 18+** or **Node.js 20 LTS** (for frontend and npm)
- **Python 3.10+** (for backend)
- **MongoDB** - Either:
  - Local MongoDB instance (version 5.0+), or
  - MongoDB Atlas cloud account ([Create free account](https://www.mongodb.com/cloud/atlas))
- **n8n** - Either:
  - Self-hosted n8n instance, or
  - n8n Cloud account ([Sign up here](https://n8n.io))
- **Hugging Face API Key** - [Get one here](https://huggingface.co/settings/tokens)
  - Recommend using `meta-llama/Llama-2-7b-chat` or similar model
- **Git** - For version control

### Optional Tools
- **Docker & Docker Compose** - For containerized deployment
- **VS Code** - Recommended IDE

## Installation

### 1. Clone Repository

```bash
git clone <repository-url>
cd hackathon
```

### 2. Install Root Dependencies

```bash
npm install
```

### 3. Install All Dependencies

```bash
npm run install:all
```

This command:
- Installs root npm packages
- Installs backend dependencies: `cd backend && pip install -r requirements.txt`
- Installs frontend dependencies: `cd frontend && npm install`

### 4. Environment Configuration

#### Backend Environment Setup

```bash
cd backend
cp .env.example .env
```

Edit `backend/.env` with your credentials:

```env
# API Keys
HF_API_KEY=your_huggingface_api_key_here
HUGGINGFACE_API_KEY=your_huggingface_api_key_here  # Alternative

# Database
MONGODB_URI=mongodb://localhost:27017/agentic_ai
# Or for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/agentic_ai

# n8n Integration
N8N_WEBHOOK_URL=http://localhost:5678/webhook/agent
N8N_BASE_URL=http://localhost:5678

# Server Configuration
PORT=5000
HOST=0.0.0.0
DEBUG=false

# JWT Configuration
JWT_SECRET=your_secret_key_change_this_in_production
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24

# CORS Configuration
CORS_ORIGINS=http://localhost:3000,http://localhost:5173
```

#### Frontend Environment Setup (Optional)

```bash
cd frontend
cp .env.example .env.local
```

Edit `frontend/.env.local`:

```env
VITE_API_URL=http://localhost:5000
VITE_WS_URL=ws://localhost:5000
```

## Configuration

### MongoDB Setup

#### Option 1: Local MongoDB

```bash
# On Windows (with MongoDB installed)
mongod

# On macOS (with Homebrew)
brew services start mongodb-community

# On Linux
sudo systemctl start mongod
```

#### Option 2: MongoDB Atlas Cloud

1. Create account at https://www.mongodb.com/cloud/atlas
2. Create a free cluster
3. Get connection string (looks like: `mongodb+srv://user:pass@cluster.mongodb.net/dbname`)
4. Add to `backend/.env`:
   ```env
   MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/agentic_ai
   ```

### n8n Setup

#### Option 1: Local n8n

```bash
# Install globally
npm install -g n8n

# Start n8n
n8n start
# Runs on http://localhost:5678

# In n8n UI:
# 1. Create new workflow
# 2. Import from: n8n/workflows/agent-task-executor.json
# 3. Configure any required credentials
# 4. Activate the workflow
```

#### Option 2: n8n Cloud

1. Sign up at https://n8n.io
2. Create new workflow
3. Import the workflow from `n8n/workflows/agent-task-executor.json`
4. Update `N8N_BASE_URL` in `backend/.env`

### Hugging Face API Configuration

1. Get API key from https://huggingface.co/settings/tokens
2. Recommend using model: `meta-llama/Llama-2-7b-chat`
3. Add to `backend/.env`:
   ```env
   HF_API_KEY=hf_your_token_here
   ```

## Running the Application

### Quick Start (All Services)

```bash
# Terminal 1: MongoDB
mongod

# Terminal 2: n8n
n8n start

# Terminal 3: Backend (FastAPI)
cd backend
python -m venv venv
# On Windows: venv\Scripts\activate
# On macOS/Linux: source venv/bin/activate
uvicorn app.main:app --reload --port 5000

# Terminal 4: Frontend (React)
cd frontend
npm run dev
# Opens http://localhost:5173
```

### Using npm Scripts

```bash
# Start backend only
npm run dev:backend

# Start frontend only
npm run dev:frontend

# Start production builds
npm run start:backend
npm run start:frontend

# Verify setup
npm run verify
```

### Health Checks

Once running, verify everything is working:

```bash
# API Health
curl http://localhost:5000/api/health

# Frontend
Open http://localhost:5173 in browser
```

## Usage

### 1. User Authentication

#### Sign Up
```
POST /api/auth/signup
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword123"
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

Response includes JWT token for subsequent requests.

### 2. Create a Goal

Navigate to `/goal` in the frontend or use API:

```
POST /api/agent/goals
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "title": "Learn React Fundamentals",
  "description": "Master React basics including components, hooks, and state management",
  "targetDate": "2026-02-25",
  "priority": "high"
}
```

**Example Goals:**
- "Learn React by building a todo app"
- "Create a study plan for machine learning fundamentals"
- "Automate weekly progress reports from GitHub activity"
- "Set up reminders for daily workout routine"

### 3. View Planning

After creating a goal:
- Navigate to Dashboard to see generated task plan
- View planning preview with priorities and dependencies
- Estimated time and resource requirements

### 4. Monitor Execution

- **Task Manager**: View and manage individual tasks
- **Execution Logs**: Real-time task execution status
- **Progress Bar**: Visual representation of completion
- **Memory Insights**: View system's learning and memories

### 5. Review Results

- **Execution Summary**: Outcome of each task
- **Reflection Insights**: AI-generated improvements
- **Memory Storage**: Historical data for future learning

## Project Structure

```
hackathon/
├── README.md                          # This file
├── package.json                       # Root npm configuration
│
├── backend/                           # Python FastAPI backend
│   ├── app/
│   │   ├── main.py                   # FastAPI app entry point
│   │   ├── __init__.py
│   │   ├── api/                      # API routes
│   │   │   ├── agent.py              # Agent endpoints
│   │   │   ├── health.py             # Health checks
│   │   │   └── __init__.py
│   │   ├── agents/                   # Agent components
│   │   │   ├── planner.py            # Goal planner
│   │   │   ├── executor.py           # Task executor
│   │   │   ├── reflector.py          # Reflection engine
│   │   │   ├── memory.py             # Memory management
│   │   │   └── __init__.py
│   │   ├── auth/                     # Authentication
│   │   │   ├── controller.py         # Auth logic
│   │   │   ├── middleware.py         # Auth middleware
│   │   │   ├── routes.py             # Auth routes
│   │   │   ├── utils.py              # Helper functions
│   │   │   └── __init__.py
│   │   ├── core/                     # Core utilities
│   │   │   ├── config.py             # Configuration
│   │   │   ├── database.py           # DB connection
│   │   │   └── __init__.py
│   │   ├── models/                   # Data models
│   │   │   ├── schemas.py            # Pydantic schemas
│   │   │   ├── user.py               # User model
│   │   │   └── __init__.py
│   │   └── services/                 # External services
│   │       ├── huggingface.py        # HF LLM API
│   │       ├── n8n.py                # n8n integration
│   │       └── __init__.py
│   ├── requirements.txt               # Python dependencies
│   ├── .env.example                  # Example environment variables
│   └── README.md                     # Backend documentation
│
├── frontend/                          # React Vite frontend
│   ├── src/
│   │   ├── App.jsx                   # Root component
│   │   ├── main.jsx                  # Entry point
│   │   ├── index.css                 # Global styles
│   │   ├── components/               # Reusable components
│   │   │   ├── Layout.jsx            # App layout
│   │   │   ├── ProtectedRoute.jsx    # Auth wrapper
│   │   │   ├── GoalCard.jsx          # Goal display
│   │   │   ├── TaskCard.jsx          # Task display
│   │   │   ├── PlanningPreview.jsx   # Plan visualization
│   │   │   ├── ProgressBar.jsx       # Progress indicator
│   │   │   ├── ExecutionSummary.jsx  # Execution results
│   │   │   ├── ReflectionInsights.jsx # AI insights
│   │   │   ├── StatusBadge.jsx       # Status indicators
│   │   │   ├── DatePicker.jsx        # Date selection
│   │   │   ├── TaskExecutionEditor.jsx # Task editor
│   │   │   └── animations.jsx        # Animation utilities
│   │   ├── pages/                    # Page components
│   │   │   ├── Login.jsx             # Login page
│   │   │   ├── Signup.jsx            # Signup page
│   │   │   ├── Dashboard.jsx         # Main dashboard
│   │   │   ├── GoalInput.jsx         # Goal creation
│   │   │   ├── TaskManager.jsx       # Task management
│   │   │   ├── TaskExecutionStatus.jsx # Execution status
│   │   │   ├── ExecutionLogs.jsx     # Execution logs
│   │   │   └── MemoryInsights.jsx    # Memory viewer
│   │   ├── services/
│   │   │   ├── api.js                # API client
│   │   │   └── authService.js        # Auth utilities
│   │   └── store/
│   │       └── agentStore.js         # Zustand state store
│   ├── package.json                  # Frontend dependencies
│   ├── vite.config.js                # Vite configuration
│   ├── tailwind.config.js            # Tailwind CSS config
│   ├── postcss.config.js             # PostCSS config
│   ├── index.html                    # HTML entry point
│   └── README.md                     # Frontend documentation
│
├── n8n/                              # n8n workflows
│   └── workflows/
│       └── agent-task-executor.json  # Main workflow
│
├── scripts/                          # Utility scripts
│   └── verify-setup.js               # Setup verification
│
└── .gitignore                        # Git ignore rules
```

## API Documentation

### Authentication Endpoints

#### POST /api/auth/signup
Register a new user

**Request:**
```json
{
  "email": "user@example.com",
  "password": "password123",
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

#### POST /api/auth/login
Authenticate user

**Request:**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "token_type": "bearer"
}
```

### Agent Endpoints

#### POST /api/agent/goals
Create a new goal

**Headers:**
```
Authorization: Bearer <token>
```

**Request:**
```json
{
  "title": "Learn React Fundamentals",
  "description": "Master React basics",
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
  "description": "Master React basics",
  "status": "planning",
  "createdAt": "2026-01-25T10:30:00Z",
  "plan": {
    "subGoals": [...],
    "tasks": [...]
  }
}
```

#### GET /api/agent/goals/{goalId}
Retrieve goal details

**Response:** `200 OK`
```json
{
  "id": "507f1f77bcf86cd799439011",
  "title": "Learn React Fundamentals",
  "status": "in_progress",
  "progress": 35,
  "plan": {...},
  "executionHistory": [...]
}
```

#### GET /api/agent/memory
Retrieve agent memories

**Query Parameters:**
- `type`: "short_term" | "long_term" | "episodic"
- `limit`: Number of results (default: 10)

**Response:** `200 OK`
```json
{
  "memories": [
    {
      "id": "...",
      "type": "episodic",
      "content": "Successfully completed React tutorial",
      "timestamp": "2026-01-25T10:30:00Z",
      "metadata": {...}
    }
  ]
}
```

#### POST /api/agent/reflect
Trigger reflection on recent executions

**Response:** `200 OK`
```json
{
  "insights": [
    "Tasks estimated at 2 hours took 3 hours on average",
    "Sequential tasks can be parallelized for better efficiency"
  ],
  "suggestions": [...]
}
```

### Health Check

#### GET /api/health
System health status

**Response:** `200 OK`
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

## Development Guide

### Adding New Features

#### 1. Backend (Python FastAPI)

Create new agent capability:

```python
# backend/app/agents/new_agent.py
from fastapi import APIRouter
from app.core.config import settings

router = APIRouter()

@router.post("/endpoint")
async def new_capability(data: dict):
    # Implementation
    return {"result": "success"}
```

Register in `backend/app/main.py`:
```python
from app.agents import new_agent
app.include_router(new_agent.router, prefix="/api/agent")
```

#### 2. Frontend (React)

Create new component:

```jsx
// frontend/src/components/NewComponent.jsx
import React from 'react';

export function NewComponent() {
  return (
    <div className="component">
      {/* Your JSX */}
    </div>
  );
}
```

Use in pages:
```jsx
import { NewComponent } from '../components/NewComponent';

export function PageName() {
  return <NewComponent />;
}
```

### Running Tests

```bash
# Backend tests
cd backend
pytest

# Frontend tests
cd frontend
npm run test
```

### Debugging

#### Backend Debugging

```bash
# Using FastAPI's automatic documentation
# Open: http://localhost:5000/docs

# Check logs
# Terminal shows detailed request/response logs
```

#### Frontend Debugging

```bash
# Use React DevTools browser extension
# Source maps available in development mode
# Use browser console: F12
```

### Code Style

- **Backend**: Follow PEP 8 with `black` formatter
- **Frontend**: Use ESLint configuration included
- **Commits**: Follow conventional commit messages

## Troubleshooting

### Common Issues

#### 1. MongoDB Connection Error

```
Error: could not connect to server: connection() call failed
```

**Solution:**
- Ensure MongoDB is running: `mongod`
- Check `MONGODB_URI` in `.env`
- Verify MongoDB credentials for Atlas

#### 2. Hugging Face API Key Invalid

```
Error: Invalid API key or model not found
```

**Solution:**
- Verify API key at https://huggingface.co/settings/tokens
- Ensure model is accessible (public or you have access)
- Check quota hasn't been exceeded

#### 3. n8n Webhook Not Responding

```
Error: Failed to execute task via n8n
```

**Solution:**
- Verify n8n is running on configured port
- Check `N8N_WEBHOOK_URL` in `.env`
- Ensure workflow is activated in n8n UI

#### 4. CORS Errors in Frontend

```
Access to XMLHttpRequest... blocked by CORS policy
```

**Solution:**
- Update `CORS_ORIGINS` in `backend/.env`
- Include frontend URL (e.g., `http://localhost:5173`)

#### 5. Port Already in Use

```
Address already in use
```

**Solution:**
```bash
# Kill process using port
# Windows PowerShell
Get-Process -Id (Get-NetTCPConnection -LocalPort 5000).OwningProcess | Stop-Process -Force

# macOS/Linux
lsof -ti:5000 | xargs kill -9
```

### Logs and Debugging

#### View Backend Logs
```bash
cd backend
# Logs are printed to console with timestamps
# Enable debug mode: DEBUG=true in .env
```

#### View Frontend Logs
```bash
# Browser console (F12 or Cmd+Option+I on macOS)
# React DevTools for component inspection
```

#### Database Inspection
```bash
# Connect to MongoDB
mongosh  # or mongo for older versions

# Show databases
show dbs

# Use agentic_ai database
use agentic_ai

# View collections
show collections

# Query documents
db.users.find()
db.goals.find()
db.memories.find()
```

## Performance Optimization

### Frontend
- Code splitting with Vite lazy loading
- Image optimization and lazy loading
- Zustand for efficient state management
- Memoization for expensive computations

### Backend
- Async/await for non-blocking I/O
- Connection pooling for MongoDB
- Caching frequently accessed data
- Rate limiting on API endpoints

### Database
- Indexes on frequently queried fields
- Data pagination for large result sets
- Archive old memories periodically

## Security Considerations

- **JWT Tokens**: Short expiration times (24 hours default)
- **Password Hashing**: Bcrypt with salt rounds
- **CORS**: Restricted to frontend domain
- **API Rate Limiting**: Prevent abuse
- **Input Validation**: Pydantic schemas
- **Environment Variables**: Never commit `.env`

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request

### Guidelines

- Follow code style conventions
- Add tests for new features
- Update documentation
- Keep commits clean and descriptive

## Deployment

### Docker Deployment

```dockerfile
# Backend Dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0"]
```

```dockerfile
# Frontend Dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Environment Variables for Production

```env
# Backend Production
ENVIRONMENT=production
DEBUG=false
JWT_SECRET=<strong_random_secret>
MONGODB_URI=<production_mongodb_uri>
CORS_ORIGINS=<production_domain>
```

## Monitoring and Analytics

- Monitor API response times
- Track error rates and types
- Log successful goal completions
- Analyze memory usage patterns
- Track user engagement metrics

## Roadmap

- [ ] Web search integration
- [ ] Multi-language support
- [ ] Advanced analytics dashboard
- [ ] Mobile app (React Native)
- [ ] Team collaboration features
- [ ] Custom LLM fine-tuning
- [ ] Advanced memory pruning algorithms

## Support

For issues, questions, or suggestions:

1. Check [Troubleshooting](#troubleshooting) section
2. Search existing GitHub issues
3. Create new issue with detailed description
4. Contact development team

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built with [FastAPI](https://fastapi.tiangolo.com/)
- Frontend with [React](https://react.dev/) and [Vite](https://vitejs.dev/)
- LLM powered by [Hugging Face](https://huggingface.co/)
- Workflows via [n8n](https://n8n.io/)
- Database by [MongoDB](https://www.mongodb.com/)

---

**Last Updated:** January 2026
**Version:** 1.0.0

You can optionally specify:
- **Start Date**: When you want to begin this goal
- **End Date**: Target completion date (helps schedule tasks)

### 2. Dashboard - Monitor Goals

The enhanced dashboard now shows all your goals organized by status:

- **🔵 Active Goals**: Currently running goals with real-time progress
- **✅ Completed Goals**: Successfully finished goals with full history
- **⏳ Planned Goals**: Upcoming goals scheduled for the future

Each goal card displays:
- Goal title and status badge
- Progress bar (completed vs total tasks)
- Start and end dates
- Task completion summary

Click on any goal to view details, including planning overview, execution results, and reflection insights.

### 3. Interactive Task Management

Navigate to the **Tasks** section (`/tasks`) to manage and track individual tasks:

**Features:**
- **Interactive Checkboxes**: Mark tasks as complete or pending
  - ✅ Green checkmark = Executed/Completed
  - ❌ Red cross = Failed
  - ⏳ Grey = Pending
  
- **Date Scheduling**: Assign specific dates to tasks within the goal's date range
  - Click on a task to reveal the date picker
  - Schedule tasks across your goal timeline
  - View tasks organized by scheduled date

- **Calendar View**: See all tasks grouped by date
  - Switch between List and Calendar views
  - Unscheduled tasks are grouped separately
  - Visual timeline of your planned work

- **Filtering & Sorting**: Control task visibility
  - Filter by status: All, Pending, Executed, Failed
  - Sort by: Scheduled date, Status, Priority
  - Real-time task count summary

### 4. Smart Data Display (No Raw JSON)

All agent outputs are now beautifully formatted:

**Planning Output:**
- Sub-goals displayed as cards with priority indicators
- Success criteria as a formatted list
- Timeline and estimated duration
- Task list with tool and time estimates

**Execution Results:**
- Status cards showing success/failure for each task
- Error messages clearly highlighted
- Success rate visualization with progress indicators
- Task execution summary

**Reflection & Insights:**
- Key insights displayed as bullet points
- Failures analysis with categorization
- Improvement recommendations
- Success patterns identified

**Memory Insights:**
- Organized by memory type (Short-term, Long-term, Episodic)
- Color-coded badges for quick identification
- Expandable memory cards with full context
- Memory statistics and patterns

### 5. Agent Execution Loop

The agent automatically:
1. **Understands** your goal
2. **Decomposes** it into sub-goals and tasks
3. **Plans** execution with priorities and scheduling
4. **Selects** appropriate tools/workflows
5. **Executes** tasks via n8n
6. **Evaluates** results with detailed metrics
7. **Reflects** on outcomes and generates insights
8. **Re-plans** if needed based on failures

### 6. Monitor Execution

- **Dashboard**: View all your goals with progress tracking and statistics
- **Execution Logs**: Detailed timeline and log viewer with filtering
  - Step-by-step execution history
  - Task execution results with success/failure status
  - Visual timeline of the entire execution process
  - Expandable log entries for detailed information
  
- **Memory Insights**: Explore agent memories organized by type
  - Memory statistics and success metrics
  - Expandable memory cards with full details
  - Outcome tracking (success/failure analysis)
  - Contextual information and metadata

## ✨ UI/UX Improvements

### Visual Design
- **Color Coding**: Green (success), Red (failure), Blue (in progress), Yellow (pending)
- **Icons & Emojis**: Visual indicators for quick status recognition
- **Progress Bars**: Visual representation of goal and task completion
- **Cards & Badges**: Clean, modular component design
- **Responsive Layout**: Optimized for desktop, tablet, and mobile

### Components
All new reusable React components:
- `GoalCard`: Display goals with progress and metadata
- `TaskCard`: Interactive task display with checkboxes and scheduling
- `StatusBadge`: Status indicators (Active, Completed, Pending, etc.)
- `ProgressBar`: Visual progress tracking
- `DatePicker`: Date selection for scheduling
- `PlanningPreview`: Formatted display of goals and sub-goals
- `ExecutionSummary`: Results visualization with charts
- `ReflectionInsights`: Formatted reflection and recommendations

### Data Models Enhanced

**Goal Object:**
```javascript
{
  goalId: "unique-id",
  title: "Goal description",
  status: "active | completed | pending",
  startDate: "2024-01-01",
  endDate: "2024-02-01",
  progress: 75,
  tasks: [],
  plan: {},
  executionResults: [],
  summary: {}
}
```

**Task Object:**
```javascript
{
  id: "task-id",
  subGoalId: "sub-goal-id",
  title: "Task title",
  description: "Task description",
  status: "pending | executed | failed",
  scheduledDate: "2024-01-15",
  executedAt: "2024-01-15T10:30:00Z",
  checkboxState: true,
  tool: "tool-name",
  estimatedTime: "2 hours"
}
```

## 🚀 Quick Start

For detailed setup instructions, see [docs/SETUP_GUIDE.md](docs/SETUP_GUIDE.md).

### 1. Install Dependencies

```bash
npm run install:all
```

### 2. Environment Setup

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

Edit `.env` with your API keys and configuration.

### 3. Start Services

**MongoDB:**
```bash
mongod  # or use MongoDB Atlas
```

**n8n:**
```bash
npm install -g n8n
n8n start
# Import workflow from n8n/workflows/agent-task-executor.json
```

**Backend:**
```bash
npm run dev:backend
# Runs on http://localhost:5000
```

**Frontend:**
```bash
npm run dev:frontend
# Runs on http://localhost:3000
```

## 🔧 API Endpoints

See [docs/API_EXAMPLES.md](docs/API_EXAMPLES.md) for detailed request/response examples.

### Execute Full Agent Loop
```bash
POST /api/agent/execute
Body: { "goal": "Your goal", "userId": "optional-user-id" }
```

### Get Plan Only
```bash
POST /api/agent/plan
Body: { "goal": "Your goal", "userId": "optional-user-id" }
```

### Execute Specific Task
```bash
POST /api/agent/task/execute
Body: { "task": {...}, "userId": "...", "goalId": "..." }
```

### Get Memories
```bash
GET /api/agent/memories/:goalId?type=episodic&limit=50
```

### Get Reflection
```bash
POST /api/agent/reflect/:goalId
Body: { "executionResults": [...] }
```

### Get User Patterns
```bash
GET /api/agent/patterns/:userId
```

### Health Check
```bash
GET /health
```

## 🔗 n8n Integration

The platform triggers n8n workflows via webhooks. Example workflows:

- **task-executor**: Generic task execution
- **study-plan**: Create structured learning plans
- **notification**: Send reminders/notifications
- **scheduler**: Schedule future tasks

### Setting up n8n Workflow

1. Import `n8n/workflows/agent-task-executor.json` into n8n
2. Activate the workflow
3. Ensure webhook URL matches `N8N_WEBHOOK_URL` in `.env`

## 🧠 Memory System

### Memory Types

- **Short-term**: Current task context
- **Long-term**: User goals, preferences, past outcomes
- **Episodic**: Task → action → result sequences

### Memory Storage

Memories are stored in MongoDB with:
- User ID
- Goal ID
- Memory type
- Content (flexible JSON)
- Context and outcomes
- Timestamps

## 🛡️ Ethical Safeguards

- No irreversible actions without user approval
- No financial or system-level actions
- Full transparency of agent decisions
- Human-in-the-loop override capability
- Rate limits and execution boundaries

## 📁 Project Structure

```
agentic-ai-platform/
├── frontend/
│   ├── src/
│   │   ├── components/     # React components
│   │   ├── pages/          # Page components
│   │   ├── store/          # Zustand state
│   │   ├── services/       # API services
│   │   └── App.jsx
│   └── package.json
├── backend/
│   ├── agents/             # Agent modules
│   │   ├── planner.js
│   │   ├── executor.js
│   │   ├── reflector.js
│   │   └── memory.js
│   ├── services/           # External services
│   │   ├── llama.service.js
│   │   └── n8n.service.js
│   ├── routes/             # API routes
│   │   └── agent.routes.js
│   ├── server.js
│   └── package.json
├── n8n/
│   └── workflows/          # n8n workflow JSON
└── README.md
```

## 🔍 Example Scenarios

See [docs/EXAMPLE_SCENARIOS.md](docs/EXAMPLE_SCENARIOS.md) for detailed scenarios.

### Quick Examples

**Learning Goal:**
```
Goal: "Learn Python by completing 5 coding challenges"
→ Creates study plan, schedules practice, sets reminders, tracks progress
```

**Workflow Automation:**
```
Goal: "Automate weekly GitHub activity reports"
→ Connects to GitHub API, fetches activity, generates report, emails weekly
```

**Study Planning:**
```
Goal: "Create a study plan for machine learning fundamentals"
→ 3-month structured plan with modules, resources, and milestones
```

For detailed API request examples, see [docs/API_EXAMPLES.md](docs/API_EXAMPLES.md).

## 🐛 Troubleshooting

### Hugging Face API Issues
- Ensure `HF_API_KEY` is set correctly
- Check model availability (some models may require access request)
- Model loading may take time on first request (503 error is normal)

### MongoDB Connection
- Verify MongoDB is running
- Check `MONGODB_URI` format
- Ensure network access if using remote MongoDB

### n8n Integration
- Verify n8n is running on port 5678
- Check webhook URL matches workflow
- Ensure workflow is activated

## 🚧 Development

### Backend Development
```bash
cd backend
npm install
npm run dev  # Uses nodemon for auto-reload
```

### Frontend Development
```bash
cd frontend
npm install
npm run dev  # Vite dev server with HMR
```

### Verify Setup
```bash
# Run verification script to check all services
npm run verify
```

## 📝 Notes

- The platform uses Hugging Face Inference API (not OpenAI)
- LLaMA models may require access approval
- n8n workflows can be customized for specific use cases
- Memory system supports flexible JSON storage
- All agent decisions are logged for transparency

## 📄 License

MIT

## 🤝 Contributing

Contributions welcome! Please ensure:
- Code follows existing patterns
- All modules are documented
- Ethical safeguards are maintained
- Tests are added for new features

---

**Built with ❤️ for autonomous AI agents**
