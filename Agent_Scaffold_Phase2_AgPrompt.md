# Agent_Scaffold (Phase 2) — Prompt Ledger

**Spawned by:** manager_ETLIntake  
**Phase:** 2 — Row-Based Extraction Layer  
**Spawned at:** 2026-05-25  
**Depends on:** Phase 1 (COMPLETE ✅)

---

## Delegation

**Target_Path:** `DWPlus/services/row_services/`, `tests/DWPlus.Tests/`  
**Framework_Rules:** .NET 8.0, Isolated Worker. TDD enforced — failing xUnit tests MUST be written before any implementation. Namespace: `DentalWriterPlus_DataIntegration`.  
**Input_Signature:** `Dictionary<string, Dictionary<string, List<string>>>` (TransformRules / ValidationRules from EF_Config)  
**Expected_Output:** Interface skeletons + failing xUnit test suite that defines the exact I/O boundary.  
**Retry_Budget:** 2  

---

## Execution Results

**Completed:** 2026-05-25 | **Build:** ✅ 0 Errors | **Tests:** ✅ 10/10 FAILING (NotImplementedException) — contract locked

### Files Created
| File | Status |
|------|--------|
| `DWPlus/services/row_services/IRowExtractionService.cs` | ✅ |
| `DWPlus/services/row_services/RowExtractionService.cs` | ✅ skeleton |
| `tests/DWPlus.Tests/RowExtractionServiceTests.cs` | ✅ 10 failing tests |

### Notes
- Debug build locked by running Functions host (PID 29372). Release build clean. Not a code issue.
- Added `using Xunit;` explicitly — `ImplicitUsings` does not include xUnit.
