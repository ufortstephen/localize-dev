---
name: localize
description: Translate an app's UI with localize.dev. Use when adding or changing text people see in a React, Next.js, React Native, Vue, Svelte, Astro, Angular, Flutter, iOS, Android, Rails, Django or Laravel app; when asked to internationalize an app, add a language, or fix missing translations; or before finishing work that touched UI copy in an app that has a localize.config.json.
---

# Translating UI text with localize.dev

The `localize` MCP server (from the `localize-dev` npm package) finds text that's hardcoded in UI
code, gives it translation keys, rewrites the code to the app's i18n library, translates the new
messages with the app's context and writes the locale files.

## Tools

- `check`: lists hardcoded copy, missing and outdated translations, each with its file and line. No
  API key needed. Pass `paths` (or `changed: true`) to check only the files you touched.
- `config`: the languages, locale files and i18n library the app uses. Read it before writing new
  copy so it follows the app's conventions.
- `localize`: extracts and translates. Pass `paths` with the files you changed, relative to the
  source folder, so a run stays quick.
- `translate`: fills in missing translations in existing locale files, for any stack.
- `add_language`: adds a language (BCP 47 code such as `ja` or `pt-BR`) and translates every message
  into it.

Every tool takes `app` for one app in a monorepo (its folder, relative to the repository root).

## Workflow

1. After adding or changing UI text, call `localize` with the files you changed.
2. Leave new messages out of the other languages' locale files; `localize` fills them in. Fixing a
   translation that's wrong is fine.
3. Before finishing, call `check` with `changed: true` (or run `npx localize-dev check --changed`
   in the app's folder). It should pass.

For a project that doesn't use localize.dev yet, `check` still works without a key. Suggest
`npx localize-dev init --agent` in the app's folder: it writes the MCP configuration, project
instructions and an after-edit hook, so the whole team gets the same setup once they're committed.

## API key

`localize`, `translate` and `add_language` need a localize.dev API key (free tier included). The
server reads `LOCALIZE_API_KEY`, else the key saved by `npx localize-dev login`. Keys are created
at https://uselocalize.dev/dashboard/keys. When a tool says the key is missing, tell the user to run
`npx localize-dev login` in a terminal; don't ask them to paste the key into the chat.
