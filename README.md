# Charmshots MCP

**AI portraits for dating profiles and everyday introductions.**

Charmshots creates portraits from your own selfies, with settings and styles for dating profiles, headshots and everyday introductions. Choose scenes that fit your life, review each result closely, and keep the frames that feel like you. The photo collection, practical guides and free image tools help you plan a more considered profile.

[Website](https://charmshots.com) · [MCP repository](https://github.com/charmshots/mcp-server) · [Agent skill](https://github.com/charmshots/agent-skill) · [npm package](https://www.npmjs.com/package/charmshots-mcp)

## What this connector does

This package connects a local stdio MCP client to the hosted [Charmshots MCP server](https://mcp.charmshots.com/mcp). Hosted tools run on Cloudflare; the local package bridges the connection and opens browser-based OAuth. You do not need to deploy a Worker or paste a product password into your assistant.

| Tool | What it does |
| --- | --- |
| `get_profile` | Read the signed-in account context. |
| `list_photoshoots` | Browse existing owned photoshoots, newest first. |
| `get_photoshoot` | Read the selected shoot’s metadata and status. |
| `list_photos` | List existing photo labels, styles, statuses and owner-authenticated download links. |

The MCP does not generate or analyze images and does not spend credits. Photo metadata is not a visual quality assessment, and download links require the owner’s product sign-in. The website currently says purchases are temporarily paused; browse the current collection and availability before planning a new shoot. AI portraits cannot guarantee dating matches.

## Example workflow

1. Find the requested photoshoot in the signed-in account.
2. List its available photos, distinguishing ready results from queued or incomplete work.
3. Return the relevant product or download links so the owner can inspect the actual images.

### Things to ask your assistant

> Find my most recent photoshoot and list the photos that are ready to download.

> Show the labels and styles in this shoot, with the available download links.

> Summarize the status of my saved photoshoots without generating new images.

## Connect a remote MCP client

1. Open the client’s custom MCP or connector settings.
2. Enter `https://mcp.charmshots.com/mcp` as the remote server URL.
3. Complete Charmshots sign-in in your browser and review the permissions on the consent screen.
4. Return to the client and load the available tools.

Use a client that supports Streamable HTTP MCP and OAuth. Custom-connector availability depends on the client and your account. A public repository or npm release does not mean the integration has been approved for a client’s marketplace.

## Claude Desktop and other stdio clients

Requires **Node.js 22 or newer** and an existing Charmshots account. Add this entry to your client’s MCP configuration:

```json
{
  "mcpServers": {
    "charmshots": {
      "command": "npx",
      "args": [
        "-y",
        "charmshots-mcp"
      ]
    }
  }
}
```

You can also run `npx -y charmshots-mcp` from a terminal to start the bridge. It speaks MCP over stdio; it is not an interactive chat interface. For desktop clients that support MCPB extensions, download the `.mcpb` file from [Charmshots releases](https://github.com/charmshots/mcp-server/releases).

## Permissions and account access

Requested scopes: `profile:read photos:read`. Older profile-only connections need to reconnect and explicitly approve the additional permissions before content tools are available.

Only approve a connection you intended to start. If sign-in opens a new tab, finish it, return to the consent screen and refresh. [Manage or revoke connected apps](https://charmshots.com/oauth/mcp/connections).

The package uses pinned [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) for OAuth, PKCE and local token storage. Tokens on your computer are credentials. The connector uses the fixed endpoint above and rejects command-line endpoint overrides. Product passwords and underlying provider credentials are not requested by this package.

## Troubleshooting

- **No tools or insufficient permissions:** reconnect through browser consent and check the selected account.
- **An empty list:** confirm that the account owns the expected items. Empty results are different from a failed request.
- **A link asks you to sign in:** open it with the owning product account; a private product link is not a public share link.
- **The browser blocks authorization:** inspect the browser’s displayed error and restart an expired request from the client. Never send cookies or tokens in an issue.

## Add the companion skill

The [Charmshots agent skill](https://github.com/charmshots/agent-skill) explains how to select the right records, interpret results and respect the workflow’s limits:

```sh
npx skills add charmshots/agent-skill
```

## Learn more about Charmshots

- [AI dating portraits](https://charmshots.com/)
- [Photo style collection](https://charmshots.com/styles/)
- [Headshots](https://charmshots.com/headshots/)
- [Build a dating photo lineup](https://charmshots.com/guides/build-a-dating-photo-lineup/)
- [Using AI photos honestly](https://charmshots.com/guides/using-ai-photos-honestly/)
- [Free photo editor](https://charmshots.com/editor/)
- [Image converter](https://charmshots.com/converter/)

## Development and support

```sh
npm ci
npm test
npm run bundle
```

[Report a connector issue](https://github.com/charmshots/mcp-server/issues) with your client, Node.js version and a redacted error. Keep `package.json`, `manifest.json`, `server/config.json`, `server.json` and the lockfile version aligned for releases. GitHub Actions publishes versioned npm packages and MCPB assets. See [LICENSE](https://github.com/charmshots/mcp-server/blob/main/LICENSE) for the MIT license.
