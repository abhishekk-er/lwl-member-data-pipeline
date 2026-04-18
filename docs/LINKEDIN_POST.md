# LinkedIn Post — Draft

---

🎉 Just wrapped up my first freelance data project — and it was a lot more than I expected!

I built a **complete data pipeline** for a client managing the LWL Community, a 25,000+ member women entrepreneur network across India.

**What started as "clean this CSV" turned into a full data engineering sprint:**

📥 **The raw reality:**
- Member data scattered across multiple Excel/CSV exports
- 59-column CRM dumps needing to map to 22 Zoho Lead fields
- 13,000+ mobile numbers in 10 different broken formats
- Status conflicts between the master DB and CRM exports
- Members with no First/Last Name (which Zoho rejects at import)

🔧 **What I built — script by script:**
- `merge_master.py` — Combined multiple sources into one clean master DB
- `find_headers.py` — Auto-detected headers across inconsistently formatted files
- `diagnose.py` — Flagged data quality issues before touching anything
- `check_status.py` + `find_conflicts.py` — Detected & resolved status mismatches
- `fix_status.py` — Applied corrections at scale
- `add_categories.py` — Tagged members by tier and category
- `Colour.html` — Colour-coded HTML view so the client could visually review records

📊 **Final outputs delivered:**
✅ 25,315 clean records for Zoho CRM import
✅ Filtered member list for the public website directory
✅ Clean master database with resolved statuses
✅ Visual HTML status report for client review

**Tech:** Python · pandas · openpyxl · regex · HTML/CSS

The biggest lesson? Real data has no single source of truth — you build it piece by piece from fragments. And every script reveals the next problem to solve.

Full documentation on GitHub (synthetic sample data only — real data is under NDA):
👉 github.com/YOUR_USERNAME/lwl-member-data-pipeline

#Python #DataEngineering #Freelance #Pandas #ZohoCRM #DataCleaning #FirstFreelanceProject #OpenToWork
