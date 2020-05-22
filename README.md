# Extracting-Articles-from-Website
'''an effort to take out the articles with titles and categories from a news website have been illustrated using python. For this purpose, Web Scraping technique has been applied and the "article contents", along with "title" and "Category of News" has been used later on to create a dataframe in a csv format
'''
# first, lets start with importing and installing necessary libraries
pip install BeautifulSoup4
from bs4 import BeautifulSoup
import pandas as pd
import requests

# now, time to get the website url; this url is the technology section of a news website, "inshorts.com"
url_tech = "https://inshorts.com/en/read/technology"

# as you can see, the category is mentioned at the end of the url, so just pick that up using .split
news_category_tech = url_tech.split('/')[-1]
# ('/'), as words in url is separated by forward slash (/), and [-1], since we are interested at the moment in the last word (/technology)

# now the html content captured by 'requests' module and refining that by 'BeautifulSoup'

data_tech = requests.get(url_tech)
soup = BeautifulSoup(data_tech.content, 'html.parser')

# using "variable = x for x in ___" expression, we are extracting every article from the site with title and stacking that in a list

news_tech = [{'news_headline': headline.find('span', 
                                                         attrs={"itemprop": "headline"}).string,
                          'news_article': article.find('div', 
                                                       attrs={"itemprop": "articleBody"}).string,
                          'news_category': news_category_tech}
                          for headline, article in                              
                             zip(soup.find_all('div', 
                                               class_=["news-card-title news-right-box"]),
                                 soup.find_all('div', 
                                               class_=["news-card-content news-right-box"]))
                ]
 # using "zip" to collect all the headlines and contents of news from a rax html content using our bs4 module and "find_all" function,
 # you should look for the class and attribute names in the developers section of a website by clicking "F12" on the keyboard
 
 # lets do the same for two more sections, 'sports' and 'science' of that website

#---------------------sports section------------------------------

url_sport = 'https://inshorts.com/en/read/sports'

news_category_sport = url_sport.split('/')[-1]
news_category_sport

data_sport = requests.get(url_sport)
soup = BeautifulSoup(data_sport.content, 'html.parser')

news_sport = [{'news_headline': headline.find('span', attrs = {"itemprop":"headline"}).string,
               'news_article': article.find('div', attrs={"itemprop":"articleBody"}).string,
               'news_category': news_category_sport}
              
              for headline, article in
              zip(soup.find_all('div', class_= ['news-card-title news-right-box']),
                  soup.find_all('div', class_= ['news-card-content news-right-box']))
    
]

#---------------------science section------------------------------
url_science = 'https://inshorts.com/en/read/science'

news_category_science = url_science.split('/')[-1]
news_category_science

data_sci = requests.get(url_science)
soup = BeautifulSoup(data_sci.content, 'html.parser')

news_sci = [{'news_headline': headline.find('span', attrs = {"itemprop":"headline"}).string,
               'news_article': article.find('div', attrs={"itemprop":"articleBody"}).string,
               'news_category': news_category_science}
              
              for headline, article in
              zip(soup.find_all('div', class_= ['news-card-title news-right-box']),
                  soup.find_all('div', class_= ['news-card-content news-right-box']))
    
]

# storing all the raw news contents in a list named "news"

news = []
news.extend(news_tech)
news.extend(news_sport)
news.extend(news_sci)

# creating a dataframe using the "news" list

df = pd.DataFrame(news)
pd.to_csv("news_articles.csv")

# and we are done
