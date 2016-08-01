1. Modify to show population of Germany

SELECT population FROM world
  WHERE name = 'Germany'

2. Show the name and the population for 'Ireland', 'Iceland' and 'Denmark'.

SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Iceland', 'Denmark');

3. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.

SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000

4. Show the name for the countries that have a population of at least 200 million.

SELECT name FROM world
WHERE population>200000000

5.  Give the name and the per capita GDP for those countries with a population of at least 200 million.

SELECT name, (gdp / population)
FROM world
WHERE population > 200000000

6. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.

SELECT name, (population / 1000000)
FROM world
WHERE continent = "South America"

7. Show the name and population for France, Germany, Italy

SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy')

8. Show the countries which have a name that includes the word 'United'

SELECT name
FROM world
WHERE name LIKE '%United%'

9.  Show the countries that are big by area or big by population. Show name, population and area.

SELECT name, population, area
FROM world
WHERE area > 3000000
OR population > 250000000


10. Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area.

SELECT name, population, area
FROM world
WHERE area > 3000000
XOR population > 250000000

11. For South America show population in millions and GDP in billions both to 2 decimal places.

SELECT name, ROUND(population/1000000, 2),ROUND(gdp/1000000000, 2)
FROM world
WHERE continent = 'South America'

12. Show per-capita GDP for the trillion dollar countries to the nearest $1000.

SELECT name, ROUND(gdp/population, -3)
FROM world
WHERE gdp >= 1000000000000

13. Show the name - but substitute Australasia for Oceania - for countries beginning with N.

SELECT name,
       CASE WHEN continent='Oceania' THEN 'Australasia'
            ELSE continent END
  FROM world
 WHERE name LIKE 'N%'

14.  Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B

SELECT name,
       CASE WHEN continent IN ('Europe', 'Asia') THEN 'Eurasia'
            WHEN continent IN ('North America', 'South America', 'Caribbean') THEN 'America'
            ELSE continent END
FROM world
WHERE name LIKE 'A%' OR name LIKE 'B%'

15.  Show the name, the original continent and the new continent of all countries.

SELECT name, continent,
CASE WHEN continent = 'Oceania' THEN 'Australasia'
     WHEN continent IN ('Eurasia', 'Turkey') THEN 'Europe/Asia'
     WHEN continent = 'Caribbean' AND name LIKE 'B%' THEN 'North America'
     WHEN continent = 'Caribbean' AND name NOT LIKE 'B%' THEN 'South America'
     ELSE continent END
FROM world
ORDER BY name

16. Change the query shown so that it displays Nobel prizes for 1950.

SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950

17. Show who won the 1962 prize for Literature.

SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'

18. Show the year and subject that won 'Albert Einstein' his prize.

SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'

19. Give the name of the 'Peace' winners since the year 2000, including 2000.

SELECT winner
FROM nobel
WHERE yr >= 2000
AND subject = 'Peace'

20. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.

SELECT yr,subject,winner
FROM nobel
WHERE yr BETWEEN 1980 AND 1989 AND subject = 'Literature'

21. Show all details of the presidential winners:

Theodore Roosevelt
Woodrow Wilson
Jimmy Carter

SELECT *
FROM nobel
 WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter')

22. Show the winners with first name John

SELECT winner
FROM nobel
WHERE winner LIKE 'John%'

23. Show the Physics winners for 1980 together with the Chemistry winners for 1984.

SELECT yr,subject,winner
FROM nobel
WHERE subject = 'Physics' AND yr = 1980
OR subject = 'Chemistry' AND yr = 1984

24. Show the winners for 1980 excluding the Chemistry and Medicine

SELECT yr,subject,winner
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine')

25. Show who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)

SELECT yr,subject,winner
FROM nobel
WHERE subject = 'Medicine' AND yr < 1910
OR subject = 'Literature' AND yr >= 2004

26. Find all details of the prize won by PETER GRÜNBERG

SELECT yr,subject,winner
FROM nobel
WHERE winner = 'Peter Grünberg'

27. Find all details of the prize won by EUGENE O'NEILL

SELECT yr,subject,winner
FROM nobel
WHERE winner = "Eugene O'Neill"

28. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.

SELECT winner,yr,subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner

29. Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.

SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'),subject,winner


JOIN QUERIES


1. Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

SELECT matchid,player FROM goal 
  WHERE teamid = 'GER'

2. Show id, stadium, team1, team2 for just game 1012

SELECT id,stadium,team1,team2
 FROM game
WHERE id = 1012

3. Modify it to show the player, teamid, stadium and mdate and for every German goal.

SELECT player,teamid, stadium, mdate
  FROM game JOIN goal ON (id=matchid)
  WHERE teamid = 'GER'

4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'

SELECT team1, team2, player
  FROM game JOIN goal ON (id=matchid)
  WHERE player LIKE 'Mario%'

5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

SELECT player, teamid, coach, gtime
  FROM goal JOIN eteam on teamid=id
 WHERE gtime<=10

6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

SELECT mdate, teamname
FROM game JOIN eteam ON (team1=eteam.id)
WHERE coach = 'Fernando Santos'

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

SELECT player
FROM goal JOIN game ON (matchid=game.id)
WHERE stadium = 'National Stadium, Warsaw'

8. Instead show the name of all players who scored a goal against Germany.

SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER') AND
    NOT teamid = 'GER'

9. Show teamname and the total number of goals scored.

SELECT teamname, COUNT(*)
  FROM eteam JOIN goal ON id=teamid
 GROUP BY teamname

10. Show the stadium and the number of goals scored in each stadium.

SELECT stadium, COUNT(matchid)
FROM game JOIN goal ON (id=matchid)
GROUP BY stadium

11. For every match involving 'POL', show the matchid, date and the number of goals scored.

SELECT matchid,mdate, COUNT(player)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid,mdate

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
GROUP BY matchid, mdate

13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.

SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
  FROM game LEFT OUTER JOIN goal ON matchid = id
  GROUP BY mdate,team1,team2


1. List the films where the yr is 1962 [Show id, title]

SELECT id, title
 FROM movie
 WHERE yr=1962

2. Give year of 'Citizen Kane'.

SELECT yr
FROM movie
WHERE title = 'Citizen Kane'

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.

SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr

4. What are the titles of the films with id 11768, 11955, 21191

SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191)

5. What id number does the actress 'Glenn Close' have?

SELECT id
FROM actor
WHERE name = 'Glenn Close'

6. What is the id of the film 'Casablanca'

SELECT id
FROM movie
WHERE title = 'Casablanca'

7. Use movieid=11768, this is the value that you obtained in the previous question.

SELECT name
FROM actor JOIN casting ON (id=actorid)
WHERE movieid = 11768

8. Obtain the cast list for the film 'Alien'

SELECT name
FROM actor JOIN casting ON (actor.id=actorid)
JOIN movie ON (movieid=movie.id)
WHERE title = 'Alien'

9. List the films in which 'Harrison Ford' has appeared

SELECT title
FROM movie JOIN casting ON (movie.id = movieid)
JOIN actor ON (actorid = actor.id)
WHERE actor.name = 'Harrison Ford'

10. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

SELECT title
FROM movie JOIN casting ON (movie.id = movieid)
JOIN actor ON (actorid=actor.id)
WHERE actor.name = 'Harrison Ford' AND
NOT ord = 1


11. List the films together with the leading star for all 1962 films. 

SELECT title, actor.name
FROM movie JOIN casting ON (movieid=movie.id)
JOIN actor ON (actorid=actor.id)
WHERE yr = 1962 AND ord = 1

12. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
WHERE name='John Travolta'
GROUP BY yr
HAVING COUNT(title)>2


13. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

SELECT title, actor.name
FROM movie JOIN casting ON movie.id=movieid JOIN actor ON actorid=actor.id
WHERE ord = 1 AND movieid IN (
  SELECT movieid FROM movie JOIN casting ON movie.id=movieid JOIN actor ON actorid=actor.id
  WHERE name='Julie Andrews')

14. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.

SELECT actor.name
FROM actor JOIN casting ON (actor.id = actorid)
WHERE ord=1
GROUP BY actor.name
HAVING COUNT(*) >= 30

15. List the films released in the year 1978 ordered by the number of actors in the cast.

SELECT title, COUNT(actorid)
FROM movie JOIN casting ON (movie.id=movieid)
WHERE yr = 1978
GROUP BY title
ORDER BY COUNT(actorid) DESC

16. List all the people who have worked with 'Art Garfunkel'.

SELECT DISTINCT actor.name
FROM casting JOIN actor ON actorid=actor.id
WHERE movieid IN (SELECT movieid
FROM casting JOIN actor ON actorid=actor.id
WHERE actor.name = 'Art Garfunkel') 
AND NOT actor.name = 'Art Garfunkel'


SUM AND COUNT

1. Show the total population of the world.

SELECT SUM(population)
FROM world

2. List all the continents - just once each.

SELECT DISTINCT continent
FROM world

3. Give the total GDP of Africa

SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa'

4. How many countries have an area of at least 1000000

SELECT COUNT(*)
FROM world
WHERE area >= 1000000

5. What is the total population of ('France','Germany','Spain')

SELECT SUM(population)
FROM world
WHERE name IN ('France','Germany','Spain')

6. For each continent show the continent and number of countries.

SELECT continent, COUNT(*)
FROM world
GROUP BY continent

7. For each continent show the continent and number of countries with populations of at least 10 million.

SELECT continent, COUNT(*)
FROM world
WHERE population >= 10000000
GROUP BY continent

8. List the continents that have a total population of at least 100 million.

SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000


SELECT WITHIN SELECT


1. List each country name where the population is larger than that of 'Russia'.

SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')

2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.

SELECT name
FROM world
WHERE continent = 'Europe'
AND gdp/population > (SELECT gdp/population FROM world WHERE name='United Kingdom')

3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))
ORDER BY name

4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.

SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Canada')
AND population < (SELECT population FROM world WHERE name = 'Poland')

5. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

SELECT name, CONCAT(ROUND((population/(SELECT population FROM world WHERE name='Germany'))*100), '%')
FROM world
WHERE continent = 'Europe'

6. Which countries have a GDP greater than every country in Europe? (Some countries may have NULL gdp values)

SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE continent = 'Europe' AND gdp > 0)

7. Find the largest country (by area) in each continent, show the continent, the name and the area:

SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)

8. List each continent and the name of the country that comes first alphabetically.

SELECT continent, name FROM world x
WHERE name <= ALL (SELECT name FROM world y WHERE y.continent = x.continent)

9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

SELECT name,continent,population
FROM world
WHERE continent NOT IN (SELECT continent FROM world WHERE population > 25000000)

10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.

SELECT name,continent
FROM world x 
WHERE population > ALL(SELECT 3*population FROM world y WHERE y.continent = x.continent AND NOT name = x.name)


USING NULL

1.  List the teachers who have NULL for their department.

SELECT name
FROM teacher
WHERE dept IS NULL

2.  Note the INNER JOIN misses the teachers with no department and the departments with no teacher.

SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)

3.   Use a different JOIN so that all teachers are listed.

SELECT teacher.name, dept.name
 FROM teacher LEFT OUTER JOIN dept
           ON (teacher.dept=dept.id)

4.  Use a different JOIN so that all departments are listed.

SELECT teacher.name, dept.name
 FROM teacher RIGHT OUTER JOIN dept
           ON (teacher.dept=dept.id)

5.  Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'

SELECT name, COALESCE(mobile, '07986 444 2266')
FROM teacher

6.  Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.

SELECT teacher.name, COALESCE(dept.name, 'None')
FROM teacher LEFT JOIN dept ON (teacher.dept=dept.id)


7.  Use COUNT to show the number of teachers and the number of mobile phones.

SELECT COUNT(name), COUNT(mobile)
FROM teacher

8.  Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.


SELECT dept.name, COUNT(teacher.dept)
FROM teacher RIGHT JOIN dept ON (teacher.dept=dept.id)
GROUP BY dept.name

9.  Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.

SELECT name, CASE WHEN dept IN (1, 2) THEN 'Sci' ELSE 'Art' END
FROM teacher

10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.

SELECT name, CASE WHEN dept IN (1,2) THEN 'Sci' WHEN dept=3 THEN 'Art' ELSE 'None' END
FROM teacher
