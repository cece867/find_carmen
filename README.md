# find_carmen

- Record your query (or queries, some clues require more than one) below the clue, then comment out the output below it
-- use two `-` to comment at the start of a line, or highlight the text and press `⌘/` to toggle comments
-- EXAMPLE: SELECT ALL FROM THE TABLE COUNTRY AND LIMIT IT TO ONE ENTRY

SELECT * FROM COUNTRY LIMIT 1;

--  -[ RECORD 1 ]--+--------------------------
-- code           | AFG-- TEST COMMAND AND SAMPLE OUTPUT

-- name           | Afghanistan
-- continent      | Asia
-- region         | Southern and Central Asia
-- surfacearea    | 652090
-- indepyear      | 1919
-- population     | 22720000
-- lifeexpectancy | 45.9
-- gnp            | 5976.00
-- gnpold         |
-- localname      | Afganistan/Afqanestan
-- governmentform | Islamic Emirate
-- headofstate    | Mohammad Omar
-- capital        | 1
-- code2          | AF


-- Clue #1: We recently got word that someone fitting Carmen Sandiego's description has been traveling through Southern Europe. She's most likely traveling someplace where she won't be noticed, so find the least populated country in Southern Europe, and we'll start looking for her there.

   --
--  -[ RECORD 1 ]--+----------------
--code |    name     | continent |          region           | surfacearea | indepyear | population | lifeexpectancy |   gnp   | gnpold |       localname       | governmentform  |  headofstate  | capital | code2 
------+-------------+-----------+---------------------------+-------------+-----------+------------+----------------+---------+--------+-----------------------+-----------------+---------------+---------+-------
 --AFG  | Afghanistan | Asia      | Southern and Central Asia |      652090 |      1919 |   22720000 |           45.9 | 5976.00 |        | Afganistan/Afqanestan | Islamic Emirate | Mohammad Omar |       1 | AF
--(1 row)
            --name              | population |     region      | code 
-------------------------------+------------+-----------------+------
-- Holy See (Vatican City State) |       1000 | Southern Europe | VAT
--(1 row)

-- Clue #2: Now that we're here, we have insight that Carmen was seen attending language classes in this country's officially recognized language. Check our databases and find out what language is spoken in this country, so we can call in a translator to work with you.

   -----
--      language      | isofficial | percentage
--language | isofficial | percentage 
----------+------------+------------
 --Italian  | t          |          0
--(1 row)



-- Clue #3: We have new news on the classes Carmen attended – our gumshoes tell us she's moved on to a different country, a country where people speak only the language she was learning. Find out which nearby country speaks nothing but that language.

--  -[ RECORD 1 ]--+----------
-- language       | Italian
-- isofficial     | t
-- percentage     | 0
--name    |     region      | language | percentage 
------------+-----------------+----------+------------
 --San Marino | Southern Europe | Italian  |        100

-- Clue #4: We're booking the first flight out – maybe we've actually got a chance to catch her this time. There are only two cities she could be flying to in the country. One is named the same as the country – that would be too obvious. We're following our gut on this one; find out what other city in that country she might be flying to.

   SELECT ci.name, ci.population, c.name AS country_name
   FROM city ci
   JOIN country c ON ci.countrycode = c.code
   WHERE c.name = 'San Marino'
     AND ci.name != c.name
   ORDER BY ci.population DESC;

 --name    | population | country_name 
------------+------------+--------------
 --Serravalle |       4802 | San Marino
--(1 row)


-- Clue #5: Oh no, she pulled a switch – there are two cities with very similar names, but in totally different parts of the globe! She's headed to South America as we speak; go find a city whose name is like the one we were headed to, but doesn't end the same. Find out the city, and do another search for what country it's in. Hurry!
   SELECT ci.name, ci.population, c.name AS country_name, c.region
   FROM city ci
   JOIN country c ON ci.countrycode = c.code
   WHERE c.region = 'South America'
     AND ci.name LIKE 'Ser%'
     AND ci.name NOT LIKE '%alle';
   --   name      | population | country_name |    region     
---------------+------------+--------------+---------------
-- Serra         |     302666 | Brazil       | South America
--Sert�ozinho |      98140 | Brazil       | South America
--(2 rows)


-- Clue #6: We're close! Our South American agent says she just got a taxi at the airport, and is headed towards the capital! Look up the country's capital, and get there pronto! Send us the name of where you're headed and we'll follow right behind you!

--country_name | capital_city | population 
--------------+--------------+------------
 --Brazil       | Bras�lia   |    1969868
--(1 row)


-- Clue #7: She knows we're on to her – her taxi dropped her off at the international airport, and she beat us to the boarding gates. We have one chance to catch her, we just have to know where she's heading and beat her to the landing dock.

-- Lucky for us, she's getting cocky. She left us a note, and I'm sure she thinks she's very clever, but if we can crack it, we can finally put her where she belongs – behind bars.

-- Our playdate of late has been unusually fun –
-- As an agent, I'll say, you've been a joy to outrun.
-- And while the food here is great, and the people – so nice!
-- I need a little more sunshine with my slice of life.
-- So I'm off to add one to the population I find
-- In a city of ninety-one thousand and now, eighty five.


-- We're counting on you, gumshoe. Find out where she's headed, send us the info, and we'll be sure to meet her at the gates with bells on.
--name     | population | countrycode 
--------------+------------+-------------
 --Santa Monica |      91084 | USA
--(1 row)


SELECT * FROM computers; id |  make  |    model    | cpu_speed | memory_size |  price  | release_date |            photo_url            | storage_amount | number_usb_ports | number_firewire_ports | number_thunderbolt_ports 
----+--------+-------------+-----------+-------------+---------+--------------+---------------------------------+----------------+------------------+-----------------------+--------------------------
  1 | Apple  | MacBook Pro | 3.2 GHz   | 16GB        | 2499.99 | 2023-01-15   | https://example.com/macbook.jpg | 512GB          |                4 |                     0 |                        2
  2 | Dell   | XPS 15      | 2.8 GHz   | 32GB        | 1899.99 | 2023-03-20   | https://example.com/dell.jpg    | 1TB            |                3 |                     0 |                        1
  3 | HP     | Pavilion    | 2.4 GHz   | 8GB         |  899.99 | 2022-11-10   | https://example.com/hp.jpg      | 256GB          |                2 |                     1 |                        0
  4 | Lenovo | ThinkPad    | 3.0 GHz   | 16GB        | 1499.99 | 2023-02-05   | https://example.com/lenovo.jpg  | 512GB          |                3 |                     0 |                        1
(4 rows)
id |  model_name  | screen_size | resolution |  price  | release_date |            photo_url            
----+--------------+-------------+------------+---------+--------------+---------------------------------
  1 | Samsung QLED | 55 inches   | 4K         | 1299.99 | 2023-04-10   | https://example.com/samsung.jpg
  2 | LG OLED      | 65 inches   | 4K         | 1999.99 | 2023-05-15   | https://example.com/lg.jpg
  3 | Sony Bravia  | 50 inches   | 1080p      |  799.99 | 2022-12-01   | https://example.com/sony.jpg
  4 | TCL Roku     | 43 inches   | 4K         |  499.99 | 2023-03-20   | https://example.com/tcl.jpg
(4 rows)

(END)
ql-lab git:(main) ✗ >....                                                                                                             

INSERT INTO storefronts (address, occupied, price, kitchen, sq_ft, owner, outdoor_seating)
VALUES
    ('10 Shopping Plaza', true, 3000, true, 1500, 'Restaurant Group LLC', true),
    ('20 Retail Row', false, 2500, false, 1200, 'Boutique Owner', false),
    ('30 Market St', true, 4000, true, 2000, 'Cafe Holdings', true);
EOF
➜  sql-lab git:(main) ✗ psql realty_db < realty_seed.sql
psql realty_db < realty.sql
INSERT 0 3
INSERT 0 3
INSERT 0 3
➜  sql-lab git:(main) ✗ psql realty_db < realty.sql
➜  sql-lab git:(main) ✗ >....                                                                                                             

\echo 'Office with lowest number of cubicles:'
SELECT * FROM offices ORDER BY cubicles ASC LIMIT 1;

\echo 'Office with most cubicles and bathrooms:'
SELECT * FROM offices ORDER BY cubicles DESC, bathrooms DESC LIMIT 1;
EOF
➜  sql-lab git:(main) ✗ psql realty_db < realty.sql
Average square footage of all offices:
          avg          
-----------------------
 3333.3333333333333333
(1 row)

Total number of apartments:
 count 
-------
     3
(1 row)

Apartments where there is no tenant:
 id | apartment_number | bedrooms | bathrooms |  address   | tenant | occupied | sq_ft | price 
----+------------------+----------+-----------+------------+--------+----------+-------+-------
  3 |              303 |        1 |         1 | 789 Elm Rd |        | f        |   600 |  1100
(1 row)

Names of all companies:
   company   
-------------
 Tech Corp
 Finance Inc
 
(3 rows)

Number of cubicles and bathrooms in the 3rd office:
 cubicles | bathrooms 
----------+-----------
       10 |         1
(1 row)

Storefronts that have kitchens:
 id |      address      | occupied | price | kitchen | sq_ft |        owner         | outdoor_seating 
----+-------------------+----------+-------+---------+-------+----------------------+-----------------
  1 | 10 Shopping Plaza | t        |  3000 | t       |  1500 | Restaurant Group LLC | t
  3 | 30 Market St      | t        |  4000 | t       |  2000 | Cafe Holdings        | t
(2 rows)

Storefront with highest square footage and outdoor seating:
 id |   address    | occupied | price | kitchen | sq_ft |     owner     | outdoor_seating 
----+--------------+----------+-------+---------+-------+---------------+-----------------
  3 | 30 Market St | t        |  4000 | t       |  2000 | Cafe Holdings | t
(1 row)

Office with lowest number of cubicles:
 id | office_number | floors | sq_ft | cubicles | bathrooms |   address    | company | occupied | price 
----+---------------+--------+-------+----------+-----------+--------------+---------+----------+-------
  3 |           300 |      1 |  2000 |       10 |         1 | 300 Trade St |         | f        |  3500
(1 row)

Office with most cubicles and bathrooms:
 id | office_number | floors | sq_ft | cubicles | bathrooms |     address     |   company   | occupied | price 
----+---------------+--------+-------+----------+-----------+-----------------+-------------+----------+-------
  2 |           200 |      3 |  5000 |       25 |         3 | 200 Commerce Dr | Finance Inc | t        |  8000
(1 row)

Office with most cubicles and bathrooms:
 id | office_number | floors | sq_ft | cubicles | bathrooms |     address     |   company   | occupied | price 
----+---------------+--------+-------+----------+-----------+-----------------+-------------+----------+-------
  2 |           200 |      3 |  5000 |       25 |         3 | 200 Commerce Dr | Finance Inc | t        |  8000
(1 row)
--enter your seed data below
INSERT INTO apartments (apartment_number, bedrooms, bathrooms, address, tenant, occupied, sq_ft, price)
VALUES
    (101, 2, 1, '123 Main St', 'John Smith', true, 850, 1500),
    (202, 3, 2, '456 Oak Ave', 'Jane Doe', true, 1200, 2200),
    (303, 1, 1, '789 Elm Rd', NULL, false, 600, 1100);

INSERT INTO offices (office_number, floors, sq_ft, cubicles, bathrooms, address, company, occupied, price)
VALUES
    (100, 2, 3000, 15, 2, '100 Business Blvd', 'Tech Corp', true, 5000),
    (200, 3, 5000, 25, 3, '200 Commerce Dr', 'Finance Inc', true, 8000),
    (300, 1, 2000, 10, 1, '300 Trade St', NULL, false, 3500);

INSERT INTO storefronts (address, occupied, price, kitchen, sq_ft, owner, outdoor_seating)
VALUES
    ('10 Shopping Plaza', true, 3000, true, 1500, 'Restaurant Group LLC', true),
    ('20 Retail Row', false, 2500, false, 1200, 'Boutique Owner', false),
    ('30 Market St', true, 4000, true, 2000, 'Cafe Holdings', true);
