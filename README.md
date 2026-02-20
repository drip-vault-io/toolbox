# Toolbox

Prebuilt CLI tools for [Vaulty](https://github.com/drip-vault-io) agents. Each tool compiles to a single static binary that can be downloaded and run with zero dependencies.

## Tools

| Tool | Description | Binary |
|------|-------------|--------|
| **[vgoog](./vgoog/)** | Google Workspace TUI — Gmail, Calendar, Drive, Sheets, Docs, Slides, Forms, Tasks, Contacts, Apps Script | `vgoog` |

## Install

### Download a prebuilt binary

Every tagged release publishes binaries for Linux, macOS, and Windows. Download the one for your platform:

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

### Vaulty agent machines (Fly.io)

On Fly machines, tools are installed to `/data/bin/` at boot:

```bash
mkdir -p /data/bin
curl -sL https://github.com/drip-vault-io/toolbox/releases/latest/download/vgoog-linux-amd64 -o /data/bin/vgoog
chmod +x /data/bin/vgoog
```

### Build from source

Each tool is a standalone Rust project:

```bash
cd vgoog
cargo build --release
# Binary at target/release/vgoog
```

## Adding a new tool

1. Create a directory at the repo root (e.g. `mytool/`)
2. Initialize a Rust project: `cargo init mytool`
3. Add the tool to the matrix in `.github/workflows/release.yml`
4. Add it to the table in this README
5. Tag a release — CI builds and publishes binaries automatically

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
