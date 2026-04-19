# Overleaff

GitHub Sync Bridge between Overleaf, GitHub, and Abacus AI.

## Is this workflow possible?

Yes. This is a valid workflow:

1. Overleaf project syncs with a GitHub repository.
2. Abacus AI edits files in that repository through a GitHub MCP server.
3. You pull the GitHub changes back into Overleaf.

## Recommended setup

### 1) Link Overleaf to GitHub

1. Create a new **private** GitHub repository (empty is best).
2. In Overleaf, open your project.
3. Open the project menu (top-left) and go to **Sync → GitHub**.
4. Authorize and connect to the repository.
5. Let Overleaf push the initial project snapshot to GitHub.

### 2) Create a GitHub token for Abacus AI

Abacus AI needs a token to read/write your repository.

Recommended: use a **fine-grained personal access token** with access only to the specific repository and only required permissions (typically `Contents: Read and write`).

If your environment only supports classic tokens, use the minimum scopes needed and avoid broad long-lived access.

### 3) Configure GitHub MCP in Abacus AI

In your Abacus AI MCP configuration, set GitHub MCP with your token:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_github_token_here"
      }
    }
  }
}
```

## Day-to-day workflow

1. Ask Abacus AI to review/edit files in the repository.
2. Abacus AI commits changes to GitHub.
3. In Overleaf, use **Sync → GitHub → Pull** to bring updates into your project.

## Important notes

- Keep the token secret and rotate it if exposed.
- Prefer short-lived or narrowly scoped credentials whenever possible.
- Test on a copy/branch first if your paper is production-critical.
