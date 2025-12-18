# Data-Bootcamp-Final-Project
## Explaining Music Popularity Using Audio Features and Machine Learning

Done by: Deema Hazim and Ameera Alrahmah


### Introduction

What makes a song popular? With streaming platforms like Spotify, we now have data that measures musical characteristics like danceability, energy, and mood. This project investigates whether these audio features can predict or explain song popularity.

Using a dataset of 114,000 Spotify tracks, we address four questions: (1) Which audio features matter most for popularity? (2) Can we classify songs as "hits" versus "non-hits"? (3) Do genre or artist factors improve prediction? (4) Do audio features cluster into underlying dimensions that explain why certain features matter for hit prediction?

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


### Question 1: Which Audio Features Matter Most for Popularity? 

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




### Question 2: Can we classify songs as “hit” vs “non-hit”?




### Question 3: Do genre or artist popularity improve prediction accuracy?




### Question 4: Do audio features cluster into underlying dimensions that explain why certain features matter for hit prediction?


**Methods**


We used Principal Component Analysis (PCA) to see if the nine audio features cluster into broader patterns. PCA is a technique that looks for correlations between features and groups them into components - new dimensions that capture the main ways songs differ from each other. This helps us understand whether we really need to track nine separate features or if a few underlying patterns explain most of the variation.


We standardized the data and ran PCA to extract components. We examined:
- How much variance each component explains
- Which original features load most strongly onto each component
- Whether these components correlate with popularity


**Results**


The first three components together explain 61% of the variance in the data. The first three components capture the most meaningful patterns in the data, each explaining more than 10% of the variance, while the remaining components contribute relatively little new information (less than 10% variance).


*Figure 1: Variance explained by each principal component* 


- **Component 1 (32% of variance)** - Energy/Loudness vs. Acoustic: Energy and loudness load positively, while acousticness loads negatively. This dimension separates high-energy, loud songs from acoustic, mellow tracks.
- **Component 2 (16% of variance)** - Danceability/Mood: Danceability and valence load most strongly here, representing upbeat, positive party music versus subdued tracks.
- **Component 3 (14% of variance)** - Live Performance: Liveness and speechiness define this dimension, capturing the difference between live recordings with audience presence and studio productions.


To understand what these components represent, we examine how strongly each original audio feature contributes to each component. The heatmap below shows these loadings: red indicates features that strongly define the positive end of a component, while blue indicates features that define the negative end.


*Figure 2: Heatmap* 


Correlation with Popularity: When we checked how these three dimensions relate to popularity, all correlations were below 0.04 (essentially zero). This means that while songs do cluster into these interpretable patterns, being high or low on any dimension doesn't predict popularity.


**Interpretation**


The PCA successfully found three clear patterns in how audio features group together- songs vary along dimensions of energy, danceability, and liveness. However, these patterns don't predict which songs become popular.


This finding reinforces what we learned in Question 1: popular tracks span all styles. Hits can be energetic or acoustic, danceable or calm, live or studio-recorded. Audio features describe musical characteristics and help us categorize songs, but they don't provide a formula for how popular a song would be. 


### Conclusion
