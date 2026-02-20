# Phase 1: Project Scaffold, Config, Auth & Core HTTP Client

## Goals
- Cargo project with all dependencies
- Config file management (~/.config/vgoog/config.toml)
- OAuth2 token management with automatic refresh
- Core HTTP client with rate limiting and error handling
- Module structure for all 10 Google services

## Dependencies
- `ratatui` + `crossterm` - TUI framework
- `reqwest` - HTTP client (with rustls)
- `tokio` - async runtime
- `serde` / `serde_json` - serialization
- `toml` - config file
- `dirs` - platform config dirs
- `anyhow` / `thiserror` - error handling
- `chrono` - date/time
- `base64` - encoding
- `urlencoding` - URL encoding
- `tui-textarea` - text input widget
- `clipboard` - clipboard support

## Module Structure
```
src/
  main.rs           - entry point, TUI app loop
  config.rs         - config management
  auth.rs           - OAuth2 token refresh
  client.rs         - HTTP client wrapper
  error.rs          - error types
  ui/
    mod.rs          - TUI framework
    app.rs          - app state
    views/          - per-service views
  api/
    mod.rs          - API module registry
    gmail.rs        - Gmail API
    calendar.rs     - Calendar API
    drive.rs        - Drive API
    sheets.rs       - Sheets API
    docs.rs         - Docs API
    slides.rs       - Slides API
    forms.rs        - Forms API
    tasks.rs        - Tasks API
    people.rs       - People/Contacts API
    apps_script.rs  - Apps Script API
```

## Config Format (~/.config/vgoog/config.toml)
```toml
[auth]
client_id = "..."
client_secret = "..."
access_token = "..."
refresh_token = "..."
token_expiry = "2024-01-01T00:00:00Z"
```
