[![05 - CI](https://github.com/ortega-alex/github-actions/actions/workflows/05-ci.yml/badge.svg)](https://github.com/ortega-alex/github-actions/actions/workflows/05-ci.yml)

# CI/CD Monorepo

Monorepo con **Turborepo** + **pnpm** que sirve como playground para explorar pipelines de **GitHub Actions**.

## Stack

| App | Stack | Test | Lint |
|-----|-------|------|------|
| `apps/api` | NestJS 11 + Express | Jest | ESLint 9 + Prettier |
| `apps/web` | React 19 + Vite 8 | Vitest + Testing Library | ESLint 10 + Prettier |

**Monorepo**: pnpm 11.2.2 · Turborepo 2.9.14 · TypeScript

## Comandos

```bash
pnpm install          # instalar dependencias
pnpm dev              # turbo run dev
pnpm build            # turbo run build
pnpm lint             # turbo run lint (todos los apps)
pnpm test             # turbo run test (todos los apps)
```

## Estructura

```
├── apps/
│   ├── api/          # NestJS REST API
│   └── web/          # React SPA (Vite)
├── packages/         # shared packages (placeholder)
├── tools/            # herramientas internas (placeholder)
├── .github/
│   └── workflows/
│       ├── 01-hello.yml        # Hello World manual
│       ├── 02-manual.yml       # Deploy manual con inputs
│       ├── 03-schedule.yml     # Schedule semanal
│       ├── 04-events.yml       # Event-driven (issues)
│       └── 05-ci.yml           # CI pipeline principal
├── turbo.json        # configuración de Turborepo
└── pnpm-workspace.yaml
```

## CI/CD Pipeline (`05-ci.yml`)

Se ejecuta en **push** o **PR** a `main`:

1. Checkout → Setup pnpm → Setup Node 22 (con cache)
2. `pnpm install --frozen-lockfile`
3. `pnpm lint`
4. `pnpm test`
5. `pnpm build`

## Notas

- El workspace apunta a `apps/*` y `packages/*`
- Builds de `esbuild` están aprobados en `pnpm-workspace.yaml`
- Los PRs corren lint + test + build antes de mergear
