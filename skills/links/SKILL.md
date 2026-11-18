---
name: links
description: Add a repo or website to the links README (aldrsze/links). Use when I paste a GitHub repo link or any site URL and want it listed, or say "add this to links".
---

# Links

Repo: `C:\Users\Miguel\projects\links` (Linux: `~/projects/links`)

One file: `README.md`. Nothing else.

## Bullet format

Two kinds of link, one bullet shape:

```
- [owner/repo](https://github.com/owner/repo) — short description
- [example.com](https://example.com) — short description
```

- GitHub repo → link text is `owner/repo`.
- Anything else → link text is the bare domain, no `www.`, no trailing slash.

Em dash before the description. No trailing period. Description is lowercase unless it starts with a proper name.

## Sections

In this order, don't reorder them:

```
Cheatsheets
CLI tools
Linux
Dotfiles
Self-hosted
```

Make a new section only when the entry fits none of them. New sections go at the end.

## Workflow

### 1. Read the link

Take the URL the user gave.

- GitHub repo (`github.com/owner/repo`, with or without `.git`, `/tree/...`, trailing `/`) → normalize to `https://github.com/owner/repo` and run:

```
gh repo view owner/repo --json description
```

- Any other URL → strip the query string and fragment, open the page, read the `<title>` and the `meta name="description"` tag.

If both routes fail, ask the user for a one-line description — never guess.

### 2. Pick the section

Use what the thing does, not what it's built with.

- Cheatsheets — lists, notes, reference docs, curated indexes
- CLI tools — terminal programs and TUIs
- Linux — distro, desktop, kernel, window managers, terminals
- Dotfiles — dotfile managers and config collections
- Self-hosted — services you run on your own machine or server

A site that is mostly a web app or browser tool goes in the section of the thing it does; if it's just a product homepage and nothing else fits, ask before inventing a section.

If two sections fit, ask. Numbered, with a suggested default.

### 3. Write the description

Trim the GitHub description or page description to about eight words. Keep the concrete noun ("self-hosted photo library"), drop marketing ("blazing fast", "next-gen").

If the description is empty or useless, ask.

### 4. Check for duplicates

Search `README.md` for the URL and for the link text.

- Same entry in the same section → stop, say it's already listed.
- Same entry in another section → allowed, tell the user and continue.

### 5. Insert

Alphabetical by link text (`owner/repo` or domain), ignoring case. Compare the whole string.

### 6. Confirm

Show the exact bullet and the section it goes in. Wait for my yes before editing.

### 7. Write

Edit `README.md` with the edit tool. Then show `git diff --stat` and the diff.

Do not commit. Do not stage. Do not push.

## Maintenance

This skill lives in two copies. When it changes, write the same file to both:

```
C:\Users\Miguel\projects\opencode-skills\skills\links\SKILL.md
C:\Users\Miguel\.config\opencode\skills\links\SKILL.md
```

And keep its row in `opencode-skills/README.md` up to date.
