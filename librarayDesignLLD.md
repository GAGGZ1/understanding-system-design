# Library Management System (LLD)

A Library Management System is one of the most common Low-Level Design interview questions because it covers:

* OOP Principles
* SOLID Principles
* Relationships (Association, Composition, Aggregation)
* Design Patterns
* Real-world Modeling

---

# Step 1: Requirements Gathering

### Functional Requirements

Users should be able to:

1. Search books
2. Borrow books
3. Return books
4. Reserve books
5. Pay fines
6. Manage inventory
7. Track issued books

### Non-Functional Requirements

* Maintainable
* Extensible
* Scalable
* Secure

---

# Step 2: Identify Entities

### Core Entities

```text
Library
Book
BookItem
Member
Librarian
BookReservation
BookLending
Fine
Catalog
```

---

# Step 3: Understand the Difference

Many candidates make this mistake:

### Book

Represents a title.

Example:

```text
Atomic Habits
Author: James Clear
ISBN: 12345
```

Only one Book object.

---

### BookItem

Represents a physical copy.

```text
Copy #1
Copy #2
Copy #3
```

There can be multiple copies of the same book.

---

# Step 4: Class Diagram

```text
                 +------------+
                 |   Library  |
                 +------------+
                       |
                       |
              +--------+--------+
              |                 |
              v                 v

          +-------+        +----------+
          |Catalog|        | Member   |
          +-------+        +----------+

                              |
                    +---------+---------+
                    |
                    v

                 +------+
                 |Book  |
                 +------+
                    |
                    |
                    v

               +-----------+
               | BookItem  |
               +-----------+

                    |
            +-------+-------+
            |               |
            v               v

      BookLending     Reservation
```

---

# Step 5: Design Classes

## Book

```java
class Book {
    private String ISBN;
    private String title;
    private String author;
    private String subject;
}
```

---

## BookItem

```java
enum BookStatus {
    AVAILABLE,
    RESERVED,
    LOANED,
    LOST
}

class BookItem extends Book {

    private String barcode;
    private BookStatus status;
    private Date publicationDate;
}
```

---

## Member

```java
class Member {

    private String memberId;
    private String name;

    public boolean borrowBook(BookItem book) {
        return true;
    }

    public void returnBook(BookItem book) {

    }

    public boolean reserveBook(BookItem book) {
        return true;
    }
}
```

---

## Librarian

```java
class Librarian {

    public void addBook(BookItem book) {

    }

    public void removeBook(BookItem book) {

    }

    public void updateBook(BookItem book) {

    }
}
```

---

## BookLending

```java
class BookLending {

    private Date issueDate;
    private Date dueDate;
    private Date returnDate;

    private Member member;
    private BookItem bookItem;
}
```

---

## Reservation

```java
class BookReservation {

    private Date creationDate;
    private Member member;
    private BookItem bookItem;
}
```

---

## Fine

```java
class Fine {

    private double amount;

    public double calculateFine() {
        return amount;
    }
}
```

---

# Step 6: Apply OOP Principles

## Encapsulation

```java
private String title;
private String author;
```

Data hidden inside objects.

---

## Inheritance

```java
BookItem extends Book
```

BookItem inherits Book properties.

---

## Abstraction

```java
interface Search {

    List<Book> searchByTitle(String title);

    List<Book> searchByAuthor(String author);
}
```

User doesn't know how searching works internally.

---

## Polymorphism

```java
Payment payment = new UpiPayment();

payment.pay();
```

Different payment implementations.

---

# Step 7: SOLID Principles

### S - Single Responsibility

Book only manages book data.

Fine only manages fine calculation.

---

### O - Open Closed

Add new payment methods without modifying existing code.

```java
interface PaymentStrategy {
    void pay(double amount);
}
```

---

### L - Liskov Substitution

Any subclass should replace parent.

```java
PaymentStrategy payment =
        new CreditCardPayment();
```

---

### I - Interface Segregation

Instead of:

```java
interface User {
    borrowBook();
    addBook();
    removeBook();
}
```

Use:

```java
Borrowable
Manageable
```

---

### D - Dependency Inversion

```java
class FineService {

   PaymentStrategy payment;
}
```

Depends on abstraction, not implementation.

---

# Step 8: Design Patterns

### Strategy Pattern

For Payments

```java
PaymentStrategy
    |
    +-- UPI
    +-- Card
    +-- NetBanking
```

---

### Factory Pattern

Create users.

```java
UserFactory.createUser("MEMBER")
```

---

### Singleton Pattern

Library Database

```java
LibraryDatabase.getInstance()
```

Only one database manager.

---

# Step 9: Typical Interview Follow-ups

### Q1: How to limit books per member?

```java
MAX_BOOKS_ALLOWED = 5
```

Check before issuing.

---

### Q2: How to handle waiting list?

Use Queue.

```java
Queue<Member> waitingList
```

FIFO reservation.

---

### Q3: How to calculate fine?

```java
daysLate * finePerDay
```

Example:

```text
3 days late
₹10/day

Fine = ₹30
```

---

