# 1D Gaussian

At the basis of the Kalman Filter is the Gaussian distribution, sometimes referred to as a bell curve or normal distribution. Recall the rover example - after executing one motion, the rover’s location was represented by a Gaussian. It’s exact location was not certain, but the level of uncertainty was bounded. It was unlikely that the rover would be more than a few meters away from its target location, and it would be nearly impossible for it to show up at the 50 meter mark.

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/e91213a4-1faf-4dbc-878a-758e0b74dc1b#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/January/5a6a7042_c2l2-a08-rover/c2l2-a08-rover.png)

This is the role of a Kalman Filter - after a movement or a measurement update, it outputs a unimodal Gaussian distribution. This is its best guess at the true value of a parameter.

A Gaussian distribution is a probability distribution, which is a continuous function. The probability that a random variable, x, will take a value between $$x_1 \space and \space  x_2$$​  is given by the integral of the function from $$x_1 \to x_2$$

$$p(x_1 < x < x_2) = \int_{x_1}^{x_2}f_x(x)dx$$

In the image below, the probability of the rover being located between 8.7m and 9m is 7%.

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/e91213a4-1faf-4dbc-878a-758e0b74dc1b#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/January/5a6a7041_c2l2-a08-graph-/c2l2-a08-graph-.png)

## Mean and Variance

A Gaussian is characterized by two parameters - its mean (μ) and its variance (σ²). The mean is the most probable occurrence and lies at the centre of the function, and the variance relates to the width of the curve. The term unimodal implies a single peak present in the distribution.

Gaussian distributions are frequently abbreviated as N(x: μ, σ²), and will be referred to in this way throughout the coming lessons.


The formula for the Gaussian distribution is printed below. Notice that the formula contains an exponential of a quadratic function. The quadratic compares the value of x to μ, and in the case that x=μ, the exponential is equal to 1 $$(e^0 = 1)$$. You’ll note here, that the constant in front of the exponential is a necessary normalizing factor.

$$\huge p(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

Just like with discrete probability, like a coin toss, the probabilities of all the options must sum to one. Therefore, the area underneath the function always sums to one.

$$\huge \int p(x)dx = 1$$

Now that you are familiar with the formula, it’s time to code the Gaussian in C++. This will allow you to calculate the probability of a value occurring given a mean and a variance!

```

#include <iostream>

#include <math.h>

using namespace std;

double f(double mu, double sigma2, double x)

{

//Use mu, sigma2 (sigma squared), and x to code the 1-dimensional Gaussian

double normal_constant = 1 / ( sqrt(sigma2) * sqrt(2 * M_PI) );

double exponent = ( 0 - ( pow((x - mu), 2) / (2 * sigma2) ) );

double prob = normal_constant * exp(exponent);

return prob;

}

int main()

{

cout << f(10.0, 4.0, 8.0) << endl;

return 0;

}
```









<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODQwOTU3OThdfQ==
-->