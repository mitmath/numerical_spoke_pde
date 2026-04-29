## 16.C21A/18.C21A Numerical Methods for Partial Differential Equations

This new (**Spring 2026**) MIT subject, part of MIT's Common Ground for Computing Education, introduces numerical methods and numerical analysis to a broad audience (assuming 18.03, 18.06, or equivalents, and some programming experience).  It is divided into two 6-unit halves:

*  [16.C21/18.C21 Interdisciplinary Numerical Methods](https://github.com/mitmath/numerical_hub) (first half-term “hub”): basic numerical methods, including curve fitting, root finding, numerical differentiation and integration, numerical differential equations, and floating-point arithmetic. Emphasizes the complementary concerns of accuracy and computational cost.  [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj) and [Prof. Youssef Marzouk](https://aeroastro.mit.edu/people/youssef-m-marzouk/).

*   Second half-term: two options for 6-unit “spokes”

    *   [16.C21A/18.C21A, Numerical methods for partial differential equations](https://github.com/mitmath/numerical_spoke_pde): finite difference, finite volume, and finite element methods; boundary conditions, accuracy, and stability. [Prof. Youssef Marzouk](https://aeroastro.mit.edu/people/youssef-m-marzouk/).

    *   [16.C21B/18.C21Bm Large-scale linear algebra](https://github.com/mitmath/numerical_spoke_linalg): sparse matrices, iterative methods, randomized methods. [Prof. Steven G. Johnson](http://math.mit.edu/~stevenj).

Taking both the hub and any spoke will count as an 18.3xx class for math majors, similar to 18.330, and as 16.90 for AeroAstro majors.

**This repository holds materials for 16.C21A/18.C21A.**

## 16.C21A/18.C21A Syllabus, Spring 2026

**Instructors**: [Prof. Youssef Marzouk](https://aeroastro.mit.edu/people/youssef-m-marzouk/) and TA [Andrey Bryutkin](https://math.mit.edu/directory/profile.html?pid=2582).

**Lectures**: MWF10 in 45-102 (Mar 30–May 11). 6 units. Prerequisite is 16.C21/18.C21 or permission of instructor.

**Assignments**: 

* 4 weekly **problem sets**, due Wednesdays at 11:59 pm (April 8, 15, 22, 29). You will have **oral check-ins** on 3 psets (randomly selected) where you have to explain your work to the TA or instructor (pass/fail per problem). 

* An individual **final project**, in which you will implement and explore a method for numerical solution of PDEs that goes beyond what was previously covered in class. (This could involve a more sophisticated numerical method and/or a different class of PDEs.) The project will comprise both an oral presentation to the class (May 6, 8, or 11) and a written report (due May 12).

* _No in-class exams and no final exam!_

**Assignment logistics**:

* **Problem set collaboration policy:** Talk to anyone you want to and read anything you want to, with two caveats: First, make a solid effort to solve a problem on your own before discussing it with classmates or googling. Second, no matter whom you talk to or what you read, write up the solution on your own, without having their answer in front of you (this includes ChatGPT and similar). (You can use [psetpartners.mit.edu](https://psetpartners.mit.edu/) to find problem set partners.)

* Homework assignments will require some programming. You can use either **Julia or Python** (your choice; instruction and examples will mostly use Julia).

* Submit your homework *electronically* via [Gradescope on Canvas](https://canvas.mit.edu/courses/36175) as a *PDF* containing code and results (e.g., from a Jupyter notebook) and a scan of any handwritten solutions.

* For accommodations, speak with [S3](https://studentlife.mit.edu/s3) and have them contact the instructors.  


**Grading**: 40% psets + 25% check-ins + 30% final project + 5% lecture attendance.

**Office hours**: Prof. Marzouk: Mondays 4-5 pm in 32-D714 (starting April 6). Prof. Bryutkin: Wednesdays 4-5 pm in 2-231A.

**Resources**: [Piazza discussion forum](https://piazza.com/mit/spring2026/18c21a), [math learning center](https://math.mit.edu/learningcenter/), [TSR^2 study/resource room](https://ome.mit.edu/programs/talented-scholars-resource-room-tsr2), [pset partners](https://psetpartners.mit.edu/).

**Textbook**: No required textbook. We will generally follow **pdf course notes** that will be posted below, along with suggestions for further reading following each lecture. Also, the book [*Fundamentals of Numerical Computation* (FNC)](https://fncbook.com/) by Driscoll and Braun is freely available online, has examples in Julia, Python, and Matlab, and is a valuable extra resource. 

## Meshers for 2D PDE Problems in Python

**Gmsh / pygmsh**: [Gmsh](https://gmsh.info/)

```python
import gmsh
# or: import pygmsh
```

General-purpose 2D/3D mesher for realistic geometries, holes, curved boundaries, and boundary tags.  
Limitation: It is more complex to set up and use than lightweight Python-only mesh generators.

**MeshPy**: [MeshPy](https://pypi.org/project/MeshPy/)

```python
from meshpy.triangle 
import MeshInfo, build
```

Lightweight Python interface to Triangle, useful for polygonal 2D domains with direct access to vertices and elements.  
Limitation: It is less convenient for CAD-like geometries and more complex boundary-tagging workflows.

**meshzoo**: [meshzoo](https://github.com/meshpro/meshzoo)

```python
import meshzoo
points, cells = meshzoo.rectangle_tri(...)
```

Simple mesh generator for standard domains such as rectangles, disks, and triangles.  
Limitation: It is mainly intended for simple geometries and is not suitable for complex domains.

## Lecture 1 (Mar 31)

* 16.C21A/18.C21A overview and syllabus: [slides](https://docs.google.com/presentation/d/1jY9VeOsxbLqwb817167KFCWfLFx68OREWwnDs7A0kw0/edit?usp=sharing)

We talked about the broad and rough categorization of PDEs into equations of elliptic, parabolic, and hyperbolic type. Then we presented certain canonical PDEs and their analytical solutions in simple settings, with a focus on intuitively understanding the behavior that these equations encode: (1) the advection equation $u_t - a u_x$, and (2) the diffusion equation $u_t = \mu u_{xx}$ (for $\mu > 0$), along with their multi-dimensional ($x \in \mathbb{R}^d$ for $d > 1$) analogs. We discussed the notions of initial condition and boundary conditions, and discussed roughly what kinds of boundary conditions make sense to impose in different situations.

## Lecture 2 (Apr 1)

* [Notes on finite difference methods](notes/FDnotes.pdf): Chapter 8
* [Problem Set #1](psets/pset1.ipynb), due on Wednesday April 8

We focused on advection-diffusion equations in one spatial dimension, and discussed how to build a semi-discrete approximation by substituting finite difference approximations of the spatial derivatives. The resulting ODE system can be solved by a variety of means; this approach (build a semi-discrete system and then apply an numerical ODE integration scheme) is generally known as the method of lines. Many—but not all—finite difference methods for PDEs can be constructed this way.

We also talked about simple boundary conditions, specifically Dirichlet and Neumann conditions, first in the continuous setting and then their discrete numerical implementations.

## Lecture 3 (Apr 3)

* [Notes on finite difference methods](notes/FDnotes.pdf): Chapter 8

We talked about how to implement Neumann boundary conditions in a finite different scheme. Then we discussed the notion of local truncation error of a finite difference PDE discretization (and how this differs subtly from the notion of local truncation error we considered for ODE discretization), and derived local truncation error for a forward-time central-space discretization of our running-example advection-diffusion PDE. Then we did tried integrating this system in a simple code and found, in practice, major instabilities!

## Lecture 4 (April 6)

* [Notes on finite difference methods](notes/FDnotes.pdf): Chapter 9
* Julia notebook examples: [this](notes/MatrixEigsFD.ipynb) and [that](notes/FTCS.ipynb)

We focused on matrix stability analysis of finite difference methods, centered on demonstrations of forward-time central-space discretizations of pure advection and advection-diffusion equations. We looked at the eigenvalues of the semi-discrete system and interpreted their locations, for increasing Péclet number (moving from diffusion-dominated to advection-dominated). We also examined the impact of boundary conditions on these eigenvalues and the impact of grid size on these eigenvalues. We also discussed the notion of "numerical domain of dependence," the CFL number, and the CFL condition. Finally, to make our demos work more nicely (e.g., avoiding instabtilities and allowing larger timesteps in the diffusive case), we showed results of implicit (backward Euler) rather than explicit time integration.

## Lecture 5 (April 8)

* [Notes on finite difference methods](notes/FDnotes.pdf): Chapter 10
* [Julia notebook](notes/eigenvalue-scalings.ipynb) to look at eigenvalues and eigenvectors of $A$

We started discussing Fourier stability analysis of finite difference methods. First we looked at very "clean" examples of the eigenvalues of the semi-discrete ODE system matrix $A$ produced by discretizing pure advection and pure diffusion with period boundary conditions and centered differences; we saw that the absolute magnitude of the largest eigenvalues scaled with $\Delta x$ and with $(\Delta x)^2$, respectively. We tried to intepret the eigenvectors as well; for instance, why does the _constant_ (e.g., "all ones") eigenvector correspond to zero eigenvalue? Which eigenvectors are more oscillatory? Then we set about understanding/predicting the entire eigenvalue spectra analytically. We derived the general Fourier solution of the continuous linear advection-diffusion PDE (with periodic BCs) and interpreted it. Then we started looking at the Fourier solution of the semi-discrete equation, but didn't get too far.

## Lecture 6 (April 10)

* [Notes on finite difference methods](notes/FDnotes.pdf): Chapter 10
* Start on [Problem Set #2](psets/pset2.ipynb), due Wednesday April 15
* [Notes on finite volume methods](notes/FVnotes.pdf)


Prof. Bryutkin finished discussing Fourier analysis of finite difference methods, focusing the semi-discretized case. Then he started discussing finite volume methods (sections 7.1 and 7.2 of the notes).


## Lecture 7 (April 13)

* [Notes on finite volume methods](notes/FVnotes.pdf)

We recapped the motivation of finite volume methods, recalling integral forms of conservation laws. Then we discussed finite volume methods for convection in one spatial dimension, introducing the notion of an upwind flux and how to calculate it.

## Lecture 8 (April 15)

* [Notes on finite volume methods](notes/FVnotes.pdf)
* [Demos of finite volume methods, in 1-D and 2-D](notes/FVconvection.ipynb)
* [Method of weighted residuals](notes/FEMnotes.pdf): Chapter 11 of our FEM notes


We discussed extensions of the finite volume method (again focusing on a simple convection problem) to two spatial dimensions, and showed how to calculate the flux terms for a triangular mesh. Then we did some computational experiments (with a rectangular mesh) and discussed conservation properties of the scheme.

Next, we switched to an entirely new topic: the method of weighted residuals (MWR), as motivations for more specific finite element schemes. We didn't quite get to MWR, but rather just set up the problem of approximating our PDE solution in some (generic) basis and started thinking about how to find the coefficients of the approximation in this basis by looking at the residual of the PDE.

## Lecture 9 (April 17)

* [Method of weighted residuals](notes/FEMnotes.pdf): Chapter 11 of our FEM notes
* [Demos/comparisons of collocation and the method of weighted residuals](notes/Collocation-MWR.ipynb)
* Start on [Problem Set #3](psets/pset3.ipynb), due **Thursday April 23** (one day later than usual)

We discussed (i) collocation and (ii) the method of weighted residuals. These are preparation for finite element methods, which we'll start next week.

We also revisited questions from last time about the conservation properties of finite volume schemes, showing that while "total mass" (e.g., $\int U dx$) is conserved, energy-type quantities like $\int |U|^2 dx$ are generally not.

## Lecture 10 (April 20)
* [Method of weighted residuals](notes/FEMnotes.pdf): Chapter 11 of our FEM notes
* [Demos/comparisons of collocation and the method of weighted residuals](notes/Collocation-MWR.ipynb)

We continued our discussion of the method of weighted residuals and demonstrated the Galerkin MWR for a simple diffusion equation, analytically and numerically. We then discussed what is and is not computationally tractable about this method, and in particular how one might want to choose basis functions that make integrals more numerically tractable and that also allow for more complex geometries.


## Lecture 11 (April 22)
* [Finite element methods in one dimension](notes/FEMnotes.pdf): Chapter 12

We launched into details of finite element methods, in one spatial dimension, using the diffusion equation with heterogeneous coefficient as our running example. We defined elements, discussed choices of basis (trial + test) functions, and settled on a linear nodal basis (for now). We then recalled integration by parts and derived equations for the residual that take special advantage of the shape of these basis functions, focused for now on interior nodes. 


## Lecture 12 (April 24)
* [Finite element methods in one dimension](notes/FEMnotes.pdf): Chapter 12
* Computational demos: [FEM with Dirichlet BCs only, in Julia](notes/FEM-1D.ipynb); [FEM with a variety of BCs and convergence plots, in python](notes/FEM-1D_python.ipynb)
* [Problem Set #4](psets/pset4.ipynb), due Friday May 1

We completed our discussion of finite element methods in 1-D: how to assemble the stiffness matrix element-by-element and how to integrate spatially varying source terms and diffusion coefficients. We then discussed how to implement boundary conditions: Dirichlet, Neumann, and Robin. Finally we looked at a computational demo (link above). 

## Lecture 13 (April 27)
* [Higher-order finite element methods](notes/FEMnotes.pdf): Chapter 14
* Computational demos: [1-D diffusion with quadratic nodal basis](notes/FEM1D-higherorder.ipynb) and [1-D diffusion with hierarchical quadratic basis](notes/FEM1D-hierarchical.ipynb)
* **Important:** [Final project guidelines and due dates](psets/projfinal.pdf)

We had a quick review of how to assemble the global stiffness matrix from elemental stiffness matrices. Then we recalled the idea of a "reference element" in 1-D. Then we switched into the main topic of the lecture: higher-order FEM, focusing for now on the quadratic case. We presented a nodal basis for quadratic elements, able to capture arbitrary quadratic functions on each element and providing a _continuous, piecewise quadratic_ approximation $\widetilde{T}$ overall. Then we discussed how to build a hierarchical basis for quadratic elements. We demonstrated both of these choices computationally on our running 1-D diffusion example, paying particular attention to: (1) the sparsity pattern of the global stiffness matrix; and (2) the rate of convergence, which here was 3rd order (i.e., error = $O(\Delta x^3)$ ). 