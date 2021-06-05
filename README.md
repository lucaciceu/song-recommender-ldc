# song-recommender-ldc
Song Recommender

Synopsis:
 build a way that allows you to recommend songs based on song/playlist inputs

Tools used:
 webscapping + api’s (in our case the spotify api used with the spotipy library)

Solutions:

1. V1 - use web scrapping: file name “First Model”
I web scrapped the billboard top 100 plus the Rolling Stones top. I put the files together and wrote a basic function that suggests 3 songs from the list if the artist or song name from the input is in the list. If not, it will return the whole songset as a playlist. In conclusion, this was a very simple recommender based on popularity

2. V2 - playlist recommender based on the spotfy api and the kaggle dataset with aprox 600k songs (https://www.kaggle.com/yamaerenay/spotify-dataset-19212020-160k-tracks):
File names: “project shite”(V2.1), “Reworked” (V2.2)
I took the csv files (used tracks.csv and data_by_artist_o.csv), and after data cleaning (some columns look like nested lists but are actually strings..) I concatenated them together while also extracting the genres. I used TfidfVectorizer on the genres and One Hot Encoder on the floats (the shape of the dataset is something like 560k per 3k). 
Created a spotify account, made a playlist 
(https://open.spotify.com/playlist/7nISMCftMmrrdPgB1qxQaN), interrogated the playlist and matched the songs in the playlist against the one in the dataset (by song id) and made predictions by weighting the newest songs a bit higher.
First try worked but the recommendations were a bit more “pop” and “r&b” than I wanted, so I had to redo the code - now the recommender returns almost only rock songs :P (invictus!)

3.  V3 - clustering:
I used PCA, KMeans and Pairwise Distances on the dataset from version 2.
Set PCA to 2 features, and KMeans to n_clusters=13, random_state=42
Defined 2 functions, using pairwise_distances_argmin_min one , one with default metric, one with metric = 'cosine'
The recommendations for the ‘cosine’ one worked better :P

4. V4 - kept trying stuff:
File name: ‘playlists and all that’
Used spotipy and the spotify api to get 2k songs from the playlist “Greatest Playlist Ever” (https://open.spotify.com/playlist/4Gz7FAUEX9RWdpdje7qLiR ) and save that to a .csv. 
Then I used clustering and Pairwise Distances, both default metric and cosine on the dataset in the .csv
As recommendations go, this one was the most to my taste, and I’m showing you some results:

Song input
'Brianstorm','Arctic Monkeys'
1.Ain't No Grave
Johnny Cash
2.Back To Black
Amy Winehouse

'Pure Morning','Placebo'
1.Glory Days
Bruce Springsteen
2.Bohemian Rhapsody - Remastered 2011
Queen

'November Rain',"Guns N' Roses"
1.Say Hello, Wave Goodbye 
David Gray
2.Listen Up - Remastered
Oasis

Approach limitations:

The songs recommended are from the songs in the files/datasets
With big databases the execution times are huge (and very memory usage intensive)
The api’s need a pretty good internet connection to run on a loop (when, for example, you want to download information from long playlists) (there is also a trick with the timeout of the requests, but in the end I did not have to use it..)

What could I have also done:

Get the songs not in the files via api, add them to the dataset. This works better when you get the songs and you do not need to work to transform the info you get (for v2.1 and v2.2 this would have been a very tragic approach..)
Find a way to work only api- based: like when a user inputs a song get him a playlist from spotify that has that song in in
.. and the most simple (or complicated, depends :P) use spotify’s recommendation engine via api

