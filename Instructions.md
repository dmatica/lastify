# Instructions
## Requirements
In order to run Lastify, the following four requirements are necessary:
1. [Python 3.6+](https://www.python.org/downloads/)
2. [PostgreSQL 10+](https://www.postgresql.org/download/)
3. A Last.fm [API account](https://www.last.fm/api/account/create)
4. A Spotify [developer account](https://developer.spotify.com/)
## Getting started
As you'll see in the python notebook, Lastify is broken into fourteen steps, with each step contained in separate cells.  The first cell is for installing dependencies.  The following dependencies are required to operate Lastify:
1. [pytz](http://pytz.sourceforge.net/) - Required to convert Unix timestamp into timezone
2. [pylast](https://github.com/pylast/pylast) - Library to interface with the Last.fm API
3. [pandas](https://pandas.pydata.org/) - Used for data handling
4. [psycopg2](http://initd.org/psycopg/) - PostgreSQL controller for python for storing and recalling scrobble history and audio features
5. [spotipy](https://spotipy.readthedocs.io/en/latest/) - Library to interface with Spotify's API
6. [sklearn](https://scikit-learn.org/stable/) - Used for logistic regression modeling on data
