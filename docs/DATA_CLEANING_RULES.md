# Data Cleaning Rules

## Rule 1 — Mobile Number Normalization

**Goal:** Standardize all mobile numbers to clean 10-digit Indian format.

**Priority:** Use `Mobile final` column if available; fall back to `Mobile`.

**Transformations applied:**
- Strip all non-digit characters (spaces, dashes, brackets, `p:+` prefixes)
- Remove leading country code `91` if total digits > 10
- If still > 10 digits, take last 10
- Keep original value if fewer than 8 digits remain (likely corrupt)

**Examples:**
| Before | After |
|--------|-------|
| `p:+916363501035` | `6363501035` |
| `9.20E+11` | `9820848308` |
| `9999,358,338` | `9999358338` |
| `1571375-9597` | `71375-9597` *(kept as-is — suspicious)* |
| `17817424870` | `7817424870` |

---

## Rule 2 — Years of Experience Column Merge

**Goal:** Combine two YoE columns into one without losing information.

**Source columns:**
- `Years of Experience` — numeric values (e.g., `10`, `15`, `6`)
- `Years of Experience -1` — free-text values (e.g., `"5 to 10 years"`, `"15+"`, `"10 to 15 Years"`)

**Priority logic:**
1. If numeric column has a value → use it
2. Else if text column has a genuine YoE value → parse and use it
3. Skip non-YoE values in text column (revenue/income like `"30 to 50 lakhs"`, `"AED 440K"`)

**Invalid values discarded:**
`Select`, `Event Guest`, `30 to 50 lakhs`, `AED 440K and above`, `₹30 - 50 lacs`, `> ₹1 crore`

---

## Rule 3 — YoE Enrichment (Raw → Cleaned)

**Goal:** Where cleaned data has plain numbers, replace with richer descriptive text from raw source if available.

**Join key:** Email address

**Replace condition:** Raw has a value matching pattern: `year`, `yr`, `above`, `to`, `+`, `-`

**Keep condition:** Raw value is blank, numeric-only, or income/revenue related

**Examples:**
| Cleaned (before) | Raw source | Cleaned (after) |
|-----------------|------------|-----------------|
| `5.0` | `5 to 10 years` | `5 to 10 years` ✓ |
| `15.0` | `15 years and above` | `15 years and above` ✓ |
| `10.0` | *(blank)* | `10.0` *(unchanged)* |
| `6.0` | `30 to 50 lakhs` | `6.0` *(kept — income, not YoE)* |

---

## Rule 4 — First Name / Last Name Extraction

**Goal:** Fill missing First/Last Name by splitting Full Name.

**Trigger:** Row has blank `First Name`

**Source priority:** `Full Name` → `Last Name` (as fallback)

**Split logic:**
- 2+ word name → First = word[0], Last = remaining words
- Single word name → **Last Name only** (First Name left blank)

**Why single name → Last Name?**
Zoho CRM marks `Last Name` as a **mandatory field**. Putting a single name in First Name would cause the entire import row to be rejected.

**Examples:**
| Full Name | First Name | Last Name |
|-----------|-----------|-----------|
| `Shikha Bhatia` | `Shikha` | `Bhatia` |
| `Aabha Bakaya` | `Aabha` | `Bakaya` |
| `Simran` | *(blank)* | `Simran` |
| `Dr. Priya Sharma` | `Dr.` | `Priya Sharma` |

---

## Rule 5 — General String Cleaning

Applied to all text columns:
- Strip leading/trailing whitespace
- Replace `"nan"`, `"NaT"`, `"None"`, `"."` strings with empty cell
- Preserve all original values — **no records deleted**
