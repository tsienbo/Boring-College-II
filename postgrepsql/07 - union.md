

``` postgresql
select testproduct_id, product_name from testproducts
union select product_id, product_name from products
order by testproduct_id;
-- 注意此处 order by 第一张表的表项，故可写作：
select product_id, product_name from products
union select testproduct_id, product_name from testproducts
order by product_id;
```



`union` selects only distinct values

`union all` returns duplicate values

``` postgresql
test=# select product_id from products union select testproduct_id from testproducts order by product_id;
 product_id
------------
          1
          2
          3
          4
          5
          6
          7
          8
          9
         10
         11
         12
test=# select product_id from products union select testproduct_id from testproducts order by product_id;
 product_id
------------
          1
          2
          3
          4
          5
          6
          7
          8
          9
         10
         11
         12
         13
         14
         15
         16
         17
         18
         19
         20
         21
         22
         23
         24
         25
         26
         27
test=# select product_id from products union all select testproduct_id from testproducts order by product_id;
 product_id
------------
          1
          1
          2
          2
          3
          3
          4
          4
          5
          5
          6
          6
          7
          7
          8
          8
          9
          9
         10
         10
         11
         12
         13
```





 
