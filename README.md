# Ticket-Booking-System

This project represents a basic ticketing platform with theatres, movies, and shows. It includes MySQL-compatible SQL for table creation, data insertion, and sample queries.

---

## Tables and Attributes

### 1. `theatre`
| Column       | Type         | Description         |
|--------------|--------------|---------------------|
| theatreId    | INT          | Primary key         |
| name         | VARCHAR(30)  | Theatre name        |
| address      | VARCHAR(200) | Theatre address     |

### 2. `movie`
| Column       | Type         | Description         |
|--------------|--------------|---------------------|
| movieId      | INT          | Primary key         |
| title        | VARCHAR(100) | Movie title         |
| release_date | DATE         | Release date        |
| language     | VARCHAR(20)  | Movie language      |
| rating       | DOUBLE(4,2)  | IMDb-style rating   |
| duration     | TIME         | Duration of movie   |

### 3. `movieShow`
| Column        | Type | Description                            |
|---------------|------|----------------------------------------|
| showId        | INT  | Primary key                            |
| theatreId     | INT  | Foreign key to `theatre`               |
| movieId       | INT  | Foreign key to `movie`                 |
| showDate      | DATE | Date of the show                       |
| showTime      | TIME | Time of the show                       |
| availableSeats| INT  | Number of available seats              |

---
## Table Creation

### `theatre`
CREATE TABLE theatre(
theatreId INT PRIMARY KEY,
name VARCHAR(30) NOT NULL,
address VARCHAR(200) NOT NULL
);

### `movie`
CREATE TABLE movie(
movieId INT PRIMARY KEY,
title VARCHAR(100) NOT NULL,
release_date DATE NOT NULL,
language VARCHAR(20) NOT NULL,
rating DOUBLE(4, 2) NOT NULL,
duration TIME NOT NULL
);

### `movieShow`
CREATE TABLE movieShow(
showId INT primary key,
theatreId INT REFERENCES theatre(theatreId),
movieId INT REFERENCES movie(movieId),
showDate DATE NOT NULL,
showTime TIME NOT NULL,
availableSeats INT NOT NULL
);

---

## Sample Data

### `theatre`
INSERT INTO theatre(theatreId, name, address) VALUES 
(1, "PVR: City Centre", "Guwahati"),
(2, "Cinepolis: Central Mall", "Guwahati"),
(3, "INOX: NCS Square", "Guwahati"),
(4, "INOX: Aurus", "Guwahati");

### `movie`
INSERT INTO movie(movieId, title, release_date, language, rating, duration) VALUES 
(1, "Mission: Impossible", "2025-05-15", "English", 8.8, "02:49:00"),
(2, "Final Destination", "2025-05-10", "English", 8.6, "01:49:00"),
(3, "Raid 2", "2025-04-11", "Hindi", 8.3, "02:19:00"),
(4, "Shinchan: Our Dinosaur Diary", "2025-04-28", "English", 8.9, "01:46:00");

### `movieShow`
INSERT INTO movieShow(showId, theatreId, movieId, showDate, showTime, availableSeats) VALUES 
(1, 1, 1, "2025-05-25", "11:00:00", 30),
(2, 1, 2, "2025-05-25", "14:00:00", 20),
(3, 1, 4, "2025-05-25", "19:00:00", 20),
(4, 2, 2, "2025-05-25", "09:00:00", 20),
(5, 2, 3, "2025-04-26", "11:00:00", 30),
(6, 3, 1, "2025-05-25", "09:00:00", 20),
(7, 3, 4, "2025-04-26", "13:00:00", 30),
(8, 4, 2, "2025-05-25", "09:00:00", 20),
(9, 4, 3, "2025-04-25", "13:00:00", 30);

---

### P1: Get list of all shows running in theatres with show timings on a selected date
SELECT name AS theatreName, title AS movieTitle, showTime 
FROM movieShow MS  
JOIN theatre T ON T.theatreId = MS.theatreId 
JOIN movie M ON M.movieId = MS.movieId 
WHERE showDate = '2025-05-25';

### P2: List all shows at a given theatre on a selected date with their timings
SELECT title AS movieTitle, showTime 
FROM movieShow MS  
JOIN movie M ON M.movieId = MS.movieId 
WHERE showDate = '2025-05-25' AND theatreId = 1;

### How to Run

- Copy the table creation and insertion queries into a MySQL-compatible DB like phpMyAdmin or MySQL CLI.
- Run the SELECT queries directly to see the expected output.
- Modify dates or theatre IDs to test further.