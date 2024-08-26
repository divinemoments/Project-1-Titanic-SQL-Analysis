# Project 2: Titanic SQL Analysis (SQL)

### Dataset:
Titanic Dataset

### Objective:
Perform SQL analysis to extract insights about passenger demographics, survival rates, and other relevant statistics.

### Description:
One slow Saturday morning, I then decided to sharpen my SQL skills by diving into the Titanic dataset. Over the course of a couple of hours, I worked on analyzing the data to uncover meaningful insights. This project showcases my proficiency in SQL through tasks such as data manipulation, aggregation, and reporting.

### SQL Tool:
Microsoft SQL Server

#

### Clean the data
Will not delete rows with NULL rather define the details better

STEP 1: Add Columns

```sql
ALTER TABLE titanic
ADD PassengerClass VARCHAR(10),
    SurvivalStatus VARCHAR(15),
    EmbarkedLocation VARCHAR(15);
```

STEP 2: Define the data in the pclass, survived, embarked


```sql
UPDATE titanic
SET PassengerClass = CASE
    WHEN pclass = 1 THEN 'First'
    WHEN pclass = 2 THEN 'Middle'
    WHEN pclass = 3 THEN 'Third'
END;

UPDATE titanic
SET SurvivalStatus = CASE
    WHEN survived = 'True' THEN 'Survived'
    WHEN survived = 'False' THEN 'Did not Survive'
END;

UPDATE titanic
SET EmbarkedLocation = CASE
    WHEN embarked = 'C' THEN 'Cherbourg'
    WHEN embarked = 'Q' THEN 'Queenstown'
    WHEN embarked = 'S' THEN 'Southampton'
END;
```

### 1. Find the Total Number of Passengers.

```sql
SELECT COUNT(*)
FROM titanic;
```

### 2. List the Names and Ages of Passengers Who Were Younger Than 18.

```sql
SELECT name, age
FROM titanic
WHERE age<18;
```

### 3. Count the Number of Passengers Who Embarked at Each Port (Embarked).

```sql
SELECT EmbarkedLocation, COUNT(*) AS PassengerCount
FROM titanic
WHERE EmbarkedLocation IS NOT NULL
GROUP BY EmbarkedLocation
ORDER BY PassengerCount DESC;
```

### 4. List the Names of All Passengers Who Survived.

```sql
SELECT name
FROM titanic
WHERE survived='True';
```

### 5. Get the Maximum Age of Passengers in Each Class (Pclass).

```sql
SELECT PassengerClass, MAX(age) AS MaxAge
FROM titanic
GROUP BY PassengerClass
ORDER BY PassengerClass;
```

### 6. Find the Passenger Who Had the Highest Fare and List Their Details.

```sql
SELECT TOP 1 *
FROM titanic
ORDER BY fare DESC;
```

### 7. Calculate the Average Age of Passengers.

```sql
SELECT ROUND(AVG(age),2) AS AvgAge
FROM titanic;
```

### 8. Find the Most Common Embarked Port.

```sql
SELECT TOP 1 EmbarkedLocation, COUNT(*) AS EmbarkedCount
FROM titanic
GROUP BY EmbarkedLocation
ORDER BY EmbarkedCount DESC;
```

### 9. List the Names and Ticket Numbers of All Passengers Who Had a Ticket Number Starting with 'A'.

```sql
SELECT name, ticket
FROM titanic
WHERE ticket LIKE 'A%';
```

