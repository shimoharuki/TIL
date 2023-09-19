# chapter 2

問題1
```
SELECT  name, seat_number
FROM rooms
WHERE seat_number >  (SELECT seat_number FROM rooms WHERE name = 701)
ORDER BY  seat_number ASC ;
```
問題2
```
SELECT  name, seat_number
FROM rooms
WHERE seat_number = (SELECT seat_number FROM rooms WHERE name = 701)
ORDER BY  seat_number ASC ;
```
問題3
```
SELECT  name,seat_number ,building_id
FROM rooms AS T1
WHERE seat_number  = 
(SELECT MAX(seat_number)
 FROM rooms AS T2
 WHERE T1.building_id = T2.building_id
)
```

問題4
```
SELECT SUM(sign_in_count)
FROM users
```
問題5
```
SELECT DISTINCT sex
FROM students
```
問題6

```
SELECT COUNT(sex)
FROM students
WHERE sex = 1
```
問題7 
```
SELECT SUM(seat_number)
FROM rooms
WHERE building_id IN (13,14,15)
```
