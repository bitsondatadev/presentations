# Query Federation Demo

-vertical

## Query Federation 

<ul>
  <li>Two datasets: Pok&eacute;mon pokedex and Pok&eacute;mon Go spawn data</li>
  <li>Pok&eacute;mon pokedex data is stored in MongoDB</li>
  <li>Pok&eacute;mon Go spawn data is stored in Postgres</li>
  <li>Find the central point of all Pok&eacute;mon of an electric type.</li>
  <!-- .element class="fragment" -->
  <li>What are some approaches to answer this query without using federation?</li>
  <!-- .element class="fragment" -->
</ul>

```sql
SELECT
  p.name, 
  AVG(s.lat) AS avg_lat, 
  AVG(s.long) AS avg_long
FROM mongo.pokemon.pokedex AS p
  JOIN postgres.pokemon.spawns AS s 
    ON p.number = s.num
WHERE p.type_1 = 'Electric'
GROUP BY p.name;
```
<!-- .element class="fragment" -->


-vertical

## Saving and updating your results 

What if you want to do this on regularly incoming data?
<!-- .element class="fragment" -->

-vertical

## Saving and updating your results 

```sql
CREATE TABLE iceberg.pokemon.electric_spawns(
    number integer,
    name varchar,
    avg_lat double,
    avg_long double
);
```

-vertical

## Saving and updating your results 

```sql
MERGE INTO iceberg.pokemon.electric_spawns t USING (
  SELECT
    number,
    p.name,
    AVG(s.lat) AS avg_lat,
    AVG(s.long) AS avg_long
  FROM mongo.pokemon.pokedex AS p
    JOIN postgres.pokemon.spawns AS s
      ON p.number = s.num
  WHERE p.type_1 = 'Electric'
  GROUP BY p.number, p.name 
) AS s ON (s.name = t.name)
WHEN MATCHED
  THEN UPDATE SET lat = s.lat, long = s.long
WHEN NOT MATCHED
  THEN INSERT (number, name, lat, long)
        VALUES (s.number, s.name, s.lat, s.long);
```

-vertical

## Saving and updating your results 

```sql
SELECT 
  name, 
  AVG(s.lat) AS avg_lat, 
  AVG(s.long) AS avg_long
FROM electric_spawns
GROUP BY p.name;
```

-vertical

## But wait...

Won't this put a lot of strain on your production databases?

<!-- .element class="fragment" -->

![](./images/silence.jpg) <!-- .element width="40%" -->

<!-- .element class="fragment" -->

-vertical

### Create Copies of your source databases in the lakehouse

```sql
CREATE TABLE iceberg.pokemon.pokedex AS
SELECT * FROM mongo.pokemon.pokedex;
```

```sql
CREATE TABLE iceberg.pokemon.spawns(
   id bigint,
   num integer,
   name varchar,
   lat double,
   long double,
   encounter_ms bigint,
   disappear_ms bigint
);
```

-vertical

### Use `MERGE` to ingest spawn data 

```sql
MERGE INTO iceberg.pokemon.spawns t USING 
(
  SELECT *
  FROM postgres.pokemon.spawns
  WHERE encounter_ms > to_unixtime(current_date - interval '1' day) AND 
        encounter_ms < to_unixtime(current_date + interval '1' day)
) AS s ON (s.id = t.id)
WHEN NOT MATCHED
  THEN INSERT (id, num, name, lat, long, encounter_ms, disappear_ms)
        VALUES (s.id, s.num, s.name, s.lat, s.long, s.encounter_ms, s.disappear_ms);
```
