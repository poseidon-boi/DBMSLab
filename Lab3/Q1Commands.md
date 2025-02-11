# LAB 3

## Creating tables:

~~~~sql
CREATE TABLE PERSON (driver_id# varchar(30), PRIMARY KEY (driver_id#), name varchar(50), address varchar(100));
CREATE TABLE CAR (regno varchar(20), model varchar(30), year int, PRIMARY KEY (regno));
CREATE TABLE ACCIDENT (report_number int, accd_date date, location varchar(50), PRIMARY KEY (report_number));
CREATE TABLE OWNS (DRIVER_ID# varchar(30), regno varchar(20), PRIMARY KEY (DRIVER_ID#, REGNO),  FOREIGN KEY(REGNO) REFERENCES CAR, FOREIGN KEY (DRIVER_ID#) REFERENCES PERSON);
CREATE TABLE PARTICIPATED (driver_id# varchar(30), regno varchar(20), report_number int, damage_amount int, PRIMARY KEY (DRIVER_ID#, REGNO, REPORT_NUMBER), FOREIGN KEY (DRIVER_ID#) REFERENCES PERSON, FOREIGN KEY (REGNO) REFERENCES CAR, FOREIGN KEY (REPORT_NUMBER) REFERENCES ACCIDENT);
~~~~

## Populating tables:

~~~~sql
INSERT INTO PERSON VALUES (1234567891, 'Verma', 'Gorakhpur');
INSERT INTO PERSON VALUES (2345678912, 'Pavit', 'Bihar');
INSERT INTO PERSON VALUES (3456789123, 'Luke', 'Mangaluru');
INSERT INTO PERSON VALUES (4567891234, 'Aryan', 'Bengaluru');
INSERT INTO PERSON VALUES (5678912345, 'Seven', 'Hyderabad');

INSERT INTO CAR VALUES ('KA01ER1234', 'ABC34321', 2019);
INSERT INTO CAR VALUES ('HP33ER8777', 'DDS63132', 2019);
INSERT INTO CAR VALUES ('KA23TT9876', 'SSF35287', 2024);
INSERT INTO CAR VALUES ('TN50BE2341', 'GWE53290', 2023);
INSERT INTO CAR VALUES ('CH54FD9830', 'BGJ63428', 2015);

INSERT INTO ACCIDENT VALUES (46321545, '29-APR-2022', 'Gorakhpur');
INSERT INTO ACCIDENT VALUES (78426213, '29-APR-2023', 'Hyderabad');
INSERT INTO ACCIDENT VALUES (65948321, '29-APR-2024', 'Manipal');
INSERT INTO ACCIDENT VALUES (59648322, '29-APR-2021', 'Bengaluru');
INSERT INTO ACCIDENT VALUES (12, '29-MAY-2023', 'Gorakhpur');

INSERT INTO OWNS VALUES (1234567891, 'HP33ER8777');
INSERT INTO OWNS VALUES (3456789123, 'KA01ER1234');
INSERT INTO OWNS VALUES (4567891234, 'KA23TT9876');
INSERT INTO OWNS VALUES (5678912345, 'TN50BE2341');
INSERT INTO OWNS VALUES (2345678912, 'CH54FD9830');

INSERT INTO PARTICIPATED VALUES (1234567891, 'HP33ER8777', 46321545, 10000);
INSERT INTO PARTICIPATED VALUES (1234567891, 'KA01ER1234', 65948321, 20000);
INSERT INTO PARTICIPATED VALUES (1234567891, 'KA23TT9876', 59648322, 33000);
INSERT INTO PARTICIPATED VALUES (2345678912, 'CH54FD9830', 12, 200000);
INSERT INTO PARTICIPATED VALUES (5678912345, 'TN50BE2341', 78426213, 1500);
~~~~

## Tables after inserting are as follows:

* PERSON:
```
DRIVER_ID#
------------------------------
NAME
--------------------------------------------------
ADDRESS
--------------------------------------------------------------------------------
1234567891
Verma
Gorakhpur

2345678912
Pavit
Bihar

DRIVER_ID#
------------------------------
NAME
--------------------------------------------------
ADDRESS
--------------------------------------------------------------------------------

3456789123
Luke
Mangaluru

4567891234
Aryan

DRIVER_ID#
------------------------------
NAME
--------------------------------------------------
ADDRESS
--------------------------------------------------------------------------------
Bengaluru

5678912345
Seven
Hyderabad
```

* CAR:
```
REGNO                MODEL                                YEAR
-------------------- ------------------------------ ----------
KA01ER1234           ABC34321                             2019
HP33ER8777           DDS63132                             2019
KA23TT9876           SSF35287                             2024
TN50BE2341           GWE53290                             2023
CH54FD9830           BGJ63428                             2015
```

* ACCIDENT:
```
REPORT_NUMBER ACCD_DATE LOCATION
------------- --------- --------------------------------------------------
     46321545 29-APR-22 Gorakhpur
     78426213 29-APR-23 Hyderabad
     65948321 29-APR-24 Manipal
     59648322 29-APR-21 Bengaluru
           12 29-MAY-23 Gorakhpur
```

* OWNS:
```
DRIVER_ID#                     REGNO
------------------------------ --------------------
1234567891                     HP33ER8777
2345678912                     CH54FD9830
3456789123                     KA01ER1234
4567891234                     KA23TT9876
5678912345                     TN50BE2341
```

* PARTICIPATED:
```
DRIVER_ID#                     REGNO                REPORT_NUMBER DAMAGE_AMOUNT
------------------------------ -------------------- ------------- -------------
1234567891                     HP33ER8777                46321545         10000
1234567891                     KA01ER1234                65948321         20000
1234567891                     KA23TT9876                59648322         33000
2345678912                     CH54FD9830                      12        200000
5678912345                     TN50BE2341                78426213          1500
```

## Update query:

~~~~sql
UPDATE PARTICIPATED SET DAMAGE_AMOUNT = 25000 WHERE (REPORT_NUMBER = 12 AND REGNO = 'CH54FD9830');
~~~~

* PARTICIPATED:
```
DRIVER_ID#                     REGNO                REPORT_NUMBER DAMAGE_AMOUNT
------------------------------ -------------------- ------------- -------------
1234567891                     HP33ER8777                46321545         10000
1234567891                     KA01ER1234                65948321         20000
1234567891                     KA23TT9876                59648322         33000
2345678912                     CH54FD9830                      12         25000
5678912345                     TN50BE2341                78426213          1500
```

## Delete accident information from 2023:

~~~~sql
DELETE FROM PARTICIPATED WHERE REPORT_NUMBER IN (SELECT REPORT_NUMBER FROM ACCIDENT WHERE EXTRACT(YEAR FROM ACCD_DATE) = 2023);
DELETE FROM ACCIDENT WHERE EXTRACT(YEAR FROM ACCD_DATE) = 2023;
~~~~

* PARICIPATED:
```
DRIVER_ID#                     REGNO                REPORT_NUMBER DAMAGE_AMOUNT
------------------------------ -------------------- ------------- -------------
1234567891                     HP33ER8777                46321545         10000
1234567891                     KA01ER1234                65948321         20000
1234567891                     KA23TT9876                59648322         33000
```

* ACCIDENT:
```
REPORT_NUMBER ACCD_DATE LOCATION
------------- --------- --------------------------------------------------
     46321545 29-APR-22 Gorakhpur
     65948321 29-APR-24 Manipal
     59648322 29-APR-21 Bengaluru
```

## Alter table to add and delete a column:

~~~~sql
ALTER TABLE OWNS ADD NAME VARCHAR(20);
ALTER TABLE OWNS DROP COLUMN NAME;
~~~~

* OWNS on adding column:
```
DRIVER_ID#                     REGNO                NAME
------------------------------ -------------------- --------------------
1234567891                     HP33ER8777
3456789123                     KA01ER1234
4567891234                     KA23TT9876
5678912345                     TN50BE2341
2345678912                     CH54FD9830
```

## Add constraint to table:

~~~~sql
ALTER TABLE OWNS ADD CONSTRAINT CHECK_ID CHECK (DRIVER_ID# > 1000000000 AND DRIVER_ID# < 9999999999);
~~~~