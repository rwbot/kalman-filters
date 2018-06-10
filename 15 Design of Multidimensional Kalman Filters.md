# Design of Multi-Dimensional Kalman Filters

[![](https://preview.ibb.co/f60S08/capture_screen_1.png)](https://www.youtube.com/watch?v=9Xb5WavDqKE)


To implement a two dimensional Kalman Filter, you will need to apply several linear algebra equations. To begin with, let’s convert the state prediction into linear algebra form. This is helpful because linear algebra allows us to work easily with multi-dimensional problems.

## State Transition

We know that the update formula below represents the relationship between the robot’s position,  $\large x$, and velocity,  $\large \dot{x}$. We can also assume that the robot’s velocity is not changing, as we have no information to support any other conclusion.

$$\large x' = x + \Delta t \dot{x}$$

$$\large \dot{x}' = \dot{x}$$

Let’s see how these two equations would look if they were re-written as a linear algebra equation. On the left, is the posterior state (denoted with the prime symbol,  $\large '\space$), and on the right are the state transition function and the prior state. This equation shows how the state changes over the time period,  $\large \Delta t$. Note that we are working with the means here; the covariance matrix will appear later.

$$\large \begin{bmatrix} x \\ \dot{x} \end{bmatrix}' = \begin{bmatrix} 1 & \Delta{t} \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \end{bmatrix}$$
$$Posterior \space State = State \space Transition \multiply Prior \space State$$
If you multiply the matrices out, you have the same position update and velocity update as you had above. The new position is the prior position plus the time traveled multiplied by the velocity. The velocity is unchanged.

If we make the same assumption about the Kalman Filter executing one iteration per second as we did before, the matrix can be simplified to the one seen below.

$$\large \begin{bmatrix} x \\ \dot{x} \end{bmatrix}' = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \end{bmatrix}$$

The State Transition Function is denoted  FF, and the formula can be written as so,

$$\large x' = Fx$$

In reality, the equation should also account for process noise, as its own term in the equation. However, process noise is a Gaussian with a mean of 0, so the update equation need not include it because we are only working with the means.

$$\large x' = Fx + noise$$

$$\large noise \sim N(0,Q)$$

Now, what happens to the covariance? How does it change in this process? Your intuition may suggest that it would be affected by the state transition function, and also by some additional uncertainty arising from the estimation. If so, you’re correct!

**Sidenote:**  While it is common to use $\Sigma$  to represent the covariance of a Gaussian distribution in mathematics, it is more common to use the letter  $P$  to represent the state covariance in localization.

To calculate the posterior covariance, the prior covariance would be multiplied by the state transition function, and  $Q$  added as an increase of uncertainty due to process noise.  $Q$  can account for a robot slowing down unexpectedly, or being drawn off course by an external influence.

$$\large P' = FPF^T + Q$$

We’ve updated the mean and the covariance as part of the state prediction. Next, the measurement update must be converted to a similar linear algebra form.

## Measurement Update

In our example, the measurements are only of the location, therefore the measurement function is very simple - a matrix containing a one and a zero. This matrix demonstrates how to map the state to the observation,  $z$.

$$\large z = \begin{bmatrix} 1 & 0 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \end{bmatrix}$$

This matrix, called the Measurement Function, is denoted  $H$.

For the measurement update step, there are a few formulas. First, we calculate the measurement residual,  $y$. The measurement residual is the difference between the measurement and the expected measurement, based on prior state (ie. where the measurement tells us where we are vs. where we think we are). The measurement residual will be used later on in a formula.

$$\large y = z - Hx'$$

Next, it's time to consider the measurement noise, denoted  RR. This formula maps the state prediction covariance into the measurement space and adds the measurement noise. The result,  $S$, will be used in the following equation to calculate the Kalman Gain.

$$\large S = HP'H^T + R$$

These equations need not be memorized, instead they can be referred to in text or implemented in code for use and reuse.

## Kalman Gain

Next, we calculate the Kalman Gain, K. As you will see in the next equation, the Kalman Gain determines how much weight should be placed on the state prediction, and how much on the measurement update. It is an averaging factor that changes depending on the uncertainty of the state prediction and measurement update.

$$\large K = P'H^TS^{-1}$$

$$\large x = x' + Ky$$

These equations may look complicated and intimidating, but they do nothing more than calculate an average factor. Let’s work through a quick example to gain a better understanding of this.

[![](https://preview.ibb.co/cGOBtT/capture_screen_2.png)](https://youtu.be/K-FobmdRMtI)

The last step in the Kalman Filter is to update the new state’s covariance using the Kalman Gain.

$$\large P = (I - KH)P'$$

## Kalman Filter Equations

These are the equations that implement the Kalman Filter in multiple dimensions.

$$\large x' = Fx$$

$$\large P' = FPF^T + Q$$

$$\large y = z - Hx'$$

$$\large S = HP'H^T + R$$

$$\large K = P'H^TS^{-1}$$

$$\large x = x' + Ky$$

$$\large P = (I - KH)P'$$

The Kalman Filter can successfully recover from inaccurate initial estimates, but it is very important to estimate the noise parameters as accurately as possible - as they are used to determine which of the estimate or the measurement to believe more.

Now it’s your chance to code the multi-dimensional Kalman Filter in C++. The code below uses the C++  `eigen`  library to define matrices and easily compute their inverse and transpose. Check out the  `eigen`library full documentation  [here](https://eigen.tuxfamily.org/dox/group__QuickRefPage.html)  and go through some of their examples. Here's a list of useful commands that you'll need while working on this quiz:

-   Initializing a 2x1 float matrix  **K**:  `MatrixXf K(2, 1)`;
-   Inserting values to matrix  **K**:  `K << 0, 0`
-   Computing the transpose of matrix  **K**:  `K.transpose()`
-   Computing the inverse of matrix  **K**:  `K.inverse()`

```
#include <iostream>

#include <math.h>

#include <tuple>

#include "Core" // Eigen Library

#include "LU" // Eigen Library

using namespace std;

using namespace Eigen;

float measurements[3] = { 1, 2, 3 };

tuple<MatrixXf, MatrixXf> kalman_filter(MatrixXf x, MatrixXf P, MatrixXf u, MatrixXf F, MatrixXf H, MatrixXf R, MatrixXf I)

{

for (int n = 0; n < sizeof(measurements) / sizeof(measurements[0]); n++) {

//****** TODO: Kalman-filter function********//

// Measurement Update

// Code the Measurement Update

// Initialize and Compute Z, y, S, K, x, and P

// Prediction

// Code the Prediction

// Compute x and P
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU4NDgwMzg0NCwtMTQyMTI4MjA3MCw3Mj
g3Mzg1MzhdfQ==
-->