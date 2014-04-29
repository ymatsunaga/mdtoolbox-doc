.. wham
.. highlight:: matlab

===========================================================================
Weighted Histogram Analysis Method (WHAM) (``example/umbrella/wham``)
===========================================================================


::
  
  %% define umbrella window centers
  window_center = 0:3:180;
  nwindow = numel(window_center);
  
  %% read dihedral angle data
  dihedral = {};
  for i = 1:nwindow
    filename = sprintf('../3_prod/run_%d.dat', window_center(i));
    x = load(filename);
    dihedral{i} = x(:, 2);
  end
  
  %% define a function handle of bias energy for each umbrella window
  fhandle = {};
  for i = 1:nwindow
    k = 200 * (pi/180)^2; % conversion of the unit from kcal/mol/rad^2 to kcal/mol/deg^2
    fhandle{i} = @(x) k*(periodic(x, window_center(i))).^2;
  end
  
  s = getconstants;
  kbt = s.KB*300;
  
  %% calculate probability along the dihedral angle and calculate
  %% the potential of mean force (PMF) along the dihedral angle
  [f, log_prob, center] = wham(dihedral, fhandle, 300, linspace(-1, 181, 82));
  pmf = - kbt * log_prob;
  pmf = pmf - pmf(1);
  
  %% plot the PMF
  hold off
  plot(center, pmf, 'k-');
  formatplot
  xlabel('angle [degree]', 'fontsize', 20);
  ylabel('PMF [kcal/mol]', 'fontsize', 20);
  axis([-1 181 -8 12]);
  exportas('script_pmf')
  hold off
  
  %% save results
  save -v7.3 script_pmf.mat;  


::
  
  function d = periodic(x, center)
  %
  %
  
  d = abs(x - center);
  while d > 180
    d = d - 360;
  end


.. image:: ./images/wham_pmf.png
   :width: 70 %
   :alt: wham_pmf
   :align: center

