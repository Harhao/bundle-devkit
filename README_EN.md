<h1 align="center">BundleKit</h1>

<p align="center">
  <strong>Unified frontend builds. No more config migration.</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@bundlekit/service"><img src="https://img.shields.io/npm/v/@bundlekit/service.svg" alt="npm version" /></a>
  <a href="https://www.npmjs.com/package/@bundlekit/cli"><img src="https://img.shields.io/npm/v/@bundlekit/cli.svg" alt="npm version" /></a>
  <a href="https://github.com/Harhao/bundlekit/blob/master/LICENSE"><img src="https://img.shields.io/npm/l/@bundlekit/service.svg" alt="license" /></a>
  <a href="https://bundlekit.harhao.workers.dev"><img src="https://img.shields.io/badge/docs-online-blue" alt="docs" /></a>
  <a href="./README.md"><img src="https://img.shields.io/badge/lang-中文-blue" alt="中文" /></a>
</p>

---

## Introduction

**BundleKit** is a unified frontend build toolkit that lets you write **one `.bundlekitrc.ts` configuration** and freely switch between bundlers — Webpack, Vite, Rollup, Rspack, Rolldown, Parcel, or esbuild — without changing a single line of config.

No more vendor lock-in. No more rewriting build configs when migrating tools. BundleKit abstracts away the differences between bundlers behind a clean adapter pattern, so you can focus on shipping features.

---

## Features

- 🚀 **Multi-bundler Support** — One config works with Webpack / Vite / Rollup / Rspack / Rolldown / Parcel / esbuild
- 🧩 **Plugin Architecture** — Extend framework support via plugins: React / Vue 3 / Svelte / Angular / Node.js
- 🎯 **CLI Scaffolding** — Interactive project creation with framework, bundler, language, and package manager selection
- 🔌 **Plugin Management** — Add framework plugins to existing projects (`bc add react`)
- 📦 **Library Mode** — Multi-format output — ESM / CJS / UMD (`--lib` / `--library-name`)
- 🖥️ **SSR Support** — Built-in server-side rendering with dual-pass build (`--ssr`)
- 🤖 **MCP Integration** — AI-powered project creation via Model Context Protocol
- 📖 **Documentation** — Full docs site with AI-powered Q&A assistant
- ✅ **Tested** — Unit tests + Integration tests + E2E tests (Vitest + Playwright)

---

## Quick Start

### Create a new project

```bash
# Using full command
npx @bundlekit/cli create my-app

# Or use the short alias
bc create my-app
```

The interactive wizard lets you choose:

- Template (framework + language)
- Bundler
- Package manager
- SSR toggle
- Library mode toggle

### Add plugins to existing projects

```bash
bc add react
bc add vue
bc add svelte
bc add angular
bc add node
```

### Common options

```bash
# Skip interactive prompts
bc create my-app --template react-ts --bundler vite

# Create SSR project
bc create my-app --template vue3-ts --bundler vite --ssr

# Create library
bc create my-lib --template node-ts --bundler rollup --lib --library-name MyLib
```

**Available templates**: `react-ts` `react-js` `vue3-ts` `vue3-js` `svelte-ts` `svelte-js` `angular-ts` `angular-js` `node-ts`

**Available bundlers**: `vite` `webpack` `rspack` `rollup` `rolldown` `parcel` `esbuild`

**Package managers**: `npm` `yarn` `pnpm` (default: pnpm)

---

## Configuration

Create a `.bundlekitrc.ts` (or `.bundlekitrc.js`) in your project root:

```typescript
import { defineConfig } from '@bundlekit/service';

export default defineConfig({
  bundler: 'vite',
  plugins: ['react'],
  // SSR config (optional)
  ssr: {
    enabled: true,
  },
  // Dev environment
  dev: {
    port: 3000,
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
      },
    },
  },
  // Production build
  build: {
    outDir: 'dist',
    sourcemap: true,
  },
});
```

> **Switching bundlers** only requires changing `bundler: 'vite'` to `bundler: 'webpack'` — everything else stays the same.

### Use the build service

```bash
# Start dev server
ds serve

# Production build
ds build
```

---

## Architecture

BundleKit follows the **Adapter Pattern** to decouple your config from any specific bundler:

```
 .bundlekitrc.ts          Unified config
        │
        ▼
   Service (core)          Orchestrates plugins & bundler resolution
        │
        ▼
  Bundler Adapter           Translates config → native bundler format
  ┌─────┴─────┐
  │  Vite     │  Webpack  │  Rollup  │  Rspack  │  Rolldown  │  Parcel  │  esbuild
  └─────┬─────┘
        │
        ▼
  Framework Plugin          Sets framework context
  ┌─────┴─────┐
  │  React    │  Vue 3   │  Svelte  │  Angular  │  Node.js
  └───────────┘
```

Each bundler adapter implements the `IBuildToolAdapter` interface with three core methods:

- `transformConfig(config)` — Convert unified config to bundler-native format
- `validateConfig(config)` — Validate config before build
- `run(config)` — Execute the build

---

## MCP Server

BundleKit provides a **Model Context Protocol (MCP) Server** for AI-driven project scaffolding:

```bash
npx @bundlekit/cli-mcp
```

| Tool | Description |
|------|-------------|
| `create-project` | Create a new project |
| `add-plugin` | Add a framework plugin |
| `list-templates` | List available templates |
| `help` | Get CLI help |

Integrate with any MCP-compatible AI assistant to scaffold projects via natural language.

---

## Project Structure

```
bundlekit/
├── packages/
│   ├── bundlekit-cli/              # CLI scaffolding (@bundlekit/cli)
│   ├── bundlekit-service/          # Core build service (@bundlekit/service)
│   ├── bundlekit-shared-utils/     # Shared utilities (@bundlekit/shared-utils)
│   ├── bundlekit-cli-mcp/          # MCP Server (@bundlekit/cli-mcp)
│   ├── bundlekit-bundler-*/        # Bundler adapters (7 adapters)
│   ├── bundlekit-plugin-*/         # Framework plugins (6 plugins)
│   ├── bundlekit-request/          # HTTP request utility
│   ├── bundlekit-docs/             # Documentation site (dumi)
│   └── bundlekit-docs-agent/       # RAG-based docs Q&A agent
├── __tests__/                      # Tests (unit + integration + E2E)
├── scripts/                        # Build & validation scripts
└── turbo.json                      # Turborepo orchestration
```

## Packages

| Package | npm | Description |
|---------|-----|-------------|
| `bundlekit-cli` | `@bundlekit/cli` | CLI scaffolding tool |
| `bundlekit-service` | `@bundlekit/service` | Core build service |
| `bundlekit-shared-utils` | `@bundlekit/shared-utils` | Shared utilities |
| `bundlekit-cli-mcp` | `@bundlekit/cli-mcp` | MCP Server |
| `bundlekit-bundler-vite` | `@bundlekit/bundler-vite` | Vite adapter |
| `bundlekit-bundler-webpack` | `@bundlekit/bundler-webpack` | Webpack adapter |
| `bundlekit-bundler-rollup` | `@bundlekit/bundler-rollup` | Rollup adapter |
| `bundlekit-bundler-rspack` | `@bundlekit/bundler-rspack` | Rspack adapter |
| `bundlekit-bundler-rolldown` | `@bundlekit/bundler-rolldown` | Rolldown adapter |
| `bundlekit-bundler-parcel` | `@bundlekit/bundler-parcel` | Parcel adapter |
| `bundlekit-bundler-esbuild` | `@bundlekit/bundler-esbuild` | esbuild adapter |
| `bundlekit-plugin-react` | `@bundlekit/plugin-react` | React plugin |
| `bundlekit-plugin-vue` | `@bundlekit/plugin-vue` | Vue 3 plugin |
| `bundlekit-plugin-svelte` | `@bundlekit/plugin-svelte` | Svelte plugin |
| `bundlekit-plugin-angular` | `@bundlekit/plugin-angular` | Angular plugin |
| `bundlekit-plugin-node` | `@bundlekit/plugin-node` | Node.js plugin |
| `bundlekit-plugin-mock` | `@bundlekit/plugin-mock` | Mock API plugin |
| `bundlekit-request` | `@bundlekit/request` | HTTP request utility |

---

## Development

### Prerequisites

- Node.js >= 20
- pnpm >= 8.15.9

### Install

```bash
pnpm install
```

### Build

```bash
# Build all packages
pnpm build:all

# Build individually
pnpm build:shared      # Shared utilities first
pnpm build:vite        # Vite adapter
pnpm build:webpack     # Webpack adapter
# ... other adapters
pnpm build:service     # Core service (depends on adapters)
pnpm build:docs        # Documentation site
```

### Test

```bash
pnpm test              # Unit tests
pnpm test:integration  # Integration tests
pnpm test:e2e          # E2E tests
pnpm test:all          # All tests
```

### Verify

```bash
pnpm verify:pack       # Verify package artifacts
node scripts/validate-templates.mjs  # Validate templates
```

---

## Documentation

- **Online docs**: [bundlekit.harhao.workers.dev](https://bundlekit.harhao.workers.dev)
- **AI docs assistant**: [llm-chat-app.harhao.workers.dev](https://llm-chat-app.harhao.workers.dev) — RAG-based Q&A for BundleKit usage and configuration

```bash
# Run docs locally
cd packages/bundlekit-docs
pnpm dev
```

### Contributing

- [Contributing Overview](packages/bundlekit-docs/docs/contributing/index.md)
- [Setup](packages/bundlekit-docs/docs/contributing/setup.md)
- [Testing](packages/bundlekit-docs/docs/contributing/testing.md)
- [Release](packages/bundlekit-docs/docs/contributing/release.md)
- [Adding a Bundler](packages/bundlekit-docs/docs/contributing/adding-bundler.md)
- [Adding a Plugin](packages/bundlekit-docs/docs/contributing/adding-plugin.md)

---

## CI/CD

Automated via GitHub Actions:

- **publish-npm** — Auto build, test & publish to npm on `master` push (Changesets)
- **create-release-pr** — Auto-create version release PRs
- **deploy-docs** — Auto-deploy documentation site

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Harhao/bundlekit&type=Date)](https://star-history.com/#Harhao/bundlekit&Date)

---

## License

[MIT](./LICENSE) © [harhao](https://github.com/Harhao)
