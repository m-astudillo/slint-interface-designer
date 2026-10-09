# Tech Stack

> Keep this file in sync whenever a dependency is added, updated, or removed.

## Core

| Technology | Version | Purpose |
| --- | --- | --- |
| Node.js | >=22.12.0 | Runtime |
| pnpm | latest | Package manager |
| Astro | ^7.3.8 | Meta-framework / SSG |
| React | ^19.3.0 | UI library (islands) |
| TypeScript | (via Astro) | Language |
| Tailwind CSS | ^4.3.3 | Styling (via `@tailwindcss/vite`) |

## UI

| Package | Version | Purpose |
| --- | --- | --- |
| `@base-ui/react` | ^1.8.0 | Headless UI primitives |
| `radix-ui` + `@radix-ui/*` | ^1.7.0 / ^1.x–^2.x | Headless accessible components (dialog, select, tooltip, tabs, …) |
| `lucide-react` | ^1.52.0 | Icons |
| `next-themes` | ^0.4.6 | Dark mode |
| `vaul` | ^1.1.2 | Drawer |
| `embla-carousel-react` | ^8.6.0 | Carousel |
| `react-resizable-panels` | ^4.14.3 | Resizable panels |
| `recharts` | 3.8.0 | Charts |
| `cmdk` | ^1.1.1 | Command palette |
| `sonner` | ^2.0.8 | Toasts |

## Forms & Data

| Package | Version | Purpose |
| --- | --- | --- |
| `react-hook-form` | ^7.89.0 | Forms |
| `@hookform/resolvers` | ^5.9.1 | Resolvers (Zod) |
| `zod` | ^4.6.5 | Validation schemas |
| `date-fns` | ^4.4.0 | Date utilities |
| `react-day-picker` | ^10.0.2 | Calendar picker |
| `input-otp` | ^1.5.0 | OTP input |

## Utilities

| Package | Version | Purpose |
| --- | --- | --- |
| `clsx` | ^2.1.1 | Conditional class names |
| `tailwind-merge` | ^3.7.0 | Tailwind class merging |
| `class-variance-authority` | ^0.7.1 | Component variants |
| `cn` | ^0.4.0 | `cn` helper |

## Dev

| Package | Version | Purpose |
| --- | --- | --- |
| `@tailwindcss/postcss` | ^4.3.3 | Tailwind PostCSS plugin |
| `postcss` | ^8.5.29 | CSS tooling |
| `@astrojs/react` | ^7.0.1 | React integration for Astro |
| `@types/react` / `@types/react-dom` | ^19.3.0 | React type definitions |
