---
title: "Controlling Delayed Systems with Continuous RL"
classes: wide
---

# Master Thesis

I conducted my Master Thesis from KTH under the supervision of Lionel Mathelin, Onofrio Semeraro and [Rémy Hosseinkhan-Boucher](https://rehoss.github.io/). We worked on controlling Delay Dynamical System with Reinforcement Learning in Continuous time. Find the [manuscript here](/assets/paper_reports/master_thesis_joachim_jobard_v2.pdf) and the [presentation slides here](/assets/presentations/presentation_internship_joachim_jobard_v7.pdf). 

## Some details on the topic

Delayed systems are ubiquitous in multiple domains. In this work, we use Continuous Time RL to stabilize different ones. More specifically, we present 2 different algorithms based on K. Doya's work to control Functional and Delayed Differential Equation (DDEs & FDEs) of the form $\dot{x} = f(x_t, u)$ where $x_t : s \mapsto x(t+s)$ for $s \in [-h, 0]$ the history function (state of our system).

The state of the system is here this history function, and to encode it we use the path signature, a powerful tool to encode the geometrical properties of such functions into an infinite tensor. 

The algorithms are then developped to work with this signature formulation and tested.