# Update №1 — borrower-by-name in the Directory + app UI polish

Date: 2026-07-22 · Status: approved (user: "push when ready")

## Request

1. The client's users enter contacts in the **Borrower Contact Directory** SharePoint
   list and want to pick the borrower **by name** and have the **RIM derived
   automatically** — in the list itself, not in the Power Apps UI. Confirmed with the
   user: the live Directory has **no borrower/RIM column today** (matches the guide's
   L1 design; contacts link to borrowers only through the Matrix).
2. "Also fix the font and layout of the UI a little bit" — confirmed: the **Power
   Apps app**, full pass at my judgment, kept small.

## Design

### Part A — SharePoint (no app change, nothing to publish)

Add a **Lookup** column named `Borrower` to Borrower Contact Directory:
source = Master List of Exposures, display column = **Borrower** (renamed Title),
additional field = **RIM** (Number — supported as a projected field). SharePoint
renders two columns: `Borrower` (searchable type-ahead picker) and `Borrower: RIM`
(read-only, auto-filled). Same mechanism as the Matrix's L3 lookup, inverted —
name is the display column, RIM rides along.

Rejected alternatives: Power Automate flow filling a text RIM column (moving
parts, sync lag); calculated column (cannot reference other lists).

Steps: D1 add column · D2 backfill via grid view · D3 reorder view columns ·
D4 verify (+ New → type-ahead by name → RIM auto-fills).

Notes: multi-borrower contacts → "Allow multiple selection" on the column;
"require a value" toggle only after backfill; the app never reads this column.

### Part B — app polish (properties only; no new controls, no formula edits)

- **U1** conLeft/conRight: `DropShadow.Light` + corner radius 12 — panels become cards.
- **U2** conBody padding & gap 12→16; conHeader PaddingLeft 16→28 (title aligns
  with panel content); conBorrowerCard radius 8→12.
- **U3** lblAppTitle Size 18→20, Width 450→560.
- **U4** lblCoverageHeader / lblOfficeHeader → UPPERCASE text, Size 15→12
  (overline style). **Not** lblPurposeHeader — its text is data and
  `galContacts.Items` compares `Purpose.Value = lblPurposeHeader.Text`
  (case-sensitive); uppercasing it would empty every group.
- **U5** multi-select all labels + txtSearch → `Font.'Segoe UI'` explicitly
  (kills any quiet Open Sans/Segoe mix; `Font.Lato` offered as alternative).
- **U6** Save + Publish (Part B changes the app; Part A needs no publish).

Geometry-affecting size bumps (row labels 12–15) deliberately excluded — they
would cascade into the fixed-anchor row math (TemplateSize, Y offsets).

## Deliverable

`ibd-update-1.html` — mini guide page in the standard design (html_hustle
template, tap-to-copy chips, ▣ element chips, progress bar, localStorage key
`ibdUpdate1_v1`). Cross-links appended to the footers of the build guide,
fix v2, fix v3, and final-build pages (footers only, per convention;
build-review-1 keeps its minimal "newer:" chain).
