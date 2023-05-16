# IS5126 Final Project by Team 3

## Data Collection
Run `crawler.ipynb` to get the raw data jason file (tracks.json), which contains

The example of track info is as below. (Please refer to https://developer.spotify.com/documentation/web-api/reference/#/operations/get-audio-analysis, https://developer.spotify.com/documentation/web-api/reference/#/operations/get-audio-features, https://developer.spotify.com/documentation/web-api/reference/#/operations/get-track for the meaning of each attribute)

```json
{
  "id": "4v1hislSSlQuI51Laod4Oi",     // The Spotify ID for the track.
  "name": "I Do",                     // The name of the track.
  "artist": "Rotimi",                 // The artists of the track, If there are multiple artists, their names will be connected with '&'.
  "popularity": 49,                   // The popularity of the artist. The value will be between 0 and 100, with 100 being the most popular. The artist's popularity is calculated from the popularity of all the artist's tracks.
  "release_date": "2021-08-27",       // The date the album which contains the track was first released.
  "num_samples": 5650730,             // The exact number of audio samples analyzed from this track.
  "duration": 256.26892,              // Length of the track in seconds.
  "sample_md5": "",                   // This field will always contain the empty string.
  "offset_seconds": 0,                // An offset to the start of the region of the track that was analyzed. (As the entire track is analyzed, this should always be 0.)
  "window_seconds": 0,                // The length of the region of the track was analyzed, if a subset of the track was analyzed. (As the entire track is analyzed, this should always be 0.)
  "analysis_sample_rate": 22050,      // The sample rate used to decode and analyze this track. May differ from the actual sample rate of this track available on Spotify.
  "analysis_channels": 1,             // The number of channels used for analysis. If 1, all channels are summed together to mono before analysis.
  "end_of_fade_in": 0.1946,           // The time, in seconds, at which the track's fade-in period ends. If the track has no fade-in, this will be 0.0.
  "start_of_fade_out": 236.84354,     // The time, in seconds, at which the track's fade-out period starts. If the track has no fade-out, this should match the track's length.
  "loudness": -7.734,                 // The overall loudness of a track in decibels (dB). Loudness values are averaged across the entire track and are useful for comparing relative loudness of tracks. Loudness is the quality of a sound that is the primary psychological correlate of physical strength (amplitude). Values typically range between -60 and 0 db.
  "tempo": 96.935,                    // The overall estimated tempo of a track in beats per minute (BPM). In musical terminology, tempo is the speed or pace of a given piece and derives directly from the average beat duration.
  "tempo_confidence": 0.015,          // The confidence, from 0.0 to 1.0, of the reliability of the tempo.
  "time_signature": 4,                // An estimated time signature. The time signature (meter) is a notational convention to specify how many beats are in each bar (or measure). The time signature ranges from 3 to 7 indicating time signatures of "3/4", to "7/4".
  "time_signature_confidence": 0.967, // The confidence, from 0.0 to 1.0, of the reliability of the time_signature.
  "key": 8,                           // The key the track is in. Integers map to pitches using standard Pitch Class notation. E.g. 0 = C, 1 = C♯/D♭, 2 = D, and so on. If no key was detected, the value is -1.
  "key_confidence": 0.489,            // The confidence, from 0.0 to 1.0, of the reliability of the key.
  "mode": 1,                          // Mode indicates the modality (major or minor) of a track, the type of scale from which its melodic content is derived. Major is represented by 1 and minor is 0.
  "mode_confidence": 0.529,           // The confidence, from 0.0 to 1.0, of the reliability of the mode.
  "danceability": 0.471,              // Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity. A value of 0.0 is least danceable and 1.0 is most danceable.
  "energy": 0.294,                    // Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy. For example, death metal has high energy, while a Bach prelude scores low on the scale. Perceptual features contributing to this attribute include dynamic range, perceived loudness, timbre, onset rate, and general entropy.
  "speechiness": 0.0476,              // Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value. Values above 0.66 describe tracks that are probably made entirely of spoken words. Values between 0.33 and 0.66 describe tracks that may contain both music and speech, either in sections or layered, including such cases as rap music. Values below 0.33 most likely represent music and other non-speech-like tracks.
  "acousticness": 0.915,              // A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 1.0 represents high confidence the track is acoustic.
  "instrumentalness": 8.63e-6,        // Predicts whether a track contains no vocals. "Ooh" and "aah" sounds are treated as instrumental in this context. Rap or spoken word tracks are clearly "vocal". The closer the instrumentalness value is to 1.0, the greater likelihood the track contains no vocal content. Values above 0.5 are intended to represent instrumental tracks, but confidence is higher as the value approaches 1.0.
  "liveness": 0.127,                  // Detects the presence of an audience in the recording. Higher liveness values represent an increased probability that the track was performed live. A value above 0.8 provides strong likelihood that the track is live.
  "valence": 0.438,                   // A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).
  "duration_ms": 256269               // The duration of the track in milliseconds.
}
```

## Data preprocess
Run `Data Processing Final.ipynb`
1. Drop several columns, which contains all the same values.
```
{
  "sample_md5": "",
  "offset_seconds": 0,
  "window_seconds": 0,
  "analysis_sample_rate": 22050,
  "analysis_channels": 1
}
```
2. Fill in NA in the popularity columns with the average popularity score.

3. Label Popularity with a new column called "Label": 
The median of popularity is 56, when a song’s popularity is over 56, we define the song as success, and label the song with 1.

4. translate the `release_data` column to `timestamp` column.

5. Prepare dataset for Linear Regression
Drop Unused Features:'num_samples',"timestamp","id","name","artist","release_date"

6. Prepare dataset for Clustering
Use Audio features "danceability","energy","speechiness","acousticness","instrumentalness","liveness","valence" for the K-Means.

## K-means Modle
Run 'K-Means.ipynb'

## Linear Regression Model
Run "LR.rmd"