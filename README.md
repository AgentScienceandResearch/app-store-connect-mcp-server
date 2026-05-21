# App Store Connect MCP Server

A Model Context Protocol (MCP) server for interacting with the App Store Connect API. Manage apps, beta testers, bundle IDs, devices, app metadata, analytics, and capabilities — all through conversational AI.

> **Fork notice:** This is an actively maintained fork of [JoshuaRileyDev/app-store-connect-mcp-server](https://github.com/JoshuaRileyDev/app-store-connect-mcp-server) (archived February 2026). Key fixes and improvements are tracked in the [changelog](#changelog).

---

## Overview

Built on the Model Context Protocol, this server bridges AI assistants like Claude with Apple's App Store Connect ecosystem — letting you manage your apps, testers, and analytics through natural language.

**Key capabilities:**
- **App Management** — list apps, view metadata and relationships
- **Beta Testing** — manage groups, add/remove testers, view feedback screenshots
- **App Store Versions** — create versions, manage localizations, update descriptions and keywords
- **Bundle IDs** — register and manage bundle IDs and capabilities
- **Device Management** — list and filter registered devices
- **User Management** — view team members and roles
- **Analytics & Reports** — download engagement, commerce, usage, performance, and framework analytics
- **Sales & Finance Reports** — download sales and finance data (requires vendor number)
- **Xcode Integration** — list schemes from Xcode projects and workspaces

---

## Installation

### From GitHub (recommended)

```bash
git clone https://github.com/AgentScienceandResearch/app-store-connect-mcp-server.git
cd app-store-connect-mcp-server
npm install
npm run build
```

Then configure your MCP client to run:

```json
{
  "mcpServers": {
    "app-store-connect": {
      "command": "node",
      "args": ["/path/to/app-store-connect-mcp-server/dist/src/index.js"],
      "env": {
        "APP_STORE_CONNECT_KEY_ID": "YOUR_KEY_ID",
        "APP_STORE_CONNECT_ISSUER_ID": "YOUR_ISSUER_ID",
        "APP_STORE_CONNECT_P8_PATH": "/path/to/your/AuthKey_XXXXXXXXXX.p8",
        "APP_STORE_CONNECT_VENDOR_NUMBER": "YOUR_VENDOR_NUMBER_OPTIONAL"
      }
    }
  }
}
```

**Config file locations:**

| Client | Path |
|--------|------|
| Claude Desktop (macOS) | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Code CLI (macOS) | `~/.claude/settings.json` |
| Claude Desktop (Windows) | `%APPDATA%\Claude\claude_desktop_config.json` |

---

## Authentication

1. Go to [App Store Connect → Users & Access → Integrations → App Store Connect API](https://appstoreconnect.apple.com/access/integrations/api)
2. Under **Team Keys**, generate a new API key (or use an existing one)
3. Download the `.p8` private key file — **Apple only lets you download it once**
4. Note your **Key ID** and **Issuer ID** from the top of that page
5. Set the environment variables in your MCP config:
   - `APP_STORE_CONNECT_KEY_ID` — your Key ID (e.g. `U53BXHMFX2`)
   - `APP_STORE_CONNECT_ISSUER_ID` — your Issuer ID (UUID format)
   - `APP_STORE_CONNECT_P8_PATH` — absolute path to your `.p8` file

> **Important:** Use a **Team Key** from the Team Keys section, not an Individual API Key. Individual keys will return authentication errors with this server.

### Optional: Sales & Finance Reports

To enable `download_sales_report` and `download_finance_report`:
- Set `APP_STORE_CONNECT_VENDOR_NUMBER` — found in App Store Connect under Sales and Trends or Payments and Financial Reports.

---

## Tool Reference

### App Management
| Tool | Description |
|------|-------------|
| `list_apps` | List all apps in App Store Connect |
| `get_app_info` | Get detailed info about a specific app |

### Beta Testing
| Tool | Description |
|------|-------------|
| `list_beta_groups` | List all beta groups |
| `list_group_testers` | List testers in a beta group |
| `add_tester_to_group` | Add a tester to a beta group |
| `remove_tester_from_group` | Remove a tester from a beta group |
| `list_beta_feedback_screenshots` | List beta feedback submissions with screenshots |
| `get_beta_feedback_screenshot` | Get a specific feedback submission (downloads image by default) |

### App Store Versions & Localizations
| Tool | Description |
|------|-------------|
| `create_app_store_version` | Create a new app store version |
| `list_app_store_versions` | List all versions for an app |
| `list_app_store_version_localizations` | List localizations for a version |
| `get_app_store_version_localization` | Get a specific localization |
| `update_app_store_version_localization` | Update description, keywords, what's new, etc. |

### Bundle IDs
| Tool | Description |
|------|-------------|
| `create_bundle_id` | Register a new bundle ID |
| `list_bundle_ids` | List bundle IDs registered to your team |
| `get_bundle_id_info` | Get details for a specific bundle ID |
| `enable_bundle_capability` | Enable a capability (Push, iCloud, etc.) |
| `disable_bundle_capability` | Disable a capability |

### Devices & Users
| Tool | Description |
|------|-------------|
| `list_devices` | List registered devices |
| `list_users` | List App Store Connect team members |

### Analytics & Reports
| Tool | Description |
|------|-------------|
| `create_analytics_report_request` | Create an analytics report request for an app |
| `list_analytics_reports` | List available reports for a request |
| `list_analytics_report_segments` | Get download segments for a report |
| `download_analytics_report_segment` | Download data from a report segment |
| `download_sales_report` | Download sales/trends report *(requires vendor number)* |
| `download_finance_report` | Download finance report by region *(requires vendor number)* |

### Xcode
| Tool | Description |
|------|-------------|
| `list_schemes` | List schemes in an Xcode project or workspace |

---

## Development

```bash
npm install        # Install dependencies
npm run build      # Compile TypeScript → dist/
npm start          # Run the compiled server
```

---

## Changelog

### v1.2.0 (2026)
- **Fix:** 21 of 25 tools were returning `[object Object]` instead of actual data due to invalid MCP response shape (`{ toolResult: value }` instead of the correct `{ content: [{ type: "text", text: JSON.stringify(value) }] }`). All affected tools now use the correct `formatResponse()` wrapper.
- Updated package name to `@agentscienceandresearch/appstore-connect-mcp-server`
- Updated dependencies

### v1.1.2 and earlier
See the [original repository](https://github.com/JoshuaRileyDev/app-store-connect-mcp-server) for prior history.

---

## License

MIT — see [LICENSE](./LICENSE).

Originally created by [Joshua Riley](https://github.com/JoshuaRileyDev). Forked and maintained by [AgentScienceandResearch](https://github.com/AgentScienceandResearch).

## Related Links
- [Model Context Protocol Documentation](https://modelcontextprotocol.io)
- [App Store Connect API Documentation](https://developer.apple.com/documentation/appstoreconnectapi)
- [Original repository (archived)](https://github.com/JoshuaRileyDev/app-store-connect-mcp-server)
