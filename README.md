# mf

This project is a Singular library for computing with [matrix factorisations](https://ncatlab.org/nlab/show/matrix+factorization), mathematical objects appearing in singularity theory and topological field theory. Features include:

* Computing Khovanov-Rozansky knot homology (`linkhom.lib`)
* Fusion of defects in Landau-Ginzburg models (`blow.lib`,`mfweb.lib`)
* Moduli spaces of matrix factorisations (`moduli.lib`) [preliminary]
* A-infinity minimal models of matrix factorisations (`ainfmf.lib`) [preliminary]

The code was written by [Nils Carqueville](http://nils.carqueville.net/) and [Daniel Murfet](http://therisingsea.org) as part of the paper "[Computing Khovanov-Rozansky homology and defect fusion](http://arxiv.org/abs/1108.1081)". Other contributors are most welcome! If you have any questions, please don't hesitate to [get in touch](mailto:d.murfet@unimelb.edu.au).

# Installation & Usage

First you need a working installation of the commutative algebra package [Singular](https://www.singular.uni-kl.de/). Singular is available for Linux, Windows and Mac OS X and is [easy to install](https://www.singular.uni-kl.de/index.php/singular-download.html). Then you should either clone this git repository onto your local machine, or [download it as a ZIP file](https://github.com/dmurfet/mf/archive/master.zip).

From within the folder containing this repository, run `Singular`. You will see a welcome screen like this:

```
                     SINGULAR                                 /
 A Computer Algebra System for Polynomial Computations       /   version 4.0.1
                                                           0<
 by: W. Decker, G.-M. Greuel, G. Pfister, H. Schoenemann     \   Sep 2014
FB Mathematik der Universitaet, D-67653 Kaiserslautern        \
```

At this point you can start cutting and pasting into the Singular shell from the files in the folder `examples` of this repository. In `examples/examples.txt` and `examples/blow-example.txt` you will find examples relating to defect fusion (i.e. functors between categories of matrix factorisations) while in `examples/linkhom-example.txt` you will find examples of Khovanov-Rozansky homology calculations.

Here is an extract from `examples.txt` which shows the canonical way to compute fusions using the library:

```
////////////////////////////////////////////////
// Example of defect fusion using webCompilePair
////////////////////////////////////////////////

// For the fusion of defects with boundary conditions, or defect-defect fusion
// (i.e. convolution of matrix factorisation "kernels") we use webCompilePair,
// which takes as input an oriented graph (called a "web") whose edges are
// decorated with potentials (i.e. the equation of an isolated singularity) and
// whose vertices are decorated with matrix factorisations. The output is a finite
// rank matrix factorisation homotopy equivalent to the "total factorisation" of
// the web. All these terms are defined more precisely in the preamble of mfweb.lib
// and the paper arXiv: 1108.1081, but let us proceed to the example.
//
// The web has two vertices (marked with X,Y) and two edges (marked x^3, y^5)
//
//    X ---- x^3 ----> Y ---- y^5 ---->
//
// Here the second edge (labelled with y^5) goes to the boundary.
//
// For this to be a valid web, Y needs to be a matrix factorisation of y^5 - x^3
// (i.e. outgoing potential minus incoming potential) while X needs to be a matrix
// factorisation of x^3. The total factorisation is Y \otimes X, as a matrix
// factorisation of y^5 over the variables y. This is infinite rank but is
// homotopy equivalent to a finite rank matrix factorisation.
//
// This finite rank matrix factorisation is, by definition, the image of
// X \in hmf(x^3) under the functor hmf(x^3) --> hmf(y^5) induced
// by the kernel Y. Let us compute this finite rank guy now. Then we will move
// onto more complicated examples.

option(noredefine);option(noloadLib);option(redSB);
LIB "mfweb.lib";
ring rr=0,(x,y),dp;

// We encode the web given above as a pair of edges, and a pair of vertices.
// Format for an edge is (source, target, variable, potential). Start indexing
// your vertices at 1, since 0 stands for the boundary.
list e1 = list(1,2,list(x),x^3);
list e2 = list(2,0,list(y),y^5);
list edgeList = list(e1,e2);

// Now define the mfs X,Y to be placed at each vertex 
matrix X[2][2] = 0, x^2, x, 0;

matrix Y0[2][2] = -x, y, -y^4, x^2;
matrix Y1[2][2] = x^2, -y, y^4, -x;
matrix z[2][2];
matrix Y = blockmat(z,Y1,Y0,z);

list mfs = X, Y;

// Define the web
list web = list(2, edgeList, mfs); // 2 for the number of vertices
webVerify(web);

// The compilation of this web is the fusion of Y with X. 
list compStrat = defaultCompStratForWeb(web);
list L = webCompilePair(web, compStrat);
print(L[1]);

// The answer is a direct sum of two copies of the Koszul factorisation (y,y^4)
```

For an explanation of the mathematics behind this code, see [Dyckerhoff-Murfet](http://arxiv.org/abs/1102.2957) and [Murfet](http://arxiv.org/abs/1402.4541). There is also extensive documentation within the libraries themselves, see `blow.lib`, `mfweb.lib` and `linkhom.lib`.