# clarify

Answer five questions now instead of redoing the work later.

## What it is

`/clarify` is a skill for Claude Code and the Claude chat app that interviews you about a prompt before Claude does any work. It asks a few sharp questions with clickable options, then hands back a rewritten prompt with your answers folded in, which you can run, edit, or copy.

## Why I built it

My best results with Claude come from long, detailed prompts. The problem is that I usually only realize what I left out after Claude has already built the wrong thing. By then I'm rewriting the prompt and starting over.

So I wanted that moment to happen first. With `/clarify`, Claude reads my draft, looks through the project for anything it can answer on its own, and only asks me about what's actually missing. If my prompt is already complete, it says so and doesn't make up questions. Then it gives me back the prompt I should have written in the first place.

It only runs when I ask for it. Claude never starts it on its own, because Claude asking questions nobody asked for would be its own kind of annoying.

## Install

### Claude Code

Copy the skill folder into your personal skills directory:

```bash
git clone https://github.com/ronakrupani/clarify.git
```

```bash
mkdir -p ~/.claude/skills && cp -r clarify/skills/clarify ~/.claude/skills/
```

Then start a new session and type `/clarify`. If you'd rather have `git pull` keep it up to date, link the folder instead of copying it:

```bash
ln -s "$PWD/clarify/skills/clarify" ~/.claude/skills/clarify
```

### Claude Code, as a plugin

In Claude Code:

```
/plugin marketplace add ronakrupani/clarify
/plugin install clarify@clarify
```

Or from your terminal:

```bash
claude plugin marketplace add ronakrupani/clarify
```

```bash
claude plugin install clarify@clarify
```

Restart Claude Code or run `/reload-plugins` to pick it up. Claude Code puts the plugin name in front of plugin skills, so it shows up in the menu as `/clarify:clarify`. Typing `/clarify` still works, as long as nothing else you have installed uses that name. If you want it to be exactly `/clarify`, use the install above instead.

### Claude chat (claude.ai)

1. Download [clarify-claude-ai.zip](https://github.com/ronakrupani/clarify/releases/latest/download/clarify-claude-ai.zip) from the latest release.
2. In Claude, open Customize, then Skills, click Add, choose Upload skill, and pick the zip.
3. Make sure the skill is turned on. Custom skills also need code execution, which is under Settings, Capabilities.

Then type `/clarify` and your prompt in any chat. The chat version lives in [claude-ai/clarify](claude-ai/clarify/SKILL.md). It works the same way, except that it looks in the conversation, your attached files, and your project and memory instead of a code repo.

## Usage

Type `/clarify` followed by the prompt you were about to send.

Before:

```
/clarify fix the bug
```

Claude checks the project and git history for the bug first. When it can't find it, it asks where the code lives, what the bug is, how you'll know it's fixed, and how far the fix is allowed to reach. After three clicks and one typed answer, it hands back:

```text
Fix the bug: the signup form submits twice when I press Enter.

I'll paste the relevant code into the chat.

Make the smallest change that fixes the bug. No refactoring or unrelated changes.

Done means: reproduce the double submit, fix it, and add or update a test that proves pressing Enter submits the form only once.
```

Then it asks whether to run it now, edit it first, or just leave it for you to copy. Nothing gets built until you say run.

There are full transcripts of four test runs, including a vague prompt, a nearly complete one, and an empty one, in [examples/before-after.md](examples/before-after.md).

## License

MIT. See [LICENSE](LICENSE).
