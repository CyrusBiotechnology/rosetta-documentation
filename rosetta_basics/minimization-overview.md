#Minimization overview and concepts

Metadata
========
This document was written 3 Oct 2007 by Ian W. Davis and last updated 10 Oct 2007.

Information was provided by Jim Havranek.

The relevant Mini-Rosetta code is in core::optimization, especially the core::optimization::Minimizer class. 

Flavors of minimization in Rosetta
==================================

All minimization algorithms choose a vector as the descent direction, determine a step along that vector, then choose a new direction and repeat. Different ways of doing those things leads to differing efficiency, in terms of function evaluations and memory.

"linmin" is a **single step** steepest-descent algorithm with an exact line search. That is, the descent direction is the gradient of the energy function (vector of partial first derivatives), and the step is to the true (local) minimum of the function along that line. Repeated invocations of linmin would converge very slowly, which is why no one does it.

Conjugate gradient minimization accumulates a small amount of information from prior steps. It performs (somewhat) better than steepest descent. Rosetta++ included an implementation of FRPR (Fletcher-Reeves-Polak-Ribiere), but it hasn't been ported to MiniRosetta because no one used it.

Quasi-Newton methods accumulate more information about the second derivatives of the energy function in terms of the inverse Hessian. This information is used to modify the descent direction (after the first step) so that its no longer necessarily straight down the gradient, but the minimization converges (much) faster. In Rosetta, this is the "dfpmin" family of functions. Although named for the DFP (Davidon-Fletcher-Powell) update method, it in fact uses the BFGS (Broyden-Fletcher-Goldfarb-Shanno) update method, which is widely regarded as better. A limited-memory variant (L-BFGS) exists but has not been incorporated into Rosetta because the cost of keeping the full N x N Hessian matrix (N = number of DOFs) in memory appears not to be prohibitive, and Paul Tseng found it gave worse solutions.

Unlike linmin, all the dfpmin algorithms are **multi step** algorithms, so they need only be called once to reach the (local) minimum of a function.

"dfpmin" uses an exact line search, and Jim says it requires \~10 function evaluations per line search.

"dfpmin\_armijo" uses an inexact line search, where the step along the search direction only needs to improve the energy by a certain amount, and also make the gradient a certain amount flatter (but not necessarily reach the minimum). This ends up being significantly more efficient, as once it gets going only 2-3 function evaluations are needed per line search, and approximately the same number of line searches are needed as for the exact dfpmin.

"dfpmin\_armijo\_nonmonotone" uses an even less exact line search along the descent direction, so that the step need only be better than one of the last few points visited. This allows temporary *increases* in energy, so that the search may escape shallow local minima and flat basins. Jim estimates this is several times more efficient than the exact dfpmin.

"dfpmin\_strong\_wolfe" uses the More-Thuente line minimization algorithm that enforces both the Armijo and Wolfe conditions. This gives a better parabolic approximation to a minimum and can run a little faster than armijo.

Jim recommends dfpmin\_strong\_wolfe or dfpmin\_armijo\_nonmonotone.

The meaning of tolerance
========================

Rosetta has at least two kinds of "tolerance" for function minimization, "regular" (for lack of a better name) tolerance and absolute tolerance. "Regular" tolerance is *fractional* tolerance for the *value* of the function being minimized; i.e. a tolerance of 1e-2 means the minimum function value found will be within 1% of the true minimum value. Absolute tolerance is specified without regard to the current function value; i.e. an absolute tolerance of 1e-2 means that the minimum function value found will be equal to the actual minimum plus or minus 1e-2, period.

[This discussion applies only to dfpmin and its variants. Other minimizers may have different notions of tolerance.]
