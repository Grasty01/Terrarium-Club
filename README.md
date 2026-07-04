# 🌿 Terrarium Club

Site vitrine et espace membre pour **Terrarium Club**, un service fictif d'abonnement mensuel à des plantes d'intérieur. Projet réalisé dans le cadre d'un parcours de formation développement web fullstack JavaScript, avec un focus sur **CSS Flexbox et Grid**.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![Responsive](https://img.shields.io/badge/Responsive-Mobile--First-C6D94D?style=flat)

---

## 📋 Table des matières

- [Aperçu](#-aperçu)
- [Objectifs pédagogiques](#-objectifs-pédagogiques)
- [Pages du site](#-pages-du-site)
- [Stack technique](#-stack-technique)
- [Architecture du projet](#-architecture-du-projet)
- [Le système de grille à 12 colonnes](#-le-système-de-grille-à-12-colonnes)
- [Design system](#-design-system)
- [Responsive — approche mobile-first](#-responsive--approche-mobile-first)
- [Installation](#-installation)
- [Validation](#-validation)
- [Livrables](#-livrables)
- [Auteur](#-auteur)

---

## 🖼️ Aperçu

> Ajoute ici une capture d'écran ou un GIF de ton site une fois terminé.
>
> `![Aperçu du site](./docs/screenshot-home.png)`

**Démo en ligne :** _(lien à ajouter après déploiement, ex. GitHub Pages / Netlify)_

---

## 🎯 Objectifs pédagogiques

Ce projet regroupe et assemble 5 exercices de layout CSS distincts en un seul site cohérent :

- Maîtriser **Flexbox** pour l'alignement sur un seul axe (navbar, cartes)
- Maîtriser **Grid** pour les structures à deux dimensions (Holy Grail, Bento)
- Construire un système **responsive sans média queries complexes** grâce à `auto-fit`/`minmax()`, puis affiner avec une approche **mobile-first**
- Respecter une **sémantique HTML stricte** (hiérarchie des titres, `alt` pertinents, bonne balise pour chaque usage)
- Structurer un CSS à grande échelle avec une **architecture modulaire** (inspirée d'ITCSS / 7-1)

---

## 📄 Pages du site

| Page                              | Description                                                                                               | Accès                    |
| --------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------ |
| **Accueil** (`index.html`)        | Navbar, Hero, section Bento (arguments clés), témoignages, formules d'abonnement (Card Deck), FAQ, footer | Publique                 |
| **Mon Jardin** (`dashboard.html`) | Tableau de bord membre : suivi des plantes, prochaine livraison, conseil du jour                          | Privée (compte connecté) |

---

## 🛠️ Stack technique

- **HTML5** sémantique (pas de framework)
- **CSS3** — Flexbox, Grid, variables CSS natives (`:root`)
- **Google Fonts** — [Fraunces](https://fonts.google.com/specimen/Fraunces) (italique, titres) & [Nunito Sans](https://fonts.google.com/specimen/Nunito+Sans) (corps de texte)
- Aucune dépendance JavaScript ni framework CSS — vanilla par choix pédagogique

---

## 🏗️ Architecture du projet

Le CSS est découpé en couches, du plus général au plus spécifique, sur le principe **ITCSS** :

```
📁 terrarium-club/
│
├── 📄 index.html              → Page d'accueil
├── 📄 dashboard.html          → Page "Mon Jardin"
│
├── 📁 img/                    → Photos et images du site
│
└── 📁 css/
    ├── 📄 app.css             → Le tronc commun : uniquement des @import, dans l'ordre
    │
    ├── 📁 base/
    │   ├── reset.css          → Remise à zéro des styles par défaut du navigateur
    │   └── variables.css      → Couleurs, polices, échelle d'espacement (custom properties)
    │
    ├── 📁 layout/
    │   ├── grid.css           → Le moteur de grille à 12 colonnes + conteneur global
    │   ├── header.css         → La barre de navigation
    │   ├── hero.css           → La section d'accueil (Hero)
    │   ├── bento.css          → La grille asymétrique "Pourquoi nous rejoindre"
    │   ├── pricing.css        → Le conteneur des formules d'abonnement
    │   └── dashboard.css      → La structure Holy Grail de "Mon Jardin"
    │
    ├── 📁 components/
    │   ├── card.css           → Le design des cartes (Sprout / Bloom / Canopy)
    │   └── button.css         → Les boutons (plein, contour, pilule)
    │
    └── 📁 utilities/
        └── utilities.css      → Classes utilitaires (.col-span-*, .text-center...), importées en dernier
```

> **Règle d'or de `app.css`** : les fichiers s'importent du plus général (`base/`) au plus spécifique (`utilities/`), jamais l'inverse — sinon les classes utilitaires risquent d'être écrasées par les styles de composants.

```css
/* css/app.css */
@import "base/reset.css";
@import "base/variables.css";

@import "layout/grid.css";
@import "layout/header.css";
@import "layout/hero.css";
@import "layout/bento.css";
@import "layout/pricing.css";
@import "layout/dashboard.css";

@import "components/card.css";
@import "components/button.css";

@import "utilities/utilities.css";
```

**Comment savoir si un style va dans `layout/` ou `components/` ?**

> _Est-ce que ce bloc se répète plusieurs fois sur la même page ?_
>
> - Oui → `components/` (ex. une carte, un bouton)
> - Non, il n'existe qu'une fois → `layout/` (ex. le header, le hero, le dashboard)

---

## 🔲 Le système de grille à 12 colonnes

Toutes les sections du site reposent sur une **grille de référence invisible à 12 colonnes**, définie une seule fois dans `layout/grid.css` :

```css
main {
  max-width: 1200px;
  margin-inline: auto;
  padding-inline: 24px;
}

.grid-12 {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 24px;
}
```

Chaque section réutilise ensuite cette base et positionne ses éléments en `grid-column: span X` plutôt qu'en valeurs arbitraires :

| Section                        | Répartition en colonnes (sur 12) |
| ------------------------------ | -------------------------------- |
| Bento — tuile principale       | `span 6` (2 lignes)              |
| Bento — tuiles secondaires     | `span 3` chacune                 |
| Card Deck                      | 3 cartes en `span 4`             |
| Dashboard — nav / main / aside | `span 2` / `span 7` / `span 3`   |

---

## 🎨 Design system

### Typographie

| Rôle                  | Police                        |
| --------------------- | ----------------------------- |
| Titres (h1, h2, logo) | `Fraunces`, italique, 600–700 |
| Corps de texte        | `Nunito Sans`, 400–600        |

### Palette de couleurs

| Usage               | Couleur   |
| ------------------- | --------- |
| Fond principal      | `#1B2E1F` |
| Fond cartes sombres | `#20351f` |
| Cartes claires      | `#F4F1E8` |
| Accent principal    | `#C6D94D` |
| Terracotta          | `#8A5A3B` |
| Vert végétal        | `#639922` |
| Alerte / urgent     | `#D4A24C` |

### Règles de forme

| Élément             | Rayon              |
| ------------------- | ------------------ |
| Boutons / CTA       | `20–24px` (pilule) |
| Cartes / tuiles     | `14–18px`          |
| Avatars / pastilles | `50%`              |

---

## 📱 Responsive — approche mobile-first

Le CSS est écrit **mobile-first** : les styles de base (sans média query) ciblent le plus petit écran, puis chaque `@media (min-width: ...)` ajoute des ajustements pour les écrans plus grands.

| Palier        | Largeur             | Comportement                             |
| ------------- | ------------------- | ---------------------------------------- |
| Mobile (base) | `< 700px`           | Nav réduite, contenu empilé en 1 colonne |
| Tablette      | `min-width: 700px`  | Nav complète, grilles en 2 colonnes      |
| Desktop       | `min-width: 1024px` | Grille 12 colonnes pleinement exploitée  |

---

## 💻 Installation

Ce projet ne nécessite aucune dépendance ni build.

```bash
# Cloner le repository
git clone https://github.com/Terrarium-Club/terrarium-club.git

# Se déplacer dans le dossier
cd terrarium-club

# Ouvrir index.html directement dans le navigateur
# ou utiliser une extension de type Live Server
```

---

## ✅ Validation

- HTML validé sans erreur via le [validateur W3C](https://validator.w3.org/)
- Structure sémantique vérifiée (hiérarchie des titres, `alt` sur les images, balises appropriées)

---

## 📦 Livrables

| #   | Livrable               | Technique principale                 |
| --- | ---------------------- | ------------------------------------ |
| 01  | Barre Nav              | Flexbox, `position: sticky`          |
| 02  | Holy Grail (Dashboard) | Grid, `grid-template-areas`          |
| 03  | Bento Gallery          | Grid, `grid-column`/`grid-row: span` |
| 04  | Card Deck              | Flexbox, `flex-wrap`                 |
| 05  | SaaS Hero              | `position: absolute` (superposition) |

---

## 👤 Auteur

**Grâsty Ghyvet SAMBA DINAULT**
Projet réalisé dans le cadre d'une formation développement web fullstack JavaScript.

---

## 📄 Licence

Projet pédagogique — libre d'utilisation à but d'apprentissage.
