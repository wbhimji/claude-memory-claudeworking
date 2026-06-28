---
name: feedback-memory-sync-ritual
description: "At end of any working session, auto-update state memories and push to the memory repo without asking"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 55d75f97-d177-4081-a273-db8b28db5a48
---

At the end of a working session (when the user signals wrap-up: "thanks",
"that's it", "see you later", explicit "end of session", or otherwise
clearly done with substantive work), proactively:

1. Update any in-flight project state memories (e.g.
   [[kalshi-lab-current-state]]) to reflect what was done this session,
   what's still stubbed, and revised next steps.
2. Add any new durable user/feedback/project/reference memories that
   emerged this session.
3. `cd` into the memory dir, `git add . && git commit -m "..." && git push`.

Do this without asking each time.

**Why:** User works across two laptops (this one + a second one) and
wants Claude on the other laptop to pick up in-flight context via the
shared memory repo at
https://github.com/wbhimji/claude-memory-claudeworking. Agreed
2026-06-28. Without the auto-push, the other laptop's session starts
stale.

**How to apply:**
- Memory dir for the ~/claudeworking project:
  `~/.claude/projects/-Users-wbhimji-claudeworking/memory/`
- Use a short imperative commit message that names what changed
  ("Update kalshi-lab-current-state: ran smoke test, fixed volume field").
- If user is mid-task or interrupts before wrap-up, don't push — wait for
  the actual end-of-session signal.
- If `git pull` would be needed first (rare, only if other laptop pushed
  during this session), do it and resolve any trivial conflicts.
- At session *start*, if the user mentions they were working from the
  other laptop, suggest a `git pull` in the memory dir first.
