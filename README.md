# Toolbox

Prebuilt CLI tools that give AI agents superpowers. Each tool compiles to a single static binary — no dependencies, no containers, no runtime setup.

## The Bigger Picture

Toolbox is one piece of a larger platform for deploying personal AI agents:

```
┌─────────────────────────────────────────────────────────┐
│                      Your Agent                         │
│       (local machine or cloud — Fly.io, etc.)          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐   │
│   │  vgoog  │  │ vlinear │  │ vnotion │  │   vai   │   │
│   │ Google  │  │  Linear │  │ Notion  │  │   AI    │   │
│   │Workspace│  │ Issues  │  │  Docs   │  │ Models  │   │
│   └─────────┘  └─────────┘  └─────────┘  └─────────┘   │
│                                                         │
│                    Toolbox (this repo)                  │
│          Static binaries at /data/bin/                  │
└─────────────────────────────────────────────────────────┘
                          ▲
                          │ OAuth tokens & credentials
                          │ (managed via web dashboard)
                          ▼
┌─────────────────────────────────────────────────────────┐
│                    Web Dashboard                        │
│   • Connect accounts (Google, Linear, Notion, etc.)    │
│   • Manage credentials with OAuth flows                │
│   • No Google Cloud Console, no CLI token pasting      │
└─────────────────────────────────────────────────────────┘
```

**Key principles:**

- **Runs anywhere, optimized for cloud.** Tools work on your laptop or in cloud environments (Fly.io, etc.). Credentials come from environment variables or config files — no interactive setup required, but local TUI modes are available when you want them.
- **Zero-friction auth.** Users connect their accounts through a web dashboard with standard OAuth flows — no navigating developer consoles or pasting tokens manually.
- **Agent-native.** Each tool exposes functionality that AI agents can invoke programmatically. TUI modes are available for human debugging and standalone use.
- **Standalone binaries.** Every tool compiles to a ~3-5MB static binary with no runtime dependencies. Download, chmod, run — on any platform.

## Tools

| Tool | Description | Status |
|------|-------------|--------|
| **[vgoog](./vgoog/)** | Google Workspace — Gmail, Calendar, Drive, Sheets, Docs, Slides, Forms, Tasks, Contacts, Apps Script | ✅ Available |
| **vlinear** | Linear issue tracking — create, update, search, manage sprints | 🚧 Planned |
| **vnotion** | Notion — pages, databases, blocks | 🚧 Planned |
| **vai** | Universal AI model interface — OpenAI, Anthropic, Google, local models | 🚧 Planned |
| **vqueue** | Background job queue with persistence | 🚧 Planned |
| **vwatch** | URL monitoring and alerts | 🚧 Planned |

## Install

### Download a prebuilt binary

Every tagged release publishes binaries for Linux, macOS, and Windows:

```bash
# Linux (amd64)
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-linux-amd64 -o vgoog
chmod +x vgoog

# Linux (arm64)
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-linux-arm64 -o vgoog
chmod +x vgoog

# macOS (Apple Silicon)
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-darwin-arm64 -o vgoog
chmod +x vgoog

# macOS (Intel)
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-darwin-amd64 -o vgoog
chmod +x vgoog

# Windows (amd64)
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-windows-amd64.exe -o vgoog.exe
```

### Cloud deployment (Fly.io example)

On Fly machines, tools are typically installed to `/data/bin/` at boot:

```bash
mkdir -p /data/bin
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-linux-amd64 -o /data/bin/vgoog
chmod +x /data/bin/vgoog
```

Credentials are provided via environment variables or mounted config files — no interactive setup required.

### Build from source

Each tool is a standalone Rust project:

```bash
cd vgoog
cargo build --release
# Binary at target/release/vgoog
```

## Configuration

Tools support two configuration modes:

### 1. Environment variables (recommended for cloud)

```bash
export VGOOG_CLIENT_ID="..."
export VGOOG_CLIENT_SECRET="..."
export VGOOG_ACCESS_TOKEN="..."
export VGOOG_REFRESH_TOKEN="..."
```

### 2. Config file

Tools read from `~/.config/{tool}/config.toml` (or equivalent paths on Windows/macOS). See each tool's README for config schema.

In cloud deployments, config is typically mounted from a secrets manager or injected at boot.

## Adding a new tool

1. Create a directory at the repo root (e.g. `mytool/`)
2. Initialize a Rust project: `cargo init mytool`
3. Add the tool to the matrix in `.github/workflows/release.yml`
4. Add it to the table in this README
5. Tag a release — CI builds and publishes binaries automatically

### Design guidelines

- **Support headless operation.** Tools should work without a TTY for agent invocation.
- **Accept credentials from environment.** Don't require interactive setup.
- **Output JSON.** Structured output that agents can parse.
- **Keep binaries small.** Use `rustls` (no OpenSSL), enable LTO, strip symbols.
- **Handle errors gracefully.** Return meaningful exit codes and error messages.

## Releases

Releases are triggered by pushing a git tag:

```bash
git tag v0.1.0
git push origin v0.1.0
```

CI builds all tools for all platforms and attaches the binaries to the GitHub release. Binary naming convention:

```
{tool}-{os}-{arch}[.exe]
```

Examples: `vgoog-linux-amd64`, `vgoog-darwin-arm64`, `vgoog-windows-amd64.exe`

## License

MIT
