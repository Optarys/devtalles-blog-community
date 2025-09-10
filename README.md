# 🌐 Blog Comunitario

Este proyecto es una aplicación web tipo blog comunitario donde los usuarios pueden compartir publicaciones sobre programación y temas relacionados con tecnología.  
El sistema está dividido en **dos repositorios principales**: **Backend (API)** y **Frontend (WebApp)**.

---

## 📂 Estructura de Repositorios

### 1. Backend - API
📌 **Repositorio:** [`blog-community-backend`](https://github.com/Optarys/devtalles-blog-community-backend)  
Este repositorio contiene la lógica del servidor y la API REST/GraphQL que expone los datos para el frontend.

- **Tecnología:** NestJS (Node.js) / Express / .NET / Laravel (según tu stack).  
- **Base de datos:** PostgreSQL / MongoDB.  
- **Autenticación:** JWT + Refresh Tokens (OAuth opcional).  
- **Características principales:**
  - Gestión de usuarios (registro, login, roles).
  - CRUD de publicaciones.
  - Comentarios y reacciones.
  - Categorías y etiquetas.
  - Sistema de notificaciones en tiempo real (WebSockets).
  - Documentación con Swagger / Postman.

📂 Estructura sugerida:


---

### 2. Frontend - WebApp
📌 **Repositorio:** [`blog-community-web`](https://github.com/Optarys/devtalles-blog-community-web)  
Este repositorio contiene la aplicación web con la que interactúan los usuarios.

- **Tecnología:** Angular / React / Vue (según preferencia).  
- **Estilos:** TailwindCSS / Bootstrap / Material UI.  
- **Características principales:**
  - Autenticación con JWT.
  - Página principal con feed de publicaciones.
  - Editor en Markdown para posts.
  - Gestión de comentarios y reacciones.
  - Perfil de usuario.
  - Panel de administración (moderación y categorías).
  - Integración con la API.

📂 Estructura sugerida:


# 📊 Arquitectura del Blog Comunitario

```mermaid
flowchart TD
    subgraph Frontend["🌐 Frontend (blog-community-web)"]
        UI[UI Web: React/Angular/Vue]
        AuthUI[Gestión de Sesión JWT]
        Pages[Páginas: Feed, Post, Perfil, Admin]
    end

    subgraph Backend["⚙️ Backend API (blog-community-api)"]
        Auth[Auth Service (JWT, OAuth)]
        Users[Users Module]
        Posts[Posts Module]
        Comments[Comments Module]
        Reactions[Reactions Module]
        Tags[Tags Module]
    end

    subgraph Database["🗄️ Base de Datos (PostgreSQL/MongoDB)"]
        TUsers[(Usuarios)]
        TPosts[(Publicaciones)]
        TComments[(Comentarios)]
        TReactions[(Reacciones)]
        TTags[(Categorías/Etiquetas)]
    end

    %% Conexiones
    UI -->|REST/GraphQL| Backend
    AuthUI --> Auth
    Pages --> Posts

    Auth --> TUsers
    Users --> TUsers
    Posts --> TPosts
    Comments --> TComments
    Reactions --> TReactions
    Tags --> TTags

