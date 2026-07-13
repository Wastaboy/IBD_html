# Master List of Exposures — confirmed columns

**Architecture decision (2026-07-13):** the build guide was rewritten around this
single list. The spec's four-list design (Borrower Contact Directory, Communication
Purpose Master, Contact Communication Matrix) has no supporting evidence on the real
site — the contact data lives in this list's columns. The four-list guide is
preserved at git commit `ec7c30f`.

**Types confirmed 2026-07-13** from the connector schema embedded in the user's own
`IBD Client Contact Repository.msapp` (`References/DataSources.json`,
`DataEntityMetadataJson`) — a working app connected to the live list. Names were
first confirmed from the client's `schema.csv` export. Nothing is assumed anymore.

| Display name | Internal name | Type | In Power Apps |
|---|---|---|---|
| Borrower | `Title` (renamed) | Text | `Borrower` |
| RIM | `field_2` | **Number** | `RIM = Value(txtSearch.Text)` |
| Office | `field_1` | Text | `Office` |
| BMO RSM 1 / BMO RSM 2 | `BMORSM1` / `BMORSM2` | **Lookup** | `.'BMO RSM 1'.Value` |
| BMO RSM 1/2: Email Address | expansion | Lookup-shaped | `.'BMO RSM 1: Email Address'.Value` |
| RM | `field_3` | Text | `RM` |
| Rim Officer Name | `RimOfficerName` | **Person** | `.DisplayName`, `.Email` |
| Office (list) | `Office_x0020_…` | Lookup | `.'Office (list)'.Value` |
| Office (list):1st/2nd Email Address | expansions | Lookup-shaped | `….Value` |
| office email | `office_x0020_email` | Text | `.'office email'` |
| Total Limit / Current Exposure / Overall Utilization % / Grade | `field_4/5/6/16` | Number | `Text(..., "#,##0")` |
| Facility Type / Status / Trade Facilities / Repayment Method / DCO / Pricing | `field_8/7/9/10/11/15` | Text | as-is |
| Group Name (if applicable) | `GroupName…` | Text | as-is |
| Active/Inactive Status · Syndicated or Bilateral · Term or WC | choice cols | **Choice** | `.Value` |
| Original Review Date · Mid-Year Rev / QCM Date · Final Maturity | `field_12/13/14` | Date | as-is |

Note: `Pricing` (`field_15`) is non-filterable/non-sortable per the connector
capabilities — don't use it in a `Filter()`.

## App build status (user's .msapp, 2026-07-13)

Built through S4b correctly (containers, one connection, search formula — the
correct number variant). Found and diagnosed: row-template labels inserted outside
the gallery (the S4c trap, now a warn note in the guide), conBody flexible height
off, containers set to Align Center instead of Stretch. Resumes at S5.
