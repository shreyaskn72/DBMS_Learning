
### Scenario: Order Management

#### Unnormalized Table

**Table: Orders**
```
| OrderID | CustomerName | ProductName | Quantity | Price |
|---------|--------------|--------------|----------|-------|
| 1       | John Doe     | Widget A     | 2        | 20    |
| 1       | John Doe     | Widget B     | 1        | 30    |
| 2       | Jane Smith   | Widget A     | 1        | 20    |
| 3       | John Doe     | Widget C     | 3        | 25    |
```

### Step 1: First Normal Form (1NF)

**1NF Requirements:**
- Eliminate repeating groups.
- Ensure that each column contains atomic values.

**Pictorial Representation:**
```
| OrderID | CustomerName | ProductName | Quantity | Price |
|---------|--------------|--------------|----------|-------|
| 1       | John Doe     | Widget A     | 2        | 20    |
| 1       | John Doe     | Widget B     | 1        | 30    |
| 2       | Jane Smith   | Widget A     | 1        | 20    |
| 3       | John Doe     | Widget C     | 3        | 25    |
```

*The table is already in 1NF since all values are atomic.*

### Step 2: Second Normal Form (2NF)

**2NF Requirements:**
- The table must be in 1NF.
- All non-key attributes must be fully functionally dependent on the primary key.

**Issues:**
- The non-key attributes (`CustomerName`, `ProductName`, `Quantity`, `Price`) depend on a composite key (`OrderID` + `ProductName`).

**Pictorial Representation:**
**1. Orders Table:**
```
| OrderID | CustomerName |
|---------|--------------|
| 1       | John Doe     |
| 2       | Jane Smith   |
| 3       | John Doe     |
```

**2. OrderDetails Table:**
```
| OrderID | ProductName | Quantity | Price |
|---------|--------------|----------|-------|
| 1       | Widget A     | 2        | 20    |
| 1       | Widget B     | 1        | 30    |
| 2       | Widget A     | 1        | 20    |
| 3       | Widget C     | 3        | 25    |
```

### Step 3: Third Normal Form (3NF)

**3NF Requirements:**
- The table must be in 2NF.
- All non-key attributes must be non-transitively dependent on the primary key.

**Issues:**
- `Price` is dependent on `ProductName`, not directly on `OrderID`.

**Pictorial Representation:**
**1. Orders Table:**
```
| OrderID | CustomerName |
|---------|--------------|
| 1       | John Doe     |
| 2       | Jane Smith   |
| 3       | John Doe     |
```

**2. OrderDetails Table:**
```
| OrderID | ProductName | Quantity |
|---------|--------------|----------|
| 1       | Widget A     | 2        |
| 1       | Widget B     | 1        |
| 2       | Widget A     | 1        |
| 3       | Widget C     | 3        |
```

**3. Products Table:**
```
| ProductName | Price |
|--------------|-------|
| Widget A     | 20    |
| Widget B     | 30    |
| Widget C     | 25    |
```

### Summary of Normalization Steps

- **Unnormalized Table**: Contains duplicate and non-atomic data.
- **1NF**: Restructured to eliminate repeating groups.
- **2NF**: Split into `Orders` and `OrderDetails` to ensure all non-key attributes are fully dependent on the primary key.
- **3NF**: Created a separate `Products` table to eliminate transitive dependency.

### Benefits of Normalization
- **Reduced Redundancy**: The data is now stored efficiently without unnecessary duplication.
- **Improved Data Integrity**: Each piece of data is stored only once, reducing the risk of inconsistency.
- **Simplified Maintenance**: Updates, deletions, and insertions can be performed more easily.

