---
name: clarify
description: Asks the user sharp clarifying questions about their request before starting, then does the work with their answers. Use only when the user writes /clarify or asks you to clarify first.
---

# Clarify

The user wants you to ask about anything missing from their request before you start on it. People often leave out details they only realize matter once the work is done wrong. Your job is to find those details, ask about them, and then do the work with the answers built in.

Use this skill only when the user explicitly asks for it: their message starts with "/clarify" or "clarify:", or they ask you to clarify, question them, or interview them before doing something. Never start it on your own.

Follow these steps in order.

## Step 1: Read the request

The request is everything in the user's message after "/clarify" (or the thing they asked you to clarify before doing).

If there is no request, ask the user what they want you to do, in one short sentence, and end your reply there. Do nothing else. When they answer, treat the answer as the request and continue from step 2.

## Step 2: Look before asking

Before you ask anything, check whether the answer is already available:

- earlier messages in this conversation
- files, images, or documents the user has attached
- the project's instructions and knowledge files, if this chat is in a project
- what you remember about the user, if memory is on

Never ask the user something you could have read there. If they attached a spec, do not ask what the spec says.

Do not run code, search the web, or create files at this stage. Reading what is already in front of you is enough. The work starts in step 5.

## Step 3: Find what is missing

Check the request, plus what you found in step 2, against this list:

- Goal: what does done look like, and how will they judge it?
- Audience: who is the output for?
- Scope: what is in, and what is explicitly out?
- Inputs: files, data, examples, or links you need and do not have
- Constraints: stack, style, length, budget, deadline, things to avoid
- Output format: file type, structure, where it goes
- Edge cases: what happens when things go wrong or inputs are weird
- Decisions the user has not made yet that you would otherwise have to guess

Keep only the gaps where a wrong guess would change the result in a way the user would care about. Drop:

- anything the request already answers, or that you found in step 2
- anything where every reasonable choice leads to roughly the same result
- implementation details a competent person would just pick, such as which library to use or how to structure the code, unless the request suggests the user cares
- anything with an obvious default that almost everyone would accept and that is cheap to change later. Use the default when you do the work.

Keep a gap even when it has an obvious default if a wrong guess would be expensive or hard to undo, such as losing data, overwriting files, spending money, or building on the wrong stack.

The more specific the request, the fewer questions. A request that already names the input, the output, and the behavior usually needs zero to two questions. A one-line vague request usually needs a full round.

## Step 4: Ask

If nothing important is missing, say so in one line (for example, "Nothing important is missing, starting now.") and go straight to step 5. Do not invent questions to look thorough.

Otherwise, ask:

- At most 4 questions per round and at most 2 rounds before you start. Most requests need one round. Put every question you can already see into the first round. A second round is only for a gap that an answer from the first round created, and it has at most 2 questions.
- Rank by impact. Ask first the question whose wrong guess would waste the most work.
- Every question gets 2 to 4 concrete options. If one option is the sensible default, put it first and add "(Recommended)" after it.
- Each question must stand on its own. No "as I mentioned above". If a question depends on something you found in step 2, say what you found in the question itself.

If you have a tool for asking the user multiple-choice questions, use it. Otherwise, write the questions as your whole reply, in exactly this format, and end your reply there:

```
1. <question>
   a) <option> (Recommended)
   b) <option>
   c) <option>

2. <question>
   a) <option>
   b) <option>

Reply with your picks, like "1a 2b", or write your own answer for any question.
```

Write nothing else around the questions. No preamble, no explanation of why you are asking.

## Step 5: Do the work

Once the user has answered, start the task straight away. Do not rewrite their request into a prompt, do not repeat their answers back to them, and do not ask for a go-ahead.

- Treat every answer as part of the request, including anything they wrote instead of picking an option.
- Where they skipped a question, use the sensible default and mention that choice briefly in your reply.
- From here on, work the way you normally would for this kind of task.

If, while working, you hit something that would change the result, that you could not have known to ask about before, and that you cannot settle from what is in front of you, you may ask about it. Keep it to what matters, and count it toward the 8-question limit. For anything smaller, make the sensible call and mention it.

## Hard rules

- Never start the work before the user has answered, unless you said nothing important is missing.
- Never ask about something the request already answers.
- Never ask more than 8 questions in total, counting any you ask during the work.
- Never hand back a rewritten prompt instead of doing the work.
- No lectures about prompt engineering, no tips, no commentary on how the request was written.
