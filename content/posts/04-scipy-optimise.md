---
title: "The scipy optimise module"
teaching: 10
exercises: 0
questions:
- "What Python libraries exist for non-linear optimisation in Python?"
- "How can I use the Scipy Optimise module to minimise an arbitrary function?"
- "What algorithms exist in scipy optimise?"
- "How can I provide scipy optimise with a jacobian"

objectives:
- "Use scipy.optimise to find the mimimum of a test function"
- "Use symbolic differentiation to calculate the jacobian of a test function"
- "Illustrate the differences in performance between (a) different optimisation 
  algorithms, and (b) using numerical or symbolic jacobian"

keypoints:
- "keypoints here"
---

 \item Implement a simple 2D quadratic function $f(x, y) = x^2 + y^2$, the 2D 
 \href{https://en.wikipedia.org/wiki/Rosenbrock_function}{Rosenbrock
function}, and the 2D \href{https://en.wikipedia.org/wiki/Rastrigin_function}{Rastrigin
    function}, and visualise these
    functions over the domain $-5 < x < 5$ and $-5 < y < 5$
  \item Use [Scipy](https://docs.scipy.org/doc/scipy/reference/optimize.html) to
    minimise each of these functions, starting at $x = y = 2.5$, using the following methods:
        \begin{itemize}
        \item Nelder-Mead Simplex
        \item Conjugate Gradient
        \item BFGS Quasi-Newton
        \item Newton-CG
        \item SHG Global Optimisation
        \end{itemize}
        In each case perform the optimisation with and without a jacobian. To calculate
        the jacobian use automatic differentiation via the \href{https://github.com/HIPS/autograd}{autograd} library.
        
    \item Plot the results for the previous question by plotting the parameter values at
      each iteration of the algorithm overlayed on the visualisation of the function to
      be minimised, and label the figure with the number of iterations and function/jacobian/hessian
      evaluations. What can you say about the performance of each algorithm for each
      function? What are the characteristics of each function that make it
      difficult/easy for the optimisation method?
