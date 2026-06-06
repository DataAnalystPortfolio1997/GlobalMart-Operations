**I am the Head of Operations at GlobalMart, a mid-sized cross-border e-commerce retailer. We sell physical products (Tech, Furniture, Office Supplies, Clothing) to customers in over 20 countries.
We are currently heading into the Q4 peak season (Black Friday & Christmas). My investors are worried because while our top-line revenue looks fine, our Net Profit has been shrinking for two quarters.
I suspect we are losing money on specific orders due to a combination of deep discounts, high shipping costs to certain regions, and inefficient payment processing fees.**

[Data link](https://drive.google.com/file/d/111_4977hmr7Eu1aSUQNCE_flnGMcG0T9/view?usp=drive_link)

**I made this project in 4 platforms:**
1. Excel (power query, pivot table, DAX measures, excel functions)
2. PL/SQL
3. Python (numpy, pandas)
4. Power BI (charts, DAX measures)

**Data cleaning, duplicate removal, and other processes were performed in PowerQuery on the original dataset. All programs use the pre-processed dataset.**

**Conclusions drawn from the data analysis:**
1. Total Net Profit lost on orders where the Discount was >20% = 6,692.29 . 
   Is there a specific product category where high discounts completely eliminate profit margins? - Office Supplies, discounts (10,15,20,25,30), average profit margin (-7%, -10%, -23%, -23%, -5%) .
2. There are not countries or regions where the average Shipping Cost exceeds the average Profit per order .
3. "Cash on Delivery" Payment Method correlates with the highest average Discount usage and encourages users to seek discounts .
4. There is not segment where shipping costs are eating up more than 15% of the sale value .
5. Highlighters Neon Pack 6 product has generated the highest total Revenue but simultaneously has a negative total Profit .
6. The analysis was conducted using data for 2023, 2024, and 2025. In 2024 and 2025, the application of discounts led to an increase in average (revenue, profit and profit margin), compared to the rest of the year. However, in 2023, all these indicators were lower for the last week of December compared to the rest of the year .
7. Average cost per delivery, number of cases where profit is less than or equal to 0, ratio of the number of such deliveries to the total number of deliveries in case of delivery of one product more than for delivery of two or more products .
8. Top customers by Sales volume who exclusively use high-fee payment methods or require international shipping : Hanna Jones, Nadia Wang, Priya Oliveira, Linda Weber, Karen Sato, Lucas Nilsson .
9. There is not product category that shows high total Units Sold but declining Profit over the 3 year period (2023-2025). The units sold and profit either increased or decreased simultaneously each year compared to the previous year, depending on the product category .
10. Are we discounting too heavily in harder-to-reach markets? - Yes, the average discount percentage for orders shipped to South America is higher than for other regions .
