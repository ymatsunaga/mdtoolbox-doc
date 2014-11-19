.. getting_started
.. highlight:: matlab

Getting Started
==================================

Data structures
----------------------------------

MDToolbox assumes specific data structures for coordinates and
trajectories. 

Coordinate variable is a row vector whose column has the XYZ
coordinates of atoms in order [x(1) y(1) z(1) x(2) y(2) z(2)
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
simulation. Thus, average of coordinates in the trajectory is coded as
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
  % or if you want to extract the coordinates in PDB
  [pdb, crd] = readpdb('protein.pdb');
  
  % after some calculations
  writepdb('protein_edit.pdb', pdb);

AMBER
^^^^^

AMBER trajectory file
::
  
  % the numbe of atoms is required for reading AMBER trajetory  
  natom = 5192;
  trj = readambertrj(natom, 'amber.trj');
  % or if the simulation was done under periodic boundary condition
  [trj, box] = readambertrjbox(natom, 'amber.trj');
  
  % after some calculations
  writeambertrj('amber_edit.trj', trj)
  % or if box size is needed
  writeambertrj('amber_edit.trj', trj, box)

AMBER NetCDF file
::
  
  trj = readnetcdf('amber.nc');
  % or if box size is needed
  [trj, box] = readnetcdf('amber.nc');
  
  % after some calculations
  writenetcdf('amber_edit.nc', trj)
  % or if box size is needed
  writenetcdf('amber_edit.nc', trj)

Logical index
----------------------------------

Text here

Atom selections
----------------------------------

Text here

Figures
----------------------------------

Text here

