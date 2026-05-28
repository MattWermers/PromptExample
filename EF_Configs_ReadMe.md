# Extraction Configuration (`EF_Config`) Reference

## Purpose
The `EF_Config` instructs the Data Integration pipeline on which column headers to extract from a source CSV file and how to map them to internal attributes within the `PropertyBag` object. It defines the internal attribute name, expected data type, and enforces any attribute-level transformations or validations required during Phase 2 (Row-Based Extraction).

---

## 1. Details
Contains metadata regarding the configuration file.

* **`EF_ID`:** The unique identifier for the config (e.g., `NewPatientLedger`).
* **`Update`:** Timestamp of the last file update or creation.
* **`Version`:** Current version string of the configuration.
* **`Note`:** Implementation notes, requirements, and structural warnings.

---

## 2. PipelineTasks
A dictionary mapping specific transformation rules to boolean flags. Indicates which `TF_Config` execution rules should be applied to the dataset during Phase 3.

---

## 3. RowTargets_List
An array defining the extraction mapping. Each object maps a `SourceHeader` to its internal rules.

### `SourceHeader`
The string value of the exact column header found in the source CSV file.

### `Rules`
Defines the extraction, validation, and transformation constraints for the mapped header.

* **`Required` (Bool):** Flag indicating if the attribute can be null upon extraction.
* **`MultiValue` (Bool):** Indicates that the source attribute contains delimited data and must be split into multiple internal attributes.
* **`Transform` (Bool):** Flag indicating if attribute-level transformations exist for this header.
* **`Validate` (Bool):** Flag indicating if attribute-level validations must be performed.
* **`DWPlus_Name` (String):** The internal key used when saving the value to the `PropertyBag`.
* **`StoredAs` (String):** The target database data type.

#### Multi-Value Handling
* **`MultiValueTargets` (List<String>):** If `MultiValue` is true, this array lists the `DWPlus_Name`s of the resulting attributes.
* **Dependency Constraint:** Target attributes must be defined in the `RowTargets_List` immediately following the parent multi-valued attribute, using the parent's `DWPlus_Name` as their `SourceHeader`.

#### Transformation & Validation Maps
* **`TransformRules`:** `Dictionary<string, Dictionary<string, List<string>>>`
  Defines specific row-level transformations (e.g., Trim, Format). Execution order is guaranteed by JSON parsing.
* **`ValidationRules`:** `Dictionary<string, Dictionary<string, List<string>>>`
  Defines specific row-level validations (e.g., Exists). Execution order is guaranteed by JSON parsing.