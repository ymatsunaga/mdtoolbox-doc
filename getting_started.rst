.. getting_started
.. highlight:: matlab

Getting Started
==================================

Data structures for coordinate and trajectory
---------------------------------------------

MDToolbox assumes a simple vector/array form for coordinate/trajectory.

Coordinate variable is a row vector whose elements are the XYZ (Cartesian) 
coordinates of atoms in order
:: highlight:: none
  
  [x(1) y(1) z(1) x(2) y(2) z(2) ... x(natom) y(natom) z(natom)]

Thus, for example, translation in the x-axis by 3.0 Angstrom is
coded as follows:
::
  
  crd(1:3:end) = crd(1:3:end) + 3.0;

Likewise, the y-coordinate of geometrical center is calculated by
::
  
  center_y = mean(crd(2:3:end));

Trajectory variable is an array whose 
column vector consists of coordinate variable.
The rows represent snapshots of coordinates.
The snapshots may correspond to time-steps for simulation data.

For example, translation in the x-axis throughout all snapshots is coded as follows:
::
  
  trj(:, 1:3:end) = trj(:, 1:3:end) + 3.0;

The coordinates at the 10th step is extracted by
::
  
  crd = trj(10, :);

Average coordinate over snapshots (without fitting) is coded by
::
  
  crd = mean(trj);

Input/Output for coordinate and trajectory
------------------------------------------

PDB
^^^

PDB file
::
  
  pdb = readpdb('protein.pdb');
  [pdb, crd] = readpdb('protein.pdb'); % if you want to extract the coordinate
  % after some calculations
  writepdb('protein_edit.pdb', pdb);
  writepdb('protein_edit.pdb', pdb, crd); % if you want to replace coordinate with crd

AMBER files
^^^^^^^^^^^

AMBER trajectory file
::
  
  natom = 5192; %the numbe of atoms is required for reading AMBER trajetory  
  trj = readambertrj(natom, 'run.trj');
  % after some calculations
  writeambertrj('run_edit.trj', trj);

AMBER NetCDF trajectory file
::
  
  trj = readnetcdf('run.nc');
  % after some calculations
  writenetcdf('run_edit.nc', trj);

CHARMM/NAMD files
^^^^^^^^^^^^^^^^^

DCD file
::
  
  trj = readdcd('run.dcd');
  % after some calculations
  writenetcdf('run_edit.nc', trj);

Atom selections
----------------------------------

Text here

Figures
----------------------------------

Text here

