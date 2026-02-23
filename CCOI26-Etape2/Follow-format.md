# Follow format

Ce challenge a dit qu'ils ont reçu un fichier .img et on devait retouver un fichier supprimé.

## Analyse du fichier

Pour être sûr sur le format du fichier (que j'ai pensé en premier), j'ai regardé le type du fichier.

```bash
file challenge.md
```

Le résultat était `challenge.img: Linux rev 1.0 ext4 filesystem data, UUID=ae6148e4-8e10-49db-a77b-9c52b2529b02, volume name "WORK_DRIVE" (needs journal recovery) (extents) (64bit) (large files) (huge files)`

## Montage et fouillage (fausses pistes)

J'ai monté le fichier puis fouiller n'importe où. J'ai essayé de trouvé des partitions supprimés mais rien. Il n'y avait que des fichiers vides.

## Titre et déclic

Après, j'ai révu le titre, je pensais directement au format du flag. Et je me suis dit pourquoi pas tenter un simple **strings**, ça ne va prendre qu'une petite seconde et bingo, le flag apparait avec le **flag format** : **CCOI26**.

```bash
strings challenge.img | grep CCOI26
```

Résultat : `CCOI26{le_vrai_flag_est_ici_pour_le_vrai_fichier}`

