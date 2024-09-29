# PAT_Task8
PAT_Task8 - MySQL

Q:
Using MySQL, design a database whose name is IMDB. Create proper MySQL tables, Primary Key, foreign Key, add data into MySQL tables and do the following as given below:
  1.	Movie should have multiple media(video or image)
  2.	Movie can belongs to multiple Genre
  3.	Movie can have multiple reviews and Reviews can belongs to a user.
  4.	Artist can have multiple skills.
  5.	Artist can perform multiple roles in a single film


Ans:
To start MySQL in CMD/shell terminal
type
mysql -u root -p
and hit enter

1. Table for Movies (Movie should have multiple media(video or image))

CREATE TABLE movie (movie_id INT AUTO_INCREMENT PRIMARY KEY, movie_title VARCHAR(50), movie_year INT(4), movie_language VARCHAR(15)) AUTO_INCREMENT=1001;
INSERT INTO movie(movie_title, movie_year, movie_language) VALUES ("Sector 36","2024","Hindi"),("Stree 2","2024","Hindi"), ("Tumbbad","2018","Hindi"),("Kill","2023","Hindi"),("Berlin","2023","English"),("Bad News","2024","Hindi");

CREATE TABLE media_types (
    media_type_id INT PRIMARY KEY AUTO_INCREMENT,
    media_type_name VARCHAR(50) NOT NULL
);

INSERT INTO media_types (media_type_name) VALUES
('Image'),
('Video');

CREATE TABLE media (
    media_id INT PRIMARY KEY AUTO_INCREMENT,
    movie_id INT,
    media_type_id INT,
    media_url VARCHAR(255) NOT NULL,
    FOREIGN KEY (movie_id) REFERENCES movie(movie_id) ON DELETE CASCADE,
    FOREIGN KEY (media_type_id) REFERENCES media_types(media_type_id) ON DELETE CASCADE
);

INSERT INTO media (movie_id, media_type_id, media_url) VALUES
(1001, 1, 'http://imdb.com/sector_36_image.jpg'), 
(1001, 2, 'http://imdb.com/sector_36_video.mp4'),
(1002, 1, 'http://imdb.com/stree_2_image.jpg'),
(1002, 2, 'http://imdb.com/stree_2_video.mp4'),
(1003, 1, 'http://imdb.com/tumbad_image.jpg'),
(1003, 2, 'http://imdb.com/tumbad_video.mp4'),
(1004, 1, 'http://imdb.com/kill_image.jpg'),
(1004, 2, 'http://imdb.com/kill_video.mp4'),
(1005, 1, 'http://imdb.com/berlin_image.jpg'),
(1005, 2, 'http://imdb.com/berlin_video.mp4'),
(1006, 1, 'http://imdb.com/bad_news_image.jpg'),
(1006, 2, 'http://imdb.com/bad_news_video.mp4');


Query to movie media 

SELECT
    m.movie_title,
    mt.media_type_name,
    md.media_url
FROM
    movie m
JOIN
    media md ON m.movie_id = md.movie_id
JOIN
    media_types mt ON md.media_type_id = mt.media_type_id
ORDER BY
    m.movie_title, mt.media_type_name;

*************************************************************************************************
2. Movie can belongs to multiple Genre

CREATE TABLE genre (genre_id INT AUTO_INCREMENT PRIMARY KEY, genre_title VARCHAR(20)) AUTO_INCREMENT=2001;
INSERT INTO genre(genre_title) VALUES("Action"), ("Adventure"),("Comedy"), ("Crime"), ("Romance"), ("Thriller"), ("Horror");

CREATE TABLE movie_genres(movie_id INT NOT NULL, genre_id INT NOT NULL, PRIMARY KEY (movie_id, genre_id), FOREIGN KEY (movie_id) REFERENCES movie(movie_id), FOREIGN KEY (genre_id) REFERENCES genre(genre_id));

INSERT INTO movie_genres (movie_id, genre_id) VALUES
(1001, 2004),
(1001, 2002),
(1002, 2003),
(1002, 2007),
(1003, 2002),
(1003, 2006),
(1004, 2004),
(1004, 2005),
(1005, 2001),
(1005, 2002),
(1005, 2005),
(1005, 2003),
(1006, 2006);

Query to check genre

SELECT
    m.movie_title,
    g.genre_title
FROM
    movie m
JOIN
    movie_genres mg ON m.movie_id = mg.movie_id
JOIN
    genre g ON mg.genre_id = g.genre_id
ORDER BY
    m.movie_title, g.genre_title;

***************************************************************************************************
3. Movie can have multiple reviews and Reviews can belongs to a user

CREATE TABLE user (user_id INT AUTO_INCREMENT PRIMARY KEY, user_name VARCHAR(20))AUTO_INCREMENT=3001;
INSERT INTO user (user_name) VALUES ("Pankaj Tripathi"),("Sonu Sood"), ("Salman Khan"),("Akshay Kumar"),("Sunil Shetty"),("Mahima Chaudhari"),("Twinkle Khanna"),("Akshay Khanna"),("Anil Kapoor"),("Kapil Sharma");

CREATE TABLE reviews(review_id INT AUTO_INCREMENT PRIMARY KEY, movie_id INT NOT NULL, FOREIGN KEY (movie_id) REFERENCES movie(movie_id), review_star DECIMAL(2,1) NOT NULL, user_id INT NOT NULL, FOREIGN KEY (user_id) REFERENCES user(user_id));

INSERT INTO reviews (movie_id, review_star, user_id) VALUES 
("1001", "9.2", "3001"), 
("1001", "9.2", "3010"), 
("1002", "9.1", "3002"),
("1002", "9.1", "3009"),
("1003", "9.4", "3003"),
("1003", "9.4", "3008"),
("1004", "6.5", "3004"),
("1004", "6.5", "3005"),
("1005", "9.9", "3001"),
("1005", "9.8", "3002"),
("1005", "9.6", "3003"),
("1005", "9.7", "3004"),
("1005", "9.9", "3005"),
("1005", "9.8", "3006"),
("1005", "9.2", "3007"),
("1005", "9.9", "3009"),
("1006", "5.2", "3010");

Query for Reviews
SELECT
    m.movie_title,
    u.user_name,
    r.review_star 
FROM
    movie m
JOIN
    reviews r ON m.movie_id = r.movie_id
JOIN
    user u ON r.user_id = u.user_id
ORDER BY
    m.movie_title;

***************************************************************************************************

4.Create artist table(Artist can have multiple skills. Artist can perform multiple roles in a single film)

CREATE TABLE artists (artist_id INT AUTO_INCREMENT PRIMARY KEY, artist_name VARCHAR(100) NOT NULL);

5. Insert Artist Data

INSERT INTO artists (artist_name) VALUES 
('Kapil'), 
('Akshay'), 
('Mahima');

6. Create skills table

CREATE TABLE skills (skill_id INT AUTO_INCREMENT PRIMARY KEY, skill_name VARCHAR(50) NOT NULL UNIQUE);

7. Inserting Skills

INSERT INTO skills (skill_name) VALUES ('Acting'), ('Singing'), ('Dancing'), ('Directing');

8.Create Artist Skill Table

CREATE TABLE artist_skills (artist_id INT NOT NULL, skill_id INT NOT NULL, PRIMARY KEY (artist_id, skill_id), FOREIGN KEY (artist_id) REFERENCES artists(artist_id) ON DELETE CASCADE, FOREIGN KEY (skill_id) REFERENCES skills(skill_id) ON DELETE CASCADE);

9. Insert artist skills

INSERT INTO artist_skills (artist_id, skill_id) VALUES (1, 1),(1, 2),(2, 2),(2, 3),(3, 1),(3, 4);

10. Create Artist roles Table

CREATE TABLE artist_roles (movie_id INT NOT NULL, artist_id INT NOT NULL, role_name VARCHAR(50) NOT NULL, PRIMARY KEY (movie_id, artist_id, role_name), FOREIGN KEY (movie_id) REFERENCES movie(movie_id) ON DELETE CASCADE, FOREIGN KEY (artist_id) REFERENCES artists(artist_id) ON DELETE CASCADE);

11. Insert artist role with movies

INSERT INTO artist_roles (movie_id, artist_id, role_name) VALUES (1001, 1, 'Lead Actor'),
(1001, 1, 'Director'),
(1002, 2, 'Cameo'),
(1002, 2, 'Supporting Actor'),
(1003, 3, 'Lead Actor'),
(1004, 3, 'Cameo'),
(1005, 2, 'Director'),
(1005, 1, 'Lead Actor'),
(1005, 3, 'Supporting Actor'),
(1005, 2, 'Lead Actor'),
(1006, 2, 'Cameo');

Query for skill and Role

SELECT
    a.artist_name,
    s.skill_name,
    m.movie_title,
    ar.role_name
FROM
    artists a
JOIN
    artist_skills ak ON a.artist_id = ak.artist_id
JOIN
    skills s ON ak.skill_id = s.skill_id
JOIN
    artist_roles ar ON a.artist_id = ar.artist_id
JOIN
    movie m ON ar.movie_id = m.movie_id;
