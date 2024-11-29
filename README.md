---
Name: Kyle Petitt
Topic: 33
Title: Accuracy and Stability of Different Types of Time-Stepping Methods
----

# Accuracy and Stability of Different Types of Time-Stepping Methods
In numerical analysis, time-stepping methods are one of the most fundamental forms of numerical integration. These methods are commonly used to integrate time-dependent ordinary differential equations (ODEs) and partial differential equations (PDEs) [CITATION]. They are approximate methods that are dependent on user parameters and model properties, but can be used to approximate the behavior of a variety of systems with high fidelity if applied correctly. They are especially useful for integrating systems with no analytical solution, and are often used in industry and research. As such, the accuracy and stability of these methods are paramount. This wiki will serve to provide an introduction to the accuracy and stability of such methods, but a more in-depth study is left to the reader. Only first-order, ordinary differential equation with given initial conditions, known as initial-value problems (IVPs), will be discussed directly, although these methods can be applied to higher order systems and a range of conditions.

## Background
The Existence an Uniqueness Theorem states that the solution to an IVP, $f$, exists and is unique over a specified internal if $f$ is continuous and differentiable over that interval [9]. This is key for numerical methods because it guarantees an exact solution under these conditions. This also allows the error of an approximation to be computable and allows for the determination of stability [10].

Numerical methods for IVPs are seperated into two primary categories, explicit and implicit. Explicit methods approximate the state of the system at a later time based off of the state of the system at the current time. Given the differential equation and an initial condition, the algorithm can compute the next system-state over and over one step at a time. This is also commonly referred to as "time-marching." The algorithm is commonly written as 

$$x(t+\Delta t) = F(x(t))$$

where the function $F$ can be calculated explicitly (example shown later) and $x_i$ is known from the given initial condition or the previous time step computation. Since all of the terms on the right side of the equation are known, the next system-state can be directly computed. The most simple simple form of explicit methods is Forward Euler Integration, but there are many other techniques such as explicit Runge-Kutta and the Adams-Bashford Algorithm [4].

Implicit methods, on the other hand, look to find the next system-state by solving an equation using the current state and a future, unknown state(s). In most cases, iterative, numerical methods are required to solve for the unknown state as these functions are usually nonlinear in nature and cannot be solved explicitly. This algorithm is typically written as 

$$G(x(t),x(t+\Delta t)) = 0$$

The numerical methods commonly neeeded to solve for $x(t+\Delta t)$ add an additional and often expensive step to the process. This may seem like a roundabout way to find the next system state when compared to explicit methods, but the implicit method has its merits which will be discussed in subsequent sections. Other examples of implicit methods include implicit Runge-Kutta and the trapezoidal method.

## Derivation of Basic Explicit and Implicit Methods
To understand these numerical methods' strengths and differences a bit better, the derivations are key. First, the simplest form of the explicit method using a first-order, ordinary differential equation of form

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

which is known as Forward Euler's integration. Note that this method depends heavily on the size of $h$, which is chosen by the user. Intuitively, as the size of $h$ is decreased to 0, representing infinite $n$ intervals, the accuracy of the approximation approaches the exact solution. Unfortunately this increase in accuracy, has a side-effect, a direct increase in run time, which will be discussed later.

The implicit method can be derived similarly given the same function. The first few steps are exactly with the exception that the slope, $f$, is a function of $f(x_{i+1},t_{i+1})$$ instead of $f(x_{i},t_{i})$

$$\frac{x_{i+1}-x_i}{h} \approx f(x_{i+1},t_{i+1})$$

Following similar steps, the equation can be simplified to 

$$f(x_{i+1},t_{i+1})h + x_i - x_{i+1} = 0$$

where $t_{i+1}$, $x_i$, and $h$ are all known, and $x_{i+1}$ is still unknown. This is often called Backward Euler Method. Depending on $f$, this can be a really ugly, nonlinear equation and may require numerical methods to find the zero to find x_{i+1}. Note that it would take infinite iterations to find the exact zero, so the user is responsible for setting some acceptable tolerance value close to zero to approximate x_{i+1}. Forward and Backward Euler are both considered one-step methods because they only rely on information from the previous step to calculate the next.

Having methods for solving otherwise unsolveable problems is a powerful tool, but the question remains: how accurate are these approximations, and how stable?

## Stability
The concept of stability refers to the sensitivity of the solution of a given problem to small perturbations in inputs such as data or parameters [7]. The problem can be sensitive for two reasons, because the system itself is fundamentally sensitive to changes in input, which is commonly described ill-conditioned, or the problem can be ill-conditioned because of how a  numerical method is applied to it. Certain methods are more stable than others, and ideally, a numerical method shouldn't introduce  additional sensitivity to a problem.

In using numerical methods to solve an IVP, the next step, $t_{i+1}$, is approximated based on information from the previous step, $t_i$. This goes for both the explicit and implicit methods. Some error may be introduced here depending on the problem. Worse, this error is then incorporated into the computation of the next step leading to errors being propagated through during the solution process.

Given an IVP

$$\dot{u} = f(t,u), \space \space u(t_i) = y_i$$

where $u$ is the solution and $y$ is the approximation. At each step, the local error can be calculated as

$$err_l = u(t_i) - y_{i}$$

and the global error can be calculated as 

$$err_g = y(t_{i})-y_{i}$$

Note that in the implicit method, the maximum local error is controlled by some tolerance value chosen by the user, limiting the global error indirectly [8]. The propagation of error can then be analyzed using  

$$y(t_{i})-y{i} = [u(t_{i})-y_{i}] + [y(t_i)-u(t_i)]$$

where the first bracketed term is the local error controlled by the user [8], and the right bracketed term is the difference in the two solutions of the ODE at $t_i$. This second term is a property of the ODE and is often referred to as the true error [8]. If the IVP is stable, the true error will be comparable in magnitude to the local error. If the IVP is unstable, the true error will grow regardless of the size of the local errors. Note this happens regardless of the numerical method, but using methods with lower local effort can delay the rate of divergence from the true solution.

stability of methods
pg17
pg48 firat pRAGRAPH

Implicit is unconditionally stable

## Accuracy
To estimate the error of the approximation, forward error analysis is used to examine the errors that occur in implementing a numerical method [7]. Similarly, backwards error analysis can be used to show that an approximated value is the exact solution of some initial value plus some perturbation.

## Derivation ??

## Example and Sample Code
Consider the IVP

$$\dot{x} = -0.4x, \space \space x(0) = x_0$$

This IVP can be solved analytically (this exercise is left to the reader) yielding the equation

$$x(t) = x_0e^{-0.4t}$$

where $x_0$ is the given initial condition. Using Forward Euler integration we can approximate the next step explicitly,

$$x_{i+1} \approx hf(x_i,t_i) + x_i = -0.4x_ih+x_i$$

By varying the value of $h$, we can produce this plot showing how the size of $h$ can directly affect the accuracy of the integration.

<p align="center">
  <img src="MATH4640_FinalProject_Figure-TimestepComparison.jpg" width="600">
</p>

We see by inspection that this algorithm is only accurate for sufficiently small time steps. As the size of the time step is decreased, the approximation converges to the analytical solution as expected.

## Stiff Systems
Stiff systems are diffifult to define[11], but loosely, they converge to a steady state very quickly compared to the timescale of the interval of integration $x\in[a,b]$ [8]. Stiff systems are very well-conditioned [6], but due to the nature of their stiffness, they require arbitrarily small step sizes when using explicit methods. For any reasonable step size, the integration method is ill-conditioned. For this reason implicit methods are typically preferred for stiff systems. Recall that implicit methods are unconditionally stable. Generally, it is much faster to compute one step using an explicit method than an implicit method, but for the same amount of error, the step size is significantly smaller for the explicit method. Thus, it is faster to compute using an implicit method. Explicit methods can be used for stiff problems, but it is impractical to use a constant step size when integrating. 

textbook pg400, pg453

## Preferred Methods

## Implicit-Explicit Method (IMEX)

## Summary

## References

1. https://en.wikipedia.org/wiki/Explicit_and_implicit_methods
2. https://www.fidelisfea.com/post/time-integration-methods-for-implicit-and-explicit-fea-what-are-they-and-how-do-they-work
3. https://fncbook.github.io/fnc/ivp/overview.html
4. https://innovationspace.ansys.com/courses/wp-content/uploads/sites/5/2021/02/Lesson2_ImplicitAndExplicitTimeIntegration-1.pdf
5. https://qucs.sourceforge.net/tech/node24.html
6. http://www.scholarpedia.org/article/Initial_value_problems
7. http://www.scholarpedia.org/article/Numerical_methods#Numerical_solution_of_differential_and_integral_equations
8. https://web.p.ebscohost.com/ehost/detail/detail?vid=0&sid=89723f7f-8a2b-498a-9314-98035c5a8fab%40redis&bdata=JkF1dGhUeXBlPWlwLHNoaWImc2l0ZT1laG9zdC1saXZlJnNjb3BlPXNpdGU%3d#db=e000xna&AN=125134
9. https://en.wikipedia.org/wiki/Picard%E2%80%93Lindel%C3%B6f_theorem
10. Textbook
11. http://www.scholarpedia.org/article/Stiff_systems
12. Bryngelson, S. H., & Freund, J. B. (2018). Global stability of flowing red blood cell trains. Physical Review Fluids, 3(7). https://doi.org/10.1103/physrevfluids.3.073101 
