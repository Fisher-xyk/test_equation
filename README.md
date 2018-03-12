# Numerical Fokker-Planck solver for nanomagnetic switching #

### What is this repository for? ###

* Quick summary

This code uses finite element method to solve the general 2D Fokker-Planck  equation for nanosize single domain magnet.

* Version 1.0

### Background ###
The magnetization switching of a nanosize magnet can be described by a phenomenological Landau-Lifshitz-Gilbert (LLG) equation (see [Wiki for LLG](https://en.wikipedia.org/wiki/Landau%E2%80%93Lifshitz%E2%80%93Gilbert_equation) in details). The solution to the LLG equation is a trajectory ( as a function of time $t$) of the magnetic moment $\vec{\mathbf{m}}$ under various fields. At finite temperature, magnetization switching is susceptable to the random thermal noise, which makes the LLG equation stochastic. The common approach to simulate the magnetization at finite temperature is to add a white noise to the LLG equation and simulate it for large number of times (sampling method). The emsemble of solutions would give information about the statistics of the magnetization switching in a given environment.

The sampling method is time consuming (especially when once tries to study the rare events, for example, the write error for memory device is usually required to be lower than $10^{-6}$ for embedded applications).  An alternative approach is to solve the corresponding Fokker-Planck equation:

$$
\frac{\partial \rho}{\partial t}=-\vec{\nabla}(\rho \vec{\mathcal{L}}_\mathrm{eff})+D\nabla^2\rho
$$
where $\rho(\mathbf{\vec{m}}, t)$ is the probability density of the magnetization being along direction $\mathbf{\vec{m}}$ at time $t$. $\vec{\mathcal{L}}_\mathrm{eff}$ is the total torque (internal + external) on the magnet. The last term describes the thermal effect. For further details about the theoretical background, please refer to the paper ["Fokker-Planck Study..."](http://ieeexplore.ieee.org/abstract/document/7797620/) . 

The above equation is a convection-diffusion equation that can be solved with Finite Element Method on a unit sphere surface. The time evolution is done through the Crank–Nicolson method.

### How do I get set up? ###

* Dependencies
	* cmake
	* Numerical linear algebra library: [Armadillo](http://arma.sourceforge.net/)
	* HDF5

There are usually three steps in a FEM approach: generating finite element space (mesh generation), converting the PDE into linear equations, and solving the linear equation (usually involves sparse matrices). This code takes care of  step 2 (and 3 with help from external library). For this particular problem, the domain shape and size is fixed (it is a 2D surface on a unit sphere) so I did not use any sophisticated mesh generation library. Rather, I treated as an external input in the HDF5 format. For my calculations/tests, I used the Matlab program [Dismesh from Berkely](http://persson.berkeley.edu/distmesh/) to generate meshes and save it as a HDF5 file.

* Installation
	* Install [armadillo](http://arma.sourceforge.net/) library
	* Install [HDF5](https://portal.hdfgroup.org/display/support/Downloads) ( make sure to use c++ version that includes "H5Cpp.h")
	* mkdir ./build_dir
	* cd ./build_dir
	* cmake /path/to/CMakeLists.txt
	* make
* How to run the code
	* ./2DFPE param.dat

There needs to be a input file (param.dat) that contains all the parameters of the simulation (include physical parameters for the simulated system and simulation parameters). The parameter file has the following format:
* comment line starts with symbol # or $
* [category]key  value(s) (e.g. "[mag]Magnetization 1e6")
See the example 'param.dat' in the repository for illustration.

> Written with [StackEdit](https://stackedit.io/).
