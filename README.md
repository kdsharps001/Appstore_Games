# Appstore_Games

## Title of Project
**App Store Games Between 2008 and 2019**

### Project Overview

The analysis of mobile app store games from 2008 to 2019 was conducted to understand the trends, performance, and characteristics of the games available on the Apple App Store. The dataset comprised over 17,000 apps developed by more than 8,000 developers, providing a comprehensive view of the mobile gaming landscape over a decade. The analysis aimed to uncover insights into app pricing, user ratings, language preferences, developer performance, and genre popularity.

### Data Sources
[FP20 Analytics]("https://zoomchartswebstorage.blob.core.windows.net/contest/Mobile_Games_datset_FP20C18.zip")

### Tools Used
- Power BI
- SQL

### Data Cleaning

Data cleaning was an essential step to ensure the accuracy and reliability of the analysis. The following actions were taken:
- **Removal of Duplicates:** Duplicate records were identified and removed to prevent skewed results.
- **Handling Missing Values:** Missing data for critical columns such as `Average User Rating`, `User Rating Count`, and `Price per App (USD)` were addressed by using the average values.
- **Data Type Conversion:** Columns like `Release Date`, `Price per App (USD)`, and `User Rating Count` were converted to appropriate data types for accurate analysis.
- **Filtering Irrelevant Data:** Apps with incomplete information or irrelevant categories were filtered out to focus on meaningful insights.

### Power BI Analysis
The following type of analysis was carried out using Power BI to gain insights into the dataset by creating the appropriate visuals:
- **Descriptive Analysis:** Basic statistics such as total number of apps, developers, and average user ratings were calculated.
- **Trend Analysis:** Price trends over the years were examined to understand pricing dynamics.
- **Comparative Analysis:** Comparisons between different developers, genres, and languages were made to identify top performers.
- **Correlation Analysis:** Relationships between app size, price, and genres were explored to identify patterns.
- **Ranking and Top-N Analysis:** Top games, developers, and genres were identified based on various metrics like user ratings and revenue.

### SQL Analysis
Data was imported into Microsoft SQL Management Studio and each question was answered using the appropriate query

```SQL

--INSPECT THE DATA
SELECT * FROM appstore_games;


--. What are the 5 most popular games from 2008 to 2019 yearly?
WITH RankRating AS (
	SELECT 
		YEAR(Release_Date) AS Release_Year,
		Name,
		User_Rating_Count,
		ROW_NUMBER() OVER (PARTITION BY YEAR(Release_Date) ORDER BY User_Rating_count DESC) AS Rank
	FROM appstore_games
	)
SELECT Release_Year,
	Name,
	User_Rating_Count
FROM RankRating
WHERE Rank <= 5;


--What game developer had the highest average game rating?
SELECT Developer, [Average User_Rating]
FROM appstore_games
WHERE [Average User_Rating] = 5;


--Who are the 3 game developers to have the most income from their app store games from 2008 to 2019?
SELECT TOP 3 Developer, SUM([Price_per_App(USD)]) AS Total_Income
FROM appstore_games
GROUP BY Developer
ORDER BY Total_Income DESC;


--What is the average rating and price trend for the games from 2008 to 2019?
SELECT YEAR(Release_Date),
ROUND(AVG([Average User_Rating]),2) AS Avg_Rating, 
ROUND(AVG([Price_per_App(USD)]),2) AS [Avg_Price(USD)]
FROM appstore_games
GROUP BY YEAR(Release_Date)
ORDER BY YEAR(Release_Date);

--Which is the most popular language for the games in the App Store from 2008 to 2019?
SELECT [Language Name], COUNT([Language Name]) AS Counts
FROM language
GROUP BY [Language Name]
ORDER BY Counts DESC;


--What are the 3 games to have the most user rating count from 2016 to 2019?
SELECT TOP 3 Name, User_Rating_Count
FROM appstore_games
WHERE YEAR(Release_Date) IN (2016,2017,2018,2019)
GROUP BY Name, User_Rating_Count
ORDER BY User_Rating_Count DESC;


--What are the most popular game genres every 3 years since 2008?
WITH Popular_genres AS (
	SELECT Primary_Genre, 
		COUNT(Primary_Genre) AS Counts, 
		YEAR(Release_Date) AS Year_of_release, 
		ROW_NUMBER() OVER (
		PARTITION BY YEAR(Release_Date) 
		ORDER BY COUNT(Primary_Genre) DESC) AS Rate
	FROM appstore_games
	GROUP BY Primary_Genre, YEAR(Release_Date))
SELECT Primary_Genre,
	Year_of_release,
	Counts
FROM Popular_genres
WHERE Year_of_release IN (2008,2011,2014,2017) AND Rate = 1
GROUP BY Primary_Genre, Year_of_release, Counts;


--For the new developers to start the App Store, what is the price recommendation for them?
SELECT ROUND(AVG([Price_per_App(USD)]),2) AS [Avg_Price(USD)] 
FROM appstore_games;


--For the new developers to start the App Store, what is the genre recommendation for them?
SELECT TOP 1 Primary_Genre 
FROM appstore_games
GROUP BY Primary_Genre
ORDER BY COUNT(Primary_genre) DESC;


--Can we find any correlation between genre, game size and price?
SELECT Primary_Genre,
Size_in_Bytes AS Size,
AVG([Price_per_App(USD)])
FROM appstore_games
GROUP BY Primary_Genre
ORDER BY AVG([Price_per_App(USD)]) DESC;

```

### Results

The analysis revealed several key findings:
- **Developer Performance:** Andrew Kudrin was the top-earning developer with over $5,000 in revenue.
- **Price Trends:** App prices saw a gradual increase over the years, with a significant spike in 2016, followed by stabilization.
- **User Engagement:** Clash of Clans (2012) and Clash Royale (2016) had the highest number of user ratings.
- **Language Preferences:** English was the most common language, followed by Chinese, German, and French.
- **Genre Popularity:** The Games genre was the most popular with an average app size of 110.35MB. Strategy, entertainment, and puzzle genres were also significant.
- **Developer Activity:** Tapps Tecnologia was the most prolific developer with over 100 games.
- **Age Rating:** The most common age rating for apps was 4+.
- **Revenue:** The total revenue generated by the apps was over $13,000, with significant user engagement reflected in over 56 million ratings.
- **User Ratings:** The highest user rating count for a game was over 3 million, and the highest rated app had a perfect rating of 5.0.

### Recommendations

Based on the analysis, the following recommendations are made for new developers entering the App Store:
- **Price Strategy:** Consider competitive pricing strategies, especially noting the price spike in 2016. Starting with lower prices and gradually increasing based on user engagement and app performance could be beneficial.
- **Genre Focus:** Given the popularity of the Games and Strategy genres, new developers should consider creating high-quality apps in these categories to attract users.
- **Language Support:** Providing multi-language support, especially for English, Chinese, German, and French, can help reach a broader audience.
- **User Engagement:** Focus on user experience and engagement strategies to drive higher ratings and reviews, similar to successful apps like Clash of Clans and Clash Royale.
- **App Size Management:** While larger app sizes are correlated with higher prices, optimizing app size without compromising quality can enhance user satisfaction and retention.
- **Target Audience:** Pay attention to age ratings, with a significant focus on the 4+ category, to align with the most common user demographic.

\

