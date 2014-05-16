.. wham
.. highlight:: matlab

=======================================================================================
Multistate Bennett Acceptance Ratio (MBAR) (``example/umbrella_alat/mbar/analyze.m``)
=======================================================================================


::
    
  % this routine calculates the Potential of Mean Force (PMF) from
  % umbrella sampling data by using MBAR
  
  %% setup constants
  C = getconstants();
  KBT = C.KB*300; % KB is the Boltzmann constant in kcal/(mol K)
  BETA = 1./KBT;
  
  %% define umbrella window centers
  window_center = 0:3:180;
  nwindow = numel(window_center);
  
  %% read potential energy data without restraint energy (eamber)
  for i = 1:nwindow
    filename = sprintf('../3_prod/run_%d.out', window_center(i));
    e = readamberout(filename);
    u_kn{i} = e.eamber*BETA;
  end
  
  %% read dihedral angle data
  for i = 1:nwindow
    filename = sprintf('../3_prod/run_%d.dat', window_center(i));
    x = load(filename);
    dihedral_kn{i} = x(:, 2);
  end
  
  %% define a function handle of bias energy for each umbrella window
  for i = 1:nwindow
    k = 200 * (pi/180)^2; % conversion of the unit from kcal/mol/rad^2 to kcal/mol/deg^2
    fhandle_k{i} = @(x) BETA*k*(periodic(x, window_center(i))).^2;
  end
  
  %% MBAR
  % calculate free energies of umbrella systems
  f_k = mbar(u_kn, fhandle_k, dihedral_kn);
  
  %% PMF
  edge = linspace(-1, 181, 101);
  for i = 1:nwindow
    [bin_kn{i}, center] = assign1dbins(dihedral_kn{i}, edge);
  end
  pmf = mbarpmf(u_kn, fhandle_k, dihedral_kn, bin_kn, f_k);
  pmf = KBT*pmf;
  pmf = pmf - pmf(1);
  
  %% plot PMF
  hold off
  plot(center, pmf, 'k-');
  formatplot
  xlabel('angle [degree]', 'fontsize', 20);
  ylabel('PMF [kcal/mol]', 'fontsize', 20);
  axis([-1 181 -8 12]);
  exportas('analyze')
  hold off
  
  %% save results
  save -v7.3 analyze.mat;


::
  
  function d = periodic(x, center)
  %
  %
  
  d = abs(x - center);
  while d > 180
    d = d - 360;
  end


.. image:: ./images/mbar_pmf.png
   :width: 70 %
   :alt: mbar_pmf
   :align: center


