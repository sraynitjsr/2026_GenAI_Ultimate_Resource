# Chain-of-Thought (CoT) Prompting

## Overview
Chain-of-Thought (CoT) prompting is a technique that encourages LLMs to break down complex problems into intermediate reasoning steps before arriving at a final answer. By "showing its work," the model produces more accurate and reliable outputs, especially for tasks requiring logic, math, or multi-step reasoning.

## Key Concept
Instead of jumping directly to an answer, the model explicitly reasons through the problem step by step.

```
Traditional: Question → Answer
Chain-of-Thought: Question → Step 1 → Step 2 → Step 3 → Answer
```

## Why Chain-of-Thought Works

### Benefits:
1. **Improved Accuracy** - Reduces errors in complex reasoning
2. **Transparency** - You can see how the model arrived at the answer
3. **Debuggability** - Easy to spot where reasoning went wrong
4. **Better Complex Problem Solving** - Multi-step problems handled better
5. **Reduced Hallucinations** - Structured thinking reduces wild guesses

### When to Use CoT:
- ✅ Mathematical problems
- ✅ Logic puzzles
- ✅ Multi-step reasoning
- ✅ Analysis requiring justification
- ✅ Complex decision-making
- ✅ Problems with intermediate steps
- ✅ When you need to verify reasoning

### When NOT to Use CoT:
- ❌ Simple factual questions
- ❌ Single-step tasks
- ❌ When speed/cost is priority
- ❌ Creative writing (unless you want structured creativity)

## Types of Chain-of-Thought

### 1. **Few-Shot CoT** (With Examples)
Show examples of step-by-step reasoning:

```
Q: A restaurant has 23 tables. Each table can seat 4 people. 
   If 78 people show up, how many will not get a seat?

A: Let's think step by step:
- Total tables: 23
- Seats per table: 4
- Total seats available: 23 × 4 = 92 seats
- People who showed up: 78
- Since 78 < 92, everyone can be seated
- People without seats: 0

Q: A store bought 144 items for $6 each and sold them for $10 each. 
   What was the total profit?

A: Let's think step by step:
- Items bought: 144
- Cost per item: $6
- Total cost: 144 × $6 = $864
- Selling price per item: $10
- Total revenue: 144 × $10 = $1,440
- Profit = Revenue - Cost
- Profit = $1,440 - $864 = $576

Now solve:
Q: A bakery makes 50 loaves of bread per day. They sell each loaf for $4. 
   If flour costs $0.50 per loaf, what is the daily profit?

A: Let's think step by step:
```

### 2. **Zero-Shot CoT** (Just Ask for Reasoning)
Simply add: "Let's think step by step" or "Let's solve this step by step"

```
Question: If it takes 5 machines 5 minutes to make 5 widgets, 
how long would it take 100 machines to make 100 widgets?

Let's think step by step:
```

The model will automatically break down its reasoning.

### 3. **Manual CoT** (You Provide the Structure)
Explicitly guide the reasoning process:

```
Analyze whether our company should expand to Europe.

Think through this systematically:

Step 1: Market Analysis
[Model analyzes market]

Step 2: Financial Feasibility
[Model analyzes costs/revenue]

Step 3: Risks
[Model identifies risks]

Step 4: Recommendation
[Model provides final recommendation]
```

## Examples

### Example 1: Math Problem

**❌ Without CoT:**
```
Q: A train travels 120 miles in 2 hours, then 180 miles in 3 hours. 
   What was the average speed?

A: 60 mph
```
*(Might be wrong - unclear reasoning)*

**✅ With CoT:**
```
Q: A train travels 120 miles in 2 hours, then 180 miles in 3 hours. 
   What was the average speed?

Let's solve this step by step:

A: 
- First segment: 120 miles in 2 hours
- Second segment: 180 miles in 3 hours
- Total distance: 120 + 180 = 300 miles
- Total time: 2 + 3 = 5 hours
- Average speed = Total distance ÷ Total time
- Average speed = 300 ÷ 5 = 60 mph

The average speed was 60 mph.
```

### Example 2: Logic Problem

```
Q: All roses are flowers. Some flowers fade quickly. Does this mean some roses fade quickly?

Let's reason through this step by step:

A:
Step 1: Identify what we know
- Premise 1: All roses are flowers
- Premise 2: Some flowers fade quickly

Step 2: Analyze the relationship
- Roses are a subset of flowers
- We know SOME flowers fade quickly, but not which ones

Step 3: Consider possibilities
- It's possible that the quick-fading flowers are roses
- It's also possible that they are other types of flowers (not roses)

Step 4: Conclusion
- We CANNOT conclude that some roses fade quickly
- The statement is logically invalid
- We would need additional information about which specific flowers fade quickly

Answer: No, we cannot conclude that some roses fade quickly based on the given information.
```

### Example 3: Business Decision

```
Should we invest in this startup?

Company details:
- Revenue: $2M annually, growing 10% yearly
- Asking for: $500K for 20% equity
- Burn rate: $150K/month
- Runway: 8 months
- Market size: $500M

Let's analyze step by step:

Step 1: Valuation
- Asking $500K for 20% implies $2.5M valuation
- Current revenue: $2M
- Valuation is 1.25x revenue (reasonable for early-stage)

Step 2: Financial Health
- Burn rate: $150K/month = $1.8M/year
- Revenue: $2M/year
- Nearly break-even, which is good
- 8-month runway is concerning - needs money soon

Step 3: Growth
- 10% yearly growth is slow for a startup
- In a $500M market, significant room to grow
- Growth rate is a red flag

Step 4: Risk Assessment
- Positive: Near break-even, reasonable valuation
- Negative: Slow growth, short runway, might need more capital soon

Step 5: Recommendation
- Do NOT invest unless:
  - They show a clear path to accelerate growth
  - They can explain why growth has been slow
  - You see evidence of a catalyst for faster growth
- The numbers alone don't justify the investment

Final Answer: Not recommended based on current information.
```

### Example 4: Code Debugging

```
Find the bug in this code:

def calculate_average(numbers):
    total = 0
    for num in numbers:
        total += num
    return total / len(numbers)

Let's debug step by step:

Step 1: Identify the function's purpose
- Calculate average of a list of numbers

Step 2: Trace through the logic
- Initialize total to 0
- Sum all numbers in the list
- Divide total by count of numbers

Step 3: Consider edge cases
- What if numbers list is empty?
- len(numbers) would be 0
- Division by zero error!

Step 4: Additional issues
- What if numbers contains non-numeric values?
- No type checking or error handling

Step 5: Bugs identified
1. **Critical**: No check for empty list (ZeroDivisionError)
2. **Important**: No type validation

Step 6: Fixed version
def calculate_average(numbers):
    if not numbers:
        return 0  # or raise ValueError("Cannot calculate average of empty list")
    
    if not all(isinstance(n, (int, float)) for n in numbers):
        raise TypeError("All elements must be numbers")
    
    total = sum(numbers)
    return total / len(numbers)

Answer: The main bug is potential division by zero with an empty list.
```

### Example 5: Text Analysis

```
Analyze the sentiment and intent of this customer message:

"I've been waiting for my order for 3 weeks now. The tracking hasn't updated 
in 10 days. I've called support twice and no one has called me back. This is 
extremely disappointing. I need this resolved immediately or I want a full refund."

Let's break this down step by step:

Step 1: Identify explicit statements
- Order delayed: 3 weeks
- Tracking issue: No updates in 10 days
- Support failure: Called twice, no callbacks
- Emotional state: "extremely disappointing"
- Demand: Resolution or refund

Step 2: Sentiment Analysis
- Overall sentiment: Strongly negative
- Frustration level: High
- Anger level: Moderate to high
- Tone: Assertive but professional

Step 3: Intent Classification
- Primary intent: Get order status
- Secondary intent: Escalate complaint
- Tertiary intent: Seek resolution or compensation

Step 4: Urgency Assessment
- Language: "immediately" indicates high urgency
- Multiple contact attempts: Shows escalating concern
- Ultimatum given: Suggests near loss of customer

Step 5: Recommended Action
- Priority: HIGH - immediate response needed
- Assign to: Senior support or manager
- Required actions:
  1. Investigate order immediately
  2. Call customer directly (don't email)
  3. Provide concrete resolution timeline
  4. Offer compensation (discount/refund)
  5. Apologize for support failures

Final Analysis:
- Sentiment: Negative (8/10 severity)
- Intent: Complaint + Resolution
- Urgency: Critical
- Risk: High churn risk if not resolved immediately
```

## Advanced CoT Techniques

### 1. **Self-Consistency CoT**
Generate multiple reasoning paths and pick the most consistent answer:

```
Problem: [Math problem]

Let's solve this 3 different ways:

Approach 1: [Method 1 with steps]
Answer: X

Approach 2: [Method 2 with steps]
Answer: X

Approach 3: [Method 3 with steps]
Answer: Y

Conclusion: Since 2 out of 3 approaches give answer X, 
the correct answer is likely X.
```

### 2. **Tree of Thoughts (ToT)**
Explore multiple reasoning branches:

```
Problem: [Complex problem]

Path A:
  If we assume X:
    → Step 1A
    → Step 2A
    → Conclusion A (seems wrong)

Path B:
  If we assume Y:
    → Step 1B
    → Step 2B
    → Conclusion B (makes sense)

Path C:
  If we assume Z:
    → Step 1C
    → Step 2C
    → Conclusion C (possible)

Evaluation: Path B seems most logical because...
```

### 3. **Verification CoT**
Have the model verify its own reasoning:

```
Problem: [Problem]

Initial Solution:
[Model's step-by-step solution]

Now let's verify this solution:
- Check step 1: [Verification]
- Check step 2: [Verification]
- Check step 3: [Verification]
- Check final answer: [Verification]

Conclusion: [Confirmed or corrected]
```

### 4. **Meta-Cognitive CoT**
Model thinks about its thinking:

```
Problem: [Problem]

Before solving, let me consider:
1. What type of problem is this?
2. What approach would work best?
3. What are potential pitfalls?

Now solving:
[Step-by-step solution]

Reflection:
- Did I use the right approach?
- Are there any assumptions I made?
- How confident am I in this answer?
```

## Best Practices

### 1. **Use Clear Step Markers**
```
✅ Good:
Step 1: [First step]
Step 2: [Second step]
Step 3: [Third step]

❌ Less clear:
First... then... after that...
```

### 2. **Show Intermediate Calculations**
```
✅ Good:
Total = 25 × 4 = 100

❌ Less useful:
Total = 100
```

### 3. **Name Your Steps**
```
✅ Good:
Step 1 (Identify knowns): ...
Step 2 (Calculate subtotal): ...
Step 3 (Apply discount): ...

❌ Generic:
Step 1: ...
Step 2: ...
Step 3: ...
```

### 4. **Include Reasoning, Not Just Math**
```
✅ Good:
Since the discount only applies to orders over $100, 
and our total is $95, we cannot apply the discount.

❌ Just calculation:
$95 < $100
```

### 5. **Explicit Final Answer**
```
✅ Good:
Therefore, the final answer is: 42 units

❌ Unclear:
So we get 42
```

## Common Patterns

### Pattern 1: Problem-Solving
```
Problem: [Question]

Let's solve this step by step:
1. What do we know?
2. What do we need to find?
3. What's the approach?
4. Calculate
5. Verify
6. Final answer
```

### Pattern 2: Decision-Making
```
Decision: [What to decide]

Let's think through this systematically:
1. Options available
2. Criteria for decision
3. Pros and cons of each option
4. Evaluation against criteria
5. Recommendation
6. Justification
```

### Pattern 3: Analysis
```
Topic: [What to analyze]

Let's break this down:
1. Key components
2. Relationships between components
3. Patterns observed
4. Implications
5. Conclusions
```

## Code Implementation

```python
def chain_of_thought_prompt(question, num_steps=None):
    """
    Create a chain-of-thought prompt.
    
    Args:
        question: The question to solve
        num_steps: Optional number of steps to guide reasoning
    
    Returns:
        Formatted prompt
    """
    prompt = f"Question: {question}\n\n"
    
    if num_steps:
        prompt += f"Let's solve this in {num_steps} steps:\n\n"
        for i in range(1, num_steps + 1):
            prompt += f"Step {i}: \n"
    else:
        prompt += "Let's think through this step by step:\n\n"
    
    return prompt

# Usage
prompt = chain_of_thought_prompt(
    "If a car rental costs $50/day plus $0.25/mile, how much for 3 days and 200 miles?"
)

# With OpenAI
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": prompt}],
    temperature=0  # Lower temperature for consistent reasoning
)
```

## Comparison with Other Techniques

| Aspect | Standard | CoT | Few-Shot CoT |
|--------|----------|-----|--------------|
| **Accuracy (complex)** | Lower | Higher | Highest |
| **Tokens Used** | Low | Medium | High |
| **Setup Time** | Fast | Fast | Slow |
| **Transparency** | None | High | High |
| **Best For** | Simple | Complex | Complex with examples |

## Troubleshooting

### Problem: Model skips steps
**Solution:** Be more explicit:
```
Let's solve this in exactly 5 steps. 
Do not skip any step. Show all calculations.
```

### Problem: Reasoning is wrong
**Solution:** Add verification:
```
After solving, verify each step is correct.
```

### Problem: Too verbose
**Solution:** Set constraints:
```
Think step by step, but keep each step under 20 words.
```

### Problem: Inconsistent format
**Solution:** Provide structure:
```
Use this exact format:
Step 1 (Description): [work]
Step 2 (Description): [work]
Final Answer: [answer]
```

## Key Takeaways

1. **CoT improves accuracy** on complex reasoning tasks
2. **"Let's think step by step"** is often enough (zero-shot CoT)
3. **Show examples** for consistent formatting (few-shot CoT)
4. **Lower temperature** (0-0.3) for more reliable reasoning
5. **Verify reasoning** is correct, not just the answer
6. **More tokens** used, but higher quality output
7. **Essential for** math, logic, analysis, debugging
8. **Optional for** simple factual questions

## Related Techniques
- **Tree of Thoughts** - Multiple reasoning branches
- **Self-Consistency** - Multiple reasoning paths
- **ReAct** - Reasoning + Acting for agents
- **Few-shot learning** - Often combined with CoT
- **Zero-shot prompting** - Can trigger CoT with simple phrases

## Quick Reference

**When to use CoT:**
- Math problems ✓
- Logic puzzles ✓
- Multi-step reasoning ✓
- Complex analysis ✓
- Decision-making ✓
- Debugging ✓

**Simple trigger phrases:**
- "Let's think step by step"
- "Let's solve this systematically"
- "Let's break this down"
- "Let's reason through this"
- "Show your work"
- "Explain your reasoning"

**Remember:** Chain-of-Thought = Making the model's reasoning explicit and structured.
