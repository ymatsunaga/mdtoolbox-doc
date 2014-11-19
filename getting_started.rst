.. getting_started
.. highlight:: matlab

Getting Started
==================================

Data structures
----------------------------------

MDToolbox assumes specific data structures for coordinates and
trajectories. 

Coordiante variable is a row vector whose column has the XYZ
cordinates of atoms in order [x(1) y(1) z(1) x(2) y(2) z(2)
\.\.\. x(natom) y(natom) z(natom)]. 

Thus, for example, the translation in the x-axis by 3.0 Angstrom is
coded in MATLAB as follows:
::
  
  crd(1:3:end) = crd(1:3:end) + 3.0;

Likewise, the translation in the y-axis by 1.5 Angstrom is
::
  
  crd(2:3:end) = crd(2:3:end) + 1.5;

Trajectory variable is an array whose 
column structure is same as that of coordinate. 

The translation in the x-axis is coded as follows:
::
  
  trj(:, 1:3:end) = trj(:, 1:3:end) + 3.0;

The rows of trajectory variable represent time-steps in
simulation. Thus, averages of coordiate variables are coded as
follows: 
::
  
  crd = mean(trj);

Input/Output
----------------------------------

PDB
^^^

PDB file
::

  pdb = readpdb('protein.pdb');
  % if you want to extract the coordinates in PDB
  [pdb, crd] = readpdb('protein.pdb');

AMBER
^^^^^

AMBER trajectory file
::

  % the numbe of atoms is required for reading AMBER trajetory  
  natom = 5192;
  trj = readambertrj(natom, 'amber.trj');
  % if interested in box size
  [trj, box] = readambertrjbox(natom, 'amber.trj');

AMBER NetCDF file
::
  
  trj = readambertrj('amber.nc');
  % if you are interested in box size
  [trj, box] readambertrj('amber.nc');

Logical index
----------------------------------

Text here

Atom selections
----------------------------------

Text here

Figures
----------------------------------

Text here

