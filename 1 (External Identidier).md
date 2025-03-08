# Understanding External Identifiers in Database Conceptual Modeling

This article explains what an **external identifier** is in the realm of database conceptual design, how it relates to **weak entities**, and why this scenario most often arises in **one-to-many (1:N)** or sometimes **one-to-one (1:1)** relationships—rather than in **many-to-many** relationships.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [What Are Weak Entities?](#what-are-weak-entities)  
3. [External Identifiers and Their Role](#external-identifiers-and-their-role)  
4. [Common Relationship Structures](#common-relationship-structures)  
   1. [1:N (Owner–Dependent)](#1n-ownerdependent)  
   2. [1:1 (Rare But Possible)](#11-rare-but-possible)  
   3. [M:N (Associative Entities vs. Weak Entities)](#mn-associative-entities-vs-weak-entities)  
5. [Real-World Examples](#real-world-examples)  
   1. [Cinema & Hall](#cinema--hall)  
   2. [University & Department Rooms](#university--department-rooms)  
   3. [Person & Birth Certificate](#person--birth-certificate)  
6. [Key Takeaways](#key-takeaways)  
7. [Further Reading](#further-reading)  

---

## 1. Introduction

In a typical data model, most entities can stand on their own with a **primary key** that uniquely identifies each instance (for example, a `Customer` entity might have `CustomerID`). However, there are times when an entity relies on another entity to **complete** its identifier. This dependency is often referred to as having an **external identifier** or being a **weak entity**.  

A “weak entity” is so named because it is “weak” in the sense that it **cannot** be uniquely identified solely by attributes it owns. Instead, it depends on the primary key (or partial key) of another (strong) entity to form a **composite identifier**.

---

## 2. What Are Weak Entities?

A **weak entity**:

- **Cannot** be uniquely identified by its own attributes alone.  
- Relies on a **strong** (or “owner”) entity for at least part of its identifier.  
- Often has a **partial key** (also called a “discriminator”)—an attribute that is unique only within the relationship to the strong entity.

In many ER diagram notations, a **weak entity** is shown with a **double rectangle**, and the **identifying relationship** is shown with a **double diamond**. You might also see the owner’s key underlined twice to indicate it is part of the weak entity’s key.

---

## 3. External Identifiers and Their Role

An **external identifier** is essentially the part of the weak entity’s **primary key** that comes from the owner entity. Formally, a weak entity’s full key is the combination of:

```
(OwnerPrimaryKey, WeakEntityPartialKey)
```

For instance, if a `Hall` in a cinema is identified by `(CinemaID, HallNumber)`, then `CinemaID` is the **external identifier** coming from the owner (`Cinema`), and `HallNumber` is the partial key specific to the `Hall` itself.

---

## 4. Common Relationship Structures

### 4.1 1:N (Owner–Dependent)

Weak entities most commonly appear in a **one-to-many** (1:N) relationship:

- **“One”** side → **Owner** (strong entity).  
- **“Many”** side → **Weak entity**, using the owner’s key + its own partial key for uniqueness.  

Example:  
- One `Cinema` can have many `Halls`.  
- A `Hall` cannot exist without a `Cinema`, and `Hall` is uniquely identified only by `(CinemaID, HallNumber)`.

### 4.2 1:1 (Rare But Possible)

A **1:1** weak entity situation occurs if each owner instance has **at most** one weak entity instance. Still, the fundamental rule holds:

- The weak entity’s key is formed by referencing the owner’s key.  

Example:  
- A `Person` and a `BirthCertificate`, where the birth certificate might be keyed by `(PersonID, CertificateNumber)` in a strictly 1:1 scenario.

### 4.3 M:N (Associative Entities vs. Weak Entities)

In a **many-to-many (M:N)** relationship, you do **not** typically have a “weak” entity. Instead, you create an **associative** or **intersection** entity that holds **foreign keys** referencing each strong entity. This does **not** make it a weak entity; it’s simply a table or entity that depends on two (or more) entities **symmetrically**, rather than being “owned” by one.

---

## 5. Real-World Examples

### 5.1 Cinema & Hall

- **Cinema** (strong entity):  
  - Attributes: `CinemaID` (PK), `Name`, `Location`  
- **Hall** (weak entity):  
  - Attributes: `HallNumber` (partial key), `Capacity`  
  - Full identifier: `(CinemaID, HallNumber)`  
- Relationship: A cinema **has** many halls → (1:N).

### 5.2 University & Department Rooms

- **University** (strong entity):  
  - Attributes: `UniversityID` (PK), `Name`, `City`  
- **DepartmentRoom** (weak entity):  
  - Attributes: `RoomNumber` (partial key), `Floor`  
  - Full identifier: `(UniversityID, RoomNumber)`  
- Relationship: A university **contains** many department rooms → (1:N).

### 5.3 Person & Birth Certificate

- **Person** (strong entity):  
  - Attributes: `PersonID` (PK), `Name`, `DateOfBirth`  
- **BirthCertificate** (weak entity):  
  - Attributes: `CertificateNumber` (partial key)  
  - Full identifier: `(PersonID, CertificateNumber)`  
- Relationship: A person **has** a birth certificate → (1:1) in many jurisdictions.

---

## 6. Key Takeaways

1. A **weak entity** needs **part of its primary key** from a **strong owner** entity.  
2. **External identifiers** are the inherited part of the primary key from that owner entity.  
3. Such a setup is **most common in 1:N** relationships (one owner, many weak entities).  
4. **1:1** weak entities exist but are rarer.  
5. In **M:N** relationships, we typically do **not** have weak entities; we use **associative** (intersection) entities that reference two or more strong entities.

---

## 7. Further Reading

- **Elmasri & Navathe**, *Fundamentals of Database Systems*  
- **Peter Chen’s Original ER Model Paper (1976)**  
- **UML Distilled** by Martin Fowler  

