---
Name: Kyle Petitt
Topic: 33
Title: Accuracy and Stability of Different Types of Time-Stepping Methods
----

# Accuracy and Stability of Different Types of Time-Stepping Methods
In numerical analysis, time-stepping methods are one of the most fundamental forms of numerical integration. These methods are commonly used to integrate time-dependent ordinary differential equations (ODEs) and partial differential equations (PDEs) \cite{xxxx}. They are approximate methods that are dependent on user parameters and model properties, but can be used to approximate the behavior of a variety of systems with high fidelity if applied correectly. They are especially useful for integrating systems with no analytical solution, and are often used in industry and research. This wiki will serve to provide an introduction to the accuracy and stability of such methods, but a more in-depth study is left to the reader.

## Background
These methods are seperated into two primary categories, explicit and implicit. Explicit methods approximate the state of the system at a later time based off of the state of the system at the current time. Given the differential equation and an initial condition, the algorithm can compute the next system-state over and over one step at a time. This is also commonly referred to as "time-marching." The algorithm is commonly written as 
$$x(t+\Delta) = \frac{dx}{dt}\Delta t + x(t)$$
or 
$$x_{i+1} = \dot{x}_i\Delta t + x_i$$

Implicit methods, on the other hand, look to find the next system-state by solving an equation


## Types of time-stepping integration

## Derivation


## Accuracy and Stabillity
Implicit is unconditionally stable

## "Why they are what they are"

## Example and Sample Code

## Applications


## Summary

## References

1. https://en.wikipedia.org/wiki/Explicit_and_implicit_methods
2. https://www.fidelisfea.com/post/time-integration-methods-for-implicit-and-explicit-fea-what-are-they-and-how-do-they-work
3. https://fncbook.github.io/fnc/ivp/implicit.html
4. Here
5. Like
6. This
7. Bryngelson, S. H., & Freund, J. B. (2018). Global stability of flowing red blood cell trains. Physical Review Fluids, 3(7). https://doi.org/10.1103/physrevfluids.3.073101 
