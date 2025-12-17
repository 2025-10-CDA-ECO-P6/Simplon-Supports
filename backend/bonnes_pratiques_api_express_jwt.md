# Bonnes pratiques sécurité – API Express avec JWT

## Objectif

Ce document présente un ensemble de bonnes pratiques de sécurité à appliquer lors de la mise en place d’une authentification basée sur JWT dans une API Express.

L’objectif est de sécuriser l’API côté serveur, d’éviter les erreurs classiques et de poser un cadre professionnel, cohérent avec les attentes du titre professionnel CDA.

---

## Authentification (JWT)

* Utiliser un **validator** pour toutes les entrées (Zod, Joi…)
* Ne jamais faire confiance aux données envoyées par le client pour l’identité
* Extraire l’identité utilisateur **uniquement depuis le JWT vérifié**
* Signer les JWT avec une **clé secrète robuste** stockée dans les variables d’environnement
* Définir une **durée de vie courte** pour l’access token
* Rejeter tout token invalide ou expiré sans message d’erreur détaillé

---

## Stockage des tokens

* Stocker les JWT dans des **cookies httpOnly**
* Ne jamais utiliser le `localStorage` pour stocker un token
* Activer l’option **Secure** en HTTPS
* Configurer correctement **SameSite** (`Strict` ou `Lax` selon le contexte)

---

## Autorisation (droits)

* Séparer clairement :

  * authentification (qui je suis)
  * autorisation (ce que j’ai le droit de faire)
* Vérifier les droits **côté backend uniquement**
* Ne jamais faire confiance au rôle envoyé par le front
* Centraliser les contrôles dans des **middlewares** (`requireAuth`, `requireRole`)
* Bloquer les actions sensibles dans la couche **service**

---

## Validation des données

* Valider toutes les entrées utilisateur
* Refuser les champs inattendus (validation stricte)
* Ne jamais faire confiance aux IDs envoyés dans le body ou les query params
* Toujours utiliser l’ID issu du token JWT

---

## Sécurité de l’API Express

* Utiliser **Helmet** pour sécuriser les headers HTTP
* Mettre en place un **rate limiting** pour limiter les abus
* Limiter la taille des payloads (`express.json({ limit })`)
* Gérer les erreurs de manière centralisée
* Ne jamais exposer les stack traces en production

---

## Base de données

* Appliquer les contraintes SQL (PK, FK, UNIQUE, CHECK)
* Utiliser des **transactions** pour les opérations critiques
* Ne jamais exposer les erreurs SQL brutes à l’utilisateur
* Utiliser un utilisateur DB avec le **principe du moindre privilège**

---

## Logs et bonnes habitudes

* Logger les erreurs d’authentification (sans données sensibles)
* Tracer les actions critiques (suppression, changement de statut)
* Ne jamais logger de mots de passe ou de tokens
* Séparer clairement les environnements **dev / prod**

---

## Bonus – Refresh Token (optionnel)

* Utiliser un refresh token à durée de vie plus longue
* Stocker le refresh token en cookie httpOnly
* Prévoir une route dédiée au renouvellement de l’access token
* Invalider le refresh token lors de la déconnexion
* Mettre en place une rotation des refresh tokens si possible

---

## Rappel clé

Une API est sécurisée par ses **contrôles serveur**.  
Le front ne doit jamais être considéré comme une source de confiance.
