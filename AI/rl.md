# Reinforcement Learning

Policy, noted π is defined as `π(S) = A`, Given a state the policy tell us what action to do.

## RL Loop
<img src="RL_process.jpg" width="550">

## State and Observation
- **State** S: is a complete description of the state of the world (there is no hidden information). In a fully observed environment.
- **Observation** O: is a partial description of the state. In a partially observed environment.

## Two methods to find the optimal policy π*:
  - **Policy-based methods** : We Directly learn the policy, by teaching the agent to learn which action to take, given the current state.
      - *deterministic policy*: a policy that output one action given a state
      - *stochastic policy*: that output a probability distribution over actions
  - **Value-based methods** : Indirectly, by training a value function that outputs the value of a state or a state-action pair. Given this value function, our policy will take an action.
      - *state-value function* : we calculate the value of a state S
      - *action-value function* :  we calculate the value of the state-action pair (S,A​) hence the value of taking that action at that state.

## The Bellman Equation

### $V_π(s) = E_π[R_{t+1} + γ * V_π(S_{t+1}) | S_t = s ]$

- $R_t$ : reward a instant t
- $E_{pi}$ : expected return
- $S_t$ : state as instant t

## Monte Carlo vs Temporal Difference Learning
- **Monte Carlo**: learning at the end of the episode
    - we update the value function from a complete episode, and so we use the actual accurate discounted return of this episode.
    - $V(S_t) \gets V(S_t) + \alpha * [G_t - V(S_t)]$
        - $G_t$ : The summed rewards
- **Temporal Difference Learning**: learning at each step
    - we update the value function from a step, and we replace $G_t$​, which we don’t know, with an estimated return called the TD target.
    - $V(S_t) \gets V(S_t) + \alpha * [R_{t+1} + \gamma V(S_{t+1}) - V(S_t)]$

## Q-learning
The Q comes from “the Quality” of that action at that state.

Q-Learning is an **off-policy** **value-based** method that uses a **TD approach** to train its **action-value function**<br>
The Q comes from “the Quality” (the value) of that action at that state.<br>
Q-Learning is the algorithm we use to train our Q-function<br>
Internally, our Q-function is encoded by a Q-table, a table where each cell corresponds to a state-action pair value. Think of this Q-table as the memory or cheat sheet of our Q-function.<br>

### The policy
### $\pi^*(s) = argmax_a(Q^*(s,a))$

#### Off-policy vs On-policy
Off-policy: using a different policy for acting (inference) and updating (training).<br>
On-policy: using the same policy for acting and updating.

## Deep Q-learning

Deep Q-Learning: Method that trains a neural network to approximate, given a state, the different Q-values for each possible action at that state. It is used to solve problems when observational space is too big to apply a tabular Q-Learning approach.

### Temporal Limitation 
Temporal Limitation is a difficulty presented when the environment state is represented by frames. A frame by itself does not provide temporal information. In order to obtain temporal information, we need to stack a number of frames together.

### Phases of Deep Q-Learning:

- Sampling: Actions are performed, and observed experience tuples are stored in a replay memory.
- Training: Batches of tuples are selected randomly and the neural network updates its weights using gradient descent.

### Solutions to stabilize Deep Q-Learning:

- **Experience Replay**: A replay memory is created to save experiences samples that can be reused during training. This allows the agent to learn from the same experiences multiple times. Also, it helps the agent avoid forgetting previous experiences as it gets new ones.

- **Random sampling** from replay buffer allows to remove correlation in the observation sequences and prevents action values from oscillating or diverging catastrophically.

- **Fixed Q-Target**: In order to calculate the Q-Target we need to estimate the discounted optimal Q-value of the next state by using Bellman equation. The problem is that the same network weights are used to calculate the Q-Target and the Q-value. This means that everytime we are modifying the Q-value, the Q-Target also moves with it. To avoid this issue, a separate network with fixed parameters is used for estimating the Temporal Difference Target. The target network is updated by copying parameters from our Deep Q-Network after certain C steps.

- **Double DQN**: Method to handle overestimation of Q-Values. This solution uses two networks to decouple the action selection from the target Value generation:
    - DQN Network to select the best action to take for the next state (the action with the highest Q-Value)
    - Target Network to calculate the target Q-Value of taking that action at the next state. This approach reduces the Q-Values overestimation, it helps to train faster and have more stable learning.

# Sources
- https://huggingface.co/learn/deep-rl-course