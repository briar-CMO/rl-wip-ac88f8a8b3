# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file, client-facing weekly WIP (work-in-progress) report for RevitaLash Cosmetics — covering the RevitaLash NZ store and the Aus Beauty Club (ABC) AU store. The entire deliverable is `index.html`: a self-contained static page with all CSS, JavaScript, and SVG charts inline. There is no build system, package manager, test suite, or linter.

The report is **rolled forward in place each week** (typically Wednesdays, NZ time): the same `index.html` is edited with the new week's numbers, the `<title>`, header date pill, and footer build-note are updated, and the change is committed with a message summarising the data (e.g. "Weekly WIP roll to July: week 1-7 Jul (NZ $13,819 net, 6.0x MER; ABC A$6,624)"). Do not create per-week copies of the file.

## Viewing / verifying changes

Open `index.html` directly in a browser, or serve it:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

There is one external dependency: Google Fonts (`@import` at the top of the inline stylesheet). Everything else must remain inline — no external scripts, stylesheets, or images.

## Page structure

`index.html` is one long line-per-rule/minified-ish document, in this order:

1. **Inline `<style>`** — design tokens live in `:root` (dark rose-on-charcoal palette: `--bg`, `--panel`, `--blue` which is actually the brand rose `#D9A7AD`, plus semantic `--pos`/`--amber`/`--neg`). Print styles at the end show all tabs and force page-break avoidance.
2. **Header** (`.hdr`) — brand bar, `<h1>`, subhead, and the week-ending date pill.
3. **Sticky tab bar** — buttons with `data-tab` attributes.
4. **Five `.tab-pane` sections**, ids: `overview`, `creative` (Creative Flywheel), `google`, `klaviyo`, `abc` (Aus Beauty Club).
5. **Footer** (`.footer-note`) — build timestamp, week range, and data-source provenance line.
6. **Inline `<script>`** — tab switching with `location.hash` sync, plus a count-up animation for elements with class `countup`. Progressive enhancement: the script adds `js-tabs` to `<body>`; without JS (and in print) all panes render stacked and the tab bar hides.

## Charts

All charts are **hand-authored inline SVG with hard-coded coordinates** (the NZ pace bar, the Klaviyo daily email-revenue line with campaign-send markers, the ABC cumulative-revenue-vs-target line). Updating a chart's data means recomputing point/rect coordinates from the viewBox scale — check the axis gridline values in the SVG to derive the y-scale before editing. Keep labels, pace/target lines, and legends consistent with the numbers shown in the surrounding KPI cards.

## Data conventions

- **NZ figures are NZD ex-GST; ABC figures are AUD** (prefix `A$`). Keep the currency explicit in labels.
- **Blended MER (net sales ÷ all paid spend) is the honest topline.** In-platform ROAS (Meta, Google, Klaviyo) double-counts across channels — the page says this explicitly and comparisons must preserve that framing.
- NZ store revenue and net-new-customer counts are **store-wide Shopify figures entered manually** (read-only Shopify API access is pending); other data comes from the Meta Marketing API, Google Ads API, ABC Shopify Admin API, Klaviyo MCP, and Motion MCP — provenance is listed in the footer and should stay accurate.
- The "Top creative by spend" table applies a **$200 ad-spend floor**.
- ABC runs **no Meta ads** (not permitted in AU) — Google + email + organic only, so ABC MER = net sales ÷ Google spend.
- Ad fatigue framing: judge creative on lifetime spend + ROAS, not last-7-days; a dipping high-lifetime ad is fatigue (refresh the angle), not a dead idea.
- Semantic colour classes: `pos` (green), `amber`, `neg` (red) on values and cards; use them consistently with whether a number is ahead/behind pace.

## Compliance rules for copy (critical)

Marketing claims in this report (and any creative ideas it proposes) are constrained:

- The **current RevitaLash serums contain a prostaglandin analogue**. Never state or imply the existing range is "prostaglandin-free" / "PGA-free". That claim is reserved exclusively for the **November Peptide Complex Plus** reformulation.
- Claims must stay **cosmetic**: "appearance of" fuller/longer lashes — never "grows" lashes or hair.
- "Guaranteed results" copy must lean on the trial/clinical backing.
- Never imply RevitaLash treats medical lash/hair loss.

A commit was previously made specifically to fix a prostaglandin claim — treat these rules as hard constraints when editing any content on the page.

## Git

- The page carries `<meta name="robots" content="noindex,nofollow">` — it is client-facing but not meant to be indexed; keep that tag.
- Commit messages follow the existing style: a short data-first summary of what changed in the report (numbers included), not a description of HTML edits.
