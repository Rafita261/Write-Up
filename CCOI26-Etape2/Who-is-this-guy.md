# Who is This Guy

Ce challenge est un challenge OSINT pour mener une enquête à propos d'un gars dont on ne connaît que le pseudo : **d3dpanda**

## Première tentative - sherlock

Mon premier instinct était de trouver les réseaux sociaux au nom de **d3dpanda** avec Sherlock.

```bash
sherlock d3dpanda
```

Le résultat affichait ceci :

```plaintext
https://www.codewars.com/users/d3dpanda
https://www.eyeem.com/u/d3dpanda
https://www.freelancer.com/u/d3dpanda
https://gitlab.gnome.org/d3dpanda
https://hackerone.com/d3dpanda
https://lemmy.world/u/d3dpanda
https://linktr.ee/d3dpanda
https://replit.com/@d3dpanda
https://www.twitch.tv/d3dpanda
https://data.typeracer.com/pit/profile?user=d3dpanda
https://www.interpals.net/d3dpanda
Total Websites Username Detected On : 11
```

## Fouilles

J'ai fouillé tous ces liens sauf gnome.org et eyeem.com, et j'ai vu que le détenteur de ce pseudo était **Clément Lagier** (un des organisateurs de CCOI). La plupart de ces comptes ont été créés il y a plus de 2 ans. Je n'ai rien remarqué de bizarre.

## Fausses pistes

Puis je me suis souvenu que ce gars était sur Discord (avec le pseudo deadpanda) et aussi avec le pseudo d3dpanda sur le Discord utilisé à l'étape 1. J'ai fouillé son profil, de GitHub à LinkedIn, jusqu'à son Spotify XD. Mais rien.

## Hint

Après quelques instants (j'avais arrêté), il y avait un hint disant ***sherlock is your friend***. Je suis donc retourné aux résultats de Sherlock, en fouillant minutieusement tout. Dans **Replit**, j'ai vu un website (que j'avais déjà vu avant, mais je m'attendais à un portfolio ; quand j'ai vu le lien YouTube, j'ai quitté directement). Puis, quand j'ai recliqué sur le lien, dans le titre de la page (visible en haut du navigateur), j'ai vu le fameux **CCOI26**, et bingo.