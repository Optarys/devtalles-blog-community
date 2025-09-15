<!-- README con identidad DevTalles usando material gráfico de /.assets (sin guías de uso) -->

<div align="center">

  <img src="./assets/LOGO%20N.png" alt="DevTalles Logo" height="84" />
  <br/>
  <img src="./assets/DEVI%20HELLO.png" alt="DEVI Hello" height="140" />

  <h1>Code Quest 2025 — Blog Comunitario</h1>
  <p>Un blog comunitario, rápido y accesible, con UI cuidada y base técnica lista para crecer.</p>

  <a href="https://roquejohanssen.atlassian.net/jira/software/projects/OD/boards/3">
    <img alt="Jira Board" src="https://img.shields.io/badge/Jira-Backlog%20%2F%20Board-0052CC?logo=jira&logoColor=white"/>
  </a>
  &nbsp;
  <a href="https://www.figma.com/design/UX4HwSBck2NOO36krAuzad/OPTARYS--DEVTALLES?node-id=2-33&t=DMUJ9IYllcsXAstD-1">
    <img alt="Figma file" src="https://img.shields.io/badge/Figma-Dise%C3%B1o%20UI-F24E1E?logo=figma&logoColor=white"/>
  </a>
  &nbsp;
  <img alt="Frontend" src="https://img.shields.io/badge/Frontend-Astro%20%2B%20React-5A45FF?logo=astro&logoColor=white"/>
  <img alt="Styles" src="https://img.shields.io/badge/UI-TailwindCSS%20%2B%20Flowbite-06B6D4?logo=tailwindcss&logoColor=white"/>
  <a href="#-licencia"><img alt="License MIT" src="https://img.shields.io/badge/License-MIT-brightgreen"/></a>

</div>

<p align="center">
  <img src=".assets/ISOLOGO%20COLOR.png" alt="Isologo DevTalles" height="18" />
</p>

## 📌 Enlaces rápidos

- 🧭 **Jira (Backlog/Board)** → https://roquejohanssen.atlassian.net/jira/software/projects/OD/boards/3  
  <sub>Requiere iniciar sesión con Atlassian.</sub>
- 🎨 **Figma (Diseño UI)** → https://www.figma.com/design/UX4HwSBck2NOO36krAuzad/OPTARYS--DEVTALLES?node-id=2-33&t=DMUJ9IYllcsXAstD-1
- 📄 **Backlog exportado (CSV)** → [docs/jira_backlog_codequest2025.csv](./docs/jira_backlog_codequest2025.csv)

<p align="center">
  <img src=".assets/DEVI%20LAPTOP.png" alt="DEVI Laptop" height="120" />
</p>

## 🎯 Objetivo

- **Landing** con listado de posts, filtros por categoría y buscador.  
- **Detalle de post** con likes y comentarios autenticados.  
- **Autenticación** vía **Discord OAuth2** (sesiones seguras).  
- **Panel administrativo** (UI) para CRUD de publicaciones.  
- **UX responsiva** y accesible con buen rendimiento.

## 🧰 Stack Tecnológico

**Frontend**: Astro + React (islas), TypeScript, TailwindCSS, Flowbite React.  
**Backend**: NestJS (TypeScript), REST (opcional GraphQL), PostgreSQL (Prisma/TypeORM).  
**Dev/Calidad**: Node 20, ESLint + Prettier, Vitest/Jest, Playwright/Cypress, GitHub Actions.

<p align="center">
  <img src=".assets/DEVI%20NORMAL%20BORDER.png" alt="DEVI Normal" height="110" />
</p>

## 🏗️ Arquitectura & módulos

```mermaid
flowchart TD
  A[Cliente (Astro/React)] -->|REST| B[API NestJS]
  B --> C[Servicios / Casos de uso]
  C --> D[ORM/Prisma]
  D --> E[(Base de datos SQL)]
  B --> F[OAuth2 Discord]
  A -->|Sesión| G[Cookies httpOnly / Refresh]
```

**Backend**: `auth`, `users`, `posts`, `comments`, `likes`, `categories`  
**Frontend**: `components/`, `pages/`, `content/`, `lib/`, `styles/`

## 🗂️ Repos/Estructura

```
blog-community-web/      # Frontend (Astro/React)
└─ src/
   ├─ components/
   ├─ pages/
   ├─ lib/
   ├─ styles/
   └─ env.d.ts

blog-community-api/      # Backend (NestJS)
└─ src/
   ├─ modules/ (auth, posts, comments, users, likes, categories)
   ├─ common/ (guards, pipes, interceptors)
   ├─ config/
   └─ main.ts
```

<p align="center">
  <img src=".assets/ISOLOGO%20B.png" alt="Isologo blanco" height="24" />
</p>

## ▶️ Cómo ejecutar localmente

### Requisitos
- Node.js **20.x**
- PNPM o NPM/Yarn
- (Opcional) PostgreSQL

### 1) Backend (NestJS)
```bash
cd blog-community-api
pnpm i
cp .env.example .env
# Si usas Prisma:
# pnpm prisma migrate dev
pnpm run start:dev
```
http://localhost:3000

### 2) Frontend (Astro/React)
```bash
cd blog-community-web
pnpm i
cp .env.example .env       # variables con prefijo PUBLIC_
pnpm run dev
```
http://localhost:4321

## 🔐 Variables de entorno

**Backend**
```
PORT=3000
DATABASE_URL=postgres://user:pass@localhost:5432/blog
JWT_SECRET=supersecret
CORS_ORIGIN=http://localhost:4321

DISCORD_CLIENT_ID=
DISCORD_CLIENT_SECRET=
DISCORD_REDIRECT_URI=http://localhost:3000/auth/discord/callback
```

**Frontend**
```
PUBLIC_API_BASE_URL=http://localhost:3000
PUBLIC_DISCORD_LOGIN_URL=http://localhost:3000/auth/discord
```

<p align="center">
  <img src="./assets/DEVI%20HELLO%20BORDER.png" alt="DEVI Hello Border" height="120" />
</p>

## 🛠 CI/CD (sugerido)

```yaml
name: CI
on: [pull_request, push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: pnpm i --frozen-lockfile
      - run: pnpm -r lint && pnpm -r test && pnpm -r build
```

## 📜 Licencia

Este proyecto se distribuye bajo licencia **MIT**.

<p align="center">
  <img src=".assets/LOGO%20B.png" alt="DevTalles Logo Blanco" height="42" />
</p>
