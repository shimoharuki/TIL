# SQL問題解答
問題１

```
SELECT  code FROM areas WHERE name = "大阪" ;
```

問題2

```
SELECT name,code FROM areas WHERE name IN ("高田馬場","大阪","京都 ");
```

問題３
```
SELECT name,code FROM areas WHERE code BETWEEN 01 AND 11;
```
問題４

```
SELECT name FROM areas WHERE name  LIKE "大%";
```

問題５
```
SELECT name FROM areas WHERE name  LIKE "%場";
```

問題6

```
SELECT name FROM areas WHERE name  LIKE "%久%";
```

問題７
```
SELECT name FROM areas WHERE name  LIKE "高%場" ;
```
問題8

```
SELECT name
FROM areas  
WHERE  LOCATE ('大',name) < LOCATE ('保',name) ;
```

問題９

```
SELECT name
FROM areas  
WHERE  name LIKE '_田%' ;
```

問題10

```
SELECT *
FROM areas  
WHERE LENGTH(name) = 4 ;
```

問題11

```
SELECT code , name
FROM roles
GROUP BY code ,name
```

問題12 
```
SELECT name
FROM courses
WHERE price >= 120000
```
問題13
```
SELECT price /10000  '万円'
FROM courses
WHERE name = "予備総合コース";
```

問題14
```
SELECT name , code
FROM roles
WHERE code IN ('STAFF_ADMIN', 'MASTER_CONTROL','STUDENT_PRIVACY_ACCESSIBLE')
```
問題15

```
SELECT name
FROM courses
WHERE name LIKE "%対策%
```

問題16
```
SELECT name , price 
FROM courses
WHERE price >= 1000000 OR LENGTH (name) >= 50
```
問題17 
```
SELECT  ROUND (price / 100000,  1 )
FROM courses
WHERE name = "面接試験対策直前特訓"
```
問題18
```
SELECT code , name
FROM roles
WHERE LENGTH (code) = LENGTH (name)
```
問題19
```
SELECT  name
FROM courses
WHERE name LIKE '予%' AND name <> '予備総合コース'
```
問題20
```
SELECT  code 
FROM roles
WHERE code LIKE '%A%'
AND code LIKE '%I%'
AND code LIKE '%U%'
AND code LIKE '%E%'
AND code NOT LIKE '%O%'
AND code NOT LIKE '%P%'
```
問題21
```
SELECT  student_id ,total
FROM student_points
WHERE total >= 7000
```

問題22
```
SELECT  student_id ,total,used
FROM student_points
WHERE total >= 7000 AND used <= 2000
```
問題23

```
SELECT  tag_id
FROM taggings
WHERE taggable_type = 'Lecture' AND taggable_id >= 6066
```
問題24 
```
SELECT  *
FROM subject_types
WHERE name IN ('通訳不要', '通訳必要', '通訳手配済み','デフォルト')

```
問題25
```
SELECT  description
FROM subject_types
WHERE description LIKE '通訳が必要でない%'
```
問題26 
```
SELECT  *
FROM rooms 
WHERE description = 'classroom 301' AND name = 301
OR description = 'classroom 305' AND name = 305
```
問題27 
```
SELECT  *
FROM rooms 
WHERE name = 201 AND description NOT LIKE '%土日%' AND description NOT LIKE '%視聴覚室予定%'
```
問題28
```
SELECT  name ,seat_number 
FROM rooms
WHERE name LIKE '20%'
ORDER BY seat_number DESC, name ASC
```
