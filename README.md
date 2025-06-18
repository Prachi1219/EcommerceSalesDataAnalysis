Ecommerce Sales Data Analysis


## Problem Statement
Walmart is a company with multiple outlets across different areas, offering products across various categories such as Electronics, Footwear, Clothing, Home Appliances, Accessories, Kitchenware, Bags, and Personal Care. The company maintains detailed data on offline customer purchases, including product quantities sold, net sales, and profits.
It also contains the details of promotion code that is applied to different products to increase the sale and profit. 


## Solution
Created a dashboard in **PowerBI** that provides valuable insights to **Walmart by analyzing sales and customer behavior**. It tracks total sales and profits by product, identifies the most and least purchased products, and monitors the impact of promotional codes and discounts on sales and profitability. Additionally, it offers area-wise and city-wise sales performance, highlighting top 5 and bottom 5 products based on quantity sold and profit earned.

The dashboard also analyzes sales trends over different time periods—daily, weekly, monthly, quarterly, and yearly—helping Walmart better understand customer preferences and optimize business strategies accordingly.

## Steps Followed :

- Step 1 : Load the data into Power BI Desktop, the dataset is a excel file. The dataset contains the 4 tables(Dim Customer, Dim Promotion, Dim Product, Fact table)
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : In Dim Promotion table, navigate to "Add Column" tab then define the rules as per the values in "Price Reduction Type" column to get the percentage of discount offered for the products.
- Step 5: The Fact table contains the data of the sales, so it has columns with null values, so to get the data for null values, Data Transformation is applied.
- Step 6 : For "Price per Unit" column in 'Fact table' contains 0% valid values, so for the valid values, pulled the data from 'Dim Product' table by applying 'Merge Queries' option under 'Home' tab. It give option to select the tables and columns to merge the tables similar to the joins in sql.
- Step 7 : For "Total Sales" column in 'Fact table' contains 0% valid values, so for the valid values, performed the operation ("Price per Unit" * "Units sold") columns from 'Fact Table' by navigating to "Add column" tab and selecting "Custom Column".
- Step 8 :For "Discount Percentage" column in 'Fact table' contains 0% valid values, so for the valid values, pulled the data from 'Dim Promotion' table by applying 'Merge Queries' option under 'Home' tab. It give option to select the tables and columns to merge the tables similar to the joins in sql.
- Step 9 : For "Discount Value" column in 'Fact table' contains 0% valid values, so for the valid values, performed the operation (("Total Sales" * "Discount Percentage")/100) columns from 'Fact Table' by navigating to "Add column" tab and selecting "Custom Column".
- Step 10 : For "Net Sales" column in 'Fact table' contains 0% valid values, so for the valid values, performed the operation ("Total Sales" - "Discount Value") columns from 'Fact Table' by navigating to "Add column" tab and selecting "Custom Column".
- Step 11 : After applying all the transformations, navigate to 'Data Model View' from left navigation pane and create the relationship between Dim tables and Fact table .
- Step 12 : Since the data contains various products, in the 'Report View', 6 visuals are selected to represent Top and Bottom 5 Products by Sales/Profit/Quantity sold.
- Step 13 : For "Profit" column in 'Fact table', performed the operation (0.1 * Net Sales)(assuming the profit is 10% of net sales) by navigating to "Add column" tab and selecting "Custom Column".
- Step 14 :Using Visual level Filter from Filters pane, Top 5 and Bottom 5 products are filters out for each visual and then visual formatting is performed for each visual under Visualization pane.

Snap of Top Sales and Bottom Sales of all the Products

Snap of Top Sales and Bottom Profit of all the Products

- Step 15 : For Sales Trend over time, select 'Line Chart' from Visualization pane and plot the graph for Net sales by Date from 'Fact Table'. By using the drill down arrow in visual, we can check for the sales trend on Daily, Weekly, Monthly and Yearly basis. Then also formatting the visual for better presentation under visualization pane.

Snap of Sales Trend 


- Step 16 : Another visual is selected to show relationship between Net Sales and Profit and then formatting is performed.

Snap of Profit vs Net Sales



- Step 17 : Another visual is selected to show Average discount offered in various discount category and then formatting is performed.
          Using visual level filter from the filters pane, basic filtering was used & blank values were unselected.
- Step 18 : Another visual(Map) is selected to show sales in different cities and then formatting is performed.
- Step 19 : To get the total number of orders, added the index column and name 'Order ID' in Power Query Editor by selecting 'Add Index Column' from table icon present at the left of table structure.
- Step 20 : Add visual(Card) and select Order Id and the select Order(Distinct) option from drop down from visualization pane and then formatting is performed.
- Step 21 : To compare sales/profit/quantity sold between any 2 periods, created the 2 date tables from below DAX query under 'Modeling' tab by selecting 'New Table' option.
          Date Table 1 : CALENDERAUTO()(will create a table with date column by referring to the other tables having date columns)
	  Date Table 2 : CALENDERAUTO()(will create a table with date column by referring to the other tables having date columns)
Create the relationship of date tables with the Fact table under Data Model view.
- Step 22 : Add visual(Slicer) for both Date Tables.
- Step 23 : Create new measure by selecting New Measure option and writing below DAX query :
          For net sales : Sum of Net Sales = CALCULATE(SUM(net sales from table fact),all(date table 1), USERALATIONSHIP(Fact table, Date Table 2))
Note : Here the relationship between Date Table 2 and Fact Table was inactive, so to make it active we used USERELATIONSHIP function.
- Step 24 : Add Visual to represent the graphs for both Date Table 1 and Date table 2.
          Follow step 22 and 23 for profit and quantity as well and then perform formatting for all the visuals.

- Step 23 and 24 can also be performed in one more way by using 'Edit Interactions' :
	(a) No need to define Date Table 1 and Date Table 2, just create slicer by using Date column from Fact table.
	(b) Create a bar chart and plot Sum of Net sales by selecting net sales from Fact table. Perform the similar task for profit as well quantity sold. Then perform 	   the formatting.
	(c) Plot the same 3 chart for the Slicer 2 as well.
	(d) Select Date slicer 1, click on 'Edit Interactions' under 'Format' tab.
	(e) Apply Filter behavior to None for all the charts created for Date Filter 2 and perform the same operation for chart created for Date Filter 1.So that when      	   we change the Date Filter 1 slicer the Date Filter 2 slicers are not updated and vice versa.
Note : The second approach is better as there is no need to create the Date tables, so it will not increase the Data Model View size, so second approach is recommended.

- Step 25 : To show all the details for each order, visual filter(Product/Date/Customer ID/Promotion Categories) is created by using slicer for each filter and then applying Style as Dropdown under the Visual while formatting the visual and also a Table(same as Fact Table is created to get the complete data of the table using Table visual).

