# <ins>SQL - MovieTasks</ins>

-----------------------------------------------------------------------JOINS-----------------------------------------------------------------------


 Task 1. From the following table, write a SQL query to find all reviewers whose ratings contain a NULL value. Return reviewer name.


      SELECT rev_name
      FROM reviewer
      INNER JOIN rating USING(rev_id)
      WHERE rev_stars IS NULL;



-----------------------------------------------------------------------------------------------------------------------------------------


 Task 2. From the following table, write a SQL query to find out who was cast in the movie 'Annie Hall'. Return actor first name, last name and role.



       SELECT a.act_fname, a.act_lname, mc.role
       FROM actor a
       INNER JOIN movie_cast mc ON mc.act_id = a.act_id
       INNER JOIN movie m ON m.mov_id = mc.mov_id
       WHERE m.mov_title = 'Annie Hall'


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 3. From the following table, write a SQL query to find the director who directed a movie that featured a role in 'Eyes Wide Shut'. Return director first name, last name and movie title.


        SELECT d.dir_fname, d.dir_lname, m.mov_title
        FROM director d
        INNER JOIN movie_direction md ON md.dir_id = d.dir_id
        INNER JOIN movie m ON m.mov_id = md.mov_id
        WHERE m.mov_title = 'Eyes Wide Shut'


-----------------------------------------------------------------------------------------------------------------------------------------

 Task 4. From the following tables, write a SQL query to find the director of a movie that cast a role as Sean Maguire. Return director first name, last name and movie title.


        SELECT d.dir_fname, d.dir_lname, m.mov_title
        FROM director d
        INNER JOIN movie_direction md ON md.dir_id = d.dir_id
        INNER JOIN movie m ON m.mov_id = md.mov_id
        INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
        WHERE mc.role = 'Sean Maguire'


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 5. From the following table, write a SQL query to find out which actors have not appeared in any movies between 1990 and 2000 (Begin and end values are included.). Return actor first name, last name, movie title and release year.

       
       SELECT a.act_fname, a.act_lname, m.mov_title, m.mov_year
       FROM actor a
       INNER JOIN movie_cast mc ON mc.act_id = a.act_id
       INNER JOIN movie m ON m.mov_id = mc.mov_id
       WHERE m.mov_year NOT BETWEEN 1990 AND 2000


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 6.  From the following table, write a SQL query to find the directors who have directed films in a variety of genres. Group the result set on director first name, last name and generic title. Sort the result-set in ascending order by director first name and last name. Return director first name, last name and number of genres movies.



       SELECT d.dir_fname, d.dir_lname, g.gen_title, COUNT(g.gen_title)
       FROM director d
       INNER JOIN movie_direction md ON md.dir_id = d.dir_id
       INNER JOIN movie m ON m.mov_id = md.mov_id
       INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
       INNER JOIN genres g ON g.gen_id = mg.gen_id
       GROUP BY d.dir_fname, d.dir_lname, g.gen_title
       ORDER BY d.dir_fname, d.dir_lname 

-----------------------------------------------------------------------------------------------------------------------------------------


 Task 7. From the following table, write a SQL query to find the movies with year and genres. Return movie title, movie year and generic title.


       SELECT m.mov_title, m.mov_year, g.gen_title
       FROM movie m
       INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
       INNER JOIN genres g ON g.gen_id = mg.gen_id


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 8. From the following tables, write a SQL query to find all the movies with year, genres, and name of the director.



     SELECT m.mov_title, m.mov_year, g.gen_title, d.dir_fname, d.dir_lname
     FROM director d
     INNER JOIN movie_direction md ON md.dir_id = d.dir_id
     INNER JOIN movie m ON m.mov_id = md.mov_id
     INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
     INNER JOIN genres g ON g.gen_id = mg.gen_id



-----------------------------------------------------------------------------------------------------------------------------------------



 Task 9. From the following tables, write a SQL query to find the movies released before 1st January 1989. Sort the result-set in descending order by date of release. Return movie title, release year, date of release, duration, and first and last name of the director.


     SELECT m.mov_title, m.mov_year, m.mov_dt_rel, m.mov_time, d.dir_fname, d.dir_lname
     FROM director d
     INNER JOIN movie_direction md ON md.dir_id = d.dir_id
     INNER JOIN movie m ON m.mov_id = md.mov_id
     WHERE m.mov_dt_rel < '1989-01-01'
     ORDER BY m.mov_dt_rel DESC	


-----------------------------------------------------------------------------------------------------------------------------------------



 Task 10. From the following table, write a SQL query to calculate the average movie length and count the number of movies in each genre. Return genre title, average time and number of movies for each genre.



     SELECT g.gen_title AS "Genre", AVG(m.mov_time) AS "avg", COUNT(g.gen_title)
     FROM movie m
     INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
     INNER JOIN genres g ON g.gen_id = mg.gen_id
     GROUP BY g.gen_title


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 11. From the following table, write a SQL query to find movies with the shortest duration. Return movie title, movie year, director first name, last name, actor first name, last name and role.


     SELECT m.mov_title, m.mov_year, d.dir_fname, d.dir_lname, a.act_fname, a.act_lname, mc.role
     FROM movie m
     JOIN movie_direction md ON m.mov_id = md.mov_id
     JOIN movie_cast mc ON m.mov_id = mc.mov_id
     JOIN director d ON md.dir_id = d.dir_id
     JOIN actor a ON mc.act_id = a.act_id
     WHERE m.mov_time = (SELECT MIN(mov_time) FROM movie);


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 12. From the following table, write a SQL query to find the years in which a movie received a rating of 3 or 4. Sort the result in increasing order on movie year.



     SELECT DISTINCT m.mov_year
     FROM movie m
     INNER JOIN rating r ON r.mov_id = m.mov_id
     WHERE r.rev_stars IN (3,4)
     ORDER BY m.mov_year ASC



-----------------------------------------------------------------------------------------------------------------------------------------



 Task 13. From the following tables, write a SQL query to get the reviewer name, movie title, and stars in an order that reviewer name will come first, then by movie title, and lastly by number of stars.


      SELECT re.rev_name, m.mov_title, r.rev_stars
      FROM movie m
      INNER JOIN rating r ON r.mov_id = m.mov_id
      INNER JOIN reviewer re ON re.rev_id = r.rev_id
      WHERE re.rev_name IS NOT NULL
      ORDER BY re.rev_name, m.mov_title, r.rev_stars


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 14. From the following table, write a SQL query to find those movies that have at least one rating and received the most stars. Sort the result-set on movie title. Return movie title and maximum review stars.



     SELECT m.mov_title, MAX(r.rev_stars)
     FROM movie m
     INNER JOIN rating r ON r.mov_id = m.mov_id
     INNER JOIN reviewer re ON re.rev_id = r.rev_id
     GROUP BY m.mov_title
     HAVING MAX(r.rev_stars) > 0
     ORDER BY m.mov_title



-----------------------------------------------------------------------------------------------------------------------------------------


 Task 15. From the following table, write a SQL query to find out which movies have received ratings. Return movie title, director first name, director last name and review stars.



     SELECT m.mov_title, d.dir_fname, d.dir_lname, r.rev_stars
     FROM movie m
     INNER JOIN movie_direction md ON md.mov_id = m.mov_id
     INNER JOIN director d ON d.dir_id = md.dir_id
     INNER JOIN rating r ON r.mov_id = m.mov_id
     WHERE r.rev_stars IS NOT NULL


-----------------------------------------------------------------------------------------------------------------------------------------



 Task 16.  From the following table, write a SQL query to find movies in which one or more actors have acted in more than one film. Return movie title, actor first and last name, and the role.



     SELECT m.mov_title, a.act_fname, a.act_lname, mc.role
     FROM movie m
     INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
     INNER JOIN actor a ON a.act_id = mc.act_id
     WHERE a.act_id IN (
         SELECT mc.act_id
         FROM movie_cast mc
         GROUP BY mc.act_id
         HAVING COUNT(mc.act_id) > 1
    )


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 17. From the following tables, write a SQL query to find the actor whose first name is 'Claire' and last name is 'Danes'. Return director first name, last name, movie title, actor first name and last name, role.



    SELECT d.dir_fname, d.dir_lname, m.mov_title, a.act_fname, a.act_lname, mc.role
    FROM movie m
    INNER JOIN movie_direction md ON md.mov_id = m.mov_id
    INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
    INNER JOIN director d ON d.dir_id = md.dir_id
    INNER JOIN actor a ON a.act_id = mc.act_id
    WHERE a.act_fname = 'Claire' AND a.act_lname = 'Danes'


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 18. From the following table, write a SQL query to find for actors whose films have been directed by them. Return actor first name, last name, movie title and role.


    SELECT a.act_fname, a.act_lname, m.mov_title, mc.role
    FROM actor a
    INNER JOIN movie_cast mc ON mc.act_id = a.act_id
    INNER JOIN movie m ON m.mov_id = mc.mov_id
    INNER JOIN movie_direction md ON md.mov_id = m.mov_id
    INNER JOIN director d on d.dir_id = md.dir_id
    WHERE a.act_fname = d.dir_fname AND a.act_lname = d.dir_lname


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 19. From the following tables, write a SQL query to find the cast list of the movie ‘Chinatown’. Return first name, last name.


    SELECT a.act_fname AS "Actor FirstName", a.act_lname AS "Actor LastName"
    FROM movie m
    JOIN movie_cast mc ON mc.mov_id = m.mov_id
    JOIN actor a ON a.act_id = mc.act_id
    WHERE m.mov_title = 'Chinatown'



-----------------------------------------------------------------------------------------------------------------------------------------


 Task 20. From the following tables, write a SQL query to find those movies where actor’s first name is 'Harrison' and last name is 'Ford'. Return movie title.



    SELECT m.mov_title
    FROM movie m
    INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
    INNER JOIN actor a ON a.act_id = mc.act_id
    WHERE a.act_fname = 'Harrison' AND a.act_lname = 'Ford'


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 21. From the following tables, write a SQL query to find the highest-rated movies. Return movie title, movie year, review stars and releasing country.


    SELECT m.mov_title, m.mov_year, r.rev_stars, m.mov_rel_country
    FROM movie m
    INNER JOIN rating r ON r.mov_id = m.mov_id
    WHERE r.rev_stars = (
       SELECT MAX(r2.rev_stars)
       FROM rating r2
    );


-----------------------------------------------------------------------------------------------------------------------------------------



 Task 22. From the following tables, write a SQL query to find the highest-rated ‘Mystery Movies’. Return the title, year, and rating.

 
    SELECT m.mov_title, m.mov_year, r.rev_stars
    FROM movie m
    INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
    INNER JOIN genres g ON g.gen_id = mg.gen_id
    INNER JOIN rating r ON r.mov_id = m.mov_id
    WHERE g.gen_title = 'Mystery'



-----------------------------------------------------------------------------------------------------------------------------------------

 Task 23. From the following tables, write a SQL query to find the years when most of the ‘Mystery Movies’ produced. Count the number of generic title and compute their average rating. Group the result set on movie release year, generic title. Return movie year, generic title, number of generic title and average rating.



    SELECT m.mov_year, COUNT(g.gen_title), AVG(r.rev_stars)
    FROM movie m
    INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
    INNER JOIN genres g ON g.gen_id = mg.gen_id
    INNER JOIN rating r ON r.mov_id = m.mov_id
    WHERE g.gen_title = 'Mystery'
    GROUP BY m.mov_year, g.gen_title


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 24. From the following tables, write a query in SQL to generate a report, which contain the fields movie title, name of the female actor, year of the movie, role, movie genres, the director, date of release, and rating of that movie.


    SELECT m.mov_title, a.act_fname, m.mov_year, mc.role, g.gen_title, d.dir_fname, d.dir_lname, m.mov_dt_rel, r.rev_stars
    FROM movie m
    INNER JOIN movie_direction md ON md.mov_id = m.mov_id
    INNER JOIN director d ON d.dir_id = md.dir_id
    INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
    INNER JOIN actor a ON a.act_id = mc.act_id
    INNER JOIN rating r ON r.mov_id = m.mov_id
    INNER JOIN movie_genres mg ON mg.mov_id = m.mov_id
    INNER JOIN genres g ON g.gen_id = mg.gen_id
    WHERE a.act_gender = 'F'


-----------------------------------------------------------------------SUBQUERY-----------------------------------------------------------------------


 Task 1. From the following tables, write a SQL query to find the actors who played a role in the movie ' Annie Hall'. Return all the fields of actor table.


     SELECT a.act_id, a.act_fname, a.act_lname, a.act_gender
     FROM actor a
     INNER JOIN movie_cast mc ON mc.act_id = a.act_id
     INNER JOIN movie m ON m.mov_id = mc.mov_id
     WHERE m.mov_id IN (
        SELECT mo.mov_id
        FROM movie mo
        WHERE mo.mov_title = 'Annie Hall'
    )


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 2. From the following tables, write a SQL query to find the director of a film that cast a role in 'Eyes Wide Shut'. Return director first name, last name.


    SELECT d.dir_fname, d.dir_lname
    FROM director d
    INNER JOIN movie_direction md ON md.dir_id = d.dir_id
    INNER JOIN movie m ON m.mov_id = md.mov_id
    INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
    INNER JOIN actor a ON a.act_id = mc.act_id
    WHERE m.mov_id IN (
       SELECT mo.mov_id
       FROM movie mo
       WHERE mo.mov_title = 'Eyes Wide Shut'
       )


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 3. From the following table, write a SQL query to find those movies that have been released in countries other than the United Kingdom. Return movie title, movie year, movie time, and date of release, releasing country.


    SELECT m.mov_title, m.mov_year, m.mov_time, m.mov_dt_rel, m.mov_rel_country
    FROM movie m
    JOIN (
        SELECT mov_id
        FROM movie
        WHERE mov_rel_country != 'UK'
    ) AS filteredMovies ON m.mov_id = filteredMovies.mov_id


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 4. From the following tables, write a SQL query to find for movies whose reviewer is unknown. Return movie title, year, release date, director first name, last name, actor first name, last name.


    SELECT m.mov_title, m.mov_year, m.mov_dt_rel, d.dir_fname, d.dir_lname, a.act_fname, a.act_lname
    FROM movie m
    INNER JOIN movie_direction md ON md.mov_id = m.mov_id
    INNER JOIN director d ON d.dir_id = md.dir_id
    INNER JOIN rating r ON r.mov_id = m.mov_id
    INNER JOIN reviewer re ON re.rev_id = r.rev_id
    INNER JOIN movie_cast mc ON mc.mov_id = m.mov_id
    INNER JOIN actor a ON a.act_id = mc.act_id
    WHERE re.rev_name IS NULL



-----------------------------------------------------------------------------------------------------------------------------------------


 Task 5. From the following tables, write a SQL query to find those movies directed by the director whose first name is Woddy and last name is Allen. Return movie title.


     SELECT m.mov_title
     FROM movie m
     INNER JOIN movie_direction md ON md.mov_id = m.mov_id
     WHERE md.mov_id = (
       SELECT md.mov_id
       FROM movie_direction md
       INNER JOIN director d ON d.dir_id = md.dir_id
       WHERE d.dir_fname = 'Woody' AND d.dir_lname = 'Allen'
    )



-----------------------------------------------------------------------------------------------------------------------------------------


 Task 6. From the following tables, write a SQL query to determine those years in which there was at least one movie that received a rating of at least three stars. Sort the result-set in ascending order by movie year. Return movie year.


    SELECT DISTINCT m.mov_year AS "MovieYear"
    FROM movie m
    WHERE m.mov_id IN (
       SELECT r.mov_id
       FROM rating r
       WHERE r.rev_stars > 3
    ) ORDER BY m.mov_year ASC


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 7. From the following table, write a SQL query to search for movies that do not have any ratings. Return movie title.


    SELECT DISTINCT m.mov_title
    FROM movie m
    WHERE NOT EXISTS (
      SELECT 1
      FROM rating r
      WHERE r.mov_id = m.mov_id
    );


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 8. From the following table, write a SQL query to find those reviewers who have not given a rating to certain films. Return reviewer name.


    SELECT re.rev_name
    FROM reviewer re
    INNER JOIN rating r ON r.rev_id = re.rev_id 
    WHERE re.rev_id IN (
      SELECT ra.rev_id
      FROM rating ra
      WHERE ra.rev_stars IS NULL
    )

-----------------------------------------------------------------------------------------------------------------------------------------


 Task 9. From the following tables, write a SQL query to find movies that have been reviewed by a reviewer and received a rating. Sort the result-set in ascending order by reviewer name, movie title, review Stars. Return reviewer name, movie title, review Stars.


    SELECT re.rev_name, m.mov_title, r.rev_stars
    FROM movie m
    INNER JOIN rating r ON r.mov_id = m.mov_id
    INNER JOIN reviewer re ON re.rev_id = r.rev_id
    WHERE re.rev_name IS NOT NULL AND r.rev_stars IS NOT NULL
    ORDER BY re.rev_name, m.mov_title, r.rev_stars


-----------------------------------------------------------------------------------------------------------------------------------------


 Task 10. From the following table, write a SQL query to find movies that have been reviewed by a reviewer and received a rating. Group the result set on reviewer’s name, movie title. Return reviewer’s name, movie title.



    SELECT rev_name, mov_title 
    FROM reviewer, movie, rating, rating r2
    WHERE rating.mov_id = movie.mov_id 
     AND reviewer.rev_id = rating.rev_ID 
     AND rating.rev_id = r2.rev_id 
    GROUP BY rev_name, mov_title 
    HAVING count(*) > 1;

