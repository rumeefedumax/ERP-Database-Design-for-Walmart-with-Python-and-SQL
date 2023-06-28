# Design of Walmart Database and GUIs Using python & SQL
For this project I obtained existing data for Walmart stores from a machine learning project, cleaned the data using the pandas and numpy libraries, designed a normalized database with sqlite3, loaded the data into the database and created interactive GUIs using Tkinter for users to interact with the database and analyze the data.
### Skills/Stack
Python (Advanced), SQL (Intermediate)

Packages : Pandas, numpy, matplotlib, sqlite3, Tkinter

## Project Summary
Walmart operates a large number of retail stores in the USA.  They want to predict sales in different departments from various factors: such as the temperature, whether there is a holiday in specific week, the unemployment rate, markdown on the prices. This project will focus on database design, data cleaning and loading into an sql database and design of Graphical User interface for users to interact with the database (create, read, update, delete).

Walmart have run a successful trial of their machine learning for some of their stores in California. Because only a trial was run the data was collected using csv files
The following files come from the pilot study

* stores_data-set.csv
* store_info.csv
* Features_data_set.csv
* sales_data-set.csv

### Objectives of this project:
* Design a database to store the data in the csv files and provide an ERD Diagram
* Short discussion of database design choices. The database design should follow the third normal form 3NF (see report)
* Write a python script to read in the csv files and populate a relational database such as SQLlite.
* Develop a GUI interface, using Tkinter, to update the manager of a specific store to the database
* Develop an additional GUI that has buttons to do the following analysis after the data has been extracted from the database using SQL.
    * Calculate the mean store size for each of the types of stores “A”, “B”, and “C” and print them out.
    * Draw a series of sales versus date for a specific store and department. The GUI should input the store number and department number and draw the appropriate time series.


### Notebook Sections:
Note that each section has accompanying pre-text and a code block

1. Structure of Sample data provided
2. SQL Database Creation and Schema Definition
3. Data Cleaning and Manipulation with pandas and numpy
4. Writing Data to the Database
5. Definition of Error Checks for GUI Inputs
6. Function definitions for 'Store Record Manager' GUI
7. GUI - Store Record Manager
8. Function Definitions for 'Average Store Size Calculations and Weekly Sales Plots' GUI
9. GUI - Average Store Size Calculations and Weekly Sales Plots

### Structure of Sample Data Provided

The tables below describe the structure of the initial data provided in csv format. This data will be cleaned, transformed and restructured to fit a normalized schema for the company database

<h3 id="stores_data-set"><code>stores_data-set.csv</code></h3>
<table>
<thead>
<tr>
<th>column</th>
<th>data type</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Store</code></td>
<td><code>integer</code></td>
<td>Unique ID for each store</td>
</tr>
<tr>
<td><code>Type</code></td>
<td><code>char</code></td>
<td>Type of store(A, B or C)</td>
</tr>
<tr>
<td><code>Size</code></td>
<td><code>float</code></td>
<td>Size of store in square feet</td>
</tr>
</tbody>
</table>

<h3 id="store_info"><code>store_info.csv</code></h3>
<table>
<thead>
<tr>
<th>column</th>
<th>data type</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Store</code></td>
<td><code>integer</code></td>
<td>Unique ID for each store</td>
</tr>
<tr>
<td><code>Manager</code></td>
<td><code>varchar</code></td>
<td>Store Manager's first and last name</td>
</tr>
<tr>
<td><code>Years_as_Manager</code></td>
<td><code>integer</code></td>
<td>number of years in managerial position</td>
</tr>
<tr>
<td><code>Email</code></td>
<td><code>varchar</code></td>
<td>email address of store manager</td>
</tr>
<tr>
<td><code>Address</code></td>
<td><code>varchar</code></td>
<td>store address</td>
</tr>
</tbody>
</table>

<h3 id="Features_data-set"><code>Features_data-set.csv</code></h3>
<table>
<thead>
<tr>
<th>column</th>
<th>data type</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Store</code></td>
<td><code>integer</code></td>
<td>Unique ID for each store</td>
</tr>
<tr>
<td><code>Date</code></td>
<td><code>Date</code></td>
<td>Date value representing the week for which the aggregated features were recorded</td>
</tr>
<tr>
<td><code>Temperature</code></td>
<td><code>float</code></td>
<td>Aggregate temperature in the region measured for that week</td>
</tr>
<tr>
<td><code>Fuel_Price</code></td>
<td><code>float</code></td>
<td>Fuel price aggregated for the week in USD/gallon</td>
</tr>
<tr>
<td><code>Markdowns_1-5</code></td>
<td><code>float</code></td>
<td>Anonymized data related to promotional markdowns that Walmart is running</td>
</tr>
<tr>
<td><code>CPI</code></td>
<td><code>float</code></td>
<td>CPI recorded for the week</td>
</tr>
<tr>
<td><code>Unemployment</code></td>
<td><code>float</code></td>
<td>Unemployment rate (%) in the US for the week</td>
</tr>
<tr>
<td><code>Isholiday</code></td>
<td><code>BOOLEAN</code></td>
<td>TRUE (1) if the week is a special holiday week and FALSE(0) if not</td>
</tr>
</tbody>
</table>

<h3 id="sales_data-set"><code>sales_data-set.csv</code></h3>
<table>
<thead>
<tr>
<th>column</th>
<th>data type</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>Store</code></td>
<td><code>integer</code></td>
<td>Unique ID for each store</td>
</tr>
<tr>
<td><code>Department</code></td>
<td><code>integer</code></td>
<td>Identifier for each department</td>
</tr>
<tr>
<td><code>Date</code></td>
<td><code>date</code></td>
<td>date value representing the week for which the sales were aggregated</td>
</tr>
<tr>
<td><code>Weekly sales</code></td>
<td><code>float</code></td>
<td>Sales aggregated for the week per department in each store in US Dollars</td>
</tr>
<tr>
<td><code>Isholiday</code></td>
<td><code>BOOLEAN</code></td>
<td>TRUE (1) if the week is a special holiday week and FALSE(0) if not</td>
</tr>
</tbody>
</table>

## Database Creation and Schema Definition

* Database created using slite3
* Tables defined based on database design - Store, Manager, Sales, Features, Calendar

<b>See ERD Diagram below (developed with MySQL workbench):</b> 

<p><img src="https://drive.google.com/uc?export=download&id=1ugWB5RtDkY_-LVblrpMJ8WJHpoiKyhXq" alt></p>


## GUI Design
###	Manager and Store Record Management GUI
The GUI is designed to enable the update of a manager’s records in the database 
#### Updating Manager Details

1.	Input Store ID, click “Show store Data”
2.	Store and manager details for specific store retrieved from database and displayed in GUI
3.	Click “Update Manager Data” to open “Updating Manager Details” window. Here manager information can be modified and saved to the database 
4.	Attempt to input email in wrong format: robert.orwig@wal.org
5.	Error prompt comes up when user attempts to save manager information with wrong email format
 
 <p><img src="https://drive.google.com/uc?export=download&id=1TqESIQyTz89wG6PlFK_54WufBNNN3S3i" alt></p>
Figure: Update manager email. Data validation to ensure correct email format

6.	Input valid Manager details
7.	Click “Save record” to save changes to database
8.	Prompt appears confirming Manager details updated

<p><img src="https://drive.google.com/uc?export=download&id=14D8Mmnw7n5vEeftS_oawIODh4ZSMO3mD" alt></p>
Figur: Updating manager details. Successful update

9.	Input Store ID, click “Show store Data” to reveal updated details for Store 25
10.	Updated manager details for store 25 retrieved from database and displayed in GUI 
-	First name changed from ‘Robert’ to ‘Robbin’
-	Last name changed from ‘Orwig’ to ‘Ewings’
-	Years a manager changed from 5 to 10

<p><img src="https://drive.google.com/uc?export=download&id=1pB0JT9LdILAxGTSGn78wATpQGO6dwlgv" alt></p>
Figur: Check for updated manager details

11.	Click “Update store Data” to open window for store data updates
12.	Updated the database with Type, Size, Address, City, State, Zipcode for the specific store 
13.	Assign a different manager to the store by inputting a new Manager ID in store details. Only managers in database can be assigned to a store.

<p><img src="https://drive.google.com/uc?export=download&id=1wlgAgZeFL8mYgCU8YLZI9EYIDcAPi-s1" alt></p> 
Figure: Update store details. Assign another manager to store

###	Average Store Size Calculator and Weekly Sales Plot 
####	Store Size Calculation:

1.	Click “Show sizes button”
2.	Average store size by store type in square feet retrieved from database and displayed in GUI
3.	Message prompt confirming values have been recalculated from database

<p><img src="https://drive.google.com/uc?export=download&id=12cRfgFqITeDYhUIVCCsKDxyC8_oTW0Bx" alt></p> 
Figure: Store Size Calculation

####	Plot Weekly Sales for store and department
Steps:

1.	Input store and department (Input validation ensures values are integers and exit in the database e.g. Inputting 96 in the Store ID entry box will throw an error because no store with that store ID exists in the database) 
2.	Click “Preview sales”
3.	Preview of historical weekly sales for selected store and department generated (1st 100 records only)
4.	Click “Plot sales button” to plot weekly sales over time
5.	Plot of weekly sales by date for store and department (N.B: Sales plot can be generated even without preview)

 
<p><img src="https://drive.google.com/uc?export=download&id=1cshPoJrHx2WhitEYHyl_ClRrK8Tg0MSP" alt></p> 
Figure : Weekly Sales plot for specific store and department
