# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

## Note:
This repo has been modified by olala7846@gmail.com, for the original repo, please reference [Udacity's github](https://github.com/udacity/CarND-MPC-Project)


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
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Fortran Compiler
  * Mac: `brew install gcc` (might not be required)
  * Linux: `sudo apt-get install gfortran`. Additionall you have also have to install gcc and g++, `sudo apt-get install gcc g++`. Look in [this Dockerfile](https://github.com/udacity/CarND-MPC-Quizzes/blob/master/Dockerfile) for more info.
* [Ipopt](https://projects.coin-or.org/Ipopt)
  * Mac: `brew install ipopt`
  * Linux
    * You will need a version of Ipopt 3.12.1 or higher. The version available through `apt-get` is 3.11.x. If you can get that version to work great but if not there's a script `install_ipopt.sh` that will install Ipopt. You just need to download the source from the Ipopt [releases page](https://www.coin-or.org/download/source/Ipopt/) or the [Github releases](https://github.com/coin-or/Ipopt/releases) page.
    * Then call `install_ipopt.sh` with the source directory as the first argument, ex: `bash install_ipopt.sh Ipopt-3.12.1`.
  * Windows: TODO. If you can use the Linux subsystem and follow the Linux instructions.
* [CppAD](https://www.coin-or.org/CppAD/)
  * Mac: `brew install cppad`
  * Linux `sudo apt-get install cppad` or equivalent.
  * Windows: TODO. If you can use the Linux subsystem and follow the Linux instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions


1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Reports

### The Model

* The state is modele by a vector of **x, y, psi, v**, the first 2 terms **(x, y)** are the 2D coordinate of car in the coordinate space, and psi is the facing of the car to the angle of the x-axis and v is the absolute speed of car in the modeled state. 
* Errors **CTE(cross track error) and E-Psi(psi error)** are the difference value the the reference trajectory(which is a fitted polynomial of degree 3).
* Actuators **delta(steering angle) and a(throttle)** are modeled as the angular acceleration and acceleration, but notice that the real angular acceleration should be 
` double d_psi = v / Lf * delta * dt ` 

### Time step Length and Elapsed Duration (N & dt)

* **T(The prediction horizon)** equals to **N(steps to predict)** multiplies **dt(time elapse per step)**
* Although theoretically we want T to be as large as possible, but since we only fits the track by 6 points with a degree=3 polynomial and normally the environments change too much during time for our car to predict, it doesn't make sense to have a very large T.
* We normally want `dt` to be as small as possible, but the cumputational time increases as `dt` decreases and `N` increases. So after some trial and error, I ended up picking a `T = 1.5 seconds` where `dt=0.08 (sec)` and `N=15 (steps)`.



