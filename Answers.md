1. Explain the relationship between the "Product" and "Product_Category" entities from the above diagram.
Answer: The "Product" and "Product_Category" entities have a relationship defined by the "category_id" column in the "Product" table. This "category_id" column is an integer and serves as a foreign key, referencing the "id" column in the "Product_Category" table. This indicates that each product is associated with one product category, while each product category can have multiple products associated with it. In other words, the relationship between "Product" and "Product_Category" is a one-to-many relationship, with "Product_Category" being the "one" side and "Product" being the "many" side.


2. How could you ensure that each product in the "Product" table has a valid category assigned to it?
Answer: To ensure that each product in the "Product" table has a valid category assigned to it, we can implement a combination of database constraints. Here are some steps to achieve this:

**i) Foreign Key Constraint**
Add a foreign key constraint on the "category_id" column in the "Product" table, referencing the "id" column in the "Product_Category" table. This will ensure that any value inserted into the "category_id" column in the "Product" table must exist in the "id" column of the "Product_Category" table.
Example:
  ALTER TABLE product;
  ADD CONSTRAINT fk_product_category;
  FOREIGN KEY (category_id) REFERENCES product_category(id);

**ii)	Check Constraint** 
Add a check constraint on the "category_id" column in the "Product" table to ensure that it is not NULL. This will guarantee that each product has a category assigned to it.
Example:
  ALTER TABLE product
  ADD CONSTRAINT chk_product_category_not_null
  CHECK (category_id IS NOT NULL);

**iii) Database Triggers**
Implement database triggers to enforce the constraint at the database level. The trigger checks if the "category_id" value exists in the "Product_Category" table before inserting the new product. If the "category_id" value does not exist, the trigger raises an error with the message "Invalid category_id: [category_id_value]".
Example:
  DELIMITER //
  CREATE TRIGGER insert_product_trigger
  BEFORE INSERT ON product
  FOR EACH ROW
  BEGIN
    DECLARE invalid_category_id INT;
    SELECT id INTO invalid_category_id FROM product_category WHERE id = NEW.category_id;
    IF invalid_category_id IS NULL THEN
      SIGNAL SQLSTATE '45000'
      SET MESSAGE_TEXT = 'Invalid category_id: ' + CAST(NEW.category_id AS CHAR);
    END IF;
  END;
  //
  DELIMITER ;








