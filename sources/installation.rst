.. _installation:

Installation
============

Linux instructions
------------------

You need first a working OpenFOAM installation on you computer.

Then, load the OpenFOAM environment, i.e. for example ::

  source /opt/OpenFOAM-v2406/etc/bashrc

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

Windows instructions
--------------------

1. Download the Windows Native version of OpenFOAM v2406, compiled with MinGW:

   https://sourceforge.net/projects/openfoam/files/v2406/OpenFOAM-v2406-windows-mingw.exe/download

2. Install in a folder (e.g., D:\OpenFOAM\v2406)

3. Go to D:\OpenFOAM\v2406\thirdParty and install the required dependency (MPI) using **msmpisetup.exe**

4. Open the OpenFOAM terminal (MSYS2)

5. Create the user directory in the OpenFOAM installation using the command:

    mkdir -p $FOAM_USER_APPBIN

6. Download the latest Windows-compiled version of PMF:

    https://github.com/phorgue/porousMultiphaseFoam/releases/download/v2503/pmf-opensuse-mingw-v2406.zip

5. Unzip the file and copy all files from the *bin/* and *lib/* folders to:

    D:\OpenFOAM\v2406\msys64\home\ofuser\OpenFOAM\USER-v2406\platforms\win64MingwDPInt32Opt\bin\

*Note: Replace “USER” with your actual username)*

*Note: DLL and EXE files must be placed directly in the folder (do not preserve the PMF file directory structure)*

6) The PMF executables should be accessible from the OpenFOAM terminal. Try to run groundwaterFoam.exe
  
.. _compatibility:

Compatibility
-------------

Depending on your installation, you should switch to the github branch corresponding to your OpenFOAM version. If you use OpenFOAM-v2406 for example::

  git checkout openfoam-v2406

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

- branch **openfoam-v10**    > PMFv2310
- branch **openfoam-v9**     > PMFv2310
- branch **foam-extend-4.0** > PMFv1809

Version not supported
^^^^^^^^^^^^^^^^^^^^^

- OpenFOAM 8 and older
- foam-extend 3.2 and older
- OpenFOAM v2006 and older
