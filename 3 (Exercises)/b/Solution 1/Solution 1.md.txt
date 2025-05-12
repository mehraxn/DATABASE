# DBMS Query Scenarios - Schema Notes and Query Explanations

This document summarizes the primary keys, nullable attributes, and explanations for queries from two sample questions with multiple scenarios.

---

## Scenario 1: University Research

**Tables and Keys:**

* **PROFESSOR(ProfID, ProfName, Department, City)**

  * Primary Key: `ProfID`
  * Nullable: `City`

* **RESEARCH\_PROJECT(ProjID, Title, Field, ProfID)**

  * Primary Key: `ProjID`
  * Nullable: `Field`

* **STUDENT(StudentID, Name, Level, City)**

  * Primary Key: `StudentID`
  * Nullable: `City`

* **ASSIGNMENT(ProjID, StudentID, HoursWorked, AssignDate)**

  * Primary Key: Composite (`ProjID`, `StudentID`)
  * Nullable: `AssignDate`

### Question 1: Students not working on Physics Department Projects

#### Explanation:

This query finds students who have never worked on a project supervised by a professor from the Physics department.

#### Corrected SQL:

```sql
SELECT StudentID
FROM STUDENT
WHERE StudentID NOT IN (
    SELECT A.StudentID
    FROM ASSIGNMENT A
    JOIN RESEARCH_PROJECT RP ON A.ProjID = RP.ProjID
    JOIN PROFESSOR P ON RP.ProfID = P.ProfID
    WHERE P.Department = 'Physics'
);
```

### Question 2: Professors with at least 5 high-hour projects in 2023

#### ✅ Situation 1: Each project can be handled by multiple professors

🔧 Schema Addition:

* **PROJECT\_PROFESSOR(ProjID, ProfID)** — composite primary key. Many-to-many relationship.

#### SQL:

```sql
SELECT ProfName
FROM PROFESSOR
WHERE ProfID IN (
    SELECT PP.ProfID
    FROM PROJECT_PROFESSOR PP
    JOIN (
        SELECT ProjID
        FROM ASSIGNMENT
        WHERE AssignDate BETWEEN '2023-01-01' AND '2023-12-31'
        GROUP BY ProjID
        HAVING SUM(HoursWorked) > 300
    ) AS QualifiedProjects ON PP.ProjID = QualifiedProjects.ProjID
    GROUP BY PP.ProfID
    HAVING COUNT(DISTINCT PP.ProjID) >= 5
);
```

#### ✅ Situation 2: Each project is handled by one professor

🔧 Schema:

* **RESEARCH\_PROJECT(ProjID, Title, Field, ProfID)** (each project has a single professor)

#### SQL:

```sql
SELECT ProfName
FROM PROFESSOR
WHERE ProfID IN (
    SELECT RP.ProfID
    FROM RESEARCH_PROJECT RP
    JOIN ASSIGNMENT A ON RP.ProjID = A.ProjID
    WHERE A.AssignDate BETWEEN '2023-01-01' AND '2023-12-31'
    GROUP BY RP.ProjID, RP.ProfID
    HAVING SUM(A.HoursWorked) > 300
)
GROUP BY ProfID
HAVING COUNT(*) >= 5;
```

---

## Additional Scenarios:

### Scenario 2: Music Festival Management

**Tables and Keys:**

* **ARTIST(ArtistID, ArtistName, Genre, Country)**

  * Primary Key: `ArtistID`
  * Nullable: `Genre`

* **FESTIVAL(FestID, Name, Location, Year)**

  * Primary Key: `FestID`
  * Nullable: None

* **PERFORMANCE(ArtistID, FestID, Duration, AudienceSize)**

  * Primary Key: Composite (`ArtistID`, `FestID`)
  * Nullable: `AudienceSize`

### Scenario 3: Airline Reservation System

**Tables and Keys:**

* **AIRLINE(AirlineID, AirlineName, Headquarters)**

  * Primary Key: `AirlineID`
  * Nullable: `Headquarters`

* **FLIGHT(FlightID, FlightNumber, AirlineID, DepartureCity, ArrivalCity)**

  * Primary Key: `FlightID`
  * Nullable: `FlightNumber`

* **PASSENGER(PassengerID, Name, Country)**

  * Primary Key: `PassengerID`
  * Nullable: `Country`

* **RESERVATION(FlightID, PassengerID, SeatNumber, BookingDate)**

  * Primary Key: Composite (`FlightID`, `PassengerID`)
  * Nullable: `SeatNumber`

### Scenario 4: Movie Streaming Platform

**Tables and Keys:**

* **PRODUCER(ProducerID, ProducerName, Country)**

  * Primary Key: `ProducerID`
  * Nullable: `Country`

* **MOVIE(MovieID, Title, Genre, ProducerID)**

  * Primary Key: `MovieID`
  * Nullable: `Genre`

* **USER(UserID, UserName, SubscriptionType)**

  * Primary Key: `UserID`
  * Nullable: `SubscriptionType`

* **WATCH\_HISTORY(UserID, MovieID, WatchDate, WatchMinutes)**

  * Primary Key: Composite (`UserID`, `MovieID`, `WatchDate`)
  * Nullable: `WatchMinutes`

### Scenario 5: Online Retail System

**Tables and Keys:**

* **SUPPLIER(SupplierID, SupplierName, Country)**

  * Primary Key: `SupplierID`
  * Nullable: `Country`

* **PRODUCT(ProductID, ProductName, Category, SupplierID)**

  * Primary Key: `ProductID`
  * Nullable: `Category`

* **CUSTOMER(CustomerID, CustomerName, City)**

  * Primary Key: `CustomerID`
  * Nullable: `City`

* **ORDER(OrderID, ProductID, CustomerID, Quantity, OrderDate)**

  * Primary Key: `OrderID`
  * Nullable: `OrderDate`

---

End of schema notes and query explanations.
