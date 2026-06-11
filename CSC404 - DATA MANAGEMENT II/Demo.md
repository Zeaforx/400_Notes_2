### College

| |**cName**|**state**|**enrollment**|
|---|---|---|---|
|1|Stanford|CA|15000|
|2|Berkely|CA|36000|
|3|MIT|MA|10000|
|4|Cornell|NY|21000|

### Student

| |**sID**|**sName**|**GPA**|**sizeHS**|
|---|---|---|---|---|
|1|123|Amy|3.9|1000|
|2|234|Bob|3.6|1500|
|3|345|Craig|3.5|500|
|4|456|Doris|3.9|1000|
|5|567|Edward|2.9|2000|
|6|678|Fay|3.8|200|
|7|789|Gary|3.4|800|
|8|987|Helen|3.7|800|
|9|876|Irene|3.9|400|
|10|765|Jay|2.9|1500|
|11|654|Amy|3.9|1000|
|12|543|Craig|3.4|2000|

### Apply

| |**sID**|**cName**|**major**|**decision**|
|---|---|---|---|---|
|1|123|Stanford|CS|Y|
|2|123|Stanford|EE|N|
|3|123|Berkeley|CS|Y|
|4|123|Cornell|EE|Y|
|5|234|Berkeley|biology|N|
|6|345|MIT|bioengineering|Y|
|7|345|Cornell|bioengineering|N|
|8|345|Cornell|CS|Y|
|9|345|Cornell|EE|N|
|10|678|Stanford|history|Y|
|11|987|Stanford|CS|Y|
|12|987|Berkeley|CS|Y|
|13|876|Stanford|CS|N|
|14|876|MIT|biology|Y|
|15|876|MIT|marine biology|N|
|16|765|Stanford|history|Y|
|17|765|Cornell|history|N|
|18|765|Cornell|psychology|Y|
|19|543|MIT|CS|N|

# 1. 
SELECT sName, sID, GPA
FROM Student
WHERE GPA > 2.0

# 2.
SELECT sName, major, sID
FROM Student, Apply
WHERE Student.sID = Apply.sID

## 3.
SELECT DISTINCT sName, major
FROM Student, Apply
WHERE Student.sID = Apply.sID

# 4. 
SELECT sName, decisions, gpa
FROM Student, Apply
WHERE Student.sID = Apply.sID AND HSSize < 1000 AND cName = Stanford AND major = CS

# 5.
SELECT College.cName
FROM Apply, College
WHERE College.cName = Apply.cName AND enrollment > 20000 AND major = CS

# 6.

SELECT DISTINCT College.cName
FROM Apply, College
WHERE College.cName = Apply.cName AND enrollment > 20000 AND major = CS

# 7.
SELECT Student.sID sName GPA College.cName enrollment
FROM Apply Student College
WHERE Student.sID = Apply.sID AND Apply.cName = College.cName 

# 8.
SELECT Student.sID sName GPA College.cName enrollment
FROM Apply Student College
WHERE Student.sID = Apply.sID AND Apply.cName = College.cName 
ORDER BY GPA desc, enrollment asc

# 9.
SELECT sID sName
FROM Apply Student
Where Student.sID = Apply.sID AND major like "%bio%"

note that prefix = "bio%" and suffix = "%bio"

# 10
SELECT *
FROM Apply Student
Where Student.sID = Apply.sID AND major like "%bio%"

# 11
SELECT *
FROM Apply Student

# 12
SELECT sID, sName, GPA, sizeHS, GPA * (sizeHS / 1000 )
FROM  Student

# 13

SELECT sID, sName, GPA, sizeHS, GPA * (sizeHS / 1000 ) as scaledGPA
FROM  Student

# 1
SELECT Student.sID sName GPA College.cName enrollment
FROM Apply Student College
WHERE Student.sID = Apply.sID AND Apply.cName = College.cName 

# 2

SELECT S.sID sName GPA C.cName enrollment
FROM Apply A, Student S, College C
WHERE S.sID = A.sID AND A.cName = C.cName 

# 3.
SELECT S1.sID, S1.GPA, S1.sName, S2.sID, S2.GPA, S2.sName
FROM Student S1, Student S2
WHERE S1.GPA = S2.GPA 


# 4.


SELECT S1.sID, S1.GPA, S1.sName, S2.sID, S2.GPA, S2.sName
FROM Student S1, Student S2
WHERE S1.GPA = S2.GPA AND S1.sID <> S2.sID

# 5
SELECT S1.sID, S1.GPA, S1.sName, S2.sID, S2.GPA, S2.sName
FROM Student S1, Student S2
WHERE S1.GPA = S2.GPA AND S1.sID < S2.sID




# 6
SELECT cName FROM  College
UnION
SELECT ScName FROM Student

# 7. 
SELECT cName as NameFROM  College
UnION
SELECT ScName as Name FROM Student