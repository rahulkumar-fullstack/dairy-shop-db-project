# Dairy Shop Database Management
- This Project is based on Relational Database Management System **(RDBMS)**
- Language: **`SQL`** is used
- Tools: **MySQL Workbench**
> *For Practice & Learning Purpose only*

## ER Diagram
![Images](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/er_diagram.png)

## Create Database
- Create database: `dairy_shop`

```
Create database dairy_shop;

Use dairy_shop;  -- select database
```

## Create Tables
**Create 5 Tables having name:**
- Godown
- Product
- Purchase
- Sales
- Customer
```
-- 1. Godown
create table godown(
godown_id int primary key auto_increment,
godown_name varchar(50),
godown_city varchar(30) 
);

-- 2. Product
create table product(
prod_id int primary key auto_increment,
prod_name varchar(60) unique,
prod_type varchar(30),
godown_id int, 
foreign key(godown_id) references godown(godown_id)
);

-- 3. Purchase
create table purchase(
purchase_id int primary key auto_increment,
prod_id int, foreign key(prod_id) references product(prod_id),
prod_price float,
prod_quantity int,
purchase_date date 
);

-- 4. Sales
create table sales(
sales_id int primary key auto_increment,
prod_id int,
foreign key(prod_id) references product(prod_id),
sales_price float, 
sales_quantity int,
cust_name varchar(50),
sales_date date
);

-- 5. Customer
create table customer(
cust_id int primary key auto_increment,
cust_name varchar(50),
rating int default 5
);
```

## Inserting Sample Data

- Inserting Sample data into all tables
```
-- 1. Populate godown table:
insert into godown values
(1, ' Alpha ', ' Thane East '),
(2, ' Beta ', ' Thane West '),
(3, ' Charlie ', ' Thane West ');

-- 2. Populate product table:
insert into product (prod_name, prod_type, godown_id) values
('Amul milk 500 ml','Milk',1),
('Govind milk 500 ml','Milk',1),
('Gokul milk 500 ml','Milk',1),
('Amul Yogurt 250 g','Yogurt',3),
('Gokul Yogurt 500 g','Yogurt',3),
('Motherdairy Yogurt 1 kg','Yogurt',3),
('Dairymilk silk','Chocolate',2),
('Qwailty Vanilla Cone','Ice-cream',2);

-- 3. Populate purchase table:
insert into purchase (prod_id, prod_price, prod_quantity, purchase_date) values
(1,30,50, curdate()), (2,32,60,curdate()), (3,35,33, curdate()),
(4,25,37, curdate()), (5,50,20, curdate()), (6,100,9, curdate()),
(7,97,43, curdate()), (8,24,62, curdate()), (3,35,11, curdate()),
(5, 50,15, curdate()), (7,97,5, curdate()), (1,30,13, curdate()),
(4,25,4, curdate());

-- 4. Populate sales table:
insert into sales(prod_id, sales_price, sales_quantity, cust_name, sales_date) values (1,38,4,'john',curdate()),
(2,40,5,'brock',curdate()), (4,32,7,'jim',curdate()), 
(3,45,1,'rohit',curdate()), (6,140,5,'jack',curdate()), 
(5,40,5,'jim',curdate()), (2,40,11,'william',curdate()), 
(7,120,7,'oggy',curdate()), (8,38,8,'jonny',curdate()), 
(6,140,3,'jack',curdate()), (3,45,9,'jim',curdate()), (5,40,4,'john',curdate());

-- 5. Populate customer table
insert into customer(cust_name, rating) values
('john',3), ('william',4),
('brock',4), ('rohit',5),
('jim',3),('jack',4), ('jonny',5);
```

## Result
- **To find total money a single customer spent**
```
select cust_name,sum(sales_price*sales_quantity) as Total_spent_money from sales where cust_name = (select cust_name from customer where cust_name = 'jack') group by cust_name;
```
![Image1](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/sub-query-1.png)

- **To find how many products a single customer brought**
```
select cust_name,count(prod_id) as No_of_product from sales where cust_name =(select cust_name from customer where cust_name = 'jim') group by cust_name;
```
![Image2](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/sub-query-2.png)

- **To find quantity and Total money spent on a single product**
```
select  sum(prod_quantity) as no_product, sum(prod_price*prod_quantity) as Total_money_spent from purchase where prod_id = (select prod_id from product where prod_id=5);
```
![Image3](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/sub-query-3.png)

- **View Purchase Record**
```
select purchase_id as sr_no, prod_name as Product, prod_price as Price, prod_quantity as Quantity, (prod_price*prod_quantity) as Total_price, purchase_date from purchase left join product on (purchase.prod_id = product.prod_id); 
```

![Image4](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/purchase_record.png)

- **View Sales Record**
```
select sales_id as sr_no, prod_name as Product, sales_price as Price, sales_quantity as Quantity, (sales_price*sales_quantity) as Total_price, cust_name, sales_date from product right join sales on (sales.prod_id = product.prod_id);
```

![Image5](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/sales_record.png)

- **View Stock Record**
```
select prod_name as Product,(sum(prod_quantity) - sum(sales_quantity)) as Available from product left join purchase on (purchase.prod_id = product.prod_id) left join sales on (purchase.prod_id = sales.prod_id)  group by prod_name;
```

![Image6](https://github.com/iamrahulkumar052/database-project/blob/main/Dairy%20Shop%20Database%20Management/images/stock_record.png)

-----


