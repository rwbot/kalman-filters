# Multivariate Gaussian

[![Video Lesson](https://preview.ibb.co/c8DrSo/capture_screen.png)](https://youtu.be/ih69P0KJgII)

Most robots that we would be interested in modeling are moving in more than one dimension. For instance, a robot on a plane would have an x & y position.

The simple approach to take, would be to have a 1-dimensional Gaussian represent each dimension - one for the x-axis and one for the y-axis.

Do you see any problems with this?

### QUIZ QUESTION

Why couldn't we use multiple 1-dimensional Gaussians to represent multi-dimensional systems?
-   There may be correlations between dimensions that we would not be able to model by using independent 1-dimensional Gaussians.
    


The image below depicts a two-dimensional Gaussian distribution.

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/519e39cc-7ae6-4e29-bec0-17ddcbfd41c8#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/May/5afb04b6_multivariate-gaussian/multivariate-gaussian.png)

Let's dive into the details!

## Formulas for the Multivariate Gaussian

### Mean

The mean is now a vector,

$$\large \mu = \left[ \begin{array}{c} \mu_{x} \\ \mu_{y} \end{array} \right]$$

### Covariance

And the multidimensional equivalent of variance is a covariance matrix,

$$\large \Sigma = \left[ \begin{array}{cc} \sigma_{x}^2 & \sigma_{x}\sigma_{y} \\ \sigma_{y}\sigma_{x} & \sigma_{y}^2 \end{array} \right]$$

Where  \sigma_{x}^2σx2​  and  \sigma_{y}^2σy2​  represent the variances, while  \sigma_{y}\sigma_{x}σy​σx​  and  \sigma_{x}\sigma_{y}σx​σy​  are correlation terms. These terms are non-zero if there is a correlation between the variance in one dimension and the variance in another. When that is the case, the Gaussian function looks 'skewed' when looked at from above.

If we were to evaluate this mathematically, the eigenvalues and eigenvectors of the covariance matrix describe the amount and direction of uncertainty.

### Multivariate Gaussian

Below is the formula for the multivariate Gaussian. Note that  xx  \muμ  are vectors, and  \SigmaΣ  is a matrix.

$$\large p(x) = \frac{1}{(2\pi)^{\frac{D}{2}}|\Sigma|^\frac{1}{2}}e^{-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)}$$

If D=1, the formula simplifies to the formula for the one-dimensional Gaussian that you have seen before.

NEXT
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYzMzgwMjE3NV19
-->