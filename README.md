# django-translator
A simple translator using python-django, html, css and javascript.
### $ python3 -m venv venv
### $ source venv/bin/activate
### $ pip install Django translator djangorestframework

Packages
CREATE TABLE customer (
    c_id INT PRIMARY KEY,
    c_name VARCHAR(255) NOT NULL,
    c_age INT,
    c_address VARCHAR(255),
    c_salary DECIMAL(20, 2)
);
select * from CUSTOMER;
CREATE OR REPLACE PACKAGE customer_pack AS
    PROCEDURE add_customer(cust_id customer.c_id%TYPE, 
                           cust_name customer.c_name%TYPE,
                           cust_age customer.c_age%TYPE,
                           cust_address customer.c_address%TYPE,
                           cust_salary customer.c_salary%TYPE);
    PROCEDURE delete_customer(cust_id customer.c_id%TYPE);
END customer_pack;
                            
CREATE OR REPLACE PACKAGE BODY customer_pack AS
    PROCEDURE add_customer(cust_id customer.c_id%TYPE, 
                           cust_name customer.c_name%TYPE,
                           cust_age customer.c_age%TYPE,
                           cust_address customer.c_address%TYPE,
                           cust_salary customer.c_salary%TYPE) IS
    BEGIN
        INSERT INTO customer (c_id, c_name, c_age, c_address, c_salary) VALUES (cust_id, cust_name, cust_age, cust_address, cust_salary);
    END add_customer;
    
    PROCEDURE delete_customer(cust_id customer.c_id%TYPE) IS
    BEGIN
        DELETE FROM customer WHERE c_id = cust_id;
    END delete_customer;
END customer_pack;

DECLARE 
    code customer.c_id%TYPE := 1;
BEGIN
    customer_pack.add_customer(1, 'Test User', 20, 'Chandigarh', 10000000);
    customer_pack.add_customer(2, 'Aniket', 21, 'Haryana', 10000000);
    customer_pack.add_customer(3, 'demo', 21, 'Haryana', 10000000);
    DBMS_OUTPUT.PUT_LINE('After adding values in the table');
    FOR rec IN (SELECT * FROM customer) LOOP
        DBMS_OUTPUT.PUT_LINE('c_id: ' || rec.c_id || ', c_name: ' || rec.c_name || ', c_age: ' || rec.c_age || ', c_address: ' || rec.c_address);
    END LOOP;
    
    customer_pack.delete_customer(code);
    DBMS_OUTPUT.PUT_LINE('After deleting id = 1');
    FOR rec IN (SELECT * FROM customer) LOOP
        DBMS_OUTPUT.PUT_LINE('c_id: ' || rec.c_id || ', c_name: ' || rec.c_name || ', c_age: ' || rec.c_age || ', c_address: ' || rec.c_address);
    END LOOP;
END;
