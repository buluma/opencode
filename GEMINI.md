# OpenCode Project Context

## Project Overview

**OpenCode** is an open-source AI coding agent designed to be provider-agnostic (works with Claude, OpenAI, Google, local models, etc.). It features a client/server architecture and a Terminal User Interface (TUI) built with modern web technologies.

- **Architecture:** Client/Server. The core runs as a server, which can be driven by various clients (TUI, Mobile, etc.).
- **Tech Stack:**
    - **Runtime:** [Bun](https://bun.sh) (Required v1.3+)
    - **Language:** TypeScript
    - **Monorepo:** [Turborepo](https://turbo.build/)
    - **TUI:** [SolidJS](https://www.solidjs.com/) with [opentui](https://github.com/sst/opentui)
    - **Web UI:** SolidJS
    - **Desktop:** [Tauri](https://tauri.app) (wrapping the web UI)
    - **Infrastructure:** [SST](https://sst.dev)

## Key Directories

- `packages/opencode`: **Core** business logic and server.
    - `src/cli/cmd/tui/`: TUI code (SolidJS + opentui).
    - `src/server/`: Server implementation.
- `packages/app`: Shared web UI components (SolidJS).
- `packages/desktop`: Native desktop application (Tauri).
- `packages/plugin`: Source for `@opencode-ai/plugin`.
- `infra/`: Infrastructure definitions (SST).

## Building and Running

**Prerequisites:** Bun v1.3+

### Development

- **Install Dependencies:**
  ```bash
  bun install
  ```

- **Run Core (TUI/Server):**
  ```bash
  bun dev
  ```
  *Runs `packages/opencode`. To run against a specific directory, use `bun dev <directory>`.*

- **Run Web App (UI only):**
  ```bash
  bun run --cwd packages/app dev
  ```
  *Starts local dev server at http://localhost:5173.*

- **Run Desktop App:**
  ```bash
  bun run --cwd packages/desktop tauri dev
  ```
  *Requires Rust/Tauri prerequisites.*

### Building

- **Compile Standalone Executable ("localcode"):**
  ```bash
  ./packages/opencode/script/build.ts --single
  # Run the binary:
  ./packages/opencode/dist/opencode-<platform>/bin/opencode
  ```

- **Regenerate SDK:**
  If API/Server changes are made, regenerate the SDK:
  ```bash
  ./script/generate.ts
  ```

- **Typecheck:**
  ```bash
  bun turbo typecheck
  ```

## Development Conventions

### Coding Style
*   **Runtime:** Use Bun APIs (e.g., `Bun.file()`) whenever possible.
*   **Variables:**
    *   Avoid `let`. Use `const`.
    *   Prefer single-word naming (e.g., `const foo` vs `const fooBar`).
    *   Avoid unnecessary destructuring (`obj.a` > `const { a } = obj`).
*   **Control Flow:**
    *   Avoid `else` statements. Use early returns or IIFEs.
    *   Keep logic in a single function unless reusability demands splitting.
*   **Error Handling:**
    *   Avoid `try/catch` blocks. Prefer `.catch(...)` on promises.
    *   Avoid `any` type.

### Contributing
*   **Issue First:** All PRs must reference an existing issue (`Fixes #123`).
*   **Commit Messages:** Follow [Conventional Commits](https://www.conventionalcommits.org/) (e.g., `feat:`, `fix:`, `docs:`).
    *   Optional scope: `feat(app):`, `fix(desktop):`.
*   **UI Changes:** Must include screenshots/videos in the PR.
*   **Logic Changes:** Must explain verification steps (what was tested, how to reproduce).
*   **Descriptions:** Keep PR descriptions and issues concise. Avoid long AI-generated text.

### Debugging
*   **Inspect:** Run with `bun run --inspect=<url> dev ...` and attach a debugger.
*   **Caveat:** `bun dev` runs the server in a worker. If breakpoints fail, try `bun dev spawn` or run server and TUI separately (see `CONTRIBUTING.md` for details).
