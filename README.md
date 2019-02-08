![Lastify](/images/lastify_logo.png)
>*A novel approach towards personalized playlist generation based on a user's preferences and listening history.*

# Intro
I first started using [Last.fm](https://last.fm) over nine years ago, as I was interested in keeping track of my music listening history. While I now faintly recall the red “a” appearing in the menu bar, I remember using it to journal my iTunes plays in a process called “scrobbling.”  Fast forward nine years, to right after hitting the 100,000 scrobble mark, and I realized I had a large set of personalized listening data that I was interested in analyzing and exploring. Analyzing to get some general statistics as to my most listened to artists ([Flying Lotus](https://www.youtube.com/watch?v=yK-VuOn1rgs)), and tracks ([DDD](https://www.youtube.com/watch?v=A2O-Ggo7yoo) by Machinedrum), amongst other musical superlatives. And exploring to see what patterns emerged, and if said patterns could help me learn about myself in new ways.

![Figure 1](/images/Figure1.png)

After pouring the initial dataset into Tableau and generating some pretty diagrams (see Fig. 1 above), I started to recognize the potential of this data set. I realized that I could use this information to analyze my listening habits, and in turn predict what type of music I would like to listen to at any given time. I decided to combine this data with [Spotify’s Web API](https://developer.spotify.com/) to extract audio features from the songs I have listened to, and determine which audio features were most important at a given time, and then construct playlists of songs based on the parameters of said features.
