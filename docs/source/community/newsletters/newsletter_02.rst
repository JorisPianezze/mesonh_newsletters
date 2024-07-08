Infolettre #02
================================================

**8 juillet 2024.** Version française, English version `here <newsletter_02_english.html>`_.

 

Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici la 2ème infolettre de notre communauté Méso-NH. Vous y trouverez un entretien avec une développeuse de Méso-NH, les dernières nouvelles de l’équipe support et la liste des dernières publications utilisant Méso-NH.

Entretien avec `Fleur Couvreux <mailto:fleur.couvreux@meteo.fr>`_ (CNRM)
**************************************************************************

.. image:: photo_fc.jpg
  :width: 450

Fleur, tu as développé la possibilité d'inclure des traceurs passifs à décroissance radioactive dans Méso-NH. Pourrais-tu résumer ce que fait cette option ?
  Cette option a initialement été développée pour identifier les structures cohérentes, à savoir les thermiques, ces panaches ascendants qui contribuent fortement au transport vertical dans les couches limites convectives, dans des simulations LES. Une fois les structures identifiées on peut quantifier leur contribution au transport vertical de chaleur, d’humidité et de quantité de mouvement.

  On peut activer sans modifier le code jusqu’à trois traceurs passifs. Le 1er est émis avec un flux constant en surface, le 2ème est émis juste en dessous de la base des nuages, il a été implémenté pour estimer les échanges entre la couche sous-nuageuse et l’intérieur des nuages, voire la couche de transition. Le 3ème est émis juste au-dessus du sommet moyen des nuages et permet d’estimer les échanges au sommet des nuages. Si on est en condition de ciel clair, on peut choisir d’émettre le traceur 3 au sommet de la couche limite et choisir comment déterminer cette hauteur de couche limite [#namelist]_. Les trois traceurs subissent une décroissance radioactive pour éviter une accumulation de leur concentration, avec un temps de vie fixé à 15 min par défaut mais qui peut être modifié en nameliste. 

  Vous trouverez ci-dessous les références de trois études utilisant ces traceurs passifs et un lien vers une vidéo illustrant leur utilisation (un grand merci à `Florent Brient <mailto:florent.brient@lmd.ipsl.fr>`_ pour la réalisation et la mise en ligne de ses vidéos).

.. [#namelist] NFINDTOP=1 : détermination de la hauteur de couche limite comme la zone de maximum de gradient de température potentielle. 
   NFINDTOP=2 : détermination de la hauteur de couche limite en utilisant le niveau où la température potentielle virtuelle dépasse celle moyenne des niveaux sous-jacents plus un seuil.

Dans quel cas cette option est-elle recommandée?
  Cette option est recommandée dans les simulations LES en conditions idéalisées lorsqu’on veut documenter les structures cohérentes de la couche limite. De nombreux diagnostics sont déjà codés qui permettent de faire une analyse conditionnelle de ces structures et sortir un ensemble de champs (moyennes, fraction couverte, moments d’ordre 2) en diagnostiquant les structures ayant une anomalie positive de concentration de traceurs et une anomalie positive de vitesse verticale et potentiellement un contenu en eau liquide non nul dans la couche nuageuse.

Quelles recommandations ferais-tu aux utilisateurs.trices ?
  Il est important de vérifier que les valeurs par défaut conviennent à l’utilisateur notamment  l’altitude d’émission sous la base et au-dessus du sommet des nuages pour les traceurs 2 et 3, le taux de décroissance et le seuil contrôlant l’échantillonnage conditionnel. Pour plus de détails, n’hesitez pas à `me contacter <mailto:fleur.couvreux@meteo.fr>`_ ou à vous reporter à l'article `Couvreux et al. (2010) <https://doi.org/10.1007/s10546-009-9456-5>`_.

Quelles sont les limites ? Dans quel cas cette option est-elle plutôt à éviter ? 
  Attention à la mise en place des diagnostics en cas d’hétérogénéités de surface (ou de relief), l’émission des traceurs fonctionne telle quelle et peut déjà être utilisée pour identifier les structures mais les calculs de diagnostics statistiques de l’analyse conditionnelle sont à adapter. Il est sans doute plus adapté dans ce cas de faire une analyse des champs 3D à partir des fichiers *OUT* plutôt que de modifier l’analyse conditionnelle. Pour cela, il faut tout d’abord bien s’assurer que les concentrations des traceurs sont sorties dans les fichiers OUT (cf. la procédure pour rajouter des variables dans les sorties fréquentes, à appliquer à SVT [#addoutvar]_). A posteriori, il est important de s'assurer d'avoir des statistiques spatiales ou temporelles qui convergent bien. Vous pouvez contacter Nathan Philippot ou Hugo Jacquet qui utilisent les traceurs  en présence d’hétérogénéités de surface respectivement de relief ou de température de surface de la mer.

.. [#addoutvar] Il faut pour cela modifier la routine *IO_Field_user_write* dans le fichier *mode_io_field_write.f90* situé dans le répertoire *src/LIB/SURCOUCHE/src/* : décommenter son contenu en suivant l'instruction donnée en commentaire, ajouter les variables SVT002 et SVT003 en s'appuyant sur l'exemple donné pour SVT001, éventuellement supprimer d'autres variables, adapter la description de chaque champ donnée par CCOMMENT, et recompiler bien-sûr.



Références
  - Resolved versus parametrized boundary-layer plumes. Part I: a parametrization-oriented conditional sampling in Large-Eddy simulations [`Couvreux et al., 2010 <https://doi.org/10.1007/s10546-009-9456-5>`_] 
  - Object-oriented identification of coherent structures in large-eddy simulations: importance of downdrafts in stratocumulus [`Brient et al., 2019 <https://doi.org/10.1029/2018GL081499>`_]
  - Coherent subsiding structures in large eddy simulations of atmospheric boundary layers [`Brient et al., 2024 <https://doi.org/10.1002/qj.4625>`_]

.. figure:: Snapshot-video-florent-brient.jpg

   |location_link|. Diurnal deepening of a continental boundary layer. 2D simulation of the IHOP Dry Convective Boundary Layer. Surface-emitted passive tracer concentration is in red, and concentration of passive tracer emitted at the domain-mean boundary-layer top is in green contours. The boundary layer top is shown with the horizontal dashed black line.

.. |location_link| raw:: html

   <a href="https://www.youtube.com/watch?v=8lpD8rd49wc" target="_blank">Link to the video</a>


.. note::

   Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par `mail <mailto:thibaut.dauhut@aero.obs-mip.fr>`_.

Les nouvelles de l’équipe support
************************************

Version 5.7.1 (en cycle de validation)
  - Les bugfixs des contributeurs sont en cours de test. Sortie au plus tard fin septembre 2024.
  - En cours d'intégration : seules les données sur le domaine physique seront écrites par défaut. Les mailles non-physiques sur les bords sont automatiquement retirées. Il en est de même pour les couches d'absorptions supérieure et éventuellement inférieure. En cas de besoin, toutes ces mailles peuvent néanmoins être sauvegardées.
  - En cours d'intégration : la possibilité de faire des écritures par boîtes (sous-domaines) dans les sorties fréquentes. Chaque boîte pourra contenir sa liste propre de champs à écrire en plus d'une liste commune.

Version 5.8
  Un appel à contribution sera lancée en novembre. Toutes les contributions prêtes pour décembre 2024, c'est-à-dire testées et livrées avec un (nouveau) cas test, seront prises pour intégration.
 
Développement en cours
  - Chimie/aérosols : le projet ACCALMIE a commencé à restructurer la chimie et les aérosols dans les modèles de Météo-France (ARPEGE, MOCAGE, AROME, MESO-NH) pour externaliser la chimie et les aérosols. La bibliothèque s'appellera ACLIB (Aerosols and Chemistry LIBrary). Le travail est en cours, les routines impactées seront nombreuses notamment à l’intérieur de ch_monitorn.f90, les ch_* et tous les *aer*.
  - ECRAD v 1.6.1 (actuellement opérationnel dans AROME et ARPEGE/IFS) sera branchée à MésoNH. ECRAD deviendra le schéma de rayonnement par défaut dans la 5.8 après validation.
  - Version 6.0 : le développement de la prochaine version majeure a commencé par la montée de version de la branche GPU (MNH-55X-dev-OPENACC-FFT) phasée sur la 5.6 dans un premier temps sans PHYEX. Cette nouvelle branche MNH-56X-dev-OPENACC-FFT-unlessPHYEX tourne sur GPU sur quelques tests. Des tests de performance sur les architectures avec GPU (AMD et Nvidia) ont été réalisés, mais cette branche n’a pas encore été validée sur CPU. Les directives OpenACC sont en cours de portage (manuel) dans PHYEX.
  - Outils : ajouts de fonctionnalités dans la librairie Python Fortran Tool pour gérer automatiquement certaines transformations du code source de Méso-NH pour produire du code qui tourne sur GPU.
  - Entrées/Sorties : plusieurs stratégies pour réduire encore la quantité de données dans les sorties fréquentes (*outputs*) sans impacter négativement leur qualité sont en cours de réflexion. Par exemple, l'utilisation de seuils pour filtrer certains champs, de retirer une constante (i.e. pour des pressions ou des températures), de pouvoir sélectionner les paramètres de compression champ par champ... Tout cela nécessitera des changements internes assez importants.

.. note::
  Si vous avez des besoins, idées, améliorations à apporter, bugs à corriger ou suggestions concernant les entrées/sorties, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ est preneur. Sinon, vous serez limités par son imagination et ses priorités du moment ;)

Stage Méso-NH
  - Le prochain stage aura lieu du 12 au 15 novembre 2024. Planning `ici <http://mesonh.aero.obs-mip.fr/mesonh57/MesonhTutorial>`_
  - Date limite d'inscription : 1er novembre
  - Inscription par mail à `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_

Autres nouvelles
  - PHYEX: la physique externalisée se dote à présent d'un pilote logiciel hors-ligne (*driver offline*) en python. Il permet de lancer les paramétrisations ICE3, TURB, EKDF et ICE_ADJUST individuellement en 1D ou 3D.
  - La demande récurrente de labellisation par l'INSU de notre code communautaire a été déposée en mai 2024, parmi les nouveautés : une estimation de l’empreinte environnementale du service "code communautaire Méso-NH" (pas de la communauté utilisatrice) à 8 tonnes équivalent CO2 par an, et l’obligation du service à intégrer une infrastructure de recherche. Une demande a été faite auprès de CLIMERI-France.

Nouvelles de SURFEX
  - SURFEX : la réunion annuelle du comité de pilotage a eu lieu le 27 mai 2024. Les présentations sont disponibles `ici <https://www.umr-cnrm.fr/surfex/spip.php?article55>`_.
  - Le `futur d'Ecoclimap <https://www.umr-cnrm.fr/surfex/IMG/pdf/surfex_steeringcommittee-27052024-physio.pdf>`_
  - Migration vers GitHub, utilisation de fourches (*forks*) pour les responsables d'intégration (Quentin R. pour Méso-NH).
  - Contribution à SURFEX à une date fixée par requête d'intégration (*Pull-Request*) avec mise à jour de la documentation obligatoire.


Dernières publications utilisant Méso-NH
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

   Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations *disponibles en ligne*), n’hésitez pas à m’écrire. A l’occasion de la mise en place de ces infolettres, je suis également preneur de vos avis sur le format proposé.

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut Dauhut et toute l’équipe Méso-NH: Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
