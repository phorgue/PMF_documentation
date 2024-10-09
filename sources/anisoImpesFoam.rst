.. _anisoImpesFoam:

anisoImpesFoam Solver
=====================

Description
-----------

The **anisoImpesFoam** solver is designed for simulating flow through an anisotropic porous medium. It solves the same set of equations as the **impesFoam** solver, but with the intrinsic permeability **K** treated as a second-order tensor:

.. math:: \displaystyle \nabla \cdot \left[-\frac{\mathbf{K}k_{ra}(S_b)}{\mu_a} \left(\nabla p_a - \rho_a \mathbf{g}\right) \right] + \nabla \cdot \left[-\frac{\mathbf{K}k_{rb}(S_b)}{\mu_b} \left(\nabla p_a - \rho_b \mathbf{g} - \nabla p_c(S_b) \right) \right] = q_a + q_b

.. math:: \epsilon \frac{\partial S_b}{\partial t} + \nabla \cdot \mathbf{U_b} = q_b

.. Where:

.. - :math:`k_{ri}(S_b)` is the relative permeability of phase :math:`i`, which varies between 0 and 1 depending on the local saturation of the wetting phase :math:`S_b`.
.. - :math:`\mu_i` is the dynamic viscosity of phase :math:`i`.
.. - :math:`\rho_i` is the density of phase :math:`i`.
.. - :math:`\mathbf{g}` is the gravity vector (:math:`\mathbf{g} = -9.81 \vec{e_y}`).
.. - :math:`p_c(S_b)` is the macro-scale capillary pressure, which depends on the saturation :math:`S_b`.
.. - :math:`p_a` is the pressure of phase :math:`a`.
.. - :math:`q_i` is a source term (typically used for injection or extraction wells).
.. - :math:`\epsilon` is the porosity of the domain.
.. - :math:`\mathbf{U_b}` is the velocity field of phase :math:`b`.

Where :math:`k_{ri}(S_b)` is the relative permeability of the phase :math:`i`, whose value between :math:`0` and :math:`1` depends on the local saturation of the wetting phase :math:`S_b`, :math:`\mu_i` is the dynamic viscosity, :math:`\rho_i` is the density, :math:`\mathbf{g}` is the gravity field :math:`\left(\mathbf{g} = -9,81 \vec{e_y}\right)`, :math:`p_c(S_b)` is the macro-scale capillary pressure depending on the saturation :math:`S_b`, :math:`p_a` is the pressure of the phase :math:`a`, :math:`q_i` is a source term (used for injection or extraction wells), :math:`\epsilon` is the domain porosity, :math:`\mathbf{U_b}` is the velocity field of the phase :math:`b`.

Note that the **anisoImpesFoam** solver can also be used for isotropic cases, although this will result in higher memory and computation costs due to the added complexity.

Numerical Schemes
-----------------

Due to the significant impact of saturation on the relative permeabilities and capillary pressure, each field :math:`\left(k_{ri}, \mathbf{K}, \frac{\partial p_c}{\partial S_b}\right)` has a user-defined interpolation scheme that can be modified in the simulation configuration files. The following are typical numerical schemes used in validation cases:

- **Relative permeability** :math:`k_{ri}` is treated using a first-order upwind scheme, providing stability in the presence of sharp saturation fronts.
- **Permeability tensor** :math:`\mathbf{K}` is calculated using a harmonic average to handle high heterogeneity in the domain.
- **Capillary pressure derivative** :math:`\frac{\partial p_c}{\partial S_b}` is interpolated linearly.

Configuration Files
-------------------

**constant/transportProperties**

.. code:: 

	activateCapillarity 0.;

	eps [0 0 0 0 0 0 0] 0.5;

	phase.a
	{
	  rho [1 -3 0 0 0 0 0] 1.0;
	  mu [1 -1 -1 0 0 0 0] 1.76e-5;
	}

	phase.b
	{
	  rho [1 -3 0 0 0 0 0] 1e3;
	  mu [1 -1 -1 0 0 0 0] 1e-3;
	}

	relativePermeabilityModel   BrooksAndCorey;

	capillarityModel	BrooksAndCorey;

	BrooksAndCoreyCoeffs
	{
	    // Relative permeability (kr) parameters
	    n 3;

	    // Capillary pressure (pc) parameters
	    pc0 5;
	    alpha 0.2;
	    	
	    Sbmin 0.001;
	    Sbmax 0.999;
	}

	sourceEventFileWater "injection.evt";

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
	
	application     anisoImpesFoam;

	startFrom       latestTime;

	startTime       0;

	stopAt          endTime;

	endTime         2500;

	deltaT          1e-5;

	writeControl    adjustableRunTime;

	writeInterval   250;

	purgeWrite      0;

	writeFormat     ascii;

	writePrecision  6;

	writeCompression off;

	timeFormat      general;

	timePrecision   6;

	runTimeModifiable yes;

	CFL             Coats;

	maxCo           1;

	dSmax           1;

	maxDeltaT       10000;

Required Fields
---------------

- **0/p**: initial pressure field in the domain with boundary conditions

- **0/Sb.org**: initial saturation field for phase :math:`b` in the domain with boundary conditions

- **0/Ua**: initial velocity field for phase :math:`a` in the domain with boundary conditions

- **0/Ub**: initial velocity field for phase :math:`b` in the domain with boundary conditions

- **constant/g**: gravity field

- **constant/K.org**: permeability field
