Infolettre #06
================================================

**9 juillet 2025.** Version française, English version `here <newsletter_06_english.html>`_.


Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici ci-dessous la 6ème infolettre de notre communauté. Vous y trouverez un entretien avec un développeur de Méso-NH, les dernières nouvelles de l’équipe support et la liste des dernières publications utilisant Méso-NH.

Entretien avec `Jean-Pierre Chaboureau <mailto:jean-pierre.chaboureau@utoulouse.fr>`_ (LAERO)
************************************************************************************

.. image:: photo_jpc.jpg
  :width: 200


Jean-Pierre, tu as implanté RTTOV dans Méso-NH. Pourrais-tu résumer ce que fait ce code ?
  RTTOV (Radiative Transfer for TOVS) est un code de transfert radiatif très rapide pour les radiomètres et autres interféromètres passifs dans le visible, l’infrarouge et les micro-ondes, ainsi que pour les radars, à bord des satellites opérationnels et de recherche. Il est développé par le consortium européen `NWP SAF <https://www.nwpsaf.eu/site/software/rttov/>`_ (Satellite Application Facility for Numerical Weather Prediction). Il s'agit d'un code Fortran 90 pour la simulation des radiances et des réflectivités obtenues à partir de satellites, conçu pour être incorporé dans les applications des utilisateurs. Dans le monde Méso-NH version 5, RTTOV peut être ainsi appelé par DIAG à l’aide de la subroutine ``call_rttov13.f90``. (Dans Méso-NH version 6, RTTOV version 14 sera appelé par la subroutine ``call_rttov14.f90``.) 

Pourquoi conseilles-tu d'utiliser RTTOV ?
  L’émulation d’une image satellite synthétique Méso-NH avec RTTOV permet de valider une sortie de simulation avec l'image satellite. Cette approche, dite modèle vers satellite, fait une comparaison directe avec les variables satellites, sans nécessiter d’hypothèses supplémentaires (contrairement aux produits satellites dérivés de l’inversion de l’équation du transfert radiatif). Toutes les erreurs proviennent donc du monde du modèle, c’est-à-dire de la simulation Méso-NH interprétée par RTTOV. Cette approche est un outil particulièrement pertinent pour évaluer la représentation des nuages dans une simulation Méso-NH à maille kilométrique. A cette échelle, la taille des pixels de l’observation satellitale est commensurable à celle de la maille du modèle.

Quelles recommandations d’utilisation ferais-tu à la communauté Méso-NH ?
  L'observation par satellite géostationnaire permet d’évaluer une simulation avec des images à toutes les 10 à 15 minutes. Cela est très utile pour déterminer si un système nuageux est bien positionné. Cette évaluation peut être réalisée dans le domaine infrarouge thermique pour le suivi des nuages profonds, dans le domaine solaire pour le suivi de nuages peu profonds (depuis RTTOV version 12), et dans le domaine radar micro-ondes pour la structure verticale des nuages et des précipitations (depuis RTTOV version 13). Des exemples sont la figure 3 de `Lfarh et al. (2023) <https://doi.org/10.1175/MWR-D-23-0099.1>`_ pour la position d’une tempête méditerranéenne à 200 m de résolution dans le domaine visible, la figure 2 de `Feger et al. (2025) <https://doi.org/10.5194/egusphere-2025-105>`_ pour le suivi de la position d’un système convectif de méso-échelle (MCS) dans le domaine infrarouge, et la figure 10 de `Feger et al. (2025) <https://doi.org/10.5194/egusphere-2025-105>`_ pour l’intrusion d’air sec dans le MCS telle que vue dans une coupe radar. RTTOV combiné à des simulations Méso-NH est également utilisé pour aider à définir les futurs capteurs spatiaux, tels que la mission `C2OMODO <https://c2omodo.ipsl.fr/>`_ (Convective Core Observations through MicrOwave Derivatives in the trOpics) qui sera lancée en 2030.

Quelles sont les limites ?
  RTTOV est un code très rapide pour lequel les effets d’absorption gazeuse et de diffusion des gaz et des particules sont paramétrés. Les utilisateurs expérimentés peuvent préférer des codes de recherche, plus précis mais plus lents, par exemple pour prendre en compte les effets tridimensionnels ou de diffusion multiple. Un effet bien connu est celui de parallaxe, valable par exemple, pour les nuages convectifs profonds observés à fine résolution avec un grand angle de vue. Un autre écueil à éviter est l’utilisation de RTTOV pour ajuster les propriétés optiques ou microphysiques des aérosols et des nuages. Il faut veiller à ce que ces propriétés soient cohérentes entre les différents schémas microphysiques et radiatifs utilisés. Il en va de même pour l'évaluation des propriétés de la surface continentale.



.. note::

  Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par `mail <mailto:thibaut.dauhut@utoulouse.fr>`_.

    
    
Les nouvelles de l’équipe support
************************************

Médaille de Cristal du CNRS pour Juan !
  Le CNRS a décerné une `médaille de cristal à Juan Escobar <https://www.insu.cnrs.fr/fr/cnrsinfo/juan-escobar-munoz-une-medaille-de-cristal-pour-son-expertise-sur-le-calcul-intensif-pour>`_  pour son expertise sur le calcul intensif pour les sciences de l’atmosphère. Toutes nos félicitations Juan ! et un grand merci pour tout ton travail pour le code Méso-NH et sa communauté !

Version 5.7.2
  - La version 5.7.2 est sortie, elle contient de nombreux commits et certaines corrections importantes.
  - Tous les détails sont présentés `ici <https://mesonh-beta-test-guide.readthedocs.io/en/latest/getting_started/releases/release_note_572.html>`_ dans la note associée à sa sortie.

Version 6
  - La préparation de la version 6 est toujours en cours, la branche de travail MNH-60-branch a été déclarée. 
  - Un appel à contribution sera proposé courant septembre.
  - Le ménage interne (déjà annoncé) se poursuit.
  - La bibliothèque de compression Zstandard est totalement intégrée dans la version 6. De plus, elle devient la bibliothèque de compression par défaut et surtout la compression sera activée par défaut (débrayable via les namelistes).
  - La librairie ACLIB (chimie et aérosols externalisés) continue son développement pour intégration dans la version 6 (une première version d'ACLIB sera intégrée MNH-60-branch en juillet).

.. note::
  **Pensez au dépôt Zenodo pour vos futures publications**, la tar ball y est déposée et un DOI y est associé. Vous pourrez faire référence à ce `dépôt <https://zenodo.org/records/15095131>`_ notamment dans la section *Data availability* ou équivalente. 

Journées des utilisateurs et utilisatrices de Méso-NH
  Elles se tiendront cette année du 13 au 15 octobre 2025 au CNRM, Météo-France.

Nouveaux sites web
  La mise en place et la rédaction des nouveaux sites internet se poursuivent.

Stage Méso-NH
  - Le prochain stage Méso-NH est programmé du 1er au 4 décembre 2025. 
  - Le stage se déroulera en présentiel et en français. Les places sont limitées.
  - Envoyez un mail à `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_ pour informations et inscriptions. 

.. note::
  Si vous avez des besoins, idées, améliorations à apporter, bugs à corriger ou suggestions concernant les entrées/sorties, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ est preneur.


Dernières publications utilisant Méso-NH
****************************************************************************************

Aerosol-cloud interactions
  - RCEMIP-ACI: Aerosol-Cloud Interactions in a Multimodel Ensemble of Radiative-Convective Equilibrium Simulations [`Dagan et al., 2025 <https://doi.org/10.22541/essoar.174534436.64971999/v1>`_]

C2OMODO satellite mission
  - Vertical velocity deduced from ice change in deep convection [`Auguste and Chaboureau, 2025 <https://doi.org/10.1175/JAS-D-24-0118.1>`_]
  - Bridging time-delayed microwave radiometric observations and deep convection characteristics: a machine learning approach for the C2OMODO mission [`Lefebvre et al., 2025 <https://doi.org/10.1109/TGRS.2025.3583571>`_]

Object tracking
  - A Unified Open-Source Toolkit for Atmospheric Object Tracking and Analysis [`Hahn et al., 2025 <https://doi.org/10.5194/egusphere-2025-1328>`_]

Shallow convection
  - An update of shallow cloud parameterization in the AROME NWP model [`Marcel et al., 2025 <https://doi.org/10.5194/egusphere-2025-2504>`_]


PhD thesis
  - Investigation des effets aérodynamiques de la canopée forestière sur le comportement de feux expérimentaux [`Antolin, Université de Toulouse, 2025 <https://theses.fr/s305185>`_]

Presentations at Ateliers de Modélisation de l'Atmosphère 2025
  De nombreux utilisateurs et utilisatrices de Méso-NH ont présenté leurs travaux lors des AMA 2025. Leurs présentations (pdf et enregistrements) sont disponibles `en ligne ici <http://www.meteo.fr/cic/meetings/2025/AMA/presentations.html>`_.

.. note::

   Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations *disponibles en ligne*), n’hésitez pas à m’écrire. A l’occasion de la mise en place de ces infolettres, je suis également preneur de vos avis sur le format proposé.

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut Dauhut et toute l’équipe Méso-NH : Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
