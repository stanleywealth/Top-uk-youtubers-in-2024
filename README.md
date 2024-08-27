# Analyzing The Top YouTubers In UK 2024


## Project Overview

- The pain point

TheHead of marketing wants to find out who are the top YouTubers in the United Kingdon, as this will help the marketing team make informed decisions on which YouTuber will be best to run marketing campaign and partnerships with throughout the year.

- The ideal solution

To create a dashboard that provide insights into the top YouTubers in the United Kingdom. This dashboard should include:
1. Subscriber count
2. Total views
3. Total videos
4. Views engagement rate

These key metrics will help decide on which YouTuber to collaborate with to run marketing campaigns.


## User Story

As the Head of Marketing, I want to use a dashboard that analyses YouTube channel data in the United Kingdom.
This dashboard should allow me to identify the top performing channels based on metrics like subscriber base and average views.
With this information, I can make more informed decisions about which Youtubers are best to collaborate with, and therefore maximize how effective each marketing campaign is.


## Data Source

We need data of the top 100 YouTubers in the United Kingdom in 2024, that includes:

1. The channel names
2. Number of videos uploaded
3. Total subscribers
4. Total views

This data is sourced from Kaggle [Get it here!](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download)


## Tools


| Tools | Purpose |
| ----- | ------- |
| Excel | Exploring the data and testing |
| Sql Server | Cleaning, testing, and analyzing the data |
| Power BI | Visualing the data through interactive dashboard |
| GitHub | Hosting projects documentation and version control |


## Stages for Analysis

- Design
- Development
- Testing
- Visualization
- Analysis


## Design


### Dashboard Component Requirements

- First we firgured out what the dashboard should contain based on the requirements provided?
To understand what it should contain, we need to figure out what questions we need the dashboard to answer:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

For now, these are some of the questions we need to answer, this may change as we progress down our analysis.


## Development


### Pseodocode 

- What's the general approach in creating this solution from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into SQL Server
4. Clean the data with SQL
5. Test the data with SQL
5. Visualize the data in Power BI
6. Generate the findings based on the insights
7. Write the documentation + commentary
8. Publish the data to GitHub Pages


### Data Exploration Note 

This is the stage where you have a scan of what's in the data, errors, inconcsistencies, bugs, weird and corrupted characters etc

- What are your initial observations with this dataset? What's caught your attention so far?

1. There are at least 4 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data.

2. The first column contains the channel ID with what appears to be channel IDs, which are separated by a @ symbol - we need to extract the channel names from this.

3. Some of the cells and header names are in a different language - we need to confirm if these columns are needed, and if so, we need to address them.

4. We have more data than we need, so some of these columns would need to be removed.


### Data Cleaning 

1. What do we expect the clean data to look like? (What should it contain? What contraints should we apply to it?)
The aim is to refine our dataset to ensure it is structured and ready for analysis.

The cleaned data should meet the following criteria and constraints:

1. Only relevant columns should be retained.
2. All data types should be appropriate for the contents of each column.
3. No column should contain null values, indicating complete data for all records.

Below is a table outlining the constraints on our cleaned dataset:


| Property | Constraint |
| -------- | ---------- |
| Number of rows | 100 |
| Number of column | 4 |


And here is a tabular representation of the expected schema for the clean data:


| Column Name | Data Type | Nullable |
| ----------- | --------- | -------- |
| channel_name| VARCHAR | NO |
| total_subscribers | INT | NO |
| total_views | INT | NO |
| total_videos | INT | NO |


 What steps are needed to clean and shape the data into the desired format?
1. Remove unnecessary columns by only selecting the ones you need
2. Extract Youtube channel names from the first column
3. Rename columns using aliases


## Testing

What data quality and validation checks are we going to create?

Here are the SQL query data quality tests i conducted:

1. Row count check

```
/*
# Count the total number of records (or rows) are in the SQL view
*/

SELECT
    COUNT(*) AS no_of_rows
FROM
    view_uk_youtubers_2024;
```

2. Column count check

```
/*
# Count the total number of columns (or fields) are in the SQL view
*/

SELECT
    COUNT(*) AS column_count
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024'
```

3. Data type check

```
/*
# Check the data types of each column from the view by checking the INFORMATION SCHEMA view
*/

-- 1.
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';
```

4. Duplicate count check

```
/*
# 1. Check for duplicate rows in the view
# 2. Group by the channel name
# 3. Filter for groups with more than one row
*/

-- 1.
SELECT
    channel_name,
    COUNT(*) AS duplicate_count
FROM
    view_uk_youtubers_2024

-- 2.
GROUP BY
    channel_name

-- 3.
HAVING
    COUNT(*) > 1;
```


## Visualization

What the dashboard look like?

![Top UK YouTubers in 2024](assets/images/Top UK Youtubers 2024 Dashboard Image.png)


### The Dax measures used to develop the dashboard

1. Total Subscribers (M):

```
Total Subscribers (M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(sumOfSubscribers,million)

RETURN totalSubscribers
```

2. Total Views (B):

```
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = ROUND(sumOfTotalViews / billion, 2)

RETURN totalViews
```

3. Total Videos:

```
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

RETURN totalVideos
```

4. Average Views Per Video:

```
Average Views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR  avgViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())

RETURN finalAvgViewsPerVideo 
```

5. Subscribers Engagement Rate:

```
Subscriber Engagement Rate = 
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())

RETURN subscriberEngRate 
```

6. Views Per Subscriber:

```
Views Per Subscriber = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews, sumOfTotalSubscribers, BLANK())

RETURN viewsPerSubscriber 
```


## Analysis 

### Findings

For this analysis we focused on these questions written below in other to get the information we need for our marketing client

Here are the key questions we need to answer for our marketing client:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?
-


1. Who are the top 10 YouTubers with the most subscribers?

| Rank | Channel Name | Subscribers (M) |
| ---- | ------------ | ----------- |
| 1 | NoCopyrightSounds | 33.60 |
| 2 | DanTDM | 28.60 |
| 3 | Dan Rhodes | 26.50 |
| 4 | Miss Katy | 24.50 |
| 5 | Mister Max | 24.40 |
| 6 | KSI | 24.10 |
| 7 | Jelly | 23.50 |
| 8 | Dua Lipa | 23.30 |
| 9 | Sidemen | 21.00 |
| 10 | Ali-A | 18.90 |


2. Which 3 channels have uploaded the most videos?

| Rank | Chennel Name | Video Upload |
| ---- | ------------ | ------------ |
| 1 | GRM Daily | 14,696 |
| 2 | Manchester City | 8,248 |
| 3 | Yogscast | 6,435 |


3. Which 3 channels have the most views?

| Rank | Channel Name | Total Views (B) |
| ---- | ------------ | --------------- |
| 1 | DanTDM | 19.78 |
| 2 | Dan Rhodes | 18.56 |
| 3 | Mister Max | 15.97 |


4. Which 3 channels have the highest average views per video?

| Rank | Channel Name | Avg Views Per Video (M) |
| ---- | ------------ | ------------------- |
| 1 | Mark Ronson | 32.27 |
| 2 | Jessie J | 5.97 |
| 3 | Dua Lipa | 5.76 |


5. Which 3 channels have the highest views per subscriber ratio?

| Rank | Channel Name | Views Per Subscriber |
| ---- | ------------ | -------------------- |
| 1 | GRM Daily | 1185.79 |
| 2 | Nickelodeon | 1061.04 |
| 3 | Disney Junior UK | 1031.97 |


6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

| Rank | Channel Name | Subscriber Engagemnt Rate |
| ---- | ------------ | ------------------------- |
| 1 | Mark Ronson | 343,000 |
| 2 | Jessie J | 110,416.67 |
| 3 | Dua Lipa | 104,954.95 |

#### Notes
For this analysis, we'll prioritize analysing the metrics that are important in generating the expected ROI for our marketing client, which are the YouTube channels with the most:

- subscribers
- total views
- videos uploaded

### Vlidation 











