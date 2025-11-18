# 🚀 Multi-MCP Server Architecture with FastMCP + LangChain

This repository demonstrates how to **build MCP servers from scratch** using FastMCP, register tools, and connect multiple servers with `MultiServerMCPClient`.

---

## 📘 Key Concepts

- `@mcp.tool()` → Registers a function as an MCP tool.
- **Docstrings** → Guide the LLM on when and how to call a tool.
- **stdio transport** → Local, fast communication via standard input/output.
- **streamable-http transport** → Works like a REST service; server must be running.

---

## 🧮 Math Server (`mathserver.py`)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("Math")  # Initialize server

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers and return the result."""
    return a + b

@mcp.tool()
def multiple(a: int, b: int) -> int:
    """Multiply two numbers and return the product."""
    return a * b

if __name__ == "__main__":
    mcp.run(transport="stdio")  # Local communication via stdin/stdout
```

**Why `stdio`?**  
- Server runs locally and fast.  
- Client communicates through standard input/output.  
- File path (`args: ["mathserver.py"]`) is used to launch server automatically.

---

## 🌦 Weather Server (`weather.py`)

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("Weather")

@mcp.tool()
def get_weather(city: str) -> str:
    """Fetch weather data for a given city."""
    return f"Weather info for {city}"

if __name__ == "__main__":
    mcp.run(transport="streamable-http")  # HTTP service like FastAPI
```

**Why `streamable-http`?**  
- Server behaves like an API endpoint.  
- Client connects via URL: `http://localhost:8000/mcp`.  
- Server must remain running.

---

## 🖥 MultiServerMCPClient (`client.py`)

```python
from mcp.client.multi import MultiServerMCPClient
import asyncio

async def main():
    client = MultiServerMCPClient(
        {
            "math": {
                "command": "python",
                "args": ["mathserver.py"],
                "transport": "stdio",
            },
            "weather": {
                "url": "http://localhost:8000/mcp",
                "transport": "streamable_http",
            }
        }
    )
    # Use client to call tools from both servers
```

**Key Points:**  
- `"math"` server is launched automatically using Python file.  
- `"weather"` server connects via running HTTP endpoint.  
- LLM can use tools from multiple servers in one session.

---

## 📦 Folder Structure

```
mcp-project/
│
├── mathserver.py
├── weather.py
└── client.py
```

---

## ✅ Summary

- MCP servers expose functions as tools via `@mcp.tool()`.  
- **Docstrings** help LLM decide when to call tools.  
- **stdio transport** → fast local communication.  
- **streamable-http transport** → API-style server.  
- **MultiServerMCPClient** → Connects multiple MCP servers.

---

## ⚡ Usage

1. Start weather server:

```bash
python weather.py
```

2. Run client:

```bash
python client.py
```

Now your LLM can call math and weather tools seamlessly.

