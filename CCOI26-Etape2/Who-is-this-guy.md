# Who is This Guy

Ce challenge est un challenge OSINT pour faire une enquête à propos d'un gars dont on ne connait que le pseudo : **d3dpanda**

## Premier tentative - sherlock

Mon premier instinct était de trouver les réseaux sociaux au nom de **d3dpanda** avec sherlock.

```bash
sherlock d3dpanda
```

Le résultat affichait ceci

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

J'ai foillé tous ces liens sauf le gnome.org et eyeem.com et j'ai vu que le détenteur de ce pseudo était **Clément Lagier** (Un des organisateurs de CCOI). La plupart de ces comptes sont déjà créé genre plus de 2 ans. Je n'ai remarqué rien de bizarre.

## Fausses pistes

Puis je souviens que ce gars était dans le discord (avec un pseudo deadpanda) et avec un pseudo d3dpanda dans le discord utilisé de l'étape 1. J'ai fouillé son profil, de github, à LinkdIn, jusqu'à son Spotify XD. Mais rien.

## Hint

Après quelques instants (j'ai arrêté), il y avait un hint disant ***sherlock is your friend***. J'ai donc retourné aux résultats de sherlock, fouillant muticuleusement tout, et dans la chaine **replit**, j'ai vu un Website (que j'ai déjà vu avant mais j'attendais à son portfolio mais quand j'ai vu le lien youtube, j'ai quitté directement). Puis quand j'ai recliqué le lien, dans le titre de de la page (vu en haut du navigateur), j'ai vu le fameux **CCOI26**, et bingo.