# Waveform Analysis for Smarter Playlists

This project aimed to improve upon the playlist building algorithms used by streaming platforms. We built a system which assesses song’s waveform, understanding it’s sound and feel directly rather than making hit and miss approximations based on listener behavior. Our presentation below gives a high level description of the work.

https://www.youtube.com/watch?v=B7gi70YxV2E&ab_channel=LeWagon

The dataset consisted of 8000 unlicensed songs, a subset of the music available on Free Music Archive (FMA).

https://freemusicarchive.org/
https://github.com/mdeff/fma

Our goal was to build a system that could discern between songs in such a way as to be relatable to human interpretation, purely on song's waveform, without any accompanying metadata such as genre, related artists, playlist and listener behavior cross referencing.

Central to this work is a numerical representation of the audio, a 2D tensor with 'rows' representing frequency bands, columns representing time and values showing the amplitude of a given frequency/time. Given that music is nothing more than air vibrating at certain frequencies and amplitudes over time, this representation maintains everything we need to know about a song. When plotted as a heat map of amplitude with frequency on the y axis, time on the x, we have a spectrogram.

https://en.wikipedia.org/wiki/Spectrogram

Initially


The below website visually shows our system in action. Displayed are 8000 songs (dots) from FMA (Free Music Archive), distributed in 3d space such that song's proximity to one another reflects a measure of sonic similarity. This distribution is achieved through supervised training of a CNN model to catagorise genre, giving the model a general abilty to differentate

https://scutellaria-aafpg-streamlit-app-67a07f.streamlitapp.com/
