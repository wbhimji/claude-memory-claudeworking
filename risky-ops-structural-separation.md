---
name: risky-ops-structural-separation
description: "For projects involving irreversible/financial actions, isolate the risky code path structurally — don't rely on review or convention"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 55d75f97-d177-4081-a273-db8b28db5a48
---

When building tools that touch irreversible or money-moving operations
(trading, transfers, deletes against external systems), enforce the
read/safe vs. write/risky boundary **structurally**, not by convention.
Concretely: keep risky code in a separately-named directory, don't import
risky SDK methods anywhere else, and add a grep-based guard test that fails
the build if forbidden symbols appear outside the allowed path.

**Why:** Confirmed when scaffolding [[kalshi-lab-project]]. User
explicitly asked for "well separated to avoid accidentally placing bets,"
and approved the design where `manual_helpers/` is the only directory
allowed to construct order tickets, the rest of the codebase doesn't even
import the order-placement SDK methods, and a pytest grep enforces it.
The pattern generalizes — user values "can't accidentally fire" over
"convenient to fire."

**How to apply:**
- Default to read-only API scopes when keys support it.
- When asked to add automated execution of risky actions later, push back
  and confirm — and if approved, put it in a clearly-named separate
  package/module so the diff is highly visible.
- For new projects in this space, suggest a guard test from day one.
