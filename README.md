# 📊 LWL Community Member Data Pipeline — Freelance Project

> **NDA Notice:** All data shown in this repository is fully synthetic/anonymized.  
> Real client data is protected under a Non-Disclosure Agreement and is not included.

---

## 🧾 Project Overview

This was my **first freelance data engineering project** — a complete end-to-end data pipeline built for a client managing the **LWL (Ladies Who Lead) Community**, a large women entrepreneur network across India with 25,000+ members.

The project involved cleaning, merging, categorizing, and preparing member data from multiple raw sources for:
1. **Zoho CRM import** (lead management)
2. **Website display** (public-facing member directory)
3. **Internal master database** management

---

## 🎯 The Problem

The client had member data scattered across:
- Multiple CSV/Excel exports from Zoho CRM
- Manual entry sheets with inconsistent formats
- Duplicate records across sources
- Conflicting status values between datasets
- No single clean master database

**Key pain points:**
- 59-column raw CRM exports needed to map to 22 Zoho Lead fields
- Mobile numbers in 10+ different formats
- Two separate "Years of Experience" columns (numeric + free-text)
- Records missing First/Last Name — blocked Zoho import (Last Name mandatory)
- Member status conflicts between master DB and CRM exports
- No colour-coded categorization for visual tracking

---

## ✅ Full Scope of Work

### Phase 1 — Data Cleaning & Normalization
| Task | Details |
|------|---------|
| Column mapping | 59 raw cols → 22 Zoho Lead fields |
| Mobile normalization | 13,907 rows fixed (scientific notation, +91, dashes, commas) |
| YoE column merge | Combined numeric + free-text into single column |
| Name extraction | First/Last split from Full Name for 3,784 rows |
| Zoho Last Name fix | 1,974 single-name records → Last Name only |

### Phase 2 — Master Database Build
| Task | Details |
|------|---------|
| Multi-source merge | Combined members across multiple CSV files (`merge_master.py`) |
| Deduplication | Identified and resolved duplicate records |
| Header detection | Auto-detected headers across inconsistent file formats (`find_headers.py`) |
| Diagnosis | Flagged anomalies and data quality issues (`diagnose.py`) |
| Cleanup | Normalized strings, whitespace, null values (`cleanup.py`) |

### Phase 3 — Status Management
| Task | Details |
|------|---------|
| Status validation | Validated membership status across sources (`check_status.py`) |
| Mismatch detection | Found records where status conflicted between files |
| Conflict resolution | Built logic to resolve status conflicts (`find_conflicts.py`, `find_conflicts2.py`) |
| Status fix | Applied corrections to produce clean final file (`fix_status.py`) |
| Edge cases | Flagged non-active records still in master (`nonactive_but_active_in_master.csv`) |

### Phase 4 — Categorization & Website Export
| Task | Details |
|------|---------|
| Category tagging | Added membership tier/category columns (`add_categories.py`) |
| Website data prep | Cleaned subset for public member directory (`members for website.csv`) |
| Final CSV export | Clean export pipelines (`export_csv.py`, `export_final.py`) |

### Phase 5 — Colour Coding & Visual Reporting
| Task | Details |
|------|---------|
| Colour-coded view | Built HTML colour-coded member status report (`Colour.html`) |
| Legend | Visual colour guide for status categories (`color_coding_legend.html`) |

---

## 📁 Repository Structure

```
lwl-member-data-pipeline/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── scripts/
│   ├── merge_master.py          ← Multi-source data merge
│   ├── find_headers.py          ← Auto-detect headers across files
│   ├── diagnose.py              ← Data quality diagnostics
│   ├── cleanup.py               ← String/format normalization
│   ├── add_categories.py        ← Category & tier tagging
│   ├── check_status.py          ← Membership status validation
│   ├── find_conflicts.py        ← Cross-source conflict detection
│   ├── find_conflicts2.py       ← Secondary conflict pass
│   ├── fix_status.py            ← Status correction logic
│   ├── export_csv.py            ← CSV export
│   └── export_final.py          ← Final clean export
│
├── zoho_pipeline/
│   └── pipeline.py              ← Full Zoho 59→22 col mapping + all cleaning
│
├── sample_data/
│   ├── sample_raw_input.csv     ← Synthetic 10-row raw sample
│   └── sample_cleaned_output.csv
│
└── docs/
    ├── COLUMN_MAPPING.md        ← 59→22 Zoho field reference
    ├── DATA_CLEANING_RULES.md   ← All cleaning rules with examples
    ├── STATUS_LOGIC.md          ← Member status resolution rules
    └── LINKEDIN_POST.md         ← LinkedIn post draft
```

---

## 🔄 Full Pipeline Flow

```
Multiple Raw Sources
(CRM exports, manual sheets, master DB)
              │
              ▼
┌──────────────────────────────┐
│  find_headers.py             │  Auto-detect headers
│  diagnose.py                 │  Flag data quality issues
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  merge_master.py             │  Combine all sources
│  cleanup.py                  │  Normalize formats
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Mobile normalization        │  → 10-digit Indian format
│  YoE merge                   │  → numeric + text → one col
│  Name splitting              │  → First + Last from Full Name
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  check_status.py             │  Validate membership status
│  find_conflicts.py           │  Detect cross-source conflicts
│  fix_status.py               │  Resolve & apply corrections
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  add_categories.py           │  Tag tiers & categories
│  export_csv.py / final       │  Generate outputs
└──────────────┬───────────────┘
               │
        ┌──────┴──────┐
        ▼             ▼
  Zoho CRM        Website
  Import          Directory
  (22 cols)       (filtered
  25,315 rows      subset)
```

---

## 💡 Key Technical Challenges & Solutions

### 1. Mobile Number Chaos
Numbers arrived as `+919876543210`, `9.20E+11`, `9999,358,338`, `p:+916363501035`

```python
def clean_mobile(val):
    digits = re.sub(r'\D', '', str(val))
    if len(digits) > 10 and digits.startswith('91'):
        digits = digits[2:]       # strip country code
    if len(digits) > 10:
        digits = digits[-10:]     # take last 10 digits
    return digits if len(digits) >= 8 else val  # keep original if suspicious
```

---

### 2. Status Conflicts Across Sources
Same member appeared as `Active` in one file and `Non-renewed` in another.  
Built a priority-based resolution system using `find_conflicts.py` + `fix_status.py`.

---

### 3. Zoho's Mandatory Last Name Field
~1,974 members had only a single name. Zoho rejects rows without Last Name.

```python
if len(name_parts) == 1:
    row['Last Name']  = name_parts[0]  # single name → Last Name
    row['First Name'] = None           # First Name left blank ✓
```

---

### 4. YoE Enrichment via Email Join
Replaced plain numbers (`10`) with richer text (`"10 to 15 years"`) from raw source,  
joined by email — while skipping income values (`"30 to 50 lakhs"`, `"AED 440K"`).

---

### 5. Colour-Coded Status Report
Built an HTML report with colour-coded rows by membership status so the client  
could visually scan and verify records without opening Python or Excel.

---

## 📊 Output Summary

| Output | Records | Purpose |
|--------|---------|---------|
| `final_members_fixed.csv` | ~25,315 | Master clean database |
| `final_categorized.csv` | ~25,315 | With tier/category tags |
| `members for website.csv` | Subset | Public member directory |
| Zoho import file | 25,315 | CRM lead upload |
| `Colour.html` | — | Visual status review |

---

## 🛠️ Tech Stack

| Tool | Usage |
|------|-------|
| `Python 3.x` | All scripting |
| `pandas` | Merge, clean, deduplicate, join |
| `openpyxl` | Excel read/write |
| `re` (regex) | Mobile & pattern parsing |
| `numpy` | NaN/null handling |
| `HTML + CSS` | Colour-coded reporting |

---

## 🚀 How to Run (on Sample Data)

```bash
git clone https://github.com/YOUR_USERNAME/lwl-member-data-pipeline.git
cd lwl-member-data-pipeline

pip install -r requirements.txt

# Run Zoho pipeline on sample data
python zoho_pipeline/pipeline.py \
  --raw sample_data/sample_raw_input.csv \
  --output output.xlsx
```

---

## 📌 What I Learned

- Real data has **no single clean source of truth** — you build it from fragments
- **Email as a join key** works even across messy, inconsistently formatted datasets
- CRM business rules (like Zoho's mandatory Last Name) completely change your cleaning logic
- Iterative pipeline design: each script revealed what the previous step missed
- Delivering a **visual HTML output** alongside data files made client review much faster
- The difference between a one-time fix and a **reusable, documented pipeline**

---

## 🔒 Data Privacy

This project was completed under a **Non-Disclosure Agreement (NDA)**.  
No real client data is included in this repository.  
All sample files use fully synthetic, randomly generated records.

---

*⭐ If this helped you, give it a star!*
>>>>>>> f2dd10b (LWL data pipeline project)