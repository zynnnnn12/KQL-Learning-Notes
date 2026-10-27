# KQL Syntax and Query Structure

## 1. Basic Syntax Rules

### 1. Case Sensitivity
KQL is generally case-sensitive for operators and identifiers in some contexts, but string comparisons depend on the operator used.

- `==` — Case-sensitive equality
- `=~` — Case-insensitive equality
- `!=` — Case-sensitive inequality
- `!~` — Case-insensitive inequality

### 2. Whitespace
KQL generally ignores extra spaces between tokens, so formatting queries with indentation and line breaks makes them easier to read.

### 3. Blank Lines
Blank lines can separate queries in some query editors or contexts, but they are not universally treated as starting a new query. Follow the conventions of the editor you are using.

### 4. Order Matters
KQL operators are processed in sequence from top to bottom. Changing their order can change the results.

### 5. Filter Early
Apply filters early using `where` to reduce the amount of data processed by later operators. This can improve query performance.

---

## 2. Comparison Operators

| Operator | Meaning |
|---|---|
| `==` | Equals (case-sensitive) |
| `!=` | Not equal to (case-sensitive) |
| `=~` | Equals (case-insensitive) |
| `!~` | Not equal to (case-insensitive) |

### Example 1: Exact Match

```kql
Customers
| where CityName == "Ballard"
```

**Purpose:** Returns rows where `CityName` exactly matches `Ballard`, including capitalization.

### Example 2: Not Equal

```kql
Customers
| where CityName != "Ballard"
```

**Purpose:** Returns rows where `CityName` is not equal to `Ballard`, using a case-sensitive comparison.

---

## 3. Case-Insensitive Comparison (`=~`)

The tilde (`~`) in `=~` indicates a case-insensitive comparison.

```kql
Customers
| where FirstName =~ "peter"
```

**Purpose:** Matches names such as `Peter`, `PETER`, and `peter`.

---

## 4. Operator Order

### Example 1: Filter Before Taking Rows

```kql
Customers
| where FirstName == "Peter"
| take 10
```

**Purpose:** Filters for Peter first, then returns up to 10 matching rows.

### Example 2: Take Before Filtering

```kql
Customers
| take 10
| where FirstName == "Peter"
```

**Purpose:** Takes up to 10 rows first, then filters those rows for Peter. It may return fewer results or none, even if other matching records exist elsewhere in the table.

**Key takeaway:** Filter first when you want matching results from the entire dataset, then use `take` to limit the output.

---



