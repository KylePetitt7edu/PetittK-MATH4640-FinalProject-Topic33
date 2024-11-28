---
Name: Kyle Petitt
Topic: 33
Title: Accuracy and Stability of Different Types of Time-Stepping Methods
----

# Accuracy and Stability of Different Types of Time-Stepping Methods
In numerical analysis, time-stepping methods are one of the most fundamental forms of numerical integration. These methods are commonly used to integrate time-dependent ordinary differential equations (ODEs) and partial differential equations (PDEs) [CITATION]. They are approximate methods that are dependent on user parameters and model properties, but can be used to approximate the behavior of a variety of systems with high fidelity if applied correctly. They are especially useful for integrating systems with no analytical solution, and are often used in industry and research. This wiki will serve to provide an introduction to the accuracy and stability of such methods, but a more in-depth study is left to the reader. Only first-order, ordinary differential equation, initial-value problems (IVPs) will be discussed directly, although these methods can be applied to higher order systems and a range of conditions.

## Background
These methods are seperated into two primary categories, explicit and implicit. Explicit methods approximate the state of the system at a later time based off of the state of the system at the current time. Given the differential equation and an initial condition, the algorithm can compute the next system-state over and over one step at a time. This is also commonly referred to as "time-marching." The algorithm is commonly written as 

$$x(t+\Delta t) = F(x(t))$$

where the function $F$ can be calculated directly (example shown later) and $x_i$ is known from the given initial condition or previous time step computation. Since all of the terms on the right side of the equation are known, the next system-state can be directly computed. The most simple simple form of explicit methods is Forward Euler Integration, but there are many other techniques such as explicit Runge-Kutta and the Adams-Bashford Algorithm [4].

Implicit methods, on the other hand, look to find the next system-state by solving an equation using the current state and a future, unknown state(s). In most cases, iterative numerical methods are required to solve for the unknown state as they are usually nonlinear in nature and cannot be solved for explicitly. This algorithm is typically written as 

$$G(x(t),x(t+\Delta t)) = 0$$

The numerical methods commonly neeeded to solve for $x(t+\Delta t)$ add an additional and often expensive step to the process. This may seem like a roundabout way to find the next system state when compared to explicit methods, but the implicit method has its merits which will be discussed in subsequent sections. Other examples of implicit methods include implicit Runge-Kutta and the trapezoidal method.

## Derivation
To understand these numerical methods' strengths and differences a bit better, the derivations are key. First we will look at the simplest form of the explicit method using a first-order, ordinary differential equation of form

$$\frac{dx}{dt} = f(x(t),t)$$

over the interval 

$$t \in [a,b]$$

where the interval $h$, this can be defined as

$$h = \frac{b-a}{n}$$

and $n$ is the integer number of subintervals in the interval $[a,b]$. The given ODE can then be approximated as

$$\frac{dx_i}{dt_i} \approx \frac{x_{i+1}-x_i}{t_{i+1}-t_i} = \frac{x_{i+1}-x_i}{h}$$

for a particular time $t_i$, and rewritten as

$$\frac{x_{i+1}-x_i}{h} \approx f(x_i,t_i)$$

This can then be simplified to its final, explicit form

$$x_{i+1} \approx hf(x_i,t_i)+x_i$$

which is known as Forward Euler's integration. Note that this method depends heavily on the size of $h$, which is chosen by the user. Intuitively, as the size of $h$ is decreased to 0, representing infinite $n$ intervals, the accuracy of the approximation approaches the exact solution. Unfortunately this increase in accuracy, has a side-effect, a direct increase in run time, which will be disussed later.

XXX Derivation of implicit method



## Accuracy and Stabillity
The accuracy and stability of these two methods


Implicit is unconditionally stable

## Example and Sample Code
Consider the same first-order, ordinary differential equation as before.

$$\dot{x} = -0.4x$$

given 

$$x(0) = x_0$$

This IVP can be solved analytically (this exercise is left to the reader) yielding the equation

$$x(t) = x_0e^{-0.4t}$$

where $x_0$ is the given initial condition. Using Forward Euler integration,

$$x_{i+1} \approx hf(x_i,t_i) + x_i = -0.4x_ih+x_i$$

the given initial condition, and varying the value of $h$, we can produce this plot showing how the size of $h$ can directly affect the accuracy of the integration.

![](MATH4640_FinalProject_Figure-TimestepComparison.jpg)

We see by inspection that this algorithm is only useful for sufficiently small time steps. As the size of the time step is decreased, the approximation converges to the analytical solution as expected.



## "Why they are what they are"

## Fringe Cases
Stiff systems

## Implicit-Explicit Method (IMEX)

## Applications


## Summary

## References

1. https://en.wikipedia.org/wiki/Explicit_and_implicit_methods
2. https://www.fidelisfea.com/post/time-integration-methods-for-implicit-and-explicit-fea-what-are-they-and-how-do-they-work
3. https://fncbook.github.io/fnc/ivp/overview.html
4. https://innovationspace.ansys.com/courses/wp-content/uploads/sites/5/2021/02/Lesson2_ImplicitAndExplicitTimeIntegration-1.pdf
5. https://qucs.sourceforge.net/tech/node24.html
6. This
7. Bryngelson, S. H., & Freund, J. B. (2018). Global stability of flowing red blood cell trains. Physical Review Fluids, 3(7). https://doi.org/10.1103/physrevfluids.3.073101 
