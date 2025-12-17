# 🧱 MCD Merise — modéliser les entités et relations métier

## Définition

Le **Modèle Conceptuel de Données (MCD)**, pilier de la méthode Merise, représente les informations métier sous forme d’entités, de relations et d’attributs indépendants de toute contrainte technique. Il décrit ce que le système doit mémoriser et comment les objets métiers interagissent, avant toute traduction en tables ou schémas API.

## Pourquoi on utilise cette méthode / outil

- Comprendre précisément la structure de l’information avant de concevoir une base ou une API.  
- Partager une vision métier unique entre product owner, experts métiers, développeurs et QA.  
- Identifier les cardinalités, dépendances fortes et règles de gestion indispensables.  
- Servir de base à la génération du MLD puis du MPD (Merise) ou aux schémas GraphQL/REST.  
- Anticiper les impacts RGPD en cartographiant où vivent les données personnelles.

## Exemple concret

**Contexte Patte & Cie** : suivi des protocoles vaccinaux.

Entités clés :

- **Animal** (`id_animal`, `nom`, `date_naissance`, `espèce`)  
- **ProtocoleVaccinal** (`id_protocole`, `statut`, `prochaine_dose`)  
- **Injection** (`id_injection`, `date`, `lot_vaccin`, `praticien`)  
- **Famille** (`id_famille`, `email`, `ville`)

Relations :

- `Famille` 1,N `Animal` (une famille suit plusieurs animaux, un animal appartient à une famille).  
- `Animal` 1,1 `ProtocoleVaccinal` (un protocole actif par animal).  
- `ProtocoleVaccinal` 1,N `Injection` (un protocole contient plusieurs injections).  
- `Injection` N,1 `Praticien` (chaque injection est réalisée par un praticien identifié).

Règles métiers issues du MCD :

- Un protocole ne peut être créé que si l’animal a un carnet valide.  
- Une injection doit référencer un lot de vaccin encore actif.  
- Lorsqu’un protocole passe à `EXPIRÉ`, toutes les injections futures deviennent inactives.

> « Tracez les liens invisibles entre les données, et les flux métier miauleront d’eux-mêmes. »  
> _— la patte noire_

## Mise en pratique (étapes détaillées)

1. **Collecter le vocabulaire métier**  
   Parcourez ateliers, Use Cases, dictionnaire de données et maquettes pour lister les objets manipulés.  
2. **Identifier les entités**  
   Une entité représente un objet stable doté d’un identifiant (Animal, Praticien, Rendez-vous).  
3. **Lister les attributs**  
   Associez à chaque entité les informations nécessaires (type, optionnel/obligatoire).  
4. **Définir les relations**  
   Reliez les entités avec un verbe métier (« Animal suit Protocole », « Famille possède Animal »).  
5. **Fixer les cardinalités**  
   Pour chaque relation, précisez le minimum et maximum côté entités (0, 1, N).  
6. **Valider les règles de gestion**  
   Documentez les contraintes spécifiques (unicité, dépendances temporelles, héritage).  
7. **Faire relire et itérer**  
   Présentez au binôme métier/technique, ajustez jusqu’à obtention d’un modèle stable puis versionnez-le.

## Bonnes pratiques professionnelles

- Utilisez un identifiant fonctionnel stable (`id_animal`, `code_praticien`) sur chaque entité.  
- Restez strictement métier : pas de type SQL ni de clé étrangère, uniquement des concepts.  
- Nommez les relations avec un verbe compréhensible (« planifie », « déclenche », « dépend de »).  
- Documentez les cardinalités minimales pour détecter les cas optionnels (0,N vs 1,N).  
- Coupez une entité trop riche en plusieurs entités si elle mélange des cycles de vie différents.  
- Synchronisez le MCD avec le dictionnaire de données : chaque attribut doit exister dans les deux.

## Canevas / checklist prêts à l’emploi

### Template de documentation MCD

````markdown
## Entités

- **Nom entité** (`id_xx`, `attribut1`, `attribut2`)
  - Description métier :
  - Observations / règles :

## Relations

- **Entité A** <verbe> **Entité B**
  - Cardinalité A ➝ B : (min,max)
  - Cardinalité B ➝ A : (min,max)
  - Règle métier :

## Invariants / règles globales

- …
````

### Checklist express

- [ ] Chaque entité possède un identifiant unique.  
- [ ] Les attributs obligatoires sont explicitement notés.  
- [ ] Toutes les relations sont verbalisées et leurs cardinalités min/max renseignées.  
- [ ] Les règles métier critiques (unicité, dépendances temporelles, héritages) figurent clairement.  
- [ ] Le MCD est aligné avec Use Cases, dictionnaire de données et référentiels RGPD.  
- [ ] Une version datée est archivée après validation.

## Erreurs fréquentes à éviter

- Confondre entité et association : une relation avec des attributs doit devenir une association (ex. RendezVous).  
- Mélanger concepts techniques (tables, index) alors que le MCD reste abstrait.  
- Oublier les cardinalités minimales, empêchant de gérer les cas facultatifs ou les créations différées.  
- Dessiner un MCD sans le confronter aux règles métier, conduisant à des incohérences dès la conception.  
- Multiplier les synonymes (Client / Propriétaire / Famille) sans harmoniser via le dictionnaire.  
- Négliger la traçabilité : pas d’ID, pas de lien avec les autres livrables.

## Glossaire

- **Entité** : objet métier stable possédant des attributs et un identifiant (Animal, Protocole).  
- **Association / relation** : lien entre deux ou plusieurs entités, porteur d’un verbe.  
- **Cardinalité** : nombre minimal et maximal d’occurrences associées (0,1,N).  
- **Attribut** : information décrivant une entité ou une association (date, statut, label).  
- **Invariants** : règles de gestion qui doivent toujours être respectées par le modèle (uniques, interdépendances).
