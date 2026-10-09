# React Doctor — Execution, Analysis & Fix Manual

> Tool: [`react-doctor`](https://github.com/millionco/react-doctor) (always `@latest`)
> Scope: deterministic static scan + optional live performance scan of running React apps.
> Output: a **0–100 health score** plus categorized, severity-tagged findings with actionable recommendations.
>
> React Doctor scans for **security, performance, correctness, state/effect, architecture, and accessibility** issues that coding agents and lint rules typically miss.

---

## 1. When to run

| Moment | Command |
| --- | --- |
| Before starting work on a component/feature | Full scan (step 3.1) |
| After finishing a change, before committing | Regression check (step 3.3) |
| When a rule fires and you don't understand it | `rules explain` (step 4.3) |
| To profile a slow interaction in the browser | Live scan (step 3.5) |
| In CI / for agent consumption | JSON scan (step 3.2) |

---

## 2. Prerequisites

- Node.js >= 22.12.0 (project engine requirement).
- No local install needed — the CLI runs via `npx`, which always resolves `@latest`.
- The project already ships a shortcut:

  ```bash
  pnpm doctor    # === npx react-doctor@latest
  ```

- Telemetry (crash traces + anonymous usage, no code) is on by default. Disable it with `--no-telemetry` if required.

---

## 3. Execution flow

### 3.1 Full scan (baseline)

```bash
npx react-doctor@latest
```

- Scans the whole codebase, prints findings grouped by category with severities (`error` / `warn`), and ends with a health score out of 100.
- Add `--verbose` for detailed per-rule output.

```bash
npx react-doctor@latest --verbose
```

### 3.2 JSON report (CI / agents / ground truth)

```bash
npx react-doctor@latest --json --json-out react-doctor-report.json
```

- Read-only; writes structured findings to a file.
- Treat the JSON as the **ground truth** for technical issues when triaging.
- Keep the file outside version control (e.g. in `.gitignore` or a temp path) and delete it when done.

### 3.3 Regression check (after a change, before commit)

```bash
npx react-doctor@latest --verbose --scope changed
```

- Limits the scan to issues **introduced versus the base branch**.
- **Gate rule:** if the score drops or new `error`-level findings appear, fix them before committing.

### 3.4 Scoped scan (specific files)

```bash
npx react-doctor@latest src/components/MyFeature.tsx src/hooks/useThing.ts
```

Useful to triage only the files a CI step or review flagged.

### 3.5 Live scan of a running app (performance trace)

```bash
# dev server running (pnpm dev)
npx react-doctor@latest scan http://localhost:3000
```

- Opens system Chrome in an **isolated temporary profile**.
- Interact with the app; rendered components are highlighted with purple outlines + names.
- Recording stops on **Enter** or after **5 minutes**.
- Returns a readable summary + path to a compressed DevTools trace.
- For agents/CI the URL **must** be explicit (no interactive prompt):

  ```bash
  npx react-doctor@latest scan http://localhost:3000 --format json
  ```

#### Authenticated flows (CDP)

```bash
npx react-doctor@latest scan https://app.example.com \
  --cdp http://127.0.0.1:9222 \
  --format json
```

- Connect through Chrome DevTools Protocol.
- Use a **dedicated debug profile**; sign in; close every non-blank tab before starting.
- Never request cookies or copy a browser profile.

---

## 4. Analysis

### 4.1 Reading the output

Each run reports:

1. **Health score (0–100)** — overall signal; track its trend across revisions.
2. **Open findings by category** — Performance, Security, State & Effects, Architecture, Accessibility, …
3. **Severity** — `error` (fix now) > `warn` (fix soon) > `off` (disabled).
4. **Regressions** — findings that increased vs. the previous revision (flagged explicitly).

Triage order: **new regressions → errors → warns → score trend**.

### 4.2 Rule identifiers

Rules are namespaced, e.g.:

```
react-doctor/no-array-index-as-key
react-doctor/effect-needs-cleanup
```

Every finding carries its rule id — that id is the key for every command below.

### 4.3 Explain a rule (why + how to fix)

```bash
npx react-doctor@latest rules explain react-doctor/no-array-index-as-key
```

Also fetch the canonical fix recipe (authoritative, reviewer-tested):

```
https://www.react.doctor/prompts/rules/<plugin>/<rule>.md
```

> Do **not** approximate fixes from memory — the recipe is the source of truth for Target and Steps.

### 4.4 Inspect the rule set

```bash
npx react-doctor@latest rules list                          # every rule + effective severity
npx react-doctor@latest rules list --configured             # only what your config changed
npx react-doctor@latest rules list --category Performance   # filter by category
```

---

## 5. Correction flow

For **each finding**, in triage order:

1. **Understand** — `rules explain <rule>` + fetch the canonical recipe (4.3).
2. **Verify it's real** — read the file at `path:line`. A finding you cannot confirm in the code is either a false positive or a config issue; do not blindly "fix" it.
3. **Apply the minimal fix** — implement exactly what the recommendation says. No drive-by refactors, no new abstractions. Keep the diff focused on the finding.
4. **Handle legitimate exceptions** — if the pattern is intentionally correct in this context, prefer a **scoped config change** over editing the code (step 6). Never disable a rule globally to silence one finding.
5. **Re-run the regression check:**

   ```bash
   npx react-doctor@latest --verbose --scope changed
   ```

   The finding must be gone and the score must not have dropped.
6. **Verify behavior** — run the app (`pnpm dev`), exercise the touched component, and for performance findings optionally re-run the live scan (3.5) to confirm the improvement.

---

## 6. Configuration (`doctor.config.ts`)

Create/extend `doctor.config.ts` at the project root for project-wide policy:

```ts
// doctor.config.ts
export default {
  rules: { "react-doctor/no-array-index-as-key": "off" },        // per-rule severity: off | warn | error
  categories: { "React Native": "warn" },                         // per-category severity
  ignore: { tags: ["design"] },                                   // skip a rule family (design, test-noise, …)
};
```

Or manage rules non-interactively:

```bash
npx react-doctor@latest rules set react-doctor/no-array-index-as-key warn   # off | warn | error
npx react-doctor@latest rules disable react-doctor/<rule>                   # rule never runs
npx react-doctor@latest rules enable react-doctor/<rule>                    # back to recommended severity
npx react-doctor@latest rules category "React Native" off                   # whole category
npx react-doctor@latest rules ignore-tag design                             # skip a rule family
npx react-doctor@latest rules unignore-tag design
```

After any config change, validate:

```bash
npx react-doctor@latest --verbose --diff
```

Commit the resulting `doctor.config.ts` with the code that justified it.

---

## 7. Recommended workflow (end to end)

```
1. pnpm dev                          # app up (for live scans / manual verification)
2. npx react-doctor@latest           # baseline score + findings
3. triage: regressions → errors → warns
4. for each finding:
     rules explain <rule>  →  read recipe  →  minimal fix  →  config if intentional
5. npx react-doctor@latest --scope changed    # must be clean, score not lower
6. verify in the running app
7. commit
```

---

## 8. Troubleshooting

| Symptom | Check |
| --- | --- |
| Scan hangs waiting for input | Live scan without a URL — pass the URL explicitly (`scan http://localhost:3000 --format json`). |
| Authenticated flow not captured | Use `--cdp` with a dedicated debug profile; close non-blank tabs first. |
| A rule fires on code that is intentionally written that way | Explain it (`rules explain`), then scope an exception in `doctor.config.ts` — never edit the code to appease a rule you disagree with. |
| Score dropped after a change | `--scope changed` lists exactly what regressed; fix or revert before commit. |
| Need machine-readable output | `--json --json-out <file>` (3.2). |
| Privacy concerns | `--no-telemetry`. |
