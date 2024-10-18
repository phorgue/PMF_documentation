.. _anisoImpesFoam-tutorials:

anisoImpesFoam-tutorials
========================

Exploitation Cases
------------------

The examples provided in the directory ``tutorials/anisoImpesFoam-tutorials/`` demonstrate the use of the **anisoImpesFoam** solver in practical scenarios.

We consider a basic simulation involving a square domain in the horizontal (:math:`xy`) plane, where the injection of the wetting phase is introduced. The permeability **K** in this domain is represented as a second-order tensor:

.. math:: 
   K_1 = \begin{bmatrix} 10^{-11} & 0 & 0 \\ 0 & 10^{-9} & 0 \\ 0 & 0 & 10^{-10} \end{bmatrix} \quad 
   K_2 = \begin{bmatrix} 10^{-9} & 0 & 0 \\ 0 & 5 \times 10^{-11} & 0 \\ 0 & 0 & 10^{-10} \end{bmatrix}

The computational domain is a square with side length :math:`L = 10m`, discretized into 800 computational cells, with finer meshing near the center to capture more detailed flow dynamics.

The solver reads an injection event file (*injection.evt*) that specifies the injection details at different times, coordinates, and flow rates:

.. code::

	date 0
	4.95 8.1 0.5 -5e-4
	5.05 8.1 0.5 -5e-4
	
	date 20000
	4.95 8.1 0.5 -4e-4
	5.05 8.1 0.5 -4e-4

The following images show the saturation distribution of the wetting phase for two distinct cases:

- **Case 1:**

.. figure:: file:///work/fabregues/milieux_poreux/porousMultiphaseFoam/doc/figures/doc/anisoImpesFoam/case1/visuMeshWettingPhase.png
	:width: 800px
	:alt: Saturation distribution for Case 1
	:align: center
	
- **Case 2:**
	
.. figure:: file:///work/fabregues/milieux_poreux/porousMultiphaseFoam/doc/figures/doc/anisoImpesFoam/case2/visuMeshWettingPhase.png
	:width: 800px
	:alt: Saturation distribution for Case 2
	:align: center
	
In these simulations, the **anisoImpesFoam** solver effectively captures the behavior of anisotropic permeability. In **Case 1**, the flow predominantly follows the preferential permeability direction, which is aligned with the gravity vector. In contrast, **Case 2** exhibits a more uniform spread of the wetting phase due to the modified permeability tensor.

