# Model-Predictive-Control
Plant.m

The Plant.m function takes the time step at the current stage, the measurement prediction and the input from the MPC module as inputs. The non-linear model of the system is solved for using ode23s function to solve the differential equations numerically and the new state values are obtained. An array of state values are calculated in the given time span and the last step value of the array is returned to the main script.

Objectivemin.m

The objectivemin.m function takes A, B, C matrices (linearized model matrices), and control horizon, prediction horizon, and value of the state variables and the set point values and the weights of the objective function phi matrix as input. The sizes of the input matrices are evaluated and correspondingly the symbolic variable for input is initialised and the dimensions of the state vector and measurement vector are calculated and initialised accordingly. The given state space model is used to find out the predicted measurement value in terms of the initial state prediction and the control inputs in the control horizon and is used to calculate the objective function value at every step and is summed over. Until the control horizon the input variables are generated and beyond the control horizon until the prediction horizons end the input value is the same as the input value at the end of the control horizon. The Constraint.m function is called to evaluate the constraint matrices and those are passed on to fmincon which is constrained non-linear multivariable optimizer. The arguments to fmincon are the objective function, start values, constraint on the measurements, constraint on input values and terminal constraint. The constraint on measurements is that the measurement predictions are greater than zero. The constraint on the inputs is that the lower bound is zero and the upper bound is thousand. The terminal constraint is such that, at the end of the prediction horizon, the predicted measurement is equal to the reference trajectory value. The first input step is then returned to the main script.

Constraint.m

This function is used to determine the matrices A,B to represent it in the form of AX ≤ B where X is the symbolic input variables and A, B are calculated according to the prediction model. The function is also used to find the matrix C such that AX = C, which is the terminal constraint.

fcc_fn_to_solve_model.m

This function is the non-linear model of the given system. It is used in the Plant.m function to solve the differential equation and get the new state values.

Main_Script.m

The main script has the initialisation for the control horizon, prediction horizon, weights for phi matrix, process covariance, measurement covariance, initial estimated state values and the estimation covariance. The user can set the values for these as required. The MPC implementation is looped for around 100 times where the functions mentioned above are called in succession and each of the returned value is passed on to the subsequent function to compute the required values. Inside the loop, Kalman filter is implemented by calculating the propagation step values, Kalman gain and the corrected state values in succession. At each time step, the values of the state and the given inputs are stored in an array and at the end of the loop, they are plotted.
