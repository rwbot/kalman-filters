# Measurement Update

#### Mean Calculation

$\mu$: Mean of the prior belief  
$\sigma^{2}$: Variance of the prior belief  
$v$: Mean of the measurement  
$r^{2}$: Variance of the measurement

#### New Belief Quiz

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/0976a728-e605-4149-9074-cba310f02b9f#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/January/5a6d0b71_c2l2-a10-measurement-update-in-1d-quiz.00-00-46-29.still001/c2l2-a10-measurement-update-in-1d-quiz.00-00-46-29.still001.png)



The new mean is a weighted sum of the prior belief and measurement means. With uncertainty, a larger number represents a more uncertain probability distribution. However, the new mean should be biased towards the measurement update, which has a smaller standard deviation than the prior. How do we accomplish this?

$$\huge \mu' = \frac{r^2 \mu + \sigma^2 v}{r^2 + \sigma^2}$$

The answer is - the uncertainty of the prior is multiplied by the mean of the measurement, to give it more weight, and similarly the uncertainty of the measurement is multiplied with the mean of the prior. Applying this formula to our example generates a new mean of 27.5, which we can label on our graph below.

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/0976a728-e605-4149-9074-cba310f02b9f#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/February/5a77fa82_c2l2-graph-1-v1/c2l2-graph-1-v1.png)

#### Variance Calculation

Next, we need to determine the variance of the new state estimate.

The two Gaussians provide us with more information together than either Gaussian offered alone. As a result, our new state estimate is more confident than our prior belief and our measurement. This means that it has a higher peak and is narrower. You can see this in the graph below.

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/0976a728-e605-4149-9074-cba310f02b9f#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/January/5a6a7575_c2l2-graph-2/c2l2-graph-2.png)

The formula for the new variance is presented below.

$$\huge \sigma^{2'} = \frac{1}{\frac{1}{r^2} + \frac{1}{\sigma^2}}$$

Entering the variances from our example into this formula produces a new variance of 2.25. The new state estimate, often called the posterior, is drawn below.

Entering the variances from our example into this formula produces a new variance


[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/0976a728-e605-4149-9074-cba310f02b9f#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/January/5a6cfd03_c2l2-graph-3/c2l2-graph-3.png)

It’s time to implement these two formulas in C++. Place your code within a function called measurement_update, such that you can use it as a building block in your Kalman Filter implementation.


#### Programming Quiz

In this C++ code, the  **measurement update**  function returns two values: the newly computed mean and variance. Usually, a  `tuple`  or  `struct`should be used in C++ to return more than one value from a function and easily assign them later to multiple variables. For more information on  `tuples`  and  `structs`  take a look at this  [link](https://dzone.com/articles/returning-multiple-values-from-functions-in-c).

```
#include <iostream>
#include <math.h>
#include <tuple>
using namespace std;

double new_mean, new_var;

// mean1 = mu variance1 = sigma_sqr
// mean2 = v variance2 = r_sqr

tuple<double, double> measurement_update(double mean1, double variance1, double mean2, double variance2)
{

//TODO: Code the measurment update mean function mu;
new_mean = ( ((variance2 * mean1) + (variance1 * mean2)) / (variance2 + variance1) );
//TODO: Code the measurment update variance function sigma square;
new_var = ( 1 / ((1/variance2) + (1/variance1)) );

return make_tuple(new_mean, new_var);
}

int main()
{
// Main data
tie(new_mean, new_var) = measurement_update(10, 8, 13, 2);
// Post-Quiz Test
//tie(new_mean, new_var) = measurement_update(20, 5, 30, 5);
printf("[%f, %f]", new_mean, new_var);
return 0;
}
```

I encourage you to think about what the posterior Gaussian would look like for the following example, and even calculate the exact values using your measurement_update function.

[](https://classroom.udacity.com/nanodegrees/nd209/parts/dad7b7cc-9cce-4be4-876e-30935216c8fa/modules/f5048868-4bd8-4e8d-8c6b-69bd559ed9db/lessons/f002d591-94af-4c70-aeac-ac2ed6f7b527/concepts/0976a728-e605-4149-9074-cba310f02b9f#)

![](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/January/5a6a705f_c2l2-graph-4-v2/c2l2-graph-4-v2.png)


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMzU2NzY0MzNdfQ==
-->