# IBD Client Contact Repository — Power Apps Build Guide Design

**Date:** 2026-07-12
**Source spec:** `IBD_Client_Contact_Repository_Specification_v3.pdf` (Sponsor: Hussain Al-Haddad, Head of Credit Assessment; Owner: BMO)
**Deliverable:** A standalone HTML step-by-step guide (in this `IBD_html` project) for building the app from scratch in Power Apps Studio.

## Goal

A **view-only** Power Apps canvas app over four existing SharePoint lists on
`bbkonline.sharepoint.com/sites/BMO`, letting IBD staff search a borrower by
RIM or name and see that borrower's contacts grouped by communication purpose.
No data entry in the app; no Power Automate flows. The Master List of
Exposures remains the single source of truth for borrower data — the app never
duplicates or writes borrower information.

## Decisions made during brainstorming

| Decision | Choice |
|---|---|
| App scope | View-only (search + display exactly per spec §8); data maintained directly in SharePoint |
| Starting point | Fresh canvas app from scratch; `BMO (1).msapp` used only as a layout reference (its container pattern), not as a base |
| SharePoint lists | All four already exist; guide does not create them |
| Power Automate | None (spec describes no flows; add later only if a need appears) |
| Schema source | Spec PDF names used as placeholders; guide opens with a mandatory verify-your-columns step. `BMO (1).msapp` was inspected and does **not** contain the IBD lists' schemas (it holds five unrelated BMO tracker lists). Formulas get trued-up if the user later exports an .msapp with the four lists connected. |
| App structure | Single-screen master-detail (search panel left, borrower detail + purpose-grouped contacts right) |
| Guide format | Single standalone HTML page in `IBD_html` |

## Data layer

Four existing SharePoint lists, all read-only from the app:

1. **Master List of Exposures** — borrower master. App reads: RIM, Borrower
   Name (Title assumed), plus display fields for the borrower header.
2. **Borrower Contact Directory** — Title (contact full name), Contact Email,
   Mobile Number, Office Number, Job Title, Active Status, Preferred
   Language, Notes. Contains no borrower information.
3. **Communication Purpose Master** — the six purposes (Audit Confirmations,
   Daily Transaction Advices, Legal Notices, Facility Documentation, Covenant
   Reporting, Business Matters). The app's grouping is driven by this list,
   not hard-coded, so new purposes (and future Wholesale Banking units, spec
   §9) appear without redesign.
4. **Contact Communication Matrix** — junction list: Title, Borrower RIM
   (lookup), Borrower Name (lookup), Contact (lookup), Purpose, Notes.

**Delegation strategy:** borrower search uses delegable `StartsWith` filters
against the Master List so it works past 500/2000 rows. On borrower
selection, the app `ClearCollect`s only that borrower's matrix rows into
`colMatrix`; all grouping and contact-field access after that is local.
Nested galleries never query SharePoint directly.

## Screen layout

One screen, auto-layout containers (same pattern as the BMO shell):

- **Header bar** — app title + BBK branding.
- **Left panel (~1/3)** — search text input (`txtSearch`) over a borrower
  results gallery (`galBorrowers`). Selection drives the right panel.
- **Right panel (~2/3)** — borrower info card (name, RIM, display fields),
  then a vertical purpose gallery (`galPurposes`) with a nested contacts
  gallery (`galContacts`) per purpose showing name, job title, email,
  mobile/office numbers.

Inactive contacts (`Active Status = No`) are excluded; the guide flags the
single formula to change if the client prefers them shown greyed-out.
Purposes with no contacts for the selected borrower are hidden.

## Key formulas (placeholders per spec names; verified in guide step 2)

- **Search:** `Filter('Master List of Exposures', StartsWith(Title, txtSearch.Text) || StartsWith('Borrower RIM', txtSearch.Text))`
  (guide includes the numeric-RIM variant)
- **On select:** `ClearCollect(colMatrix, Filter('Contact Communication Matrix', 'Borrower RIM'.Id = galBorrowers.Selected.ID))`
- **Purpose gallery items:** `Filter('Communication Purpose Master', CountRows(Filter(colMatrix, Purpose.Value = Title)) > 0)`
- **Contacts gallery items:** matching `colMatrix` rows, contact fields via the
  Contact lookup, filtered to `Active Status = Yes`.

The Matrix's Purpose column may be a Choice or a Lookup to Communication
Purpose Master — the spec doesn't say. The verify-your-columns step
determines which, and the guide provides both formula variants.

## Edge cases

- No borrower selected → "Search for a borrower" placeholder in right panel.
- No search results → "No borrowers found" label.
- Borrower with zero matrix rows → "No contacts recorded for this borrower."
- Schema drift → guide step 2 is a verify-your-columns checklist table of
  every list/column the formulas reference (display vs internal name noted).
- Slow load → contacts area shows a loading state around the `ClearCollect`.

## HTML guide structure

Single self-contained HTML page, sections:

1. Overview + architecture diagram (four lists → app)
2. Prerequisites + verify-your-columns checklist
3. Create the app (tablet-format canvas app, naming, settings)
4. Connect data (add the four SharePoint lists)
5. Build the layout (containers, step by step)
6. Search panel (controls + formulas)
7. Borrower info card
8. Contacts grouped by purpose (nested galleries, small verified steps)
9. Polish (theme, empty states, icons)
10. Test checklist (pass/fail behavior table)
11. Publish & share (view-only sharing)

Each build step gives: where to click, the control name to use (so formulas
match), the exact formula to paste, and a "you should now see…" checkpoint.

## Out of scope

- Any write operations from the app (add/edit contacts or matrix rows)
- Power Automate flows
- Creating or modifying the SharePoint lists
- Changes to the Master List of Exposures
