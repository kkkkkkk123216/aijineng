---
name: prompt-optimizer
description: >
  Transform vague, unstructured natural language into professional-grade AI prompts.
  This skill diagnoses missing elements (role, context, constraints, format, examples, reasoning guide),
  then reconstructs the prompt into a structured, logically rigorous form that any LLM can execute with precision.
  Use when the user says "optimize this prompt", "improve my prompt", "帮我优化提示词", "改写提示词",
  "turn this into a good prompt", "make this prompt better", "prompt engineering help",
  or shares a raw prompt that needs professional restructuring.
  Also use when the user asks for help crafting a prompt from scratch for a specific task.
---

# Prompt Optimizer

Transform rough, ambiguous natural language into production-grade prompts through diagnostic analysis and structural reconstruction.

## Workflow

### Phase 1: Diagnose

Analyze the user's input against the 7 diagnostic dimensions in [techniques.md](references/techniques.md).
Identify which dimensions are weak or missing. Produce a concise diagnosis:

```
**缺失诊断**：
- 角色：未指定 / 模糊
- 上下文：缺少背景信息
- 约束：未设定边界
- 输出格式：未指定
- 示例：无
- 推理引导：无
```

Keep this diagnosis brief — no more than one line per dimension. Only list dimensions that are genuinely deficient.

### Phase 2: Reconstruct

Based on the diagnosis, rebuild the prompt. Follow these rules:

1. **Fill every gap**: For each missing dimension, infer reasonable defaults from the user's intent. Never leave a dimension blank unless truly inapplicable.

2. **Match the tier**: Choose the structural tier based on task complexity:
   - Simple Q&A / one-shot tasks → Tier 1 (Role + Task + Format)
   - Analysis / creation / multi-step tasks → Tier 2 (add Context + Constraints)
   - Complex workflows / high-stakes output → Tier 3 (add Examples + Reasoning Guide)

3. **Infer, don't ask**: Make reasonable assumptions for missing details rather than leaving placeholders. Infer from the domain, task type, and common practice. Only flag truly ambiguous intent for user clarification.

4. **Be specific, not generic**: Replace "You are an expert" with concrete domain-seniority descriptors. Replace "write well" with measurable quality criteria.

5. **Preserve intent**: Never alter the user's core goal. Only add structure, clarity, and guardrails.

6. **Restraint over over-engineering (克制优先)** — This is the most important rule:
   - Give the model principles to follow, not templates to fill. Trust its ability to organize output based on actual content.
   - Constraints should be minimal but sharp: 3-5 core principles hit harder than 15 checklist items.
   - When in doubt, err on the side of fewer words. A 10-line prompt that nails the essence beats an 80-line prompt that micromanages.
   - Never write pre-fabricated table schemas, fixed section orders, or rigid word counts unless the user explicitly demands them.
   - Use phrases like "自行组织"、"怎么讲清楚就怎么讲"、"根据内容决定" instead of "按以下格式输出".
   - The optimized prompt should feel like talking to a smart colleague, not filling out a bureaucratic form.
   - Never prescribe the model's thinking process (e.g., "先通读全文，再逐项核实，最后审视"). The model knows how to reason — your job is to define WHAT to evaluate, not HOW to think.

### Phase 3: Present — Three-Tier Output

Always output 3 versions of the optimized prompt, from minimal to comprehensive. Let the user choose the level that fits their needs.

```
## 诊断

[Concise diagnosis — one line per dimension, only deficient ones]

---

### 极简版

[1-3 sentences: role + core task. Ready to copy-paste for quick use.]

### 规范版

[Add context + key constraints on top of the minimal version. For formal work scenarios.]

### 发布级版

[Full dimensional coverage: add reasoning guide, examples (if helpful), adaptive structure hints. For high-stakes tasks or prompts shared with others.]

---

## 改进说明

[3-5 bullet points: what was changed and why. Be specific.]
```

**Version guidelines**:
- **极简版**: ≤ 3 sentences. Role + task + one essential constraint. If the original prompt is already this simple, acknowledge it and skip to 规范版.
- **规范版**: ≤ 10 sentences. The recommended default for most use cases. Covers role, context, task, and 3-5 sharp constraints.
- **发布级版**: ≤ 20 sentences. Add reasoning hints and examples only if they genuinely help — never pad for length. Do NOT prescribe thinking flow or analysis steps; trust the model to reason based on content. Focus on: sharper role definition, broader context, and stronger quality criteria.

Each version should be self-contained and independently usable. Never write "see above" or "add the following to the previous version."

### Quality Checklist

Before outputting, verify all three versions:

- [ ] Each version can be executed independently without additional clarification
- [ ] Every sentence serves a purpose (no filler)
- [ ] The role is specific (domain + seniority + perspective)
- [ ] Constraints include both format AND quality criteria
- [ ] Restraint is applied: each version is as short as it can be while remaining effective
- [ ] The user's original intent is fully preserved in all versions
- [ ] No placeholder text like "[insert here]" or "TBD"

### Handling Edge Cases

- **Already good prompt**: If the input is already well-structured, acknowledge it and offer targeted enhancements (e.g., "Your prompt is already strong. Suggested micro-improvements: ...")
- **Too vague to infer**: If the task itself is ambiguous (not just poorly structured), ask ONE clarifying question before optimizing. Do not optimize a prompt whose purpose is unclear.
- **Multiple prompts**: If the user provides several prompts, optimize each separately with its own diagnosis.
- **Non-prompt input**: If the user provides content that is not a prompt (e.g., a story, code, data), clarify: "This appears to be content rather than a prompt. Are you asking me to create a prompt that would generate content like this, or did you mean something else?"

## Reference

For detailed prompt engineering techniques, patterns, and anti-patterns, read [techniques.md](references/techniques.md) when:
- The task requires advanced techniques (Tier 3)
- The user's domain is specialized and needs domain-specific patterns
- You need to refresh on specific technique implementation details
