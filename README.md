# Sales data analysis of Adventure Works Bike Shop

## Overview
### Situation
You've just been hired as an Analyst by **_AdventureWorks_**, a global manufacturing company that produces cycling equipment and accessories.

### Brief
The management team needs a way to *track KPIs* (sales, revenue, profit, returns), **compare regional performance, analyze product-level trends,** and **identify high-value customers**.<br><br>

### Objective
I'll use Power BI Desktop to:
- Connect and transform the raw data
- Build a relational model
- Create calculated columns and measures with DAX
- Design an interactive dashboard to visualize the data

## Data
- **Source**<br>
This is a simulated data is from [Maven Analytics](https://mavenanalytics.io/) as part of their *Microsoft Power BI Desktop* course.

- **Format & Schema**<br>
There are 10 files in csv format. Below are the schemas of each file:<br>

    |File|Column|Comment|
    |:-|:-|:-|
    |AdventureWorks Sales Data 2020.csv<br>AdventureWorks Sales Data 2021.csv<br>AdventureWorks Sales Data 2022.csv|OrderDate<br>Stock Date<br>OrderNumber<br>ProductKey<br>CustomerKey<br>TerritoryKey<br>OrderLineItem<br>OrderQuantity|Sales data for 2020, 2021, 2022
    |AdventureWorks Returns Data.csv|ReturnDate<br>TerritoryKey<br>ProductKey<br>ReturnQuantity|Product return data
    |AdventureWorks Product Lookup.csv|ProductKey<br>ProductSubcategoryKey<br>ProductSKU<br>ProductName<br>ModelName<br>ProductDescription<br>ProductColor<br>ProductSize<br>ProductStyle<br>ProductCost<br>ProductPrice|Individual product meta data
    |AdventureWorks Product Categories Lookup.csv|ProductCategoryKey<br>CategoryName|Product category meta data
    |AdventureWorks Product Subcategories Lookup.csv|ProductSubcategoryKey<br>SubcategoryName<br>ProductCategoryKey|Product subcategory meta data
    |AdventureWorks Territory Lookup.csv|SalesTerritoryKey<br>Region<br>Country<br>Continent|Region meta data
    |AdventureWorks Customer Lookup.csv|CustomerKey<br>Prefix<br>FirstName<br>LastName<br>BirthDate<br>MaritalStatus<br>Gender<br>EmailAddress<br>AnnualIncome<br>TotalChildren<br>EducationLevel<br>Occupation<br>HomeOwner|Customer meta data
    |AdventureWorks Calendar Lookup.csv|Date|Contains each date in *YYYY-MM-DD* format from Jan 2020 to Jun 2022
    |||

## Data Transformation
Once the data is imported in Power BI Desktop, I have applied data transformation steps to aid in writing measures and/or creating visualizations. Below are some of the main data transformation steps applied:

### Power Query
- **Data Type Validation**<br>
Validate and update, if required, data type of each column to make sure that it is appropriate for the column. Example, changing the `ProductCost` column data type to `Currency`, changing `OrderDate` column data type to `Date` etc.

- **Filtering rows or columns**<br>
Example, removing the rows with `NULL` `CustomerKey` values from `Customer Lookup`. Removing `ProductDescription` column from `Product Lookup` which will not be used in our analysis.

- **Adding new columns**<br>
Example, to aid in creating time intelligence measures, adding new columns in `Calendar Lookup` table like `Start of Week`, `Start of Month`, `Start of Quarter`, `Start of Year`, `Year` etc.

- **Combining Multiple Files**<br>
We had 3 files containing sales data from 2020, 2021 and 2022. Here, I kept all the sales data file in a separate folder, and then chose the option `Get Data` -> ` Folder` in Power BI `Home` tab. This will combine all the files in the folder. Here I have combined all the data into a single table `Sales Data`.

> [!Tip]
>
> Using `Folder` option to combine the file will also result in Power BI automatically picking up new files as and when they are added to the folder.

### Table View
- **Hierarchies**<br>
Creating hierarchies is way to organize data and help in drilling down from parent levels to lower levels in a specific order. For e.g., by creating a date hierarchy in the `Calendar Lookup` table using the columns (in following order), `Start of Year`, `Start of Month` & `Start of Week`, we can drill down or up in a visualization showing data at year, month or week level.

## Data Model
For the purpose of data modelling, I have classified the tables into two types:

- Dimension Tables
    - Calendar Lookup
    - Customer Lookup
    - Product Lookup
    - Product Categories Lookup
    - Product Subcategories Lookup
    - Territory Lookup

- Fact Tables
    - Return Data
    - Sales Data

Below is the final data model diagram after creating the relationships:

![Data Model](/assets/images/data_model.png)

> [!NOTE]
> Above data model follows the [**_Star Schema_**](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema) design for Power BI.

## DAX Measures
I have created various DAX measures to aid in visualizing KPIs metrics. Some of the are to calculate ```Total Revenue```, ```Total Profit```, ```Total Orders```, ```Total Customers```, ```Repeat Customer Rate```, ```Return Rate```, with measures to calculate the previous month values as well (for goal comparisons).

## Report
I have designed the report into 4 pages. Below are the key takeaways.

### **Executive Dashboard Page**

This is the main landing page for the report and displays a high level overview of common KPIs.

![Executive Dashboard](/assets/images/executive_dashboard.png)

#### Key takeaways

- ${\textsf{\color{cyan}Revenue}}$ is overall showing a ${\textsf{\color{cyan}healthy uptrend}}$, although there were few instances where the revenue dropped significantly from previous month e.g., in Jul 2020 & Jan 2022.

- Overall, ${\textsf{\color{cyan}Accessories}}$ is the ${\textsf{\color{cyan}most ordered category}}$ of product selling approx. ${\textsf{\color{cyan}17k}}$ items.

- ${\textsf{\color{cyan}Fender Set - Mountain}}$ is the ${\textsf{\color{cyan}highest revenue}}$ generating product.

- Among top revenue products, ${\textsf{\color{indianred}Sport Helmet}}$ is showing ${\textsf{\color{indianred}high return rate}}$ which is 50% more than average return rate, this warrants a ${\textsf{\color{indianred}quality review}}$.

- ${\textsf{\color{cyan}Current month revenue}}$ is 1.83 Million which is ${\textsf{\color{cyan}3.3 percent more}}$ than previous month, ${\textsf{\color{indianred}monthly orders}}$ are slightly ${\textsf{\color{indianred}down by 0.8 percent}}$ compared to previous month. ${\textsf{\color{cyan}Monthly returns}}$ have marginally ${\textsf{\color{cyan}improved by 1.78 percent}}$ over previous month.

- ${\textsf{\color{cyan}Tires and Tubes}}$ is the ${\textsf{\color{cyan}most ordered}}$ product and ${\textsf{\color{indianred}Shorts}}$ is the ${\textsf{\color{indianred}most returned}}$ product.

### **Order Map Page**

This page uses [Microsoft Azure maps](https://learn.microsoft.com/en-us/azure/azure-maps/power-bi-visual-get-started) to display location of the orders.

![Order Map](/assets/images/order_map.png)

#### Key takeaways

- The bubble size is in proportion to the number of orders.
- Thus with a quick glance we can observe:
    - **United States** has **highest** number of orders
    - **Germany** has the **lowest** number of orders
- User can also hover on the bubble to reveal the number of orders.