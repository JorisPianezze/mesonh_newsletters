Newsletter #07
================================================

**10 October 2025.** English version, version française `ici <newsletter_07.html>`_.


Dear Meso-NH users,

This is the 7th newsletter newsletter from our community. It includes an interview with the developer of a useful tool for Meso-NH users, the latest news from the support team, and a list of the latest publications using Meso-NH.

Interview with `Clotilde Augros <mailto:clotilde.augros@meteo.fr>`_ (CNRM)
************************************************************************************

.. image:: photo_ca.jpg
  :width: 250

Clotilde, you have developed a radar simulator that can run on Meso-NH outputs. Could you summarize what this tool does?
  This code, called **operadar** (Observation oPErator for polarimetric RADAR, `Augros et al., 2016 <https://doi.org/10.1002/qj.2572>`_, `David et al. 2025 <https://doi.org/10.5194/amt -18-3715-2025>`_) takes the model variables as input (mixing ratios and possibly number concentrations of hydrometeors, temperature, altitude, pressure) and calculates the radar variables (reflectivity :math:`Z_H`, differential reflectivity :math:`Z_{DR}`, specific differential phase :math:`K_{DP}`, correlation coefficient :math:`\rho _{HV}`) at each grid point of the model from backscatter coefficient tables. The scattering method used is the T-matrix method (`Waterman, 1965 <https://doi.org/10.1109/PROC.1965.4058>`_), which represents hydrometeors as flattened spheroids, but it is also possible to calculate the radar variables using the Rayleigh approximation. 

  The code consists of two main steps: the first generates the backscattering coefficient tables for each frequency band, each type of hydrometeor, and each microphysical scheme. It is based in part on the Fortran77 code by `Mishchenko and Travis (1994) <http://www.sciencedirect.com/science/article/pii/0030401894907315>`_. A second stage, coded in Python, allows the simulated fields and coefficient tables to be read and the radar variables to be calculated. 

  The entire **operadar** code is designed to run *offline*. It is independent of the Meso-NH code and can also be used with AROME outputs, for example. Improvements made when applying it to one of these two models benefit everyone thanks to the collaborative maintenance and development of the code on the `operadar git repository <https://github.com/UMR-CNRM/operadar>`_.

Why is it better to use this tool than the other radar simulators included in Meso-NH diagnostics?
  There are actually three *online* versions of the radar simulator in Meso-NH implemented in the DIAG section [#oponline]_ : one that only uses the Rayleigh approximation for calculating scattering, another that takes into account the flattening of hydrometeors but has not been maintained since 2018, and a third that is adapted to the W band only for airborne or ground-based radars, which does not take into account the flattening of hydrometeors. More advanced options are now available with **operadar**: consideration of hydrometeor oscillation in addition to their flattening, different choices of dielectric constant formulation, and a more advanced version of the representation of melting and mixed phase. These improvements have, for example, made it possible to successfully simulate the columns of :math:`Z_{DR}` associated with the presence of large supercooled water droplets within the updrafts of thunderstorms (`Kumjian et al., 2014 <https://doi.org/10.1175/JAMC-D-13-0354.1>`_) with AROME and the LIMA scheme (`David et al., 2025 <https://doi.org/10.5194/amt-18-3715-2025>`_).

  In 2025, as part of Cloé David's thesis, new options were also added to ensure consistency with microphysical options recently made available in AROME and Meso-NH, such as the new size distribution function for snow (PSD from `Wurtz et al., 2023 <https://doi.org/ 10.1002/qj.4437>`_). The code also allows the number of moments (1 or 2) to be specified independently for each species in order to adapt to different LIMA configurations (2 moments for liquid water, 2 moments for liquid water and ice crystals, or 2 moments entirely).

  Comparisons between simulations and observations show a very good representation of the variables :math:`Z_H`, :math:`Z_{DR}`, and :math:`K_{DP}` in rain with the LIMA microphysical scheme (`David et al., 2025 <https://doi.org/10.5194/10.5194/amt -18-3715-2025>`_) on more than 34 storm cases simulated with AROME, better than with the ICE3 microphysical scheme, which underestimates maximum reflectivities (Figure 1).

.. figure:: figure_reflectivity.png
  :width: 500

  Excerpt from `David et al. (2025) <https://doi.org/10.5194/amt -18-3715-2025>`_ (Figure 5): Relative frequency of maximum radar reflectivity values (maximum on the vertical) within the convective cores of thunderstorms, defined by a reflectivity threshold > 40 dBZ, for 34 cases of severe thunderstorms observed by radar over mainland France. Gray curve: all observations not associated with hail (as detected by radar), black curve: all observations. Orange curve: AROME simulations with ICE3, green curve: AROME simulations with LIMA.

In what cases do you recommend using this module?
  I recommend using **operadar** whenever you are interested in cases of heavy rain for C and lower frequency bands, as well as for any type of precipitation with all frequency bands below C (W, K, Ka, Ku), when you want to take into account the flattening of hydrometeors.

What recommendations would you make to users? 
  The code is constantly evolving, particularly in the context of Cloé David's thesis. Improvement work will continue in 2025  with a particular focus on frozen species (revisiting the choices of axis ratio, oscillation, density-diameter laws, PSD). It is best to contact `me <mailto:clotilde.augros@meteo.fr>`_ for any usage requests, so that we can determine together the most relevant options available at the time of the study.

What are the limitations? In what cases should this option be avoided?
  For the moment, there are two main limitations. On the one hand, the simulation of radar geometry is not yet integrated into this code but will be soon. On the other hand, for the K, Ka, Ku, and W frequency bands, the relevance of simulations using the T-matrix method remains to be confirmed for snow.  Other more complex methods (Discrete Dipole Approximation DDA, Self Similar Rayleigh Gans Approximation SSRGA) are used in the literature. As such, a comparison with the RTTOV-SCAT radar simulator, which uses tables produced with the DDA method, is planned for 2026.

.. [#oponline] There are also three online versions of the radar simulator in Meso-NH, implemented in the DIAG section:
   **(1)** the first version of the Meso-NH radar simulator (NVERSION_RAD=1, `Richard et al., 2003 <https://doi.org/ 10.1256/qj.02.50>`_) allows radar variables to be calculated in the model geometry (3D grid), applying the Rayleigh approximation for the calculation of scattering, which remains valid as long as the size of the hydrometeors is very small compared to the wavelength :math:`\lambda`. For S-band radars (:math:`\lambda` ~ 10 cm), this assumption is valid for all hydrometeors except hail. For C-band radars (:math:`\lambda` ~ 5 cm), this assumption no longer holds when simulating intense rainfall with large raindrops (~ 8 mm).
   **(2)** A second version (NVERSION_RAD=2, Caumont et al., 2006, `Augros et al., 2016 <https://doi.org/10.1002/qj.2572>`_) has been implemented in Méso-NH in Fortran to include different scattering methods, including T-matrix scattering (`Waterman, 1965 <https://doi.org/10.1109/PROC. 1965.4058>`_) which allows diffusion to be simulated for flattened hydrometeors, including when outside the Rayleigh regime (i.e., for intense rain from the C band onwards, or for hail, or for lower frequency bands: K, Ka, Ku, W). However, this second version has not been maintained since 2018.
   **(3)** A third version has been implemented in the aircraft_balloon_evol routine. The frequency band is set to that of the Rasta cloud radar: W band (:math:`\lambda` = 3.15 :math:`10^{-3}` m, frequency = 95.04 GHz). This version uses Mie scattering, so hydrometeors are considered as spheres. It takes into account attenuation by hydrometeors along the beam. The bright band is simulated by adding a liquid fraction to the graupel species, as proposed in Augros et al. (2016).

References
  - Comparisons between S, C, and X band polarimetric radar observations and convective-scale simulations of HyMeX first special observing period [`Augros et al., 2016 <https://doi.org/10.1002/qj.2572>`_]
  - Improved Simulation of Thunderstorm Characteristics and Polarimetric Signatures with LIMA 2-Moment Microphysics in AROME [`David et al., 2025 <https://doi.org/10.5194/amt-18-3715-2025>`_]
  - The Anatomy and Physics of ZDR Columns: Investigating a Polarimetric Radar Signature with a Spectral Bin Microphysical Model [`Kumjian et al., 2014 <https://doi.org/10.1175/jamc-d-13-0354.1>`_]
  - T-matrix computations of light scattering by large spheroidal particles [`Mishchenko and Travis, 1994 <http://www.sciencedirect.com/science/article/pii/0030401894907315>`_]
  - High-resolution numerical simulations of the convective system observed in the Lago Maggiore area on 17 September 1999 (MAP IOP 2a) [`Richard et al., 2003 <https://doi.org/10.1256/qj.02.50>`_]
  - Matrix formulation of electromagnetic scattering [`Waterman, 1965 <https://doi.org/10.1109/PROC.1965.4058>`_]

.. note::

  If you would also like to explain a development you have implemented in Meso-NH, or an analysis method you would like to share with the community, please let me know by `emailing <mailto:thibaut.dauhut@utoulouse.fr>`_.

    
    
News from the support team
************************************

The next Meso-NH user days are fast approaching! They will take place at the CNRM, in the Joël Noilhan room, from Monday, October 13 to Wednesday, October 15, 2025. You can find the program `here <https://mesonh.cnrs.fr/13th-meso-nh-users-meeting-13-15-oct-2025/>`_.

Version 6
  - Preparation of version 6 is underway, with the aim of distributing it by the end of 2025.
  - A call for contributions for version 6 is open from early September until the end of October.
  - The ACLIB library (externalized chemistry and aerosols) and the new version of ECRAD have been integrated into the MNH-60 branch.
  - Single precision Meso-NH tests are underway on all eligible test cases.
  - Source cleaning and restructuring are continuing in preparation for version 6.0.0, with, for example, the removal of LFI format file writes.

Other developments in progress
  - Progress on the (long-term) overhaul of parallel inputs and outputs in Meso-NH.
  - Preparation of the websites is progressing well.

Meso-NH training course
  - The next Meso-NH training course is scheduled for December 1-4, 2025.
  - The course will be held in person and in French. There are 3 places left.
  - Send an email to `Quentin Rodier <quentin.rodier@meteo.fr>`_ for information and registration.




.. note::
  Si vous avez des besoins, idées, améliorations à apporter, bugs à corriger ou suggestions concernant les entrées/sorties, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ est preneur.


Dernières publications utilisant Méso-NH
****************************************************************************************

Boundary layer and Interactions with the surface
  - Model and Observation for surface–atmosphere interactions over heterogeneous landscape: MOSAI project [`Lohou et al. (2025) <https://doi.org/10.1016/j.jemets.2025.100019>`_]
  - Energetically Consistent Eddy-Diffusivity Mass-Flux Convective Schemes: 2. Implementation and Evaluation in an Oceanic Context [`Perrot and Lemarié (2025) <http://dx.doi.org/10.1029/2024MS004616>`_]

Fire Meteorology
  - A simplified model to incorporate firebrand transport into coupled fire atmosphere models [`Alonso-Pinar et al. (2025) <https://doi.org/10.1071/WF24200>`_]
  - Synoptic and Regional Meteorological Drivers of a Wildfire in the Wildland–Urban Interface of Faro (Portugal) [`Couto et al. (2025) <https://doi.org/10.3390/fire8090362>`_]

Microphysics and Precipitations
  - Improving supercooled liquid water representation in LIMA using ICICLE data [July-Wormit et al., *accepted* (2025)]
  - Localized precipitation enhancement induced by orography and wind dynamics in southern Réunion Island during Tropical Cyclone Batsirai [`Ramanamahefa et al. (2025) <https://doi.org/10.2139/ssrn.5529525>`_]
  - Model intercomparison of the impacts of varying cloud droplet nucleating aerosols on the lifecycle and microphysics of isolated deep convection [`Saleeby et al. (2025) <https://doi.org/10.1175/JAS-D-24-0181.1>`_]

Volcanic plume and Chemistry
  - Removal Processes of the Stratospheric SO2 Volcanic Plume From the 2015 Calbuco Eruption [`Baray et al. (2025) <https://doi.org/10.1029/2025JD043850>`_]

.. note::

   If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other information about your work (including posters and presentations *available online*), please feel free to write to me. As we are setting up these newsletters, I would also appreciate your feedback on the proposed format.

Happy simulating with Meso-NH,

see you soon!

Thibaut Dauhut et toute l’équipe Méso-NH : Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau

