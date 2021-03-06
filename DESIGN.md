
# Songmapper Design! 

Going into our project, we knew it would be difficult working with an API (Spotify) and library (Leaflet.js) that we had never seen before, but the challenge was much more difficult than we had initially suspected. Regardless, we're proud of the final product we've created and had a lot of fun creating it. As a general note, we started this entire project from scratch, implementing only small parts from our final problem set in the class called "finance", and created our own database in SQL, own style with our homepage using Nicepage (https://nicepage.com/), and of course, used a unique and challenging API and javascript library. The learning curve was steep, but we learned a lot and became better programmer's because of it. Below is a detailed explanation of how our project came together, including how we created the various files and what design decisions we had to make to ensure our project came together how we wanted it to.

## Working with Spotify API

As mentioned, one of the hardest parts of creating this website was working with the Spotify Developer API. We decide to build our backend database using Flask, instead of Node.js, which is what Spotify itself uses to share information about using their API to developers. With more experience using Javascript, we would've built our backend using it, but since we assumed the learning curve would be somewhat steep and we had limited time to make our website, we decided to use Flask and stick to Python.

To build our website, we used the Spotify devlopers website to register an app using our Spotify account. This is Spotify's way of preventing unlimited use of their API and slow down traffic in their own product, and in each developer app they limit the amount of registered users to 25. So, once we registered our "Songmapper" app in the devloper database, we were able to start creating our authorization method. This is where we used another open-source repository called "Flask-Spotify-Auth" (https://github.com/vanortg/Flask-Spotify-Auth) to handle the authentication and authorization. This proved to be an incredibly tricky part of the process.

We took ideas from that code to implement in our own website. This role of this authorization code was essentially to handle a user's login with Spotify, ensure that that proper scope of the user's info are being authenticated, and provide us with certain data points called "access tokens" which are were used as unique sets of numbers, letters, and symbols that would allow our website to interact with Spotify.

The various Python files we have in our repository, flask-spotify-auth.py, setup.py, and startup.py were all implemented and built to handle this entire authentication process. The code is fully commented, so to get into the details of how each is implemented feel free to explore the code. It took a lot of trial and error to get this working, but eventually, we were able to create a flow in our website that went from our own homepage (index.html) to the Spotify authentication/login page to connect a user's account and finally back to our own website to show our map (map.html).

Another important Python function, which was inspired by another open-source repository called Vibester (https://github.com/JayA96/Vibester), is called helpers.py. For our project, we only really needed to access two main things from Spotify, which were the user's user_id and the user's saved songs library. Other methods could've been implemented to extract things like a user's playlists, or other metrics about the saved songs, like specific buckets of genres that they belong to, but those were outside of the scope of our project. Getting the user_id was a fairly straightforward task, however extracting a user's saved songs proved to be quite difficult. We learned that Spotify stores most of its information in JSON (Javascript Object Notation) files, which are massive data structures that store much more data than we expected. Indexing into these data structures was tricky, as we sought to extract three things: each song name, each artist name, and the album artwork for each song.

Once this data was collected, we stored all of the information for each song in a pandas dataframe, which would make them easy to transfer over to a SQL database (which would be used by our app.py program).

There were a few things of interest though with the API that we didn't fully resolve. Mainly, Spotify only allows developer's to access 50 songs in a saved library at any point in the developer space. This makes sense, because say if a user had 10,000 liked songs in their library, accessing and displaying all 10,000 of those songs all at once would cause issues with memory and speed. This process used by Spotify is called "pagination" and its common practice. However, we didn't implement any methods to collect more than a user's last 50 saved songs, in large part because we wanted to keep our website as quick as possible. Future version could allow for more songs to be accessed at once. To access more songs past the first 50, you can adjust line 25 in helpers.py and change the "offset" in the URL to be something other than 0. For example, changing the offset to 50 would allow access of the user's next 50 saved songs.

## Working with Leaflet.js

We originally tried using the Google Maps API to implement a mapping feature, but this proved to cause certain issues, so we scrapped that idea instead found this Javascript library to implement our mapping feature. In fact, Leaflet.js (https://leafletjs.com/) is the basis for the interactive features for our map. 

Getting a map to appear on our screen was fairly straightforward, as we simply had to follow along the Leaflet.js documentation to implement a basic map on our HTML page (map.html). Implementing the interactive features on the map were more complicated, as we needed to be able to add and view songs on our map. Here, we had the javascript, inspired from leaflet.js, on our map.html talk with our app.py page and our SQL database we created to be able to add a popup with the song name, artist name, album artwork, and latitude and longitude coordinates. After these five parameters were stored, every time we logged in as a certain user, our own personal songmap would be saved. So in short, we devloped a way to select a location on the map by clicking, sending the latitude and longitude coordinates to a SQL database, then we select the song of choice on the next page (in pin.html), then the three data points are sent to the same row in our SQL database as the latitude and longitude. After that, the page redirects back to our map, which then shows the new marker on the map at the specific coordinates, combined with its unique popup with the song information located in the popup.

Our SQL database was created as a way to save all of the markers on each user's map, as we needed a way to see multiple markers and have our website remember where all of the datapoints were. Without the database, using SQL or some other language/database structure, we wouldn't have been able to save the progress and we would not be able to adequately add points to the map and essentially do what we intended to do in creating a songmap.

## App.py

Additionally, our app.py is the main driving engine of our website's function, as it brings everything together and talks with all of  our HTML pages to execute our vision. It follows the basic format of a Flask app with its configuration, and we included a few different methods that interact with our SQL database and Spotify API to select and store songs that then appear on our map.

The syntax in each method was somewhat difficult to figure out, but eventually we were able to seamlessly implement the calls to our SQL database with the accessing of the information in the Spotify API. One note, new user's who are creating a new map, we created a dummy song on everyone's map to start off ("Blowin' in the Wind" by Bob Dylan), however this song is just to show an example of what the songs on the map should look like and will disappear once songs from the user's library are submitted to be placed on the map. 

## Final Notes

Lastly, we adapted some of the style for our website from Nicepage. Also, note our own unique Songmapper logo, inspired by the Spotify logo with its own geographic twist!

Thank you so much for exploring, learning about, and using Songmapper, we hope you enjoy it! And hopefully, future versions will come that will allow you to create even bigger and better songmaps!
