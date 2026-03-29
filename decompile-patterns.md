# Decompile Patterns — context-bridge

Signal extraction rules for Decompile Mode. Use when a user pastes a raw AI conversation and wants a clean Context Block extracted from it.

---

## Signal Detection — What to Keep

### High-confidence signals (always include)

| Pattern | Example | Extract as |
|---|---|---|
| Explicit choice | "let's go with X", "we'll use X", "decided on X" | Decision |
| Ruled out | "don't use X", "avoid X", "X won't work" | Constraint or Decision |
| Stack mention | "I'm using React", "it's a Next.js project" | Stack |
| File path | any `/path/to/file` or `filename.ext` | Files to Know |
| Error confirmed fixed | "that worked", "fixed it" | Current State |
| Task boundary | "now I need to", "next step is", "can you help me with" | Next Step |

### Medium-confidence signals (include with light qualifier)

| Pattern | Example | Extract as |
|---|---|---|
| Implied tech | user pastes `import { useState }` | Stack: React (inferred) |
| Described feature | "I want it to do X" and AI said "done" | Current State |
| Preference stated | "I prefer TypeScript" | Constraint |

### Low-confidence (flag or discard)

| Pattern | Action |
|---|---|
| "maybe", "thinking about", "might use" | Discard unless it's the only mention of that dimension |
| AI suggestion not confirmed | Discard |
| User asked a question, no resolution shown | Note as "Open Question" at the bottom of block |

---

## Noise Detection — What to Strip

Always discard:
- Greetings and pleasantries ("sure!", "great question", "happy to help")
- Failed code blocks the user said didn't work (unless the failure is informative)
- Off-topic tangents
- Redundant re-explanations of the same thing
- Anything after "thanks, that's all for now" or similar session closers
- Version history / changelogs the user pasted as context (summarize instead)

---

## Conflict Resolution

If the conversation shows a decision being reversed:

1. Take the **most recent** version
2. Note the reversal in the DECISIONS section: `[Changed from X to Y — use Y]`

Example:
```
### Decisions Made
3. Auth: JWT stored in httpOnly cookies [Changed from localStorage after security review — use cookies]
```

---

## Open Questions

If the conversation ended with unresolved questions, add an OPEN QUESTIONS section to the block:

```
### Open Questions (Resolve Before Starting)
- [question that was raised but not answered]
- [decision that was deferred]
```

---

## Conversation Quality Signals

Use these to assess how reliable the decompiled block will be:

| Signal | Reliability impact |
|---|---|
| User confirmed AI outputs worked | High reliability |
| User corrected AI multiple times | Check the corrected version carefully |
| Very short conversation (<10 turns) | May be missing decisions — ask user to fill gaps |
| Long conversation (50+ turns) | Likely has reversals — check for conflicts |
| Conversation is multi-session (timestamps jump) | High risk of context drift — flag this to user |

---

## Output Addition for Decompile Mode

After the standard Context Block, always add:

```
---
### Decompile Notes
- Source: [brief description of what was pasted — "20-turn Claude conversation", "Cursor chat", etc.]
- Reliability: [High / Medium / Low — based on quality signals above]
- Gaps: [any dimensions that couldn't be extracted and need user input]
- Conflicts found: [yes/no — if yes, list them]
```

Then ask: `"Does this look right? Anything missing or wrong before you take this to the next tool?"`

Never send the decompiled block without this confirmation step.
