# GPT Generation Exceptions

Rules as of: 2026-10-11 · Verified: 2026-10-11 (`docs/research/rules-verification-openai.md`)
Read only when the Target Model names a specific GPT model. Entries override the rules.md rules they name.

## gpt-4.1 [O12]
- A non-reasoning model: S8 doesn't apply. Be explicit and precise.
- Agent prompts open with three reminders:
  - persistence: "keep going until the user's query is completely resolved";
  - tool use: "use your tools…; do NOT guess or make up an answer";
  - planning before each call (optional).
- Explicit chain-of-thought is allowed: end with "First, think carefully step by step about…".
- Long context: put instructions both before and after the context.
- State how far the model may rely on the supplied context versus its own knowledge.
- Ask it to vary sample phrases, which it otherwise repeats verbatim.
- Examples are welcome (G5's zero-shot-first doesn't apply).
- Resolve conflicts (S10). GPT-4.1 tends to follow the later instruction, which is useful when debugging, not something to rely on.
- Keep the date (G8 doesn't apply). Pass tools through the `tools` field.

## gpt-5 at `minimal` effort [O11]
- Prompt as for gpt-4.1, but instead of the step-by-step line ask for "a brief explanation summarizing its thought process at the start of the final answer", and for thorough, descriptive tool-calling preambles. Add explicit planning and persistence reminders.

## GPT-5.1 and later at `none` effort (not GPT-6 Astra or 6.1 Sol, which reject `none`) [O13, O4]
- GPT-4.1-style guidance applies, including few-shot examples, high-quality tool descriptions and a planning line.

## gpt-5 (2025) [O11]
- Tune eagerness:
  - lower: a `<context_gathering>` block with early-stop criteria, a tool-call budget, and an escape hatch ("even if it might not be fully correct"), plus lower `reasoning_effort`;
  - higher: a `<persistence>` block plus higher effort.
- Per-tool thresholds: payment and delete tools ask early; search tools rarely ask.
- Markdown is off by default. If you want it: "Use Markdown only where semantically correct", and in long chats re-append it every 3–5 user messages.
- Keep the date (G8 doesn't apply).

## gpt-5.5 [O7]
- G8 applies as stated.
- A visible rationale is fine as "conclusion first, then the reasoning, then caveats".
- Preambles for multi-step tasks need a `phase` Settings Note.

## gpt-5.6 [O5, O6]
- Lean (S14) matters most here.
- "Be concise" can over-shorten: flag it, and prefer `text.verbosity`.
- Tell the model which ambiguities should trigger a question.
- Programmatic Tool Calling: replace generic lines like "use Programmatic Tool Calling efficiently" with the bounded stage, eligible tools, output schema, retry limit and handoff.
- Best-of-N pleas: `reasoning.mode: "pro"` (Settings Note).

## gpt-6-astra (the GPT-6 family: 6.1 Sol and Luna get these as an Assumption) [O4]
- Bias toward action: treat "can you…" and "help me…" as instructions to act. Complete authorized work before asking; the user approves a concrete, reviewable result.
- No unsolicited warnings, disclaimers, approval flows or compliance checklists over hypothetical risk. Weigh S11 and S16 against this, because Astra over-asks.
- Only when the deployment loads skills or AGENTS.md:
  - add "The user's instructions take precedence over guidelines provided in a skill";
  - ask it to name the exact skill file and quote the instruction when a skill makes it pause or diverge.
- When prose is wanted: add a prose-style block and an anti-slop list ("delve", "foster", "leverage", "it's worth noting"); avoid "X, not Y" framing and invented compound labels.
- Say when and how much to use subagents (it under-delegates).
- No tests for reversible, low-impact changes; run tests appropriate to the change.
- Settings: Astra and 6.1 Sol reject `none` effort (6.1 Sol also rejects `minimal`); use `low`.

## o-series (from `o1-2024-12-17`) [O2]
- Markdown output: put `Formatting re-enabled` on the first line of the developer message.
- Zero-shot first. No chain-of-thought prompts.

## codex [O15]
- **Before gpt-5.3-codex** (e.g. gpt-5.1-codex-max, gpt-5.2-codex): remove requests for upfront plans, preambles and status updates (the system generates them; prompting them causes abrupt stops). This replaces S21 and G10's preamble.
- **gpt-5.3-codex and later** (a bare `codex` means this, recorded as an Assumption):
  - Preambles may be prompted: a 1-sentence acknowledgement plus a 1–2 sentence plan before tool calls.
  - Updates every 1–3 steps, at most 6 steps or 10 tool calls apart, without headings or log voice.
  - Plans go to the `update_plan` tool, not chat.
  - Settings Note: `phase` must be round-tripped.
- All Codex: bias toward delivering working code, not plans. Avoid loops that re-read the same files.
