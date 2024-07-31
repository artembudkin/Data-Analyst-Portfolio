# Excel PivotTable Project  

## Project inspiration
I am a big fan of good coffee. At home, I have different devices for making coffee in different variations, like filter coffee or cold brew. I always purchase my coffee from professional roasters that I know and enjoy. My love for good coffee inspired me to pick a dataset of coffee sales to work on. Furthermore, this dataset was particularly interesting to me as working with it involved refreshing and showcasing my lookup knowledge and skills in working with PivotTables.  

## Project summary  
The provided Excel workbook contains the sales data of the mid-sized online coffee retailer. The coffee is sold across three countries: the United States, the United Kingdom and Ireland. The business sells four different types of coffee, with various types of roast and in several sizes. The sales dataset only contains Order ID, Order Date, Customer ID, Product ID, and Quantity measures, the dimensional data is located in customers and products datasets.  

## Aim  
To understand sales distribution based on the product category over time. Identify the general distribution of the sales based on the country and identify the top 5 most contributed customers to those sales.  

## Method  
**Getting familiar with the datasets**  
I was provided with an Excel workbook that contained the following data tables:  
* Orders - fact dataset containing order information between the 2nd of January 2019 and 19th of August 2022. Contains several unpopulated columns that relate the other two dimensions datasets.    
* Customers - dimensions dataset containing detailed information about each customer.  
* Products - dimensions dataset containing coffee and roast types and their related information, like unit price, size, profit, etc.  

**Populating columns**  
The "Orders" dataset contains several columns that were not populated. The headings of these columns relate to the columns in the other two datasets. To then effectively use PivotTable, those columns should be populated with correlating information from other two datasets based on either "Product ID" or "Customer ID" as unique identifiers.  
For this project, two approaches were used to populate the empty columns. XLOOKUP function was used to populate the columns related to the "Customers" dataset, which were "Customer name", "Email", and "Country". INDEX and MATCH functions were used to populate the columns related to the "Products" dataset, which were "Coffee Type", "Roast Type", "Size", and "Unit Price".  
  
XLOOKUP function was utilized to use the unique "Customer ID" column as a search value in the "Customers" dataset. Then it returned values for each of the columns with the customer's name, their email address, the country of the order, and whether the customer had a loyalty card at the time of purchase. The following screenshot demonstrates the formula used to populate the "Customer Name", "Country", and "Loyalty Card" columns:  
![image](https://github.com/user-attachments/assets/711ad818-b967-47b0-922a-c82ebc15d464)  
  
As the column "Email" contained empty values, the formula was adjusted with an if statement to return blank instead of 0 (standard return when the formula cannot locate a value). The following screenshot demonstrates the adjusted formula used to populate the "Email" column:  
![image](https://github.com/user-attachments/assets/05587259-d8da-4217-93a2-ae0e6264cd35)  
  
INDEX and MATCH functions were utilized to use the unique "Product ID" column as a search value in the "Products" dataset. The INDEX function returns the value from a cell based on the location provided, while the MATCH function locates the desired value in the array and returns its position. The INDEX function was firstly provided with the whole products array, then the MATCH function was used to locate the row number based on the "PRODUCT ID" column and another MATCH function was used to locate the column number based on the heading of the column. The following screenshot demonstrates the formula used to populate the "Coffee Type", "Roast Type", "Size", and "Unit Price".  
![image](https://github.com/user-attachments/assets/2ca90c02-b1ff-4989-ace6-93458d508562)  
  
**Creating calculated columns**  
After populating the columns, there were a few more values that were identified as important for further steps. The total sales value for each sale was required as well as full names for the coffee and roast types.  
To calculate the total sale for each transaction, it was only required to multiply the unit price by the quantity of the order. The simple multiplication formula was used in this step.  
To get the full names of the coffee and roast types, the nested if statement was used. The following screenshots demonstrate those nested if statements.  
For the coffee type:  
![image](https://github.com/user-attachments/assets/599e767f-758c-4a8b-b15a-2522733f4ed6)  

For the roast type:  
![image](https://github.com/user-attachments/assets/ef22ed3e-a0af-418f-89b4-2ee807ce3cfd)  

Lastly, the final dataset was transformed into a table. Table format allows the PivotTable to automatically adjust to the new orders being added as well as the formulas would automatically populate unpopulated fields for the new orders.  

**PivotTables**  
There were three PivotTables created in this step, for each visualization used on the final dashboard.  
  
The first PivotTable was created to see the distribution of sales over time and named "Total Sales". The columns of PivotTable were populated with coffee types and the rows were populated with months and years from the "Order Date" column. The sum of sales was added as values, which highlighted the total number of sales for each type of coffee over time.  
The following screenshot demonstrates the "TotalSales" PivotTable:  
![image](https://github.com/user-attachments/assets/aa832bc5-cb33-47f2-a57f-0d3eaea1cc07)  

The second PivotTable was created to see the distribution of sales by country and named "CountrySales". It was a simple PivotTable with countries located as rows and the sum of sales used as values.  
The following screenshot demonstrates the "Country Sales" PivotTable:  
![image](https://github.com/user-attachments/assets/f2706f69-da94-46a0-97a2-97b289225139)  

The last PivotTable was created to identify the top 5 customers by sales and named "Top5Customers". Another simple PivotTable with customer names located as rows and the sum of sales used as values.  
The following screenshot demonstrates the "Top5Customers" PivotTable:  
![image](https://github.com/user-attachments/assets/d41d7624-95bc-4b74-8172-49d3f504bb4d)  

**Creating a dashboard**  











