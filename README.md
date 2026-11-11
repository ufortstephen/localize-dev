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

## Hosted server (Lovable, Bolt, v0 and other browser-based builders)

Agents that can't start a local process can connect to `https://uselocalize.dev/api/mcp` as a
Streamable HTTP server. Clients that support OAuth sign in and you approve the connection on
localize.dev; otherwise send a localize.dev API key as the bearer token (or in an `x-api-key` header):

```json
{
  "mcpServers": {
    "localize": {
      "type": "http",
      "url": "https://uselocalize.dev/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

It has the same tools. Each call carries the files the tool needs (`files: [{ path, content }]`)
and returns the files to write back. Every call needs a key or an OAuth sign-in; `check` and
`config` still don't count toward your plan.

## API key

There's a free tier. Create a key at
[uselocalize.dev/dashboard/keys](https://uselocalize.dev/dashboard/keys), then run
`npx localize-dev login` once, or set `LOCALIZE_API_KEY`.

## Data

`check` and `config` run on your machine and send nothing. `localize`, `translate` and
`add_language` send the localize.dev API only the UI text being translated, the path of the file
it came from and short component context, never whole files. The API translates them with a
hosted model (Google Gemini, with OpenAI and Mistral as backups, through Vercel AI Gateway), which
doesn't train on them. localize.dev doesn't store the source text, only a one-way fingerprint of
it, and keeps the returned translations in your workspace's translation memory until you delete
the workspace. The hosted server writes the files an agent sends to a temporary folder for that
call and deletes them when it answers; they aren't stored or logged. Details: [uselocalize.dev/privacy](https://uselocalize.dev/privacy).

## Set up a project

```bash
npx localize-dev init --agent    # MCP config, project instructions and an after-edit hook, committed for the team
npx localize-dev run -l es,fr,de # or localize the whole app from the terminal
npx localize-dev check           # in CI: fails on hardcoded copy and missing translations
```

The GitHub Action is [ufortstephen/autolocalize-action](https://github.com/ufortstephen/autolocalize-action).

## License

The files in this repository (plugin manifests, skill, rule and MCP configuration) are MIT
licensed: see [legal/plugin-files-license.txt](legal/plugin-files-license.txt). The `localize-dev`
package they run isn't open source; its license is included in the package.
