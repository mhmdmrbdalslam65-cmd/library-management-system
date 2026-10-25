# Library Management System — Assignment III

**Course task:** SQL Table Manipulation  
**DBMS:** PostgreSQL using pgAdmin 4  
**Project:** Library Management System

> Before submission, replace the student/course details below with your own details. Run the SQL in your own PostgreSQL database, generate the ERD from your database, and include your own screenshots/results. Do not claim that a screenshot or ERD was generated if you have not generated it.

## Student details
- Student name: YOUR FULL NAME
- Student ID: YOUR STUDENT ID
- Course/class: YOUR COURSE
- Instructor: Eric Maniraguha

## Project description
This project models a small library. It stores books, registered members, and borrowing transactions. A member can have multiple borrowing records, and a book can appear in multiple borrowing records.

## Tables and relationships
- `books`: book information; primary key `book_id`.
- `members`: library member information; primary key `member_id`.
- `borrowings`: borrowing transactions; primary key `borrowing_id`, foreign keys `book_id` and `member_id`.
- One book can have many borrowing records.
- One member can have many borrowing records.

## Requirements demonstrated
- Three related tables, generated primary keys, and foreign keys.
- At least five initial records per table.
- Three SELECT queries.
- Three JOIN queries: INNER JOIN, LEFT JOIN, and a multi-table JOIN.
- Three UPDATE operations.
- Three ALTER TABLE operations.
- Three INSERT operations, including a related record.
- Verification SELECT queries.

## Files
- `assignment_iii.sql` — complete PostgreSQL script.
- `erd.png` — **add the ERD exported from your own pgAdmin 4 database**.
- `evidence/` — **create this folder and add screenshots of successful execution and result grids**.
- `report_notes.md` — a guide for preparing your handwritten report.

## How to run
1. Open pgAdmin 4 and connect to your PostgreSQL server.
2. Create a new, empty database for this project.
3. Right-click that database and open **Query Tool**.
4. Open `assignment_iii.sql`, then execute the script from top to bottom.
5. Check the **Messages** and **Data Output** areas for successful execution and query results.
6. Generate an ERD from the database using pgAdmin's ERD feature if available. Export/save it as `erd.png` in this repository.
7. Capture your own screenshots of table creation, records, joins, updates/alterations/inserts, and the ERD. Put them in `evidence/`.
8. Prepare the handwritten report and redraw the ERD by hand if required by the assignment.

## Evidence checklist
- [ ] Screenshot of the three tables in pgAdmin's object browser.
- [ ] Screenshot/result evidence showing at least five initial rows in each table.
- [ ] Screenshot/result evidence for SELECT and JOIN queries.
- [ ] Screenshot/result evidence for UPDATE, ALTER TABLE, and INSERT tasks.
- [ ] Exported `erd.png`.
- [ ] Handwritten report, including a hand-redrawn ERD.
- [ ] Public GitHub repository URL checked in a browser while signed out or in a private window.

## Notes
Run this script against a fresh database. It creates tables and inserts sample data, so running it twice in the same database will cause “relation already exists” or duplicate-data errors. If you already ran part of the script, use a new empty database or carefully reset the tables before rerunning.
