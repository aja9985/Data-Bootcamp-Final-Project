# Data-Bootcamp-Final-Project
## Explaining Music Popularity Using Audio Features and Machine Learning

Done by: Deema Hazim and Ameera Alrahmah


# Introduction

What makes a song popular? With streaming platforms like Spotify, we now have data that measures musical characteristics like danceability, energy, and mood. This project investigates whether these audio features can predict or explain song popularity.

Using a dataset of 114,000 Spotify tracks, we address four questions: (1) Which audio features matter most for popularity? (2) Can we classify songs as "hits" versus "non-hits"? (3) Do genre or artist factors improve prediction? (4) Do audio features cluster into underlying dimensions that explain why certain features matter for hit prediction?

# **Dataset Description**

 We use the "maharshipandya/spotify-tracks-dataset" from Hugging Face, containing 114,000 Spotify tracks across 125 genres. The data comes from Spotify's Web API.

The dataset includes nine audio measurements:
- Danceability, Energy, Valence (0-1): How danceable, energetic, and positive a song sounds
- Acousticness, Instrumentalness (0-1): How acoustic or instrumental a track is
- Speechiness, Liveness (0-1): Presence of spoken words and live audience
- Loudness (dB): Overall volume
- Tempo (BPM): Speed in beats per minute

Most features are scaled 0-1, except loudness (measured in decibels) and tempo (measured in beats per minute).


The dataset has no missing values. We standardized all features (mean = 0, standard deviation = 1) before modeling to ensure fair comparison across different scales.


# Question 1: Which Audio Features Matter Most for Popularity? 

### **Method**


We used linear regression to identify which audio features correlate most strongly with popularity. The model estimates how much each feature independently contributes to a song's popularity score. We split the data into training (80%) and testing (20%) sets. After standardizing the features, we trained the model and evaluated it using:

- R-squared: How much variance the model explains (0 = explains nothing, 1 = perfect prediction)
- RMSE: Average prediction error in popularity points
- MAE: Average absolute prediction error


### **Results**


Model Performance: The model achieved an R-squared of 0.020, meaning audio features explain only 2% of popularity variance. The RMSE of 21.99 and MAE of 18.35 show substantial prediction errors- on a 0-100 scale, predictions are often off by about 20 points.


Which Features Matter (looking at the regression coefficients):
- Positive effects: Danceability and loudness show the strongest positive associations- more danceable and louder songs tend to be more popular
- Negative effects: Instrumentalness and valence show negative associations- purely instrumental tracks and extremely positive-sounding songs tend to be less popular


### **Interpretation**


While we successfully identified which features correlate with popularity, the extremely low R-squared reveals a critical insight: audio features alone cannot predict success. The characteristics that make a song sound a certain way (danceable, loud, acoustic) account for only a tiny fraction of what makes it popular.


This suggests popularity depends primarily on factors beyond the music itself- artist fame, marketing budget, playlist placements, release timing, social media buzz, and cultural trends. Audio features describe what songs sound like, but they don't determine which songs succeed. This explains why predicting exact popularity scores is so difficult: the music industry's success factors extend far beyond sonic properties. However, while predicting exact popularity scores proves difficult, a more practical question emerges: can these same audio features distinguish between hits and non-hits instead?



# Question 2: Can we classify songs as “hit” vs “non-hit”?

### **Method**

We defined a "hit" as any song with a popularity score of 50 or above, resulting in 25.8% of songs classified as hits. We trained three classification models—Logistic Regression, Decision Tree (max_depth=5), and Random Forest (100 estimators)—using nine audio features: danceability, energy, loudness, speechiness, acousticness, instrumentalness, liveness, valence, and tempo. The dataset was split 80/20 for training and testing, and all features were standardized using StandardScaler to ensure fair comparison across different scales.

### **Results**

<img width="500" height="490" alt="image" src="https://github.com/user-attachments/assets/fd2c0342-17a0-4eec-8606-3be2fab28431" />

*Figure 1: Model performance comparison for identifying hit songs* 

All three models achieved reasonable classification accuracy. Logistic Regression and Decision Tree both achieved 74.6% accuracy, while Random Forest performed significantly better at 85.1% accuracy. The confusion matrices reveal that all models were highly effective at identifying non-hits (98% recall for Random Forest) but struggled more with identifying actual hits (48% recall for Random Forest). However, when the Random Forest model predicted a song would be a hit, it was correct 88% of the time (precision).

To explore whether the definition of "hit" matters, we tested different popularity thresholds (40, 50, 60, and 70). Interestingly, accuracy improved as the threshold increased: 77.5% at threshold 40, 85.1% at threshold 50, 93.3% at threshold 60, and 97.8% at threshold 70. This suggests that distinguishing extremely popular songs from the rest is easier than identifying moderately popular songs.


<img width="500" height="490" alt="image" src="https://github.com/user-attachments/assets/2eaff9d4-8801-4a8e-82ae-d98300e34402" />

*Figure 2: Feature importance for hit vs. non-hit songs*

Feature comparison analysis showed hit songs are characterized by significantly higher loudness (+0.384) and slightly higher danceability (+0.020). Non-hits tended to have lower tempo (-0.758), more instrumentalness (-0.056), more liveness (-0.038), and higher acousticness (-0.026).

### **Interpretation**


Songs can be classified as hits versus non-hits using audio features alone, with Random Forest achieving 85% accuracy. This demonstrates that audio characteristics contain meaningful predictive signals about song popularity.

The analysis reveals clear patterns that hit songs are louder and more danceable, aligning with music industry practices where songs intended for radio and streaming are mastered louder to capture attention. The negative associations with instrumentalness, acousticness, and slower tempos suggest mainstream hits favor vocal-centric, electronically produced tracks with upbeat rhythms.

The model's conservative behavior (88% precision, 48% recall for hits) makes it practical for identifying songs with genuine hit potential; however, it misses some actual hits that succeed through non-musical factors such as artist reputation, marketing, or cultural timing. The threshold analysis further confirms that the most popular songs (at threshold 70 with 97.8% accuracy) have very distinctive audio signatures, while moderately popular songs occupy a more ambiguous space where audio features are less deterministic.


# Question 3: Do genre or artist popularity improve prediction accuracy?

### **Method**

To test whether contextual information improves hit prediction, we trained four Random Forest models with different feature combinations: audio features only, audio and genre, audio and artist popularity, and all combined. Genre was one-hot encoded (114 unique genres), and artist popularity was calculated as each artist's average song popularity score. All models used the same 80/20 train-test split with standardized features.

### **Results**

<img width="500" height="490" alt="image" src="https://github.com/user-attachments/assets/8c350eba-1b4c-42ad-9d68-a074ce8318aa" />

*Figure 3: Accuracy of finding hits based on different factors*

The results revealed striking differences. Adding genre information actually decreased accuracy to 82.5% (-2.6 percentage points, -3.0% relative decrease from baseline). In contrast, adding artist popularity dramatically improved performance to 94.3% (+9.3 percentage points, +10.9% relative increase). Combining all features achieved 92.0% accuracy (+6.9 percentage points), which surprisingly performed worse than artist information alone.

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/8551d773-9b09-4248-82e4-0184a9d12da1" />

*Figure 4: Genres with the most hits*

Genre hit rate analysis showed pop-film had the highest hit rate at 94.4%, followed by chill (78.3%), k-pop (71.2%), and sad (69.6%). Regular pop had 64.4%, while genres like metal, hip-hop, and house music had rates around 56-60%.

### **Interpretation**
Artist popularity substantially improves prediction accuracy while genre information does not. The 94.3% accuracy with artist popularity demonstrates that who creates the music is more important than what the music sounds like. This aligns with industry dynamics where established artists benefit from existing fan bases, promotional resources, and algorithmic playlist placements that give their songs advantages.

The genre hit rate analysis provides additional context: certain genres like pop-film (94.4%), chill (78.3%), and k-pop (71.2%) have much higher hit rates than others. However, this information appears less useful for prediction than artist popularity, possibly because successful artists already tend to work within popular genres, or because the audio features capture genre-specific characteristics more effectively than categorical labels.



# Question 4: Do audio features cluster into underlying dimensions that explain why certain features matter for hit prediction?


### **Methods**


We used Principal Component Analysis (PCA) to see if the nine audio features cluster into broader patterns. PCA is a technique that looks for correlations between features and groups them into components - new dimensions that capture the main ways songs differ from each other. This helps us understand whether we really need to track nine separate features or if a few underlying patterns explain most of the variation.


We standardized the data and ran PCA to extract components. We examined:
- How much variance each component explains
- Which original features load most strongly onto each component
- Whether these components correlate with popularity

### **Results**

The first three components together explain 61% of the variance in the data, each explaining more than 10% of the variance, while the remaining components contribute relatively little new information (less than 10% variance).


<img width="500" height="490" alt="pca_variance (1)" src="https://github.com/user-attachments/assets/08f4ab80-3c3f-422f-8681-96cc54f1ed28" />

*Figure 5: Variance explained by each principal component* 

To understand what these components represent, we examine how strongly each original audio feature contributes to each component. The heatmap below shows these loadings: red indicates features that strongly define the positive end of a component, while blue indicates features that define the negative end.


<img width="500" height="490" alt="pca_heatmap" src="https://github.com/user-attachments/assets/11616a2c-8155-4fc4-8140-c89abf93961b" />

*Figure 6: Feature loadings on the first three principal components* 

- **Component 1 (32% of variance)** - Energy/Loudness vs. Acoustic: Energy and loudness load positively, while acousticness loads negatively. This dimension separates high-energy, loud songs from acoustic, mellow tracks.
- **Component 2 (16% of variance)** - Danceability/Mood: Danceability and valence load most strongly here, representing upbeat, positive party music versus subdued tracks.
- **Component 3 (14% of variance)** - Live Performance: Liveness and speechiness define this dimension, capturing the difference between live recordings with audience presence and studio productions.

Correlation with Popularity: When we checked how these three dimensions relate to popularity, all correlations were below 0.04 (essentially zero). This means that while songs do cluster into these interpretable patterns, being high or low on any dimension doesn't predict popularity.


### **Interpretation**

The PCA successfully found three clear patterns in how audio features group together- songs vary along dimensions of energy, danceability, and liveness. However, these patterns don't predict which songs become popular.

This finding reinforces what we learned in Question 1: popular tracks span all styles. Hits can be energetic or acoustic, danceable or calm, live or studio-recorded. Audio features describe musical characteristics and help us categorize songs, but they don't provide a formula for how popular a song would be. 


# Conclusion

Our findings highlight that while audio features effectively describe and categorize music, popularity depends on factors beyond the music itself: marketing, social media virality, playlist placements, cultural timing, and artist platforms.

* **Audio features alone poorly predict exact popularity** (R² = 0.02), but can distinguish hits from non-hits with 85% accuracy, suggesting they contain signals about hit potential rather than determining success.

* **Hits have distinctive characteristics:** louder, more danceable, and favor vocal-centric production. Hit songs are engineered to be louder, confirming common industry production practices

* **Artist reputation dominates music quality:** Adding artist popularity improved accuracy from 85% to 94%, while genre information decreased performance. This tells us that who makes the music matters more than what it sounds like.

* **Audio features cluster into interpretable dimensions** (energy, danceability, liveness), but these don't predict popularity. Hits span all musical styles and there's no single sonic formula for success.

# Next Steps

Future work could enhance predictions by incorporating additional data sources beyond audio features. This includes temporal information like release timing and seasonal trends, lyrical analysis to understand themes and sentiment, social media signals such as TikTok trends and playlist additions, and collaboration networks between artists. Additionally, comparing popularity across different platforms (YouTube, Apple Music, radio) and building genre-specific models could reveal patterns that a general model misses.
