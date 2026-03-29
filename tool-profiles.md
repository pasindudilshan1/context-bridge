# Tool Profiles — context-bridge

Target-tool-specific formatting rules. Read only the profile matching the user's target tool.

---

## Cursor / Windsurf / Copilot (Code Editor AI)

**Key behavior:** These tools work inside a codebase. They need file-path awareness, not just architectural concepts.

**Format additions:**
- Add a `FILES TO KNOW` section listing key files and what they do
- Add a `DO NOT TOUCH` section listing files/directories to leave alone
- Keep TASK section as a single, focused file-edit instruction
- Add `CURRENT FILE` if the user is about to work in a specific file

**Cursor-specific:**
- Prepend with: `// context-bridge handoff` as a comment
- Cursor reads `.cursorrules` — if the user has one, note its location in the block
- Format MEMORY Block as inline comments if injecting into a file directly

**Template addition for code editors:**
```
### Files to Know
- `[filepath]` — [what it does, one line]
- `[filepath]` — [what it does, one line]

### Do Not Touch
- `[filepath or directory]` — [reason]

### Current File
[exact file path the AI should open and work in]
```

---

## ChatGPT / GPT-4o

**Key behavior:** Strong at following structured instructions but has no project memory across sessions.

**Format additions:**
- Start the block with: `You are a senior [role]. You are continuing an existing project.`
- ChatGPT responds well to explicit role assignment — always include it
- Use numbered lists over bullets where possible (GPT follows numbered lists more reliably)
- Add `OUTPUT FORMAT` section if the user needs a specific response format

**Memory Block format for ChatGPT:**
```
CARRY FORWARD (do not contradict):
1. [key decision]
2. [key decision]
```

**ChatGPT-specific caution:** ChatGPT will sometimes "helpfully" suggest alternatives to closed decisions. Add this line to CONSTRAINTS: `Do not suggest alternative approaches to decisions already made. Build from the established foundation.`

---

## Gemini / Gemini Advanced

**Key behavior:** Strong at long-context synthesis. Handles large blocks well.

**Format additions:**
- No special role prefix needed — Gemini infers context well
- Can handle longer DECISIONS sections than other tools (up to 15 items before confusion)
- Add `RELATED CONTEXT` section if there are adjacent systems or APIs the user's project interacts with

**Gemini-specific:**
- Gemini 1.5+ handles uploaded files — note any relevant files the user should attach alongside the block
- Format code examples with language tags — Gemini renders them correctly

---

## v0 (Vercel)

**Key behavior:** UI/component generation only. Focused entirely on React + Tailwind output.

**Strip from the block:**
- Backend architecture
- Database decisions
- Auth details
- Anything not directly relevant to the UI component being built

**Keep only:**
- Design system (colors, fonts, spacing)
- Component library (shadcn, Radix, Headless UI, etc.)
- Exact component to build
- Reference component if one exists

**v0 template (condensed):**
```
## v0 Context — [Component Name]

Design System: [Tailwind config / custom colors / fonts]
Component Library: [shadcn/ui / Radix / Headless / none]
Style Rules: [any hard constraints on appearance]
Reference: [describe or link a visual reference if available]

Build: [exact component description — be visual and precise]
Do not add: [list anything to exclude — extra wrappers, unnecessary states, etc.]
```

---

## Bolt / Bolt.new

**Key behavior:** Full-stack app generation from scratch. Reads context blocks well.

**Format additions:**
- Add `ENTRY POINT` — what file/route/page Bolt should generate first
- Add `INTEGRATIONS` — list any APIs, services, or webhooks the app connects to
- Bolt works best with a `PROJECT TYPE` label at the top: Web App / API / CLI / etc.

**Bolt-specific:**
- Bolt will generate a full file tree — add `FILE STRUCTURE` if you want to constrain it
- If continuing an existing Bolt project, describe what Bolt already generated so it doesn't redo it

---

## Claude Code (Terminal)

**Key behavior:** Agentic, runs bash commands, edits files directly.

**Format additions:**
- Add `WORKING DIRECTORY` — absolute path where Claude Code should operate
- Add `RUN FIRST` — any setup commands to run before starting (e.g., `npm install`, `source .env`)
- Add `AGENT RULES` — specific behavioral rules for the agentic session

**Claude Code template addition:**
```
### Agent Rules
- Always confirm before deleting files
- Run tests after each functional change
- Commit with descriptive messages after each completed task
- Do not install new packages without asking

### Working Directory
[absolute path]

### Run First
```bash
[setup commands]
```
```

---

## GitHub Copilot (in VS Code / JetBrains)

**Key behavior:** Inline completion and chat. Reads surrounding code more than blocks.

**Format additions:**
- Keep the block short — Copilot chat has limited effective context window for blocks
- Focus the block entirely on the TASK and CONSTRAINTS
- Skip full architectural context — Copilot reads the open file for that

**Copilot-specific:**
- Best practice: paste a shortened version (STACK + TASK + CONSTRAINTS only)
- Add: `Complete this task using patterns consistent with the existing codebase.`

---

## Replit AI

**Key behavior:** Beginner-friendly, works well with explicit step-by-step tasks.

**Format additions:**
- Break TASK into explicit numbered steps (Replit AI follows step lists better than paragraphs)
- Add `EXPECTED OUTPUT` — describe what the finished feature looks like when working
- Simpler language works better here than technical jargon

---

## Perplexity

**Key behavior:** Research and synthesis, not code generation.

**Strip from the block:** All stack details, code conventions, file structure. Keep only what's needed for the research task.

**Use for:** When the user wants Perplexity to research a library, API, or approach before returning to a coding tool.

**Perplexity template:**
```
Research context: I am building [project description].
Stack: [relevant parts only]
Research task: [exact question or topic to investigate]
Return: [what format the answer should be in — comparison table, pros/cons, code example, etc.]
```

---

## Unknown Tool — Universal Format

If the target tool is not listed above, use the Universal Format defined in SKILL.md.

Always add this line to the block when using Universal Format:
`[Context generated by context-bridge. Tool-specific formatting not available for this target.]`
