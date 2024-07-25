# Zomato Dashboard

## Data loading:- Loading files to Power BI and then using Power Query to clean data.

## Data cleaning:- 
1-	using first row as header
2-	Merging queries from order table and restaurant table and renaming the merged column as City after filtering the data.
3-	Looking for null values & then replacing it by Other
4-	Unpivoting columns sales_qty & sales_amount to get them into row form
5-	Renaming the unpivoted column as Type and replacing sales_qty as Quantity and sales_amount as Amount.

## DAX Queries Used:-
1-	Sale Value - Sale_Value = SUM(orders[Value])
2-	Top N Sales - TopN_Sales = VAR RankValue = RANKX(ALL(orders[restaurant.city]), [Sale_Value],, DESC)
VAR SelectedRank= SELECTEDVALUE(RankTable[No])
RETURN
IF(SelectedRank=0, [Sale_Value],
IF(RankValue <=SelectedRank, [Sale_Value], BLANK()))
3-	Active Users - ActiveUsers = DISTINCTCOUNT(orders[user_id])
4-	User Count - UserCount = DISTINCTCOUNT(users[user_id])
5-	Current Year - CurrYear = 2020
6-	Previous Year - PrevYear = [CurrYear] - 1
7-	Current Year Sale - CurrYearSale = VAR Yr = [CurrYear]
RETURN
CALCULATE([Sale_Value], orders[Year] = Yr)
8-	Previous Year Sale - PrevYearSale = VAR Yr = [PrevYear]
RETURN
CALCULATE([Sale_Value], orders[Year] = Yr)
9-	Dynamic Sub-Heading - Dynamic SubHeading = "Zomato proving services in " & COUNT(orders[restaurant.city]) & " and connected with " &DISTINCTCOUNT(users[user_id]) & " where got " &COUNT(orders[user_id]) &" orders." 
10-	Dynamic Top N Titles - Dynamic_TopN_Title = VAR SelectRank= SELECTEDVALUE(RankTable[Type])
VAR SelectType= SELECTEDVALUE(orders[Type])
RETURN
SelectRank & " City " & SelectType
11-	Customers Gained - GainCustomers = VAR FilterUsers =  FILTER(SUMMARIZE(users, users[user_id]), AND([PrevYearSale]<=0, [CurrYearSale]>0))
RETURN CALCULATE([UserCount], FilterUsers)
12-	Customers Lost - LostCustomers = VAR FilterUsers =  FILTER(SUMMARIZE(users, users[user_id]), AND([CurrYearSale]<=0, [PrevYearSale]>0))
RETURN CALCULATE([UserCount], FilterUsers)
13-	Ratings Count - Rating_Count = COUNT(restaurant[rating_count])

# Dashboards:-
## 1-	Overview Dashboard
The Power BI Dashboard provides a comprehensive overview of key performance indicators (KPIs) and insightful visualizations essential for understanding business performance across various metrics.

### Major KPIs Used:
•	Total Sales: Represents the overall revenue generated.
•	Quantity: Indicates the total number of items sold.
•	Rating Count: Shows the cumulative customer ratings received.
•	Order Count: Tracks the total number of orders placed.

### Food Category Ratings:
•	Veg Rating: Displays ratings specifically for vegetarian food items.
•	Non-Veg Rating: Displays ratings for non-vegetarian food items.
•	Others Rating: Covers ratings for all other food categories.

### Visualizations:
1.	Clustered Bar Chart - Top N Sales vs City: This visualization compares the top performing sales figures across different cities. It provides a clear picture of which cities are driving the highest sales, helping in localized strategy development and resource allocation.
2.	Line Chart - Sales by Year: The line chart visualizes sales trends over time, broken down by year. This allows stakeholders to identify patterns, seasonality, and overall growth or decline in sales across different years.
Dashboard Purpose: The dashboard is designed to facilitate data-driven decision-making by presenting a consolidated view of crucial metrics such as total sales, quantity sold, customer ratings, and order counts. It empowers users to quickly grasp performance insights, identify trends, and make informed business decisions to drive growth and efficiency.

### Key Benefits:
•	Holistic View: Provides a holistic view of business performance through comprehensive KPIs.
•	Granular Insights: Allows drill-down into food category ratings for deeper insights.
•	Geographical Analysis: Enables geographical analysis through the clustered bar chart, aiding in regional strategy formulation.
•	Historical Perspective: Historical sales data represented in the line chart offers perspective on long-term performance trends.

### Conclusion: The Power BI Dashboard delivers actionable insights essential for optimizing operational efficiency, enhancing customer satisfaction, and maximizing revenue streams across different food categories and geographical locations.

## 2-	User Performance Dashboard
The User Performance Dashboard in Power BI provides critical insights into user engagement, retention, and demographic distribution, enabling strategic decisions to enhance user experience and business growth.

### Major KPIs Used:
•	Total Users: Represents the cumulative number of registered users.
•	Active Users: Indicates the number of users actively engaging with the platform.
•	Rating Count: Tracks the total number of ratings provided by users.
•	Total Orders: Shows the overall number of orders placed by users.

### Visualizations:
1.	Clustered Column Chart - Users by Age: This chart visually represents the distribution of users across different age groups. It helps in understanding the demographics of the user base and can aid in targeted marketing and product development strategies.
2.	Clustered Bar Chart - Total Users Gained: Shows the total number of users gained over a specified period, categorized into male and female. Understanding user churn rates by gender can inform retention strategies and customer relationship management efforts.
3.	Clustered Bar Chart - Total Users Lost: Shows the total number of users lost over a specified period, categorized into male and female. Understanding user churn rates by gender can inform retention strategies and customer relationship management efforts.
4.	Slicer - Quantity and Amount: This slicer allows users to filter data based on quantity and amount criteria, providing flexibility in analyzing user behaviors and transaction patterns.
Dashboard Purpose: The User Performance Dashboard aims to empower stakeholders with actionable insights into user acquisition, retention, and engagement metrics. By visualizing key KPIs and demographic data, it facilitates data-driven decision-making to optimize user strategies and improve overall business performance.

### Key Benefits:
•	User Engagement Insights: Provides a clear view of active users and their interaction levels.
•	Demographic Analysis: Enables understanding of user demographics through age and gender distribution charts.
•	Retention Strategies: Identifies trends in user retention and loss, supporting initiatives to improve user satisfaction and loyalty.
•	Transactional Analysis: Allows detailed examination of user transactions through quantity and amount slicers, aiding in revenue optimization strategies.

### Conclusion: The User Performance Dashboard serves as a vital tool for monitoring user-centric metrics and leveraging data-driven strategies to enhance user acquisition, retention, and overall business success.

## 3-	 City Performance Dashboard
The City Performance Dashboard in Power BI provides a comprehensive view of key performance indicators (KPIs) essential for evaluating and comparing business performance across different cities.

### Major KPIs Used:
•	Total Sales: Represents the total revenue generated across all cities.
•	Quantity: Indicates the total number of items sold across cities.
•	Rating Count: Shows the cumulative customer ratings received in each city.
•	Order Count: Tracks the total number of orders placed in each city.

### Visualizations:
1.	Clustered Bar Chart - Total Sales by City: This chart compares the total sales figures across different cities, providing insights into revenue contributions from each geographical area. It helps in identifying top-performing cities and potential growth areas.
2.	Clustered Bar Chart - Rating Count by City: Visualizes the distribution of customer ratings across cities. This helps in understanding customer satisfaction levels and identifying areas where improvements may be needed.
3.	Clustered Bar Chart - Active Users by City: Displays the number of active users in each city. This metric is crucial for assessing user engagement and market penetration in different geographical regions.
4.	Slicers - City, Gender, Cuisine: Enables interactive filtering based on city names, gender demographics, and cuisine preferences. These slicers enhance flexibility in analyzing and segmenting data to uncover meaningful insights.
5.	Table Chart - City, Orders, and Sales: Presents a detailed table listing cities alongside corresponding order counts and total sales figures. This tabular format allows for quick comparisons and detailed analysis of performance metrics across cities.
Dashboard Purpose: The City Performance Dashboard is designed to provide actionable insights into regional sales trends, customer satisfaction levels, and user engagement metrics. It enables stakeholders to optimize marketing strategies, allocate resources effectively, and capitalize on regional opportunities.

### Key Benefits:
•	Performance Comparison: Facilitates comparison of sales, quantity, ratings, and user activity across different cities.
•	Insightful Visualizations: Visual representations aid in quickly identifying trends and outliers in city-level performance.
•	Data-Driven Decisions: Empowers decision-makers with the information needed to prioritize and implement targeted business strategies.
•	Detailed Analysis: The table chart provides a granular view of city-specific metrics, supporting in-depth analysis and strategic planning.

### Conclusion: The City Performance Dashboard leverages data visualization to provide a comprehensive understanding of regional performance metrics. By focusing on key KPIs and using visual insights, businesses can enhance operational efficiency, improve customer satisfaction, and drive growth across diverse geographical markets.
Create summary of the Zomato Power BI Dashboard containing three slides namely Overview, User Performance and City Performance. Below are the details of what these three dashboard slides contains within.
