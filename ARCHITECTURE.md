# Sudo-Flix Architecture

## 1. Introduction/Overview
*   **Project Name:** Sudo-Flix
*   **Purpose:** A web-based media streaming application designed to allow users to watch movies and TV shows. Its goal is to provide a free, ad-free platform for this.
*   **Key Goals/Features:**
    *   User-friendly interface for browsing and discovering media.
    *   Effective media streaming (using HLS.js).
    *   User accounts for bookmarks and progress tracking.
    *   Good user experience across devices (PWA, Chromecast).
    *   Support for multiple languages.
    *   Ad-free viewing.

## 2. Project Setup and Dependencies
*   **Package Manager:** Uses `pnpm`. Enforced by a `preinstall` script.
*   **Key Scripts:**
    *   `dev`: Starts Vite development server.
    *   `build`: Creates production build (Vite).
    *   `build:pwa`: Production build with PWA features.
    *   `test`: Runs tests (Vitest).
    *   `lint`: Lints codebase (ESLint).
*   **Key Dependencies:** React, react-dom, react-router-dom, zustand, tailwindcss, @headlessui/react, i18next, react-i18next, hls.js, ofetch, @noble/hashes, jwt-decode, @plasmohq/messaging.
*   **Key Dev Dependencies:** Vite, typescript, eslint, prettier, vitest, vite-plugin-pwa, tailwindcss-themer, million.

## 3. Core Technologies
*   **React:** JavaScript library for building UIs with reusable components.
*   **Vite:** Modern frontend build tool (fast dev server, Rollup for bundling).
*   **TypeScript:** Superset of JavaScript adding static typing.
*   **Tailwind CSS:** Utility-first CSS framework for rapid UI development.
*   **Zustand:** Small, fast, scalable state management for React.
*   **i18next & react-i18next:** Internationalization framework.
*   **HLS.js:** JavaScript library for HLS playback.
*   **PNPM:** Fast, disk space-efficient package manager.

## 4. Directory Structure
*   **High-Level:** `.github/` (workflows, issue templates), `.vscode/` (editor settings), `plugins/` (custom Vite plugins), `public/` (static assets, `config.js`), `src/` (main source code), `themes/` (Tailwind themes). Config files for Docker, ESLint, Prettier, Vite, etc.
*   **`src/` Deep Dive:**
    *   `@types/`: Custom TypeScript definitions.
    *   `assets/`: CSS, locales (i18n JSON files), Handlebars templates.
    *   `backend/`: Logic for backend interactions (accounts, extension messaging, metadata fetching from TMDB/JustWatch, stream providers).
    *   `components/`: Reusable React UI components (buttons, forms, layout, media display, overlays, extensive custom player).
    *   `hooks/`: Custom React hooks (auth, etc.).
    *   `pages/`: Page components (Home, Discover, Player, Settings, Admin, Error pages, Layouts).
    *   `setup/`: Application initialization (root App component, config, i18n, PWA, Chromecast).
    *   `stores/`: Zustand global state stores (auth, bookmarks, progress, language, theme, player state, etc.). Includes "Syncer" components for persistence. `__old/` for legacy/migration.
    *   `utils/`: General utility functions.
    *   `index.tsx`: Main React application entry point.

## 5. Frontend Architecture
*   **Overall:** SPA built with React, TypeScript, Vite. Component-based, global state management, separation of concerns.
*   **Components (`src/components/`):** Modular, reusable, organized by function. Styled with Tailwind CSS. Uses `@headlessui/react`, `@dnd-kit`. Custom player is a major component area.
*   **State Management (`src/stores/`):** Zustand for global state. Modular stores for auth, bookmarks, progress, language, theme, player. Syncer components suggest persistence.
*   **Routing (`src/pages/`, `src/index.tsx`):** `react-router-dom` for client-side routing. Supports `BrowserRouter` and `HashRouter`. Page components in `src/pages/`, with shared layouts.
*   **UI (Styling & Theming):** Tailwind CSS primary. PostCSS for RTL support. Global styles in `src/assets/css/index.css`. Theming via `themes/` and `tailwindcss-themer`, managed by `ThemeProvider` and Zustand store.

## 6. Backend Interaction (`src/backend/`)
*   **Overview:** Interacts with backend services for auth, account management, media metadata, stream sourcing. Backend URL is configurable.
*   **Authentication:** User registration, login, session management (likely token-based). Client-side crypto operations.
*   **User Account Management:** Bookmarks, viewing progress, user settings persistence. Data import functionality.
*   **Media Metadata:** Fetches movie/TV show details (TMDB integration), search functionality, JustWatch integration (adapted).
*   **Stream Providers:** Manages multiple media stream providers (`@movie-web/providers` fork). Fetches stream data and subtitles.
*   **Browser Extension Interaction:** Communicates with a companion extension (`@plasmohq/messaging`) for enhanced capabilities.
*   **Helpers:** HTTP request utilities, error reporting.

## 7. Build Process (`vite.config.mts`)
*   **Vite & Rollup:** Fast dev server (HMR), Rollup for production bundling. React compilation via `@vitejs/plugin-react`, Babel, core-js. `million/compiler` for optimization.
*   **Code Splitting:** Automatic and custom manual chunks (language-db, hls, auth, locales, react-dom, Icons, caption-parsing).
*   **CSS Handling:** Tailwind CSS, PostCSS with RTL support.
*   **PWA Generation:** `vite-plugin-pwa` for service worker (Workbox), manifest, offline capabilities. Conditional via `VITE_PWA_ENABLED`.
*   **Environment Variables:** Vite `loadEnv` for configuring features (base URL, domain, OpenSearch, router type, PWA).
*   **Versioning:** `vite-plugin-package-version`.
*   **Analysis:** `rollup-plugin-visualizer`.
*   **Path Aliases:** For cleaner imports.

## 8. Key Features and Modules
*   **Media Player (`src/components/player/`):** Custom HLS.js based player, rich controls, subtitle/quality selection, Chromecast. Modular design, Zustand for state.
*   **Authentication & User Accounts (`src/backend/accounts/`, `src/stores/auth/`):** Registration, login, persistent bookmarks, progress, settings.
*   **Internationalization (i18n) (`src/assets/locales/`, `src/setup/i18n.ts`):** `i18next` for multi-language support. Many language files.
*   **Progressive Web App (PWA):** Installable, app-like experience, offline caching via service worker, rich manifest, splash screens.
*   **Content Discovery & Metadata (`src/backend/metadata/`):** TMDB for rich metadata, search, discovery features, provider integration.
*   **Theming (`themes/`, `src/stores/theme/`):** Customizable UI via `tailwindcss-themer`.
*   **Browser Extension Integration (`src/backend/extension/`):** Companion extension for enhanced functionality.

## 9. Conclusion
*   **Summary:** Sudo-Flix is a modern, well-architected React application using Vite and TypeScript.
*   **Strengths:** Component-based design, scalable state management (Zustand), clear separation of concerns, extensibility (PWA, theming, i18n, extension), focus on UX, optimized build process.
*   **Outlook:** Solid foundation for a comprehensive media streaming platform.
