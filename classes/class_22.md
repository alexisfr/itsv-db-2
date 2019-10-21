[Essentials_7.1_01_Architecture_Overview.pptx](../images/Essentials_7.1_01_Architecture_Overview.pptx)

[Essentials_7.1_LabManual_1_Node.docx](../images/Essentials_7.1_LabManual_1_Node.docx)


[OLAPIntro.pdf](../images/OLAPIntro.pdf)

```sql 
Vertica Excercises #1

-- 1 Exaplain this query

SELECT fat_content 
FROM (
  SELECT DISTINCT fat_content 
  FROM product_dimension 
  WHERE department_description 
  IN ('Dairy') ) AS food 
  ORDER BY fat_content
  LIMIT 5;

-- 2 Explain this query

SELECT order_number, date_ordered
FROM store.store_orders_fact orders
WHERE orders.store_key IN (
  SELECT store_key
  FROM store.store_dimension
  WHERE store_state = 'MA') 
AND orders.vendor_key NOT IN (
    SELECT vendor_key
    FROM public.vendor_dimension
    WHERE vendor_state = 'MA')
AND date_ordered < '2003-03-01';


-- 3 Requests female and male customers with the maximum 
-- annual income from customers

SELECT customer_name, annual_income
FROM public.customer_dimension
WHERE (customer_gender, annual_income) IN
      (SELECT customer_gender, MAX(annual_income)
       FROM public.customer_dimension
       GROUP BY customer_gender);


-- 4 IN predicate
-- Find all products supplied by stores in MA

 SELECT DISTINCT s.product_key, p.product_description
FROM store.store_sales_fact s, public.product_dimension p
WHERE s.product_key = p.product_key
    AND s.product_version = p.product_version
    AND s.store_key IN
	  (SELECT store_key
	   FROM store.store_dimension
	   WHERE store_state = 'MA')
ORDER BY s.product_key;

-- 5
-- EXISTS predicate
-- Get a list of all the orders placed by all stores on 
-- January 2, 2003 for the vendors with records in the 
-- vendor_dimension table 



-- 6
-- EXISTS predicate
-- Orders placed by the vendor who got the best deal 
-- on January 4, 2004


-- 7
-- Equi join
-- Joins online_sales_fact table and the call_center_dimension 
-- table with the ON clause

```