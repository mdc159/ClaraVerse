# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Core Development
- `npm run dev` - Start Vite development server (web version)
- `npm run build` - Build for production
- `npm run lint` - Run ESLint
- `npm run preview` - Preview production build

### Testing
- `npm test` - Run all tests once
- `npm run test:watch` - Run tests in watch mode
- `npm run test:ui` - Run tests with UI
- `npm run test:coverage` - Run tests with coverage report

### Electron Desktop App
- `npm run electron:dev` - Start Electron in development mode
- `npm run electron:dev:hot` - Start with hot reload (runs dev server + electron)
- `npm run electron:build` - Build desktop app for current platform
- `npm run electron:build-all` - Build for Mac, Windows, and Linux

### Docker
- `npm run docker:build` - Build Docker image
- `npm run docker:run` - Run Docker container on port 8069
- `npm run docker:publish` - Build and publish to registry

## Architecture Overview

### Multi-Platform AI Application
Clara is a comprehensive AI platform built as both a web application and Electron desktop app. It combines multiple AI capabilities into a unified interface:

### Core Technologies
- **Frontend**: React 18 + TypeScript + Vite
- **Desktop**: Electron with Node.js backend integration
- **Styling**: Tailwind CSS with custom themes
- **State Management**: React Context + localStorage
- **Flow Builder**: ReactFlow for visual app creation
- **Backend**: Python FastAPI for AI services

### Application Structure

**Main Entry Points**:
- `src/App.tsx` - Main React application with page routing
- `electron/main.ts` - Electron main process with Docker integration
- `py_backend/main.py` - Python FastAPI backend for AI services

**Page Architecture**:
The app uses a single-page architecture with conditional rendering based on `activePage` state:
- Dashboard (widgets + quick access)
- Assistant (LLM chat interface)
- App Creator (visual flow builder)
- Image Generation (Stable Diffusion via ComfyUI)
- Gallery (image management)
- N8N (workflow automation)
- UI Builder (code generation)

### Visual App Creator System

**Node-Based Architecture**:
- **Nodes**: Individual processing units (`src/components/appcreator_components/nodes/`)
- **Executors**: Runtime execution logic (`src/nodeExecutors/`)
- **Registry**: Dynamic node registration (`NodeRegistry.tsx`)
- **Engine**: Flow execution coordinator (`ExecutionEngine.tsx`)

**Node Types**:
- Input nodes (text, image, webcam)
- Processing nodes (LLM, image manipulation, conditional logic)
- Output nodes (text, image, file export)

**Execution Flow**:
1. User creates visual flow with ReactFlow
2. `ExecutionEngine.generateExecutionPlan()` creates dependency graph
3. `ExecutionEngine.executeFlow()` runs nodes in topological order
4. Each node uses corresponding executor from `nodeExecutors/`

### AI Integration Points

**LLM Integration**:
- Ollama client (`src/utils/OllamaClient.ts`)
- OpenAI API support
- Model selection and management

**Image Generation**:
- ComfyUI integration (`src/utils/ComfyuiClient.ts`)
- Stable Diffusion workflows
- Gallery management system

**Document AI**:
- RAG (Retrieval Augmented Generation) via Python backend
- PDF/CSV/text document processing
- Vector database storage

**Speech Processing**:
- Speech-to-text via Whisper (Python backend)
- Audio file upload and transcription

### Backend Services Architecture

**Python FastAPI Backend** (`py_backend/`):
- Document processing and RAG
- Audio transcription
- Image generation APIs
- SQLite database for metadata

**Electron Backend Integration**:
- Docker container management
- Service orchestration
- IPC communication between renderer and main process

### Database Systems

**Client-side Storage**:
- IndexedDB via `src/db/index.ts`
- localStorage for user preferences
- Electron-store for desktop settings

**Server-side Storage**:
- SQLite for document metadata
- ChromaDB for vector embeddings
- File system for generated content

### Key Development Patterns

**Component Organization**:
- Feature-based folder structure (`components/[feature]_components/`)
- Shared components in `components/common/`
- UI library components in `uibuilder_components/ui_builder_libraries/`

**State Management**:
- Context providers for complex state (InterpreterContext, OllamaContext)
- localStorage for persistence
- Component-level state for UI interactions

**Node System Development**:
When adding new nodes:
1. Create node component in `appcreator_components/nodes/`
2. Create executor in `nodeExecutors/`
3. Export metadata for toolbox registration
4. Both files must use matching naming conventions

**Error Handling**:
- Try-catch blocks with user-friendly error messages
- Fallback mechanisms for AI service failures
- Toast notifications for user feedback

### Docker Integration

The Electron app automatically manages Docker containers:
- N8N workflow automation
- Python backend services
- ComfyUI image generation
- Container health monitoring via `electron/dockerSetup.cjs`

### Development Notes

**Hot Reload**: Use `npm run electron:dev:hot` for fastest development cycle
**Testing**: Vitest with React Testing Library setup
**Type Safety**: Strict TypeScript configuration
**Build Process**: Vite handles bundling with code splitting for vendor libraries