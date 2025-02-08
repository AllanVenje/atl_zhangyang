<h1>ATL Project Report</h1>

<h2>Content</h2>

- [1. Design decisions:](#1-design-decisions)
  - [1.1 MySQL Connection Pooling](#11-mysql-connection-pooling)
  - [1.2 Global Database Cursor Management](#12-global-database-cursor-management)
  - [1.3 Modular Route Organization](#13-modular-route-organization)
  - [1.4 Dynamic Template Rendering](#14-dynamic-template-rendering)
  - [1.5 Error Handling and Custom Error Pages](#15-error-handling-and-custom-error-pages)
  - [1.6 Template Reusability](#16-template-reusability)
  - [1.7 Testing Considerations](#17-testing-considerations)
  - [1.8 Development Workflow](#18-development-workflow)
- [2. Image sources:](#2-image-sources)
- [3. Database questions:](#3-database-questions)
  - [3.1 What SQL statement creates the tours table and defines its fields/columns? (Copy and paste only the relevant lines of SQL.](#31-what-sql-statement-creates-the-tours-table-and-defines-its-fieldscolumns-copy-and-paste-only-the-relevant-lines-of-sql)
  - [3.2 Which lines of SQL script set up the relationship between the tours and tourgroups tables?](#32-which-lines-of-sql-script-set-up-the-relationship-between-the-tours-and-tourgroups-tables)
  - [3.3 The current ATL only works for individual travellers. Write SQL script to create a new table called families, which includes a unique family ID, the family name, an optional short description. The ID must be added automatically by the database. (Relationships to other tables not required.)](#33-the-current-atl-only-works-for-individual-travellers-write-sql-script-to-create-a-new-table-called-families-which-includes-a-unique-family-id-the-family-name-an-optional-short-description-the-id-must-be-added-automatically-by-the-database-relationships-to-other-tables-not-required)
  - [3.4 Write an SQL statement that adds a row for a new family to your new families table. Make up some values for a fictional family](#34-write-an-sql-statement-that-adds-a-row-for-a-new-family-to-your-new-families-table-make-up-some-values-for-a-fictional-family)
  - [3.5 5.3.5 What changes would you need to make to other tables in the database to fully incorporate the new families table into the data model below? (Describe the changes. SQL script not required.)](#35-535-what-changes-would-you-need-to-make-to-other-tables-in-the-database-to-fully-incorporate-the-new-families-table-into-the-data-model-below-describe-the-changes-sql-script-not-required)


## 1. Design decisions:
### 1.1 MySQL Connection Pooling
Why: To optimize database connectivity by reusing connections instead of creating a new one for each request.
How: Implemented using MySQLConnectionPool to manage multiple database connections efficiently.
### 1.2 Global Database Cursor Management
Why: Simplify database interactions and prevent errors from repeated connection/cursor setups.
How: A global getCursor function checks connection status and returns a valid cursor each time.
### 1.3 Modular Route Organization
Why: Improve code readability and maintenance by separating features into distinct routes (e.g., /tours, /makebooking).
How: Each route handles specific tasks (e.g., listing tours, processing bookings) using request.method to switch between GET/POST logic.
### 1.4 Dynamic Template Rendering
Why: Enable responsive UI updates without reloading the page.
How: Passed data from Flask routes to Jinja2 templates using render_template(), which generates HTML dynamically.

### 1.5 Error Handling and Custom Error Pages
Why: Enhance user experience by providing clear feedback on errors.
How: Defined custom error handlers (e.g., @app.errorhandler(404)) to redirect users to friendly error pages.

### 1.6 Template Reusability
Why: Reduce duplication of HTML structures across pages.
How: Shared a consistent layout in templates (e.g., home.html, tours.html) using Jinja2 blocks.
### 1.7 Testing Considerations
Why: Enable thorough testing of application components.
How: Structured routes and functions logically to facilitate whitebox testing (e.g., unit testing individual endpoints).
### 1.8 Development Workflow
Why: Expedite development and debugging.
How: Ran the application in debug mode locally with app.run(debug=True).

## 2. Image sources:
> All image search from bing engine under Free license
- 404 page image: 
https://sitechecker.pro/wp-content/uploads/2023/06/404-status-code.png

- Home page background image
https://www.tripsavvy.com/thmb/7pEOGMVeCbvcNmT60iqYPH-J6-A=/2119x1415/filters:fill(auto,1)/panoramic-view-nature-landscape-in-south-island-new-zealand-898280184-b1d7fd1b8e86441083b564d31a75066e.jpg

- New Zealand Flags
https://th.bing.com/th/id/R.3c0e2d5669061634226a134eaf89bd61?rik=A24kJfYsQUU8hQ&pid=ImgRaw&r=0
  
- Tours page background image:  
https://guestnewzealand.com/wp-content/uploads/2013/03/Kea-Motorhome-travel-2.jpg


- Customer page background image: 
https://cdn.kimkim.com/files/a/content_articles/featured_photos/bc55703b5accf7a1c9783ec84ae4a997ca64f1c2/big-6ffe2a03fdd2112b46ffd1bc070650b8.jpg 

## 3. Database questions: 
### 3.1 What SQL statement creates the tours table and defines its fields/columns? (Copy and paste only the relevant lines of SQL.
SQL statement to create the tours table:
```SQL
CREATE TABLE tours (
    tourid INT NOT NULL AUTO_INCREMENT,
    tourname VARCHAR(50) NOT NULL,
    agerestriction int not null,
    PRIMARY KEY (tourid)
);
```

### 3.2 Which lines of SQL script set up the relationship between the tours and tourgroups tables?
The lines of the SQL script that set up the relationship between the tours and tourgroups tables are:
```sql
    PRIMARY KEY (tourgroupid),
    CONSTRAINT fk_tourid_tourgroups FOREIGN KEY (tourid)
        REFERENCES tours (tourid)
        ON DELETE NO ACTION ON UPDATE NO ACTION
```

### 3.3 The current ATL only works for individual travellers. Write SQL script to create a new table called families, which includes a unique family ID, the family name, an optional short description. The ID must be added automatically by the database. (Relationships to other tables not required.)
```sql
CREATE TABLE families (
    family_ID INT AUTO_INCREMENT PRIMARY KEY,
    family_name VARCHAR(255) NOT NULL,
    description TEXT
);
```

### 3.4 Write an SQL statement that adds a row for a new family to your new families table. Make up some values for a fictional family
```sql
INSERT INTO families (family_name, description) 
VALUES ('Yang Family', 'This is amazing family.');

```

### 3.5 5.3.5 What changes would you need to make to other tables in the database to fully incorporate the new families table into the data model below? (Describe the changes. SQL script not required.)
To seamlessly integrate the new families table into the existing data model, the following steps would be necessary:
- Introduce a Foreign Key in the Customers Table: A new column named family_id should be added to the customers table. This will serve as a bridge to connect individual customers to their corresponding families.
- Revise Relationships: Reevaluate and adjust any existing queries or logic that previously operated under the assumption that individuals were not part of families. This could entail modifying SQL query joins or conditions to accommodate the new family structure.
- Adjust Application Logic: Update the application logic to manage family-related operations effectively. This includes tasks such as retrieving a list of all family members or aggregating data across an entire family.