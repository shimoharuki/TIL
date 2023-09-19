# chapter 2

問題1
```
SELECT  name, seat_number
FROM rooms
WHERE seat_number >  (SELECT seat_number FROM rooms WHERE name = 701)
ORDER BY  seat_number ASC ;
```
