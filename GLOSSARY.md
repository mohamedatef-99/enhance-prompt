# Prompt Enhancement

Turning a rough prompt into one that follows the published prompt-engineering guidance from Anthropic and OpenAI.

## Language

**Source Prompt**:
The prompt the user hands in to be improved, in whatever state it is in.
_Avoid_: Input prompt, original, raw prompt

**Enhanced Prompt**:
The rewritten prompt produced from a Source Prompt for a given Target Model.
_Avoid_: Optimized prompt, improved prompt, output

**Enhance**:
The single operation of rewriting a Source Prompt into an Enhanced Prompt for a Target Model. Moving a prompt from one vendor's style to another is Enhance with a different Target Model, not a separate operation.
_Avoid_: Convert, optimize, rewrite, translate

**Target Model**:
The model the Enhanced Prompt is written for: a family (`claude`, `gpt`, or `generic` for shared guidance only), optionally narrowed to a specific model generation. A bare family means that family's current reasoning models.
_Avoid_: Vendor, provider, destination model

**Prompt Kind**:
The category of a Source Prompt: a one-off task prompt, a system prompt, or an agentic prompt.
_Avoid_: Prompt type, mode

**Shared Rule**:
A practice that both the Anthropic and the OpenAI guidance recommend, and so applies to every Target Model.
_Avoid_: Best practice, universal rule

**Vendor Rule**:
A practice that only one vendor's guidance recommends, and so applies only when that vendor's model family is the Target Model.
_Avoid_: Model-specific tip

**Generation Exception**:
A variation of a Vendor Rule that applies only when the Target Model names a specific model generation.
_Avoid_: Model override, special case

**Diagnosis**:
The internal assessment of a Source Prompt, done before Enhancing, that decides how much to change. Its findings surface only as Changes.
_Avoid_: Analysis, audit, scoring

**Settings Note**:
Advice to adjust an API setting such as reasoning effort or verbosity, given in place of prompt wording that would try to do the same job.
_Avoid_: Config tip, parameter advice

**Change**:
One edit made while Enhancing, recorded together with the Shared Rule or Vendor Rule that justifies it.
_Avoid_: Diff, improvement

**Assumption**:
A fact missing from the Source Prompt that the Enhanced Prompt fills with a guess, flagged so the user can check it.
_Avoid_: Default, placeholder

**Outcome Ask**:
Something the Source Prompt explicitly asks for about the result or the model's behaviour (persistence, a format, an opening line, a length). Enhancing may reword it but never overrules it.
_Avoid_: Requirement, user instruction

**Method Instruction**:
Something the Source Prompt tells the model to do on the way to the result (read every file first, think step by step, use caps for emphasis). When a rule flags it as an anti-pattern, Enhancing rewrites it and quotes the original.
_Avoid_: Process step, technique

**Clarifying Question**:
A question asked before Enhancing, only when a missing fact would materially change the Enhanced Prompt. At most about three are asked per run.
_Avoid_: Interview, follow-up
