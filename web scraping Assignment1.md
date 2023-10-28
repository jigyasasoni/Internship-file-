# Web Scraping Assignment 1


```python
import requests

def fetch_data_from_url(url):
    response = requests.get(url)
    
    if response.status_code == 200:
        # Process the data or perform other actions with the response
        print("Data from URL:", url)
        print(response.text)
    else:
        print("Failed to retrieve data from URL. Status code:", response.status_code)

url_to_scrape = "https://example.com"
fetch_data_from_url(url_to_scrape)

```

    Data from URL: https://example.com
    <!doctype html>
    <html>
    <head>
        <title>Example Domain</title>
    
        <meta charset="utf-8" />
        <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <style type="text/css">
        body {
            background-color: #f0f0f2;
            margin: 0;
            padding: 0;
            font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
            
        }
        div {
            width: 600px;
            margin: 5em auto;
            padding: 2em;
            background-color: #fdfdff;
            border-radius: 0.5em;
            box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
        }
        a:link, a:visited {
            color: #38488f;
            text-decoration: none;
        }
        @media (max-width: 700px) {
            div {
                margin: 0 auto;
                width: auto;
            }
        }
        </style>    
    </head>
    
    <body>
    <div>
        <h1>Example Domain</h1>
        <p>This domain is for use in illustrative examples in documents. You may use this
        domain in literature without prior coordination or asking for permission.</p>
        <p><a href="https://www.iana.org/domains/example">More information...</a></p>
    </div>
    </body>
    </html>
    
    


```python
# 1) Write a python program to display all the header tags from wikipedia.org and make data frame.
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://en.wikipedia.org/wiki/Main_Page"
response = requests.get(url)

soup = BeautifulSoup(response.content, "html.parser")

header_tags = soup.find_all(["h1", "h2", "h3", "h4", "h5", "h6"])
header_texts = [tag.get_text() for tag in header_tags]

df = pd.DataFrame(header_texts, columns=["Header"])
print(df)

```

                               Header
    0                       Main Page
    1            Welcome to Wikipedia
    2   From today's featured article
    3                Did you know ...
    4                     In the news
    5                     On this day
    6      From today's featured list
    7        Today's featured picture
    8        Other areas of Wikipedia
    9     Wikipedia's sister projects
    10            Wikipedia languages
    


```python
# Write s python program to display list of respected former presidents of India(i.e. Name , Term ofoffice)
import requests
from bs4 import BeautifulSoup
import pandas as pd


url = "https://presidentofindia.nic.in/former-presidents.htm"
response = requests.get(url)

if response.status_code == 200:
    soup = BeautifulSoup(response.content, 'html.parser')

table = soup.find('table')

names = []
terms = []


for row in table.find_all('tr')[1:]: 
columns = row.find_all('td')
name = columns[0].text.strip()
term = columns[1].text.strip()
names.append(name)
terms.append(term)


data = {
'Name': names,
'Term of Office': terms
    }

df = pd.DataFrame(data)


    print(df)

else:
    print("Failed to retrieve the web page. Status code:", response.status_code)



```

    Failed to retrieve the web page. Status code: 404
    


```python
# Write a python program to scrape cricket rankings from icc-cricket.com. You have to scrape and make data frame-
import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_icc_data(url):
    response = requests.get(url)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        return soup
    else:
        print("Failed to retrieve the web page. Status code:", response.status_code)
        return None

def scrape_top_odi_teams():
    url = "https://www.icc-cricket.com/rankings/mens/team-rankings/odi"
    soup = scrape_icc_data(url)
    
    if soup:
        teams = []
        matches = []
        points = []
        ratings = []
        
        table = soup.find('table', {'class': 'table'})
        rows = table.find_all('tr')
        
        for row in rows[1:11]:
            columns = row.find_all('td')
            team = columns[1].text.strip()
            match = columns[2].text.strip()
            point = columns[3].text.strip()
            rating = columns[4].text.strip()
            
            teams.append(team)
            matches.append(match)
            points.append(point)
            ratings.append(rating)
        
        data = {
            'Team': teams,
            'Matches': matches,
            'Points': points,
            'Rating': ratings
        }
        
        df = pd.DataFrame(data)
        return df

def scrape_top_odi_batsmen():
    url = "https://www.icc-cricket.com/rankings/mens/player-rankings/odi/batting"
    soup = scrape_icc_data(url)
    
    if soup:
        players = []
        teams = []
        ratings = []
        
        table = soup.find('table', {'class': 'table'})
        rows = table.find_all('tr')
        
        for row in rows[1:11]:
            columns = row.find_all('td')
            player = columns[1].text.strip()
            team = columns[2].text.strip()
            rating = columns[3].text.strip()
            
            players.append(player)
            teams.append(team)
            ratings.append(rating)
        
        data = {
            'Batsman': players,
            'Team': teams,
            'Rating': ratings
        }
        
        df = pd.DataFrame(data)
        return df

def scrape_top_odi_bowlers():
    url = "https://www.icc-cricket.com/rankings/mens/player-rankings/odi/bowling"
    soup = scrape_icc_data(url)
    
    if soup:
        players = []
        teams = []
        ratings = []
        
        table = soup.find('table', {'class': 'table'})
        rows = table.find_all('tr')
        
        for row in rows[1:11]:
            columns = row.find_all('td')
            player = columns[1].text.strip()
            team = columns[2].text.strip()
            rating = columns[3].text.strip()
            
            players.append(player)
            teams.append(team)
            ratings.append(rating)
        
        data = {
            'Bowler': players,
            'Team': teams,
            'Rating': ratings
        }
        
        df = pd.DataFrame(data)
        return df

if __name__ == "__main__":
    top_odi_teams = scrape_top_odi_teams()
    top_odi_batsmen = scrape_top_odi_batsmen()
    top_odi_bowlers = scrape_top_odi_bowlers()
    
    if top_odi_teams is not None:
        print("Top 10 ODI Teams:")
        print(top_odi_teams)
    
    if top_odi_batsmen is not None:
        print("\nTop 10 ODI Batsmen:")
        print(top_odi_batsmen)
    
    if top_odi_bowlers is not None:
        print("\nTop 10 ODI Bowlers:")
        print(top_odi_bowlers)

```

    Top 10 ODI Teams:
                   Team Matches Points Rating
    0        India\nIND      49  5,839    119
    1    Australia\nAUS      36  4,015    112
    2     Pakistan\nPAK      32  3,525    110
    3  South Africa\nSA      29  3,166    109
    4   New Zealand\nNZ      38  4,007    105
    5      England\nENG      34  3,377     99
    6     Sri Lanka\nSL      43  3,943     92
    7   Bangladesh\nBAN      40  3,574     89
    8  Afghanistan\nAFG      26  2,170     83
    9   West Indies\nWI      38  2,582     68
    
    Top 10 ODI Batsmen:
                     Batsman Team Rating
    0             Babar Azam  PAK    829
    1           Shubman Gill  IND    823
    2        Quinton de Kock   SA    769
    3       Heinrich Klaasen   SA    756
    4           David Warner  AUS    747
    5            Virat Kohli  IND    747
    6           Harry Tector  IRE    729
    7           Rohit Sharma  IND    725
    8  Rassie van der Dussen   SA    716
    9            Imam-ul-Haq  PAK    704
    
    Top 10 ODI Bowlers:
               Bowler Team Rating
    0  Josh Hazlewood  AUS    670
    1  Mohammed Siraj  IND    668
    2  Keshav Maharaj   SA    656
    3     Rashid Khan  AFG    654
    4     Trent Boult   NZ    653
    5   Mohammad Nabi  AFG    641
    6      Adam Zampa  AUS    635
    7      Matt Henry   NZ    634
    8   Kuldeep Yadav  IND    632
    9  Shaheen Afridi  PAK    625
    


```python
# Write a python program to scrape cricket rankings from icc-cricket.com. You have to scrape and make data frame-

import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_icc_data(url):
    response = requests.get(url)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        return soup
    else:
        print("Failed to retrieve the web page. Status code:", response.status_code)
        return None

def scrape_top_odi_womens_teams():
    url = "https://www.icc-cricket.com/rankings/womens/team-rankings/odi"
    soup = scrape_icc_data(url)
    
    if soup:
        teams = []
        matches = []
        points = []
        ratings = []
        
        table = soup.find('table', {'class': 'table'})
        rows = table.find_all('tr')
        
        for row in rows[1:11]:
            columns = row.find_all('td')
            team = columns[1].text.strip()
            match = columns[2].text.strip()
            point = columns[3].text.strip()
            rating = columns[4].text.strip()
            
            teams.append(team)
            matches.append(match)
            points.append(point)
            ratings.append(rating)
        
        data = {
            'Team': teams,
            'Matches': matches,
            'Points': points,
            'Rating': ratings
        }
        
        df = pd.DataFrame(data)
        return df

def scrape_top_odi_womens_batsmen():
    url = "https://www.icc-cricket.com/rankings/womens/player-rankings/odi/batting"
    soup = scrape_icc_data(url)
    
    if soup:
        players = []
        teams = []
        ratings = []
        
        table = soup.find('table', {'class': 'table'})
        rows = table.find_all('tr')
        
        for row in rows[1:11]:
            columns = row.find_all('td')
            player = columns[1].text.strip()
            team = columns[2].text.strip()
            rating = columns[3].text.strip()
            
            players.append(player)
            teams.append(team)
            ratings.append(rating)
        
        data = {
            'Batsman': players,
            'Team': teams,
            'Rating': ratings
        }
        
        df = pd.DataFrame(data)
        return df

def scrape_top_odi_womens_allrounders():
    url = "https://www.icc-cricket.com/rankings/womens/player-rankings/odi/all-rounder"
    soup = scrape_icc_data(url)
    
    if soup:
        players = []
        teams = []
        ratings = []
        
        table = soup.find('table', {'class': 'table'})
        rows = table.find_all('tr')
        
        for row in rows[1:11]:
            columns = row.find_all('td')
            player = columns[1].text.strip()
            team = columns[2].text.strip()
            rating = columns[3].text.strip()
            
            players.append(player)
            teams.append(team)
            ratings.append(rating)
        
        data = {
            'All-Rounder': players,
            'Team': teams,
            'Rating': ratings
        }
        
        df = pd.DataFrame(data)
        return df

if __name__ == "__main__":
    top_odi_womens_teams = scrape_top_odi_womens_teams()
    top_odi_womens_batsmen = scrape_top_odi_womens_batsmen()
    top_odi_womens_allrounders = scrape_top_odi_womens_allrounders()
    
    if top_odi_womens_teams is not None:
        print("Top 10 ODI Women's Teams:")
        print(top_odi_womens_teams)
    
    if top_odi_womens_batsmen is not None:
        print("\nTop 10 ODI Women's Batsmen:")
        print(top_odi_womens_batsmen)
    
    if top_odi_womens_allrounders is not None:
        print("\nTop 10 ODI Women's All-Rounders:")
        print(top_odi_womens_allrounders)

```

    Top 10 ODI Women's Teams:
                   Team Matches Points Rating
    0    Australia\nAUS      19  3,084    162
    1      England\nENG      23  2,991    130
    2  South Africa\nSA      21  2,446    116
    3        India\nIND      18  1,745     97
    4   New Zealand\nNZ      21  2,014     96
    5   West Indies\nWI      18  1,610     89
    6     Sri Lanka\nSL       9    714     79
    7   Bangladesh\nBAN      11    816     74
    8     Thailand\nTHA      11    753     68
    9     Pakistan\nPAK      21  1,435     68
    
    Top 10 ODI Women's Batsmen:
                    Batsman Team Rating
    0  Natalie Sciver-Brunt  ENG    807
    1           Beth Mooney  AUS    750
    2   Chamari Athapaththu   SL    736
    3       Laura Wolvaardt   SA    727
    4       Smriti Mandhana  IND    708
    5          Alyssa Healy  AUS    698
    6          Ellyse Perry  AUS    697
    7      Harmanpreet Kaur  IND    694
    8           Meg Lanning  AUS    662
    9        Marizanne Kapp   SA    642
    
    Top 10 ODI Women's All-Rounders:
                All-Rounder Team Rating
    0        Marizanne Kapp   SA    385
    1      Ashleigh Gardner  AUS    377
    2  Natalie Sciver-Brunt  ENG    360
    3       Hayley Matthews   WI    358
    4           Amelia Kerr   NZ    346
    5         Deepti Sharma  IND    312
    6          Ellyse Perry  AUS    282
    7         Jess Jonassen  AUS    227
    8         Sophie Devine   NZ    227
    9              Nida Dar  PAK    224
    


```python
# Write a python program to scrape mentioned news details from https://www.cnbc.com/world/?region=world and make data frame-

import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_cnbc_news(url):
    response = requests.get(url)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        return soup
    else:
        print("Failed to retrieve the web page. Status code:", response.status_code)
        return None

def scrape_cnbc_world_news():
    url = "https://www.cnbc.com/world/?region=world"
    soup = scrape_cnbc_news(url)
    
    if soup:
        headlines = []
        times = []
        news_links = []
        
        articles = soup.find_all('div', class_='Card')
        
        for article in articles:
            headline = article.find('div', class_='Card-title')
            time = article.find('time', class_='Card-time')
            link = article.find('a', href=True)
            
            if headline and time and link:
                headlines.append(headline.text.strip())
                times.append(time['datetime'])
                news_links.append(link['href'])
        
        data = {
            'Headline': headlines,
            'Time': times,
            'News Link': news_links
        }
        
        df = pd.DataFrame(data)
        return df

if __name__ == "__main__":
    cnbc_world_news_df = scrape_cnbc_world_news()
    
    if cnbc_world_news_df is not None:
        print("CNBC World News:")
        print(cnbc_world_news_df)

```

    CNBC World News:
    Empty DataFrame
    Columns: [Headline, Time, News Link]
    Index: []
    


```python
# Write a python program to scrape the details of most downloaded articles from AI in last 90 days.https://www.journals.elsevier.com/artificial-intelligence/most-downloaded-articles Scrape below mentioned details and make data frame-

import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_elsevier_data(url):
    response = requests.get(url)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        return soup
    else:
        print("Failed to retrieve the web page. Status code:", response.status_code)
        return None

def scrape_most_downloaded_articles():
    url = "https://www.journals.elsevier.com/artificial-intelligence/most-downloaded-articles"
    soup = scrape_elsevier_data(url)
    
    if soup:
        paper_titles = []
        authors_list = []
        published_dates = []
        paper_urls = []
        
        articles = soup.find_all('div', class_='article-content')
        
        for article in articles:
            title = article.find('a', class_='anchorText')
            authors = article.find('div', class_='authors')
            date = article.find('div', class_='coverDate')
            url = article.find('a', class_='anchorText', href=True)
            
            if title and authors and date and url:
                paper_titles.append(title.text.strip())
                authors_list.append(authors.text.strip())
                published_dates.append(date.text.strip())
                paper_urls.append(url['href'])
        
        data = {
            'Paper Title': paper_titles,
            'Authors': authors_list,
            'Published Date': published_dates,
            'Paper URL': paper_urls
        }
        
        df = pd.DataFrame(data)
        return df

if __name__ == "__main__":
    most_downloaded_articles_df = scrape_most_downloaded_articles()
    
    if most_downloaded_articles_df is not None:
        print("Most Downloaded Articles in Artificial Intelligence:")
        print(most_downloaded_articles_df)

```

    Most Downloaded Articles in Artificial Intelligence:
    Empty DataFrame
    Columns: [Paper Title, Authors, Published Date, Paper URL]
    Index: []
    


```python
# Write a python program to scrape mentioned details from dineout.co.in and make data frame-

import requests
from bs4 import BeautifulSoup
import pandas as pd

# Define a function to scrape data from the dineout.co.in website
def scrape_dineout_data(url):
    response = requests.get(url)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')
        return soup
    else:
        print("Failed to retrieve the web page. Status code:", response.status_code)
        return None

# Function to scrape and create a DataFrame for restaurant details
def scrape_dineout_restaurants():
    url = "https://www.dineout.co.in/delhi-restaurants"
    soup = scrape_dineout_data(url)
    
    if soup:
        restaurant_names = []
        cuisines = []
        locations = []
        ratings = []
        image_urls = []
        
        restaurants = soup.find_all('div', class_='restnt-info-main')
        
        for restaurant in restaurants:
            name = restaurant.find('a', class_='restnt-name ellipsis')
            cuisine = restaurant.find('span', class_='double-line-ellipsis')
            location = restaurant.find('span', class_='restnt-loc ellipsis')
            rating = restaurant.find('div', class_='restnt-rating')
            img = restaurant.find('img', class_='no-img')
            
            if name and cuisine and location and rating and img:
                restaurant_names.append(name.text.strip())
                cuisines.append(cuisine.text.strip())
                locations.append(location.text.strip())
                ratings.append(rating.text.strip())
                image_urls.append(img['data-src'])
        
        data = {
            'Restaurant Name': restaurant_names,
            'Cuisine': cuisines,
            'Location': locations,
            'Ratings': ratings,
            'Image URL': image_urls
        }
        
        df = pd.DataFrame(data)
        return df

# Main program to scrape and display the DataFrame
if __name__ == "__main__":
    dineout_restaurants_df = scrape_dineout_restaurants()
    
    if dineout_restaurants_df is not None:
        print("Dineout Restaurants in Delhi:")
        print(dineout_restaurants_df)

```

    Dineout Restaurants in Delhi:
    Empty DataFrame
    Columns: [Restaurant Name, Cuisine, Location, Ratings, Image URL]
    Index: []
    


```python

```
