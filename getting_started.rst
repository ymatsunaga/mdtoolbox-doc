.. getting_started
.. highlight:: matlab

Getting Started
==================================

Data structures for coordinate and trajectory
---------------------------------------------

MDToolbox assumes a simple vector/matrix form for coordinate/trajectory.

Coordinate variable is a row vector whose elements are the XYZ coordinates of atoms in order
::
  
  [x(1) y(1) z(1) x(2) y(2) z(2) .. x(natom) y(natom) z(natom)]

Thus, for example, translation in the x-axis by 3.0 Angstrom is coded as follows:
::
  
  crd(1:3:end) = crd(1:3:end) + 3.0;

Likewise, the y-coordinate of geometrical center is calculated by
::
  
  center_y = mean(crd(2:3:end));

Trajectory variable is a matrix whose column vectors consist of
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

Typical usages for I/O functions are summarized hre.

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
for atom selection. Logical indexing is a vector or matrix whose
elements consists of logical variables, i.e., true(1) or false(0). It
is useful for selecting subset of vector/matrix that matches a given
condition since many MATLAB functions return logical indexing.

For example, the following example returns a logical indexing whose
elements are greater than 1:
::

  >> x = [1 2 3];
  >> index = x > 1
  
  index =
  
       0     1     1
  
  >> whos index
    Name               Size            Bytes  Class      Attributes
  
    index      1x3                 3  logical

  >> x(index)
  
  ans =
  
       2     3

Another advantage of logical indexing is that it is easy to
combine the results of different conditions to select subset on
multiple criteria. The following example select the subset whose
elements are greater than 1, and also smaller than 3:
::
  
  >> index2 = x < 3
  
  index2 =
  
       1     1     0
  
  >> index3 = index & index2  % Boolean AND
  
  index3 =
  
       0     1     0
  
  >> x(index3)
  
  ans =
  
       2

MDToolbox has three types of atom-selecting functions,
``selectname()``, ``selectid()``, and ``selctrange()``. All of them
returns logical indexing for use with other MDToolbox functions, such as
file I/O, and geometry calculations.

``selectname()`` returns a logical indexing which matches given
names (characters). The following code returns logical indexing of 
alpha-carbon atoms,
::
  
  [pdb, crd] = readpdb('example/anm_lys/lys.pdb'); %pdb is a structure variable containing PDB records
  index_ca = selectname(pdb.name, 'CA');

``selectid()`` returns a logical indexing which matches given
IDs (integers). The following code returns logical indexing of 
atoms of the 1st and 2nd residue IDs.
::
  
  index_resid = selectid(pdb.resseq, 1:2);

``selectrange()`` returns a logical indexing of atoms within cutoff
distance of given reference coordinates.
The following code returns logical indexing of 
atoms within 8.0 Angstrom distance of the 1st and 2nd residue.
::
  
  index_range = selectrange(crd, index_resid, 8.0);

As noted above, logical indexing can be combined to select subset on
multiple conditions. For example, alpha-carbons of the 1st and 2nd
residues are selected by
::
  
  index = index_ca & index_resid;  % Boolean AND

Obtained logical indexings can be used with other MDToolbox
function, such as I/O funcitons. The following reads the trajectory of 
subset atoms specified by the logical index ``index``:
::

  trj = readdcd('run.dcd', index);

As an alternative, users can directly choose subset from coordinate or
trajectory variable. This can be done by using a utility function of
MDToolbox ``to3()``. ``to3()`` converts given logical indexing to
XYZ-type logical indexing. For example, the following code extracts
the subset trajectory as same as above.
::

  trj_all = readdcd('run.dcd');
  trj = trj_all(:, to3(index));

The following explains how ``to3()`` works by using simple indexing:
::
  
  >> index = [true false true]
  
  index =
  
       1     0     1
  
  >> to3(index)
  
  ans =
  
       1     1     1     0     0     0     1     1     1

Figures
----------------------------------

Text here

