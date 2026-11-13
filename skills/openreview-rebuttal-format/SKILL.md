---
name: openreview-rebuttal-format
description: Write or convert rebuttal replies, reviewer responses and end-of-discussion summaries as Markdown that renders correctly on OpenReview, using one bold lead line per reviewer point. Use when the user asks for "md code" for OpenReview, wants a draft reply formatted, or needs tables, math, reviewer quotes or case studies to display properly in an OpenReview comment.
---

# OpenReview rebuttal format

Each reviewer point gets one bold lead line that names the point, its topic and the answer. The author's own paragraphs follow, with tables, quotes and case studies formatted so OpenReview displays them as intended.

## How OpenReview renders a comment

These rules come from OpenReview's frontend source.

- **Line breaks:** comments are Markdown with GitHub-style tables, and single line breaks are ignored. Consecutive lines join into one paragraph, so put a blank line between every paragraph, list, table, quote and heading.
- **No HTML or images:** raw HTML and images (`![](...)`) appear as literal text, so you can't choose colors. To point at a figure, link to its page of the PDF: `[Fig. 3](https://openreview.net/pdf?id=<forum-id>#page=7)`.
- **Sizes and colors:**
  - `#` and `##` headings are large; `###` is only slightly larger than normal text.
  - `####` and smaller headings are smaller than normal text.
  - `**bold**` stays at normal size and shows in dark blue.
  - Text in backticks shows in red, monospaced. Backticks are the only way to make text red. Markdown and math inside them don't render.
- **Math:** MathJax renders `$...$` and `$$...$$`, but Markdown processes the text first.
  - A backslash before punctuation is removed, so `\,` arrives as `,` and `\{` as `{`. Double the backslash: `\\,`, `\\;`, `\\!`, `\\{`, `\\}`.
  - A LaTeX line break needs four backslashes: `\\\\`.
  - A pair of `*` turns into italics. Write `\*`, or use `\ast` or `\times`. If a `_` causes italics, write `\_`.

## Reply structure

1. An optional opening line of thanks.
2. One block per reviewer point, starting with a bold lead line:
   `**W1 – <topic> – <one-sentence answer>.**`
   - Write the answer in active voice ("We provide ...", not "... are provided").
   - Points that share one answer share one line: `**W2, Q1 – <topic> – <answer>.**`
   - For a red label at normal size, put the label in backticks: ``**`W1 – <topic>` – <answer>.**``
   - For a larger, more prominent line, use a heading instead: ``## `W1 – <topic>`: <answer>``. Use one style throughout a reply.
3. The author's paragraphs for that point.
4. **Tables:**
   - Put the caption as a bold line above the table: `**Table R1. <caption>.**`
   - Number the tables R1, R2, … so they don't clash with the paper's tables.
   - Right-align number columns with `---:`, and write "—" in empty cells.
   - Bold the best value in each column. If the proposed method ties it, bold the method's value too. A caption that explains the bold says "best", not "higher".
5. **Reviewers:** quote them in italics, `*"..."*`. Refer to them by their ID in backticks, such as `` `AbC1` ``.
6. **Sub-points:** nested bullets, indented 4 spaces under the main bullet.
7. **Examples and case studies:** a block quote with `>` at the start of every line. End each line except the last with `\`, or the lines run together.
8. **Dividers:** `---` on its own line, with a blank line above it. Without the blank line, the line above turns into a heading.

## Templates

Reply to one reviewer:

```markdown
We thank the reviewer for <what they noted>. Below, we address each point.

**W1 – <topic> – We <answer>.**

<paragraph in the author's words>

**Table R1. <caption>.**

| Setting | Metric A | Metric B |
|---|---:|---:|
| <row> | 0.00 | 0.00 |

<paragraph>

**W2, Q1 – <topic> – We <answer>.**

<paragraph>

> <Input>: <text>\
> **Baseline output:** <text> // <why it fails>\
> **Our output:** <text> // <why it works>
```

Summary for the area chair at the end of the discussion:

```markdown
We summarize the reviewers' main concerns and how we addressed them.

* **`AbC1` asks for <request> — added.**
    * *"<short quote from the review>"*
    * <what we added, and where in the revision>.
* **`XyZ2` and `AbC1` question <issue> — addressed.**
    * *"<quote>"*
    * <our answer>.
    * **Both reviewers acknowledged our response.**

---

**<one closing sentence>**
```

## Editing rules

When converting a draft:

- **Keep the author's words.** Converting a draft to this format changes only its structure. Don't reword, shorten or polish the author's sentences. If you add, remove or change any word, list each change after the Markdown. A new lead line counts as a change.
- **Lead lines** are short, plain and in active voice. Don't add claims, hedges or apologies the author didn't write.
- **Drop a bold topic that opens a paragraph** when the new lead line already names it, and list the removal.
- **Flag claims, don't fix them.** If a claim is wrong or overstated, for example "prove" when the paper has no theorem, say so after the Markdown and leave the text as written. Don't suggest changes of tone or emphasis.

When writing new text, such as a lead line or a summary from notes:

- **Don't make the reply defensive.** Don't add sentences that concede gaps or limitations the author didn't raise.
- **Keep prose short and plain.** State the finding in the text and leave the numbers to the table.

Before returning a reply:

- **Deliver Markdown in a fenced code block**, so it can be copied as source.
- **Don't split a reply** into several comments to fit a length limit unless the author asks. Limits differ by venue.
- Run `python3 check_openreview_md.py reply.md` to see its length and any rendering problems.
- When converting a draft, run `python3 check_openreview_md.py --diff draft.md reply.md`. It confirms that only the intended words changed.

## Seeing the Markdown behind someone else's reply

- **Edited comments:** click **Revisions** under the comment, tick two versions and click **View Differences**. The raw text appears, with its line breaks run together.
- **Any comment:** copy the **Revisions** link. Its `id=` value is the comment's note ID. On an openreview.net page, run this in the browser console:

  ```js
  fetch('https://api2.openreview.net/notes?id=<note-id>', {credentials: 'include'})
    .then(r => r.json()).then(d => console.log(d.notes[0].content.comment.value))
  ```
