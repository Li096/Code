以下為SQL的內碼：
```htmlembedded=
create database 明新公司; -- 建立資料庫
use 明新公司; --USE使用資料庫

-- 建立部門、員工、產品、銷售資料表
create table 部門
(
部門ID nchar(4) primary key,
部門名稱 nchar(20)
);

create table 員工
(
員工ID nchar(4) primary key,
員工姓名 nchar(20),
性別 nchar(1),
部門名稱 nchar(10),  --不好的設計，但配合課本使用
所屬部門ID nchar(4), --這是FK，正確的設計方式
FOREIGN KEY (所屬部門ID) REFERENCES 部門(部門ID)
);

create table 產品
(
產品ID nchar(4) primary key,
產品名稱 nchar(20),
定價 money
);

create table 銷售
(
員工ID nchar(4),
產品ID nchar(4),
銷售數量 int,
primary key(員工ID, 產品ID),
FOREIGN KEY (員工ID) REFERENCES 員工(員工ID),
FOREIGN KEY (產品ID) REFERENCES 產品(產品ID)
);

-- 插入部門、員工、產品、銷售資料表的資料紀錄，資料插入順序很重要
INSERT INTO 部門
VALUES ('D001','總經理室'),('D002','業務部'),('D003','研發部');

INSERT INTO 員工
VALUES
('E001','Apple','女','業務部','D001'),('E002','Bob','男','研發部','D002'),('E003','Catty','女','製造部','D003'),('E004','David','男','業務部','D001'),
('E005','Frank','男','研發部','D002'),('E006','Emy','女','製造部','D003');

INSERT INTO 員工(員工ID,員工姓名,性別)
VALUES
('N001','AMY臨時人員','女'),('N002','Bob臨時人員','男'),
('N005','Frank臨時人員','男'),('N006','Emy臨時人員','女');

INSERT INTO 產品
VALUES
('P001','手機',2000),('P002','手機',3000),('P003','記憶體',1000),
('P004','鍵盤',500),('P005','滑鼠',100),('P006','螢幕',1500);

INSERT INTO 產品
VALUES
('P007','米老鼠',2000),('P008','大老鼠',3000),('P009','天竺鼠車車',1000),
('P010','鼠來寶',500),('P011','鳳梨鼠鼠',100),('P012','小老鼠',1500);


INSERT INTO 銷售
VALUES
('E001','P001',5),('E001','P002',2),('E001','P003',15),('E001','P004',25),('E001','P005',3),
('E002','P001',6),('E002','P002',15),('E002','P004',25),('E002','P005',25),('E002','P006',3),
('E003','P003',7),('E003','P002',4),('E004','P006',35),('E004','P002',15),('E004','P001',6),
('E005','P005',9),('E005','P003',55),('E005','P006',15),('E006','P001',5),('E006','P004',7);


SELECT * FROM 銷售;

SELECT 員工ID, 性別, 部門名稱  FROM 員工;

SELECT * FROM 員工 WHERE 性別='女';

SELECT * FROM 員工 WHERE 所屬部門ID='D001';

SELECT 員工ID, 性別, 部門名稱  FROM 員工;
SELECT 員工ID AS ID, 性別 AS SEX, 部門名稱 AS DEPT
FROM 員工;

SELECT * FROM 銷售 WHERE 銷售數量>10;
SELECT * FROM 銷售 WHERE NOT 銷售數量>10;

SELECT * FROM 員工 WHERE 性別='女' AND 部門名稱='業務部';
SELECT * FROM 員工 WHERE NOT(性別='女' AND 部門名稱='業務部');
SELECT * FROM 員工 WHERE 性別='女' AND NOT 部門名稱='業務部';
SELECT * FROM 員工 WHERE NOT 性別='女' AND 部門名稱='業務部';

SELECT * FROM 員工 WHERE 性別='女' OR 部門名稱='業務部';
SELECT * FROM 員工 WHERE NOT(性別='女' OR 部門名稱='業務部');
SELECT * FROM 員工 WHERE NOT 性別='女' OR 部門名稱='業務部';
SELECT * FROM 員工 WHERE 性別='女' OR NOT 部門名稱='業務部';

select * from 產品 where 產品名稱 like '%鼠%'
select * from 產品 where 產品名稱 like '鼠%'
select * from 產品 where 產品名稱 like '%鼠' 
select * from 產品 where 產品名稱 like '_鼠%' 

select * from 產品 
where 產品ID IN ('P001','P005','P003')

select * from 產品 
where 產品ID='P001'OR 產品ID='P005' OR 產品ID='P003'

select * from 產品
where 定價 between 100 and 500;

select * from 產品
where 定價 between 100 and 500  and (產品名稱 like '%鼠%');

select * from 產品
where (定價 >= 100 and 定價 <=500)  and (產品名稱 like '%鼠%');

select 產品ID,產品名稱,定價, 定價*1.2 as 售價 from 產品

select 產品ID,產品名稱,定價, 定價*1.2 as 售價
from 產品 where 定價*1.2 < 1000;

alter table 產品
add 成本 money;

update 產品
set 成本=定價*0.8;

INSERT INTO 產品
VALUES
('P013','NVIDA顯示卡',null,null),('P014','NVIDA CPU',null,null);

select *, 定價-成本 as 利潤 
from 產品

select * from 產品
select count(*) as 資料表筆數 from 產品
select count(產品名稱) as 產品名稱筆數 from 產品
select count(定價) as 有定價的筆數 from 產品

select count(*) as 資料表筆數,count(產品名稱) as 產品名稱筆數, count(定價) as 有定價的筆數 
from 產品

select  count(定價) as 有定價的筆數,  avg(定價) as 定價平均值,  sum(定價) as 定價的總合
from 產品

select  count(定價) as 有定價的筆數,  avg(定價) as 定價平均值,  sum(定價) as 定價的總合,
max(定價) as 最高定價,  min(定價) as 最低定價
from 產品

select  count(定價) as 有定價的筆數,  avg(定價) as 定價平均值,  
sum(定價) as 定價的總合, max(定價) as 最高定價,  min(定價) as 最低定價
from 產品
where 產品名稱 like '%鼠%';

select * from 產品
order by 定價;

select * from 產品
order by 定價 desc;


select * from 產品
order by 定價 desc, 製造成本 asc;

alter table 產品
add 產品類型 nchar(10);

drop table

update 產品
set 產品類型 = '電腦零件';

update 產品
set 產品類型 = '玩具'
where 產品名稱 like '%鼠%';

update 產品
set 產品類型 = '高級電腦零件'
where 產品名稱 like '%NVIDA%';

select 產品類型 from 產品 group by 產品類型;

select  產品類型,count(定價) as 有定價的筆數,  avg(定價) as 定價平均值,  
sum(定價) as 定價的總合, max(定價) as 最高定價,  min(定價) as 最低定價
from 產品
group by 產品類型;

select  產品類型,count(定價) as 有定價的筆數,  avg(定價) as 定價平均值,  
sum(定價) as 定價的總合, max(定價) as 最高定價,  min(定價) as 最低定價
from 產品
group by 產品類型
having count(定價)>0;

SELECT * FROM 銷售;
SELECT 員工ID FROM 銷售;
SELECT DISTINCT 員工ID FROM 銷售;

SELECT * 
FROM 部門,員工  --卡氏積PRODUCT, CROSS JOIN

SELECT * 
FROM 部門,員工 
WHERE 部門.部門ID=員工.所屬部門ID  --EQUI JOIN

SELECT * 
FROM 部門 INNER JOIN 員工 
ON 部門.部門ID=員工.所屬部門ID 

INSERT INTO 部門
VALUES
('P004',null),('P005',null);

SELECT * 
FROM 部門 AS A INNER JOIN 員工 AS B
ON A.部門ID=B.所屬部門ID 

SELECT * 
FROM 部門 LEFT OUTER JOIN 員工 
ON 部門.部門ID=員工.所屬部門ID 

SELECT * 
FROM 部門 LEFT OUTER JOIN 員工 
ON 部門.部門ID=員工.所屬部門ID 
WHERE 員工.所屬部門ID IS NULL

SELECT * 
FROM 部門 RIGHT OUTER JOIN 員工 
ON 部門.部門ID=員工.所屬部門ID 

SELECT * 
FROM 部門 FULL OUTER JOIN 員工 
ON 部門.部門ID=員工.所屬部門ID 

SELECT * 
FROM 員工,銷售

SELECT * 
FROM 員工,銷售
WHERE 員工.員工ID = 銷售.員工ID

SELECT * 
FROM 員工 INNER JOIN 銷售
ON 員工.員工ID = 銷售.員工ID

SELECT * 
FROM 員工 LEFT OUTER JOIN 銷售
ON 員工.員工ID = 銷售.員工ID

SELECT * 
FROM 員工 RIGHT OUTER JOIN 銷售
ON 員工.員工ID = 銷售.員工ID

SELECT * 
FROM 員工 FULL OUTER JOIN 銷售
ON 員工.員工ID = 銷售.員工ID

select *
from 員工;

select *
from 銷售
where 銷售數量>20;

select *
from 員工
where 員工ID = (select 員工ID from 銷售 where 銷售數量>20);

select *
from 員工
where 員工ID in (select 員工ID from 銷售 where 銷售數量>20);

select top 1 員工ID,sum(銷售數量) as 總銷售數量
from 銷售
group by 員工ID
order by sum(銷售數量) desc;

select top 3 員工ID,sum(銷售數量) as 總銷售數量
from 銷售
group by 員工ID
order by sum(銷售數量) desc;

select top 1 員工ID
from 銷售
group by 員工ID
order by sum(銷售數量) desc;

select top 3 員工ID
from 銷售
group by 員工ID
order by sum(銷售數量) desc;

select *
from 員工
where 員工ID in (select top 3 員工ID from 銷售 group by 員工ID order by sum(銷售數量)desc);

select *
from 員工
where 員工ID = (select top 1 員工ID from 銷售 group by 員工ID order by sum(銷售數量)desc);
use 明新公司
go
create view 檢視高銷售數量產品
as
select *
from 銷售
where 銷售數量>30;

use 明新公司
go
create view 檢視高售價產品
as
select *
from 產品
where 定價>1000;

update 檢視高售價產品
set 成本=定價*0.5;

use 明新公司
go
create view 部門R員工view
as
select 部門ID,部門.部門名稱,員工ID,員工姓名
from 部門 inner join 員工
on 部門.部門ID=員工.所屬部門ID;
```
