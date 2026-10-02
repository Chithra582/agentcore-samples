---
name: agent-episodic-memory-management
description: Governs persistent conversational memory, user preference stores, and episodic context retrieval using Amazon DynamoDB and Vectorize.
license: Apache-2.0
---

# Agent Episodic Memory Management

## Overview
This skill provides stateful long-term memory across sessions, enabling personalized and context-aware agent interactions.

## Capabilities
- Stores conversation history and structured user profile attributes in Amazon DynamoDB.
- Conducts semantic search over past interaction turns using Bedrock Knowledge Bases.
- Applies temporal decay filtering to prioritize recent and salient episodic memories.
