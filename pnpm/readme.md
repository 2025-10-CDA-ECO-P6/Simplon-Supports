# 🚀 Atelier : Découvrir PNPM dans un monorepo Front / Back

## 🧩 Qu’est-ce qu’un monorepo et pourquoi l’utiliser ?

### 📦 Définition

Un **monorepo** (_monolithic repository_) est **un seul dépôt Git** qui contient **plusieurs projets ou packages**.

Exemple :

```
monorepo/
 ├─ front/
 ├─ back/
 ├─ shared/
 ├─ pnpm-workspace.yaml
 └─ package.json
```

Chaque dossier représente un projet indépendant (appelé _package_), mais tous partagent :

- la même gestion des dépendances,
- le même lockfile,
- la même configuration CI/CD.

### 🚀 Pourquoi utiliser un monorepo ?

#### 1️⃣ Centralisation du code

Tout le code (front, back, libs partagées) vit au même endroit → plus besoin de jongler entre plusieurs dépôts.

#### 2️⃣ Réutilisation facilitée

Le monorepo favorise le **partage de code** entre projets :

```
shared/
 └─ utils/
     └─ formatDate.js
```

➡️ Réutilisé à la fois dans `front/` et `back/` sans package publié sur npm.

#### 3️⃣ Maintenance simplifiée

Une seule installation, un seul `pnpm-lock.yaml`, une seule CI/CD.
Cohérence des versions, moins de duplication, tests unifiés.

#### 4️⃣ Meilleur workflow d’équipe

Les PR peuvent impacter plusieurs projets sans casser la synchro.
CI/CD et versioning plus simples à gérer.

#### 5️⃣ Support natif dans PNPM

PNPM gère nativement les workspaces via `pnpm-workspace.yaml` :

```yaml
packages:
  - "front"
  - "back"
  - "shared"
```

Permet de :

- partager le store global,
- exécuter des commandes ciblées (`--filter`),
- gérer des dépendances internes (`workspace:*`).

### ⚖️ Inconvénients et solutions

| Inconvénient                   | Explication                      | Solution                         |
| :----------------------------- | :------------------------------- | :------------------------------- |
| 🔀 Conflits Git plus fréquents | Plusieurs devs sur le même repo  | CI bien structurée + conventions |
| 🧱 Builds plus longs           | Plusieurs projets compilés       | Utiliser `--filter`              |
| 🧩 Complexité tooling          | Nécessite un gestionnaire adapté | PNPM, Nx ou Turborepo            |

### 💡 En résumé

| 🟢 Avantages           | 🔴 Inconvénients                  |
| :--------------------- | :-------------------------------- |
| Code centralisé        | Demande une CI adaptée            |
| Dépendances cohérentes | Peut devenir lourd sans structure |
| Réutilisation facile   | Risque de couplage                |
| Builds/tests unifiés   | Git plus sensible                 |

> **Un monorepo, c’est une seule source de vérité pour plusieurs projets interconnectés.**
> Et **PNPM** est l’un des outils les plus performants pour le gérer efficacement.

---

## 🎯 Objectifs

- Comprendre ce qu’est **pnpm** et pourquoi l’utiliser.
- Créer un **monorepo** avec deux projets : un **front** (React) et un **back** (Express).
- Découvrir les commandes principales de pnpm.
- Comparer pnpm avec npm et yarn.

---

## 📦 Qu’est-ce que PNPM ?

### Définition

**pnpm** est un gestionnaire de paquets JavaScript (comme npm ou yarn), mais :

- plus **rapide**,
- plus **économe en espace disque**,
- et mieux **adapté aux monorepos**.

### Fonctionnement

pnpm ne copie pas toutes les dépendances dans chaque `node_modules`.
Il utilise un **store global** et crée des **liens symboliques (symlinks)**.

**Avantage :**

- Installation ultra rapide 🚀
- Pas de doublons 💾
- Isolation stricte des dépendances ✅

**Structure interne simplifiée :**

```
~/.pnpm-store/
 ├─ lodash@4.17.21
 ├─ react@18.3.1
front/node_modules/ → symlink vers store
back/node_modules/ → symlink vers store
```

---

## 🧱 Mettre en place un monorepo

### Structure cible

```
monorepo/
 ├─ front/
 ├─ back/
 ├─ pnpm-workspace.yaml
 └─ package.json
```

(… contenu existant inchangé …)

---

## ⚙️ Étapes clés

1️⃣ **Initialisation du projet**

```bash
mkdir monorepo
cd monorepo
pnpm init
```

Créer le fichier **`pnpm-workspace.yaml`** :

```yaml
packages:
  - "front"
  - "back"
```

2️⃣ **Création des sous-projets**

```bash
mkdir front back
cd front && pnpm init && cd ..
cd back && pnpm init && cd ..
```

3️⃣ **Installation des dépendances**

```bash
pnpm add react react-dom vite --filter front
pnpm add express --filter back
```

4️⃣ **Ajout des scripts**
`front/package.json` :

```json
"scripts": {
  "dev": "vite"
}
```

`back/package.json` :

```json
"scripts": {
  "start": "node server.js"
}
```

Créer le fichier **`back/server.js`** :

```js
import express from "express";
const app = express();
app.get("/", (_, res) => res.send("Hello depuis le back 👋"));
app.listen(3000, () => console.log("Back lancé sur http://localhost:3000"));
```

5️⃣ **Lancer les projets**

```bash
pnpm --filter front dev
pnpm --filter back start
```

6️⃣ **Partager du code entre packages (optionnel)**
Créer un dossier `shared` et l’ajouter dans `pnpm-workspace.yaml`.

---

## 🧩 Pourquoi PNPM est mieux adapté aux monorepos

### 1️⃣ Gestion native des workspaces

pnpm gère les _workspaces_ sans configuration complexe via un simple `pnpm-workspace.yaml`.
Il reconnaît automatiquement tous les sous-projets et gère un **unique lockfile**.

### 2️⃣ Store global et liens symboliques

pnpm utilise un **store unique** avec des symlinks, économisant l’espace disque et accélérant les installations.

### 3️⃣ Protocole `workspace:` pour les dépendances internes

Les dépendances entre projets du monorepo sont automatiquement liées avec `workspace:*`, sans `npm link`.

### 4️⃣ Isolation stricte des dépendances

Chaque projet n’a accès qu’à ses dépendances déclarées — plus de “phantom dependencies”.

### 5️⃣ Performances et CI/CD

Cache partagé, installation incrémentale, compatibilité avec Turborepo, Nx, Lerna, etc.

| Fonctionnalité              |    npm     |    yarn    |      pnpm       |
| :-------------------------- | :--------: | :--------: | :-------------: |
| Workspaces intégrés         | ⚙️ Partiel |     ✅     |      ✅✅       |
| Lockfile unique             |     ✅     |     ✅     |       ✅        |
| Store global partagé        |     ❌     | ⚙️ Partiel |       ✅        |
| Isolation stricte           |     ❌     |     ⚠️     |       ✅        |
| Lien interne (`workspace:`) |     ❌     |     ✅     |       ✅        |
| Installation rapide         | ⚙️ Moyenne | 🚀 Rapide  | ⚡ Ultra-rapide |
| Économie d’espace           |     ❌     |     ⚠️     |       ✅        |
| Idéal pour monorepo         |  ⚙️ Moyen  |     ✅     |  🥇 Excellent   |

---

## 💡 Commandes et astuces utiles

| Action                              | Commande                          |
| :---------------------------------- | :-------------------------------- |
| Voir le chemin du store global      | `pnpm store path`                 |
| Lister les paquets du store         | `ls $(pnpm store path)/v3`        |
| Vérifier les paquets inutilisés     | `pnpm store status`               |
| Nettoyer le store global            | `pnpm store prune`                |
| Supprimer les doublons du lockfile  | `pnpm dedupe`                     |
| Ajouter une dépendance à un package | `pnpm add <pkg> --filter <nom>`   |
| Lancer un script spécifique         | `pnpm --filter <projet> <script>` |
| Lancer pour tous les projets        | `pnpm -r <script>`                |

---

## 🧹 `pnpm store prune` — Nettoyage du store global

### 🧾 Description

Supprime les **packages non référencés** du **store global**, c’est-à-dire ceux non utilisés par aucun projet sur le système.

> Exemple : `foo@1.0.0` est remplacé par `foo@1.0.1` → `foo@1.0.0` devient inutilisé et sera supprimé par `pnpm store prune`.

### ✅ Commande

```bash
pnpm store prune
```

### 🧠 À retenir

- 💡 Sans danger : n’affecte aucun projet.
- 🔁 Si un package supprimé devient nécessaire plus tard, il sera retéléchargé automatiquement.
- 🧼 Bonne pratique : exécuter de temps en temps, pas après chaque install.
- 🚫 Interdit si un _store server_ pnpm est en cours d’exécution.

### ⚡ Astuce

Combine avec :

```bash
pnpm store status   # voir les paquets inutilisés
pnpm store path     # localiser le store global
```

---

## 🎉 Conclusion

**pnpm** est une alternative moderne et performante à npm/yarn :

- Plus rapide ⚡
- Plus économe 💾
- Parfait pour les monorepos 🧩

**À utiliser :**

- Pour les projets multi-modules (front/back/libs)
- En CI/CD pour accélérer les builds
- Pour éviter les dépendances fantômes et améliorer la cohérence
