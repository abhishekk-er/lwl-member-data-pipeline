# Member Status Resolution Logic

## Status Values Used

| Status | Meaning |
|--------|---------|
| `Active` | Current paying member |
| `Non-renewed` | Membership expired, not renewed |
| `Prospect` | Potential member, not yet joined |
| `Ex-Member` | Previously active, now inactive |

---

## Conflict Detection (`find_conflicts.py`)

A conflict occurs when the **same record** (matched by Email) has **different status values** across two source files.

**Example conflict:**
| Email | Status in Master DB | Status in CRM Export |
|-------|--------------------|--------------------|
| member@example.com | `Active` | `Non-renewed` |

---

## Resolution Priority

When a conflict is found, the following priority order applies:

1. **CRM Export** (most recently updated)
2. **Master Database**
3. **Manual entry sheets**

---

## Edge Case: `nonactive_but_active_in_master.csv`

Records that are marked `Non-renewed` or `Ex-Member` in the CRM export  
but still appear in the "Active members" section of the master database.

These were flagged separately for manual client review rather than  
auto-corrected, since they may represent billing/renewal delays.

---

## Final Status Fix (`fix_status.py`)

After manual review of flagged conflicts, `fix_status.py` applied the  
confirmed corrections to produce `final_members_fixed.csv`.
