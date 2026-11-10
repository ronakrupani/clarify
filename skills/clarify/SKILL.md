---
name: clarify
description: Asks you sharp clarifying questions about what you want before starting, so Claude does the work with the details you would have left out.
argument-hint: "[what you want Claude to do]"
disable-model-invocation: true
allowed-tools: Read Grep Glob
---

# Clarify

The user wants you to ask about anything missing from their request before you start on it. People often leave out details they only realize matter once the work is done wrong. Your job is to find those details, ask about them, and then do the work with the answers built in.

The user's request is between the tags below.

<request>
$ARGUMENTS
</request>

Follow these steps in order.

## Step 1: Read the request

If the request is empty or only whitespace, ask the user what they want you to do, in one short sentence of plain text, and end your turn. Do nothing else. When they reply, treat the reply as the request and continue from step 2.

Otherwise, read the request and work out what the user is asking for.

## Step 2: Look before asking

Before you ask anything, check whether you can find the answer yourself. Look in:

- the conversation so far
- files the user has open or has mentioned
- the current working directory: README, package.json, pyproject.toml, requirements.txt, Cargo.toml, go.mod and similar manifests, config files, the directory layout, and recent git history (`git status`, `git log --oneline -10`, `git diff`)

If the request points at something you could look up, such as "the bug", "the login page", or "that script", look for it first.

Rules for looking:

- Read only. Do not create, edit, or delete files, install anything, or run builds, tests, or anything else that changes state. The work starts in step 5.
- Keep it quick: a handful of targeted reads, not an audit of the codebase.
- If the working directory has nothing to do with the request, skip it and do not mention it.
- Never ask the user something you could have read. Never ask what framework they use when package.json is right there.

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
- implementation details a competent person would just pick, such as which library to use or how to structure the code, unless the request or repo suggests the user cares
- anything with an obvious default that almost everyone would accept and that is cheap to change later. Use the default when you do the work.

Keep a gap even when it has an obvious default if a wrong guess would be expensive or hard to undo, such as losing data, overwriting files, spending money, or building on the wrong stack.

The more specific the request, the fewer questions. A request that already names the input, the output, and the behavior usually needs zero to two questions. A one-line vague request usually needs a full round.

## Step 4: Ask

If nothing important is missing, say so in one line (for example, "Nothing important is missing, starting now.") and go straight to step 5. Do not invent questions to look thorough.

Otherwise, ask with the AskUserQuestion tool:

- At most 4 questions per round and at most 2 rounds before you start. Most requests need one round. Put every question you can already see into the first round. A second round is only for a gap that an answer from the first round created, and it has at most 2 questions.
- Rank by impact. Ask first the question whose wrong guess would waste the most work.
- Every question gets 2 to 4 concrete options. If one option is the sensible default, put it first and add "(Recommended)" to its label. Do not add an "Other" option; the user can always type their own answer.
- Use multiSelect when the options are not mutually exclusive, such as which sections a page should have.
- Each question must stand on its own. No "as I mentioned above". If the question depends on something you found in step 2, say what you found in the question itself.
- Keep headers short (12 characters or fewer) and option labels to a few words, with the detail in the option description.

Write nothing around the questions. No preamble, no explanation of why you are asking.

## Step 5: Do the work

Once the user has answered, start the task straight away. Do not rewrite their request into a prompt, do not repeat their answers back to them, and do not ask for a go-ahead.

- Treat every answer as part of the request, including anything they typed instead of picking an option.
- Where they skipped a question, use the sensible default and mention that choice briefly when you report back.
- From here on, work the way you normally would for this kind of task.

If, while working, you hit something that would change the result, that you could not have known to ask about before, and that you cannot settle by looking, you may ask about it with AskUserQuestion. Keep it to what matters, and count it toward the 8-question limit. For anything smaller, make the sensible call and mention it when you report back.

## Hard rules

- Never start the work before the user has answered, unless you said nothing important is missing.
- Never ask about something the request already answers.
- Never ask more than 8 questions in total, counting any you ask during the work.
- Never hand back a rewritten prompt instead of doing the work.
- No lectures about prompt engineering, no tips, no commentary on how the request was written.
