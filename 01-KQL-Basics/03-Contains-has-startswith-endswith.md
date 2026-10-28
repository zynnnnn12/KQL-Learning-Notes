# KQL String Operators: `contains`, `has`, `startswith`, and `endswith`

## 1. The `contains` Operator

The `contains` operator searches for a substring anywhere within a string — at the beginning, in the middle, or at the end. It is case-insensitive.

**Syntax:**

```kql
TableName
| where ColumnName contains "value"
```

### Example: Search URLs for a substring

```kql
InboundBrowsing
| where url contains "envolve"
```

**Purpose:** Returns records where the URL contains `envolve`, regardless of capitalization.

**Note:** `contains` can require more processing because it searches for arbitrary substrings. Prefer `has` when you need to match a complete term. 

## 2. The `has` Operator

The `has` operator searches for a complete term rather than an arbitrary substring. It is case-insensitive and can use KQL's term index for faster searches when the term is at least three characters long.

### Example 1: Search user agents

```kql
InboundBrowsing
| where user_agent has "Firefox"
```

**Purpose:** Returns records where `user_agent` contains `Firefox` as a complete term.

For example, a user-agent string such as `Mozilla/5.0 Firefox/120.0` can match.

### Example 2: Compare `has` with `==`

```kql
InboundBrowsing
| where user_agent == "Firefox"
```

**Purpose:** Returns records where the entire `user_agent` value equals `Firefox`.

**Why might this return no results?**

If the column contains full user-agent strings such as `Mozilla/5.0 Firefox/120.0`, the complete value does not equal `Firefox`.

**Key difference:**
- `has` — Searches for a complete term within the string.
- `==` — Checks whether the entire string matches the specified value.

---

## 3. The `startswith` Operator

The `startswith` operator checks whether a string begins with a specified sequence of characters. It is case-insensitive.

### Example: Find IP addresses starting with `145`

```kql
InboundBrowsing
| where src_ip startswith "145"
```

**Purpose:** Returns records where the `src_ip` string starts with `145`.

For example, `145.10.20.30` matches.

## 4. The `endswith` Operator

The `endswith` operator checks whether a string ends with a specified sequence of characters. It is case-insensitive.

### Example: Find IP addresses ending with `33`

```kql
InboundBrowsing
| where src_ip endswith "33"
```

**Purpose:** Returns records where the `src_ip` string ends with `33`.

For example, `145.10.20.33` matches.

---

## 5. Negative String Operators (`!`)

Adding `!` negates the condition. These operators return records that do not satisfy the specified match.

| Operator | Meaning |
|---|---|
| `!contains` | Does not contain the specified substring |
| `!has` | Does not contain the specified complete term |
| `!startswith` | Does not start with the specified sequence |
| `!endswith` | Does not end with the specified sequence |

### Example 1: URLs that do not start with `http`

```kql
InboundBrowsing
| where url !startswith "http"
```

**Purpose:** Returns records where the URL does not start with `http`, regardless of capitalization.

### Example 2: Exclude IP addresses containing specified sequences

```kql
InboundBrowsing
| where src_ip !contains "220"
    and src_ip !contains "143"
```

**Purpose:** Excludes records whose `src_ip` contains either `220` or `143`.

Both conditions must be true for a record to remain in the results.

---

## 6. Case-Sensitive Variants (`_cs`)

By default, `contains`, `has`, `startswith`, and `endswith` are case-insensitive.

Adding `_cs` makes the relevant comparison case-sensitive.

| Case-insensitive | Case-sensitive |
|---|---|
| `contains` | `contains_cs` |
| `has` | `has_cs` |
| `startswith` | `startswith_cs` |
| `endswith` | `endswith_cs` |

### Example: Case-sensitive prefix search

```kql
InboundBrowsing
| where user_agent startswith_cs "Mozilla"
```

**Purpose:** Matches strings beginning with `Mozilla` using the exact capitalization shown.

---

## 7. Practical Example: Find iPhone Users

```kql
InboundBrowsing
| where user_agent has "iPhone"
```

**Purpose:** Returns browsing records where the user-agent string contains `iPhone` as a complete term, regardless of capitalization.

To make the match case-sensitive, use:

```kql
InboundBrowsing
| where user_agent has_cs "iPhone"
```

---

## Quick Revision

| Operator | Use case |
|---|---|
| `contains` | Find a substring anywhere in a string |
| `has` | Find a complete term |
| `startswith` | Find strings beginning with a sequence |
| `endswith` | Find strings ending with a sequence |
| `!contains`, `!has`, etc. | Exclude matching strings |
| `_cs` suffix | Make the string comparison case-sensitive |

**Performance tip:** Prefer `has` for complete-term searches and case-sensitive operators when appropriate. Actual performance depends on the data and query.
