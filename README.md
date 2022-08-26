# Waveform Analysis for Smarter Playlists

## Summary

This project aimed to improve upon the playlist building algorithms used by streaming platforms. We built a system which assesses song’s waveform, understanding it’s sound and feel directly rather than making hit and miss approximations based on listener behavior. Our presentation below gives a high level description of the work.

https://www.youtube.com/watch?v=B7gi70YxV2E&ab_channel=LeWagon


## The Problem

When it comes to automated playlist generation, streaming platforms have some notable flaws due to an apparent over-reliance on song meta-data such as genre, artist and label, in conjunction with cross-referencing user listening patterns and playlists (what songs are listened to together or appear in playlists together). The effect of this is that when a user requests a playlist based on a particular 'seed' song (as with spotify radio), if the songs is not well known, the cross referencing system has insufficient data to be effective. Further, even when the seed song does have good data, songs with low data won't appear in the generated playlist, even if they are a great match terms of sound and feel. This causes something of a feedback loop with lesser known music not appearing in automatic playlists and hence remaining lesser known.

The dataset consisted of 8000 unlicensed songs, a subset of the music available on Free Music Archive (FMA).

https://freemusicarchive.org/
https://github.com/mdeff/fma

Central to this work is a numerical representation of the audio, a 2D tensor with 'rows' representing frequency bands, columns representing time and values showing the amplitude of a given frequency/time. Given that music is nothing more than air vibrating at certain frequencies and amplitudes over time, this representation maintains everything we need to know about a song. When plotted as a heat map of amplitude with frequency on the y axis, time on the x, we have a spectrogram.

https://en.wikipedia.org/wiki/Spectrogram

Initially we take various summary metrics extracted by the python library, Librosa. These metrics contain less information than the tensors described above, summarising these with in terms of frequency averages and variances. To get a sense of the extent to which these metrics can be used to differentiate songs, we conducted a principal components analysis, overlaying genre to show correlation within the PCA distribution.

[insert notebook]

This process can be explored interactively at the below web address.

https://projector.tensorflow.org/?config=https://gist.githubusercontent.com/scutellaria/6b7665ede566f36088a87b33da2ba620/raw/8d0c5d752a6ceda80da66c12ad0b8589af0eff25/ml_config.json

With some
The below website visually shows our system in action. Displayed are 8000 songs (dots) from FMA (Free Music Archive), distributed in 3d space such that song's proximity to one another reflects a measure of sonic similarity. This distribution is achieved through supervised training of a CNN model to catagorise genre, giving the model a general abilty to differentate

https://scutellaria-aafpg-streamlit-app-67a07f.streamlitapp.com/
