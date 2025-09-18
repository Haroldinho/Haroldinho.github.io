---
title: Clip Time Extraction Using FFT
date: 2025-07-04 22:10:11 -0500
categories: [ML, Fourier Transform]
tags: [video, audio, computer-vision]
---

# Introduction

Earlier in the year, our group was presented with a mini-challenge.
We had extracted multiple clips from a video, but didn't save the exact locations of these clips.
Although it is always possible to operate on the extracted clips, it is often desired to adjust the time boundaries of the clips or to pass the clip boundaries and the full video files to avoid duplicating data.
For these reason, it is important to have a precise, reliable, fast and scalable method to precisely localize the time boundaries of a clip within a larger context.
This work is closely related to the task of fingerprinting or indexing a video/audio signal to detect time invariants and temporary localized time signals in a longer time series. Such taks have widespead applications such as being able to identify songs or identify video tracks for copyright infringements or for simple multimodal search.

# ... enters M. Joseph Fourier

There are many methods to solve such complex problems (FFT, Piecewise-hashing, Transformers/RNN Embeddings...). 
Indeed, the author will expand on different solutions in these blogs. 
The first solution that was chosen is the use of the Fast Fourier Transform (FFT). 
This method provides many desireable capabilities:
- It is elegant, due to its simplicity.
- It is a good introduction to the world of linear representation and embeddings on which machine learning methods are based.
- As will be seen in the results sections, it is remarkably fast and accurate.

In spite of all these benefits, the FFT is not a panacea for the problem at hand. In many situations, the audio tracks may have been corrupted or may be missing from the video available. In these cases, we will forgo the FFT for more visual methods such as Piecewise-hashing or different evolutions of Vector Embeddings. These methods will be presented in subsequent articles.

# Fourier Series and Fourier Transforms

A Fourier Series is an infinite dimensional function:\
$$
f(x) = \frac{a_0}{2} + \sum_{k} \left[a_k cos(\frac{2 \pi x}{L}) + b_k sin(\frac{2 \pi x}{L}) \right]
$$


An integral transform is a functional that changes a function from the real domain to the function domain by applying some type of inner product:\
$$
<f, g> = \int_{-\infty}{\infty} f(x) \bar{g(x)} dx
$$

Examples of popular integral transforms are the Laplace Transform, an extension of Fourier with non-purely imaginary terms and Moment Generating Function to characterize probability distribution functions. These transforms are really useful in control theory and probability/statistics respectively, but are way beyond the context of this blog post.\

The Fourier Transform is a linear transformation that changes the coordinates of a function to the Fourier domain.\
$$
\mathcal{F}\{f(t)\}(\omega) = \int_{-\infty}^{\infty} f(t)\, e^{-i \omega t}\, dt
$$\
When dealing with time signals, it can be understood as a tranformation from the time domain to the frequency domain. 
The transformation is similar to other important linear tranformation such as the Laplace transformation or the transformation used to obtain Moment Generating Function (MGF) in statistics. 
{% cite brunton_book_2019 %}






# References:
{% bibliography --cited %}