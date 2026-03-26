---
name: render-text-table
description: Render compact terminal-style text tables for model outputs. Use when information clearly fits a two-dimensional structure and should be presented as a readable table, especially for Unicode box-drawing borders, mixed Chinese/English width alignment, narrow terminal-friendly layouts, truncation or multi-line cells, and situations where Markdown tables should be avoided by default. 适用于需要输出紧凑文本表格、终端盒线表格、中英文混排列宽对齐、并默认避免 Markdown 表格的场景。
---

# Render Text Table

Render table-shaped information only when a table genuinely improves readability.

## Decide Whether To Use A Table

- Check whether the information naturally forms a two-dimensional structure with stable columns and rows.
- Prefer bullets, numbered lists, or key-value blocks when the content is mostly narrative, explanatory, procedural, or hierarchical.
- Avoid tables by default for long remarks, error messages, file paths, URLs, code, JSON, logs, or dense prose.
- Avoid tables when the content would become too wide, too sparse, or harder to scan than a list.
- Prefer a summary table plus a detail list when only a few short fields fit well in columns.

## Default Output Rules

- Do not use Markdown tables unless the user explicitly asks for them.
- Wrap every rendered table in a fenced `text` code block.
- Use Unicode box-drawing characters: `┌ ┬ ┐ ├ ┼ ┤ └ ┴ ┘ │ ─`.
- Use spaces only. Never use tabs.
- Keep the table narrow enough for the default chat area. Aim for 80 half-width columns or less when possible.
- Reduce column count, shorten headers, wrap content, or truncate with `…` before allowing the table to become too wide.

## Width And Alignment Rules

- Compute alignment by display width, not by character count.
- Count Chinese characters, full-width punctuation, and other full-width glyphs as width `2`.
- Count English letters, digits, ASCII punctuation, and ordinary spaces as width `1`.
- Pad the right side of each cell with spaces until it reaches the maximum display width of that column.
- Keep every physical line in the table exactly the same total width.
- Close the left and right borders on every line.

## Long Content Rules

- Prefer short column names.
- Truncate overly long single-line content with `…` when that preserves the meaning.
- Use multi-line cells when a short manual wrap keeps the table readable.
- Keep multi-line rows fully boxed: each visual line must preserve all vertical borders.
- Do not dump full paths, URLs, code snippets, or long error strings into wide columns unless the table remains compact.
- Fall back to a summary table plus numbered details when several fields are too long to fit safely.

## Rendering Workflow

1. Decide whether a table is truly the best structure.
2. Keep only the fields that belong in short columns.
3. Shorten headers and values before rendering.
4. Compute each column's maximum display width.
5. Build borders and rows with box-drawing characters.
6. Right-pad each cell to the column width.
7. Re-check that every rendered line has the same total width.
8. If widths do not align reliably, reduce columns, wrap cells, truncate content, or switch to a non-table format.

## Preferred Fallbacks

- Use bullets for descriptive comparisons with uneven detail.
- Use numbered items for options followed by explanations.
- Use key-value blocks for a small set of attributes.
- Use a summary table plus details when only identifiers, status, counts, or short labels fit cleanly.

## Output Checklist

- Confirm the content is actually two-dimensional.
- Confirm the table is in a `text` code block.
- Confirm no Markdown table syntax is used unless explicitly requested.
- Confirm all columns align by display width.
- Confirm all rows have the same total width.
- Confirm the table remains compact and readable in the default chat area.
- Confirm long content was wrapped, truncated, or moved out of the table.

## Example Pattern

```text
┌────┬────────┬──────┐
│ ID │ 名称   │ 状态 │
├────┼────────┼──────┤
│ 1  │ 门户   │ 正常 │
│ 2  │ API    │ 延迟 │
└────┴────────┴──────┘
```

Use this pattern only when the result stays compact and clearly readable.
