# Power BI Data Visualization Project

## Project inspiration
As an Assistant Operations Manager, a big part of my job is to control and improve business performance. To understand the current state of a business, there are a variety of key business indicators that provide meaningful information about it. For this project, I took a hypothetical global business sales dataset to create a visually appealing report that would drive informative decisions by highlighting the most relevant business performance information and allowing the identification of trends and patterns through relevant visuals.

## Project summary
The company Plant Co. is a hypothetical global plant retailer. The company would like to utilize its data to create a clear dashboard highlighting the business performance for the management team. It provided an Excel workbook containing three datasets. Two of them contained dimensions tables: a customer accounts table and a products table, and one fact table containing sales data.

## Aim
To derive key business indicators and create a meaningful report to showcase the current state of the business.

## Method
### Getting familiar with the datasets
I was provided with an Excel workbook that contained the following data tables:  
* Plant_FACT - fact table containing sales data for the company between the 1st of January 2022 and the 14th of April 2024.
* Accounts - dimensions table containing unique customer accounts with a unique column "Account_id".
* Plant_Hierarchy - dimensions table containing unique products sold by a company with a unique column "Product_Name_id".

### Power Query data pre-processing
After opening the Excel workbook file in Power BI, there were a few minor data pre-processing techniques that were done.  
The name of each table was changed to the appropriate format:  
* Plant_FACT = Fact_Sales  
* Accounts = Dim_Account  
* Plant_Hierarchy = Dim_Product  
A formatting convention is required to ensure that any other Power BI developer can understand the purpose of each table straight away

A few column names were changed and data types were checked to ensure that they were appropriate for each column. Lastly, for unique columns, the function "Remove duplicates" was applied to avoid any duplicate values in them.  
Finally, to ensure that everything was done correctly, the "Column quality" was checked under the "View" ribbon.

### Creating date table
The next step was to create a specific date table with a unique entry for each day.  
Under the "Modelling" ribbon, the "New table" function was used.  
  
For the DAX formula, the following query was used:  
![image](https://github.com/user-attachments/assets/c481f072-c70c-45a1-8e7f-c9d767abca7e)  
This formula created a table comprised of one column containing dates for every single day between the 1st of January 2022 and the 31st of December 2024.  
  
Also, a calculated column was added to this table using the following DAX formula:  
![image](https://github.com/user-attachments/assets/3d770f68-78ca-4378-a9dc-5296a3d63818)  
This formula returned TRUE in all cases where the date was before the date of the last purchase and FALSE for all other dates.  
This column was created to be used in the further steps for calculating PYTD figures.  

### Table relationships
The next step was to establish relationships between tables to form a star schema.  
The following relationships were established:  
* Product_name_id[Dim_product] - One-to-many - Product_id[Fact_Sales]  
* Account_id[Dim_account] - Many-to-many - Account_id[Fact_sales] - due to one blank value present, it was not possible to establish a one-to-many relationship
* Date[Dim_Date] - One-to-many - Date_Time[Fact_Sales]  
  
The following screenshot demonstrates the star schema containing the above relationships:  
![image](https://github.com/user-attachments/assets/4f7aa7c3-ea49-44f6-9a17-4746207e1c8c)  

### Creating measures
The following measures were created to derive key business indicators:  
* COGS = SUM(Fact_Sales[COGS_USD])  
* Sales = SUM(Fact_Sales[Sales_USD])  
* Quantity = SUM(Fact_Sales[quantity])  
* Gross Profit = [Sales] - [COGS]
* GP% = DIVIDE([Gross Profit],[Sales])  
  
These are all the base measures. They were added to the same named folder.  
From here it was required to identify how the business is performing relative to the previous year.  
  
The following measures were derived for YTD:
* YTD_GrossProfit = TOTALYTD([Gross Profit],Fact_Sales[Date_Time])  
* YTD_Quantity = TOTALYTD([Quantity],Fact_Sales[Date_Time])  
* YTD_Sales = TOTALYTD([Sales],Fact_Sales[Date_Time])
  
The following measures were derived for PYTD:
* PYTD_GrossProfit = CALCULATE([Gross Profit], SAMEPERIODLASTYEAR(Dim_Date[Date]), Dim_Date[Inpast]=TRUE())  
* PYTD_Quantity = CALCULATE([Quantity], SAMEPERIODLASTYEAR(Dim_Date[Date]), Dim_Date[Inpast]=TRUE())  
* PYTD_Sales = CALCULATE([Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]), Dim_Date[Inpast]=TRUE())  
  
All these measures would allow us to understand the performance of the business across three main categories in comparison to the previous year.  
  
Next, to make a report more dynamic, the table with three category values(Sales, Gross Profit, Quantity) named "Slicer_Values" was created.
Lastly, the measures to change values between these three categories were created to make all the future visuals dynamic, when picking a specific category in the slicer:  
* S_PYTD = VAR selected_value = SELECTEDVALUE('Slicer_Values'[Values])VAR result = SWITCH(selected_value,"Sales", [PYTD_Sales],"Quantity", [PYTD_Quantity],"Gross Profit", [PYTD_GrossProfit],BLANK()) RETURN result  
* S_YTD = VAR selected_value = SELECTEDVALUE(Slicer_Values[Values])VAR result = SWITCH(selected_value,"Sales", [YTD_Sales],"Quantity", [YTD_Quantity],"Gross Profit", [YTD_GrossProfit],BLANK()) RETURN result  
* YTD vs PYTD = [S_YTD] - [S_PYTD]  
  
The following image demonstrates the result of the current section:  
  ![image](https://github.com/user-attachments/assets/0b6ea5fd-f37e-477b-9cd7-48cc44066b08)

### Creating visuals
The following section is dedicated to each specific visual presented in the report.  
  
**Slicer:**  
There are two slicers present on the report page. The first one slices the report by the year-to-date and is formatted as a drop-down list named "YTD". The second one allows an end-user to filter the report by three main categories and formatted as buttons. For this slicer also a hover effect was added to highlight the hovered field.  
  
The following screenshot highlights the slicers:  
![image](https://github.com/user-attachments/assets/3150c7e3-4134-470b-a9a1-9e5fdf2bb9bc)  

**Card:**  
The report contains two different card visuals. Card visuals are a great way to have the most important and influential information displayed to the end user.  
  
The one located in the left top corner contains a title for the report. It is comprised of a company name and two dynamic fields. These fields change depending on the selection made in both slicers.  
  
The second card visual is located in the top right corner containing for most important measures: YTD, YTD vs PYTD, PYTD, and GP%. Each of them dynamically changes depending on the category selected in the slicer. For the "YTD vs PYTD" field conditional formatting was added to highlight the value in the correct colour depending on whether it is negative or positive. "GP%" is the only static field that changes depending on the YTD selected.  

The following screenshot highlights the card visuals:  
![image](https://github.com/user-attachments/assets/8565ea83-aee4-40cd-a8f5-662a96b9cebc)  

**Treemap:**  
Treemap visual comprises several tiles varying in size based on their value.  
  
For this report, a treemap visual was used to highlight the Top 10 bottom-performing countries based on three categories. It uses a Top N filtering to determine the bottom 10 countries by their variance in specific categories. This visual highlights to the end user the areas that require immediate attention.  
  
The following screenshot highlights the treemap visual:  
![image](https://github.com/user-attachments/assets/2de0ad51-9863-495f-9fb1-83d554322121)  

**Waterfall:**  
The waterfall visual is exceptionally useful in highlighting the significant changes in performance. This highlights to the end user a specific period where the biggest performance change has occurred to identify the potential drivers for such behaviour.  
  
The report's waterfall visual allows an end user to drill down on specific periods to see changes split by country and then product. It is also equipped with a dynamic title that changes depending on the selected category.  
  
The following screenshot highlights the waterfall visual:  
![image](https://github.com/user-attachments/assets/9c1472f9-d892-491d-b5ac-9f495ad23586)  

**Stacked bar chart:**
The stacked bar chart provides its use in differentiating three categories by product type over a period of time. This allows an end user to identify patterns based on categories as well as identify the least performing category and drive informative business decisions.  
  
The stacked bar chart in the report allows to drill down based on the time period required, from quarters to months. The title for this visual is also dynamic and a PYTD line has been added to highlight the areas which outperformed prior year-to-date.  
  
The following screenshot highlights the stacked bar chart:  
![image](https://github.com/user-attachments/assets/be74fdf7-cc39-4d2a-b695-90144164cea7)  

**Scatter chart:**
The scatter chart plots single sales dot points based on their value and GP%. This visual helps an end user to identify the outlier sales in the data and focus on them. This can drive informative decisions based on the outlier analysis to drive further sales.  
  
The scatter chart is equipped with a dynamic title and two average lines, for x and y axes. It also contains two sliders to allow the user to zoom into certain regions of the chart.  
  
The following screenshot highlights the scatter chart:  
![image](https://github.com/user-attachments/assets/668cf586-e069-4bba-ba8e-622f4ada0157)  

## Final result  
The final result is a dashboard containing key performance indicators, and slicers that allow filtering of data by year, gross revenue, quantity of units sold, and sales. The dashboard contains several visualizations comprised of a treemap, waterfall graph, stacked bar chart, and scatter plot highlighting the business performance relative to the previous year to date. The waterfall graph and stacked bar chart allow end-users to drill down to more detailed information based on time to identify performance drivers for those sections. This allows an end user to quickly see the current business performance and quickly identify patterns and trends to focus on and make informative decisions.  
  
The following screenshots demonstrate the report based on three different categories:  
![image](https://github.com/user-attachments/assets/53633014-e8e0-4d0a-88ee-25ac2f68ac61)  
  
![image](https://github.com/user-attachments/assets/56e84a71-7b13-4f7b-a2cd-22eb9f6a5bbe)  
  
![image](https://github.com/user-attachments/assets/0b2f3d35-0a99-4393-bde0-d9edb6295d3d)










  





