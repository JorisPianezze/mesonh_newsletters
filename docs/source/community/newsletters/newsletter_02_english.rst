Newsletter #02
================================================

**8 July 2024.** English version, version française `ici <newsletter_02.html>`_.

 

Dear Meso-NH users,

This is the 2nd newsletter from our Meso-NH community. You'll find an interview with a Meso-NH developer, the latest news from the support team and a list of the latest publications using Meso-NH.

Interview with `Fleur Couvreux <mailto:fleur.couvreux@meteo.fr>`_ (CNRM)
**************************************************************************

.. image:: photo_fc.jpg
  :width: 450

Fleur, you have developed the option of including passive radioactive-decay tracers in Meso-NH. Could you summarize what this option does?
  This option was initially developed to identify coherent structures, i.e. thermals, the rising plumes that contribute strongly to vertical transport in convective boundary layers, in LES simulations. Once the structures have been identified, their contribution to the vertical transport of heat, moisture and momentum can be quantified.

  Up to three passive tracers can be activated without modifying the code. The 1st is emitted with a constant surface flux, the 2nd is emitted just below the cloud base, and has been implemented to estimate exchanges between the sub-cloud layer and the cloud interior, or even the transition layer. The 3rd is emitted just above the mean cloud top and is used to estimate exchanges at the cloud top. If the sky is clear, you can choose to emit tracer 3 at the top of the boundary layer, and choose how to determine this boundary layer height [#namelist]_. All three tracers undergo radioactive decay to avoid an accumulation of their concentration, with a lifetime set at 15 min by default but which can be modified in namelist. 

  Below you'll find references to three studies using these passive tracers and a link to a video illustrating their use (many thanks to `Florent Brient <mailto:florent.brient@lmd.ipsl.fr>`_ for producing and posting his videos).

.. [#namelist] NFINDTOP=1 : determination of the boundary layer height as the area of maximum potential temperature gradient. 
   NFINDTOP=2: determination of the boundary layer height using the level where the virtual potential temperature exceeds that of the average of the underlying levels plus a threshold.

When is this option recommended?
  This option is recommended for LES simulations under idealized conditions, when you want to document coherent boundary layer structures. Numerous diagnostics are already coded that allow conditional analysis of these structures and output a set of fields (mean, fraction covered, second-order moment) diagnosing structures with a positive tracer concentration anomaly and a positive vertical velocity anomaly and potentially non-zero liquid water content in the cloud layer.

What recommendations would you make to users?
  It's important to check that the default values are suitable for the user, in particular the emission altitude below cloud base and above cloud top for tracers 2 and 3, the decay rate and the threshold controlling conditional sampling. For more details, don't hesitate to `contact me <mailto:fleur.couvreux@meteo.fr>`_ or refer to the article `Couvreux et al. (2010) <https://doi.org/10.1007/s10546-009-9456-5>`_.

What are the limits? When should this option be avoided? 
  The tracer emission works as it is and can already be used to identify structures, but the statistical diagnostic calculations of the conditional analysis need to be adapted. In this case, it may be more appropriate to perform a 3D field analysis from the *OUT* files, rather than modifying the conditional analysis. To do this, you first need to ensure that tracer concentrations are output in the OUT files (see the procedure for adding variables to frequent output, to be applied to SVT [#addoutvar]_). A posteriori, it is important to ensure that spatial and temporal statistics converge. You can contact Nathan Philippot or Hugo Jacquet, who use tracers in the presence of surface heterogeneities such as relief or sea surface temperature.

.. [#addoutvar] This involves modifying the *IO_Field_user_write* routine in the *mode_io_field_write.f90* file located in the *src/LIB/SURCOUCHE/src/* directory: uncomment its contents following the instruction given in the commentary, add variables SVT002 and SVT003 based on the example given for SVT001, delete any other variable, adapt the description of each field given by CCOMMENT, and of course recompile.

References
  - Resolved versus parametrized boundary-layer plumes. Part I: a parametrization-oriented conditional sampling in Large-Eddy simulations [`Couvreux et al., 2010 <https://doi.org/10.1007/s10546-009-9456-5>`_] 
  - Object-oriented identification of coherent structures in large-eddy simulations: importance of downdrafts in stratocumulus [`Brient et al., 2019 <https://doi.org/10.1029/2018GL081499>`_]
  - Coherent subsiding structures in large eddy simulations of atmospheric boundary layers [`Brient et al., 2024 <https://doi.org/10.1002/qj.4625>`_]

.. figure:: Snapshot-video-florent-brient.jpg

   |location_link|. Diurnal deepening of a continental boundary layer. 2D simulation of the IHOP Dry Convective Boundary Layer. Surface-emitted passive tracer concentration is in red, and concentration of passive tracer emitted at the domain-mean boundary-layer top is in green contours. The boundary layer top is shown with the horizontal dashed black line.

.. |location_link| raw:: html

   <a href="https://www.youtube.com/watch?v=8lpD8rd49wc" target="_blank">Link to the video</a>


.. note::

  If you too would like to explain a development you've implemented in Meso-NH, or an analysis method you share with the community, please don't hesitate to let me know by `mail <mailto:thibaut.dauhut@aero.obs-mip.fr>`_.

News from the support team
***********************************

Version 5.7.1 (in validation cycle)
  - Contributors' bugfixes are being tested. Release no later than end of September 2024.
  - Integration in progress: only data on the physical domain will be written by default. Non-physical meshes on edges are automatically removed. The same applies to the top and bottom absorption layers. If required, however, all these meshes can be saved.
  - Integration in progress: the possibility of making entries by boxes (sub-areas) in the frequent outputs. Each box will be able to contain its own list of fields to be written to, in addition to a common list.

Version 5.8
  A call for contributions will be launched in November. All contributions ready by December 2024, i.e. tested and delivered with a (new) test case, will be considered for integration.

Current developments
  - Chemistry/aerosols: the ACCALMIE project has begun restructuring chemistry and aerosols in Météo-France models (ARPEGE, MOCAGE, AROME, MESO-NH) to externalize chemistry and aerosols. The library will be called ACLIB (Aerosols and Chemistry LIBrary). Work is in progress, and many routines will be impacted, notably inside ch_monitorn.f90, ch_* and all *aer*.
  - ECRAD v 1.6.1 (currently operational in AROME and ARPEGE/IFS) will be connected to MesoNH. ECRAD will become the default radiation scheme in 5.8 after validation.
  - Version 6.0: development of the next major version has begun with the upgrade of the GPU branch (MNH-55X-dev-OPENACC-FFT) phased on 5.6 initially without PHYEX. This new MNH-56X-dev-OPENACC-FFT-unlessPHYEX branch is running on GPUs on some tests. Performance tests on GPU architectures (AMD and Nvidia) have been carried out, but this branch has not yet been validated on CPUs. The OpenACC directives are currently being ported (manually) to PHYEX.
  - Tools: added functionality to the Python Fortran Tool library to automatically handle certain transformations of Meso-NH source code to produce code that runs on GPUs.
  - Inputs/Outputs: several strategies for further reducing the amount of data in frequent outputs without negatively impacting their quality are currently being developed. For example, using thresholds to filter certain fields, removing a constant (e.g. for pressures or temperatures), being able to select compression parameters field by field... All this will require some fairly significant internal changes.

.. note::

  If you have any need, idea, improvement to make, bug to fix or suggestion concerning inputs/outputs, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ is happy to help. Otherwise, you'll be limited by his imagination and current priorities ;)

Meso-NH course
  - The next course will take place from 12 to 15 November, 2024. Schedule `here <http://mesonh.aero.obs-mip.fr/mesonh57/MesonhTutorial>`_.
  - Registration deadline: 1st November
  - Registration by mail to `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_

Other news
  - PHYEX: outsourced physics now comes with an offline driver in python. It enables ICE3, TURB, EKDF and ICE_ADJUST parameterizations to be launched individually in 1D or 3D.
  - The recurring application for INSU certification of our community code was submitted in May 2024. Among the new features: an estimate of the environmental footprint of the "Méso-NH community code" service (not of the user community) at 8 tonnes of CO2 equivalent per year, and the service's obligation to integrate a research infrastructure. A request has been made to CLIMERI-France.

News from SURFEX
  - SURFEX: the annual steering committee meeting took place on May 27, 2024. The presentations are available `here <https://www.umr-cnrm.fr/surfex/spip.php?article55>`_.
  - The `future of Ecoclimap <https://www.umr-cnrm.fr/surfex/IMG/pdf/surfex_steeringcommittee-27052024-physio.pdf>`_
  - Migration to GitHub, use of *forks* for integration managers (Quentin R. for Méso-NH).
  - Contribution to SURFEX at a date set by *Pull-Request* with mandatory documentation update.



Latest publications using Meso-NH
****************************************************************************************

Boundary layer processes & Complex terrain meteorology
  - Impact of surface turbulent fluxes on the formation of roll vortices in a Mediterranean windstorm [`Lfarh et al., 2024 <https://doi.org/10.22541/essoar.169774560.07703883/v1>`_]
  - The Pyrenean Platform for Observation of the Atmosphere: Site, long-term dataset and science [`Lothon et al., 2024 <https://doi.org/10.5194/amt-2024-10>`_]
  - Irrigation strongly influences near-surface conditions and induces breeze circulation: Observational and model-based evidence [`Lunel et al., 2024 <https://doi.org/10.1002/qj.4736>`_]

Fire meteorology
  - Triggering pyro-convection in a high-resolution coupled fire–atmosphere simulation [`Couto et al., 2024 <https://doi.org/10.3390/fire7030092>`_]
  - Exploring the atmospheric conditions increasing fire danger in the Iberian Peninsula [`Purificação et al., 2024 <https://doi.org/10.1002/qj.4776>`_]

Microphysics
  - A full two-moment cloud microphysical scheme for the mesoscale non-hydrostatic model Meso-NH v5-6 [`Taufour et al., 2024 <https://doi.org/10.5194/egusphere-2024-946>`_]

Sea spray & Cyclogenesis over Mediterranean sea
  - Study of the atmospheric transport of sea-spray aerosols in a coastal zone using a high-resolution model [`Limoges et al., 2024 <https://doi.org/10.3390/atmos15060702>`_]
  -  The crucial representation of deep convection for the cyclogenesis of medicane Ianos [`Pantillon et al., 2024 <https://doi.org/10.5194/egusphere-2024-1105>`_]

Urban meteorology
  - Urban influence on convective precipitation in the Paris region: Hectometric ensemble simulations in a case study [`Forster et al., 2024 <https://doi.org/10.1002/qj.4749>`_]
  - The heat and health in cities (H2C) project to support the prevention of extreme heat in cities [`Lemonsu et al., 2024 <https://doi.org/10.1016/j.cliser.2024.100472>`_]


PhD theses
  - De l'impact des aérosols sur le cycle de vie des nuages stratiformes au sud de l'Afrique de l'Ouest [`Delbeke <https://theses.fr/s295025>`_, Université de Toulouse, 2024]
  - Impact des surfaces urbanisées sur la convection en région parisienne : observations et simulations numériques hectométriques [`Forster <https://theses.fr/s302779>`_, Université de Toulouse, 2024]


.. note::

   If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other communication about your work (including posters and presentations *available online*), please write to me. I'd also be delighted to hear your views on the proposed format for these newsletters.

Enjoy simulating with Meso-NH!

See you soon,

Thibaut Dauhut and the entire Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar and Jean-Pierre Chaboureau.
