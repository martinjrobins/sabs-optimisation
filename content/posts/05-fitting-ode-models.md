---
title: "Fitting ODE models"
teaching: 10
exercises: 0
questions:
- "How can I use non-linear optimisation to fit an ODE model to data?"
- "What is a likelihood funciton and how is it calculated?"

objectives:
- "Calculate the likelihood function for an ODE model with a iid gaussian noise 
  model"
- "Use scipy.optimise to maximise this likelihood"

keypoints:
- "keypoints here"
---


# Fitting ODE models

- Our model is now:

  $$\frac{dy}{dx} = f(x, y, \beta)$$

- More difficulte to evaluate than a polynomial model, but can take advantage of 
  domain-specific mechanistic models of the underlying process. The parameters $\beta$ 
  are often physically relevent parameters

- How to fit? Simplest approach is to assume that the data has been sampled from a 
  normal distribution with the mean given by the solution of the ODE:

  $$z_j \sim y(x_j, \beta) + \mathcal{N}(0, \sigma)$$

---------------------

- We can calculate a likelihood of a given set of parameters $\beta$, given the data 
  $\mathbf{y}$:

  $$\mathcal{L}(\beta | \mathbf{z}) = \prod_j^m \frac{1}{\sqrt{2 \pi \sigma^2}} \exp 
  \left( -\frac{(z_j-y(x_j,\beta))^2}{2 \sigma^2} \right)$$

- Normally work with the log-likelihood:

  $$\log \mathcal{L}(\beta | \mathbf{z}) = -\frac{m}{2} \log(2\pi) - m \log(\sigma) 
  - \frac{1}{2\sigma^2} \sum_j^m (z_j-y(x_j,\beta))^2$$

- We want to find the parameters $\beta$ that maximise the log-likelihoood. Assuming $m$ 
  and $\sigma$ are fixed, this corresponds to minimising the sum-of-squares of the 
  residuals:

  $$\sum_j^m (z_j-y(x_j,\beta))^2$$


\begin{enumerate}[label=\Alph*]
    \item The logistic growth equation is given by

      $$ \frac{dy}{dt} = r y (1 - y/k) $$

      Solve this equation using $r = 10$, $k = 1$, and intial condition of $y(0) = p_0
      = 1e-2$ using `scipy.integrate.odeint`, and compare with the exact solution
      given by

      $$ y(t) = \frac{k}{1 + (k/p_0 - 1) \exp(-r t)} $$

  \item If $\mathbf{\omega} = [r, k] $ is the parameters and $f(\omega, y)$ is the RHS
    of the ODE, calculate $\partial f/ \partial \omega$ and $\partial f/ \partial y$,
    and use forward sensitivity analysis to calculate the sensitivities $\partial y(t) /
    \partial r$ and $\partial y(t) / \partial k$. Compare this to the exact solution for
    the sensitivities (i.e. calculate the derivative of the the exact solution above)

  \item Generate some fake data using $r = 10$, $k = 1$, and $p_0 = 1e-2$, using
    independent normally distributed measurement noise with a variance of 0.1.  Attempt
    to recover the true parameters $r,k$ using an initial guess of $r=1$, $k=5$ using
    the Nelder-Mead, BFGS, and Newton-CG optimisation methods. 
