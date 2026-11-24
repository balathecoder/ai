sample Agent code,
* using openai library, first create client,
* choose model along with that mention role and prompt for a conversation,
* retrieve the response and print it with input_tokens, output_tokens and total_tokens.
* 
  
```
#!/usr/bin/env python3
"""
Task 5: Understanding Tokens and Business Costs
Learn how tokens work and calculate real business costs for AI usage.
"""

import openai
import os

client = openai.OpenAI(
    api_key=os.getenv("OPENAI_API_KEY"),
    base_url=os.getenv("OPENAI_API_BASE")
)

# Make an API call with a business-relevant prompt
prompt = "Explain the benefits of using AI for customer support in a business"
response = client.chat.completions.create(
    model="gpt-5.6-luna",
    messages=[{"role": "user", "content": prompt}]
)

# ==========================================
# WHAT ARE TOKENS?
# ==========================================
#
# Think of tokens as "pieces of words" that AI uses:
# - Simple words = 1 token (e.g., "cat", "run")
# - Complex words = multiple tokens (e.g., "unbelievable" = 3 tokens)
# - Rough estimate: 1 token ≈ 4 characters or 0.75 words
#
# The response.usage object tells you EXACTLY how many tokens you used:
# ┌────────────────────────────────────┐
# │ response.usage                      │
# │  ├── prompt_tokens      (input)    │ ← What you asked
# │  ├── completion_tokens  (output)   │ ← What AI answered
# │  └── total_tokens       (sum)      │ ← What you pay for
# └────────────────────────────────────┘
# ==========================================

# TODO: Extract the token counts from response.usage
input_tokens = response.usage.prompt_tokens    # TODO: prompt_tokens
output_tokens = response.usage.completion_tokens   # TODO: completion_tokens
total_tokens = response.usage.total_tokens    # TODO: total_tokens

print("📊 Token Usage Report:")
print("="*50)
print(f"  Your question used: {input_tokens} tokens")
print(f"  AI's response used: {output_tokens} tokens")
print(f"  Total tokens billed: {total_tokens} tokens")
print("="*50)

# ==========================================
# CALCULATING REAL BUSINESS COSTS
# ==========================================
#
# GPT-4.1-mini Official Pricing:
# ┌─────────────────────────────────────┐
# │ Input:  $0.80 per 1M tokens         │
# │         = $0.0008 per 1K tokens     │
# │                                      │
# │ Output: $3.20 per 1M tokens         │
# │         = $0.0032 per 1K tokens     │
# └─────────────────────────────────────┘
#
# Notice: Output costs 4x more than input!
# This is why keeping AI responses concise matters for your budget.
# ==========================================

# GPT-4.1-mini pricing (per 1,000 tokens) - already set for you!
input_price_per_1k = 0.0008   # That's $0.80 per million tokens
output_price_per_1k = 0.0032  # That's $3.20 per million tokens

# Calculate actual costs for this API call
input_cost = (input_tokens / 1000) * input_price_per_1k
output_cost = (output_tokens / 1000) * output_price_per_1k
total_cost = input_cost + output_cost

print("\n💰 Cost Breakdown for This Call:")
print("-"*50)
print(f"  Input cost:  ${input_cost:.6f} ({input_tokens} tokens)")
print(f"  Output cost: ${output_cost:.6f} ({output_tokens} tokens)")
print(f"  TOTAL COST:  ${total_cost:.6f}")
print("-"*50)

# Create completion marker
os.makedirs("/root/markers", exist_ok=True)
with open("/root/markers/task5_costs_complete.txt", "w") as f:
    f.write("SUCCESS")

print("\n✅ Task 5 completed! You now understand tokens and costs!")
```

### LangChain

Custom chaos,
* Custom code for each LLM,
* No prompt management,
* Manual tool integration
* Zero conversation memory,
* No knowledge retrievel,
* Scattered AI deployments

With LangChain,
* Unified LangChain framework,
* Professional Prompt templates,
* Multi model orchestration
* Intelligent memory systems
* RAG knowledge integration
* Production ready deployment

Objective :
Transform from LangChain novice to expert practioner. Build a complete production ready AI chatbot, with advanced features including multi-model support, memory systems, and RAG capabilities.
+------------------------------------------------+
| Component              | Role                  |
+------------------------------------------------+
| LangChain              | Core Framework        |
+------------------------------------------------+
| LangChain Community    | Integrations          |
+------------------------------------------------+
| LCEL                   | Chain Language        |
+------------------------------------------------+
| FAISS                  | Vector store          |
+------------------------------------------------+

```
$ cd /root/

$ mkdir -p langchain-lab

$ cd langchain-lab/

$ ls


$ python3 -m venv venv && source venv/bin/activate

root@controlplane ~/langchain-lab via 🐍 v3.12.3 (venv) ➜  pip install --upgrade pip uv
Requirement already satisfied: pip in ./venv/lib/python3.12/site-packages (24.0)
Collecting pip
  Downloading pip-26.3-py3-none-any.whl.metadata (4.6 kB)
Collecting uv
  Downloading uv-0.13.9-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
Downloading pip-26.3-py3-none-any.whl (2.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.0/2.0 MB 48.9 MB/s eta 0:00:00
Downloading uv-0.13.9-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (17.6 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 17.6/17.6 MB 97.3 MB/s eta 0:00:00
Installing collected packages: uv, pip
  Attempting uninstall: pip
    Found existing installation: pip 24.0
    Uninstalling pip-24.0:
      Successfully uninstalled pip-24.0
Successfully installed pip-26.3 uv-0.13.9

# core langchain packages
$ uv pip install langchain langchain-community langchain-core langchain-text-splitters langchain-huggingface
Using Python 3.12.3 environment at: venv
Resolved 63 packages in 377ms
Prepared 30 packages in 478ms
Installed 30 packages in 103ms
 + aiohappyeyeballs==2.7.1
 + aiohttp==3.14.5
 + aiosignal==1.4.0
 + attrs==26.1.0
 + click==8.5.1
 + filelock==4.3.0
 + frozenlist==1.8.0
 + fsspec==2026.9.0
 + hf-xet==1.8.0
 + httpx-sse==0.4.3
 + huggingface-hub==1.33.0
 + langchain==1.5.2
 + langchain-classic==1.0.8
 + langchain-community==0.4.2
 + langchain-huggingface==1.2.3
 + langchain-text-splitters==1.1.3
 + langgraph==1.2.24
 + langgraph-checkpoint==4.3.2
 + langgraph-prebuilt==1.1.3
 + langgraph-sdk==0.4.14
 + multidict==7.2.1
 + numpy==2.5.5
 + ormsgpack==1.13.1
 + propcache==0.5.4
 + pydantic-settings==2.16.3
 + python-dotenv==1.2.4
 + sqlalchemy==2.1.9
 + tokenizers==0.23.3
 + tqdm==4.70.1
 + yarl==1.25.1

# LLM Providers
$ uv pip install langchain-openai langchain-anthropic langchain-google-genai
Using Python 3.12.3 environment at: venv
Resolved 51 packages in 433ms
Prepared 51 packages in 593ms
Installed 51 packages in 290ms
 + annotated-types==0.8.0
 + anthropic==2.12.0
 + anyio==4.16.0
 + certifi==2026.11.20
 + cffi==2.1.1
 + charset-normalizer==3.5.3
 + cryptography==50.0.2
 + distro==1.9.0
 + docstring-parser==0.18.0
 + filetype==1.2.0
 + google-auth==2.63.1
 + google-genai==3.6.1
 + h11==0.16.0
 + httpcore==1.0.9
 + httpcore2==2.16.2
 + httpx==0.28.1
 + httpx2==2.16.2
 + idna==3.20
 + jiter==0.19.0
 + jsonpatch==1.48
 + jsonpointer==3.12
 + langchain-anthropic==1.7.12
 + langchain-core==1.8.4
 + langchain-google-genai==4.5.2
 + langchain-openai==1.8.5
 + langchain-protocol==0.0.22
 + langsmith==0.16.0
 + openai==3.57.0
 + opentelemetry-api==1.45.2
 + orjson==3.13.0
 + packaging==26.3
 + pyasn1==0.6.5
 + pyasn1-modules==0.4.2
 + pycparser==3.12
 + pydantic==2.14.4
 + pydantic-core==2.50.5
 + pyyaml==6.0.4
 + regex==2026.11.19
 + requests==2.34.2
 + requests-toolbelt==1.0.0
 + sniffio==1.3.1
 + tenacity==9.1.4
 + tiktoken==0.14.0
 + truststore==0.10.5
 + typing-extensions==4.16.0
 + typing-inspection==0.4.5
 + urllib3==2.8.0
 + uuid-utils==0.19.0
 + websockets==17.4
 + xxhash==4.1.0
 + zstandard==0.25.0

# Vector store and embeddings
$ uv pip install faiss-cpu sentence-transformers

# UI and utilities
$ uv pip install python-dotenv gradio

```

### Basic Prompt Template
Basic templates are the simplest form - a string with placeholders like {variable} that get replaced with actual values.
```
from langchain_core.prompts import PromptTemplate

# 1. Create a basic template
template = PromptTemplate(
    input_variables=["product", "feature"],
    template="Generate a marketing slogan for {product} highlighting {feature}."
)

# 2. Use the template
prompt = template.format(product="LangChain", feature="AI orchestration")
print("Generated prompt:", prompt)

# 3. Try different variables
examples = [
    {"product": "Smartphone", "feature": "camera quality"},
    {"product": "Electric Car", "feature": "eco-friendly"},
    {"product": "AI Assistant", "feature": "natural conversation"}
]

for example in examples:
    result = template.format(**example)
    print(f"- {result}")

# Save progress
with open('/root/basic-templates.txt', 'w') as f:
    f.write("BASIC_TEMPLATES_COMPLETE")
```

```
$ python basic_template.py 
Generated prompt: Generate a marketing slogan for LangChain highlighting AI orchestration.
- Generate a marketing slogan for Smartphone highlighting camera quality.
- Generate a marketing slogan for Electric Car highlighting eco-friendly.
- Generate a marketing slogan for AI Assistant highlighting natural conversation.
```

### Chat Templates - Conversation flow
Chat templates structure conversation with different message types
* system(instructions)
* human(user)
* assistant(AI)

Message Flow :
* SYSTEM : You are a {role} expert.
* HUMAN : Explain {concept} to me.
* ASSISTANT : I'll explain {concept} as a {role} expert.

chat_template.py
```
from langchain_core.prompts import ChatPromptTemplate

# Create a chat template
chat_template = ChatPromptTemplate.from_messages([
    ("system", "You are a {role} expert with {years} years of experience."),
    ("human", "Explain {concept} to me in simple terms."),
    ("assistant", "I'll explain {concept} step by step.")
])

# Generate messages
messages = chat_template.format_messages(
    role="Python programming",
    years="10",
    concept="decorators"
)


print("Generated Chat Conversation:")
for msg in messages:
    print(f"{msg.type.upper()}: {msg.content}")

# Try different scenarios
scenarios = [
    {"role": "Data Science", "years": "5", "concept": "machine learning"},
    {"role": "Web Development", "years": "8", "concept": "REST APIs"},
]

for scenario in scenarios:
    print(f"\nScenario: {scenario['concept']}")
    messages = chat_template.format_messages(**scenario)
    print(f"   System: {messages[0].content}")
    for msg in messages:
        print(f"{msg.type.upper()}: {msg.content}")

with open('/root/chat-templates.txt', 'w') as f:
    f.write("CHAT_TEMPLATES_COMPLETE")```

```
$ python chat_template.py 
Generated Chat Conversation:
SYSTEM: You are a Python programming expert with 10 years of experience.
HUMAN: Explain decorators to me in simple terms.
AI: I'll explain decorators step by step.

Scenario: machine learning
   System: You are a Data Science expert with 5 years of experience.
SYSTEM: You are a Data Science expert with 5 years of experience.
HUMAN: Explain machine learning to me in simple terms.
AI: I'll explain machine learning step by step.

Scenario: REST APIs
   System: You are a Web Development expert with 8 years of experience.
SYSTEM: You are a Web Development expert with 8 years of experience.
HUMAN: Explain REST APIs to me in simple terms.
AI: I'll explain REST APIs step by step.
```

### Few-shot templates - Learn by Example
Concept : Few shot templates teach the AI by showing examples. The AI learns the pattern from examples and applies it to new inputs.

Pattern learning process:
* Example 1 : happy -> sad
* Example 2 : tall -> short
* New Input : hot -> ???
* AI Prection : hot -> cold

few_shot_template.py :
```
from langchain_core.prompts import PromptTemplate, FewShotPromptTemplate

# Define examples that teach the pattern
examples = [
    {"input": "happy", "output": "sad"},
    {"input": "tall", "output": "short"},
    {"input": "fast", "output": "slow"},
    {"input": "hot", "output": "cold"}
]

# Template for each example
example_template = PromptTemplate(
    input_variables=["input", "output"],
    template="Input: {input}\nOutput: {output}"
)

# Few-shot template
few_shot_template = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_template,
    prefix="Find the opposite of each word:",
    suffix="Input: {word}\nOutput:",
    input_variables=["word"]
)

# Test the pattern learning
test_words = ["big", "light", "expensive", "difficult"]

print("Few-Shot Learning in Action:")
print("=" * 40)

for word in test_words:
    prompt = few_shot_template.format(word=word)
    print(f"\nGenerated Prompt for '{word}':")
    print(prompt)
    print("-" * 30)

# Advanced: Dynamic example selection
print("\nAdvanced: Selective Examples")
selected_examples = examples[:2]  # Use only first 2 examples
dynamic_template = FewShotPromptTemplate(
    examples=selected_examples,
    example_prompt=example_template,
    prefix="Learn the pattern from these examples:",
    suffix="Input: {word}\nOutput:",
    input_variables=["word"]
)

prompt = dynamic_template.format(word="bright")
print(prompt)

with open('/root/few-shot-templates.txt', 'w') as f:
    f.write("FEW_SHOT_TEMPLATES_COMPLETE")
```

output :
```
python few_shot_template.py 
Few-Shot Learning in Action:
========================================

Generated Prompt for 'big':
Find the opposite of each word:

Input: happy
Output: sad

Input: tall
Output: short

Input: fast
Output: slow

Input: hot
Output: cold

Input: big
Output:
------------------------------

Generated Prompt for 'light':
Find the opposite of each word:

Input: happy
Output: sad

Input: tall
Output: short

Input: fast
Output: slow

Input: hot
Output: cold

Input: light
Output:
------------------------------

Generated Prompt for 'expensive':
Find the opposite of each word:

Input: happy
Output: sad

Input: tall
Output: short

Input: fast
Output: slow

Input: hot
Output: cold

Input: expensive
Output:
------------------------------

Generated Prompt for 'difficult':
Find the opposite of each word:

Input: happy
Output: sad

Input: tall
Output: short

Input: fast
Output: slow

Input: hot
Output: cold

Input: difficult
Output:
------------------------------

Advanced: Selective Examples
Learn the pattern from these examples:

Input: happy
Output: sad

Input: tall
Output: short

Input: bright
Output:

```

Advanced Templates - Production Ready :
Concept: Advanced features include validation, partial variables, output parsers, and conditional logic for production applications.
Template Validate Process : INPUT ( Template + Variabls) -> Validate(Check Rules) -> Output ( Sage Prompt).

advanced_templates.py
```
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field

# 1. Template with Validation
def validate_topic_length(topic):
    if len(topic) > 50:
        raise ValueError("Topic too long!")
    return True

safe_template = PromptTemplate(
    input_variables=["topic"],
    template="Write a summary about {topic}",
    validate_template=True
)

# 2. Partial Templates (Pre-filled variables)
partial_template = PromptTemplate(
    input_variables=["task"],
    template="As an expert {role}, please help with: {task}",
    partial_variables={"role": "AI consultant"}
)

# 3. Structured Output with Pydantic
class ProductReview(BaseModel):
    rating: int = Field(description="Rating from 1-5")
    pros: list[str] = Field(description="List of advantages")
    cons: list[str] = Field(description="List of disadvantages")
    recommendation: str = Field(description="Final recommendation")

parser = PydanticOutputParser(pydantic_object=ProductReview)

structured_template = PromptTemplate(
    template="Review this product: {product}\n{format_instructions}",
    input_variables=["product"],
    partial_variables={"format_instructions": parser.get_format_instructions()}
)

# 4. Conditional Templates
def create_conditional_template(user_level):
    if user_level == "beginner":
        template = "Explain {concept} in very simple terms with examples."
    elif user_level == "intermediate":
        template = "Explain {concept} with some technical details."
    else:  # advanced
        template = "Provide a comprehensive technical explanation of {concept}."

    return PromptTemplate(
        input_variables=["concept"],
        template=template
    )

# Test all features
print("Advanced Template Features Demo")
print("=" * 45)

# Test partial template
print("\n1. Partial Template:")
prompt1 = partial_template.format(task="optimize database queries")
print(f"   {prompt1}")

# Test structured output
print("\n2. Structured Output Template:")
prompt2 = structured_template.format(product="iPhone 15")
print(f"   {prompt2[:100]}...")

# Test conditional templates
print("\n3. Conditional Templates:")
for level in ["beginner", "intermediate", "advanced"]:
    template = create_conditional_template(level)
    prompt = template.format(concept="machine learning")
    print(f"   {level.upper()}: {prompt}")

# Best Practices Summary
print("\nTemplate Best Practices:")
best_practices = [
    "Always validate user inputs",
    "Use partial variables for constants",
    "Structure outputs with Pydantic",
    "Make templates conditional when needed",
    "Keep templates readable and maintainable"
]

for i, practice in enumerate(best_practices, 1):
    print(f"   {i}. {practice}")

with open('/root/advanced-templates.txt', 'w') as f:
    f.write("ADVANCED_TEMPLATES_COMPLETE")
```

```

```
