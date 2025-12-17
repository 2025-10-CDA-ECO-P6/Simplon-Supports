# 🧭 Use Case fonctionnel — capturer un scénario utilisateur clé

## Définition

Un **Use Case** décrit, du point de vue d’un acteur (humain ou système externe), la chaîne d’interactions nécessaires pour atteindre un objectif métier précis. Il formalise : l’acteur déclencheur, les prérequis, le flux nominal, les variantes et le résultat observable. Ce livrable relie directement besoins fonctionnels, règles métier et futures fonctionnalités.

## Pourquoi on utilise cette méthode / outil

- Clarifier rapidement ce que le produit doit accomplir pour chaque type d’utilisateur.
- Partager une vision commune entre PO, dev, QA et métiers sans jargon technique.
- Aligner les priorités projet avec les critères d’acceptation testables.
- Identifier les scénarios critiques avant de modéliser des écrans, des APIs ou un MCD.

## Exemple concret

**Contexte Patte & Cie** : la responsable clinique veut **renouveler un protocole vaccinal** depuis le carnet numérique.

| Élément | Description |
| --- | --- |
| Acteur principal | Responsable clinique Patte & Cie |
| Objectif | Mettre à jour un protocole vaccin pour un animal |
| Précondition | Le carnet numérique de l’animal est actif et l’authentification est validée |
| Déclencheur | La responsable ouvre la fiche d’un animal et choisit “Renouveler le protocole” |
| Flux nominal (extrait) | 1) Le système affiche les vaccins en cours 2) L’acteur sélectionne le vaccin à renouveler 3) Le système propose les dates légales 4) L’acteur confirme 5) Le système enregistre et notifie la famille |
| Extensions | Pas de stock vaccin → proposition de programmer une livraison |
| Résultat | Le protocole est daté, signé et visible pour la famille |

> « Les scénarios les mieux décrits laissent des traces aussi nettes que des coussinets sur la neige fraîche. »  
> _— la patte noire_

## Mise en pratique (étapes détaillées)

1. **Cadrer l’objectif métier**  
   Reformulez avec le sponsor : « Que veut obtenir l’acteur en une seule interaction ? »
2. **Lister les acteurs et leurs responsabilités**  
   Incluez acteurs secondaires (système tiers, API, ERP vétérinaire) si leur action influe sur le flux.
3. **Définir les préconditions et postconditions**  
   Ce qui doit être vrai avant/après garantit la testabilité et facilite les critères d’acceptation QA.
4. **Écrire le flux nominal en étapes numérotées**  
   Une étape = action de l’acteur ou réponse du système. Restez chronologique.
5. **Décrire les extensions / alternatives**  
   Précisez quand l’extension commence, comment elle se termine et son impact sur l’objectif.
6. **Vérifier la complétude**  
   Challengez CRO, dev et support client : manque-t-il une règle métier, un message, un contrôle datas ?
7. **Tracer les liens vers les autres livrables**  
   Associez l’ID du Use Case aux user stories, règles de gestion, maquettes et tests.

## Bonnes pratiques professionnelles

- Commencez chaque Use Case par un verbe d’action (« Consulter », « Renouveler », « Alerter »).  
- Restez indépendant de l’interface : « Le système affiche » plutôt que « L’écran montre un bouton vert ».  
- Numérotez systématiquement le flux principal pour faciliter la référence croisée avec les extensions.  
- Ajoutez les règles métier (validation, droits, délais légaux) au plus près de l’étape concernée.  
- Faites relire par un binôme métier + technique pour limiter les angles morts.

## Canevas / checklist prêts à l’emploi

### Template Use Case Patte & Cie

````markdown
## UC-XX — <Verbe + objectif métier>

- **Acteur principal** :
- **Acteurs secondaires** :
- **Objectif** :
- **Préconditions** :
- **Postconditions** :
- **Déclencheur** :
- **Flux nominal**
  1. …
- **Extensions**
  - E1 — Condition : … / Étapes : …
  - E2 — …
- **Données manipulées / règles métier clés** :
- **Liens** : User stories, maquettes, jeux de données, tests.
````

### Checklist éclair

- [ ] L’objectif est formulé en bénéfice utilisateur unique.  
- [ ] Les acteurs secondaires sont explicités.  
- [ ] Le flux nominal tient entre 5 et 15 étapes claires.  
- [ ] Chaque extension référence l’étape d’origine.  
- [ ] Les règles métier critiques sont associées à leur étape.  
- [ ] Les liens vers user stories, dictionnaire de données et MCD sont notés.

## Erreurs fréquentes à éviter

- Mélanger plusieurs objectifs dans un même Use Case (ex. “créer” + “consulter”).  
- Décrire l’interface au lieu du comportement fonctionnel.  
- Oublier de documenter les exceptions (erreurs réseau, droits insuffisants, données manquantes).  
- Garder le Use Case hors de synchro avec les stories ou les maquettes après une itération.  
- Négliger les acteurs non humains (capteurs, APIs) alors qu’ils déclenchent ou referment le scénario.

## Glossaire

- **Acteur** : personne ou système externe qui interagit avec le produit pour atteindre un objectif précis.  
- **Flux nominal** : séquence d’étapes qui se déroule quand tout se passe sans incident.  
- **Extension** : variation du flux nominal déclenchée par une condition particulière (erreur, choix différent).  
- **Postcondition** : état observable garantissant que l’objectif de l’acteur est atteint.  
- **ID de Use Case** : identifiant unique (ex. `UC-04`) facilitant le suivi inter-livrables.
