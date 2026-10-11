# Prompt engineering guidance: Anthropic and OpenAI (primary sources)

Research date: 2026-10-11. Purpose: actionable rules for a Claude Code skill that rewrites a rough prompt into an enhanced one.
Citation keys like [A1] / [O5] point to section 1. "(model: X)" means the source scopes the rule to that model generation.
Items marked **UNCERTAIN** could not be fully verified against a current primary page.

---

## 1. Sources

All fetched 2026-10-11.

Note on URLs: `docs.claude.com` / `docs.anthropic.com` now redirect to `platform.claude.com/docs`. The old per-technique pages (`use-xml-tags`, `prompt-improver`, `prompt-generator`, `extended-thinking-tips`, `docs.anthropic.com/en/docs/prompt-generator`) all redirect to [A1]; Anthropic has consolidated every technique (clarity, examples, XML, roles, thinking, chaining, long context, prefill) into that one page plus per-model pages. `platform.openai.com/docs/*` returns 301 to `developers.openai.com/api/docs/*`; the OpenAI Cookbook is served at `developers.openai.com/cookbook` (raw Markdown via `.md` suffix, indexed in `developers.openai.com/api/docs/llms.txt` and `/cookbook/llms.txt`).

### Anthropic

| Key | Title | URL |
|---|---|---|
| A1 | Prompting best practices (covers Fable 5.1, Mythos 5.1, Fable 5, Mythos 5, Opus 5.5, Opus 5, Opus 4.8/4.7/4.6, Sonnet 5.5/5/4.6, Haiku 5.5/4.5) | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices |
| A2 | Prompt engineering overview | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview |
| A3 | Prompting Claude Opus 5.5 | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5 |
| A4 | Prompting Claude Opus 5 | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5 |
| A5 | Prompting Claude Fable 5.1 (also Mythos 5.1) | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1 |
| A6 | Prompting Claude Fable 5 (also Mythos 5) | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5 |
| A7 | Prompting Claude Sonnet 5.5 | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-sonnet-5-5 |
| A8 | Prompting Claude Haiku 5.5 | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-haiku-5-5 |
| A9 | Best practices for prompt engineering for 2026 (Claude blog, dated 2025-11-10) | https://claude.com/blog/best-practices-for-prompt-engineering |
| A10 | Improve your prompts in the developer console (Claude blog, 2024-10-14; redirected from anthropic.com/news/prompt-improver) | https://claude.com/blog/prompt-improver |
| A11 | Generate better prompts in the developer console (Claude blog, 2024-05-20; redirected from anthropic.com/news/prompt-generator) | https://claude.com/blog/prompt-generator |
| A12 | Metaprompt (Claude Cookbook) | https://platform.claude.com/cookbook/misc-metaprompt |

Not fetched (listed in A1 but not read): Prompting Claude Sonnet 5, Prompting Claude Opus 4.8, the Effort and Thinking reference pages.

### OpenAI

| Key | Title | URL |
|---|---|---|
| O1 | Prompt engineering | https://developers.openai.com/api/docs/guides/prompt-engineering |
| O2 | Reasoning best practices (o-series vs GPT models) | https://developers.openai.com/api/docs/guides/reasoning-best-practices |
| O3 | Reasoning models ("Advice on prompting" section) | https://developers.openai.com/api/docs/guides/reasoning |
| O4 | Using GPT-6 (GPT-6 Astra, GPT-6.1 Sol, GPT-6 Luna; current "latest model" guide) | https://developers.openai.com/api/docs/guides/latest-model/gpt-6-astra |
| O5 | Prompting guidance for GPT-5.6 Sol | https://developers.openai.com/api/docs/guides/prompt-guidance-gpt-5p6 |
| O6 | Using GPT-5.6 (Prompting best practices section) | https://developers.openai.com/api/docs/guides/latest-model/gpt-5.6 |
| O7 | Using GPT-5.5 | https://developers.openai.com/api/docs/guides/latest-model/gpt-5.5 |
| O8 | Prompt optimizer | https://developers.openai.com/api/docs/guides/prompt-optimizer |
| O9 | Prompt generation (Playground meta-prompts) | https://developers.openai.com/api/docs/guides/prompt-generation |
| O10 | Prompting (overview) | https://developers.openai.com/api/docs/guides/prompting |
| O11 | GPT-5 prompting guide (Cookbook, marked archived) | https://developers.openai.com/cookbook/examples/gpt-5/gpt-5_prompting_guide |
| O12 | GPT-4.1 Prompting Guide (Cookbook, marked archived) | https://developers.openai.com/cookbook/examples/gpt4-1_prompting_guide |
| O13 | GPT-5.1 prompting guide (Cookbook; metaprompting section) | https://developers.openai.com/cookbook/examples/gpt-5/gpt-5-1_prompting_guide |
| O14 | GPT-5 Prompt Migration and Improvement Using the New Optimizer (Cookbook) | https://developers.openai.com/cookbook/examples/gpt-5/prompt-optimization-cookbook |
| O15 | Codex Prompting Guide (Cookbook) | https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide |

Current model landscape per these sources: Anthropic's newest are Claude Fable 5.1 / Mythos 5.1, Opus 5.5, Sonnet 5.5, Haiku 5.5 (thinking always on or default-on, `effort` parameter). OpenAI's newest are GPT-6 Astra / GPT-6.1 Sol / GPT-6 Luna, then the GPT-5.6 family; every GPT-5+ model is a reasoning model with `reasoning.effort` (some accept `none`). "Non-reasoning" guidance in OpenAI docs now means GPT-4.1 and GPT-5-class models run at `minimal`/`none` effort [O7, O11].

---

## 2. Shared principles (both vendors agree)

1. **State exactly what output you want: format, length, audience, constraints.** [A1 "Be clear and direct"; A9] [O1 "GPT models... benefit from more explicit instructions"; O2 "Provide specific guidelines"; O7]
2. **Define success criteria / the end goal before tuning the prompt.** [A2 "clear definition of the success criteria"; A1 research section] [O2 "Be very specific about your end goal"; O5, O7 "Success means:" blocks]
3. **Explain the purpose and context behind the task, not just the instruction.** [A1 "Add context to improve performance"; A6 "Give the reason, not only the request"] [O5 "define the outcome, important constraints, available evidence"; O11 Cursor: adding product-behavior context reduced unnecessary check-ins]
4. **Separate instructions, context, examples, and inputs with explicit delimiters.** [A1 XML tags] [O1 Markdown headers + XML tags; O2 "Use delimiters for clarity"] (they differ on which delimiter; see section 5)
5. **Put the role/identity and standing rules in the system (Anthropic) / developer (OpenAI) message.** [A1 "Give Claude a role"] [O1 developer message starts with "Identity"; O7/O5 structure starts with "Role"]
6. **Make examples diverse, representative, and consistent with the written rules.** [A1 "Relevant... Diverse"; A9 examples are copied closely] [O1 "diverse range of possible inputs"; O2 examples must "align very closely with your prompt instructions"; O12 behaviors shown in examples must also be cited in rules]
7. **For reasoning/thinking models, give goals and constraints rather than hand-written step lists.** [A1 "Prefer general instructions over prescriptive steps"] [O1 reasoning model = "senior co-worker"; O7 "Reduce or remove detailed step-by-step process guidance"; O3]
8. **Do not add "think step by step" / "explain your reasoning" to models that already reason internally.** [A1, A3, A4 (reasoning-in-response may be refused)] [O2 "Avoid chain-of-thought prompts"]
9. **Control thinking depth with the API effort setting, not prompt wording; higher is not automatically better.** [A3 "lower the effort level first"; A7, A8 prompting "think less" does not reliably work] [O7 "Higher reasoning effort isn't automatically better"; O5 check prompt for missing criteria before raising effort]
10. **Reserve ALL CAPS / ALWAYS / NEVER / MUST for true invariants; soften legacy emphasis.** [A1 replace "CRITICAL: You MUST use this tool" with "Use this tool when..."] [O12 all-caps "generally not necessary"; O7/O5 "Avoid unnecessary absolute rules"; O11 Cursor's "Be THOROUGH" was counterproductive]
11. **Use structured-output features instead of describing schemas or forcing formats in prose.** [A1 prefill migration: use Structured Outputs; tools with enum for classification] [O7 "Remove output schema definitions from the prompt... Use Structured Outputs"]
12. **Spell out the autonomy boundary: what the model may do without asking and what needs confirmation.** [A1 `<default_to_action>` / `<do_not_act_before_instructions>`, reversibility block; A5, A6] [O5 compact autonomy policy; O4; O11 safe vs unsafe action thresholds]
13. **For agents, instruct persistence until the task is fully done, with explicit stop conditions.** [A5 "Finish the whole task"; A6 checkpoint instruction] [O11/O12 `<persistence>` "keep going until the user's query is completely resolved"; O5/O7 "Stop rules"]
14. **Give an explicit escape hatch for missing information instead of forcing an answer or a tool call.** [A9 "say so rather than speculating"; A1 "Never use placeholders or guess missing parameters"] [O12 "if you don't have enough information to call the tool, ask the user"; O5 "ask for the smallest missing field"]
15. **Require grounding: read/look up before answering; never guess file contents or facts.** [A1 `<investigate_before_answering>`; A1 long-context quote-first] [O12 "use your tools... do NOT guess or make up an answer"; O5/O7 citation and retrieval rules]
16. **Run independent tool calls in parallel; keep dependent ones sequential.** [A1 `<use_parallel_tool_calls>`] [O5 "When several reads are independent, parallelize them"]
17. **Ask for real verification of work (tests, builds, renders), calibrated to the model.** [A7, A8 verification paragraph] [O5/O7 "run the most relevant validation available"] (both note some models over-verify; see sections 3-4)
18. **Constrain scope: no unrequested features, refactors, files, or tests.** [A1 "Avoid over-engineering"; A5, A7] [O4 "Do not write tests for reversible, low-impact changes"; O5 "do not add extra features"]
19. **Specify cadence and content of user-facing progress updates in long agentic tasks.** [A3, A4, A5] [O5 "short visible preamble... sparse outcome-based updates"; O7, O11 tool preambles]
20. **When editing/summarizing, state what must be preserved.** [A5 compaction "Be sure to preserve: (1)...(6)"] [O5/O7 "Preserve the requested artifact, length, structure, genre, and factual claims first"]
21. **Strip legacy scaffolding when moving to a newer model; re-measure.** [A1 "Tune anti-laziness prompting"; A6 "Skills developed for prior models are often too prescriptive"] [O5 "Remove obsolete scaffolding"; O7 "Avoid carrying over every instruction from an older prompt stack"]
22. **Iterate empirically: change one thing at a time and test against evals.** [A2; A9 "Add only one technique at a time"] [O5 "Do not rewrite a working prompt stack all at once"; O1 pin snapshots + evals]
23. **Use the model itself to critique and improve prompts (metaprompting).** [A1 ask Claude to evaluate/generate examples; A12 metaprompt] [O11, O13, O15 metaprompting recipes]

---

## 3. Anthropic / Claude-specific techniques

### 3a. All current Claude models [A1 unless noted]
- **Apply the colleague test:** if a colleague with minimal context would be confused by the prompt, Claude will be too.
- **Request "above and beyond" explicitly** when wanted (e.g., "Include as many relevant features and interactions as possible. Go beyond the basics"); request animations/interactivity explicitly.
- **Use numbered lists/bullets for steps** when order or completeness matters.
- **Phrase instructions positively:** say what to do ("smoothly flowing prose paragraphs"), not what to avoid ("no markdown"). [A1, A9]
- **Use XML format indicators** for output sections (e.g., "write the prose in `<smoothly_flowing_prose_paragraphs>` tags").
- **Match prompt style to desired output style** (less markdown in the prompt yields less markdown in the reply).
- **XML tags:** consistent, descriptive tag names; nest for hierarchy (`<documents>` > `<document index="n">` > `<source>`, `<document_content>`).
- **Examples:** 3-5, wrapped in `<example>` (multiple in `<examples>`); relevant, diverse, structured. [A1] (A9 suggests starting with one and adding more only if needed.)
- **Role:** even one sentence in the system prompt helps (A1). A9 adds: "use sparingly", avoid over-constrained personas; stating the perspective directly often works better.
- **Long context (20k+ tokens):** put documents at the top, query/instructions at the end (up to 30% better); wrap each document with source metadata; ask for relevant quotes in `<quotes>` first, then answer.
- **Action vs. suggestion:** "Change this function" gets edits; "Can you suggest changes" gets suggestions only.
- **Ask for a summary after tool use** if you want visibility (latest models skip it by default).
- **Plain text math:** latest models default to LaTeX; ask explicitly for plain text if needed.
- **No preamble:** "Respond directly without preamble. Do not start with phrases like 'Here is...'" (replacement for prefill).
- **Prefill is gone (Claude 4.6+ and Mythos Preview; 400 error):** use Structured Outputs, tool enums, direct instructions, or put continuation text in the user turn ("Your previous response was interrupted and ended with `...`. Continue").
- **Self-check line:** "Before you finish, verify your answer against [test criteria]." (Exception: Claude Opus 5, see below.)
- **Manual CoT only when thinking is off:** ask to reason first and put the answer in `<answer>` tags. (Exception: on Fable 5.1, Fable 5, Opus 5.5, Opus 5, Sonnet 5.5, rely on thinking; asking to write reasoning out, e.g. in `<thinking>` tags, may be declined as `reasoning_extraction`.) [A1, A3, A4, A6, A7]
- **Multishot examples shape thinking:** present each as problem, method, expected answer.
- **Steer thinking frequency in prompt if needed:** "Thinking adds latency... When in doubt, respond directly."
- **Agentic coding:** clean up temp files; write general solutions, not test-specific hardcoding ("Tests are there to verify correctness, not to define the solution"); flag infeasible tasks or wrong tests.
- **Reversibility block:** take local reversible actions freely; ask before destructive, hard-to-reverse, or externally visible actions; never bypass safety checks.
- **Research tasks:** give success criteria; ask for cross-source verification; competing hypotheses, confidence tracking, notes file.
- **Subagents:** say when subagents are warranted (parallel, isolated, independent) and when to work directly.
- **Prompt chaining** still useful for inspectable pipelines; standard pattern is draft -> review against criteria -> refine.
- **Multi-window agent workflows:** first window sets up framework (tests, `init.sh`); track tests in `tests.json`, progress in `progress.txt`, state in git; tell the agent how to resume ("Review progress.txt, tests.json, and the git logs").
- **Model identity line** if the app needs it ("The assistant is Claude, created by Anthropic. The current model is ...").

### 3b. Generation-specific
- **Opus 4.5 / 4.6:** more responsive to system prompts; dial back aggressive language to avoid overtriggering; overengineering tendency -> add minimal-scope block; strong `<frontend_aesthetics>` block for distinctive UI. Opus 4.6: more upfront exploration; replace "If in doubt, use [tool]" with targeted conditions; may take risky actions without a confirmation rule; overuses subagents. [A1]
- **Opus 4.5 with thinking disabled:** avoid the word "think"; use "consider", "evaluate", "reason through". [A1]
- **Sonnet 5, Sonnet 4.6, Sonnet 4.5, Haiku 4.5 (context awareness):** tell the model context will be compacted so it doesn't stop early. [A1]
- **Claude 4.7+:** `budget_tokens` returns 400; use adaptive thinking + `effort`. [A1]
- **Opus 5:** prompt explicitly for conciseness (effort doesn't shorten visible output); add a short reminder near the end of long system prompts; specify narration cadence ("Before your first tool call, say in one sentence..."); add document-length calibration; **remove** verification and "double-check" instructions (causes over-verification); add scope-constraint paragraph; damp subagent spawning; limit correction narration; in review prompts, ask to report everything and filter later rather than "only high severity". [A4]
- **Opus 5.5:** start effort at `medium`; remove "think carefully before answering" from chat system prompts; optional "treat earlier answers as settled" line; mark user-pasted text with `<pasted_content id="...">` plus a system note; for multi-app agents add "explore broadly... before taking any action"; give time budgets in multi-agent harnesses; for frontend, name specific patterns to avoid (generic "avoid an AI look" just swaps defaults); unattended runs: name the kinds of early stop to avoid. [A3]
- **Fable 5 / Mythos 5:** brief instructions suffice (no need to enumerate behaviors); "Lead with the outcome" + be selective rather than compressed; ground progress claims in tool results; state boundaries (assessment vs. fix); give the reason behind the request; don't show token countdowns; final-summary readability addendum; send-to-user tool needs an explicit instruction to be used; re-audit older skills as too prescriptive. [A6]
- **Fable 5.1 / Mythos 5.1:** writes fewer progress updates -> ask for them and delete "hold findings for the final response" lines; formats less -> remove anti-markdown blocks; tends toward dense/mannered prose -> "Please remove all mannered prose"; finish-the-whole-task and "Delivering work" scope blocks; explicit compaction-summary preservation list; scope tests/changes; search unfamiliar or fast-moving names as written; prefer surgical edits; for long deliverables at `xhigh`/`max`, tell it not to draft the full output in thinking; for quoting, give one complete example plus rationale. [A5]
- **Sonnet 5.5:** "keep working until done... don't add features... not asked for" block; for open-ended asks: "When the user asks for ideas, options or a plan, give them that and stop"; for JSON on multi-step reasoning tasks add "Think the problem through before you answer." (with adaptive thinking); remove "minimize tool calls" language and add a "use search for specifics that may have changed" line; verification paragraph at low effort. [A7]
- **Haiku 5.5:** give today's date with search tools plus a "training data ends well before today... search for those" line; avoid blanket "always search" rules; chatbot adherence line ("rules in this system prompt hold for the whole conversation..."); early-stopping and verification paragraphs at low effort. [A8]

---

## 4. OpenAI / GPT-specific techniques

### 4a. All models [O1, O10]
- **Use the role hierarchy:** `developer` (or `instructions` param) outranks `user`; treat developer = function definition, user = arguments.
- **Developer message order:** Identity -> Instructions -> Examples -> Context (context near the end because it varies per request).
- **Markdown headers/lists for sections and hierarchy; XML tags for content boundaries; XML attributes for metadata** the instructions can reference.
- **Put few-shot examples in the developer message**, using paired tags (e.g., `<product_review id="example-1">` / `<assistant_response id="example-1">`).
- **Order for caching:** stable, reused content first; dynamic content last. [O1, O7]
- **Pin model snapshots and build evals**; prompts should live in versioned code (reusable prompt objects shut down 2026-11-30). [O1, O10]
- O10: put tone/role in the system message, task details and examples in user messages; keep few-shot blocks compact (YAML-style or bulleted).

### 4b. Reasoning models (o-series; GPT-5.x/GPT-6 at non-`none` effort) [O2, O3]
- Use developer messages; keep prompts simple and direct.
- No "think step by step" / "explain your reasoning".
- Use delimiters (markdown, XML, section titles).
- Try zero-shot first; add few-shot only if needed, aligned exactly with instructions.
- State constraints explicitly (e.g., "budget under $500"); define success criteria and tell it to keep iterating until met.
- o-series API output avoids Markdown by default; put `Formatting re-enabled` on the first line of the developer message to turn it on. [O2] (GPT-5 API also defaults to no Markdown [O11]; GPT-6 Astra instead tends to heavy Markdown [O4].)
- Give task, constraints, and output contract; define "done" and how to verify; treat effort as a tuning knob, not the main quality lever. [O3]

### 4c. Non-reasoning GPT models (GPT-4.1) and GPT-5 at `minimal` effort [O12, O11]
- Treat as a "junior coworker": explicit, precise instructions. [O1]
- Start every agent prompt with three reminders: persistence, tool-calling ("do NOT guess"), optional planning ("plan extensively before each function call, and reflect") (~20% SWE-bench gain; planning +4%). [O12]
- Induce CoT explicitly: end prompt with "First, think carefully step by step about...", then codify strategies that work into a "Reasoning Strategy" section. [O12]
- Long context: place instructions at both the beginning and end of the context; if only once, above beats below. [O12]
- Specify context reliance: "Only use the documents in the provided External Context..." vs. allow internal knowledge. [O12]
- Conflicts: GPT-4.1 tends to follow the instruction closer to the end of the prompt. [O12]
- Pass tools via the `tools` field, not pasted into the prompt; put tool usage examples in an `# Examples` section. [O12]
- Tell the model to vary sample phrases (it repeats them verbatim otherwise). [O12]
- Suggested skeleton: `# Role and Objective / # Instructions / ## Sub-categories / # Reasoning Steps / # Output Format / # Examples / # Context / # Final instructions and prompt to think step by step`. [O12]
- Delimiters: start with Markdown; XML also good; JSON poor for document lists; `ID: 1 | TITLE: ... | CONTENT: ...` works. [O12]
- GPT-5 `minimal`: prompt like GPT-4.1; ask for a brief explanation summary at the start of the final answer; ask for descriptive tool preambles; add explicit planning and persistence reminders. [O11]

### 4d. GPT-5 family (2025 guides, still referenced) [O11, O13]
- Tune agentic eagerness: lower via effort + `<context_gathering>` (early-stop criteria, tool-call budgets, escape hatch "even if it might not be fully correct"); raise via `<persistence>` ("Do not ask the human to confirm or clarify assumptions").
- Set per-tool uncertainty thresholds (e.g., payment/delete tools low threshold for asking; search tools high).
- Steer tool preambles (rephrase goal, outline plan, narrate steps, summarize distinctly).
- Contradictory instructions are "more damaging" than for other models (it spends reasoning reconciling them); audit and resolve conflicts.
- Use `verbosity` API param globally, override in natural language for specific contexts (Cursor: low verbosity, but "Use high verbosity for writing code").
- Markdown adherence can decay in long chats; re-append the Markdown instruction every 3-5 user messages.
- Structured XML specs like `<[instruction]_spec>` improved adherence (Cursor).
- One task per agent turn performs best for separable tasks.

### 4e. GPT-5.5 / GPT-5.6 [O7, O5, O6]
- **Outcome-first prompts:** describe the destination, success criteria, constraints, evidence; let the model choose the path.
- **Leaner prompts:** remove repeated rules, examples that don't change behavior, irrelevant tools (internal coding-agent evals: +10-15% score, -41-66% tokens). Keep outcome, success/stop criteria, safety/permission constraints, context-dependent tool routing, output shape. [O5, O6]
- **Decision rules over absolutes** for judgment calls (when to search/ask/iterate). Preserve explicit user values; avoid keyword maps and universal defaults. [O5]
- **Stopping conditions + retrieval budgets** ("start with one broad search... search again only when..."). [O5, O7]
- **Personality vs. collaboration style:** define both, keep both short; describe concrete writing choices instead of labels like "friendly". [O5, O7]
- **Length:** use `text.verbosity` for the default; in-prompt, say what a short answer must keep and what to drop first. GPT-5.6 is terser than 5.5; "Be concise" may now over-shorten. [O5]
- **Creative drafting guardrails:** separate source-backed facts from creative wording; no invented names/metrics; placeholders if unsupported. [O5, O7]
- **Autonomy policy stated once** (answer/explain/review = inspect and report; change/build/fix = act locally and validate; confirm external writes/destructive/purchases). Repeating "ask first" causes needless approval requests. [O5]
- **Tool guidance lives in tool descriptions** (what, when, inputs, side effects, errors); expose only relevant tools; try 1-2 fallbacks on empty results. [O5, O7]
- **Preamble for perceived latency:** one-to-two-sentence visible update before tools. [O7]
- **Drop the current date** from prompts; the model knows the UTC date (add only for business timezone/policy dates). [O7]
- **Remove schemas from prompts; use Structured Outputs.** [O7]
- **Before raising effort, check the prompt for a missing success criterion, dependency rule, routing rule, or verification loop.** [O5]
- **Suggested skeleton:** `Role / Personality / Goal / Success criteria / Constraints / Tools / Output / Stop rules`, each short. [O5, O7]
- **Migration:** switch model, keep effort, run evals, remove scaffolding, add only the smallest fix for a measured regression, re-run. [O5]

### 4f. GPT-6 (Astra, 6.1 Sol, Luna) [O4]
- Astra asks clarifying questions more -> add bias-to-action prompts; treat "can you...", "help me..." as instructions to act.
- Do authorized work first, ask approval only on a "concrete, reviewable result"; no unsolicited warnings or compliance checklists.
- State instruction priority explicitly ("The user's instructions take precedence over guidelines provided in a skill"); audit skills/AGENTS.md (Astra is more sensitive to them).
- Ask it to name the exact skill file and quote the instruction when a skill makes it pause or diverge.
- Prose-style block to counter Markdown-heavy defaults; anti-slop word list ("delve", "leverage", "it's worth noting", "In short:"); avoid "X, not Y" contrastive framing and invented compound labels.
- Say when to delegate to subagents (Astra under-delegates); calibrate testing (Astra over-tests small changes).

### 4g. Codex models [O15]
- Remove prompting for upfront plans/preambles/status updates; it can make the model stop before finishing.
- Bias to action, deliver working code not plans; avoid loops of re-reading the same files.

---

## 5. Divergences between the two guides

| Topic | Anthropic | OpenAI |
|---|---|---|
| **Delimiters** | XML tags are the primary structuring tool (A1); A9 (blog) softens this: XML "optional", headings/whitespace often work as well. | Markdown headers first for sections, XML for content boundaries (O1, O12); JSON performs poorly for document lists (O12). |
| **Long-context placement** | Documents at the top, query/instructions at the end (up to 30% better) (A1). | Context near the end of the developer message (O1); GPT-4.1: instructions both before and after context, and if only once, *above* context beats below (O12). Direct conflict for single-placement. |
| **Role prompting** | Role in system prompt helps, "even a single sentence" (A1); blog says use sparingly, avoid elaborate personas (A9). | Identity/Role is the first section, plus a separate short Personality and Collaboration-style block (O1, O5, O7). OpenAI invests more in personality specification. |
| **CoT on reasoning models** | Rely on built-in thinking; on Fable 5.x, Opus 5/5.5, Sonnet 5.5 a request to write reasoning in the response may be *refused* (`reasoning_extraction`) (A1, A3, A4, A6). But Sonnet 5.5 JSON tasks: add "Think the problem through before you answer." (thinks internally) (A7). | Avoid CoT prompts on reasoning models (O2). But on GPT-4.1 / GPT-5 `minimal`, explicitly prompt CoT/planning and a "brief explanation summary at the start of the final answer" (O11, O12). OpenAI's own meta-prompt enforces "Reasoning before conclusions" in output (O9). |
| **Manual CoT format** | `<thinking>`/`<answer>` tags only when thinking is off and not on the newest models (A1). | "First, think carefully step by step..." at the end of the prompt (GPT-4.1) (O12). |
| **Few-shot default** | 3-5 examples recommended (A1); blog: start with one (A9). | Reasoning models: zero-shot first (O2); GPT-5.6: delete examples that don't change behavior (O5); meta-prompt: 1-3 examples with placeholders (O9). |
| **Current date** | Haiku 5.5: give today's date when a search tool is present (A8). | GPT-5.5: drop the current date; model knows UTC date (O7). |
| **Verbosity control** | Prompt instructions (effort does not control visible length on Opus 5) (A4). | `text.verbosity` API parameter for defaults, prompt for overrides (O5, O7, O11). |
| **Markdown default** | Older Claude over-formats; Fable 5.1 under-formats, so remove anti-formatting rules (A5). | o-series and GPT-5 API default to no Markdown (`Formatting re-enabled`) (O2, O11); GPT-6 Astra over-formats (O4). Both: model-specific, check per model. |
| **Prefill** | Unsupported from Claude 4.6 (400 error); use structured outputs/instructions (A1). The 2024 Console prompt improver *added* prefill (A10), now obsolete for current models. | No prefill concept; Structured Outputs. |
| **Progress updates** | Fable 5.1, Opus 5.5: ask for updates (and enable `display: "updates"`) (A3, A5). | GPT-5/5.5: prompt for preambles (O7, O11); Codex: *remove* preamble/plan prompting, it causes early stops (O15). |
| **Self-verification instructions** | Add for Sonnet 5.5/Haiku 5.5 at low effort (A7, A8); remove for Opus 5 (A4). | Add validation commands (O5, O7); calibrate down for GPT-6 Astra (O4). |
| **Emphasis in practice** | Principle: soften "CRITICAL/MUST" (A1), yet A1's own samples use "NEVER" and "MUST read the file". | Principle: reserve ALWAYS/NEVER for invariants (O5, O7), yet O9's meta-prompt uses "ATTENTION!... NEVER START EXAMPLES WITH CONCLUSIONS!". Treat caps as allowed for genuine invariants only. |
| **Instruction conflicts** | Not explicitly addressed in A1. | Explicit: GPT-4.1 favors the later instruction (O12); GPT-5 wastes reasoning on contradictions (O11); review for contradictions (O5). |

---

## 6. Anti-patterns both guides warn against

1. **Aggressive emphasis and over-prompting** ("CRITICAL", "MUST", "Be THOROUGH", "If in doubt, use [tool]") causing overtriggering. [A1] [O11, O12, O7]
2. **Carrying legacy scaffolding to newer models** (anti-laziness, forced verification, prescriptive step lists). [A1, A4, A6] [O5, O7, O11]
3. **Asking built-in reasoners to "think step by step" / write out reasoning.** [A1, A3] [O2]
4. **Vague prompts that expect mind-reading.** [A1 colleague test; A9 "Don't assume the AI reads minds"] [O1, O12 implicit rules are no longer inferred]
5. **Blanket "always use the tool/search" rules** -> over-calling or hallucinated tool inputs. [A1, A8] [O12 "you must call a tool before responding" failure mode]
6. **Over-long, over-engineered prompts.** [A9 "Don't over-engineer"] [O5 leaner prompts]
7. **Examples that contradict or exceed the written rules.** [A9] [O2, O12, O14 optimizer targets this]
8. **Changing several prompt elements at once without evals.** [A9] [O5]
9. **Using prompt wording to cut thinking instead of the effort setting.** [A3, A7, A8] [O5, O7 effort as the knob]
10. **Scope creep: unrequested features, refactors, tests, files.** [A1, A5, A7] [O4, O5]
11. **Under-specified output format** (missing length/structure). [A1] [O9 meta-prompt makes Output Format mandatory; O14 optimizer fixes "missing or unclear format specifications"]

Vendor-only warnings (no counterpart found): contradictory instructions [OpenAI: O5, O11, O12]; repeated sample phrases [O12]; surfacing token/context countdowns to the model [Anthropic: A6, A7]; generic "avoid generic X" negatives that just swap defaults [A3]; asking for reasoning in response text on newest Claude models [A1].

---

## 7. Official prompt-improvement tools and their transformations

### Anthropic
- **Console prompt improver** [A10, 2024-10-14]. Transformations: (1) adds a chain-of-thought section for systematic reasoning before responding; (2) standardizes examples into a consistent XML format; (3) enriches examples with reasoning matching the new structure; (4) rewrites for clearer structure and fixes grammar/spelling; (5) adds an assistant prefill to enforce format. Accepts user feedback and re-runs. Reported: +30% accuracy on a multilabel classification test (Claude 3 Haiku); 100% word-count adherence on a summarization test. Companion features: example management (structured input/output pairs, synthetic example generation), "ideal output" column for grading.
  - **UNCERTAIN:** the former docs page (prompt-improver) now redirects to A1; a search-engine snippet of it described four steps: example identification, initial draft with XML sections, CoT refinement, example enhancement. Current Console behavior was not verified. Items (1) CoT-in-response and (5) prefill conflict with current guidance for Claude 4.6+/5.x (prefill returns 400; reasoning-in-response may be refused).
- **Console prompt generator** [A11, 2024-05-20]: task description -> prompt template with role setting, a reasoning scratchpad, XML-tagged sections, `{{VARIABLES}}` (long/ambiguous variables wrapped in XML, short ones inline), and sometimes examples. Built on a long metaprompt with an XML "spine" that plans the template structure before writing it.
- **Metaprompt notebook** [A12; linked from A2 as "Prompt generator notebook"]: multishot task->instructions examples; model writes three blocks in order: `<Inputs>` (minimal, non-overlapping variables), `<Instructions Structure>` (plan; long inputs placed *before* the directions about them), `<Instructions>` (the template). Encoded rules: XML-delimit variables; justification before score; scratchpad only for complex tasks; it writes instructions rather than doing the task. Updated for 4.6+ (no prefill; steering text appended to the user turn). Caveats: single-turn only; output "not guaranteed to be optimal".
- **In-docs suggestion** [A1]: ask Claude to evaluate examples for relevance/diversity or to generate more from an initial set.

### OpenAI
- **Prompt optimizer** (Playground, `platform.openai.com/chat/edit?optimize=true`) [O8, O14]. Optimizes a pasted developer message "according to current best practices" for the target model and task (agentic, coding, multimodal). Explicitly removes: contradictions in instructions; missing or unclear format specifications; inconsistencies between prompt and few-shot examples. Shows changed snippets with rationale (comments / inline review); accepts specific requested edits; iterate. Dataset mode uses Good/Bad annotations, `output_feedback` critiques, and grader results (needs >=3 rows with outputs and >=1 grade/annotation per row); "always evaluate and manually review" since outputs can regress on some inputs.
  - **Deprecation:** the dataset-backed optimizer is part of Evals, which becomes read-only 2026-10-31 and shuts down 2026-11-30 [O8]. **UNCERTAIN** whether the non-dataset Optimize button remains after that.
- **Prompt generation / "Generate" button** [O9]: meta-prompts (text, audio variants) + meta-schemas for JSON/function definitions. Text meta-prompt guidelines: understand task; minimal changes (for complex prompts, add missing elements without altering structure); reasoning before conclusions (reverse examples that conclude first); high-quality examples with `[placeholders]`; clear, concise language, no "bland statements"; Markdown, no code fences unless requested; preserve all user content, variables, and placeholders; include constants (rubrics, guides); explicit output format with length and syntax; bias to JSON for structured data. Output skeleton: first line = concise task instruction (no header) -> details -> optional `# Steps` -> `# Output Format` -> optional `# Examples` (1-3, with placeholders, note if real ones are longer) -> optional `# Notes` (edge cases).
  - **Edit meta-prompt** adds a `<reasoning>` pre-analysis before outputting the full revised prompt: Simple change? (yes/no; skip rest if yes); Reasoning present / where / ordering before-or-after; Structure (yes/no); Examples present + representativeness (1-5); Complexity of prompt and task (1-5); Specificity (1-5); Prioritization (top 1-3 categories); Conclusion (<=30 words, imperative). The reasoning is parsed out of the final result.
- **Metaprompting recipes** (no UI): GPT-5 template "explain what specific phrases could be added to, or deleted from, this prompt... minimal edits" (O11); GPT-5.1 two-step: (1) diagnose failure modes and quote the prompt lines driving them, (2) "surgical revision" with `patch_notes` + `revised_system_prompt`, keep structure and length, group related failures per call (O13); Codex: ask at end of a weak turn how to change its instructions, sample several times, keep common suggestions, generalize, verify with evals (O15).
- **OpenAI Docs skill for Codex** [O4]: `$openai-docs migrate this project to the GPT-6 model family` applies the migration guidance automatically.

### Implications for our skill (derived from the above, not vendor guidance)
- Both vendors' tools converge on: preserve user content and variables, add an explicit output-format section, resolve contradictions, align examples with rules, and make minimal edits to already-complex prompts. These are safe default transformations.
- Run a short diagnostic pass before rewriting (OpenAI edit meta-prompt rubric; GPT-5.1 diagnose-then-patch) and emit change notes with the rewrite.
- Branch on target model: for current Claude and GPT-5+/GPT-6 reasoning models, do **not** inject "think step by step", reasoning-in-output sections, or prefill; prefer goals, success criteria, constraints, and stop rules. Add explicit CoT only for non-reasoning targets (GPT-4.1, effort `none`/`minimal`, Claude with thinking off on older models).
- Choose delimiters by target: XML tags for Claude; Markdown sections plus XML for content blocks for GPT.
- Avoid adding caps/"CRITICAL"; keep absolutes for true invariants.
- Keep rewrites lean: GPT-5.6 data and Anthropic's Fable guidance both show shorter, non-redundant prompts perform better.
- Ask the user for missing success criteria or audience only when it would materially change the rewrite (mirrors both vendors' clarification guidance).
