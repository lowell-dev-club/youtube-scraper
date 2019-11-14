# Youtube Scraper

using an online [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) **[REPL.it](https://repl.it)**

## Libraries and Webscraping

What is an Library?  

A library is a set of implenmentations written by previous programmers that we can use to perform mechanisms such as web scraping, which is what we'll be doing in this workshop. Now to explain webscraping is a method for extracting data usually in the form of text for our use. 

[Read more about Libraries!](https://en.wikipedia.org/wiki/Library_(computing))  

In this workshop we'll be using webscraping library called beautiful soup to scrape data from youtube videos.

## Repl.it

Create a new repl and choose python. Make sure its specified as Python or Python 3. **Do Not Use Python 2**

![repl it image](https://github.com/lowell-dev-club/python-text-game/blob/master/replit.png?raw=true)

## Creating the program

#### Imports

We start by importing the needed code from the beautifulsoup library.

```python
import requests
from bs4 import BeautifulSoup as bs
```

#### Making the scrape function

This is a bunch of code so don't bother typing this out. Just copy and paste this code below the lines of code that we added to import the beauitfulsoup library. Now a few things to pay attention to is that the lines ```content = requests.get(url)``` and ```soup = bs(content.content, "html.parser")``` contain all of the HTML code (website code) from the specific youtube video. The rest of the code you see below parses, analyzes, and edits the data from the site. **If you use your mouse to hover over the video description and click on the inspect element button you should be able to see HTML code.**


```python
def get_video_info(url):
  # download HTML code
  content = requests.get(url)
  # create beautiful soup object to parse HTML
  soup = bs(content.content, "html.parser")
  # initialize the result
  result = {}
  # video title
  result['title'] = soup.find("span", attrs={"class": "watch-title"}).text.strip()
  # video views (converted to integer)
  result['views'] = int(soup.find("div", attrs={"class": "watch-view-count"}).text[:-6].replace(",", ""))
  # video description
  result['description'] = soup.find("p", attrs={"id": "eow-description"}).text
  # date published
  result['date_published'] = soup.find("strong", attrs={"class": "watch-time-text"}).text
  # number of likes as integer
  result['likes'] = int(soup.find("button", attrs={"title": "I like this"}).text.replace(",", ""))
  # number of dislikes as integer
  result['dislikes'] = int(soup.find("button", attrs={"title": "I dislike this"}).text.replace(",", ""))
  # channel details
  channel_tag = soup.find("div", attrs={"class": "yt-user-info"}).find("a")
  # channel name
  channel_name = channel_tag.text
  # channel URL
  channel_url = f"https://www.youtube.com{channel_tag['href']}"
  # number of subscribers as str
  channel_subscribers = soup.find("span", attrs={"class": "yt-subscriber-count"}).text.strip()
  result['channel'] = {'name': channel_name, 'url': channel_url, 'subscribers': channel_subscribers}
  return result
```
#### Run the scraping program

You can also copy and paste this code below the function that you previous copied and pasted. The code below just calls the ```def get_video_info(url)``` function which runs our scraping script. We pass in the ```url``` variable which will be the link to our youtube video. The rest of the code with the ```print()``` functions are displaying the video data for us to see.  

```python
if __name__ == "__main__":
    # parse the video URL from command line
    url = "https://www.youtube.com/watch?v=D9lVNzyhYnc"
    
    data = get_video_info(url)

    # print in nice format
    print(f"Title: {data['title']}")
    print(f"Views: {data['views']}")
    print(f"\nDescription: {data['description']}\n")
    print(data['date_published'])
    print(f"Likes: {data['likes']}")
    print(f"Dislikes: {data['dislikes']}")
    print(f"\nChannel Name: {data['channel']['name']}")
    print(f"Channel URL: {data['channel']['url']}")
    print(f"Channel Subscribers: {data['channel']['subscribers']}")
```

#### Advanced steps

Scrape you own website.

```
Try these websites
https://weather.com/weather/today/l/37.78,-122.49?par=google&temp=f
https://www.amazon.com/s?k=toys&ref=nb_sb_noss_2
https://www.target.com/s?searchTerm=games
https://www.rottentomatoes.com/tv/the_mandalorian
https://www.boxofficemojo.com/release/rl252151297/?ref_=bo_sh_tx
```

You can now customize your html email messages with css styling and html elements!

View the code file [here](emailer.py)  
Check out a functioning program [here](https://repl.it/@calee14/Youtube-Scraper)
