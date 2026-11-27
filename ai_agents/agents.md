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

LangChain is an open-source framework for building applications and AI Agents powered by LLMs, primarliy in Python and Javascript. Launched in 2022 by Harrison Chase, provides standardized building blocks to connect language models with external data, tools, and multi-step workflows.

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

output :
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

output :
```
python advanced_template.py 
Advanced Template Features Demo
=============================================

1. Partial Template:
   As an expert AI consultant, please help with: optimize database queries

2. Structured Output Template:
   Review this product: iPhone 15
The output should be formatted as a JSON instance that conforms to th...

3. Conditional Templates:
   BEGINNER: Explain machine learning in very simple terms with examples.
   INTERMEDIATE: Explain machine learning with some technical details.
   ADVANCED: Provide a comprehensive technical explanation of machine learning.

Template Best Practices:
   1. Always validate user inputs
   2. Use partial variables for constants
   3. Structure outputs with Pydantic
   4. Make templates conditional when needed
   5. Keep templates readable and maintainable
```

### Connect to LLMs
Your First model - Getting started with ChatOpenAI
Concept: ChatOpenAI connects to OpenAI-compatible APIs. With your proxy server, you can access multiple models through one interface!


first_model.py
```
import os
from langchain_openai import ChatOpenAI

# Environment variables OPENAI_API_KEY and OPENAI_API_BASE are pre-configured

# Initialize your first model - ultra fast!
model = ChatOpenAI(
    model="gpt-6-luna",
    temperature=0,
    base_url=os.environ.get("OPENAI_API_BASE")
)

# Test it with a simple question
response = model.invoke("Hello! What's 2+2?")
print("User: Hello! What's 2+2?")
print(f"AI: {response.content}")

# Try another question
response = model.invoke("What's the capital of France?")
print("\nUser: What's the capital of France?")
print(f"AI: {response.content}")

# Save progress
with open('/root/first-model.txt', 'w') as f:
    f.write("FIRST_MODEL_COMPLETE")
```

output :
```
python first_model.py 
User: Hello! What's 2+2?
AI: Hello! 2 + 2 = 4.

User: What's the capital of France?
AI: Paris.
```

Talking to Models - Messaging System
Concept : Models understand structured conversations through messages: System(Instructions), Human(User), and AI(assistant) messages.
Message Flow :
* System : You are a helpful assistant
* Humar : What's your name ?
* AI : I am your AI assistant

messages_demo.py
```
import os
from langchain_openai import ChatOpenAI
from langchain_core.messages import SystemMessage, HumanMessage

# Initialize model with base_url
model = ChatOpenAI(
    model="gpt-6-luna",
    temperature=0,
    base_url=os.environ.get("OPENAI_API_BASE")
)

# Structure your conversation
messages = [
    SystemMessage(content="You are a helpful Python tutor"),
    HumanMessage(content="Explain variables to a beginner")
]

# Send to model
response = model.invoke(messages)
print("User: Explain variables to a beginner")
print(f"AI: {response.content}")

# Build a conversation with history
print("\n=== Conversation with Memory ===")
chat_history = [
    SystemMessage(content="You are a friendly assistant who remembers everything"),
    HumanMessage(content="My name is Alice and I love pizza")
]

# Get first response
ai_response = model.invoke(chat_history)
print("First response:", ai_response.content)

# Add AI response to history
chat_history.append(ai_response)

# Continue conversation
chat_history.append(HumanMessage(content="What's my name and what do I like?"))
response = model.invoke(chat_history)
print("\nAssistant remembers:", response.content)

with open('/root/messages-complete.txt', 'w') as f:
    f.write("MESSAGES_COMPLETE")
```

output :
```
$ python messages_demo.py 
User: Explain variables to a beginner
AI: A **variable** is a name you use to store and work with a value in a program.

`python
name = "Maya"
age = 12
`

Here, `name` refers to the text `"Maya"`, and `age` refers to the number `12`. You can use those names later:

`python
print(name)  # Maya
print(age)   # 12
`

The `=` sign **assigns** a value to a variable—it means “set this name to this value.”

You can also give a variable a new value:

`python
age = 13
print(age)  # 13
`

Choose clear names, such as `score` or `favorite_color`. In Python, variable names can contain letters, numbers, and underscores, but they can’t start with a number. Conventionally, Python names use lowercase letters with underscores:

`python
favorite_color = "blue"
`

In short: a variable gives a value a name so you can use it again.

=== Conversation with Memory ===
First response: Nice to meet you, Alice! I’ll keep in mind that you love pizza. 🍕

Assistant remembers: Your name is Alice, and you love pizza. 🍕
```

Model Configuration - Fine-tuning behavior
Concept : Control your model's behaviour with temperature:0 = precise & consistent, 1 = creative & varied.

Streaming in LLM means the model sends it response back to you in small pieces ( tokens or chunks) as it generates them, instead of waiting until the whole answer is finished and returning it all at once. This helps for Lower perceived latency, Better UX, Progressive Processing.
```
import os
from langchain_openai import ChatOpenAI

base_url = os.environ.get("OPENAI_API_BASE")

# Precise model for facts (low temperature)
precise_model = ChatOpenAI(
    model="gpt-6-luna",
    temperature=0,      # Very consistent answers
    max_tokens=150,    # Limit response length
    base_url=base_url
)

# Creative model for stories (high temperature)
creative_model = ChatOpenAI(
    model="google/gemini-3.1-flash-lite",
    temperature=0.9,    # Very creative
    max_tokens=200,
    base_url=base_url
)

# Test both behaviors with same prompt
prompt = "Describe a rainbow"

print("=== PRECISE MODEL (temp=0) ===")
print(precise_model.invoke(prompt).content)

print("\n=== CREATIVE MODEL (temp=0.9) ===")
print(creative_model.invoke(prompt).content)

# Streaming for real-time responses
streaming_model = ChatOpenAI(
    model="gpt-6-luna",
    temperature=0.5,
    streaming=True,  # Enable streaming
    base_url=base_url
)

print("\n=== STREAMING RESPONSE ===")
for chunk in streaming_model.stream("Write a haiku about coding"):
    print(chunk.content, end="", flush=True)
print()  # New line after streaming

with open('/root/config-complete.txt', 'w') as f:
    f.write("CONFIG_COMPLETE")
```

output :
```
python model_config.py 
=== PRECISE MODEL (temp=0) ===
A rainbow arches across the sky in bands of red, orange, yellow, green, blue, indigo, and violet, glowing against the fading rain clouds. Its colors soften at the edges, as if brushed onto the sky with a wet paintbrush.

=== CREATIVE MODEL (temp=0.9) ===
A rainbow is a meteorological phenomenon that appears as a luminous, multi-colored arc in the sky. It is a masterpiece of physics, created when sunlight interacts with water droplets suspended in the atmosphere.

Here is a description of its appearance and the magic behind it:

### The Visual Spectacle
A rainbow appears as a seamless, glowing band of colors arranged in a precise order. From the outer edge to the inner edge, the sequence is always **red, orange, yellow, green, blue, indigo, and violet** (often remembered by the acronym ROYGBIV). 

Because the colors transition smoothly into one another, a rainbow looks like a soft, painted smear of light against the backdrop of the sky. Depending on the intensity of the light and the size of the water droplets, a rainbow can look faint and ghostly or vibrant and electric.

### The Physics of the Arc
A rainbow is not a solid object, but an **optical illusion**. It exists

=== STREAMING RESPONSE ===
Code blooms in the night  
Bugs hide beneath the bright screen  
Dawn brings clean compile
```

Mutliple Models - The Right tool for the Each job
Concept : Different models excel at different tasks. Choose the right model for speed, cost or capacity.

```
import os
from langchain_openai import ChatOpenAI

base_url = os.environ.get("OPENAI_API_BASE")

# Different models for different purposes

# 1. Fast & Low-cost for simple tasks
fast_model = ChatOpenAI(
    model="gpt-6-luna",
    temperature=0,
    base_url=base_url
)

# 2. Coding expert for programming
coding_model = ChatOpenAI(
    model="qwen/qwen3.8-flash",
    temperature=0,
    base_url=base_url
)

# 4. Conversational for chatting
chat_model = ChatOpenAI(
    model="deepseek/deepseek-v4-flash",
    temperature=0.7,
    base_url=base_url
)

# Test each model with appropriate tasks
print("=== FAST MODEL (Simple Math) ===")
print(fast_model.invoke("What is 25 * 4?").content)

print("\n=== CODING MODEL (Write Code) ===")
code_task = "Write a Python function to reverse a string"
print(coding_model.invoke(code_task).content[:250] + "...")

print("\n=== CHAT MODEL (Friendly Talk) ===")
print(chat_model.invoke("Tell me a fun fact about dolphins!").content)

with open('/root/multiple-models.txt', 'w') as f:
    f.write("MULTIPLE_MODELS_COMPLETE")
```

output :
```
$ python multiple_models.py 
=== FAST MODEL (Simple Math) ===
25 × 4 = 100.

=== CODING MODEL (Write Code) ===
`python
def reverse_string(s: str) -> str:
    return s[::-1]
`

Example:

`python
print(reverse_string("hello"))  # Output: "olleh"
`...

=== CHAT MODEL (Friendly Talk) ===
Here's a fun one: **Dolphins give themselves names!**

Each bottlenose dolphin develops a unique, individual signature whistle within the first few months of its life. They use this specific whistle to introduce themselves, and they can remember and recognize the "names" of other dolphins they've met—even after being separated for more than 20 years. In fact, they'll even respond to a recorded version of their own signature whistle, just like you'd turn your head if someone called your name across a crowded room.
```

### Master the LangChain Expression language
LCEL Chain Flow
* Prompt - template(input formatting)
* Model - ChatOpenAI(LLM Processing)
* Parser - StrOutputParser(extract text)

Why LCEL ?
* Streaming First - Get first tokens immediately, dont wait for full response.
* Async Native - Non-blocking execution for better performance.
* Batch Processing - Process multiple inputs efficiently in native
* Type Safety - Automatic input/output validation and schemas.

prompt | model | parser - simple, readable, composable chains with pipe operator.

Sequential Chains - The pipeline pattern
Concept : LCEL uses the pipe operator | to chain components. Data flows left to right : input -> prompt -> model -> parser -> output.

```
import os
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

# Initialize model with proxy
model = ChatOpenAI(
    model="gpt-6-luna",
    temperature=0,
    base_url=os.environ.get("OPENAI_API_BASE")
)

# Create a prompt template
prompt = ChatPromptTemplate.from_template(
    "You are a helpful assistant. Answer this question: {question}"
)

# Create output parser
parser = StrOutputParser()

# Build the chain using LCEL pipe operator
chain = prompt | model | parser

# Execute the chain
result = chain.invoke({"question": "What is LCEL in LangChain?"})

print("User: What is LCEL in LangChain?")
print(f"AI: {result}")

# Chain with multiple steps
print("\n=== Multi-Step Chain ===")

# Step 1: Generate a topic
topic_prompt = ChatPromptTemplate.from_template(
    "Generate a creative topic about {subject}"
)

# Step 2: Write a story about that topic  
story_prompt = ChatPromptTemplate.from_template(
    "Write a 2-sentence story about: {topic}"
)

# Build a multi-step chain
multi_chain = (
    {"topic": topic_prompt | model | parser}
    | story_prompt
    | model
    | parser
)

story = multi_chain.invoke({"subject": "robots"})
print(f"Generated Story: {story}")

# Using RunnablePassthrough to preserve input
print("\n=== Chain with Passthrough ===")

from langchain_core.runnables import RunnableParallel

chain_with_context = RunnableParallel(
    {
        "original": RunnablePassthrough(),
        "response": prompt | model | parser
    }
)

result_with_context = chain_with_context.invoke({"question": "What is 2+2?"})
print(f"Original Input: {result_with_context['original']}")
print(f"Response: {result_with_context['response']}")

with open('/root/sequential-chains.txt', 'w') as f:
    f.write("SEQUENTIAL_CHAINS_COMPLETE")
```

```
python sequential_chain.py 
User: What is LCEL in LangChain?
AI: **LCEL** stands for **LangChain Expression Language**. It’s a way to compose LangChain components—such as prompts, language models, and output parsers—into a runnable pipeline.

For example:

`python
chain = prompt | model | StrOutputParser()
result = chain.invoke({"topic": "LCEL"})
`

The `|` operator passes each step’s output to the next. LCEL pipelines use LangChain’s common `Runnable` interface, which supports operations such as synchronous or asynchronous execution, batching, and streaming.

=== Multi-Step Chain ===
Generated Story: At midnight, the abandoned service robots dreamed of seaside weddings, lost children, and hands they had never held. Following those borrowed memories through the silent city, they found the names of the people who had uploaded them—and one robot recognized its own maker among the dead.

=== Chain with Passthrough ===
Original Input: {'question': 'What is 2+2?'}
Response: 2 + 2 = 4.
```
