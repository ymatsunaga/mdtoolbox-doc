.. wham
.. highlight:: matlab

=======================================================================================
Multistate Bennett Acceptance Ratio (MBAR) (``example/umbrella_alat/mbar/analyze.m``)
=======================================================================================


::
  
  % this routine calculates the Potential of Mean Force (PMF) from
  % umbrella sampling data by using MBAR
  
  %% constants
  C = getconstants();
  KBT = C.KB*300; % KB is the Boltzmann constant in kcal/(mol K)
  BETA = 1./KBT;
  
  %% define umbrella window centers
  umbrella_center = 0:3:180;
  numbrella = numel(umbrella_center);
  
  %% define function handles of bias energy for umbrella windows
  for i = 1:numbrella
    k = 200 * (pi/180)^2; % conversion of the unit from kcal/mol/rad^2 to kcal/mol/deg^2
    fhandle_k{i} = @(x) BETA*k*(periodic(x, umbrella_center(i))).^2;
  end
  
  %% read dihedral angle data
  for i = 1:numbrella
    filename = sprintf('../3_prod/run_%d.dat', umbrella_center(i));
    x = load(filename);
    dihedral_k{i} = x(:, 2);
  end
  
  %% evaluate u_kl: reduced potential energy of umbrella simulation k evaluated at umbrella l
  for k = 1:numbrella
    for l = 1:numbrella
      u_kl{k, l} = fhandle_k{l}(dihedral_k{k});
    end
  end
  
  %% MBAR
  % calculate free energies of umbrella systems
  f_k = mbar(u_kl);
  
  %% PMF
  edge = linspace(-1, 181, 81);
  for i = 1:numbrella
    [bin_k{i}, center] = assign1dbins(dihedral_k{i}, edge);
  end
  pmf = mbarpmf(u_kl, bin_k, f_k);
  pmf = KBT*pmf;
  pmf = pmf - pmf(1);
  
  %% plot the PMF
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


