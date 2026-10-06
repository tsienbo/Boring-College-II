`inner join` , inner 是 join 的 default type

``` postgresql
select product_id, product_name, category_name
-- 这里也可以是 from categories
from products 
-- inner join 另外一个表，inner join 也可以是 join
inner join categories on products.category_id = categories.category_id;
```



`left join` ，包含左边表格，匹配右边表格; `right join` 反之。`full join` 包含两边表格。

`left join`

``` postgresql
test=# select * from testproducts;
 testproduct_id |      product_name      | category_id
----------------+------------------------+-------------
              1 | Johns Fruit Cake       |           3
              2 | Marys Healthy Mix      |           9
              3 | Peters Scary Stuff     |          10
              4 | Jims Secret Recipe     |          11
              5 | Elisabeths Best Apples |          12
              6 | Janes Favorite Cheese  |           4
              7 | Billys Home Made Pizza |          13
              8 | Ellas Special Salmon   |           8
              9 | Roberts Rich Spaghetti |           5
             10 | Mias Popular Ice       |          14
(10 rows)


test=# select * from categories;
 category_id | category_name  |                        description
-------------+----------------+------------------------------------------------------------
           1 | Beverages      | Soft drinks, coffees, teas, beers, and ales
           2 | Condiments     | Sweet and savory sauces, relishes, spreads, and seasonings
           3 | Confections    | Desserts, candies, and sweet breads
           4 | Dairy Products | Cheeses
           5 | Grains/Cereals | Breads, crackers, pasta, and cereal
           6 | Meat/Poultry   | Prepared meats
           7 | Produce        | Dried fruit and bean curd
           8 | Seafood        | Seaweed and fish
(8 rows)


test=# select testproduct_id, product_name, category_name from testproducts left join categories on testproducts.category_id = categories.category_id;
 testproduct_id |      product_name      | category_name
----------------+------------------------+----------------
              1 | Johns Fruit Cake       | Confections
              2 | Marys Healthy Mix      |
              3 | Peters Scary Stuff     |
              4 | Jims Secret Recipe     |
              5 | Elisabeths Best Apples |
              6 | Janes Favorite Cheese  | Dairy Products
              7 | Billys Home Made Pizza |
              8 | Ellas Special Salmon   | Seafood
              9 | Roberts Rich Spaghetti | Grains/Cereals
             10 | Mias Popular Ice       |
(10 rows)
-- 等价于：
select testproduct_id, product_name, category_name from categories right join testproducts on testproducts.category_id = categories.category_id;
```

`right join`

``` postgresql
test=# select testproduct_id, product_name, category_name from testproducts right join categories on testproducts.category_id = categories.category_id;
 testproduct_id |      product_name      | category_name
----------------+------------------------+----------------
              1 | Johns Fruit Cake       | Confections
              6 | Janes Favorite Cheese  | Dairy Products
              8 | Ellas Special Salmon   | Seafood
              9 | Roberts Rich Spaghetti | Grains/Cereals
                |                        | Condiments
                |                        | Meat/Poultry
                |                        | Beverages
                |                        | Produce
(8 rows)
```

`full join` 

``` postgresql
test=# select testproduct_id, product_name, category_name from testproducts full join categories on testproducts.category_id = categories.category_id;
 testproduct_id |      product_name      | category_name
----------------+------------------------+----------------
              1 | Johns Fruit Cake       | Confections
              2 | Marys Healthy Mix      |
              3 | Peters Scary Stuff     |
              4 | Jims Secret Recipe     |
              5 | Elisabeths Best Apples |
              6 | Janes Favorite Cheese  | Dairy Products
              7 | Billys Home Made Pizza |
              8 | Ellas Special Salmon   | Seafood
              9 | Roberts Rich Spaghetti | Grains/Cereals
             10 | Mias Popular Ice       |
                |                        | Condiments
                |                        | Meat/Poultry
                |                        | Beverages
                |                        | Produce
(14 rows)
```

`cross join`

``` postgresql
test=# select testproduct_id, product_name, category_name from testproducts cross join categories;
 testproduct_id |      product_name      | category_name
----------------+------------------------+----------------
              1 | Johns Fruit Cake       | Beverages
              1 | Johns Fruit Cake       | Condiments
              1 | Johns Fruit Cake       | Confections
              1 | Johns Fruit Cake       | Dairy Products
              1 | Johns Fruit Cake       | Grains/Cereals
              1 | Johns Fruit Cake       | Meat/Poultry
              1 | Johns Fruit Cake       | Produce
              1 | Johns Fruit Cake       | Seafood
              2 | Marys Healthy Mix      | Beverages
              2 | Marys Healthy Mix      | Condiments
              2 | Marys Healthy Mix      | Confections
              2 | Marys Healthy Mix      | Dairy Products
              2 | Marys Healthy Mix      | Grains/Cereals
              2 | Marys Healthy Mix      | Meat/Poultry
              2 | Marys Healthy Mix      | Produce
              2 | Marys Healthy Mix      | Seafood
              3 | Peters Scary Stuff     | Beverages
              3 | Peters Scary Stuff     | Condiments
              3 | Peters Scary Stuff     | Confections
              3 | Peters Scary Stuff     | Dairy Products
              3 | Peters Scary Stuff     | Grains/Cereals
              3 | Peters Scary Stuff     | Meat/Poultry
              3 | Peters Scary Stuff     | Produce
              3 | Peters Scary Stuff     | Seafood
              4 | Jims Secret Recipe     | Beverages
              4 | Jims Secret Recipe     | Condiments
              4 | Jims Secret Recipe     | Confections
              4 | Jims Secret Recipe     | Dairy Products
              4 | Jims Secret Recipe     | Grains/Cereals
              4 | Jims Secret Recipe     | Meat/Poultry
              4 | Jims Secret Recipe     | Produce
              4 | Jims Secret Recipe     | Seafood
              5 | Elisabeths Best Apples | Beverages
              5 | Elisabeths Best Apples | Condiments
              5 | Elisabeths Best Apples | Confections
              5 | Elisabeths Best Apples | Dairy Products
              5 | Elisabeths Best Apples | Grains/Cereals
              5 | Elisabeths Best Apples | Meat/Poultry
              5 | Elisabeths Best Apples | Produce
              5 | Elisabeths Best Apples | Seafood
              6 | Janes Favorite Cheese  | Beverages
              6 | Janes Favorite Cheese  | Condiments
              6 | Janes Favorite Cheese  | Confections
              6 | Janes Favorite Cheese  | Dairy Products
              6 | Janes Favorite Cheese  | Grains/Cereals
              6 | Janes Favorite Cheese  | Meat/Poultry
              6 | Janes Favorite Cheese  | Produce
              6 | Janes Favorite Cheese  | Seafood
              7 | Billys Home Made Pizza | Beverages
              7 | Billys Home Made Pizza | Condiments
              7 | Billys Home Made Pizza | Confections
              7 | Billys Home Made Pizza | Dairy Products
              7 | Billys Home Made Pizza | Grains/Cereals
              7 | Billys Home Made Pizza | Meat/Poultry
              7 | Billys Home Made Pizza | Produce
              7 | Billys Home Made Pizza | Seafood
              8 | Ellas Special Salmon   | Beverages
              8 | Ellas Special Salmon   | Condiments
              8 | Ellas Special Salmon   | Confections
              8 | Ellas Special Salmon   | Dairy Products
              8 | Ellas Special Salmon   | Grains/Cereals
              8 | Ellas Special Salmon   | Meat/Poultry
              8 | Ellas Special Salmon   | Produce
              8 | Ellas Special Salmon   | Seafood
              9 | Roberts Rich Spaghetti | Beverages
              9 | Roberts Rich Spaghetti | Condiments
              9 | Roberts Rich Spaghetti | Confections
              9 | Roberts Rich Spaghetti | Dairy Products
              9 | Roberts Rich Spaghetti | Grains/Cereals
              9 | Roberts Rich Spaghetti | Meat/Poultry
              9 | Roberts Rich Spaghetti | Produce
              9 | Roberts Rich Spaghetti | Seafood
             10 | Mias Popular Ice       | Beverages
             10 | Mias Popular Ice       | Condiments
             10 | Mias Popular Ice       | Confections
             10 | Mias Popular Ice       | Dairy Products
             10 | Mias Popular Ice       | Grains/Cereals
             10 | Mias Popular Ice       | Meat/Poultry
             10 | Mias Popular Ice       | Produce
             10 | Mias Popular Ice       | Seafood
(80 rows)
```

