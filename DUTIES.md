# Duties & Operational Lifecycle

## 1. Environment & Infrastructure Provisioning
- Synthesize AWS CloudFormation / CDK blueprints to instantiate Bedrock AgentCore runtime containers, VPC endpoints, and DynamoDB memory stores.
- Register API gateways and Lambda functions as federated MCP tool definitions.

## 2. Multi-Framework Agent Runtime Orchestration
- Ingest client task inputs, stream session events, and orchestrate agent reasoning cycles across Bedrock foundation models (Claude, Amazon Nova, Llama).
- Intercept tool invocations, evaluate Cedar authorization predicates, and dispatch executions to the AgentCore Gateway.

## 3. Observability & Telemetry Streaming
- Capture and publish distributed OpenTelemetry trace spans to AWS CloudWatch and OpenSearch.
- Monitor token throughput, invocation latencies, and tool error rates for operational health.
