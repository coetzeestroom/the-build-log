---
layout: post
title: "How to Set Up Your Own Model Context Protocol (MCP) Server"
date: 2025-08-02 10:00:00 +0000
author: Ray
categories: [AI, LLM, Server]
tags: [MCP, Model Context Protocol, AI Tools, LLM, Server Setup, Python]
image: assets/images/Gemini_Generated_Image_o5krhvo5krhvo5kr.png
prompt: generate me a header page image for a personal blog called "the build log" which is about playing with computers, ai, music, linux and general techi stuff. The image should use an illustrative style, and use colours that are 'retro' - beige, brown, cyan. There should cute miniature jack russel wearing a pair of headphones. The jack russel is be completely white, but has brown ears. He's the focal point of the image. His name is 'Nobby', and he wearing a collar and name tag in the shape of a raspberry pi.The theme for today's blog article setting up an Model context protocol (MCP server) which allows LLMs to use functionality online. Include imagery for AI and cloud services. 

---
## How to Set Up Your Own Model Context Protocol (MCP) Server

Large Language Models (LLMs) are incredibly powerful, but they often lack real-time access to external information or the ability to perform specific actions. This is where the **Model Context Protocol (MCP)** comes in. MCP provides a standardized way for LLMs to interact with external tools, resources, and data, effectively extending their capabilities.

This article will guide you through setting up a basic MCP server, using a weather server example, and illustrate how it integrates with coding tools and LLMs.

### What is an MCP Server?

An MCP server acts as an intermediary, exposing specific functionalities (tools) that an LLM can call. It can provide three main types of capabilities:

1.  **Resources**: File-like data that clients can read (e.g., API responses, file contents).
2.  **Tools**: Functions that can be called by the LLM (with user approval), enabling actions like fetching data or performing calculations.
3.  **Prompts**: Pre-written templates to help users accomplish specific tasks.

This tutorial focuses primarily on **tools**.

### Building a Simple MCP Weather Server

Let's walk through setting up a basic weather server that exposes `get_alerts` and `get_forecast` tools, similar to the official MCP quickstart guide.

**Prerequisites:**
*   Python 3.10 or higher
*   MCP Python SDK 1.2.0 or higher

**1. Set Up Your Environment:**

First, install `uv` (a fast Python package installer and resolver) and set up your project:

```bash
# Install uv (MacOS/Linux)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create a new project directory and navigate into it
uv init weather
cd weather

# Create and activate a virtual environment
uv venv
source .venv/bin/activate # For MacOS/Linux

# Install dependencies
uv add "mcp[cli]" httpx
```

**2. Create Your Server File (`weather.py`):**

Create a file named `weather.py` in your project directory.

**3. Import Packages and Initialize FastMCP:**

Add the following to the top of your `weather.py` file:

```python
from typing import Any
import httpx
from mcp.server.fastmcp import FastMCP

# Initialize FastMCP server with a unique name
mcp = FastMCP("weather")

# Constants for the National Weather Service API
NWS_API_BASE = "https://api.weather.gov"
USER_AGENT = "weather-app/1.0" # Replace with your app name/version
```

**4. Add Helper Functions:**

These functions will handle making requests to the NWS API and formatting the responses:

```python
async def make_nws_request(url: str) -> dict[str, Any] | None:
    """Make a request to the NWS API with proper error handling."""
    headers = {
        "User-Agent": USER_AGENT,
        "Accept": "application/geo+json"
    }
    async with httpx.AsyncClient() as client:
        try:
            response = await client.get(url, headers=headers, timeout=30.0)
            response.raise_for_status()
            return response.json()
        except Exception:
            return None

def format_alert(feature: dict) -> str:
    """Format an alert feature into a readable string."""
    props = feature["properties"]
    return f"""
Event: {props.get('event', 'Unknown')}
Area: {props.get('areaDesc', 'Unknown')}
Severity: {props.get('severity', 'Unknown')}
Description: {props.get('description', 'No description available')}
Instructions: {props.get('instruction', 'No specific instructions provided')}
"""
```

**5. Implement Your Tools:**

Use the `@mcp.tool()` decorator to expose functions as tools that the LLM can call.

```python
@mcp.tool()
async def get_alerts(state: str) -> str:
    """Get weather alerts for a US state.

    Args:
        state: Two-letter US state code (e.g. CA, NY)
    """
    url = f"{NWS_API_BASE}/alerts/active/area/{state}"
    data = await make_nws_request(url)

    if not data or "features" not in data:
        return "Unable to fetch alerts or no alerts found."

    if not data["features"]:
        return "No active alerts for this state."

    alerts = [format_alert(feature) for feature in data["features"]]
    return "\n---\n".join(alerts)

@mcp.tool()
async def get_forecast(latitude: float, longitude: float) -> str:
    """Get weather forecast for a location.

    Args:
        latitude: Latitude of the location
        longitude: Longitude of the location
    """
    # First get the forecast grid endpoint
    points_url = f"{NWS_API_BASE}/points/{latitude},{longitude}"
    points_data = await make_nws_request(points_url)

    if not points_data:
        return "Unable to fetch forecast data for this location."

    # Get the forecast URL from the points response
    forecast_url = points_data["properties"]["forecast"]
    forecast_data = await make_nws_request(forecast_url)

    if not forecast_data:
        return "Unable to fetch detailed forecast."

    # Format the periods into a readable forecast
    periods = forecast_data["properties"]["periods"]
    forecasts = []
    for period in periods[:5]:  # Only show next 5 periods
        forecast = f"""
{period['name']}:
Temperature: {period['temperature']}°{period['temperatureUnit']}
Wind: {period['windSpeed']} {period['windDirection']}
Forecast: {period['detailedForecast']}
"""
        forecasts.append(forecast)

    return "\n---\n".join(forecasts)
```

**6. Run the Server:**

Add this block to the end of `weather.py` to run your MCP server:

```python
if __name__ == "__main__":
    # Initialize and run the server
    mcp.run(transport='stdio')
```

You can now run your server using `uv run weather.py`. It will listen for messages from MCP hosts.

### How MCP Interacts with Coding Tools and LLMs

The Model Context Protocol facilitates a powerful interaction flow, allowing LLMs to leverage external capabilities. Here's a simplified diagram illustrating how an MCP server, a coding tool (like Opencode), and an LLM work together:

```
+-----------------+       +-----------------+       +-----------------+
|   Coding Tool   |       |   MCP Server    |       |       LLM       |
|  (e.g., Opencode)|       | (Tool Provider) |       | (Tool Consumer) |
+-----------------+       +-----------------+       +-----------------+
        |                         ^                         ^
        | (Develop/Deploy)        | (Tool Definitions)      | (Tool Calls)
        |                         |                         |
        v                         |                         |
+-----------------+       +-----------------+       +-----------------+
|     Client      |------>|   MCP Server    |<------|       LLM       |
| (e.g., Claude   |<------| (Tool Execution)|------>| (Tool Selection)|
|   for Desktop)  |       +-----------------+       +-----------------+
        ^                         |
        | (User Interaction)      | (External Data)
        |                         v
        +-----------------+
        | External APIs   |
        +-----------------+
```

**Explanation of the Flow:**

1.  **Coding Tool (e.g., Opencode):** This is where you, the developer, write and deploy your MCP server code. Opencode can assist in writing the Python code, managing dependencies, and running the server.
2.  **MCP Server:** This is the application you just built (`weather.py`). It exposes defined tools (like `get_alerts`, `get_forecast`) and handles their execution.
3.  **Client (e.g., Claude for Desktop):** This is an application that integrates with LLMs and can connect to MCP servers. It acts as the bridge between the LLM and your server.
4.  **LLM:** The Large Language Model (e.g., Claude) receives user queries. It analyzes these queries and, if appropriate, identifies which MCP tools can help fulfill the request.
5.  **Interaction:**
    *   The LLM, through the Client, makes a "tool call" to your MCP server.
    *   Your MCP server executes the requested tool (e.g., calls the NWS API).
    *   The results from the external API are returned to your MCP server, then back to the Client, and finally to the LLM.
    *   The LLM uses these results to formulate a comprehensive and accurate natural language response to the user.

### Integrating with an LLM Client (e.g., Claude for Desktop)

To make your MCP server usable by an LLM, you typically configure an LLM client to connect to it. For Claude for Desktop, this involves editing a configuration file (`~/Library/Application Support/Claude/claude_desktop_config.json`) to specify how to launch your server:

```json
{
  "mcpServers": {
    "weather": {
      "command": "uv",
      "args": [
        "--directory",
        "/ABSOLUTE/PATH/TO/PARENT/FOLDER/weather",
        "run",
        "weather.py"
      ]
    }
  }
}
```
**Important:** Remember to use the absolute path to your project directory and the full path to the `uv` executable. After saving, restart your Claude for Desktop application.

### Troubleshooting Common Issues

*   **Server Not Showing Up:** Double-check your `claude_desktop_config.json` syntax, ensure absolute paths are correct, and restart the client application.
*   **Tool Calls Failing Silently:** Check client logs (e.g., `~/Library/Logs/Claude/mcp*.log`) and verify your MCP server runs without errors independently.
*   **API Errors:** Ensure correct input (e.g., US coordinates for NWS API), consider rate limiting (add delays), and check the external API's status.

### Conclusion

Setting up an MCP server empowers LLMs with real-world capabilities, bridging the gap between language understanding and external actions. By following this guide, you've built a foundational MCP server and understood its crucial role in enhancing AI interactions. Explore further by building clients, adding more complex tools, and integrating with various LLM platforms!