# Investigating Netflix Movie Length
This is one of my first Python projects! This project focuses on answering the question of whether or not Netflix movies have been getting shorter over time. You can find the CSV file to the dataset used in this project in this repository.

## Table of Contents
1. [Investigating and Preparing the Dataset](#investigating-the-dataset)
2. [Filtering on Short Movies](#filtering-on-short-movies)
3. [Assigning Colors for Plot](#assigning-colors-for-plot)
4. [Creating and Inspecting the Plot](#creating-and-inspecting-the-plot)
5. [Conclusion](#conclusion)

## Investigating and Preparing the Dataset
To start off the analysis, I import the packages needed for my analysis including pandas and matplotlib's pyplot. I upload the CSV file titled "netflix_data.csv" as a dataframe titled "netflix_df" and get familiar with the data by looking at the column headers and first five rows of the data.
```
import pandas as pd
import matplotlib.pyplot as plt

netflix_df = pd.read_csv("netflix_data.csv")
print(netflix_df.head(5))
```
Since Netflix houses TV shows as well, as reflected in the "type" column, I filter the data to only look at observations where the type is "Movie" and save it as a new dataframe "netflix_subset". Additionally, for my own sake, I further filter the to only return the title, country, genre, release year, and duration columns and save it as a new dataframe titled "netflix_movies".
```
netflix_subset = netflix_df[netflix_df["type"] == "Movie"]

netflix_movies = netflix_subset.loc[:, ["title", "country", "genre", "release_year", "duration"]]
```
## Filtering on Short Movies
Since the main focus of this project is to look at whether or not Netflix movies have been getting shorter over time, I filter the data on movies whose duration is less than 60 minutes and print the first 20 rows to verify.
```
short_movies = netflix_movies[netflix_movies["duration"] < 60]
print(short_movies.head(20))
```

## Assigning Colors for Plot
Using a for loop and if/elif statements, I iterate through the rows of the "netflix_movies" dataframe to assign colors to four of the major genre groups: children, documentaries, stand-up, and other for everything else. I save the colors in a list to be later referenced when I create the plot.
```
colors = []
for lab, row in netflix_movies.iterrows():
    if row["genre"] == "Children":
        colors.append("red")
    elif row["genre"] == "Documentaries":
        colors.append("blue")
    elif row["genre"] == "Stand-Up":
        colors.append("yellow")
    else:
        colors.append("green")
```
As seen above, I assign the color red to children, blue to documentaries, yellow to stand-up, and green to everything else.

## Creating and Inspecting the Plot
Finally, I initialize a matplotlib figure titled "fig" with a width of 12 and a height of 8 to create a scatter plot with release_year on the x-axis and duration on the y-axis. I also use the colors list created before.
```
fig = plt.figure(figsize=(12, 8))
plt.scatter(netflix_movies["release_year"], netflix_movies["duration"], c=colors)
plt.xlabel("Release year")
plt.ylabel("Duration (min)")
plt.title("Movie Duration by Year of Release")
plt.show()
```

## Conclusion
Based on the scatterplot, while genres such as children's movies and documentaries are clustered around the bottom half of the plot, there is no clear clear indication of a change in duration. Thus, we can conclude that no, movies are not getting shorter over time.
