# Instructions
## Requirements
In order to run Lastify, the following four requirements are necessary:
1. [Python 3.6+](https://www.python.org/downloads/)
2. [PostgreSQL 10+](https://www.postgresql.org/download/)
3. A Last.fm [API account](https://www.last.fm/api/account/create)
4. A Spotify [developer account](https://developer.spotify.com/)
## Getting started
As you'll see in the python notebook, Lastify is broken into fourteen steps, with each step contained in separate cells.  The first two cells are for installing and importing various libraries.  The following dependencies need to be installed to operate Lastify:
1. [pytz](http://pytz.sourceforge.net/) - Required to convert Unix timestamp into timezone
2. [pylast](https://github.com/pylast/pylast) - Library to interface with the Last.fm API
3. [pandas](https://pandas.pydata.org/) - Used for data handling
4. [psycopg2](http://initd.org/psycopg/) - PostgreSQL controller for python for storing and recalling scrobble history and audio features
5. [spotipy](https://spotipy.readthedocs.io/en/latest/) - Library to interface with Spotify's API
6. [sklearn](https://scikit-learn.org/stable/) - Used for logistic regression modeling on data

Lastify uses PostgreSQL to store scrobble history and the compilation of audio features. This allows for easy recall of data without having to rerun Lastify to get information. **Step 3** is the `DatabaseConnection` class used to allow Lastify to interface with a PostgreSQL database. In the initializing step, the PostgreSQL credentials need to be input in order to access the PostgreSQL database. This includes the database name, database user and password, the PostgreSQL hosting address as well as the server port.

**Step 4** is for inputting the necessary credentials to access both the Last.fm and Spotify APIs. When you create a Last.fm API account, you are given an API key and an API secret. Do not lose track of these. Input these values in Step 4, along with your username and password. For Spotify's API, I am currently implenting two separate applications, the first for acquisition of the various audio features, and the second for generating playlists. Add these Client IDs and Client Secrets as well as your Spotify username.

**Step 5** is to acquire the entirety of your scrobble history.  Based on how many scrobbles are associated with your account, this step can take around 10-15 minutes to complete. For each scrobble in the scrobble history, we are able to see the track, track artist, track album, and Unix timestamp for when the track was played. To set the `start_time` use the date your Last.fm account was created and input that date into an [epoch converter](https://www.epochconverter.com/) to get the timestamp to use as the start time.

**Step 6** then records your scrobble history to a PostgreSQL table named `lastFM` in chronological order. **Step 7** is located in the same cell as Step 6, and is used to filter out tracks played for less than 30 seconds.

**Step 8** makes a new table called `audio_features` of just the unique tracks, and **Step 9** goes through each track to see which tracks are available in Spotify's library, removing tracks not on Spotify, and **Step 10** is an additional check to ensure only tracks on Spotify remain in the table.

**Step 11** acquires the audio features for all of the tracks in the `audio_features` table. This step takes on the order of hours to run to completion, so be aware. Future development of Lastify aims to reduce this time dramatically.

**Step 12** takes the timestamps we have for the scrobbles, and converts that into time-zone specific information, including hour-of-day, the day-of-week, month, and year.

**Step 13** requires the user to input the day-of-week and time-of-day, and based on tracks in Spotify's library that are played on that day assigns a binary classifier to identify tracks played at the specific time input. A logistic regression is then run to identify the audio features most important for listening, and for the four most important features assigns the first and third quartiles as the min and max values for those features, and the median as the target value. 

**Step 14** is the final step, and starts with genre selection. The genre of choice along with the features and their values are input into Spotify's recommendation function in its API, and are used to come up with a list of 10 tracks that fit the selection criteria (based on feature values). Input a name for a playlist, and the playlist of tracks can then be saved to your Spotify account.
