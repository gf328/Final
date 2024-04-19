# Blog  - Spotify Streams: What are we listening to?
## Contents Table
##### Description 
##### Why Analyse Streams?
##### Data
##### Technologies
##### How to use the Code
##### Insights from Analysis
##### Web Scraping Script

## Description:
Spotify has transformed how consumers listen to music over the past twenty years. This blog aims to look at Spotify's 'most streamed' and 'number one' songs, using data scraped from Wikipedia to provide analysis into what Spotify users are chosing to listen to. By examining this data, I aim to understand trends and preferences in the musical landscape.

## Why Analyse Streams?
Streams play an important role in the music industry. They directly affect artists' income as Spotify pays them based on the number of streams their songs receive. Moreover, the popularity of songs feeds into a never ending circle - the higher number of streams a song receives, the more visible it is on the platform, leading to more streams.

## Data
I analysed 2 primary datasets:
* Most Streamed Songs - this dataset contains information on songs with the highest number of streams on Spotify.
* Number One Songs - this dataset contains information on songs that spent at least a week at number one on Spotify's charts.

## Technologies 
* Python Jupyter Notebook Version 6.4.12
* pandas Version 1.4.4
* numpy Version 1.26.2
* ipython Version 7.31.1
* matplotlib Version 3.5.2
* plotly Version 5.18.0
* wordcloud Version 1.9.3
* sklearn Version
* beautifulsoup4 Version 4.11.1
* urllib3 Version 1.26.11

### How to use the Code
* Web Scraping - Use BeautifulSoup to scrape the data from the Wikipedia page ['List of Spotify streaming records'] (https://en.wikipedia.org/wiki/List_of_Spotify_streaming_records). Script for scraping provided in Code section below.
* Import Data - Load the two csv datasets ('Number-one songs.csv' and 'Most-streamed songs.csv')
* Data Preperation - I removed the last column of 'Most_streamed' as it only contained references. Check for missing data and duplicates. Merge datasets using an inner join for later analysis.
* Data Analysis - Used libraries listed under Technologies section to create visualisations and interpretations.

## Insights from Analysis
* Pop music is the most popular genre across both datasets
* Ariana Grande has the most 'Number One' songs in the list.
* The Weekend has the most songs in the 'Most Streamed' list.
* September, April and October see the release of the most successful songs.

### Web Scraping Script
from urllib import request, error
from bs4 import BeautifulSoup

site = 'https://en.wikipedia.org/wiki/List_of_Spotify_streaming_records'

response = BeautifulSoup(request.urlopen(site), 'html.parser')

#Find all tables on the page
tables = response.find_all('table')

#Find the table with the header "Most-streamed songs"
target_table = None
for table in tables:
    headers = table.find_all('th')
    for header in headers:
        if header.text.strip() == "Rank":
            target_table = table
            break
    if target_table:
        break

if target_table:
    rows = target_table.find_all('tr')
    
    with open('Most-streamed songs.csv', 'w') as file:
        file.write('Rank,Song,Artist,Streams(billions),ReleaseDate,Ref\n')
        for row in rows[1:]:  # Exclude header row
            cells = row.find_all(['th', 'td'])
            row_contents = [cell.get_text(strip=True) for cell in cells]
            file.write(','.join(row_contents) + '\n')
    print("Table scraped successfully!")
else:
    print("Table not found.")


site = 'https://en.wikipedia.org/wiki/List_of_Spotify_streaming_records'

response = BeautifulSoup(request.urlopen(site), 'html.parser')

#Find all tables on the page
tables = response.find_all('table')

#Look for the table containing the header "Wks"
target_table = None
for table in tables:
    header_row = table.find('tr')
    if header_row and "Wks" in header_row.text:
        target_table = table
        break

if target_table:
    rows = target_table.find_all('tr')

    with open('List of number-one songs on Spotify.csv', 'w') as file:
        file.write('Song,Artist,Release Date,Issue Date,Weeks,Avs.\n')
        for row in rows[1:]:  # Exclude header row
            cells = row.find_all(['th', 'td'])
            row_contents = [cell.get_text(strip=True) for cell in cells]
            file.write(','.join(row_contents) + '\n')
    print("Table scraped successfully!")
else:
    print("Table not found.")

