---
title: "Constrained Model-based Relaxation Parameter Mapping using Balanced Steady State Free Precession"
collection: publications
permalink: /publication/2024-07-30-paper-title-number-7
date: 2024-07-30
venue: ' Abstract for British Chapter International Society for Magnetic Resonance in Medicine 2024'
citation: Zimu Huo, Minghao Zhang, Yiyun Dong, Joshua Kaggie, Pete Lally, Neal Bangerter, Michael Hoff, Martin Graves
---


[Conference Abstract](../files/BCISMRM_bssfp.pdf)

## Background
The steady-state condition in phase-cycled bSSFP is influenced by various factors including off-resonance, flip angle, RF phase, and crucially, T1, T2, and M0 (initial magnetization). By acquiring sufficient phase cycle data, we can derive quantitative parameters from the images.

## Current Approach
An elliptical fitting model has been proposed to extract T1 and T2 information, where an ellipse is fitted per voxel across the phase cycle dimension. Yiyun has been advancing the mathematical aspects of this model to enhance the accuracy of the quantitative information derived from fewer data points.


## Proposed Approach (simplified)


In the work, we aim to cast the parameter mapping as an unconstrained nonlinear optimization problem of the standard form: 

$$
\underset{T_1, T_2, M_0, B_0}{\text{minimize}}\;\;
C = || F  \cdot \text{M}(T_1, T_2, M_{0}, B_{0}) - y ||^2 
$$

Here, \(y\) signifies the collected data, \(F\) denotes the linear Fourier transform operator, and \(M\) operator denotes the nonlinear steady state magnetization equations. The goal is to find the \(T_1\), \(T_2\), \(M_0\), and \(B_0\) maps that best align with the acquired k-space measurement \(y\). \(R\) is an optional regularization function. 

For practical implementations, we aim to provide detailed descriptions of each operator and its implementation. We define the following variables:

- **ny**: the number of phase encoding steps
- **nx**: the number of frequency encoding steps
- **ph**: the number of phase cycles
- **ndim**: the number of different parameters we aim to estimate

We will use these notations throughout the following sections. For example, the acquired data \( \mathbf{y} \) has dimensions [ny * nx * ph, 1]. The operator \(\mathbf{M}\) has dimensions [ny * nx * ph, ny * nx * ndim]. The matrix \(\mathbf{F}\) has dimensions [ny * nx * ph, ny * nx * ph].

To efficiently solve for the desired parameters, it is necessary to compute the corresponding gradient vector and Hessian matrix. The closed-form expression for steady-state magnetization enables the calculation of these gradients and the Hessian with respect to the parameters \(T_1\), \(T_2\), \(M_0\), and \(B_0\) in K-space with a simple substitution.

## Calculating the Exact Gradient

The gradient vector provides first-order information of the cost function regarding the local behavior of the cost function. By following the direction of the negative gradient, one can iteratively update the parameters to find a local minimum, which is the goal in many optimization problems.

For the sake of simplicity, we ignore the regularization term. The gradient can be calculated by taking the derivative of the least square cost function with respect to the desired parameters.

$$
\begin{aligned}
\triangledown C & = ( F  \cdot \partial M )^\mathrm{H} \cdot (F \cdot B - y)\\
              & = ( F  \cdot J )^\mathrm{H} r
\end{aligned}
$$

\(J\) is the Jacobian matrix of the steady state equation 3 with respect to each parameter. The first term is the projection of the derivatives of each relaxation parameter into k-space, and the second term \(r\) is the residual between the current estimate and the acquired k-space data \(y\). 

The Jacobian matrix is of dimension [1, 1, ph, ny, nx, ndim]. This corresponds to a pixel-wise derivative with respect to the current estimate in the image domain. The Jacobian matrix is then element-wise multiplied with a Fourier basis function to propagate the image domain derivative to k-space, resulting in a dimension of [ny * nx * ph, ny * nx * ndim]. Alternatively, it can be reshaped to [ny * nx * ph, ny * nx * ndim]. The residual vector \(r\) is of dimension [ny * nx * ph, 1]. The Jacobian matrix in Fourier domain is then matrix-multiplied with the residual vector to yield a gradient vector with dimension [ny * nx * ndim, 1]

## Calculating the Exact Hessian

The Hessian matrix, which is the second derivative of the cost function, provides information about the curvature of the cost function and is essential for second-order optimization methods. Similarly, the Hessian matrix can also be derived analytically by appling the chain rule on the gradient equation.

$$
\begin{aligned}
\nabla^2 C & =(F \cdot J)^H \cdot (F \cdot J) + (F \cdot \nabla J)^\mathrm{H} ( F \cdot M - y)
\end{aligned}
$$

The first term represents the Gauss-Newton approximation of the Hessian matrix. The Gauss-Newton method is used in non-linear least squares problems and approximates the Hessian matrix by ignoring the second derivative terms of the residuals. Specifically, it considers only the product of the Jacobian matrix with its conjugate transpose. This approximation is often efficient and sufficient for many optimization problems when the local behavior is relatively linear. The second correction term captures additional curvature information that arises from the changes in the Jacobian itself with respect to the parameters. It becomes important in cases where the non-linearity of the model is significant, and the Gauss-Newton approximation alone is not accurate enough.
