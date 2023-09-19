# chapter 3

問題1
```
INSERT INTO provinces (name,code)
VALUES ('奈良','sh')
```

問題2
```
INSERT INTO roles (name,code)
VALUES ('営業管理','SALES_ADMIN')
```
問題3
```
UPDATE provinces 
SET name = '宮崎', code = 'MY'
WHERE name = '奈良' AND code = 'sh'
```

問題4
```
UPDATE roles
SET name = '営業管理者'
WHERE name = '営業管理' AND code = 'SALES_ADMIN'
```
