📘 DAY 46 — Reinforcement Learning Fundamentals: MDPs, Rewards, and Policies

Goal:

Understand the mathematical framework of Reinforcement Learning — Markov Decision Processes (MDPs), reward functions, and policies. Build the foundation for Q-learning, policy gradients, and PPO.

---

# 🎯 Beginner's Big Picture

> ⭐ **Think of RL like learning to play a board game.** You (the **agent** 🤖) sit at a game board (the **environment** 🌍). Each turn, you see the board position (**state**), pick a move (**action**), the dice roll determines what happens next (**transition**), and you earn or lose points (**reward**). Your **strategy guide** for what move to make in every position is your **policy**. The goal? Maximize your total score over the whole game! 🏆

| RL Concept | Board Game Analogy 🎲 |
|---|---|
| Agent | You, the player |
| Environment | The game board + rules |
| State $s$ | Current board position |
| Action $a$ | A legal move you can make |
| Transition $P(s'\|s,a)$ | Dice roll — what happens after your move |
| Reward $R$ | Points scored (or lost) |
| Policy $\pi$ | Your strategy guide / playbook |
| Discount factor $\gamma$ | "A bird in hand is worth two in the bush" 🐦 — prefer rewards sooner |

---

# 1️⃣ Why RL for AI Founders?

| Application | RL Connection |
|------------|---------------|
| RLHF / DPO | Align LLMs to human preferences |
| Game AI | AlphaGo, OpenAI Five |
| Robotics | Continuous control |
| Recommendation systems | Sequential decision making |
| Autonomous agents | Tool use, planning |

---

# 2️⃣ Markov Decision Process (MDP)

⭐ An MDP is the **mathematical blueprint** for any RL problem — like the rulebook of your board game.

An MDP is defined by the tuple $(S, A, P, R, \gamma)$:

| Symbol | Meaning | Analogy 🎲 |
|--------|--------|------------|
| $S$ | State space | All possible board positions |
| $A$ | Action space | All legal moves |
| $P(s'\|s,a)$ | Transition probability | Dice roll — chance of landing on next square |
| $R(s,a,s')$ | Reward function | Points you get for each move |
| $\gamma \in [0,1)$ | Discount factor | How much you value future vs. immediate points |

> 📖 **Research origin**: Richard Bellman (1957) introduced dynamic programming and formalized MDPs as a framework for sequential decision-making under uncertainty. Puterman (1994) wrote the definitive mathematical treatment in *Markov Decision Processes: Discrete Stochastic Dynamic Programming*.

**Markov property**: The future depends only on the current state, not the history.

$$P(s_{t+1} | s_t, a_t) \quad \text{— no dependence on } s_{t-1}, \ldots, s_0$$

> 💡 **Analogy**: In chess, you only need the current board position to decide your next move — you don't need to remember every move that led there. That's the Markov property!

---

# 3️⃣ Implementing a Simple MDP

```python
import numpy as np

class GridWorld:
    """Simple 4x4 grid world MDP."""
    
    def __init__(self, size=4):
        self.size = size
        self.n_states = size * size
        self.n_actions = 4  # up, down, left, right
        self.goal = (size-1, size-1)
        self.state = (0, 0)
    
    def reset(self):
        self.state = (0, 0)
        return self._state_to_idx(self.state)
    
    def step(self, action):
        """Take action, return (next_state, reward, done)."""
        r, c = self.state
        if action == 0: r = max(r-1, 0)           # up
        elif action == 1: r = min(r+1, self.size-1) # down
        elif action == 2: c = max(c-1, 0)           # left
        elif action == 3: c = min(c+1, self.size-1)  # right
        
        self.state = (r, c)
        done = self.state == self.goal
        reward = 1.0 if done else -0.01
        
        return self._state_to_idx(self.state), reward, done
    
    def _state_to_idx(self, state):
        return state[0] * self.size + state[1]
    
    def _idx_to_state(self, idx):
        return (idx // self.size, idx % self.size)

# Test
env = GridWorld(4)
state = env.reset()
for _ in range(10):
    action = np.random.randint(4)
    next_state, reward, done = env.step(action)
    print(f"State {state} → Action {action} → State {next_state}, Reward {reward}")
    state = next_state
    if done:
        break
```

---

# 4️⃣ Policies

⭐ A **policy** $\pi(a|s)$ maps states to actions (or distributions over actions).

> 🎲 **Analogy**: Your policy is your **strategy guide**. A deterministic policy says "in position X, always do move Y." A stochastic policy says "in position X, do move Y 70% of the time and move Z 30% of the time." Better policies → higher scores!

Formally, a policy $\pi$ assigns a probability to each action given a state:

$$\pi(a|s) = P(A_t = a \mid S_t = s)$$

**The objective of RL** is to find the policy $\pi^*$ that maximizes expected return:

$$J(\pi) = \mathbb{E}_{\pi}[G_0] = \mathbb{E}_{\pi}\left[\sum_{k=0}^{\infty} \gamma^k R_{t+k+1}\right]$$

```python
# Deterministic policy
def greedy_policy(Q, state):
    return np.argmax(Q[state])

# Stochastic policy (ε-greedy)
def epsilon_greedy(Q, state, epsilon=0.1):
    if np.random.random() < epsilon:
        return np.random.randint(len(Q[state]))
    return np.argmax(Q[state])

# Softmax policy (for policy gradient methods)
def softmax_policy(preferences, state, temperature=1.0):
    logits = preferences[state] / temperature
    probs = np.exp(logits - logits.max())
    probs /= probs.sum()
    return np.random.choice(len(probs), p=probs)
```

---

# 5️⃣ Returns and Discounting

⭐ The **return** $G_t$ is the discounted sum of future rewards:

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$$

> 🐦 **Analogy — "A bird in hand is worth two in the bush"**: Getting \$100 today is better than \$100 next year. The discount factor $\gamma$ encodes exactly this intuition. With $\gamma = 0.9$, a reward of 1 received 10 steps from now is worth only $0.9^{10} \approx 0.35$ today.

```python
def compute_returns(rewards, gamma=0.99):
    """Compute discounted returns for an episode."""
    returns = []
    G = 0
    for r in reversed(rewards):
        G = r + gamma * G
        returns.insert(0, G)
    return returns

# Example
rewards = [0, 0, 0, 1]  # Reward only at the end
returns = compute_returns(rewards, gamma=0.99)
print(f"Returns: {returns}")
# [0.970, 0.980, 0.990, 1.000]
```

---

# 6️⃣ Why γ < 1?

| γ Value | Behavior |
|---------|----------|
| γ = 0 | Only cares about immediate reward (myopic) |
| γ = 0.9 | Plans ~10 steps ahead (1/(1-γ)) |
| γ = 0.99 | Plans ~100 steps ahead |
| γ = 0.999 | Plans ~1000 steps ahead |
| γ = 1 | Undiscounted (may diverge for infinite episodes) |

Effective horizon $\approx \frac{1}{1 - \gamma}$

---

# 7️⃣ Reward Design

The reward function is the **specification** of what you want the agent to learn.

| Good Reward | Bad Reward |
|------------|-----------|
| Sparse but clear (+1 at goal) | Dense but misaligned |
| Matches true objective | Hackable (reward gaming) |
| Simple to compute | Expensive to evaluate |

**Reward hacking example**: Agent finds a shortcut that maximizes reward but doesn't achieve the intended behavior. This is directly relevant to RLHF!

> ⚠️ **Production example**: In RLHF for LLMs (like ChatGPT), the reward model can be "hacked" — the model learns to produce verbose, confident-sounding answers that score high with the reward model but aren't actually helpful. This is why alignment research (Christiano et al., 2017) is critical.

---

---

# 🏭 Production & Industry Applications

| Domain | How MDPs Are Used | Example |
|---|---|---|
| 🤖 **RLHF for LLMs** | Human preferences as rewards, LLM responses as actions | ChatGPT, Claude alignment (Ouyang et al., 2022) |
| 🎮 **Game AI** | Game states, moves, win/loss rewards | AlphaGo (Silver et al., 2016), OpenAI Five |
| 🦾 **Robotics** | Joint angles as states, torques as actions, task completion as reward | Boston Dynamics, OpenAI hand manipulation |
| 📱 **Recommendation** | User history as state, items as actions, clicks/engagement as reward | YouTube, Netflix, TikTok |
| 🚗 **Autonomous Driving** | Road scene as state, steering/acceleration as actions | Waymo, Tesla |
| 💊 **Healthcare** | Patient vitals as state, treatments as actions, health outcomes as reward | Dynamic treatment regimes |

> 🛠️ **Getting started**: Use [Gymnasium](https://gymnasium.farama.org/) (successor to OpenAI Gym) to experiment with pre-built MDP environments. Try `CartPole-v1` or `FrozenLake-v1` as your first environments!

---

# 📋 Cheat Sheet — MDPs at a Glance

| Formula | Name | What It Means |
|---|---|---|
| $(S, A, P, R, \gamma)$ | MDP Tuple | The 5 ingredients of any RL problem |
| $P(s'\|s,a)$ | Transition Function | Probability of landing in state $s'$ after taking action $a$ in state $s$ |
| $\pi(a\|s)$ | Policy | Probability of choosing action $a$ in state $s$ |
| $G_t = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}$ | Return | Total discounted future reward from time $t$ |
| $J(\pi) = \mathbb{E}_\pi[G_0]$ | Objective | Expected total return — what we maximize |
| $\frac{1}{1-\gamma}$ | Effective Horizon | How far ahead the agent "looks" |

---

# 📝 Summary

| Concept | Definition |
|---------|----------|
| MDP | $(S, A, P, R, \gamma)$ — full RL framework |
| Policy $\pi(a|s)$ | Maps states to actions |
| Return $G_t$ | Discounted cumulative reward |
| Discount $\gamma$ | Tradeoff between immediate and future |
| Markov property | Future independent of past given present |

---

# 📚 Resources

### Books 📖
| Book | Authors | Why Read It |
|---|---|---|
| *Reinforcement Learning: An Introduction* (2nd ed.) | Sutton & Barto (2018) | **The RL bible** — free online at [incompleteideas.net/book](http://incompleteideas.net/book/the-book-2nd.html). Start here! |
| *Markov Decision Processes* | Puterman (1994) | Rigorous mathematical treatment of MDPs |
| *Algorithms for Reinforcement Learning* | Szepesvári (2010) | Concise monograph on RL algorithms |

### Lectures & Courses 🎓
- ⭐ **David Silver's RL Course** (UCL/DeepMind) — YouTube, 10 lectures, gold standard intro
- **OpenAI Spinning Up** — [spinningup.openai.com](https://spinningup.openai.com/) — practical RL from scratch
- **Stanford CS234** — Reinforcement Learning (Emma Brunskill)

### Key Papers 📄
| Paper | Contribution |
|---|---|
| Bellman (1957) *Dynamic Programming* | Founded the MDP framework and Bellman equations |
| Sutton & Barto (2018) *RL: An Introduction* | Definitive modern RL textbook |
| Mnih et al. (2015) *Human-level control through deep RL* | Deep Q-Networks (DQN) — MDPs + deep learning |
| Silver et al. (2016) *Mastering Go with DNNs and tree search* | AlphaGo — MDP thinking at superhuman level |
| Ouyang et al. (2022) *Training LMs to follow instructions with human feedback* | RLHF — MDPs applied to LLM alignment |

**Tomorrow**: Value functions — V(s), Q(s,a), and the Bellman equations.
