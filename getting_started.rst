.. getting_started
.. highlight:: matlab

Getting Started
==================================

Data structures for coordinate and trajectory
---------------------------------------------

MDToolbox assumes a simple vector/array form for coordinate/trajectory.

Coordinate variable is a row vector whose elements are the XYZ coordinates of atoms in order
::
  
  [x(1) y(1) z(1) x(2) y(2) z(2) .. x(natom) y(natom) z(natom)]

Thus, for example, translation in the x-axis by 3.0 Angstrom is coded as follows:
::
  
  crd(1:3:end) = crd(1:3:end) + 3.0;

Likewise, the y-coordinate of geometrical center is calculated by
::
  
  center_y = mean(crd(2:3:end));

Trajectory variable is an array whose column vectors consist of
coordinate variables. The rows represent snapshots of coordinates. 
For simulation data, the snapshots may correspond to time-steps. 

Thus, for example, translation in the x-axis throughout all snapshots is coded as follows: 
::
  
  trj(:, 1:3:end) = trj(:, 1:3:end) + 3.0;

The coordinate at the 10th snapshot is extracted by
::
  
  crd = trj(10, :);

Average coordinate over all snapshots (without fitting) is obtained by
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

Atom selection
----------------------------------

MDToolbox uses `logical indexing
<http://blogs.mathworks.com/loren/2013/02/20/logical-indexing-multiple-conditions/>`_
for atom selection. Logical indexing is a vector or array whose
elements consists of logical varibles, i.e., true(1) or false(0). It
is useful for selecting subset of vector/matrix that matches a given
condition since many MATLAB functions return logical indexing.

For example, the follwing example returns a logical indexing whose
elements are greater than 1:
::

  >> x = [1 2 3];
  >> logical_index = x > 1
  
  logical_index =
  
       0     1     1
  
  >> whos logical_index
    Name               Size            Bytes  Class      Attributes
  
    logical_index      1x3                 3  logical

  >> x(logical_index)
  
  ans =
  
       2     3

Another advantage of logical indexing is that it is very easy to
combine the results of different conditions to select subset on
multiple criteria. The follwoing example select the subset whose
elements are greater than 1, and also smaller than 3:
::
  
  >> logical_index2 = x < 3
  
  logical_index2 =
  
       1     1     0
  
  >> logical_index3 = logical_index & logical_index2
  
  logical_index3 =
  
       0     1     0
  
  >> x(logical_index3)
  
  ans =
  
       2

MDtoolbox has three types of atom selectiting function,
``selectname()``, ``selectid()``, and ``selctrange()``. All of them
returns logical indexing for use of other MDtoolbox functions, such as
file I/O, and geometry calculations.

:: ``selectname()`` matches the given input characters

Figures
----------------------------------

Text here

