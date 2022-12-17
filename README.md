# LiDAR-attack-on-Autonomous-Vehicles

**You are requested to give proper Acknowledgement/ References to the author in order to use the images used below. **

![image](https://user-images.githubusercontent.com/70091050/208250556-8e6f4bcc-156f-48b4-bc29-f45d82217802.png)

By adding mesh to the point cloud or obscuring the images being taken by the real-time camera on the autonomous car, we hope to better understand how attacks against LIDAR work. The main goal is to figure out how these attacks on LIDAR and cameras reduce the autonomous vehicle’s accuracy in detection of objects on the road while driving.
 ![image](https://user-images.githubusercontent.com/70091050/208250677-ae2c14fe-5dad-4ba0-bb2c-ac9631e9e76e.png)

Figure: Symbolic representation of how point cloud of real-time LiDAR/ camera detects vehicles, pedestrians, cyclists etc.


1	Attack Methodology

To work on a model of realistic adversarial attack on the camera and LIDAR, we need a realistic representation of the 3D physical environment. To create the adversarial attack, we add a mesh on the point cloud that is formed by the camera/LIDAR data in real-time, for our project we are using KITTI dataset that contains frames of road driving videos. In order to prevent the roadside objects from being successfully recognized during this attack, we need to know where the LIDAR rays will meet in order to create a mesh on the point cloud.

To move forward with adversarial attack with the use of KITTI dataset we first take the frame captures from the dataset, make the depth maps of the 2D images to generate and work on 3D projections. The KITTI dataset, which offers camera calibration matrices for all cameras used, a rectification matrix to rectify the geometric alignment between cameras, and transformation matrices for stiff physical transformation between multiple sensors, is used to create the depth map. The illustration below demonstrates the many projections needed when using LiDAR data.
![image](https://user-images.githubusercontent.com/70091050/208250658-0029cbaf-6690-4e1e-ab4d-d57052ed1659.png)

 
Figure: Sensor setup (camera, LiDAR and GPS/IMU) of the KITTI dataset through a symbolic representation

In the depth map matrix has the min value as 0 which represents the noise i.e there is no distance while the max value 2980 represents the distance of the farthest pixels.


2	Depth Camera Calibration

Calibrating a camera means estimating lens and sensor parameters by finding the distortion coefficients and the camera matrix also called the intrinsic parameters. In general, there are three methods for calibrating a camera: using the standard parameters provided by the factory, using the results obtained in calibration research or calibrating the Kinect manually. Calibrating the camera manually consists of applying one of the calibration algorithms such as the chess-board algorithm [18]. This algorithm is implemented in Robot Operating System (ROS) and OpenCV. The calibration matrix M is a 3×3 matrix:
          ![image](https://user-images.githubusercontent.com/70091050/208250744-489112c0-f856-47cf-9430-5775f8795a82.png)
                                                                                      
Here, fx, fy and cx, cy are the focal length and the optical centers respectively. These values can be obtained from the KITTI dataset Velodyne data matrices.
.
 
3	Point Cloud Computing

Computing point cloud means transforming the depth pixel from the depth image 2D coordinate system to the depth camera 3D coordinate system (x, y and z). The 3D coordinates are computed using the following formulas [19], where depth (i, j) is the depth value at the row i and column j and the values of fx, fy, cx,cy are the same as above, representing focal lengths and optical centres.  
![image](https://user-images.githubusercontent.com/70091050/208250612-2abc94c6-78ba-4178-9a61-6de67f50b105.png)

Figure: KITTI sensor coordinate transformations
The point cloud is made using MATLAB
