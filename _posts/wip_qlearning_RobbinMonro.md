# Understanding Q Learning

_This is my first blog note so be indulgent! don't hesitate to communicate me any mistakes or indulgences._

One of the most known Reinforcement Learning algorithm is Q-learning <span style="color:red">**Citation needed**</span>. It was first established in 1992 by Watkins and Dayan and has proven quite efficient for a number of tasks. However, it has several limitations as it can only converge when the state space $\mathcal{S}$ and action space $\mathcal{A}$ are finite <span style="color:red">**testify this and look to papers**</span>. In this blog note, we will then only present tabular Q-learning, and will reserve DQN for a next time. 

Lots has been already said on Q-learning, I will try to tackle it from the begining

## What is Q? Defining our Framework
If you're already familiar with Reinforcement Learning, you've probably already stumbled on the $Q$ function. Before defining what this is, we have to talk a bit about the framework of Reinforcement Learning.

### Markov Decision Process
Reinforcement Learning is all about **Markov Decision Processes** (MDPs), that's a fancy name to simply say that we - what is called the **Agent** - live in a world that is decomposed into **States**. On each state, the agent can take some **Actions** that give him a **Reward**. This is the decision process part, I personally like to think about it with the example of being stuck in a tiled maze (mainly because it was my first RL environment that I studied!). This maze can be represented as tiles that are either walls or corridor. You want to find the escape and you can only move from your tile to adjacent tiles that aren't walls. Here, the states are the tiles, and for each you have a simple set of actions that you can take : left, right, top, bottom, or just staying still, as long as you're not jumping to a wall. This is also a great way to illustrate that the actions that you can take depend on which state you are. Of course, this maze has an escape which gives you 100 points if you reach it (this is the reward part). We could have been more inventive and add more rewards like a reward of -1 for every state. This means that the longer we stay in this maze, the less reward points we will get. If we solve the maze by taking 10 steps, meaning going on 10 tiles, we would then get a reward of 90, but we would only get 85 if we do it in 15 steps etc...

Ok, we've talked about the Decision Process part... But who is Mr. Markov? He actually is the one that will make our life a lot more easier. Let's fall back into our maze, and notice an important property : when I'm on some tile, does it matter the one on which I was before? On a philosophical viewpoint maybe, but right now, we have to take a **decision** and make a move. However, I don't really care about the tile on which I was before, I only care about the one on which I am right now, because it is the one that restrict my movement. If there is a wall on the tile above me, I will not be able to go to this one. The configuration around the previous tile is not relevant at the moment, the only one that matter is the one on which we are now. In more crude words, the future is now, we don't care about what happened before. This is a rather strong property that simplify a lot of computations for what will follow. If you're coming from an analysis background, think of it as being like an ordinary differential equation of the form 
$\frac{dx}{dt} = f(x, t)$. 
The dynamics at time $t$ only depend on the current state $x(t)$, and nothing else.

### Introducing the $V$ function

Let's go back to our goal: a computer doesn't understand the idea of "getting out of a maze", it only understands numbers. This is why we introduced an innocent reward in our previous maze. If the agent is a computer, we can now told him to maximize the end reward, which is something it can understand. The problem can then be reformulated as finding the best **policy** $\pi$ such that for each state $s$, $\pi(s)$ tells us which action $a$ we should take in order to **maximize** the reward at the end of the game.

However we need a way to told the agent if the state (the tile) in which it is, is a great one (close to the escape for example) or a bad one (dead end). This is the goal of the 
$V^\pi$ 
function: giving for each state the gain that we can expect **if we follow the policy $\pi$**. I will voluntarly ommit a lot of details and probability theory behind this, as my goal is not to give a lecture on Bellman equation or Markov Chains. Let's just see the 
$V^\pi$ 
function as some kind of oracle that, given the current state $s$ of the agent, gives us the reward that it can hope for. 

Of course, we don't want to know the $V$ function of any policy $\pi$. It doesn't matter to know how a random policy would behave, or in fact any policy except the one giving us the best results, the one that would escape the maze the quickest, the optimal policy. We call it $\pi*$ and note $V^{\pi^\star}$ as simply $V^* $ for some notation clarity. $V^* (s) $ is then the amount of points we could expect from state $s$ if we follow the optimal policy $\pi*$.


### Introducing the $Q$ Function

The $V^\pi$ function is quite helpful to tell us if we are in a state that is of interest, or from which we can expect a high reward. But how can we know what to do in order to access to this reward? This is when the $Q$ function intervienes. The $Q$ function also depends on the policy we want to follow, which is why we note it as $Q^\pi$. It takes two arguments: the state $s$ in which the agent is and the action $a$ it can take, and gives back **The reward we can expect if in state $s$ we take action $a$ and then follow the policy $\pi$ for the rest of the game**. I want to emphasize the actual power and expressivity this function has. It feels like gaining conciousness at state $s$, looking to the possible actions $a$ and having a way to say: "if I take action $a$ and then just follow my policy, I would gain this much points in the end". 

As you can already imagine, this $Q$ function is at the heart of $Q$ learning.


## Robbins-Monro algorithm
Before presenting the actual algorithm of $Q$-learning, I want to talk about the underlying mechanism of $Q$-learning, The **Robbins-Monro algorithm**. The goal of this algorithm is to the root of a function $h: \mathbb{R}^d \rightarrow \mathbb{R}^d$, with the hypotheses that 
- $h$ is a Lipschitz and continuous function (a continuous function that has a bounded gradient)
- $\dot{x} = h(x)$ has a unique globally stable point at $x^\star$
The algorithm is an interative one, meaning that it outputs a sequence $(x_k)$ such that $\lim_{x\rightarrow \infty} = x^\star$. 

## Learning the Q function
