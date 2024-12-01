---
Name: Kyle Petitt
Topic: 33
Title: Accuracy and Stability of Different Types of Time-Stepping Methods
----

# Accuracy and Stability of Different Types of Time-Stepping Methods
In numerical analysis, time-stepping methods are one of the most fundamental forms of numerical integration. These methods are commonly used to integrate time-dependent ordinary differential equations (ODEs) and partial differential equations (PDEs) [5]. They are approximate methods that are dependent on user parameters and model properties, but can be used to approximate the behavior of a variety of systems with high fidelity if applied correctly. They are especially useful for integrating systems with no analytical solution, and are often used in industry and research. As such, the accuracy and stability of these methods are paramount. This wiki will serve to provide an introduction to the accuracy and stability of such methods, but a more in-depth study is left to the reader. Only first-order, ordinary differential equation with given initial conditions, known as initial-value problems (IVPs), will be discussed directly in detail, although these methods can be applied to higher order systems and a range of conditions.


## Background
The Existence an Uniqueness Theorem states that the solution to an IVP, $f$, exists and is unique over a specified internal if $f$ is continuous and differentiable over that interval [10]. This is key for numerical methods because it guarantees an exact solution under these conditions. This also allows the error of an approximation to be computable and allows for the determination of stability [6].

Numerical methods for IVPs are seperated into two primary categories, explicit and implicit [7]. Explicit methods approximate the state of the system at a later time based off of the state of the system at the current time. Given the differential equation and an initial condition, the algorithm can compute the next system-state over and over one step at a time. This is also commonly referred to as "time-marching." The algorithm is commonly written as 

$$x(t+\Delta t) = F(x(t))$$

where the function $F$ can be calculated explicitly ([example shown later](#derivation-of-basic-explicit-and-implicit-methods)) and $x_i$ is known from the given initial condition or the previous time step computation. Since all of the terms on the right side of the equation are known, the next system-state can be directly computed. The most simple simple form of explicit methods is Forward Euler Integration, but there are many other techniques such as explicit Runge-Kutta and the Adams-Bashford Algorithm [8].

Implicit methods, on the other hand, look to find the next system-state by solving an equation using the current state and a future, unknown state(s). In most cases, iterative, numerical methods are required to solve for the unknown state as these functions are usually nonlinear in nature and cannot be solved explicitly. This algorithm is typically written as 

$$G(x(t),x(t+\Delta t)) = 0$$

The numerical methods commonly neeeded to solve for $x(t+\Delta t)$ add an additional and often expensive step to the process. This may seem like a roundabout way to find the next system state when compared to explicit methods, but the implicit method has its merits which will be discussed in subsequent sections. Other examples of implicit methods include implicit Runge-Kutta and the trapezoidal method.


## Derivation of Basic Explicit and Implicit Methods
To understand these numerical methods' strengths and differences a bit better, the derivations are key. First, the simplest form of the explicit method using a first-order, ordinary differential equation of form

$$\frac{dx}{dt} = f(x(t),t)$$

over the interval 

$$t \in [a,b]$$

where the step size $h$, this can be defined as

$$h = \frac{b-a}{n}$$

and $n$ is the integer number of subintervals in the interval $[a,b]$. The given ODE can then be approximated as

$$\frac{dx_i}{dt_i} \approx \frac{x_{i+1}-x_i}{t_{i+1}-t_i} = \frac{x_{i+1}-x_i}{h}$$

for a particular time $t_i$ using a finite difference approximation, and rewritten as

$$\frac{x_{i+1}-x_i}{h} \approx f(x_i,t_i)$$

This can then be simplified to its final, explicit form

$$x_{i+1} \approx hf(x_i,t_i)+x_i$$

which is known as Forward Euler's integration. Note that this method depends heavily on the size of $h$, which is chosen by the user. Intuitively, as the size of $h$ is decreased to 0, representing infinite $n$ intervals, the accuracy of the approximation approaches the exact solution. Unfortunately this increase in accuracy, has a side-effect, a direct increase in run time, which will be discussed [later](practical-use).

The implicit method can be derived similarly given the same function. The first few steps are exactly the same with the exception that the slope, $f$, is a function of $f(x_{i+1},t_{i+1})$ instead of $f(x_{i},t_{i})$ [9].

$$\frac{x_{i+1}-x_i}{h} \approx f(x_{i+1},t_{i+1})$$

Following similar steps, the equation can be simplified to 

$$f(x_{i+1},t_{i+1})h + x_i - x_{i+1} = 0$$

where $t_{i+1}$, $x_i$, and $h$ are all known, and $x_{i+1}$ is still unknown. This is often called Backward Euler Method. Depending on $f$, this can be a really ugly, nonlinear equation and may require numerical methods to find the zero that yields the desired $x_{i+1}$. Note that it would take infinite iterations to find the exact zero, so the user is responsible for setting some acceptable tolerance value close to zero to approximate $x_{i+1}$. Smaller tolerances yield more accurate results but require a longer computational run time. Forward and Backward Euler are both considered one-step methods because they only rely on information from the previous step to calculate the next.

Having methods for solving otherwise unsolveable problems is a powerful tool, but the question remains: how accurate are these approximations, and how stable?


## Stability
The concept of stability refers to the sensitivity of the solution of a given problem to small perturbations in inputs such as data or parameters [1]. The problem can be sensitive for two reasons, because the system itself is fundamentally sensitive to changes in input, i.e. the problem is ill-conditioned, or the applied numerical method can be ill-conditioned. Certain methods are more stable than others, and ideally, a numerical method shouldn't introduce additional sensitivity to a problem.

In using single-step numerical methods to solve an IVP, the next step at $t_{i+1}$ is approximated based on information from the previous step $t_i$. This goes for both explicit and implicit methods. Some error is typically introduced during this approximation. Worse, this error is then incorporated into the computation of the next step leading to errors being propagated through during the solution process. This has massive implications on explicit methods especially as we will see in the following discussion. 

Given an IVP

$$\dot{u} = f(t,u), \space \space u(t_i) = y_i$$

where $u$ is the solution and $y$ is the approximation. At each step, the local error can be calculated as

$$err_l = u(t_i) - y_{i}$$

and the global error can be calculated as 

$$err_g = y(t_{i})-y_{i}$$

Note that in the implicit method, the maximum local error is controlled by some tolerance value chosen by the user, limiting the global error indirectly [11]. The propagation of error can then be analyzed using  

$$y(t_{i})-y{i} = [u(t_{i})-y_{i}] + [y(t_i)-u(t_i)]$$

where the first bracketed term is the local error, controlled by the user [11], and the second bracketed term is the difference in the two solutions of the ODE at $t_i$. Control of local error is an extremely important part of selecting a numerical method. Without a reasonable local error, the results are essentially meaningless. How to properly manage local error will be further discussed in the [practical use](#practical-use) section. For explicit and implicit methods, local error can be controlled by manipulating the step size $h$. Recall that this has direct implications on computational run time. Implicit methods do have the additional benefitof partiall having their local error controlled by the tolerance value used to root find $y_{i+1}$, and as such they are unconditionally stable [2]. This means regardless of the step size, implicit methods are stable. This becomes very important when dealing with [stiff systems](#stiff-systems), which will be discussed later. The second bracketed term above is a property of the ODE and is often referred to as the true error [11]. If the IVP is stable, the true error will be comparable in magnitude to the local error for all time. If the IVP is unstable, the true error will grow regardless of the size of the local errors. Note this happens regardless of the numerical method, but using methods with lower local error can delay the rate of divergence from the true solution.


## Accuracy and Error Estimation
Error estimation is key to understanding how well the approximation will match system behaviour. To estimate error for one-step methods, the approximation is simply compared to the definite integral over the interval. Typically, the definite integral cannot be calculated explicitly as $f$ is unknown, making it impossible to calculate the exact error of a numerical method, however, the error of individual methods can be estimated based off of known parameters. The numerical methods discussed thus far have form

$$u(t_i+h) = y_i + \int_{t_i}^{t_i+h}f(x)dx$$

where $u$ is the true value at the next step, $y$ is the approximate value at the previous step, and the integral is the change over that interval [11]. Since $f$ is typically unknown for our use case, it is often approximated using an interpolating polynomial, $P$, over the interval. As [we have discussed](derivation-of-basic-explicit-and-implicit-methods), for Forward Euler

$$\int_{t_i}^{t_i+h}f(x)dx \approx hf(t_i) = P(x)$$

and for backwards Euler

$$\int_{t_i}^{t_i+h}f(x)dx \approx hf(t_{i+1}) = P(x)$$

Using polynimial interpolation theory [11], given that $P$ is the unique polynomial of degree less than $s$ that approximates $f$ with a smooth interpolating function with $s$ distinct nodes, then there exists some value $C$ such that

$$\left|f(x) - p(x)\right| \le Ch^s$$

for all $x\in[t_i,t_i+h]$ yielding 

$$f(x)-P(x) = \mathcal{O}\left(h^s\right)$$

The estimated error of a numerical method can then be computed by taking the integral [11]. 

$$\int_{t_i}^{t_i+h}f(x)dx \approx \int_{t_i}^{t_i+h}P(x)dx + \mathcal{O}\left(h^{s+1}\right)$$

Applying the Forward and Backwards Euler methods to this relationship yields that the magnitude of the local error for these methods is 

$$u(t_i+h) - y_{i+1} = \mathcal{O}\left(h^{2}\right)dx$$

This shows the direct dependence of local error on step size, and that it is quadratic in nature. The error of other methods can be computed similary using the method shown. As one might expect, the simple, one-step Euler methods are less accurate locally than the more complicated, widely used algorithms. By how much will be discussed [later](#practical-use).


## Example and Sample Code
The importance of accuracy and stability is best illustrated with a brief example. Consider the IVP

$$\dot{x} = \tau x, \space \space x(0) = x_0, \space \space \tau = -0.4$$

This IVP can be solved analytically (this exercise is left to the reader) yielding the equation

$$x(t) = x_0e^{-0.4t}$$

where $x_0$ is the given initial condition. Using Forward Euler integration we can approximate the next step explicitly,

$$x_{i+1} \approx hf(x_i,t_i) + x_i = -0.4x_ih+x_i$$

By varying the value of $h$, we can produce this plot showing how the size of $h$ can directly affect the accuracy of the integration.

<p align="center">
  <img src="MATH4640_FinalProject_Figure-TimestepComparison - Explicit Method.jpg" width="600">
</p>

We see that this algorithm is only qualitatively accurate for sufficiently small time steps. As the size of the time step is decreased, the approximation converges to the analytical solution as expected. We also see that using an explicit method can lead to instability, which is shown when too large of a time step is chosen in this example. Selecting the correct time step is paramount for these methods to prevent instability and increase local accuracy.


## Stiff Systems
Stiff systems are diffifult to define [13], but loosely, they converge to a steady state very quickly compared to the timescale of the interval of integration $x\in[a,b]$ [11]. Stiff systems are very well-conditioned [12], but due to the nature of their stiffness, they require arbitrarily small step sizes when using explicit methods. For any reasonable step size, the integration method is ill-conditioned. For this reason implicit methods are typically preferred for stiff systems. Recall that implicit methods are unconditionally stable. Generally, it is much faster to compute one step using an explicit method than an implicit method, but for the same amount of error, the step size is significantly smaller for the explicit method. Thus, it is faster to compute using an implicit method. Explicit methods can be used for stiff problems, but it is impractical to use a constant step size when integrating. 


## Practical Use
Thus far the discussion has focused primarily on the theory of basic numerical methods in their basic form, and how to determine key properties such as stability and accuracy. This section looks to discuss stability and accuracy for practical use including more advanced methods of numerical integration such as Runge-Kutta methods and IMEX, and how to control and limit local error.

IMEX methods are methods that combine implicit and explicit methods, taking advantage of both of their strengths. One such IMEX method is the Crank-Nicolson method which computes the next step by averaging the Forward Euler and Backwards Euler outputs [3]. By combining these methods, it is more accurate than backwards Euler, while maintaining its unconditional stability and being approximately the same computational run time.

Probably the most widely used numerical method are Runge-Kutta methods [4]. They are multi-stage methods that evaluate the ODE function, $f$, multiple times per time step. Where the Euler method only uses the first two terms of the Taylor expansion when approximating $u_{i+1}$, Runge-Kutta methods use the first three terms. This is done by approximating the second derivative as

$$\ddot{u}(x,t) \approx \dot{f}(x,t) = \frac{\partial f}{\partial t} + \frac{\partial f}{\partial u}\frac{du}{dt}$$

These partial derivatives must be approximiated to complete the calculation. There are several ways to do this produce a whole class of methods, i.e. the Runge-Kutta methods. This is done through multiple stages where values of $f$ are calculated using interpolated values of $t$ and weighted using coefficients determined by the method order. The number of interpolated values defines the number of stages and the order of the method. For example, fourth order Runge-Kutta uses

$$k_1 = hf(t_i,u_i)$$
$$k_2 = hf\left(t_i+\frac{h}{2},u_i+\frac{k_1}{2}\right)$$
$$k_3 = hf\left(t_i+\frac{h}{2},u_i+\frac{k_2}{2}\right)$$
$$k_4 = hf(t_i+h,u_i+k_3)$$
$$u_{i+1} = u_i + \frac{(k_1+2k_2+2k_3+k_4)}{6}$$

By incorporating this second order term using several stages, the local error is decreased to $\mathcal{O}(h^{3})dx$ [12]. This alone should be proof that the extra work is worth the benfits. Implicit Runge-Kutta methods can also be implemented similar to how Backwards Euler is modified from Forward Euler.

Numerical methods are a balancing act of accuracy and efficiency. It is important that results be accurate enough to be useful, but if they are too useful, the computational run time may become too expensive to be worthwhile. Implicit methods are more computationally expensive than explicit methods and are often unnecessary for most problems. They guarantee stability, but the resources needed for the root-finding step are so expensive they can't compete with explicit methods. However, [stiff systems](#stiff-systems) are a great counter-example of this. To make explicit methods accurate enough to effectively solve a stiff system, an extremely small time step is needed to maintain stability and accuracy. Implicit methods are then required because of their unconditional stability. They are more computationally expensive per step, but overall they are less computationally expensive to achieve the same accuracy because they don't need such small time steps. Similarly, fourth order Runge-Kutta methods require more steps than Forward Euler, but the gains in accuracy make it worth it. Runge-Kutta can also be applied using a higher order, but increasing the order increases the accuracy less and less as the order increases. Eventually, the computational time required to compute more terms begins to outweigh the gains achieved in accuracy. This sweet spot is typically around fourth order [4]. 

In practice, most softwares have built in solvers. These solvers are optimized to estimate and control error as they solve [12]. It is not expensive to estimate local error, so it is common practice to track local error during the integration process. There are several ways to do this, including doing a finite difference approximation to approximate the derivative of the solution or comparing results between solutions given by different methods [12] to determine error. This last example is commonly implemented with different order Runge-Kutta methods. If the error is greater than some tolerance value when using step size $h_j$, then the algorithm can estimate what the error will be using a new step size $h_{j+1}$ and the solution can be recomputed with the new step size. Similarly, if error is much smaller than some tolerance value, the step size can be increased. Estimating error is useful because then the algorithm can tune the step size throughout the solution process so that the smallest step size isn't used continuously for the entire interval [11]. This is especially useful for stiff problems where an explicit method would typically be very expensive. This practice ensures stability while preventing unnecessarily long computational run time. One such method for computing the step size is [6]

$$h_j \le \sqrt{\frac{2tol}{\left|\ddot{y}_i\right|}}$$

where 

$$\left|\ddot{y_i}\right|\approx\left|\frac{\dot{y_i}-\dot{y_{i-1}}}{t_i-t_{i-1}}\right|$$


## Summary



## References
1. Atkinson, K. (2007). Numerical Analysis. Scholarpedia, 2(8), 3163. https://doi.org/10.4249/scholarpedia.3163 [(link)](http://www.scholarpedia.org/article/Numerical_methods)
2. Belytschko, T. (1984). Explicit, Implicit, and Hybrid Methods. Ntrs.nasa.gov; Northwestern University. https://ntrs.nasa.gov/api/citations/19890015285/downloads/19890015285.pdf [(link)](https://ntrs.nasa.gov/api/citations/19890015285/downloads/19890015285.pdf)
3. Crank–Nicolson method. (2021, January 15). Wikipedia; Wikimedia Foundation. https://en.wikipedia.org/wiki/Crank%E2%80%93Nicolson_method [(link)](https://en.wikipedia.org/wiki/Crank%E2%80%93Nicolson_method)
4. Driscoll, T., & Braun, R. (2020). Initial-value problems for ODEs — Fundamentals of Numerical Computation. Github.io. https://fncbook.github.io/fnc/ivp/overview.html [(link)](https://fncbook.github.io/fnc/ivp/overview.html)
5. Explicit and implicit methods. (2021, January 14). Wikipedia. https://en.wikipedia.org/wiki/Explicit_and_implicit_methods [(link)](https://en.wikipedia.org/wiki/Explicit_and_implicit_methods)
6. Heath, M. T. (n.d.). Scientific computing : an introductory survey. Philadelphia Society For Industrial And Applied Mathematics.
7. Implicit and Explicit Time Integration Time Integration Method -Lesson 2. (n.d.). ANSYS. Retrieved December 1, 2024, from https://innovationspace.ansys.com/courses/wp-content/uploads/sites/5/2021/02/Lesson2_ImplicitAndExplicitTimeIntegration-1.pdf [(link)](https://innovationspace.ansys.com/courses/wp-content/uploads/sites/5/2021/02/Lesson2_ImplicitAndExplicitTimeIntegration-1.pdf)
8. Integration methods. (2024). Sourceforge.net. https://qucs.sourceforge.net/tech/node24.html [(link)](https://qucs.sourceforge.net/tech/node24.html)
9. Kosaraju, S. (2022, October 24). Time integration Methods For Implicit And Explicit FEA - What Are They And How Do They Work? Fidelis FEA. https://www.fidelisfea.com/post/time-integration-methods-for-implicit-and-explicit-fea-what-are-they-and-how-do-they-work [(link)](https://www.fidelisfea.com/post/time-integration-methods-for-implicit-and-explicit-fea-what-are-they-and-how-do-they-work)
10. Picard–Lindelöf theorem. (2021, September 6). Wikipedia; Wikimedia Foundation. https://en.wikipedia.org/wiki/Picard%E2%80%93Lindel%C3%B6f_theorem [(link)](https://en.wikipedia.org/wiki/Picard%E2%80%93Lindel%C3%B6f_theorem)
11. Shampine, L. F., Shampine, L. F., Gladwell, I., & Thompson, S. (2003). Solving ODEs with MATLAB. Cambridge University Press.
12. Shampine, L., & Thompson, S. (2007a). Initial value problems. Scholarpedia, 2(3), 2861. https://doi.org/10.4249/scholarpedia.2861 [(link)](http://www.scholarpedia.org/article/Initial_value_problems)
13. Shampine, L., & Thompson, S. (2007b). Stiff systems. Scholarpedia, 2(3), 2855. https://doi.org/10.4249/scholarpedia.2855 [(link)](http://www.scholarpedia.org/article/Stiff_systems)
