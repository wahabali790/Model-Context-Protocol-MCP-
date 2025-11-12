# Model-Context-Protocol-MCP-
# 🧠 Model Context Protocol (MCP) Integration

## 📘 Overview

This project demonstrates how to **integrate multiple MCP (Model Context Protocol) servers** with **Claude Desktop** using the `claude_desktop_config.json` configuration file.

We have successfully integrated the following MCP servers:

- 🏠 **Airbnb** — via `@openbnb/mcp-server-airbnb`
- 🌦️ **Weather Server** — via `@isdaniel/mcp_weather_server` (Smithery)
- 🔍 **Exa Search** — via `exa` (Smithery)
## We can explore more MCPs on https://smithery.ai/
A **testing video** is also provided that shows how the integration works step by step inside Claude Desktop.

---

## ⚙️ Configuration Setup

To connect MCP servers, open your Claude Desktop configuration file located at:

File → Settings →Developer →claude_desktop_config.json


Then, add the following configuration:

```json
{
  "mcpServers": {
    "airbnb": {
      "command": "npx",
      "args": [
        "-y",
        "@openbnb/mcp-server-airbnb",
        "--ignore-robots-txt"
      ]
    },

    "mcp_weather_server": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@isdaniel/mcp_weather_server",
        "--key",
        "d1b05579-0fbd-458b-baed-3ba557d863df",
        "--profile",
        "mutual-aardwolf-8go8Up"
      ]
    },

    "exa": {
      "command": "npx",
      "args": [
        "-y",
        "@smithery/cli@latest",
        "run",
        "exa",
        "--key",
        "d1b05579-0fbd-458b-baed-3ba557d863df",
        "--profile",
        "mutual-aardwolf-8go8Up"
      ]
    }
  }
}
