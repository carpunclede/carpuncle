# GitHub MCP Server Installation Guides

This directory contains detailed installation instructions for the GitHub MCP Server across different host applications and IDEs. Choose the guide that matches your development environment.

## Installation Guides by Host Application
- **[GitHub Copilot CLI](install-copilot-cli.md)** - Installation guide for GitHub Copilot CLI (command-line interface)
- **[GitHub Copilot in other IDEs](install-other-copilot-ides.md)** - Installation for JetBrains, Visual Studio, Eclipse, and Xcode with GitHub Copilot
- **[Claude Applications](install-claude.md)** - Installation guide for Claude Web, Claude Desktop and Claude Code CLI
- **[Cursor](install-cursor.md)** - Installation guide for Cursor IDE
- **[Google Gemini CLI](install-gemini-cli.md)** - Installation guide for Google Gemini CLI
- **[Windsurf](install-windsurf.md)** - Installation guide for Windsurf IDE

## Support by Host Application

| Host Application | Local GitHub MCP Support | Remote GitHub MCP Support | Prerequisites | Difficulty |
|-----------------|---------------|----------------|---------------|------------|
| Copilot CLI | ✅ | ✅ Full (on by default; no auth needed for enterprise) | Local: Docker or Go build, GitHub PAT<br>Remote: Copilot CLI installed, Active subscription | Easy |
| Copilot in VS Code | ✅ | ✅ Full (OAuth + PAT) | Local: Docker or Go build, GitHub PAT<br>Remote: VS Code 1.101+ | Easy |
| Copilot Coding Agent | ✅ | ✅ Full (on by default; no auth needed) | Any _paid_ copilot license | Default on |
| Copilot in Visual Studio | ✅ | ✅ PAT + ❌ No OAuth | Local: Docker or Go build, GitHub PAT<br>Remote: Visual Studio 17.14+ | Easy |
| Copilot in JetBrains | ✅ | ✅ PAT + ❌ No OAuth | Local: Docker or Go build, GitHub PAT<br>Remote: JetBrains Copilot Extension v1.5.35+ | Easy |
| Claude Code | ✅ | ✅ PAT + ❌ No OAuth| GitHub MCP Server binary or remote URL, GitHub PAT | Easy |
| Claude Desktop | ✅ | ✅ PAT + ❌ No OAuth | Docker or Go build, GitHub PAT | Moderate |
| Cursor | ✅ | ✅ PAT + ❌ No OAuth | Docker or Go build, GitHub PAT | Easy |
| Google Gemini CLI | ✅ | ✅ PAT + ❌ No OAuth | Docker or Go build, GitHub PAT | Easy |
| Windsurf | ✅ | ✅ PAT + ❌ No OAuth | Docker or Go build, GitHub PAT | Easy |
| Copilot in Xcode | ✅ | ✅ PAT + ❌ No OAuth | Local: Docker or Go build, GitHub PAT<br>Remote: Copilot for Xcode latest version | Easy |
| Copilot in Eclipse | ✅ | ✅ PAT + ❌ No OAuth | Local: Docker or Go build, GitHub PAT<br>Remote: TBD | Easy |

**Legend:**
- ✅ = Fully supported
- ❌ = Not yet supported

**Note:** Remote MCP support requires host applications to register a GitHub App or OAuth app for OAuth flow support – even if the new OAuth spec is supported by that host app. Currently, only VS Code has full remote GitHub server support. 

## Installation Methods

The GitHub MCP Server can be installed using several methods. **Docker is the most popular and recommended approach** for most users, but alternatives are available depending on your needs:

### 🐳 Docker (Most Common & Recommended)
- **Pros**: No local build required, consistent environment, easy updates, works across all platforms
- **Cons**: Requires Docker installed and running
- **Best for**: Most users, especially those already using Docker or wanting the simplest setup
- **Used by**: Claude Desktop, Copilot in VS Code, Cursor, Windsurf, etc.

### 📦 Pre-built Binary (Lightweight Alternative)
- **Pros**: No Docker required, direct execution via stdio, minimal setup
- **Cons**: Need to manually download and manage updates, platform-specific binaries
- **Best for**: Minimal environments, users who prefer not to use Docker
- **Used by**: Claude Code CLI, lightweight setups

### 🔨 Build from Source (Advanced Users)
- **Pros**: Latest features, full customization, no external dependencies
- **Cons**: Requires Go development environment, more complex setup
- **Prerequisites**: [Go 1.24+](https://go.dev/doc/install)
- **Build command**: `go build -o github-mcp-server cmd/github-mcp-server/main.go`
- **Best for**: Developers who want the latest features or need custom modifications

### Important Notes on the GitHub MCP Server

- **Docker Image**: The official Docker image is now `ghcr.io/github/github-mcp-server`
- **npm Package**: The npm package @modelcontextprotocol/server-github is no longer supported as of April 2025
- **Remote Server**: The remote server URL is `https://api.githubcopilot.com/mcp/`

## General Prerequisites

All installations with Personal Access Tokens (PAT) require:
- **GitHub Personal Access Token (PAT)**: [Create one here](https://github.com/settings/personal-access-tokens/new)

Optional (depending on installation method):
- **Docker** (for Docker-based installations): [Download Docker](https://www.docker.com/)
- **Go 1.24+** (for building from source): [Install Go](https://go.dev/doc/install)

## Security Best Practices

Regardless of which installation method you choose, follow these security guidelines:

1. **Secure Token Storage**: Never commit your GitHub PAT to version control
2. **Limit Token Scope**: Only grant necessary permissions to your GitHub PAT
3. **File Permissions**: Restrict access to configuration files containing tokens
4. **Regular Rotation**: Periodically rotate your GitHub Personal Access Tokens
5. **Environment Variables**: Use environment variables when supported by your host

## Getting Help

If you encounter issues:
1. Check the troubleshooting section in your specific installation guide
2. Verify your GitHub PAT has the required permissions
3. Ensure Docker is running (for local installations)
4. Review your host application's logs for error messages
5. Consult the main [README.md](README.md) for additional configuration options

## Configuration Options

After installation, you may want to explore:
- **Toolsets**: Enable/disable specific GitHub API capabilities
- **Read-Only Mode**: Restrict to read-only operations
- **Dynamic Tool Discovery**: Enable tools on-demand

