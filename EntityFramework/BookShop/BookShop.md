## BookShop

Your task is to create a database application, using Entity Framework Core,   
using the Code First approach. Design the domain models and methods for  
manipulating the data, as described below.  

![alt text](https://github.com/skyepaper/C-Sharp-DB/blob/main/EntityFramework/BookShop/DbTables.bmp)  
 
 ### Project Skeleton Overview
 
You are given a project skeleton, which includes the following folders:
-	Data - contains the BookShopContext class, Models folder which contains  
 the entity classes and the Configuration class with connection string  
-	DataProcessor - contains the Serializer and Deserializer classes,  
 which are used for importing and exporting data  
-	Datasets - contains the .json and .xml files for the import part  
-	ImportResults - contains the import results you make in the Deserializer class  
-	ExportResults - contains the export results you make in the Serializer class  

##	Model Definition (50 pts)

The application needs to store the following data:
### Author
-	Id - integer, Primary Key
-	FirstName - text with length [3, 30]. (required)
-	LastName - text with length [3, 30]. (required)
-	Email - text (required). Validate it! There is attribute for this job.
-	Phone - text. Consists only of three groups (separated by '-'),  
 the first two consist of three digits and the last one - of 4 digits. (required)
-	AuthorsBooks - collection of type AuthorBook
### Book
-	Id - integer, Primary Key
-	Name - text with length [3, 30]. (required)
-	Genre - enumeration of type Genre, with possible values   
 (Biography = 1, Business = 2, Science = 3) (required)
-	Price - decimal in range between 0.01 and max value of the decimal
-	Pages – integer in range between 50 and 5000
-	PublishedOn - date and time (required)
-	AuthorsBooks - collection of type AuthorBook
### AuthorBook
-	AuthorId - integer, Primary Key, Foreign key (required)
-	Author -  Author
-	BookId - integer, Primary Key, Foreign key (required)
-	Book - Book

##	Data Import (25pts)

For the functionality of the application, you need to create several methods that manipulate the database.  
The project skeleton already provides you with these methods, inside the Deserializer class.  
Usage of Data Transfer Objects is optional.  
Use the provided JSON and XML files to populate the database with data.   
Import all the information from those files into the database.  
You are not allowed to modify the provided JSON and XML files.  
If a record does not meet the requirements from the first section, print an error message:  

**Error message**
Invalid data!
### XML Import  
**Import Books**  
Using the file books.xml, import the data from the file into the database.  
Print information about each imported object in the format described below.  
  
**Constraints**  
	If there are any validation errors for the book entity   
 (such as invalid name, genre, price, pages or published date),  
 do not import any part of the entity and append an error message to the method output.  
  
**NOTE**: Date will be in format "MM/dd/yyyy", do not forget to use CultureInfo.InvariantCulture  
  
**Success message**  
Successfully imported book {bookName} for {bookPrice}.  

**Example**  
books.xml  
\<?xml version='1.0' encoding='UTF-8'?>    
\<Books>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Hairy Torchwood</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Genre>3</Genre>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Price>41.99</Price>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Pages>3013</Pages>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<PublishedOn>01/13/2013</PublishedOn>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Anise Burnet Saxifrage</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Genre>1</Genre>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Price>-1.51</Price>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Pages>2920</Pages>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<PublishedOn>12/02/2015</PublishedOn>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Hand Fern</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Genre>2</Genre>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Price>3.57</Price>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Pages>5303</Pages>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<PublishedOn>02/23/2018</PublishedOn>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
...  
\</Books>  

**Output**  
Successfully imported book Hairy Torchwood for 41.99.  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Invalid data!  
Successfully imported book Mojave Linanthus for 82.29.  
...  

Upon correct import logic, you should have imported 70 books.  

### JSON Import  
**Import Authors**  
Using the file authors.json, import the data from that file into the database.   
Print information about each imported object in the format described below.  

**Constraints**
- If any validation errors occur (such as invalid first name, last name, email or phone),  
  do not import any part of the entity and append an error message to the method output.
- If an email exists, do not import the author and append and error message.
- If a book does not exist in the database, do not append an error message and continue with the next book.  
- If an author have zero books (all books are invalid) do not import the author 
 and append an error message to the method output.
 
**Success message**  
Successfully imported author - {first name + last name} with {booksCount} books.

**Example**  
authors.json  
[  
&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"FirstName": "K",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"LastName": "Tribbeck",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Phone": "808-944-5051",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Email": "btribbeck0@last.fm",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Books": [  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Id": 79  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Id": 40  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]  
&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"FirstName": "Maridel",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"LastName": "N",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Phone": "658-437-4751",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Email": "mdeamaya1@theatlantic.com",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Books": [  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Id": 117  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Id": 88  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]   
&nbsp;&nbsp;&nbsp;&nbsp;},  
  ...  
]  
  
**Output**
Invalid data!
Invalid data!
Invalid data!
Invalid data!
Invalid data!
Successfully imported author - Nataniel Pembery with 2 books.
Successfully imported author - Aila Fallanche with 2 books. 
...
Upon correct import logic, you should have imported 75 authors and 150 author books.

##	Data Export (25 pts)

Use the provided methods in the Serializer class. Usage of Data Transfer Objects is optional.

### JSON Export  
**Export Most Craziest Authors**  
Select all authors along with their books. Select their name in format   
first name + ' ' + last name. For each book select its name and price   
formatted to the second digit after the decimal point. Order the books   
by price in descending order. Finally sort all authors by book count   
descending and then by author full name.  
**NOTE**: Before the orders, materialize the query (This is issue by Microsoft in InMemory database library)!!!  
**Example**  
Serializer.ExportMostCraziestAuthors(context)
[  
&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"AuthorName": "Angelina Tallet",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Books": [  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Allen Fissidens Moss",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "78.44"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Earlyleaf Brome",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "63.66"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Sky Mousetail",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "13.14"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Arrowleaf Clover",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "1.71"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]  
&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"AuthorName": "Ashia Esh",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Books": [  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Twoflower Melicgrass",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "29.06"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Sky Mousetail",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "13.14"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Candle Tree",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "9.00"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookName": "Arrowleaf Clover",  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"BookPrice": "1.71"  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]  
&nbsp;&nbsp;&nbsp;&nbsp;},  
...  
]  

### XML Export  
**Export Oldest Books**  
Export top 10 oldest books that are published before the given   
date and are of type science. For each book select its name, date   
(in format "d") and pages. Sort them by pages in descending order and then bydate in descending order.  
**NOTE**: Before the orders, materialize the query   
(This is issue by Microsoft in InMemory database library)!!!  
**Example**  
Serializer.ExportOldestBooks(context, date)  
\<?xml version="1.0" encoding="utf-16"?>  
\<Books>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book Pages="4881">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Sierra Marsh Fern\</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Date>03/18/2016\</Date>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book Pages="4786">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Little Elephantshead\</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Date>12/16/2014\</Date>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book Pages="3245">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Airplant\</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Date>11/24/2016\</Date>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book Pages="3039">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Palo Blanco\</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Date>06/25/2014\</Date>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book Pages="3013">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Hairy Torchwood\</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Date>01/13/2013\</Date>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
&nbsp;&nbsp;&nbsp;&nbsp;\<Book Pages="1870">  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Name>Bigelow's Monkeyflower\</Name>  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<Date>11/20/2015\</Date>  
&nbsp;&nbsp;&nbsp;&nbsp;\</Book>  
...  
\</Books>  

