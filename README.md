# Network_Optimization
Optimizing the location of pressure sensors in a water distribution network to maximize pipe burst detection.

## Introduction and Background
Ensuring the security of critical infrastructures such as water distribution networks is crucial for
the welfare and prosperity of our society. Such infrastructure networks may span huge geographical
areas, and are inherently vulnerable to disruptions. In recent years, numerous incidents have been
reported that highlight the inherent vulnerability of infrastructure networks (website). Operations
Research can provide new solutions to help the water utilities to proactively recognize the security
risks to their infrastructure, and develop specific capabilities to reduce them.

In this project, a water utility company, and more specifically a network operator, is interested
in allocating pressure sensors on nodes in order to detect pipe bursts in its water distribution
network. The network is located in Kentucky, and is composed of 1123 pipes and 811 nodes. It
provides 2.09 million gallons of water per day to its 5,010 customers. The layout of the water
distribution network is given in Figure 1.

![image](https://github.com/vaani-r/Network_Optimization/assets/76833593/76ac1a50-0dcc-4e95-8bb2-9e3f79a2d3cf)

We denote V the set of 811 nodes/sensor locations, and E the set of 1123 network pipes.
Each node can receive at most one sensor. You will be given a detection matrix, denoted F ∈
{0, 1}|E|×|V|, that represents the sensing capabilities of the pressure sensors. F is a binary matrix
of size |E| × |V| = 1123 × 811, and is such that fe,v = 1 if a sensor placed at location v ∈ V can
detect a burst of pipe e ∈ E.

## Problem 1
Problem Statement: Identify the minimum number of sensors and their locations to ensure that if a pipe bursts, at least one sensor will detect it.

Decision Variable:
For this part we will define the following binary variable

$$
x_v = \begin{cases} 
1 & \text{if a sensor is placed on node position $v$} \\
0 & \text{otherwise}
\end{cases}
$$

Objective Function:
We are minimizing the number of sensors to use which will ensure that a pipe burst in any of the pipes will not go undetected.
$$\min \sum_{v=1}^{811}  x_v $$

Constraints:
We have two sets of constraints. Firstly, we need to have atleast one sensor in each pipe that is placed at a node capable of detecting a burst (i.e) $F_{ev} = 1$. Secondly, each node can have either $1$ or $0$ sensor.

$$\begin{flalign*}
&& \text{s.t. } \sum_{v=1}^{811} F_{ev}x_v && \forall e \in E &
\end{flalign*}$$

$$\begin{flalign*}
&& \text{s.t. } \ x_v \in \{ 0,1 \} && \forall v \in V &
\end{flalign*}$$

Answer: 
By solving the problem computationally, we see that the minimum number of sensors would be 19. 

## Problem 2
Problem Statement: Identify the location of $b$ sensors that maximizes the expected number of pipe bursts that are detected

Decision Variable:

$$
x_v = \begin{cases} 
1 & \text{if a sensor is placed on node position $v$} \\
0 & \text{otherwise}
\end{cases}
$$

$$
z_e = \begin{cases} 
1 & \text{if pipe burst is detected in pipe $e$} \\
0 & \text{otherwise}
\end{cases}
$$

$z_e$ is a binary variable that states if pipe e's burst will be detected.

Objective Function:
$$max \sum_{e=1}^{1123} 0.1z_e$$ 

Constraints:
There will be fours constraints for this problem. The first constraint ensures that $z_e$ is $0$ if a pipe burst in pipe $e$ is not detected by any of the $b$ sensors. The constraint forces $z_e$ to be $0$ if the sum of all the products of $F_{ev}$'s and respective $x_v$'s is $0$ for each pipe $e$. If this sum is not $0$, it means that a burst in pipe $e$ can be detected in which case $z_e$ is unrestricted. The second constraint makes sure that we do not use more than $b$ sensors. The third and fourth constraints are binary constraints for the variables $z_e$ and $x_v$

$$
\begin{flalign*}
&& \text{s.t. } z_e \leq \sum_{v=1}^{811} F_{ev} x_v && \forall e \in E &
\end{flalign*}
$$

$$
\begin{flalign*}
&& \text{s.t. } \sum_{v=1}^{811} x_v \leq b && &
\end{flalign*}
$$

$$
\begin{flalign*}
&& \text{s.t. } \ x_v \in \{0,1\} && \forall v \in V &
\end{flalign*}
$$

$$
\begin{flalign*}
&& \text{s.t. } \ z_e \in \{0,1\} && \forall e \in E &
\end{flalign*}
$$

Answer: We need atleast 19 sensors to detect all 1123 pipes in case of a burst. 
![image](https://github.com/vaani-r/Network_Optimization/assets/76833593/03fa03d8-882f-43e8-b168-269ab8fffd9e)


## Problem 3
Problem Statement: For a faster solution, sequentially and myopically select sensor locations. Code this algorithm and compare solutions.

Algorithm:
Step 1 – Select node which detects the maximum expected number of pipe bursts. Place sensor at the selected node.
Step 2 – Remove all pipes that have been detected through the above sensor placement
Step 3 – If all pipes are detected or the number of sensors have run out, stop. Otherwise, repeat Step 1 and Step 2

Solution: 
When plotting the optimal value against the number of sensors, we see that at 20 sensors, the expected number of pipe bursts that are detected is lesser than the total number of pipes - 1123. 

![image](https://github.com/vaani-r/Network_Optimization/assets/76833593/6420462e-8000-4fac-9dc9-fcc466edc03d)

The time taken to run the algorithm is less than 1 second. 

Comparison of Solutions:
- Time Taken: IP takes ~5 minutes while the Sequential solution takes less than a second
- Optimal Solution: IP states that atleast 19 sensors are required to detect all pipe bursts. The Sequential solution states that more than 20 sensors are required to detect all pipe bursts.

![image](https://github.com/vaani-r/Network_Optimization/assets/76833593/429c80c0-64b1-459d-a4ea-694088c90f31)

## Problem 4
Problem Statement: Identify the position of b sensors to minimize the highest criticality of a pipe that is not detected by any sensor.

Decision Variable:

$$
x_v = \begin{cases} 
1 & \text{if a sensor is placed on node position $v$} \\
0 & \text{otherwise}
\end{cases}
$$

$$
z_e = \begin{cases} 
1 & \text{if pipe burst is detected in pipe $e$} \\
0 & \text{otherwise}
\end{cases}
$$

$w_e$ is a continuous variable that ranges between [0, 1]. The higher $w_e$ is, the more critical pipe $e$ is.

Objective Function:

$$
\begin{flalign*}
&& \ min max w_e(1- z_e) && \forall e \in E &
\end{flalign*}
$$

Let's represent $max$ $w_e(1- z_e)$ as $k$. The linearized objective function would be:
<p align="center">
$min$ $k$
</p>

Constraints:

$$
\begin{flalign*}
&& \ k \geq w_e(1-z_e) && \forall e \in E &
\end{flalign*}
$$

$$
\begin{flalign*}
&& \ z_e \leq \sum_{v=1}^{811} F_{e,v} z_e && \forall e \in E &
\end{flalign*}
$$

$$
\begin{flalign*}
&& \ z_e \leq \sum_{v=1}^{811} F_{e,v} z_e && \forall e \in E &
\end{flalign*}
$$

$$\sum_{v=1}^{v=811} x_v \leq b$$

$$
\begin{flalign*}
&& \text{s.t. } \ x_v \in \{0,1\} && \forall v \in V &
\end{flalign*}
$$

$$
\begin{flalign*}
&& \text{s.t. } \ z_e \in \{0,1\} && \forall e \in E &
\end{flalign*}\\
$$

