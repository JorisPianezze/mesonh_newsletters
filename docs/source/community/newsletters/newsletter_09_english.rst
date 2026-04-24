Newsletter #09
================================================

**24 April 2026.** English version, version française `ici <newsletter_09.html>`_.


Dear Meso-NH users,

Here is the 9th newsletter from our community. You will find an interview with one member of Meso-NH support team, the latest news from the team, and a list of the latest publications and PhD theses using Meso-NH.

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
  The team's efforts have been focused on the release of this new version 6 and of the new websites: main website and technical website.

Launch of the Meso-NH Users' Forum
  The first Meso-NH users' forum will take place on the **morning of Wednesday 3 June in the Coriolis room** at OMP (14 avenue Edouard Belin, Toulouse). A reception will be held on site on this occasion! If you plan to come in person, could you please send me `an email <mailto:thibaut.dauhut@utoulouse.fr>`_ so that I can estimate the number of participants as accurately as possible? Thank you!

Meso-NH Training Course
  The Meso-NH training course held from 10 to 13 March 2026, in hybrid format and in English, went very well. For this session we had 23 participants (15 on site and 8 online): trainees, PhD students, postdocs and fixed-term staff (LAERO, CNRM GMME, CERFACS, LMD, University of Reims, Universities of Évora and Lisbon in Portugal, Institut Néel of CNRS in Grenoble), as well as researchers and engineers (University of Warsaw in Poland, University of Reims, CNAM – Geomatics and Land Laboratory in Le Mans, RISE Research Institute at Uppsala University in Sweden).

Other news
  - The technical team of the Meso-NH Service is now led by Philippe Wautelet and Quentin Rodier.
  - A policy on the lifetime of Meso-NH branches and versions will be trialled, in order to provide a degree of stability for users who need to keep the same version of Meso-NH for several years while still having access to improvements. The numbering will remain the same as at present, in the form X-Y-Z, where X is the major version number, Y the minor version number and Z the bugfix number. Each new minor version will be maintained for at least two years from the release of the following one (for example, a version 5.7.3 is currently being prepared). Bugfix releases with increasing Z numbers will contain only corrections and should guarantee identical results for a given minor version (in the same working environment), up to bug fixes. New features will only be incorporated into new minor versions, which should be released somewhat more frequently than at present.
  - Thought is being given within the technical team to the management of code branches, with the aim of making it more rigorous, organised and understandable.

.. note::
  If you have any requirements, ideas, improvements to make, bugs to fix or suggestions regarding inputs/outputs, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ is always interested.

Latest publications using Meso-NH
************************************

Convection and organisation
  - Effect of vertical wind shear on convective clouds: development, organization, and turbulence [`Bidou et al. <https://doi.org/10.5194/egusphere-2026-323>`_, *in discuss.*]
  - Sensitivity of flower trade-wind cloud organisation to mesoscale atmospheric heterogeneities [`Dauhut et al. <https://doi.org/10.1002/qj.70141>`_, 2026]

Cyclones and waterspouts
  - On the tropical nature of an intense Mediterranean cyclone in the ocean-atmosphere system [`Brumer et al. <https://hal.science/hal-05444958v1>`_, *submitted*]
  - Can hot water discharged from industrial processes enhance the likelihood of waterspouts? [`Capecchi et al. <https://doi.org/10.48550/arXiv.2603.24233>`_, *in press* 2026]
  - Orographic impacts of Réunion Island and Madagascar on heavy rainfall during Tropical Cyclone Batsirai (2022) [`Lee et al. <https://doi.org/10.5194/egusphere-2026-1893>`_, *in discuss.*]

Fire meteorology
  - Wildfires in the Southern Amazon: Insights into Pyro-Convective Cloud Development from Two Case Studies in August 2021 [`Bezerra et al. <https://doi.org/10.3390/atmos17020173>`_, 2026]
  - Numerical Investigation of Surface–Atmosphere Interaction and Fire Danger in Northern Portugal: Insights into the Wildfires on July 29, 2025 [`Couto et al. <https://doi.org/10.3390/fire9030111>`_, 2026]

Model development and parametrisations
  - Meso-NH-ISO v1.0: a water stable isotopes scheme in the non-hydrostatic mesoscale atmospheric model Meso-NH. Application to a 2D West African squall line [`Barthe et al. <https://doi.org/10.5194/egusphere-2026-548>`_, *in discuss.*]
  - Atmosphere, ocean, and coupled ocean–atmosphere models in a single code [`Redelsperger <https://doi.org/10.1002/qj.70133>`_, 2026]
  - An update of shallow cloud parameterization in the AROME NWP model [`Marcel et al. <https://doi.org/10.5194/acp-26-3901-2026>`_, 2026]

Surface-atmosphere interactions
  - PANAME-Urban campaign: investigating multiscale surface-atmosphere interactions and thermal-contrasts in the Paris region (France) [Lemonsu et al., *in press* 2026]
  - Impact of surface interactivity on the initiation of deep convection in the Sahel: A cloud-resolving model study [`Tomasini and Couvreux <https://doi.org/10.1016/j.atmosres.2026.108941>`_, 2026]

PhD thesis
  - Microphysique et propriétés optiques des nuages de glace dans le visible et l’infrarouge appliquées au transfert radiatif pour l’observation satellitaire opérationnelle [`Joseph <https://theses.fr/s361932>`_, Université de Toulouse, 2026]

.. note::

  If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other information about your work (including posters and presentations *available online*), please do not hesitate to write to `me <mailto:thibaut.dauhut@utoulouse.fr>`_. I also always appreciate your feedback on the newsletters.

Happy simulations with Meso-NH!

See you soon,

Thibaut Dauhut and the entire Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
