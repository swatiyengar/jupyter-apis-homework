# Assignment Summary
**"Homework 04: Jupyter, APIs, git and GitHub"** was a three-part assignment:
    Part 1: Answer questions using the WeatherAPI
        - The questions and related code to answer the questions can be found in "*WeatherAPI API (Weather).ipynb*" in this repo
    Part 2: Answer questions LastFM's API
        - The questions and related code to answer the questions can be found in "*Last FM API (Music).ipynb*" in this repo
    Part 3: Uploading the files to this GitHub repository
 
# What I Learned During the Process

## String "automation"
 - I discovered how to use a user's input value to automatically create urls for API endpoint requests. For instance, in the code below, defining api_key and location based on the user's input and using those variables inside an f-string to define the weather endpoint - the code becomes more flexible.
 
 'code'
api_key = input("What is your WeatherAPI.com API key? Don't have one? Sign-up here: https://www.weatherapi.com/")
api_key = str(api_key)
location = input('What city are you interested in?')
location = str(location)
current_weather_url = f"https://api.weatherapi.com/v1/current.json?key={api_key}&q={location}&aqi=no"
 'code'
 
## For Loops
 - I learned more about defining variables. For instance, in the code below, I initially did not define hottest_day prior to running the for loop. Thus, the maximum temperature was linked to the last day of the forecast, not the actual hottest day.
 
Incorrect:
`code`
 max_temp = -200
 for days in forecastday:
    day_max_temp = days['day']['maxtemp_f']
    print(f"On {days['date']} the max temperature is {day_max_temp} degrees F.")
    if day_max_temp > max_temp:
        max_temp = day_max_temp
        hottest_day = days['date']
print(f"{hottest_day} is the hottest day with a max temperature of {max_temp} degrees F.") 
`code`
---
Correct:
`code`
 max_temp = -200
 hottest_day = ""
 for days in forecastday:
    day_max_temp = days['day']['maxtemp_f']
    print(f"On {days['date']} the max temperature is {day_max_temp} degrees F.")
    if day_max_temp > max_temp:
        max_temp = day_max_temp
        hottest_day = days['date']
print(f"{hottest_day} is the hottest day with a max temperature of {max_temp} degrees F.") 
`code`

- Additionally, in an earlier version of this code, I placed the print statement inside the 'if' block which resulted in every day being called the hottest day during a heat wave.

- I also learned how to check if an element is in a list of elements

# Challenges

## Jupyter Notebooks

- Having become accustomed to VSCode and the process to right-click code snippets to test in an interactive terminal, I found Juypter Notebooks a bit...irritating. I would often accidentally execute code out of order, so code would result in errors unnecessarily. 
- I found myself copying code from the notebook into a regular python file to experiment and de-bug and then re-copying the functioonal code into the Notebook; 
    
## Weather API Homework
- The last question kept resulting in a 401 error - meaning that my API key doesn't allow queries for any historial data. However, the weatherapi.com documentation insists that free accounts should still be able to query historial data after 1 Jan 2010.

## LastFM Homework
- Q12: Using the first ten mbids, print the artist's name and whether they're a rapper
        - overwriting data in for loops:
            - Incorrect Code
             `code`
for url_requests in mbid_urls[:10]:
        url_request = requests.get(url_requests)
        url_request = url_request.json()
        mbid_url_artist = url_request['artist']
        mbid_url_tags = mbid_url_artist['tags']['tag']
        for mtag in mbid_url_tags:
            tag_name = mtag['name']
            tag_name = tag_name.lower()
            print(tag_name)
            hip_hop_tags = ["hip hop","swag","crunk","rap","dirty south","memphis rap","gangsta rap","hip-hop", "trill shit"]
            if tag_name in hip_hop_tags:
                artist_tag = "hip hop" #This structure keeps overwriting artist_tag with every subsequent tag
            else:
                artist_tag = "not hip hop"
            print(f"{mbid_url_artist['name']} : {artist_tag}") #Because of that error, the artist is tagged as hip hop or not hip hop based on the LAST tag given (e.g. Lil Kim is not hip hop because her last tag is rnb. INCORRECT because hip hop is her first tag)
             `code`
---
             - Correct Code
             `code`
hip_hop_tags = ["hip hop","swag","crunk","rap","dirty south","memphis rap","gangsta rap","hip-hop", "trill shit"] #Genres that might be considered hip are defined at the beginning
for url_requests in mbid_urls[:10]:
    url_request = requests.get(url_requests)
    url_request = url_request.json()
    artist_name = url_request['artist']['name']
    mbid_url_artist = url_request['artist']
    mbid_url_tags = mbid_url_artist['tags']['tag']
    is_hiphop = False #is_hiphop variable defined before the for loop to avoid overwriting data
    for mtag in mbid_url_tags:
        tag_name = mtag['name']
        tag_name = tag_name.lower()
        print("we found",tag_name,'in',artist_name)
        if tag_name in hip_hop_tags:
            is_hiphop = True #if any of the tags have a name found in the hip_hop_tags list, the is_hiphop variables turns True for the artist
    if is_hiphop: #additional if statement to if an artist has is_hiphop == T, then a statement prints that the artist is hiphop
        print(artist_name, "is hip-hop.")
    else:
        print(artist_name, "is not hip-hop.")
    #print(f"{mbid_url_artist['name']} : {artist_tag}")
             `code`