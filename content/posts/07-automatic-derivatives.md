---
title: "Automatic Differentiation"
teaching: 10
exercises: 0
questions:
- "How can I automate the process of calculating a gradient or Jacobian?"
objectives:
- "Describe forward and reverse automatic differentiation"
- "Manually implement a simple example of each"
- "Use a library (JAX) to calculate the Jacobian of a Python function"
---


# Numerical Differentiation

Finite Difference approximation can be constructed from the Taylor series
expansion:

$$
f(x_0 + h) = f(x_0) + h f'(x_0) + \frac{h^2}{2!} f''(x_0) + \frac{h^3}{3!} f'''(x_0) + 
...
$$
$$
f(x_0 - h) = f(x_0) - h f'(x_0) + \frac{h^2}{2!} f''(x_0) - \frac{h^3}{3!} f'''(x_0) + 
$$

leads to:

$$\text{Forward Difference: }f'(x_0) = \frac{f(x_0 + h) - f(x_0)}{h} + \mathcal{O}(h)$$
$$\text{Backwards Difference: }f'(x_0) = \frac{f(x_0 + h) - f(x_0)}{h} + 
\mathcal{O}(h)$$
$$\text{Central Difference: }f'(x_0) = \frac{f(x_0 + h) - f(x_0 - h)}{2h} + 
\mathcal{O}(h^2)$$

# Automatic Differentiation

- Automatic differentiation is the application of the Chain Rule to a computer program
- Imagine a Python function with an input $x$ and result $y$. Statements in this 
  function might consist of:

\begin{align*}
  w_0 &= x \\
  w_1 &= h(w_0) \\
  w_2 &= g(w_1) \\
  w_3 &= f(w_2) \\
  y &= w_3 \\
\end{align*}

-------------------

- This is the same as:

$$
y = f(g(h(x)))
$$

- And the chain rule gives 


$$
\frac{\partial y}{\partial x} = \frac{\partial y}{\partial w_3} \frac{\partial 
w_3}{\partial w_2} \frac{\partial w_2}{\partial w_1} \frac{\partial w_1}{\partial w_0} 
\frac{\partial w_0}{\partial x}
$$


# Forward mode AD

- Evaluate the derivative from right to left, calculating each $\partial w_i / \partial 
  x$ in turn:

$$
\frac{\partial y}{\partial x} = \frac{\partial y}{\partial w_3} \left( \frac{\partial 
w_3}{\partial w_2} \left( \frac{\partial w_2}{\partial w_1} \left( \frac{\partial 
w_1}{\partial w_0} \left( \frac{\partial w_0}{\partial x} \right) \right) \right) 
\right)
$$

- This corresponds to the natural flow of the program (i.e. top to bottom)
- To compute, each program statement is augmented to also calculate $\partial w_i / 
  \partial x$
- Require one pass of forward mode AD for each input, efficient for functions with less 
  inputs than outputs.

# Forward mode AD example 

Note: taken from [wikipedia](https://en.wikipedia.org/wiki/Automatic_differentiation)

$$y = f(x_0, x_1) = x_0 x_1 + \sin(x_1)$$

- LHS shows each operation used to calculate the expression
- RHS shows how each each operation is augmented to calculate the derivative

\begin{center}
\begin{tabular}{ |l|l| } Evaluate $y$ & Evaluate $\partial y / \partial x_0$ \\
  $w_0 = x_0$ & $w_0' = 1$ \\ $w_1 = x_1$ & $w_1' = 0$ \\
  $w_2 = w_0 w_1$ & $w_2' = w_0' w_1 + w_0 w_1'$ \\
  $w_3 = \sin w_0$ & $w_3' = w_0' \cos w_0$ \\
  $w_4 = w_2 + w_3$ & $w_4' = w_2' + w_3'$\\
  $y = w_4$ & $y' = w_4'$
\end{tabular}
\end{center}


# Reverse mode AD

- Evaluate the derivative from left to right, calculating the *adjoint* $\overline{w}_i 
  = \frac{\partial y}{\partial w_i}$:

$$
\frac{\partial y}{\partial x} = \left( \left( \left( \left( \frac{\partial y}{\partial 
w_3} \right) \frac{\partial w_3}{\partial w_2} \right) \frac{\partial w_2}{\partial w_1} 
\right) \frac{\partial w_1}{\partial w_0} \right) \frac{\partial w_0}{\partial x}  $$

- Corresponds to the reverse flow of the program. Need to keep a record of what 
  operations have been performed, and in which order (i.e. reverse mode AD execution 
  occurs from bottom to top)
- Computational cost scales with the number of *outputs*, efficient for functions with 
  more inputs than outputs.

# Reverse mode AD example 

Note: taken from [wikipedia](https://en.wikipedia.org/wiki/Automatic_differentiation)

$$y = f(x_0, x_1) = x_0 x_1 + \sin(x_1)$$

1. Evaluate $y$:
  \begin{align*}
    w_0 &= x_0  \\
    w_1 &= x_1 \\
    w_2 &= w_0 w_1\\
    w_3 &= \sin w_0\\
    w_4 &= w_2 + w_3\\
    y &= w_4
  \end{align*}

---------------------

2. Evaluate $\partial y / \partial x$:
 \begin{align*}
   \overline{w}_4 &= 1 \\
   \overline{w}_3 &= \overline{w}_4 \\
   \overline{w}_2 &= \overline{w}_4 \\
   \overline{w}_1 &= \overline{w}_2 w_0 \\
   \overline{w}_0 &= \overline{w}_3 \cos w_0 + \overline{w}_2 w_1\\
   y' &= \overline{w}_0
  \end{align*}

