# Expense Dashboard

A one-page dashboard that answers a single question: **where is our money going, and is anything moving in the wrong direction?**

> แดชบอร์ดค่าใช้จ่ายหน้าเดียว ตอบคำถามว่า "เงินไปไหน และมีอะไรไปในทิศทางที่แย่หรือไม่" (หน้าเว็บเป็นภาษาไทยผสมอังกฤษ)

<!-- Live demo: add the Vercel URL here after the first deploy -->

**All figures in this repository are fictional demo data.** The vendors and amounts were invented for this demo. They are not real company figures.

## What it shows

From top to bottom, in order of importance:

1. **Verdict strip.** Total spend for the latest month, how that changed versus the month before (with a plain word: spend rose / fell), and the single fastest-growing category.
2. **Monthly trend.** Total spend as a line, with the fastest-growing category overlaid as a thicker line.
3. **Category breakdown.** Horizontal bars, largest first. Categories growing unusually fast are marked.
4. **Expense table.** Every row, filterable by month and category, sortable by amount.

One accent colour (orange) is reserved for "moving in the wrong direction". Everything else is neutral.

### It tells you when not to trust a number

A dashboard should never make you more confident than the data deserves. When something could distort a figure, the page says so, calmly, next to the numbers:

- A **\*** appears on the month-over-month change (and the fastest-growing category, when affected). A "spend fell ✓" badge loses its tick when a caveat applies.
- A grey **"Things to know about these numbers"** box lists the reasons: rows with a missing amount (not counted), possible duplicate charges, dates that could be read two ways, a latest month that may be incomplete, or a month with unusually few rows.
- Months with caveats get a small ring on the trend line, and suspicious rows in the table carry a small tag.
- If nothing is found, it says so: "checked, nothing to flag". That does not guarantee the source data is correct.

The demo data contains a missing amount (March) and a possible double charge (April), and its latest month stops on 25 June, so you can see these notes in action. The box is titled "ข้อควรรู้เกี่ยวกับตัวเลขนี้" on the page.

## Privacy

The page is a single static HTML file. It runs entirely in your browser. There is no server, no upload and no tracking. When you drop your own CSV on it, the file is read locally and never leaves your machine.

## Try it with your own data

Drag a CSV onto the page (or use the **เปลี่ยนไฟล์ CSV…** / "Change CSV" button). Columns needed: `Date`, `Vendor`, `Category`, `Amount` (and optionally `Notes`). See [`demo_expenses.csv`](demo_expenses.csv) for the format.

The reader is forgiving: several date formats (`2026-01-31`, `31/01/2026`, `1/31/26`, `Jan 31, 2026`), amounts like `$ 1,200.00` or `2,400.00 USD`, repeated rows, and a stray "TOTAL" row. Rows it cannot trust are left out and listed, not silently guessed. Currencies are not converted: if a file mixes currencies, only the main one is counted and the page says so.

## How the data gets refreshed (for the maintainer)

The demo data is embedded in `index.html`. To update it:

1. Open the dashboard and drop in the new CSV. It should contain **all months** (old and new), so the trend and the month-over-month comparison work.
2. Check the line under the title ("Data through …") shows the new month, and the grey line under the filters shows how many rows were counted.
3. Press **บันทึก dashboard พร้อมข้อมูลนี้** ("Save dashboard with this data"). The browser downloads a new HTML file with the data embedded.
4. Rename it to `index.html`, replace the one in this repository, commit and push. If the repository is connected to Vercel, pushing triggers a new deployment.

**Careful:** the saved file contains the whole CSV. Whatever you save and publish becomes public. Only publish data that is meant to be public. Dropping a CSV onto the page without saving changes nothing for other visitors.

## How it was built

This was built by directing **Claude Code** (Anthropic's coding assistant). The author does not write code: they described what they wanted in plain language, approved a written plan before any code was written, and reviewed the results. Claude Code wrote and tested the code.

Some of the checks along the way:

- Monthly totals were compared against a separate calculation done in Python, on both a clean file and a deliberately messy one (mixed date formats, duplicate rows, a missing amount, a junk "TOTAL" row). They matched.
- The design was reworked in rounds ("make the good/bad reading unmistakable", "make the two lines clearly different") and then audited for honesty: what a viewer could be misled by, and how to show it.

Known limits: the duplicate check is a heuristic (same vendor, near-identical amount, within 7 days) and can flag genuine repeat subscriptions; it flags rather than removes. The "incomplete month" check only looks at the latest and earliest month. Rows are only as good as the file you give it.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole dashboard (HTML, CSS and JavaScript in one file, with the demo data embedded) |
| `demo_expenses.csv` | The same fictional demo data, as a file you can drop onto the page |
| `README.md` | This file |
| `LICENSE` | MIT license |

No build step, no dependencies, no external requests.

## License

[MIT](LICENSE).
