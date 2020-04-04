# codewander.com-odoo-pos-talend-job
 

Entities and Tables
There are three entities in the Point of Sale (PoS) context. These three entities are stored in multiple tables. Following are the entities and the table details.

Entity: Order
Table: pos_order
Order line
Tables: pos_order_line
Product
Tables:
product_product
product_template
uom_uom
uom_category
product_category
Odoo-PoS-Tables-and-Entities
Odoo PoS Entities and Tables
Extract Data to flat files
Now that we identified the required tables and the entities, we will now generate three output files one for each of the entity.

The talend job does the following activities

Create a common database connection using the postgreSQL connector.
Extract Order using the following query and output it to the flat file orders.csv
select * from pos_order
Extract order lines using the following query and output it to flat file orderlines.csv
select 
id order_line_id, company_id, name order_line_name, 
notice order_line_notice, product_id, price_unit order_line_price_unit, 
qty order_line_qty,price_subtotal order_line_price_subtotal, price_subtotal_incl order_line_price_subtotal_incl, discount order_line_discount,order_id, create_uid order_line_create_uid, create_date order_line_create_date, write_uid order_line_write_uid, write_date order_line_write_date
from 
pos_order_line
Get Product details using the following query from two tables product_product and product_template
select product_product.id product_id,  product_template.name product_name,
product_template.type product_type,product_template.categ_id product_category_id,
product_template.list_price product_list_price,product_template.uom_id product_uom_id,
product_template.pos_categ_id product_pos_categ_id
from product_product left join product_template 
on product_product.product_tmpl_id =  product_template.id
Merge UoM details using the below query from two tables uom_uom and uom_category
select uom_uom.id product_uom_id, uom_uom.name product_uom_name,uom_category.name product_uom_category_name  from uom_uom left join uom_category on uom_uom.category_id = uom_category.id
Extract product category details using the following query from product_category table
SELECT id product_category_id, name product_category_name, complete_name product_category_complete_name
FROM public.product_category
Join Product, UoM and Product Category details using tJoin to create one single output file products.csv
The reason for keeping the product separate rather than merging with order or order lines is that these products can later be linked with inventory entities. If we link it to Orders then we may not be able to get the list of products that are not sold.

Odoo PoS Dashoboard Talend Integration Job
Odoo PoS Dashoboard Talend Integration Job
Once the job runs successfully, we will have the following three files.

orders.csv
orderlines.csv
products.csv
As the data is ready now, we can start building dashboard in Qlik sense, Power BI or any other tool of your choice.
