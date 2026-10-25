---
name: simplify
description: Rewrite a file to beginner-level code following the AGENTS.md Code rules. Use when I say "simplify <file>", "@simplify <file>", or "make this beginner-friendly".
---

# Simplify

Same behavior, simpler shape. I'm a beginner: loops, ifs, functions, basic OOP only.

## Steps
1. Read the whole file, plus `AGENTS.md` > `## Code`. No path given: ask for it.
2. Report first, change nothing: language, line count, and every construct that breaks a rule as `file:line - what - which rule`.
3. Plan in 3-5 lines: what gets replaced by what (e.g. `map` chain -> `for` loop). Ask ok, wait.
4. Keep behavior identical. If a rule can't be kept without changing behavior (dropping `async`, dropping an error case), stop and say why.
5. Rewrite the file in place. Comments: short, why not what.
6. Re-read the result, check: no banned construct left, functions <20 lines, nesting <=2. List leftovers, 1 line each.

## Don't
- Add libraries, helpers, classes, or abstractions that weren't there.
- Rename things or reformat lines I didn't ask about.
- Run code or tests unless I say.
- Touch a file over ~200 lines in one go: ask to split it.

If the file already follows the rules, say so and change nothing.
