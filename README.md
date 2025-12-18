# Data-Bootcamp-Final-Project
Explaining Music Popularity Using Audio Features and Machine Learning

**Introduction**

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
