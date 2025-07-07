Newsletter #05
================================================

**7 April 2025.** English version, version française `ici <newsletter_05.html>`_.


Dear Meso-NH users,

This is the 5th newsletter from our Meso-NH community. You'll find an interview with the developer of a useful tool for Meso-NH users, the latest news from the support team and a list of the latest publications using Meso-NH.

Interview with `Najda Villefranque <mailto:najda.villefranque@meteo.fr>`_ (CNRM)
*************************************************************************************

.. image:: photo_nv.jpg
  :width: 250


.. note::

   A longer version of the interview, with more details, is available `here <newsletter_05_english_extended.html>`_.


Najda, you've developed a 3D radiation simulator that can be applied to 3D fields from Meso-NH simulations. Could you summarize what this software does?
  `htrdr <https://www.meso-star.com/projects/htrdr/htrdr.html>`_ is a free software based on Monte Carlo methods that simulates the trajectory of a large number of photons to solve atmospheric radiative transfer. It takes as input a 3D description of the atmosphere, typically output from Meso-NH simulations, as well as a description of surfaces (e.g. of a city like `here <https://web.lmd.jussieu.fr/~nvillefranque/pages/teapot_city>`_ but we can obviously also prescribe a flat surface). Photons interact with hydrometeors, aerosols, gases and surfaces through absorption/emission and scattering/reflection phenomena. The distribution of photons on virtual sensors positioned in the scene by the user gives an estimate of the intensity of radiation on these sensors.

What applications do you recommend using htrdr for?
  htrdr is the ideal reference calculation software for accurately determining radiative quantities, whether solar (SW) or thermal (LW), in map form or horizontally integrated. It is particularly useful for estimating the radiative effect of clouds or aerosols, studying their impact as a function of their properties, and evaluating the parameterizations of large-scale models. htrdr also serves as a target for calibrating these parameterizations in idealized cases, where observations are lacking. In short, htrdr is to radiation what Meso-NH is to atmospheric dynamics! 

  htrdr can also be used to produce computer-generated images, using techniques similar to those used by Disney for their films. These simulations can be used to visualize a 3D Meso-NH field as a photo or film, or to synthesize satellite-like images, useful for designing or evaluating inversion algorithms.

Why is it better to use htrdr than other equivalent programs? 
  The advantages of Monte Carlo methods, such as those used by htrdr, are their precision (taking into account detailed processes, absence of bias), their flexibility (notably over spatial and spectral integration domains) and their theoretical insensitivity to the complexity of the virtual scenes in which photons propagate. Unlike other codes, htrdr uses tools derived from graphics research, making computation time independent of the amount of data to be processed, and extended to volume data by |villefranque_etal|. This means that a high-resolution simulation over detailed terrain will take no longer than a low-resolution simulation over flat ground. What's more, htrdr is a continuously developing software, used and enriched by various communities, making it a living, evolving tool.

What recommendations would you make to users?
    I'd recommend that they don't hesitate to contact me (or Robert Schoetter, who's also an htrdr expert!) if they want to get to grips with the software. Although `documentation <https://www.meso-star.com/projects/htrdr/man/man1/htrdr-atmosphere.1.html>`_ and `training material <https://mattermost.lmd.ipsl.fr/g3t-rayonnement/channels/htrdr>`_ are available online, prior discussion helps to ensure that htrdr meets specific needs. It can also help choose appropriate configurations and options, and guide users through input and output. It's also an opportunity to discuss radiation physics, even if the scientific question is not directly related to radiative transfer. Don't hesitate, because in real life, radiation is a lot more fun than you might think (laughs)!

What are its limitations?
  The main drawback of htrdr is the computation time, which can be limiting in the presence of highly scattering clouds. Although this is not technically limiting for users accustomed to large simulations, it is important to note that it consumes a lot of energy. For initial tuning, it is advisable to run simulations with a small number of photons to obtain a first estimate, even if it is noisy. There are also limits to the physical modeling of the transfer, such as the approximation of the diffusion phase function of hydrometeors and aerosols. However, these limitations can be overcome with time and further research. In short, htrdr is a powerful and scalable tool, capable of meeting new needs through future developments. Finally, I'd like to point out that to date htrdr is essentially diagnostic software, to be used *off-line* on simulation outputs produced elsewhere, with or without radiation. One of my research perspectives is to couple htrdr with Meso-NH to be able to run atmospheric simulations including 3D radiation resolution.

.. |villefranque_etal| raw:: html

   <a href="https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2018MS001602" target="_blank">Villefranque et al. (2019)</a>

.. note::

   If you too would like to explain a development you've implemented in Meso-NH, or an analysis method you share with the community, please don't hesitate to let me know by `mail <mailto:thibaut.dauhut@univ-tlse3.fr>`_.



News from the support team
************************************

Version 6
  - Preparations for version 6 are still underway, and the MNH-60-branch has been declared.
  - The ACLIB library (externalized chemistry and aerosols) continues to be developed for integration into version 6. A first version of ACLIB will be integrated into MNH-60-branch in May.
  - The GPU-ported version of PHYEX within Meso-NH has been transferred to the PHYEX repository for integration into IAL cycle 50 (IFS-ARPEGE-LAM). The latest version of PHYEX will be transferred to MNH-60-branch in May-June. As a result, the physics of AROME cycle 50 will be very close to that of Meso-NH version 6.
  - A more efficient lossless compression library, Zstandard, mainly in terms of CPU cost and slightly in compression ratio, is currently being integrated.
  - Coming soon, an internal clean-up program with restructuring of source directories and major pruning of parts of the repository that are no longer maintained and/or topical.

In the meantime, version 5.7.2 will be released shortly
  - For frequent *output* (not *backup* files), you will be able to:
  - perform threshold filtering by removing or assigning a particular value to elements of a variable that are smaller, larger or outside a range, criteria that may or may not be absolute values.
  - round variable values to a multiple of a chosen value (e.g. round all to a multiple of 0.1). Combined with compression, this is a form of lossy compression.


Other ongoing and recent developments
  - The websites (showcase site + new documentation site) continue to be developed.
  - The inclusion of ocean surface currents in the Meso-NH turbulence scheme has been validated.

Meso-NH Zenodo repository
  In order to have a DOI associated with each new version of Meso-NH, a `Zenodo repository <https://zenodo.org/records/15095131>`_ has just been created. For each version of Meso-NH, the tar ball will be deposited there and a DOI will be associated with it. 

.. note::
  **Think about it for your future publications**, you'll be able to refer to this Zenodo repository in the *Data availability* section or equivalent. 

Meso-NH internship
  The Meso-NH course was held successfully for the first time in hybrid mode, with 8 people in the room and 13 remotely, from March 10 to 13, 2025.

.. note::
  If you have any needs, ideas, improvements to make, bugs to fix or suggestions concerning inputs/outputs, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ is keen to hear from you.


Latest publications using Meso-NH
****************************************************************************************

Atmosphere-surface interactions
  - Rolling DICE to advance knowledge of land–atmosphere interactions [`Best et al., 2025 <https://doi.org/10.1002/qj.4944>`_]
  - Atmosphere response to an oceanic submesoscale SST front: A coherent structure analysis [`Jacquet et al. <https://doi.org/10.1029/2024JD042312>`_]
  - Land-surface interactions with the atmosphere over the Iberian Semi-arid Environment (LIAISE): First mesoscale modelling intercomparison [`Jiménez et al., 2025 <https://doi.org/10.1002/qj.4949>`_]

Cloud electrification
  - Numerical investigation of the role of Saharan dust on the anomalous electrical structure of a thunderstorm over Corsica [`Barthe et al., 2025 <https://doi.org/10.1016/j.atmosres.2025.107988>`_]
  - Distinct effects of several ice production processes on thunderstorm electrification and lightning activity [`Vongpaseut and Barthe, 2025 <http://doi.org/10.5194/egusphere-2025-214>`_]

Deep and shallow convection around Africa
  - Failed cyclogenesis of a mesoscale convective system near Cape Verde: The role of the Saharan trade wind layer among other inhibiting factors observed during the CADDIWA field campaign [`Feger et al., 2025 <https://doi.org/10.5194/egusphere-2025-105>`_]
  - The dry-season low-level cloud cover over western equatorial Africa: A case study with a mesoscale atmospheric model [`Berger et al., 2025 <https://doi.org/10.1002/qj.4962>`_]
      
Fire meteorology
  - Modelling aerodynamics and combustion of firebrands in long-range spotting [`Alonso-Pinar et al., 2025 <https://doi.org/10.1016/j.firesaf.2025.104348>`_]
  - Fire-weather conditions during two fires in Southern Portugal: Meteorology, Orography, and Fuel Characteristics [`Purificação et al., 2025 <https://doi.org/10.1007/s40808-025-02308-z>`_]
      
Porting on GPU supercomputers
  - Porting the Meso-NH atmospheric model on different GPU architectures for the next generation of supercomputers (version MESONH-v55-OpenACC) [`Escobar et al., 2025 <https://doi.org/10.5194/egusphere-2024-2879>`_]

PhD theses
  - Analyse globale, régionale et locale des mesures de vapeur d'eau dans la haute TTL pendant STRATÉOLE 2 [`Carbone, Université de Reims Champagne-Ardenne, 2025 <https://theses.fr/s297336>`_]

.. note::

   If you would like to share with the community the fact that one of your projects using Meso-NH has been funded, or any other communication about your work (including posters and presentations *available online*), please write to `me <thibaut.dauhut@univ-tlse3.fr>`_.

Happy simulations with Meso-NH!

See you soon,

Thibaut Dauhut and the entire Meso-NH team: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar and Jean-Pierre Chaboureau
