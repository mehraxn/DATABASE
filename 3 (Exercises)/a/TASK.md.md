# 5 Advanced SQL Database Questions

## Question 1: Online Retail Database

### Schema:
```
CUSTOMER(<u>CustID</u>, Name, Email, Country, JoinDate)
PRODUCT(<u>ProdID</u>, Name, Category, Price, StockQuantity)
ORDER(<u>OrderID</u>, CustID, OrderDate, TotalAmount, Status)
ORDER_DETAIL(<u>OrderID, ProdID</u>, Quantity, UnitPrice)
SUPPLIER(<u>SupplierID</u>, Name, Country, Rating)
PRODUCT_SUPPLIER(<u>ProdID, SupplierID</u>, SupplyDate, Quantity)
```

### Scenario:
You are working with an online retail database that tracks customers, products, orders, and suppliers. Each product can be sourced from multiple suppliers, and supplier performance is tracked with ratings.

### Question:
Find the names of customers who have never ordered any products supplied by suppliers with a rating lower than 4. Display the results ordered by customer join date.

## Question 2: Hospital Management System

### Schema:
```
DOCTOR(<u>DocID</u>, Name, Specialization, YearsExperience, DepartmentID)
PATIENT(<u>PatientID</u>, Name, DateOfBirth, BloodType, InsuranceID)
DEPARTMENT(<u>DepartmentID</u>, Name, Location, HeadDocID)
APPOINTMENT(<u>AppID</u>, PatientID, DocID, AppDate, AppTime, Status)
MEDICATION(<u>MedID</u>, Name, Category, ManufacturerID)
PRESCRIPTION(<u>PresID</u>, AppID, MedID, Dosage, Duration)
MANUFACTURER(<u>ManufacturerID</u>, Name, Country, Certification)
```

### Scenario:
You are working with a hospital management system database that tracks doctors, patients, appointments, medications, and prescriptions. The hospital wants to analyze prescription patterns and doctor-patient interactions.

### Question:
Find the departments where no patients have been prescribed medications from manufacturers based in India during the last month. List the department names and their respective head doctors' names.

## Question 3: University Academic System

### Schema:
```
STUDENT(<u>StudentID</u>, Name, DoB, Major, EnrollmentYear, AdvisorID)
PROFESSOR(<u>ProfID</u>, Name, Department, Rank, HireDate)
COURSE(<u>CourseID</u>, Title, Department, Credits, ProfID)
ENROLLMENT(<u>StudentID, CourseID, Semester, Year</u>, Grade)
ASSIGNMENT(<u>AssignmentID</u>, CourseID, Title, DueDate, MaxScore)
SUBMISSION(<u>SubmissionID</u>, AssignmentID, StudentID, SubmissionDate, Score)
DEPARTMENT(<u>DeptID</u>, Name, Building, Budget, ChairProfID)
```

### Scenario:
You are working with a university database that tracks students, professors, courses, enrollments, assignments, and submissions. The university is conducting an academic audit.

### Question:
Find the names of professors who have never taught a course in which all enrolled students submitted all assignments on or before their due dates. Return the professor names along with their departments.

## Question 4: Movie Streaming Platform

### Schema:
```
USER(<u>UserID</u>, Username, Email, CountryCode, SubscriptionType, JoinDate)
MOVIE(<u>MovieID</u>, Title, ReleaseYear, Genre, Director, Runtime, Rating)
ACTOR(<u>ActorID</u>, Name, Nationality, BirthYear)
MOVIE_ACTOR(<u>MovieID, ActorID</u>, Role)
VIEWING(<u>ViewID</u>, UserID, MovieID, ViewDate, WatchPercentage, DeviceType)
REVIEW(<u>ReviewID</u>, UserID, MovieID, Rating, ReviewText, ReviewDate)
COUNTRY(<u>CountryCode</u>, Name, Region, Language)
```

### Scenario:
You are working with a movie streaming platform database that tracks users, movies, actors, viewing history, and reviews. The platform is analyzing user engagement patterns.

### Question:
Find the names of users who have watched at least 90% of all movies featuring actors from Japan but have never reviewed any movie with a rating below 7. Display the results ordered by the number of Japanese actor movies they've watched in descending order.

## Question 5: E-Learning Platform

### Schema:
```
LEARNER(<u>LearnerID</u>, Name, Email, Country, RegisterDate)
INSTRUCTOR(<u>InstructorID</u>, Name, Biography, Rating, JoinDate)
COURSE(<u>CourseID</u>, Title, Category, InstructorID, Price, DifficultyLevel)
ENROLLMENT(<u>EnrollmentID</u>, LearnerID, CourseID, EnrollDate, CompletionStatus)
MODULE(<u>ModuleID</u>, CourseID, Title, Duration)
QUIZ(<u>QuizID</u>, ModuleID, Title, PassingScore)
QUIZ_ATTEMPT(<u>AttemptID</u>, LearnerID, QuizID, AttemptDate, Score)
CERTIFICATE(<u>CertificateID</u>, LearnerID, CourseID, IssueDate, ExpiryDate)
```

### Scenario:
You are working with an e-learning platform database that tracks learners, instructors, courses, enrollments, modules, quizzes, and certificates. The platform wants to analyze learning patterns and certification rates.

### Question:
Find the names of learners who have enrolled in courses from all categories offered by instructors with ratings above 4.5, but have not yet received certificates for any courses in the "Data Science" category. List the learner names along with the number of pending certifications they have.