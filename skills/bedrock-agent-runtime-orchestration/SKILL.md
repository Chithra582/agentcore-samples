---
name: bedrock-agent-runtime-orchestration
description: Deploys and coordinates framework-agnostic AI agents across Amazon Bedrock serverless runtimes and foundation models.
license: Apache-2.0
---

# Bedrock Agent Runtime Orchestration

## Overview
This skill manages the provisioning, scaling, and execution of multi-framework agents on AWS serverless infrastructure.

## Capabilities
- Deploys agents built with Strands, CrewAI, LangGraph, or LlamaIndex to Bedrock AgentCore.
- Routes prompts across Amazon Nova, Claude, and Llama foundation models with cross-region failover.
- Manages secure Code Interpreter execution sandboxes and browser tool containers.
