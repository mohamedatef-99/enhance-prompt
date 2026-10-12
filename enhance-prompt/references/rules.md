# Enhancement rules

Rules as of: 2026-10-11 · Verified against sources: 2026-10-11
Distilled from `docs/research/prompt-engineering-guides.md` and corrected by `docs/research/rules-verification-{anthropic,openai}.md` in the skill's source repo. Bracketed keys ([A1], [O5]) are source IDs from those files. **Policy** marks a choice this skill makes that neither vendor states.

Each rule names the Prompt Kinds it applies to: **all**, **task**, **system**, **agentic**. Anything a rule adds beyond the Source Prompt's goal is an Assumption, not a Change.

**When sources disagree:** model-specific pages beat general pages ([A1] over [A9]/[A12]; [O4]/[O5]/[O7] over [O1]/[O9]), and newer beats archived.

**Generation Exceptions** live in [claude-generations.md](claude-generations.md) and [gpt-generations.md](gpt-generations.md). Read the matching file only when the Target Model names a specific model; its entries override the rules here that they name.

---

## Shared Rules: every Target Model

A bare family target (and `generic`) means current reasoning models.

- **S1 Explicit output** (all). State the format, length, structure and audience of the response. [A1] [O7, O9, O14]
- **S2 Success criteria** (all). Say what a good result looks like, or when the task is done. [A1, A3] [O2, O5]
- **S3 Context** (all). Give the background and product behaviour a newcomer would need. For Claude also give the reason behind the request, and apply the colleague test: would a smart colleague with no context understand this prompt? [A1, A6] [O5, O11]
- **S4 Delimiters** (all). Separate instructions, context, examples and input data visibly. The form depends on the target: C1, G1–G2, N1. [A1] [O1, O2]
- **S5 Role** (system, agentic; task only when it changes the answer). One or two plain sentences stating the role or perspective, placed in the system/developer prompt. Skip elaborate personas. [A1, A9] [O1, O7]
- **S6 Examples** (all). Keep examples diverse, representative and consistent with the rules, and fix any that contradict a rule. *Policy:* don't invent examples. An added format example, if one truly helps, is an Assumption. [A1, A9] [O1, O2, O12]
- **S7 Goals over procedures** (all). State the goal, constraints and success criteria rather than a hand-written procedure. Keep numbered steps where order or completeness matters. [A1] [O3, O7]
- **S8 No thinking out loud** (all). Remove instructions whose only purpose is to make a reasoning model think out loud: "think step by step", "explain your reasoning" used as a performance trick, scratchpad or reasoning sections. **Keep** a short explanation, the evidence, or "conclusion, then rationale, then caveats" when the user wants it as part of the deliverable. A brief "think it through before you answer" nudge (internal thinking) may stay. For Claude, see C6. Non-reasoning targets are handled by the Generation Exceptions. [A1, A3, A7] [O2, O7]
- **S9 Calm emphasis** (all). Keep ALWAYS/NEVER/MUST and caps only for true invariants: safety rules, required output fields, actions that must never happen, data loss. Rewrite the rest as a plain instruction with its reason: "CRITICAL: You MUST use search" → "Use search when the answer depends on recent facts." Keep any capitalisation inside an invariant the user wrote. [A1] [O5, O11, O12]
- **S10 Consistency** (all). Find contradictory instructions and resolve them toward the Source Prompt's evident intent. If the intent is unclear, ask a Clarifying Question or record an Assumption. [O5, O11, O14]
- **S11 Missing information** (all). Task/system: tell the model to ask, or to say so plainly, rather than guess, and name which ambiguities deserve a question. Agentic: look it up with tools first, make routine judgment calls and state the assumption, and ask only when different readings would lead to materially different work. [A1, A4, A5, A9] [O5, O6, O12]
- **S12 Grounding** (all with supplied documents or tools). Base claims on the supplied material or tool results. Label inferences separately from supported facts, and say when something is unsupported. Missing evidence is not automatically a "no". [A1, A6, A9] [O5, O7, O12]
- **S13 Preservation** (all). Keep the user's content, facts, variables, placeholders and constants (rubrics, policies, names) exactly. For edit or summary tasks, list what the output must preserve. [A12; A5 by extension] [O5, O9]
- **S14 Lean** (all). Remove repetition, filler and legacy scaffolding (anti-laziness pleas, repeated reminders, process steps the model already does).
  - **Keep:** the outcome; success and stop criteria; safety, business, evidence and permission constraints; context-dependent tool routing; output shape; validation requirements; examples or style rules that encode a product requirement.
  - When you can't tell whether something changes behaviour, keep it and mention the doubt.
  - Blocks added by a Generation Exception are not repetition.
  - OpenAI reports leaner prompts scored about 10–15% higher on internal coding-agent evals (directional). [A1, A6, A9] [O5, O6]
- **S15 Scope** (agentic, and task prompts involving code). Ask for the requested change only: no unrequested features, refactors or files. Add tests where the task asks or where the repo already keeps tests for that kind of change, and skip them for reversible, low-impact changes. Ask for other worthwhile ideas to be mentioned at the end, not made. [A5, A7] [O4, O5]
- **S16 Autonomy boundary** (agentic). Say what the model may do freely (local, reversible, read-only) and what needs confirmation (destructive, external writes, purchases, a material expansion of scope). State it once. [A1, A6] [O4, O5]
- **S17 Done and stop rules** (agentic). Define what "done" means and when to stop and report. Add a strong "keep going until the task is fully done" block only for unattended or asynchronous agents; in human-in-the-loop work it suppresses useful questions. If the Source Prompt already asks for persistence, keep it (pair it with the stop conditions). [A3, A5] [O5, O7, O11, O12]
- **S18 Investigate first** (agentic). Read files or look things up before making claims about them. [A1] [O5, O12]
- **S19 Parallel tools** (agentic). Run independent tool calls in parallel and dependent ones in sequence, and synthesise the results before acting. [A1] [O5]
- **S20 Real verification** (agentic). Ask for the most relevant real check (tests, build, render), sized to the change. If it can't be run, say why and do the next best check. Generation Exceptions remove this for models that over-verify. [A7, A8] [O4, O5, O7]
- **S21 Progress updates** (agentic, long or human-in-the-loop tasks). Ask for a short preamble before the first tool call, then sparse, outcome-based updates at phase changes, without narrating routine tool calls. Generation Exceptions adjust this. [A3, A5] [O5, O7]

---

## Generic target: settling where the vendors disagree

- **N1 Delimiters.** Use Markdown `#` headings for sections and XML tags around content blocks (documents, data, examples).
- **N2 Long documents.** Put a one-line instruction above the document and the actual question after it.
- **N3 Date.** Don't add the current date.
- **N4 Examples.** Add none. Keep and fix the ones the Source Prompt already has (S6).

---

## Claude Vendor Rules (`claude`)

A bare `claude` target means the current models: Fable/Mythos 5.x, Opus 5/5.5, Sonnet 5/5.5, Haiku 5.5.

- **C1 XML structure** (all). When the prompt mixes instructions, context, examples and inputs, wrap each in descriptive, consistent tags (`<context>`, `<instructions>`, `<output_format>`). A short single-purpose prompt needs at most one tag around its input. Nest multiple documents as `<documents><document index="1"><source>…</source><document_content>…</document_content></document></documents>`. [A1, A9]
- **C2 Long context** (all with documents of roughly 20k+ tokens). Put documents first and the question or instructions last (up to 30% better). For document Q&A, ask for relevant quotes in `<quotes>` before the answer. *Policy:* for a slot of unknown size, documents-first is a safe default. [A1]
- **C3 Positive formatting** (all). For output format and style, say what to do ("write flowing prose paragraphs") rather than what to avoid, and make the prompt's own style match the output style you want. Keep named prohibitions, scope limits and safety boundaries as the user wrote them. [A1, A9]
- **C4 Examples** (all). Wrap examples in `<example>` tags, or several in `<examples>`. Start with one; A1 says 3–5 give the best results. [A1, A9]
- **C5 Prefill** (all). On Claude 4.6+ and all 5.x models, remove a last-assistant-turn prefill (it returns a 400 error). Replace it with:
  - a direct instruction ("Respond directly, without a preamble");
  - output inside named XML tags;
  - Structured Outputs or tool calling (Settings Note);
  - for continuations, the continuation text moved into the user turn.

  On older models it still works (see claude-generations.md). [A1]
- **C6 No reasoning extraction** (all). On Fable/Mythos 5.x, Opus 5/5.5 and Sonnet 5.5 (and a bare `claude` target), a request to put reasoning into the reply may be refused as `reasoning_extraction`.
  - **Triggers to remove:** `<thinking>`, `<reasoning>` or scratchpad sections; a running log of reasoning; `reasoning`, `thinking` or `trace` fields in JSON output or in tool input schemas; a "justification before the score" field.
  - **Replace with:** a short explanation of the answer, the evidence behind it, or a summary of the actions taken. Depth goes to effort (Settings Note).

  [A1, A3, A4, R]
- **C7 Act vs suggest** (task, agentic). Match the verb to the intent: "Change this function" produces edits; "Can you suggest changes" produces suggestions only. [A1]
- **C8 Self-check line** (task, agentic). "Before you finish, verify your answer against [criteria]." It works especially well for coding and math. Removed for opus-5. [A1]
- **C9 Explicit ambition** (task: creative, frontend, open-ended builds). If the user wants an expansive result, say so explicitly ("go beyond the basics") rather than relying on the model to infer it. [A1]
- **C10 Agentic blocks** (agentic). Add:
  - a reversibility block: take local, reversible actions freely; ask before actions that are hard to reverse, affect shared systems, or could be destructive;
  - guidance on when subagents are and aren't warranted;
  - a short summary after tool use, if visibility matters;
  - for coding, "Tests verify correctness; they don't define the solution", with no hard-coding to pass tests.

  [A1]
- **C11 No "don't think" rules** (all). Remove instructions telling the model not to think or reason; they increase reasoning leaking into the reply. Lower effort instead (Settings Note). [A3, A4, A7]

---

## GPT Vendor Rules (`gpt`)

A bare `gpt` target means the current reasoning models (GPT-5.5 / 5.6 / GPT-6 family).

- **G1 Markdown skeleton** (all).
  - **task:** first line is a concise task instruction (no heading), then details, optional `# Steps` (S7), `# Output Format`, optional `# Examples`, optional `# Notes` (edge cases). [O9]
  - **system / agentic, complex prompts:** a 1–2 sentence role line, optional personality, goal, success criteria, constraints, optional tools, output, stop rules. Keep each short. A short system prompt stays short rather than being padded into every section. [O5, O7]
- **G2 XML for content** (all). Wrap documents, data and examples in XML tags, using attributes for metadata (`<doc id="1" source="…">`). For many documents in long context, prefer XML or `ID: 1 | TITLE: … | CONTENT: …` over JSON. Skip XML wrappers for documents that are themselves XML-heavy. Leave JSON that *is* the user's input data or a tool payload as it is. [O1, O12]
- **G3 Developer message order** (system, agentic). Put stable identity and rules first and variable context last, which also helps caching. [O1, O7]
- **G4 Outcome first** (all). Describe the destination and constraints and let the model choose the path. For judgment calls (when to search, ask or iterate), use decision rules instead of absolutes or keyword maps. [O5, O7]
- **G5 Examples** (all). For reasoning-effort targets, start zero-shot. Remove only duplicate examples or ones that contradict the rules, and keep those that encode a product requirement. Pair examples with ids (`<input id="ex1">` / `<output id="ex1">`). [O1, O2, O5, O6]
- **G6 Formatting** (all). Always say which format you want. Defaults differ by model: GPT-5 (2025) and the o-series give plain text, GPT-6 Astra gives heavy lists, tables and Markdown, and GPT-5.5 is highly steerable. [O2, O4, O7, O11]
- **G7 Length** (all). Say what a short answer must keep and what to drop first. Recommend `text.verbosity` for the default (Settings Note). On GPT-5.6, "Be concise" can over-shorten, so flag it as an Assumption rather than deleting it. [O5, O7]
- **G8 Date** (all). For GPT-5.5 and later, drop the current date: the model knows the UTC date. Keep a date that serves a business timezone, a policy-effective date, the user's local date, or any other non-UTC reference. For 5.6 and GPT-6 this is inherited, not stated, so record it as an Assumption. [O7]
- **G9 Facts vs flourish** (task: creative, marketing). Keep source-backed facts separate from creative wording. Use placeholders or labelled assumptions instead of invented names, metrics or data claims. [O5, O7]
- **G10 Agentic policy** (agentic).
  - State the autonomy policy once; repeating "ask first" causes needless approval requests.
  - Move per-tool how-to into the tool descriptions. Keep cross-tool policy and context-dependent routing in the prompt.
  - Set retrieval budgets ("start with one broad search; search again only when…") and allow one or two fallbacks on empty results.
  - For multi-step or tool tasks, ask for a 1–2 sentence preamble, with a `phase` Settings Note.

  [O5, O7]
- **G11 Tone as choices** (system, task). Replace labels like "friendly" or "empathetic" with the concrete writing choices that define the tone. Keep personality and collaboration style short. [O5]
- **G12 Language rule** (system). Replace a blanket "always respond in the user's language" with the intended output language and when it should change, unless the blanket rule truly is the product requirement. [O5]
- **G13 Layers of work** (agentic, long tasks). Name which layer the task is in (research, design, implementation, review, external coordination) so the model doesn't silently move from one to another. [O5]

---

## Settings Notes

Add one when the fix belongs in an API setting rather than prompt text.

| Symptom in the Source Prompt | Settings Note |
|---|---|
| "think harder", "be thorough", "think less", depth pleas | Claude: adjust `effort`. GPT: `reasoning.effort` (Responses) / `reasoning_effort` (Chat Completions). Higher is not automatically better. GPT-6 Astra and 6.1 Sol reject `none`, and 6.1 Sol also rejects `minimal`, so use `low` there. [A3, A7] [O3, O4, O5, O7] |
| Best-of-N, "generate several candidates", self-consistency pleas (GPT-5.6 / GPT-6) | `reasoning.mode: "pro"`. [O6] |
| A long JSON schema in prose, or "output ONLY valid JSON" | Structured Outputs. For Claude classification, a tool with an enum also works. A short "JSON only" line may stay. [A1] [O7] |
| Global length pleas for a GPT target | `text.verbosity` (Responses) / `verbosity` (Chat Completions); `low` is a good start for concise output. [O5, O7, O11] |
| Prefill text for a Claude 4.6+/5.x target | Structured Outputs, tool calling, or the C5 instruction. [A1] |
| Tool descriptions pasted into a GPT prompt | Pass tools through the `tools` field; usage examples may stay under `# Examples`. [O12] |
| Preambles prompted for GPT-5.5+ or gpt-5.3-codex | Round-trip `phase` (`commentary` / `final_answer`) on assistant items, or use `previous_response_id`; otherwise preambles can be treated as final answers. [O3, O7, O15] |
