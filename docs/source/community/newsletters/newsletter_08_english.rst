Newsletter #08
================================================

**8 January 2025.** English version, version française `ici <newsletter_08.html>`_.


Dear Meso-NH users,

Here is the 8th newsletter from our community. You will find an interview with two Meso-NH developers, the latest news from the support team, and a list of the (many!) latest publications and PhD theses using Meso-NH.

Interview with `Quentin Libois <quentin.libois@meteo.fr>`_ and Sophia Schaefer (CNRM)
*************************************************************************************************************************************

|pic1|     |pic2|

.. |pic1| image:: photo_ql_bw.png
  :width: 250

.. |pic2| image:: photo_ss.jpg
  :width: 230

Quentin, in 2017 you included the option to use the ecRad code for radiation representation in Meso-NH, and Sophia, you helped update the new version of ecRad in the next version of Meso-NH with Quentin Rodier. Could you summarise what this code does?
  ecRad is the radiative transfer code used in IFS (Integrated Forecasting System, the global atmospheric model of the ECMWF, the European Centre for Medium-Range Weather Forecasts). It is primarily a state-of-the-art code that allows the use of recent developments both for solving the radiative transfer equation (taking into account sub-grid cloud heterogeneities, parameterisation of sub-grid 3D effects) and for the optical properties of aerosols and clouds. Originally, it relied on the optical properties of gases as provided by RRTM (Rapid Radiative Transfer Model), but since version 1.6, it offers new properties via the ecckd option, which makes it particularly fast. Its modular design makes it easy to access, both for use and for contributing to its development. The modular design allows certain parts of the code to be varied individually, the impact of these modifications to be tested, and the settings to be optimised for different applications.

Why is it preferable to use ecRad rather than another radiation representation (e.g. ECMWF in Meso-NH)?
  ecRad offers a wide range of options (assumptions regarding cloud sub-grid heterogeneity, vertical cloud cover, optical properties of gases, aerosols and hydrometeors, etc.) that make it easy to explore the sensitivity of radiative transfer to these options or to take into account processes that could not be considered previously (consideration of scattering for thermal infrared radiation, 3D effects, consideration of different types of liquid and ice hydrometeors). Today, it is also faster and more accurate than the historical option called ECMWF in Meso-NH. In general, ecRad can do everything that the historical option offered, but much more, and the code is also easier to use and modify.

In what cases is ecRad recommended?
  There are no cases in which ecRad is not recommended. Depending on the application, different ecRad options may be recommended (e.g. different radiation solvers).

What recommendations would you make to users?
  Radiation remains a relatively unknown component for Meso-NH users. However, a simulation can be very sensitive to the choice of radiation options. We encourage users to estimate the sensitivity of their simulations to key parameters such as the vertical overlap assumption or sub-grid heterogeneity. It should also be noted that the estimation of the effective radius of hydrometeors can have a significant impact on the optical properties of a cloud, and consequently on the energy balance and life cycle of a cloud. For the radiation solver, the default option is currently McICA, but for LES or single-column cases, Tripleclouds is a better option (since it does not introduce stochastic noise).

What are the limitations? In what cases should this option be avoided?
  ecRad remains a two-stream model based on the plane-parallel assumption and therefore assumes independent columns. As a result, its performance remains limited for configurations where the sun is low on the horizon and at high spatial resolution when horizontal radiation transfers between adjacent columns become significant. An option to handle these inter-column transfers could be included in the future. Despite this limitation, ecRad remains the best option available today in Meso-NH.

.. note::

 If you would also like to explain a development you have implemented in Meso-NH, or an analysis method you would like to share with the community, please do not hesitate to let me know by `emailing <mailto:thibaut.dauhut@utoulouse.fr>`_.

News from the support team
************************************

Version 6
  - Version 6.0.0 is currently being prepared and will be released shortly.
  - The PHYEX update in Méso-NH is currently underway. This requires the phasing of changes from the Méso-NH version of PHYEX to the central PHYEX repository. Then, retrieving this version from the central repository and implementing it in Méso-NH allows us to retrieve the PHYEX changes made by other contributors (such as members of the ACCORD community or those developing in AROME).
  - Writing files in LFI format is no longer supported.
  - In netCDF files, SURFEX variables are now separated into a specific group.
  - The call to RTTOV14 has been integrated for the diagnostic step and for online calculation, the call to RTTOV13 has been removed.
  - The latest optimisations of GPU porting on different machines (AMD MI250X and MI300A GPU cards on Adastra) and NVIDIA APUs have been integrated into the ``MNH-60-branch`` branch.
  - The latest version of the wind turbine code (EOL-v2) and an update to the LIMA-ecRad-aerosols coupling have been integrated into the ``MNH-60-branch`` branch.
  - Some updates to the documentation have been made and the future Méso-NH technical website will be launched when version 6 is released.

Current and recent developments
  - Single-precision Meso-NH tests are currently being conducted on all eligible test cases. The switch to single precision by default has not yet been finalised and will likely be postponed until version 6.1.


Meso-NH training course
  - The next Meso-NH training course is scheduled for 10 to 13 March 2026.
  - The course will be held in a hybrid format and in English.
  - Send an email to `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_ for information and registration.

.. note::
  If you have any requirements, ideas, improvements to make, bugs to fix or suggestions regarding inputs/outputs, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ is always interested.

Latest publications using Meso-NH
************************************

Fire meteorology
  - Eulerian modelling of spotting using a coupled fire-atmosphere approach [`Alonso-Pinar et al. <https://doi.org/10.5194/egusphere-2025-4855>`_, *in discuss.*]
  - Assessing wildfire dynamics during a megafire in Portugal using the MesoNH/ForeFire coupled model [`Campos et al., 2025 <https://doi.org/10.1002/qj.70075>`_]
  - ForeFire: A Modular, Scriptable C++ Simulation Engine and Library for Wildland-Fire Spread [`Filippi et al., 2025 <https://doi.org/10.21105/joss.08680>`_]

Global model evaluation and improvement
  - Process-Level Evaluation of the Land-Atmosphere Interactions Within CNRM-CM6-1 Single-Column Model Configuration [`Bernard et al., 2025 <https://doi.org/10.1029/2025MS005090>`_]
  - Prognostic modeling of total specific humidity variance induced by shallow convective clouds in a GCM [`d'Alençon et al. <https://doi.org/10.5194/egusphere-2025-5798>`_, *in discuss.*]
  - Evaluation and improvement of a cold pool parameterization against Large Eddy Simulations [`Thiam et al. <https://doi.org/10.5194/egusphere-2025-5329>`_, *in discuss.*]

Microphysics
  - Evaluation of the vertical microphysical properties of fog as simulated by Meso-NH during the SOFOG3D experiment [`Mazoyer et al. <https://doi.org/10.5194/egusphere-2025-5528>`_, *in discuss.*]

Urban meteorology and Wind energy
  - Numerical study of dust plume impact on urban thermal comfort [`Bernard et al. <https://doi.org/10.5194/egusphere-2025-4829>`_, *in discuss.*]
  - A purely analytical and physical wind turbine wake model accounting for atmospheric stratification [`Noël et al. <https://doi.org/10.48550/arXiv.2510.17236>`_, *submitted*]

PhD theses
  - Modélisation multiéchelle des sautes de feu [`Alonso-Pinar, Université de Corte and University of Melbourne, 2025 <https://theses.fr/2025CORT0012>`_]
  - Le rôle de la couche des alizés sahariens dans la cyclogenèse de l'Atlantique tropical [`Feger, Université de Toulouse, 2025 <https://theses.fr/s362119>`_]
  - Le rôle des ondes d'est africaines dans la formation des cyclones tropicaux dans l'Atlantique [`Jonville, Sorbonne Université, 2025 <https://theses.fr/s383920>`_]
  - Amélioration de la prévision du givrage par eau liquide surfondue [`July-Wormit, Université de Toulouse, 2025 <https://theses.fr/s362083>`_]
  - Restitution de propriétés microphysiques et dynamiques de la convection profonde par satellite : préparation de la mission C²OMODO [`Lefebvre, Université Paris-Saclay, 2025 <https://theses.fr/s347512>`_]
  - Amélioration de la représentation des stratocumulus et cumulus, ainsi que des précipitations associées, dans le modèle AROME [`Marcel, Université de Toulouse, 2025 <https://theses.fr/s362112>`_]
  - Paramétrisation du soulèvement de poussières au Sahel par les poches froides [`Thiam, Sorbonne Université and Université Cheikh Anta Diop de Dakar, 2025 <https://theses.fr/s384690>`_]
  - Modélisation des interactions aérosols - microphysique - électricité atmosphérique dans les systèmes de convection profonde [`Vongpaseut, Université de Toulouse, 2025 <https://theses.fr/s361984>`_]


.. note::

  If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other information about your work (including posters and presentations *available online*), please do not hesitate to write to me. As we are setting up these newsletters, I would also appreciate your feedback on the proposed format.

Happy simulating with Meso-NH!

See you soon,

Thibaut Dauhut and the entire Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
