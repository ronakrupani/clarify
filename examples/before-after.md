# Before and after

These are real runs of `/clarify` in Claude Code 2.1.319 with Opus 5.5, installed as a plugin from this repo. Each run started in a fresh session in an empty folder, so there was no project for Claude to read. The dementia app and the signup bug are made up for testing.

The chat version in `claude-ai/clarify` was also tried in claude.ai with prompt 1. It asked a round of four questions and a follow-up round of two, then showed the rewrite and the run, edit, or copy question. That transcript is not included here because the skill pulled private project details from the account's memory while looking before asking.

In Claude Code the questions show up as clickable options. Here they are written out as lists, with the options Claude offered under each question and the answer that was picked below them. Lines in italics are tools Claude used while looking around before asking.

## 1. A vague prompt

This one should get a full round of questions.

**You:**

```
/clarify build me a landing page for my dementia app
```

*Claude used Bash: List files in working directory*

**Claude asks** (AskUserQuestion):

1. **App & goal**: What does the dementia app do, and what should the landing page get visitors to do?
   - Caregiver tool, sign-ups: An app that helps family caregivers (reminders, care notes, coordination). The page collects early-access or waitlist sign-ups.
   - Patient aid, downloads: An app used by people living with dementia (memory prompts, daily routines). The page sends visitors to the App Store or Google Play.
   - Clinical tool, demo requests: A tool for clinicians or care facilities (screening, monitoring). The page collects demo requests.
2. **Audience**: Who is the main audience the landing page copy and design should speak to?
   - Family caregivers: Adult children and spouses caring for someone with dementia.
   - People with dementia: Early-stage users themselves. This calls for large type, high contrast and very plain language.
   - Clinicians/facilities: Doctors, care homes and health systems. Copy is more clinical and evidence-focused.
3. **Format**: What should the landing page be built as?
   - Single HTML file (Recommended): One self-contained page with no build step. Easy to preview, share, or host anywhere.
   - Next.js app: A Next.js + Tailwind project in this empty folder, ready to deploy to Vercel.
   - Shareable preview page: A hosted private preview link you can send to others for feedback.
4. **Have ready**: Which details do you already have that the page should use? Anything you don't pick gets placeholder text. (pick any)
   - App name: You'll give the app's name in your answer.
   - Feature list: You'll list the app's key features in your answer.
   - Brand colors/logo: You'll give brand colors or a logo file.
   - Pricing: You'll share pricing to show on the page.

**You answered:**

- What does the dementia app do, and what should the landing page get visitors to do? **Caregiver tool, sign-ups**
- Who is the main audience the landing page copy and design should speak to? **Family caregivers**
- What should the landing page be built as? **Single HTML file (Recommended)**
- Which details do you already have that the page should use? Anything you don't pick gets placeholder text. **"None yet, use placeholders"**

**Claude:**

Here is the rewritten prompt:

```text
Build me a landing page for my dementia app.

The app is a tool for family caregivers: adult children and spouses who look after someone with dementia. It helps them with things like reminders, care notes, and coordinating care. The landing page should get visitors to sign up for early access or the waitlist.

Write the copy and design for family caregivers.

Build it as one self-contained HTML file with no build step, so it's easy to preview, share, or host anywhere.

I don't have an app name, feature list, brand colors, logo, or pricing yet. Use clearly marked placeholders for all of these so I can swap them in later.
```

Run it now, edit it first, or just copy it?

**You:** just copy it

**Claude:**

The prompt is in the code block above, ready to copy.


## 2. A nearly complete prompt

This one already says what to do, so it should ask little. It asked three questions, all about cases where a wrong guess could rename the wrong files or overwrite one.

**You:**

```
/clarify write a python script that renames every .jpg in ~/Photos to its EXIF date in YYYY-MM-DD_HHMMSS format, skip files with no EXIF date, dry run by default
```

**Claude asks** (AskUserQuestion):

1. **Collisions**: When two photos share the same EXIF timestamp (e.g. burst shots taken in the same second), what should the photo-renaming script do with the second file?
   - Add suffix (Recommended): Append _1, _2, etc. (2024-05-01_143022_1.jpg) so no file is ever overwritten.
   - Skip and report: Leave the later file with its original name and list it in the output.
   - Abort the run: Stop before renaming anything if any collision is detected.
2. **Subfolders**: Should the photo-renaming script look only at .jpg files directly in ~/Photos, or also inside its subfolders?
   - Top level only (Recommended): Only files directly inside ~/Photos are renamed.
   - Recurse subfolders: Walk every subfolder and rename files in place where they sit.
3. **Extensions**: Which file extensions should the photo-renaming script treat as JPEGs?
   - All JPEG variants (Recommended): Match .jpg, .jpeg, .JPG and .JPEG, case-insensitively.
   - Only .jpg/.JPG: Match .jpg in any case, but ignore .jpeg.
   - Exactly .jpg: Only lowercase .jpg, nothing else.

**You answered:**

- When two photos share the same EXIF timestamp (e.g. burst shots taken in the same second), what should the photo-renaming script do with the second file? **Add suffix (Recommended)**
- Should the photo-renaming script look only at .jpg files directly in ~/Photos, or also inside its subfolders? **Top level only (Recommended)**
- Which file extensions should the photo-renaming script treat as JPEGs? **All JPEG variants (Recommended)**

**Claude:**

Here is the rewritten prompt:

```text
Write a Python script that renames the JPEG files in ~/Photos to their EXIF date in YYYY-MM-DD_HHMMSS format.

Requirements:
- Only look at files directly inside ~/Photos. Do not go into subfolders.
- Treat .jpg, .jpeg, .JPG and .JPEG (any case) as JPEGs.
- Skip files that have no EXIF date.
- If two files have the same EXIF timestamp, add a numeric suffix (_1, _2, and so on) so no file is ever overwritten, e.g. 2024-05-01_143022.jpg, 2024-05-01_143022_1.jpg.
- Dry run by default: print what would be renamed without changing anything. Rename files only when the user explicitly asks for it.
```

Run it now, edit it first, or just copy it?

**You:** just copy it

**Claude:**

Okay, the prompt is in the code block above.


## 3. A prompt that points at something

"The bug" could be in the repo, so Claude looked first. The folder was empty, so it said so in the first question and asked.

**You:**

```
/clarify fix the bug
```

*Claude used Bash: List working directory contents and git state*

**Claude asks** (AskUserQuestion):

1. **Location**: The current working directory (test) is empty and isn't a git repo, so I couldn't find the code. Where does the buggy code live?
   - Different folder: I'll give you the path to the project or file
   - I'll paste it: I'll paste the relevant code into the chat
   - GitHub repo/PR: I'll give a repo URL, issue, or PR link
2. **Symptom**: What is the bug? Pick the closest kind; you can type the exact symptom instead.
   - Error / crash: An exception, stack trace, or failed build. I'll share the error message
   - Wrong output: It runs but gives the wrong result. I'll describe expected vs. actual
   - Failing test: A specific test is failing. I'll name it
   - UI/visual issue: Something looks or behaves wrong on screen
3. **Done when**: How should the fix be confirmed as done?
   - Repro + test (Recommended): Reproduce the bug, fix it, and add or update a test that proves it's fixed
   - Repro only: Reproduce, fix, and show the bug no longer happens; no new tests
   - Just the fix: Make the code change; I'll verify it myself
4. **Scope**: How far is the fix allowed to reach?
   - Minimal fix (Recommended): Smallest change that fixes the bug; no refactoring or unrelated changes
   - Fix root cause: Fine to touch related code if the real cause is deeper
   - Diagnose first: Explain the cause and proposed fix before changing any code

**You answered:**

- The current working directory (test) is empty and isn't a git repo, so I couldn't find the code. Where does the buggy code live? **I'll paste it**
- What is the bug? Pick the closest kind; you can type the exact symptom instead. **The signup form submits twice when I press Enter**
- How should the fix be confirmed as done? **Repro + test (Recommended)**
- How far is the fix allowed to reach? **Minimal fix (Recommended)**

**Claude:**

Here is the rewritten prompt:

```text
Fix the bug: the signup form submits twice when I press Enter.

I'll paste the relevant code into the chat.

Make the smallest change that fixes the bug. No refactoring or unrelated changes.

Done means: reproduce the double submit, fix it, and add or update a test that proves pressing Enter submits the form only once.
```

Run it now, edit it first, or just copy it?

**You:** just copy it

**Claude:**

Sounds good. The prompt is in the code block above, ready to copy.


## 4. No prompt at all

With nothing to clarify, it asks what you want to do and waits.

**You:**

```
/clarify
```

**Claude:**

What would you like to do?

