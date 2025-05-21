# Sudo-Flix Code Style Guide

## 1. Introduction

This document outlines the coding style and conventions to be followed when contributing to the Sudo-Flix project. Adhering to these guidelines helps maintain code consistency, readability, and maintainability across the codebase.

Our primary goal is to write clean, understandable, and efficient code. This guide is a living document and may be updated as the project evolves.

## 2. Formatting

**Primary Tool: Prettier**

*   The project uses [Prettier](https://prettier.io/) for automatic code formatting. Configuration is in `prettierrc.js`.
*   **Key Prettier settings:**
    *   `singleQuote: true` (use single quotes for strings).
    *   `trailingComma: "all"` (use trailing commas wherever possible).
*   Prettier is integrated into ESLint (`plugin:prettier/recommended`), so formatting issues will be flagged as linting errors.
*   Run `pnpm lint:fix` (which runs `eslint --fix`) to automatically format your code according to Prettier rules.

**Tailwind CSS Class Sorting**

*   The `prettier-plugin-tailwindcss` is used to automatically sort Tailwind CSS classes in your `.tsx` files. This ensures a consistent class order.

**Manual Formatting**

*   For aspects not covered by Prettier (e.g., logical grouping of code, complex layout in JSX), strive for clarity and readability.
*   Keep lines to a reasonable length.

## 3. Naming Conventions

*   **Variables and Functions:** Use `camelCase`.
    *   Example: `const videoPlayer = ...;`, `function calculateTotal() { ... }`
*   **TypeScript Types and Interfaces:** Use `PascalCase`.
    *   Example: `interface UserProfile { ... }`, `type MediaId = string;`
*   **React Components:** Use `PascalCase`.
    *   Example: `function MediaGrid(props: MediaGridProps) { ... }`
*   **React Component Props Interfaces:** Typically named `Props` if it's the main props interface for a component, or `ComponentNameProps` for clarity if needed.
    *   Example: `interface ButtonProps { ... }`
*   **File Names:**
    *   React components: `PascalCase.tsx` (e.g., `UserProfileCard.tsx`).
    *   Non-component TypeScript files (hooks, utils, stores, backend): `camelCase.ts` (e.g., `useAuth.ts`, `formatters.ts`).
    *   Barrel files: `index.ts` (e.g., `src/components/index.ts`).
*   **Enum-like Types:** Use `PascalCase` for the type name and `PascalCase` for string literal union members if they represent distinct states, or `kebab-case` / `camelCase` if they map to CSS values or other specific string constants.
    *   Example: `type PlayerStatus = 'Playing' | 'Paused' | 'Buffering';`
    *   Example: `type ThemeType = "light" | "dark" | "sepia";` (as seen with button themes)

## 4. TypeScript Specifics

*   **Types vs. Interfaces:** Prefer `interface` for defining the shape of objects (e.g., component props, API responses, store state). `type` can be used for unions, intersections, primitives, or more complex type manipulations.
*   **Explicit Typing:** Provide explicit types for function parameters and return values.
    *   Example: `function greet(name: string): string { return \`Hello, \${name}\`; }`
*   **`any` Type:** The use of `any` is allowed (`@typescript-eslint/no-explicit-any: "off"` in ESLint) but should be minimized. Use it only when the type is genuinely unknown or during initial prototyping. Strive to use more specific types like `unknown` or generics where possible.
*   **Module Imports/Exports:**
    *   Use ES6 module syntax (`import` and `export`).
    *   Prefer named exports: `export function MyComponent() {}`, `export const utilFunction = () => {}`.
    *   Default exports are allowed but not enforced (`import/prefer-default-export: "off"`).
    *   Do not use file extensions (`.ts`, `.tsx`) in import paths for TypeScript files: `import/extensions: ["error", "ignorePackages", { ts: "never", tsx: "never" }]`.
    *   Use path aliases like `@/` for imports from the `src` directory (e.g., `import { MyComponent } from '@/components/MyComponent';`).
    *   Import order is strictly enforced by ESLint (`import/order`). Imports are grouped (builtin, external, internal, etc.) and alphabetized.
*   **Arrow Functions vs. `function` Keyword:** Both are acceptable.
    *   Use `function MyComponent() {}` for React component declarations.
    *   Arrow functions are common for callbacks (especially with `useCallback`), inline functions, and sometimes for utility functions. Choose based on context and `this` binding needs (though less relevant in functional components/modern JS).
*   **Unused Variables:** Unused variables will trigger a warning. If a variable or parameter is intentionally unused, prefix it with an underscore `_` (e.g., `function handleClick(_event: React.MouseEvent) {}`). ESLint is configured to ignore these (`@typescript-eslint/no-unused-vars` with `argsIgnorePattern: "^_"`).

## 5. React Component Style

*   **Functional Components:** All React components should be functional components using hooks.
*   **Props:** Define props using TypeScript interfaces.
*   **Hooks:** Utilize React hooks (`useState`, `useEffect`, `useContext`, `useCallback`, `useMemo`, etc.) for state and side effects.
*   **Custom Hooks:** Encapsulate reusable component logic in custom hooks, named with the `use` prefix (e.g., `useAuth`).
*   **JSX:**
    *   JSX syntax should only be used in `.tsx` files (or `.jsx`).
    *   Spread props (`{...props}`) are allowed where appropriate (`react/jsx-props-no-spreading: "off"`).
*   **Accessibility (a11y):** While automated `jsx-a11y` ESLint rules are currently turned **off**, strive to write accessible components. Consider ARIA attributes, semantic HTML, and keyboard navigation. The custom `.tabbable` class is provided for enhanced focus visibility.

## 6. Commenting

*   Write clear and concise comments to explain complex logic, non-obvious decisions, or important context.
*   Avoid over-commenting simple or self-explanatory code.
*   JSDoc-style comments for functions (explaining parameters and return values) are encouraged for complex utility functions or public APIs within the codebase, though not strictly enforced for all functions.
*   Use `// TODO:` comments to mark areas that need future attention.

## 7. CSS and Tailwind CSS

*   **Tailwind First:** Prioritize using [Tailwind CSS](https://tailwindcss.com/) utility classes directly in your `.tsx` components for styling.
*   **Custom CSS (`src/assets/css/index.css`):**
    *   When utility classes are insufficient, custom CSS can be added.
    *   Prefer using `@apply` with Tailwind's theme tokens to maintain consistency (e.g., `@apply text-primary bg-background-main;`).
    *   For highly specialized components (like the range slider), custom CSS classes and CSS variables are acceptable.
*   **Theming:**
    *   The project uses `tailwindcss-themer` for multi-theme support. Theme definitions are in the `themes/` directory.
    *   Utilize theme-aware colors and styles defined within the theme objects (e.g., `bg-primary`, `text-accent-text`). Refer to the theme files for available tokens.
*   **Font:** The primary application font is 'DM Sans', applied via the `font-main` utility class (defined in `tailwind.config.ts`).
*   **RTL (Right-to-Left) Support:** Styling should be RTL-aware. `postcss-rtlcss` is used to help automate this, but manual adjustments might be needed. Pay attention to layout, text alignment, and transforms.

## 8. General Best Practices & Project Conventions

*   **ESLint Rules:** Familiarize yourself with the ESLint configuration (`.eslintrc.cjs`). It extends Airbnb's style guide with TypeScript support and project-specific overrides. Address or disable ESLint warnings/errors before committing.
*   **Console Logging:** `console.log()` will produce a warning. Use it for temporary debugging. `console.warn()`, `console.error()`, and `console.debug()` are permitted for more persistent messages.
*   **Error Handling:** Implement proper error handling, especially for API calls and asynchronous operations.
*   **State Management (Zustand):**
    *   Global state is managed with Zustand. Stores are located in `src/stores/`.
    *   Use `immer` for immutable updates within store actions.
    *   `persist` middleware is used for local storage persistence where appropriate.
*   **Code Readability:** Write code that is easy to understand. Break down complex functions and components into smaller, manageable pieces.
*   **Avoid Magic Strings/Numbers:** Use constants or enums where appropriate.

This guide serves as a baseline. Always prioritize clarity and maintainability in your code.
