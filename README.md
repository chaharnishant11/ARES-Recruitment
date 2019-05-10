# ARES-Recruitment

##SENSOR FUSION FOR DRIFT CORRECTION
There is a 9-DOF IMU which consisits of a 6-DOF( 3-axis accelerometer combined with a 3-axis gyroscope ) combined with a magnetometer to find and measure the location of the rover and the drift correction variables namely pitch, yaw and roll.

###Kalman Filter
**What is Kalman Filter?**
A kalman filter is an optimal estimation algorithm. One can use a kalman filter in any place where you have some uncertain information about some dynamic system and you have to make an educated guess about what will the system is going to do next.
Kalman filters are ideal for systems which are continiously changing.The advantange of using kalman filters is that they are light on memory as they don't have to keep any history of the previous state, they are very fast and therefore well suited for realtime and embedded systems.

We have the current state of the system and we predict the future state of the system using maths. We take in account all the factors that can affect the system whether it be internal factors or external uncertainty.

We have one gaussian blob we got from our predictions and others from the sensors. We multiply them and the resultant is a new gaussian blob with it's own mean and covariance matrix.
