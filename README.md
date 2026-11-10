# clarify

Answer five questions now instead of redoing the work later.

## What it is

`/clarify` is a skill for Claude Code and the Claude chat app that makes Claude ask about what you left out before it starts working. It asks a few sharp questions with clickable options, then does the work with your answers built in.

## Why I built it

My best results with Claude come from long, detailed prompts. The problem is that I usually only realize what I left out after Claude has already built the wrong thing. By then I'm explaining what I actually meant and starting over.

So I wanted that moment to happen first. With `/clarify`, Claude reads my request, looks through the project for anything it can answer on its own, and only asks me about what's actually missing. Then it gets on with the work, with the details I would have forgotten already included. If my request is already complete, it says so and starts right away instead of making up questions.

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

Then type `/clarify` and your request in any chat. The chat version lives in [claude-ai/clarify](claude-ai/clarify/SKILL.md). It works the same way, except that it looks in the conversation, your attached files, and your project and memory instead of a code repo.

## Usage

Type `/clarify` followed by whatever you were about to ask for.

Before:

```
/clarify fix the bug
```

Claude checks the project and git history for the bug first. When it can't find it, it asks where the code is, what the bug does, how to check the fix, and how far the change should go. I pasted the code, typed "the signup form submits twice when I press Enter", and took the defaults for the other two. Claude went straight to the fix: the keydown handler was calling submit on the same Enter press that already submits the form, so it said to delete that handler and showed the corrected code.

If it hits something important partway through that it couldn't have known to ask about, it can come back with another question, but never more than eight in total.

There are full transcripts of four test runs, including a vague request, a nearly complete one, and an empty one, in [examples/before-after.md](examples/before-after.md).

## License

MIT. See [LICENSE](LICENSE).
