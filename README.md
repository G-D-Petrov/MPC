# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

## Intro

This repository contains my implementation of a Model Predictive Controler for the Udacity  MPC Project. The goal of this project is to navigate a track in a Udacity-provided simulator, which communicates telemetry and track waypoint data via websocket, by sending steering and acceleration commands back to the simulator. The solution must be robust to 100ms latency, as one may encounter in real-world application.

## Implementation Details

### Model

The project implement a kinematic model. The state is defined by the vehicle's x and y coordinates, its velocity, the cross-track error and psi error. The actuator outputs are the acceleration and the steering angle. The model uses the previous state and actuations to calculate the current timestep. This is achieved with the following calculations:

![MPC equations](equations.png)
this picture was taken from [this github repo](https://github.com/jeremy-shannon/CarND-MPC-Project/)

### Timestep Length and Elapsed Duration (N & dt)

The final values that I chose for N and dt are 10 and 0.05. With these values I got the car to drive the fastest while still driving safely. When using an inappropriate values for N, the car was either driving too inconsistently or too dangerously. When changing the dt, the car either drove too slowly or drove off the track.

Other values that I tested for N/dt were 10/0.01, 15/0.1, 5/0.05.

### Polynomial Fitting and MPC Preprocessing

Before fitting the polynomial, the waypoints are transformed to the vehicle's perpective. This way the origin of the curve is defined the the vehicle's x and y coordinates and the orientation angle is zero.

### Model Predictive Control with Latency

As discussed in the lectures, the latency is handled by predicting a future timestep, as well as punishing CTE, epsi, difference between velocity and a reference velocity, delta, change in delta, acceleration and change in acceleration.
Another cost based on hte combination of velocity and delta was also added. I got this idea from [Jeremy Shannon's work](https://github.com/jeremy-shannon/CarND-MPC-Project).
