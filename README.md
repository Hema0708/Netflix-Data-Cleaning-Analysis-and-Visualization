Netflix Data: Cleaning, Analysis and Visualization


Project Overview 
The main goal of this project is to explore and analyze the Netflix dataset by performing data cleaning, preprocessing, and visualization. This helps uncover insights about content distribution, trends over time, and user consumption patterns on the platform.
Tools Used 
Programming Languages : Python 
Database: SQL 
Spreadsheet Software: Excel 

About Dataset
Netflix is a popular streaming service that offers a vast catalog of movies, TV shows, and original contents. This dataset is a cleaned version of the original version which can be found here. The data consist of contents added to Netflix from 2008 to 2021. The oldest content is as old as 1925 and the newest as 2021. This dataset will be cleaned with PostgreSQL and visualized with Tableau. The purpose of this dataset is to test my data cleaning and visualization skills. The cleaned data can be found below and the Tableau dashboard can be found here . 
Data Cleaning
We are going to:
1.	Treat the Nulls
2.	Treat the duplicates
3.	Populate missing rows
4.	Drop unneeded columns
5.	Split columns
Extra steps and more explanation on the process will be explained through the code comments                                                                                                                

Example: You can get the basic idea how you can create a project from here 

Netflix Data: Cleaning, Analysis, and Visualization (Beginner ML Project)

This project involves loading, cleaning, analyzing, and visualizing data from a Netflix dataset. We'll use Python libraries like Pandas, Matplotlib, and Seaborn to work through the project. The goal is to explore the dataset, derive insights, and prepare for potential machine learning tasks. 
Step 1: Import Required Libraries
import pandas as pd import numpy as np import matplotlib.pyplot as plt import seaborn as sns from wordcloud import WordCloud    
Step 2: Load the Dataset
Assume we have a dataset named netflix_titles.csv.
# Load the dataset data = pd.read_csv('netflix_titles.csv')
# Display the first few rows of the dataset print(data.head())
Step 3: Data Cleaning 
Identify and handle missing data, correct data types, and drop duplicates.
# Check for missing values print(data.isnull().sum())
# Drop duplicates if any data.drop_duplicates(inplace=True)
# Drop rows with missing critical information data.dropna(subset=['director',	'cast',	'country'],
inplace=True)
# Convert 'date_added' to datetime data['date_added'] = pd.to_datetime(data['date_added'])                    # Show data types to confirm changes print(data.dtypes) 
Step 4: Exploratory Data Analysis (EDA) 
1.  Content Type Distribution (Movies vs. TV Shows) # Count the number of Movies and TV Shows type_counts = data['type'].value_counts()
# Plot the distribution plt.figure(figsize=(8, 6)) sns.barplot(x=type_counts.index,	y=type_counts.values, palette='Set2') plt.title('Distribution of Content by Type') plt.xlabel('Type') plt.ylabel('Count') plt.show()
1.	Most Common Genres
# Split the 'listed_in' column and count genres data['genres'] = data['listed_in'].apply(lambda x: x.split(',
')) all_genres = sum(data['genres'], []) genre_counts = pd.Series(all_genres).value_counts().head(10)
# Plot the most common genres plt.figure(figsize=(10, 6)) sns.barplot(x=genre_counts.values,	y=genre_counts.index, palette='Set3') plt.title('Most Common Genres on Netflix') plt.xlabel('Count') plt.ylabel('Genre') plt.show()
3.	Content Added Over Time
# Extract year and month from 'date_added' data['year_added'] = data['date_added'].dt.year data['month_added'] = data['date_added'].dt.month
# Plot content added over the years plt.figure(figsize=(12, 6)) sns.countplot(x='year_added', data=data, palette='coolwarm') plt.title('Content Added Over Time') plt.xlabel('Year') plt.ylabel('Count') plt.xticks(rotation=45) plt.show()
4.	Top 10 Directors with the Most Titles
# Count titles by director top_directors = data['director'].value_counts().head(10)

# Plot top directors plt.figure(figsize=(10, 6)) sns.barplot(x=top_directors.values,	y=top_directors.index, palette='Blues_d') plt.title('Top 10 Directors with the Most Titles') plt.xlabel('Number of Titles') plt.ylabel('Director') plt.show()

5. Word Cloud of Movie Titles # Generate word cloud movie_titles = data[data['type'] == 'Movie']['title']
	wordcloud	=	WordCloud(width=800,	height=400,
background_color='black').generate(' '.join(movie_titles))
# Plot word cloud plt.figure(figsize=(10, 6)) plt.imshow(wordcloud, interpolation='bilinear') plt.axis('off')
 plt.show()                                                      

Step 5: Conclusion and Insights  
                                                                                                                               In this project, we:
1.	Cleaned the data by handling missing values, removing duplicates, and converting data types.
2.	Explored the data through various visualizations such as bar plots and word clouds.
3.	Analyzed content trends over time, identified popular genres, and highlighted top directors.

Step 6: Next Steps
1.	Feature Engineering: Create new features, such as counting the number of genres per movie or extracting the duration in minutes.
2.	Machine Learning: Use the cleaned and processed data to build models for recommendations or trend predictions.
3.	Advanced Visualization: Use interactive plots or dashboards for more detailed analysis.
This project is a foundational exercise that introduces essential data analysis techniques, paving the way for more advanced projects.

Sample code:   

# Netflix-Data-Cleaning-Analysis-and-Visualization
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
data=pd.read_csv("netflix1.csv")
data.head()
data.info()
data.shape
data=data.drop_duplicates()
data['type'].value_counts()
freq=data['type'].value_counts()
fig, axes=plt.subplots(1,2, figsize=(8, 4))
sns.countplot(data, x=data['type'], ax=axes[0])
plt.pie(freq, labels=['Movie', 'TV Show'], autopct='%.0f%%')
plt.suptitle('Total Content on Netflix', fontsize=20)
data.info()
data['rating'].value_counts()
ratings=data['rating'].value_counts().reset_index().sort_values
(by='count', ascending=False)
plt.bar(ratings['rating'], ratings['count'])
plt.xticks(rotation=45, ha='right')
plt.xlabel("Rating Types")
plt.ylabel("Rating Frequency")
plt.suptitle('Rating on Netflix', fontsize=20)
plt.pie(ratings['count'][:8], labels=ratings['rating'][:8],
autopct='%.0f%%')
plt.suptitle('Rating on Netflix', fontsize=20)
# lets convert column date_added to datetime.
data['date_added']=pd.to_datetime(data['date_added'])
data.describe()
data['country'].value_counts()
top_ten_countries=data['country'].value_counts().reset_index().
sort_values(by='count', ascending=False)[:10]
plt.figure(figsize=(10, 6))
plt.bar(top_ten_countries['country'],
top_ten_countries['count'])
plt.xticks(rotation=45, ha='right')
plt.xlabel("Country")
plt.ylabel("Frequency")
plt.suptitle("Top 10 countries with most content on Netflix")
plt.show()
data['year']=data['date_added'].dt.year
data['month']=data['date_added'].dt.month
data['day']=data['date_added'].dt.day
monthly_movie_release=data[data['type']=='Movie']['month'].valu
e_counts().sort_index()
monthly_series_release=data[data['type']=='TV Show']['month'].value_counts().sort_index()
plt.plot(monthly_movie_release.index,
monthly_movie_release.values, label='Movies')
plt.plot(monthly_series_release.index,
monthly_series_release.values, label='Series')
plt.xlabel("Months")
plt.ylabel("Frequency of releases")
plt.xticks(range(1, 13), ['Jan', 'Feb', 'Mar', 'Apr', 'May',
'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.legend()
plt.grid(True)
plt.suptitle("Monthly releases of Movies and TV shows on Netflix")
plt.show()
yearly_movie_releases=data[data['type']=='Movie']['year'].value
_counts().sort_index()
yearly_series_releases=data[data['type']=='TV
Show']['year'].value_counts().sort_index()
plt.plot(yearly_movie_releases.index,
yearly_movie_releases.values, label='Movies')
plt.plot(yearly_series_releases.index,
yearly_series_releases.values, label='TV Shows')
plt.xlabel("Years")
plt.ylabel("Frequency of releases")
plt.grid(True)
plt.suptitle("Yearly releases of Movies and TV Shows on Netflix")
plt.legend()
popular_movie_genre=data[data['type']=='Movie'].groupby("listed
_in").size().sort_values(ascending=False)[:10]
popular_series_genre=data[data['type']=='TV Show'].groupby("listed_in").size().sort_values(ascending=False)
[:10]

plt.bar(popular_movie_genre.index, popular_movie_genre.values)
plt.xticks(rotation=45, ha='right')
plt.xlabel("Genres")
plt.ylabel("Movies Frequency")
plt.suptitle("Top 10 popular genres for movies on Netflix")
plt.show()

plt.bar(popuiar_seres_genre.index,
popular_series_genre.values)
plt.xtricks(rotation=45, ha='right')
plt.xlabel("Genres")
plt.ylabel("TV Shows Frequency")
plt.suptitle("Top 10 popular genres for TV Shows on Netflix")
plt.show()
directors=data['director'].value_counts().reset_index().sort_values(by='count', ascending=False)[1:15]
plt.bar(directors['director'], directors['count'])
plt.xticks(rotation=45, ha='right')



