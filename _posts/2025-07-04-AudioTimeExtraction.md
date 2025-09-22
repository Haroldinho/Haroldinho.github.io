---
layout: post
title: Clip Time Extraction Using FFT
date: 2025-07-04 22:10:11 -0500
categories: [ML, Fourier Transform]
tags: [video, audio, computer-vision]
---

## Introduction

Earlier in the year, our group was presented with a mini-challenge.
We had extracted multiple clips from a video, but didn't save the exact locations of these clips.
Although it is always possible to operate on the extracted clips, it is often desired to adjust the time boundaries of the clips or to pass the clip boundaries and the full video files to avoid duplicating data.
For these reason, it is important to have a precise, reliable, fast and scalable method to precisely localize the time boundaries of a clip within a larger context.
This work is closely related to the task of fingerprinting or indexing a video/audio signal to detect time invariants and temporary localized time signals in a longer time series. Such taks have widespead applications such as being able to identify songs or identify video tracks for copyright infringements or for simple multimodal search.

## ... enters M. Joseph Fourier

There are many methods to solve such complex problems (FFT, Piecewise-hashing, Transformers/RNN Embeddings...).
Indeed, the author will expand on different solutions in these blogs.
The first solution that was chosen is the use of the Fast Fourier Transform (FFT).
This method provides many desireable capabilities:

- It is elegant, due to its simplicity.
- It is a good introduction to the world of linear representation and embeddings on which machine learning methods are based.
- As will be seen in the results sections, it is remarkably fast and accurate.

In some instances, the audio tracks may be corrupted or missing from the video proxy. In these situations, we will forgo the FFT for more visual methods that rely on visual embeddings.
In any cases, the Fourier Series provide a good first step into the functional space.

## Fourier Series and Fourier Transforms

Most of the content of this section comes from the excellent book on data science and engineering from Steve Brunton and Nathan Kutz {% cite brunton_book_2019 %}.
An inner product allows you to project a vector space into a new bases.
The inner product between two functions defines a transformation from one space to the next.
For example an integral transform is generally defined as follows where $\bar{a}$ defines the complex conjugate of a complex value:
$$
<f, g> = \int_{-\infty}{\infty} f(x) \bar{g(x)} dx
$$

Examples of popular integral transforms are the Laplace Transform, an extension of Fourier with non-purely imaginary terms and Moment Generating Function to characterize probability distribution functions. These transforms are really useful in control theory and probability/statistics respectively, but are way beyond the context of this blog post.

The Fourier Transform is a linear transformation that changes the coordinates of a function to the Fourier domain.
More concretely, it is used to go from the time domain to the frequency domain.
It uses an infinite dimensional function, called a Fourier series as its bases:
$$
f(x) = \frac{a_0}{2} + \sum_{k} \left[a_k cos(\frac{2 \pi x}{L}) + b_k sin(\frac{2 \pi x}{L}) \right]\\
f(x) = \sum_{k=-\infty}^{\infty} c_k e^{-\frac{i k \pi x}{L}}
$$
The Fourier Series is the limit of a Fourier series on the length of the domin $\left(0, L\right] \to (-\infty, \infty)$

$$
\hat{f}(\omega)=\mathcal{F}\{f(t)\}(\omega) = \int_{-\infty}^{\infty} f(t)\, e^{-i \omega t}\, dt
$$

The Inverse Fourier Transform is defineed as
$$
f(x) = \mathcal{F}^{-1}\{\hat{f}(\omega)\} = \int_{-2\pi}^{\pi} \hat{f}(\omega) e^{-i \omega t} d\omega
$$

Some important facts from {% cite brunton_book_2019 %}:

1. If $f(x)$ is periodic an piecewise continuous, it can be written as a sum of Fourier Series.
2. The Uncertainty Principles:  A signal can be completely resolved in time, but will then provide no information in the frequency domain and vice-versa. In other words, as one gains more information in one of the two domains: time or frequency, one loses information in the other. No signal can be fully known in both domains. The Fourier Transform will give you the spectral characteristic of a signal, but is incapable of telling you when in time the frequencies occur.
3. A direct consequence, is that the Fourier Transform is only able to fully characterize truly periodic and stationary signals. In other words, we can approximate aperiodic signals, but cannot resolve them fully. If the signal is not stationary (changes with time), we can approximate locally or resort to hierarchical methods such as Gabor Decomposition or wavelets for higher resolution. Gabor transform operates on short frequencies allowing to handle transitions more elegantly than we simple Fourier transforms. Wavelet transforms use multi-scale orthogonal basis to extract information both in the time and frequency domain. Wavelets are more appropriate for image processing, compressiona and high dimensional processing.

## Discrete Fourier Transforms and Fast Fourier Transforms

Because the Fourier Transform is linear it can be represented by matrix operations on a finite set.
The Discrete Fourier Transform is that representation over a finite set that allows you to transform a function from the time domain to the frequency domain and vice versa.

$$
\hat{f}_k = \sum_{j=0}^{n-1} f_j e^{-\frac{i 2 \pi x}{n}}
$$

$$
f_k = \frac{1}{n} \sum_{j=0}^{n-1} f_j e^{i \frac{2 \pi j k}{n}}
$$

The Fast Fourier Transform leverages the high symmetricity of the Fourier matrix to transform the quadratic $O(n^2)$ matrix multiplication operation into a loglinear operation $O(n log n)$.
This is this operation that we utilize to index the frequency signature of our videos.

## Indexing Frequency Signatures with Fourier Transforms

## Results

## Conclusions and next steps

## References

{% bibliography --cited %}
