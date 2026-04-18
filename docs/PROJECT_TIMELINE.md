# Project Timeline & Work Log

## Day 1 — Initial Setup & First Merge (18-03-2026)

| Time | File Created | What Was Done |
|------|-------------|---------------|
| 05:39 PM | `members (1) - members (1).csv` | First raw data received |
| 06:01 PM | `.venv/` | Set up Python virtual environment |
| 06:11 PM | `merge.py`, `final_members.xlsx` | First merge attempt |
| 06:20 PM | `LWL Community Master Database.csv` | Master database received |
| 06:29 PM | `export_csv.py` | Export script created |
| 06:31 PM | `final_members.csv` | First cleaned output |
| 07:12 PM | `diagnose.py` | Built data diagnostics tool |
| 09:50 PM | `find_headers.py` | Built header auto-detection |
| 09:51 PM | `merge_master.py`, `final_members_v2.csv` | Improved merge with master DB |
| 09:53 PM | `cleanup.py`, `final_members_clean.csv` | Cleaned version |
| 09:59 PM | `export_final.py`, `final_all_members.csv` | Full export |
| 10:10 PM | `add_categories.py`, `final_categorized.csv` | Added tier/category tagging |

---

## Day 2 — Status Validation & Conflict Resolution (19-03-2026)

| Time | File Created | What Was Done |
|------|-------------|---------------|
| 02:14 PM | `Final data/`, `members - final_members_clean.csv` | Final data folder organized |
| 02:25 PM | `check_status.py` | Built status validation script |
| 11:12 PM | `status_mismatches.csv` | Found status conflicts across sources |

---

## Day 3 — Conflict Fix & Website Prep (20-03-2026)

| Time | File Created | What Was Done |
|------|-------------|---------------|
| 02:47 PM | `fix_status.py`, `final_members_fixed.csv` | Applied status corrections |
| 03:08 PM | `members for website.csv` | Filtered subset for website |
| 03:18 PM | `find_conflicts.py`, `status_conflicts.csv` | Conflict detection refined |
| 03:30 PM | `Colour.html`, `Colour_files/` | Colour-coded HTML status view |
| 03:31 PM | `color_coding_legend.html` | Visual legend for colours |
| 03:51 PM | `nonactive_but_active_in_master.csv` | Flagged edge cases |
| 04:13 PM | `find_conflicts2.py` | Second-pass conflict detection |

---

## Key Insight

The project evolved organically — each script was written to solve a problem  
discovered by the previous script. This is normal in real-world data work:

```
merge → discover header issues → fix headers
→ discover status conflicts → build conflict detector
→ resolve conflicts → discover edge cases
→ flag edge cases → deliver final clean output
```

Total active development: **~2 days across 3 calendar days**
