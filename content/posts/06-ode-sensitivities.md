---
title: "ODE model sensitivities"
teaching: 10
exercises: 0
questions:
- "How can I calculate a gradient for my ODE model likelihood function?"

objectives:
- "Describe forward sensitivity analysis"
- "Summarise adjoint sensitivity analysis, and point to useful external material"
- "Implement forward sensitivity algorithm using scipy solve_ivp"
- "Fit an ODE model to data using sensitivities"
---


# Gradient of the log-likeliood

- We want to minimise the function:

  $$f(\beta) = \sum_j^m (y(x_j,\beta) - z_j)^2$$

- If we want to use a gradient-based optimisation method, we need the gradient wrt 
  $\beta$

  $$\frac{d f}{d\beta} = \sum_j^m 2\frac{dy}{d\beta}(y-z_j)$$

---------------------

- To find $\frac{dy}{d\beta}$, take the derivative of the ODE rate equation

  $$\frac{d}{d\beta} \frac{dy}{dx} = \frac{\partial f}{\partial y}\frac{\partial 
  y}{\partial \beta} + \frac{\partial f}{\partial \beta}$$

- Rearrange to get an ODE equation in terms of $s = \frac{dy}{d\beta}$

  $$\frac{ds}{dx} = \frac{\partial f}{\partial y} s + \frac{\partial f}{\partial 
  \beta}$$

- Solve the augmented ODE system comprising of the equation for $\frac{ds}{dx}$ and 
  $\frac{dy}{dx}$

  $$\frac{d}{dx} \begin{bmatrix}y \\ s\end{bmatrix} = \begin{bmatrix}f \\ \frac{\partial 
  f}{\partial y} s + \frac{\partial f}{\partial \beta}\end{bmatrix}$$


