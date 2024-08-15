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
> Using `Folder` option to combine the file will also result in Power BI automatically picking up new files as and when they are added to the folder.
