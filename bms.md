````md
# Problem Solving Case: BookMyShow

## Table of Contents
- [P1](#p1)
  - [Description](#p1-description)
  - [Solution](#p1-solution)
- [P2](#p2)
  - [Description](#p2-description)
  - [Solution](#p2-solution)

---

## P1

### P1 Description

As part of this assignment, we need to list down all the entities, their attributes, and the table structures for the scenario.  
We also need to write the SQL queries required to create these tables along with a few sample entries.  

Ensure the tables follow:
- 1NF (First Normal Form)
- 2NF (Second Normal Form)
- 3NF (Third Normal Form)
- BCNF (Boyce–Codd Normal Form)

---

### P1 Solution

### Tables

```sql
CREATE TABLE bms_theatres (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    city VARCHAR(100) NOT NULL,
    UNIQUE KEY uk_theatre_name_city (name, city)
);

CREATE TABLE bms_screens (
    id INT AUTO_INCREMENT PRIMARY KEY,
    theatre_id INT NOT NULL,
    name VARCHAR(50) NOT NULL,
    capacity INT NOT NULL,
    UNIQUE KEY uk_theatre_screen (theatre_id, name),
    FOREIGN KEY (theatre_id) REFERENCES bms_theatres(id)
);

CREATE TABLE bms_movies (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    language VARCHAR(50) NOT NULL,
    duration_minutes INT NOT NULL
);

CREATE TABLE bms_shows (
    id INT AUTO_INCREMENT PRIMARY KEY,
    screen_id INT NOT NULL,
    movie_id INT NOT NULL,
    show_date DATE NOT NULL,
    start_time TIME NOT NULL,
    UNIQUE KEY uk_screen_slot (screen_id, show_date, start_time),
    FOREIGN KEY (screen_id) REFERENCES bms_screens(id),
    FOREIGN KEY (movie_id) REFERENCES bms_movies(id)
);
````

### Sample Data

```sql
INSERT INTO bms_theatres (name, city)
VALUES ('PVR Forum Mall', 'Bangalore');

INSERT INTO bms_screens (theatre_id, name, capacity)
VALUES
(1, 'Screen 1', 150),
(1, 'Screen 2', 200);

INSERT INTO bms_movies (title, language, duration_minutes)
VALUES
('Inception', 'English', 148),
('Interstellar', 'English', 169);

INSERT INTO bms_shows (screen_id, movie_id, show_date, start_time)
VALUES
(1, 1, '2026-01-20', '10:00:00'),
(1, 1, '2026-01-20', '18:00:00'),
(2, 2, '2026-01-20', '14:00:00'),
(2, 2, '2026-01-21', '11:00:00');
```

---

## P2

### P2 Description

Write a query to list down all the shows on a given date at a given theatre along with their respective show timings.

---

### P2 Solution

```sql
SELECT
    m.title AS movie_title,
    s.show_date,
    s.start_time,
    sc.name AS screen_name
FROM bms_shows s
JOIN bms_screens sc ON sc.id = s.screen_id
JOIN bms_theatres t ON t.id = sc.theatre_id
JOIN bms_movies m ON m.id = s.movie_id
WHERE
    t.id = <theatre_id>
    AND s.show_date = '<date>'
ORDER BY s.start_time;
```

Example usage:

```sql
SELECT
    m.title AS movie_title,
    s.show_date,
    s.start_time,
    sc.name AS screen_name
FROM bms_shows s
JOIN bms_screens sc ON sc.id = s.screen_id
JOIN bms_theatres t ON t.id = sc.theatre_id
JOIN bms_movies m ON m.id = s.movie_id
WHERE
    t.id = 1
    AND s.show_date = '2026-01-20'
ORDER BY s.start_time;
```

```
```
