# SQL Table Relationships

## Learning Goals

- Introducing domain modeling for table relationships

- One to many relationships

- Many to many relationships

- Foreign keys

- The JOIN keyword

## Getting Started

We currently have four tables:

```
cats (id, name)
toys (id, name)
doctors (id, name, specialty)
patients (id, name)
```

Once you've cloned the repository, you'll be able to open it in sqlite3 with `sqlite3 main.db`.

## Terminal Output

```
SQLite version 3.37.2 2022-01-06 13:25:41
Enter ".help" for usage hints.
sqlite> .schema
CREATE TABLE doctors (
id INTEGER PRIMARY KEY,
name TEXT,
specialty TEXT
);
CREATE TABLE patients (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE cats (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE toys (
id INTEGER PRIMARY KEY,
name TEXT
);
sqlite> SELECT * FROM cats;
sqlite> INSERT INTO cats (name) VALUES ("Fiji");
sqlite> INSERT INTO cats (name) VALUES ("Puff");
sqlite> INSERT INTO cats (name) VALUES ("Onyx");
sqlite> SELECT * FROM cats;
1|Fiji
2|Puff
3|Onyx
sqlite> INSERT INTO toys (name) VALUES ("Catnip Rat Type of Thing");
sqlite> SELECT * FROM toys;
1|Catnip Rat Type of Thing
sqlite> INSERT INTO toys (name) VALUES ("Bottle Caps");
sqlite> INSERT INTO toys (name) VALUES ("Lazer Lights");
sqlite> SELECT * FROM toys;
1|Catnip Rat Type of Thing
2|Bottle Caps
3|Lazer Lights
sqlite> ALTER TABLE toys ADD COLUMN cat_id INTEGER;
sqlite> .schema
CREATE TABLE doctors (
id INTEGER PRIMARY KEY,
name TEXT,
specialty TEXT
);
CREATE TABLE patients (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE cats (
id INTEGER PRIMARY KEY,
name TEXT
);
CREATE TABLE toys (
id INTEGER PRIMARY KEY,
name TEXT
, cat_id INTEGER);
sqlite> SELECT * FROM toys WHERE id = 1;
1|Catnip Rat Type of Thing|
sqlite> .headers on
sqlite> .mode columns
sqlite> SELECT * FROM toys WHERE id = 1;
id  name                      cat_id
--  ------------------------  ------
1   Catnip Rat Type of Thing        
sqlite> UPDATE cats SET cat_id = 1;
Error: in prepare, no such column: cat_id (1)
sqlite> UPDATE toys SET cat_id = 1;
sqlite> SELECT * FROM toys WHERE id = 1;
id  name                      cat_id
--  ------------------------  ------
1   Catnip Rat Type of Thing  1     
sqlite> SELECT * FROM toys;
id  name                      cat_id
--  ------------------------  ------
1   Catnip Rat Type of Thing  1     
2   Bottle Caps               1     
3   Lazer Lights              1     
sqlite> UPDATE toys SET cat_id = 2 WHERE id = 2;
sqlite> SELECT * FROM toys;
id  name                      cat_id
--  ------------------------  ------
1   Catnip Rat Type of Thing  1     
2   Bottle Caps               2     
3   Lazer Lights              1     
sqlite> SELECT name FROM toys;
name                    
------------------------
Catnip Rat Type of Thing
Bottle Caps             
Lazer Lights            
sqlite> SELECT toys.name FROM toys;
name                    
------------------------
Catnip Rat Type of Thing
Bottle Caps             
Lazer Lights            
sqlite> SELECT toys.name, cats.name FROM toys
   ...> JOIN cats ON toys.cat_id = cats.id;
name                      name
------------------------  ----
Catnip Rat Type of Thing  Fiji
Bottle Caps               Puff
Lazer Lights              Fiji
sqlite> INSERT INTO toys (name) VALUES ("Cardboard Box");
sqlite> SELECT * FROM toys;
id  name                      cat_id
--  ------------------------  ------
1   Catnip Rat Type of Thing  1     
2   Bottle Caps               2     
3   Lazer Lights              1     
4   Cardboard Box                   
sqlite> SELECT toys.name, cats.name FROM toys
   ...> JOIN cats ON cats.id = toys.cat_id;
name                      name
------------------------  ----
Catnip Rat Type of Thing  Fiji
Bottle Caps               Puff
Lazer Lights              Fiji
sqlite> SELECT toys.name, cats.name FROM toys
   ...> LEFT JOIN cats ON cats.id = toys.cat_id;
name                      name
------------------------  ----
Catnip Rat Type of Thing  Fiji
Bottle Caps               Puff
Lazer Lights              Fiji
Cardboard Box                 
sqlite> CREATE TABLE appointments (
   ...> id INTEGER PRIMARY KEY,
   ...> patient_id INTEGER,
   ...> doctor_id INTEGER
   ...> );
sqlite> SELECT * FROM doctors;
id  name         specialty          
--  -----------  -------------------
1   Strangelove  Nuclear Science    
2   Doolittle    Veterinary Medicine
3   No           unknown            
sqlite> SELECT * FROM patients;
id  name   
--  -------
1   Sakib  
2   Chett  
3   Octavia
4   Ursula 
sqlite> INSERT INTO appointments (doctor_id, patient_id)
   ...> VALUES (2, 1);
sqlite> SELECT doctors.name, patients.name FROM appointments
   ...> JOIN doctors ON doctors.id = appointments.doctor_id
   ...> JOIN patients ON patients.id = appointments.patient_id;
name       name 
---------  -----
Doolittle  Sakib
sqlite> SELECT doctors.id, doctors.name FROM appointments
   ...> JOIN doctors ON doctors.id = appointments.doctor_id
   ...> WHERE appointments.patient_id = 1;
id  name     
--  ---------
2   Doolittle

```

## DBDiagram Examples
[https://dbdiagram.io/d/031124-Relational-Examples-662a75e603593b6b61f8d637](https://dbdiagram.io/d/031124-Relational-Examples-662a75e603593b6b61f8d637)

## Resources

[https://dbdiagram.io](https://dbdiagram.io)