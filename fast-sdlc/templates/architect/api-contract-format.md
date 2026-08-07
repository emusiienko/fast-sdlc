<!-- Default format for the API Contract artifact — accept as-is or customize with architect during Step 0 (Confirm Artifact Formats). Created or updated only at Reconcile — never written directly in Project Setup or Plan Next Scope mode. -->

# API Contract

Mandatory for every Service Relation that crosses a bounded-context boundary (see Architecture > Services). For relations within the same bounded context, use judgment — formalize one here only if it warrants a stable contract even inside the context.

This document is the contract — every Service it applies to must honor it exactly.

For each endpoint:

### [METHOD] [/path]
- **Request**: [TypeScript interface]
- **Response**: [TypeScript interface]
- **Errors**: [error responses]
- **Auth**: [auth requirements]
- **Example**: [example request/response]
