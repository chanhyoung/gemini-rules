# nuxt-project 개발 규칙

이 문서는 `nuxt-project`의 일관성 있는 개발을 위한 규칙을 정의합니다.

## 1. 코드 스타일 및 포맷팅

- **Prettier**: 코드 포맷팅은 Prettier 설정을 따릅니다.
  - `semi`: `false` (세미콜론 사용 안 함)
  - `singleQuote`: `true` (작은따옴표 사용)
  - `arrowParens`: `avoid` (화살표 함수 파라미터가 하나일 때 괄호 생략)
  - `tabWidth`: `2` (탭 너비는 2칸)
  - `trailingComma`: `all` (항상 후행 쉼표 사용)
  - `printWidth`: `80` (한 줄의 최대 길이는 80자)
- **ESLint**: 코드 품질은 ESLint 설정을 따릅니다.
  - `no-console`: `off` (개발 중 `console.log` 사용 가능)
  - `vue/require-default-prop`: `off` (Vue 컴포넌트의 prop에 기본값 정의를 강제하지 않음)

### 실행 명령어

- 코드 포맷팅 검사: `npm run prettier`
- 코드 포맷팅 수정: `npm run prettier:fix`
- 코드 품질 검사: `npm run lint`
- 코드 품질 수정: `npm run lint:fix`

## 2. 주요 기술 스택

- **프레임워크**: [Nuxt.js](https://nuxt.com/) v3
- **UI 라이브러리**: [Quasar UI](https://quasar.dev/)
- **상태 관리**: [Pinia](https://pinia.vuejs.org/)
- **TypeScript**: 타입 검사를 필수로 사용합니다 (`typeCheck: true`).
- **CSS**: 기본적으로 컴포넌트 스코프 스타일을 사용합니다.

## 3. 디렉토리 구조 및 컨벤션

- **`components`**: 재사용 가능한 UI 컴포넌트를 배치합니다. 기능별 하위 디렉토리(예: `Post`, `Course`)로 그룹화함.
- **`composables`**: 재사용 가능한 로직(Composition API)을 작성. (예: `useAuth.js`)
- **`pages`**: 라우팅과 연결되는 페이지 컴포넌트를 배치. Nuxt의 파일 기반 라우팅 규칙을 따름.
- **`layouts`**: 페이지의 레이아웃을 정의. (예: `default.vue`, `admin.vue`)
- **`stores`**: Pinia 스토어를 기능별로 분리하여 작성. (예: `post.ts`, `auth.ts`)
- **`types`**: 프로젝트 전역에서 사용되는 TypeScript 타입을 정의.

## 4. 상태 관리 (Pinia)

- 기능별로 스토어 파일을 분리하여 `stores` 디렉토리에서 관리.
- 스토어 ID는 파일명과 일치시키고, `defineStore`를 사용하여 명확하게 정의.

## 5. API 통신

- 클라이언트 사이드의 API 통신은 `stores` 에 구현하고 `useFetchWithAuth` 또는 `fetchWithAuth`를 사용하여 API를 호출.
  - **API 통신 흐름**: `***.vue` -> `stores` -> `Backend RestAPI` 순서로 진행됨.
  - **store 작성 예**: `stores/post.ts`

## 6. 환경 변수

- 환경 변수는 `nuxt.config.ts`의 `runtimeConfig`을 통해 관리합니다.
- 서버 전용 변수는 최상위 `runtimeConfig`에, 클라이언트와 서버에서 모두 사용 가능한 변수는 `public` 객체 내에 정의합니다.
