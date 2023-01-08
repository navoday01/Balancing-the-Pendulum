# Balancing the Pendulum
Implemented Q-learning with a Q-table to achieve desired position for simple pendulum

<p align = 'center'><img src ='assets/pendulum.gif'></p>   
<p align = 'center'><em>Simulation of Simple Pendulum</em></p> 

## Problem Formulation

The inverted pendulum has a limit on the maximum torque it can apply, therefore it is necessary for the pendulum to do a few "back and forth" motions to be able to reach the inverted position ( $\theta=\pi$ ) from the standing still non-inverted position ( $\theta=0$ ).

<p align = 'center'><img src ='pendulum.png' width="150" height="300" ></p> 
<p align = 'center'><em>Pendulum Model</em></p> 

In the following,the vector of states of the system is given by:

$$x = \begin{pmatrix} 
\theta \\ 
\omega 
\end{pmatrix}$$

We will also work with time-discretized dynamics, and refer to $x_n$ as the state at time $t = n \Delta t$ (assuming discretization time $\Delta t$)

We want to minimize the following discounted cost function

$$\sum_{i=0}^{\infty} \alpha^i g(x_i, u_i)$$ where 

$$g(x_i, u_i) = (\theta-\pi)^2 + 0.01 \cdot \dot{\theta}_i^2 + 0.0001 \cdot u_i^2 $$ and $$\alpha=0.99$$.

This cost mostly penalizes deviations from the inverted position but also encourages small velocities and control.

## Q-table

The Q-learning algorithm is implemented with a table. For the action value function given in equation~\ref{eq:action_value} the assumptions are made such that $u$ can take only {\bf three} possible values. For states $\theta$ can take any value in the range of $[0,2\pi)$ and that $\omega$ can take any value between $[-6,6]$.

In order to build the table, we will need to discretize the states. So for the learning algorithm, we will use {\bf 50} discretized states for $\theta$ and {\bf 50} for $\omega$. Hence the dimension of the Q-table will be of dimension $\mathbf{50x50x3}$.
\begin{equation} Q(x_t, u_t) \label{eq:action_value} \end{equation}

Q-table gives the quality of the state and action pair, where value of Q is given by 
\begin{equation} Q(x_t,u_t)=g(x_t, u_t)+ \alpha \min_{u}Q(x_{t+1}, u) \label{eq:av-break} \end{equation}

To obtain next state in equation~\ref{eq:av-break}, $x_{n+1}$ given $(x_n, u_n)$ a function is defined that integrates the pendulum for one step of 0.1 seconds and returns the next state of the pendulum as a 2D numpy array at the end of the integration.
