# DBMS Query Scenarios - Full Overview

This document includes five different DBMS query scenarios. Each scenario provides a unique relational schema, two query-based questions, and the primary keys and nullable (optional) attributes for each table.

---

## Scenario 1: University Research

**Schema:**

* **PROFESSOR(ProfID, ProfName, Department, City)**
* **RESEARCH\_PROJECT(ProjID, Title, Field, ProfID)**
* **STUDENT(StudentID, Name, Level, City)**
* **ASSIGNMENT(ProjID, StudentID, HoursWorked, AssignDate)**

**Primary Keys and Nullable Attributes:**

* `PROFESSOR`: Primary Key: `ProfID`, Nullable: `City`
* `RESEARCH_PROJECT`: Primary Key: `ProjID`, Nullable: `Field`
* `STUDENT`: Primary Key: `StudentID`, Nullable: `City`
* `ASSIGNMENT`: Primary Key: (`ProjID`, `StudentID`), Nullable: `AssignDate`

**Questions:**

1. Find the names of students who have never worked on a project supervised by professors from the 'Physics' department.
2. Find the names of professors who supervised at least 5 different research projects in 2023 where students contributed more than 300 total hours.

---

## Scenario 2: Music Festival Management

**Schema:**

* **ARTIST(ArtistID, ArtistName, Genre, Country)**
* **FESTIVAL(FestID, Name, Location, Year)**
* **PERFORMANCE(ArtistID, FestID, Duration, AudienceSize)**

**Primary Keys and Nullable Attributes:**

* `ARTIST`: Primary Key: `ArtistID`, Nullable: `Genre`
* `FESTIVAL`: Primary Key: `FestID`, Nullable: None
* `PERFORMANCE`: Primary Key: (`ArtistID`, `FestID`), Nullable: `AudienceSize`

**Questions:**

1. Find the names of festivals where no rock artists have performed.
2. Find the names of artists who performed at least 3 different festivals in 2024 with a total audience size exceeding 50,000.

---

## Scenario 3: Airline Reservation System

**Schema:**

* **AIRLINE(AirlineID, AirlineName, Headquarters)**
* **FLIGHT(FlightID, FlightNumber, AirlineID, DepartureCity, ArrivalCity)**
* **PASSENGER(PassengerID, Name, Country)**
* **RESERVATION(FlightID, PassengerID, SeatNumber, BookingDate)**

**Primary Keys and Nullable Attributes:**

* `AIRLINE`: Primary Key: `AirlineID`, Nullable: `Headquarters`
* `FLIGHT`: Primary Key: `FlightID`, Nullable: `FlightNumber`
* `PASSENGER`: Primary Key: `PassengerID`, Nullable: `Country`
* `RESERVATION`: Primary Key: (`FlightID`, `PassengerID`), Nullable: `SeatNumber`

**Questions:**

1. Find the names of passengers who have never flown with airlines based in France.
2. Find airlines that operated at least 15 different flights in 2023 between 'New York' and 'London'.

---

## Scenario 4: Movie Streaming Platform

**Schema:**

* **PRODUCER(ProducerID, ProducerName, Country)**
* **MOVIE(MovieID, Title, Genre, ProducerID)**
* **USER(UserID, UserName, SubscriptionType)**
* **WATCH\_HISTORY(UserID, MovieID, WatchDate, WatchMinutes)**

**Primary Keys and Nullable Attributes:**

* `PRODUCER`: Primary Key: `ProducerID`, Nullable: `Country`
* `MOVIE`: Primary Key: `MovieID`, Nullable: `Genre`
* `USER`: Primary Key: `UserID`, Nullable: `SubscriptionType`
* `WATCH_HISTORY`: Primary Key: (`UserID`, `MovieID`, `WatchDate`), Nullable: `WatchMinutes`

**Questions:**

1. Find the names of users who have never watched a movie produced in 'Japan'.
2. Find producers who had at least 8 different movies watched for more than 1,000 total minutes each in 2022.

---

## Scenario 5: Online Retail System

**Schema:**

* **SUPPLIER(SupplierID, SupplierName, Country)**
* **PRODUCT(ProductID, ProductName, Category, SupplierID)**
* **CUSTOMER(CustomerID, CustomerName, City)**
* **ORDER(OrderID, ProductID, CustomerID, Quantity, OrderDate)**

**Primary Keys and Nullable Attributes:**

* `SUPPLIER`: Primary Key: `SupplierID`, Nullable: `Country`
* `PRODUCT`: Primary Key: `ProductID`, Nullable: `Category`
* `CUSTOMER`: Primary Key: `CustomerID`, Nullable: `City`
* `ORDER`: Primary Key: `OrderID`, Nullable: `OrderDate`

**Questions:**

1. Find the names of customers who have never ordered products from suppliers based in 'Germany'.
2. Find supplier names who sold at least 10 different products with more than 500 total units ordered in 2023.

---

End of document.
