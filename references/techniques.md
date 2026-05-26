# Prompt Engineering Techniques Reference

## Diagnostic Dimensions

When analyzing a raw prompt, evaluate across these 7 dimensions:

| Dimension | What to Check | Red Flags |
|-----------|--------------|-----------|
| **Clarity** | Is the goal unambiguous? | Vague verbs ("make it better"), missing subject/object |
| **Context** | Is background info provided? | No audience, domain, or scenario context |
| **Constraints** | Are boundaries explicit? | No format, length, tone, or scope limits |
| **Role** | Is a persona assigned? | No expert role defined for the AI |
| **Structure** | Is output format specified? | No sections, steps, or template guidance |
| **Examples** | Are few-shot examples given? | No demonstration of desired style/quality |
| **Reasoning** | Is thinking process guided? | No chain-of-thought, no step-by-step instruction |

## Structural Patterns

### Tier 1: Skeleton (minimum viable)
```
[Role] + [Task] + [Output Format]
```
Example: "You are a data analyst. Analyze this sales data. Output as a table."

### Tier 2: Standard (recommended baseline)
```
[Role] + [Context] + [Task] + [Constraints] + [Output Format]
```
Example: "You are a senior product manager at a SaaS company. Our target is mid-market B2B. Draft a PRD for a new analytics dashboard feature. Keep under 800 words. Use bullet points for requirements."

### Tier 3: Advanced (production quality)
```
[Role] + [Context] + [Task] + [Constraints] + [Examples] + [Reasoning Guide] + [Output Format]
```
Example: Includes few-shot examples, chain-of-thought instruction, and anti-pattern warnings.

## Technique Catalog

### 1. Role Assignment
Assign a specific expert persona with domain, seniority, and perspective.
- Weak: "Write a blog post"
- Strong: "You are a veteran cybersecurity journalist with 15 years of experience writing for Wired and Krebs on Security"

### 2. Constraint Layering
Layer constraints by category:
- Format: word count, structure (markdown/JSON/table), section requirements
- Tone: professional/casual/academic/persuasive
- Scope: what to include AND what to exclude
- Quality: accuracy requirements, citation rules, verification steps

### 3. Few-Shot Conditioning
Provide 1-3 examples showing desired input→output pattern.
- Use real, representative examples
- Cover edge cases in examples
- Show both "good" and "bad" outputs when helpful

### 4. Chain-of-Thought Elicitation
Instruct the model to think step by step:
- "Before answering, analyze the problem in these steps: 1) Identify... 2) Evaluate... 3) Decide..."
- "Show your reasoning before giving the final answer"
- "For each option, list pros and cons before selecting"

### 5. Negative Constraints
Explicitly state what NOT to do:
- "Do NOT use technical jargon"
- "Never mention competitor names"
- "Avoid numbered lists; use prose only"

### 6. Output Scaffolding
Provide a template skeleton for the response:
- "Respond in this exact format: ## Summary / ## Analysis / ## Recommendations / ## Risks"
- "For each item, include: Name | Description | Priority (High/Medium/Low) | Estimated Effort"

### 7. Persona Framing
Define the AI's relationship to the user:
- "You are a critical peer reviewer, not a cheerleader"
- "You are a patient tutor who explains concepts to a beginner"
- "You are a devil's advocate who challenges every assumption"

### 8. Iteration Hook
Build in a feedback loop:
- "After your initial response, ask me 2-3 clarifying questions"
- "Rate your confidence in each recommendation on a scale of 0-10"

## Anti-Patterns to Eliminate

| Anti-Pattern | Example | Fix |
|-------------|---------|-----|
| **Ambiguous goal** | "Make this better" | "Improve readability by shortening sentences and adding subheadings" |
| **Missing "why"** | "List 10 startup ideas" | "List 10 startup ideas for the AI developer tools space, targeting solo founders with <$50k budget" |
| **No success criteria** | "Write a good email" | "Write an email that: 1) states the problem in 1 sentence, 2) proposes a solution, 3) ends with a single clear call to action" |
| **Over-constraining** | 15 bullet points of contradictory rules | Merge related constraints, prioritize by importance |
| **Assumed knowledge** | "Use the standard framework" | Name the specific framework or provide a brief definition |
| **Lazy role** | "You are an expert" | "You are a PhD-level computational linguist specializing in transformer architectures" |
| **Over-specifying output** | "输出 5 个表格，每个表格 7 列，列名分别是..." | 只规定核心原则（如"用表格对比"），让模型根据内容决定最佳呈现方式 |
| **扼杀自主组织** | 规定死板的章节顺序和字数限制 | 用"输出结构由你根据内容自行组织"替代固定模板，信任模型判断力 |
