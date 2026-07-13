# Master List of Exposures — confirmed columns

Source: client export `schema.csv` (header row only, no data rows), received 2026-07-13.
These are SharePoint **display names**. Column *types* are inferred where the export
gives evidence; blank means unknown.

| Column | Type evidence |
|---|---|
| Borrower | Text (borrower name — may be the built-in Title column renamed) |
| RIM | unknown — text vs number still unverified (guide keeps both formula variants) |
| Office | |
| BMO RSM 1 | Person/lookup (export has `BMO RSM 1: Email Address` expansion) |
| Office (list) | Lookup (export has `:1st Email Address` and `:2nd Email Address` expansions) |
| RM | |
| Total Limit | likely Number/Currency |
| Facility Type | |
| Rim Officer Name | |
| Active/Inactive Status | |
| Current Exposure | likely Number/Currency |
| Overall Utilization % | likely Number |
| Status | |
| Syndicated or Bilateral | |
| Term or WC | |
| Group Name (if applicable) | |
| Trade Facilities | |
| Repayment Method | |
| DCO | |
| Original Review Date | likely Date |
| Mid-Year Rev / QCM Date | likely Date |
| Final Maturity | likely Date |
| Grade | |
| office email | |
| BMO RSM 2 | Person/lookup (export has `BMO RSM 2: Email Address` expansion) |
| ID | SharePoint item ID |

Expansion columns in the export (not real list columns): `BMO RSM 1: Email Address`,
`Office (list):1st Email Address`, `Office (list):2nd Email Address`,
`BMO RSM 2: Email Address`.

## Still unconfirmed (spec-placeholder names in the guide)

- **Borrower Contact Directory** — all columns, incl. `Active Status` type (yes/no vs choice)
- **Communication Purpose Master** — all columns
- **Contact Communication Matrix** — all columns, incl. whether `Borrower RIM` is a lookup or plain text
