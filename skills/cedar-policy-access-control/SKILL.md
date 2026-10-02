---
name: cedar-policy-access-control
description: Evaluates declarative Cedar policies to enforce least-privilege role-based and attribute-based access control over agent tools.
license: Apache-2.0
---

# Cedar Policy Access Control

## Overview
This skill acts as an authorization boundary, verifying whether an agent possesses explicit permission to invoke specific tools on given resources.

## Capabilities
- Formulates and evaluates deterministic Cedar policy statements (`permit` / `forbid`).
- Evaluates principal identity, action verb, and target resource attributes in real time.
- Emits structured audit logs capturing authorization decisions and policy evaluation context.
