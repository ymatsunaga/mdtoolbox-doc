.. alad_2D_umbrella_wham
.. highlight:: matlab

===========================================================================================
2D Umbrella Sampling of Alanine-Dipeptide and WHAM
===========================================================================================

Files for this example can be downloaded from `here <https://briefcase.riken.jp/public/bzXsQA0f2g7AXVIBSRRPbX0AkkO1ymuB7UkKJvj1QO52>`_.
This example is located in ``mdtoolbox_example/umbrella_alad/wham/``.

::
  
  % this routine calculates free energies of umbrella systems
  % by using WHAM
  
  %% define umbrella window centers
  center_phi = -180:15:-15;
  center_psi = 0:15:165;
  
  numbrella = numel(center_phi) * numel(center_psi);
  umbrella_center = zeros(numbrella, 2);
  k = 0;
  for i = 1:numel(center_phi)
    for j = 1:numel(center_psi)
      k = k + 1;
      umbrella_center(k, :) = [center_phi(i) center_psi(j)];
    end
  end
  
  %% define function handles of bias energy for umbrella windows
  for k = 1:numbrella
    K = 50 * (pi/180)^2; % conversion of the unit from kcal/mol/rad^2 to kcal/mol/deg^2
    fhandle_k{k} = @(x) sum(K*(periodic(x, umbrella_center(k, :))).^2, 2);
  end
  
  %% read dihedral angle data
  k = 0;
  for i = 1:numel(center_phi)
    for j = 1:numel(center_psi)
      k = k + 1;
      filename = sprintf('../4_prod/run_%d_%d.dat', umbrella_center(k, 1), umbrella_center(k, 2));
      x = load(filename);
      dihedral_k{k} = x(:, 2:3);
    end
  end
  
  %% WHAM
  % evaluate the potential of mean force (PMF) in dihedral angle space
  [f_k, pmf, center_mx, center_my] = wham2d(dihedral_k, fhandle_k, 300, linspace(-180, 0, 81), linspace(0, 180, 71));
  
  %% visualization
  landscape(center_mx, center_my, pmf, 0:0.25:6); colorbar;
  xlabel('phi [degree]', 'FontSize', 20, 'FontName', 'Helvetica');
  ylabel('psi [degree]', 'FontSize', 20, 'FontName', 'Helvetica');
  exportas('analyze');
  
  %% save results
  save -v7.3 analyze.mat;

.. image:: ./images/pmf_wham2d.png
   :width: 90 %
   :alt: pmf
   :align: center

