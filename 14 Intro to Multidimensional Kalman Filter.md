
In our one-dimensional examples the system state was represented by one variable. In n-dimensional systems the state is a vector with n state variables

If we are to explore a 2-dimensional example these two state variables can be the $\large X$ and $\large Y$ position of a robot:
$$\begin{bmatrix}
 x  \\
 y 
\end{bmatrix}$$

or if we're looking for a more interesting example, it could be the position and velocity of a robot:

$$\begin{bmatrix}
 x  \\
 \dot{x} 
\end{bmatrix}$$

This leads right into another important matter. When working in one dimension your state has to be **observable** meaning that it had to be something that can be **measured directly**. In multidimensional States there may exist hidden state variables, ones that you cannot measure with the sensors available.

  However, you may be able to infer their value from other states and measurements. In this example, the location of the robot is observable while its velocity is not making it a hidden state variable.

![](https://preview.ibb.co/c7OHiT/054.png)

However, a robot's position and velocity over time are linked through a very simple formula

$$\large x' = x + \Delta t \dot{x}$$

Let's look at this on a graph. If initially the position of the robot is known but it's velocity is not the estimate of the robot state would look like so:

![](https://preview.ibb.co/n2JiOT/108.png)
  

A Gaussian that is very narrow in the $\large x$ dimension representing confidence about the robot's location.

While in in the $\large \dot{x}$ dimension, its very wide since the robot's velocity is completely unknown:

![](https://preview.ibb.co/jyxXHo/117.png)
  

Next a state prediction can be calculated, this is where the Kalman filter starts to get exciting !!! Knowing the relationship between the hidden variable and observable variable is key.

Let's assume that one iteration of the Kalman filter takes 1 second. Now using the formula we can calculate the posterior state for each possible velocity $\large v$. For instance, if the velocity $\large v=0$ the posterior state would be identical to the prior. And if the velocity $\large v=1$ the posterior will be here and so forth from this, we can draw a posterior Gaussian that looks like so:

  

![](https://preview.ibb.co/m2BEV8/158.png)
  

This doesn't yet tell us anything about the velocity, it just graphs the correlation between the velocity and the location of the robot however, what do we have next? 

A measurement update!

Let's see if it will be able to provide us with sufficient information to identify the velocity of the robot. The initial belief was useful to calculate the state prediction but has no additional value and the result of the state prediction can be called the prior belief for our measurement update. Let's say that the new measurement suggests a location of $\large x=50$:

![](https://preview.ibb.co/nxyHiT/231.png)
  

Now if we apply the measurement update to our prior we will have a very small posterior centered around $\large x=50$ and $\large \dot{x}=15$:

![](https://preview.ibb.co/gv5Kxo/237.png)

This may look like magic but it's the exact same measurement update that you applied before implemented in two dimensions. Look, this was the prior belief and here is the measurement the posterior belief is just a weighted sum of the two and more confident than either the prior or the measurement:

![](https://preview.ibb.co/eWz1A8/257.png)
  
  

Then the relationship between the two dimensions narrows down the posterior for the $\large dot{x}$ axis. after all if the robot moved from $\large x=35$ equals $\large x=50$ in one second the speed should be trivial to calculate. And so it seems that two iterations of the Kalman filter cycle were enough to infer the robots velocity.

Continuing iterating through the measurement updates and state prediction steps will update the robot's internal state to keep it aligned with where it is in the real world.
<!--stackedit_data:
eyJoaXN0b3J5IjpbODcxMzExNzM0XX0=
-->