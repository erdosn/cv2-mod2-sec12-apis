
### Questions
* getting a handle on api workflow

### Objectives
YWBAT 
* hide api permissions in your notebook
* access api and use it in data collection

### Outline
* build a twitter data collection tool
* store our tweets in a mongodb


```python
import json
import pandas as pd
import numpy as np
import tweepy as tw

from pymongo import MongoClient

import matplotlib.pyplot as plt
```


```python
with open("/Users/rafael/.ssh/twitter_app00.json") as f:
    d = json.load(f)
```


```python
oauth = tw.OAuthHandler(consumer_key=d["consumer_key"], consumer_secret=d["consumer_secret"])
oauth.set_access_token(key=d["access_token"], secret=d["access_token_secret"])
```


```python
api = tw.API(auth_handler=oauth)
```

## What can I do with Twitter's API?
* udpate a status


```python
def update_statuses(api, status_list):
    for status in status_list:
        api.update_status(status)
    pass


def delete_recent_tweet(api):
    last_tweet_id = api.me()._json["status"]["id"]
    print("found id")
    print("deleting that ish")
    api.destroy_status(last_tweet_id)
    print("deleted...whew....")
    pass
```


```python
# search stuff
results = api.search("#rickandmortyseason4", tweet_mode='extended')
```


```python
len(results)
```




    14



boooooo!!!! only 14 results? Dude, I want thousands....


```python
class MyStreamListener(tw.StreamListener):
    
    
    def on_status(self, status):
#         try:
#             print(status.extended_tweet["full_text"])
#         except:
#             print('---------not extended status---------')
#             print(status.text)
        print("inserted tweet into collection")
        got_collection.insert_one(status._json)
        pass
        
     
    def on_error(self, status_code):
        if status_code == 420:
            #returning False in on_error disconnects the stream
            return False

        # returning non-False reconnects the stream, with backoff.
```


```python
# Make a streamer object
listener = MyStreamListener()

# make a stream using our listener
stream = tw.Stream(auth=oauth, listener=listener)
```

# Connect to Mongo and Create 'Table' (collection) for got tweets


```python
client = MongoClient(host='localhost', port=27017)
tweets = client["tweets"]
```


```python
got_collection = tweets["got_collection"]
```


```python
rick_and_morty_list = ["@RickAndMorty", "#rickandmortyseason4", "#rickandmorty", '#wubbalubbadubdub', '#picklerick', '#tinyrick', 'rick and morty']
got = ["game of thrones", "#got"]
stream.filter(track=got, is_async=True)


```

    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection
    inserted tweet into collection


    Exception in thread Thread-7:
    Traceback (most recent call last):
      File "/anaconda3/lib/python3.7/http/client.py", line 544, in _get_chunk_left
        chunk_left = self._read_next_chunk_size()
      File "/anaconda3/lib/python3.7/http/client.py", line 511, in _read_next_chunk_size
        return int(line, 16)
    ValueError: invalid literal for int() with base 16: b''
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/anaconda3/lib/python3.7/http/client.py", line 576, in _readinto_chunked
        chunk_left = self._get_chunk_left()
      File "/anaconda3/lib/python3.7/http/client.py", line 546, in _get_chunk_left
        raise IncompleteRead(b'')
    http.client.IncompleteRead: IncompleteRead(0 bytes read)
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/anaconda3/lib/python3.7/site-packages/urllib3/response.py", line 360, in _error_catcher
        yield
      File "/anaconda3/lib/python3.7/site-packages/urllib3/response.py", line 442, in read
        data = self._fp.read(amt)
      File "/anaconda3/lib/python3.7/http/client.py", line 447, in read
        n = self.readinto(b)
      File "/anaconda3/lib/python3.7/http/client.py", line 481, in readinto
        return self._readinto_chunked(b)
      File "/anaconda3/lib/python3.7/http/client.py", line 592, in _readinto_chunked
        raise IncompleteRead(bytes(b[0:total_bytes]))
    http.client.IncompleteRead: IncompleteRead(0 bytes read)
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "/anaconda3/lib/python3.7/threading.py", line 917, in _bootstrap_inner
        self.run()
      File "/anaconda3/lib/python3.7/threading.py", line 865, in run
        self._target(*self._args, **self._kwargs)
      File "/anaconda3/lib/python3.7/site-packages/tweepy/streaming.py", line 300, in _run
        six.reraise(*exc_info)
      File "/anaconda3/lib/python3.7/site-packages/six.py", line 693, in reraise
        raise value
      File "/anaconda3/lib/python3.7/site-packages/tweepy/streaming.py", line 269, in _run
        self._read_loop(resp)
      File "/anaconda3/lib/python3.7/site-packages/tweepy/streaming.py", line 319, in _read_loop
        line = buf.read_line()
      File "/anaconda3/lib/python3.7/site-packages/tweepy/streaming.py", line 181, in read_line
        self._buffer += self._stream.read(self._chunk_size)
      File "/anaconda3/lib/python3.7/site-packages/urllib3/response.py", line 459, in read
        raise IncompleteRead(self._fp_bytes_read, self.length_remaining)
      File "/anaconda3/lib/python3.7/contextlib.py", line 130, in __exit__
        self.gen.throw(type, value, traceback)
      File "/anaconda3/lib/python3.7/site-packages/urllib3/response.py", line 378, in _error_catcher
        raise ProtocolError('Connection broken: %r' % e, e)
    urllib3.exceptions.ProtocolError: ('Connection broken: IncompleteRead(0 bytes read)', IncompleteRead(0 bytes read))
    


### Assessment
