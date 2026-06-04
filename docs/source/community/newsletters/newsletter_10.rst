Infolettre #10
================================================

**2 juillet 2026.** Version française, English version `here <newsletter_10_english.html>`_.


Chers utilisateurs, chères utilisatrices de Méso-NH,

Voici ci-dessous la 10ème infolettre de notre communauté. Vous y trouverez un entretien avec le développeur d'un outil qui peut vous être bien utile, les dernières nouvelles de l’équipe support et la liste des dernières publications et thèses utilisant Méso-NH.

Entretien avec `Clément Soufflet <mailto:clement.soufflet@univ-reunion.fr>`_ (LACy)
******************************************************************************************************************

|pic1|

.. |pic1| image:: photo_cs.png
  :width: 400

Clément, tu as développé FrameIt pour faciliter l’analyse de simulations de cyclones. FrameIt est particulièrement adapté pour les utilisateur.ices de Méso-NH. Pourrais-tu résumer ce que fait cet outil ?
  Dans sa première version **FrameIt** est orienté comme un outil de gestion de données de sortie modèle entièrement codé en Python et basé sur la librairie *xarray*. L'idée du développement de **FrameIt** vient de l’analyse de simulations de cyclone pour lesquelles un grand domaine est nécessaire mais seul une petite partie (le cyclone) nous intéresse vraiment.

  **FrameIt** permet d’extraire un sous domaine mobile (ou fixe) dans le temps, centré sur un objet météorologique (par exemple un cyclone), tout en sélectionnant les variables et les niveaux verticaux d’intérêt. Ce sous domaine est aussi disponible en coordonnée polaire centré sur l’objet étudié. En sortie on obtient une série de fichiers *netcdf* très légers, triés par système de coordonnées et par dimension, sur le sous-domaine voulu, contenant tous les pas de temps de la simulation.

  À noter que **FrameIt** peut aussi bien traiter des fichiers *netcdf* venant de Méso-NH que des fichiers *grib* issue du modèle AROME.

En quoi est-ce que cela facilite la vie aux modélisateur.ices ?
  **FrameIt** ne remplace pas l’analyse des fichiers de sortie modèle, mais il permet une prise en main rapide de ces fichiers en s’affranchissant des problématiques de gestion de donnée ou de traitement de gros fichiers de données. En d’autres termes c’est un outil qui va faciliter l’analyse en réduisant la taille des fichiers et en les normalisant par rapport à l’objet météorologique étudié.

  En plus de ça, **FrameIt** permet l’analyse des données dans un système de coordonnées polaires, utile pour les objets météorologiques à symétrie azimutale.

  Enfin, les sorties étant des fichiers *netcdf* relativement légers, **FrameIt** contribue à faciliter le partage de données de simulations notamment dans le cadre de collaboration avec des chercheurs non-modélisateur (et contribuera ainsi au rayonnement de Méso-NH).

Y a-t-il d'autres situations, non-cycloniques, pour lesquels FrameIt serait également utile ?
  Pour le moment **FrameIt** est utilisé majoritairement pour des simulations de cyclones mais cet outil n’est pas limité à ça. Selon moi, **FrameIt** possède un vrai intérêt pour les cas où le domaine d’une simulation numérique est bien plus grand que l’objet météorologique étudié, mais aussi les cas où un système de coordonnées polaires permet de faciliter l’analyse de l’objet météorologique en question.

Quelles recommandations ferais-tu aux utilisateurs.rices de Méso-NH qui voudrait commencer à l’utiliser ?
  Évidemment je ne peux que recommander, en premier lieu, d'aller voir la documentation, assez fournie, associée au projet sur le `GitHub de Météo France <https://meteofrance.github.io/>`_. L'installation et l'utilisation de **FrameIt** nécessite un environnement virtuel conda spécifique dont l'installation est décrite dans cette documentation.

  Ensuite il faut s'approprier la syntaxe de l’unique fichier de configuration de l’outil dont un exemple est disponible dans le projet. Et enfin, bien sûr, commencer par un cas simple que vous connaissez déjà, avec peu de fichiers, pour vous faire la main.

Quelles sont les limites actuelles ? As-tu des perspectives de développements futurs ? 
  Un aspect de **FrameIt** qui peut, au premier abord, paraître limitant c’est l’orientation vers l’étude des cyclones. En effet la méthode de suivi d’objet est calibrée pour les cyclones tropicaux et n’est pas applicable à tous les autres objets météorologiques. Cependant pour palier cette limitation, l’utilisateur.ice à la possibilité de fournir une trajectoire a priori pour guider l’extraction sur l’objet météorologique de son choix. Ceci étant dit, **FrameIt** a été pensé modulable (Git) notamment pour permettre aux utilisateur.ices intéressé.es de développer à leur tour des fonctionnalités utiles pour la communauté. Il est donc tout à fait envisageable d’implémenter une nouvelle méthode de suivi associée à un autre type d’objet météorologique (cellule convective, orage…), une rubrique est d’ailleurs dédiée à l’implémentation de nouvelles méthode de suivie dans la documentation.

  Ensuite cette première version va servir de base à de nombreux développement futurs, notamment l’implémentation dans l’étape diagnostic de Méso-NH de post-traitements dédiés aux cyclones tropicaux. 

  Enfin, comme un des axes de recherche au LACy porte sur les interactions océan-atmosphère, l’idée est de pouvoir à terme appliquer cet outil sur les sorties de modèle d’océan et de vagues issue de simulations couplées afin d’avoir une vue d’ensemble des interactions de ces trois compartiments.


.. note::

  Si vous aussi vous souhaitez expliquer un développement que vous avez mis en place dans Méso-NH, ou une méthode d’analyse que vous partagez à la communauté, n’hésitez pas à me le signaler par `mail <mailto:thibaut.dauhut@utoulouse.fr>`_.
