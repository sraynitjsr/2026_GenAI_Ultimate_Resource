# Clear Instructions & Context

## Overview
Providing clear, detailed instructions with relevant context is the foundation of effective prompt engineering. The more specific and contextualized your prompt, the better the LLM can understand and fulfill your request.

## Key Principles

### 1. Be Specific and Detailed
- Avoid vague requests
- State exactly what you want
- Define the scope and boundaries

### 2. Provide Context
- Background information
- Target audience
- Purpose of the output
- Constraints and requirements

### 3. Specify Format
- Desired structure
- Length requirements
- Output format (JSON, markdown, etc.)

## Examples

### ❌ Vague Prompt
```
Write about AI.
```

### ✅ Clear Prompt with Context
```
Write a 300-word introduction to artificial intelligence for high school students 
who have no prior programming experience. Focus on real-world applications they 
can relate to (social media, gaming, voice assistants). Use simple language and 
avoid technical jargon. Structure: 
1. What is AI in simple terms
2. 3 everyday examples
3. Why it matters for their future
```

### ❌ Unclear Instructions
```
Create a function to process data.
```

### ✅ Clear Instructions with Context
```
Create a Python function that:
- Takes a list of dictionaries as input
- Each dictionary has 'name' (string), 'age' (int), 'score' (float) keys
- Filters out entries where age < 18
- Sorts by score in descending order
- Returns the top 10 results
- Includes error handling for missing keys
- Has type hints and docstring

Example input format:
[{"name": "Alice", "age": 25, "score": 95.5}, ...]
```

## Best Practices

### 1. **Use Delimiters**
Separate different parts of your prompt clearly:
```
Task: [Your task]
Context: [Background information]
Input: [Data to process]
Format: [Desired output structure]
Constraints: [Limitations]
```

### 2. **Define Success Criteria**
Tell the model what "good" looks like:
```
Generate product descriptions that:
- Are between 50-75 words
- Use active voice
- Include 2-3 key features
- End with a call-to-action
- Maintain a professional yet friendly tone
```

### 3. **Provide Reference Material**
Include relevant information directly in the prompt:
```
Using the following product specifications:
[Paste specs here]

Write a marketing email that highlights the top 3 features...
```

### 4. **Break Down Complex Tasks**
```
Step 1: Analyze the customer feedback below
Step 2: Identify the top 3 pain points
Step 3: For each pain point, suggest a solution
Step 4: Summarize in a bullet list

Customer Feedback:
[Paste feedback]
```

### 5. **Use Explicit Constraints**
```
Write a blog post about cybersecurity tips.

Requirements:
- Target audience: Small business owners
- Length: 800-1000 words
- Tone: Informative but not technical
- Include: 5 actionable tips
- Avoid: Fear-mongering language
- Format: Introduction, 5 sections (one per tip), conclusion
```

## Common Patterns

### Pattern 1: Task + Context + Format
```
Task: Summarize this research paper
Context: For a non-technical executive audience
Format: 3 bullet points, each under 30 words
```

### Pattern 2: Role + Task + Constraints
```
You are a technical writer. 
Explain how JWT authentication works.
Constraints: Use analogies, avoid code, under 200 words.
```

### Pattern 3: Input + Process + Output
```
Input: [Customer email]
Process: Identify the issue, sentiment, and urgency level
Output: JSON with keys: issue, sentiment, urgency, suggested_response
```

## Advanced Techniques

### Negative Instructions
Tell the model what NOT to do:
```
Explain blockchain technology.

DO:
- Use everyday analogies
- Focus on practical benefits

DON'T:
- Use technical jargon
- Discuss cryptocurrency prices
- Include code examples
```

### Contextual Priming
Set the stage before the main task:
```
Our company sells eco-friendly cleaning products to environmentally 
conscious consumers aged 25-45. Our brand voice is warm, authentic, 
and educational without being preachy.

Now write a product description for our new all-purpose cleaner.
```

## Template

Use this template for complex tasks:

```
# ROLE
You are [role/expertise]

# CONTEXT
[Background information, target audience, purpose]

# TASK
[Specific task to complete]

# INPUT
[Data/information to process]

# FORMAT
[Desired output structure]

# CONSTRAINTS
- [Limitation 1]
- [Limitation 2]
- [Limitation 3]

# EXAMPLE (optional)
[Show desired output style]
```

## Common Mistakes

### ❌ Too Brief
```
Summarize this article.
```

### ✅ Properly Detailed
```
Read the article below and create a 150-word executive summary 
highlighting the main findings and business implications.
```

### ❌ Ambiguous
```
Make it better.
```

### ✅ Specific Feedback
```
Revise the paragraph to:
- Shorten sentences (max 20 words each)
- Replace passive voice with active voice
- Add specific metrics where possible
```

### ❌ Assumed Context
```
Write the report.
```

### ✅ Explicit Context
```
Write a quarterly sales report for our CEO covering Q1 2026. 
Include: total revenue, top 5 products, regional breakdown, 
YoY comparison. Use data from the table below. Format as a 
professional email with executive summary at the top.
```

## Testing Your Instructions

### Checklist:
- [ ] Is the task clearly stated?
- [ ] Have I provided necessary context?
- [ ] Are format requirements explicit?
- [ ] Have I specified length/scope?
- [ ] Are constraints clearly listed?
- [ ] Would someone unfamiliar with the project understand?
- [ ] Have I included examples if needed?

## Related Techniques
- **Few-shot learning** - Provide examples to clarify expectations
- **Role assignment** - Set expertise context
- **Chain-of-thought** - Break down reasoning steps

## Key Takeaway
**The clearer your instructions, the better the output.** Spend time crafting detailed prompts—it's more efficient than iterating on vague ones.
