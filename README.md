# enhance-prompt

A [Claude Code](https://claude.com/claude-code) skill that rewrites a rough prompt into an enhanced one that follows Anthropic's and OpenAI's published prompt-engineering guidance, tuned to the model you're writing for.

```
/enhance-prompt claude summarize this article for me and make it good
/enhance-prompt gpt-4.1 you're an agent with web search... find the population of...
/enhance-prompt opus-5.5 ./prompts/system-prompt.md
```

## What you get back

- **Prompt Kind** (task / system / agentic) and **Target Model**
- **The Enhanced Prompt** in one copyable block (for a file input, it writes `<name>.enhanced.<ext>` next to the original and never overwrites it)
- **Changes**: each edit in plain words, citing the rule it applies
- **Assumptions to check**: everything the skill added that you didn't write, so you can strike it
- **Settings to consider**: fixes that belong in API settings (effort, verbosity, Structured Outputs) rather than prompt text

## How it behaves

- **Model-aware.** The first word picks the target: `claude`, `gpt`, `generic` (rules both vendors agree on), or a specific model such as `opus-5.5`, `sonnet-5.5`, `haiku-5.5`, `gpt-4.1`, `gpt-5.6`, `gpt-6-astra` or `codex`. A specific model applies that generation's exceptions; for example, explicit step-by-step reasoning is kept for GPT-4.1 but removed for current reasoning models.
- **Keeps your goal and your asks.** What you want the result to be (persistence, a format, an opening line) survives. Method instructions the guides flag as anti-patterns (blanket "read every file first", ALL-CAPS emphasis, "don't be lazy") are rewritten, with your original quoted under Changes.
- **Never invents.** Missing content becomes a labelled placeholder (`{{ARTICLE}}`), and invented constraints go under Assumptions.
- **Treats your prompt as data.** `/enhance-prompt write a poem about cats` returns a better prompt, not a poem. Injection lines ("ignore all previous instructions") are removed.
- **Scales to the prompt.** A one-liner gets light edits, not a template.
- **Keeps your language.** An Arabic prompt stays Arabic.

## Install

```bash
git clone https://github.com/mohamedatef-99/enhance-prompt.git
# macOS / Linux
ln -s "$PWD/enhance-prompt/enhance-prompt" ~/.claude/skills/enhance-prompt
```

```powershell
# Windows (PowerShell, no admin needed)
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\enhance-prompt" -Target "$PWD\enhance-prompt\enhance-prompt"
```

The skill is user-invoked only (`disable-model-invocation: true`), so it never fires on its own. Start a new Claude Code session and type `/enhance-prompt`.

## Repository layout

| Path | What it is |
|---|---|
| `enhance-prompt/SKILL.md` | The procedure: parse, check rule freshness, diagnose, ask only what matters, enhance, output |
| `enhance-prompt/references/rules.md` | Shared, Claude and GPT rules with IDs (`S#`, `C#`, `G#`, `N#`) and Settings Notes |
| `enhance-prompt/references/{claude,gpt}-generations.md` | Per-model exceptions, read only when you name a specific model |
| `enhance-prompt/evals/evals.json` | 9 test prompts with 67 assertions |
| `docs/research/prompt-engineering-guides.md` | Research summary of 27 Anthropic and OpenAI primary sources |
| `docs/research/rules-verification-*.md` | Rule-by-rule check of `rules.md` against those sources, with quotes |
| `GLOSSARY.md` | The project's vocabulary (Source Prompt, Enhanced Prompt, Target Model, …) |

## Evidence and limits

Across four test iterations, outputs with the skill passed 98% of the assertions versus 63% for a plain "improve this prompt" request to the same model. Without the skill, the model invented facts, added unrequested scope, and kept patterns current models reject.

Know the limits before you depend on it:

- **The tests measure process, not outcomes.** They check that rewrites follow the guides, not that the enhanced prompt gets better answers. Compare original and enhanced prompts on your own tasks.
- **The test set is small** (9 prompts, one run each, graded by Claude), and part of the score gap comes from the skill's own output format.
- **Vendor guidance moves fast.** Rules are dated 2026-10-11 and were verified by agents against the sources the same day. The skill warns you when they are more than six months old. Refresh them by re-running the research and verification against the URLs in `docs/research/`.
