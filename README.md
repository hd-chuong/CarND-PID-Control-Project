# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Reflection

### The effect of Proportional, Integral and Derivative component of the PID Controller

**Proportional**: The P component is responsible for steering the car back to the lane center. For example, the car is steered to the right if the car is on the left of the center lane, and vice versa. The wider distance between the car and the lane center, the harder the car will be steered back.
- If P coeff is too high, the car will overshoot very hard. Meanwhile, if P coeff is too low, the car is very slow at reacting at the deviation.

**Differential**: If the P component is applied alone, the car will be droven back to the center and is very likely to overshoot the center line. If this steering back is hard, then the car will oscillates around the center lane dramatically. The D component is introduced to ensure that the car is moving to the center smoothly. The D component reacts stronger when the change is more dramatic.
- Too high D coeff will lead the car being inflexible and hard to change its state. Too low D coeff will make the car oscillate dramatically as it has little reaction on the P component.

**Integral**: The I component is used to eliminate the systematic bias. Good examples of systematic bias are strong wind, or improper aligned wheels. After enough long time of averaging, the bias will cancell each other in the sum.
- If the I component is applied alone, the car will drive in a circle.

### How the parameter is tuned
Due to time constraints, I decided to manually tune the P, I and D coefficients. 

Below are the iterations of tuning:

- I started with Sebastian's set of coefficients: (P, I, D) = (0.2, 0.004, 3).

- While the initial coefficients could drive the car through one lap, the car was unnecessarily oscillating even when the road ahead was relatively straight. This could be due to the P component too high. I utilized the idea of binary search to seek for acceptable coefficients. As the P component leads to the high oscillation, I reduced P by 2. So the current coefficient was (P, I, D) = (0.1, 0.004, 3).

- Now, it was observed that when the car is difficult to a sharp turn, which could be due to P not large enough, therefore, I took the middle point between 0.1 and 0.2, to be the P coefficient. (P, I, D) = (0.15, 0.004, 3). From then, attempts to tune P did not make a clear difference, so I decided to tune D.

- I observed that D is too high, which meant that the car might be difficult to make a sharp turn (high D highly resists the large change). So D coeff is reduced by its half. (P, I, D) = (0.15, 0.004, 1.5). The car started to oscillate more uncomfortably.

- I again increased D to the mid point of 1.5 and 3. (P, I, D) = (0.15, 0.004, 2.25). The final performance was good enough with little oscillation.

- It was also observed that the I component did not make clear change when it was less than 0.01. I decided to keep it the same.

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

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

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

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

