# Charmshots MCP

Create AI portraits from your own photographs.

get_profile reads only your own account email, available credits and onboarding status. It does not expose photos, body measurements or orders, generate images, or spend credits.

## Remote MCP

Use **https://mcp.charmshots.com/mcp** in a client that supports remote MCP with OAuth. Sign in to Charmshots and explicitly approve the connection.

## Claude Desktop and other stdio clients

Requires Node.js 22 or newer. Add this configuration:

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

Or install the `.mcpb` file from [Releases](https://github.com/charmshots/mcp-server/releases) in your desktop client's Extensions settings. The connector opens your browser for sign-in. If the consent page asks you to sign in, use its new-tab link, then return and refresh the consent page.

## Privacy and permissions

Your product password and provider credentials are not requested by this package. The pinned [mcp-remote](https://www.npmjs.com/package/mcp-remote) bridge handles OAuth, PKCE and local token storage. It connects only to the fixed endpoint above; command-line endpoint overrides are not supported. OAuth tokens are stored locally by mcp-remote and should be treated as credentials.

Revoke a connection at [Charmshots MCP connections](https://charmshots.com/oauth/mcp/connections).

## Development and publishing

Run `npm ci` and `npm test`. GitHub Actions publishes a new package version using the organization’s `NPM_TOKEN` secret, then builds and releases the desktop bundle. Keep package.json, manifest.json, server/config.json and server.json versions aligned.

Marketplace approval is separate from npm publication. See the product’s submission notes for endpoint tests and review prerequisites.

[Website](https://charmshots.com) · [Issues](https://github.com/charmshots/mcp-server/issues)
