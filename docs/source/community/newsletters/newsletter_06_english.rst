Newsletter #06
================================================

**9 July 2025.** English version, version française `ici <newsletter_06.html>`_.


Dear Meso-NH users,

This is the 6th newsletter from our Meso-NH community. You'll find an interview with a Meso-NH developer, the latest news from the support team and a list of the latest publications using Meso-NH.

Interview with `Jean-Pierre Chaboureau <mailto:jean-pierre.chaboureau@utoulouse.fr>`_ (LAERO)
*************************************************************************************

.. image:: photo_jpc.jpg
  :width: 200


Jean-Pierre, you've implemented RTTOV in Meso-NH. Could you summarize what this code does?
  RTTOV (Radiative Transfer for TOVS) is a very fast radiative transfer code for radiometers and other passive interferometers in the visible, infrared and microwave ranges, as well as for radars, on board operational and research satellites. It is being developed by the European consortium `NWP SAF <https://www.nwpsaf.eu/site/software/rttov/>`_ (Satellite Application Facility for Numerical Weather Prediction). It is a Fortran 90 code for simulating radiances and reflectivities obtained from satellites, designed to be incorporated into user applications. In the Meso-NH version 5 world, RTTOV can be called by DIAG using the subroutine ``call_rttov13.f90``. (In Meso-NH version 6, RTTOV version 14 will be called by the subroutine ``call_rttov14.f90``).

Why do you recommend using RTTOV?
  Emulating a synthetic Meso-NH satellite image with RTTOV allows you to validate a simulation output with the satellite image. This approach, known as model-to-satellite, makes a direct comparison with satellite variables, without the need for additional assumptions (unlike satellite products derived from inversion of the radiative transfer equation). All errors therefore come from the model world, i.e. from the Meso-NH simulation interpreted by RTTOV. This approach is a particularly relevant tool for assessing cloud representation in a kilometer-scale Meso-NH simulation. At this scale, the pixel size of the satellite observation is commensurate with that of the model mesh.

What recommendations for use would you make to the Meso-NH community?
  Geostationary satellite observation allows us to evaluate a simulation with images every 10 to 15 minutes. This is very useful for determining whether a cloud system is well positioned. This evaluation can be carried out in the thermal infrared domain for tracking deep clouds, in the solar domain for tracking shallow clouds (since RTTOV version 12), and in the microwave radar domain for the vertical structure of clouds and precipitation (since RTTOV version 13). Examples are figure 3 of `Lfarh et al. (2023) <https://doi.org/10.1175/MWR-D-23-0099.1>`_ for the position of a Mediterranean storm at 200 m resolution in the visible range, figure 2 of `Feger et al. (2025) <https://doi.org/10. 5194/egusphere-2025-105>`_ for tracking the position of a mesoscale convective system (MCS) in the infrared domain, and figure 10 from `Feger et al. (2025) <https://doi.org/10.5194/egusphere-2025-105>`_ for the intrusion of dry air into the MCS as seen in a radar cross-section. RTTOV combined with Meso-NH simulations is also being used to help define future space-based sensors, such as the `C2OMODO <https://c2omodo.ipsl.fr/>`_ (Convective Core Observations through MicrOwave Derivatives in the trOpics) mission to be launched in 2030.

What are the limits?
  RTTOV is a very fast code for which gas absorption and gas/particle diffusion effects are parameterized. Experienced users may prefer more precise but slower search codes, for example to take into account three-dimensional or multiple scattering effects. A well-known effect is parallax, valid for example for deep convective clouds observed at fine resolution with a wide angle of view. Another pitfall to avoid is the use of RTTOV to adjust the optical or microphysical properties of aerosols and clouds. Care must be taken to ensure that these properties are consistent between the different microphysical and radiative schemes used. The same applies to the evaluation of continental surface properties.




.. note::

   If you too would like to explain a development you've implemented in Meso-NH, or an analysis method you share with the community, please don't hesitate to let me know by `mail <mailto:thibaut.dauhut@utoulouse.fr>`_.



News from the support team
************************************

CNRS Crystal Medal for Juan!
  The CNRS has awarded a `Cristal Medal to Juan Escobar <https://www.insu.cnrs.fr/fr/cnrsinfo/juan-escobar-munoz-une-medaille-de-cristal-pour-son-expertise-sur-le-calcul-intensif-pour>`_ for his expertise in supercomputing for atmospheric sciences. Congratulations Juan! and many thanks for all your work for the Meso-NH code and its community!

Version 5.7.2
  - Version 5.7.2 has been released, containing numerous commits and some important corrections.
  - All the details are presented `here <https://mesonh-beta-test-guide.readthedocs.io/en/latest/getting_started/releases/release_note_572.html>`_ in the note associated with its release.

Version 6
  - Preparations for version 6 are still underway, and the MNH-60-branch has been declared. 
  - A call for contributions will be issued in September.
  - Internal housekeeping (already announced) continues.
  - The Zstandard compression library has been fully integrated into version 6. In addition, it becomes the default compression library and, more importantly, compression will be enabled by default (disengageable via namelists).
  - The ACLIB library (externalized chemistry and aerosols) continues to be developed for integration into version 6 (a first version of ACLIB will be integrated into MNH-60-branch in July).

.. note::
  **Think of the Zenodo repository for your future publications**, the tar ball is deposited there and a DOI is associated with it. You can refer to `this repository <https://zenodo.org/records/15095131>`_ in the *Data availability* section or equivalent. 

Méso-NH User Days
 This year's event will be held from 13 to 15 October 2025 at CNRM, Météo-France.

New websites
 Work continues on setting up and editing the new websites.

Meso-NH course
  - The next Meso-NH course is scheduled for 1-4 December 2025. 
  - The course will be held face-to-face and in French. Places are limited.
  - Send an e-mail to `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_ for information and registration.


.. note::
  If you have any needs, ideas, improvements to make, bugs to fix or suggestions concerning inputs/outputs, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ is keen to hear from you.


Latest publications using Meso-NH
****************************************************************************************

Aerosol-cloud interactions
  RCEMIP-ACI: Aerosol-Cloud Interactions in a Multimodel Ensemble of Radiative-Convective Equilibrium Simulations [`Dagan et al., 2025 <https://doi.org/10.22541/essoar.174534436.64971999/v1>`_]

C2OMODO satellite mission
  - Vertical velocity deduced from ice change in deep convection [`Auguste and Chaboureau, 2025 <https://doi.org/10.1175/JAS-D-24-0118.1>`_]
  - Bridging time-delayed microwave radiometric observations and deep convection characteristics: a machine learning approach for the C2OMODO mission [`Lefebvre et al., 2025 <https://doi.org/10.1109/TGRS.2025.3583571>`_]

Object tracking
  A Unified Open-Source Toolkit for Atmospheric Object Tracking and Analysis [`Hahn et al., 2025 <https://doi.org/10.5194/egusphere-2025-1328>`_]

Shallow convection
  An update of shallow cloud parameterization in the AROME NWP model [`Marcel et al., 2025 <https://doi.org/10.5194/egusphere-2025-2504>`_]


PhD thesis
  Investigation des effets aérodynamiques de la canopée forestière sur le comportement de feux expérimentaux [`Antolin, Université de Toulouse, 2025 <https://theses.fr/s305185>`_]

Presentations at Ateliers de Modélisation de l'Atmosphère 2025
 Many Meso-NH users presented their work at AMA 2025. Their presentations (pdf and recordings) are available `on line here <http://www.meteo.fr/cic/meetings/2025/AMA/presentations.html>`_.

.. note::

   If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other communication about your work (including posters and presentations *available online*), please write to `me <thibaut.dauhut@utoulouse.fr>`_.

Happy simulations with Meso-NH!

See you soon,

Thibaut Dauhut and the entire Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar and Jean-Pierre Chaboureau
