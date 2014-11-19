.. getting_started
.. highlight:: matlab

Getting Started
==================================

Input/Output
----------------------------------

PDB
^^^

Red PDB file
::
  
  [pdb, crd] = readpdb('protein.pdb');

AMBER
^^^^^

AMBER trajectory file
::
  
  natom = 5192;
  trj = readambertrj(natom, 'amber.trj');
  % if you are interested in box size
  [trj, box] = readambertrjbox(natom, 'amber.trj');

AMBER NetCDF file
::
  
  trj = readambertrj('amber.nc');
  % if you are interested in box size
  [trj, box] readambertrj('amber.nc');

Data structures
----------------------------------

Text here

Logical index
----------------------------------

Text here

Atom selections
----------------------------------

Text here

Figures
----------------------------------

Text here

