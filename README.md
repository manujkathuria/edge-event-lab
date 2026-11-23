# Event Lab

A hands-on TypeScript playground for learning backend event processing and the concepts behind Cloudflare Zaraz and Managed Components.

## Current status

The project currently contains only the TypeScript setup and a minimal entry point. The provisional reader and event model have been removed so we can agree on requirements before designing and implementing the CSV reader. CSV ingestion, HTTP services, Managed Components, and Cloudflare deployment are not implemented yet.

Our reusable engineering standard is in [AGENTS.md](AGENTS.md). Use the [task brief template](docs/templates/task-brief.md) to prepare meaningful work before implementation.

The intended learning flow is:

```text
CSV events → Reader → Event processor → Component → Mock destination
```

This is a learning architecture, not a reproduction of Zaraz's private production architecture.

## Prerequisites

- Node.js and npm installed and available in your terminal.
- GoLand or another editor.

The initial setup was verified with Node.js `v26.7.0` and npm `11.19.0`.

Check your installation:

```bash
node --version
npm --version
```

## Getting started

Open this folder in GoLand and use its terminal. Install the locked dependencies:

```bash
npm ci
```

Run the entry point:

```bash
npm run dev
```

Expected output:

```text
Event lab is running!
```

Edit `src/index.ts`, then run the command again to see your changes. The development command runs once; it does not watch for changes.

## Commands

| Command | Purpose |
| --- | --- |
| `npm run dev` | Run `src/index.ts` using tsx. |
| `npm run typecheck` | Check TypeScript types without generating JavaScript. |
| `npm run build` | Compile TypeScript from `src/` into JavaScript in `dist/`. |
| `npm start` | Run the compiled `dist/index.js`; build first. |

`tsx` executes TypeScript but does not type-check it. Run `npm run typecheck` separately.

To check, compile, and run:

```bash
npm run typecheck
npm run build
npm start
```

## Project structure

```text
.
├── src/
│   └── index.ts       # Minimal entry point
├── package.json       # Project metadata, commands, and dependencies
├── package-lock.json  # Locked dependency versions
├── tsconfig.json      # Type checking and compilation settings
├── .gitignore         # Excludes generated files and local settings
└── README.md
```

`node_modules/` contains installed dependencies. `dist/` contains compiled output. Both are generated and excluded from version control.

## How the setup works

- **Node.js** runs our local application and provides APIs such as filesystem access.
- **npm** installs dependencies and runs the commands defined in `package.json`.
- **TypeScript** checks types and compiles `.ts` files to JavaScript.
- **tsx** runs TypeScript conveniently during development.
- **@types/node** supplies TypeScript definitions for Node.js APIs.
- **GoLand** is the editor; this project does not require a Go module.

The project uses ES modules through `"type": "module"`. TypeScript's `NodeNext` settings align module handling with Node.js. Strict checking is enabled, and `noUncheckedIndexedAccess` makes unchecked indexed access account for potentially missing values.

TypeScript types do not validate CSV or HTTP input at runtime. We will add explicit input validation when ingestion is implemented.

## Recreating the initial setup

In an empty project folder:

```bash
npm init -y
npm install --save-dev typescript tsx @types/node
```

Then create `src/index.ts` and `tsconfig.json`, and configure the scripts and ES module setting in `package.json` as shown in this project. The install commands alone do not create those files or scripts.

## Planned learning steps

1. Read synthetic events from a CSV file.
2. Validate records and explore arrays and maps.
3. Send events over HTTP to a processor and mock destination.
4. Explore configuration, field filtering, and Managed Component interfaces.
5. Introduce slow responses, failures, duplicates, and recovery.
6. Explore running the processor on Cloudflare Workers while keeping the CSV reader local.

Each step will define its behavior and guarantees before implementation.
