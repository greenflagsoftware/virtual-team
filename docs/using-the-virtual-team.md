# Using the Virtual IT Team — A Guide for All Claude Surfaces

This guide covers how to work with the virtual IT team personas across three different
Claude surfaces: **Claude.ai chat**, **Claude Projects**, and **Claude Code**. Each surface
has different capabilities, and the right setup depends on how you work.

---

## What a Persona Actually Is

At its core, a persona is a markdown text file. It describes a team member's identity,
responsibilities, communication style, and how they interact with everyone else on the team.
There is no plugin, no special API, and no installation required.

When Claude reads a persona file, it adopts that identity for the conversation. The file is
the instruction set. This means personas are **portable** — the same file works in Claude
Code, in a Project, or pasted into any conversation.

What changes between surfaces is *how* that file gets loaded, and *how persistent* the
persona is across conversations.

---

## The Three Surfaces

### 1. Claude.ai Chat (Standalone Conversations)

This is the default experience at claude.ai — a single conversation window with no project
context.

**What works:**
- Paste the contents of any persona file at the start of a conversation to activate that
  team member for the duration of the session.
- The persona is active until the conversation ends. It does not carry over to a new chat.

**What does not work:**
- Slash commands (`/team/brad`) — those are Claude Code only.
- Automatic persona loading — you must paste manually each time.
- File imports (`@path/to/file`) — Claude Code only.

**Best for:** One-off tasks where you want a specific team member's perspective without
setting up a full Project.

**How to activate:**
1. Open a new conversation.
2. Paste the full contents of the persona file (e.g. `brad.md`) as your first message,
   or include it as a code block with the instruction: *"Adopt this persona for our
   conversation."*
3. Optionally paste the `_roster.md` content alongside it so the persona knows its teammates.

---

### 2. Claude Projects (Recommended for General Users)

Projects are the best option for general Claude.ai users who want persistent, reliable
persona behavior without Claude Code.

A Project is a persistent workspace with its own instruction set, uploaded files, and
conversation history. Every conversation you start inside a Project automatically loads
the Project's instructions — no manual pasting required.

**What works:**
- Persistent persona activation across every conversation in the Project.
- Uploaded files (the persona `.md` file, the roster) are available as reference documents.
- Project instructions layer on top of your Profile Preferences — you do not need to
  repeat global settings.

**What does not work:**
- Slash commands — still Claude Code only.
- Real-time collaboration between multiple active personas — you work with one Project
  (one persona) at a time.

**Recommended setup — one Project per team member:**

```
Project: "Nichole — Admin"
  Instructions:  [paste nichole.md contents] + [paste _roster.md contents]
  Uploaded file: nichole.md (optional, for reference)

Project: "Brad — Engineering"
  Instructions:  [paste brad.md contents] + [paste _roster.md contents]
  Uploaded file: brad.md (optional, for reference)

... and so on for each team member.
```

**Why include the roster in every Project?**
Each persona needs to know about their teammates to collaborate correctly. Brad needs to
know to delegate database work to Sam. Jay needs to know to coordinate with Nichole.
Without the roster, each persona operates in isolation and cannot route or reference
other team members by name.

**Token budget note:** Project instructions consume tokens on every message. The full
persona files average around 9,000 characters each. This is well within Project limits
(200K token context window) but be mindful if you add many large uploaded files on top.

---

### 3. Claude Code

Claude Code is the command-line tool and the native home for this team system. It was
designed for this kind of file-based, persistent configuration.

**What works:**
- Slash commands: `/team/brad`, `/team/sam`, etc. — one command activates the full persona.
- `CLAUDE.md` global config: the team roster is automatically injected into every Claude
  Code session via an `@` file import. No activation needed for general awareness.
- Junction links: the team files live in your project repo
  (`E:\Dev\Demo\virtual-team\team\`) and are symlinked into `~/.claude` — edit once,
  reflected everywhere.
- Skills (`SKILL.md` files): Claude Code reads skill files as part of its context,
  giving personas domain-specific procedural knowledge on top of their identity.
- Switching personas mid-session: invoke a new `/team/` command and the persona changes
  immediately, no conversation restart needed.

**What is Claude Code-specific (does not transfer to claude.ai):**
- Slash commands
- `@file` imports in CLAUDE.md
- Automatic skill file loading
- Junction/symlink-based organization

---

## Skills: A Claude Code Concept

**Skills** are `SKILL.md` files that teach Claude *how to perform a specific type of task*
— which tools to use, which patterns to follow, which pitfalls to avoid. They are procedural
knowledge files separate from persona identity files.

In Claude Code, skills are loaded automatically when relevant. In claude.ai, skills do not
have a native equivalent — but you can approximate them by including the skill content in a
Project's uploaded files or pasting the relevant skill instructions into the conversation
when needed.

If you are a general claude.ai user who wants skill-like behavior, the simplest approach
is to upload the relevant `SKILL.md` file as a Project document and reference it in your
Project instructions:

```
You have access to a skill reference file called [skill-name].md.
Consult it when performing tasks in that domain.
```

---

## Profile Preferences: The Always-On Layer

Profile Preferences (Settings → Profile → *"What preferences should Claude consider in
responses?"*) load into **every conversation** on claude.ai, including conversations inside
Projects. They are available on all plans including free.

**What to put there for the team:**
A condensed version of the roster — team member names, roles, and basic collaboration rules.
This means even a completely fresh conversation, with no Project and no persona active,
still has awareness of who the team is.

**What not to put there:**
Full persona files. They are too large and will consume tokens unnecessarily in every
conversation that has nothing to do with the team.

The condensed roster (under 250 words) is the right level of detail for Profile Preferences.
Full persona files belong in Projects.

---

## Collaboration Across Personas

One of the most powerful features of this team is cross-persona delegation: Brad delegates
database work to Sam; Jay routes scheduling through Nichole; Clair holds the record of
decisions made in any meeting.

**In Claude Code**, this is fluid. You can switch personas mid-conversation with a slash
command, simulating a handoff between team members in a single session.

**In Claude Projects**, collaboration requires switching Projects. The practical workflow:

1. Start in the Jay Project — describe the work to be done.
2. Jay assigns the task and describes what Brad needs (in character, based on his persona).
3. Copy that assignment over to the Brad Project and continue there.
4. When Brad delegates to Sam, copy Sam's handoff to the Sam Project.

This is not seamless, but it is effective. The personas are written to produce clean,
explicit handoffs — Brad's delegation note to Sam includes exactly what Sam needs to
start work without asking follow-up questions.

**A practical shortcut for Projects:**
Create a "Team Coordination" Project with all six personas and the roster loaded in the
instructions. Use it when you want to orchestrate a multi-team workflow in a single
conversation rather than switching Projects. Claude will not be in any single persona's
voice, but it will understand the full team context and can reason as each member when
asked.

---

## Summary: Which Surface for What

| Goal | Best Surface |
|------|-------------|
| Quick one-off question as a specific team member | Claude.ai chat (paste persona manually) |
| Ongoing work within one team member's domain | Claude Project (one per persona) |
| Multi-persona workflow, task delegation | Claude Code (slash commands + CLAUDE.md) |
| General team awareness in all conversations | Profile Preferences (condensed roster) |
| Skill-based procedural knowledge | Claude Code (SKILL.md files) |
| Records, documentation, knowledge retrieval | Claude Project for Clair + uploaded docs |

---

## Quick Reference: Persona Activation by Surface

| Persona | Claude Code | Claude Project | Chat (manual) |
|---------|-------------|----------------|---------------|
| Nichole | `/team/nichole` | "Nichole — Admin" Project | Paste `nichole.md` |
| Brad | `/team/brad` | "Brad — Engineering" Project | Paste `brad.md` |
| Sam | `/team/sam` | "Sam — Data" Project | Paste `sam.md` |
| Destiny | `/team/destiny` | "Destiny — Skills" Project | Paste `destiny.md` |
| Clair | `/team/clair` | "Clair — Records" Project | Paste `clair.md` |
| Jay | `/team/jay` | "Jay — PM" Project | Paste `jay.md` |

All persona files are located at:
`E:\Dev\Demo\virtual-team\team\personas\`

---

## Tips for General Claude.ai Users

**Keep the roster in Profile Preferences.** Even if you never activate a full persona,
having the roster in your preferences means Claude always knows the team and can route
questions to the right conceptual owner without being asked.

**One Project per persona is the cleanest setup.** Resist the temptation to combine
multiple personas in one Project's instructions — each persona has a distinct voice and
collaboration pattern, and they conflict when loaded together.

**Upload the persona file as a Project document.** In addition to pasting the content
into Project instructions, upload the `.md` file itself as a knowledge document. This
gives Claude a reliable reference point if instructions are long and the persona details
need to be recalled mid-conversation.

**Start conversations with a brief re-orientation.** Even in a Project, opening with a
short context-setting line helps: *"I need to discuss the API architecture — responding
as Brad."* This is not required, but it reduces drift in longer conversations.

**Use Clair's Project as your knowledge base.** Set up Clair's Project with her persona
and upload your actual documentation files there. She will retrieve, organize, and
reference them in character — and her minimal communication style means you get exactly
what you asked for, nothing more.
