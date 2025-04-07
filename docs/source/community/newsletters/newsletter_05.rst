Infolettre #05
================================================

**7 avril 2025.** Version française, English version `here <newsletter_05_english.html>`_.


Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici ci-dessous la 5ème infolettre de notre communauté. Vous y trouverez un entretien avec la développeuse d'un outil utile aux utilisateurs.trices de Méso-NH, les dernières nouvelles de l’équipe support et la liste des dernières publications utilisant Méso-NH.

Entretien avec `Najda Villefranque <mailto:najda.villefranque@meteo.fr>`_ (CNRM)
************************************************************************************

.. image:: photo_nv.jpg
  :width: 250


.. note::

   Une version longue de l'entretien, avec plus de détails, est disponible `ici <https://mesonh-beta-test-guide.readthedocs.io/en/latest/community/newsletters/newsletter_05_extended.html>`_.


Najda, tu as développé un simulateur de rayonnement 3D qui peut être appliqué à des champs 3D issus de simulations Méso-NH. Pourrais-tu résumer ce que fait ce logiciel ?
  `htrdr <https://www.meso-star.com/projects/htrdr/htrdr.html>`_ est un logiciel libre basé sur les méthodes de Monte Carlo qui simule la trajectoire d'un grand nombre de photons pour résoudre le transfert radiatif atmosphérique. Il prend en entrée une description 3D de l'atmosphère, typiquement des sorties de simulations Meso-NH, ainsi qu'une description des surfaces (par exemple d'une ville comme `ici <https://web.lmd.jussieu.fr/~nvillefranque/pages/teapot_city>`_ mais on peut évidemment aussi prescrire une surface plane). Les photons interagissent avec les hydrométéores, les aérosols, les gaz et les surfaces au travers de phénomènes d'absorption / émission et de diffusion / réflexion. La répartition des photons sur les capteurs virtuels positionnés dans la scène par l'utilisatrice donne une estimation de l'intensité du rayonnement sur ces capteurs.

Pour quelles applications conseilles-tu d'utiliser htrdr ?
  htrdr est un logiciel de calcul de référence idéal pour déterminer précisément les grandeurs radiatives, qu'elles soient solaires (SW) ou thermiques (LW), sous forme de cartes ou intégrées horizontalement. Il est particulièrement utile pour estimer l'effet radiatif des nuages ou des aérosols, étudier leur impact en fonction de leurs propriétés, et évaluer les paramétrisations des modèles de grande échelle. htrdr sert également de cible pour calibrer ces paramétrisations dans des cas idéalisés, là où les observations manquent. En somme, htrdr est au rayonnement ce que Meso-NH est à la dynamique atmosphérique ! 

  htrdr permet également de produire des images de synthèse, utilisant des techniques similaires à celles de Disney pour leurs films. Ces simulations permettent de visualiser un champ 3D de Méso-NH sous forme de photo ou de film, ou encore de synthétiser des images de type satellite, utiles pour concevoir ou évaluer des algorithmes d'inversion.

Pourquoi vaut-il mieux utiliser htrdr plutôt que d'autres programmes équivalents ? 
  Les avantages des méthodes de Monte Carlo, comme celles utilisées par htrdr, sont leur précision (prise en compte de processus détaillés, absence de biais), leur flexibilité (notamment sur les domaines d'intégration spatiaux et spectraux) et leur insensibilité théorique à la complexité des scènes virtuelles dans lesquelles les photons se propagent. Contrairement à d'autres codes, htrdr utilise des outils issus de la recherche en informatique graphique, rendant le temps de calcul indépendant de la quantité de données à traiter, et étendus aux données volumiques par |villefranque_etal|. Cela signifie qu'une simulation à haute résolution au-dessus d'un relief détaillé ne prendra pas plus de temps qu'une simulation à basse résolution sur un sol plat. De plus, htrdr est un logiciel en développement continu, utilisé et enrichi par diverses communautés, ce qui en fait un outil vivant et évolutif.

Quelles recommandations ferais-tu aux utilisateurs.trices ?
    Je leur recommanderais de ne pas hésiter à me contacter (ou Robert Schoetter qui est aussi un expert de htrdr !) si ils et elles souhaitent prendre en main ce logiciel. Bien que de la `documentation <https://www.meso-star.com/projects/htrdr/man/man1/htrdr-atmosphere.1.html>`_ et du `matériel de formation <https://mattermost.lmd.ipsl.fr/g3t-rayonnement/channels/htrdr>`_ soient disponibles en ligne, une discussion préalable permet de s'assurer que htrdr répond bien aux besoins spécifiques. Elle peut également aider à choisir les configurations et options appropriées, et guider les utilisateurs pour les entrées et sorties. C'est aussi une occasion de discuter de physique du rayonnement, même si la question scientifique n'est pas directement liée au transfert radiatif. N'hésitez pas, parce qu'en vrai, le rayonnement, c'est vachement plus fun que ce qu'on pense (rire) !

Quelles sont les limites ?
  Le principal inconvénient d'htrdr est le temps de calcul, qui peut être limitant en présence de nuages très diffusants. Bien que cela ne soit pas techniquement limitant pour les utilisateurs.trices habitué.es à de grosses simulations, il est important de noter que cela consomme beaucoup d'énergie. Pour des réglages initiaux, il est conseillé de lancer des simulations avec un petit nombre de photons pour obtenir une première estimation, même si elle est bruitée. Il existe également des limites dans la modélisation physique du transfert, comme l'approximation de la fonction de phase de diffusion des hydrométéores et aérosols. Cependant, ces limites peuvent être surmontées avec du temps et des recherches supplémentaires. En résumé, htrdr est un outil puissant et évolutif, capable de répondre à de nouveaux besoins grâce à des développements futurs. Pour finir, je noterais qu'à ce jour htrdr est un logiciel qui sert essentiellement de diagnostic, à utiliser *hors ligne* sur des sorties de simulations réalisées par ailleurs avec ou sans rayonnement. Une des perspectives de mes recherches est de coupler htrdr à Meso-NH pour pouvoir faire des simulations atmosphériques incluant une résolution 3D du rayonnement.

.. |villefranque_etal| raw:: html

   <a href="https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2018MS001602" target="_blank">Villefranque et al. (2019)</a>

.. note::

   Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par `mail <mailto:thibaut.dauhut@univ-tlse3.fr>`_.

    
    
Les nouvelles de l’équipe support
************************************

Version 6
  - La préparation de la version 6 est toujours en cours, la branche de travail MNH-60-branch a été déclarée.
  - La librairie ACLIB (chimie et aérosols externalisés) continue son développement pour intégration dans la version 6. Une première version d'ACLIB sera intégrée ``MNH-60-branch`` courant mai.
  - La version portée sur GPU de PHYEX au sein de Méso-NH a été reportée dans le dépot PHYEX pour intégration dans le cycle 50 d'IAL (IFS-ARPEGE-LAM). La dernière version de PHYEX sera reportée dans ``MNH-60-branch`` courant mai-juin. Ainsi la physique du cycle 50 d'AROME sera très proche de celle de la version 6 de Méso-NH.
  - Une bibliothèque de compression sans pertes plus performante, principalement en coût CPU et un peu en taux de compression, Zstandard est en cours d'intégration.
  - A venir, un programme de ménage interne avec restructuration des répertoires des sources et gros élagage de parties du dépôt qui ne sont plus maintenues et/ou d'actualité.

En attendant, une version 5.7.2 va sortir prochainement
  - Pour les sorties fréquentes *output* : 
  - possibilité de faire du filtrage par seuil en retirant ou en mettant une valeur particulière aux éléments d'une variable qui sont plus petits, plus grands ou en dehors d'une plage, critères qui peuvent être en valeur absolue ou pas.
  - possibilité d'arrondir les valeurs d'une variable à un multiple d'une valeur choisie (par exemple tout arrondir à un multiple de 0.1). Associé à de la compression, il s'agit d'une forme de compression avec pertes.


Autres développements en cours et récents
  - Les sites internet (site vitrine + nouveau site de documentation) continuent d'être développés
  - La prise en compte des courants de surface océanique dans le schéma de turbulence de Méso-NH a été validée

Dépôt Zenodo de Méso-NH
  Afin d'avoir un DOI associé à chaque nouvelle version de Méso-NH, un `dépôt Zenodo <https://zenodo.org/records/15095131>`_ vient d'être créé. Pour chaque version de Méso-NH, la tar ball y sera déposée et un DOI y sera associé. 

.. note::
  **Pensez-y pour vos futures publications**, vous pourrez faire référence à ce dépôt notamment dans la section *Data availability* ou équivalente. 

Stage Méso-NH
  Le stage Méso-NH s'est déroulé avec succès pour la première fois en hybride, avec 8 personnes dans la salle et 13 à distance, du 10 au 13 mars 2025.

.. note::
  Si vous avez des besoins, idées, améliorations à apporter, bugs à corriger ou suggestions concernant les entrées/sorties, `Philippe Wautelet <mailto:philippe.wautelet@cnrs.fr>`_ est preneur.


Dernières publications utilisant Méso-NH
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
  - Analyse globale, régionale et locale des mesures de vapeur d'eau dans la haute TTL pendant STRATÉOLE 2 [`Carbone <https://theses.fr/s297336>`_, Université de Reims Champagne-Ardenne, 2025]

.. note::

   Si vous souhaitez partager avec la communauté le fait qu’un de vos projets utilisant Méso-NH a été financé ou toute autre communication sur vos travaux (notamment posters et présentations *disponibles en ligne*), n’hésitez pas à `m’écrire <mailto:thibaut.dauhut@univ-tlse3.fr>`_.

Bonnes simulations avec Méso-NH !

A bientôt,

Thibaut Dauhut et toute l’équipe Méso-NH : Philippe Wautelet, Quentin Rodier, Didier Ricard, Joris Pianezze, Juan Escobar et Jean-Pierre Chaboureau
