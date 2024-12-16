# Atelier Neo4j: Movie Graph By Wissal charraki

Bienvenue dans cet atelier sur Neo4j où vous apprendrez à explorer et interagir avec une base de données graphe basée sur les films et les relations entre acteurs, réalisateurs, et autres personnes de l'industrie cinématographique. Ce tutoriel couvre les bases pour les débutants et inclut des exemples pratiques avec la Movie Database préexistante.

---

## Table des Matières

---

<a name="introduction"></a>

## 1. Introduction à Neo4j et Movie Graph

Neo4j est une base de données orientée graphe qui stocke les données sous forme de nœuds (nodes) et de relations (relationships). 

La Movie Graph est une base de données démonstrative qui contient :

- **Nœuds** :
  - `Person` : Acteurs, réalisateurs, producteurs...
  - `Movie` : Films avec des détails comme le titre, l'année de sortie, etc.

- **Relations** :
  - `ACTED_IN` : Une personne a joué dans un film.
  - `DIRECTED` : Une personne a réalisé un film.
  - `PRODUCED` : Une personne a produit un film.
  - `WROTE` : Une personne a écrit un film.
  - `FOLLOWS` : Une personne suit une autre.
  - `REVIEWED` : Une personne a évalué un film.
  - `KNOWS` : Une personne connaît une autre.

Cet atelier vous guidera à travers l'exploration, la création et la manipulation de ces données.

---

<a name="installation"></a>

## 2. Installation de Neo4j

1. **Installer Neo4j Desktop :**
   - [Téléchargez Neo4j Desktop](https://neo4j.com/download/).
   - Installez l'application sur votre système.

2. **Lancer Neo4j Desktop :**
   - Créez un projet.
   - Ajoutez une base de données locale.
   - Lancez la base de données "Movie Database" préinstallée.

3. **Accéder à l'interface de requêtes :**
   - Cliquez sur le bouton "Open Browser".
   - Connectez-vous avec les identifiants par défaut :
     - Utilisateur : `neo4j`
     - Mot de passe : `neo4j` (changez-le si demandé).

---

<a name="requetes-de-base"></a>

## Cypher
Cypher est le langage de requêtes de Neo4j, conçu pour interagir facilement avec des données stockées sous forme de graphes. Cet atelier vous guidera à travers l'exploration, la création et la manipulation des données dans la base de données **Movie Graph**.

### 1. **Pattern simple :**
Un pattern simple correspond à un chemin unique entre des nœuds liés par une relation.

#### Exemple de pattern simple :
```cypher
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
RETURN a, m;
```
**Explication :**
- Ce modèle recherche un chemin entre un **Person** (acteur) et un **Movie** auquel il a participé via la relation **ACTED_IN**.
- Le résultat retourné contient les acteurs et les films dans lesquels ils ont joué.

### 2. **Pattern complexe :**
Un pattern complexe inclut plusieurs chemins ou relations.

#### Exemple de pattern complexe :
```cypher
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person)
RETURN a, m, d;
```
**Explication :**
- Ce modèle recherche un chemin entre un **Person** (acteur), un **Movie**, et un autre **Person** (réalisateur) qui a dirigé le film.
- Cela permet de récupérer les acteurs, les films et les réalisateurs associés.

### 3. **Syntaxe des nœuds :**

- **Nœud anonyme :** Représente un nœud sans label spécifique.
  ```cypher
  MATCH ()-[:ACTED_IN]->()
  RETURN DISTINCT labels(n);
  ```

- **Nœud avec un label :** Représente un nœud avec un label spécifique.
  ```cypher
  MATCH (:Movie)
  RETURN COUNT(*) AS number_of_movies;
  ```

- **Nœud avec des propriétés :** Représente un nœud avec un label et des propriétés spécifiques.
  ```cypher
  MATCH (:Movie {title: 'The Matrix'})
  RETURN *;
  ```

### 4. **Syntaxe des relations :**
- **Relation non dirigée :** Représente une relation sans direction spécifiée.
  ```cypher
  MATCH (:Person)-[:KNOWS]-(p:Person)
  RETURN p;
  ```

- **Relation dirigée :** Spécifie une direction dans la relation.
  ```cypher
  MATCH (:Person)-[:ACTED_IN]->(:Movie)
  RETURN COUNT(*) AS movies_played_in;
  ```

#### Exemple de relation avec des propriétés : (il faut créer la relation avec les properties si le cas échoue
```cypher
MATCH (:Person)-[r:ACTED_IN]->(:Movie)
WHERE r.roles CONTAINS 'Neo'
RETURN r;
```
**Explication :**
- Cette requête trouve les acteurs qui ont joué le rôle de **Neo** dans un film, en utilisant une propriété de relation **roles**.

### 5. **Principales Clauses Cypher :**

#### Clauses de recherche :

- **MATCH :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  WHERE m.title = 'The Matrix'
  RETURN a;
  ```
  **Explication :** Recherche les acteurs ayant joué dans "The Matrix".

- **OPTIONAL MATCH :**
  ```cypher
  OPTIONAL MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  WHERE m.title = 'The Matrix'
  RETURN a, m;
  ```
  **Explication :** Recherche les acteurs d'un film, mais inclut également les résultats même si le film n'est pas trouvé pour tous les acteurs.

#### Clauses de création et mise à jour :

- **CREATE :**
  ```cypher
  CREATE (a:Person {name: 'Keanu Reeves'})-[:ACTED_IN]->(m:Movie {title: 'The Matrix'})
  RETURN a, m;
  ```

- **MERGE :**
  ```cypher
  MERGE (a:Person {name: 'Keanu Reeves'})
  MERGE (m:Movie {title: 'The Matrix'})
  MERGE (a)-[:ACTED_IN]->(m);
  ```

#### Clauses de suppression : (Essayer d'éviter ces commandes à l'instant pour continuer l'atelier)

- **DELETE :**
  ```cypher
  MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
  DELETE r;
  ```

- **REMOVE :**
  ```cypher
  MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
  REMOVE r.roles;
  ```

#### Clauses de filtrage et agrégation :

- **WHERE :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  WHERE m.year > 2000
  RETURN a, m;
  ```

- **DISTINCT :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  RETURN DISTINCT a;
  ```

- **ORDER BY :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  ORDER BY m.year DESC
  RETURN a, m;
  ```

- **COLLECT :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  RETURN a.name, COLLECT(m.title) AS movies;
  ```

#### Clauses de pagination :

- **SKIP :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  SKIP 10 LIMIT 5
  RETURN a, m;
  ```

- **LIMIT :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  LIMIT 5
  RETURN a, m;
  ```

#### Clauses pour combiner et analyser des requêtes :

- **UNION :**
  ```cypher
  MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  RETURN a, m
  UNION
  MATCH (a:Person)-[:DIRECTED]->(m:Movie)
  RETURN a, m;
  ```

- **EXPLAIN :**
  ```cypher
  EXPLAIN MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  RETURN a, m;
  ```

- **PROFILE :**
  ```cypher
  PROFILE MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
  RETURN a, m;
  ```

#### Appel de procédures :

- **CALL :**
  ```cypher
  CALL db.info()
  YIELD name, value
  RETURN name, value;
  ```

---



## Conseils Supplémentaires

1. **Indexation :** Pour améliorer les performances des requêtes, créez des index sur les propriétés fréquemment recherchées.

    ```cypher
    CREATE INDEX ON :Person(name)
    CREATE INDEX ON :Movie(title)
    ```

2. **Contraintes d’Unicité :** Assurez-vous que certaines propriétés, comme le nom des films ou des personnes, sont uniques.

    ```cypher
    CREATE CONSTRAINT ON (p:Person) ASSERT p.name IS UNIQUE
    CREATE CONSTRAINT ON (m:Movie) ASSERT m.title IS UNIQUE
    ```

3. **Utilisation de Paramètres :** Pour rendre les requêtes plus dynamiques et sécurisées, utilisez des paramètres.

    ```cypher
    MATCH (p:Person { name: $nom })
    RETURN p
    ```

4. **Exploration Interactive :** Utilisez Neo4j Browser ou Neo4j Bloom pour visualiser les résultats des requêtes de manière interactive.

5. **Documentation :** Référez-vous à la [documentation officielle de Cypher](https://neo4j.com/docs/cypher-manual/current/) pour des informations détaillées et des exemples supplémentaires.

---

## Conclusion

Cet atelier présente une série d'exemples de requêtes Cypher pour la base de données **Movie Graph**. En maîtrisant ces requêtes, vous pourrez explorer, créer et manipuler efficacement les données dans un environnement de base de données graphe. N'hésitez pas à expérimenter avec ces requêtes et à les adapter à vos besoins spécifiques. Si vous avez des questions supplémentaires ou besoin de précisions sur certains aspects, je suis à votre disposition pour vous aider !

---

## Bonnes Pratiques pour l'Atelier

- **Organisation des Fichiers :** Placez toutes vos images dans un dossier dédié, par exemple `images/`, et référencez-les correctement dans vos requêtes si nécessaire.
- **Versionnement :** Utilisez un système de contrôle de version comme Git pour suivre les modifications apportées à votre fichier `README.md`.
- **Clarté des Exemples :** Assurez-vous que les exemples de requêtes sont clairs et faciles à suivre, avec des explications concises.
- **Validation des Requêtes :** Testez toutes les requêtes dans Neo4j Browser pour vous assurer qu'elles fonctionnent comme prévu avant de les inclure dans le README.

---

## Ressources Supplémentaires

- [Documentation Officielle de Cypher](https://neo4j.com/docs/cypher-manual/current/)
- [Neo4j Browser](https://neo4j.com/developer/neo4j-browser/)
- [Neo4j Bloom](https://neo4j.com/developer/neo4j-bloom/)

---

Bonne exploration et manipulation de votre **Movie Graph** avec Cypher !

---
