---
title: "Newtons method"
teaching: 10
exercises: 0
questions:
- "What is the Newtons method for non-linear optimisation?"

objectives:
- "Implement the Newton's algorithm in Python"
---


- Assume the function $f(x)$ you wish to optimise is differentiable, then we can 
  consider the equation of a tangent line at a given point $x_n$

  $$ y(x) = f(x_n) + f'(x_n) (x - x_n)$$

- We want to find the value of $x_{n+1}$ such that $y(x_{n+1}) = 0$, i.e.

  $$ 0 = f(x_n) + f'(x_n) (x_{n+1} - x_n)$$

- Rearange to get:

  $$ x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$$


# Symbolic Differentiation


$$ f'(x) = \lim_{h \rightarrow 0} \frac{f(x + h) - f(x)}{h} $$

- Can calculate the derivative of a given function $f$ by hand, or use a symbolic 
  package. Latter is often less error-prone...

- [Sympy](https://www.sympy.org/en/index.html) in Python, but more powerful specialised 
  packages (e.g. [Mathematica](https://www.wolfram.com/mathematica/)) exist.

# Sympy

- A symbolic package like Sympy allows you to construct a tree-based representation of 
  an equation

```python
import sympy as sp
x = sp.Symbol('x')
f = 2*x + 1
```

\includegraphics[width=0.3\textwidth]{lectures/figs/sympy_test.pdf}

# Sympy diff

- The `diff()` function in Sympy calculates the derivative of an expression tree using 
  standard derivative rules (e.g. chain rule, product rule etc.)
- Results in another expression tree

```python
f = 2*x + 1
df = f.diff()
```

\includegraphics[width=0.2\textwidth]{lectures/figs/sympy_test_diff.pdf}


 \item Implement a simple 2D quadratic function $f(x, y) = x^2 + y^2$, the 2D 
 \href{https://en.wikipedia.org/wiki/Rosenbrock_function}{Rosenbrock
function}, and the 2D \href{https://en.wikipedia.org/wiki/Rastrigin_function}{Rastrigin
    function}, and visualise these
    functions over the domain $-5 < x < 5$ and $-5 < y < 5$


- example of a line search algorithm
- often quasi-Newton method are used in practice, which is any method that replaces the 
  Jacobian with an approximation.
  - simplest quasi-Newton method is the chord method, which replaces H(x) by H(x0) for 
    all iterations.
  - requires the inversion of the hessian, look at the LU decomposition in Scipy 
    (https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu_solve.html)

 Numerical Recipes: The Art of Scientific Computing, Third Edition, 2007.

- Implement Newton's method to optimise the Rosenbrock function in 2D


Broyden, C. G. (1972). "Quasi-Newton Methods". In Murray, W. (ed.). Numerical Methods 
for Unconstrained Optimization. London: Academic Press. pp. 87–106. ISBN 0-12-512250-0.
