# 📚 Dictionnaire de données — sécuriser le langage commun

## Définition

Le **dictionnaire de données** est un référentiel vivant qui décrit chaque information manipulée par le système : nom de l’attribut, description métier, type, contraintes, source, usages. Il relie les exigences métiers aux modèles (MCD, MLD, APIs) et garantit qu’équipe fonctionnelle et technique parlent de la même chose.

## Pourquoi on utilise cette méthode / outil

- Harmoniser les termes utilisés par le métier, le produit, les développeurs et la QA.  
- Traquer les incohérences avant la conception technique (doublons, formats divergents).  
- Servir de point de contrôle pour les contrôles de saisie, tests et connecteurs externes.  
- Accélérer l’onboarding des nouveaux membres en leur fournissant une source unique fiable.  
- Anticiper les impacts RGPD (catégorie de données, durée de conservation, sensibilité).

## Exemple concret

**Contexte Patte & Cie** : suivi des protocoles vaccins dans le carnet numérique.

| ID | Champ | Description métier | Type / format | Source | Contraintes | Consommateurs |
| --- | --- | --- | --- | --- | --- | --- |
| DD-01 | `animal_id` | Identifiant unique d’un animal chez Patte & Cie | UUID | Backend | Obligatoire, unique | API dossier patient, dashboard clinique |
| DD-02 | `protocole_statut` | Statut courant du protocole vaccinal | Enum (`ACTIF`, `EXPIRÉ`, `EN_ATTENTE`) | Service vaccins | Valeur par défaut `EN_ATTENTE` | Notifications familles, moteur d’alertes |
| DD-03 | `lot_vaccin` | Numéro de lot injecté | Chaîne 10 char | Clinique | Format `[A-Z0-9]{10}` | Export ANSV, audit traçabilité |
| DD-04 | `date_prochaine_dose` | Date planifiée pour la prochaine injection | Date ISO | Service vaccins | ≥ date du jour | Agenda praticien, email automatisé |

> « Aligner les mots, c’est étirer un tapis soyeux où chaque équipe pose ses pattes sans trébucher. »  
> _— la patte noire_

## Mise en pratique (étapes détaillées)

1. **Recenser les sources d’information**  
   Parcourez user stories, MCD, APIs, maquettes et imports externes pour identifier tous les champs.  
2. **Normaliser les noms**  
   Choisissez un style (snake_case, PascalCase) cohérent avec les conventions back et front.  
3. **Documenter chaque champ**  
   Pour chaque attribut : description métier compréhensible, type précis, format, cardinalité, règles.  
4. **Cartographier les usages**  
   Indiquez quelles fonctionnalités, écrans ou flux s’appuient sur ce champ pour prioriser les tests.  
5. **Gérer la gouvernance**  
   Définissez qui peut ajouter/modifier une entrée, comment versionner et valider les changements.  
6. **Connecter aux autres livrables**  
   Référencez l’ID du champ dans le MCD, les Use Cases et les stories pour garder les artefacts synchronisés.

## Bonnes pratiques professionnelles

- Utilisez un identifiant unique (`DD-XX`) pour chaque entrée afin de tracer les impacts.  
- Rédigez les descriptions côté métier (bénéfice, usage) puis complétez avec la contrainte technique.  
- Mentionnez la **qualité de donnée** attendue (ex. valeur par défaut, plage acceptable, masque).  
- Ajoutez la **sensibilité** (public, interne, confidentiel) pour simplifier la conformité RGPD.  
- Versionnez le dictionnaire comme un code : PR dédiées, reviewers métiers + techniques.  
- Intégrez-le aux revues de sprint pour détecter les écarts dès qu’une story évolue.

## Canevas / checklist prêts à l’emploi

### Modèle Markdown

````markdown
| ID | Champ | Description métier | Type / format | Source | Contraintes / règles | Consommateurs |
| --- | --- | --- | --- | --- | --- | --- |
| DD-XX | `nom_champ` | Ce que signifie ce champ pour le métier | Type + format (ex. Date ISO, Enum) | Origine (formulaire, API, import) | Obligatoire ? Valeurs autorisées ? Calcul ? | Apps, APIs, exports concernés |

### Métadonnées complémentaires

- **Sensibilité / RGPD** : Public / Interne / Sensible / Donnée perso.  
- **Cycle de vie** : Collecte ➝ Consultation ➝ Conservation ➝ Suppression.  
- **Références croisées** : Use Case, User Story, MCD, règles métier, tests automatisés.
````

### Checklist qualité

- [ ] Chaque champ a une description métier orientée usage.  
- [ ] Type et format sont explicités (ex. `Number(10,2)`, Enum…).  
- [ ] Les règles métier et validations sont présentes.  
- [ ] Les consommateurs (modules, APIs, exports) sont listés.  
- [ ] Les impacts conformité (sensibilité, durée de conservation) sont précisés.  
- [ ] Les liens vers d’autres livrables sont à jour.

## Erreurs fréquentes à éviter

- Se contenter d’une simple liste de champs sans mentionner la règle métier ou le format exact.  
- Mélanger plusieurs définitions pour un même terme (ex. “client” côté facturation vs. côté carnet).  
- Oublier les attributs calculés ou importés, alors qu’ils conditionnent des KPI clés.  
- Laisser le dictionnaire dans un tableur isolé sans versionnage ni revue.  
- Réécrire les contraintes déjà présentes dans la base sans expliquer l’usage métier : manque de contexte.  
- Négliger l’évolution des termes quand une nouvelle fonctionnalité les étend (ex. nouveaux statuts).

## Glossaire

- **Champ / attribut** : information élémentaire décrite dans le dictionnaire (colonne, propriété d’objet).  
- **Contraintes** : règles imposées au champ (unicité, format, valeur minimale, dépendances).  
- **Consommateur** : fonctionnalité, API, rapport ou acteur qui lit ou écrit cette donnée.  
- **Sensibilité** : classification de confidentialité influençant les contrôles RGPD et sécurité.  
- **Gouvernance de données** : processus de validation, versionning et diffusion du dictionnaire.
