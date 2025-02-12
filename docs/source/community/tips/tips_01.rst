Tips #01
================================================

**12 February 2025**

Il est possible de compresser des fichiers de sortie Méso-NH au format netCDF4 qui ne l'étaient pas encore. L'utilité de cette opération est de réduire l'espace disque occupé par vos simulations.

L'opération se fait simplement avec la commande :

.. code-block:: bash

   nccopy -d4 -s fich_noncompresse.nc4 fich_compress.nc4

Le -d4 pour dire de compresser avec un niveau 4 de compression (0: pas de compression, 9 maximum). 4 ou 5 devrait généralement suffire. Au-delà, on ne gagne généralement pas grand chose (voir rien) et çà allonge pas mal la durée de l'opération. Le -s est pour activer le "shuffle". Je vous le conseille fortement.

Attention, les gains sont très variables d'un fichier à l'autre (de quelques pourcents à un facteur 2 voire plus) et le traitement est un peu long. Si vous avez pas mal de fichiers, écrire un petit script shell pour boucler sur la liste de fichiers est une bonne idée. Petit exemple à adapter :

.. code-block:: bash

  > cat compr.sh  
  #!/bin/bash
  
  dir_in="Sorties_non_compressees"
  dir_out="Sorties_compressees"
  
  for file in $dir_in/*.nc; do
     echo "Treating: $file"
     base_file=$(basename "$file")
     nccopy -d4 -s ${file} ${dir_out}/${base_file}
  done

Philippe
