# Transformation Configuration (`TF_Config`) Reference

## Purpose
The `TF_Config` acts as the orchestrator for building database transactions from standardized, parameterized input data. Executed during Phase 3, it instructs the pipeline on how to group collections of `PropertyBag` objects, what sequential dataframe-level transformations to perform, and how to route the final transaction script.

---

## 1. Details
Contains metadata regarding the configuration file.

* **`TF_ID`:** The unique identifier for the config (.g., `NewPatientLedger`).
* **`Update`:** Timestamp of the last file update or creation.
* **`Version`:** Current version string of the configuration.
* **`Note`:** Implementation notes, requirements, and architectural warnings.

---

## 2. Grouping
Defines the structuralization of the in-memory dataset (the `PropertyBag` collection) before pipeline validation. Columns are added sequentially in the order defined by the Grouping block.

### `GroupingRule`
* **Data Type:** `List<String>`
* **Rule:** Must be the first definition in the Grouping block. Only one instance is permitted.
* **Action:** Groups the input data sequentially based on the provided header list.
* **Error States:** Target column not found; Duplicate columns defined; Provided column attribute is null.

### `AddMeasure`
* **Data Type:** Object (`Nullable: bool`, `Columns: List<string>`).
* **Action:** Sequentially searches for the non-null attribute provided in the input list and adds the value to the dataset.
* **Rules:** If `Nullable` is false, exactly one of the provided attributes must be non-null. If `Nullable` is true, all target attributes may evaluate to null.
* **Error States:** Multiple columns evaluate to non-null; All columns evaluate to null (when `Nullable` is false); Target column not found.

### `AddDimensions`
* **Data Type:** `List<String>` 
* **Action:** Adds non-null columns from the source data to the output dataset in the defined order.
* **Rules:** Do not duplicate columns already established in the `GroupingRule`.
* **Error States:** Target column not found; Duplicate columns defined; Provided column attribute is null.

---

## 3. Validation Pipeline
* **`ValidationFlag` (Bool):** Indicates if  `ValidationPipeline` array exists in the configuration.

### `ValidationPipeline` Array
Instructs the transformation service on which validation functions to execute, targeting specific parameters, in the strict order required to build the output payload. Used to aggregate database lookups, concatenate values, or serialize JSON objects.

**Execution Schema:**
Requires generating  JSON dictionary mapping to the standardized call/return pattern:
```json
{
    "Action": "[FunctionName]",
    "InputParams": ["Appropriate parameters from input dataset"],
    "OutputParams": ["Generated state output keys"],
    "On": ["Target tables, or empty array for the output dataframe itself"]
}

---

## 4. TransactionScript
Data Type: String
Purpose: Declares the specific # transaction script (.g., T_NewPatientLedger.cs) responsible for the final database commit. This architecture permits multiple EF_Configs to map to  single TF_Config, which in turn routes to  unified transaction script.