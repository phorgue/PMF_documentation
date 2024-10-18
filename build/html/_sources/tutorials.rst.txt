.. _solvers:

Tutorials
=========

.. toctree::
   :maxdepth: 1

   anisoImpesFoam-tutorials
   groundwater2DFoam-tutorials
   groundwaterFoam-tutorials
   groundwaterTransport2DFoam-tutorials
   groundwaterTransportFoam-tutorials
   impesFoam-tutorials
   porousScalarTransport2DFoam-tutorials
   porousScalarTransportFoam-tutorials
   
.. _2D-solver-introduction-tutorials:

**2D Solvers for Groundwater Modeling**

The 2D solvers provided in this toolbox are designed for efficient modeling of groundwater flows in porous media. These solvers leverage the Dupuit-Forchheimer approximation, which simplifies the problem by reducing it to two dimensions, assuming the vertical velocity component is negligible.

This approximation significantly reduces the computational cost, making the 2D solvers ideal for large-scale simulations of aquifers and watersheds, especially when detailed 3D simulations are not necessary or feasible. By solving for water height and flow, without capillary effects, the solvers can accurately simulate the dynamics of water and solute transport in both saturated and unsaturated zones.

The following solvers are available in the 2D groundwater modeling suite:

- **steadyGroundwater2DFoam** for steady-state simulations

- **groundwater2DFoam** for transient groundwater flow

- **porousScalarTransport2DFoam** for passive tracer transport

- **groundwaterTransport2DFoam** for coupled water and solute transport simulations

Here a quick view of the watershed for every 2D validation cases:

.. list-table::
   :widths: 50 50
   :header-rows: 0

   * - .. figure:: file:///work/fabregues/milieux_poreux/porousMultiphaseFoam/doc/figures/doc/2D_solver/2D_watershed_modeling/mesh.png
        :width: 500px
        :alt: mesh

     - .. figure:: file:///work/fabregues/milieux_poreux/porousMultiphaseFoam/doc/figures/doc/2D_solver/2D_watershed_modeling/permeability_field.png
        :width: 500px
        :alt: permeability_field
        
.. list-table::
   :widths: 50 50
   :header-rows: 0

   * - .. figure:: file:///work/fabregues/milieux_poreux/porousMultiphaseFoam/doc/figures/doc/2D_solver/2D_watershed_modeling/DEM_for_lower_limit.png
        :width: 500px
        :alt: DEM_for_lower_limit

     - .. figure:: file:///work/fabregues/milieux_poreux/porousMultiphaseFoam/doc/figures/doc/2D_solver/2D_watershed_modeling/DEM_for_upper_limit.png
        :width: 500px
        :alt: DEM_for_upper_limit
        
With: (a) Mesh - (b) Permeability field - (c) DEM for upper limit - (d) DEM for lower limit

