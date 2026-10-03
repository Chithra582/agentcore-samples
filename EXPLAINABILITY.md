# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **# Explainability & Decision Transparency Report** (`agentcore-samples`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** # Explainability & Decision Transparency Report (`agentcore-samples`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Enterprise AI Agent Runtime, Bedrock AgentCore & MCP Tool Gateway  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

### 1. Deterministic Multi-Stage Decision Pipeline
The agent executes enterprise agentic workflows through a deterministic, 5-stage orchestration pipeline on Amazon Bedrock AgentCore.

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



### 3. Thresholding & Refusal Decision Criteria

# Explainability & Decision Transparency Report enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on Policy Violation**: Requests violating boundary constraints halt with code `ERR_POLICY_VIOLATION`.
- **Refusal on Timeout**: Executions exceeding budget limits terminate with code `ERR_EXECUTION_TIMEOUT`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Operational Review**: Sensitive actions require operator sign-off.
- **Audit Logging**: All decisions are recorded for auditability.

---

## The Data It Uses

# Explainability & Decision Transparency Report operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Input Directives**: Operational tasks and data payloads.

### 2. Configuration & Reference Data

- **Configuration Schemas**: Declarative system configuration files.

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

### 1. Deterministic Multi-Stage Decision Pipeline
The agent executes enterprise agentic workflows through a deterministic, 5-stage orchestration pipeline on Amazon Bedrock AgentCore.

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

### 2. Mathematical Decision & Affinity Scoring
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
When agent requests exceed security limits or violate policy rules, execution is rejected with structured error codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Cedar Policy Verdict** | `Deny` verdict | Refuse tool execution with 403 Forbidden | `ERR_CEDAR_POLICY_DENIAL` |
| **IAM Token Expiration** | Token TTL $\le 0$ | Terminate runtime session immediately | `ERR_AUTH_EXPIRED_TOKEN` |
| **Code Interpreter Execution Timeout** | $> 60$ seconds | Terminate sandboxed container process | `ERR_SANDBOX_TIMEOUT` |
| **Bedrock Quota Throttle** | HTTP 429 ThrottlingException | Backoff and trigger secondary model failover | `ERR_BEDROCK_RATE_THROTTLED` |
| **Memory Payload Overflow** | Context $> 128$ KB per chunk | Chunk payload and reject oversized memory write | `ERR_MEMORY_PAYLOAD_EXCEEDED` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Bedrock Cross-Region Failover)**: If a specific AWS region experiences capacity constraints, the runtime re-routes inference to an alternate configured AWS region within 500ms.
2. **Tier 2 (Degraded Tool Fallback)**: If an external enterprise MCP gateway tool times out, the agent falls back to cached schema responses or alternative data pipelines.
3. **Tier 3 (Human Administrator Intervention)**: Critical infrastructure modifications (e.g. database schema migrations, IAM permission escalations) trigger an Amazon SNS notification awaiting explicit administrative approval.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **Agent Queries**: Natural language prompts, system role instructions, and conversational history.
- **Tool Payloads**: OpenAPI 3.0 parameters, JSON-RPC 2.0 frames, and Python scripts for Code Interpreter.
- **Identity Context**: AWS IAM role ARNs, Cognito JWT tokens, and OAuth2 claims.

### 2. Reference Storage & Database Services
- **Amazon DynamoDB**: High-throughput key-value storage for session metadata and episodic memory.
- **Amazon OpenSearch / Bedrock Knowledge Bases**: Vector search indices for enterprise document retrieval (RAG).
- **AWS Secrets Manager**: Encrypted storage for external API tokens and database credentials.

### 3. Model Lineage & System Architecture
- **Foundation Models**: Anthropic Claude 3.5/3.7 Sonnet, Amazon Nova Premiere/Pro, Meta Llama 3.3.
- **Framework Support**: AWS Bedrock AgentCore SDK, Strands, CrewAI, LangGraph, LlamaIndex.

### 4. Data Privacy, Governance & Retention
- **Customer Data Isolation**: No training on customer prompts or completions per AWS Bedrock service terms.
- **At-Rest & In-Transit Encryption**: All memory stores and runtime communication encrypted via AWS KMS customer-managed keys (CMK) and TLS 1.3.
- **Memory Retention Controls**: Configurable DynamoDB TTL attributes automatically expire ephemeral chat history after 30 days.

---

## Limitations

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
| - Deterministic Multi-Stage Decision Pipeline
The agent executes enterprise agentic workflows through a deterministic, 5-stage orchestration pipeline on Amazon Bedrock AgentCore.

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

### 2. Mathematical Decision & Affinity Scoring
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
When agent requests exceed security limits or violate policy rules, execution is rejected with structured error codes:

| Threshold Parameter | Value | Decision / Refusal Action | Error Code |
| :--- | :--- | :--- | :--- |
| **Cedar Policy Verdict** | `Deny` verdict | Refuse tool execution with 403 Forbidden | `ERR_CEDAR_POLICY_DENIAL` |
| **IAM Token Expiration** | Token TTL $\le 0$ | Terminate runtime session immediately | `ERR_AUTH_EXPIRED_TOKEN` |
| **Code Interpreter Execution Timeout** | $> 60$ seconds | Terminate sandboxed container process | `ERR_SANDBOX_TIMEOUT` |
| **Bedrock Quota Throttle** | HTTP 429 ThrottlingException | Backoff and trigger secondary model failover | `ERR_BEDROCK_RATE_THROTTLED` |
| **Memory Payload Overflow** | Context $> 128$ KB per chunk | Chunk payload and reject oversized memory write | `ERR_MEMORY_PAYLOAD_EXCEEDED` |

### 4. Multi-Tier Fallback Mechanisms & Human-in-the-Loop Governance
1. **Tier 1 (Automated Bedrock Cross-Region Failover)**: If a specific AWS region experiences capacity constraints, the runtime re-routes inference to an alternate configured AWS region within 500ms.
2. **Tier 2 (Degraded Tool Fallback)**: If an external enterprise MCP gateway tool times out, the agent falls back to cached schema responses or alternative data pipelines.
3. **Tier 3 (Human Administrator Intervention)**: Critical infrastructure modifications (e.g. database schema migrations, IAM permission escalations) trigger an Amazon SNS notification awaiting explicit administrative approval.

---

## The Data It Uses

### 1. Ingestion Data & Input Types
- **Agent Queries**: Natural language prompts, system role instructions, and conversational history.
- **Tool Payloads**: OpenAPI 3.0 parameters, JSON-RPC 2.0 frames, and Python scripts for Code Interpreter.
- **Identity Context**: AWS IAM role ARNs, Cognito JWT tokens, and OAuth2 claims.

### 2. Reference Storage & Database Services
- **Amazon DynamoDB**: High-throughput key-value storage for session metadata and episodic memory.
- **Amazon OpenSearch / Bedrock Knowledge Bases**: Vector search indices for enterprise document retrieval (RAG).
- **AWS Secrets Manager**: Encrypted storage for external API tokens and database credentials.

### 3. Model Lineage & System Architecture
- **Foundation Models**: Anthropic Claude 3.5/3.7 Sonnet, Amazon Nova Premiere/Pro, Meta Llama 3.3.
- **Framework Support**: AWS Bedrock AgentCore SDK, Strands, CrewAI, LangGraph, LlamaIndex.

### 4. Data Privacy, Governance & Retention
- **Customer Data Isolation**: No training on customer prompts or completions per AWS Bedrock service terms.
- **At-Rest & In-Transit Encryption**: All memory stores and runtime communication encrypted via AWS KMS customer-managed keys (CMK) and TLS 1.3.
- **Memory Retention Controls**: Configurable DynamoDB TTL attributes automatically expire ephemeral chat history after 30 days.

---

## Limitations

### 1. Cold Start Latency in Containerized Serverless Runtimes | Section 1 | Verified |
| - AWS Service Quota Burst Ceilings | Section 2 | Verified |
| - Cross-Account MCP Gateway Latency | Section 3 | Verified |
| - Context Boundary Saturation in Long-Horizon Memory | Section 4 | Verified |
| - Deterministic Guarantee Boundaries of External Web Tools | Section 5 | Verified |
