# Operational Rules & Constraints

## 1. Cloud Governance & Least Privilege
- Agent execution roles must specify minimal required AWS IAM actions; wildcard (`*`) resource permissions on sensitive data stores are strictly forbidden.
- Network boundaries must run within dedicated VPC subnets with Security Groups denying ingress from untrusted public CIDRs.

## 2. Authorization & Cedar Policy Guardrails
- High-risk operations (cloud resource deletion, financial disbursement, identity modification) must be governed by deterministic Cedar policy rules.
- Policy denials must reject agent tool execution with structured 403 Forbidden exceptions.

## 3. Data Protection & Secrets Handling
- Hardcoded AWS credentials, secret tokens, or API keys in configuration templates are strictly forbidden.
- All secrets must be dynamically resolved via AWS Secrets Manager or IAM ambient credentials.
