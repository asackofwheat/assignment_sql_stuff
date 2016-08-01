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
4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
8. Instead show the name of all players who scored a goal against Germany.
9. Show teamname and the total number of goals scored.
10. Show the stadium and the number of goals scored in each stadium.
11. For every match involving 'POL', show the matchid, date and the number of goals scored.
12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.