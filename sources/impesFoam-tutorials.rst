.. _impesFoam-tutorials:

impesFoam tutorials
===================

The examples provided in the directory ``tutorials/impesFoam-tutorials/`` illustrate the usage of the **impesFoam** solver.

Validation Case 1: Buckley-Leverett
-----------------------------------

The Buckley-Leverett case simulates two-phase flow in a 1D domain with a length of :math:`L = 1 \, \text{m}`, porosity :math:`\epsilon = 0.5`, and intrinsic permeability :math:`K = 1 \times 10^{-11} \, \text{m}^2` using 400 computational cells. In this case, capillary effects are excluded, and a semi-analytical solution is available to compute the shock saturation, front velocity, and saturation profile behind the front.

In this example, the fluids involved are air, water, and oil. Initially, the domain is fully saturated with the non-wetting fluid (air or oil), and water is injected at a velocity of :math:`U_b = 1 \times 10^{-5} \, \text{m} \, \text{s}^{-1}`. For the water-air system, the Brooks and Corey model is applied, and the Van Genuchten model is used for the water-oil system.

In the gravity-driven scenario, with vertical water injection, the front saturation is computed as follows:

.. math:: 
   U_b - \frac{K k_{rb}(S_{b,\text{front}})}{\mu_b} \rho_b g = 0

The resulting front saturations are :math:`S_{b,\text{front}} = 0.467` for the Brooks and Corey model and :math:`S_{b,\text{front}} = 0.754` for the Van Genuchten model. The corresponding front velocities are :math:`4.28 \times 10^{-5} \, \text{m} \, \text{s}^{-1}` and :math:`2.65 \times 10^{-5} \, \text{m} \, \text{s}^{-1}` respectively.

In this validation, the key parameters are:

.. list-table::
   :header-rows: 0
   :align: center

   * - **Fluid**
     - **Density** :math:`\rho \, (\text{kg/m}^3)`
     - **Viscosity** :math:`\mu \, (\text{Pa s})`
   * - Air
     - 1
     - :math:`1.76 \times 10^{-5}`
   * - Water
     - 1000
     - :math:`1 \times 10^{-3}`
   * - Oil
     - 800
     - :math:`1 \times 10^{-1}`
   * - **Model**
     - :math:`m`
     - --
   * - Brooks and Corey
     - 3
     - --
   * - Van Genuchten
     - 0.5
     - --
   * - **Variable**
     - **Value**
     - -- 
   * - :math:`p_a` tolerance
     - :math:`10^{-12}`
     - --
   * - :math:`CFL`
     - :math:`0.75`
     - --
   * - :math:`\Delta S_{b,max}`
     - :math:`0.01`
     - --

Here are visual comparisons for both cases, showing excellent agreement between numerical and analytical results:
   
- **Brooks and Corey model:**

.. list-table::
   :widths: 50 50
   :header-rows: 0

   * - .. figure:: figures/impesFoam/BL_Brooks_gravity.png
        :width: 500px
        :alt: Buckley-Leverett Brooks and Corey gravity case
     
     - .. figure:: figures/impesFoam/BL_Brooks_noGravity.png
        :width: 500px
        :alt: Buckley-Leverett Brooks and Corey no gravity case

- **Van Genuchten model:**

.. list-table::
   :widths: 50 50
   :header-rows: 0

   * - .. figure:: figures/impesFoam/BL_VanGenuchten_gravity.png
        :width: 500px
        :alt: Buckley-Leverett Van Genuchten gravity case
     
     - .. figure:: figures/impesFoam/BL_VanGenuchten_noGravity.png
        :width: 500px
        :alt: Buckley-Leverett Van Genuchten no gravity case

Validation Case 2: Capillary-Gravity Equilibrium
-------------------------------------------------

This validation case explores the two-phase flow (air and water) in a vertical 1D domain of height :math:`H = 1 \, \text{m}` under the influence of capillary pressure. Initially, the domain is filled with the non-wetting phase (air) above the wetting phase (water). The case aims to investigate how capillary forces impact saturation distribution over time.

The domain is initialized with a saturation of :math:`S_b = 0.5` for water in the lower half. Capillary pressure effects are considered, enabling us to observe the balance between gravitational and capillary forces as the system approaches equilibrium.

The governing equation for saturation is derived from mass conservation, given by:

.. math:: 
   \displaystyle \frac{\partial S_b}{\partial y} = \frac{(\rho_b - \rho_a) g_y}{\frac{\partial p_c}{\partial S_b}(S_b)}

This equation describes how the saturation gradient depends on the density difference between the phases and the capillary pressure gradient, resulting in a saturation profile that balances these forces.

Simulations are conducted using both the Brooks and Corey and Van Genuchten models for relative permeability. The parameters for each model are:

.. list-table::
   :header-rows: 1
   :align: center

   * - **Model**
     - :math:`p_{c,0}`
     - :math:`m`
     - :math:`\alpha`
   * - Brooks and Corey
     - 1000
     - --
     - 0.5
   * - Van Genuchten
     - 100
     - 0.5
     - --

The results show the establishment of a stable saturation profile as the system reaches equilibrium. These profiles demonstrate the interaction between capillary pressure and gravity, with good agreement between theoretical predictions and numerical findings.

Visual representations of the results for both models are shown below:

- **Brooks and Corey model:**

.. list-table::
   :widths: 50 50
   :header-rows: 0

   * - .. figure:: figures/impesFoam/PC_BrooksAndCorey.png
        :width: 500px
        :alt: Capillary_Brooks
     
     - .. figure:: figures/impesFoam/Sb_BrooksAndCorey.png
        :width: 500px
        :alt: Capillary_Brooks

- **Van Genuchten model:**

.. list-table::
   :widths: 50 50
   :header-rows: 0

   * - .. figure:: figures/impesFoam/PC_VanGenuchten.png
        :width: 500px
        :alt: Capillary_Brooks
     
     - .. figure:: figures/impesFoam/Sb_VanGenuchten.png
        :width: 500px
        :alt: Capillary_Brooks
