.. _installation:

Installation
============

How to install
--------------

You need first a working OpenFOAM installation on you computer.

Then, load the OpenFOAM environment, i.e. for example ::

  source /opt/OpenFOAM-v2206/etc/bashrc

Then in the "porousMultiphaseFoam" directory, run ::

  ./Allwmake -j

to install the package (**-j** allow the parallel compilation).

The PMF dynamic libraries are compiled and stored in the standard OpenFOAM user directory ::

  $FOAM_USER_LIBBIN

while the executable solvers are placed in the standard OpenFOAM user directory ::

  $FOAM_USER_APPBIN.

- Each tutorial directory contains "run" and "clean" files to test installation
  and validate the solver.

- A python script runTutorials.py can be used to test all components.

- To remove compilation and temporary files, run ::

  ./Allwclean --purge

**--purge** is optional and force the deletion of the executables and libraries.
 
- see the ReleaseNotes.txt file for detailed information about the toolbox.

.. _compatibility:

Compatibility
-------------

Depending on your installation, you should switch to the github branch corresponding to your OpenFOAM version. If you use OpenFOAM-v2206 for example::

  git checkout openfoam-v2206

Note that if you want to use the latest version of PMF, it is necessary to have a sufficiently recent installation of openfoam.

*OpenFOAM-v11* and *OpenFOAM-v12* are not supported currently, use OpenFOAM.com versions.

Development branches
^^^^^^^^^^^^^^^^^^^^

- branch **dev** works with OpenFOAM-v2406 `openfoam.com <https://www.openfoam.com/>`_

Updated branches: PMFv2406.0
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- branch **openfoam-v2406**
- branch **openfoam-v2306**
- branch **openfoam-v2206**
- branch **openfoam-v2106**

*openfoam-v10 has errors when using postProcess/setSet in tutorials*

*Note that intermediate version (i.e. v1912, v2012v, v2112...) are not tested.*

Old branches not updated
^^^^^^^^^^^^^^^^^^^^^^^^

- branch **openfoam-v2006**  > PMFv2107.2
- branch **openfoam-v1906**  > PMFv2107.1
- branch **openfoam-v1812**  > PMFv1906
- branch **openfoam-v1806**  > PMFv1809
- branch **openfoam-v1712**  > PMFv1805

- branch **openfoam-v10**    > PMFv2310
- branch **openfoam-v9**     > PMFv2107.2
- branch **openfoam-v8**     > PMFv2107.2
- branch **openfoam-v7**     > PMFv2107
- branch **openfoam-v6**     > PMFv1906
- branch **openfoam-v5**     > PMFv1809

- branch **foam-extend-4.0** > PMFv1809

Version not supported
^^^^^^^^^^^^^^^^^^^^^

- OpenFOAM 4.0 and older
- foam-extend 3.2 and older
- OpenFOAM v1706 and older
