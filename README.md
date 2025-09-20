# Website Builder Platform

This repository contains a skeleton implementation of a fully‑automated website builder as described in the provided step‑by‑step guide. The intent is to give you a starting point for building your own multi‑service platform for small and medium‑sized businesses. The code is organised as a monorepo with separate folders for the front‑end, API, orchestrator, infrastructure, reusable packages, and templates.

> **Important:** This skeleton does not implement every feature from the guide. It provides the necessary folder structure, configuration files, and basic implementations to help you get started. You will need to expand each section to suit your needs (e.g. implement the chat intake logic, connect to your database, configure LLM interactions, add build pipelines, etc.).

## Repository Layout

```
website_builder/
├── apps
│   ├── api               # Express API gateway & orchestrator
│   ├── web-frontend       # Next.js front‑end and chat interface
│   └── agent-orchestrator # (stub) LLM orchestrator & build pipeline
├── infra
│   └── compose           # Docker Compose configuration for local development
├── packages
│   ├── types             # Shared TypeScript types (SiteSpec, DB DTOs)
│   └── ui                # Shared UI components (empty for now)
├── templates
│   ├── starter-themes     # HTML template fragments for generated sites
│   ├── components         # Reusable partials (navigation, hero, etc.)
│   └── blocks             # Optional content block library
└── prisma
    └── schema.prisma     # Prisma schema for PostgreSQL
```

### apps/web‑frontend

A barebones [Next.js](https://nextjs.org/) application configured with TypeScript and Tailwind CSS. It includes a simple chat component and a form for uploading images. Authentication and advanced chat logic are not yet implemented—use this as a starting point for your own chat intake.

### apps/api

A minimal [Express](https://expressjs.com/) server written in TypeScript. It exposes stub endpoints for authentication, project creation, file upload sign‑in, and generation triggers. It also includes a Prisma client for PostgreSQL and a simple example of how to create tables. You will need to extend this API to perform actual site specification generation, queue build jobs, and update preview containers.

### infra/compose

Docker Compose configuration for local development. It includes Postgres, Redis, MinIO, and Traefik. Read through `docker‑compose.yml` and customise environment variables as needed.

### packages/types

Shared TypeScript types and Zod schemas. The `SiteSpec` interface represents the normalised requirements document generated from the chat questionnaire. The corresponding Zod schema is provided to validate a spec on the server.

### templates

Seed templates for generating static websites. Each page in a site will be rendered using Nunjucks (or another templating system of your choice) based on these building blocks. There is a basic layout defined in `starter‑themes/base.html` and individual components under `components/`.

## Getting Started

1. **Install dependencies.** You will need [Node.js](https://nodejs.org/en/) v18 or later and [pnpm](https://pnpm.io/) installed locally.
   ```bash
   cd website_builder
   pnpm install
   ```
2. **Start the infrastructure.** Use docker‑compose to run Postgres, Redis, MinIO, and Traefik locally.
   ```bash
   cd infra/compose
   docker compose up -d
   ```
3. **Run the API server.** In a new terminal, run:
   ```bash
   cd apps/api
   pnpm dev
   ```
4. **Run the front‑end.** In another terminal, run:
   ```bash
   cd apps/web-frontend
   pnpm dev
   ```
5. Open your browser to `https://localhost` (via Traefik) or the specific port printed in the logs. You should see a simple home page with a chat component stub.

## Next Steps

This skeleton leaves many features unimplemented. Some suggested next tasks:

* Implement a robust chat questionnaire that collects and validates user requirements. Use a conversational UI and allow image uploads.
* Convert chat answers into a `SiteSpec` object. Add AI interactions via OpenAI or similar to help generate content and design decisions.
* Build a queue and job processor to take a `SiteSpec` and render static HTML, CSS, JS, and PHP files from templates. Use Nunjucks or another templating engine.
* Create preview containers on demand. Use Docker or ECS/Fargate to spin up isolated containers with the generated site, and expose them through Traefik.
* Implement change requests via chat by diffing `SiteSpec` versions and updating only the necessary parts of the site.
* Add subscription management and hosting infrastructure. Integrate with a payment provider such as Stripe.

Feel free to adapt and expand this skeleton to suit your own architecture and preferences.
