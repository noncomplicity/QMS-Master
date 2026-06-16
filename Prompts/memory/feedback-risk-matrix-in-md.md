---
name: Risk matrix as data not code
description: Risk acceptance criteria should be defined in markdown documents and parsed by scripts, not hardcoded in Python
type: feedback
---

Define risk acceptance matrices in markdown (e.g., in the risk management plan), not hardcoded in scripts. Scripts should parse the matrix from the document so there's a single source of truth.

**Why:** Avoids divergence between the regulatory document and the tooling. The risk management plan is the authoritative source.

**How to apply:** When building risk report generators or similar tooling, read the acceptance matrix from the relevant system document rather than embedding it in code.
