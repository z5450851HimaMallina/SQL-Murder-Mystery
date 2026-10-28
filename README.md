# SQL Murder Mystery: A Data Investigation
<img width="1119" height="1051" alt="image" src="https://github.com/user-attachments/assets/65878a31-0df8-4bce-9a47-31de500ee79b" />
<img width="1070" height="230" alt="image" src="https://github.com/user-attachments/assets/ec676ecc-d104-4917-a45d-bc73852a2da5" />
<img width="1033" height="208" alt="image" src="https://github.com/user-attachments/assets/cdfc5361-7cae-4c4a-97bc-cd1d6bc2cb2f" />

## Project Overview

This project documents my investigation of a fictional murder in SQL
City using SQL. I queried crime reports, witness interviews, gym
membership records, check-in logs, driver's licence data, and income
records to follow the evidence and identify the people responsible.

-   **Database:** SQLite
-   **Project:** [SQL Murder Mystery](https://mystery.knightlab.com/)
-   **Skills practised:** SQL filtering, pattern matching, sorting,
    limiting results, and investigating related records across tables

> **Spoiler warning:** This README contains the solution to the game.

------------------------------------------------------------------------

## 1. Investigate the Crime Scene

I began by searching for murder reports in SQL City.

``` sql
SELECT *
FROM crime_scene_report
WHERE city = 'SQL City'
  AND type = 'murder';
```

The relevant report was dated **15 January 2018**. It mentioned two
witnesses:

-   The first witness lived at the last house on Northwestern Drive.
-   The second witness, Annabel, lived somewhere on Franklin Avenue.

### Find the witnesses

Search for Annabel by name:

``` sql
SELECT *
FROM person
WHERE name LIKE '%Annabel%';
```

Find the person living at the highest house number on Northwestern
Drive:

``` sql
SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;
```

These searches identified the witnesses as **Morty Schapiro** and
**Annabel Miller**.

------------------------------------------------------------------------

## 2. Read the Witness Interviews

I retrieved the interview records for both witnesses.

``` sql
SELECT *
FROM interview
WHERE person_id IN (14887, 16371);
```

### Morty Schapiro's statement

Morty reported hearing a gunshot and seeing a man leave the scene. The
man carried a Get Fit Now Gym bag with a membership number beginning
with `48Z`. Morty also remembered that the vehicle's registration plate
contained `H42W`.

### Annabel Miller's statement

Annabel said she recognised the killer from her gym and had seen him
working out on 9 January 2018.

These statements gave me leads to check against the gym and vehicle
records.

------------------------------------------------------------------------

## 3. Investigate Gym Memberships

Morty's statement suggested that the person he saw was a gold member of
Get Fit Now Gym with a membership ID beginning with `48Z`.

``` sql
SELECT *
FROM get_fit_now_member
WHERE membership_status = 'gold'
  AND id LIKE '48Z%';
```

The results returned two matching memberships: `48Z7A` and `48Z55`.

### Check gym attendance

I checked whether either member visited the gym on 9 January 2018.

``` sql
SELECT *
FROM get_fit_now_check_in
WHERE check_in_date = 20180109
  AND membership_id IN ('48Z7A', '48Z55');
```

Both membership IDs appeared in the check-in records for that date. I
used the membership records and the witness statements together to
narrow the investigation.

------------------------------------------------------------------------

## 4. Investigate the Vehicle

Morty remembered a registration plate containing `H42W`. I searched for
matching vehicle records.

``` sql
SELECT *
FROM drivers_license
WHERE plate_number LIKE '%H42W%';
```

I then used the matching licence IDs to find the associated people.
Replace the IDs below with the licence IDs returned by your own query if
your results differ.

``` sql
SELECT *
FROM person
WHERE license_id IN (
    118009,
    423327,
    183779,
    664760
);
```

Cross-referencing the vehicle records with the person table helped
identify potential suspects.

------------------------------------------------------------------------

## 5. Identify the Murderer

I queried Jeremy Bowers's interview record to examine his statement.

``` sql
SELECT *
FROM interview
WHERE person_id = 67318;
```

Jeremy said that he had been hired by a wealthy woman. He described her
appearance, vehicle, and attendance at the SQL Symphony Concert.

After comparing the available evidence, I submitted **Jeremy Bowers** as
the murderer. The game accepted the answer.

------------------------------------------------------------------------

## 6. Investigate the Mastermind

Jeremy's interview described the woman who hired him as:

-   Wealthy
-   Red-haired
-   The driver of a Tesla Model S
-   Approximately 65 or 67 inches tall
-   Someone who attended the SQL Symphony Concert three times in
    December 2017

### Search vehicle and appearance records

I searched for red-haired people associated with a Tesla Model S.

``` sql
SELECT *
FROM drivers_license
WHERE hair_color = 'red'
  AND car_make = 'Tesla'
  AND car_model = 'Model S';
```

### Match licence records to people

I used the licence IDs returned by the previous query to find the
corresponding people.

``` sql
SELECT *
FROM person
WHERE license_id IN (
    202298,
    291182,
    736081,
    918773
);
```

### Compare income records

To investigate the candidates' financial circumstances, I searched the
income table using their Social Security numbers.

``` sql
SELECT *
FROM income
WHERE ssn IN (
    '924648898',
    '961388910',
    '337169072',
    '987756388'
);
```

### Check event attendance

I checked the event records for the candidate I was investigating.

``` sql
SELECT *
FROM facebook_event_checkin
WHERE person_id = 99716;
```

I reviewed the returned records to see whether they supported the
attendance detail in Jeremy's statement.

After submitting the second answer, the game accepted **Miranda
Priestly** as the mastermind.

------------------------------------------------------------------------

## 7. Final Results

  Role         Person identified   Verification
  ------------ ------------------- ----------------------
  Murderer     Jeremy Bowers       Accepted by the game
  Mastermind   Miranda Priestly    Accepted by the game

The conclusions above are the answers accepted by the SQL Murder Mystery
game.

------------------------------------------------------------------------

## 8. SQL Concepts Practised

  -----------------------------------------------------------------------
  SQL concept                         How I used it
  ----------------------------------- -----------------------------------
  `SELECT`                            Retrieve records from a table

  `WHERE`                             Filter records using conditions

  `AND`                               Apply multiple conditions together

  `IN`                                Match a column against a list of
                                      values

  `LIKE`                              Search for partial text matches

  `ORDER BY`                          Sort results by a column

  `DESC`                              Sort in descending order

  `LIMIT`                             Return a specified number of rows

  Cross-table investigation           Use related identifiers to
                                      investigate people, interviews,
                                      memberships, vehicles, and income
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## Conclusion

This project gave me practical experience using SQL to investigate a
question through structured data. I combined witness statements with
records from several tables, used filtering and pattern matching to
narrow down possible leads, and checked my final conclusions in the
game.

The main lesson was to treat each statement as a lead to verify rather
than as proof on its own. Comparing multiple records helped me build a
more structured investigation.

## Project Credit

The database and interactive challenge are provided by [SQL Murder
Mystery](https://mystery.knightlab.com/).
