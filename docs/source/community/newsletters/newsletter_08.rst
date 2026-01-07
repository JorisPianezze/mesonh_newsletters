Infolettre #08
================================================

**8 janvier 2025.** Version française, English version `here <newsletter_08_english.html>`_.


Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici ci-dessous la 8ème infolettre de notre communauté. Vous y trouverez un entretien avec deux dévelopeur.euses de Méso-NH, les dernières nouvelles de l’équipe support et la liste des (nombreuses !) dernières publications et thèses utilisant Méso-NH.

Entretien avec `Quentin Libois <quentin.libois@meteo.fr>`_ et Sophia Schaefer (CNRM)
*************************************************************************************************************************************

|pic1|     |pic2|

.. |pic1| image:: photo_ql_bw.png
  :width: 250

.. |pic2| image:: photo_ss.jpg
  :width: 230

Quentin, tu as inclus en 2017 la possibilité d'utiliser le code ecRad pour la représentation du rayonnement dans Méso-NH, et Sophia, tu as contribué à mettre à jour la nouvelle version d'ecRad dans la prochaine version de Méso-NH avec Quentin Rodier. Pourriez-vous résumer ce que permet l'utilisation de ce code ?
  ecRad est le code de transfert radiatif utilisé dans IFS (Integrated Forecasting System, le modèle atmosphérique global de l’ECMWF, le Centre Européen pour les Prévisions Météorologiques à Moyen Terme). C'est avant tout un code à l'état-de-l'art qui permet d'utiliser des développements récents tant pour la résolution de l'équation du transfert radiatif (prise en compte des hétérogénéités sous-maille des nuages, paramétrisation des effets 3D sous-maille) que pour les propriétés optiques des aérosols et des nuages. A l'origine il s'appuyait sur les propriétés optiques des gaz telles que fournies par RRTM (Rapid Radiative Transfer Model) mais, depuis la version 1.6, il propose de nouvelles propriétés via l'option ecckd qui le rend particulièrement rapide. Son aspect modulaire le rend facile d'accès, tant pour son utilisation que pour contribuer à son développement. La conception modulaire permet de faire varier individuellement certaines parties du code, de tester l’impact de ces modifications et d'optimiser les réglages pour différentes applications.

Pourquoi vaut-il mieux utiliser ecRad qu’une autre représentation du rayonnement (par ex. celle nommée ECMWF dans Méso-NH) ?
  ecRad dispose d'un grand nombre d'options (hypothèse d'hétérogénéité sous-maille des nuages, recouvrement vertical des fractions nuageuses, propriétés optiques des gaz, aérosols et hydrométéores, etc.) qui permettent d'explorer simplement la sensibilité du transfert radiatif à ces options ou de prendre en compte des processus qui ne pouvaient pas l'être auparavant (prise en compte de la diffusion pour le rayonnement infrarouge thermique, effets 3D, prise en compte de différents types d'hydrométéores liquides et glacés). Aujourd'hui il est aussi plus rapide et plus précis que l'option historique nommée ECMWF dans Méso-NH. De manière générale ecRad peut faire tout ce que proposait l'option historique, mais bien plus encore, et le code est également plus facile à utiliser et à modifier.

Dans quel cas ecRad est-il recommandé ?
  Il n'y a a priori aucun cas où ecRad n'est pas recommandé. En fonction des applications, différentes options d'ecRad peuvent être recommandées (par exemple, différents solveurs de rayonnement).

Quelles recommandations feriez-vous aux utilisateur.ices ?
  Le rayonnement reste une composante assez mal connue des utilisateur.ices de Méso-NH. Pourtant une simulation peut être très sensible au choix des options de rayonnement. Nous encourageons les utilisateur.ices à estimer la sensibilité de leurs simulations à des paramètres clés tels que l'hypothèse de recouvrement vertical ou l'hétérogénéité sous-maille. Noter aussi que l'estimation du rayon effectif des hydrométéores peut fortement impacter les propriétés optiques d'un nuage, et en conséquence le bilan d'énergie et le cycle de vie d'un nuage. Pour le solveur de rayonnement, l'option par défaut est actuellement McICA, mais pour les cas LES ou à colonne unique, Tripleclouds est une meilleure option (puisqu'elle n'introduit pas de bruit stochastique).

Quelles sont les limites ? Dans quel cas cette option est-elle plutôt à éviter ?
  ecRad demeure un modèle deux-flux basé sur l'hypothèse plan-parallèle et suppose donc des colonnes indépendantes. En conséquence ses performances demeurent limitées pour des configurations où le soleil est bas sur l'horizon, et à haute résolution spatiale lorsque les transferts radiatifs horizontaux entre colonnes adjacentes deviennent importants. Une option pour traiter ces transferts inter-colonnes pourrait être incluse à l'avenir. Malgré cette limite, ecRad n'en demeure pas moins la meilleure option disponible aujourd'hui dans Méso-NH.


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

   Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations *disponibles en ligne*), n’hésitez pas à m’écrire. A l’occasion de la mise en place de ces infolettres, je suis également preneur de vos avis sur le format proposé.

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut Dauhut et toute l’équipe Méso-NH : Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
