# ARES-Recruitment

## SENSOR FUSION FOR DRIFT CORRECTION
There is a 9-DOF IMU which consisits of a 6-DOF( 3-axis accelerometer combined with a 3-axis gyroscope ) combined with a magnetometer to find and measure the location of the rover and the drift correction variables namely pitch, yaw and roll.

#### Kalman Filter
**What is Kalman Filter?**
A kalman filter is an optimal estimation algorithm. One can use a kalman filter in any place where you have some uncertain information about some dynamic system and you have to make an educated guess about what will the system is going to do next.
Kalman filters are ideal for systems which are continiously changing.The advantange of using kalman filters is that they are light on memory as they don't have to keep any history of the previous state, they are very fast and therefore well suited for realtime and embedded systems.

We have the current state of the system and we predict the future state of the system using maths. We take in account all the factors that can affect the system whether it be internal factors or external uncertainty.

We have one gaussian blob we got from our predictions and others from the sensors. We multiply them and the resultant is a new gaussian blob with it's own mean and covariance matrix. we find out the kalman factor/ kalman gain.

Kalman filter is used to model linear systems and for nonlinear systems we have **Extended Kalman Filter**.

#### Extended Kalman Filter
In real life most of the problems involve non linear functions.If we feed a Gaussian with a linear function then the output is also gaussian but in case of a non linear(angles,sine and cosine functions) function the output is a non gaussian distribution.

We use approximation to make a non linear funtion linear with the help of Taylor series. We use the first derivative of the linear function obtained by the Taylor series and feed it to the Gaussian function. Then we just do the same series of operations we do with a normal kalman filter.

So in our case we use 2 Extended kalman filters. One is used to find out the pitch,yawn and roll of the rover,the extended kalman filter is feeded with the output given by the 9-DOF IMU and is upated with the help of output of the stereo visual odometer from the **localization thread**. The output is feeded to the local_map_optimization unit of the localization thread.

The other filter is used to find out the velocity and cooridnates of the rover, it is feeded with the same output from the 9-DOF IMU in addition with the longitude and latitude given by the GPS module. The output is feeded to the global_map_representation unit of the **localization thread**.

## Localization Thread
