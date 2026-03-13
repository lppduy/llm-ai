# Prompt Engineering Patterns

## The Message Roles
Every chat API uses 3 roles:
```
system    → "who you are" — sets behavior, constraints
user      → "what I want" — the request
assistant → "what you said" — model's previous responses
```

System message is the most powerful lever — shapes all subsequent responses.

## Zero-shot vs Few-shot

**Zero-shot** — no examples, just ask:
```
Classify this review as positive or negative: "The food was terrible"
```

**Few-shot** — teach by example:
```
Review: "Amazing service!" → positive
Review: "Waited 2 hours" → negative
Review: "The food was terrible" →
```
Model sees pattern → follows it. 2-3 examples usually enough.

Use few-shot for: structured output, classification, formatting.

## Chain-of-Thought (CoT)

Force step-by-step reasoning before answering.

```
Q: 15 apples. 3 customers buy 4 each. How many left? Think step by step.
A: 3 × 4 = 12 bought. 15 - 12 = 3 left.
```

**Why it works:** reasoning tokens become context for next token prediction. The model literally thinks better when it writes out steps.

Trigger: `"Think step by step"` or `"Show your reasoning"`

Use for: math, logic, debugging, comparison.
Skip for: simple lookups, classification (wastes tokens).

## Practical Patterns

### Output Formatting
```
Respond in JSON only, no explanation.
Use this exact format: {"sender": "", "action": "", "deadline": ""}
```
Critical for programmatic use.

### Persona
```
system: "You are a PostgreSQL DBA with 15 years experience.
         Only suggest solutions you've seen work in production."
```
Constrains domain and tone.

### Constraints
```
- Do not make up information. Say "I don't know" if unsure.
- Standard library only.
- Keep under 100 words.
```

### Delimiters
```
Summarize the text between triple backticks:
\```
{user document here}
\```
```
Prevents model from confusing instructions with input data.

## Cheat Sheet

| Pattern | When |
|---------|------|
| System message | Always — persona + constraints |
| Few-shot | Structured output, classification |
| CoT | Math, logic, multi-step |
| Output format | Parsing response in code |
| Constraints | Production — reduce hallucination |
| Delimiters | Input data could confuse instructions |
