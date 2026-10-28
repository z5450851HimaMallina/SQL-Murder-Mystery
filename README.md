# SQL-Murder-Mystery
<img width="1119" height="1051" alt="image" src="https://github.com/user-attachments/assets/65878a31-0df8-4bce-9a47-31de500ee79b" />

A Data Investigation
SQL Murder Mystery: A Data Investigation

Project Overview

This project uses SQL to investigate a fictional murder in SQL City. By querying a relational database, I examined crime scene reports, witness statements, gym memberships, check-in records, driver’s licences, and income data to identify the murderer and uncover the mastermind behind the crime.

Tools: SQL (SQLite)
Project: SQL Murder Mystery
Key skills: Data querying, filtering, pattern matching, relational data investigation, and evidence-based reasoning.

Spoiler warning: This project documents the investigation and its conclusion.

⸻

1. Investigating the Crime Scene

I started by searching for murder reports in SQL City.

SELECT *
FROM crime_scene_report
WHERE city = 'SQL City'
  AND type = 'murder';

The results included a report dated 15 January 2018. The report indicated that there were two witnesses:

* The first witness lived at the last house on Northwestern Drive.
* The second witness, Annabel, lived on Franklin Avenue.

Finding the witnesses

I searched for Annabel by name.

SELECT *
FROM person
WHERE name LIKE '%Annabel%';

I then searched for the person living at the highest house number on Northwestern Drive.

SELECT *
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;

These queries helped identify the two witnesses:

* Morty Schapiro
* Annabel Miller

⸻

2. Examining Witness Interviews

I retrieved the interview records for the identified witnesses.

SELECT *
FROM interview
WHERE person_id IN (14887, 16371);

Key evidence

Morty Schapiro reported hearing a gunshot and seeing a man leave the scene. The man carried a Get Fit Now Gym bag with a membership number beginning with 48Z. Morty also remembered part of the vehicle’s registration plate: H42W.

Annabel Miller reported recognising the killer from her gym. She had seen him working out on 9 January 2018.

These statements provided several leads for further investigation.

⸻

3. Investigating Gym Memberships

Morty’s statement suggested that the suspect was a gold member of Get Fit Now Gym with a membership ID beginning with 48Z.

I searched for matching memberships.

SELECT *
FROM get_fit_now_member
WHERE membership_status = 'gold'
  AND id LIKE '48Z%';

The results identified two potential members:

* Joe Germuska
* Jeremy Bowers

Checking gym attendance

I checked whether these members had visited the gym on 9 January 2018.

SELECT *
FROM get_fit_now_check_in
WHERE check_in_date = 20180109
  AND membership_id IN ('48Z7A', '48Z55');

Both members had checked into the gym that day.

The membership records and attendance data provided additional evidence to investigate alongside the witness statements.

⸻

4. Investigating Vehicle Registration Records

Morty remembered that the vehicle’s registration plate contained H42W.

I searched the driver’s licence database for vehicles with matching registration details.

SELECT *
FROM drivers_license
WHERE plate_number LIKE '%H42W%';

The results identified several possible vehicles and their associated licence records.

Identifying the vehicle owners

I used the licence IDs to find the corresponding people.

SELECT *
FROM person
WHERE license_id IN (
    118009,
    423327,
    183779,
    664760
);

Cross-referencing the vehicle and person records helped narrow down the potential suspects.

⸻

5. Identifying the Murderer

After reviewing the witness statements, gym membership records, check-in dates, and vehicle information, I investigated Jeremy Bowers further.

I retrieved his interview transcript.

SELECT *
FROM interview
WHERE person_id = 67318;

Jeremy’s statement revealed that he had been hired by a wealthy woman. He described her physical appearance, vehicle, and attendance at the SQL Symphony Concert.

These details provided new leads for identifying the person behind the crime.

Finding: Jeremy Bowers was identified as the murderer.

⸻

6. Investigating the Mastermind

Jeremy’s interview described the woman who hired him as:

* Wealthy
* Red-haired
* The owner of a Tesla Model S
* Approximately 5’5” or 5’7” tall
* Someone who attended the SQL Symphony Concert three times in December 2017

Searching driver’s licence records

I searched for red-haired individuals who drove Teslas.

SELECT *
FROM drivers_license
WHERE hair_color = 'red'
  AND car_make = 'Tesla'
  AND car_model = 'Model S';

I then retrieved the corresponding people.

SELECT *
FROM person
WHERE license_id IN (
    202298,
    291182,
    736081,
    918773
);

Investigating income records

To compare the candidates’ financial circumstances, I searched the income table using their Social Security numbers.

SELECT *
FROM income
WHERE ssn IN (
    '924648898',
    '961388910',
    '337169072',
    '987756388'
);

The results helped me investigate which candidate matched the description of a wealthy woman.

Checking event attendance

I also examined the event check-in records for the candidate matching the description.

SELECT *
FROM facebook_event_checkin
WHERE person_id = 99716;

The event records provided another way to investigate the claim in Jeremy’s interview.

Finding: Miranda Priestly was identified as the mastermind behind the murder.

⸻

7. Final Verification

The SQL Murder Mystery game provides a solution table for checking the answers.

The first answer identified Jeremy Bowers as the murderer.

The second answer identified Miranda Priestly as the mastermind.

Both answers were accepted by the game.

⸻



8. Key SQL Concepts Practised

SQL concept	Application
SELECT	Retrieve records from database tables
WHERE	Filter records using specific conditions
AND	Apply multiple filtering conditions
IN	Match values against a list of IDs
LIKE	Search for partial text matches
ORDER BY	Sort records by a selected column
LIMIT	Restrict the number of returned rows
Cross-table investigation	Connect people, interviews, memberships, and vehicle records

Conclusion

This investigation demonstrated how SQL can be used to explore relational data, test witness statements against database records, narrow down potential suspects, and build an evidence-based conclusion.

Rather than relying on a single record, I used multiple sources of information to investigate the case.

The exercise strengthened my understanding of SQL filtering, pattern matching, sorting, and structured investigation.
