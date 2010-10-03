---
layout: post
title: Estimate frequency using Numpy
---

# {{ page.title }} #

[![Estimating frequency of noisy sine curve][fig_img]][fig_link]

[fig_img]: http://farm5.static.flickr.com/4154/4956012351_126a54fca0.jpg
[fig_link]: http://www.flickr.com/photos/arataka/4956012351/

Numpy/Scipy doesn't have function for estimating frequency (pitch) of
given 1D time series, but simple functions can do this. I use a very
simple method called "Pisarenko Harmonic Decomposition". On the
picture above, I estimate the frequency of sine curve + noise.  The
estimate (vertical black line on the top panel) is matched to maximum
of the power spectral density.  Note that argument `x` of function
`freq` must satisfy `x.mean() == 0`.


{% highlight python %}
import numpy
PI = numpy.pi


def covariance(x, k):
    N = len(x) - k
    return (x[:-k] * x[k:]).sum() / N


def phd1(x):
    """Estimate frequency using Pisarenko Harmonic Decomposition"""
    r1 = covariance(x, 1)
    r2 = covariance(x, 2)
    a = (r2 + numpy.sqrt(r2 ** 2 + 8 * r1 ** 2)) / 4 / r1
    if a > 1:
        a = 1
    elif a < -1:
        a = -1
    return numpy.arccos(a)


def freq(x, sample_step=1, dt=1.0):
    """Estimate frequency using `phd1`"""
    omega = phd1(x[::sample_step])
    return omega / 2.0 / PI / sample_step / dt
{% endhighlight %}


I posted full source code to get the picture above here:
[gist: 565034 - GitHub](http://gist.github.com/565034).

You can find the formulation of Pisarenko Harmonic Decomposition method
here:
[CiteSeerX — AN UNBIASED PISARENKO HARMONIC DECOMPOSITION ESTIMATOR FOR SINGLE-TONE FREQUENCY](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.75.4859).
