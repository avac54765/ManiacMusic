# Concert API, table, and search here

use api from here: https://rapidapi.com/seatgeek/api/seatgeek/


import requests

url = 'https://api.seatgeek.com/2/events'
params = {
    'client_id': 'MzM3NjkyNzh8MTY4NDgxODg3Mi45OTMyOTk1',
    'client_secret': 'bb0a4e78293d02ac50573254f61e3fd487680ca5678a8c58d1d7656ed5bff1f8'
}

response = requests.get(url, params=params)

# Check the response status code
if response.status_code == 200:
    data = response.json()
    # Process the returned data
    # ...
else:
    print('Error:', response.status_code)