# Rebuttal Done Right

Claude Code skills for writing rebuttals.

## Skills

| Skill | What it does |
|---|---|
| [`openreview-rebuttal-format`](skills/openreview-rebuttal-format/SKILL.md) | Writes or converts rebuttal replies in the markup of strong published OpenReview rebuttals: a bold heading per reviewer point with its label in red, `---` between points, and bold, red and italic markers where those rebuttals put them. It also covers tables, math, reviewer quotes, case studies and end-of-discussion summaries. A checker reports length, rendering problems and changed words. |

## Example

[A real OpenReview discussion, with each author reply shown before and after the skill](https://a-little-hoof.github.io/Rebuttal-Done-Right/). The page is [`docs/index.html`](docs/index.html).

## Install

```bash
git clone https://github.com/a-little-hoof/Rebuttal-Done-Right.git
mkdir -p ~/.claude/skills
ln -s "$PWD/Rebuttal-Done-Right/skills/openreview-rebuttal-format" ~/.claude/skills/
```

The link keeps the installed skill in step with the clone, so `git pull` updates it, and a running session picks it up without a restart. If you set `CLAUDE_CONFIG_DIR`, link into `$CLAUDE_CONFIG_DIR/skills` instead.

Claude Code then uses the skill when you ask for OpenReview Markdown. You can also call it with `/openreview-rebuttal-format`.

## Checker

The checker needs Python 3.9 or later and nothing else.

```bash
# length and rendering problems (add --limit N to check your venue's limit)
python3 skills/openreview-rebuttal-format/check_openreview_md.py reply.md

# words that differ between your draft and the formatted reply
python3 skills/openreview-rebuttal-format/check_openreview_md.py --diff draft.md reply.md
```
