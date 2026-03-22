# 🧠 Online IDE (VS Code-like) — System Design

## 📌 Overview

This project is a browser-based IDE inspired by VS Code, supporting:

* File editing
* Workspaces
* Code execution
* Git integration
* Live terminal

The system is designed to be:

* Scalable
* Secure
* Real-time
* Modular

---

## 🏗️ High-Level Architecture

```
Browser (React + Monaco)
        ↓
API Gateway (Spring Boot)
        ↓
-------------------------------------------------
| Auth Service                                  |
| Workspace Service                             |
| Execution Service                             |
| Git Service                                   |
| Terminal Service (WebSocket)                  |
-------------------------------------------------
        ↓
Infrastructure Layer
(Docker + Redis + PostgreSQL + S3)
```

---

## 🔑 Core Concepts

### 1. Workspace

A workspace represents a developer environment:

* File system
* Git repository
* Runtime environment

Each workspace runs inside a **Docker container**.

---

### 2. Separation of Concerns

| Layer    | Responsibility              |
| -------- | --------------------------- |
| Frontend | UI, editor state            |
| Backend  | API, execution, persistence |
| Infra    | Containers, storage         |

---

### 3. Stateless Backend

* Authentication via JWT
* Optional session caching in Redis
* Enables horizontal scaling

---

## 🧩 Services Breakdown

### 🔐 Auth Service

* Login / Signup
* JWT + Refresh tokens
* Redis session store (optional)

---

### 📁 Workspace Service (Core)

* Create/Delete workspace
* Attach Docker container
* File operations
* Snapshot & restore

---

### ⚙️ Execution Service

* Run user code inside container
* Enforce:

    * CPU limits
    * Memory limits
    * Timeouts
* Return output

---

### 🐙 Git Service

* Initialize repo per workspace
* Branch management
* Commit & diff

---

### 🖥️ Terminal Service

* WebSocket-based communication
* PTY inside container
* Real-time shell interaction

---

## 🗄️ Database Design

### Users Table

```
id
email
password_hash
created_at
```

### Workspaces Table

```
id
user_id
container_id
status
created_at
```

### Files Metadata (Optional)

```
workspace_id
file_path
last_modified
```

---

## 🔄 Data Flow

### Code Execution Flow

```
User clicks Run
        ↓
Frontend sends request
        ↓
Execution Service
        ↓
Docker Container
        ↓
Output captured
        ↓
Sent back via API/WebSocket
        ↓
Displayed in UI
```

---

## ⚡ Frontend Design

### Tech Stack

* React + TypeScript
* Monaco Editor
* Zustand (state management)
* Tailwind CSS

---

### Layout

```
-------------------------------------------------
| Top Bar (Run, Project, User)                  |
-------------------------------------------------
| Sidebar | Editor Tabs                         |
|         |-------------------------------------|
| Files   | Monaco Editor                       |
| Git     |                                     |
| Ext     |-------------------------------------|
|         | Terminal / Output Panel             |
-------------------------------------------------
```

---

### State Management (Zustand)

Global State:

```
files
activeFile
tabs
editorContent
isSaving
terminalOutput
```

---

## 🚀 Performance Considerations

* Debounced auto-save (500–1000ms)
* Lazy loading Monaco Editor
* Optimistic UI updates
* WebSocket for real-time features

---

## 🔐 Security Considerations

* All code execution inside Docker
* Read-only root filesystem
* Resource limits (CPU, RAM)
* No direct host access
* Input sanitization

---

## 📈 Scalability Plan

* Stateless services → horizontal scaling
* Redis for:

    * Caching
    * Job queues
* Load balancer (NGINX / AWS ALB)
* Container orchestration (future: Kubernetes)

---

## 🧪 Future Enhancements

* GitHub integration (OAuth + sync)
* Extension system
* Multi-user collaboration
* Real-time editing (CRDT / OT)
* AI code assistant

---

## 🎯 Key Highlights (Resume Worthy)

* Browser-based IDE with containerized execution
* Persistent workspaces with snapshot recovery
* Git-compatible branching system
* Secure sandboxed execution environment
* Real-time terminal via WebSockets

---
