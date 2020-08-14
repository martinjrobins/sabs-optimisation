---
title: "Nonlinear Optimisation and Root Finding"
teaching: 10
exercises: 0
questions:
- "What is non-linear optimisation and how is it related to root-finding?"
- "What are the challenges in non-linear optimisation"
- "What algorithms exist for solving these problems?"

objectives:
- "Explain the main concepts of non-linear optimisation"
- "Describe the main classes of optimisation algorithms "
katex: true
markup: "mmark"
---

## Mathematical formulation

Optimisation aims to find the minimum (or equivilently the maximum) of some *objective*, 
or *loss* function $f$, given a set of $n$ parameters $\theta$

$$
min_{\theta \in R^n} f(\theta)
$$

We might also have a set of *constraints*, for example a parameter might be required to 
be non-negative (e.g. a concentration or population number). These are often written as 
a set of equality $\mathcal{E}$ and inequality $\mathcal{I}$ constraints

$$
min_{\theta \in \mathcal{R}^n} f(\theta) \text{ subject to } \begin{cases}
c_i(x) = 0, & i \in \mathcal{E} \\
c_i(x) \ge 0, & i \in \mathcal{I} \\
$$

Or these might simply be defined as *bounds* in parameter space that restrict the 
minimisation to a given domain $\Omega \in \mathcal{R}^n$

$$
min_{\theta \in \Omega} f(\theta)
$$


## Useful terms 

*Modelling* is the process of defining the objective function $f$, the parameters of 
interest $\theta$, and the constraints. The algorithms for performing the minimisation 
fall under the field of optimisation. Sub-fields of this are concerned with the 
minimisation of discrete function, often called *integer programming*. It is common to 
see the terms "optimisation" and "programming" used interchangably, as before the advent 
of computer software and programming languages If the function $f$ is linear, then there 
are specific algorithms for this class of problem that fall under the topic of *linear 
programming*, or *linear optimisation*. The more general problem of a non-linear $f$ is 
termed *non-linear programming*, or *non-linear optimisation*. If a set of equality 
and/or inequality constraints are needed then algorithms that deal with these fall under 
the topic of *constrained* optimisation.

An important distinction when looking at optimisation problems is the notion of *global* 
versus *local* optimisation. The latter finds a point in parameter space $\theta_m$ that 
has a function value $f(\theta_m)$ greater than the surrounding points, but might not 
neccessarily by the global minimum. These algorithms are often initialised to a point 
that is near to the minima of interest. The more general problem of global optimisation 
is significantly more difficult as it requires that the optimisation be robust to 
finding and rejecting such local minima. For a function that is *convex*, then local and 
global minimisation are the same, which is very advantagous since local minimisation 
algorithms are often both faster and often have more guarentees of convergence. The 
function $f$ is a convex function if its domain $\Omega$ is a convex set, and for any 
two points $\theta_x$ and $\theta_y$:

$$
f(\alpha \theta_x + (1 - \alpha) \theta_y ) \le \alpha f(\theta_x) + (1 - \alpha) 
f(\theta_y), \text{ for all } \alpha \in [0, 1].
$$

The term *convex programming* is used to describe the case of contrained optimisation 
where $f$ is convex, the equality constraints are linear and the inequality contraints 
are concave.

## Non-linear optimisation and Root-Finding

Non-linear optimisation is closely related to finding the roots, or zeros, of a 
function. This can be seen easily by considering the fact that at each local minima or 
maxima of the function the value of the gradient of $f$ is zero, i.e. $\grad f = 0$. 
Therefore finding a local minima or maxima of $f$ corresponds to finding the zeros of 
the function $g = \Nabla f$.

### Gradient descent

One of the simplest local optimisation algoriths is *gradient descent*. It is 
initialised at some point in parameter space $a_0$, and at each iteration the function 
$f(x)$ is reduced by following the direction of *steepest descent* $-\Nabla f(a)$ 

$$
a_{n+1} = a_n - \gamma \Nabla f(a_n)
$$

This is an example of an imporant class of algorithms called the *line search* methods. 
These algorithms choose a *search direction* at each iteration $p_k$, and search along 
this direction from the initial point $a_k$ to a new point $a_{k+1} = a_k + \alpha p_k$ 
with a lower function value. The problem at each iteration becomes a one-dimensional 
optimisation problem along $p_k$ to find the optimal value of $\alpha$. Each line search 
algorithm is defined on how it chooses both the search direction $p_k$ and the optimal 
$\alpha$.

### Plataus with low gradient 

An obvious downside to simple gradient descent can be seen for functions which have 
regions of zero or small gradients, or plataus. Here a gradient descent algorithm with a 
constant $gamma$ will proceed very slowly, if at all. This motivates another important 
line search algorithm, *Newtons method*.

The Newtons direction $p^N_k$ can be derived by considering the second-order Taylor 
expansion of the function $f(x)$

$$
f(a_k + p) \approx f(a_k) + p^T \nabla f(a_k) + \frac{1}{2} p^T \nabla^2 f(a_k) = 
m_k(p).
$$

We find the value of $p$ that minimises $m_k(p)$ by setting the derivative of $m_k$ to 
zero, leading to 

$$
p_k^N = - (\nabla^2 f(a_k))^{-1} \nabla f(a_k)
$$

Unlike the steepest descent, Newtons method has a natural step length $\alpha = 1$, 
which is suitable for a wide variety of problems and can quickly cross areas of low 
gradient. Natually, since the algorithm is based on a *second-order* approximation of 
the function $f$, it works better if this approximation is reasonably accurate.

Newtons method can be used as long as the inverse of the second derivative of the 
function $(\nabla^2 f(a_k))^{-1}$, exists (i.e. for a positive definite $\nabla^2 f$). 
However, even when this inverse does exist it is possible that the direction $p^N_k$ 
does not satisfy the descent condition $f(a_k + \alpha p^N_k) < f(a_k)$, (or 
equivilently $\nabla f(a_k)^T p^N < 0$), so many modifications to Newtons methods, 
falling under a class of methods called *Quasi-Newton* methods, have been proposed to 
satisfy this descent condition. You can find more details on these and other imporant 
concepts such as the *Wolfe conditions* for calculating the step length $\alpha$ in 
Chapter 3 of the Nocedal and Wright textbook, or in the other textbooks listed at the 
end of this lession.

### Saddle points

![Two examples of saddle points](/saddle.svg)

Saddle point pose a particular challenge in non-linear optimisation, particularly in 
higher dimensions.

- newton methods can be attracted to saddle points
- saddle points are more likely in higher dimensions
- trust region methods are designed to escape saddle points effectivly
 Hessian of the Lagrangian function of (2.3)–(2.4) is positive semi-definite [53, 113], 
 and that it has at most one non-global local minimizer. Links to quadradic programming 
 and convex optimisation, mention these here

Yuan, Y. X. (2015). Recent advances in trust region algorithms. Mathematical 
Programming, 151(1), 249-281.
https://link.springer.com/article/10.1007/s10107-015-0893-2

https://www.offconvex.org/2016/03/22/saddlepoints/

Pascanu, R., Dauphin, Y. N., Ganguli, S., & Bengio, Y. (2014). On the saddle point 
problem for non-convex optimization. arXiv preprint arXiv:1405.4604.


- easier case: convex optimisation describe what this is 

## Other algorithms

methods mentioned above are all based on the gradient of the function, but many 
alternatives exist

### Derivative-free

otherwise known as Direct Search methods.
- Randomised: e.g: Simulated annealing 
  
  Kirkpatrick, S., Gelatt, C. D., & Vecchi, M. P. (1983). Optimization by simulated 
  annealing. science, 220(4598), 671-680.
https://science.sciencemag.org/content/220/4598/671.abstract?casa_token=6bixtHQ0L6YAAAAA:huR-7qDhWMNe11HjqI40mVepRY2Wsjv4bHcB1g0QwK0S-on0-X_iJzOdVKNJTDmnkJBgtgf5AJyElA

  - Simplex methods, e.g. Nelder-Mead method
  
  Nelder, John A.; R. Mead (1965). "A simplex method for function minimization". 
  Computer Journal. 7 (4): 308–313. doi:10.1093/comjnl/7.4.308.


### Global Optimization

simplest is multi-start
- evolutionary strategies: Covariance matrix adaptation evolution strategy (CMA-ES) is a 
  popular example: 

[1]	The CMA Evolution Strategy: A Tutorial Nikolaus Hanse, arxiv 
    https://arxiv.org/abs/1604.00772
[2]	Hansen, Mueller, Koumoutsakos (2006) “Reducing the time complexity of the derandomized evolution strategy with covariance matrix adaptation (CMA-ES)”. Evolutionary Computation https://doi.org/10.1162/106365603321828970

- Others: , swarm algorithms inspired from bees/ants etc. e.g. Particle Swarm 
  Optimisation: 
  
  [1]	Kennedy, Eberhart (1995) Particle Swarm Optimization. IEEE International 
      Conference on Neural Networks https://doi.org/10.1109/ICNN.1995.488968



a wide variety of metaheuristics algorithms (wikipedia has a large list: 
https://en.wikipedia.org/wiki/Metaheuristic)

  - See a (Non-exhaustive) list of methods here: 

https://github.com/pints-team/pints/issues/684


## Textbooks

Bazaraa, Sherali, and Shetty, Nonlinear Optimization, 2/e, Wiley

Griva, Nash, and Sofer, Linear and Nonlinear Optimization, 2/e, SIAM Press

Luenberger, Linear and Nonlinear Programming, 3/e, Springer

Bertsekas, Nonlinear Programming, Athena

Ruszczynski, Nonlinear Optimization, Princeton University Press

Gould, Nicholas I. M.; Toint, Philippe L. (2000). "A Quadratic Programming Bibliography" 
(PDF). RAL Numerical Analysis Group Internal Report 2000-1. 
ftp://ftp.numerical.rl.ac.uk/pub/qpbook/qp.pdf

Numerical optimization
by Nocedal, Jorge; Wright, Stephen J., 1960-


## Software

scipy.optimise https://docs.scipy.org/doc/scipy/reference/optimize.html
PINTS: https://pints.readthedocs.io/en/latest/optimisers/index.html 
https://pints-team.github.io/pints-methods-overview/
NLOpt: https://nlopt.readthedocs.io/en/latest/NLopt_Algorithms/
PaGMO: https://esa.github.io/pagmo/

{% include links.md %}
