# KQL: Date and Time Operators – `between`, `ago()`, and `now()`

## 1. The `ago()` Function

The `ago()` function returns a date and time relative to the current UTC time by subtracting the specified time duration.

**Syntax:**

```kql
ago(TimeDuration)
```

### Example 1: Get records from the last 1000 days

```kql
Trips
| where pickup_datetime >= ago(1000d)
```

**Purpose:** Returns trip records where `pickup_datetime` is within the last 1000 days, including future-dated records if present.

### Example 2: Get records from the last 500 days

```kql
Trips
| where pickup_datetime >= ago(500d)
```

**Purpose:** Returns records from the last 500 days up to the current time, including any future-dated records.

**Common time units:**

| Unit | Meaning |
|---|---|
| `s` | Seconds |
| `m` | Minutes |
| `h` | Hours |
| `d` | Days |
| `w` | Weeks |

---

## 2. The `between` Operator

The `between` operator filters records within a specified range of values.

It includes both the starting and ending values.

**Syntax:**

```kql
TableName
| where DateColumn between (StartDate .. EndDate)
```

### Example 1: Get records between two dates

```kql
Trips
| where pickup_datetime between
    (datetime(2023-02-10) .. datetime(2023-08-11))
```

**Purpose:** Returns trip records with `pickup_datetime` between February 10, 2023, and August 11, 2023, including both boundary dates.

### Example 2: Get records from a date until the present

```kql
Trips
| where pickup_datetime between
    (datetime(2023-02-10) .. now())
```

**Purpose:** Returns trip records from February 10, 2023, up to the current date and time.

---

## 3. The `now()` Function

The `now()` function returns the current UTC date and time.

It is commonly used to filter records up to the current time or compare timestamps with the present.

### Example:

```kql
Trips
| where pickup_datetime <= now()
```

**Purpose:** Returns trip records whose pickup date and time are less than or equal to the current time.

**Note:** `now()` remains consistent throughout a single query statement.

---

## 4. Combining Date, String, and Numeric Conditions

You can combine date filters with other conditions using logical operators such as `and` and `or`.

### Example: Find expensive trips in Brooklyn from the last 500 days

```kql
Trips
| where pickup_datetime >= ago(500d)
    and pickup_boroname == "Brooklyn"
    and fare_amount >= 50
```

**Purpose:** Returns trip records that satisfy all three conditions:

1. Pickup date is within the last 500 days.
2. Pickup borough is Brooklyn.
3. Fare amount is greater than or equal to 50.

---



