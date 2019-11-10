# What is SSMS(SQL Server Management Studio)

SSMS is an integrated environment for managing any SQL infrastructure. SSMS provides tools to configure, monitor
and produce/host instances of an SQL server and databases. To download SSMS, click [here](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)
where you will be able to find an SSMS for your needs.

## SQL Review
Structured query language or SQL is a programming language used for editing and querying information within different 
database managment systems. The SQL language is base off of several different elements. These elements can be executed
by command line interface in most databse management systems, in my case I am using an SSMS. These elements include...

* Expressions – Produce scalar values or tables, made up of rows and columns of data.
* Clauses – These are the components of different queries and statements.
* Queries – Queries can retrieve data based on the provided criteria.
* Predicates – These specify conditions that are used to limit the effects of Queries and Statements or to change the flow of the entire program.
* Statements – With statements, the data handler is able to control program flow, transactions, sessions, connecntions and diagnostics.

Essentially, SQL is the main language that allows allows your database server to store and edit data within it.

## FracFoucs Registry and Database

FracFocus.org is the national registry for hydraulic fracturing chemical data. The website provides a user friendly interface in which chemical data can easily be searched by state, well API, chemical additive etc. The downfall to this is the website returns the chemical 
data in the form of pdfs which are tedious to sort through and in most cases you will recieve thousands of matches (data nightmare). The
entire database can be downloaded as a .bak or backup file and then restored using SSMS. Once resotred in SSMS, you have created your own
SQL instance or "internal server" that hosts the database directly from your computer.

## Connecting R to SSMS

