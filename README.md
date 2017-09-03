# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

## Reflection

The code in PID.cpp aims to implement a PID controller which tries to minimize the error of the control output to the desired trajectory of the car driving along a track.

PID stands for Proportional Integral Derivate Controller and it is a control loop feedback mechanism used in systems which require continuosly modulated control. It takes a measurement from a sensor and applies correction to a control function to minimize the error difference to a desired setpoint value.

PID controller takes three terms (Proportional gain, Integral gain and Derivate gain) which are used to calculated the weighted sum of the error:
* P computes a proportional value to the Cross Track Error (CTE). It modifies the steer to make the car converge to the desired trajectory. However, P controller alone would result in a highly unstable and oscillating behavior.
* I accumulates the past CTE and integrates them over the time. The integral term seeks to eliminate the error by contributing a control effect due to the historic and present cumulative value of the error. Using this term, biases can be mitigated if for example a zero steering angle doesn't correspond to a straight trajectory.
* D seeks to reduce the effect of the CTE. The proportional derivative term can help to reduce the oscillating behavior of the P proportional term.

The fomula used to update the error:
```
Kp*cte + Ki*sum(cte)+ Kd*(cte - previous_cte)
```
### Hyperparameters selection
The throttle was adjusted to 0.48, so that the car reached a speed of almost 55 mph in the simulator. A that speed, the values which proved to make the car follow the track whithin the road boundaries where:
* P: -0.07
* I: 0.0
* D: -0.8

The values where tuned manually, so that it was easier to check the effects of any change to them on the car trajectory. To ease this process, the values were passed as arguments in the command line. If no values are passed, the code takes the values above as the default.

The speed of the car is decisive when choosing the parameters. Higher speeds require lower values of P, otherwise the car will fall out of the track. For this reason, if different value of throttle is chosen, the values above won't work, and the car will loose the track. An important improvement would be to implement an dynamic parameters selection proportional to the vehicle speed. 

Also, the CPU processing speed to render simulator graphics plays an important role. The same code run in a computer which runs the simulator slower (even when the vehicle output speed is the same) will produce different results for the same parameters. For this reason, an output video is provided, with the controller running with parameters set to the above mentioned values.

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
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

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
