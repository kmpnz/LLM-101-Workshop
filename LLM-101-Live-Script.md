# LLM 101 Workshop: Live Session Script
**Duration: 60 minutes | Interactive Workshop Format**

---

## Session Overview

**Objective**: Help participants understand how LLMs work through interactive demonstrations, hands-on activities, and group discussions.

**Assumed Knowledge**: Participants have read the pre-read material and know how to use LLMs like ChatGPT.

**Materials Needed**:
- Access to OpenAI Tokenizer (https://platform.openai.com/tokenizer)
- Breakout room capability
- Shared document for group notes
- Example prompts and responses prepared

---

## Session Agenda

| Time | Section | Format |
|------|---------|--------|
| 0-5 min | Welcome & Objectives | Presentation |
| 5-15 min | Interactive Tokenization Demo | Hands-on |
| 15-30 min | Understanding the Transformer | Presentation + Demo |
| 30-40 min | Hallucinations Deep Dive | Interactive Discussion |
| 40-55 min | Breakout Room Activities | Group Work |
| 55-60 min | Wrap-up & Q&A | Discussion |

---

## PART 1: Welcome & Objectives (5 minutes)

### Opening [Facilitator]

"Welcome to LLM 101! By the end of this hour, you'll understand not just *what* LLMs do, but *how* they do it. This knowledge will change how you interact with these tools and help you understand their capabilities and limitations.

Today we'll cover:
1. **See tokenization in action** - Watch how models break down language
2. **Understand the transformer architecture** - The "brain" behind LLMs
3. **Explore why hallucinations happen** - And how to spot them
4. **Apply your learning** - Through hands-on breakout activities

**Ground rules:**
- Questions are welcome anytime
- We'll learn by doing, not just listening
- No question is too basic
- Share your experiences and insights

Let's get started!"

---

## PART 2: Interactive Tokenization Demo (10 minutes)

### Setup [Facilitator]

"Everyone, please navigate to: **https://platform.openai.com/tokenizer**

This is OpenAI's official tokenizer tool. We're going to see how LLMs actually 'see' text."

### Activity 1: Basic Tokenization (3 minutes)

**Facilitator Instructions:**

"Let's start with a simple sentence. Everyone type this into the tokenizer:

**'The quick brown fox jumps over the lazy dog'**

Look at the tokens—what do you notice?"

**Expected Observations (guide discussion):**
- Each common word gets its own token
- Spaces are included in tokens
- Token count = 10 tokens

**Facilitator:** "Now try: **'Supercalifragilisticexpialidocious'**

What happens?"

**Expected Observation:**
- Long/rare words split into multiple subword tokens
- "This is why LLMs can handle words they've never seen—they break them into familiar pieces"

### Activity 2: Comparing Languages (3 minutes)

**Facilitator:** "Let's see something interesting. Type the same sentence in different languages:

**English**: 'Hello, how are you today?'  
**German**: 'Hallo, wie geht es dir heute?'  
**Chinese**: '你好，你今天怎么样？'

Compare the token counts. What do you notice?"

**Expected Observations:**
- English and European languages: ~6-8 tokens
- Chinese/Japanese: Often MORE tokens per character
- "This matters for cost and context windows—the same meaning costs different amounts in different languages"

### Activity 3: Prompt Engineering Implications (4 minutes)

**Facilitator:** "Now let's see why tokenization matters for your prompts. Try both versions:

**Version 1 (verbose)**: 'I would really appreciate it if you could please help me understand this concept'

**Version 2 (concise)**: 'Please explain this concept'

Compare token counts."

**Discussion Points:**
- Verbose version uses significantly more tokens
- More tokens = higher cost and less room for actual content
- "Being concise isn't just good writing—it's efficient use of the model's attention"

**Key Takeaway:** "Tokens are the fundamental unit LLMs work with. Every word you write becomes tokens, and the model processes them one by one. Understanding this helps you write better prompts."

---

## PART 3: Understanding the Transformer (15 minutes)

### Visual Explanation (8 minutes)

**Facilitator:** "Now let's look at what happens AFTER tokenization. Let me walk you through the transformer architecture using a concrete example."

**Use this example throughout:**  
*"The cat sat on the mat because it was comfortable"*

#### Step 1: Embeddings

"After tokenization, each token becomes an **embedding**—a list of hundreds or thousands of numbers that represent its meaning.

Think of it like this: If 'cat' is at coordinates [0.2, 0.8, -0.3, ...], then 'dog' might be at [0.3, 0.7, -0.2, ...]—close by because they're similar concepts. 'Car' would be far away at [-0.8, 0.1, 0.5, ...].

The model learns these positions by processing billions of sentences during training."

#### Step 2: Attention Mechanism

"Here's the magic: When processing the word 'it' in our sentence, the **attention mechanism** looks at ALL other words to figure out what 'it' refers to.

It asks: How related is 'it' to:
- 'The'? → Low relevance
- 'cat'? → High relevance  
- 'sat'? → Low relevance
- 'mat'? → Medium relevance
- 'comfortable'? → High relevance

The model calculates attention scores and focuses on 'cat' and 'comfortable' to understand 'it'."

**Interactive Question:** "What would happen if this was 'The cat sat on the mat because **it was dusty**'? What should 'it' refer to now?"

*[Take responses—the point is that attention would shift to 'mat']*

#### Step 3: Multi-Head Attention

"The model doesn't just do this once—it has multiple 'attention heads' running in parallel:
- **Head 1**: Might focus on grammar (subjects and verbs)
- **Head 2**: Might focus on semantic meaning (related concepts)
- **Head 3**: Might focus on sentence structure (clauses and phrases)

GPT-3 has 96 layers × 96 attention heads per layer = 9,216 different attention mechanisms!"

#### Step 4: Prediction

"After all the processing:
1. The model has a rich understanding of context
2. It predicts probabilities for the next token
3. It might output: 'and' (25%), 'The' (15%), 'so' (12%), etc.
4. It samples from these probabilities and adds the chosen token
5. The process repeats with the new token included

This is why LLMs generate text token-by-token—you see it streaming in!"

### Live Demonstration (7 minutes)

**Facilitator:** "Let's see this in action. I'm going to show you the same prompt with different temperatures."

**Demo Setup:** Use ChatGPT or similar, share your screen

**Prompt**: "Complete this sentence: The most surprising thing about artificial intelligence is"

**Run multiple times with notes:**

"Notice how each completion is different? The model is:
1. Looking at all previous tokens
2. Calculating probabilities for next token
3. Sampling (with some randomness)
4. Repeating

There's no pre-written answer—it's genuinely generating token by token based on patterns learned from training data."

**Key Takeaway:** "LLMs are sophisticated prediction machines. They don't 'know' facts—they predict what tokens are likely to come next based on patterns in massive amounts of text."

---

## PART 4: Hallucinations Deep Dive (10 minutes)

### Interactive Case Study (10 minutes)

**Facilitator:** "Now let's talk about the elephant in the room: hallucinations. Let me show you real examples."

#### Example 1: Confident But Wrong

**Display this exchange:**

*User: "What are the main health benefits of drinking hydrated water?"*

*LLM: "Hydrated water, also known as structured water, has several scientifically proven benefits including improved cellular hydration, enhanced nutrient absorption, and better detoxification. Studies from Stanford University in 2019 showed 34% improvement in cellular function."*

**Facilitator:** "Everything here is made up. There's no such thing as 'hydrated water' as a special category, no Stanford study, no 34% statistic. But it SOUNDS authoritative. Why?"

**Discussion Points (5 minutes):**

1. **Pattern Matching**: The model has seen many sentences structured like "[X] has scientifically proven benefits including [list]" and "[Studies from Y in YEAR showed Z% improvement]"

2. **No Fact-Checking**: The model can't verify if Stanford did this study or if the number is real

3. **Confidence Without Knowledge**: The model outputs probabilities that these tokens should follow these tokens—it has no concept of truth

4. **Training to Always Answer**: During training, the model learned to generate completions, not to say "I don't know"

#### Example 2: Real-World Impact

**Facilitator:** "Here's why this matters. Imagine asking:

*'What was the capital of Poland in 1800?'*

The correct answer is: there was no unified Poland in 1800 (it was partitioned). But an LLM might confidently say 'Warsaw' because:
- Warsaw IS the capital of Poland (true now)
- It WAS the capital before partition (true historically)  
- The pattern '[capital of X in YEAR]' usually has a simple answer

The model doesn't reason about complex historical context—it pattern-matches."

### Group Discussion (5 minutes)

**Facilitator:** "Let's discuss: When have you encountered hallucinations? Share in chat or speak up."

**Guide discussion around:**
- Made-up citations or sources
- Plausible but incorrect technical information
- Mixing real and fake facts
- Being overly specific with numbers/dates that are wrong

**Key Questions for Group:**
1. "How can you tell when an LLM might be hallucinating?"
2. "What types of questions are LLMs more likely to get wrong?"

**Facilitator Summary:**

"LLMs hallucinate more often when:
- **Information is rare** in training data
- **Complex reasoning** is required
- **Specific facts** (dates, numbers, names) are needed
- **Recent events** after their training cutoff are mentioned
- The question **assumes false premises**

**Key Takeaway:** Always verify important information. LLMs are tools for drafting, brainstorming, and explaining—not authoritative fact sources."

---

## PART 5: Breakout Room Activities (15 minutes)

**Facilitator:** "Now you'll apply what you've learned. We're splitting into breakout rooms for 12 minutes, then reconvening to share insights."

### Breakout Room Setup

**Divide participants into 3-4 groups (4-6 people each)**

**Provide each group with:**
- Shared document for notes
- Activity instructions
- 12 minutes work time + 3 minutes reporting

### Activity Option 1: Tokenization Detective

**Objective**: Understand how tokenization affects prompt efficiency

**Instructions**:
1. Your team works for a company with a tight token budget
2. You have this verbose prompt that needs optimization:

```
"I would greatly appreciate it if you could take some time to thoroughly analyze the following document and provide me with a comprehensive summary that includes all of the most important key points and main ideas that are discussed throughout the entire document."
```

3. Using the tokenizer tool:
   - Count the tokens in the original
   - Rewrite it to convey the same meaning with fewer tokens
   - Aim for 40-50% reduction
   - Test your rewrite in the tokenizer

4. **Discuss**: What did you learn about efficient prompting?

**Expected Outcome**: Teams discover that "Analyze this document and summarize the key points" conveys the same instruction with far fewer tokens.

### Activity Option 2: Hallucination Scenarios

**Objective**: Identify situations where LLMs are likely to hallucinate

**Instructions**:
1. Your team is creating guidelines for when to trust LLM outputs
2. Categorize these use cases as HIGH, MEDIUM, or LOW hallucination risk:
   - Summarizing a provided document
   - Explaining how photosynthesis works
   - Providing the release date of a movie from 2024
   - Writing Python code to sort a list
   - Citing specific academic papers on a niche topic
   - Translating English to Spanish
   - Providing medical diagnosis for symptoms
   - Explaining the concept of opportunity cost

3. For each HIGH risk scenario, propose a mitigation strategy

4. **Prepare**: 2-minute summary of your most interesting finding

**Expected Outcome**: Teams recognize that hallucination risk depends on verifiability, training data prevalence, and whether the information is in the prompt.

### Activity Option 3: Attention Mechanism Challenge

**Objective**: Understand how attention resolves ambiguity

**Instructions**:
1. For each sentence below, identify the ambiguous word and what it could refer to
2. Discuss what context clues the attention mechanism should focus on
3. Try rewriting the sentence to make it clearer (reducing the attention challenge)

**Sentences**:
- "The trophy doesn't fit in the suitcase because it is too large."
- "The city council denied the protestors a permit because they feared violence."
- "The cat chased the mouse until it got tired."
- "Sarah told Emma that she needed to work harder."

4. **Discuss**: How does understanding attention change how you write prompts?

**Expected Outcome**: Teams realize that clear writing helps models understand context better—ambiguity makes attention harder.

### Breakout Work (12 minutes)

*[Facilitator monitors rooms and is available for questions]*

---

## PART 6: Wrap-up & Insights (5 minutes)

### Group Sharing (3 minutes)

**Facilitator:** "Let's hear from each group. In 60 seconds or less, share your key insight or most interesting finding."

*[One representative from each group shares]*

### Key Takeaways Summary (2 minutes)

**Facilitator:** "Let's recap what we've learned today:

**1. Tokenization is fundamental**
- Text becomes numbers before processing
- Efficient prompts use fewer tokens
- Different languages tokenize differently

**2. The Transformer uses attention**
- Models look at all words when processing each word
- Multiple attention heads capture different relationships
- This is why context matters so much

**3. LLMs are prediction machines**
- They don't have knowledge—they have patterns
- Each token is predicted based on previous tokens
- No built-in truth verification

**4. Hallucinations are systemic**
- Result from prediction-based architecture
- More likely with rare information or complex reasoning
- Always verify important information

**Next Steps:**
- Experiment with the tokenizer tool on your own prompts
- Notice when LLMs might be hallucinating in your work
- Think about tokens when crafting prompts
- Join us for the next workshop where we'll dive deeper into prompt engineering!

Questions?"

---

## Facilitator Notes

### Timing Flexibility
- If discussions run long, you can shorten breakout rooms to 10 minutes
- Have backup questions ready if activities finish early
- The hallucinations section often generates lots of discussion—be prepared to extend it

### Technical Troubleshooting
- Have the tokenizer tool URL ready to paste in chat
- Test your screen sharing setup beforehand
- Have example screenshots ready in case the tool is slow

### Engagement Tips
- Use polls or chat to gather quick responses
- Encourage participants to keep cameras on in breakout rooms
- Walk through breakout rooms to check on progress and answer questions

### Follow-up Resources
- Share links to the tokenizer tool
- Provide reading list for deeper learning
- Collect questions that came up for future workshops

---

## Optional Extensions (If Time Permits)

### Advanced Topic: Temperature Parameter
"Temperature controls randomness in token selection. Low temperature = more predictable, high temperature = more creative. This is why the same prompt gives different results."

### Advanced Topic: Context Windows
"Models have limits on how many tokens they can process at once (context window). GPT-4 handles ~8K-128K tokens depending on version. Beyond that, the model 'forgets' earlier parts."

### Advanced Topic: Training Process
"Models learn by predicting next tokens on massive datasets (books, websites, articles). They adjust billions of parameters to get better at prediction. This is why they know patterns but not 'truth'."

---

## Post-Workshop Survey Questions

1. On a scale of 1-5, how well do you now understand how LLMs process language?
2. What was the most valuable insight from today's session?
3. What topic would you like to explore more deeply in future workshops?
4. How will this knowledge change how you use LLMs in your work?
5. Any feedback for improving this workshop?

---

**End of Live Session Script**
