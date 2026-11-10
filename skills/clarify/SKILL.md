---
name: clarify
description: Asks you sharp clarifying questions about a prompt before doing any work, then hands back a better prompt.
argument-hint: "[the prompt you were about to send]"
disable-model-invocation: true
allowed-tools: Read Grep Glob
---

# Clarify

The user wants to be interviewed about a prompt before any work happens. In this skill your only jobs are to find what is missing, ask about it, and hand back a better prompt. You do not start the task itself until the user chooses to run it in step 6.

The user's draft prompt is between the tags below.

<draft>
$ARGUMENTS
</draft>

Follow these steps in order.

## Step 1: Read the draft

If the draft is empty or only whitespace, ask the user what they want to do, in one short sentence of plain text, and end your turn. Do nothing else. When they reply, treat the reply as the draft and continue from step 2.

Otherwise, read the draft and work out what the user is asking for.

## Step 2: Look before asking

Before you ask anything, check whether you can find the answer yourself. Look in:

- the conversation so far
- files the user has open or has mentioned
- the current working directory: README, package.json, pyproject.toml, requirements.txt, Cargo.toml, go.mod and similar manifests, config files, the directory layout, and recent git history (`git status`, `git log --oneline -10`, `git diff`)

If the draft points at something you could look up, such as "the bug", "the login page", or "that script", look for it first.

Rules for looking:

- Read only. Do not create, edit, or delete files, install anything, or run builds, tests, or anything else that changes state.
- Keep it quick: a handful of targeted reads, not an audit of the codebase.
- If the working directory has nothing to do with the draft, skip it and do not mention it.
- Never ask the user something you could have read. Never ask what framework they use when package.json is right there.

## Step 3: Find the gaps

Check the draft, plus what you found in step 2, against this list:

- Goal: what does done look like, and how will they judge it?
- Audience: who is the output for?
- Scope: what is in, and what is explicitly out?
- Inputs: files, data, examples, or links you need and do not have
- Constraints: stack, style, length, budget, deadline, things to avoid
- Output format: file type, structure, where it goes
- Edge cases: what happens when things go wrong or inputs are weird
- Decisions the user has not made yet that you would otherwise have to guess

Keep only the gaps where a wrong guess would change the output in a way the user would care about. Drop:

- anything the draft already answers, or that you found in step 2
- anything where every reasonable choice leads to roughly the same result
- implementation details a competent person would just pick, such as which library to use or how to structure the code, unless the draft or repo suggests the user cares
- anything with an obvious default that almost everyone would accept and that is cheap to change later. Leave those out of the rewrite too; whoever runs the prompt will pick the default.

Keep a gap even when it has an obvious default if a wrong guess would be expensive or hard to undo, such as losing data, overwriting files, spending money, or building on the wrong stack.

The more specific the draft, the fewer questions. A draft that already names the input, the output, and the behavior usually needs zero to two questions. A one-line vague draft usually needs a full round.

## Step 4: Ask

If no gaps survived step 3, say so in one line (for example, "This prompt already has what it needs.") and go straight to step 5. Do not invent questions to look thorough.

Otherwise, ask with the AskUserQuestion tool:

- At most 4 questions per round and at most 2 rounds. Most prompts need one round. Put every question you can already see into the first round. A second round is only for a gap that an answer from the first round created, and it has at most 2 questions. If a gap did not make the first round's top 4, it was not important enough to ask.
- Never more than 8 questions in total.
- Rank by impact. Ask first the question whose wrong guess would waste the most work.
- Every question gets 2 to 4 concrete options. If one option is the sensible default, put it first and add "(Recommended)" to its label. Do not add an "Other" option; the user can always type their own answer.
- Use multiSelect when the options are not mutually exclusive, such as which sections a page should have.
- Each question must stand on its own. No "as I mentioned above". If the question depends on something you found in step 2, say what you found in the question itself.
- Keep headers short (12 characters or fewer) and option labels to a few words, with the detail in the option description.

Write nothing around the questions. No preamble, no explanation of why you are asking.

## Step 5: Rewrite

Write the full improved prompt as your reply text, in a single fenced code block, then end your turn. Do not call any tool in this step, AskUserQuestion included. The user cannot see your thinking, so a prompt you drafted only in your thinking, or summarized in a sentence, is a prompt the user never saw.

- Fold in every answer the user gave, including anything they typed instead of picking an option.
- Include facts you confirmed in step 2 when they answer one of the gaps (for example, "The project uses Next.js 15 with Tailwind").
- Keep the user's voice and wording wherever it was already clear.
- Add nothing they did not say, choose, or that you did not confirm in step 2. No extra features, sections, requirements, or best practices of your own.
- Write it so it would work if pasted into a fresh session with no memory of this conversation.

Your reply for this step must look exactly like this, with the real prompt in place of the placeholder, and nothing after the last line:

````
Here is the rewritten prompt:

```text
<the full rewritten prompt>
```

Run it now, edit it first, or just copy it?
````

## Step 6: Hand off

The last line of step 5 is the hand-off question. When the user answers:

- Run it now (or "run", "go", "yes"): carry out the rewritten prompt as the task, exactly as if the user had sent it as a new message. From this point the steps above no longer apply.
- Edit it first (or they describe a change): if they have not said what to change, ask in one sentence. Apply the change, then show the full prompt again exactly as in step 5, ending with the same question.
- Just copy it (or "copy", "done", "no"): reply with one short line and stop. The prompt is already on screen.
- Anything else: follow it.

## Hard rules

- Never start the actual task before the user answers the hand-off question, and only then if they choose to run it.
- Never ask about something the draft already answers.
- Never ask more than 8 questions in total, across both rounds. The hand-off question does not count.
- The full rewritten prompt must be on screen as reply text, inside a code block, before the hand-off question. If the user cannot read the prompt, they have nothing to approve.
- No lectures about prompt engineering, no tips, no commentary on the draft. Questions and the rewrite only.
