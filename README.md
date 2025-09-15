# 🧩 Code Quest 2025 – Blog Comunitario (DevTalles)

[![CI](https://img.shields.io/badge/CI-GitHub_Actions-informational)](#-cicd)
[![Stack](https://img.shields.io/badge/Frontend-Astro%20%2B%20React-blue)](#-stack-tecnol%C3%B3gico)
[![Stack](https://img.shields.io/badge/Backend-NestJS-red)](#-stack-tecnol%C3%B3gico)
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](LICENSE)

> Proyecto del reto **Code Quest 2025**: un **blog comunitario** orientado a la comunidad de **DevTalles**. El foco es entregar una app usable, rápida y con una base técnica sólida.

---

## 📌 Enlaces rápidos

- 🧭 **Jira (Backlog/Board)** → https://roquejohanssen.atlassian.net/jira/software/projects/OD/boards/3  
  > *Nota:* requiere iniciar sesión con cuenta Atlassian para visualizar.
- 🎨 **Figma (Diseño UI)** → https://www.figma.com/design/UX4HwSBck2NOO36krAuzad/OPTARYS--DEVTALLES?node-id=2-33&t=DMUJ9IYllcsXAstD-1
- 📄 **Backlog exportado (CSV)** → [`docs/backlog-jira.csv`](./docs/backlog-jira.csv) *(adjuntar en el repo)*
- 🎬 **Demo (WIP)** → `docs/demo.mp4` *(opcional, adjuntar cuando esté listo)*

---

## 🗺️ Tabla de contenidos

- [Objetivo](#-objetivo)
- [Stack Tecnológico](#-stack-tecnológico)
- [Arquitectura & módulos](#-arquitectura--módulos)
- [Repos/estructura](#-reposestructura)
- [Cómo ejecutar localmente](#-cómo-ejecutar-localmente)
- [Variables de entorno](#-variables-de-entorno)
- [Calidad: lint, tests, accesibilidad](#-calidad-lint-tests-accesibilidad)
- [CI/CD](#-cicd)
- [Convenciones: ramas y commits](#-convenciones-ramas-y-commits)
- [Roadmap y épicas](#-roadmap-y-épicas)
- [Licencia](#-licencia)

---

## 🎯 Objetivo

Entregar un **blog comunitario** con:
- **Landing** con listado de posts, filtros por categoría y buscador.
- **Detalle de post** con likes y comentarios autenticados.
- **Autenticación** vía **Discord OAuth2** (sesiones seguras).
- **Panel administrativo (admin)** para CRUD de publicaciones.
- **UX responsiva**, accesible y con buen rendimiento.

Todo versionado en repos separados (**Frontend** y **Backend**), con **CI**, **tests** y **documentación**.

---

## 🧰 Stack Tecnológico

**Frontend**
- **Astro** + **React (islas)**, **TypeScript**.
- **TailwindCSS**.
- Fetch hacia API REST del backend.

**Backend**
- **NestJS** (Node.js) + **TypeScript**.
- **REST API** (y opción a GraphQL si se desea).
- **PostgreSQL** (via Prisma/TypeORM) u otra base SQL.
- **Auth**: OAuth2 (Discord) + cookies httpOnly / refresh tokens.

**Infra/Dev**
- Node 20 LTS, PNPM o NPM.
- ESLint + Prettier, Vitest/Jest, Playwright/Cypress.
- GitHub Actions.

---

## 🏗️ Arquitectura & módulos

```mermaid
flowchart TD
  A[Cliente (Astro/React)] -->|REST| B[API NestJS]
  B --> C[Servicios / Casos de uso]
  C --> D[ORM/Prisma]
  D --> E[(Base de datos SQL)]
  B --> F[OAuth2 Discord]
  A -->|Sesion| G[Cookies httpOnly / Refresh]
```

**Backend (módulos sugeridos)**
- `auth` (OAuth2 Discord, sesión)
- `users`
- `posts`
- `comments`
- `likes`
- `categories`

**Frontend (capas)**
- `components/` (UI reutilizable)
- `pages/` (rutas Astro)
- `content/` (si se usa MDX/markdown para estáticos)
- `lib/` (hooks, helpers)
- `styles/`

---

## 🗂️ Repos/Estructura

> Se recomiendan **repos separados**. Opcional: monorepo con workspaces.

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

---

## ▶️ Cómo ejecutar localmente

### Requisitos
- Node.js **20.x**
- PNPM (o NPM/Yarn)
- PostgreSQL (o motor elegido)

### 1) Backend (NestJS)
```bash
cd blog-community-api
pnpm i
cp .env.example .env            # rellena variables
# Si usas Prisma:
# pnpm prisma migrate dev
pnpm run start:dev
```
Arranca en `http://localhost:3000` (por defecto).

### 2) Frontend (Astro/React)
```bash
cd blog-community-web
pnpm i
cp .env.example .env            # rellena variables (prefijo PUBLIC_)
pnpm run dev
```
Arranca en `http://localhost:4321` (por defecto).

---

## 🔐 Variables de entorno

### Backend (`blog-community-api/.env`)
```
PORT=3000
DATABASE_URL=postgres://user:pass@localhost:5432/blog
JWT_SECRET=supersecret
CORS_ORIGIN=http://localhost:4321

DISCORD_CLIENT_ID=
DISCORD_CLIENT_SECRET=
DISCORD_REDIRECT_URI=http://localhost:3000/auth/discord/callback
```

### Frontend (`blog-community-web/.env`)
> En Astro, las variables de cliente deben llevar el prefijo `PUBLIC_`.

```
PUBLIC_API_BASE_URL=http://localhost:3000
PUBLIC_DISCORD_LOGIN_URL=http://localhost:3000/auth/discord
```

---

## ✅ Calidad: lint, tests, accesibilidad

- **Lint/Format**: ESLint + Prettier (husky + lint-staged opcional).
- **Unit tests**: Vitest/Jest en FE/BE.
- **E2E**: Playwright/Cypress (smoke: login, listar, filtrar, detalle, like, comentar).
- **A11Y**: foco visible, roles/labels, contraste AA.

Scripts sugeridos (package.json):
```json
{
  "scripts": {
    "lint": "eslint .",
    "format": "prettier --write .",
    "test": "vitest",
    "test:e2e": "playwright test"
  }
}
```

---

## 🛠 CI/CD

**GitHub Actions** (ejemplo mínimo):
- Instalar dependencias
- Lint + Tests
- Build FE/BE
- (Opcional) Deploy

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

---

## 🌿 Convenciones: ramas y commits

- Ramas: `main`, `develop`, `feature/*`, `fix/*`.
- **Conventional Commits**: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`…
- Pull Requests con checks verdes (CI) y revisión cruzada.

---

## 🗺 Roadmap y épicas

Épicas principales (ver Jira para detalle):
1. **Arquitectura y Setup** (repos, CI, .env.example, MIT).
2. **Landing & Listado** (cards responsive, paginación/scroll).
3. **Búsqueda y Filtros** (querystring, categorías).
4. **Autenticación (Discord OAuth2)** y roles.
5. **Panel Admin (CRUD)** de posts.
6. **Detalle + Likes + Comentarios**.
7. **Contenido, Demo y Entrega** (README, video 1–1:30).
8. **Calidad y UX** (tests, lint/format, A11Y).

> Backlog completo: ver Jira (requiere login) y/o el archivo [`docs/backlog-jira.csv`](./docs/backlog-jira.csv).

---

## 📜 Licencia

Este proyecto se distribuye bajo licencia **MIT**. Incluye un archivo `LICENSE` en la raíz del repositorio.

---

### 👋 Notas finales

- Cualquier feedback de diseño se centraliza en **Figma** (comentarios por frame).
- Cualquier cambio funcional se registra como **issue en Jira** con criterios de aceptación y DoD.
- Para vistas rápidas, adjunta capturas en `docs/` y enlázalas desde este README.
