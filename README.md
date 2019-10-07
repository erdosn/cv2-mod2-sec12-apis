
### Questions


### Outline
- go over objectives
- take questions
- learn about json files
- dive into api
- load data into pandas 
- that's it

### Objectives
- access api permissions in python securely
- get data from yelp's api
- load data into pandas for analysis


### Tools I use
- Lots of documentation
- Postman


```python
import json

# makes url requests
import requests 

import numpy as np
import pandas as pd

import pprint

import matplotlib.pyplot as plt
```

#### Load in my permissions (fake)


```python
with open("/Users/rafael/yelp-permissions.json") as f:
    d = json.load(f)
d.keys()
```




    dict_keys(['client_id', 'api_key'])



#### Load in my permissions (real)


```python
with open("/Users/rafael/.ssh/yelp.json") as f:
    d = json.load(f)
d.keys()
```




    dict_keys(['client_id', 'api_key'])



### Let's just hit an endpoint or find a tutorial


```python
client_id = d['client_id']
api_key = d['api_key']
```


```python
term = 'brisket'
location = 'TX'

url = 'https://api.yelp.com/v3/businesses/search'

headers = {
        'Authorization': f'Bearer {api_key}',
    }

url_params = {
                'term': term.replace(' ', '+'),
                'location': location.replace(' ', '+'),
}
```


```python
response = requests.get(url, headers=headers, params=url_params) #Your code here
print(response)
print(type(response.text))
print(response.text[:1000])
```

    <Response [200]>
    <class 'str'>
    {"businesses": [{"id": "mStkEcVeXv1HXlGqoYQx2A", "alias": "the-brisket-house-houston", "name": "The Brisket House", "image_url": "https://s3-media4.fl.yelpcdn.com/bphoto/TYH3OEZ0_n6606_vpYsnRA/o.jpg", "is_closed": false, "url": "https://www.yelp.com/biz/the-brisket-house-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=WYlfXHB2UshG86YQDS-QxQ", "review_count": 698, "categories": [{"alias": "bbq", "title": "Barbeque"}, {"alias": "southern", "title": "Southern"}], "rating": 4.0, "coordinates": {"latitude": 29.7601024211866, "longitude": -95.482542514801}, "transactions": ["delivery", "pickup"], "price": "$$", "location": {"address1": "5775 Woodway", "address2": "", "address3": "", "city": "Houston", "zip_code": "77057", "country": "US", "state": "TX", "display_address": ["5775 Woodway", "Houston, TX 77057"]}, "phone": "+12818880331", "display_phone": "(281) 888-0331", "distance": 11837.613865770443}, {"id": "RoU8KznQMrRFc



```python
response_dict = response.json()
response_dict.keys()
```




    dict_keys(['businesses', 'total', 'region'])




```python
df = pd.DataFrame(response_dict['businesses'])
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
      <td>mStkEcVeXv1HXlGqoYQx2A</td>
      <td>the-brisket-house-houston</td>
      <td>The Brisket House</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/TYH3OE...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
      <td>698</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}, {'alia...</td>
      <td>4.0</td>
      <td>{'latitude': 29.7601024211866, 'longitude': -9...</td>
      <td>[delivery, pickup]</td>
      <td>$$</td>
      <td>{'address1': '5775 Woodway', 'address2': '', '...</td>
      <td>+12818880331</td>
      <td>(281) 888-0331</td>
      <td>11837.613866</td>
    </tr>
    <tr>
      <td>1</td>
      <td>RoU8KznQMrRFcIjYVtdZMw</td>
      <td>the-pit-room-houston</td>
      <td>The Pit Room</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/kOrAG3...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/the-pit-room-houston?...</td>
      <td>1589</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.7342352703345, 'longitude': -9...</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '1201 Richmond Ave', 'address2': ...</td>
      <td>+12818881929</td>
      <td>(281) 888-1929</td>
      <td>4585.949511</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1Xg9YJ6EcFKYdWCLyWwn1A</td>
      <td>killens-bbq-pearland-2</td>
      <td>Killen's BBQ</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/4Frmor...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/killens-bbq-pearland-...</td>
      <td>2137</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.5640332, 'longitude': -95.2822...</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '3613 E Broadway', 'address2': ''...</td>
      <td>+12814852272</td>
      <td>(281) 485-2272</td>
      <td>23365.076039</td>
    </tr>
    <tr>
      <td>3</td>
      <td>i3VjO0i060Lo2T0gICjwkg</td>
      <td>rays-bbq-shack-houston-4</td>
      <td>Ray's BBQ Shack</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/tye5Jt...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/rays-bbq-shack-housto...</td>
      <td>652</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}, {'alia...</td>
      <td>4.5</td>
      <td>{'latitude': 29.7026499, 'longitude': -95.3567...</td>
      <td>[pickup]</td>
      <td>$$</td>
      <td>{'address1': '3929 Old Spanish Trl', 'address2...</td>
      <td>+17137484227</td>
      <td>(713) 748-4227</td>
      <td>6693.235867</td>
    </tr>
    <tr>
      <td>4</td>
      <td>x9N8KDSNTkvi2arF0B0BNg</td>
      <td>pinkertons-barbecue-houston-2</td>
      <td>Pinkerton's Barbecue</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/Ri2xtP...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/pinkertons-barbecue-h...</td>
      <td>618</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.79868, 'longitude': -95.3815}</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '1504 Airline Dr', 'address2': ''...</td>
      <td>+17138022000</td>
      <td>(713) 802-2000</td>
      <td>4475.408969</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['actual_url'] = df["url"].apply(lambda s: s.split("?")[0])
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
      <th>actual_url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>mStkEcVeXv1HXlGqoYQx2A</td>
      <td>the-brisket-house-houston</td>
      <td>The Brisket House</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/TYH3OE...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
      <td>698</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}, {'alia...</td>
      <td>4.0</td>
      <td>{'latitude': 29.7601024211866, 'longitude': -9...</td>
      <td>[delivery, pickup]</td>
      <td>$$</td>
      <td>{'address1': '5775 Woodway', 'address2': '', '...</td>
      <td>+12818880331</td>
      <td>(281) 888-0331</td>
      <td>11837.613866</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>RoU8KznQMrRFcIjYVtdZMw</td>
      <td>the-pit-room-houston</td>
      <td>The Pit Room</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/kOrAG3...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/the-pit-room-houston?...</td>
      <td>1589</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.7342352703345, 'longitude': -9...</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '1201 Richmond Ave', 'address2': ...</td>
      <td>+12818881929</td>
      <td>(281) 888-1929</td>
      <td>4585.949511</td>
      <td>https://www.yelp.com/biz/the-pit-room-houston</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1Xg9YJ6EcFKYdWCLyWwn1A</td>
      <td>killens-bbq-pearland-2</td>
      <td>Killen's BBQ</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/4Frmor...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/killens-bbq-pearland-...</td>
      <td>2137</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.5640332, 'longitude': -95.2822...</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '3613 E Broadway', 'address2': ''...</td>
      <td>+12814852272</td>
      <td>(281) 485-2272</td>
      <td>23365.076039</td>
      <td>https://www.yelp.com/biz/killens-bbq-pearland-2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>i3VjO0i060Lo2T0gICjwkg</td>
      <td>rays-bbq-shack-houston-4</td>
      <td>Ray's BBQ Shack</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/tye5Jt...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/rays-bbq-shack-housto...</td>
      <td>652</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}, {'alia...</td>
      <td>4.5</td>
      <td>{'latitude': 29.7026499, 'longitude': -95.3567...</td>
      <td>[pickup]</td>
      <td>$$</td>
      <td>{'address1': '3929 Old Spanish Trl', 'address2...</td>
      <td>+17137484227</td>
      <td>(713) 748-4227</td>
      <td>6693.235867</td>
      <td>https://www.yelp.com/biz/rays-bbq-shack-houston-4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>x9N8KDSNTkvi2arF0B0BNg</td>
      <td>pinkertons-barbecue-houston-2</td>
      <td>Pinkerton's Barbecue</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/Ri2xtP...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/pinkertons-barbecue-h...</td>
      <td>618</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.79868, 'longitude': -95.3815}</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '1504 Airline Dr', 'address2': ''...</td>
      <td>+17138022000</td>
      <td>(713) 802-2000</td>
      <td>4475.408969</td>
      <td>https://www.yelp.com/biz/pinkertons-barbecue-h...</td>
    </tr>
  </tbody>
</table>
</div>



### let's make a reviews table based on a single business


```python
term = 'brisket'
location = 'TX'

url = f'https://api.yelp.com/v3/businesses/{df["id"][0]}/reviews'

headers = {
        'Authorization': f'Bearer {api_key}',
    }

url
```




    'https://api.yelp.com/v3/businesses/mStkEcVeXv1HXlGqoYQx2A/reviews'




```python
response_rev0 = requests.get(url, headers=headers) #Your code here
print(response)
print(type(response.text))
print(response.text[:1000])
```

    <Response [200]>
    <class 'str'>
    {"businesses": [{"id": "mStkEcVeXv1HXlGqoYQx2A", "alias": "the-brisket-house-houston", "name": "The Brisket House", "image_url": "https://s3-media4.fl.yelpcdn.com/bphoto/TYH3OEZ0_n6606_vpYsnRA/o.jpg", "is_closed": false, "url": "https://www.yelp.com/biz/the-brisket-house-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=WYlfXHB2UshG86YQDS-QxQ", "review_count": 698, "categories": [{"alias": "bbq", "title": "Barbeque"}, {"alias": "southern", "title": "Southern"}], "rating": 4.0, "coordinates": {"latitude": 29.7601024211866, "longitude": -95.482542514801}, "transactions": ["delivery", "pickup"], "price": "$$", "location": {"address1": "5775 Woodway", "address2": "", "address3": "", "city": "Houston", "zip_code": "77057", "country": "US", "state": "TX", "display_address": ["5775 Woodway", "Houston, TX 77057"]}, "phone": "+12818880331", "display_phone": "(281) 888-0331", "distance": 11837.613865770443}, {"id": "RoU8KznQMrRFc



```python
response_rev0.json().keys()
```




    dict_keys(['reviews', 'total', 'possible_languages'])



### Let's load the reviews into a dataframe


```python
df_rev = pd.DataFrame(response_rev0.json()['reviews'])
df_rev.head()
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
      <th>url</th>
      <th>text</th>
      <th>rating</th>
      <th>time_created</th>
      <th>user</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>f6oO0EtVW80PNV5K8rnHcw</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
      <td>Not enough really good things to say about thi...</td>
      <td>5</td>
      <td>2019-07-15 13:05:23</td>
      <td>{'id': 'paxMReqiDkPtT-ih_HrvqQ', 'profile_url'...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>BVv-vyRdtXJTzmc4io5uOA</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
      <td>Customer service is pretty good at this place,...</td>
      <td>3</td>
      <td>2019-09-21 17:26:59</td>
      <td>{'id': 'j9hAimAcmK-S3456AkI3oA', 'profile_url'...</td>
    </tr>
    <tr>
      <td>2</td>
      <td>JLUIJxqeq2g7pYeqHaDXUQ</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
      <td>I ordered the plate with two meats, turkey  an...</td>
      <td>4</td>
      <td>2019-08-16 07:49:45</td>
      <td>{'id': 'LIFHSTtpiF7H2Fs12QbiFg', 'profile_url'...</td>
    </tr>
  </tbody>
</table>
</div>



### Let's get all the reviews for the restaurants we have from above


```python
df["id"][:10]
```




    0    mStkEcVeXv1HXlGqoYQx2A
    1    RoU8KznQMrRFcIjYVtdZMw
    2    1Xg9YJ6EcFKYdWCLyWwn1A
    3    i3VjO0i060Lo2T0gICjwkg
    4    x9N8KDSNTkvi2arF0B0BNg
    5    Rx7JP3hLKI5N9Pdra6J-Nw
    6    wHIR-YyRFTslRh9anDIQ2w
    7    UUHxvbYpSzmuYpHsvDuDhw
    8    Gwr0TRqSeODo1nlCzeDrCw
    9    _CADk-UMmJpDAV3plf9ZxA
    Name: id, dtype: object




```python
import time

master_review_list = []
headers = {'Authorization': f'Bearer {api_key}',}

for id_ in df["id"][:10]:
    
    url = f'https://api.yelp.com/v3/businesses/{id_}/reviews'


    response_rev = requests.get(url, headers=headers) 
    
    review_list = response_rev.json()['reviews']
    print(response_rev.json()['reviews'])
    print(len(response_rev.json()['reviews']))
    
    master_review_list.extend(review_list)
    print(len(master_review_list))
    time.sleep(10)
```

    [{'id': 'f6oO0EtVW80PNV5K8rnHcw', 'url': 'https://www.yelp.com/biz/the-brisket-house-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=f6oO0EtVW80PNV5K8rnHcw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': "Not enough really good things to say about this place. I love that everyone is so friendly, it's clean but best of all....it's delicious!\n\nIf you can stand...", 'rating': 5, 'time_created': '2019-07-15 13:05:23', 'user': {'id': 'paxMReqiDkPtT-ih_HrvqQ', 'profile_url': 'https://www.yelp.com/user_details?userid=paxMReqiDkPtT-ih_HrvqQ', 'image_url': 'https://s3-media2.fl.yelpcdn.com/photo/uW6e5SInLD4vy1xb3TyELg/o.jpg', 'name': 'Linh P.'}}, {'id': 'BVv-vyRdtXJTzmc4io5uOA', 'url': 'https://www.yelp.com/biz/the-brisket-house-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=BVv-vyRdtXJTzmc4io5uOA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': "Customer service is pretty good at this place, one of the first things I noticed, aside from the super smokey stench. It smells good, don't get me wrong,...", 'rating': 3, 'time_created': '2019-09-21 17:26:59', 'user': {'id': 'j9hAimAcmK-S3456AkI3oA', 'profile_url': 'https://www.yelp.com/user_details?userid=j9hAimAcmK-S3456AkI3oA', 'image_url': 'https://s3-media2.fl.yelpcdn.com/photo/uamR8OEzXhqntzVWOdl2NQ/o.jpg', 'name': 'Shana H.'}}, {'id': 'JLUIJxqeq2g7pYeqHaDXUQ', 'url': 'https://www.yelp.com/biz/the-brisket-house-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=JLUIJxqeq2g7pYeqHaDXUQ&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': "I ordered the plate with two meats, turkey  and lean brisket   which came with two sides! The turkey wasn't really dry so I was a big fan of it and the...", 'rating': 4, 'time_created': '2019-08-16 07:49:45', 'user': {'id': 'LIFHSTtpiF7H2Fs12QbiFg', 'profile_url': 'https://www.yelp.com/user_details?userid=LIFHSTtpiF7H2Fs12QbiFg', 'image_url': 'https://s3-media3.fl.yelpcdn.com/photo/5u0ybRJJZemn_D47yApAow/o.jpg', 'name': 'Adel Q.'}}]
    3
    3
    [{'id': 'u7Y3Jxi8d2cLrsYEPX--RQ', 'url': 'https://www.yelp.com/biz/the-pit-room-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=u7Y3Jxi8d2cLrsYEPX--RQ&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': 'This place knocked our socks off! Our crew ordered 2 pulled pork sandwiches, a 1/2 lb. of brisket, and the housemade the garlic-pepper venison sausage and...', 'rating': 5, 'time_created': '2019-09-21 13:38:38', 'user': {'id': 'qkLAPqDL9nv3xLkwAJJW3w', 'profile_url': 'https://www.yelp.com/user_details?userid=qkLAPqDL9nv3xLkwAJJW3w', 'image_url': 'https://s3-media1.fl.yelpcdn.com/photo/Km66mtmcaKbHXB-G-pcseg/o.jpg', 'name': 'Lora W.'}}, {'id': 'uDb0a19LcgJWShr9NsU8qg', 'url': 'https://www.yelp.com/biz/the-pit-room-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=uDb0a19LcgJWShr9NsU8qg&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': 'My bf and I came Sunday night 8 pm and the only meats they had left were the beef brisket and pulled pork so we ordered a two meat dish to share. As for...', 'rating': 3, 'time_created': '2019-10-07 10:23:10', 'user': {'id': 'iA8ryDbfpgHP3lZiNTjZ2g', 'profile_url': 'https://www.yelp.com/user_details?userid=iA8ryDbfpgHP3lZiNTjZ2g', 'image_url': 'https://s3-media2.fl.yelpcdn.com/photo/N9MhxEursl_7GJPE2X8p6g/o.jpg', 'name': 'Cassie X.'}}, {'id': 'F8nUhnfRvG-shVnu-2qopw', 'url': 'https://www.yelp.com/biz/the-pit-room-houston?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=F8nUhnfRvG-shVnu-2qopw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': "Typically would bring people all the way out to Pearland for Killeen's bbq but if you don't want to wait in and drive that far out and wait in line, this is...", 'rating': 4, 'time_created': '2019-09-18 22:05:03', 'user': {'id': 'EAEIqGUYiTIrr4xmzspBfQ', 'profile_url': 'https://www.yelp.com/user_details?userid=EAEIqGUYiTIrr4xmzspBfQ', 'image_url': 'https://s3-media1.fl.yelpcdn.com/photo/JNCQqgmQ9VrQ1QM5qpbjpA/o.jpg', 'name': 'Kate G.'}}]
    3
    6
    [{'id': 'OxtcyBcoNaR3EqNz_bgfEQ', 'url': 'https://www.yelp.com/biz/killens-bbq-pearland-2?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=OxtcyBcoNaR3EqNz_bgfEQ&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': "5 star alert! I remember hearing the buzz about Killen's years back. Never made it to Pearland. Finally made it today and I am very happy did! \n\nVery rarely...", 'rating': 5, 'time_created': '2019-10-01 17:06:52', 'user': {'id': '_4SoSOIMyv1Qwtnm6JyZug', 'profile_url': 'https://www.yelp.com/user_details?userid=_4SoSOIMyv1Qwtnm6JyZug', 'image_url': 'https://s3-media3.fl.yelpcdn.com/photo/qDU_t2NxEQO0brzniV3srA/o.jpg', 'name': 'Rex C.'}}, {'id': 'SV9jCZwY9R41A1PWd2TQ8g', 'url': 'https://www.yelp.com/biz/killens-bbq-pearland-2?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=SV9jCZwY9R41A1PWd2TQ8g&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': 'Traditional upper tier American BBQ \n\nFood here is amazing, tender, seasoned, smoked and even tastes good cold. I generally get the beef ribs, and bread...', 'rating': 5, 'time_created': '2019-09-10 13:39:49', 'user': {'id': 'Yg04BiLM_F0yGFhO0xsCYA', 'profile_url': 'https://www.yelp.com/user_details?userid=Yg04BiLM_F0yGFhO0xsCYA', 'image_url': 'https://s3-media2.fl.yelpcdn.com/photo/ccjKYgAlGtW1aDPpWGzMtg/o.jpg', 'name': 'Jason P.'}}, {'id': 'aXLgzTflrCM4p7uSlE7uOg', 'url': 'https://www.yelp.com/biz/killens-bbq-pearland-2?adjust_creative=WYlfXHB2UshG86YQDS-QxQ&hrid=aXLgzTflrCM4p7uSlE7uOg&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=WYlfXHB2UshG86YQDS-QxQ', 'text': 'Was in Houston for a wedding one weekend and wanted to try something that would define the city.  Also, I wanted to pick up some food to bring back to the...', 'rating': 5, 'time_created': '2019-09-08 21:35:48', 'user': {'id': '9jwBAt8PlP4j9-M9KEC_RQ', 'profile_url': 'https://www.yelp.com/user_details?userid=9jwBAt8PlP4j9-M9KEC_RQ', 'image_url': 'https://s3-media1.fl.yelpcdn.com/photo/xCUELDYefycdfUuMb7n-IA/o.jpg', 'name': 'Eric D.'}}]
    3
    9



    ------------------------------------------------------------------------

    KeyboardInterrupt                      Traceback (most recent call last)

    <ipython-input-70-81fce4ec23f8> in <module>
         17     master_review_list.extend(review_list)
         18     print(len(master_review_list))
    ---> 19     time.sleep(10)
    

    KeyboardInterrupt: 



```python
df_reviews_master = pd.DataFrame(master_review_list)
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
      <th>actual_url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>mStkEcVeXv1HXlGqoYQx2A</td>
      <td>the-brisket-house-houston</td>
      <td>The Brisket House</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/TYH3OE...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
      <td>698</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}, {'alia...</td>
      <td>4.0</td>
      <td>{'latitude': 29.7601024211866, 'longitude': -9...</td>
      <td>[delivery, pickup]</td>
      <td>$$</td>
      <td>{'address1': '5775 Woodway', 'address2': '', '...</td>
      <td>+12818880331</td>
      <td>(281) 888-0331</td>
      <td>11837.613866</td>
      <td>https://www.yelp.com/biz/the-brisket-house-hou...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>RoU8KznQMrRFcIjYVtdZMw</td>
      <td>the-pit-room-houston</td>
      <td>The Pit Room</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/kOrAG3...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/the-pit-room-houston?...</td>
      <td>1589</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.7342352703345, 'longitude': -9...</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '1201 Richmond Ave', 'address2': ...</td>
      <td>+12818881929</td>
      <td>(281) 888-1929</td>
      <td>4585.949511</td>
      <td>https://www.yelp.com/biz/the-pit-room-houston</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1Xg9YJ6EcFKYdWCLyWwn1A</td>
      <td>killens-bbq-pearland-2</td>
      <td>Killen's BBQ</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/4Frmor...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/killens-bbq-pearland-...</td>
      <td>2137</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.5640332, 'longitude': -95.2822...</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '3613 E Broadway', 'address2': ''...</td>
      <td>+12814852272</td>
      <td>(281) 485-2272</td>
      <td>23365.076039</td>
      <td>https://www.yelp.com/biz/killens-bbq-pearland-2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>i3VjO0i060Lo2T0gICjwkg</td>
      <td>rays-bbq-shack-houston-4</td>
      <td>Ray's BBQ Shack</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/tye5Jt...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/rays-bbq-shack-housto...</td>
      <td>652</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}, {'alia...</td>
      <td>4.5</td>
      <td>{'latitude': 29.7026499, 'longitude': -95.3567...</td>
      <td>[pickup]</td>
      <td>$$</td>
      <td>{'address1': '3929 Old Spanish Trl', 'address2...</td>
      <td>+17137484227</td>
      <td>(713) 748-4227</td>
      <td>6693.235867</td>
      <td>https://www.yelp.com/biz/rays-bbq-shack-houston-4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>x9N8KDSNTkvi2arF0B0BNg</td>
      <td>pinkertons-barbecue-houston-2</td>
      <td>Pinkerton's Barbecue</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/Ri2xtP...</td>
      <td>False</td>
      <td>https://www.yelp.com/biz/pinkertons-barbecue-h...</td>
      <td>618</td>
      <td>[{'alias': 'bbq', 'title': 'Barbeque'}]</td>
      <td>4.5</td>
      <td>{'latitude': 29.79868, 'longitude': -95.3815}</td>
      <td>[]</td>
      <td>$$</td>
      <td>{'address1': '1504 Airline Dr', 'address2': ''...</td>
      <td>+17138022000</td>
      <td>(713) 802-2000</td>
      <td>4475.408969</td>
      <td>https://www.yelp.com/biz/pinkertons-barbecue-h...</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.alias.value_counts()
```




    ikes-love-and-sandwiches-houston             1
    triple-js-smokehouse-houston                 1
    the-brisket-house-houston-2                  1
    goode-company-barbeque-houston               1
    blood-bros-bbq-bellaire-4                    1
    corkscrew-bbq-spring-2                       1
    jimees-bbq-houston                           1
    gatlins-bbq-houston-4                        1
    killens-bbq-pearland-2                       1
    rudys-country-store-and-bar-b-q-houston-3    1
    pinkertons-barbecue-houston-2                1
    brookstreet-bar-b-que-houston-2              1
    the-brisket-house-houston                    1
    rays-bbq-shack-houston-4                     1
    jackson-street-bbq-houston                   1
    el-burro-and-the-bull-houston                1
    the-pit-room-houston                         1
    burns-original-bbq-houston                   1
    victorians-barbecue-houston                  1
    stockyard-bar-b-q-houston-3                  1
    Name: alias, dtype: int64




```python

```
