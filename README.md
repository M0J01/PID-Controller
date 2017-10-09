# PID Controller

## Description
This program implements a P-I-D controller in order to control the turning of a simulated vehicle around a track.
You can find a link to the simulator [here](https://github.com/udacity/self-driving-car-sim/releases).

The program uses a simulator provided CTE (Cross Track Error) to determine how far away it's target position is. The vehicle will
then adjust turning radius based off current and prior CTE values.

This project was completed as part of the UDACITY Self-Driving Car Engineer Nanodegree Program. For more information, or 
to enroll today, please check [here.](https://www.udacity.com/course/self-driving-car-engineer-nanodegree--nd013) 


---

## Main.cpp

This file controls the communication between the simulator and the program. 

It also contains the majority of the code used to calculate the turning rate of our vehicle, including;
* The initialization of our PID class, and our PID's initial Kp, Ki, Kd settings 
* Storing the sum of our total CTE, and our CTE's last value 
* Performing our steer_value claculation based off our current CTE **p**, the sum of our CTE **i**, and 
the difference between our current and previous CTE **d**.

## P. I. D. selection

Each value of the PID controller has a different affect on the turn rate of the vehicle.

#### **P** 
controls the speed at which the vehicle responds to our CTE. a high value of P is usefull for when the track curves
sharply. Our Vehicle will be able to keep up with turning rate if our value of P is set high enough. Unfortunately, High 
values of P also can cause our vehicle to over correct, and create a sinusoidal pattern of flight around our desired course 
(center of the track).

#### **D** 
controls our vehicles treatment of the derivative of our CTE between our current and last measurement. This helps
dampen and smooth our turning, which is useful to prevent abrupt turning of the vehicle when it is not required. Additionally
D helps significantly dampen the sinusoidal pattern caused by our P value. Unfortunately, with a high D value, our vehicle
can become "stuck" at a fixed distance away from our desired course.

#### **I** 
controls our vehicles treatment of the integral of our CTE over all time. higher the CTE becomes over time, the larger 
the correction factor I provides. This is very useful if the vehicle becomes stuck at a fixed point away from our desired 
course, or if our vehcile is not turning sharply enough around a turn.

### Selection process
Selection of PID values was performed manually in order to ensure that the vehicle was able to complete the track atleast
once. 
* P was addressed first, and tuned to an area where the vehicle was choosing turn rates well below the maximum. 
* D was then adjusted to dampen the oscillation around the desired course, and to ensure the vehicle was able to handle 
straight paths.
* I was then adjusted to ensure the value was high enough to implmenet course corrections when vehicle became fixed away
from our desired course, however low enough so as to avoid over powereing P and D. 
* P was then addressed again to ensure that the vehicle was capable of turning when required.
* D was then re addressed to smooth out the vehicle during straight aways.
* P, I, and D were then addressed in concert to handle the sharper turns, while ensuring the vehicle was stable during 
other manuevers. I was of crucial import for ensuring the vehicle was capable during sharp turns, as it provided extra 
turning power when the CTE continued to grow (as during sharp turns).

Ultimately, _Very High_ values of **D** were desirable, as were _Very Low_ values of **I**. **P** ended up between the 
two values for this project.

**Note** Some expirementation has been done with twiddling the values every 2 laps around the track, however this method
is not in production yet.




## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
