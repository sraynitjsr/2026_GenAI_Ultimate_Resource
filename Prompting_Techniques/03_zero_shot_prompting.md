# Zero-Shot Prompting

## Overview
Zero-shot prompting is asking an LLM to perform a task without providing any examples. You rely solely on clear instructions and the model's pre-trained knowledge. Modern LLMs like GPT-4 and Claude are remarkably capable at zero-shot tasks.

## Definition
**Zero-shot** = Zero examples + Clear instructions

The model uses only:
- Its training data knowledge
- Your task description
- Any context you provide

## When to Use Zero-Shot

### ✅ Use Zero-Shot When:
- The task is straightforward
- Instructions can be clearly stated
- You want to save tokens (cost)
- The model is capable (GPT-4, Claude)
- Quick prototyping
- General knowledge tasks
- Standard formats (summaries, explanations)

### ❌ Consider Few-Shot Instead When:
- You need very specific output format
- The task is unusual or nuanced
- Consistency is critical
- The pattern is hard to describe
- You're using a weaker model

## Examples

### Example 1: Summarization
```
Summarize the following article in 3 bullet points:

[Article text here]
```

**Why zero-shot works:** Summarization is a common, well-defined task.

### Example 2: Translation
```
Translate the following English text to French:

"Artificial intelligence is transforming how we work and live."
```

**Why zero-shot works:** Translation is straightforward with clear input/output.

### Example 3: Sentiment Analysis
```
Analyze the sentiment of this customer review as positive, negative, or neutral:

"The product arrived quickly but the quality was disappointing."
```

**Why zero-shot works:** Simple classification with clear categories.

### Example 4: Question Answering
```
Answer this question based on the context below:

Context: [Relevant information]

Question: What were the main factors contributing to the company's growth?
```

**Why zero-shot works:** Extractive QA from provided context.

### Example 5: Code Generation
```
Write a Python function that takes a list of numbers and returns the median value. 
Include error handling for empty lists.
```

**Why zero-shot works:** Standard programming task with clear requirements.

### Example 6: Content Creation
```
Write a professional email to a client apologizing for a delayed shipment. 
Explain that it was due to weather-related logistics issues and offer a 10% 
discount on their next order. Keep it under 150 words.
```

**Why zero-shot works:** Common business communication with explicit parameters.

### Example 7: Data Extraction
```
Extract all email addresses from this text:

"Contact us at support@company.com or sales@company.com. For urgent matters, 
reach John at john.doe@company.com."

Return as a JSON array.
```

**Why zero-shot works:** Well-defined extraction task with format specified.

### Example 8: Classification
```
Classify the following job role into one of these categories: 
Engineering, Marketing, Sales, Customer Support, Operations, Finance

Job Role: "Solutions Architect"
```

**Why zero-shot works:** Clear categories and straightforward classification.

## Best Practices

### 1. **Be Explicit and Detailed**
```
❌ Too vague:
"Explain AI"

✅ Clear and detailed:
"Explain what artificial intelligence is in 2-3 paragraphs for a non-technical 
business executive. Focus on practical business applications rather than technical 
details."
```

### 2. **Specify Output Format**
```
❌ Ambiguous:
"List the main points"

✅ Explicit format:
"List the main points as a numbered list with exactly 5 items, each under 20 words."
```

### 3. **Provide Context**
```
❌ No context:
"Is this good?"

✅ With context:
"You are reviewing a technical proposal. Evaluate whether this proposed architecture 
is appropriate for a high-traffic web application expecting 1M daily users. Focus on 
scalability and cost."
```

### 4. **Use Constraints**
```
Generate a product tagline for an eco-friendly water bottle.

Constraints:
- Maximum 8 words
- Mention sustainability
- Avoid clichés like "save the planet"
- Target audience: young professionals
```

### 5. **Define Success Criteria**
```
Write a blog post introduction about remote work trends.

Success criteria:
- Hook the reader in the first sentence
- Mention a surprising statistic
- 100-150 words
- End with a preview of what's coming
```

## Zero-Shot Patterns

### Pattern 1: Direct Task
```
[Action verb] + [object] + [specifications]

Example: "Translate this text to Spanish using formal language."
```

### Pattern 2: Task + Context + Format
```
Task: [What to do]
Context: [Background info]
Format: [How to structure output]

Example:
Task: Summarize this research paper
Context: For a non-technical executive audience
Format: 3 bullet points, each under 30 words
```

### Pattern 3: Input + Process + Output
```
Input: [Your data]
Process: [What to do with it]
Output format: [Desired structure]

Example:
Input: Customer feedback below
Process: Identify the top 3 complaints
Output format: JSON array with issue, frequency, severity
```

### Pattern 4: Conditional Instructions
```
If [condition], then [action A], otherwise [action B]

Example:
"If the text contains technical jargon, explain it in simpler terms. 
Otherwise, keep the original language."
```

## Advanced Zero-Shot Techniques

### 1. **Meta-Instructions**
Add instructions about how to interpret instructions:
```
Read the following task carefully. If anything is unclear, make reasonable 
assumptions and state them in your response.

Task: [Your task]
```

### 2. **Negative Instructions**
Tell the model what NOT to do:
```
Explain quantum computing.

DO:
- Use everyday analogies
- Keep it under 200 words

DON'T:
- Use mathematical equations
- Assume prior physics knowledge
```

### 3. **Step-by-Step Guidance**
Break down the task:
```
Analyze this customer complaint:

Step 1: Identify the primary issue
Step 2: Assess the severity (low/medium/high)
Step 3: Suggest a resolution
Step 4: Draft a response

Complaint: [Text]
```

### 4. **Output Constraints**
Use structural constraints:
```
Explain the benefits of cloud computing.

Structure:
- Introduction (1 sentence)
- Benefit 1 with example
- Benefit 2 with example
- Benefit 3 with example
- Conclusion (1 sentence)

Maximum 200 words total.
```

### 5. **Role-Based Context**
Set expertise level:
```
You are a senior cybersecurity consultant. Explain the OWASP Top 10 
vulnerabilities to a development team. Use practical examples and 
focus on prevention strategies.
```

## Zero-Shot vs Few-Shot Comparison

| Aspect | Zero-Shot | Few-Shot |
|--------|-----------|----------|
| **Examples Provided** | 0 | 2-5+ |
| **Token Usage** | Low | Higher |
| **Setup Time** | Fast | Slower |
| **Best For** | Common tasks | Specific formats |
| **Consistency** | Good (GPT-4) | Better |
| **Model Requirement** | Strong model needed | Works with weaker models |
| **Cost** | Lower | Higher |
| **Use Case** | Standard operations | Custom patterns |

## When Zero-Shot Fails

### Signs You Need Few-Shot:
1. **Inconsistent formatting** despite clear instructions
2. **Unusual task** the model hasn't seen often
3. **Complex transformations** hard to describe
4. **Specific style** that's easier to show than tell
5. **Weaker model** (GPT-3.5, older models)

### Troubleshooting Zero-Shot

**Problem:** Output format is inconsistent
```
❌ Original:
"Extract the date"

✅ Fixed with explicit format:
"Extract the date and return in YYYY-MM-DD format. Return 'null' if no date found."
```

**Problem:** Model misunderstands the task
```
❌ Original:
"Review this"

✅ Fixed with clear instructions:
"Review this code for security vulnerabilities. List each issue with:
- Line number
- Severity (low/medium/high)
- Description
- Suggested fix"
```

**Problem:** Output is too verbose
```
❌ Original:
"Explain"

✅ Fixed with constraints:
"Explain in exactly 2 sentences."
```

## Code Implementation

```python
from openai import OpenAI

def zero_shot_completion(task, context="", constraints="", model="gpt-4"):
    """
    Generate a zero-shot completion.
    
    Args:
        task: The main task description
        context: Optional background context
        constraints: Optional constraints/requirements
        model: Model to use
    
    Returns:
        Model response
    """
    prompt = f"Task: {task}\n\n"
    
    if context:
        prompt += f"Context: {context}\n\n"
    
    if constraints:
        prompt += f"Constraints:\n{constraints}\n\n"
    
    client = OpenAI()
    response = client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": prompt}],
        temperature=0.7
    )
    
    return response.choices[0].message.content

# Usage
result = zero_shot_completion(
    task="Summarize this article",
    context="Article about AI trends in 2026",
    constraints="- 3 bullet points\n- Each under 25 words\n- Focus on business impact"
)
```

## Optimization Tips

### 1. **Start Simple, Add Details**
```
Iteration 1: "Summarize this"
Iteration 2: "Summarize this in 3 bullet points"
Iteration 3: "Summarize this in 3 bullet points, each under 20 words"
```

### 2. **Use Delimiters**
```
Analyze the text below:

---
[Text to analyze]
---

Provide your analysis in the following format:
1. Main theme
2. Key points
3. Overall tone
```

### 3. **Template Approach**
```
Complete this analysis:

Subject: [Topic]
Audience: [Who is this for]
Goal: [What should this achieve]
Format: [How to structure]

[Your input here]
```

### 4. **Provide Reference Material**
```
Using the information below:

[Paste relevant data/docs]

Answer the following question: [Question]
```

## Measuring Zero-Shot Effectiveness

Test these aspects:
- **Accuracy**: Correct output?
- **Format adherence**: Matches specifications?
- **Consistency**: Similar inputs → similar outputs?
- **Completeness**: Addresses all requirements?

## Best Models for Zero-Shot (2026)

### Excellent Zero-Shot:
- **GPT-4 Turbo / GPT-5** ⭐⭐⭐⭐⭐
- **Claude 3.5 Sonnet / Claude 4** ⭐⭐⭐⭐⭐
- **Gemini Ultra** ⭐⭐⭐⭐⭐

### Good Zero-Shot:
- **GPT-4** ⭐⭐⭐⭐
- **Claude 3 Opus** ⭐⭐⭐⭐
- **Gemini Pro** ⭐⭐⭐⭐

### May Need Few-Shot:
- **GPT-3.5 Turbo** ⭐⭐⭐
- **Smaller open-source models** ⭐⭐

## Common Zero-Shot Use Cases

### 1. **Content Generation**
- Blog posts
- Emails
- Product descriptions
- Social media posts

### 2. **Analysis**
- Sentiment analysis
- Topic classification
- Content moderation
- Text summarization

### 3. **Transformation**
- Translation
- Reformatting
- Style transfer
- Simplification

### 4. **Extraction**
- Named entities
- Key information
- Structured data from text

### 5. **Question Answering**
- FAQ responses
- Document Q&A
- General knowledge

### 6. **Code Tasks**
- Function generation
- Bug finding
- Code explanation
- Documentation

## Key Takeaways

1. **Zero-shot is the default** - Try it first before few-shot
2. **Modern LLMs are very capable** at zero-shot tasks
3. **Clear instructions are critical** - Be specific and detailed
4. **Save tokens and time** compared to few-shot
5. **Works best for standard tasks** that models have seen during training
6. **Upgrade to few-shot** if format consistency is critical
7. **Test with your specific use case** - GPT-4 handles most zero-shot well

## Quick Decision Tree

```
Need a task done?
  ↓
Is it a standard task (summarize, translate, explain)?
  ↓ Yes
Try ZERO-SHOT first
  ↓
Does it work well?
  ↓ Yes → Done! ✅
  ↓ No
Need specific format?
  ↓ Yes → Use FEW-SHOT
  ↓ No
Need complex reasoning?
  ↓ Yes → Use CHAIN-OF-THOUGHT
```

## Template

```
# TASK
[Clear, specific task description]

# CONTEXT (optional)
[Background information, audience, purpose]

# INPUT
[Data to process]

# OUTPUT REQUIREMENTS
- Format: [Specify structure]
- Length: [Word/character count]
- Style: [Tone, voice]
- Constraints: [Any limitations]

# ADDITIONAL INSTRUCTIONS (optional)
[Any other specific requirements]
```

## Related Techniques
- **Few-shot learning** - Add examples when zero-shot isn't enough
- **Chain-of-thought** - Add reasoning steps for complex problems
- **Clear instructions** - Foundation of effective zero-shot
- **Role assignment** - Add expertise context
