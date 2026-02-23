# Follow format

Ce challenge disait qu'ils ont reçu un fichier .img et qu'on devait retrouver un fichier supprimé.

## Analyse du fichier

Pour être sûr du format du fichier (ce que j'ai pensé en premier), j'ai regardé son type.

```bash
file challenge.img
```

Le résultat était :

challenge.img: Linux rev 1.0 ext4 filesystem data, UUID=ae6148e4-8e10-49db-a77b-9c52b2529b02, volume name "WORK_DRIVE" (needs journal recovery) (extents) (64bit) (large files) (huge files)

## Montage et fouilles (fausses pistes)

J'ai monté le fichier puis fouillé un peu partout. J'ai essayé de trouver des partitions supprimées, mais rien. Il n'y avait que des fichiers vides.

## Titre et déclic

Après, j'ai revu le titre. J'ai tout de suite pensé au format du flag. Et je me suis dit : pourquoi ne pas tenter un simple **strings** ? Ça ne prend qu'une seconde. Et bingo, le flag apparaît avec le **flag format** : **CCOI26**.

```bash
strings challenge.img | grep CCOI26
```

Résultat :

CCOI26{le_vrai_flag_est_ici_pour_le_vrai_fichier}