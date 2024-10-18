.. _impesFoam:

impesFoam solver
================

Description
-----------

The **impesFoam** solver simulates flow through isotropic porous media. It solves the following equations:

.. math:: 
   \nabla \cdot \left[-\frac{K k_{ra}(S_b)}{\mu_a} \left(\nabla p_a - \rho_a \mathbf{g}\right)\right] + \nabla \cdot \left[-\frac{K k_{rb}(S_b)}{\mu_b} \left(\nabla p_a - \rho_b \mathbf{g} - \nabla p_c(S_b)\right)\right] = q_a + q_b

.. math:: 
   \epsilon \frac{\partial S_b}{\partial t} + \nabla \cdot \mathbf{U_b} = q_b

Where :math:`k_{ri}(S_b)` is relative permeability of phase :math:`i`, varies with local saturation :math:`S_b`, :math:`\mu_i`, :math:`\rho_i` is the dynamic viscosity and density of phase :math:`i`, :math:`\mathbf{g}` is the gravity vector (:math:`\mathbf{g} = -9.81 \vec{e_y}`), :math:`p_c(S_b)` is the capillary pressure depending on saturation :math:`S_b`, :math:`p_a` is the pressure of phase :math:`a`, :math:`\epsilon` is the porosity of the domain, :math:`\mathbf{U_b}` is the velocity field of phase :math:`b`, :math:`q_i` is the source term (injection/extraction).

The **impesFoam** solver is optimized for isotropic cases, reducing memory and computational costs compared to anisotropic solvers.

Relative Permeability Models
-----------------------------

Two relative permeability models are available in the library. Both models utilize the concept of effective saturation, which is a normalized saturation of the wetting phase, defined as follows:

.. math:: \displaystyle \frac{S_b - S_{b,irr}}{1 - \left(S_{a,irr} + S_{b,irr}\right)}

where :math:`S_{a,irr}` and :math:`S_{b,irr}` are the user-defined minimal irreducible saturations for phases :math:`a` and :math:`b`, respectively.

Both correlations depend on the effective saturation :math:`S_{b,eff}`, the power coefficient :math:`m`, and the maximum relative permeabilities :math:`k_{ra,max}` and :math:`k_{rb,max}` (set to :math:`1` by default):

- **Brooks and Corey Model:**

.. math:: \displaystyle k_{ra}(S_{b,eff}) = k_{ra,max}(1 - S_{b,eff})^m
.. math:: \displaystyle k_{rb}(S_{b,eff}) = k_{rb,max} S_{b,eff}^m

- **Van Genuchten Model:**

.. math:: \displaystyle k_{ra}(S_{b,eff}) = k_{ra,max}(1 - S_{b,eff})^{1/2}\left(1 - S_{b,eff}^{1/m}\right)^{2m}
.. math:: \displaystyle k_{rb}(S_{b,eff}) = k_{rb,max} S_{b,eff}^{1/2}\left[1 - \left(1 - S_{b,eff}^{1/m}\right)^m\right]^2


Numerical Schemes
-----------------

Due to the significant impact of saturation on the relative permeabilities and capillary pressure, each field :math:`\displaystyle \left(k_{ri}, \mathbf{K}, \frac{\partial p_c}{\partial S_b}\right)` has a user-defined interpolation scheme that can be modified in the simulation configuration files. The following are typical numerical schemes used in validation cases:

- **Relative permeability** :math:`k_{ri}`: first-order upwind.

- **Permeability** :math:`K`: constant scalar.

- **Capillary pressure derivative** :math:`\displaystyle \frac{\partial p_c}{\partial S_b}`: linear interpolation.

Configuration Files
-------------------

- **Brooks and Corey model:**

**constant/transportProperties**

.. code:: 

	activateCapillarity	0.;

	eps eps [0  0  0 0 0 0 0] 0.5;

	phase.a
	{
	  rho rho [1 -3 0 0 0 0 0] 1e0;
	  mu mu [1 -1 -1 0 0 0 0] 1.76e-5;
	}
		
	phase.b
	{
	  rho rho [1 -3 0 0 0 0 0] 1e3;
	  mu mu [1 -1 -1 0 0 0 0] 1e-3;
	}

	relativePermeabilityModel   BrooksAndCorey;

	capillarityModel	BrooksAndCorey;

	BrooksAndCoreyCoeffs
	{
	    // kr 
	    n 3;

	    // pc 
	    pc0 5;
	    alpha	0.2;
	    
	    //Sbmin	in constant/porousModels
	    Sbmax 0.9999;
	}


**system/fvSolution**

.. code::

	solvers
	{
	    p
	    {
		solver          PCG;
		preconditioner  DIC;
		tolerance       1e-12;
		relTol          0;
	    }

	    Sb
	    {
		solver         no_solver_required;
	    }

	}

**system/controlDict**

.. code:: 
	
	application     impesFoam;

	startFrom       startTime;

	startTime       0.0;

	stopAt          endTime;

	endTime         20000;

	deltaT          1e-5;

	writeControl    adjustableRunTime;

	writeInterval   1000;

	purgeWrite      0;

	writeFormat     ascii;

	writePrecision  6;

	writeCompression no;

	timeFormat      general;

	timePrecision   6;

	runTimeModifiable yes;

	CFL            Coats;

	maxCo		0.75;

	dSmax           1.;

	maxDeltaT	100;
	
- **Van Genuchten model:**

**constant/transportProperties**

.. code::

	activateCapillarity	0.;

	eps eps [0 0 0 0 0 0 0]	0.5;

	phase.a
	{
	  rho	rho [1 -3 0 0 0 0 0] 800;
	  mu	mu [1 -1 -1 0 0 0 0] 0.1;
	}
		
	phase.b
	{
	  rho	rho [1 -3 0 0 0 0 0] 1e3;
	  mu	mu [1 -1 -1 0 0 0 0] 1e-3;
	}

	relativePermeabilityModel  VanGenuchten;

	capillarityModel	VanGenuchten;

	VanGenuchtenCoeffs
	{
	    pc0 5;
	    m	0.5;
	    Sbmin 1e-4;
	    Sbmax 0.9999;
	}

**system/fvSolution**

.. code::

	solvers
	{
	    p
	    {
		solver          PCG;
		preconditioner  DIC;
		tolerance       1e-12;
		relTol          0;
	    }

	    Sb
	    {
		solver          no_solver_required;
	    }

	}


**system/controlDict**

.. code::

	application     impesFoam;

	startFrom       startTime;

	startTime       0.0;

	stopAt          endTime;

	endTime         30000;

	deltaT          1e-5;

	writeControl    adjustableRunTime;

	writeInterval   1000;

	purgeWrite      0;

	writeFormat     ascii;

	writePrecision  6;

	writeCompression no;

	timeFormat      general;

	timePrecision   6;

	runTimeModifiable yes;

	CFL            Coats;

	maxCo		0.75;

	dSmax           1;

	maxDeltaT	100;

Required Fields
---------------

**Brooks and Corey model:**

- **0/p:** initial pressure field in the domain with boundary conditions

- **0/Sb:** initial saturation field for phase :math:`b` in the domain with boundary conditions

- **0/Ua:** initial velocity field for phase :math:`a` in the domain with boundary conditions

- **0/Ub:** initial velocity field for phase :math:`b` in the domain with boundary conditions

- **constant/g:** gravity field

- **constant/K:** permeability field

**Van Genuchten model:**

- **0/p:** initial pressure field in the domain with boundary conditions

- **0/Sa:** initial saturation field for phase :math:`a` in the domain with boundary conditions

- **0/Sb:** initial saturation field for phase :math:`b` in the domain with boundary conditions

- **0/Ua:** initial velocity field for phase :math:`a` in the domain with boundary conditions

- **0/Ub:** initial velocity field for phase :math:`b` in the domain with boundary conditions

- **constant/g:** gravity field

- **constant/K:** permeability field
