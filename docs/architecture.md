# Architecture

The project follows **Scream Architecture**: the codebase is organized in concentric layers where each inner layer is independent of the outer ones. Dependencies point inward (presentation → application → domain), never outward.

## Layers (in this Astro + React project)

| Layer | Location | Responsibility | May depend on |
| --- | --- | --- | --- |
| Domain | `src/lib/` (pure logic), `src/hooks/` | Core business rules and pure utilities. No framework imports, no I/O. | Nothing |
| Application | `src/components/` (feature components), form/state logic | Orchestrates use cases: forms, validation (Zod), state. | Domain |
| Presentation | `src/pages/`, `src/layouts/`, `src/components/ui/` | Rendering: Astro islands, layouts, shadcn/ui primitives. | Application, Domain |
| Infrastructure / Adapters | `node_modules` (Astro, React, Radix, Tailwind) | Framework and platform capabilities. | — |

## Rules

- Inner layers never import from outer layers.
- UI primitives in `src/components/ui/` stay generic; feature-specific composition lives in `src/components/`.
- Pure, testable logic belongs in `src/lib/`; keep it free of React/Astro imports.
- Styling is utility-first (Tailwind v4) via `src/styles/global.css`.
