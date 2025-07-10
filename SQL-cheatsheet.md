# SQL Cheat-Sheet

## Qu'est-ce que le SQL ?
  SQL (**Structured Query Language**) est un langage utilisé pour **interroger**, **manipuler** et **transformer** des données dans une **base de données relationnelle**.

## Bases de données relationnelles
  Une base de données relationnelle est composée de **tables** :
    - Chaque **table** a des **colonnes** (attributs) et des **lignes** (données).
    - Les tables peuvent être **liées entre elles** par des relations (clés primaires / étrangères).

## Objectifs de SQL
SQL permet de :
- Rechercher des données (`SELECT`)
- Ajouter des données (`INSERT`)
- Modifier des données (`UPDATE`)
- Supprimer des données (`DELETE`)
- Gérer la structure des tables (`CREATE`, `ALTER`, `DROP`)

Les bases du SQL incluent :
- La structure des requêtes (`SELECT ... FROM ... WHERE`)
- La création et modification de tables
- Les jointures entre tables
- Les fonctions d’agrégation (`COUNT`, `SUM`, etc.)

---

## SQL - Requêtes avec contraintes (WHERE) 

`WHERE` permet de **filtrer** les lignes selon des conditions.

### Syntaxe de base

```sql
SELECT colonne1, colonne2, ...
FROM table
WHERE condition
  AND/OR autre_condition;

```

| Opérateur                | Description                 | Exemple SQL                       |
|------------------------ | --------------------------- | --------------------------------- |
| =, !=, <, <=, >, >=      | Opérateurs standards        | `age != 30`                       |
| `BETWEEN a AND b`        | Entre deux valeurs (inclus) | `prix BETWEEN 10 AND 100`         |
| `NOT BETWEEN a AND b`    | En dehors de deux valeurs   | `note NOT BETWEEN 5 AND 15`       |
| `IN (val1, val2, …)`     | Est dans une liste          | `id IN (1, 2, 3)`                 |
| `NOT IN (val1, val2, …)` | N'est pas dans une liste    | `statut NOT IN ('banni', 'test')` |

---

## SQL – Contraintes sur les chaînes de caractères (WHERE)

### Opérateurs utiles pour le texte

| Opérateur           | Description                                             | Exemple                                |
|---------------------|---------------------------------------------------------|----------------------------------------|
| `=`                 | Comparaison exacte (sensible à la casse)                | `nom = 'Jean'`                         |
| `!=` ou `<>`        | Différent (sensible à la casse)                         | `nom != 'Paul'`                        |
| `LIKE`              | Comparaison insensible à la casse avec joker            | `ville LIKE 'PARIS'`                   |
| `NOT LIKE`          | N’est pas égal (insensible à la casse)                  | `pays NOT LIKE 'FRANCE'`               |
| `%`                 | Joker : **0 ou plusieurs caractères** (avec LIKE)       | `nom LIKE '%ette%'` → 'Annette'        |
| `_`                 | Joker : **1 seul caractère** (avec LIKE)                | `code LIKE 'A_3'` → 'AB3', 'AC3'       |
| `IN (...)`          | Valeur textuelle dans une liste                         | `genre IN ('Homme', 'Femme')`          |
| `NOT IN (...)`      | Valeur textuelle non dans une liste                     | `statut NOT IN ('test', 'archivé')`    |

---

#### Astuces

- Les **chaînes de caractères doivent être entourées de guillemets simples** `'...'`

---

## SQL – Filtrer, trier et limiter les résultats

### Éliminer les doublons avec `DISTINCT`

```
SELECT DISTINCT colonne1, colonne2, ...
FROM table
WHERE condition;
```
Supprime les lignes dupliquées dans les résultats.
Supprime les doublons sur toutes les colonnes sélectionnées.


#### Trier les résultats avec `ORDER BY`

```
SELECT colonne1, colonne2, ...
FROM table
WHERE condition
ORDER BY colonne ASC|DESC;
```

Trie les résultats selon une colonne :

    ASC = ordre croissant (par défaut)

    DESC = ordre décroissant

Peut trier du texte, des dates ou des nombres.

####  Limiter les résultats avec `LIMIT` et `OFFSET`

```
SELECT colonne1, ...
FROM table
WHERE condition
ORDER BY colonne
LIMIT nombre
OFFSET décalage;
```

- LIMIT : nombre maximal de lignes à retourner
- OFFSET : décalage (saute les x premières lignes) 
- Idéal pour la pagination (ex : page 2 = LIMIT 10 OFFSET 10)
- La clause WHERE est exécutée avant ORDER BY, puis LIMIT/OFFSET.

---

## SQL – Révision : Requêtes `SELECT` simples

### Structure d'une requête complète

```
SELECT colonne1, colonne2, ...
FROM table
WHERE condition(s)
ORDER BY colonne ASC|DESC
LIMIT nombre
OFFSET décalage;
```

 Éléments de base

    SELECT : indique quelles colonnes afficher

    FROM : indique de quelle table proviennent les données

    WHERE : filtre les lignes selon une ou plusieurs conditions

    ORDER BY : tri les résultats (ASC = croissant, DESC = décroissant)

    LIMIT : limite le nombre de lignes retournées

    OFFSET : ignore un certain nombre de lignes au début (utile pour la pagination)

Exemple

```
SELECT nom, age
FROM utilisateurs
WHERE age >= 18
ORDER BY age DESC
LIMIT 10 OFFSET 0;
```
→ Affiche les 10 utilisateurs majeurs les plus âgés.

## SQL - Requêtes multi-tables avec JOIN

  La normalisation est le processus de division des données en plusieurs tables pour :

    - Réduire les duplications.
    - Permettre une évolution indépendante des données.

### Syntaxe de la jointure INNER JOIN

```
SELECT colonne, autre_table.colonne, ...
FROM table_principale
INNER JOIN autre_table
    ON table_principale.id = autre_table.id
WHERE condition(s)
ORDER BY colonne ASC/DESC
LIMIT nombre OFFSET décalage;
```
IMPORTANT :
  - INNER JOIN = ne garde que les lignes avec des correspondances dans les deux tables.
  - On peut écrire simplement JOIN au lieu de INNER JOIN, c’est équivalent.
  - Les autres clauses (WHERE, ORDER BY, LIMIT, etc.) s’appliquent après la jointure.

  ## SQL - Requêtes avec OUTER JOIN

    - Une INNER JOIN ne renvoie que les lignes présentes dans les deux tables.

    - Si les données sont asymétriques (ex : saisies à différents moments), on peut perdre des informations.

    - Pour éviter cela, on utilise :

        * LEFT JOIN = Garde toutes les lignes de la table de gauche (même sans correspondance dans la table de droite).

        * RIGHT JOIN = Garde toutes les lignes de la table de droite (même sans correspondance dans la table de gauche).

        * FULL JOIN = Garde toutes les lignes des deux tables, avec ou sans correspondance.

### Syntaxe générale des OUTER JOINs

``` SELECT colonne, autre_colonne, ...
FROM table1
LEFT | RIGHT | FULL JOIN table2
    ON table1.id = table2.matching_id
WHERE condition(s)
ORDER BY colonne ASC/DESC
LIMIT nombre OFFSET décalage;
```

## SQL - Les valeurs NULL
  
  - NULL représente une valeur inconnue ou absente dans une base de données.
  - Il faut des conditions spécifiques pour gérer les NULL.

```
SELECT colonne1, colonne2
FROM table
WHERE colonne1 IS NULL
  OR colonne2 IS NOT NULL;
```

  - IS NULL → Vrai si la colonne n’a pas de valeur
  - IS NOT NULL → Vrai si la colonne contient une valeur
