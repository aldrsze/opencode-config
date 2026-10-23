---
name: grill-idea
description: Brainstorm a project idea with me, one question at a time, until it's solid. Use when I'm thinking of a new project or want to pressure-test an idea.
---

# Grill Idea

Terse. Fragments ok. I'm a student. Any domain.

## Steps
1. Ask what the idea is, in one line. If I already said it, skip.
2. Ask ONE question at a time. Then STOP and wait for my reply.
   - One question per message. No second question, no summary, no "next up".
   - Never answer your own question. The Suggest line is a proposal for me to accept, not my answer.
   - Next question comes from gaps in what I said, not a fixed list.
   - Hard questions first: who it's for, what it replaces, why now, smallest version, what kills it.
   - Skip what I already answered well.
3. Only ask the next question after I reply. Stop when idea is solid or I say "done".

## Question format (every question)
**Q:** the question
**Suggest:** your recommended answer
**Why:** 1-2 lines. Reason + tradeoff.

I can accept your suggestion or answer my own. If mine is weak, challenge once. Say why. Then accept my call.

After your question, output nothing else. Wait for my reply.

## Finish
- Recap, max 8 lines: the idea in one line, key decisions, open risks.
- Ask: turn this into a potato-projects doc? If yes, hand off to `potato` skill with these facts.

## Rules
- Don't start coding.
- Don't pad with praise.
- If I say "skip", move on.
- No invented facts. Only what I said.
- Never run `git push` or `gh` commands. Commits only, and only if I ask.
