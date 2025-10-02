# Install GitHub MCP Server in GitHub Copilot CLI

GitHub Copilot CLI is a command-line interface that allows you to use Copilot directly from your terminal. It can integrate with MCP (Model Context Protocol) servers to extend its capabilities with additional tools and context.

## About GitHub Copilot CLI

GitHub Copilot CLI gives you quick access to a powerful AI agent without leaving your terminal. It can help you complete tasks more quickly by working on your behalf, and you can work iteratively with GitHub Copilot CLI to build the code you need.

**Note**: GitHub Copilot CLI is in public preview and subject to change.

### Who Can Use This Feature?

GitHub Copilot CLI is available with:
- GitHub Copilot Pro
- GitHub Copilot Pro+
- GitHub Copilot Business
- GitHub Copilot Enterprise plans

If you receive Copilot from an organization, the Copilot CLI policy must be enabled in the organization's settings.

### Supported Operating Systems
- Linux
- macOS
- Windows from within Windows Subsystem for Linux (WSL)
- Native Windows support in PowerShell (experimental)

## Prerequisites

- GitHub Copilot CLI installed (see [Installing GitHub Copilot CLI](https://docs.github.com/en/copilot/using-github-copilot/using-github-copilot-in-the-command-line))
- Active GitHub Copilot subscription (Pro, Pro+, Business, or Enterprise)
- [GitHub Personal Access Token](https://github.com/settings/personal-access-tokens/new) (for the MCP server)
- For local setup: [Docker](https://www.docker.com/) installed and running

## MCP Server Configuration

GitHub Copilot CLI can work with MCP servers to access additional tools and capabilities. The GitHub MCP Server provides tools for interacting with GitHub repositories, issues, pull requests, and more.

### Remote Server Setup (Recommended)

The remote GitHub MCP Server is the easiest way to get started:

1. **Configure Copilot CLI to use the remote GitHub MCP Server**:

   The GitHub MCP Server is available at `https://api.githubcopilot.com/mcp/`

2. **Authentication**:
   - With GitHub Copilot Enterprise or Business: OAuth authentication is enabled by default
   - With Personal Access Token: Include the Authorization header

### Local Server Setup (Docker)

For environments that require local execution or enhanced privacy:

1. **Start the GitHub MCP Server using Docker**:

   ```bash
   docker run -i --rm \
     -e GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_GITHUB_PAT \
     ghcr.io/github/github-mcp-server
   ```

2. **Configure as a named MCP server** (if your Copilot CLI version supports MCP configuration):

   Add to your MCP configuration file:

   ```json
   {
     "mcpServers": {
       "github": {
         "command": "docker",
         "args": [
           "run",
           "-i",
           "--rm",
           "-e",
           "GITHUB_PERSONAL_ACCESS_TOKEN",
           "ghcr.io/github/github-mcp-server"
         ],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_PAT"
         }
       }
     }
   }
   ```

### Local Server Setup (Binary)

If you prefer not to use Docker:

1. **Download the [latest release binary](https://github.com/github/github-mcp-server/releases)**

2. **Make it executable and add to your PATH** (Linux/macOS):
   ```bash
   chmod +x github-mcp-server
   sudo mv github-mcp-server /usr/local/bin/
   ```

3. **Configure as a named MCP server**:

   ```json
   {
     "mcpServers": {
       "github": {
         "command": "github-mcp-server",
         "args": ["stdio"],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_PAT"
         }
       }
     }
   }
   ```

## Using the GitHub MCP Server with Copilot CLI

Once configured, you can reference the GitHub MCP Server in your Copilot CLI prompts:

### Example Prompts

**List open pull requests**:
```bash
copilot -p "Use the GitHub MCP server to list my open PRs"
```

**Find good first issues**:
```bash
copilot -p "Use the GitHub MCP server to find good first issues for a new team member to work on from owner/repo"
```

**Check pull request changes**:
```bash
copilot -p "Use the GitHub MCP server to check the changes made in PR https://github.com/owner/repo/pull/123"
```

**Create an issue**:
```bash
copilot -p "Use the GitHub MCP server to create an issue in owner/repo about improving documentation"
```

### MCP Server Name

When prompting Copilot CLI, you can refer to the server as:
- "GitHub MCP server"
- "the GitHub MCP server"
- Simply mention specific GitHub operations (Copilot may automatically use the appropriate MCP server)

**Note**: Specifying the MCP server in your prompt can help Copilot deliver more accurate results, especially when multiple MCP servers are configured.

## Security Considerations

When using GitHub MCP Server with Copilot CLI:

### Trusted Directories
- Only run Copilot CLI from directories you trust
- Be cautious when running in directories containing sensitive data
- Don't launch Copilot CLI from your home directory

### Tool Permissions
Copilot CLI will ask for permission before using tools that could modify or execute files. You can:

1. **Approve once**: Allow the specific operation
2. **Approve for session**: Allow the tool for the current session
3. **Deny**: Cancel the operation and suggest alternatives

### Automatic Approval Options

For scripting or programmatic use, Copilot CLI supports approval flags:

```bash
# Allow all tools
copilot -p "your prompt" --allow-all-tools

# Allow specific tools
copilot -p "your prompt" --allow-tool 'github-mcp-server'

# Deny specific tools
copilot -p "your prompt" --allow-all-tools --deny-tool 'shell(rm)'
```

**⚠️ Warning**: Using `--allow-all-tools` gives Copilot the same access you have to files and commands without prior approval. Use with caution.

## Configuration Options

### Toolsets

You can configure which GitHub capabilities are available by setting toolsets:

```bash
docker run -i --rm \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_PAT \
  -e GITHUB_TOOLSETS="repos,issues,pull_requests,actions" \
  ghcr.io/github/github-mcp-server
```

Available toolsets:
- `context` - Current user and GitHub context
- `actions` - GitHub Actions workflows and CI/CD
- `code_security` - Code scanning tools
- `dependabot` - Dependabot tools
- `discussions` - GitHub Discussions
- `experiments` - Experimental features
- `gists` - GitHub Gist tools
- `issues` - Issues management
- `notifications` - Notifications
- `orgs` - Organization tools
- `pull_requests` - Pull request management
- `repos` - Repository operations
- `secret_protection` - Secret scanning
- `security_advisories` - Security advisories
- `users` - User tools

### Read-Only Mode

To restrict to read-only operations:

```bash
docker run -i --rm \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_PAT \
  -e GITHUB_READ_ONLY=1 \
  ghcr.io/github/github-mcp-server
```

## Troubleshooting

### MCP Server Not Found
- Verify the GitHub MCP Server is configured correctly
- Check that Docker is running (for Docker-based setup)
- Ensure the binary is in your PATH (for binary-based setup)

### Authentication Issues
- Verify your GitHub PAT is valid and not expired
- Check that the PAT has the required scopes (typically `repo` is needed)
- Ensure the `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable is set correctly

### Tools Not Working
- Check Copilot CLI logs for error messages
- Verify the GitHub MCP Server is running
- Ensure you're explicitly mentioning "GitHub MCP server" in your prompts if you have multiple MCP servers configured

### Permission Denied Errors
- Review the tool permissions when Copilot CLI prompts you
- Consider using `--allow-tool` flags for specific trusted operations
- Check that you're running from a trusted directory

## Additional Resources

- [GitHub Copilot CLI Documentation](https://docs.github.com/en/copilot/using-github-copilot/using-github-copilot-in-the-command-line)
- [GitHub MCP Server Repository](https://github.com/github/github-mcp-server)
- [MCP Protocol Specification](https://modelcontextprotocol.io/)
- [Installing GitHub Copilot CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli/installing-github-copilot-in-the-cli)

## Important Notes

- GitHub Copilot CLI is in public preview and subject to change
- The npm package `@modelcontextprotocol/server-github` is deprecated as of April 2025
- Always review suggested commands before execution
- Use read-only mode when you only need to query GitHub data
- The remote server URL is `https://api.githubcopilot.com/mcp/`
