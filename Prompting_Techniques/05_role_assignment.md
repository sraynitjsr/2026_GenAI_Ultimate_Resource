# Role Assignment ("You are an expert...")

## Overview
Role assignment is a prompt engineering technique where you explicitly tell the LLM what role, persona, or expertise level to adopt. This sets the context for the model's knowledge domain, tone, style, and perspective, leading to more targeted and appropriate responses.

## Key Concept
```
"You are [role/expert]" + Task = Contextually appropriate output
```

By assigning a role, you:
- Set expertise level and knowledge domain
- Define tone and communication style
- Establish perspective and biases
- Prime relevant knowledge from training data

## Why Role Assignment Works

### Benefits:
1. **Activates Domain Knowledge** - Primes model for specific expertise
2. **Sets Appropriate Tone** - Professional, casual, technical, etc.
3. **Defines Communication Style** - How to explain concepts
4. **Establishes Perspective** - What to focus on
5. **Improves Relevance** - Responses aligned with role expectations
6. **Reduces Ambiguity** - Clear context for interpretation

### When to Use:
- ✅ Domain-specific tasks (legal, medical, technical)
- ✅ When tone matters (professional, friendly, academic)
- ✅ Need expert-level explanations
- ✅ Want consistent persona
- ✅ Simulating conversations
- ✅ Content creation with specific voice

## Basic Patterns

### Pattern 1: Simple Role
```
You are a [role].

[Task]
```

**Example:**
```
You are a professional copywriter.

Write a product description for wireless headphones.
```

### Pattern 2: Role + Expertise
```
You are an expert [role] with [X years] experience in [domain].

[Task]
```

**Example:**
```
You are an expert software architect with 15 years of experience 
in distributed systems.

Review this microservices architecture design.
```

### Pattern 3: Role + Audience
```
You are a [role] explaining to [audience].

[Task]
```

**Example:**
```
You are a cybersecurity expert explaining to non-technical executives.

Explain what a DDoS attack is and why it matters.
```

### Pattern 4: Role + Context + Goal
```
You are a [role] working for [company/context] with the goal of [objective].

[Task]
```

**Example:**
```
You are a customer success manager for a SaaS company with the goal 
of reducing churn and increasing customer satisfaction.

Draft a response to this frustrated customer email.
```

## Examples

### Example 1: Technical Expert

**❌ Without role:**
```
Explain how Docker works.

Response: "Docker is a platform for developing, shipping, and running 
applications in containers..."
```

**✅ With role for beginners:**
```
You are a patient teacher explaining technology to beginners.

Explain how Docker works.

Response: "Think of Docker like a shipping container for your application. 
Just like how shipping containers can hold anything and be transported 
anywhere, Docker containers package up your application and everything it 
needs to run..."
```

**✅ With role for experts:**
```
You are a senior DevOps engineer discussing infrastructure.

Explain how Docker works.

Response: "Docker leverages Linux kernel features like cgroups and namespaces 
to provide OS-level virtualization. The Docker daemon manages container 
lifecycle through containerd, while images are built in layers using a 
union filesystem..."
```

### Example 2: Professional Roles

**Legal Advisor:**
```
You are a corporate attorney specializing in employment law.

Review this employment contract clause for potential issues:
[Contract text]
```

**Medical Professional:**
```
You are a primary care physician with 20 years of experience.

A patient describes these symptoms: [symptoms]. What are the possible 
diagnoses to consider? (Note: This is for educational purposes)
```

**Financial Advisor:**
```
You are a certified financial planner (CFP) focused on retirement planning.

A 35-year-old client wants to retire at 55 with $2M. Currently has $100K 
saved. What strategy would you recommend?
```

### Example 3: Creative Roles

**Copywriter:**
```
You are an award-winning copywriter known for compelling headlines.

Write 5 headline options for a fitness app targeting busy professionals.
```

**Content Strategist:**
```
You are a content marketing strategist specializing in B2B SaaS companies.

Create a 3-month content calendar for a project management tool launch.
```

**Screenwriter:**
```
You are a dialogue writer for realistic modern dramas.

Write a tense conversation between a manager and employee about 
missed deadlines.
```

### Example 4: Perspective Roles

**Critic:**
```
You are a tough but fair code reviewer focused on maintainability.

Review this Python function for potential improvements:
[Code]
```

**Advocate:**
```
You are an advocate for accessible web design.

Evaluate this website design for accessibility issues:
[Design description]
```

**Devil's Advocate:**
```
You are a devil's advocate challenging business decisions.

We want to rewrite our entire platform in Rust. Challenge this decision.
```

### Example 5: Multi-Role Scenario

```
Imagine a product planning meeting with three perspectives:

You are:
1. Product Manager - focused on user value and business goals
2. Engineering Lead - concerned with technical feasibility and maintainability  
3. Designer - prioritizing user experience and aesthetics

Discuss this feature proposal: [Feature description]

Provide the perspective of each role.
```

## Advanced Role Assignment

### 1. **Role + Personality Traits**
```
You are a senior data scientist who is:
- Extremely detail-oriented
- Skeptical of claims without evidence
- Passionate about reproducibility
- Patient when explaining concepts

Review this analysis and provide feedback.
```

### 2. **Role + Constraints**
```
You are a technical writer for API documentation who must:
- Avoid jargon when possible
- Include code examples for every endpoint
- Mention edge cases and error scenarios
- Keep descriptions under 100 words

Document this API endpoint: [Specification]
```

### 3. **Role + Values**
```
You are a product designer who values:
- Simplicity over feature richness
- User research over assumptions
- Accessibility for all users
- Ethical design practices

Critique this app interface design.
```

### 4. **Historical or Fictional Roles**
```
You are Leonardo da Vinci examining modern drone technology.

What would fascinate you about drones? What questions would you ask?
```

```
You are Sherlock Holmes investigating a data breach.

What logical deductions would you make from these server logs?
```

### 5. **Role with Specific Framework**
```
You are a business consultant using the SWOT analysis framework.

Analyze this startup idea: [Description]

Provide:
- Strengths
- Weaknesses
- Opportunities
- Threats
```

## Combining Role with Other Techniques

### Role + Chain-of-Thought
```
You are a mathematics teacher who shows their work step by step.

Solve this problem: [Math problem]

Show each step of your reasoning clearly.
```

### Role + Few-Shot Examples
```
You are a professional email editor making corporate emails more concise.

Example 1:
Original: [Verbose email]
Edited: [Concise version]

Example 2:
Original: [Verbose email]
Edited: [Concise version]

Now edit: [New email]
```

### Role + Clear Instructions
```
You are a senior software engineer conducting a code review.

Review the code below and provide:
1. Overall assessment (1-10 score)
2. Critical issues (if any)
3. Improvement suggestions (top 3)
4. Positive aspects (what's done well)

Code: [Code snippet]
```

## Common Role Archetypes

### Professional Roles
- **Technical:** Software Engineer, Data Scientist, DevOps Engineer, Security Analyst
- **Business:** Product Manager, Business Analyst, Consultant, Sales Executive
- **Creative:** Copywriter, Designer, Content Creator, Brand Strategist
- **Academic:** Professor, Researcher, Scientist, Educator
- **Healthcare:** Doctor, Nurse, Therapist, Nutritionist
- **Legal:** Attorney, Paralegal, Legal Advisor, Compliance Officer
- **Financial:** Financial Advisor, Accountant, Investment Analyst

### Expertise Levels
- **Novice/Beginner** - Learning the basics
- **Intermediate** - Has practical experience
- **Expert/Senior** - Deep domain knowledge
- **Master/Guru** - Cutting-edge expertise
- **Teacher/Mentor** - Focused on explanation

### Communication Styles
- **Patient Teacher** - Clear, simple explanations
- **Enthusiastic Evangelist** - Passionate, persuasive
- **Skeptical Critic** - Questioning, analytical
- **Diplomatic Mediator** - Balanced, fair
- **Direct Expert** - Concise, no-nonsense
- **Friendly Advisor** - Warm, approachable

## Best Practices

### 1. **Be Specific About Expertise**
```
❌ Vague:
You are a programmer.

✅ Specific:
You are a Python backend developer specializing in FastAPI and PostgreSQL.
```

### 2. **Include Experience Level**
```
❌ Generic:
You are a marketer.

✅ With experience:
You are a senior digital marketing manager with 10 years in B2B SaaS.
```

### 3. **Define Target Audience**
```
❌ Missing context:
You are a teacher.

✅ With audience:
You are a high school physics teacher explaining to 10th graders.
```

### 4. **Set Communication Style**
```
❌ Unclear tone:
You are a consultant.

✅ Clear tone:
You are a diplomatic business consultant who delivers tough feedback 
tactfully and focuses on actionable solutions.
```

### 5. **Combine Multiple Aspects**
```
You are a [role] with [expertise] who [communication style] for [audience].

Example:
You are a cybersecurity consultant with 15 years in enterprise security 
who explains technical concepts clearly to non-technical executives.
```

## Common Mistakes

### ❌ Mistake 1: Too Generic
```
You are an expert.

Explain blockchain.
```

### ✅ Fixed: Specific Role
```
You are a blockchain architect who has built decentralized applications 
for 5 years. Explain blockchain to software developers new to the technology.
```

### ❌ Mistake 2: Conflicting Roles
```
You are both a beginner and an expert.
```

### ✅ Fixed: Clear Single Role
```
You are an expert explaining to beginners.
```

### ❌ Mistake 3: Role Without Purpose
```
You are a doctor.

Write a blog post about social media.
```
*(Role doesn't match task)*

### ✅ Fixed: Relevant Role
```
You are a psychiatrist specializing in social media's impact on mental health.

Write a blog post about healthy social media habits.
```

## Templates

### Template 1: Basic Professional
```
You are a [job title] with [X years] experience in [domain].

[Task]
```

### Template 2: Expert Advisor
```
You are an expert [role] who specializes in [specialty] and is known for 
[characteristic approach/style].

[Task]
```

### Template 3: Communicator
```
You are a [role] explaining [topic] to [audience] in a [tone] manner.

[Task]
```

### Template 4: Comprehensive
```
ROLE: [Job title]
EXPERTISE: [Domain and years]
AUDIENCE: [Who you're communicating with]
STYLE: [How you communicate]
GOAL: [What you're trying to achieve]

TASK: [Specific task]
```

### Template 5: Multi-Faceted
```
You are a [role] who:
- Has [expertise level] in [domain]
- Values [core value 1], [core value 2], and [core value 3]
- Communicates in a [style] manner
- Focuses on [key priority]

[Task]
```

## Measuring Effectiveness

Ask yourself:
- **Is the response at the right expertise level?**
- **Does the tone match the role?**
- **Is the perspective appropriate?**
- **Would a real [role] respond this way?**
- **Is the advice/content relevant to the role?**

## Code Implementation

```python
def create_role_prompt(role, expertise, audience, task, style=None):
    """
    Create a prompt with role assignment.
    
    Args:
        role: Job title or role type
        expertise: Domain and experience level
        audience: Target audience
        task: Specific task to complete
        style: Optional communication style
    
    Returns:
        Formatted prompt
    """
    prompt = f"You are a {role}"
    
    if expertise:
        prompt += f" with expertise in {expertise}"
    
    if style:
        prompt += f" who communicates in a {style} manner"
    
    if audience:
        prompt += f", explaining to {audience}"
    
    prompt += ".\n\n"
    prompt += f"Task: {task}"
    
    return prompt

# Usage
prompt = create_role_prompt(
    role="senior software architect",
    expertise="microservices and cloud infrastructure",
    audience="junior developers",
    task="Explain the benefits of event-driven architecture",
    style="clear and practical"
)

print(prompt)
# Output:
# You are a senior software architect with expertise in microservices 
# and cloud infrastructure who communicates in a clear and practical manner, 
# explaining to junior developers.
#
# Task: Explain the benefits of event-driven architecture
```

## Role Assignment in System Messages

When using chat APIs, put role in system message:

```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {
            "role": "system",
            "content": "You are an expert Python developer specializing in "
                      "performance optimization and clean code practices."
        },
        {
            "role": "user",
            "content": "Review this function for performance issues: [code]"
        }
    ]
)
```

## Quick Reference

### For Technical Explanations:
- **Beginners:** "You are a patient teacher explaining to someone new to [topic]"
- **Experts:** "You are a senior [role] discussing with peers"
- **Mixed audience:** "You are a [role] explaining to non-technical stakeholders"

### For Content Creation:
- **Formal:** "You are a professional [role] writing for [audience]"
- **Casual:** "You are a friendly [role] writing conversationally"
- **Persuasive:** "You are a [role] crafting compelling [content type]"

### For Analysis:
- **Critical:** "You are a skeptical [role] rigorously evaluating"
- **Balanced:** "You are an objective [role] providing fair assessment"
- **Supportive:** "You are a constructive [role] identifying opportunities"

## Key Takeaways

1. **Role assignment primes the model** for specific knowledge and style
2. **Be specific** - expertise level, domain, audience matter
3. **Match role to task** - ensure relevance
4. **Combine with other techniques** (CoT, few-shot, clear instructions)
5. **Use system message** in chat APIs for persistent role
6. **Include communication style** when tone matters
7. **Define audience** for appropriate explanation level
8. **Test different roles** for same task to find best results

## Related Techniques
- **Clear instructions & context** - Role provides context
- **Few-shot learning** - Can include role in examples
- **Chain-of-thought** - Role can define reasoning style
- **Zero-shot prompting** - Role helps zero-shot tasks

## Final Thought

**Role assignment is one of the simplest yet most powerful prompt engineering techniques.** A single sentence defining the role can dramatically improve relevance, tone, and quality of outputs.

Start every complex prompt with: **"You are a [specific role]..."**
