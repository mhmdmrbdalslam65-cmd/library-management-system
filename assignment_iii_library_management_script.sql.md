\-- ============================================================================= -- ASSIGNMENT III: SQL TABLE MANIPULATION -- Course: Database Management Systems -- Instructor: Eric Maniraguha -- Institution: Kigali Independent University (ULK) -- Student Name: \[mohamed omer\] -- Student ID: \[202513481\] -- Selected Business Idea: Library Management System -- Database Engine: PostgreSQL 15+ -- =============================================================================

\-- SECTION 1: DATABASE CREATION

\-- Naming convention format: mohamed_omer_202513481_projectname DROP DATABASE IF EXISTS mohamed_Omer_202513481_library_system;

CREATE DATABASE mohamed_omer_202513481_library_system;

\-- Connect to database (Uncomment if executing via psql command line) -- \\c mohamed_omer_202513481_library_system;

\-- SECTION 2 & 3: TABLE CREATION AND CONSTRAINTS

\-- Drop existing tables if re-running script (Child tables first, then Parent tables) DROP TABLE IF EXISTS loans; DROP TABLE IF EXISTS books; DROP TABLE IF EXISTS members;

\-- 1. PARENT TABLE 1: members -- Stores library members/borrowers. CREATE TABLE members ( member_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY, full_name VARCHAR(150) NOT NULL, email VARCHAR(100) NOT NULL UNIQUE, phone VARCHAR(20), membership_date DATE DEFAULT CURRENT_DATE );

\-- 2. PARENT TABLE 2 / CHILD TABLE 1: books -- Relates to categories/authors implicitly or acts as standalone parent entity for loan records CREATE TABLE books ( book_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY, title VARCHAR(200) NOT NULL, author VARCHAR(150) NOT NULL, genre VARCHAR(50) NOT NULL, publication_year INT CHECK (publication_year > 0), available_copies INT NOT NULL CHECK (available_copies >= 0) );

\-- 3. CHILD TABLE 2: loans -- Relates to members and books via foreign keys 'member_id' and 'book_id' CREATE TABLE loans ( loan_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY, member_id INT NOT NULL, book_id INT NOT NULL, borrow_date DATE DEFAULT CURRENT_DATE, due_date DATE NOT NULL, return_status VARCHAR(20) DEFAULT 'Borrowed' CHECK (return_status IN ('Borrowed', 'Returned', 'Overdue')), CONSTRAINT fk_loans_member FOREIGN KEY (member_id) REFERENCES members(member_id) ON DELETE RESTRICT ON UPDATE CASCADE, CONSTRAINT fk_loans_book FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE RESTRICT ON UPDATE CASCADE );

\-- SECTION 4: INITIAL DATA INSERTION -- Minimum of 5 initial records per table (Parent tables first, then Child tables)

\-- Insert into PARENT TABLE: members INSERT INTO members (full_name, email, phone, membership_date) VALUES ('Jean Paul Ndayishimiye', 'jeanpaul@gmail.com', '+250788111222', '2026-01-15'), ('Marie Claire Uwineza', 'm.uwineza@yahoo.com', '+250788333444', '2026-02-10'), ('Eric Mugisha', 'eric.mugisha@ulk.ac.rw', '+250788555666', '2026-03-01'), ('Aline Umutoni', 'aline.umutoni@gmail.com', '+250788777888', '2026-04-12'), ('David Kwizera', 'david.k@outlook.com', '+250788999000', '2026-05-20');

\-- Insert into PARENT TABLE: books INSERT INTO books (title, author, genre, publication_year, available_copies) VALUES ('Database System Concepts', 'Abraham Silberschatz', 'Computer Science', 2019, 5), ('Clean Code', 'Robert C. Martin', 'Software Engineering', 2008, 2), ('Introduction to Algorithms', 'Thomas H. Cormen', 'Computer Science', 2009, 1), ('The Pragmatic Programmer', 'Andrew Hunt', 'Software Engineering', 1999, 4), ('To Kill a Mockingbird', 'Harper Lee', 'Fiction', 1960, 0);

\-- Insert into CHILD TABLE: loans INSERT INTO loans (member_id, book_id, borrow_date, due_date, return_status) VALUES (1, 1, '2026-10-10', '2026-10-24', 'Borrowed'), (2, 2, '2026-10-01', '2026-10-15', 'Overdue'), (3, 4, '2026-10-18', '2026-11-01', 'Borrowed'), (4, 3, '2026-09-15', '2026-09-29', 'Returned'), (5, 5, '2026-10-05', '2026-10-19', 'Overdue');

\-- SECTION 5: SQL PRACTICAL TASKS

\-- ============================================================================= -- SUBSECTION 5.1: SELECT (Data Retrieval) -- =============================================================================

\-- Q1.1: Basic Data Retrieval -- Objective: Retrieve title, author, and available copies for all books in the library. SELECT title, author, available_copies FROM books;

\-- Q1.2: Conditional Data Retrieval -- Objective: Retrieve books that currently have no available copies left for loan. SELECT book_id, title, author, available_copies FROM books WHERE available_copies = 0;

\-- Q1.3: Sorted Data Retrieval -- Objective: Retrieve all members ordered alphabetically by full name. SELECT member_id, full_name, email, membership_date FROM members ORDER BY full_name ASC;

\-- ============================================================================= -- SUBSECTION 5.2: JOIN (Retrieving Related Data) -- =============================================================================

\-- Q2.1: INNER JOIN -- Objective: Retrieve loan records displaying member name and borrowed book title. SELECT l.loan_id, m.full_name AS borrower_name, b.title AS book_title, l.borrow_date, l.return_status FROM loans l INNER JOIN members m ON l.member_id = m.member_id INNER JOIN books b ON l.book_id = b.book_id;

\-- Q2.2: LEFT JOIN -- Objective: Display all members and any books they currently have on loan, including members without active loans. SELECT m.full_name, m.email, l.loan_id, l.return_status FROM members m LEFT JOIN loans l ON m.member_id = l.member_id;

\-- Q2.3: Multi-Table JOIN -- Objective: Generate a comprehensive loan report linking members, books, and loan statuses. SELECT l.loan_id, m.full_name AS member_name, m.phone, b.title AS book_title, b.genre, l.borrow_date, l.due_date, l.return_status FROM loans l INNER JOIN members m ON l.member_id = m.member_id INNER JOIN books b ON l.book_id = b.book_id ORDER BY l.due_date ASC;

\-- ============================================================================= -- SUBSECTION 5.3: UPDATE (Modifying Existing Data) -- =============================================================================

\-- Q3.1: Update One Record -- Objective: Update phone number for member Jean Paul Ndayishimiye (member_id = 1). -- Target Identification: SELECT member_id, full_name, phone FROM members WHERE member_id = 1;

\-- Execution: UPDATE members SET phone = '+250788000111' WHERE member_id = 1;

\-- Verification: SELECT member_id, full_name, phone FROM members WHERE member_id = 1;

\-- Q3.2: Update Multiple Records -- Objective: Increase available copies by 2 for all books in the 'Computer Science' genre. -- Target Identification: SELECT book_id, title, genre, available_copies FROM books WHERE genre = 'Computer Science';

\-- Execution: UPDATE books SET available_copies = available_copies + 2 WHERE genre = 'Computer Science';

\-- Verification: SELECT book_id, title, genre, available_copies FROM books WHERE genre = 'Computer Science';

\-- Q3.3: Conditional Update -- Objective: Change loan return_status to 'Overdue' for all loans past due date. -- Target Identification: SELECT loan_id, member_id, due_date, return_status FROM loans WHERE due_date < CURRENT_DATE AND return_status = 'Borrowed';

\-- Execution: UPDATE loans SET return_status = 'Overdue' WHERE due_date < CURRENT_DATE AND return_status = 'Borrowed';

\-- Verification: SELECT loan_id, member_id, due_date, return_status FROM loans WHERE return_status = 'Overdue';

\-- ============================================================================= -- SUBSECTION 5.4: ALTER TABLE (Modifying Table Structure) -- =============================================================================

\-- Q4.1: Rename a Column -- Objective: Rename column 'return_status' to 'loan_status' in the loans table. ALTER TABLE loans RENAME COLUMN return_status TO loan_status;

\-- Verification: SELECT loan_id, due_date, loan_status FROM loans LIMIT 3;

\-- Q4.2: Change a Column's Data Type -- Objective: Change 'genre' column in books table from VARCHAR(50) to VARCHAR(100). ALTER TABLE books ALTER COLUMN genre TYPE VARCHAR(100);

\-- Verification: SELECT column_name, data_type, character_maximum_length FROM information_schema.columns WHERE table_name = 'books' AND column_name = 'genre';

\-- Q4.3: Add or Remove a Column -- Objective: Add a new column 'isbn' to the books table. ALTER TABLE books ADD COLUMN isbn VARCHAR(20) DEFAULT '978-0000000000';

\-- Verification: SELECT book_id, title, isbn FROM books LIMIT 3;

\-- ============================================================================= -- SUBSECTION 5.5: INSERT (Adding New Records) -- =============================================================================

\-- Q5.1: Insert a Single Record -- Objective: Insert a single new member into the members table. INSERT INTO members (full_name, email, phone, membership_date) VALUES ('Kevine Umhoza', 'kevine.u@gmail.com', '+250788123123', CURRENT_DATE);

\-- Verification: SELECT \* FROM members WHERE email = 'kevine.u@gmail.com';

\-- Q5.2: Insert Multiple Records -- Objective: Insert multiple new books into the books table. INSERT INTO medicines (title, author, genre, publication_year, available_copies, isbn) VALUES -- Corrected table syntax: INSERT INTO books (title, author, genre, publication_year, available_copies, isbn) VALUES ('Design Patterns', 'Erich Gamma', 'Software Engineering', 1994, 3, '978-0201633610'), ('Compilers: Principles, Techniques, and Tools', 'Alfred V. Aho', 'Computer Science', 2006, 2, '978-0321486813');

\-- Verification: SELECT \* FROM books WHERE title IN ('Design Patterns', 'Compilers: Principles, Techniques, and Tools');

\-- Q5.3: Insert a Related Record -- Objective: Insert a new loan record linked to an existing member (member_id = 3) and book (book_id = 2). INSERT INTO loans (member_id, book_id, borrow_date, due_date, loan_status) VALUES (3, 2, CURRENT_DATE, CURRENT_DATE + INTERVAL '14 days', 'Borrowed');

\-- Verification: SELECT l.loan_id, m.full_name, b.title, l.borrow_date, l.due_date, l.loan_status FROM loans l JOIN members m ON l.member_id = m.member_id JOIN books b ON l.book_id = b.book_id WHERE l.loan_id = (SELECT MAX(loan_id) FROM loans);

\-- ============================================================================= -- END OF ASSIGNMENT III SQL SCRIPT -- =============================================================================