Infolettre #09
================================================

**24 avril 2026.** Version française, English version `here <newsletter_08_english.html>`_.


Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici ci-dessous la 9ème infolettre de notre communauté. Vous y trouverez un entretien avec un membre de l'équipe support de Méso-NH, les dernières nouvelles de l’équipe et la liste des dernières publications et thèses utilisant Méso-NH.

Entretien avec `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_ (CNRM)
*************************************************************************************************************************************

|pic1|

.. |pic1| image:: photo_qr.jpg
  :width: 250

Quentin, après un énorme travail, tu viens de publier la version 6 de Méso-NH. Peux-tu nous expliquer en quoi est-ce qu'elle est révolutionnaire ?
  La version 6.0.0 de Méso-NH est la première version officielle qui intègre le portage du code sur GPU. Depuis plus de 10 ans, le portage sur GPU de Méso-NH est en effet développé par le LAERO sur des branches en parallèle de la branche principale du code pour ne pas impacter les développements classiques. Avec le portage sur GPU de la physique Méso-NH utilisée dans AROME, qui a entraîné une réécriture conséquente de la physique atmosphérique dans le module externalisé PHYEX, la version 6.0.0 a permis de finaliser officiellement le portage GPU de Méso-NH et de consolider le code commun entre la physique Méso-NH et AROME. De nombreux nouveaux développements ont été intégrés, fruits de plusieurs années de recherche et développement de notre communauté. Je vous renvoie vers `la note de version <https://mesonh.readthedocs.io/en/latest/getting_started/releases/release_note_600.html>`_ qui est très riche et je vous encourage à en consulter les parties qui vous intéressent (il y en a forcément au moins une !).

Quels sont les autres changements qui vont améliorer notre quotidien d'utilisateur.ices ?
  Cela dépend des usages particuliers. Pour les utilisateur.ices GPU de Méso-NH, notamment au LAERO, il n’est plus nécessaire d’utiliser une branche en parallèle de la branche master mais directement la version officielle qui contient tous les autres développements disponibles à tous les utilisateurs de Méso-NH. Pour les utilisateur.ices du schéma de rayonnement ecRaD, il n’y a plus besoin de positionner une variable d’environnement spécifique avant de compiler le code (qui était un oubli fréquent et source de frustration). ecRaD est à présent compilé par défaut. Enfin, on peut citer aussi l’activation par défaut de la compression netCDF avec la librairie Zstandard qui permet non seulement de gagner un facteur 2 à 3 sur la taille des fichiers de sortie mais aussi de gagner légèrement en temps de calcul. Le format LFI n’est plus supporté en écriture.

Et pour les développeur.euses, qu'est-ce que cela change ?
  Beaucoup de choses. Sur les parties de code portées sur GPU (advection, solveur de pression, turbulence, microphysique ICE3), les nombreuses directives OpenACC ont été introduites. Chaque modification de code dans ces parties doivent respecter au mieux ces directives afin de ne pas casser les optimisations réalisées. Sur les parties de codes liées aux aérosols et à la chimie, la librairie ACLIB introduit de nouveaux objets et standards de codage afin que le code spécifique à Méso-NH puisse être utilisé dans d’autres modèles et vice-versa. Une restructuration importante du dépôt a également été effectuée : par exemple le dossier  src/LIB/SURCOUCHE a été déplacé dans MNH/io and MNH/parallel ; de nouveaux sous-dossiers dans src/MNH ont fait leur apparition pour mieux ranger les sources nombreuses. Il faut s’y retrouver mais je trouve personnellement que le code et le dépôt sont beaucoup mieux rangés.

Pourquoi recommandes-tu à toutes et tous de passer dès que possible à cette version 6 ?
  Parce que nous avons mis tout notre cœur dedans ! La remarque suivante s’applique à chaque version majeure : si vous êtes en fin de thèse ou en train de finaliser des simulations pour un article scientifique, je ne vous conseille pas de changer de version. Pour tous les autres, foncez ! La version 6 apporte beaucoup de nouvelles fonctionnalités scientifiques également (voir `la note de version <https://mesonh.readthedocs.io/en/latest/getting_started/releases/release_note_600.html>`_). Si vous développez dans Méso-NH, il est indispensable de migrer vers cette nouvelle version au plus vite parce que le code a beaucoup évolué depuis la version 5.7.2. Une fusion tardive de vos développements avec les nouvelles versions (*merge*) sera pénible.

Quelles perspectives à court et moyen termes à présent ?
  La réponse à cette question est moins personnelle et regroupe des réponses d’une partie de l’équipe support. Les perspectives à court et moyen termes sont de poursuivre les tests en simple précision sur l’ensemble des cas tests tout en travaillant sur l'optimisation de certaines parties du code. La mise en place de l'intégration et réalisation de tests en continu (CI/CD) sur le dépôt gitlab permettra d’accélérer les intégrations de développement et de détecter les bugs au plus proche des modifications qui en sont la cause.
  Sur le portage GPU, un travail sur la convection peu profonde et la microphysique LIMA sera réalisée ainsi que la possibilité d’utiliser l’imbrication de domaines (*grid-nesting*). Une réflexion est également en cours sur la stratégie à adopter face au désengagement du support d'OpenACC du constructeur AMD. Toujours à propos de coût de calcul, l'utilisation d'une grille verticale fine impacte très souvent la stabilité numérique du code et impose un pas de temps petit. Pour pallier ce problème, un travail est en cours pour rendre implicite le couplage entre la surface et l'atmosphère. Côté entrées-sorties (I/O), à court terme il est prévu d’implémenter la possibilité d’avoir plusieurs séries temporelles de variables écrites dans les fichiers de sorties fréquentes « OUT »(via NAM_OUTPUT). A moyen terme, il s’agirait de disposer d’une écriture de fichiers en parallèle par plusieurs tâches au lieu d’une seule actuellement. A court terme, nous continuerons également de faire évoluer le contenu des nouveaux sites web en fonction de vos retours. Toutes ces perspectives seront discutées au sein du futur Conseil Scientifique / Comité de Développement de Méso-NH.


.. note::

  Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par `mail <mailto:thibaut.dauhut@utoulouse.fr>`_.

    
    
Les nouvelles de l’équipe support
************************************

Version 6
  - La préparation de la version 6.0.0 est en cours, sa sortie est imminente.
  - La mise à jour de PHYEX dans Méso-NH est en cours. Cela nécessite le phasage des modifications de la version de PHYEX de Méso-NH vers le dépot central de PHYEX. Ensuite, la récupération de cette version sur le dépôt central et son implantation dans Méso-NH permet de récupérer les modifications de PHYEX réalisées par les autres contributeurs (tels que les personnes de la communauté ACCORD ou qui développent dans AROME).
  - L'écriture des fichiers au format LFI n'est plus supportée.
  - Dans les fichiers netCDF, les variables SURFEX sont à présent séparées dans un groupe spécifique.
  - L'appel à RTTOV14 a été intégré pour l'étape de diagnostic et pour le calcul en ligne, l'appel à RTTOV13 a été enlevé.
  - Les dernières optimisations du portage GPU sur différentes machines (cartes GPU AMD MI250X et MI300A sur Adastra) et APU NVIDIA ont été intégrées à la branche ``MNH-60-branch``.
  - La dernière version du code d'éolienne (EOL-v2) et une mise à jour du couplage LIMA-ecRad-aérosols ont été intégrées à la branche ``MNH-60-branch``.
  - Quelques mises à jour de la documentation ont été réalisées et le futur site technique de Méso-NH sera diffusé à l'occasion de la sortie de la version 6.

Développements en cours et récents
  - Des tests de Méso-NH en simple précision sont en cours sur tous les cas tests éligibles. Le passage en simple précision par défaut n'est pas encore acté et sera probablement repoussé à la version 6.1.


Stage Méso-NH
  - Le prochain stage Méso-NH est programmé du 10 au 13 mars 2026.
  - Le stage se déroulera en hybride et en anglais.
  - Envoyez un email à `Quentin Rodier <mailto:quentin.rodier@meteo.fr>`_ pour informations et inscriptions.

.. note::
  Si vous avez des besoins, idées, améliorations à apporter, bugs à corriger ou suggestions concernant les entrées/sorties, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ est toujours preneur.


Dernières publications utilisant Méso-NH
****************************************************************************************

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

   Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations *disponibles en ligne*), n’hésitez pas à `m’écrire <mailto:thibaut.dauhut@utoulouse.fr>`_. Je suis également toujours preneur de vos avis sur les infolettres.

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut Dauhut et toute l’équipe Méso-NH : Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
