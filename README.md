# Project 1: Titanic SQL Analysis
## Skill: SQL
## Difficulty: ★☆☆☆☆


### Dataset:
Titanic Dataset

### Objective:
Perform SQL analysis to extract insights about passenger demographics, survival rates, and other relevant statistics.

### Description:
One slow Saturday morning, I then decided to sharpen my SQL skills by diving into the Titanic dataset. For an hour or so, I worked on analyzing the data to uncover meaningful insights. This project showcases my proficiency in SQL through tasks such as data manipulation, aggregation, and reporting.

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

### 10. Get the Distribution of Passengers by Class and Gender.

```sql
SELECT
	PassengerClass,
	Sex,
	COUNT(*) AS PassengerCount
FROM titanic
GROUP BY
	PassengerClass,
	Sex
ORDER BY
	PassengerClass,
	Sex;
```

### 11. Find the Number of Passengers Who Had a Missing Age Value.

```sql
SELECT COUNT(*) AS PassengerCount
FROM titanic
WHERE Age IS NULL;
```

### 12. Determine the Survival Rate (Percentage) for Each Class (Pclass).
STEP 1. Define the table: pclass, survived, total

```sql
SELECT
	PassengerClass,
	SUM(CASE WHEN survived = 'True' THEN 1 ELSE 0 END) AS SurvivedPassenger,
	COUNT(*) AS TotalPassenger
FROM titanic
GROUP BY PassengerClass;
```

STEP 2. Calculate the survival rate

```sql
WITH SurvivalStat AS(
SELECT
	PassengerClass,
	SUM(CASE WHEN survived = 'True' THEN 1 ELSE 0 END) AS SurvivedPassenger,
	COUNT(*) AS TotalPassenger
FROM titanic
GROUP BY PassengerClass
)

SELECT
	PassengerClass,
	ROUND((CAST(SurvivedPassenger AS FLOAT) / TotalPassenger) * 100,2) AS SurvivalRate
FROM SurvivalStat;
```

### 13. Find All the Passengers with the Same Name and Count How Many There Are.

```sql
SELECT
	name,
	COUNT(*) AS PassengerCount
FROM titanic
GROUP BY name
HAVING Count(*) > 1
ORDER BY PassengerCount DESC;
```

### 14. List the Names and Ages of Passengers Who Were in the Same Cabin (Cabin) as Someone Who Did Not Survive.
Self-join Approach

```sql
SELECT DISTINCT t1.name, t1.age
FROM titanic t1
JOIN titanic t2 ON t1.cabin = t2.cabin
WHERE t2.survived = 0
  AND t1.survived = 1;
```

CTE (Common Table Expression) Query Approach.
*I am more comfortable with this, though it is longer. Need to practice self-join more.*

STEP 1. Did not survive Cabin list

```sql
SELECT DISTINCT cabin
FROM titanic
WHERE
	survived=0
	AND cabin IS NOT NULL;
```

STEP 2. Get the survived passengers of STEP 1 list

```sql
WITH CabinDidnotSurvive AS(
	SELECT DISTINCT cabin
	FROM titanic
	WHERE
		survived=0
		AND cabin IS NOT NULL
)

SELECT
	name,
	age
FROM titanic
WHERE
	cabin IN(SELECT cabin FROM cabindidnotsurvive)
	AND survived=1;
```

### 15. Calculate the Average Fare Paid by Passengers in Each Port (Embarked).

```sql
SELECT EmbarkedLocation, ROUND(AVG(fare),2) AS AvgFare
FROM titanic
WHERE EmbarkedLocation IS NOT NULL
GROUP BY EmbarkedLocation;
```

### 16. Find the Number of Passengers Who Survived and Had Siblings or Spouses on Board (SibSp > 0).

```sql
SELECT COUNT(*) AS PassengerCount
FROM titanic
WHERE
	sibsp > 1
	AND survived = 1;
```

### 17. List the Top 5 Most Expensive Tickets and List the Details of the Passengers Who Purchased Them.

```sql
SELECT TOP 5 *
FROM titanic
ORDER BY fare DESC;
```

### 18. Get the Distribution of Survivors by Gender (Sex).

```sql
SELECT sex, COUNT (*) AS PassengerCount
FROM titanic
WHERE survived = 'true'
GROUP BY sex;
```

### 19. Find the Oldest Passenger.

```sql
SELECT TOP 1 name, age
FROM titanic
ORDER BY age DESC;
```

### 20. List the Details of the Oldest and Youngest Passengers.

```sql
SELECT *
FROM Titanic
WHERE
	Age = (SELECT MIN(age) FROM titanic)
	OR Age = (SELECT MAX(age) FROM titanic);
```


### End
*What a rewarding way to spend a Saturday morning! This SQL exercise not only allowed me to refine my skills but also gave me a chance to delve deep into the Titanic dataset, uncovering valuable insights along the way. It's amazing how much you can learn and achieve in just a couple of hours when you're passionate about data analysis. I hope you find this project as enjoyable and insightful as I did!*
