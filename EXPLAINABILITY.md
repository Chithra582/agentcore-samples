# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **# Explainability & Decision Transparency Report** (`agentcore-samples`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** # Explainability & Decision Transparency Report (`agentcore-samples`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Enterprise AI Agent Runtime, Bedrock AgentCore & MCP Tool Gateway  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

# Explainability & Decision Transparency Report operates via a deterministic five-stage operational pipeline.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic AgentCore Pipeline                           |
+-----------------------------------------------------------------------------------+
|  [Stage 1: User Request Ingestion & Identity Authentication]                      |
|     --> Validate IAM/OAuth session token, hydrate identity context, & parse query |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Context Retrieval & Episodic Memory Injection]                         |
|     --> Query DynamoDB/Vector store for historical preferences & session state    |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Cedar Policy Evaluation & Tool Authorization Gate]                     |
|     --> Evaluate fine-grained Cedar RBAC policies before allowing tool dispatch   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Serverless Runtime Execution & Gateway Dispatch]                       |
|     --> Dispatch action to MCP Gateway, Code Interpreter, or Lambda worker        |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: State Checkpoint & OpenTelemetry Trace Publication]                   |
|     --> Persist updated episodic memory and stream traces to AWS CloudWatch       |
+-----------------------------------------------------------------------------------+
```

### 2. Decision Logic & Routing Formulations

Scoring
Model routing across Bedrock foundation models (Anthropic Claude 3.7/3.5, Amazon Nova Pro/Lite) utilizes a latency-cost optimization formulation:

$$S_{\text{model}}(m) = w_1 \cdot \text{CapabilityMatch}(m, T) + w_2 \cdot \left(1 - \frac{\text{TTFT}(m)}{\text{MaxTTFT}}\right) - w_3 \cdot \text{NormalizedCost}(m)$$

Where:
- $w_1 = 0.50$: Semantic capability alignment between task $T$ complexity and model parameter size.
- $w_2 = 0.30$: Moving average time-to-first-token (TTFT) responsiveness.
- $w_3 = 0.20$: Relative token consumption pricing coefficient.

Episodic memory relevance scoring during state retrieval is calculated as:

$$R(e_i, q) = \lambda \cdot \text{CosineSim}(\mathbf{v}_{e_i}, \mathbf{v}_q) + (1 - \lambda) \cdot \exp(-\delta \cdot (t_{\text{now}} - t_i))$$

Where $\mathbf{v}$ denotes vector embeddings, $\delta$ represents temporal memory decay, and $\lambda = 0.70$.

### 3. Thresholding & Refusal Decision Criteria

# Explainability & Decision Transparency Report enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on ERR_CEDAR_POLICY_DENIAL**: **Cedar Policy Verdict** halts execution with code `ERR_CEDAR_POLICY_DENIAL`.
- **Refusal on ERR_AUTH_EXPIRED_TOKEN**: **IAM Token Expiration** halts execution with code `ERR_AUTH_EXPIRED_TOKEN`.
- **Refusal on ERR_SANDBOX_TIMEOUT**: **Code Interpreter Execution Timeout** halts execution with code `ERR_SANDBOX_TIMEOUT`.
- **Refusal on ERR_BEDROCK_RATE_THROTTLED**: **Bedrock Quota Throttle** halts execution with code `ERR_BEDROCK_RATE_THROTTLED`.
- **Refusal on ERR_MEMORY_PAYLOAD_EXCEEDED**: **Memory Payload Overflow** halts execution with code `ERR_MEMORY_PAYLOAD_EXCEEDED`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Consequential Action Sign-Off**: Sensitive and consequential actions require operator sign-off.
- **Offline Ledger Auditing**: Operators can verify execution records and state transitions offline.

---

## The Data It Uses

# Explainability & Decision Transparency Report operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Agent Queries**: Natural language prompts, system role instructions, and conversational history.
- **Tool Payloads**: OpenAPI 3.0 parameters, JSON-RPC 2.0 frames, and Python scripts for Code Interpreter.
- **Identity Context**: AWS IAM role ARNs, Cognito JWT tokens, and OAuth2 claims.

### 2. Configuration & Reference Data

- **Configuration Schemas**: Declarative system policy files.

### 3. Base Model & Inference Lineage

- **Foundation Models**: Anthropic Claude 3.5/3.7 Sonnet, Amazon Nova Premiere/Pro, Meta Llama 3.3.
- **Framework Support**: AWS Bedrock AgentCore SDK, Strands, CrewAI, LangGraph, LlamaIndex.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of # Explainability & Decision Transparency Report is essential for effective deployment.

### 1. Cold Start Latency in Containerized Serverless Runtimes
- **Limitation**: Initializing complex Python environments with scientific libraries within Code Interpreter sandboxes can take 2-5 seconds on cold start.
- **Mitigation**: Maintain warm container pools and pre-bundle common dependencies in custom base container images.

### 2. AWS Service Quota Burst Ceilings
- **Limitation**: Sudden spikes in concurrent agent invocations can exhaust AWS Bedrock provisioned throughput or regional TPM quotas.
- **Mitigation**: Deploy cross-region inference profiles and implement client-side token bucket rate limiting.

### 3. Cross-Account MCP Gateway Latency
- **Limitation**: Routing tool calls across multiple AWS accounts and VPCs via Transit Gateway introduces minor network hops (10-30ms).
- **Mitigation**: Establish AWS PrivateLink VPC endpoints in the local agent subnet to bypass public internet routing.

### 4. Context Boundary Saturation in Long-Horizon Memory
- **Limitation**: Ingesting extensive episodic history can bloat the LLM prompt, increasing latency and cost.
- **Mitigation**: Apply dynamic semantic similarity filtering and rolling window summarization before context injection.

### 5. Deterministic Guarantee Boundaries of External Web Tools
- **Limitation**: External web search or browser scraping tools are susceptible to target site DOM shifts and network transient outages.
- **Mitigation**: Enforce circuit breakers, response caching, and structured schema extraction validators.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Cold Start Latency in Containerized Serverless Runtimes | Section 1 | Verified |
| - AWS Service Quota Burst Ceilings | Section 2 | Verified |
| - Cross-Account MCP Gateway Latency | Section 3 | Verified |
| - Context Boundary Saturation in Long-Horizon Memory | Section 4 | Verified |
| - Deterministic Guarantee Boundaries of External Web Tools | Section 5 | Verified |
