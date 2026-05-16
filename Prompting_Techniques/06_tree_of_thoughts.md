# Tree of Thoughts (ToT) Prompting

## Overview
Tree of Thoughts (ToT) is an advanced prompting technique that extends Chain-of-Thought by exploring multiple reasoning paths simultaneously. Instead of following a single linear chain of reasoning, ToT generates multiple possible thought sequences (branches), evaluates them, and selects the most promising paths to continue exploration.

## Key Concept
```
Chain-of-Thought: A → B → C → Answer
Tree of Thoughts: A → B1 → C1 → Answer1
                    ↓ B2 → C2 → Answer2
                    ↓ B3 → C3 → Answer3
                    (Evaluate and select best path)
```

## How Tree of Thoughts Works

### The ToT Framework

**Four Key Components:**
1. **Thought Generation** - Generate multiple next steps
2. **Evaluation** - Assess each thought's promise
3. **Search Strategy** - Decide which branches to explore (BFS, DFS)
4. **Backtracking** - Abandon dead ends and try alternatives

### ToT vs CoT vs Zero-Shot

| Method | Paths | Evaluation | Backtracking |
|--------|-------|------------|--------------|
| Zero-Shot | Single | None | No |
| CoT | Single | None | No |
| ToT | Multiple | Yes | Yes |

## When to Use Tree of Thoughts

### ✅ Use ToT When:
- Problem has multiple valid approaches
- Solution requires strategic planning
- Dead ends are possible
- Need to compare alternatives
- Problem is complex with many steps
- Creative solutions needed
- High-stakes decisions

### ❌ Use Simpler Methods When:
- Problem is straightforward
- Only one clear path exists
- Speed/cost is priority
- Simple factual answers needed

## Basic ToT Pattern

### Pattern 1: Generate-Evaluate-Select

```
Problem: [Your problem]

Generate 3 different approaches to solve this:

Approach 1:
[Reasoning path 1]

Approach 2:
[Reasoning path 2]

Approach 3:
[Reasoning path 3]

Now evaluate each approach:
- Which is most likely to succeed?
- What are the risks of each?
- Which is most efficient?

Select the best approach and solve the problem using it.
```

## Examples

### Example 1: Strategic Planning

```
Problem: Our SaaS startup has $500K runway (5 months). We need to reach 
profitability or raise more funding. What should we do?

Step 1: Generate possible strategies

Strategy A: Focus on revenue - aggressive sales push
- Hire 2 sales reps immediately
- Launch paid ads campaign
- Offer discounts for annual plans
- Target enterprise customers

Strategy B: Reduce burn rate - extend runway
- Freeze hiring
- Cut marketing spend
- Renegotiate vendor contracts
- Move to cheaper office space

Strategy C: Fundraising preparation
- Prepare pitch deck
- Improve metrics (growth, retention)
- Start investor conversations
- Build product demos

Step 2: Evaluate each strategy

Strategy A Evaluation:
✅ Pros: Direct revenue impact, could solve problem quickly
❌ Cons: Expensive upfront, takes time to close deals (3-6 months)
⚠️ Risk: High - might spend money without results in time
Score: 6/10

Strategy B Evaluation:
✅ Pros: Immediate impact, extends runway to 8-10 months
❌ Cons: Slows growth, hurts morale, harder to fundraise later
⚠️ Risk: Medium - buys time but doesn't solve core problem
Score: 7/10

Strategy C Evaluation:
✅ Pros: Long-term solution, positions for growth
❌ Cons: Takes 2-3 months minimum, not guaranteed
⚠️ Risk: High if not successful, but high reward
Score: 5/10

Step 3: Select best path

Best approach: Hybrid of B + C
1. Immediate: Reduce burn (Strategy B) - extend runway to 8 months
2. Parallel: Improve metrics and start fundraising (Strategy C)
3. Selective: Small revenue initiatives from A (existing customers)

Final Recommendation:
Week 1-2: Cut burn rate by 40% (no new hires, reduce marketing)
Week 3-4: Prepare investor materials, improve key metrics
Month 2-4: Active fundraising while monitoring cash
Fallback: If no term sheet by month 4, aggressive revenue push
```

### Example 2: Technical Problem Solving

```
Problem: Website is slow (5 second load time). Need to get under 2 seconds.

Step 1: Generate possible causes and solutions

Path A: Frontend Performance
Possible issue: Large JavaScript bundles
Solution approach:
- Code splitting
- Lazy loading
- Remove unused dependencies
Expected impact: 1-2 second improvement

Path B: Backend Performance  
Possible issue: Slow database queries
Solution approach:
- Add database indexes
- Optimize N+1 queries
- Implement caching
Expected impact: 2-3 second improvement

Path C: Network/Infrastructure
Possible issue: No CDN, large images
Solution approach:
- Setup CDN (Cloudflare)
- Optimize images (WebP, compression)
- Enable browser caching
Expected impact: 1-2 second improvement

Step 2: Evaluate feasibility

Path A - Frontend:
Effort: 2-3 days
Cost: $0
Risk: Low
Likelihood of success: 80%

Path B - Backend:
Effort: 3-5 days
Cost: Redis cache ($50/month)
Risk: Medium (might break things)
Likelihood of success: 90%

Path C - Network:
Effort: 1 day
Cost: $20/month CDN
Risk: Very low
Likelihood of success: 95%

Step 3: Select optimal approach

Best strategy: Start with C, then B, then A
1. Day 1: Implement CDN + image optimization (quick win)
2. Day 2-3: Profile backend, add critical indexes
3. Day 4-5: Frontend optimization if still needed

Reasoning: Path C is fastest to implement with highest success rate. 
Path B likely has biggest impact. Path A is backup if needed.
```

### Example 3: Creative Problem (Game of 24)

```
Problem: Using numbers 4, 7, 8, 8, can you make 24 using +, -, ×, ÷?

Step 1: Explore different starting operations

Branch 1: Start with 8 × 8
8 × 8 = 64
Now have: 64, 4, 7
Try: 64 - 4 = 60, then 60 - 7 = 53 ❌
Try: 64 - 7 = 57, then 57 - 4 = 53 ❌
Try: 64 / 4 = 16, then 16 + 7 = 23 ❌ (close!)
Try: 64 / 4 = 16, then 16 - 7 = 9 ❌
Branch 1 looks unpromising.

Branch 2: Start with 8 + 8
8 + 8 = 16
Now have: 16, 4, 7
Try: 16 + 4 = 20, then 20 + 7 = 27 ❌
Try: 16 + 7 = 23, then 23 + 4 = 27 ❌
Try: 16 - 4 = 12, then 12 + 7 = 19 ❌
Try: 16 × 4 = 64, then 64 - 7 = 57 ❌
Branch 2 not working.

Branch 3: Start with 8 - 7
8 - 7 = 1
Now have: 1, 4, 8
Try: 8 × 4 = 32, then 32 - 1 = 31 ❌
Try: 8 + 4 = 12, then 12 × 1 = 12 ❌
Try: 4 - 1 = 3, then 8 × 3 = 24 ✅

Solution found!
(8 - 7) = 1
(4 - 1) = 3
8 × 3 = 24

Answer: 8 × (4 - (8 - 7)) = 24
or written out: 8 × (4 - 8 + 7) = 8 × 3 = 24
```

### Example 4: Writing - Multiple Angles

```
Task: Write an opening paragraph for an article about AI in healthcare.

Generate 3 different angles:

Angle 1: Patient Story (Emotional)
---
"When Sarah's doctors couldn't diagnose her rare condition after months of 
testing, an AI system analyzed her medical history in minutes and identified 
a pattern that saved her life. This isn't science fiction—it's happening now 
in hospitals worldwide, where artificial intelligence is transforming how we 
detect, diagnose, and treat disease."

Evaluation: Strong emotional hook, relatable, but perhaps too anecdotal
Engagement: 9/10 | Credibility: 7/10 | Clarity: 8/10

Angle 2: Data/Statistics (Authoritative)
---
"Healthcare AI is projected to save the medical industry $150 billion annually 
by 2026, with diagnostic accuracy improving by 30-40% over human-only 
approaches. From radiology to drug discovery, machine learning algorithms are 
analyzing millions of data points to catch diseases earlier, predict patient 
outcomes, and personalize treatment plans at unprecedented scale."

Evaluation: Credible and impressive, but less engaging
Engagement: 6/10 | Credibility: 10/10 | Clarity: 8/10

Angle 3: Problem-Solution (Balanced)
---
"Doctors are drowning in data. The average physician must process 40 hours 
of reading per week just to stay current with medical research—an impossible 
task. Enter artificial intelligence: systems that can read every medical 
journal, analyze every patient record, and surface the exact insights doctors 
need, exactly when they need them. The result? Better care, fewer errors, 
and doctors who can focus on what they do best—caring for patients."

Evaluation: Relatable problem, clear solution, balanced tone
Engagement: 8/10 | Credibility: 8/10 | Clarity: 9/10

Selected: Angle 3 - Best balance of engagement and credibility for a 
general audience article.
```

## Advanced ToT Patterns

### Pattern 1: Breadth-First Search (BFS)

```
Problem: [Complex problem]

Level 1: Generate 3 initial approaches
→ Approach A
→ Approach B  
→ Approach C

Evaluate Level 1: Which 2 are most promising?
Selected: A and B (C eliminated)

Level 2: Expand A and B
A → A1, A2, A3
B → B1, B2, B3

Evaluate Level 2: Which 2 are most promising?
Selected: A2 and B1

Level 3: Expand A2 and B1 to completion
A2 → Full solution
B1 → Full solution

Compare final solutions and select best.
```

### Pattern 2: Depth-First Search (DFS)

```
Problem: [Complex problem]

Try Path A:
Step 1 → Step 2 → Step 3 → STUCK (dead end)
Backtrack to Step 2
Try alternative Step 3b → Success!

If Path A fails completely:
Backtrack to beginning
Try Path B:
Step 1 → Step 2 → Step 3 → Success!
```

### Pattern 3: Self-Evaluation with Scoring

```
Problem: [Your problem]

Generate 3 solutions, then score each on:
1. Feasibility (1-10)
2. Impact (1-10)
3. Time to implement (1-10)
4. Risk (1-10, lower is better)

Solution A: [description]
Scores: F:7, I:8, T:6, R:5 → Total: 26/40

Solution B: [description]
Scores: F:9, I:7, T:8, R:3 → Total: 31/40

Solution C: [description]
Scores: F:6, I:9, T:5, R:7 → Total: 27/40

Winner: Solution B (highest total score)
```

### Pattern 4: Pros-Cons Tree

```
Decision: Should we migrate from MongoDB to PostgreSQL?

Option 1: Migrate to PostgreSQL
│
├─ Pros:
│  ├─ Better ACID compliance
│  ├─ Mature tooling
│  ├─ Team knows SQL
│  └─ Lower costs at scale
│
└─ Cons:
   ├─ 3-month migration effort
   ├─ Potential downtime
   ├─ Learning curve for complex queries
   └─ Risk of data loss

Score: +6 pros, -4 cons, Net: +2

Option 2: Stay with MongoDB
│
├─ Pros:
│  ├─ No migration needed
│  ├─ Team expertise
│  ├─ Fast for current use case
│  └─ Flexible schema
│
└─ Cons:
   ├─ Scaling costs
   ├─ Consistency issues
   ├─ Limited to specific use cases
   └─ Technical debt growing

Score: +4 pros, -4 cons, Net: 0

Option 3: Hybrid (PostgreSQL + MongoDB)
│
├─ Pros:
│  ├─ Best of both worlds
│  ├─ Gradual migration
│  └─ Reduced risk
│
└─ Cons:
   ├─ Operational complexity
   ├─ Two systems to maintain
   ├─ Data synchronization challenges
   └─ Higher initial costs

Score: +3 pros, -4 cons, Net: -1

Decision: Migrate to PostgreSQL (Option 1)
Reasoning: Highest net score, long-term benefits outweigh short-term costs
```

## Implementation with Code

### Python Example

```python
from openai import OpenAI

client = OpenAI()

def tree_of_thoughts(problem, num_branches=3):
    """
    Implement Tree of Thoughts prompting
    """
    
    # Step 1: Generate multiple thought branches
    prompt_generate = f"""
    Problem: {problem}
    
    Generate {num_branches} different approaches to solve this problem.
    For each approach, explain the reasoning.
    
    Format:
    Approach 1: [description]
    Approach 2: [description]
    Approach 3: [description]
    """
    
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt_generate}],
        temperature=0.8  # Higher for diversity
    )
    
    branches = response.choices[0].message.content
    
    # Step 2: Evaluate each branch
    prompt_evaluate = f"""
    Here are {num_branches} approaches to solve the problem:
    
    {branches}
    
    Evaluate each approach on:
    1. Likelihood of success (1-10)
    2. Difficulty to implement (1-10, lower is better)
    3. Time required (1-10, lower is better)
    
    Provide scores and select the best approach.
    """
    
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt_evaluate}],
        temperature=0.3  # Lower for more analytical
    )
    
    evaluation = response.choices[0].message.content
    
    # Step 3: Execute best approach
    prompt_execute = f"""
    Based on this evaluation:
    {evaluation}
    
    Now solve the original problem using the best approach:
    {problem}
    
    Provide a detailed, step-by-step solution.
    """
    
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt_execute}],
        temperature=0.5
    )
    
    return response.choices[0].message.content

# Usage
problem = "Design a scalable chat application for 1 million users"
solution = tree_of_thoughts(problem)
print(solution)
```

### Iterative ToT with Backtracking

```python
def tot_with_backtracking(problem, max_depth=3):
    """
    Tree of Thoughts with backtracking on dead ends
    """
    
    def explore_path(problem, current_path, depth):
        if depth >= max_depth:
            return current_path
        
        # Generate next steps
        prompt = f"""
        Problem: {problem}
        Current path: {current_path}
        
        Generate 2 possible next steps.
        
        Step A: [description]
        Step B: [description]
        
        For each step, assess if it's promising (Yes/No/Maybe).
        """
        
        response = client.chat.completions.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}],
            temperature=0.7
        )
        
        next_steps = response.choices[0].message.content
        
        # Parse and evaluate
        # Try Step A
        path_a = current_path + " → Step A"
        result_a = explore_path(problem, path_a, depth + 1)
        
        if is_successful(result_a):
            return result_a
        
        # Backtrack and try Step B
        path_b = current_path + " → Step B"
        result_b = explore_path(problem, path_b, depth + 1)
        
        return result_b
    
    return explore_path(problem, "Start", 0)
```

## Best Practices

### 1. **Start Broad, Then Narrow**
```
❌ Bad: Generate 10 approaches
✅ Good: Generate 3-4 diverse approaches, evaluate, then expand top 2
```

### 2. **Use Clear Evaluation Criteria**
```
✅ Good criteria:
- Feasibility (can we actually do this?)
- Impact (how much does it help?)
- Time (how long will it take?)
- Risk (what could go wrong?)
- Cost (what's the investment?)
```

### 3. **Temperature Settings**
```python
# Generation phase (explore diversity)
temperature=0.7-0.9

# Evaluation phase (be analytical)
temperature=0.2-0.4

# Execution phase (balanced)
temperature=0.5-0.6
```

### 4. **Limit Depth**
```
Level 1: 3-4 options
Level 2: Top 2-3 expanded
Level 3: Best 1-2 fully developed

Don't go deeper than 3-4 levels (gets expensive and complex)
```

### 5. **Document the Path**
Keep track of why branches were eliminated:
```
Branch A: Eliminated (too expensive)
Branch B: Eliminated (not feasible)
Branch C: Selected (best balance)
```

## When to Use What

| Problem Type | Best Method |
|-------------|-------------|
| Simple factual | Zero-shot |
| Single clear path | Chain-of-Thought |
| Multiple approaches | Tree of Thoughts |
| Need creativity | Tree of Thoughts |
| Strategic decision | Tree of Thoughts |
| Time-sensitive | Chain-of-Thought |
| Cost-sensitive | Chain-of-Thought |

## Common Pitfalls

### ❌ Pitfall 1: Too Many Branches
```
❌ Generating 10+ branches
✅ Generate 3-4 quality branches
```

### ❌ Pitfall 2: No Real Evaluation
```
❌ "All approaches look good"
✅ Score objectively, eliminate weaker options
```

### ❌ Pitfall 3: Going Too Deep
```
❌ 5-6 levels of branching
✅ 2-3 levels maximum
```

### ❌ Pitfall 4: Not Backtracking
```
❌ Stuck with first path even if failing
✅ Recognize dead ends, try alternatives
```

## Key Takeaways

1. **ToT explores multiple paths** instead of just one
2. **Evaluation is critical** - score and compare branches
3. **Best for complex problems** with multiple solutions
4. **Higher cost** than CoT (more tokens, multiple calls)
5. **Backtracking allowed** - try alternatives if stuck
6. **Not always necessary** - use simpler methods when possible
7. **Temperature matters** - adjust for each phase
8. **Document reasoning** - track why paths were chosen/eliminated

## Summary Table

| Feature | Zero-Shot | CoT | ToT |
|---------|-----------|-----|-----|
| Paths explored | 1 | 1 | Multiple |
| Reasoning shown | No | Yes | Yes |
| Evaluation | No | No | Yes |
| Backtracking | No | No | Yes |
| Cost | $ | $$ | $$$ |
| Best for | Simple | Complex | Strategic |
| Token usage | Low | Medium | High |

---

**Remember:** Tree of Thoughts is powerful but expensive. Use it when the problem's complexity justifies the additional cost and time. For most tasks, Chain-of-Thought is sufficient.
