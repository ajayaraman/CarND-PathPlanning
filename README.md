# CarND-Path-Planning-Project
Path Planning project term 3 of Self-Driving Car Engineer Nanodegree Program
   
### Goals of the project
A program that outputs a path comprising of waypoints to drive a self-driving-car around a highway track with 2 way traffic while following rules and causing minimal jerk to provide a comfortable experience. Ultimately it has to satisfy the following [rubrics](https://review.udacity.com/#!/projects/318/rubric)

### Setup and Compilation 
For instructions for compiling the project please refer to this [link](https://github.com/udacity/CarND-Path-Planning-Project)

### Final result

Link to youtube video of final simulation result

[![Path Planning](https://img.youtube.com/vi/Cg1LbXJaZtY/0.jpg)](https://youtu.be/Cg1LbXJaZtY)

### Path Planner Details
The path planner module developed for this project comprises of the following modules
1. Behavior Module
2. Trajectory Module using splines

#### Behavior Module
The behavior planner uses high level logic for the upcoming few seconds in order to decide whether the vehicle needs to stay in its lane or needs to change lanes. The resulting decision of the behavior module is used by the trajectory planner to generate the desired trajectory to execute the behavior planner. In this project the behavior module needed is very compact and runs at the same rate as the trajectory generator.

The behavior module is comprised of a state machine with the following states
1. Keep Lane 
2. Lane Change Left
3. Lane Change Right

To transition between each of these states a set of cost functions is evaluated. Then the state with the minimum cost is chosen. For instance, to keep the vehicle from changing lanes when vehicles exist in other lanes, the cost function assigns a high cost if the sensor-fusion module has a car in the adjacent lane such that when the safe gap required for transitioning to the next lane is not satisfied, that state will have a high cost and hence will not be chosen. A lot of this code is in lines 282 - 372 in [main.cpp](src/main.cpp).

#### Trajectory Generator Module
To generate smooth trajectories with minimum lateral and s-direction jerk, a spline based model is used. The spline is fit between 50 points. These 50 points are determined by the car longitudinal (s) and lateral(d) coordinates as well as the desired longitudinal and lateral directions. A trick used to keep the path smooth is to reuse unexecuted path coordinates from a previous time-step and only fill in future values. This allows the path to avoid varying too much. The [opensource spline c++ library](http://kluge.in-chemnitz.de/opensource/spline/) is used for heavylifting most of the spline curve fitting.

### Areas for further improvement
Currently the vehicle is able to successfully and smoothly navigate the highway track. It transitions smoothly to a different lane if it is blocked in front and needs to slow down. But improvements are feasible.

1. Use a prediction module to predict how vehicles will move in the future and incorporate such predictions in the behavior module when evaluating cost functions to decide state transitions.
2. Run behavior module at a slower rate than the trajectory generator to avoid making too many quick decisions and also given sufficient reaction time to the vehicle to execute the planned path.
3. Add a two additional states for preparing for lane change to the left or right lane. These two additional states will allow the vehicle to match the speed of the vehicle in the lane we wish to transition to, so that the ego vehicle can make these transitions smoother.
4. Better seperation in code between the behavior module and the trajectory generation module.  