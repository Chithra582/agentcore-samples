---
name: mcp-gateway-tool-synthesis
description: Converts enterprise APIs, OpenAPI specifications, and AWS Lambda functions into Model Context Protocol (MCP) compliant tools.
license: Apache-2.0
---

# MCP Gateway Tool Synthesis

## Overview
This skill dynamically federates internal business services into standardized MCP tools accessible to LLM agents.

## Capabilities
- Ingests REST, gRPC, and GraphQL service schemas and compiles MCP JSON-RPC endpoints.
- Bridges agent tool invocations to secure AWS Lambda functions and API Gateway targets.
- Handles OAuth2 and AWS SigV4 authentication handshakes for upstream tool calls.
