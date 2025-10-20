🦾 Atelier : Automatiser ses vérifications Git avec Lefthook

🎯 Objectifs
• Comprendre le rôle des Git hooks.
• Découvrir les limites des solutions classiques comme Husky.
• Mettre en place Lefthook pour automatiser lint, tests et validation de commits.

⸻

🧩 1. Qu’est-ce qu’un Git Hook ?

Les Git hooks sont des scripts qui se déclenchent automatiquement à certains moments du workflow Git :

Hook Moment d’exécution Exemple d’usage
pre-commit avant le commit Vérifier le lint ou les tests
commit-msg après le message du commit Valider le format du message
pre-push avant le push Lancer les tests unitaires

💡 Objectif : éviter de “casser” le projet avant même que le code parte sur le dépôt distant.

⸻

🐶 2. Pourquoi pas Husky ?

Husky est une excellente solution historique pour gérer les hooks Git côté Node.js. Mais il montre ses limites :

Problème Conséquence
Exécution séquentielle Les hooks peuvent être lents sur les gros projets
Scripts Shell dispersés Configuration fragmentée dans .husky/
Pas conçu pour les monorepos Difficile de cibler plusieurs sous-projets
Nécessite souvent d’autres outils (lint-staged, commitlint, etc.) pour un setup complet

🧠 Conclusion : Husky est parfait pour les petits projets front, mais devient rigide sur les gros projets ou les monorepos PNPM.

⸻

⚡ 3. Pourquoi Lefthook ?

💪 Conçu pour la performance

Lefthook exécute les tâches en parallèle → beaucoup plus rapide que Husky.

🧱 Idéal pour les monorepos

Il permet de cibler des répertoires spécifiques et de partager la même configuration à tous les packages.

🔧 Configuration centralisée

Tout se gère dans un seul fichier lefthook.yml (clair, versionnable, portable).

🧠 Cross-platform

Écrit en Go → fonctionne parfaitement sous Windows, macOS et Linux.

🚀 Intégration simple

Compatible avec tous les outils Node (ESLint, Prettier, Commitlint, Jest, etc.).

⸻

🧰 4. Installation et configuration

1️⃣ Installer Lefthook

pnpm add -D @evilmartians/lefthook

2️⃣ Initialiser le projet

pnpm lefthook install

Cela crée automatiquement un dossier .lefthook et active les hooks Git.

3️⃣ Créer le fichier de configuration

lefthook.yaml

pre-commit:
parallel: true
commands:
lint:
run: pnpm lint
test:
run: pnpm test --silent
commit-msg:
commands:
commitlint:
run: pnpm commitlint --edit $1

4️⃣ Vérifier le fonctionnement
• Fais un commit → Lefthook lancera les tests/lint automatiquement.
• Si une commande échoue, le commit est bloqué ✅

⸻

🧠 5. Exemple pour un monorepo PNPM

pre-commit:
parallel: true
commands:
lint-front:
run: pnpm --filter front lint
lint-back:
run: pnpm --filter back lint
test-shared:
run: pnpm --filter shared test

💡 Ici, Lefthook cible directement les sous-packages front, back et shared sans aucune configuration additionnelle.

⸻

⚖️ 6. Husky vs Lefthook — le comparatif

Critère Husky 🐶 Lefthook 🦾
Langage JavaScript Go
Exécution parallèle ❌ Non ✅ Oui
Support monorepo ⚠️ Moyen 🟢 Excellent
Config unique ❌ Non ✅ Oui (lefthook.yaml)
Performance ⚙️ Moyenne ⚡ Ultra rapide
Maintenance Bonne Très active

⸻

🧭 7. Bonnes pratiques
• Versionne ton fichier lefthook.yaml : il doit être commun à toute l’équipe.
• Garde les hooks rapides : évite de lancer des tests e2e à chaque commit.
• Combine avec Commitlint pour forcer un format de message de commit cohérent.
• Documente les hooks dans ton README pour que l’équipe sache ce qui est automatisé.

⸻

🎓 En résumé

Lefthook est la solution moderne, rapide et scalable pour gérer les Git hooks.
• 💨 Plus rapide que Husky grâce à l’exécution parallèle
• 🧩 Parfait pour les monorepos PNPM
• 🧱 Configuration unique, claire et portable

👉 Adoptez Lefthook pour industrialiser vos workflows Git et garantir la qualité du code dès le commit !
