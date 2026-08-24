# AeroBeat Web Standard Package Template

Use this template for AeroBeat-authored `aerobeat-web-*` library, singleton, package, and domain repos.

## Responsibility

A standard package owns one bounded domain, its public exports, repo-local tests, demos, browser scenes, fixtures, assets, decisions, and validation. It does not own product assembly, public docs publishing, or hidden imports into neighboring repos.

Create packages as private npm packages named `@aerobeat/web-<domain>` with version `0.0.0`.

## Folder Shape

```text
/
  README.md
  LICENSE.md
  .gitignore
  package.json
  package-lock.json
  src/
    index.js
  .testbed/
    README.md
    package.json
    package-lock.json
    test/
      setup/
    demo/
      index.html
      main.js
    scenes/
    debug-data/
    playwright.config.js
    node_modules/
      @aerobeat/
        web-this-repo -> ../../../src
  scripts/
  fixtures/
  assets/
  docs/
    decisions/
  .github/
    workflows/
  .plans/
  .beads/
```

Generated `.testbed/node_modules` symlinks are local state and must not be committed. Commit scripts and docs that recreate them.

## Source Boundary

Runtime code lives under `src/` and is exposed through `package.json` `exports`. Tests, demos, scenes, debug data, screenshots, traces, and Playwright harnesses live under `.testbed/`.

Testbed code must import the package through the local `@aerobeat/web-*` symlink, not through relative imports into `../src`.

## Public Imports

Repos may import only declared public exports from other `@aerobeat/web-*` packages. Do not import sibling repo internals, private testbed files, unexported source paths, or vendor-native shapes across domain boundaries.

## JavaScript Posture

- Use JavaScript, native ES modules, `// @ts-check`, and JSDoc.
- Every exported value, public structure, service shape, event payload, and typedef needs JSDoc.
- Do not use `any`, star-shaped JSDoc escapes, or undocumented escape hatches.
- Unknown external values must be narrowed into documented shapes before use.

## Web Component Rules

When a repo owns UI, every visible primitive, control, widget, HUD element, panel, modal, overlay, and screen must be a named `aero-*` Web Component.

Screens and testbed scenes may own layout and composition, but they may not define one-off visible UI. Every component needs a standalone `.testbed/scenes/*.scene.html` file and representative `.testbed/debug-data/*.debug-data.js` states.

## Validation

Run these commands before handoff:

```bash
npm run check
npm test
npm run test:browser
```

This template includes no-dependency placeholder validators for strict JSDoc/no-escape checks, public import boundaries, component-only screen/scene composition, and Playwright console-warning/error posture. Replace them with stronger repo-specific checks as the repo gains implementation.

When a browser-visible package needs mobile or remote validation, add `npm run testbed:serve`. It must state the host, port, cache-busting/version display, QR/link helper, served roots, and HTTPS or secure-context path for Tailscale devices.

## Docs Handoff

Keep repo-local implementation notes and accepted decisions under `docs/`. Public contributor/user docs belong in `aerobeat-web-docs`; mirror cross-repo decisions there after they are accepted.
