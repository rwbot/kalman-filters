# 1D Kalman Filter

In the programming quiz below, write the code that will iteratively go through the available measurements and motions, and apply a measurement update or a state prediction to each one of them.

[Video Lesson](https://www.youtube.com/watch?v=1nHSG4U_v2g)

```
#include <iostream>
#include <math.h>
#include <tuple>
using namespace std;
double new_mean, new_var;

tuple<double, double> measurement_update(double mean1, double var1, double mean2, double var2)
{
	new_mean = (var2 * mean1 + var1 * mean2) / (var1 + var2);
	new_var = 1 / (1 / var1 + 1 / var2);
	return make_tuple(new_mean, new_var);
}

tuple<double, double> state_prediction(double mean1, double var1, double mean2, double var2)
{
	new_mean = mean1 + mean2;
	new_var = var1 + var2;

	return make_tuple(new_mean, new_var);
}

int main()
{
	//Measurements and measurement variance
	double measurements[5] = { 5, 6, 7, 9, 10 };
	double measurement_sig = 4;
	//Motions and motion variance
	double motion[5] = { 1, 1, 2, 1, 1 };
	double motion_sig = 2;
	//Initial state
	//Initial state
    double mu = 0;
    double sig = 1000;
    
    //######TODO: Put your code here below this line######//
    // Loop through all the measurments
    for (int i = 0; i < sizeof(measurements) / sizeof(measurements[0]); i++)
    {
        tie(mu, sig) = measurement_update(mu, sig, measurements[i], measurement_sig);
        printf("update:  [%f, %f]\n", mu, sig);
        tie(mu, sig) = state_prediction(mu, sig, motion[i], motion_sig);
        printf("predict: [%f, %f]\n", mu, sig);
    }
    
    return 0;
}

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3NzU4OTA1N119
-->