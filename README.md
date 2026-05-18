# Adaptive Traffic Control System Simulation 🚦

An intelligent, state-space traffic light intersection simulator built in Python. This project models vehicle queues using linear algebra and applies an adaptive control algorithm to dynamically prioritize lanes based on real-time traffic volume.

## The Mathematical Model

This simulation models a standard 4-way intersection (North, South, East, West) using a discrete-time state-space equation:

**X(k+1) = X(k) + A(k) - D(k)**

* **X(k)**: State Vector `[N, S, E, W]` representing the current queue length in each lane.
* **A(k)**: Arrival Vector, generated dynamically using a **Poisson Distribution** to simulate realistic, random vehicle arrivals.
* **D(k)**: Discharge Vector, representing the number of vehicles that successfully clear the intersection.

The discharge is calculated using a Green Light Matrix ($G$):
* $D(k) = \min(G \cdot X(k), C_{max})$
* **G**: A diagonal matrix where `1` represents a green light and `0` represents a red light. 
  * *Example:* $G_{NS} = diag(1, 1, 0, 0)$
* **C_max**: The absolute physical capacity vector (maximum cars that can cross per tick).

##  Adaptive Control Algorithm

Unlike traditional fixed-timer traffic lights, this system utilizes an **Adaptive Phase Controller**. 

1. **Queue Tracking**: At each tick, the system evaluates the state vector $X$ using `np.argmax(X)` to identify the lane with the heaviest traffic.
2. **Phase Switching**: The system automatically switches the Green Light Matrix ($G$) to prioritize the longest queue.
3. **Flicker Prevention**: To prevent rapid, inefficient light switching (flickering), the algorithm enforces a `min_green_time` constraint, ensuring a minimum phase duration before a switch is permitted.

##  Prerequisites

To run the simulation, you need Python installed along with the following packages:

```bash
pip install numpy matplotlib ipympl