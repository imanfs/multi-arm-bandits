# multi-arm-bandits

In this repository, different reinforcement learning agents are implemented and their performance is copmared for both stationary and non-stationary environments. 

Since UCB works by prioritising the actions which have been selected less, it encourages exploration, thus is likely to perform well in general.

When $\epsilon$ is set to 0, this is effectively a fully greedy algorithm. While it has lower regret than the random bandit, this algorithm suffers from getting stuck picking a subpotimal action, with the total regret growing linearly as a result. Otherwise however, there is always a chance that the $\epsilon$ greedy bandit will choose a random action "off-policy" in a sense, and this behaviour lowers total regret by encouraging exploration as can be seen for $\epsilon = \{0.1,1/t, 1/\sqrt{t}\}$ .

The REINFORCE algorithm uses sampled rewards directly with gradient ascent to update the policy. Policy gradients are especially useful because they update the policy (our objective) directly, as opposed to other methods that update the value function to implicitly learn the policy. However, because the reward for failures is equal to 0, the gradient ascent term $R_t \nabla log \pi_{\theta}(A_t)$ for those terms goes to 0. In contrast, because the baselined REINFORCE algorithm incorporates an average reward, there is always an update, hence its slightly 

For $\epsilon = 0$, there is no exploration at all, meaning that the agent only ever picks the greedy action which can lead to it getting stuck at a local minimum, hence its relative lowest performance. 

For $\epsilon = 0.1$, there is a constant exploration 10% of the time, which instantly leads to better results (current regret of 0.05 vs 0.1 at the end of the experiment). 

Time-varying $\epsilon$ values perform the best, depending on the run, as the models are more incentivised to explore at the beginning of each experiment. The range of $\epsilon = 1/t \in [0.001,1]$. In contrast, the range of $\epsilon = 1/\sqrt{t} \in [0.0316,1]$, which means that the 1/sqrt(t) algorithm always has a higher exploration probability than 1/t. For example, at $t = 4$, $\epsilon = 1/t = 0.25$ while $\epsilon = 1/\sqrt{t} = 0.5$, hence the overall better performance. 


Because of the non-stationary nature, $\epsilon$ values that are time-dependent no longer perform as well as previously. This is made more evident when comparing with  
UCB would also work well in the general non-stationary context because exploration is not time dependent. 


UCB and time varying $\epsilon$-greedy rank the highest in current regret. Because they explore less at later time steps they get stuck at the poorly performing actions, and furthermore, because these actions still have postitive +1 reward, the algorithms are not incentivised to explore. At 800 timesteps $1/\sqrt{t} = 0.03$ which means that by the time the rewards change, there is only a 3% probability that the bandit will choose the exploratory action. Conversely, the constant $\epsilon = 0.1$ value, because it is always inclined to explore irrespective of time, "bounces back" more quickly so to speak, and after peaking, the regret begins to decrease more steeply than for the time-dependent $\epsilon$. The REINFORCE algorithm performs best as it directly updates the policy in the direction that improves the reward $R_t$ the most. The baseline factor further ensures that the policy moves in the direction of the difference between the new reward values vs the average reward before the change in rewards. 

