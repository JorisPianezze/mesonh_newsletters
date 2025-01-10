Newsletter  #04
================================================

**10 January 2025** English version, version française `ici <newsletter_04.html>`_.


Dear Meso-NH users,

First of all, we'd like to wish you a very happy 2025! Full of discoveries thanks to your Meso-NH simulations :)

Here is our 4th community newsletter. You'll find an interview with a Meso-NH developer, the latest news from the support team and a list of the latest publications using Meso-NH.

Interview with `Robert Schoetter <mailto:robert.schoetter@meteo.fr>`_ (CNRM)
************************************************************************************

.. image:: photo_rs.jpg
  :width: 200

Robert, you developed the multi-level coupling between Meso-NH and TEB, the SURFEX module that represents interactions between cities and the atmosphere. Could you summarize what motivated this development?
  The multi-level coupling was developed to represent the fact that buildings are immersed in the atmosphere. This contrasts with the classical coupling between Meso-NH and TEB, where the roofs of the buildings are at Meso-NH ground level. With the classical coupling, the buildings are below ground level, so an artificial volume of air is introduced and there is no advection of this volume of air between different grid points. This is particularly problematic for heterogeneous cities with high-rise buildings, as horizontal exchanges can only take place via the air above the buildings.

How does multi-level coupling between Meso-NH and SURFEX-TEB work?
  With multi-level coupling, meteorological variables from all Meso-NH levels intersecting buildings are transmitted to SURFEX-TEB. In TEB, they are used as roof, wall and ground meteorological forcing. In Meso-NH, the effect of buildings on the flow is considered using a drag coefficient approach on all levels intersecting buildings. Sensible and latent heat fluxes from the ground, walls and roof to the atmosphere are converted into potential temperature and water vapor mixing ratio trends over all vertical levels intersecting these urban facets. For more details on how multi-level coupling works, as well as an illustration of the improvements induced, please consult the following article: |schoetter_etal|

Why do you recommend this development? 
  Multilevel coupling was originally developed for cities with high-rise buildings. But it can be useful for any city, as it enables Meso-NH's vertical resolution to be increased and, in particular, meteorological variables such as temperature and humidity to be simulated prognostically at 2 m, while taking into account urban buildings. All that's needed to get this is to set vertical resolution to 4 m close to the surface.

What are the possible limitations? In which cases should this coupling be avoided?
  Limitations of this approach have been highlighted for cities surrounded by forests, as certain parameterizations in the ISBA vegetation model do not give good results with such fine vertical resolution. There are also theoretical limits to the drag coefficient approach for very dense cities (e.g. more than 50% of the mesh surface occupied by buildings), which could lead to biases. We also neglect the fact that part of the Meso-NH mesh volume is not air but buildings, which could also lead to inaccuracies for dense cities.

.. |schoetter_etal| raw:: html

   <a href="https://gmd.copernicus.org/articles/13/5609/2020/" target="_blank">Schoetter et al. (2020)</a>

.. note::

   If you too would like to explain a development you've implemented in Meso-NH, or an analysis method you share with the community, please don't hesitate to let me know by `mail <mailto:thibaut.dauhut@univ-tlse3.fr>`_.



News from the support team
************************************

“Community Code” label granted to Meso-NH by CNRS-INSU
  The application to renew the label for the period 2025-2029 has been examined by the Commission spécialisée Océan-Atmosphère (CSOA). The very favorable opinion underlines, among other things, the scientific production of the community, the coupling capabilities of the model and the efforts to reduce the environmental footprint of simulations (porting on GPU, simple precision, output compression, realization of community simulations).

Version 6
  A call for contributions was sent out on January 6. All contributions ready by the end of March, i.e. tested and delivered with a (new) test case, will be considered for integration.

Current and recent developments
  - Chemistry/aerosols: the ACCALMIE project is pursuing the restructuring of chemistry and aerosols in Météo-France models (ARPEGE, MOCAGE, AROME, Méso-NH) in order to externalize chemistry and aerosols. The ACLIB (Aerosols and Chemistry LIBrary) is currently being set up. Numerous routines will be impacted, notably within the ch_monitorn.f90, ch_* and *aer* files. This development will be included in version 6.
  - GPU porting: the PHYEX-OpenACC version is now fully merged (branch: MNH-56X-dev-OPENACC-FFT) and running on some test cases. Performance tests on GPU architectures (AMD and Nvidia) have been carried out and are also validated on CPUs. The following MNH-57X-dev-OPENACC-FFT branch continues to be corrected following the merge with GPU developments.
  - Data compression: the ability to set compression parameters (lossy or lossless) on a variable-by-variable basis in output files, whether for the whole domain or in each box (sub-domain), has been added, for the moment only to the MNH-57X-dev-IO branch. This involved an internal reorganization, as the choice of compression was made at file level only, and not at field level.
  - Frequent output: further functionalities under study, such as filtering by threshold.
  - Preparation of version 6 is now underway (merging contribution by contribution).
  - Website and documentation 2.0: development underway, with a first version planned for this year's Meso-NH Users days.

Meso-NH repository on software forge 
  - The koda.cnrs git repository host is now officially in use: https://src.koda.cnrs.fr/mesonh/mesonh-code
  - Documentation can also be updated by anyone via pull-request: https://src.koda.cnrs.fr/mesonh/mesonh-doc

Meso-NH course
  - The next workshop will take place from March 10 to 13, 2025. Schedule `here <http://mesonh.aero.obs-mip.fr/mesonh57/MesonhTutorial>`_
  - Registration deadline: February 14
  - Registration by mail to `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_

... note::
  If you have any needs, ideas, improvements to make, bugs to fix or suggestions concerning inputs/outputs, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ is keen to hear from you.


Latest publications using Meso-NH
****************************************************************************************

Marine atmospheric boundary layer
  - Adjustment of the marine atmospheric boundary-layer to the North Brazil Current during the EUREC4A-OA experiment [`Giordani et al., 2024 <https://doi.org/10.1016/j.dynatmoce.2024.101500>`_]

Drone measurements of cumulus
  - Experimental UAV flights to collect data within cumulus clouds [`Hattenberger et al., 2024 <https://doi.org/10.1109/TFR.2024.3478216>`_]

PhD theses
  - Amélioration de la prise en compte du givrage par la modélisation et la prévision météorologique pour l'exploitation des parcs éoliens [`Dupont, Université de Toulouse, 2024 <https://theses.fr/s305624>`_]
  - Etude de l'évolution de la couche limite atmosphérique et des nuages de pente sur l'île de la Réunion [`El Gdachi, Université de La Réunion, 2024 <https://theses.fr/s311244>`_]
  - Interactions entre irrigation, couche limite atmosphérique et vents de méso-échelle en région semi-aride : observations et modélisation [`Lunel, Université de Toulouse, 2024 <https://theses.fr/s304370>`_]

.. note::

   If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other communication about your work (including posters and presentations *available online*), please write to me. I'm also keen to hear your views on the proposed format for these newsletters.

Enjoy simulating with Meso-NH!

See you soon,

Thibaut Dauhut and the Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
