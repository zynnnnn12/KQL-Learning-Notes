# KQL: `distinct`, `count`, `sort by`, and `order by`

## 1. The `sort by` Operator

The `sort by` operator sorts records based on one or more columns.

**Syntax:**

```kql
TableName
| sort by ColumnName asc
```

- `asc` – Ascending order (A to Z, smallest to largest).
- `desc` – Descending order (Z to A, largest to smallest).
- Default – Descending (`desc`) if no direction is specified.

### Example 1: Sort in ascending order

```kql
ProcessEvents
| where process_name == "excel.exe"
| sort by hostname asc
```

**Purpose:** Filters Excel process events and sorts the results by hostname in ascending order.

### Example 2: Sort in descending order

```kql
ProcessEvents
| where process_name == "excel.exe"
| sort by hostname
```

**Purpose:** Sorts the results by hostname in descending order because `desc` is the default.

### Example 3: Sort using multiple columns

```kql
ProcessEvents
| where process_name == "excel.exe"
| sort by parent_process_name asc, process_hash asc
```

**Purpose:** Sorts records first by `parent_process_name` and then by `process_hash` when the first column has matching values.

---

## 2. The `order by` Operator

`order by` is another name (alias) for `sort by`. Both operators work in the same way.

### Example:

```kql
ProcessEvents
| where process_name == "excel.exe"
| order by hostname
```

**Purpose:** Filters Excel process events and sorts the results by hostname in descending order.

**Note:** You can use either `sort by` or `order by`.

---

## 3. The `distinct` Operator

The `distinct` operator returns unique values from one or more specified columns, removing duplicates.

**Syntax:**

```kql
TableName
| distinct ColumnName
```

### Example 1: Find unique hostnames

```kql
ProcessEvents
| distinct hostname
```

**Purpose:** Returns all unique hostnames from the `ProcessEvents` table.

### Example 2: Find unique employee roles

```kql
Employees
| distinct role
```

**Purpose:** Returns all unique roles available in the `Employees` table without duplicate values.

**Note:** You can use multiple columns with `distinct` to find unique combinations of their values.

---

## 4. The `count` Operator

The `count` operator returns the total number of records in the input dataset.

**Syntax:**

```kql
TableName
| count
```

### Example: Count employees with a specific role

```kql
Employees
| where role == "Trial Administrator"
| count
```

**Purpose:** Returns the total number of employee records whose role is `Trial Administrator`.

---




