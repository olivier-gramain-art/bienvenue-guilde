# La Guilde de Poitiers — site web

Site vitrine de **La Guilde de Poitiers**, association de jeu de rôle (section JDR du Foyer du Porteau) : une page unique qui présente l'association, les infos pratiques et l'adhésion.

En ligne : **https://laguilde-poitiers.com/**

---

## Stack technique

Site **statique** en HTML / CSS / JavaScript **vanilla** — aucun framework, aucune étape de build, aucune dépendance à installer pour faire tourner le site. On ouvre le fichier, ça marche.

> Les outils `sharp` / `png-to-ico` mentionnés plus bas servent **uniquement** à régénérer les images (logo, favicons). Ils ne font PAS partie du site livré.

---

## Structure des fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | La page du site (contenu + métadonnées SEO dans le `<head>`). |
| `style.css` | Toute la mise en forme. La palette de couleurs est centralisée dans les **variables CSS** en haut du fichier (bloc `:root`). |
| `logo_guilde.png` | Logo **complet** (papillon + dé + « La Guilde »). Utilisé pour le JSON-LD et l'image de partage. |
| `logo_guilde_mark.png` | Logo **sans le texte** (papillon + dé seuls). Utilisé dans la barre de navigation et le hero (plus lisible en petit / sur fond sombre). |
| `favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png` | Favicons (icône de l'onglet du navigateur) — le dé « 86 » sur fond crème. |
| `apple-touch-icon.png` | Icône quand on ajoute le site à l'écran d'accueil d'un iPhone/iPad. |
| `og-image.png` | Image d'aperçu (1200×630) affichée quand on partage le lien sur Discord, Facebook, etc. |
| `site.webmanifest` | Décrit le site pour l'installation en « app » sur mobile (nom, icônes, couleurs). |
| `robots.txt` | Autorise les moteurs de recherche à tout indexer + indique où trouver le sitemap. |
| `sitemap.xml` | Plan du site pour Google (une seule URL pour l'instant). |
| `README.md` | Ce fichier. |

---

## Voir le site en local

Il suffit d'**ouvrir `index.html`** dans un navigateur (double-clic).

Pour un rendu plus fidèle (chemins absolus, favicons…), on peut lancer un petit serveur local. Par exemple, avec Python installé :

```bash
python -m http.server 8000
```
puis ouvrir http://localhost:8000 dans le navigateur.

---

## Déploiement

Ce dépôt (`olivier-gramain-art/bienvenue-guilde`) est le **dépôt principal** du site : la source de référence.

Le **webmaster synchronise automatiquement son propre dépôt depuis celui-ci**, puis se charge de la mise en ligne chez l'hébergeur. (Le site n'est pas servi par GitHub Pages — pas de fichier `CNAME`.)

👉 Pour publier une modification :

1. Commiter et **pousser sur `master`** : `git push origin master`.
2. **Prévenir le webmaster** (ex. sur Discord) qu'une mise à jour est dispo — surtout pour une grosse modif.
3. Il synchronise et déploie, puis confirme une fois en ligne.

> Autrement dit : côté ce dépôt, on s'arrête au `git push`. Le déploiement final vers l'hébergeur est géré par le webmaster.

---

## SEO — référencement

- Le domaine officiel est **sans `www`** : `https://laguilde-poitiers.com/`. La version `www` doit **rediriger** (redirection 301) vers la version sans `www` — à vérifier/configurer côté hébergeur.
- Après une mise en ligne : penser à déclarer le site dans la **Google Search Console** et à y **soumettre le sitemap** (`https://laguilde-poitiers.com/sitemap.xml`).
- Les métadonnées (titre, description, Open Graph, données structurées JSON-LD) sont dans le `<head>` de `index.html`.
- Pour rafraîchir l'aperçu de partage après mise à jour de `og-image.png` : [Sharing Debugger de Facebook](https://developers.facebook.com/tools/debug/).

---

## Personnaliser les couleurs (thème)

Presque toutes les couleurs passent par des **variables CSS** définies en haut de `style.css` (bloc `:root`). Pour changer le thème, il suffit en grande partie de modifier ces valeurs.

Thème actuel (« vitrail », repris des couleurs du logo) :

| Variable | Couleur | Usage |
|---|---|---|
| `--color-bg-dark` | `#241019` | Fond bordeaux très sombre (sections sombres, navbar, footer) |
| `--color-bg-light` | `#f5f0eb` | Fond crème (sections claires) |
| `--color-gold` | `#e8871e` | **Accent principal** — orange (liens, titres, boutons) |
| `--color-accent` | `#2ba7cf` | **Accent secondaire** — cyan (encadrés) |

---

## Régénérer les images (logo, favicons, og-image)

À faire uniquement si le **logo change**. La machine n'ayant pas d'outil image, on installe temporairement `sharp` (redimensionnement) et `png-to-ico` (fabrication du `.ico`) **hors du projet** :

```bash
npm install sharp png-to-ico
```

À partir du logo source (fichier `.webp`/`.png` haute résolution), on génère :
- `logo_guilde.png` (logo complet redimensionné),
- `logo_guilde_mark.png` (recadrage papillon + dé, sans le texte),
- les favicons (`favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`) = recadrage du dé sur fond crème,
- `apple-touch-icon.png` (180×180),
- `og-image.png` (1200×630, fond crème + logo + « de Poitiers »).

Puis on remplace les fichiers dans le projet **en gardant les mêmes noms** pour ne rien casser.
