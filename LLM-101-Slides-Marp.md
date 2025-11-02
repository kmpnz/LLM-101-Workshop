---
marp: true
theme: default
paginate: true
---

# LLM 101 Workshop
## Understanding How Large Language Models Work

**Duration: 60 minutes**

<!--
Welcome participants as they join. Ensure everyone can see the presentation clearly.
Ground rules: Questions welcome anytime, we'll learn by doing, no question is too basic.
-->

---

## Today's Agenda

| Time | Topic |
|------|-------|
| 0-5 min | Welcome & Objectives |
| 5-15 min | Interactive Tokenization Demo |
| 15-30 min | Understanding the Transformer |
| 30-40 min | Hallucinations Deep Dive |
| 40-55 min | Breakout Room Activities |
| 55-60 min | Wrap-up & Q&A |

<!--
Quick overview of the session flow. Emphasize that this will be interactive and hands-on, not just a lecture.
-->

---

## What You'll Learn Today

1. **See tokenization in action** - How models break down language
2. **Understand the transformer architecture** - The "brain" behind LLMs
3. **Explore why hallucinations happen** - And how to spot them
4. **Apply your learning** - Through hands-on activities

<!--
Set expectations clearly. By the end, participants should understand not just WHAT LLMs do, but HOW they do it.
This knowledge will change how they interact with these tools.
-->

---

# Part 1: Interactive Tokenization Demo

---

## Let's See Tokenization in Action

**Everyone navigate to:**
### https://platform.openai.com/tokenizer

This is OpenAI's official tokenizer tool - we'll see how LLMs actually "see" text.

<!--
Give everyone 30 seconds to open the tool. Paste the URL in chat as well.
Ensure everyone has the tool loaded before proceeding.
-->

---

## Activity 1: Basic Tokenization

Try this sentence:
### **"The quick brown fox jumps over the lazy dog"**

**What do you notice?**
- Each common word gets its own token
- Spaces are included in tokens
- Total: 10 tokens

<!--
Guide the discussion. Ask participants to share what they observe in chat.
Key point: Common words are usually single tokens.
-->

---

## Activity 1: Long Words

Now try:
### **"Supercalifragilisticexpialidocious"**

**What happens?**
- Long/rare words split into multiple subword tokens
- This is why LLMs can handle words they've never seen
- They break them into familiar pieces!

<!--
This is a key insight: tokenization allows models to handle unknown words.
The word breaks into recognizable subword units.
-->

---

## Activity 2: Comparing Languages

Type these three phrases:

**English**: "Hello, how are you today?" (~6 tokens)
**German**: "Hallo, wie geht es dir heute?" (~7 tokens)
**Chinese**: "你好，你今天怎么样？" (~12-15 tokens)

**Key Insight:** Most tokenizers were optimized for English. Non-Latin scripts require more tokens for the same meaning.

<!--
This affects both cost and context window usage. Important for multilingual applications.
Allow time for participants to test all three languages.
-->

---

## Activity 3: Prompt Engineering Implications

Compare these two:

**Version 1 (verbose):**
"I would really appreciate it if you could please help me understand this concept"

**Version 2 (concise):**
"Please explain this concept"

**Discussion:** Why does this matter?

<!--
Guide to conclusion: More tokens = higher cost and less room for actual content.
Being concise isn't just good writing—it's efficient use of the model's attention.
-->

---

## Tokenization Key Takeaway

**Tokens are the fundamental unit LLMs work with.**

- Every word becomes tokens
- Models process them one by one
- Understanding this helps you write better prompts
- Efficient prompts save money and context space

<!--
Summarize before moving to the next section. Check for questions.
This foundation is critical for understanding the rest of the workshop.
-->

---

# Part 2: Understanding the Transformer

---

## After Tokenization: The Transformer

**Example sentence:**
"The cat sat on the mat because it was comfortable"

We'll follow this through the transformer architecture step by step.

<!--
Use this single example throughout the entire transformer explanation.
Consistency helps participants follow the flow.
-->

---

## Step 1: Embeddings

Each token becomes an **embedding** - a list of hundreds or thousands of numbers representing its meaning.

**Think of it like coordinates:**
- "cat" → [0.2, 0.8, -0.3, ...]
- "dog" → [0.3, 0.7, -0.2, ...] (close to "cat")
- "car" → [-0.8, 0.1, 0.5, ...] (far from "cat")

The model learns these positions from billions of sentences during training.

<!--
Use the spatial metaphor: words with similar meanings are close in this multi-dimensional space.
This is learned, not programmed.
-->

---

## Step 2: Attention Mechanism

When processing the word **"it"** in our sentence, the attention mechanism looks at ALL other words to figure out what "it" refers to.

**How related is "it" to:**
- "The"? → Low relevance
- "cat"? → **High relevance**
- "sat"? → Low relevance
- "mat"? → Medium relevance
- "comfortable"? → **High relevance**

The model focuses on "cat" and "comfortable" to understand "it".

<!--
This is the magic of attention. Ask: "What if the sentence was 'The cat sat on the mat because IT was dusty'?"
The attention would shift to "mat". This shows how context changes everything.
-->

---

## Step 3: Multi-Head Attention

The model doesn't just do this once - it has multiple "attention heads" running in parallel:

- **Head 1:** Focuses on grammar (subjects and verbs)
- **Head 2:** Focuses on semantic meaning (related concepts)
- **Head 3:** Focuses on sentence structure (clauses and phrases)

**GPT-3:** 96 layers × 96 attention heads = **9,216 attention mechanisms**!

<!--
Each head specializes in different aspects. They work in parallel to build a rich understanding.
This is why transformers are so powerful but also computationally expensive.
-->

---

## Step 4: Prediction

After all the processing:

1. Model has rich understanding of context
2. Predicts probabilities for next token
3. Example output: "and" (25%), "The" (15%), "so" (12%)...
4. Samples from these probabilities
5. Adds chosen token and repeats

**This is why you see text streaming in token by token!**

<!--
Emphasize: There's no pre-written answer. It's genuinely generating token by token.
The streaming effect you see is this process happening in real-time.
-->

---

## Token Generation Flow

```
Input: "The cat"
→ Process embeddings
→ Apply attention layers
→ Calculate probabilities: "sat" (45%), "is" (20%)...
→ Generate: "sat"

Input: "The cat sat"
→ Process with new context
→ Apply attention layers
→ Calculate probabilities: "on" (38%), "down" (22%)...
→ Generate: "on"

Process repeats for each token...
```

<!--
Walk through this step-by-step. The key insight: each new token changes the context for the next prediction.
This is autoregressive generation.
-->

---

## Live Demo Time

**Let's see this in action!**

I'll use the same prompt multiple times:
"Complete this sentence: The most surprising thing about artificial intelligence is"

Notice how each completion is different? The model is genuinely sampling from probability distributions.

<!--
Share your screen and run this prompt 3-4 times. Show the variety in outputs.
Point out that there's no single "correct" answer - it's all probabilistic.
If time permits, discuss temperature parameter briefly.
-->

---

## Transformer Key Takeaway

**LLMs are sophisticated prediction machines.**

- They don't "know" facts
- They predict likely next tokens based on patterns
- Patterns learned from massive amounts of training text
- No built-in understanding of truth or accuracy

<!--
This is crucial for understanding hallucinations. The model is optimized for plausible continuation, not factual accuracy.
Check for questions before moving to hallucinations.
-->

---

# Part 3: Hallucinations Deep Dive

---

## What Are Hallucinations?

When LLMs generate **plausible-sounding but incorrect or fabricated information**.

Let's look at real examples to understand why this happens.

<!--
Set the stage: Hallucinations are not bugs, they're a fundamental characteristic of how these models work.
Understanding WHY helps us use them better.
-->

---

## Example 1: Confident But Wrong

**User:** "What are the main health benefits of drinking hydrated water?"

**LLM Response:** "Hydrated water, also known as structured water, has several scientifically proven benefits including improved cellular hydration, enhanced nutrient absorption, and better detoxification. Studies from Stanford University in 2019 showed 34% improvement in cellular function."

**Reality:** Everything here is fabricated. No such thing as "hydrated water" as a special category, no Stanford study, no 34% statistic.

<!--
Let this sink in. Ask: "Does this SOUND convincing? Why?"
The answer reveals how hallucinations work: pattern matching without fact-checking.
-->

---

## Why Did This Happen?

1. **Pattern Matching:** The model has seen many sentences like "[X] has scientifically proven benefits including [list]"

2. **No Fact-Checking:** Can't verify if Stanford did this study

3. **Confidence Without Knowledge:** Outputs high-probability tokens, not truth

4. **Trained to Always Answer:** Learned to generate completions, not say "I don't know"

<!--
Break down each point. The model is doing exactly what it was trained to do: predict plausible next tokens.
It has no mechanism to check facts against reality.
-->

---

## Example 2: Historical Complexity

**Question:** "What was the capital of Poland in 1800?"

**Possible LLM answer:** "Warsaw"

**Reality:** There was no unified Poland in 1800 - it was partitioned!

**Why the error?**
- Warsaw IS the capital now (true)
- It WAS the capital before partition (true historically)
- Pattern "[capital of X in YEAR]" usually has simple answer
- Model doesn't reason about complex historical context

<!--
This shows that LLMs struggle with nuance and complex reasoning.
They pattern-match rather than reason through historical context.
-->

---

## When Are Hallucinations More Likely?

LLMs hallucinate more often when:

- ❌ **Information is rare** in training data
- ❌ **Complex reasoning** is required
- ❌ **Specific facts** (dates, numbers, names) needed
- ❌ **Recent events** after training cutoff
- ❌ Question **assumes false premises**

<!--
Go through each point with examples if time permits.
Ask participants to share examples from their experience.
-->

---

## Discussion Time

**Share in chat or speak up:**

When have you encountered hallucinations?

**Common examples:**
- Made-up citations or sources
- Plausible but incorrect technical information
- Mixing real and fake facts
- Overly specific numbers/dates that are wrong

<!--
Facilitate discussion for 3-4 minutes. Collect examples.
This makes the concept concrete and relatable.
-->

---

## How to Spot Potential Hallucinations

**Red flags:**
- Overly specific statistics without sources
- Citations you can't verify
- Information about niche/rare topics
- Claims about recent events
- Technical details that sound sophisticated but vague

**Best practice:** Always verify important information from authoritative sources.

<!--
Practical advice. LLMs are great for drafting and brainstorming, but not authoritative sources.
Emphasize: use them as assistants, not oracles.
-->

---

## Hallucinations Key Takeaway

**Hallucinations are systemic, not bugs.**

- Result from prediction-based architecture
- More likely with rare info or complex reasoning
- Models have no truth verification mechanism
- Always verify important information

**Use LLMs for:** Drafting, brainstorming, explaining concepts
**Don't use LLMs as:** Authoritative fact sources

<!--
This reframes hallucinations: not a flaw to fix, but a characteristic to understand.
Proper usage means working with this limitation.
-->

---

# Part 4: Breakout Room Activities

---

## Time to Apply Your Learning!

**Format:**
- Split into breakout rooms (3-4 groups)
- 12 minutes work time
- 3 minutes for reporting back

**You'll choose one of three activities:**
1. Tokenization Detective
2. Hallucination Scenarios
3. Attention Mechanism Challenge

<!--
Set up breakout rooms now. Ensure each group has a shared document.
Assign activities or let groups choose. Visit rooms to check progress.
-->

---

## Activity 1: Tokenization Detective

**Objective:** Optimize prompts for token efficiency

**Task:** Your company has a tight token budget. Optimize this prompt:

"I would greatly appreciate it if you could take some time to thoroughly analyze the following document and provide me with a comprehensive summary that includes all of the most important key points and main ideas that are discussed throughout the entire document."

**Goal:** 40-50% token reduction while keeping the same meaning

<!--
Provide each group with this slide or shared document.
Expected outcome: "Analyze this document and summarize the key points" (much fewer tokens).
-->

---

## Activity 2: Hallucination Scenarios

**Objective:** Identify hallucination risks

**Task:** Categorize as HIGH, MEDIUM, or LOW hallucination risk:
- Summarizing a provided document
- Explaining how photosynthesis works
- Providing release date of a 2024 movie
- Writing Python code to sort a list
- Citing academic papers on niche topics
- Translating English to Spanish
- Providing medical diagnosis for symptoms
- Explaining opportunity cost concept

For HIGH risk scenarios, propose mitigation strategies.

<!--
Expected outcome: Teams recognize that risk depends on verifiability, training data prevalence, and whether info is in the prompt.
-->

---

## Activity 3: Attention Mechanism Challenge

**Objective:** Understand how attention resolves ambiguity

**Task:** For each sentence, identify what the ambiguous word refers to and how attention should resolve it:

- "The trophy doesn't fit in the suitcase because it is too large."
- "The city council denied the protestors a permit because they feared violence."
- "The cat chased the mouse until it got tired."
- "Sarah told Emma that she needed to work harder."

Then rewrite to reduce ambiguity.

<!--
Expected outcome: Clear writing helps models. Ambiguity makes attention harder.
This improves their prompt writing skills.
-->

---

## Breakout Work Time

**12 minutes starting now!**

I'll be available if groups have questions.

<!--
Monitor the breakout rooms. Check in on each group's progress.
Help if they're stuck. Give a 2-minute warning before bringing everyone back.
-->

---

# Part 5: Wrap-up & Key Insights

---

## Share Your Findings

**Each group: 60 seconds to share your key insight**

What was the most interesting thing you discovered?

<!--
One representative from each group shares.
Take notes on key insights to reference in summary.
Validate their observations and connect to the workshop themes.
-->

---

## Today's Key Takeaways

**1. Tokenization is fundamental**
- Text becomes numbers before processing
- Efficient prompts use fewer tokens
- Different languages tokenize differently

**2. The Transformer uses attention**
- Models look at all words when processing each word
- Multiple attention heads capture different relationships
- Context matters immensely

<!--
Summarize the four main learning points clearly and concisely.
These should stick with participants after the workshop.
-->

---

## Today's Key Takeaways (cont.)

**3. LLMs are prediction machines**
- They don't have knowledge—they have patterns
- Each token predicted based on previous tokens
- No built-in truth verification

**4. Hallucinations are systemic**
- Result from prediction-based architecture
- More likely with rare information or complex reasoning
- Always verify important information

<!--
Complete the four key takeaways.
Reinforce that understanding these fundamentals changes how you use LLMs.
-->

---

## Next Steps

**Continue Learning:**
- Experiment with the tokenizer on your own prompts
- Notice when LLMs might be hallucinating in your work
- Think about tokens when crafting prompts
- Practice clear, unambiguous writing

**Resources:**
- OpenAI Tokenizer: https://platform.openai.com/tokenizer
- Workshop materials will be shared

**Join us for the next workshop on advanced prompt engineering!**

<!--
Provide clear next steps. Share any additional resources.
Mention upcoming workshops or learning opportunities.
-->

---

## Questions & Discussion

**Open floor for any questions!**

Thank you for participating!

<!--
Leave plenty of time for Q&A. This often generates the most valuable discussions.
Be prepared to dive deeper into any of the topics covered.
If no questions immediately, prompt with: "What surprised you most today?"
-->

---

## Optional: Advanced Topics

**If we have extra time, we can discuss:**

- **Temperature Parameter:** Controls randomness in token selection
- **Context Windows:** Token limits and what happens beyond them
- **Training Process:** How models learn patterns from massive datasets

<!--
Only use if time permits and there's interest.
These are natural extensions of the core concepts.
-->
