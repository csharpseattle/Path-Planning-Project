# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

### Introduction
In this project I navigate a vehicle in simulated highway traffic around an approximate 4 1/3 mile loop. A speed of 50 MPH is maintained whenever possible and lane changes are made when necessary.  In addition, the following constraints are imposed on the motion of the car as it makes its loop around the track:
1. The car should not go over 50 MPH.
2. Maximum acceleration of 10 m/s^2
3. Maximum jerk of 10 m/s^3.
4. The car should only be outside of a lane for a few seconds when making lane changes.
5. The car should not collide with any other vehicles on the road.

### Processing Sensor Fusion Data
The function `processSensorFusion()`, line 239 of `main.cpp`, processes sensor fusion data multiple times each second and provides a recommendation (Stay in Lane, Change lane left, or Change lane right). In addition, a recommended speed is also calculated if a lane change is not possible.
To calculate the cost of a lane the distance and speed of the vehicles immediately surrounding our car are calculated. The approximate future position of surrounding vehicles at the time of the end of our current path is used to determine if a lane is safe or we need to move from the lane we are in.

### Path Planning
Path planning is done after the call to `processSensorFusion()` on line 504 of `main.cpp`.  To create a path that kept motion within the contraints of maximum acceleration and maximum jerk I used the spline tool recommended in the class.  First, on lines 510 - 537, the recommended lane change and speed changes coming out of sension fusion are made.   Since we need at least 2 points to generate a spline we either calculate 2 points based on our current position and heading or use 2 unprocessed points left over from the previous frame's path.  The spline control points are then augmented with a few more points further out.  These spline points are then used to interpolate many points along the updated path.  The new points are then concatenated with the list of previous points and sent back over the socket to the simulator.
