# VinSmart AI HomeCare — VinUni Lab 02 (15.000 users scenario)

> **Nhóm:** [ĐIỀN] | **Thành viên + email:** [ĐIỀN]
> Independent adaptation of instructor Lab 02. VinSmart HomeCare is a **hypothetical** product. **15,000 assumed users is not verified MAU.**

## File mapping to instructor worksheet
- `01-problem-scan.md` → Phase 1 SCAN (6 items), Phase 2 QUICK-ASSESS (3 Cards)
- `02-deep-dive-report.md` → Phase 3 current-state/6-field/AI Fit, Phase 5 GO offline
- `04-workflow-diagram.png` → **Current-state** 8-min/ticket workflow (hypothetical)
- `03-ai-log.md` → Phase 6 AI/reflection; add personal run evidence
- `starter-code/prompt_prototype.py` → Phase 4 independent Gemini prototype (not proof of original autograder compatibility)

## Core assumptions (uniform across artifacts)
15,000 assumed users × 10% monthly one-ticket incidence = 1,500 tickets/mo; 8 min handling/ticket = 200 hours/mo; 70% low-risk × 70% successfully automated = **735 auto-resolved tickets**, **765 tickets still require people**, **102 hours human handling**, ~98 hours potentially saved **before AI ops/QA**. 765 ≠ number of employees.

## Decision
**GO for offline prototype only**; NOT YET for production or real customer auto-send. Compare against basic FAQ/rule solution. Data, support workflow, economics and permissions require validation.

## Running local Python demo
```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install google-genai
$env:GEMINI_API_KEY="YOUR_API_KEY"
python starter-code/prompt_prototype.py
```
No Gemini API testing was performed during generation. `python -m py_compile` is a syntax test only.

## Git submission caveat
Instructor repository requests **individual Python code on individual branches only**, and selected `.md`/workflow on `main`. Use the instructor's original `starter-code/prompt_prototype.py` if grading requires its names/functions; port over prompt and tests after verifying contract. Fill all team details and obtain approval for VinSmart scope, as original exercise examples emphasize other Vingroup business units.

Reference: https://github.com/GDGoC-FPTU/VinUni_Codelab_Day02_Template
