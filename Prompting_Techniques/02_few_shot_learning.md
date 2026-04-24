# Few-Shot Learning (Examples in Prompt)

## Overview
Few-shot learning is a technique where you provide the LLM with a few examples of the desired input-output pattern before asking it to perform the task. The model learns from these examples to understand the format, style, and logic you want.

## How It Works
```
Example 1: [Input] → [Desired Output]
Example 2: [Input] → [Desired Output]
Example 3: [Input] → [Desired Output]

Now do the same for: [Your actual input]
```

## When to Use Few-Shot

### ✅ Use When:
- You need consistent output format
- The task has a specific pattern
- Instructions alone are ambiguous
- You want a particular style or tone
- Complex transformations are needed
- Edge cases need demonstration

### ❌ Don't Use When:
- The task is straightforward (zero-shot works)
- You can explain clearly in words
- You don't have good examples
- Examples would be longer than instructions

## Examples

### Example 1: Data Extraction

```
Extract the name, email, and phone number from the text.

Example:
Text: "Hi, I'm Sarah Johnson. You can reach me at sarah.j@email.com or call 555-0123."
Output: {"name": "Sarah Johnson", "email": "sarah.j@email.com", "phone": "555-0123"}

Example:
Text: "John Smith here, email: jsmith@company.org, mobile: 555-9876"
Output: {"name": "John Smith", "email": "jsmith@company.org", "phone": "555-9876"}

Example:
Text: "Contact Mike at mike.wilson@tech.com"
Output: {"name": "Mike", "email": "mike.wilson@tech.com", "phone": null}

Now extract from:
Text: "Please get in touch with Dr. Emily Chen at emily.chen@hospital.org or 555-4567"
```

### Example 2: Sentiment Classification

```
Classify the sentiment as positive, negative, or neutral.

Example:
Review: "This product exceeded my expectations! Amazing quality."
Sentiment: positive

Example:
Review: "Terrible experience. Would not recommend to anyone."
Sentiment: negative

Example:
Review: "The item arrived on time. Standard quality."
Sentiment: neutral

Example:
Review: "Great price but the customer service could be better."
Sentiment: neutral

Now classify:
Review: "Absolutely love this! Best purchase I've made this year."
```

### Example 3: Code Generation Pattern

```
Convert the English description to a Python function.

Example:
Description: "Check if a number is even"
Function:
def is_even(n):
    return n % 2 == 0

Example:
Description: "Find the maximum value in a list"
Function:
def find_max(lst):
    return max(lst) if lst else None

Example:
Description: "Count vowels in a string"
Function:
def count_vowels(s):
    return sum(1 for c in s.lower() if c in 'aeiou')

Now generate:
Description: "Reverse the words in a sentence"
```

### Example 4: Text Transformation

```
Convert casual language to professional business language.

Example:
Casual: "Hey, just wanted to check if you got my email?"
Professional: "Good morning, I wanted to follow up regarding the email I sent earlier."

Example:
Casual: "Can we push the meeting back? Something came up."
Professional: "Would it be possible to reschedule our meeting? An urgent matter has arisen."

Example:
Casual: "Thanks a bunch for the help!"
Professional: "Thank you for your valuable assistance."

Now convert:
Casual: "Sorry, I totally forgot about the deadline."
```

### Example 5: Complex Reasoning Pattern

```
Solve the word problem step by step.

Example:
Problem: "A store has 45 apples. They sell 12 in the morning and 8 in the afternoon. How many are left?"
Solution:
- Starting apples: 45
- Sold in morning: 12
- Sold in afternoon: 8
- Total sold: 12 + 8 = 20
- Remaining: 45 - 20 = 25 apples

Example:
Problem: "A book costs $15. If you buy 3 books, you get 10% off. How much for 3 books?"
Solution:
- Price per book: $15
- Number of books: 3
- Subtotal: 3 × $15 = $45
- Discount: 10% of $45 = $4.50
- Final price: $45 - $4.50 = $40.50

Now solve:
Problem: "A train travels 60 mph for 2.5 hours. How far does it go?"
```

## Best Practices

### 1. **Use 2-5 Examples**
- 1 example = one-shot (sometimes enough)
- 2-3 examples = ideal for most cases
- 4-5 examples = complex patterns
- 6+ examples = diminishing returns, use fine-tuning instead

### 2. **Show Edge Cases**
Include examples that demonstrate:
- Normal cases
- Edge cases (empty input, nulls)
- Boundary conditions
- Variety in input format

```
Example 1: Standard case
Example 2: Edge case (empty)
Example 3: Edge case (multiple values)
```

### 3. **Maintain Consistency**
All examples should follow the same format:
```
❌ Inconsistent:
Input: "text" → Output: result
"another text" => different format

✅ Consistent:
Input: "text"
Output: result

Input: "another text"
Output: another result
```

### 4. **Use Clear Delimiters**
```
Example 1:
---
[Input]
[Output]
---

Example 2:
---
[Input]
[Output]
---
```

### 5. **Order Examples Strategically**
- Simple to complex
- Common to rare
- Generic to specific

## Patterns

### Pattern 1: Input/Output Pairs
```
Input: [data]
Output: [result]

Input: [data]
Output: [result]

Input: [your data]
Output:
```

### Pattern 2: Labeled Examples
```
Example 1 (standard case):
Q: [question]
A: [answer]

Example 2 (edge case):
Q: [question]
A: [answer]

Your turn:
Q: [question]
A:
```

### Pattern 3: Conversational Format
```
User: [request]
Assistant: [response]

User: [request]
Assistant: [response]

User: [your request]
Assistant:
```

## Advanced Techniques

### Dynamic Examples
Select examples based on similarity to the input:
```python
# Pseudo-code
examples = find_most_similar_examples(user_input, example_database, k=3)
prompt = format_few_shot_prompt(examples, user_input)
```

### Chained Few-Shot
Break complex tasks into steps with examples for each:
```
Step 1 Examples: [Input → Intermediate]
Step 2 Examples: [Intermediate → Output]

Now apply:
Step 1: [Your input]
Step 2: [Use Step 1 output]
```

### Few-Shot with Explanations
Include reasoning in examples:
```
Input: "The cat chased the mouse"
Thought: Subject is "cat", action is "chased", object is "mouse"
Output: {"subject": "cat", "action": "chased", "object": "mouse"}

Input: "Birds fly south"
Thought: Subject is "birds", action is "fly", direction is "south"
Output: {"subject": "birds", "action": "fly", "direction": "south"}
```

## Combining with Other Techniques

### Few-Shot + Clear Instructions
```
Task: Extract product information from reviews.

Format: JSON with keys: product, rating, pros, cons

Example 1:
Review: "The wireless mouse is great! Battery lasts forever but it's a bit pricey."
Output: {"product": "wireless mouse", "rating": "positive", "pros": ["long battery life"], "cons": ["expensive"]}

Example 2:
[Another example]

Now extract from: [Your review]
```

### Few-Shot + Role Assignment
```
You are a professional translator specializing in technical documentation.

Example 1:
English: "The API endpoint returns a JSON response."
Spanish: "El punto final de la API devuelve una respuesta JSON."

Example 2:
[Another example]

Translate: [Your text]
```

## Comparison with Zero-Shot

| Aspect | Zero-Shot | Few-Shot |
|--------|-----------|----------|
| **Setup** | Just instructions | Instructions + examples |
| **Token Cost** | Lower | Higher |
| **Consistency** | Variable | More consistent |
| **Use Case** | Simple, clear tasks | Complex, nuanced tasks |
| **Quality** | Good for GPT-4 | Better for specific formats |

## Troubleshooting

### Problem: Model Doesn't Follow Pattern
**Solution:**
- Add more examples (3-5)
- Make examples more diverse
- Ensure examples are consistent
- Add explicit instructions before examples

### Problem: Too Much Token Usage
**Solution:**
- Reduce to 2-3 best examples
- Shorten examples while keeping pattern clear
- Consider fine-tuning for frequent tasks

### Problem: Inconsistent Outputs
**Solution:**
- Show edge cases in examples
- Add explicit constraints
- Use temperature=0 for consistency
- Ensure examples cover input variety

## Template

```
# TASK DESCRIPTION
[Brief description of what you want]

# FORMAT
[Specify output format]

# EXAMPLES

Example 1:
Input: [example input 1]
Output: [example output 1]

Example 2:
Input: [example input 2]
Output: [example output 2]

Example 3:
Input: [example input 3]
Output: [example output 3]

# YOUR TASK

Input: [actual input]
Output:
```

## Code Implementation

```python
def create_few_shot_prompt(examples, user_input):
    """
    Create a few-shot prompt from examples.
    
    Args:
        examples: List of (input, output) tuples
        user_input: The actual input to process
    
    Returns:
        Formatted prompt string
    """
    prompt = "Complete the task following these examples:\n\n"
    
    for i, (inp, out) in enumerate(examples, 1):
        prompt += f"Example {i}:\n"
        prompt += f"Input: {inp}\n"
        prompt += f"Output: {out}\n\n"
    
    prompt += "Now complete:\n"
    prompt += f"Input: {user_input}\n"
    prompt += "Output:"
    
    return prompt

# Usage
examples = [
    ("What's 2+2?", "4"),
    ("What's 10-3?", "7"),
    ("What's 5*6?", "30")
]

prompt = create_few_shot_prompt(examples, "What's 8+15?")
```

## Measuring Effectiveness

Test with these metrics:
- **Format adherence**: Does output match example format?
- **Consistency**: Similar inputs → similar outputs?
- **Edge case handling**: Correct on unusual inputs?
- **Quality**: Better than zero-shot?

## Key Takeaways

1. **Few-shot learning teaches by example** rather than instruction
2. **2-3 good examples** are usually sufficient
3. **Show variety and edge cases** in examples
4. **Consistency in format** is critical
5. **Costs more tokens** but improves output quality
6. **Great for specific formats** and nuanced tasks
7. **Combine with clear instructions** for best results

## Related Techniques
- **Zero-shot prompting** - No examples needed
- **Chain-of-thought** - Show reasoning in examples
- **Clear instructions** - Foundation for any prompt
