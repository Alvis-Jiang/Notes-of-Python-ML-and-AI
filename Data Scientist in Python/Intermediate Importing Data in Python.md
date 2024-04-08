# Importing flat files from the web

## Saving web files with urlretrieve()
# Import package
from urllib.request import urlretrieve

# Import pandas
import pandas as pd

# Assign url of file: url
url = "https://assets.datacamp.com/production/course_1606/datasets/winequality-red.csv"

# Save file locally
urlretrieve(url, "winequality-red.csv")

# Read file into a DataFrame and print its head
df = pd.read_csv('winequality-red.csv', sep=';')
print(df.head())

## Opening and reading flat files from the web
`# Import packages`
`import matplotlib.pyplot as plt`
`import pandas as pd`

`# Assign url of file: url`
`url = "https://assets.datacamp.com/production/course_1606/datasets/winequality-red.csv"`

`# Read file into a DataFrame: df`
`df = pd.read_csv(url, sep = ";")`



# HTTP requests to import files from the web
##  HTTP requests in Python using urllib()
`# Import packages`
`from urllib.request import urlopen, Request`

`# Specify the url`
`url = "https://campus.datacamp.com/courses/1606/4135?ex=2"`

`# This packages the request: request`
`request = Request(url)`

`# Sends the request and catches the response: response`
`response = urlopen(request)`

`# Extract the response: html`
`html = response.read()`

`# Be polite and close the response!`
`response.close()`


## Performing HTTP requests in Python using requests
`# Import package`
`import requests`

`# Specify the url`
`url = "http://www.datacamp.com/teach/documentation"`

`# Packages the request, send the request and catch the response: r`
`r = requests.get(url)`

`# Extract the response: text`
`text = **r.text**`

`# Print the html`
`print(text)`


## Scraping the Web in Python -- Beautiful Soup
`# Import packages`
`import requests`
`from bs4 import BeautifulSoup`

`# Specify url: url`
`url = "https://www.python.org/~guido/"`

`# Package the request, send the request and catch the response: r`
`r = requests.get(url)`

`# Extracts the response as html: html_doc`
`html_doc = r.text`

`# Create a BeautifulSoup object from the HTML: soup`
`soup = BeautifulSoup(html_doc)`

`# Prettify the BeautifulSoup object: pretty_soup`
`pretty_soup = soup.prettify()`

`# Get the title of Guido's webpage: guido_title`
`guido_title = soup.title`

### # Getting the text
`# Get Guido's text: guido_text`
`guido_text = soup.get_text()`


### # Getting the hyperlinks
`# Find all 'a' tags (<a>, which define hyperlinks): a_tags`
`a_tags = soup.find_all('a')`

`# Print the URLs to the shell`
`for link in a_tags:`
	`print(link.get('href'))```

# JASON and APIs
##  Loading and exploring a JSON
`# Load JSON: json_data`
`with open("a_movie.json") as json_file:`
    `json_data = json.loads(json_file)`

`# Print each key-value pair in json_data`
`for k in json_data.keys():`
    `print(k + ': ', json_data[k])`

## JSON–from the web to Python
`# Import requests package`
`import requests`

`# Assign URL to variable: url`
`query_string = "apikey=72bc447a&t=the+social+network"`
`url = "http://www.omdbapi.com"`
`url_final = url + "/?" + query_string`
  
`# Package the request, send the request and catch the response: r(JSON)`
`r = requests.get(url_final)`

`# Decode the JSON data into a dictionary: json_data`
`json_data = r.json()`


## The Twitter API and Authentication

### Streaming tweets
#### # credentials: 
consumer_key, consumer_secret, access_token and access_token_secret
#### # Stream variable:
track = ['xxx', 'xxxx']

`# Store credentials in relevant variables`
`consumer_key = "nZ6EA0FxZ293SxGNg8g8aP0HM"`
`consumer_secret = "fJGEodwe3KiKUnsYJC3VRndj7jevVvXbK2D5EiJ2nehafRgA6i"`
`access_token = "1092294848-aHN7DcRP9B4VMTQIhwqOYiB14YkW92fFO8k8EPy"`
`access_token_secret = "X4dHmhPfaksHcQ7SCbmZa2oYBBVSD2g8uIHXsp5CTaksx"`

`# Create your Stream object with credentials`
`stream = tweepy.Stream(consumer_key, consumer_secret, access_token, access_token_secret)`

`# Filter your Stream variable`
`stream.filter(track = ["clinton", "trump", "sanders", "cruz"])`

### Twitter data to DataFrame

`# Import package`
`import pandas as pd`

`# Build DataFrame of tweet texts and languages`
`# First argument is a dictionary(tweets_data), the second argument is a list of keys you wish to have as columns`
`df = pd.DataFrame(tweets_data, columns=['text', 'lang'])`

`# iterate through df`
`for index, row in df.iterrows():`
`





