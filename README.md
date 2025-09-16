# Library Management System using SQL

## Project Overview

**Project Title**: Library Management System  
**Level**: Intermediate  
**Database**: `library_management`

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

<img width="800" height="500" alt="image" src="https://github.com/user-attachments/assets/4a9ec27f-cf88-4945-af32-df920e93a813" />

## Objectives

1. **Set up the Library Management System Database**: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
2. **CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
3. **CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.
4. **Advanced SQL Queries**: Develop complex queries to analyze and retrieve specific data.

## Project Structure

### 1. Database Setup
<img src="./er_diagram.png" alt="Entity-Relationship Diagram" width="800">


- **Database Creation**: Created a database named `library_management`.
- **Table Creation**: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.

'''sql
CREATE DATABASE library_management;

USE library_management;

-- CREATE TABLES

-- Create table "books"

CREATE TABLE books (
    isbn VARCHAR(20) PRIMARY KEY,
    book_title VARCHAR(50),
    category VARCHAR(20),
    rental_price DECIMAL(10,2),
    status VARCHAR(20),
    author VARCHAR(40),
    publisher VARCHAR(40)
);
-- Create table "branch"

CREATE TABLE branch (
    branch_id VARCHAR(6) PRIMARY KEY,
    manager_id VARCHAR(6),  
    branch_address VARCHAR(100),
    contact_no VARCHAR(20)
);
-- Create table "employees"

CREATE TABLE employees (
    emp_id VARCHAR(6) PRIMARY KEY,
    emp_name VARCHAR(50),
    position VARCHAR(20),
    salary DECIMAL(10,2),
    branch_id VARCHAR(6),
    CONSTRAINT fk_employees_branch FOREIGN KEY (branch_id) REFERENCES branch(branch_id)
);

-- Create table "members"

CREATE TABLE members (
    member_id VARCHAR(6) PRIMARY KEY,
    member_name VARCHAR(50),
    member_address VARCHAR(100),
    reg_date VARCHAR(10)
);

-- Create table "issued_status"

CREATE TABLE issued_status (
    issued_id VARCHAR(6) PRIMARY KEY,
    issued_member_id VARCHAR(6),
    issued_book_isbn VARCHAR(20),
    issued_date DATE,
    issued_emp_id VARCHAR(6),
    CONSTRAINT fk_issued_status_emp FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
    CONSTRAINT fk_issued_status_book FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn),
    CONSTRAINT fk_issued_status_member FOREIGN KEY (issued_member_id) REFERENCES members(member_id)
);

-- Create table "return_status"

DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(6) PRIMARY KEY,
            issued_id VARCHAR(6),
            return_book_name VARCHAR(30),
            return_date VARCHAR(10),
            return_book_isbn VARCHAR(20),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);
-- add new column to issued_status
ALTER TABLE issued_status
ADD COLUMN issued_book_name VARCHAR(80)

'''
-- 2.CRUD OPERATION

-- 1.CREATE NEW RECORD

'''sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES('978-0-45228-849-0', 'Pride and Prejudice', 'Romance', 5.50, 'yes', 'Jane Austen', 'T. Egerton, Whitehall'),
VALUES('978-0-7432-7356-5', 'The Great Gatsby', 'Classic', 5.75, 'yes', 'F. Scott Fitzgerald', 'Charles Scribner\'s Sons');
'''
-- 2.UPDATE RECORD

'''sql
UPDATE members
SET member_address = '125 Oak St'
WHERE member_id = 'C103';
'''
-- 3.DELETE A RECORD
'''sql
DELETE FROM issued_status
WHERE   issued_id =   'IS121';
'''
-- 4.RETRIEVE RECORDS 
'''sql
SELECT * FROM members;
'''

--3.CTAS (Create Table As Select)

**Create Summary Tables**: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**

'''sql
CREATE TABLE book_issued_count as
SELECT b.isbn,b.book_title,count(*) as TOTAL_BOOK_ISSUED
FROM books b
INNER JOIN issued_status i
ON b.isbn=i.issued_book_isbn
GROUP BY b.isbn,b.book_title
'''
