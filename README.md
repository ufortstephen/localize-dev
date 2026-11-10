# localize.dev

Ship your app in every language. localize.dev finds hardcoded UI text in your code, gives it
translation keys, translates it with your app's context, writes the locale files and opens one
pull request. It works with React, Next.js, React Native, Vue, Svelte, Astro, Angular, Flutter,
iOS, Android, Rails, Django and Laravel.

```tsx
// before                                   // after
<h1>Welcome back</h1>                       <h1>{t('auth.welcome_heading')}</h1>
<p>Hello {user.name}!</p>                   <p>{t('auth.greeting', { name: user.name })}</p>
```

This repository holds the agent plugins and MCP listing for the
[`localize-dev`](https://www.npmjs.com/package/localize-dev) package. Docs, pricing and API keys
are at [uselocalize.dev](https://uselocalize.dev). Report problems in
[Issues](https://github.com/ufortstephen/localize-dev/issues).

## Claude Code

```bash
claude plugin marketplace add ufortstephen/localize-dev
claude plugin install localize-dev@uselocalize
```

The plugin adds the `localize` MCP server and a skill that tells Claude when to use it.

## Cursor

Install **localize.dev** from the plugin list, or add the server to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "localize": { "command": "npx", "args": ["-y", "localize-dev", "mcp"] }
  }
}
```

## Other MCP clients (Codex, Windsurf, Cline, VS Code)

Run `npx -y localize-dev mcp` as a stdio server. It's listed in the
[MCP Registry](https://registry.modelcontextprotocol.io) as `io.github.ufortstephen/localize-dev`.

| Tool | What it does | Needs a key |
| --- | --- | --- |
| `check` | Hardcoded copy, missing and outdated translations, each with its file and line | No |
| `config` | The app's languages, locale files and i18n library | No |
| `localize` | Gives hardcoded text keys, rewrites the code and translates it | Yes |
| `translate` | Fills in missing translations in existing locale files | Yes |
| `add_language` | Adds a language and translates every message into it | Yes |

## API key

There's a free tier. Create a key at
[uselocalize.dev/dashboard/keys](https://uselocalize.dev/dashboard/keys), then run
`npx localize-dev login` once, or set `LOCALIZE_API_KEY`.

## Set up a project

```bash
npx localize-dev init --agent    # MCP config, project instructions and an after-edit hook, committed for the team
npx localize-dev run -l es,fr,de # or localize the whole app from the terminal
npx localize-dev check           # in CI: fails on hardcoded copy and missing translations
```

The GitHub Action is [ufortstephen/autolocalize-action](https://github.com/ufortstephen/autolocalize-action).

## License

The files in this repository (plugin manifests, skill, rule and MCP configuration) are MIT
licensed. The `localize-dev` package they run has its own license, included in the package.
