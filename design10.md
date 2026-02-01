# LearnX - Technical Design Document

## System Overview

AI Study Buddy is an intelligent learning companion that provides personalized study planning, visual concept mapping, adaptive quiz generation, and curated resource recommendations. The system leverages a modern AI-first architecture built on AWS services with a focus on scalability, performance, and user experience.

### Core Design Principles
- **AI-First Architecture**: Every feature is enhanced by intelligent automation
- **Conversational Interface**: Natural language interaction as the primary UX paradigm
- **Personalization**: Adaptive learning that evolves with user behavior
- **Visual Learning**: Emphasis on concept visualization and relationship mapping
- **Modular Design**: Loosely coupled components for maintainability and scalability

## High-Level Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Frontend      │    │    Backend       │    │   AI Services   │
│                 │    │                  │    │                 │
│ React.js/       │◄──►│   FastAPI        │◄──►│ Amazon Bedrock  │
│ Streamlit       │    │                  │    │ (Claude/Titan)  │
│                 │    │ ┌──────────────┐ │    │                 │
│ - Chat UI       │    │ │ LangChain    │ │    │ Titan           │
│ - Mind Maps     │    │ │ Orchestrator │ │    │ Embeddings      │
│ - Study Plans   │    │ └──────────────┘ │    │                 │
│ - Quizzes       │    │                  │    └─────────────────┘
└─────────────────┘    └──────────────────┘              │
                                │                        │
                                │                        │
                       ┌──────────────────┐    ┌─────────────────┐
                       │   Data Layer     │    │ Vector Database │
                       │                  │    │                 │
                       │ PostgreSQL       │    │ ChromaDB /      │
                       │ - User Data      │    │ OpenSearch      │
                       │ - Study Plans    │    │                 │
                       │ - Progress       │    │ - Embeddings    │
                       │ - Resources      │    │ - Semantic      │
                       └──────────────────┘    │   Search        │
                                               └─────────────────┘

```

## Component Architecture

### Frontend Layer

**Technology Choice**: React.js (Primary) with Streamlit (Prototyping)
- **Rationale**: React provides better user experience for interactive features like mind maps and real-time chat, while Streamlit enables rapid prototyping

**Key Components**:
- **Chat Interface**: Real-time conversation with the AI assistant
- **Mind Map Renderer**: Interactive visualization using D3.js or React Flow
- **Study Plan Dashboard**: Calendar and task management interface
- **Quiz Interface**: Dynamic question presentation and feedback
- **Resource Browser**: Curated content discovery and organization

### Backend Layer

**Technology Choice**: FastAPI (Python)
- **Rationale**: Excellent async support, automatic API documentation, and seamless integration with Python AI libraries


### AI Orchestration Layer

**Technology Choice**: LangChain
- **Rationale**: Provides robust framework for chaining AI operations, memory management, and tool integration

**LangChain Components**:
- **Conversation Memory**: Maintains context across chat sessions
- **Tool Integration**: Connects LLM to external functions (calendar, quiz generation)
- **Prompt Templates**: Standardized prompts for consistent AI behavior
- **Chain Orchestration**: Complex workflows combining multiple AI operations

### Workflow Automation Layer (n8n)

- n8n is used to automate learning workflows triggered by AI decisions
- Sends study reminders, revision alerts, and quiz follow-ups automatically
- Triggers re-learning or revision when quiz performance drops
- Ensures consistent study habits with minimal manual effort
- n8n workflows are triggered by insights generated through LangChain-based AI reasoning

### LLM Services

**Technology Choice**: Amazon Bedrock (Claude 3 Sonnet/Haiku + Titan)
- **Rationale**: Enterprise-grade security, cost optimization, and model variety

**Model Selection Strategy**:
- **Claude 3 Sonnet**: Complex reasoning tasks (study planning, concept explanation)
- **Claude 3 Haiku**: Quick responses (chat, simple questions)
- **Titan Text**: Content generation and summarization
- **Titan Embeddings**: Semantic search and similarity matching

### Vector Database

**Technology Choice**: ChromaDB (Development) → Amazon OpenSearch (Production)
- **Rationale**: ChromaDB for rapid development, OpenSearch for production scalability

**Data Organization**:
```
The vector database stores embeddings of study materials, concepts, questions, and curated learning resources.       
```

## Feature Implementation Design

### 1. AI Daily Study Planner

**Architecture Pattern**: Event-driven scheduling with ML optimization

**Components**:
- **Schedule Optimizer**: Uses constraint satisfaction algorithms
- **Habit Tracker**: Monitors completion patterns
- **Adaptive Engine**: Adjusts plans based on performance data

**Process Flow**:
1. User inputs goals and constraints via chat
2. LangChain extracts structured data from natural language
3. Optimization engine creates initial schedule
4. Progress tracking provides feedback for continuous improvement



### 2. AI Mind Maps & Concept Graphs

**AI-generated Infographics & Visual Summaries**

- Automatically converts text-heavy content into visual infographics
- Generates flowcharts, comparison tables, timelines, and summary visuals
- Helps students understand complex topics faster through visual learning
- Improves memory retention and reduces cognitive overload


**Architecture Pattern**: Knowledge graph generation with visual rendering

**Components**:
- **Concept Extractor**: NLP pipeline for identifying key concepts
- **Relationship Mapper**: Graph algorithms for connection discovery
- **Visual Renderer**: Frontend component for interactive display

**Process Flow**:
1. User provides text or topic via chat
2. Claude analyzes content and extracts concepts
3. Embedding similarity identifies relationships
4. Graph structure sent to frontend for visualization




### 3. AI Self-Test Quiz Generator

**Architecture Pattern**: Adaptive question generation with difficulty scaling

**Components**:
- **Question Generator**: Template-based question creation
- **Difficulty Estimator**: ML model for question complexity
- **Performance Analyzer**: Tracks user strengths/weaknesses

**Process Flow**:
1. User requests quiz on specific topic
2. System retrieves relevant content from vector database
3. Claude generates questions with varying difficulty
4. User responses analyzed for knowledge gap identification

**Question Types**:
- Multiple choice with distractor generation
- Short answer with semantic similarity scoring
- Essay questions with rubric-based evaluation

### 4. RAG-based Resource Recommendation

**Architecture Pattern**: Retrieval-Augmented Generation with personalization

**Components**:
- **Content Indexer**: Processes and embeds educational resources
- **Retrieval Engine**: Semantic search across resource database
- **Recommendation Ranker**: Personalizes results based on user profile

**RAG Pipeline**:
1. User query embedded using Titan Embeddings
2. Vector similarity search retrieves relevant resources
3. Claude synthesizes recommendations with explanations
4. Results ranked by relevance and user preferences

## Data Flow Architecture

### Primary Data Flows

**1. Conversational Interaction**:
```
User Input → FastAPI → LangChain → Bedrock → Response Processing → Frontend
```

**2. Content Processing**:
```
Raw Content → Embedding Generation → Vector Storage → Semantic Search → Retrieval
```

**3. Personalization Loop**:
```
User Behavior → Analytics → Profile Update → Recommendation Adjustment → Improved Experience
```

**4. Automated Learning Workflow**:
```
AI Insight → n8n Workflow Trigger → Reminder / Revision / Notification → User
```
### Data Storage Strategy

User profiles and study plans are stored in a structured database, while uploaded
study materials and generated embeddings are stored in a vector database to enable
semantic search and Retrieval Augmented Generation (RAG).

## Scalability Considerations



The solution is designed with a modular and cloud-ready architecture, allowing
individual components to scale independently as user demand increases.



