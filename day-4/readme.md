# Day 4 - Active reconnaissance & Network scanning

## [Over The Wire - Natas](https://overthewire.org/wargames/natas/)

### Level 0
- Username: natas0
- URL: http://natas0.natas.labs.overthewire.org
- Solution: The password for the next level is in the source code of the webpage.
- Password for natas1: natas0

### Level 1
- Username: natas1
- Solution: View the page source to find the password for the next level.
- Url: http://natas1.natas.labs.overthewire.org
- Password for natas2: ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi 

### Level 2
- Username: natas2
- Solution: The solution is hidden in /files folder. You can access it by going to http://natas2.natas.labs.overthewire.org/files
- Url: http://natas2.natas.labs.overthewire.org
- Password for natas3: sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14

### Level 3
- Username: natas3
- Checked robots.txt file to find the password for the next level. Than entered the password in the URL to access the next level.
- Url: http://natas3.natas.labs.overthewire.org
- Password for natas4: Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ

### Level 4
- Username: natas4
- Solution: Opened Burpsuite and added ```referer``` header with value ```http://natas5.natas.labs.overthewire.org``` to the request and sent it to the server. The password for the next level is in the response.
- Url: http://natas4.natas.labs.overthewire.org
- Password for natas5: iX6IOfmpN7AYOQGPwtn3fXpbaJVJcHfq

### Level 5
- Username: natas5
- Solution: Found the password using burpsuite ```cookie: loggedin=0``` and changed it to ```cookie: loggedin=1``` to access the next level.
- Url: http://natas5.natas.labs.overthewire.org
- Password for natas6: aGoY4q2Dc6MgDq4oL4YtoKtyAg9PeHa1