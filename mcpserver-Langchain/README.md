# Multi-MCP Server Setup (FastMCP + LangChain)

This project shows how to build and connect multiple **MCP servers** using **FastMCP** and **MultiServerMCPClient**.

---

## 📌 Overview
We create two MCP servers:

1. **Math Server (stdio transport)** – runs locally through stdin/stdout  
2. **Weather Server (streamable-http transport)** – runs like an HTTP service  

Both are connected in one client using **MultiServerMCPClient**.

---

## 🧮 Math Server (mathserver.py)

```python
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("Math")

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b

@mcp.tool()
def multiple(a: int, b: int) -> int:
    """Multiply two numbers."""
    return a * b

if __name__ == "__main__":
    mcp.run(transport="stdio")
stdio transport → server communicates via standard input/output.

🌦 Weather Server (weather.py)
python
Copy code
mcp.run(transport="streamable-http")
streamable-http transport → server runs like an HTTP API at:

bash
Copy code
http://localhost:8000/mcp
This server must stay running.

🖥 MultiServerMCPClient (client.py)
python
Copy code
client = MultiServerMCPClient({
    "math": {
        "command": "python",
        "args": ["mathserver.py"],
        "transport": "stdio",
    },
    "weather": {
        "url": "http://localhost:8000/mcp",
        "transport": "streamable_http",
    }
})
This connects both servers so an LLM can call tools from both.

📝 Key Points
@mcp.tool() turns a function into an MCP tool.

The docstring explains the tool to the LLM.

stdio → local, fast, client launches the server.

streamable-http → acts like a REST API.

MultiServerMCPClient manages all servers in one place.

📂 Folder Structure
Copy code
project/
│── mathserver.py
│── weather.py
└── client.py
