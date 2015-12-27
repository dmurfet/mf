# mf

Singular library for computing with [matrix factorisations](https://ncatlab.org/nlab/show/matrix+factorization), mathematical objects which appear in singularity theory and topological field theory. Features include:

* Computing Khovanov-Rozansky knot homology (linkhom.lib)
* Fusion of defects in Landau-Ginzburg models (blow.lib)
* Moduli spaces of matrix factorisations (moduli.lib) 

The code is based on the paper of Nils Carqueville and Daniel Murfet, "[Computing Khovanov-Rozansky homology and defect fusion](http://arxiv.org/abs/1108.1081)". Get started by cutting and pasting the examples in the folder `examples` into a running Singular shell.

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

