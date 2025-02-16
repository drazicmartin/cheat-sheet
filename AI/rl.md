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

Q-Learning is an **off-policy** **value-based** method that uses a **TD approach** to train its **action-value function**<br>
The Q comes from “the Quality” (the value) of that action at that state.<br>
Q-Learning is the algorithm we use to train our Q-function<br>
Internally, our Q-function is encoded by a Q-table, a table where each cell corresponds to a state-action pair value. Think of this Q-table as the memory or cheat sheet of our Q-function.<br>

#### Off-policy vs On-policy
Off-policy: using a different policy for acting (inference) and updating (training).<br>
On-policy: using the same policy for acting and updating.

# Sources
- https://huggingface.co/learn/deep-rl-course