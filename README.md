![Lastify](/images/lastify_logo.png)
>*A novel approach towards personalized playlist generation based on a user's preferences and listening history.*

# Intro
I first started using [Last.fm](https://last.fm) over nine years ago, as I was interested in keeping track of my music listening history. While I now faintly recall the red “a” appearing in the menu bar, I remember using it to journal my iTunes plays in a process called “scrobbling.”  Fast forward nine years, to shortly after passing the 100,000 scrobble mark, I realized I had a large set of personalized listening data that I was interested in analyzing and exploring. Analyzing to get some general statistics as to my most listened to artists ([Flying Lotus](https://www.youtube.com/watch?v=yK-VuOn1rgs)), and tracks ([DDD by Machinedrum](https://www.youtube.com/watch?v=A2O-Ggo7yoo)), amongst other musical superlatives. And exploring to see what patterns emerged, and if said patterns could help me learn about myself in new ways.

![Figure 1](/images/Figure1.png)
![Figure 2](/images/Figure2.png)

After pouring the initial dataset into Tableau and generating some pretty diagrams (see Fig. 1-2 above), I started to recognize the potential of this data set. I realized that I could use this information to analyze my listening habits, and in turn predict what type of music I would like to listen to at any given time. I decided to combine this data with [Spotify’s Web API](https://developer.spotify.com/) to extract audio features from the songs I have listened to, and determine which audio features were most important at a given time, and then construct playlists of songs based on the parameters of said features.
# Getting started (Not Really 100k Scrobbles)
Seeing that I was well past the 100k scrobble mark, my first question was how do I access this data? A quick google search directed me to [pylast](https://github.com/pylast/pylast), a python package that interfaces with Last.fm. I set up my Last.fm [API account](https://www.last.fm/api), and started exploring what was available to me. The `get_recent_tracks` function returns a list of tracks played in sequential order from the specified start and stop dates. Setting the limit to None, and having the start and end points set to None returns a list of all tracks played. Approximately 4,000 scrobbles had no timestamp (timestamp = 0), which were discarded, leaving me with around 85,000 scrobbles to work with. This remaining list of scrobbles contains the artist, track name, album name, and [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time). This data is then fed into a PostgreSQL table labeled `lastFM`.

**Note**: There is about a 20,000 scrobble difference between what I can see using the API (~90,000), and the number of scrobblems associated with [my account](https://www.last.fm/user/dmatica) (109,000). This has been observed [elsewhere](https://getsatisfaction.com/lastfm/topics/scrobbles-count-in-library-differs-from-scrobbles-count-in-overview), and is speculated to be attributed to iTunes uploading duplicate scrobbles.

# Filtering and retrieving audio features
While Last.fm informs you when you initially start the track, it does not inform you when the track is stopped or finished. This means that partial plays, even when the track is started over after a couple of seconds counts as multiple listens. To account for these, I set a 30 second filter, where I removed tracks whose play length before the next track played was less than 30 seconds, although a more precise filter could undoubtedly be used.

![Figure 3](/images/Figure3.png)
>*Figure 3: Track filtering steps*

Once the tracks in the table have been filtered for presumed play length, a separate table was made, containing only unique tracks from this list.  Using Spotify’s API, a search was performed to identify songs in Spotify’s database by artist and track name, and a Spotify ID was appended to the table.  From there, the tracks with a Spotify ID were checked for audio features using the `audio_features` function.

Spotify performs a proprietary [audio analysis](https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-features/) on the tracks, identifying certain features. This includes track length, tempo, key, mode, time signature, loudness, acousticness (likelihood track is acoustic), danceability (how danceable the track is), energy (general gauge of track loudness and speed), instrumentalness (whether or not the track is instrumental only), liveness (if there is an audience presence on the track), speechiness (determines if the track is spoken word), and valence (conveyance of musical positiveness).
