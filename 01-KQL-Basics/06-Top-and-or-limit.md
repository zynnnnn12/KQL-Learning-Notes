# KQL: `top`, `limit`, `and`, and `or`

## 1. The `and` Operator

The `and` operator combines conditions. **All conditions must be true** for a record to be returned.

### Example 1: Filter employees by role and IP address

```kql
Employees
| where role == "IT associate" and ip_addr contains "192"
```

**Purpose:** Returns employees whose role is `IT associate` and whose IP address contains `192`.

---

## 2. The `or` Operator

The `or` operator combines conditions. **At least one condition must be true** for a record to be returned.

### Example

```kql
Employees
| where role == "IT associate" or role == "Administrator"
```

**Purpose:** Returns employees whose role is either `IT associate` or `Administrator`.

---

## 3. Combining Conditions Using `and`, `or`, and Parentheses

Parentheses help group conditions and control how they are evaluated.

### Example: Filter customer records

```kql
Customers
| where (Gender == "F" and ContinentName == "Europe")
    and Education == "High School"
| project Gender, ContinentName, Education
```

**Purpose:** Returns customers who meet all three conditions:
- Gender is `F`.
- Continent is `Europe`.
- Education is `High School`.

### Example: Combine `and` with `or`

```kql
Customers
| where (ContinentName == "Europe" or ContinentName == "Asia")
    and Education == "High School"
```

**Purpose:** Returns customers from Europe or Asia who have a High School education.

---

## 4. The `limit` Operator

The `limit` operator restricts the number of records returned.

### Example

```kql
Customers
| limit 10
```

**Purpose:** Returns up to 10 records from the `Customers` table.

**Note:** Without sorting, the returned records are not guaranteed to be in a particular order.

---

## 5. The `top` Operator

The `top` operator returns the specified number of records after sorting them by a column.

**Syntax:**

```kql
TableName
| top Number by ColumnName desc
```

### Example

```kql
Customers
| top 10 by CustomerKey
```

**Purpose:** Returns the 10 records with the highest `CustomerKey` values because descending order (`desc`) is the default.

### Example: Top 10 customers with the lowest keys

```kql
Customers
| top 10 by CustomerKey asc
```

**Purpose:** Returns the 10 records with the lowest `CustomerKey` values.

**Important:** A high `CustomerKey` does not necessarily mean a recent customer. To find recent records, sort by an appropriate timestamp column.

---

## 6. `take` vs `top` vs `limit`

| Feature | `take` | `limit` | `top` |
|---|---|---|---|
| Main purpose | Return a sample of rows | Restrict the number of rows | Return the highest- or lowest-ranked rows |
| Number specified | Yes | Yes | Yes |
| Sorts by a column | No | No | Yes |
| Guarantees ranking | No | No | Yes, by the specified sort column |
| Best use | Quick inspection or sampling | Limit query output | Find top or bottom records |

### Example: `take`

```kql
Customers
| take 10
```

Returns up to 10 records without ranking them by a column.

### Example: `limit`

```kql
Customers
| limit 10
```

Returns up to 10 records without ranking them by a column.

### Example: `top`

```kql
Customers
| top 10 by CustomerKey desc
```

Returns the 10 records with the highest `CustomerKey` values.

**Key takeaway:** Use `take` or `limit` to restrict the number of rows. Use `top` when you want the highest- or lowest-ranked records based on a specific column.

---



