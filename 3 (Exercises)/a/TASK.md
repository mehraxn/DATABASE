# 5 Advanced SQL Database Questions

## Question 1: Online Retail Database

### Schema:
```
CUSTOMER(CustID, Name, Email, Country, JoinDate)
PRODUCT(ProdID, Name, Category, Price, StockQuantity)
ORDER(OrderID, CustID, OrderDate, TotalAmount, Status)
ORDER_DETAIL(OrderID, ProdID, Quantity, UnitPrice)
SUPPLIER(SupplierID, Name, Country, Rating)
PRODUCT_SUPPLIER(ProdID, SupplierID, SupplyDate, Quantity)
```

### Scenario:
You are working with an online retail database that tracks customers, products, orders, and suppliers. Each product can be sourced from multiple suppliers, and supplier performance is tracked with ratings.

### Question:
Find the names of customers who have never ordered any products supplied by suppliers with a rating lower than 4. Display the results ordered by customer join date.

## Question 2: Hospital Management System

### Schema:
```
DOCTOR(DocID, Name, Specialization, YearsExperience, DepartmentID)
PATIENT(PatientID, Name, DateOfBirth, BloodType, InsuranceID)
DEPARTMENT(DepartmentID, Name, Location, HeadDocID)
APPOINTMENT(AppID, PatientID, DocID, AppDate, AppTime, Status)
MEDICATION(MedID, Name, Category, ManufacturerID)
PRESCRIPTION(PresID, AppID, MedID, Dosage, Duration)
MANUFACTURER(ManufacturerID, Name, Country, Certification)
```

### Scenario:
You are working with a hospital management system database that tracks doctors, patients, appointments, medications, and prescriptions. The hospital wants to analyze prescription patterns and doctor-patient interactions.

### Question:
Find the departments where no patients have been prescribed medications from manufacturers based in India during the last month. List the department names and their respective head doctors' names.

## Question 3: University Academic System

### Schema:
```
STUDENT(StudentID, Name, DoB, Major, EnrollmentYear, AdvisorID)
PROFESSOR(ProfID, Name, Department, Rank, HireDate)
COURSE(CourseID, Title, Department, Credits, ProfID)
ENROLLMENT(StudentID, CourseID, Semester, Year, Grade)
ASSIGNMENT(AssignmentID, CourseID, Title, DueDate, MaxScore)
SUBMISSION(SubmissionID, AssignmentID, StudentID, SubmissionDate, Score)
DEPARTMENT(DeptID, Name, Building, Budget, ChairProfID)
```

### Scenario:
You are working with a university database that tracks students, professors, courses, enrollments, assignments, and submissions. The university is conducting an academic audit.

### Question:
Find the names of professors who have never taught a course in which all enrolled students submitted all assignments on or before their due dates. Return the professor names along with their departments.

## Question 4: Movie Streaming Platform

### Schema:
```
USER(UserID, Username, Email, CountryCode, SubscriptionType, JoinDate)
MOVIE(MovieID, Title, ReleaseYear, Genre, Director, Runtime, Rating)
ACTOR(ActorID, Name, Nationality, BirthYear)
MOVIE_ACTOR(MovieID, ActorID, Role)
VIEWING(ViewID, UserID, MovieID, ViewDate, WatchPercentage, DeviceType)
REVIEW(ReviewID, UserID, MovieID, Rating, ReviewText, ReviewDate)
COUNTRY(CountryCode, Name, Region, Language)
```

### Scenario:
You are working with a movie streaming platform database that tracks users, movies, actors, viewing history, and reviews. The platform is analyzing user engagement patterns.

### Question:
Find the names of users who have watched at least 90% of all movies featuring actors from Japan but have never reviewed any movie with a rating below 7. Display the results ordered by the number of Japanese actor movies they've watched in descending order.

## Question 5: E-Learning Platform

### Schema:
```
LEARNER(LearnerID, Name, Email, Country, RegisterDate)
INSTRUCTOR(InstructorID, Name, Biography, Rating, JoinDate)
COURSE(CourseID, Title, Category, InstructorID, Price, DifficultyLevel)
ENROLLMENT(EnrollmentID, LearnerID, CourseID, EnrollDate, CompletionStatus)
MODULE(ModuleID, CourseID, Title, Duration)
QUIZ(QuizID, ModuleID, Title, PassingScore)
QUIZ_ATTEMPT(AttemptID, LearnerID, QuizID, AttemptDate, Score)
CERTIFICATE(CertificateID, LearnerID, CourseID, IssueDate, ExpiryDate)
```

### Scenario:
You are working with an e-learning platform database that tracks learners, instructors, courses, enrollments, modules, quizzes, and certificates. The platform wants to analyze learning patterns and certification rates.

### Question:
Find the names of learners who have enrolled in courses from all categories offered by instructors with ratings above 4.5, but have not yet received certificates for any courses in the "Data Science" category. List the learner names along with the number of pending certifications they have.