# Oeil du cyclone

C’est un challenge crypto basé sur Shamir Secret Sharing (SSS).

## Comprendre le SSS

En gros, le challenge nous donne exactement 5 points (de coordonnées `(x, y)`) appartenant à la courbe d’un polynôme `f(x)` de degré 5, ainsi que 4 autres points dont la valeur de `x` est complète mais celle de `y` a perdu 20 bits de données.


## But du challenge

Le but du challenge est de retrouver le secret `f(0)` grâce aux informations données.

Mais pour trouver `f(0)` dans notre cas, il nous faut au moins 6 points.

Explication :

Un polynôme de degré 5 peut s’écrire sous la forme :

```
f(x) = a₁x⁵ + a₂x⁴ + a₃x³ + a₄x² + a₅x + a₆
```

En gros, pour trouver les 6 coefficients du polynôme, il faut au minimum 6 équations, donc 6 points de la courbe.

Mais ici, le but n’est pas vraiment de retrouver tous les coefficients. Ce qui nous intéresse uniquement, c’est le coefficient constant :

```
a₆ = f(0)
```

## Retrouver le secret f(0)

Pour retrouver `f(0)` lorsque l’on possède suffisamment de points, on peut utiliser la formule d’interpolation de Lagrange.

C’est une formule mathématique qui permet de reconstruire directement la valeur d’un polynôme en un point donné, sans avoir besoin de calculer explicitement tous ses coefficients.

Dans mon cas, j’ai demandé de l’aide à une IA pour écrire une fonction qui calcule directement l’interpolation de Lagrange en `x = 0`.

```python
def lagrange_at_0(points, p=2**521 - 1):
    f0 = 0
    for i, (xi, yi) in enumerate(points):
        li = 1
        for j, (xj, _) in enumerate(points):
            if i != j:
                num = -xj
                den = xi - xj
                if p:
                    den = pow(den, -1, p)
                    li = (li * num * den) % p
                else:
                    li *= num / den
        if p:
            f0 = (f0 + yi * li) % p
        else:
            f0 += yi * li
    return f0
```


## Scope d’attaque

Puisque nous avons déjà 5 points complets, il nous suffit de compléter un seul point partiel (celui dont 20 bits sont manquants).

Avec seulement 20 bits manquants, on peut brute-forcer toutes les possibilités.  
Cela représente au maximum :

```
2²⁰ = 1 048 576 possibilités
```

Ce qui reste totalement faisable.

On s’arrête dès que l’on retrouve un flag valide :

- il commence par `CCOI26{`
- il se termine par `}`
- les 16 premiers caractères hexadécimaux de son SHA256 sont `f687cb74fdcefefc`

Dans mon cas, j’ai choisi le point avec `x = 6`.


## Brute-force

Après avoir reconstruit l’idée mathématique, j’ai lancé la brute-force.

Au début, je vérifiais seulement le début et la fin du flag.  
Mais il y avait plusieurs candidats qui respectaient ce format.

J’ai donc ajouté une vérification avec la signature SHA256 pour être sûr d’obtenir le bon flag.

Ce script appelle la fonction vue précédemment pour calculer `f(0)` à chaque tentative de valeur pour `y = f(6)`.

```python
from hashlib import sha256

def simple_decrypt(int_val):
    hex_string = hex(int_val)[2:]
    hex_bytes = bytes.fromhex(hex_string)
    return hex_bytes


points = [(1,0xd0393fd5aa76c02f53757a5883d97a0f0ade112cffc590c8378f2b5a6696a284dcc1ef10c29f7275958952bca3c40922f75258f47e808d587aca867f48f0d798f5), 
          (2,0xa0deb8650c459c78e99ca5ae29c1399c8221723e6c966a4a4494ec69bcb20399336bba13c10998b4b0b554cffdaec9b8b536e6fa9ea4eefa7321782797b84672e4),
          (3,0x9dc6d639cbda2c6893efafe086027e1f9126a9e27f2d342e45e8090675c2eca7e4ae330b163f8f059fa665a20ea4be41a4de9fe882ac3b08387ba8649622293745),
          (4,0xff3a80c762b7a71ee3793ed87a7951f819960a86b067cefbe94cac78b9f556291ebf42ae21395da1a5e9d3d426624b6cf5bebb4487d9311737417749e401c0cb57),
          (5,0x748755843bdf0733e28882bb8f096fdd4c4ae2142cba5fb2ea4ba7e65a7b007a75f34a4f7a94b4b8e5b9d425d415b5750066cb52e451f11933b086614b816d4ecb)]

partial = (6,0x18e91e304d2372e99ce65481f4a15284c423aa9ac47a25b639109b2c0c5d60cb6ba133679b80d2d34cfdc2c2968c5b83977eaa1b6e5ad7ed0368e3d0a9639300000)

flag_format = b"CCOI26{"
sha1 = "f687cb74fdcefefc"
p = 2**521 - 1

for i in range(2**20):
    px = 6
    py = partial[1] + i

    hex_secret_test = lagrange_at_0(points + [(px, py)], p)
    s = simple_decrypt(hex_secret_test)

    if s.startswith(flag_format) and s.endswith(b'}') and sha256(s).hexdigest()[:16] == sha1:
        print(s)
        break
```

Le résultat de ce script affiche le flag en plaintext :

**CCOI26{le_vrai_flag_affiche_ici_pour_apres_le_lancement_de_ce_script}**