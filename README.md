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
