
### Example ER Diagram for a Bookstore

#### Entities
1. **Customer**
   - Attributes: CustomerID, Name, Email

2. **Book**
   - Attributes: BookID, Title, Author, Price

3. **Order**
   - Attributes: OrderID, OrderDate, TotalAmount

#### Relationships
- **Customer** (1) places (N) **Order**
- **Order** (N) contains (M) **Book**

### Pictorial Representation (Text-Based)

You can visualize it like this:

```
 +-----------------+       +-----------------+
 |     Customer    |       |      Order      |
 |-----------------|       |-----------------|
 | CustomerID (PK) |<----- | OrderID (PK)    |
 | Name            |       | OrderDate       |
 | Email           |       | TotalAmount     |
 +-----------------+       +-----------------+
                             |
                             | contains
                             |
                             |       +-----------------+
                             |       |      Book       |
                             |       |-----------------|
                             |-------<| BookID (PK)    |
                                     | Title           |
                                     | Author          |
                                     | Price           |
                                     +-----------------+
```

### Key:
- **PK** indicates the Primary Key for each entity.
- The arrows represent the relationships, with cardinality implied (e.g., one customer can place many orders, and each order can contain many books).

### How to Draw This
1. **Draw rectangles** for each entity.
2. **Add ovals** connected to the rectangles for attributes.
3. **Draw diamonds** for relationships, connecting the related entities.
4. **Label the relationships** and indicate cardinality (1, N, M) near the lines connecting the entities.

