The Comprehensive .windsurfrules File
Here is the full .windsurfrules file that captures best practices for a production-grade React + TypeScript app. Just copy it into your project root as .windsurfrules (no file extension).

{
  "$schema": "https://windsurf.dev/schema.json",

  // Root-level source folder
  "entryPoints": [
    "src"
  ],

  // Aliases matching tsconfig paths
  "aliases": {
    "@app": "src/app",
    "@pages": "src/pages",
    "@widgets": "src/widgets",
    "@features": "src/features",
    "@entities": "src/entities",
    "@shared": "src/shared",
    "@components": "src/shared/components",
    "@hooks": "src/shared/hooks",
    "@utils": "src/shared/utils",
    "@types": "src/shared/types",
    "@testing": "src/shared/testing",
    "@docs": "docs",
    "@tests": "tests"
  },

  "rules": [

    // ────────────────
    // Architecture & Layering
    // ────────────────

    {
      "name": "no-cross-feature-imports",
      "type": "forbid",
      "from": "src/features/*",
      "to": "src/features/*",
      "exceptSameEntry": true,
      "errorMessage": "Avoid cross-feature imports. Use public API exports only."
    },

    {
      "name": "no-import-from-app-layer",
      "type": "forbid",
      "from": [
        "src/entities/**",
        "src/features/**",
        "src/widgets/**"
      ],
      "to": "src/app/**",
      "errorMessage": "Lower layers must not import from the app layer."
    },

    {
      "name": "layered-architecture-entities",
      "type": "forbid",
      "from": "src/entities/**",
      "to": [
        "src/features/**",
        "src/widgets/**",
        "src/pages/**",
        "src/app/**"
      ],
      "errorMessage": "Entities must remain decoupled from higher layers."
    },

    {
      "name": "layered-architecture-features",
      "type": "forbid",
      "from": "src/features/**",
      "to": [
        "src/widgets/**",
        "src/pages/**",
        "src/app/**"
      ],
      "errorMessage": "Features cannot import from widgets or app."
    },

    {
      "name": "prevent-circular-dependencies",
      "type": "forbid-circular",
      "errorMessage": "Circular dependencies are not allowed. Break the loop."
    },

    // ────────────────
    // React Component Structure
    // ────────────────

    {
      "name": "component-structure",
      "type": "forbid",
      "from": "src/**/*.tsx",
      "to": "src/**/*.tsx",
      "exceptSameFile": true,
      "exceptImportingPatterns": ["**/index.tsx"],
      "errorMessage": "Components should be imported from their index files, not directly."
    },

    {
      "name": "component-barrel-exports",
      "type": "require",
      "from": "src/**/index.tsx",
      "to": "src/**/*.tsx",
      "exceptSameFile": true,
      "errorMessage": "All components should be exported through their directory's index.tsx barrel file."
    },

    {
      "name": "component-file-naming",
      "type": "file-pattern",
      "pattern": "src/**/*.tsx",
      "naming": "PascalCase",
      "errorMessage": "React component files must use PascalCase naming."
    },

    {
      "name": "hook-file-naming",
      "type": "file-pattern",
      "pattern": "src/**/use*.ts",
      "naming": "camelCase",
      "errorMessage": "Hook files must use camelCase and start with 'use'."
    },
    {
      "name": "component-max-length",
      "type": "file-size",
      "pattern": "src/**/*.tsx",
      "maxLines": 150,
      "errorMessage": "React components should not exceed 150 lines. Break it down into smaller, reusable components."
    }

    // ────────────────
    // TypeScript Best Practices
    // ────────────────

    {
      "name": "type-exports",
      "type": "require",
      "from": "src/shared/types/**/*.ts",
      "to": "src/shared/types/index.ts",
      "errorMessage": "Types should be exported through the types index file."
    },

    {
      "name": "no-any-types",
      "type": "forbid-pattern",
      "pattern": "src/**/*.ts?(x)",
      "forbiddenPattern": "any",
      "errorMessage": "Avoid using 'any' type. Use proper typing or 'unknown' instead."
    },

    {
      "name": "enforce-type-imports",
      "type": "require-pattern",
      "pattern": "src/**/*.ts?(x)",
      "requiredPattern": "import type",
      "errorMessage": "Use 'import type' for type-only imports to improve bundle size."
    },

    // ────────────────
    // State Management
    // ────────────────

    {
      "name": "state-management-isolation",
      "type": "forbid",
      "from": "src/!(features)/**",
      "to": "src/**/store/**",
      "exceptSameEntry": true,
      "errorMessage": "State management should be accessed only through feature APIs, not directly."
    },

    {
      "name": "store-structure",
      "type": "require",
      "from": "src/**/store/**",
      "to": "src/**/store/index.ts",
      "errorMessage": "Store modules must be exported through their index.ts file."
    },
    {
      "name": "redux-slice-structure",
      "type": "require",
      "from": "src/**/store/slices/**/*.ts",
      "to": "src/**/store/slices/**/index.ts",
      "errorMessage": "Redux slices must be exported through their index.ts file."
    },
    {
      "name": "redux-slice-naming",
      "type": "file-pattern",
      "pattern": "src/**/store/slices/**/*.slice.ts",
      "naming": "camelCase",
      "errorMessage": "Redux slice files must use camelCase and end with .slice.ts"
    },
    {
      "name": "redux-slice-isolation",
      "type": "forbid",
      "from": "src/**",
      "to": "src/**/store/slices/**/*.ts",
      "exceptSameEntry": true,
      "exceptImportingPatterns": ["**/store/**"],
      "errorMessage": "Redux slices should only be imported by store modules, not directly by components."
    },
    {
      "name": "redux-selectors-required",
      "type": "require-pattern",
      "pattern": "src/**/store/slices/**/*.ts",
      "requiredPattern": "export const select",
      "errorMessage": "Redux slices must export selectors for accessing state."
    },
    {
      "name": "redux-thunk-structure",
      "type": "require",
      "from": "src/**/store/thunks/**/*.ts",
      "to": "src/**/store/thunks/**/index.ts",
      "errorMessage": "Redux thunks must be exported through their index.ts file."
    },
    {
      "name": "redux-state-typing",
      "type": "require-pattern",
      "pattern": "src/**/store/slices/**/*.ts",
      "requiredPattern": "interface.*State",
      "errorMessage": "Redux slice state must be properly typed with an interface."
    },
    {
      "name": "redux-immutable-updates",
      "type": "forbid-pattern",
      "pattern": "src/**/store/slices/**/*.ts",
      "forbiddenPattern": "state\\..* =",
      "errorMessage": "Redux state updates must use immutable patterns. Use the Redux Toolkit state mutation syntax instead."
    }

    // ────────────────
    // Shared Access Rules
    // ────────────────

    {
      "name": "allow-hooks-global",
      "type": "allow",
      "from": "**",
      "to": "src/shared/hooks/**"
    },

    {
      "name": "allow-utils-global",
      "type": "allow",
      "from": "**",
      "to": "src/shared/utils/**"
    },

    {
      "name": "allow-types-global",
      "type": "allow",
      "from": "**",
      "to": "src/shared/types/**"
    },

    {
      "name": "allow-shared-components-global",
      "type": "allow",
      "from": "**",
      "to": "src/shared/components/**"
    },

    {
      "name": "shared-components-structure",
      "type": "require",
      "from": "src/shared/components/*",
      "to": "src/shared/components/*/index.tsx",
      "errorMessage": "Shared components must have an index.tsx file for public API exports."
    },

    // ────────────────
    // Test & Documentation Rules
    // ────────────────

    {
      "name": "no-test-imports-in-prod",
      "type": "forbid",
      "from": "src/**",
      "to": [
        "**/*.test.ts",
        "**/*.test.tsx",
        "src/shared/testing/**",
        "tests/**"
      ],
      "errorMessage": "Production code must not import from test files or test helpers."
    },

    {
      "name": "test-utils-isolation",
      "type": "allow",
      "from": "tests/**",
      "to": "src/shared/testing/**"
    },

    {
      "name": "test-coverage",
      "type": "require",
      "from": "src/**/*.tsx",
      "to": "src/**/*.test.tsx",
      "errorMessage": "All React components should have corresponding test files."
    },

    {
      "name": "no-docs-in-prod",
      "type": "forbid",
      "from": "src/**",
      "to": "docs/**",
      "errorMessage": "Documentation code must remain isolated from runtime."
    },

    {
      "name": "storybook-isolation",
      "type": "forbid",
      "from": "**",
      "to": "**/*.stories.tsx",
      "errorMessage": "Storybook stories must not be imported into other modules."
    },

    {
      "name": "storybook-coverage",
      "type": "require",
      "from": "src/shared/components/**/*.tsx",
      "to": "src/shared/components/**/*.stories.tsx",
      "errorMessage": "All shared components should have Storybook stories."
    },

    // ────────────────
    // Internal APIs
    // ────────────────

    {
      "name": "no-imports-from-internal-folders",
      "type": "forbid",
      "from": "**",
      "to": "**/internal/**",
      "errorMessage": "Do not import internal modules directly. Use public API exports."
    },

    {
      "name": "public-api-exports",
      "type": "require",
      "from": [
        "src/features/*",
        "src/entities/*",
        "src/widgets/*"
      ],
      "to": [
        "src/features/*/index.ts",
        "src/entities/*/index.ts",
        "src/widgets/*/index.ts"
      ],
      "errorMessage": "Each module must have a public API through its index.ts file."
    },

    // ────────────────
    // Performance Rules
    // ────────────────

    {
      "name": "no-direct-redux-imports",
      "type": "forbid",
      "from": "src/**",
      "to": "redux",
      "errorMessage": "Import from our abstraction layer instead of directly from Redux."
    },

    {
      "name": "memoization-required",
      "type": "require-pattern",
      "pattern": "src/**/*.tsx",
      "requiredPattern": "(React.memo|useMemo|useCallback)",
      "errorMessage": "Use memoization techniques for optimized rendering."
    },

    // ────────────────
    // Code Quality Rules
    // ────────────────

    {
      "name": "no-direct-dom-manipulation",
      "type": "forbid-pattern",
      "pattern": "src/**/*.tsx",
      "forbiddenPattern": "document\\.|window\\.",
      "errorMessage": "Avoid direct DOM manipulation. Use React refs instead."
    },

    {
      "name": "enforce-error-boundaries",
      "type": "require-pattern",
      "pattern": "src/app/**/*.tsx",
      "requiredPattern": "ErrorBoundary",
      "errorMessage": "Top-level components should use ErrorBoundary for graceful error handling."
    },

    {
      "name": "enforce-accessibility",
      "type": "require-pattern",
      "pattern": "src/**/*.tsx",
      "requiredPattern": "aria-|role=",
      "errorMessage": "Ensure components use proper accessibility attributes."
    }
  ]
}
