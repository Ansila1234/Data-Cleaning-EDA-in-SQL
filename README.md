# Data-Cleaning-EDA-in-SQL
## About the Project
In this project, I cleaned and prepared a dataset on worldwide layoffs for exploratory data analysis (EDA). The primary goal was to transform raw data into a clean, standardized format suitable for exploratory analysis. This process aimed to uncover useful insights and patterns from the layoffs data collected between 2020 and 2023.

## About the Dataset
The dataset contains 2,361 records of layoffs from 2020 to 2023 around the world for 1,628 distinct companies. The dataset has the following variables:

+ Company: The name of the company where layoffs occurred
+ Location: The location of the company (city/region)
+ Industry: The industry to which the company belongs
+ Total Laid Off: The total number of employees laid off
+ Percentage Laid Off: The percentage of the workforce laid off
+ Date: The date when the layoffs were announced
+ Stage: The stage of the company (e.g., startup, public)
+ Country: The country where the company is located
+ Funds Raised (Millions): The amount of funds raised by the company


# Data Cleaning Process
### 1. Create a Staging Table
I created a staging table to work on the data while keeping the original table intact. This approach preserves data integrity and allows for better tracking of changes.

### 2. Find and Remove Duplicates
To ensure data integrity, I identified and removed duplicate rows. This involved creating a column with row numbers partitioned by all columns and considering rows with row numbers greater than 1 as duplicates.

### 3. Standardize the Data
I standardized the data to ensure consistency and accuracy. This involved:

+ Trimming white spaces
+ Correcting inconsistencies in labeling
+ Removing punctuation after words
+ Changing the date format
+ Changing data types where necessary
### 4. Handle Null and Blank Values
Handling null and blank values is crucial for ensuring the completeness and reliability of the dataset. These missing values, if not properly managed, can lead to inaccurate analysis and misleading insights. In this project, I employed several strategies to address the null and blank values effectively.

### 4.1 Identifying Null and Blank Values
I began by identifying columns with null or blank values. Specifically, the Industry, Total Laid Off, Percentage Laid Off, and Funds Raised (Millions) columns were found to contain missing data.

For example:

+ Companies like Airbnb, Bally’s Interactive, Carvana, and Juul had null or blank values in the Industry column.
+ The Total Laid Off and Percentage Laid Off columns also had missing values that could potentially distort any analysis if left unaddressed.
### 4.2 Populating Missing Values in the Industry Column
Where possible, I populated the missing values based on available data within the dataset. For instance:

+ For companies like Airbnb, which belong to the Travel industry, I populated the null values in the Industry column with "Travel".
+ This method was applied to other companies where the industry could be confidently inferred from other records in the dataset.
### 4.3 Dealing with Missing Values in Numeric Columns
When handling missing values in numeric columns like Total Laid Off, Percentage Laid Off, and Funds Raised (Millions), I considered the following approaches:

### 1.Removing Rows with Irreparable Missing Data:

+ I deleted rows where both Total Laid Off and Percentage Laid Off were null, as they provided no usable data for analysis. This step resulted in the removal of 366 rows, reducing the dataset from 2,361 to 1,995 rows.
### 2.Replacing Null Values with Statistical Measures:

+ For the remaining null values, I employed industry-specific median values to replace them. This approach provided a more accurate and contextually relevant estimation compared to using a blanket statistic like the mean or mode.
+ The median was chosen over the mean to avoid skewing the data, particularly in industries where company sizes and the number of layoffs varied significantly.
For instance:

+ The Total Laid Off for companies in the healthcare industry is vastly different from those in the marketing industry. By using the median within each industry group, I minimized the risk of outliers distorting the analysis.
### 4.4 Addressing the Challenges in SQL
Due to limitations in MySQL, such as the lack of support for the PERCENTILE_CONT() function, I transferred the dataset to MS SQL Server to calculate the median values. Afterward, I updated the Total Laid Off, Percentage Laid Off, and Funds Raised (Millions) columns by replacing the null values with these calculated medians.

### 4.5 Final Adjustments and Verification
After replacing the null values, I verified the dataset to ensure that no null values remained in the key numeric columns. However, I found that companies like Bally's Interactive, which had only one record in the dataset, still had null values since a median could not be calculated for them.

Additionally, I addressed issues that arose after transferring the data to SQL Server:

+ The data type for the Date column was changed from text to datetime, requiring conversion back to the original format.
+ String representations of null values (i.e., "null") were converted back to actual NULL values in nominal columns such as Industry, Stage, and Location.
Finally, I confirmed that approximately 99.54% of the null values were successfully handled, leaving the dataset in a much cleaner state for subsequent analysis.

### 4.6 Considerations for Future Analysis
While the data cleaning process significantly improved the quality of the dataset, it's important to note that no cleaning process is entirely foolproof. During exploratory data analysis (EDA), further issues may be uncovered that require additional cleaning or adjustments.

### 5. Remove Unnecessary Columns and Rows
I dropped columns and rows that were irrelevant or unnecessary for future analysis. This included rows where both total_laid_off and percentage_laid_off were null.

## Final Table After Cleaning
The final table is now structured and ready for analysis. It is free of duplicates, standardized, and has null values appropriately handled. This clean dataset will support accurate and meaningful insights during exploratory data analysis.

## Conclusion
Data cleaning is an essential step in data analysis, ensuring that data is accurate, consistent, and suitable for analysis. Through a systematic approach, I transformed a raw dataset into a clean and structured format, making it ready for exploratory data analysis to uncover insights into recent layoffs. This project underscores the critical role of meticulous data cleaning in obtaining reliable insights from data.

# EDA Process
I created the layoffs_staging_new dataset during my data cleaning project as the foundation for this exploratory analysis. The dataset includes 1,995 records of layoffs from 2020 to 2023, covering 1,628 distinct companies worldwide. It provides a comprehensive overview of job market disruptions during this period, reflecting significant variability across companies, industries, and geographies.

Problem Statements
The primary goal of my project was to explore how different companies and industries across the globe have experienced layoffs during this period. I guided my analysis with the following research questions:

+ How do layoff statistics vary across different companies?
+ What are the yearly trends in layoffs from 2020 to 2023?
+ Which industries experienced the highest number of layoffs?
+ How does the geographical distribution of layoffs vary across different countries?
+ How do layoffs differ across various stages of company maturity?
## Key Findings
### Companies with the Highest Number of Layoffs in a Single Event
Google had the highest number of layoffs in a single event, affecting 12,000 employees, though this accounted for only 6% of its workforce. Meta and Amazon also saw significant layoffs, with 11,000 and 10,000 employees affected, representing 13% and 17% of their respective workforces.

### Yearly Trends in Layoffs from 2020 to 2023
I observed significant fluctuations in layoffs from 2020 to 2023. In 2020, the pandemic caused 532 layoff events, affecting 86,419 employees. Layoffs sharply declined in 2021 but surged again in 2022, with 1,030 events affecting 178,486 employees. Early 2023 trends suggest ongoing economic instability.

### Industries with the Most Layoffs
The Retail and Consumer sectors faced the most layoffs, each with over 46,000 employees affected. Other significantly impacted industries include Transportation, Finance, Healthcare, and Food.

### Countries Experiencing the Highest Number of Layoffs
The United States experienced the highest number of layoffs, with 278,471 employees affected. India followed with 37,198 layoffs, while other countries like the Netherlands, Sweden, and Brazil also saw substantial layoffs.

### Top U.S. Cities with the Highest Number of Layoffs
The San Francisco Bay Area had the highest number of layoffs in the U.S., affecting 133,692 employees, followed by Seattle and New York City with 35,881 and 32,834 layoffs, respectively.

### Companies with the Highest Total Number of Layoffs
Amazon had the highest total number of layoffs with 18,150 employees affected, followed by Google and Meta. Other companies like Salesforce, Microsoft, Philips, and Ericsson also experienced significant layoffs.

### Correlation Between Funds Raised and Layoff Counts
The correlation coefficient of 0.07 indicates a very weak positive relationship between the amount of funds raised and the number of layoffs, suggesting little to no linear relationship between these variables.

### Distribution of Layoffs Across Different Stages of Companies
Post-IPO companies experienced the highest total number of layoffs, though the average percentage was moderate. Seed stage companies showed the highest average percentage of layoffs, indicating vulnerability to workforce reductions.

# Conclusion
Through this analysis, I discovered that major tech companies faced substantial layoffs, there were significant fluctuations in layoff trends over the years, and various industries and countries were heavily impacted. These patterns are crucial for job seekers navigating a tough job market and for organizations planning their workforce strategies.

