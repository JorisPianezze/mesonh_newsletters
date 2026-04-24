Newsletter #09
================================================

**24 April 2026.** English version, version française `ici <newsletter_09.html>`_.


Dear Meso-NH users,

Here is the 9th newsletter from our community. You will find an interview with on member of Meso-NH support team, the latest news from the team, and a list of the latest publications and PhD theses using Meso-NH.

Interview with `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_ (CNRM)
*************************************************************************************************************************************

|pic1|

.. |pic1| image:: photo_qr.jpg
  :width: 250

Quentin, after a tremendous amount of work, you’ve just released version 6 of Meso-NH. Can you explain to us what makes it revolutionary?
  Version 6.0.0 of Meso-NH is the first official release to include the GPU porting of the code. For over 10 years, the GPU porting of Meso-NH has been developed by LAERO on branches parallel to the main code branch in order not to impact standard development. With the GPU porting of the Meso-NH physics engine used in AROME, which required a significant rewrite of the atmospheric physics in the externalized PHYEX module, version 6.0.0 officially completed the GPU porting of Meso-NH and consolidated the shared code between the Meso-NH and AROME physics engines. Numerous new developments have been integrated, the result of several years of research and development by our community. I refer you to the `release note <https://mesonh.readthedocs.io/en/latest/getting_started/releases/release_note_600.html>`_, which are very detailed, and I encourage everyone to have a look at the sections that interest them (there’s bound to be one!).

What other changes will improve our daily experience as users?
  That depends on specific use cases. For Meso-NH GPU users, particularly at LAERO, it is no longer necessary to use a parallel branch of the master branch; instead, you can use the official version directly, which includes all other developments available to all Meso-NH users. For users of the ecRad radiation model, there is no longer a need to set a specific environment variable before compiling the code (which was a common oversight and a source of frustration). ecRad is now compiled by default. Finally, we should also mention the default activation of netCDF compression using the Zstandard library, which not only reduces the size of output files by a factor of 2 to 3 but also slightly reduces computation time. The LFI format is no longer supported for writing.

And for developers, what does this change?
  A lot. In the parts of the code ported to the GPU (advection, pressure solver, turbulence, ICE3 microphysics), numerous OpenACC directives have been introduced. Any code changes in these sections must adhere as closely as possible to these directives in order not to break the optimisations that have been implemented.  In the code sections related to aerosols and chemistry, the ACLIB library introduces new objects and coding standards so that the code specific to Meso-NH can be used in other models and vice versa. A major restructuring of the repository has also been carried out: for example, the src/LIB/SURCOUCHE folder has been moved to MNH/io and MNH/parallel; new subfolders in src/MNH have been created to better organise the numerous source files. It takes a bit of getting used to, but personally I think the code and the repository are much better organised.

Why do you recommend that everyone upgrade to version 6 as soon as possible?
  Because we’ve put our heart and soul into it! The following point applies to every major release: if you’re in the final stages of your thesis or finalising simulations for a scientific paper, I wouldn’t advise you to switch versions. For everyone else, go for it! Version 6 also introduces a host of new scientific features (see the release note). If you are developing in Meso-NH, it is essential that you upgrade to this new version as soon as possible, as the code has changed significantly since version 5.7.2. Merging your developments with the new versions at a later stage will be a difficult process.
  
What are the short- and medium-term prospects at this stage?
  The answer to this question is less personal and reflects the views of part of the support team. The short- and medium-term prospects are to continue single-precision testing across all test cases while working on optimizing certain parts of the code. Implementing continuous integration and continuous deployment (CI/CD) on the GitLab repository will help speed up integrations and detect bugs as close as possible to the change in question. For the GPU port, work will be conducted on shallow convection and LIMA microphysics, as well as the possibility of using grid nesting. Discussions are also underway regarding the strategy to adopt in light of AMD’s discontinuation of OpenACC support. Still on the topic of computational cost, the use of a fine vertical grid very often impacts the code’s numerical stability and requires a small time step. To address this issue, work is currently underway to make the coupling between the surface and the atmosphere implicit. On the input/output (I/O) side, plans are in place to implement, in the short term, the ability to have multiple time series of variables written to the "OUT" output files (via NAM_OUTPUT). In the medium term, the goal is to enable multiple tasks to write to files in parallel, rather than just a single task as is currently the case. In the short term, we will continue to update the content of the new websites based on your feedback. All of these plans will be discussed by the future Meso-NH Scientific Advisory Board / Development Committee.

.. note::

 If you would also like to explain a development you have implemented in Meso-NH, or an analysis method you would like to share with the community, please do not hesitate to let me know by `emailing <mailto:thibaut.dauhut@utoulouse.fr>`_.

News from the support team
************************************

Version 6
  - Version 6.0.0 is currently being prepared and will be released shortly.
  - The PHYEX update in Meso-NH is currently underway. This requires the phasing of changes from the Meso-NH version of PHYEX to the central PHYEX repository. Then, retrieving this version from the central repository and implementing it in Meso-NH allows us to retrieve the PHYEX changes made by other contributors (such as members of the ACCORD community or those developing in AROME).
  - Writing files in LFI format is no longer supported.
  - In netCDF files, SURFEX variables are now separated into a specific group.
  - The call to RTTOV14 has been integrated for the diagnostic step and for online calculation, the call to RTTOV13 has been removed.
  - The latest optimisations of GPU porting on different machines (AMD MI250X and MI300A GPU on Adastra) and NVIDIA APUs have been integrated into the ``MNH-60-branch`` branch.
  - The latest version of the wind turbine code (EOL-v2) and an update to the LIMA-ecRad-aerosols coupling have been integrated into the ``MNH-60-branch`` branch.
  - Some updates to the documentation have been made and the future Meso-NH technical website will be launched when version 6 is released.

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

  If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other information about your work (including posters and presentations *available online*), please do not hesitate to write to `me <mailto:thibaut.dauhut@utoulouse.fr>`_. I also always appreciate your feedback on the newsletters.

Happy simulating with Meso-NH!

See you soon,

Thibaut Dauhut and the entire Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
