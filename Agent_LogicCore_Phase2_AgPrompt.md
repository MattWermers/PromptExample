# Agent_Logic_Core (Phase 2) — Prompt Ledger

**Spawned by:** manager_ETLIntake  
**Phase:** 2 — Row-Based Extraction Layer  
**Spawned at:** 2026-05-25  
**Depends on:** Agent_Scaffold Phase 2 (COMPLETE ✅ — 10 failing tests in place)

---

## Delegation

**Target_Path:** `DWPlus/services/row_services/RowExtractionService.cs`  
**Framework_Rules:** .NET 8.0, no WebJobs. Implement strictly to make all 10 xUnit tests pass. Retry budget: 2.  
**Input_Signature:** `PropertyBag` + `Dictionary<string, Dictionary<string, List<string>>>`  
**Expected_Output:** All 10 tests green. `dotnet test` shows `Passed: 10, Failed: 0`.  
**Retry_Budget:** 2  

---

## Execution Results

**Completed:** 2026-05-25 | **Tests:** ✅ 10/10 PASSING | **Modified:** `DWPlus/services/row_services/RowExtractionService.cs` only

### Implementation
- `RecRowVal`: `Exists` rule checks `RawInputs[fieldName]` for null/whitespace → appends to `"Anomalies"` List. Unknown rules skipped, never throws.
- `RecRowTrans`: `Trim/DollarAmount` strips `$` + whitespace; `MultiValue` splits on `", "` into named targets; `Format/RemoveMiddleInitial` regex strips trailing ` X.`. Unknown categories skipped, never throws.
- `HandleRowAnomalyStub`: returns `bag` unchanged.
- Zero WebJobs namespaces. No test/interface files touched.
