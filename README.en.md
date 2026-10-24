# personal-ai

**A personal system of notes and rules that lets AI remember you.**

[日本語版はこちら](README.md)

## What is this?
A personal project that lets AI tools such as ChatGPT or Claude act like an assistant who already knows you, so you do not have to explain yourself from scratch every time.

I write things about myself (work, schedule, preferences, to-dos) in plain text files, the kind a basic text editor can open, and organize them in folders. The AI reads those folders to answer, and adds notes to them when needed.

## Background (why I built it)
Today's AI tools had four annoyances:
- What they "remember" is stored separately by each service
- It is hard to see what is remembered
- Every time I switch services, I have to explain everything again
- There is a limit to how much they can keep in mind, so things do not fit once the information grows

So I decided to keep that memory in my own files instead of inside a service. Whichever AI I use, it knows me. As the amount grows, I organize it in folders and let the AI read only the parts it needs, so I hit the limit less often.

I also had two more aims:
- **Use it from any device.** If the memory and rules sit on my home computer, I can have the AI work on that computer from another device when I am out.
- **Make it my own.** I do not split conversations by topic; I handle work, schedule and shopping in one conversation, and switch between topics with rules and folders.

## Concept
1. **Keep memory in my own files.** The AI is the one that reads and writes. If I change the AI or the model, the memory stays.
2. **Separate what the AI does from what a human decides.** The AI records notes, adds calendar entries and writes email drafts. A human sends the email and deletes anything.
3. **Turn mistakes into rules.** When the AI gets something wrong, I add a rule so it does not repeat the mistake.

## How I use it (examples)
- **Chatting for advice (the way I use it most).** It answers questions and consults with me in light of what it knows about me. When it looks something up, it keeps the sources too. Anything new that comes up in the conversation is recorded in the files on the spot, so it is available in the next conversation.
- Every morning, it puts today's schedule, deadlines and to-dos on one screen.
- Every day, it picks AI and IT news and saves it with "facts" and "guesses" kept apart.
- Every week, it reviews the folder structure, rules, memory and scheduled runs, and proposes ways to cut waste and contradictions (it makes small cleanups itself).

## How is this different from a regular chat?
| | Regular chat | personal-ai |
|---|---|---|
| About me | I have to explain each time (even with a memory feature, the contents are hard to see) | It is written in my own files, so I do not re-explain. I can look at it and fix it |
| New things learned | Harder to use once the conversation ends | Recorded in the files on the spot, so it is available in the next conversation |
| What it can do | Only answers | Answers, and also records notes, adds calendar entries and writes email drafts (a human sends and deletes) |
| Mistakes | The same mistake is likely to happen again | Mistakes become written rules, so repeats go down (for example: always check the time) |
| Taking the initiative | Acts only when I speak to it | Acts on its own at set times, such as the morning summary |
| Switching | Switching services means starting over | Even if I change the AI, I can continue by letting it read the same files |

Note: "regular chat" means typical use and varies by service and settings. I have not yet measured the effect (speed or accuracy of answers).

---
Everything below is a detailed explanation of how it works (for engineers).

## Design principles
1. **Memory lives in files; the AI reads and writes them.** The source of truth is Markdown. If I change the AI or the model, the memory stays. It is readable, editable and portable.
2. **Limit what the agent reads.** Numbered folders and an index let the agent open only what it needs, not everything. The aim is lower token use and faster answers. I have not measured the effect yet.
3. **Separate "do" from "approve."** The agent may record notes, add calendar entries and write email drafts. A human sends the email and permanently deletes anything.
4. **Never present the unverified as fact.** The agent must separate facts, guesses and unverified points, and must not write steps or numbers it cannot back up.
5. **Turn failures into rules.** Mistakes that happen in daily use are written into AGENTS.md or the rules files.

## Structure (example)
```
AGENTS.md          # Source of truth: reading order and the rules that must not be broken
CLAUDE.md          # For Claude Code: bridges to AGENTS.md with @import
00-rules/          # How to think, answer modes, sources, output format, placement, recording
01-private/        # Sensitive information (outside git, never sent out)
02-knowledge/      # Facts about me, read by the agent while it works
03-output/         # Deliverables
04-resources/      # Research notes and original materials (never edited)
05-todo/           # Pending tasks
99-trash/          # Where files go instead of being deleted
```
The folders are numbered so the agent has to search fewer times.

## Where "do" ends and "approve" begins
| Action | Agent | Human |
|---|---|---|
| Record and update Markdown | Does it | |
| Add calendar entries | Does it | |
| Email | Writes a draft | Sends it |
| Deleting files | Moves them to 99-trash | Deletes for good |
| Pushing to GitHub or elsewhere | Never | Only when explicitly told |

## Rules that came from failures (examples)
These are real mistakes from daily use and the rule each one produced. The wording is rewritten as examples.

| What happened | Rule |
|---|---|
| The agent guessed the time of day | Before saying anything about time, schedule or "now," run `date` and check |
| The agent stopped researching at "no mention found" | Check in order: the product's own terms, official support, the previous model, the browser, the original text, then ask the owner. If nothing turns up, write what was checked |
| A tool failed and the agent rephrased the error message as the cause | Run a diagnosis first, and state only what was confirmed as the cause |
| Facts and guesses got mixed | Write "fact / guess / unverified" separately |
| The agent wrote plausible but unconfirmed steps or screen names | Say what is known and what is not, and ask to see the screen if needed |

## Scheduled runs (examples)
- Every morning: summarize today's schedule, deadlines and checklist on one screen
- Every day: pick AI and IT news and save it with facts and guesses separated
- Every week: review the folder structure, rules, memory, scheduled runs and backups to optimize the operation (small cleanups are done by the agent; bigger changes are proposals only)

The procedures live in `00-rules/`, and results are saved to `03-output/`.

## How to build your own
1. Create empty folders as above (put `01-private/` in `.gitignore`)
2. Write the index and the rules to keep in `AGENTS.md`
3. Move existing memory (notes, AI service history) into `02-knowledge/`, fact by fact
4. When the agent makes a mistake in use, add a rule
5. Once it feels natural, add scheduled runs

## Track record (as of 2026-10-24, my own setup)
- Started on 2026-10-05; 20 days in (commits on 12 of them)
- 262 commits
- 8 rule files (`00-rules/`) and 14 knowledge files (`02-knowledge/`)
- 3 scheduled runs (morning summary, news, weekly review); the morning summary and the news are saved automatically every day (while the Mac and the app are running)

## Next
- Measure the effect (files read per question, mistakes before and after a rule is added)
- Turn the failure records into test cases for the rules

## What is not published
Personal data, real records and credentials are not in this repository. All examples are fictional or rewritten.
