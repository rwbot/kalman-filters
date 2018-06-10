# State Prediction
[Video Lesson](https://youtu.be/mjBpoGmNaqU)

## State Prediction Formulas

$$Posterior\ Mean \ \ \ \ \ \ \ \huge \mu' = \mu_1 + \mu_2$$

$$Posterior\ Variance \ \ \ \ \huge \sigma^{2'} = \sigma^2_1 + \sigma^2_2
$$
```
#include <iostream>
#include <math.h>
#include <tuple>
using namespace std;

double new_mean, new_var;

tuple<double, double> state_prediction(double mean1, double variance1, double mean2, double variance2)
{

//TODO: Code the state prediction mean function mu;
new_mean = mean1 + mean2;
//TODO: Code the state prediction variance function sigma_sqr;
new_var = variance1 + variance2;

return make_tuple(new_mean, new_var);
}

int main()
{
tie(new_mean, new_var) = state_prediction(10, 4, 12, 4);
printf("[%f, %f]", new_mean, new_var);
return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQwMjIzNjgzMF19
-->