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


## Lets take a look at SSMS

Once your database is restored in your SSMS, get an understanding of how it is structured. Now that you understand the layout of your database and the table associated with it, you can execute a simple query.

```
SELECT * FROM RegistryUploadIngredients Where IngredientName = 'Acetic Acid'
```

## Connecting R to SSMS


### Load ODBC library
```
library(odbc)
```
This package allows R to connect to any database that is an Open Database Connectivity Drivers such as SSMS.

### Connect to your database
```
con <- dbConnect(odbc(),Driver="SQL Server",Server="localhost\\SQLEXPRESS", Database="FracFocusRegistry", Trusted_Connection="True")
```
Now that we are connected lets see if can get the same results our database is showing us. Here I will pull in the entire Registry Upload Purpose Table into R

```
RUP <- data.table(dbGetQuery(con, "SELECT * FROM [FracFocusRegistry].[dbo].[RegistryUploadPurpose]"))
```

Remember the Acetic Acid example, lets pull it directly into R instead.

```
Acetic <- data.table(dbGetQuery(con, "SELECT * FROM RegistryUploadIngredients Where IngredientName = 'Acetic Acid';"))
```

Now that the data is into R, it makes manipulating significantly easier.

## Example from my project

Lets query all the ingredients that serve as a Breaker

```
Breaker <- data.table(dbGetQuery(con, "SELECT * FROM RegistryUploadPurpose WHERE Purpose = 'Breaker';"))
```

Now lets count how many times each Breaker is reported in this table

```
library(plyr)
Br_Freq <- count(Breaker, "TradeName")
```
By putting the table in descending order we can see the most 10 most reported Breakers, lets make a plot.

```
top10_Br <- Br_Freq[c(610,578,315,469,374,313,561,452,222,318),]

Br_Plot <- ggplot(top10_Br, aes(x=TradeName, y=freq)) + geom_bar(stat="identity", color="blue",fill="white") + 
     labs(x="TradeName", y="Frequency",title="10 Most Used Breakers in FracFocus")+geom_text(aes(label=freq), vjust=1.6, color="black", size=3.5)+theme(axis.text.x=element_text(size=10,angle=45,hjust=1,vjust=1))

Br_Plot
```
