# Direct Twitch Link

Ouvre directement le **player Twitch** (`player.twitch.tv`) d'une chaîne, sans passer par l'application Twitch, pour limiter la consommation de données mobiles.

- Tape un nom de chaîne (ou colle un lien `twitch.tv/...`) et lance le player en un tap
- Favoris enregistrés sur l'appareil (étoile pour ajouter, ✕ pour retirer)
- Installable sur Android comme une vraie application (PWA), avec son icône
- Fonctionne hors ligne pour l'interface (le flux, lui, demande évidemment du réseau)
- Aucun compte, aucun serveur, aucune collecte de données

---

## 1. Mettre le projet en ligne (GitHub Pages, gratuit)

Tout se fait depuis le navigateur, sans rien installer.

1. Crée un compte sur [github.com](https://github.com) si besoin.
2. Clique sur **+** (en haut à droite) puis **New repository**.
   - Nom : `direct-twitch-link`
   - Visibilité : **Public** (obligatoire pour Pages gratuit)
   - Clique sur **Create repository**.
3. Sur la page du dépôt vide, clique sur le lien **uploading an existing file**.
4. Décompresse le ZIP sur ton ordinateur, ouvre le dossier, sélectionne **tout son contenu** (`index.html`, `manifest.json`, `sw.js`, `.nojekyll`, `README.md` et le dossier `icons`) et fais-le glisser dans la page GitHub.
   Fais glisser le contenu du dossier, pas le dossier lui-même, sinon `index.html` ne sera pas à la racine.
5. Clique sur **Commit changes**.
6. Va dans **Settings → Pages**. Sous *Build and deployment*, choisis :
   - Source : **Deploy from a branch**
   - Branch : **main** / dossier **/ (root)** puis **Save**.
7. Attends 1 à 2 minutes. Ton adresse apparaît en haut de la page Pages :

   `https://VOTRE_PSEUDO.github.io/direct-twitch-link/`

> Variante en ligne de commande : `git init`, `git add .`, `git commit -m "init"`, `git branch -M main`, `git remote add origin https://github.com/VOTRE_PSEUDO/direct-twitch-link.git`, `git push -u origin main`, puis l'étape 6.

## 2. Installer l'application sur Android

1. Ouvre ton adresse GitHub Pages dans **Chrome** sur ton téléphone.
2. Menu **⋮ → Installer l'application** (ou *Ajouter à l'écran d'accueil*).
3. Confirme. L'icône **Direct Twitch Link** apparaît sur ton écran d'accueil et s'ouvre en plein écran.

## 3. Partager

Envoie simplement l'adresse `https://VOTRE_PSEUDO.github.io/direct-twitch-link/` : chaque personne l'installe avec l'étape 2. Les favoris sont propres à chaque appareil.

## 4. (Optionnel) Obtenir un vrai fichier APK

Sans Android Studio :

1. Va sur [pwabuilder.com](https://www.pwabuilder.com) et colle ton adresse GitHub Pages.
2. Clique sur **Package for stores → Android** et télécharge l'archive.
3. Installe l'APK sur ton téléphone (autorise les « sources inconnues » quand Android le demande).

## Fonctionnement

L'application construit ce lien :

```
https://player.twitch.tv/?channel=NOM_DE_LA_CHAINE&parent=twitch.tv&autoplay=true
```

Twitch demande normalement un paramètre `parent` correspondant au site qui intègre le player. Ici le player est ouvert directement, la valeur `twitch.tv` est utilisée. Si Twitch refuse un jour le lecteur, change la valeur dans `index.html` :

```js
var KEY="dtl_favs", PARENT="twitch.tv", favs=[];
```

(essaie par exemple `localhost` ou le domaine `VOTRE_PSEUDO.github.io`).

## Mettre à jour l'application

1. Modifie un fichier sur GitHub (icône crayon), puis **Commit changes**.
2. Dans `sw.js`, augmente le numéro de version (`direct-twitch-link-v1` → `v2`) pour que les téléphones récupèrent la nouvelle version.

## Structure du projet

```
index.html      l'application (page, style et code)
manifest.json   description de l'appli installable (nom, couleurs, icônes)
sw.js           service worker : cache pour l'usage hors ligne
icons/          logo en SVG et PNG (192, 512, maskable, Apple)
.nojekyll       demande à GitHub Pages de servir les fichiers tels quels
```

## Dépannage

| Problème | Solution |
|---|---|
| Page 404 après la mise en ligne | Attends 2 minutes, vérifie que `index.html` est à la racine du dépôt et que Pages est sur *main / root* |
| « Installer l'application » n'apparaît pas | Utilise Chrome, ouvre l'adresse `https://...github.io/...` (pas un fichier local) et recharge la page |
| Les favoris ont disparu | Ils sont liés au navigateur : ils sont perdus si tu effaces les données du site ou changes de navigateur |
| Le player affiche une erreur | Voir la section *Fonctionnement* (paramètre `parent`) |
| L'ancienne version s'affiche | Augmente la version dans `sw.js`, puis ferme et rouvre l'appli |

## Avertissement

Projet indépendant, non affilié à Twitch. « Twitch » est une marque de Twitch Interactive, Inc.
