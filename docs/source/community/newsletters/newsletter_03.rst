Infolettre #03
================================================

**10 octobre 2024.** Version française, English version `here <newsletter_03_english.html>`_.


Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici la 3ème infolettre de notre communauté Méso-NH. Vous y trouverez un entretien avec un membre de l'équipe support de Méso-NH, les dernières nouvelles de l’équipe support et la liste des dernières publications utilisant Méso-NH.

Entretien avec `Juan Escobar <mailto:juan.escobar-munoz@cnrs.fr>`_ (LAERO, CNRS)
************************************************************************************

.. image:: photo_je.jpg
  :width: 450

Juan, tu as porté Méso-NH 5-5-1 sur GPU avec Philippe. Pourrais-tu décrire de quoi il s'agit et les perspectives d'utilisation que cela ouvre ?
  Les GPU (*Graphics Processing Unit*) sont devenus les successeurs des supercalculateurs parallèles et vectoriels des années 2000 (comme les CRAY ou NEC). Ils possèdent des dizaines de processeurs pouvant exécuter des milliers d'instructions en parallèle, avec une bande passante mémoire bien supérieure aux CPU, tout en étant bien plus économiques et moins énergivores.

  Dès les années 2010, j’ai commencé à porter Méso-NH sur GPU grâce aux compilateurs Fortran de PGI, en utilisant la programmation par directives (ACC), qui a évolué vers le standard OpenACC. Aujourd'hui, environ 90 % des parties les plus coûteuses en calcul (advection, turbulence, microphysique à un moment et solveur de pression) sont portées sur GPU, ce qui permet d’utiliser Méso-NH pour des simulations scientifiques. Cette version a été testée lors du Grand Défi Adastra GPU avec succès, simulant des événements extrêmes sur environ 1/3 de la partition GPU d’Adastra (128 nœuds de calcul dotés au total de 1024 GPU).

Quel intérêt y a-t-il à réaliser des simulations sur les calculateurs dotés de GPU plutôt que sur ceux basés sur des CPU uniquement ?
  L'intérêt principal des GPU est leur puissance de calcul supérieure. Par exemple, sur Adastra, 338 nœuds GPU fournissent 61 Pétaflops, contre seulement 13 Pétaflops [#flop1]_ pour 556 nœuds CPU. De plus, les GPU sont bien plus efficaces énergétiquement : lors du Grand Challenge Adastra, le code tournait 5 fois plus vite sur GPU tout en consommant 2 fois moins d’énergie. Cet écart va s’accentuer. Par exemple, le futur supercalculateur européen à Jülich atteindra 1 Exaflop [#flop2]_ avec sa partition GPU, tandis que la partition CPU ne fournira que 50 Pétaflops, soit 0,5 % de la puissance totale.

.. [#flop1] Pétaflops = millions de milliards d'opérations de calcul par seconde. 
.. [#flop2] 1 Exaflop = 1000 Pétaflops.

Y a-t-il des situations qui se prêtent particulièrement bien à l'utilisation de Méso-NH 5-5-1 sur GPU ?
  Les GPU sont adaptés aux simulations massivement parallèles, comme les Giga-LES, où les grilles de calcul contiennent des milliards de points. Par exemple, lors du Grand Challenge Adastra GPU, nous avons utilisé des grilles de 2,14 milliards de points (4096x4096x128) pour des simulations à 100 mètres de résolution.

  Les GPU permettent aussi de réaliser un grand nombre de simulations simultanées sur des configurations plus petites. Par exemple, sur Adastra, nous avons simulé 350 jours de météo en Corse avec une grille de 256x256x70 points à 1 km de résolution, en utilisant 1 nœud GPU par simulation. Grâce à cette approche, l’ensemble des simulations a été réalisé en seulement 3 jours.

Quelles recommandations ferais-tu aux utilisateur.trices qui souhaiteraient simuler sur GPU ?
  Il faut se rapprocher de nous, Support Méso-NH, pour mettre le pied à l'étrier sur GPU. Au niveau utilisation du code il n'y a pas de différence fondamentale pour quelqu'un qui a déjà fait tourner Méso-NH sur un supercalculateur à base de CPU. La version GPU actuelle est basée sur MNH-5-5-1, donc il faut utiliser cette version d'abord sur CPU pour mettre au point et calibrer ses simulations, et ensuite passer sur GPU.

  Il faut bien sûr de préférence utiliser les paramétrisations déjà portées sur GPU, pour avoir un gain en performance. Toutefois, cela n'est pas obligatoire dans un premier temps, car le code peut être compilé pour être bit-reproductible tout en tournant en partie sur CPU et en partie sur GPU.

Quelles sont les limites pour l'instant de ce portage de Méso-NH ? les perspectives ?
  Actuellement, seule une partie du code a été portée sur GPU, et dans la version MNH-5-5-1, donc il faut faire des simulations avec cette version du code.

  Avec l'introduction de PHYEX, dans les versions MNH-5-6-X et MNH-5-7-X, une partie du travail de portage sur GPU réalisé a été perdu (notamment la turbulence, ICE3 et la condensation sous-maille). Toutefois, Quentin Rodier et Sebastien Riette au CNRM, ont en charge le portage de PHYEX sur GPU en utilisant la méthodologie choisie par Météo-France, soit la technique des DSL (*Domain Specific Language*). Pour ce faire, ils ont développé un pré-compilateur en python, `pyft_tool <https://github.com/UMR-CNRM/pyft>`_, permettant des transformations semi-automatiques du code pour le porter sur GPUs. La librairie transforme le code à la volée avant la compilation sur GPU, ce qui permet d'avoir un code source un peu plus allégé.

  Phillippe Wautelet et moi-même collaborons avec eux, en apportant notre expertise GPU et OpenACC pour rajouter les fonctionnalités nécessaires à cet outil pyft pour transformer le code de PHYEX dans MNH-5-6-X et l'adapter aux GPU. Parmi ces fonctionnalités, on retrouve les "outils faits main" pour le portage de Méso-NH sur GPU, comme le rajout automatique d'appel à des fonctions mathématiques bit-reproductibles, le rajout automatique des appels aux routines MPPDB_CHECK permettant la vérification "au fil de l'eau" de l'identité des calculs sur GPU et CPU, la gestion optimisée de la mémoire pour les tableaux, l'expansion de l'*array syntax* en boucle imbriquée, etc.

  La version MNH-5-6 + PHYEX sur GPU est en bonne voie, et devrait être intégrée "rapidement" (avant la fin d'année ;-) ) dans la toute dernière version MNH-5-7-X. Il sera ensuite question de porter d'autres parties du code, comme par exemple la convection peu profonde et LIMA.

.. note::

   Une version longue du début de l'entretien, avec plus de détails, est disponible `ici <https://mesonh-newsletters.readthedocs.io/en/latest/community/newsletters/newsletter_03_extended.html>`_.

   Un article en discussion dans *Geosci. Model Dev.* relate le travail de portage de Méso-NH sur GPU, il est disponible `en ligne ici <https://doi.org/10.5194/egusphere-2024-2879>`_ et en pdf : `Escobar et al. (2024) <https://egusphere.copernicus.org/preprints/2024/egusphere-2024-2879/egusphere-2024-2879.pdf>`_.

   Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par `mail <mailto:thibaut.dauhut@univ-tlse3.fr>`_.

    
    
Les nouvelles de l’équipe support
************************************

Version 5.7.1 (sortie le 4 septembre)
  - Liste des bugfixs et principaux nouveaux développements `ici <http://mesonh.aero.obs-mip.fr/mesonh57/Download?action=AttachFile&do=view&target=WHY_BUGFIX_571.pdf>`_
  - Notez que tous les cas tests (namelists et scripts de lancement) sont à présent historisés et se trouvent dans MY_RUN/INTEGRATION_CASES

Version 5.8
  Un appel à contribution sera lancée en décembre. Toutes les contributions prêtes pour décembre 2024, c’est-à-dire testées et livrées avec un (nouveau) cas test, seront prises pour intégration.

Développements en cours et récents
  - Chimie/aérosols : le projet ACCALMIE continue de restructurer la chimie et les aérosols dans les modèles de Météo-France (ARPEGE, MOCAGE, AROME, MESO-NH) pour externaliser la chimie et les aérosols. La bibliothèque ACLIB (Aerosols and Chemistry LIBrary) est en cours de montage. Les routines impactées seront nombreuses notamment à l’intérieur de ch_monitorn.f90, les ch_* et tous les *aer*.
  - Version 6.0 : le développement de la prochaine version majeure a commencé par la montée de version de la branche GPU (MNH-55X-dev-OPENACC-FFT) phasée sur la 5.6 dans un premier temps sans PHYEX. Cette nouvelle branche MNH-56X-dev-OPENACC-FFT-unlessPHYEX tourne sur GPU sur quelques tests. Des tests de performance sur les architectures avec GPU (AMD et Nvidia) ont été réalisés, mais cette branche n’a pas encore été validée sur CPU. Les directives OpenACC sont en cours de portage (manuel) dans PHYEX. La turbulence a été portée. A présent c'est au tour de ICE3. La branche compile sur Belenos !
  - Outils : ajouts de fonctionnalités dans la librairie `Python Fortran Tool <https://github.com/UMR-CNRM/pyft>`_ pour gérer automatiquement certaines transformations du code source de Méso-NH dans le but de produire du code qui tourne sur GPU.
  - Forge logicielle : l'hébergeur de dépôt git koda.cnrs a été testé. Migration le 15 octobre. Les branches sur MNH-ladev seront supprimées sauf si une demande contraire est envoyée au support pour une branche particulière.
  - Site vitrine : démarches identifiées pour le nom de domaine et l'hébergement.
  - Couplage : compilation parallèle de Meso-NH débuggée quand on active OASISAUTO.

Ménage des fichiers en sortie
  - les fichiers .des inutiles (car vides) ne seront plus écrits. Ça concerne principalement les fichiers PGD et issus de DIAG.
  - les fichiers de statistiques détaillées des performances du solveur de pression ne sont plus écrits. Si besoin, il suffit de changer le parameter GFULLSTAT_PRESS_SLV dans modeln.f90 pour les regénérer.
  - le fichier file_for_xtransfer a également disparu (ainsi que quelques morceaux de code devenus inutiles).
  - le fichier OUTPUT_LISTING0 est conservé sauf s'il est vide (Méso-NH le détruit automatiquement à la fin ; il continuera d'exister pendant l'exécution et en cas de plantage). Cela concerne essentiellement l'exécutable MESONH et si des sorties complémentaires dans ce fichier ne sont pas faites (il y en a dans quelques endroits du code).

Stage Méso-NH
  - Le prochain stage aura lieu du 12 au 15 novembre 2024. Planning `ici <http://mesonh.aero.obs-mip.fr/mesonh57/MesonhTutorial>`_
  - Date limite d'inscription : 1er novembre
  - Inscription par mail à `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_

.. note::
  Si vous avez des besoins, idées, améliorations à apporter, bugs à corriger ou suggestions concernant les entrées/sorties, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ est preneur.


Dernières publications utilisant Méso-NH
****************************************************************************************

Fire meteorology
  - A case study of the possible meteorological causes of unexpected fire behavior in the Pantanal Wetland, Brazil [`Couto et al., 2024 <https://doi.org/10.3390/earth5030028>`_]
  - The Role of atmospheric circulation in favouring forest fires in the extreme southern Portugal [`Purificação et al., 2024 <https://doi.org/10.3390/su16166985>`_]

Microphysics
  - Improving supercooled liquid water representation in the microphysical scheme ICE3 [`Dupont et al., 2024 <http://dx.doi.org/10.1002/qj.4806>`_]
  - Importance of CCN activation for fog forecasting and its representation in the two-moment microphysical scheme LIMA [`Vié et al., 2024 <https://doi.org/10.1002/qj.4812>`_]

Model development
  - Porting the Meso-NH atmospheric model on different GPU architectures for the next generation of supercomputers (version MESONH-v55-OpenACC) [`Escobar et al., in discussion <https://doi.org/10.5194/egusphere-2024-2879>`_]

Radiation
  - How to observe the small-scale spatial distribution of surface solar irradiance [`He et al., in discussion <https://doi.org/10.5194/egusphere-2024-1064>`_]

Thermodynamics over complex terrain and in urban environment
  - Thermodynamic processes driving thermal circulations on slopes: Modeling anabatic and katabatic flows on Reunion Island [`El Gdachi et al., 2024 <https://doi.org/10.1029/2023JD040431>`_]
  - Energy and environmental impacts of air-to-air heat pumps in a mid-latitude city [`Meyer et al., 2024 <https://doi.org/10.1038/s41467-024-49836-3>`_]


.. note::

   Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations *disponibles en ligne*), n’hésitez pas à m’écrire. A l’occasion de la mise en place de ces infolettres, je suis également preneur de vos avis sur le format proposé.

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut Dauhut et toute l’équipe Méso-NH : Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
