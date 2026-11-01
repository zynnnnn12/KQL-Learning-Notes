# KQL: Project, Project-Reorder, and Column Renaming

## 1. The `project` Operator

The `project` operator selects specific columns from a table and displays only those columns in the query results.

**Syntax:**

```kql
TableName
| project Column1, Column2, Column3
```

### Example 1: Select Specific Columns

```kql
Employees
| project name, user_agent, ip_addr, role
```

**Purpose:** Displays only the specified columns: `name`, `user_agent`, `ip_addr`, and `role`.

Other columns from the original table are excluded.

### Example 2: Filter and Select Columns

```kql
Employees
| where role == "IT associate"
| project name, user_agent, ip_addr, role
```

**Purpose:**
- `where` filters employees whose role is `IT associate`.
- `project` displays only the selected columns from the filtered records.

---

## 2. The `project-reorder` Operator

The `project-reorder` operator changes the order of columns in the output.

It moves the specified columns to the beginning while retaining the other columns.

**Syntax:**

```kql
TableName
| project-reorder Column1, Column2, Column3
```

### Example:

```kql
Employees
| project-reorder name, user_agent, ip_addr, role
```

**Purpose:** Moves the specified columns to the beginning of the output and keeps all remaining columns after them.

**Note:** Unlike `project`, `project-reorder` does not remove unspecified columns.

---

## 3. Renaming Columns Using `project`

The `project` operator can also rename columns by assigning a new name to an existing column.

**Syntax:**

```kql
TableName
| project NewColumnName = ExistingColumnName
```

### Example: Rename a Column

```kql
Employees
| project name, AGENTS = user_agent, ip_addr
```

**Purpose:** Displays the selected columns and renames `user_agent` to `AGENTS` in the query output.

**Note:** The original table is not modified. The new column name applies only to the query results.

---

## Quick Revision

| Operator | Purpose |
|---|---|
| `project` | Selects specific columns and removes the rest |
| `project-reorder` | Reorders selected columns while retaining others |
| `NewName = ExistingName` | Renames a column in the query output |




