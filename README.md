# mf

This project is a Singular library for computing with [matrix factorisations](https://ncatlab.org/nlab/show/matrix+factorization), which are mathematical objects which appear in singularity theory and topological field theory. Features include:

* Computing Khovanov-Rozansky knot homology (linkhom.lib)
* Fusion of defects in Landau-Ginzburg models (blow.lib)
* Moduli spaces of matrix factorisations (moduli.lib) 

The code was written by Nils Carqueville and Daniel Murfet as part of the paper "[Computing Khovanov-Rozansky homology and defect fusion](http://arxiv.org/abs/1108.1081)". Other contributors are most welcome! If you have any questions, please don't hesitate to [get in touch](mailto:d.murfet@unimelb.edu.au).

# Installation & Usage

First you need a working installation of the commutative algebra package [Singular](https://www.singular.uni-kl.de/). Singular is available for Linux, Windows and Mac OS X and is [easy to install](https://www.singular.uni-kl.de/index.php/singular-download.html). Then you should either clone this git repository onto your local machine, or [download it as a ZIP file](https://github.com/dmurfet/mf/archive/master.zip).

Then, from within the folder containing this repository, run `Singular`. You will see a welcome screen like this:

```
                     SINGULAR                                 /
 A Computer Algebra System for Polynomial Computations       /   version 4.0.1
                                                           0<
 by: W. Decker, G.-M. Greuel, G. Pfister, H. Schoenemann     \   Sep 2014
FB Mathematik der Universitaet, D-67653 Kaiserslautern        \
```

At this point you can start cutting and pasting the examples in the folder `examples` into the shell. In `examples/blow-example.txt` you will find examples relating to defect fusion (i.e. functors between categories of matrix factorisations) while in `examples/linkhom-example.txt` you will find examples of Khovanov-Rozansky homology calculations.

Here is an example from `blow-example.txt`:

```
//////////////////////////////////////////////////////////////////////////////////////
// EXAMPLE A1 - Tensoring (x,x2) with a factorisation of y^5 - x^3
//////////////////////////////////////////////////////////////////////////////////////

option(noredefine);option(noloadLib);option(redSB);
LIB "blow.lib";
ring rr=0,(x(1),y(1)),dp;
poly W = x(1)^3;
poly V = y(1)^5 - x(1)^3;

matrix X[2][2] = 0, x(1)^2, x(1), 0; // X is a MF of W

// Y is a factorisation of V, which we consider as a functor between MF(W) and MF(y(1)^5).
// We want to compute the image of X under this functor, that is, the fusion of Y and X.
matrix Y0[2][2] = -x(1), y(1), -y(1)^4, x(1)^2;
matrix Y1[2][2] = x(1)^2, -y(1), y(1)^4, -x(1);
matrix z[2][2];
matrix Y = blockmat(z,Y1,Y0,z);

list l = fuseDefects(Y, X, W);
matrix RD = l[1]; // Reduced inflation of Y x X
matrix ep = l[2]; // Idempotent endomorphism of RD (up to homotopy)
ep * ep == ep; // This endomorphism is actually a strict idempotent
matrix A = mfSplitIdempotent(RD, ep)[1]; // Differential on the MF A splitting ep
matrix final = mfSuspend( A, 1 ); // Differential on A[1]

print(final); // This is the differential on the fusion of Y with X

// The answer is a direct sum of two copies of the Koszul factorisation (y(1),y(1)^4)
```

You can paste this directly into the Singular shell. For an explanation of the mathematics behind this code, see [Dyckerhoff-Murfet](http://arxiv.org/abs/1102.2957).