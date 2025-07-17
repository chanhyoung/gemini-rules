# Gemini Project Brief: Vote (hodolog & nuxt-project)

This document provides a brief overview of the "Vote" project for the Gemini AI assistant.

## 1. Project Overview (프로젝트 개요)

- **Backend:** `hodolog` - A Java Spring Boot application serving as the API server.
- **Frontend:** `nuxt-project` - A Nuxt.js (Vue.js) application for the user interface.
- **Database:** PostgreSQL, managed via Docker.

The frontend (`nuxt-project`) communicates with the backend (`hodolog`) via REST API calls.

## 2. Directory Structure (디렉토리 구조)

- `D:/projects/vote/`: Project root
  - `gemini`: gemini 실행 참고 파일들.
  - `hodolog/`: Java Spring Boot backend source code.
  - `nuxt-project/`: Nuxt.js frontend source code.
  - `docker-postgresql/`: Docker configuration for the PostgreSQL database.

## 3. Tech Stack (기술 스택)

**Backend (hodolog):**

- Language: Java
- Framework: Spring Boot
- Build Tool: Maven
- Database ORM: JPA/Hibernate

**Frontend (nuxt-project):**

- Language: TypeScript
- Framework: Nuxt.js 3
- Package Manager: npm
- UI Components: Custom components, Quasar Framework.
- CSS: SCSS

## 4. Setup & Execution (설치 및 실행)

**Running the development servers:**

- **Backend:** `cd hodolog && ./mvnw spring-boot:run`
  - The server will typically start on `http://localhost:8080`.
- **Frontend:** `cd nuxt-project && npm run dev`
  - The development server will typically start on `http://localhost:3000`.

## 5. API & Configuration (API 및 설정)

- The frontend expects the backend API server to be running at `http://localhost:8080`. This might be configured in `nuxt.config.ts` or via a proxy setup.
- Backend database connection is configured in `hodolog/src/main/resources/application.properties` or `application.yml`.
- Frontend environment variables are managed in `nuxt-project/.env`.

## 6. Frontend Coding Rules (nuxt-project)

- Refer to the `D:/projects/vote/gemini/rules/FRONTEND_DEVELOPMENT_GUIDE.md` file.

## 7. Backend Coding Rules (hodolog)

- Refer to the `D:/projects/vote/gemini/rules/BACKEND_DEVELOPMENT_GUIDE.md`  file.
