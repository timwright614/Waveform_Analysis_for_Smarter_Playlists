# Waveform Analysis for Smarter Playlists

## Summary

This project aimed to improve upon the playlist building algorithms used by streaming platforms. We built a system which assesses song’s waveform, understanding it’s sound and feel directly rather than making hit and miss approximations based on listener behavior. Our presentation below gives a high level description of the work.

https://www.youtube.com/watch?v=B7gi70YxV2E&ab_channel=LeWagon


## The Problem

When it comes to automated playlist generation, streaming platforms have some notable flaws due to an apparent over-reliance on song meta-data such as genre, artist and label, in conjunction with cross-referencing user listening patterns and playlists (what songs are listened to together or appear in playlists together). The effect of this is that when a user requests a playlist based on a particular 'seed' song (as with spotify radio), if the songs is not well known, the cross referencing system has insufficient data to be effective. Further, even when the seed song does have good data, songs with low data won't appear in the generated playlist, even if they are a great match terms of sound and feel. This causes something of a feedback loop with lesser known music not appearing in automatic playlists and hence remaining lesser known. A separate issue arises when the seed song is an outlier for it's artist or genre. This generally leads to a generated playlist containing music typical of the artists or genre and not similar in sound to the seed.

## The Proposal

With these issues in mind, we aimed to build a system which used only a song's audio, placing it within a network of others, an n-dimensional distribution where a song's proximity to another is a measure of it's similarity in terms of sound and feel. This distribution would have multiple uses in playlist generation, all of which are free of reliance on song metadata or user behavior.

It is worth noting that despite a song's genre classification being a fairly blunt instrument in terms of defining and categorising sound (it is quite possible for a playlist of mixed genres to sound more cohesive than a playlist of a single genre), on average it does tap into something that listeners can relate to despite the issues of significant overlap and lack of nuance. As such it is a useful guide to check correlations and guide our models in their distributions.

## The Data

The dataset consisted of 8000 unlicensed songs, a subset of the music available on Free Music Archive (FMA).

https://freemusicarchive.org/
https://github.com/mdeff/fma

## The Process

Central to this work is a numerical representations of the audio, primarily a 2D tensor with 'rows' representing frequency bands, columns representing time and values showing the amplitude of a given frequency/time. Given that music is nothing more than air vibrating at certain frequencies and amplitudes over time, this representation maintains everything we need to know about a song. When plotted as a heat map of amplitude with frequency on the y axis, time on the x, we have a spectrogram.

https://en.wikipedia.org/wiki/Spectrogram

Initially we take various summary metrics extracted by the python library, Librosa. These metrics contain less information than the tensors described above, summarising songs in terms of frequency averages and variances. To get a sense of the extent to which these metrics can be used to differentiate songs, we conducted a principal components analysis, overlaying genre to show correlation within the PCA distribution.

[insert notebook]

This process can be explored interactively at the below web address.

https://projector.tensorflow.org/?config=https://gist.githubusercontent.com/scutellaria/6b7665ede566f36088a87b33da2ba620/raw/8d0c5d752a6ceda80da66c12ad0b8589af0eff25/ml_config.json

With some evidence to suggest these numerical representations of songs could reflect human interpretation, we


The below website visually shows our system in action. Displayed are 8000 songs (dots) from FMA (Free Music Archive), distributed in 3d space such that song's proximity to one another reflects a measure of sonic similarity. This distribution is achieved through supervised training of a CNN model to catagorise genre, giving the model a general abilty to differentate

https://scutellaria-aafpg-streamlit-app-67a07f.streamlitapp.com/


This would allow us to collect songs surrounding a seed song within the distribution, songs that have been determined to be most similar in sound and feel independent of metadata and user listening behavior. The distribution could also be used to generate
