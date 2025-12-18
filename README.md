# Data-Bootcamp-Final-Project
## Explaining Music Popularity Using Audio Features and Machine Learning

Done by: Deema Hazim and Ameera Alrahmah


### Introduction

What makes a song popular? With streaming platforms like Spotify, we now have data that measures musical characteristics like danceability, energy, and mood. This project investigates whether these audio features can predict or explain song popularity.

Using a dataset of 114,000 Spotify tracks, we address four questions: (1) Which audio features matter most for popularity? (2) Can we classify songs as "hits" versus "non-hits"? (3) Do genre or artist factors improve prediction? (4) Do audio features cluster into underlying patterns?

**Dataset Description**

 We use the "maharshipandya/spotify-tracks-dataset" from Hugging Face, containing 114,000 Spotify tracks across 125 genres. The data comes from Spotify's Web API.

The dataset includes nine audio measurements:
- Danceability, Energy, Valence (0-1): How danceable, energetic, and positive a song sounds
- Acousticness, Instrumentalness (0-1): How acoustic or instrumental a track is
- Speechiness, Liveness (0-1): Presence of spoken words and live audience
- Loudness (dB): Overall volume
- Tempo (BPM): Speed in beats per minute

Most features are scaled 0-1, except loudness (measured in decibels) and tempo (measured in beats per minute).


The dataset has no missing values. We standardized all features (mean = 0, standard deviation = 1) before modeling to ensure fair comparison across different scales.


###Question 1: Which Audio Features Matter Most for Popularity?** 

**Method**


We used linear regression to identify which audio features correlate most strongly with popularity. The model estimates how much each feature independently contributes to a song's popularity score. We split the data into training (80%) and testing (20%) sets. After standardizing the features, we trained the model and evaluated it using:

- R-squared: How much variance the model explains (0 = explains nothing, 1 = perfect prediction)
- RMSE: Average prediction error in popularity points
- MAE: Average absolute prediction error


**Results**


Model Performance: The model achieved an R-squared of 0.020, meaning audio features explain only 2% of popularity variance. The RMSE of 21.99 and MAE of 18.35 show substantial prediction errors- on a 0-100 scale, predictions are often off by about 20 points.


Which Features Matter (looking at the regression coefficients):
- Positive effects: Danceability and loudness show the strongest positive associations- more danceable and louder songs tend to be more popular
- Negative effects: Instrumentalness and valence show negative associations- purely instrumental tracks and extremely positive-sounding songs tend to be less popular


**Interpretation**


While we successfully identified which features correlate with popularity, the extremely low R-squared reveals a critical insight: audio features alone cannot predict success. The characteristics that make a song sound a certain way (danceable, loud, acoustic) account for only a tiny fraction of what makes it popular.


This suggests popularity depends primarily on factors beyond the music itself- artist fame, marketing budget, playlist placements, release timing, social media buzz, and cultural trends. Audio features describe what songs sound like, but they don't determine which songs succeed. This explains why predicting exact popularity scores is so difficult: the music industry's success factors extend far beyond sonic properties. However, while predicting exact popularity scores proves difficult, a more practical question emerges: can these same audio features distinguish between hits and non-hits instead?

