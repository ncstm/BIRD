# **Experiment Log: SQL Generation on BIRD Dataset**

## BIRD benchmark performance: 68%

---

## 1. Task Structure

`Schema + Question + Evidence` -> `SQL`

---

## 2. Agent Architecture


| Component     | Model Version | Purpose         | Notes                              |
| ------------- | ------------- | --------------- | ---------------------------------- |
| SQL Generator | o4 - mini | Generate SQL  | Performs analysis, exploration and SQL construction simultanously   |
| SQL Repair    | GPT 4.1   | Fix broken SQL  | Concentrates on just delivering working SQL code      |
| Decider       | GPT 4.1 - nano | Validate answer | Compares answers to objective SQL provide with question |


### 2.1 SQL Generator (v0.1 – mini, Collab 13)

**Tool:** `Execute SQL`

**Instructions to model:**

* Understand schema structure. 
* Then slowly construct the SQL, running tests and probing information by using the SQL tool. 
* Directly try the final SQL to ensure accuracy.

---

### 2.2 SQL Repair (GPT v1.1)

**Tool:** `Execute SQL`

**Instructions to model:**

* Aim to fix SQL ASAP
* Finish chat only if you have a tested and working SQL.

---

### 2.3 Decider (GPT v2 – nano)

**Role:**

* Evaluate if the given answer is acceptable given the true answer.

**Instructions to model:**

* Only answer correct or incorrect.

---

## 3. Additional Strategies Attempted (Rejected or Modified)

### 3.1 Reduced Schema

**Aim:**

* Focus model attention on relevant schema fields.
* Reduce cognitive load and cost.

**Approach:**

* LLM based reduction + tools to compensate over - pruning.
* Tools:
    * Search column in schema.
    * Retrieve a complete table.
    * Retrieve full schema.

**Result:** ❌ Fail

* Models often needed broader context.
* Truncated schemas led to decreased performance.
* Without selection ensemble cost savings stopped being.
* I speculate it could work better with sharper, smarter schema field reduction.

---

### 3.2 Selection Ensemble

**Goal:** Combine outputs from multiple agents

**Result:** ❌ Fail

**Observations:**

* Very dependent in judge, overall performance was inconsistent and did not optimize answer choice.
* Often trained in literature, but without training, other tool calling, user input or example driven improvement performance left much to be desired.

---

### 3.3 Pre-Analysis of Data

**Result:** ⚠️ Partial Success

**Rationale:**

* Sometimes extracted useful insights.
* However, it’s better to have it mesh with SQL generation as needs arise.
* Dividing analysis and construction wasted resources

**Conclusion:** Added to final product in the form of additional instructions.

---

### 3.4 COT / POT (Chain of Thought / Plan of Thought)

**Observation:**

* Reasoning models fulfil the roles these aim in a more natural and effective way.
* No major performance improvement.
* More direct formats with added tooling yielded better results.
