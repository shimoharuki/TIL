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
