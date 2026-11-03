# 🗄️ SQL Filtering for Security Investigations

**Platform:** MariaDB shell (provided by the Google Cybersecurity Professional Certificate)
**Skills demonstrated:**
- Running SQL queries to retrieve information from a database
- Applying `AND`, `OR`, and `NOT` operators to filter SQL queries

---

## 📌 Project Description

As a cybersecurity trainee, I was responsible for filtering the information my team needed from the organization's database. The team needed data to investigate potential security issues and to update employee computers.

Using SQL in the MariaDB shell, I retrieved data from two tables:

- **`log_in_attempts`**: `event_id`, `username`, `login_date`, `login_time`, `country`, `ip_address`, `success`
- **`employees`**: `employee_id`, `device_id`, `username`, `department`, `office`

Through this project I:
- Retrieved failed login attempts made after business hours and logins on specific dates, which helps flag suspicious activity.
- Identified login attempts that originated outside of Mexico.
- Located employees by department and office to support team-wide computer updates.
- Combined `AND`, `OR`, `NOT`, and the `LIKE` wildcard to narrow results to exactly the records needed.

---

## 1️⃣ Retrieve After Hours Failed Login Attempts

My team was investigating failed login attempts made after business hours, so I retrieved this information from the login activity data.

- The `login_time` column in `log_in_attempts` records when each attempt was made. Office hours end at `18:00`, so any attempt after that is after hours.
- The `success` column stores `TRUE` or `FALSE`. MySQL/MariaDB stores Boolean values as `1` for `TRUE` and `0` for `FALSE`, so a failed attempt is `success = FALSE` (`0`).
- I used the `AND` operator so that a record is returned only when **both** conditions are true.

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = FALSE;
```
<img width="873" height="597" alt="task1-after-hours-failed-logins" src="https://github.com/user-attachments/assets/11023d3d-e170-4e50-ae8e-90bd7cb3ddc9" />

---

## 2️⃣ Retrieve Login Attempts on Specific Dates

My team was investigating a suspicious event that occurred on `2022-05-09`. I retrieved all login attempts from that day and the day before (`2022-05-08`).

- The `login_date` column in `log_in_attempts` records the date of each attempt.
- I used the `OR` operator because a record only needs to match **one** of the two dates.

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```
<img width="883" height="687" alt="task2-specific-dates" src="https://github.com/user-attachments/assets/9ee33ba5-766d-4b93-b83d-3597a10456b2" />

---

## 3️⃣ Retrieve Login Attempts Outside of Mexico

My team was investigating logins that did not originate in Mexico.

- The `country` column contains both `MEX` and `MEXICO` for Mexican logins, so filtering on one exact value would have missed some records.
- I used the `LIKE` operator with the `%` wildcard (`'MEX%'`) to match both values, then added `NOT` to exclude them.

```sql
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```
<img width="888" height="697" alt="task3-outside-mexico" src="https://github.com/user-attachments/assets/e1cd6ca0-51cd-47a6-b34a-cf598b2a418f" />

---

## 4️⃣ Retrieve Employees in Marketing

My team was updating employee machines, and I needed the Marketing employees located in the East building.

First, I ran this query to view the columns and values in the `employees` table, especially `department` and `office`:

```sql
SELECT *
FROM employees;
```
<img width="878" height="643" alt="task4a-employees-table" src="https://github.com/user-attachments/assets/b60b90da-82ea-4eba-a776-dc09a3f15330" />

Next, I filtered for employees in the `Marketing` department whose office is in the East building (such as `East-170` or `East-320`). I used `AND` so both conditions must be true, and `LIKE 'East%'` to match every East office.

```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```
<img width="751" height="355" alt="task4b-marketing-east" src="https://github.com/user-attachments/assets/b79172a2-c8c2-4a2f-89e6-d2f63e11a5ea" />

---

## 5️⃣ Retrieve Employees in Finance or Sales

My team needed to perform a different update on the computers of all employees in the Finance or Sales department. I used `OR` because an employee only needs to belong to one of the two departments.

```sql
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';
```
<img width="823" height="686" alt="task5-finance-or-sales" src="https://github.com/user-attachments/assets/81c92174-0bb3-4ded-80e3-f6d4dc50f82c" />

---

## 6️⃣ Retrieve All Employees Not in IT

My team needed to make one more update that had already been applied to the Information Technology department, so they needed the employees **not** in that department. I used the `NOT` operator to exclude them.

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```
<img width="823" height="690" alt="task6-not-it" src="https://github.com/user-attachments/assets/b794fe24-23e6-414b-8c35-1c39e99f5a79" />

---

## ✅ Summary

In this project I used SQL in the MariaDB shell to filter the organization's `log_in_attempts` and `employees` tables for two purposes: 
1. Supporting security investigations and
2. Preparing computer updates.

- I used **`AND`** to combine conditions that must both be true, such as failed logins after 18:00 and Marketing employees in the East building.
- I used **`OR`** to match records that satisfy either condition, such as logins on two dates or employees in Finance or Sales.
- I used **`NOT`** to exclude records, such as logins from Mexico and employees in Information Technology.
- I used **`LIKE`** with the `%` wildcard to match patterns like `MEX%` and `East%`, which handled inconsistent values in the data.

These queries showed me how precise filtering turns a large database into the specific information a security team needs, and how small details like Boolean values (`1`/`0`) and inconsistent country entries affect the accuracy of results. This is a skill I can apply to log analysis and security investigations.
