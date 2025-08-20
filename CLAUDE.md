# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

WireMCP is a Model Context Protocol (MCP) server that provides real-time network traffic analysis capabilities to LLMs. It leverages Wireshark's `tshark` tool to capture and analyze network data, converting it into structured JSON outputs that LLMs can understand and reason about.

## Key Commands

### Running the Server
```bash
node index.js
```

### Installation
```bash
npm install
```

### Testing
```bash
npm test  # Currently just outputs error message - no tests implemented
```

## Architecture

### Core Components

1. **MCP Server Framework** (`index.js`):
   - Uses `@modelcontextprotocol/sdk` for MCP server implementation
   - Implements 7 network analysis tools
   - Provides corresponding prompts for each tool

2. **Network Analysis Tools**:
   - `capture_packets`: Live packet capture with JSON output
   - `get_summary_stats`: Protocol hierarchy statistics
   - `get_conversations`: TCP/UDP conversation statistics
   - `check_threats`: Threat intelligence via URLhaus integration
   - `check_ip_threats`: Targeted IP threat analysis
   - `analyze_pcap`: PCAP file analysis
   - `extract_credentials`: Credential extraction from PCAP files

3. **Dependencies**:
   - `@modelcontextprotocol/sdk`: MCP server framework
   - `axios`: HTTP requests for threat intelligence
   - `which`: Utility to locate `tshark` executable
   - `zod`: Input validation and schema definition

### Key Patterns

- **tshark Integration**: All tools rely on `tshark` for packet capture and analysis
- **Error Handling**: Comprehensive try-catch blocks with console.error logging
- **Output Trimming**: Large JSON outputs are trimmed to fit within token limits (720,000 chars)
- **Dynamic tshark Detection**: Automatic discovery of `tshark` binary across platforms
- **Temporary File Management**: PCAP files are created and cleaned up during analysis

### Security Features

- **Threat Intelligence**: URLhaus integration for IP reputation checking
- **Credential Analysis**: Extracts plaintext and encrypted credentials from network traffic
- **Protocol Support**: HTTP Basic Auth, FTP, Telnet, and Kerberos credential extraction

## Development Notes

- Requires Wireshark with `tshark` installed and in PATH
- Default network interface is `en0` (macOS), tools accept interface parameter
- Capture duration defaults to 5 seconds for live traffic tools
- All console.log output is redirected to stderr to avoid MCP protocol conflicts
- Tools are designed to be defensive security analysis tools only

## Tool Usage Context

The tools are designed to help LLMs:
- Understand network traffic patterns
- Identify security threats
- Provide network diagnostics
- Generate human-readable network analysis reports
- Assist in security audits and forensic analysis