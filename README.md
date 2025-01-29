# Table of contents
- [Table of contents](#table-of-contents)
  - [Section 1: Course Introduction](#section-1-course-introduction)
  - [Section 2: Installation and Setup](#section-2-installation-and-setup)
    - [Docker](#docker)
    - [MySQL Workbench](#mysql-workbench)
    - [Commands](#commands)
  - [Secction 3: Data Definition Language](#secction-3-data-definition-language)
    - [Data Definition Language (DDL)](#data-definition-language-ddl)
    - [Data Manipulation Language (DML)](#data-manipulation-language-dml)
    - [Data Types](#data-types)
    - [Creating the Coffee Store Database](#creating-the-coffee-store-database)
    - [Modifying Tables: Adding and Removing Columns](#modifying-tables-adding-and-removing-columns)
    - [Deleting Tables](#deleting-tables)
    - [Truncating Tables](#truncating-tables)
  - [Section 4: More On Alter Table](#section-4-more-on-alter-table)
    - [Creating Our Test Database](#creating-our-test-database)
    - [Add and Remove Primary Key](#add-and-remove-primary-key)
    - [Add and Remove Foreign Key](#add-and-remove-foreign-key)
    - [Add Unique Constraint](#add-unique-constraint)
    - [Change Column Name](#change-column-name)

## Section 1: Course Introduction

## Section 2: Installation and Setup
### Docker
docker-compose.yml

```dockerfile
services:
  mysql:
    image: mysql:8.0
    container_name: mysql-server      # Name the container
    environment:                      # Define environment variables
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
    ports:
      - "3306:3306"                   # Map port 3306
    volumes:
      - mysql-data:/var/lib/mysql     # Persist data using a named volume

volumes:
  mysql-data:                         # Define the named volume for MySQL data
```
To start the MySQL server with Docker Compose, run:
```shell
docker-compose up -d
```
Verify the version of MySQL
```shell
docker exec -it mysql-server mysql --version
```
If you want to start the services again, you can use the following command:
```shell
docker-compose start
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### MySQL Workbench
1. Open **MySQL Workbench**
2. Create a New Connection
Click on **+** next to "MySQL Connections" on the main screen.
A dialog box titled Setup New Connection will appear.
3. Configure the Connection
Fill in the following fields:  
  **Connection Name**: Docker MySQL - Give it a name  
  **Hostname**: ``127.0.0.1`` - Enter localhost (if you're running Docker on your local machine).  
  **Port**: ``3306`` (as defined in your docker-compose.yml file).  
  **Username**: ``root`` (as specified in MYSQL_USER in your docker-compose.yml).  
  **Password**: Click **Store in Vault...** and enter ``admin`` (as specified in MYSQL_PASSWORD).  
  4. **Test the Connection**  
  Click the **Test Connection** button.
  If everything is configured correctly, you'll see a success message: "Successfully made the MySQL connection".  
  **If it fails**:  
  Make sure the MySQL container is running (docker ps).  
  Ensure port ``3306`` is open and not blocked by a firewall.

**Notes**:  
Confirm that the container is running:
```shell
docker ps
```
**Steps to apply change**  
Stop and delete the current container:
```shell
docker-compose down
```
Delete old data on your volume
```shell
docker volume rm mysql-data
```
Start over
```shell
docker-compose up -d
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Commands

Create a new database named 'test'
```sql
CREATE DATABASE test;
```
Switch to the 'test' database to work within it
```sql
USE test;
```
Show the list of all databases available
```sql
SHOW DATABASES;
```
Delete the 'test' database
```sql
DROP DATABASE test;
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

## Secction 3: Data Definition Language
### Data Definition Language (DDL)
SQL commands that are used to **define the structure of the database** objects.  

**CREATE**: Create a database, and its tables, columns and indexes.  
**ALTER:** Alter the structure of the database objects - add or remove columns, indexes, etc.  
**DROP:** Delete tables, indexes, and even the entire database.  
**RENAME:** Rename a table.  
**TRUNCATE:** Clear out the contents of a table. Effectively the same as deleting and re-creating the table.  
### Data Manipulation Language (DML)  
SQL commands that are used to **manipulate** the data.    

**SELECT**: Query the database to retrieve rows of data.  
**INSERT**: Insert data into a table.   
**UPDATE**: Change the data in columns of a table (or tables).  
**DELETE**: Delete rows in a table (or tables).  

### Data Types  
**Numeric Types**  
**INT** → age INT → **Example**: 25  
**DECIMAL(m, d)** → price DECIMAL(10, 2) → **Example**: 1234.56  
**FLOAT** → temperature FLOAT → **Example**: 36.5  

**String Types**  
**VARCHAR(n)** → name VARCHAR(50) → **Example**: 'John Doe'  
**CHAR(n)** → code CHAR(5) → **Example**: 'A1234'  
**TEXT** → description TEXT → **Example**: 'Long text here'  

**Date and Time Types**  
**DATE** → dob DATE → **Example**: '1990-01-01'  
**DATETIME** → created_at DATETIME → **Example**: '2025-01-28 12:34:56'  
**TIME** → duration TIME → **Example**: '02:30:00'  

**Boolean**  
**BOOLEAN** → is_active BOOLEAN → **Example**: 1  

**Binary Types**  
**BLOB** → image BLOB → **Example**: (binary data)  

**Enumeration Type (ENUM)**   
**ENUM** → status ENUM('active', 'inactive', 'pending') → **Example**: 'active'

<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Creating the Coffee Store Database

```sql
CREATE DATABASE coffee_store;

USE coffee_store;

CREATE TABLE products (
  id INT auto_increment PRIMARY KEY,
  name VARCHAR(30),
  price DECIMAL(3,2)
);

SHOW TABLES;

CREATE TABLE customers(
  id INT auto_increment PRIMARY KEY,
  first_name VARCHAR(30),
  last_name VARCHAR(30),
  gender ENUM('M','F'),
  phone_number VARCHAR(11)
);

CREATE TABLE orders (
  id INT auto_increment PRIMARY KEY,
  product_id INT,
  customer_id INT,
  order_time DATETIME,
  FOREIGN KEY (product_id) REFERENCES products(id),
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```
[Script Database](https://github.com/ovidiocbba/MySQL/blob/main/script_database/creating_the_coffee_store_database.sql)

<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Modifying Tables: Adding and Removing Columns
```sql
ALTER TABLE [tableName] ADD COLUMN [columnName] [dataType]([size]);
```
```sql
ALTER TABLE [tableName]
```
- Specifies the table you want to modify.   
```sql
ADD COLUMN [columnName] [dataType]([size])
```
- Adds a new column to the table.
- [dataType] defines the type of data the column will store (e.g., VARCHAR, INT, etc.).
- [size] (optional) specifies the maximum size of the data, if applicable.

```sql
-- How to add and remove columns from a table

-- Select the database to work with
USE coffee_store;

-- Show the structure of the "products" table to understand its current columns
DESCRIBE products;

-- Add a new column "coffe_origin" to the "products" table
ALTER TABLE products
ADD COLUMN coffe_origin VARCHAR(30);
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Deleting Tables
```sql
DROP TABLE [tableName];
```
- Completely deletes the **table**, its **data**, and its **structure** from the database.
- Use this when you no longer need the table.

```sql
-- How to delete tables forom a database
-- Step 1: Create a new database
CREATE DATABASE example;

-- Step 2: Switch to the "example" database to work within it
USE example;

-- Step 3: Create a table named "test" with three columns:
CREATE TABLE test (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(30),
    age INT
);

-- Step 4: Display the structure of the "test" table to verify its columns
DESCRIBE test;

-- Step 5: List all tables in the current database to confirm the "test" table exists
SHOW TABLES;

-- Step 6: Delete (drop) the "test" table from the database
DROP TABLE test;

-- Step 7: Attempt to describe the "test" table again
-- This will produce an error because the table no longer exists
DESCRIBE test;
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

###  Truncating Tables
```sql
TRUNCATE TABLE [tableName];
```
- Removes all rows but retains the table structure for future use.
- Use this when you need to empty the table but plan to reuse it.

```sql
-- Step 1: Create a new database named "example"
CREATE DATABASE example;

-- Step 2: Switch to the "example" database to work within it
USE example;

-- Step 3: Create a table named "test"
CREATE TABLE test (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(30),
    age INT
);

-- Step 4: Insert sample data into the "test" table
INSERT INTO test (name, age) 
VALUES  ('Ben', 19), ('Simon', 28), ('Claire', 23);

-- Step 5: Retrieve and display all records from the "test" table
SELECT * FROM test;

-- Step 6: Remove all data from the "test" table without deleting its structure
-- This operation clears the table while keeping its definition intact
TRUNCATE TABLE test;
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

## Section 4: More On Alter Table
### Creating Our Test Database
```sql
CREATE DATABASE test;

USE test;

CREATE TABLE addresses (
  id INT,
  house_number INT,
  city VARCHAR(20),
  postcode VARCHAR(7)
);

CREATE TABLE people (
  id INT,
  first_name VARCHAR(20),
  last_name VARCHAR(20),
  address_id INT
);

CREATE TABLE pets (
  id INT,
  name VARCHAR(20),
  species VARCHAR(20),
  owner_id INT
);

SHOW TABLES;
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Add and Remove Primary Key
SQL  to **add** a **primary key** to a table.
```sql
ALTER TABLE <table_name>
ADD PRIMARY KEY (<column_name>);
```

SQL to **remove** a **primary key** from a table.
```sql
ALTER TABLE <table_name>
DROP PRIMARY KEY;
```

SQL to **modify** a **column's** data type:
```sql
ALTER TABLE <table_name>
MODIFY <column_name> <new_data_type>;
```
Changes the data type of an existing column to a new one, for example, from **VARCHAR** to **INT**.

```sql
-- Describe the structure of the "addresses" table
DESCRIBE addresses;

-- Add a primary key to the "addresses" table using the "id" column
ALTER TABLE addresses
ADD PRIMARY KEY (id);

-- Remove the primary key from the "addresses" table
ALTER TABLE addresses
DROP PRIMARY KEY;

-- Modify the "id" column in the "addresses" table to be of type INT
ALTER TABLE addresses
MODIFY id INT;

-- Describe the structure of the "people" table
DESCRIBE people;

-- Add a primary key to the "people" table using the "id" column
ALTER TABLE people
ADD PRIMARY KEY (id);
```
<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

 ### Add and Remove Foreign Key
 ![Foreign Key](images/foreign_key.png)
SQL to **add a foreign key** to a table:
```sql
ALTER TABLE <table_name>
ADD CONSTRAINT <constraint_name>
FOREIGN KEY (<column_name>) REFERENCES <referenced_table>(<referenced_column>);
```
SQL to **remove a foreign key** and its index from a table:
```sql
ALTER TABLE <table_name>
DROP FOREIGN KEY <constraint_name>,
DROP INDEX <constraint_name>;
```
**Note:**
The ``constraint_name`` is necessary when you want to **delete** or **modify** a constraint in a table. Without a unique name for the constraint, you won't be able to reference it later for changes. When adding a constraint, **it is a good practice** to assign a descriptive name for easy identification.

```sql
-- Describe the structure of the "people" table
-- Ensure "people" has a primary key on "id"
DESCRIBE people;

-- Describe the structure of the "addresses" table
-- Ensure "addresses" has a primary key on "id"
DESCRIBE addresses;

-- Add a foreign key to "people" linking "address_id" to "addresses(id)"
ALTER TABLE people
ADD CONSTRAINT FK_PeopleAddress
FOREIGN KEY (address_id) REFERENCES addresses(id);

-- Remove the foreign key and its index from "people"
ALTER TABLE people
DROP FOREIGN KEY FK_PeopleAddress,
DROP INDEX FK_PeopleAddress;
```

<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Add Unique Constraint
SQL to **add a unique constraint** to a column:
```sql
ALTER TABLE <table_name>
ADD CONSTRAINT <constraint_name> UNIQUE (<column_name>);
```
Ensures that all values in the specified column are unique, preventing duplicate entries.    
**Example**  
Let's say we have a **users** table, and we want to ensure that emails are **unique** to prevent duplicate accounts.

SQL to **remove a unique constraint** from a column:
```sql
ALTER TABLE <table_name>
DROP INDEX <constraint_name>;
```
Deletes the unique constraint, allowing duplicate values in the column.

```sql
DESCRIBE pets;

-- How to add a unique constraint to a column
ALTER TABLE pets
ADD CONSTRAINT u_species UNIQUE (species);

-- How to remove a unique constraint from a column
ALTER TABLE pets
DROP INDEX u_species;
```
 ![Add Unique constraint](images/add_unique_constraint.png)

<div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>

### Change Column Name
SQL to **rename a column** and **change its data type**:
```sql
ALTER TABLE <table_name>
CHANGE <old_column_name> <new_column_name> <new_data_type>;
```
This **renames the column** and **updates its data type** at the same time.  
**Example**
```sql
ALTER TABLE pets
CHANGE species animal_type VARCHAR(20);
```
Renames the **species** column to **animal_type** and changes its **data type** to **VARCHAR(20)**.

SQL to **rename a column** without changing its data type:
```sql
ALTER TABLE <table_name>
RENAME COLUMN <old_column_name> TO <new_column_name>;
```
This simply **renames the column** while keeping the data type unchanged.  
**Example**
```sql
ALTER TABLE pets
RENAME COLUMN animal_type TO species;
```
Renames the **animal_type** column back to **species**.

```sql
DESCRIBE pets;

-- How to change a column's name
ALTER TABLE pets
CHANGE species animal_type VARCHAR(20);

ALTER TABLE pets
RENAME COLUMN animal_type TO species;
```

 ![Change column_name](images/change_column_name.png)

 <div align="right">
  <strong>
    <a href="#table-of-contents" style="text-decoration: none;">↥ Back to top</a>
  </strong>
</div>