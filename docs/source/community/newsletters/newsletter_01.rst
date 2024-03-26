Newsletter     #01
================================================

Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici la 1ère infolettre de notre communauté Méso-NH. Vous y trouverez un entretien avec un développeur de Méso-NH, les dernières nouvelles de l’équipe support et la liste des dernières publications utilisant Méso-NH.

.. image:: photo_bv.jpg
  :width: 300


Entretien avec `Benoît Vié <mailto:benoit.vie@meteo.fr>`_ (CNRM) au sujet de LIMA pour la représentation de la microphysique à deux moments
************************************************************************************************************************************************

Benoît, tu as fortement contribué à LIMA pour la représentation à deux moments de la microphysique dans Méso-NH. Pourrais-tu résumer ce que fait cette option ?
  Oui, j’ai repris le développement de LIMA, dans la suite du travail de thèse de Sarah Berthet. C'est un des schémas microphysiques disponibles dans Meso-NH, qui s'occupe donc de prévoir la formation des nuages et des précipitations. Comme ICE3 dont il s'inspire largement, la composition nuageuse est décomposée en grandes catégories d'hydrométéores (gouttelettes, pluie, petits cristaux, neige, graupel et grêle). Mais la nouveauté de LIMA, c'est de prévoir le nombre d'hydrométéores de chaque type, en plus de leur masse totale. Cela permet de mieux estimer leur taille et de mieux représenter l'ensemble des processus (condensation/évaporation, collisions...) qui dirigent leur évolution. LIMA représente aussi les interactions aérosols-nuages, en s'appuyant sur une population d'aérosols pronostique réaliste pour déterminer le nombre d'hydrométéores. LIMA est ainsi le schéma microphysique de Méso-NH le plus à même de prévoir avec précision la composition nuageuse, son évolution et la formation des précipitations.

Pourquoi vaut-il mieux utiliser LIMA qu’une autre représentation de la microphysique ?
  L'apport de LIMA se traduit généralement par de meilleures prévisions ou simulations. Ainsi, il a déjà été montré par exemple que LIMA est plus performant pour la prévision du brouillard, et produit des systèmes convectifs plus réalistes en comparaison à des observations radar, notamment parce qu'il est capable de représenter des grosses gouttes de pluie et gère mieux l'évaporation des précipitations sous le nuage. Donc, que ce soit pour de la prévision ou de l'étude de processus, n'hésitez pas à l'essayer !

Dans quel cas cette option est-elle recommandée?
  En fait, quel que soit le système nuageux qui vous intéresse, il est probable que LIMA soit le bon choix ! D'autant plus que LIMA est maintenant très modulaire et qu'il est possible de choisir le niveau de complexité souhaité en fonction du sujet d'étude.

Quelles recommandations ferais-tu aux utilisateurs.trices ? 
  Le meilleur conseil est de ne pas hésiter à demander conseil, justement ! LIMA peut être utilisé très simplement, en mettant CCLOUD="LIMA" dans votre namelist, avec des réglages par défaut qui devraient donner une bonne base de travail. Néanmoins, le vrai potentiel de LIMA s'exprimera si sa configuration est adaptée au cas d'étude. Il est par exemple souvent important de bien définir la population d'aérosols que LIMA utilisera pour former des gouttelettes et des cristaux pour obtenir les meilleurs résultats, et parfois même nécessaire d'activer également les schémas d'émission d'aérosols. Au delà de LIMA, Meso-NH dispose maintenant d'un ensemble de paramétrisations cohérent (pour les aérosols, la microphysique, le rayonnement, l'électricité...) et il est facile de se perdre parmi toutes les options disponibles, donc, je le répète : n'hésitez pas à nous demander des conseils !

Quelles sont les limites ? Dans quel cas cette option est-elle plutôt à éviter ?
  Il est inutile d'activer LIMA si ... vous simulez un cas de couche limite sèche très stable ! Plus sérieusement, nous travaillons sur deux biais encore importants de LIMA : la sous-estimation des concentrations en petits cristaux, que l'on corrige en ajoutant des processus de formation secondaire de glace qui manquaient jusqu'à maintenant et ne sont pas encore complètement validés, et la sous-estimation de l'eau liquide surfondue, que l'on a bon espoir d'améliorer en s'appuyant sur des campagnes de mesure récentes. Mais il n'y a pas mieux que LIMA dans Meso-NH... jusqu'à ce qu'un schéma bin y soit ajouté !


*Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par* `mail <mailto:thibaut.dauhut@aero.obs-mip.fr>`_.

Les nouvelles de l’équipe support
***********************************

Version 5.7.0
  - La version 5.7 a été développée, évaluée et validée de septembre à décembre 2023. Plus d'infos `ici <http://mesonh.aero.obs-mip.fr/mesonh57/BooksAndGuides?action=AttachFile&do=view&target=update_from_masdev56_to_570.pdf>`_. 
  - A noter également : pour l’étape DIAG, le bogue LGXT dans compute_r00 a été corrigé, des variables supplémentaires (vent, nuages, aérosols, …) du temps courant ont été ajoutées, les valeurs négatives ont été changées par XFILLVALUE.

Version 5.7.1 (en developpement)
  - Tous les cas tests ont été ajoutés dans un commit de la branche MNH-57-branch.
  - Les noms de fichiers seront plus flexibles (CEXP et CSEG pouvant aller jusqu'à 32 caractères, modifiable au besoin).
  - Le nombre de fichiers de couplage ne sera plus limité (était de 24, maintenant à 1000 et extensible si nécessaire).
  - La gestion de tous les fichiers a été réorganisée en interne. Par exemple, on ne stockera plus dès le début la totalité de la liste des fichiers de reprise (*backups*) et de sorties fréquentes (*outputs*). Le gain en espace mémoire n'est pas du tout négligeable lorsque le nombre de backups et d'outputs est important surtout si les I/O parallèles sont activées. Pour cela, il faut spécifier les instants d'écriture avec les options en fréquence dans les namelistes (ie utiliser XBAK_TIME_FREQ et non XBAK_TIME).
  - La compression (sans pertes) sera possible pour tous les fichiers écrits (au format netCDF).
  - Pour les trajectoires lagrangiennes (programme DIAG), les champs seront maintenant stockés dans des variables 4D (x, y, z, t) et non plus dans des variables différentes pour chaque instant (fichier plus lisible et moins de *post-processing* à réaliser).
  - Les fichiers de reprise (*backups*) pourront être écrits avec diminuation de la précision des nombres en virgule flottante (attention : option dangereuse).
  - Même fonctionnalité pour les fichiers écrits par DIAG.
  - Pour l’étape DIAG : il sera possible d'écrire directement XT_traj, RV_traj (PW).

Développement en cours
  - Chimie/aérosols : un projet a commencé à restructurer la chimie et les aérosols dans les modèles de Météo-France (ARPEGE, MOCAGE, AROME, MéSONH) pour externaliser la chimie et les aérosols. Le travail est en cours, les routines impactées seront nombreuses notamment à l'intérieur de ch_monitorn.f90, les ch_* et tous les *aer*.
  - Version 6.0 : le développement de la prochaine version majeure a commencé par la montée de version de la branche GPU (MNH-55X-dev-OPENACC-FFT) phasé sur la 5.6 dans un premier temps sans PHYEX. Cette nouvelle branche MNH-56X-dev-OPENACC-FFT-unlessPHYEX tourne sur GPU sur quelques tests. Des tests de performances sur les architectures avec GPU (AMD et Nvidia) ont été réalisés, mais cette branche n’a pas encore été validée sur CPU. Les directives OpenACC sont en cours de portage (manuel) dans PHYEX.
  - SURFEX :  les modifications des fichiers dans SURFEX sont remontés au dépot de SURFEX-offline officiel pour la prochaine version 9.2.
  - ECRAD va prochainement faire peau neuve : suppression de la version (non open-source) 1.0.1, branchement d'une version plus récente.
  - Outils : ajouts de fonctionnalités dans la librairie `Python Fortran Tool <https://github.com/UMR-CNRM/pyft>`_ pour gérer automatiquement certaine transformation du code source de MésoNH pour produire du code qui tourne sur GPU.
  - Une nouvelle mise en page du site et de la documentation est en cours de test sur des parties spécifiques.
  - Une note pour l'utilisation de l'outil d'extraction développé par Jean Wurtz est en cours de préparation.
  - Une comparaison de Méso-NH avec d'autres modèles concurrents en termes de performance est en cours.

Développement en cours de réflexion
  Dans les sorties fréquentes (*outputs*) la possibilité d'écrire des champs sur des sous-domaines plutôt que sur toute la grille est actuellement à l'étude.

Autres nouvelles
  Le stage Méso-NH s'est bien déroulé avec 11 stagiaires de différents établissements (ONERA, Université de Lille, Université de Corse, LAERO, SUPAERO et CNRM) du 4 au 7 mars 2024. Le prochain stage aura lieu du 12 au 15 novembre 2024.


La liste des publications utilisant Méso-NH parues dernièrement (depuis le début 2024)
****************************************************************************************

Air-sea interactions
------------------------

The wave-age-dependent stress parameterisation (WASP) for momentum and heat turbulent fluxes at sea in SURFEX v8.1
  Bouin, M.-N., C. Lebeaupin Brossier, S. Malardel, A. Voldoire, and C. Sauvage. Geosci. Model Dev., 17, 117-141, 2024. [ `http <https://doi.org/10.5194/gmd-17-117-2024>`_ ]


A numerical study of ocean surface layer response to atmospheric shallow convection: impact of cloud shading, rain and cold pool,
  Brilouet, P.-E., J.-L. Redelsperger, M.-N. Bouin, F. Couvreux, and N. Villefranque. Quart. J. Roy. Meteor. Soc., n/a, accepted, 2024. [ http ]


Boundary layer processes
--------------------------

Coherent subsiding structures in large eddy simulations of atmospheric boundary layers
  Brient, F., F. Couvreux, C. Rio, and R. Honnert. Quart. J. Roy. Meteor. Soc., 150, 834-856, 2024. [ http ]

Breakdown of the velocity and turbulence in the wake of a wind turbine – Part 1: Large-eddy-simulation study
  Jézéquel, E., F. Blondel, and V. Masson. Wind Energ. Sci., 9, 97–117, 2024a. [ http ]

Breakdown of the velocity and turbulence in the wake of a wind turbine – Part 2: Analytical modeling
  Jézéquel, E., F. Blondel, and V. Masson. Wind Energ. Sci., 9, 119–139, 2024b. [ http ]

Impact of surface turbulent fluxes on the formation of convective rolls in a Mediterranean windstorm
  Lfarh, W., F. Pantillon, and J.-P. Chaboureau. J. Geophys. Res., n/a, submitted, 2024. [ http ]


Fire meteorology
------------------

Numerical investigation of the Pedrógão Grande pyrocumulonimbus using a fire to atmosphere coupled model
  Couto, F. T., J.-B. Filippi, R. Baggio, C. Campos, and R. Salgado. Atmos. Res., 299, 107223, 2024. [ http ]

Aerosols and their interactions with clouds and dynamics
----------------------------------------------------------

Fractional solubility of iron in mineral dust aerosols over coastal Namibia: a link to marine biogenic emissions?
  Desboeufs, K., P. Formenti, R. Torres-Sánchez, K. Schepanski, J.-P. Chaboureau, H. Andersen, J. Cermak, S. Feuerstein, B. Laurent, D. Klopper, A. Namwoonde, M. Cazaunau, S. Chevaillier, A. Feron, C. Mirande-Bret, S. Triquet, and S. J. Piketh. Atmos. Chem. Phys., 24, 1525–1541, 2024. [ http ]

Cyclogenesis in the tropical Atlantic: First scientific highlights from the Clouds-Atmospheric Dynamics-Dust Interactions in West Africa (CADDIWA) field campaign
  Flamant, C., J.-P. Chaboureau, J. Delanoë, M. Gaetani, C. Jamet, C. Lavaysse, O. Bock, M. Borne, Q. Cazenave, P. Coutris, J. Cuesta, L. Menut, C. Aubry, A. Benedetti, P. Bosser, S. Bounissou, C. Caudoux, H. Collomb, T. Donal, G. Febvre, T. Fehr, A. H. Fink, P. Formenti, N. Gomes Araujo, P. Knippertz, E. Lecuyer, M. Neves Andrade, C. G. Ngoungué Langué, T. Jonville, A. Schwarzenboeck, and A. Takeishi. Bull. Amer. Meteo. Soc., 105, E387–E417, 2024a. [ http ] 

The radiative impact of biomass burning aerosols on dust emissions over Namibia and the long-range transport of smoke observed during AEROCLO-sA
  Flamant, C., J.-P. Chaboureau, M. Gaetani, K. Schepanski, and P. Formenti. Atmos. Chem. Phys., n/a, submitted, 2024b. [ http ]


Extreme precipitations
------------------------

Impact of urban land use on mean and heavy rainfall during the Indian summer monsoon
  Falga, R., and C. Wang. Atmos. Chem. Phys., 24, 631–647, 2024. [ http ]

Chemistry and atmospheric composition
----------------------------------------

Measurement Report: Bio-physicochemistry of tropical clouds at Maïdo (Réunion Island, Indian Ocean): overview of results from the BIO-MAÏDO campaign
  Leriche, M., P. Tulet, L. Deguillaume, F. Burnet, A. Colomb, A. Borbon, C. Jambert, V. Duflot, S. Houdier, J.-L. Jaffrezo, M. Vaïtilingom, P. Dominutti, M. Rocco, C. Mouchel-Vallon, S. El Gdachi, M. Brissy, M. Fathalli, N. Maury, B. Verreyken, C. Amelynck, N. Schoon, V. Gros, J.-M. Pichon, M. Ribeiro, E. Pique, E. Leclerc, T. Bourrianne, A. Roy, E. Moulin, J. Barrie, J.-M. Metzger, G. Péris, C. Guadagno, C. Bhugwant, J.-M. Tibere, A. Tournigand, E. Freney, K. Sellegri, A.-M. Delort, P. Amato, M. Joly, J.-L. Baray, P. Renard, A. Bianco, A. Réchou, and G. Payen. Atmos. Chem. Phys., n/a, in discussion, 2024. [ http ]

Measurement Report: Insights into the chemical composition of molecular clusters present in the free troposphere over the Southern Indian Ocean: observations from the Maïdo observatory (2150 m a.s.l., Reunion Island)
  Salignat, R., M. Rissanen, S. Iyer, J.-L. Baray, P. Tulet, J.-M. Metzger, J. Brioude, K. Sellegri, and C. Rose. Atmos. Chem. Phys., n/a, in discussion, 2024. [ http ]




*Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations disponibles en ligne), n’hésitez pas à m’écrire. A l’occasion de la mise en place de ces infolettres, je suis également preneur de vos avis.*

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut
et toute l’équipe support
