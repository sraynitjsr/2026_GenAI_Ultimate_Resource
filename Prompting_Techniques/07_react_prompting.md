# ReAct (Reasoning + Acting) Prompting

## Overview
ReAct is a prompting paradigm that combines **Reasoning** (thinking through a problem) with **Acting** (taking concrete actions using tools/functions). The model alternates between thinking about what to do next and executing actions, creating a synergy between internal reasoning and external tool use.

## Key Concept
```
Traditional: Question → Answer
Chain-of-Thought: Question → Think → Answer
ReAct: Question → Think → Act → Observe → Think → Act → ... → Answer
```

**The ReAct Loop:**
1. **Thought** - Reason about the current situation
2. **Action** - Execute a tool/function
3. **Observation** - See the result
4. **Repeat** until goal is achieved

## Why ReAct?

### Problems ReAct Solves
❌ **Pure reasoning fails** when external information is needed
❌ **Pure action fails** without strategic planning
❌ **Hallucinations** occur when model lacks real data

✅ **ReAct succeeds** by combining both:
- Reasoning guides which actions to take
- Actions provide real data to inform reasoning
- Observable results ground the model in reality

### Benefits
1. **Reduced hallucination** - Uses real data, not guesses
2. **Transparency** - You see the model's thinking process
3. **Debuggability** - Can identify where reasoning or action failed
4. **Flexibility** - Can use any tools/APIs
5. **Grounded in reality** - Observations provide facts

## Basic ReAct Pattern

```
Question: [Your question]

Thought 1: [What I should do first]
Action 1: [Tool to use] with [parameters]
Observation 1: [Result from tool]

Thought 2: [What this means and what to do next]
Action 2: [Next tool] with [parameters]
Observation 2: [Result from tool]

Thought 3: [Analysis and conclusion]
Answer: [Final answer based on observations]
```

## Examples

### Example 1: Web Research Task

```
Question: What is the current stock price of Tesla and how does it compare to 
a year ago?

Thought 1: I need to get Tesla's current stock price first.
Action 1: search_web("Tesla stock price today")
Observation 1: Tesla (TSLA) is trading at $182.47 as of May 16, 2026

Thought 2: Now I need the price from a year ago (May 2025).
Action 2: search_web("Tesla stock price May 2025")
Observation 2: Tesla was trading at $167.30 on May 16, 2025

Thought 3: I can now calculate the change and compare.
Action 3: calculate("182.47 - 167.30")
Observation 3: 15.17

Thought 4: Let me calculate the percentage change.
Action 4: calculate("(15.17 / 167.30) * 100")
Observation 4: 9.07%

Thought 5: I have all the information needed to answer.
Answer: Tesla's current stock price is $182.47 (as of May 16, 2026), compared 
to $167.30 a year ago. This represents an increase of $15.17, or approximately 
9.07% growth year-over-year.
```

### Example 2: Data Analysis Task

```
Question: Analyze our sales data for Q1 2026 and identify the top performing 
product category.

Thought 1: I need to first load the Q1 sales data.
Action 1: query_database("SELECT * FROM sales WHERE date BETWEEN '2026-01-01' 
AND '2026-03-31'")
Observation 1: Retrieved 15,847 sales records

Thought 2: Now I need to group by category and sum the revenue.
Action 2: query_database("SELECT category, SUM(revenue) as total_revenue FROM 
sales WHERE date BETWEEN '2026-01-01' AND '2026-03-31' GROUP BY category 
ORDER BY total_revenue DESC")
Observation 2: 
- Electronics: $2,450,000
- Home & Garden: $1,820,000
- Clothing: $1,650,000
- Sports: $980,000
- Books: $450,000

Thought 3: Electronics is the clear winner. Let me get more details about 
what's driving this.
Action 3: query_database("SELECT product_name, SUM(revenue) as revenue FROM 
sales WHERE category = 'Electronics' AND date BETWEEN '2026-01-01' AND 
'2026-03-31' GROUP BY product_name ORDER BY revenue DESC LIMIT 5")
Observation 3:
- Laptop Pro X1: $680,000
- Wireless Headphones: $520,000
- Smartphone Y2: $490,000
- Tablet Z3: $340,000
- Smart Watch: $280,000

Thought 4: I now have a complete picture to answer the question.
Answer: Electronics is the top performing category in Q1 2026 with $2.45M in 
revenue (34% of total sales). The Laptop Pro X1 is the bestselling product at 
$680K. Electronics significantly outperformed the second-place category 
(Home & Garden at $1.82M) by 35%.
```

### Example 3: Customer Service Task

```
Question: Customer ID 12847 is asking about their order status.

Thought 1: I need to look up this customer's recent orders.
Action 1: query_crm("SELECT * FROM orders WHERE customer_id = 12847 ORDER BY 
order_date DESC LIMIT 5")
Observation 1: Customer has 2 active orders:
- Order #98234: Placed May 10, Status: Shipped
- Order #97556: Placed April 28, Status: Delivered

Thought 2: Let me get tracking info for the shipped order.
Action 2: get_tracking("98234")
Observation 2: Package is in transit, expected delivery May 18, currently in 
Denver distribution center

Thought 3: I should also check if there are any known issues with this shipment.
Action 3: check_shipping_alerts("98234")
Observation 3: No alerts or delays reported

Thought 4: Let me get details about what's in the order.
Action 4: query_crm("SELECT * FROM order_items WHERE order_id = 98234")
Observation 4: 
- Blue Wireless Headphones (Qty: 1)
- Phone Case - Black (Qty: 2)

Thought 5: I have complete information to provide a helpful response.
Answer: Order #98234 (Blue Wireless Headphones and 2 Phone Cases) was shipped 
on May 10 and is currently in transit. It's expected to arrive by May 18. 
The package is currently at the Denver distribution center with no delays 
reported. Your previous order #97556 was successfully delivered on May 2.
```

### Example 4: Debugging Code

```
Question: Why is my Python function returning None instead of the expected sum?

[Code provided]
def calculate_total(items):
    total = 0
    for item in items:
        total += item['price']

Thought 1: I should first run this code with test data to see the behavior.
Action 1: run_code("""
items = [{'price': 10}, {'price': 20}, {'price': 30}]
result = calculate_total(items)
print(f"Result: {result}")
""")
Observation 1: Result: None

Thought 2: The function is indeed returning None. Let me analyze the code.
Action 2: analyze_code("""
def calculate_total(items):
    total = 0
    for item in items:
        total += item['price']
""")
Observation 2: Function has no return statement. The 'total' variable is 
calculated but never returned.

Thought 3: That's the issue! Let me verify by checking if adding a return 
statement fixes it.
Action 3: run_code("""
def calculate_total(items):
    total = 0
    for item in items:
        total += item['price']
    return total

items = [{'price': 10}, {'price': 20}, {'price': 30}]
result = calculate_total(items)
print(f"Result: {result}")
""")
Observation 3: Result: 60

Thought 4: Confirmed! The fix works.
Answer: Your function is missing a return statement. Python functions return 
None by default if no return statement is specified. Add "return total" at 
the end of your function to return the calculated sum.

Fixed code:
def calculate_total(items):
    total = 0
    for item in items:
        total += item['price']
    return total  # ← Add this line
```

### Example 5: Planning a Trip

```
Question: Help me plan a 2-day trip to San Francisco focusing on tech and food.

Thought 1: I should get the current weather forecast for San Francisco.
Action 1: get_weather("San Francisco", "next 2 days")
Observation 1: Sunny, 68-72°F both days, no rain expected

Thought 2: Good weather! Let me find top tech attractions.
Action 2: search_places("San Francisco tech attractions")
Observation 2:
- Computer History Museum (Mountain View, 40 min away)
- Exploratorium (hands-on science museum)
- Apple Park Visitor Center (Cupertino, 45 min)
- Twitter/X HQ area (Mid-Market)

Thought 3: Now let me find highly-rated restaurants.
Action 3: search_restaurants("San Francisco top rated restaurants")
Observation 3:
- Gary Danko (Fine dining, Michelin star)
- Tartine Bakery (French bakery, famous)
- Mission Chinese Food (Trendy Chinese)
- Swan Oyster Depot (Seafood, cash only)
- La Taqueria (Best burritos)

Thought 4: I should check distances between these places.
Action 4: calculate_route(["Exploratorium", "Swan Oyster Depot", 
"Mission District", "Tartine Bakery"])
Observation 4: These are all within 15-30 minutes of each other in the city

Thought 5: Let me check current hotel prices.
Action 5: search_hotels("San Francisco", "2 nights")
Observation 5: Average: $220/night, good options in Union Square area

Thought 6: I have enough information to create a solid itinerary.
Answer: Here's your 2-day SF tech & food trip:

Day 1:
- Morning: Exploratorium (science/tech museum) - 3 hours
- Lunch: Swan Oyster Depot (cash only!) - incredible fresh seafood
- Afternoon: Walk through Mid-Market tech district
- Dinner: Mission Chinese Food
- Dessert: Bi-Rite Creamery in Mission

Day 2:
- Breakfast: Tartine Bakery (arrive early, lines form!)
- Morning: Computer History Museum in Mountain View (Uber/drive 40 min)
- Lunch: La Taqueria (best burrito in SF)
- Afternoon: Apple Park Visitor Center
- Dinner: Gary Danko (make reservation NOW - books up fast)

Weather: Perfect! 68-72°F, sunny both days
Stay: Union Square area hotels (~$220/night) for central location
Pro tip: Get a Clipper card for public transit
```

## ReAct with Function Calling

### Python Implementation

```python
from openai import OpenAI
import json

client = OpenAI()

# Define available tools
tools = [
    {
        "type": "function",
        "function": {
            "name": "search_web",
            "description": "Search the web for current information",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "The search query"
                    }
                },
                "required": ["query"]
            }
        }
    },
    {
        "type": "function",
        "function": {
            "name": "calculate",
            "description": "Perform mathematical calculations",
            "parameters": {
                "type": "object",
                "properties": {
                    "expression": {
                        "type": "string",
                        "description": "The math expression to evaluate"
                    }
                },
                "required": ["expression"]
            }
        }
    },
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get weather information for a location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "City name"
                    },
                    "days": {
                        "type": "integer",
                        "description": "Number of days forecast"
                    }
                },
                "required": ["location"]
            }
        }
    }
]

def react_agent(user_question, max_iterations=5):
    """
    Implement ReAct pattern with function calling
    """
    messages = [
        {
            "role": "system",
            "content": """You are a helpful assistant that uses tools to answer questions.
            
            Follow the ReAct pattern:
            1. Think about what information you need
            2. Use available tools to get that information
            3. Observe the results
            4. Repeat until you can answer the question
            
            Be explicit about your reasoning at each step."""
        },
        {
            "role": "user",
            "content": user_question
        }
    ]
    
    for iteration in range(max_iterations):
        print(f"\n--- Iteration {iteration + 1} ---")
        
        # Get model response
        response = client.chat.completions.create(
            model="gpt-4",
            messages=messages,
            tools=tools,
            tool_choice="auto"
        )
        
        assistant_message = response.choices[0].message
        messages.append(assistant_message)
        
        # Check if model wants to use a tool
        if assistant_message.tool_calls:
            for tool_call in assistant_message.tool_calls:
                function_name = tool_call.function.name
                function_args = json.loads(tool_call.function.arguments)
                
                print(f"Thought: Calling {function_name} with {function_args}")
                
                # Execute the function
                result = execute_function(function_name, function_args)
                print(f"Observation: {result}")
                
                # Add function result to messages
                messages.append({
                    "role": "tool",
                    "tool_call_id": tool_call.id,
                    "content": str(result)
                })
        else:
            # Model provided final answer
            print(f"\nFinal Answer: {assistant_message.content}")
            return assistant_message.content
    
    return "Max iterations reached without final answer"

def execute_function(name, args):
    """Execute the requested function"""
    if name == "search_web":
        # In production, call actual search API
        return f"Mock search results for: {args['query']}"
    
    elif name == "calculate":
        try:
            result = eval(args['expression'])  # Use safe eval in production!
            return result
        except Exception as e:
            return f"Error: {str(e)}"
    
    elif name == "get_weather":
        # In production, call actual weather API
        return f"Mock weather for {args['location']}: 72°F, Sunny"
    
    return "Function not found"

# Usage
question = "What's 15% of 240, and is that enough to cover a $30 expense?"
answer = react_agent(question)
```

### Example Output

```
--- Iteration 1 ---
Thought: Calling calculate with {'expression': '240 * 0.15'}
Observation: 36.0

--- Iteration 2 ---
Thought: Calling calculate with {'expression': '36 - 30'}
Observation: 6.0

Final Answer: 15% of 240 is $36. Yes, that is enough to cover a $30 
expense, with $6 remaining.
```

## ReAct Patterns

### Pattern 1: Sequential Research

```
Question: [Requires multiple information lookups]

Thought: Start with the most fundamental information
Action: [Tool 1]
Observation: [Result 1]

Thought: Build on what I learned
Action: [Tool 2]
Observation: [Result 2]

Thought: Synthesize findings
Answer: [Final answer]
```

### Pattern 2: Conditional Branching

```
Question: [Depends on intermediate results]

Thought: Check condition first
Action: [Check tool]
Observation: [Result]

Thought: Based on result, decide next step
If [condition]:
    Action: [Tool A]
Else:
    Action: [Tool B]
Observation: [Result]

Answer: [Final answer]
```

### Pattern 3: Verification Loop

```
Question: [Needs verification]

Thought: Get initial information
Action: [Tool 1]
Observation: [Result 1]

Thought: Verify this information from another source
Action: [Tool 2]
Observation: [Result 2]

Thought: Compare results
If consistent:
    Answer: [High confidence answer]
Else:
    Action: [Tool 3 for tiebreaker]
    Answer: [Clarified answer]
```

### Pattern 4: Iterative Refinement

```
Question: [Requires optimization]

Thought: Try first approach
Action: [Tool with parameters v1]
Observation: [Result not optimal]

Thought: Adjust strategy
Action: [Tool with parameters v2]
Observation: [Better result]

Thought: Fine-tune further
Action: [Tool with parameters v3]
Observation: [Optimal result]

Answer: [Final answer with best approach]
```

## Best Practices

### 1. **Explicit Thoughts**
```
❌ Bad: 
Action: search("weather")

✅ Good:
Thought: I need to check the weather before recommending outdoor activities
Action: search("San Francisco weather this weekend")
```

### 2. **One Action at a Time**
```
❌ Bad:
Thought: I need weather and restaurant info
Action: search("weather") AND search("restaurants")

✅ Good:
Thought: First, let me check the weather
Action: search("weather")
Observation: [result]

Thought: Now I'll search for restaurants
Action: search("restaurants")
```

### 3. **Use Observations in Reasoning**
```
❌ Bad:
Observation: Temperature is 45°F
Thought: Let's recommend beach activities

✅ Good:
Observation: Temperature is 45°F
Thought: That's quite cold. Beach activities wouldn't be comfortable. 
Let me find indoor alternatives.
Action: search("indoor activities")
```

### 4. **Know When to Stop**
```
✅ Set clear stopping conditions:
- Question is fully answered
- Sufficient information gathered
- Max iterations reached
- Dead end identified

Thought: I now have all information needed to answer the question
Answer: [final answer]
```

### 5. **Handle Errors Gracefully**
```
Action: query_database("SELECT * FROM users")
Observation: Error: Database connection timeout

Thought: The database is unavailable. Let me try an alternative approach.
Action: read_cache("users")
Observation: [Cached data from 1 hour ago]

Thought: Using cached data with timestamp disclaimer
Answer: Based on data from 1 hour ago... [note the limitation]
```

## ReAct vs Other Patterns

| Feature | CoT | ToT | ReAct |
|---------|-----|-----|-------|
| External tools | No | No | Yes |
| Real-time data | No | No | Yes |
| Multi-step reasoning | Yes | Yes | Yes |
| Hallucination risk | High | Medium | Low |
| Transparency | Medium | High | High |
| Cost | $ | $$$ | $$ |
| Best for | Pure reasoning | Strategic planning | Research & actions |

## Common Use Cases

### ✅ Perfect for ReAct:
- **Web research** - Need current information
- **Data analysis** - Query databases
- **Multi-step tasks** - Booking, planning
- **Fact-checking** - Verify information
- **API integration** - Call external services
- **Customer support** - Query CRM systems
- **Code execution** - Run and test code

### ❌ Not ideal for ReAct:
- **Simple questions** - Overkill
- **Creative writing** - No tools needed
- **Opinion-based** - No facts to look up
- **Speed-critical** - Too many steps

## Advanced Techniques

### 1. **Self-Reflection**
```
Thought: My previous approach didn't work. Let me reconsider.
Action: analyze_previous_results()
Observation: I was searching too broadly

Thought: I need to be more specific
Action: search("specific query")
```

### 2. **Parallel Actions** (when supported)
```
Thought: I can gather multiple pieces of information simultaneously
Action: [search("A"), get_weather("B"), query_db("C")]
Observations: [Result A, Result B, Result C]

Thought: Now I can synthesize all three results
```

### 3. **Planning Ahead**
```
Thought: To answer this, I'll need: 1) Current data, 2) Historical data, 
3) Calculate the difference. Let me start.

Action: get_current_data()
...
```

### 4. **Confidence Scoring**
```
Observation: Found 3 sources confirming the same information

Thought: High confidence in this result (3/3 sources agree)
Answer: [Answer with confidence level noted]
```

## Common Pitfalls

### ❌ Pitfall 1: Ignoring Observations
```
❌ Bad:
Observation: The store is closed on Sundays
Thought: Let's recommend visiting on Sunday
```

### ❌ Pitfall 2: Too Many Actions
```
❌ Calling 20 different tools for a simple question
✅ 3-5 actions should answer most questions
```

### ❌ Pitfall 3: No Clear Reasoning
```
❌ Action: random_action()
✅ Thought: I need X because Y
    Action: specific_action()
```

### ❌ Pitfall 4: Infinite Loops
```
❌ Thought: Still not sure, let me search again
    Action: search(same query)
    [Repeats forever]

✅ Set max iterations and track what's been tried
```

## Key Takeaways

1. **ReAct = Think + Act** in alternating loops
2. **Grounds AI in reality** through tool use
3. **Reduces hallucinations** with real data
4. **Transparent reasoning** - see the thinking process
5. **Best for research** and multi-step tasks
6. **Requires tool integration** - not just prompting
7. **More expensive** than simple prompts (multiple API calls)
8. **Essential for agents** - foundation of AI agents

## Summary

ReAct prompting bridges the gap between reasoning and action, enabling LLMs to:
- **Think strategically** about problems
- **Take concrete actions** using tools
- **Learn from observations** to adjust approach
- **Provide grounded answers** based on real data

It's the foundation of modern AI agents and essential for building practical applications that need to interact with external systems and data sources.

---

**Next Steps:** Learn about implementing ReAct agents with LangChain, LangGraph, or AutoGen frameworks that provide built-in ReAct patterns and tool integration.
