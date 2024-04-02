Newsletter #01
================================================

Le 4 avril 2024. English version, version française: :doc:`newsletter_01.rst`

Dear Meso-NH users,

This is the 1st newsletter from our Meso-NH community. You'll find an interview with a Meso-NH developer, the latest news from the support team and a list of the latest publications using Meso-NH.

Interview with `Benoît Vié <mailto:benoit.vie@meteo.fr>`_ (CNRM)
*****************************************************************

.. image:: photo_bv.jpg
  :width: 300

Benoît, you made a major contribution to LIMA for the two-moment representation of microphysics in Meso-NH. Could you summarise what this option does?
  Yes, I took over the development of LIMA, following on from Sarah Berthet's thesis work. This is one of the microphysical schemes available in Meso-NH, which is therefore concerned with predicting cloud and precipitation formation. Like ICE3, from which it draws much of its inspiration, cloud composition is broken down into major hydrometeor categories (droplets, rain, small crystals, snow, graupel and hail). But what's new about LIMA is that it predicts the number of hydrometeors of each type, in addition to their total mass. This gives a better estimate of their size and a better representation of all the processes (condensation/evaporation, collisions, etc.) that drive their evolution. LIMA also represents aerosol-cloud interactions, using a realistic prognostic aerosol population to determine the number of hydrometeors. LIMA is thus the Meso-NH microphysical scheme most capable of accurately predicting cloud composition, its evolution and the formation of precipitation.

Why is it better to use LIMA than another representation of microphysics?
  LIMA's contribution generally results in better forecasts or simulations. For example, it has already been shown that LIMA is better at forecasting fog, and produces more realistic convective systems compared with radar observations, notably because it is capable of representing large raindrops and better manages the evaporation of precipitation under the cloud. So, whether you're forecasting or studying processes, don't hesitate to give it a try!

In which cases is this option recommended?
  In fact, whatever cloud system you're interested in, LIMA is likely to be the right choice! Especially as LIMA is now very modular and you can choose the level of complexity you want, depending on the subject you're studying.

What recommendations would you make to users? 
  The best advice is not to hesitate to ask for advice! LIMA can be used very simply, by putting CCLOUD="LIMA" in your namelist, with default settings that should provide a good basis for your work. However, LIMA's true potential will be expressed if its configuration is adapted to the case in question. For example, it is often important to clearly define the aerosol population that LIMA will use to form droplets and crystals in order to obtain the best results, and it is sometimes even necessary to activate the aerosol emission patterns as well. Beyond LIMA, Meso-NH now has a coherent set of parameterisations (for aerosols, microphysics, radiation, electricity...) and it's easy to get lost among all the available options, so, I repeat: don't hesitate to ask us for advice!

What are the limits? When should this option be avoided?
  There's no point in activating LIMA if ... you're simulating a very stable dry boundary layer! On a more serious note, we are working on two of LIMA's remaining major biases: the underestimation of small crystal concentrations, which we are correcting by adding secondary ice formation processes that have been missing until now and are not yet fully validated, and the underestimation of supercooled liquid water, which we hope to improve by drawing on recent measurement campaigns. But there's nothing better than LIMA in Meso-NH... until a bin scheme is added!

.. note::

   If you too would like to explain a development that you have implemented in Meso-NH, or an analysis method that you are sharing with the community, please let me know by `email <mailto:thibaut.dauhut@aero.obs-mip.fr>`_.

News from the support team
***********************************

Version 5.7.0
  - Version 5.7 was developed, evaluated and validated from September to December 2023. More info `here <http://mesonh.aero.obs-mip.fr/mesonh57/BooksAndGuides?action=AttachFile&do=view&target=update_from_masdev56_to_570.pdf>`_. 
  - Also worth noting: for the DIAG stage, the LGXT bug in compute_r00 has been fixed, additional variables (wind, clouds, aerosols, ...) in the current weather have been added, negative values have been changed by XFILLVALUE.

Version 5.7.1 (under development)
  - All the test cases have been added to a commit of the MNH-57-branch.
  - File names will be more flexible (CEXP and CSEG can be up to 32 characters long, modifiable if necessary).
  - The number of coupling files will no longer be limited (previously 24, now 1000 and expandable if necessary).
  - The management of all files has been reorganised internally. For example, the entire list of *backups* and *outputs* will no longer be stored from the start. The gain in memory space is not at all negligible when the number of backups and outputs is large, especially if parallel I/O is enabled. To do this, you need to specify the write times with the frequency options in the namelists (i.e. use XBAK_TIME_FREQ and not XBAK_TIME).
  - Compression (lossless) will be possible for all written files (in netCDF format).
  - For Lagrangian trajectories (DIAG programme), the fields will now be stored in 4D variables (x, y, z, t) and no longer in different variables for each instant (more readable file and less *post-processing* required).
  - Backup* files can be written with a reduction in the precision of floating-point numbers (warning: dangerous option).
  - The same functionality applies to files written by DIAG.
  - For the DIAG stage: it will be possible to write XT_traj, RV_traj (PW) directly.

Current developments
  - Chemistry/aerosols: a project has begun to restructure the chemistry and aerosols in the Météo-France models (ARPEGE, MOCAGE, AROME, MESO-NH) in order to externalise the chemistry and aerosols. Work is in progress, and many routines will be impacted, notably within ch_monitorn.f90, ch_* and all *aer*.
  - Version 6.0: development of the next major version has begun with the upgrade of the GPU branch (MNH-55X-dev-OPENACC-FFT) phased on 5.6 initially without PHYEX. This new MNH-56X-dev-OPENACC-FFT-unlessPHYEX branch is running on GPUs in a number of tests. Performance tests on GPU architectures (AMD and Nvidia) have been carried out, but this branch has not yet been validated on CPUs. The OpenACC directives are currently being (manually) ported to PHYEX.
  - SURFEX: changes to files in SURFEX are uploaded to the official SURFEX-offline repository for the next version 9.2.
  - ECRAD will soon be getting a makeover: removal of the (non-open-source) version 1.0.1, plugging in a more recent version.
  - Tools: added functionality in the `Python Fortran Tool <https://github.com/UMR-CNRM/pyft>` library to automatically manage certain transformations of MesoNH source code to produce code that runs on a GPU.
  - A new layout for the site and documentation is currently being tested in specific areas.
  - A note on the use of the extraction tool developed by Jean Wurtz is being prepared.
  - A comparison of Meso-NH with other competing models in terms of performance is underway.

Development under consideration
  In outputs, we are currently looking into the possibility of writing fields on sub-domains rather than on the entire grid.

Other news
  The Meso-NH course went well with 11 trainees from various institutions (ONERA, Université de Lille, Université de Corse, LAERO, SUPAERO and CNRM) from 4 to 7 March 2024. The next course will take place from 12 to 15 November 2024.


Latest publications using Meso-NH
****************************************************************************************

Air-sea interactions
  - The wave-age-dependent stress parameterisation (WASP) for momentum and heat turbulent fluxes at sea in SURFEX v8.1 [`Bouin et al., 2024 <https://doi.org/10.5194/gmd-17-117-2024>`_]
  - A numerical study of ocean surface layer response to atmospheric shallow convection: impact of cloud shading, rain and cold pool [`Brilouet et al., 2024 <https://doi.org/10.1002/qj.4651>`_]

Boundary layer processes
  - Coherent subsiding structures in large eddy simulations of atmospheric boundary layers Brient [`Brient et al., 2024 <https://doi.org/10.1002/qj.4625>`_]
  - Breakdown of the velocity and turbulence in the wake of a wind turbine – Part 1: Large-eddy-simulation study [`Jézéquel et al., 2024a <https://doi.org/10.5194/wes-9-97-2024>`_]
  - Breakdown of the velocity and turbulence in the wake of a wind turbine – Part 2: Analytical modeling [`Jézéquel et al., 2024b <https://doi.org/10.5194/wes-9-119-2024>`_]
  - Impact of surface turbulent fluxes on the formation of convective rolls in a Mediterranean windstorm [`Lfarh et al., 2024 <https://doi.org/10.22541/essoar.169774560.07703883/v1>`_]
  - The Marinada fall wind in the eastern Ebro sub-basin: Physical mechanisms and role of the sea, orography and irrigation [`Lunel et al., 2024 <http://dx.doi.org/10.5194/egusphere-2024-495>`_]

Lightnings and Fire meteorology
  - Numerical investigation of the Pedrógão Grande pyrocumulonimbus using a fire to atmosphere coupled model [`Couto et al., 2024 <https://doi.org/10.1016/j.atmosres.2024.107223>`_]
  - 3D Monte-Carlo simulations of lightning optical waveforms and images observable by on-board operational instruments [`Rimboud et al., 2024 <http://dx.doi.org/10.1016/j.jqsrt.2024.108950>`_]

Aerosols and their interactions with clouds and dynamics:
  - Fractional solubility of iron in mineral dust aerosols over coastal Namibia: a link to marine biogenic emissions? [`Desboeufs et al., 2024 <https://doi.org/10.5194/acp-24-1525-2024>`_]
  - Cyclogenesis in the tropical Atlantic: First scientific highlights from the Clouds-Atmospheric Dynamics-Dust Interactions in West Africa (CADDIWA) field campaign [`Flamant et al., 2024a <https://doi.org/10.1175/BAMS-D-23-0230.1>`_]
  - The radiative impact of biomass burning aerosols on dust emissions over Namibia and the long-range transport of smoke observed during AEROCLO-sA [`Flamant et al., 2024b <https://doi.org/10.5194/egusphere-2023-2371>`_]

Extreme precipitations
  - Impact of urban land use on mean and heavy rainfall during the Indian summer monsoon [`Falga and Wang, 2024 <https://doi.org/10.5194/acp-24-631-2024>`_]

Chemistry and atmospheric composition:
  - Measurement Report: Bio-physicochemistry of tropical clouds at Maïdo (Réunion Island, Indian Ocean): overview of results from the BIO-MAÏDO campaign [`Leriche et al., 2024 <https://doi.org/10.5194/egusphere-2023-1362>`_]
  - Measurement Report: Insights into the chemical composition of molecular clusters present in the free troposphere over the Southern Indian Ocean: observations from the Maïdo observatory (2150 m a.s.l., Reunion Island) [`Salignat et al., 2024 <https://doi.org/10.5194/acp-24-3785-2024>`_]

.. note::

   If you would like to share with the community the fact that one of your projects using Meso-NH has been funded or any other communication about your work (in particular posters and presentations available online), please do not hesitate to write to me. I'd also be delighted to hear your views on the proposed format for these newsletters.

Enjoy simulating with Méso-NH!

See you soon,

Thibaut
and the entire support team
