# Column Mapping Reference

## Raw Data (59 columns) → Zoho CRM Lead Fields (22 columns)

| # | Zoho Field | Raw Source Column | Notes |
|---|-----------|-------------------|-------|
| 1 | First Name | `First Name` | Extracted from Full Name if blank |
| 2 | Last Name | `Last Name` | Extracted from Full Name; single names go here |
| 3 | Full Name | `Contact Name` | Direct map |
| 4 | Email | `Email` | Direct map |
| 5 | Mobile | `Mobile final` → `Mobile` | Cleaned to 10-digit format |
| 6 | Company Name | `CompanyName` | Direct map |
| 7 | Industry | `Industry` | Direct map |
| 8 | Designation | `Designation` | Direct map |
| 9 | Website | — | Left blank (no source) |
| 10 | Years of Experience | `Years of Experience` + `Years of Experience -1` | Merged two columns into one |
| 11 | Tier | `Tier` | Direct map |
| 12 | Lead Type | `Tag` | Renamed |
| 13 | City | `City` | Direct map |
| 14 | Lead Owner | `Contact Owner` | Renamed |
| 15 | Lead Source | `Lead Source` | Direct map |
| 16 | Lead Status | `Membership Status` | Renamed |
| 17 | Created By | `Created By` | Direct map |
| 18 | Modified By | `Modified By` | Direct map |
| 19 | Lead Quality | — | Left blank (no source) |
| 20 | Remarks | `Description` | Renamed |
| 21 | LinkedIn Profile | `LinkedIn Profile` | Direct map |
| 22 | Instagram Profile | `Instagram Profile` | Direct map |

## Dropped Columns (not needed for Zoho import)

These 37 columns were present in raw data but not required for Zoho Lead import:

`Contact Id`, `Sales POC`, `Contact Owner.id`, `Email Opt Out`, `Salutation`,
`Date of Birth`, `RoleType`, `Onboarding Date`, `Membership Amount`,
`Tenure of Membership`, `Payment Date`, `Annual Income`, `Payment Method`,
`Created By.id`, `Created By`, `Modified By.id`, `Created Time`, `Modified Time`,
`Last Activity Time`, `Unsubscribed Mode`, `Unsubscribed Time`, `Renewal Amount`,
`Renewal Date`, `Referred By.id`, `Referred By`, `Total Amount`,
`Mentorship Taken`, `Member Collaborations`, `Bonds Group`, `Business Type`,
`Single/ Married/Single Parent`, `Speaker/ Hosted a Workshop/ Moderated an Event`,
`Meal Preference`, `Number of Kids`, `Upgrade`, `Address Line 1`,
`Area They Live in`, `Reassignment`
