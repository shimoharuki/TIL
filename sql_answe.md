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
