---
title: "Working with APIs: Collecting Your Own Data"
date: 2026-01-26
draft: false
description: "Learn how to collect data from APIs including authentication, rate limits, error handling, and building your own datasets from social media, weather, financial, and other services"
tags: ["API", "data-collection", "python", "R", "web-data", "automation"]
categories: ["Data Collection", "APIs"]
weight: 5
---

## What is an API?

An API (Application Programming Interface) is like a waiter at a restaurant. You (the client) ask the waiter (the API) for what you want, and they bring you the data from the kitchen (the server). APIs let you programmatically collect data instead of manually downloading files.

**Why use APIs?**
- Get current, up-to-date data
- Automate data collection
- Access data not available for download
- Build custom datasets for your research

---

## API Basics: The Request-Response Cycle

```
You → Request → API → Server
You ← Response ← API ← Server
```

**Key concepts:**
- **Endpoint:** The URL you send requests to
- **Method:** GET (retrieve), POST (create), PUT (update), DELETE (remove)
- **Parameters:** Filters for your request (dates, keywords, etc.)
- **Headers:** Meta-information (authentication, content type)
- **Response:** Data returned (usually JSON or XML)
- **Status Code:** Whether it worked (200=success, 404=not found, 500=server error)

---

## Your First API Request

### Python Example

```python
import requests
import json

# Simple GET request to a public API
response = requests.get('https://api.github.com/users/octocat')

# Check if successful
if response.status_code == 200:
    data = response.json()  # Parse JSON response
    print(f"Name: {data['name']}")
    print(f"Followers: {data['followers']}")
    print(f"Public repos: {data['public_repos']}")
else:
    print(f"Error: {response.status_code}")
```

### R Example

```r
library(httr)
library(jsonlite)

# Simple GET request
response <- GET('https://api.github.com/users/octocat')

# Check if successful
if (status_code(response) == 200) {
  data <- content(response, as = "parsed")
  cat(sprintf("Name: %s\n", data$name))
  cat(sprintf("Followers: %d\n", data$followers))
  cat(sprintf("Public repos: %d\n", data$public_repos))
} else {
  cat(sprintf("Error: %d\n", status_code(response)))
}
```

---

## Example 1: Weather Data API

Let's collect weather data from OpenWeatherMap (free with signup).

### Python Implementation

```python
import requests
import pandas as pd
from datetime import datetime

class WeatherCollector:
    """Collect weather data from OpenWeatherMap API"""
    
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.openweathermap.org/data/2.5"
    
    def get_current_weather(self, city):
        """Get current weather for a city"""
        endpoint = f"{self.base_url}/weather"
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric'  # Celsius
        }
        
        response = requests.get(endpoint, params=params)
        
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Error {response.status_code}: {response.text}")
            return None
    
    def get_forecast(self, city, days=5):
        """Get weather forecast"""
        endpoint = f"{self.base_url}/forecast"
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric',
            'cnt': days * 8  # 8 readings per day (3-hour intervals)
        }
        
        response = requests.get(endpoint, params=params)
        
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Error {response.status_code}: {response.text}")
            return None
    
    def to_dataframe(self, weather_data):
        """Convert weather data to pandas DataFrame"""
        records = []
        
        for item in weather_data['list']:
            record = {
                'datetime': datetime.fromtimestamp(item['dt']),
                'temp': item['main']['temp'],
                'feels_like': item['main']['feels_like'],
                'humidity': item['main']['humidity'],
                'pressure': item['main']['pressure'],
                'weather': item['weather'][0]['main'],
                'description': item['weather'][0]['description'],
                'wind_speed': item['wind']['speed']
            }
            records.append(record)
        
        return pd.DataFrame(records)

# Use it!
api_key = "YOUR_API_KEY_HERE"  # Get from openweathermap.org
collector = WeatherCollector(api_key)

# Get current weather
current = collector.get_current_weather("London")
print(f"Temperature in London: {current['main']['temp']}°C")

# Get forecast and convert to dataframe
forecast = collector.get_forecast("London")
df = collector.to_dataframe(forecast)
print(df.head())

# Save to CSV
df.to_csv('london_weather_forecast.csv', index=False)
```

### R Implementation

```r
library(httr)
library(jsonlite)
library(tidyverse)

#' Weather Data Collector
#' @param api_key Your OpenWeatherMap API key
WeatherCollector <- R6::R6Class("WeatherCollector",
  public = list(
    api_key = NULL,
    base_url = "https://api.openweathermap.org/data/2.5",
    
    initialize = function(api_key) {
      self$api_key <- api_key
    },
    
    get_current_weather = function(city) {
      endpoint <- paste0(self$base_url, "/weather")
      
      response <- GET(
        endpoint,
        query = list(
          q = city,
          appid = self$api_key,
          units = "metric"
        )
      )
      
      if (status_code(response) == 200) {
        return(content(response, as = "parsed"))
      } else {
        cat(sprintf("Error %d: %s\n", status_code(response), content(response, "text")))
        return(NULL)
      }
    },
    
    get_forecast = function(city, days = 5) {
      endpoint <- paste0(self$base_url, "/forecast")
      
      response <- GET(
        endpoint,
        query = list(
          q = city,
          appid = self$api_key,
          units = "metric",
          cnt = days * 8
        )
      )
      
      if (status_code(response) == 200) {
        return(content(response, as = "parsed"))
      } else {
        cat(sprintf("Error %d\n", status_code(response)))
        return(NULL)
      }
    },
    
    to_dataframe = function(weather_data) {
      records <- map_dfr(weather_data$list, function(item) {
        tibble(
          datetime = as.POSIXct(item$dt, origin = "1970-01-01"),
          temp = item$main$temp,
          feels_like = item$main$feels_like,
          humidity = item$main$humidity,
          pressure = item$main$pressure,
          weather = item$weather[[1]]$main,
          description = item$weather[[1]]$description,
          wind_speed = item$wind$speed
        )
      })
      
      return(records)
    }
  )
)

# Use it!
api_key <- "YOUR_API_KEY_HERE"
collector <- WeatherCollector$new(api_key)

# Get current weather
current <- collector$get_current_weather("London")
cat(sprintf("Temperature in London: %.1f°C\n", current$main$temp))

# Get forecast
forecast <- collector$get_forecast("London")
df <- collector$to_dataframe(forecast)
print(head(df))

# Save to CSV
write_csv(df, 'london_weather_forecast.csv')
```

---

## Example 2: Financial Data API

Get stock prices from Alpha Vantage (free with signup).

### Python Implementation

```python
import requests
import pandas as pd
import time

class StockDataCollector:
    """Collect stock data from Alpha Vantage API"""
    
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://www.alphavantage.co/query"
    
    def get_daily_prices(self, symbol, outputsize='compact'):
        """
        Get daily stock prices
        outputsize: 'compact' (100 days) or 'full' (20+ years)
        """
        params = {
            'function': 'TIME_SERIES_DAILY',
            'symbol': symbol,
            'outputsize': outputsize,
            'apikey': self.api_key
        }
        
        response = requests.get(self.base_url, params=params)
        
        if response.status_code == 200:
            data = response.json()
            
            if 'Time Series (Daily)' in data:
                # Convert to DataFrame
                df = pd.DataFrame.from_dict(
                    data['Time Series (Daily)'], 
                    orient='index'
                )
                df.index = pd.to_datetime(df.index)
                df = df.astype(float)
                df.columns = ['open', 'high', 'low', 'close', 'volume']
                df = df.sort_index()
                return df
            else:
                print(f"Error: {data.get('Note', data.get('Error Message', 'Unknown error'))}")
                return None
        else:
            print(f"HTTP Error: {response.status_code}")
            return None
    
    def get_multiple_stocks(self, symbols, delay=12):
        """
        Get data for multiple stocks
        delay: Seconds to wait between requests (avoid rate limits)
        """
        results = {}
        
        for i, symbol in enumerate(symbols):
            print(f"Fetching {symbol} ({i+1}/{len(symbols)})...")
            results[symbol] = self.get_daily_prices(symbol)
            
            # Wait to respect rate limits (5 requests/minute for free tier)
            if i < len(symbols) - 1:
                time.sleep(delay)
        
        return results

# Use it!
api_key = "YOUR_API_KEY_HERE"
collector = StockDataCollector(api_key)

# Get single stock
aapl = collector.get_daily_prices('AAPL')
print(aapl.head())

# Get multiple stocks
symbols = ['AAPL', 'GOOGL', 'MSFT']
stocks = collector.get_multiple_stocks(symbols)

# Combine and save
combined = pd.concat(stocks, names=['symbol', 'date'])
combined.to_csv('stock_prices.csv')
```

---

## Example 3: Social Media API (Twitter/X)

Collect tweets for research using the Twitter API v2.

### Python Implementation

```python
import requests
import pandas as pd
from datetime import datetime, timedelta

class TwitterCollector:
    """Collect tweets using Twitter API v2"""
    
    def __init__(self, bearer_token):
        self.bearer_token = bearer_token
        self.base_url = "https://api.twitter.com/2"
    
    def _get_headers(self):
        return {"Authorization": f"Bearer {self.bearer_token}"}
    
    def search_recent_tweets(self, query, max_results=100):
        """
        Search recent tweets (last 7 days)
        query: Search query (e.g., "climate change -is:retweet")
        """
        endpoint = f"{self.base_url}/tweets/search/recent"
        
        params = {
            'query': query,
            'max_results': min(max_results, 100),  # Max 100 per request
            'tweet.fields': 'created_at,public_metrics,lang,author_id',
            'expansions': 'author_id',
            'user.fields': 'username,verified,public_metrics'
        }
        
        response = requests.get(
            endpoint,
            headers=self._get_headers(),
            params=params
        )
        
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Error {response.status_code}: {response.text}")
            return None
    
    def to_dataframe(self, tweet_data):
        """Convert tweet data to pandas DataFrame"""
        if not tweet_data or 'data' not in tweet_data:
            return pd.DataFrame()
        
        records = []
        
        # Create user lookup
        users = {}
        if 'includes' in tweet_data and 'users' in tweet_data['includes']:
            users = {u['id']: u for u in tweet_data['includes']['users']}
        
        for tweet in tweet_data['data']:
            user = users.get(tweet['author_id'], {})
            
            record = {
                'tweet_id': tweet['id'],
                'text': tweet['text'],
                'created_at': pd.to_datetime(tweet['created_at']),
                'author_id': tweet['author_id'],
                'username': user.get('username', 'unknown'),
                'verified': user.get('verified', False),
                'likes': tweet['public_metrics']['like_count'],
                'retweets': tweet['public_metrics']['retweet_count'],
                'replies': tweet['public_metrics']['reply_count'],
                'language': tweet.get('lang', 'unknown')
            }
            records.append(record)
        
        return pd.DataFrame(records)

# Use it!
bearer_token = "YOUR_BEARER_TOKEN_HERE"
collector = TwitterCollector(bearer_token)

# Search for tweets about climate change
query = "climate change lang:en -is:retweet"
tweets = collector.search_recent_tweets(query, max_results=100)

# Convert to dataframe and analyze
df = collector.to_dataframe(tweets)
print(f"Collected {len(df)} tweets")
print(f"\nMost liked tweet:")
print(df.loc[df['likes'].idxmax()]['text'])

# Save
df.to_csv('climate_tweets.csv', index=False)
```

---

## Handling Authentication

Most APIs require authentication. Here are common methods:

### API Keys (Simplest)

```python
# Pass in URL
response = requests.get(f"https://api.example.com/data?api_key={API_KEY}")

# Pass in headers
headers = {'Authorization': f'Bearer {API_KEY}'}
response = requests.get("https://api.example.com/data", headers=headers)

# Pass as parameter
params = {'key': API_KEY, 'query': 'data'}
response = requests.get("https://api.example.com/data", params=params)
```

### OAuth 2.0 (More Complex)

```python
from requests_oauthlib import OAuth2Session

# OAuth configuration
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
authorization_base_url = "https://api.example.com/oauth/authorize"
token_url = "https://api.example.com/oauth/token"

# Step 1: Get authorization
oauth = OAuth2Session(client_id)
authorization_url, state = oauth.authorization_url(authorization_base_url)
print(f"Visit this URL to authorize: {authorization_url}")

# Step 2: User visits URL and gets redirected with code
redirect_response = input("Paste the full redirect URL here: ")

# Step 3: Fetch access token
token = oauth.fetch_token(
    token_url,
    client_secret=client_secret,
    authorization_response=redirect_response
)

# Step 4: Make authenticated requests
response = oauth.get("https://api.example.com/data")
```

---

## Rate Limiting: Don't Get Banned!

APIs limit how many requests you can make. Respect these limits!

### Strategy 1: Sleep Between Requests

```python
import time

for item in items:
    response = requests.get(f"https://api.example.com/data/{item}")
    # Do something with response
    time.sleep(1)  # Wait 1 second between requests
```

### Strategy 2: Exponential Backoff

```python
import time

def make_request_with_retry(url, max_retries=5):
    """Make request with exponential backoff on failure"""
    for attempt in range(max_retries):
        response = requests.get(url)
        
        if response.status_code == 200:
            return response
        elif response.status_code == 429:  # Rate limited
            wait_time = 2 ** attempt  # 1, 2, 4, 8, 16 seconds
            print(f"Rate limited. Waiting {wait_time} seconds...")
            time.sleep(wait_time)
        else:
            print(f"Error {response.status_code}")
            return None
    
    print("Max retries reached")
    return None
```

### Strategy 3: Use Rate Limiter Library

```python
from ratelimit import limits, sleep_and_retry

# Limit to 10 calls per minute
@sleep_and_retry
@limits(calls=10, period=60)
def call_api(url):
    response = requests.get(url)
    return response

# Use it - automatically handles rate limiting
for url in urls:
    response = call_api(url)
```

---

## Error Handling

Always handle errors gracefully!

```python
def robust_api_call(url, max_retries=3):
    """Robust API call with comprehensive error handling"""
    
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=10)
            
            # Check status code
            if response.status_code == 200:
                try:
                    return response.json()
                except json.JSONDecodeError:
                    print("Invalid JSON response")
                    return None
            
            elif response.status_code == 429:
                print("Rate limited. Waiting...")
                time.sleep(60)
                continue
            
            elif response.status_code == 404:
                print("Resource not found")
                return None
            
            elif response.status_code >= 500:
                print(f"Server error {response.status_code}. Retrying...")
                time.sleep(5)
                continue
            
            else:
                print(f"Error {response.status_code}: {response.text}")
                return None
        
        except requests.exceptions.Timeout:
            print(f"Request timed out (attempt {attempt + 1}/{max_retries})")
            time.sleep(2)
        
        except requests.exceptions.ConnectionError:
            print(f"Connection error (attempt {attempt + 1}/{max_retries})")
            time.sleep(5)
        
        except Exception as e:
            print(f"Unexpected error: {e}")
            return None
    
    print("Max retries reached")
    return None
```

---

## Saving and Resuming Data Collection

For long-running collection, save progress!

```python
import json
import os

class DataCollector:
    """Collect data with progress saving"""
    
    def __init__(self, checkpoint_file='checkpoint.json'):
        self.checkpoint_file = checkpoint_file
        self.load_checkpoint()
    
    def load_checkpoint(self):
        """Load previous progress"""
        if os.path.exists(self.checkpoint_file):
            with open(self.checkpoint_file, 'r') as f:
                self.checkpoint = json.load(f)
        else:
            self.checkpoint = {'completed': [], 'last_id': None}
    
    def save_checkpoint(self):
        """Save current progress"""
        with open(self.checkpoint_file, 'w') as f:
            json.dump(self.checkpoint, f)
    
    def collect_data(self, items):
        """Collect data with checkpointing"""
        results = []
        
        for item in items:
            # Skip if already completed
            if item in self.checkpoint['completed']:
                print(f"Skipping {item} (already completed)")
                continue
            
            # Collect data
            data = self.fetch_from_api(item)
            if data:
                results.append(data)
                
                # Mark as complete and save
                self.checkpoint['completed'].append(item)
                self.save_checkpoint()
        
        return results
    
    def fetch_from_api(self, item):
        """Your API call here"""
        # Implement your actual API call
        pass
```

---

## Best Practices

1. **Read the API documentation** - Every API is different!
2. **Get a key** - Don't use someone else's
3. **Respect rate limits** - Add delays between requests
4. **Handle errors gracefully** - Networks fail, servers crash
5. **Save incrementally** - Don't lose hours of data to a crash
6. **Cache responses** - Don't request the same data twice
7. **Check terms of service** - Know what you're allowed to do
8. **Be a good citizen** - Don't overload servers

---

## Common APIs for Research

| API | Use Case | Free Tier |
|-----|----------|-----------|
| Twitter | Social media analysis | Yes (limited) |
| Reddit | Community discussions | Yes |
| OpenWeatherMap | Weather data | Yes (limited) |
| Alpha Vantage | Stock prices | Yes (limited) |
| NASA | Space/earth data | Yes |
| World Bank | Economic indicators | Yes |
| PubMed | Academic papers | Yes |
| GitHub | Code repositories | Yes |
| Spotify | Music data | Yes (limited) |
| Google Maps | Geographic data | Yes (limited) |

---

## Next Steps

Now that you can collect data from APIs:

1. **[Web Scraping](../web-scraping)** - When there's no API
2. **[Data Cleaning](../data-cleaning)** - Clean your collected data
3. **[Exploratory Analysis](../exploratory-analysis)** - Analyze what you collected

**Remember:** With great API access comes great responsibility. Collect ethically and responsibly!

*Happy API wrangling!* 🚀
