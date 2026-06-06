**You can download my Power BI project using this link:**
[Project in Power BI](https://drive.google.com/file/d/19phBtfrUgwLHxxJCzI72Z4lYWBMjuOyP/view?usp=drive_link)

**Tasks that were solved in this file:**
1. Profit Erosion: Calculate the total Net Profit lost on orders where the Discount was >20%. Is there a specific product category where high discounts completely eliminate profit margins?
2. Regional Drag: Identify the top 3 countries or regions where the average Shipping Cost exceeds the average Profit per order. (i.e., we lose money on every shipment there).
3. Payment Friction: Which Payment Method correlates with the highest average Discount usage? I suspect certain payment types encourage users to seek discounts.
4. The "Free Shipping" Myth: Compare the average Shipping Cost for Consumer segment vs. Corporate segment. Is there a segment where shipping costs are eating up more than 15% of the sale value?
5. Product Performance: Which specific Product Name has generated the highest total Revenue but simultaneously has a negative total Profit? (Identify the "Loss Leader").
6. Seasonal Timing: What is the average Sales amount per order for the last week of December compared to the rest of the year? Is the revenue boost worth the apparent profit drop?
7. Order Sizing: Is there a correlation between low Quantity (e.g., 1 unit) and high Shipping Cost to Profit ratio? Are small item orders killing our logistics budget?
8. High-Value Risk: Identify the top 10 customers by Sales volume. Filter this list to show only those who exclusively use high-fee payment methods or require international shipping.
9. Under-performing Inventory: Find the Product Category that shows high total Units Sold but declining Profit over the 3-year period (2023-2025). Is it time to drop this category?
10. Logistics Efficiency: Calculate the average Discount given to orders that require shipping to South America vs. Europe. Are we discounting too heavily in harder-to-reach markets?
    
**This project used various charts, the following measures and one additional table created via DAX:**
1) Correlation between quantity and shipping_cost/profit ratio = 
VAR Filtered_table = FILTER(global_ecommerce_sales, ISNUMBER(global_ecommerce_sales[Quantity]) = TRUE() && ISBLANK(global_ecommerce_sales[Quantity]) = FALSE() && global_ecommerce_sales[Profit] > 0)
VAR Average_quantity = AVERAGEX(Filtered_table, [Quantity])
VAR Average_shipping_profit_ratio = AVERAGEX(Filtered_table, DIVIDE([Shipping_Cost], [Profit], 0))
VAR Sumproduct = SUMX(Filtered_table, ([Quantity] - Average_quantity)*(DIVIDE([Shipping_Cost], [Profit], 0) - Average_shipping_profit_ratio))
VAR Quantity_sqsum = SUMX(Filtered_table, ([Quantity] - Average_quantity)^2)
VAR Shipping_profit_ratio_sqsum = SUMX(Filtered_table, (DIVIDE([Shipping_Cost], [Profit], 0) - Average_shipping_profit_ratio)^2)
RETURN DIVIDE(Sumproduct, SQRT(Quantity_sqsum*Shipping_profit_ratio_sqsum), 0)
2) Count critical_cases profit equal or less 0 = COUNTROWS(FILTER(global_ecommerce_sales, global_ecommerce_sales[Profit] <= 0))
3) Critical_case rate = DIVIDE(COUNTROWS(FILTER(global_ecommerce_sales, global_ecommerce_sales[Profit] <= 0)), COUNTROWS(global_ecommerce_sales), 0)
4) Discount rate = DIVIDE(COUNTROWS(FILTER(global_ecommerce_sales, global_ecommerce_sales[Discount_Percent] > 0)), COUNTROWS(global_ecommerce_sales), 0)
5) Europe discount percent = AVERAGEX(FILTER(global_ecommerce_sales, global_ecommerce_sales[Region] = "Europe"), global_ecommerce_sales[Discount_Percent]) 
6) Is it time to drop this category? = 
 VAR Current_year = MAX(global_ecommerce_sales[Order_Date].[Year])
 RETURN IF(Current_year > 2023, IF(SUM(global_ecommerce_sales[Quantity]) > CALCULATE(SUM(global_ecommerce_sales[Quantity]), global_ecommerce_sales[Order_Date].[Year] = Current_year - 1) && SUM(global_ecommerce_sales[Profit]) < CALCULATE(SUM(global_ecommerce_sales[Profit]), global_ecommerce_sales[Order_Date].[Year] = Current_year - 1), "Yes", "No"), "No")
7) Lost net profit on orders with discount > 20% = SUMX(FILTER(global_ecommerce_sales, global_ecommerce_sales[Discount_Percent] > 20), IF([Profit] < 0, [Quantity]*[Unit_Price] - [Total_Sales] + [Profit], [Quantity]*[Unit_Price] - [Total_Sales]))
8) Other Regions discount percent = AVERAGEX(FILTER(global_ecommerce_sales, global_ecommerce_sales[Region] <> "Europe" && global_ecommerce_sales[Region] <> "South America"), global_ecommerce_sales[Discount_Percent]) 
9) Profit < 0 and revenue is max = (SUM(global_ecommerce_sales[Total_Sales]) = CALCULATE(MAXX(FILTER(SUMMARIZE(global_ecommerce_sales, global_ecommerce_sales[Product_Name], "total revenue", SUM(global_ecommerce_sales[Total_Sales]), "total profit", SUM(global_ecommerce_sales[Profit])), [total profit] < 0), [total revenue]), ALL(global_ecommerce_sales)))*1
10) Profit change = DIVIDE(AVERAGEX(FILTER(global_ecommerce_sales, MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31), [Profit]), AVERAGEX(FILTER(global_ecommerce_sales, NOT(MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31)), [Profit]), 0)
11) Profit margin = AVERAGEX(global_ecommerce_sales, DIVIDE([Profit], [Total_Sales], 0) )
12) Profit margin dec last week = DIVIDE(AVERAGEX(FILTER(global_ecommerce_sales, MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31), [Profit]), AVERAGEX(FILTER(global_ecommerce_sales, MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31), [Total_Sales]), 0)
13) Profit margin rest year = DIVIDE(AVERAGEX(FILTER(global_ecommerce_sales, NOT(MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31)), [Profit]), AVERAGEX(FILTER(global_ecommerce_sales, NOT(MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31)), [Total_Sales]), 0)
14) Profit performance = 
 VAR Current_year = MAX(global_ecommerce_sales[Order_Date].[Year])
 RETURN IF(Current_year > 2023, DIVIDE(SUM(global_ecommerce_sales[Profit]), CALCULATE(SUM(global_ecommerce_sales[Profit]), global_ecommerce_sales[Order_Date].[Year] = Current_year - 1), 0), 1)
15) Quantity performance = 
 VAR Current_year = MAX(global_ecommerce_sales[Order_Date].[Year])
 RETURN IF(Current_year > 2023, DIVIDE(SUM(global_ecommerce_sales[Quantity]), CALCULATE(SUM(global_ecommerce_sales[Quantity]), global_ecommerce_sales[Order_Date].[Year] = Current_year - 1), 0), 1)
16) Revenue boost = DIVIDE(AVERAGEX(FILTER(global_ecommerce_sales, MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31), [Total_Sales]), AVERAGEX(FILTER(global_ecommerce_sales, NOT(MONTH(global_ecommerce_sales[Order_Date]) = 12 && DAY(global_ecommerce_sales[Order_Date]) >= 25 && DAY(global_ecommerce_sales[Order_Date]) <= 31)), [Total_Sales]), 0)
17) Shipping cost percent from sales = DIVIDE(AVERAGE(global_ecommerce_sales[Shipping_Cost]), AVERAGE(global_ecommerce_sales[Total_Sales]), 0)
18) shipping_cost/profit ratio (profit > 0) = AVERAGEX(FILTER(global_ecommerce_sales, global_ecommerce_sales[Profit] > 0), DIVIDE([Shipping_Cost], [Profit], 0))
19) South America discount percent = AVERAGEX(FILTER(global_ecommerce_sales, global_ecommerce_sales[Region] = "South America"), global_ecommerce_sales[Discount_Percent])   
20) **Table created via DAX** : High value risk = 
VAR Grouped_customers = SUMMARIZE(global_ecommerce_sales,global_ecommerce_sales[Customer_Name], "total_revenue", CALCULATE(SUM(global_ecommerce_sales[Total_Sales])), "total profit", CALCULATE(SUM(global_ecommerce_sales[Profit])))
VAR Top10_customers = TOPN(10, Grouped_customers, [total_revenue], DESC)
VAR Countries_by_cost = SUMMARIZE(global_ecommerce_sales, global_ecommerce_sales[Country], "average_shipping_cost", CALCULATE(AVERAGE(global_ecommerce_sales[Shipping_Cost])))
VAR Global_average_cost = AVERAGEX(Countries_by_cost, [average_shipping_cost])
VAR Top_countries_and_cost = FILTER(Countries_by_cost, [average_shipping_cost] > Global_average_cost)
VAR Customers_and_countries = SUMMARIZE(global_ecommerce_sales, global_ecommerce_sales[Customer_Name], global_ecommerce_sales[Country])
VAR Top_cust_and_cont = FILTER(Customers_and_countries, global_ecommerce_sales[Country] IN SUMMARIZE(Top_countries_and_cost, global_ecommerce_sales[Country]))
VAR Filter_top10_cust_step1 = ADDCOLUMNS(Top10_customers, "countries_ordered_from",VAR Current_customer = [Customer_Name] RETURN CONCATENATEX(FILTER(Customers_and_countries, [Customer_Name] = Current_customer), [Country], ", "))
RETURN ADDCOLUMNS(Filter_top10_cust_step1, "high_cost_countries", VAR Current_customer = [Customer_Name] RETURN COALESCE(CONCATENATEX(FILTER(Top_cust_and_cont, [Customer_Name] = Current_customer), [Country], ", "), "None"))


