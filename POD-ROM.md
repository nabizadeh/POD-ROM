Reduced Order Model for non-linear Schrödinger Equation
================
Mohamad Nabizadeh
3/7/2022

-   [Problem definition](#problem-definition)
-   [Numerical solution](#numerical-solution)
    -   [Results](#results)
-   [Reduced order modeling](#reduced-order-modeling)
    -   [First-order model](#first-order-model)
    -   [Third-order model](#third-order-model)
        -   [Implementation](#implementation)
        -   [Results](#results-1)

# Problem definition

The objective of this report is to present a numerical solution for the
non-linear Schrödinger equation and to showcase the applicability of
reduced order model on such computationally exhaustive problems.
Schrödinger equation can be written as follows:

<img src="figs/Eq1.png" width="25%" />

Where *u* is the physical state of the system, *u*<sub>*t*</sub> is the
time derivative, *u*<sub>*x**x*</sub> is the second spatial derivative,
and \|*u*\| is a scalar representing the absolute size of *u*. To solve
this ODE, first, let’s keep *u*<sub>*t*</sub> on the left hand side of
the equation and take all the other terms to the right hand side, and
multiply both sides by  − *i*, we get:

<img src="figs/Eq2.png" width="25%" />

# Numerical solution

We can now use Fourier basis algorithm to computationally solve this
equation, taking Fourier transform of both sides gives the following:

<img src="figs/Eq3.png" width="25%" />

Now we can use this first order ODE, and solve it in a time-stepper
loop. A sample code is imlemented in Matlab.

<img src="figs/M1.png" width="40%" />

where the **nls\_rhs** is defined as:

<img src="figs/M2.png" width="50%" />

## Results

<img src="figs/NDimSol.jpg" width="32%" /><img src="figs/VectorWeights.jpg" width="32%" /><img src="figs/first_3_modes.jpg" width="32%" />

1.  Surface plot of the numeric solution, b. weights of different modes
    in percentage, and c. Top three modes.

# Reduced order modeling

## First-order model

Despite the high dimensional nature of the problem, we see -both
visually and quantitatively- that 3 orthogonal components can explain
more than 99% of the standard deviation in the answer. Therefore, we’re
planning to use only those components to build the solution. First,
let’s consider only one component. Let’s use separation of variables:

<img src="figs/Eq4.png" width="15%" />

where,

<img src="figs/Eq5.png" width="25%" />

there is only one mode, and we know that the inner product of each
component by itself is:

<img src="figs/Eq6.png" width="15%" />

Plug that in:

<img src="figs/Eq7.png" width="25%" />

Therefore, in the case of reducing the order to 1, we get to the
following ODE:

<img src="figs/Eq8.png" width="25%" />

which in fact has the following analytical solution.

<img src="figs/Eq9.png" width="25%" />

But, we know that existence of an analytical solution is highly unlikely
for higher orders and we’ll need numerical techniques to aquire a
solution.

## Third-order model

Let’s use the first three orthogonal components now, rewrite the ODE
first:

<img src="figs/Eq10.png" width="25%" />

From seperation of variables, we have:

<img src="figs/Eq11.png" width="17%" />

Also, we have from eigenvalue decomposition the following:

<img src="figs/Eq12.png" width="13%" />

where,

<img src="figs/Eq13.png" width="12%" />

Let’s plug that into our ODE:

<img src="figs/Eq14.png" width="25%" />

Which is our approximation of the solution for this 2D system. Apart
from the equation, we also need initial conditions, where
*s**e**c**h*(*x*) is proposed as an appropriate initial condition for
*u*(*x*, 0). Therefore:

<img src="figs/Eq15.png" width="25%" />

Having established the reduced order model, and the initial conditions,
we can now proceed to the implementation of the model, but before that,
let’s quickly remind about how we calculate a second derivative by the
means of Fourier transform.

<img src="figs/Eq16.png" width="25%" />

Let’s now take a look at our implementation in Matlab:

### Implementation

<img src="figs/M3.png" width="45%" />

where,

<img src="figs/M4.png" width="60%" />

### Results

We can see that using the top three vectors, we can build a model that
can reproduce a solution that closely resembles that of the original
problem. This is a great example on how reduced order modeling can
provide much more efficient solutions with minor loss of information.

<img src="figs/TwoDimSol.jpg" width="40%" />

A special shout-out to [Nathan
Kutz](https://www.youtube.com/channel/UCoUOaSVYkTV6W4uLvxvgiFA) for his
great content on the subject.
