<!-- Updated on 2025-03-24: Submission for BookMyShow Database Design -->

mysql> USE BookMyShowDB;
Database changed
mysql> CREATE TABLE Theatre (
 -> theatre_id INT PRIMARY KEY AUTO_INCREMENT,
 -> name VARCHAR(255) NOT NULL,
 -> location VARCHAR(255) NOT NULL
 -> );
Query OK, 0 rows affected (0.11 sec)
mysql> CREATE TABLE Screen (
 -> screen_id INT PRIMARY KEY AUTO_INCREMENT,
 -> theatre_id INT,
 -> screen_number INT NOT NULL,
 -> FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id)
 -> );
Query OK, 0 rows affected (0.08 sec)
mysql> CREATE TABLE Movie (
 -> movie_id INT PRIMARY KEY AUTO_INCREMENT,
 -> name VARCHAR(255) NOT NULL,
 -> language VARCHAR(50) NOT NULL,
 -> format VARCHAR(50) NOT NULL
 -> );
Query OK, 0 rows affected (0.04 sec)
mysql> CREATE TABLE `Show` (
 -> show_id INT PRIMARY KEY AUTO_INCREMENT,
 -> movie_id INT,
 -> screen_id INT,
 -> show_date DATE NOT NULL,
 -> show_time TIME NOT NULL,
 -> FOREIGN KEY (movie_id) REFERENCES Movie(movie_id),
 -> FOREIGN KEY (screen_id) REFERENCES Screen(screen_id)
 -> );
Query OK, 0 rows affected (0.07 sec)
mysql> INSERT INTO Theatre (name, location) VALUES ('PVR Nexus', 'Bangalore');
Query OK, 1 row affected (0.03 sec)
mysql> INSERT INTO Screen (theatre_id, screen_number) VALUES (1, 1), (1, 2);
Query OK, 2 rows affected (0.01 sec)
Records: 2 Duplicates: 0 Warnings: 0
mysql> INSERT INTO Movie (name, language, format) VALUES
 -> ('Dasara', 'Telugu', '2D'),
 -> ('Kisi Ka Bhai Kisi Ki Jaan', 'Hindi', '2D'),
 -> ('Tu Jhoothi Main Makkaar', 'Hindi', '2D'),
 -> ('Avatar: The Way of Water', 'English', '3D');
Query OK, 4 rows affected (0.01 sec)
Records: 4 Duplicates: 0 Warnings: 0
mysql> INSERT INTO `SHOW` (movie_id, screen_id, show_date, show_time) VALUES
 -> (1, 1, '2025-04-25', '12:15:00'),
 -> (2, 1, '2025-04-25', '13:00:00'),
 -> (2, 1, '2025-04-25', '16:10:00'),
 -> (2, 1, '2025-04-25', '18:20:00'),
 -> (2, 2, '2025-04-25', '19:20:00'),
 -> (3, 1, '2025-04-25', '21:15:00'),
 -> (4, 2, '2025-04-25', '19:20:00');
Query OK, 7 rows affected (0.01 sec)
Records: 7 Duplicates: 0 Warnings: 0
mysql> SELECT m.name AS movie_name, m.language, m.format, s.screen_number, 
sh.show_time
 -> FROM `SHOW` sh
 -> JOIN Screen s ON sh.screen_id = s.screen_id
 -> JOIN Theatre t ON s.theatre_id = t.theatre_id
 -> JOIN Movie m ON sh.movie_id = m.movie_id
 -> WHERE t.name = 'PVR Nexus' AND sh.show_date = '2025-04-25'
 -> ORDER BY sh.show_time;
+---------------------------+----------+--------+---------------+-----------+
| movie_name | language | format | screen_number | show_time |
+---------------------------+----------+--------+---------------+-----------+
| Dasara | Telugu | 2D | 1 | 12:15:00 |
| Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | 1 | 13:00:00 |
| Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | 1 | 16:10:00 |
| Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | 1 | 18:20:00 |
| Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | 2 | 19:20:00 |
| Avatar: The Way of Water | English | 3D | 2 | 19:20:00 |
| Tu Jhoothi Main Makkaar | Hindi | 2D | 1 | 21:15:00 |
+---------------------------+----------+--------+---------------+-----------+
7 rows in set (0.03 sec)