
# Objectives
YWBAT
* make a request using the `requests` library in Python
* make a request using the Postman App
* make a request using the Yelp API Library
* build custom functions to make yelp requests

# What is an API? (Application Programming Interface)
* allows 2 software systems to interact
* how to get software to do what you need it to do
* a way for people to access someone else's data or software


# Example
* Pandas API
    * allows a user to interact with a dataframe (xlsx, csv, tsv, json)
    
* Git API
    * allows a user to interact with repos

# Some APIs are meant to interact with a database
* Yelp API
* Folium
* NASA
* Twitter <- my favorite

### To interact you have to do a few things
* Go up to a database with a request
* You knock on the database door and you show a key to the door person
* The door person hands you the data you want (with some constraints)

# Did we just learn anything new? 
* Pandas is an API
* Basic overview of an API
* Real Time Markdown Rendering
* You've been working with APIs

# We'll be using the YELP API to get some data....


```python
import json
import requests
```

# Load in our file contents


```python
# why should you use 'with' -> because it opens the file and closes it at the end of the task
with open("/Users/rafael/.ssh/yelp.json") as f:
    yelp = json.load(f)
```


```python
yelp.keys()
```




    dict_keys(['client_id', 'api_key'])




```python
f = open("/Users/rafael/.ssh/yelp_app_2_string.txt")
```


```python
contents = f.read()
```


```python
contents
```




    '"{\n\t"clientID":"your id goes here",\n\t"AppKey":"key goes here",\n\t"listKey":[1, 2, 3, 4]\n}"\n'




```python
json.loads(contents.replace("\n\t", "").replace("\n", "").strip('"'))
```




    {'clientID': 'your id goes here',
     'AppKey': 'key goes here',
     'listKey': [1, 2, 3, 4]}



### json.load() vs json.loads()

* json.load() -> when loading in a `.json` file
* json.loads() -> when loading in a stringed file, but the string is a json

# Making a YELP request using their API

### Create request without using the Python/Yelp API


```python
business_endpoint = "https://api.yelp.com/v3/businesses/search"
params = {"term":"boba tea", "location": "Irving, Tx"}
headers = {"Authorization": f"Bearer {yelp['api_key']}"}
```


```python
r = requests.get(business_endpoint, params=params, headers=headers)
```


```python
help(requests.models.Response)
```

    Help on class Response in module requests.models:
    
    class Response(builtins.object)
     |  The :class:`Response <Response>` object, which contains a
     |  server's response to an HTTP request.
     |  
     |  Methods defined here:
     |  
     |  __bool__(self)
     |      Returns True if :attr:`status_code` is less than 400.
     |      
     |      This attribute checks if the status code of the response is between
     |      400 and 600 to see if there was a client error or a server error. If
     |      the status code, is between 200 and 400, this will return True. This
     |      is **not** a check to see if the response code is ``200 OK``.
     |  
     |  __enter__(self)
     |  
     |  __exit__(self, *args)
     |  
     |  __getstate__(self)
     |  
     |  __init__(self)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  __iter__(self)
     |      Allows you to use a response as an iterator.
     |  
     |  __nonzero__(self)
     |      Returns True if :attr:`status_code` is less than 400.
     |      
     |      This attribute checks if the status code of the response is between
     |      400 and 600 to see if there was a client error or a server error. If
     |      the status code, is between 200 and 400, this will return True. This
     |      is **not** a check to see if the response code is ``200 OK``.
     |  
     |  __repr__(self)
     |      Return repr(self).
     |  
     |  __setstate__(self, state)
     |  
     |  close(self)
     |      Releases the connection back to the pool. Once this method has been
     |      called the underlying ``raw`` object must not be accessed again.
     |      
     |      *Note: Should not normally need to be called explicitly.*
     |  
     |  iter_content(self, chunk_size=1, decode_unicode=False)
     |      Iterates over the response data.  When stream=True is set on the
     |      request, this avoids reading the content at once into memory for
     |      large responses.  The chunk size is the number of bytes it should
     |      read into memory.  This is not necessarily the length of each item
     |      returned as decoding can take place.
     |      
     |      chunk_size must be of type int or None. A value of None will
     |      function differently depending on the value of `stream`.
     |      stream=True will read data as it arrives in whatever size the
     |      chunks are received. If stream=False, data is returned as
     |      a single chunk.
     |      
     |      If decode_unicode is True, content will be decoded using the best
     |      available encoding based on the response.
     |  
     |  iter_lines(self, chunk_size=512, decode_unicode=False, delimiter=None)
     |      Iterates over the response data, one line at a time.  When
     |      stream=True is set on the request, this avoids reading the
     |      content at once into memory for large responses.
     |      
     |      .. note:: This method is not reentrant safe.
     |  
     |  json(self, **kwargs)
     |      Returns the json-encoded content of a response, if any.
     |      
     |      :param \*\*kwargs: Optional arguments that ``json.loads`` takes.
     |      :raises ValueError: If the response body does not contain valid json.
     |  
     |  raise_for_status(self)
     |      Raises stored :class:`HTTPError`, if one occurred.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  apparent_encoding
     |      The apparent encoding, provided by the chardet library.
     |  
     |  content
     |      Content of the response, in bytes.
     |  
     |  is_permanent_redirect
     |      True if this Response one of the permanent versions of redirect.
     |  
     |  is_redirect
     |      True if this Response is a well-formed HTTP redirect that could have
     |      been processed automatically (by :meth:`Session.resolve_redirects`).
     |  
     |  links
     |      Returns the parsed header links of the response, if any.
     |  
     |  next
     |      Returns a PreparedRequest for the next request in a redirect chain, if there is one.
     |  
     |  ok
     |      Returns True if :attr:`status_code` is less than 400, False if not.
     |      
     |      This attribute checks if the status code of the response is between
     |      400 and 600 to see if there was a client error or a server error. If
     |      the status code is between 200 and 400, this will return True. This
     |      is **not** a check to see if the response code is ``200 OK``.
     |  
     |  text
     |      Content of the response, in unicode.
     |      
     |      If Response.encoding is None, encoding will be guessed using
     |      ``chardet``.
     |      
     |      The encoding of the response content is determined based solely on HTTP
     |      headers, following RFC 2616 to the letter. If you can take advantage of
     |      non-HTTP knowledge to make a better guess at the encoding, you should
     |      set ``r.encoding`` appropriately before accessing this property.
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __attrs__ = ['_content', 'status_code', 'headers', 'url', 'history', '...
    



```python
response = r.json()
```


```python
response.keys()
```




    dict_keys(['businesses', 'total', 'region'])




```python
import pandas as pd
df = pd.DataFrame(response['businesses'])
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>alias</th>
      <th>name</th>
      <th>image_url</th>
      <th>is_closed</th>
      <th>url</th>
      <th>review_count</th>
      <th>categories</th>
      <th>rating</th>
      <th>coordinates</th>
      <th>transactions</th>
      <th>price</th>
      <th>location</th>
      <th>phone</th>
      <th>display_phone</th>
      <th>distance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Zga13Phl6BRrWxjAmNEuiQ</td>
      <td>zero-degrees-irving</td>
      <td>Zero Degrees</td>
      <td>https://s3-media3.fl.yelpcdn.com/bphoto/um1w6d...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/zero-degrees-irving?a...</td>
      <td>91</td>
      <td>[{'alias': 'bubbletea', 'title': 'Bubble Tea'}...</td>
      <td>3.5</td>
      <td>{'latitude': 32.839413, 'longitude': -96.991215}</td>
      <td>[pickup, delivery]</td>
      <td>$$</td>
      <td>{'address1': '3401 W Airport Fwy', 'address2':...</td>
      <td>+19724570051</td>
      <td>(972) 457-0051</td>
      <td>3509.160524</td>
    </tr>
    <tr>
      <td>1</td>
      <td>neQZ06_BDrhBB8Moyak67Q</td>
      <td>tiger-sugar-carrollton-2</td>
      <td>Tiger Sugar</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/XNEEdw...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/tiger-sugar-carrollto...</td>
      <td>49</td>
      <td>[{'alias': 'bubbletea', 'title': 'Bubble Tea'}]</td>
      <td>4.0</td>
      <td>{'latitude': 32.98474, 'longitude': -96.91038}</td>
      <td>[pickup, delivery]</td>
      <td>NaN</td>
      <td>{'address1': '2625 Old Denton Rd', 'address2':...</td>
      <td></td>
      <td></td>
      <td>14586.052811</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AKpFdTD-HEWY4hqEqqds7g</td>
      <td>9-rabbits-bakery-dallas</td>
      <td>9 Rabbits Bakery</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/bSAtNx...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/9-rabbits-bakery-dall...</td>
      <td>425</td>
      <td>[{'alias': 'bakeries', 'title': 'Bakeries'}, {...</td>
      <td>4.0</td>
      <td>{'latitude': 32.8957035472635, 'longitude': -9...</td>
      <td>[pickup, delivery]</td>
      <td>$</td>
      <td>{'address1': '2546 Royal Ln', 'address2': '', ...</td>
      <td>+19722434478</td>
      <td>(972) 243-4478</td>
      <td>7676.587553</td>
    </tr>
    <tr>
      <td>3</td>
      <td>PvA5d3nKcLdW9Z9Je9scvw</td>
      <td>thai-tea-asian-fusion-cafe-irving</td>
      <td>Thai Tea Asian Fusion Cafe</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/oZVY1S...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/thai-tea-asian-fusion...</td>
      <td>269</td>
      <td>[{'alias': 'thai', 'title': 'Thai'}, {'alias':...</td>
      <td>3.5</td>
      <td>{'latitude': 32.91897, 'longitude': -96.99423}</td>
      <td>[pickup, delivery]</td>
      <td>$$</td>
      <td>{'address1': '8251 N Belt Line Rd', 'address2'...</td>
      <td>+19729299888</td>
      <td>(972) 929-9888</td>
      <td>6788.836269</td>
    </tr>
    <tr>
      <td>4</td>
      <td>FJoH2gkwGEM2VgRlOn51Sw</td>
      <td>itea-lounge-euless</td>
      <td>iTea Lounge</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/JEbp33...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/itea-lounge-euless?ad...</td>
      <td>122</td>
      <td>[{'alias': 'coffee', 'title': 'Coffee &amp; Tea'},...</td>
      <td>4.5</td>
      <td>{'latitude': 32.8806157399706, 'longitude': -9...</td>
      <td>[delivery]</td>
      <td>$</td>
      <td>{'address1': '1301 W Glade Rd', 'address2': 'S...</td>
      <td>+18173581610</td>
      <td>(817) 358-1610</td>
      <td>12817.445970</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
