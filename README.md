# MCP Options Order Flow Server

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)](https://python.org)
[![MCP](https://img.shields.io/badge/MCP-Compatible-green)](https://modelcontextprotocol.io)
[![FastMCP](https://img.shields.io/badge/FastMCP-Framework-orange)](https://github.com/jlowin/fastmcp)
[![gRPC](https://img.shields.io/badge/gRPC-Protocol-blue?logo=grpc)](https://grpc.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A **high-performance Model Context Protocol (MCP) server** for comprehensive **options order flow analysis**.  
This server provides **real-time options order flow data, pattern detection, and institutional bias analysis** through an intuitive MCP interface.

---

# 🚀 Features

- **Real-time Options Flow Analysis**  
  Monitor options order flow across multiple contracts with sub-10ms response times.

- **Advanced Pattern Detection**  
  Identify sweeps, blocks, and unusual volume patterns with institutional-grade algorithms.

- **Institutional Bias Tracking**  
  Monitor smart money positioning and directional sentiment.

- **Historical Trend Analysis**  
  30-minute interval analysis with key directional changes.

- **Dynamic Monitoring**  
  Configure and monitor specific strike ranges and expirations without restarts.

- **High Performance**  
  Built for production use with distributed **Go + Python architecture**.

---

# 🏗️ Architecture

This MCP server integrates with the **mcp-trading-data-broker** Go service to provide:

```
┌─────────────────────┐    gRPC     ┌─────────────────────┐    WebSocket    ┌─────────────────────┐
│ MCP Options Server  │ ──────────► │ mcp-trading-data-   │ ──────────────► │ Market Data         │
│ (Python)            │             │ broker (Go)         │                 │ Provider            │
│                     │             │                     │                 │                     │
│ • MCP Tools         │             │ • Options Analysis  │                 │ • Options Quotes    │
│ • XML Formatting    │             │ • Pattern Detection │                 │ • Real-time Data    │
│ • Context Building  │             │ • Data Storage      │                 │                     │
└─────────────────────┘             └─────────────────────┘                 └─────────────────────┘
```

---

# 📋 Prerequisites

### 1. mcp-trading-data-broker

The Go-based data broker service.

It:

- Provides a **gRPC server on port 9090**
- Handles **real-time options data collection and analysis**
- Must be running **before starting this MCP server**

### 2. Python 3.8+

Required to run the MCP server.

### 3. Network Access

Required for **gRPC communication between services**.

---

# ⚡ Quick Start

## 1. Installation

```bash
# Clone the repository
git clone <repository-url>

cd mcp-options-order-flow-server

# Create virtual environment
python -m venv venv

# Activate environment

# macOS / Linux
source venv/bin/activate

# Windows
# venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

---

# ⚙️ Configuration

Set environment variables:

```bash
# gRPC Data Broker Connection
export GRPC_HOST=localhost
export GRPC_PORT=9090
export GRPC_TIMEOUT=30

# Optional logging configuration
export LOG_LEVEL=INFO
```

---

# ▶️ Start the Server

You can start the server using **three methods**.

### Method 1 — Convenience Script

```bash
python run_server.py
```

### Method 2 — Module Execution

```bash
python -m src.mcp_server
```

### Method 3 — Direct Execution

```bash
python src/mcp_server.py
```

---

# 🧠 Claude Desktop Integration

Add this configuration to your **Claude Desktop MCP config**:

```json
{
  "mcpServers": {
    "options-flow": {
      "command": "python",
      "args": ["run_server.py"],
      "cwd": "/path/to/mcp-options-order-flow-server",
      "env": {
        "GRPC_HOST": "localhost",
        "GRPC_PORT": "9090"
      }
    }
  }
}
```

---

# 🛠️ Available MCP Tools

## 1. analyze_options_flow

Get **comprehensive options order flow analysis** for a ticker.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ticker | string | Stock ticker symbol (Example: SPY, QQQ) |

### Returns

XML formatted analysis including:

- Monitored contracts grouped by expiration and strike
- Current activity levels and directional bias
- Detected patterns with confidence scores
- Historical trend analysis with 30-minute intervals
- Institutional bias and most active strikes

---

## 2. configure_options_monitoring_tool

Configure options monitoring for specific contracts.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ticker | string | Stock ticker symbol |
| configurations | array | Monitoring configuration objects |

Each configuration contains:

- `expiration` → Expiration date (YYYYMMDD)
- `strike_range` → List of strike prices
- `include_both_types` → Monitor both calls and puts

### Example

```json
{
  "ticker": "SPY",
  "configurations": [
    {
      "expiration": 20240419,
      "strike_range": [400, 405, 410],
      "include_both_types": true
    }
  ]
}
```

---

## 3. get_monitoring_status_tool

Retrieve the **current monitoring configuration status**.

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ticker | string | Stock ticker symbol |

### Returns

XML formatted status showing all **actively monitored contracts**.

---

## 4. data_broker_health_check

Check connectivity and **health status of the data broker**.

### Returns

- Connection status
- Response time metrics
- Broker availability

---

# 💡 Example Usage

## Configure Monitoring

```
Please monitor SPY options for expiration 2024-04-19, strikes 400-410, both calls and puts
```

---

## Analyze Options Flow

```
Analyze the current options flow for SPY
```

---

## Check Monitoring Status

```
What options contracts are currently being monitored for SPY?
```

---

# 📁 Project Structure

```
mcp-options-order-flow-server
│
├── src
│   │
│   ├── formatters
│   │   ├── __init__.py
│   │   ├── context_builder.py
│   │   └── xml_formatter.py
│   │
│   ├── proto
│   │   ├── __init__.py
│   │   ├── options_order_flow_pb2.py
│   │   └── options_order_flow_pb2_grpc.py
│   │
│   ├── storage
│   │   ├── __init__.py
│   │   └── grpc_client.py
│   │
│   ├── tools
│   │   ├── __init__.py
│   │   ├── options_flow_tool.py
│   │   └── options_monitoring_tool.py
│   │
│   └── mcp_server.py
│
├── CHANGELOG.md
├── LICENSE
├── MANIFEST.in
├── README.md
├── pyproject.toml
├── requirements.txt
├── run_server.py
└── test_tools.py
```

---

# 📜 License

This project is licensed under the **MIT License**.


# 👨‍💻 Author

**Gagandeep Singh**

Computer Science Student  
Interested in **Artificial Intelligence, Computer Vision, and Automation**

---

# ⭐ GitHub Repository Description

Real-time Smart Detection System using **Python and OpenCV** for face, eye, motion, and blue object detection with alarm alerts.

---

# 🏷 GitHub Topics

python  
opencv  
computer-vision  
motion-detection  
face-detection  
eye-detection  
color-detection  
pygame  
surveillance
