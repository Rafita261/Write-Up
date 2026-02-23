# Shadow preview

Pour ce challenge web, on a comme contexte ceci

*Une équipe produit a déployé en urgence un nouveau module “URL Preview” pour améliorer l’expérience utilisateur. Depuis, des comportements étranges ont été observés dans les journaux d’accès, sans qu’aucune compromission évidente ne soit détectée.*

Il a donné le lien github du projet et le lien du site deployé.

## Premier instinct - Test

Après une lecture de cette description et après avoir vu beaucoup de code(que je n'ai pas encore lu XD), j'ai décidé de faire de test pour essayer de visualiser localhost avec `http://localhost` dans le url-preview. Mais le message d'erreur  `{"error":"host not allowed"}` m'a fait pensé directement aux listes des host restreints. J'ai donc, testé `127.0.0.1` et avec le même message d'erreur. Mais par contre `0.0.0.0` ne donne pas le même message d'erreur. Son message d'erreur était `502 Bad Gateway`, donc peut-être que le port n'est pas bien précisé. J'ai testé des ports les plus utilisés par les devs comme 8080, 3000 ou 5000, 5173 mais rien.

## Retour à la lecture du code

Je vois que le web express écoute sur le port 8080 et que le filtre que j'ai vu au test était confirmé avec interdiction de `localhost`, `internal` et `127.0.0.1`.

Dans le dossier `internal`, j'ai vu une application **flask** qui expose un endpoint `/flag` accessible seulement en local mais il ne précise pas le port et j'ai recherché le port par défaut utilisé par flask mais c'était le 5000 que j'ai déjà testé avec un `Bad getway`.

## Déclic de la description

La description parlait volontairement d'un deployement en urgence. J'ai donc regardé le repo github pour trouvé des fichiers de deploiement si le port est précisé la-bas.

À la fin du fichier `Dockerfile`, il y a cette ligne qui lance le fichier entrypoint.sh

```Dockerfile
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/usr/bin/tini","--","/entrypoint.sh"]
```

Dans la fonction start_internal (pour lancer l'application flask), le script a précisé le port 9000

```sh
start_internal() {
  cd /app/internal
  BIND_ADDR="0.0.0.0"
  if [ "$SERVICE" = "all" ]; then
    BIND_ADDR="127.0.0.1"
  fi
  /venv/bin/gunicorn \
    -w 2 \
    -b "${BIND_ADDR}:9000" \
    app:app \
    --access-logfile - \
    --error-logfile - &
  INTERNAL_PID="$!"
}
```

Donc, à la fin notre payload est ```http://0.0.0.0:9000/flag```

Et le résultat a donné ceci avec le flag dans le snippet :

```json
{
  "requested_url": "http://0.0.0.0:9000/flag",
  "final_url": "http://0.0.0.0:9000/flag",
  "status": 200,
  "content_type": "text/plain; charset=utf-8",
  "title": "",
  "description": "",
  "snippet": "CCOI26{le_vrai_flag_est_dans_l'application_shadow_preview}\n"
}
```