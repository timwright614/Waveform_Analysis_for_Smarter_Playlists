# Waveform Analysis for Smarter Playlists

## Summary

This project aimed to improve upon the playlist building algorithms used by streaming platforms. We built a system which assesses song’s waveform, understanding it’s sound and feel directly rather than making hit and miss approximations based on listener behavior. Our presentation below gives a high level description of the work.

https://www.youtube.com/watch?v=B7gi70YxV2E&ab_channel=LeWagon


## The Problem

When it comes to automated playlist generation, streaming platforms have some notable flaws due to an apparent over-reliance on song meta-data such as genre, artist and label, in conjunction with cross-referencing user listening patterns and playlists (analysing which songs are listened to together or appear in playlists together). The effect of this is that when a user requests a playlist based on a particular 'seed' song (as with spotify radio), if the songs is not well known, the cross referencing system has insufficient data to be effective. Further, even when the seed song does have good data, songs with low data won't appear in the generated playlist, even if they are a great match terms of sound and feel. This causes something of a feedback loop with lesser known music not appearing in automatic playlists and hence remaining lesser known. A separate issue arises when the seed song is an outlier for it's artist or genre. This generally leads to a generated playlist containing music typical of the artists or genre and not similar in sound to the seed.

## The Proposal

With these issues in mind, we aimed to build a system which used only a song's audio, placing it within a network of others, an n-dimensional distribution where a song's proximity to another is a measure of it's similarity in terms of sound and feel. This distribution would have multiple uses in playlist generation, all of which are free of reliance on song metadata or user behavior.

It is worth noting that despite a song's genre classification being a fairly blunt instrument in terms of defining and categorising sound (it is quite possible for a playlist of mixed genres to sound more cohesive than a playlist of a single genre), on average it does tap into something that listeners can relate to despite the issues of significant overlap and lack of nuance. As such it is a useful guide to check correlations and guide our models in their distributions.

## The Data

The dataset consisted of 8000 unlicensed songs, a subset of the music available on Free Music Archive (FMA).

https://freemusicarchive.org/
https://github.com/mdeff/fma

## Numerically Representing Songs

Central to this work is a numerical representations of the audio, primarily a 2D tensor with 'rows' representing frequency bands, columns representing time and values showing the amplitude of each frequency/time. Given that music is nothing more than air vibrating at certain frequencies and amplitudes over time, this representation maintains everything we need to know about a song. When plotted as a heat map of amplitude with frequency on the y axis, time on the x, we have a spectrogram.

https://en.wikipedia.org/wiki/Spectrogram

Initially we take various summary metrics extracted by the python library, Librosa. These metrics contain less information than the tensors described above, summarising songs in terms of frequency averages and variances. To get a sense of the extent to which these metrics can be used to differentiate songs, we conducted a principal components analysis, overlaying genre to show correlation within the PCA distribution.

[insert notebook]

This process can be explored interactively at the below web address.

https://projector.tensorflow.org/?config=https://gist.githubusercontent.com/scutellaria/6b7665ede566f36088a87b33da2ba620/raw/8d0c5d752a6ceda80da66c12ad0b8589af0eff25/ml_config.json

With some evidence to suggest these numerical representations of songs could reflect human interpretation, we explored two CNN approaches, models that would work with the spectrogram images described above (again, which describe everything we could want to know about a song).

## Supervised/Semi-supervised CNN approach:
Here we aimed to train a CNN model to catagorise songs using their listed genres as targets. Given that genre classifications are at times misleading and imprecise, we don't expect the model to perform perfectly at this task, we just need it to 'tune in' to the way humans distinguish music. Once fit, we remove the top end of the model (catagorising layers) leaving a fitted model that outputs 64 dense dimensions that contain information useful for discerning between songs in a way that aligns with human intuition.

Using predict on the dataset outputs 64 values for each track, giving them a location in our overall distribution. This location should represent a particular sound and feel, meaning songs which are near to one another in the distribution should be sonically similar while distant songs should not sound alike.

The below notebook shows the processing of the audio into spectrogram image datasets, training of a CNN model using genre as targets, deconstruction of the model to output the 64 dimension representation of each song, and the plotting of this distribution showing it's correlation with genre.

### Automated Playlist Building
The most obvious use for this distribution is to build a playlist based on a seed song by taking the nearest neighbours to the seed song within the distribution.

A more unique use is to use two seed songs, ideally not close together in the distribution, calculate a vector between these points and take a specified number of songs that lay progressively further along the vector from the first seed song to the second, thus creating a smooth playlist journey between start and end point.

For best results we used all 64 dimensions and cosign similarity in order to calculate songs similarity within this high dimensional space. For visualisation we applied T-SNE dimensional reduction in order to build a 3d distribution. The visualisation and playlist building can be explored at our website below.

https://scutellaria-aafpg-streamlit-app-67a07f.streamlitapp.com/

### CNN Autoencoder Approach.
For background:

https://en.wikipedia.org/wiki/Autoencoder

We had initially envisioned using an auto encoder to compress the high dimension spectrogram tensors into a dense layer of something around 50 neurons, hoping that within this compressed information would be useful information ragarding a songs sound. We found that in order for the autoencoder to train effectively, the spectrogram needed to be reduced in resolution to around 128x32 during pre-processing, becoming the input dimension for the auto encoder which goes on to condense the information to a dense layer of 256 neurons. We had hoped to use a much higher resolution (something like 1024x128) and compress it to 64 or 32 dense, latent space dimensions but were unable to get the model to fit with such a reduction. In retrospect, this application is perhaps not suited to an autoencoder, which likely struggles to find an underlying pattern pattern to songs waveforms, such that they can be effectively compressed to such an extent.

The notebook below shows the processing of audio and building/training of the auto encoder. The plots of the spectrograms show the input image, immediately followed by the decoders reconstruction having been compressed by a factor of 16 (128x32 / 256). There is clear similarity but also obvious loss of information. The distribution obtained by running predict on the dataset is also shown and it is clear that it does not well reflect human distinction when overlaid with genre.


https://colab.research.google.com/drive/103GocfVN8qfDaqOiJEEEnWjFJSQTzWfb?usp=sharing
