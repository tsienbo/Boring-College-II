`select distinct` 不重复查询，排除结果中重复数据

``` postgresql
select distinct country from customers;
```



`select count` 查询数量

``` postgresql
-- 查询不重复国家数量
select count(distinct country) from customers;
select count(customer_id) from customers where country = 'London';
```



`desc` 降序

```postgresql
select * from products order by price desc;
```



`limit` 限制数量

``` postgresql
-- 只看前 20 条数据
select * from customers limit 20;
```



`offset` 从哪开始

``` postgresql
从第 41 条数据开始输出 20 条数据，也就是说默认 offset 0
select * from customers limit 20 offset 40;
```



`min()` 最小 `max()` 最大

``` postgresql
select min(price) from products;
```



`sum()` 总数

``` postgresql
select sum(quantity) from order_details;
```



`avg()` 平均数

``` postgresql
# select avg(price) from products;
         avg
---------------------
 28.8663636363636364
(1 row)
-- with 2 decimals 
# select avg(price):numeric(10,2) from products;
  avg
-------
 28.87
(1 row)
```



`like`

``` postgresql
select * from customers where customer_name like 'A%';
```



`in`

``` postgresql
select * from customers where country in ('UK', 'USA', 'Germany');
```

`not in`

``` postgresql
select * from customers wheer country not in ('UK', 'USA', 'Germany');
```

注意，使用常量值查询（即手动输入一堆值），值与值之间要用逗号隔开；使用子查询，不需要加逗号。

``` postgresql
# SELECT customer_id FROM orders;
 customer_id
-------------
          90
          81
          34
          84
          
test=# SELECT * FROM customers WHERE customer_id IN (SELECT customer_id FROM orders);
 customer_id |           customer_name            |     contact_name     |                    address                     |      city       | postal_code |   country
-------------+------------------------------------+----------------------+------------------------------------------------+-----------------+-------------+-------------
           1 | Alfreds Futterkiste                | Maria Anders         | Obere Str. 57                                  | Berlin          | 12209       | Germany
           2 | Ana Trujillo Emparedados y helados | Ana Trujillo         | Avda. de la Constitucion 2222                  | Mexico D.F.     | 05021       | Mexico
           3 | Antonio Moreno Taquera             | Antonio Moreno       | Mataderos 2312                                 | Mexico D.F.     | 05023       | Mexico
           4 | Around the Horn                    | Thomas Hardy         | 120 Hanover Sq.                                | London          | WA1 1DP     | UK

```



`between` 

``` postgresql
select * from products where price between 10 and 20;
-- 下面这个查询是查询 Pavlova 与 Tofu 之间的产品，二者顺序不能颠倒
select * from products where product_name between 'Pavlova' and 'Tofu' order by product_name;
```



`as` 

``` postgresql
select customer_id as id from customers;
-- as 可以省略
select product_id id from customers;
-- 多个单词。注意只能用 "" ，不能用 ''
select product_id as "my product" from products;
-- 可以用中文
select product_id 产品 from products;
```

``` postgresql
select product_name || unit as product from products;
-- 因为两项数据之间没有间隙，所以我们加上空格。
-- 这里的 as 也是可以省略的
```



