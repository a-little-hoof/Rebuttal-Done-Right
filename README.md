# Rebuttal Done Right

Claude Code skills for writing rebuttals.

## Skills

| Skill | What it does |
|---|---|
| [`openreview-rebuttal-format`](skills/openreview-rebuttal-format/SKILL.md) | Writes or converts rebuttal replies as Markdown that renders correctly on OpenReview: one bold lead line per reviewer point, tables with captions, math, reviewer quotes, case studies and end-of-discussion summaries. A checker reports length, rendering problems and changed words. |

## Install

```bash
git clone git@github.com:a-little-hoof/Rebuttal-Done-Right.git
mkdir -p ~/.claude/skills
cp -r Rebuttal-Done-Right/skills/openreview-rebuttal-format ~/.claude/skills/
```

Claude Code then uses the skill when you ask for OpenReview Markdown. You can also call it with `/openreview-rebuttal-format`.

## Checker

The checker needs Python 3.9 or later and nothing else.

```bash
# length and rendering problems
python3 skills/openreview-rebuttal-format/check_openreview_md.py reply.md

# words that differ between your draft and the formatted reply
python3 skills/openreview-rebuttal-format/check_openreview_md.py --diff draft.md reply.md
```
