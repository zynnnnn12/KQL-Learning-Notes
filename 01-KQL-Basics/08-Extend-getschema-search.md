# KQL: `extend`, `getschema`, and `search`

## 1. The `search` Operator

The `search` operator searches for matching values across the columns of a table or specified tables.

**Syntax:**

```kql
TableName
| search "value"
```

### Example 1: Search for a specific value

```kql
Employees
| search "Trial Administrator"
```

**Purpose:** Searches for `Trial Administrator` across the searchable columns of the `Employees` table.

### Example 2: Find usernames starting with specific characters

```kql
Employees
| search username:"ba*"
```

**Purpose:** Returns records where the `username` column starts with `ba`.

### Example 3: Find usernames ending with specific characters

```kql
Employees
| search username:"*ker"
```

**Purpose:** Returns records where the `username` column ends with `ker`.

### Example 4: Find usernames containing specific characters

```kql
Employees
| search username:"*ker*"
```

**Purpose:** Returns records where the `username` column contains `ker` anywhere in the value.

**Note:** 
- `ba*` → Starts with `ba`.
- `*ker` → Ends with `ker`.
- `*ker*` → Contains `ker` anywhere.

The wildcard `*` represents zero or more characters.

---

## 2. The `getschema` Operator

The `getschema` operator returns the structure of a table or query result, including column names, data types, and other schema information.

**Syntax:**

```kql
TableName
| getschema
```

### Example:

```kql
Trips
| getschema
```

**Purpose:** Displays the schema of the `Trips` table.

It helps identify:
- Column names.
- Data types (string, int, datetime, real, etc.).
- Column order.
- Other column metadata.

**When to use it:** When you want to understand the structure of a table before writing queries.

---

## 3. The `extend` Operator

The `extend` operator creates a new calculated column or adds a new expression to the query results without modifying the original table.

**Syntax:**

```kql
TableName
| extend NewColumn = Expression
```

### Example 1: Create a calculated column

```kql
Trips
| extend cost = tip_amount + 100
| project tip_amount, cost
```

**Purpose:**
1. `extend cost = tip_amount + 100` creates a new column called `cost`.
2. It calculates the value by adding 100 to `tip_amount`.
3. `project` displays only `tip_amount` and the newly created `cost` column.

### Example 2: Create a column using multiple fields

```kql
Trips
| extend total_amount = fare_amount + tip_amount
| project fare_amount, tip_amount, total_amount
```

**Purpose:** Creates a new column called `total_amount` by adding the fare and tip amounts.

**Note:** `extend` does not permanently change the original table. The calculated column exists only in the query results.

---

