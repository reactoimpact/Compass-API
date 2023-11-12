# using the Compass API (if you use SSO)
I belive that the session key warrants other actions such as viewing your profile, learning tasks, reports etc. 

## Walkthrough

### Sign into compass via the web. You can use SSO or username/password to sign in.

![blank-page](https://github.com/reactoimpact/CompassCal2/assets/122259384/d822944a-ba9e-4b48-a0f5-c6c7a79aa2da)

### Once the webpage is loaded, use Inspect element to view the webpage html. Go to the Storage tab and coppy the ASP.NET_SessionId. Keep this key secret and *don't share it with anyone*! This allows whoever has acess to the key to log into your compass account and _steal personal information_.

![session string](https://github.com/reactoimpact/CompassCal2/assets/122259384/5fdaaa12-66ee-429e-8856-a22259ba40d0)

### Once you have the session id, create a new python file in your editor of choice (I use VS code) and copy&paste the code bellow ( put the session key and user id in the correct spots aswell as your abriviated schools name):

To get the userId, go to [SyncMySchedule](https://phs-vic.compass.education/Communicate/ManageCalendars.aspx) ( gear icon top right in compass homepage ).
Then find the url used to sync the schedule, there should be a parameter called "uid" and a string of digit value in the url (4 or 3). The digits are your userId.

```py
import requests

url = 'https://YOURSCHOOL-vic.compass.education/Services/Calendar.svc/GetCalendarEventsByUser?sessionstate=readonly'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/114.0',
    'Content-Type': 'application/json',
}

# put your user id in the "userId" value
# you can change the date of the callander view here
data = {
    "userId":YOUR_USER_ID_HERE,
    "startDate":"2023-09-13",
    "endDate":"2023-09-13",
    "start":0,
}

# put your session key here
cookies = {
    'ASP.NET_SessionId': 'YOUR_KEY_GOES_HERE'
}

response = requests.post(url, headers=headers, cookies=cookies, json=data)

print(response.status_code)

for session in response.json()['d']:
    print(session['longTitle'])
    print("")
```

If the session key is correct, running the python program should show the events on that specific day ( If there are none, switch to a day were there are ones ).
You can view all the data by adding this:
```py
print(response.json())
```

I hope this guide has helped show you how to interface the callender through the compass api.









