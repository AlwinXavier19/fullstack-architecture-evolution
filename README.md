# 🏗️ Full-Stack Architecture & DevOps Evolution

> A complete engineering journey from a Django monolithic application to a modern, separated, containerized, Kubernetes-deployed platform with automated CI/CD and JWT authentication.

---

## 🚀 Overview

This project documents the complete modernization of a full-stack web application.

The application evolved from a traditional Django server-rendered architecture into a production-ready platform with clear separation between the frontend, backend, infrastructure, deployment, and authentication layers.

### Final Technology Stack

- **Frontend:** Next.js, TypeScript, Tailwind CSS
- **Backend:** Python, Django, Django REST Framework
- **API:** REST / JSON
- **Authentication:** JWT with Access Token + Refresh Token
- **Containerization:** Docker
- **Container Registry:** Azure Container Registry (ACR)
- **Cloud:** Microsoft Azure
- **Orchestration:** Kubernetes / Azure Kubernetes Service (AKS)
- **Networking:** Kubernetes Services + Ingress
- **CI/CD:** Azure DevOps
- **Configuration:** Environment variables + Kubernetes configuration

---

# 🛤️ How I Actually Got Here

The architecture was not changed all at once. I worked through the system step by step, solving the limitations of the previous architecture and improving the application as the requirements increased.

```text
Django Monolith
      ↓
Frontend / Backend Separation
      ↓
REST API Architecture
      ↓
Docker Containerization
      ↓
Azure Container Registry
      ↓
Kubernetes / AKS
      ↓
Azure DevOps CI/CD
      ↓
Independent Frontend & Backend Pipelines
      ↓
Session Authentication → JWT Authentication
```

---

# 1️⃣ Django Monolithic Application

The application initially used Django for both the frontend and backend.

```text
Browser
   │
   ▼
Django Application
   ├── Templates
   ├── Views
   ├── Business Logic
   ├── Authentication
   └── Database
```

Django handled server-side HTML rendering, backend processing, authentication, and database operations inside the same application.

### What I worked with

- Python
- Django
- Django Templates
- Models
- Views
- Business Logic
- Database Operations
- Django Authentication

This architecture provided a simple development and deployment model while the application was smaller.

### 🌐 Initial Single-Domain Setup

Initially, the application was accessed through a **single domain**, with the Django application handling the web traffic.

```text
User
 │
 ▼
Single Domain
 │
 ▼
Django Application
 │
 ▼
Database
```

---

# 2️⃣ Frontend / Backend Separation

As the frontend requirements became more advanced, I separated the frontend from the Django application and built a dedicated **Next.js frontend**.

```text
                  Browser
                     │
                     ▼
          ┌────────────────────┐
          │   Next.js Frontend │
          │                    │
          │ TypeScript         │
          │ Tailwind CSS       │
          │ Components         │
          │ Pages              │
          └─────────┬──────────┘
                    │
               REST / JSON
                    │
                    ▼
          ┌────────────────────┐
          │ Django REST API    │
          │                    │
          │ Authentication     │
          │ Validation         │
          │ Business Logic     │
          │ Serializers        │
          └─────────┬──────────┘
                    │
                    ▼
                 Database
```

### What I implemented

- Created the Next.js frontend architecture
- Worked with TypeScript
- Built reusable frontend components
- Used Tailwind CSS for UI development
- Connected frontend pages to Django APIs
- Created reusable API request functions
- Handled API responses and errors
- Configured frontend environment variables
- Configured CORS
- Separated UI responsibilities from backend business logic

The result was a clean separation:

```text
Frontend
  ↓
User Interface + Client Logic
  ↓
REST API
  ↓
Backend
  ↓
Business Logic + Data
```

---

# 3️⃣ REST API Architecture

After separating the frontend and backend, communication was established through REST APIs.

```text
Next.js
   │
   │ HTTP Request
   ▼
Django REST Framework
   │
   ├── Authentication
   ├── Permissions
   ├── Validation
   ├── Business Logic
   └── Database
   │
   ▼
JSON Response
   │
   ▼
Next.js
```

### API responsibilities

The backend became responsible for:

- Authentication
- Authorization
- Request validation
- Business logic
- Database operations
- Serialization
- JSON responses
- API error handling

The frontend became responsible for:

- UI
- Forms
- Components
- Client-side state
- User interaction
- API integration
- Displaying API responses

This created a clear contract between frontend and backend.

---

# 4️⃣ Session Authentication → JWT Authentication

Authentication was also modernized as part of the architectural separation.

### Earlier Session-Based Flow

```text
User
  │
  ▼
Next.js Login
  │
  ▼
Django
  │
  ▼
Session Created
  │
  ▼
Session Cookie
  │
  ▼
Browser
  │
  ▼
Authenticated Requests
```

The application initially relied on Django session-based authentication.

I then converted the authentication architecture to **JWT-based authentication**.

---

## 🔐 Final JWT Authentication Flow

```text
                    User
                     │
                     ▼
              Next.js Login
                     │
                     ▼
             Django REST API
                     │
                     ▼
           Verify Credentials
                     │
                     ▼
        ┌────────────────────────┐
        │ Access Token            │
        │ Short-lived             │
        ├────────────────────────┤
        │ Refresh Token           │
        │ Long-lived              │
        └───────────┬────────────┘
                    │
                    ▼
               Next.js
                    │
                    │ Authorization:
                    │ Bearer <access-token>
                    ▼
             Django REST API
                    │
                    ▼
            Verify JWT Token
                    │
                    ▼
           Protected Resources
```

### JWT implementation work

I worked on:

- Login authentication
- Access token generation
- Refresh token handling
- Authorization headers
- Protected API requests
- Token validation
- Access-token expiry handling
- Refresh-token flow
- Logout/authentication state handling
- Frontend authentication integration
- Backend API authentication

This removed the frontend's dependency on Django session cookies and provided a token-based authentication model suitable for independently deployed clients.

---

# 5️⃣ Docker Containerization

After frontend/backend separation, I containerized the applications independently using Docker.

```text
Frontend Source
      │
      ▼
Docker Build
      │
      ▼
Frontend Image
      │
      ▼
Azure Container Registry


Backend Source
      │
      ▼
Docker Build
      │
      ▼
Backend Image
      │
      ▼
Azure Container Registry
```

Each application has its own runtime environment and Docker image.

### Benefits

- Reproducible environments
- Consistent dependencies
- Easier deployment
- Isolated application runtimes
- Same container artifact across environments

---

# 6️⃣ Azure Container Registry

Docker images are stored in **Azure Container Registry** before being deployed to Kubernetes.

```text
Source Code
     │
     ▼
Azure DevOps
     │
     ▼
Docker Build
     │
     ├───────────────┐
     ▼               ▼
Frontend Image   Backend Image
     │               │
     └───────┬───────┘
             ▼
           ACR
             │
             ▼
            AKS
```

ACR provides a centralized private registry for the container images used by the Kubernetes environment.

---

# 7️⃣ Kubernetes / Azure Kubernetes Service

The containerized applications are deployed and managed using **Azure Kubernetes Service (AKS)**.

```text
                     AKS Cluster
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
     Frontend Deployment      Backend Deployment
             │                       │
             ▼                       ▼
       Frontend Pods             Backend Pods
             │                       │
             ▼                       ▼
     Frontend Service          Backend Service
             │                       │
             └───────────┬───────────┘
                         ▼
                      Ingress
                         │
                         ▼
                       Users
```

### Kubernetes responsibilities

I worked with Kubernetes concepts including:

- Deployments
- Pods
- Services
- Ingress
- Namespaces
- Configuration
- Secrets
- Container images
- Replica management
- Application restarts
- Rolling deployments
- Resource configuration

The application could therefore be managed as independent containerized workloads instead of one manually operated server process.

---

# 8️⃣ Kubernetes Ingress

Ingress provides the routing layer between external traffic and Kubernetes services.

```text
Internet
   │
   ▼
Ingress
   │
   ├───────────────► Frontend Service
   │                       │
   │                       ▼
   │                 Frontend Pods
   │
   └───────────────► Backend Service
                           │
                           ▼
                     Backend Pods
```

This provides controlled routing into the Kubernetes cluster and keeps service-level networking separate from the public entry point.

---

# 9️⃣ Azure DevOps CI/CD

I automated the build and deployment process using Azure DevOps.

```text
Developer
    │
    │ git push
    ▼
Git Repository
    │
    ▼
Azure DevOps Pipeline
    │
    ├── Build
    ├── Test
    ├── Docker Build
    ├── Push Image
    └── Deploy
          │
          ▼
        AKS
```

### Deployment flow

```text
Code
 ↓
Build
 ↓
Test
 ↓
Docker Image
 ↓
Push to ACR
 ↓
Deploy to AKS
 ↓
Application Running
```

This replaced repetitive manual deployment work with a repeatable CI/CD process.

---

# 🔟 Separate Frontend & Backend Pipelines

Initially, frontend and backend deployments were handled together.

As the applications started changing independently, I separated the CI/CD pipelines.

```text
                 Git Repository
                       │
          ┌────────────┴────────────┐
          │                         │
     frontend/                  backend/
          │                         │
          ▼                         ▼
Frontend Pipeline            Backend Pipeline
          │                         │
          ▼                         ▼
Frontend Docker Image        Backend Docker Image
          │                         │
          ▼                         ▼
          ACR                      ACR
          │                         │
          ▼                         ▼
      Frontend AKS             Backend AKS
```

### Result

- Frontend can deploy independently
- Backend can deploy independently
- Smaller deployment scope
- Faster CI/CD execution
- Easier troubleshooting
- Independent release cycles
- Reduced unnecessary builds

---

# 🏗️ Complete Production Architecture

```text
                              USERS
                                │
                                ▼
                         ┌─────────────┐
                         │   Browser   │
                         └──────┬──────┘
                                │
                                ▼
                     ┌────────────────────┐
                     │ Next.js Frontend   │
                     │ TypeScript         │
                     │ Tailwind CSS       │
                     └─────────┬──────────┘
                               │
                          REST / JSON
                               │
                    Authorization: Bearer
                               │
                               ▼
                     ┌────────────────────┐
                     │ Django REST API    │
                     │                    │
                     │ JWT Authentication │
                     │ Permissions        │
                     │ Validation         │
                     │ Business Logic     │
                     │ Serializers        │
                     └─────────┬──────────┘
                               │
                               ▼
                         ┌───────────┐
                         │ Database  │
                         └───────────┘


                    DEPLOYMENT PLATFORM

                         Git Push
                            │
                            ▼
                    ┌───────────────┐
                    │ Azure DevOps  │
                    │ CI/CD         │
                    └───────┬───────┘
                            │
                            ▼
                       Docker Build
                            │
                            ▼
                    ┌───────────────┐
                    │      ACR      │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   AKS Cluster │
                    │               │
                    │ ┌───────────┐ │
                    │ │ Frontend  │ │
                    │ │ Pods      │ │
                    │ └───────────┘ │
                    │               │
                    │ ┌───────────┐ │
                    │ │ Backend   │ │
                    │ │ Pods      │ │
                    │ └───────────┘ │
                    │               │
                    │   Ingress     │
                    └───────────────┘
```

---

# 🛠️ Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js |
| Language | TypeScript |
| Styling | Tailwind CSS |
| Backend | Python + Django |
| API | Django REST Framework |
| Authentication | JWT Access + Refresh Tokens |
| Database | Relational Database |
| Containerization | Docker |
| Container Registry | Azure Container Registry |
| Cloud | Microsoft Azure |
| Orchestration | Kubernetes / AKS |
| Networking | Kubernetes Services + Ingress |
| CI/CD | Azure DevOps |
| Source Control | Git |

---

# 💪 Technical Work Covered

This work involved the complete application lifecycle:

```text
Frontend Development
        ↓
API Development
        ↓
Authentication
        ↓
Database Integration
        ↓
Docker
        ↓
Container Registry
        ↓
Kubernetes
        ↓
Ingress
        ↓
CI/CD
        ↓
Production Deployment
```

### Core engineering areas

- Full-stack application architecture
- Next.js development
- TypeScript
- Tailwind CSS
- Django
- Django REST Framework
- REST API design
- API integration
- JWT authentication
- Access/refresh token handling
- CORS configuration
- Docker
- Azure Container Registry
- Kubernetes
- AKS
- Kubernetes Services
- Ingress
- Azure DevOps
- CI/CD pipelines
- Environment configuration
- Production deployment and troubleshooting

---

# 📈 Architecture Evolution

```text
┌──────────────────────────────┐
│  1. Django Monolith          │
│                              │
│  Templates + Backend         │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  2. Frontend / Backend Split │
│                              │
│  Next.js → REST → Django     │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  3. Containerized Platform   │
│                              │
│  Docker + ACR + AKS          │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  4. Automated Deployment     │
│                              │
│  Azure DevOps + CI/CD        │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  5. JWT Authentication       │
│                              │
│  Access + Refresh Tokens     │
└──────────────────────────────┘
```

---

# 🎯 Engineering Outcome

The application was transformed from a tightly coupled Django application into a modern, independently deployable full-stack platform.

### Final architecture provides

- Clear frontend/backend separation
- Reusable REST APIs
- Modern TypeScript frontend
- Independent application deployments
- Containerized runtime environments
- Kubernetes-based orchestration
- Automated Azure DevOps deployment
- Centralized container image management
- Ingress-based application routing
- Token-based authentication
- Better scalability and maintainability

---

# ⭐ Key Takeaway

The most valuable part of this work was not simply learning individual technologies.

It was understanding how they connect together:

```text
Next.js
   ↓
REST API
   ↓
Django
   ↓
JWT Authentication
   ↓
Docker
   ↓
ACR
   ↓
AKS
   ↓
Ingress
   ↓
Azure DevOps CI/CD
   ↓
Production
```

This progression gave me hands-on experience across the complete **frontend → backend → authentication → container → cloud → Kubernetes → CI/CD → production** lifecycle of a modern full-stack application.
