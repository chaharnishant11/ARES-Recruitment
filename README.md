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

##Stereo setup
A stereo camera is a type of camera with two or more image sensors. This allows the camera to simulate human binocular vision and therefore gives it the ability to perceive depth.

#### Human binocular vision
The human binocular vision perceives depth by using Stereo disparity which refers to the difference in image location of an object seen by the left and right eyes, resulting from the eyes’ horizontal separation.

The brain uses this binocular disparity to extract depth information from the two-dimensional retinal images which are known as stereopsis.

The rover is generating a 3D image if the surrounding using the 2 cameras which is then feeded to the localization thread.

## Localization Thread

### Stereo Visual odometry

#### What is Visual odometry?
VO is the process of estimating the camera’s relative motion
by analyzing a sequence of camera images. VO is defined as the process of estimating the robot’s motion
(translation and rotation with respect to a reference frame)
by observing a sequence of images of its environment. VO
is a particular case of a technique known as Structure From
Motion (SFM) that tackles the problem of 3D reconstruction
of both the structure of the environment and camera poses
from sequentially ordered or unordered image sets.

The output from the cameras is feeded to stereo_visual_odometry unit and a number of operations are perfored to convert the 3D to 2D motion estimation.

**1. Feature detection**
The 2 images from the stereo camera are feeded to this OpenCV function which detects features from the images.The output from this function is then passed to TriangulatePoints().

**2. Triangulate Points**
This function triangulates the 3d position of 2d correspondences between the two images.

**3. RANSAC()**
RANSAC is abbreviation of RANdom SAmple Consensus, in computer vision, we use it as a method to calculate homography between two images.



Resources used:
Kalman and extended kalman filter:
https://in.mathworks.com/videos/understanding-kalman-filters-part-1-why-use-kalman-filters--1485813028675.html
https://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/
https://towardsdatascience.com/extended-kalman-filter-43e52b16757d
Stereo Vision:
https://www.e-consystems.com/blog/camera/what-is-a-stereo-vision-camera/
http://www.cs.cmu.edu/~kaess/vslam_cvpr14/media/VSLAM-Tutorial-CVPR14-A12-StereoVO.pdf
Localisation thread:
https://medium.com/machine-learning-world/feature-extraction-and-similar-image-search-with-opencv-for-newbies-3c59796bf774
http://eric-yuan.me/ransac/
https://link.springer.com/article/10.1007/s40903-015-0032-7
