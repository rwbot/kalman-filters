# Design of Multi-Dimensional Kalman Filters

[![](https://preview.ibb.co/f60S08/capture_screen_1.png)](https://www.youtube.com/watch?v=9Xb5WavDqKE)


To implement a two dimensional Kalman Filter, you will need to apply several linear algebra equations. To begin with, let’s convert the state prediction into linear algebra form. This is helpful because linear algebra allows us to work easily with multi-dimensional problems.

## State Transition

We know that the update formula below represents the relationship between the robot’s position,  <img src="https://tex.s2cms.ru/svg/%5Clarge%20x" alt="\large x" />, and velocity,  <img src="https://tex.s2cms.ru/svg/%5Clarge%20%5Cdot%7Bx%7D" alt="\large \dot{x}" />. We can also assume that the robot’s velocity is not changing, as we have no information to support any other conclusion.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x'%20%3D%20x%20%2B%20%5CDelta%20t%20%5Cdot%7Bx%7D" alt="\large x' = x + \Delta t \dot{x}" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20%5Cdot%7Bx%7D'%20%3D%20%5Cdot%7Bx%7D" alt="\large \dot{x}' = \dot{x}" />

Let’s see how these two equations would look if they were re-written as a linear algebra equation. On the left, is the posterior state (denoted with the prime symbol,  <img src="https://tex.s2cms.ru/svg/%5Clarge%20'%5Cspace" alt="\large '\space" />), and on the right are the state transition function and the prior state. This equation shows how the state changes over the time period,  <img src="https://tex.s2cms.ru/svg/%5Clarge%20%5CDelta%20t" alt="\large \Delta t" />. 

<img src="https://tex.s2cms.ru/svg/%5Clarge%20%5Cbegin%7Bbmatrix%7D%20x%20%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bbmatrix%7D'%20%3D%20%5Cbegin%7Bbmatrix%7D%201%20%26%20%5CDelta%7Bt%7D%20%5C%5C%200%20%26%201%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20x%20%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bbmatrix%7D" alt="\large \begin{bmatrix} x \\ \dot{x} \end{bmatrix}' = \begin{bmatrix} 1 &amp; \Delta{t} \\ 0 &amp; 1 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \end{bmatrix}" />

<img src="https://tex.s2cms.ru/svg/Posterior%20%5Cspace%20State%20%3D%20State%20%5Cspace%20Transition%20*Prior%20%5Cspace%20State" alt="Posterior \space State = State \space Transition *Prior \space State" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x'%20%3D%20Fx" alt="\large x' = Fx" />

### Note that we are working with the means here; the covariance matrix will appear later.

If you multiply the matrices out, you have the same position update and velocity update as you had above. The new position is the prior position plus the time traveled multiplied by the velocity. The velocity is unchanged.

If we make the same assumption about the Kalman Filter executing one iteration per second as we did before, the matrix can be simplified to the one seen below.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20%5Cbegin%7Bbmatrix%7D%20x%20%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bbmatrix%7D'%20%3D%20%5Cbegin%7Bbmatrix%7D%201%20%26%201%20%5C%5C%200%20%26%201%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20x%20%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bbmatrix%7D" alt="\large \begin{bmatrix} x \\ \dot{x} \end{bmatrix}' = \begin{bmatrix} 1 &amp; 1 \\ 0 &amp; 1 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \end{bmatrix}" />

The State Transition Function is denoted  <img src="https://tex.s2cms.ru/svg/F" alt="F" />, and the formula can be written as so,

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x'%20%3D%20Fx" alt="\large x' = Fx" />

In reality, the equation should also account for process noise, as its own term in the equation. However, process noise is a Gaussian with a mean of 0, so the update equation need not include it because we are only working with the means.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x'%20%3D%20Fx%20%2B%20noise" alt="\large x' = Fx + noise" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20noise%20%5Csim%20N(0%2CQ)" alt="\large noise \sim N(0,Q)" />

Now, what happens to the covariance, <img src="https://tex.s2cms.ru/svg/P" alt="P" />? How does it change in this process? Your intuition may suggest that it would be affected by the state transition function, and also by some additional uncertainty arising from the estimation. If so, you’re correct!

**Sidenote:**  While it is common to use <img src="https://tex.s2cms.ru/svg/%5CSigma" alt="\Sigma" />  to represent the covariance of a Gaussian distribution in mathematics, it is more common to use the letter  <img src="https://tex.s2cms.ru/svg/P" alt="P" />  to represent the state covariance in localization.

To calculate the posterior covariance <img src="https://tex.s2cms.ru/svg/P'" alt="P'" />, the prior covariance <img src="https://tex.s2cms.ru/svg/P" alt="P" /> would be multiplied by the state transition function <img src="https://tex.s2cms.ru/svg/F" alt="F" />, and  <img src="https://tex.s2cms.ru/svg/Q" alt="Q" />  added as an increase of uncertainty due to process noise.  <img src="https://tex.s2cms.ru/svg/Q" alt="Q" />  can account for a robot slowing down unexpectedly, or being drawn off course by an external influence.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20P'%20%3D%20FPF%5ET%20%2B%20Q" alt="\large P' = FPF^T + Q" />

We’ve updated the mean and the covariance as part of the state prediction. Next, the measurement update must be converted to a similar linear algebra form.

## Measurement Update

In our example, the measurements are only of the location, therefore the measurement function is very simple - a matrix containing a one and a zero. This matrix demonstrates how to map the state to the observation,  <img src="https://tex.s2cms.ru/svg/z" alt="z" />.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20z%20%3D%20%5Cbegin%7Bbmatrix%7D%201%20%26%200%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20x%20%5C%5C%20%5Cdot%7Bx%7D%20%5Cend%7Bbmatrix%7D" alt="\large z = \begin{bmatrix} 1 &amp; 0 \end{bmatrix} \begin{bmatrix} x \\ \dot{x} \end{bmatrix}" />

This matrix, called the **Measurement Function**, is denoted  <img src="https://tex.s2cms.ru/svg/H" alt="H" />.

For the measurement update step, there are a few formulas. First, we calculate the **Measurement Residual**,  <img src="https://tex.s2cms.ru/svg/y" alt="y" />. The measurement residual is the difference between the measurement and the expected measurement, based on prior state **(ie. where the measurement tells us where we are vs. where we think we are)**. The measurement residual will be used later on in a formula.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20y%20%3D%20z%20-%20Hx'" alt="\large y = z - Hx'" />

Next, it's time to consider the measurement noise, denoted  <img src="https://tex.s2cms.ru/svg/R" alt="R" />. This formula maps the state prediction covariance into the measurement space and adds the measurement noise. The result,  <img src="https://tex.s2cms.ru/svg/S" alt="S" />, will be used in the following equation to calculate the Kalman Gain.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20S%20%3D%20HP'H%5ET%20%2B%20R" alt="\large S = HP'H^T + R" />

These equations need not be memorized, instead they can be referred to in text or implemented in code for use and reuse.

## Kalman Gain

Next, we calculate the Kalman Gain, <img src="https://tex.s2cms.ru/svg/K" alt="K" />. As you will see in the next equation, the Kalman Gain **determines how much weight should be placed on the state prediction, and how much on the measurement update.** It is an averaging factor that changes depending on the uncertainty of the state prediction and measurement update.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20K%20%3D%20P'H%5ETS%5E%7B-1%7D" alt="\large K = P'H^TS^{-1}" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x%20%3D%20x'%20%2B%20Ky" alt="\large x = x' + Ky" />

These equations may look complicated and intimidating, but they do nothing more than calculate an average factor. Let’s work through a quick example to gain a better understanding of this.

[![](https://preview.ibb.co/cGOBtT/capture_screen_2.png)](https://youtu.be/K-FobmdRMtI)

The last step in the Kalman Filter is to update the new state’s covariance using the Kalman Gain.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20P%20%3D%20(I%20-%20KH)P'" alt="\large P = (I - KH)P'" />

## Kalman Filter Equations

These are the equations that implement the Kalman Filter in multiple dimensions.

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x'%20%3D%20Fx" alt="\large x' = Fx" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20P'%20%3D%20FPF%5ET%20%2B%20Q" alt="\large P' = FPF^T + Q" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20y%20%3D%20z%20-%20Hx'" alt="\large y = z - Hx'" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20S%20%3D%20HP'H%5ET%20%2B%20R" alt="\large S = HP'H^T + R" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20K%20%3D%20P'H%5ETS%5E%7B-1%7D" alt="\large K = P'H^TS^{-1}" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20x%20%3D%20x'%20%2B%20Ky" alt="\large x = x' + Ky" />

<img src="https://tex.s2cms.ru/svg/%5Clarge%20P%20%3D%20(I%20-%20KH)P'" alt="\large P = (I - KH)P'" />

| Symbol         | Meaning                                              | Formula                    |
|----------------|------------------------------------------------------|----------------------------|
| x              | Prior State                                          |                            |
| x'             | Posterior State                                      | <img src="https://tex.s2cms.ru/svg/%5Clarge%20x'%20%3D%20Fx" alt="\large x' = Fx" />         |
| F              | State Transition Function                            |                            |
| P              | Prior Covariance                                     |                            |
| P'             | Posterior Covariance                                 | <img src="https://tex.s2cms.ru/svg/%5Clarge%20P'%20%3D%20FPF%5ET%20%2B%20Q" alt="\large P' = FPF^T + Q" />  |
| z              | Observation                                          |                            |
| H              | Measurement Function                                 |                            |
| y              | Measurement Residual                                 | <img src="https://tex.s2cms.ru/svg/%5Clarge%20y%20%3D%20z%20-%20Hx'" alt="\large y = z - Hx'" />     |
| R              | Measurement Noise                                    |                            |
| S              | State Prediction + Meas Noise                        | <img src="https://tex.s2cms.ru/svg/%5Clarge%20S%20%3D%20HP'H%5ET%20%2B%20R" alt="\large S = HP'H^T + R" />  |
| K              | Kalman Gain                                          | <img src="https://tex.s2cms.ru/svg/%5Clarge%20K%20%3D%20P'H%5ETS%5E%7B-1%7D" alt="\large K = P'H^TS^{-1}" /> |
|                | Not sure about the ones below                        |                            |
| <img src="https://tex.s2cms.ru/svg/P_%7Bnew%7D" alt="P_{new}" /> ?? | New State Covariance Updated  with Kalman Gain----- | <img src="https://tex.s2cms.ru/svg/%5Clarge%20P%20%3D%20(I%20-%20KH)P'" alt="\large P = (I - KH)P'" />  |
| <img src="https://tex.s2cms.ru/svg/x_%7Bnew%7D" alt="x_{new}" /> ?? | New State Updated  with Kalman Gain ???              | <img src="https://tex.s2cms.ru/svg/%5Clarge%20x%20%3D%20x'%20%2B%20Ky" alt="\large x = x' + Ky" />     |


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
#include "LU"   // Eigen Library
using namespace std;
using namespace Eigen;
float measurements[3] = { 1, 2, 3 };

tuple<MatrixXf, MatrixXf> kalman_filter(MatrixXf x, MatrixXf P, MatrixXf u, MatrixXf F, MatrixXf H, MatrixXf R, MatrixXf I)
{
    for (int n = 0; n < sizeof(measurements) / sizeof(measurements[0]); n++) {
        //****** TODO: Kalman-filter function********//
        // Measurement Update
        // Initialize and Compute Z, y, S, K, x, and P
        MatrixXf Z(1, 1);
        Z << measurements[n];

        MatrixXf y(1, 1);
        y << Z - (H * x);

        MatrixXf S(1, 1);
        S << H * P * H.transpose() + R;

        MatrixXf K(2, 1);
        K << P * H.transpose() * S.inverse();

        x << x + (K * y);

        P << (I - (K * H)) * P;
        
        // Prediction
        // Compute x and P
        x << (F * x) + u;
        P << F * P * F.transpose();
    }
    return make_tuple(x, P);
}


int main()
{

    MatrixXf x(2, 1);// Initial state (location and velocity) 
    x << 0,
    	 0; 
    MatrixXf P(2, 2);//Initial Uncertainty
    P << 100, 0, 
    	 0, 100; 
    MatrixXf u(2, 1);// External Motion
    u << 0,
    	 0; 
    MatrixXf F(2, 2);//Next State Function
    F << 1, 1,
    	 0, 1; 
    MatrixXf H(1, 2);//Measurement Function
    H << 1,
    	 0; 
    MatrixXf R(1, 1); //Measurement Uncertainty
    R << 1;
    MatrixXf I(2, 2);// Identity Matrix
    I << 1, 0,
    	 0, 1; 

    tie(x, P) = kalman_filter(x, P, u, F, H, R, I);
    cout << "x= " << x << endl;
    cout << "P= " << P << endl;

    return 0;
}
```
