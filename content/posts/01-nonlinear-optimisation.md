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
---


## Non-linear optimisation and Root-Finding

- specify form of nlopt to be solved
- root finding is closely related to nlopt, maxima/minma can be found at zeros of the 
  gradient
- nlopt is a difficult problem: illustrate three potential problems:


### local minima/maxima

- introduce gradient descent

### plataus with low gradient 

- gd proceeds slowly
- newton methods fix this by introducing second order gradients


### saddle points

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
