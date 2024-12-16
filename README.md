# Atelier Neo4j: Movie Graph By Wissal charraki

Bienvenue dans cet atelier sur Neo4j où vous apprendrez à explorer et interagir avec une base de données graphe basée sur les films et les relations entre acteurs, réalisateurs, et autres personnes de l'industrie cinématographique. Ce tutoriel couvre les bases pour les débutants et inclut des exemples pratiques avec la Movie Database préexistante.

---

## Table des Matières

1. [Introduction à Neo4j et Movie Graph](#introduction)
2. [Installation de Neo4j](#installation)
3. [Requêtes de base avec Cypher](#requetes-de-base)
4. [Manipulation des données](#manipulation-des-donnees)
   - [Ajout de données](#ajout-de-donnees)
   - [Suppression de données](#suppression-de-donnees)
   - [Mise à jour et MERGE](#mise-a-jour-et-merge)
5. [Requêtes avancées](#requetes-avancees)
   - [Recommandations et recherche complexe](#recommandations)
   - [Chemin de Bacon](#chemin-de-bacon)
6. [Ressources supplémentaires](#ressources)

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
Bien sûr ! Voici la section **Atelier** formatée en Markdown pour votre fichier `README.md`. Cette section guide les utilisateurs à travers diverses opérations sur la base de données **Movie Graph** en utilisant le langage Cypher.

---

## Atelier : Manipulation de la Base de Données Movie Graph avec Cypher

Cypher est le langage de requêtes de Neo4j, conçu pour interagir facilement avec des données stockées sous forme de graphes. Cet atelier vous guidera à travers l'exploration, la création et la manipulation des données dans la base de données **Movie Graph**.

### **1. Introduction à la Movie Graph**

La base de données **Movie Graph** contient les éléments suivants :

- **Nœuds :**
  - `Person` : Acteurs, réalisateurs, producteurs...
  - `Movie` : Films avec des détails comme le titre, l'année de sortie, etc.

- **Relations :**
  - `ACTED_IN` : Une personne a joué dans un film.
  - `DIRECTED` : Une personne a réalisé un film.
  - `PRODUCED` : Une personne a produit un film.
  - `WROTE` : Une personne a écrit un film.
  - `FOLLOWS` : Une personne suit une autre.
  - `REVIEWED` : Une personne a évalué un film.
  - `KNOWS` : Une personne connaît une autre.

### **2. Création de Nœuds et Relations**

#### **2.1. Création de Nœuds Simples**

**Exemple : Création d'une personne**

```cypher
CREATE (p:Person { name: 'Brie Larson', born: 1989 })
RETURN p
```

**Exemple : Création d'un film**

```cypher
CREATE (m:Movie { title: 'The Matrix', released: 1999 })
RETURN m
```

#### **2.2. Création de Relations**

**Exemple : Relier une personne à un film en tant qu'acteur**

```cypher
MATCH (p:Person { name: 'Keanu Reeves' }), (m:Movie { title: 'The Matrix' })
CREATE (p)-[:ACTED_IN { roles: ['Neo'] }]->(m)
RETURN p, m
```

**Exemple : Relier une personne à un film en tant que réalisateur**

```cypher
MATCH (p:Person { name: 'Lana Wachowski' }), (m:Movie { title: 'The Matrix' })
CREATE (p)-[:DIRECTED]->(m)
RETURN p, m
```

### **3. Requêtes de Recherche**

#### **3.1. Recherche de Personnes ayant Agi dans un Film Spécifique**

**Objectif :** Trouver tous les acteurs ayant joué dans le film "Inception".

```cypher
MATCH (p:Person)-[:ACTED_IN]->(m:Movie { title: 'Inception' })
RETURN p.name AS Acteur
```

#### **3.2. Trouver les Films Réalisés par une Personne**

**Objectif :** Lister tous les films réalisés par "Christopher Nolan".

```cypher
MATCH (p:Person { name: 'Christopher Nolan' })-[:DIRECTED]->(m:Movie)
RETURN m.title AS Film, m.released AS Année
```

#### **3.3. Identifier les Relations de Connaissance entre Deux Personnes**

**Objectif :** Vérifier si "Leonardo DiCaprio" connaît "Ellen Page".

```cypher
MATCH (p1:Person { name: 'Leonardo DiCaprio' })-[:KNOWS]->(p2:Person { name: 'Ellen Page' })
RETURN p1.name AS Personne1, p2.name AS Personne2
```

### **4. Clauses de Filtrage et Agrégation**

#### **4.1. Filtrer les Résultats avec WHERE**

**Objectif :** Récupérer les films sortis après l'année 2000.

```cypher
MATCH (m:Movie)
WHERE m.released > 2000
RETURN m.title AS Film, m.released AS Année
```

#### **4.2. Éliminer les Doublons avec DISTINCT**

**Objectif :** Lister les genres de films distincts dans lesquels "Leonardo DiCaprio" a joué.

```cypher
MATCH (p:Person { name: 'Leonardo DiCaprio' })-[:ACTED_IN]->(m:Movie)
RETURN DISTINCT m.genre AS Genre
```

#### **4.3. Trier les Résultats avec ORDER BY**

**Objectif :** Trier les films par année de sortie décroissante.

```cypher
MATCH (m:Movie)
RETURN m.title AS Film, m.released AS Année
ORDER BY m.released DESC
```

#### **4.4. Agréger les Données avec COLLECT**

**Objectif :** Regrouper les films par acteur.

```cypher
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
RETURN p.name AS Acteur, COLLECT(m.title) AS Films
```

### **5. Clauses de Pagination**

#### **5.1. Sauter un Nombre de Résultats avec SKIP**

**Objectif :** Sauter les 10 premiers résultats et récupérer les suivants.

```cypher
MATCH (p:Person)
RETURN p.name AS Personne
SKIP 10
```

#### **5.2. Limiter le Nombre de Résultats avec LIMIT**

**Objectif :** Limiter les résultats à 5 personnes.

```cypher
MATCH (p:Person)
RETURN p.name AS Personne
LIMIT 5
```

#### **5.3. Combinaison de SKIP et LIMIT**

**Objectif :** Sauter les 10 premiers résultats et récupérer les 5 suivants.

```cypher
MATCH (p:Person)
RETURN p.name AS Personne
SKIP 10
LIMIT 5
```

### **6. Combinaison et Analyse de Requêtes**

#### **6.1. Combiner des Requêtes avec UNION**

**Objectif :** Récupérer les personnes qui ont soit joué dans "Inception" soit réalisé "Inception".

```cypher
MATCH (p:Person)-[:ACTED_IN]->(m:Movie { title: 'Inception' })
RETURN p.name AS Personne
UNION
MATCH (p:Person)-[:DIRECTED]->(m:Movie { title: 'Inception' })
RETURN p.name AS Personne
```

#### **6.2. Analyser le Plan d’Exécution avec EXPLAIN**

**Objectif :** Afficher le plan d'exécution de la requête qui trouve les amis d'un acteur ayant joué dans "Inception".

```cypher
EXPLAIN
MATCH (p:Person)-[:ACTED_IN]->(m:Movie { title: 'Inception' })-[:ACTED_IN]->(coActor:Person)
RETURN coActor.name AS Collègues
```

#### **6.3. Profilage d’une Requête avec PROFILE**

**Objectif :** Exécuter et afficher des informations détaillées sur la requête qui trouve les films suivis par "Alice".

```cypher
PROFILE
MATCH (a:Person { name: 'Alice' })-[:FOLLOWS]->(m:Movie)
RETURN m.title AS Film, m.released AS Année
```

### **7. Appel de Procédures**

#### **7.1. Utiliser une Procédure pour Trouver le Plus Court Chemin**

**Objectif :** Trouver le plus court chemin de connaissances entre "Alice" et "Bob".

```cypher
CALL algo.shortestPath.stream(
  'MATCH (p:Person { name: "Alice" }) RETURN id(p) AS id',
  'MATCH (p:Person { name: "Bob" }) RETURN id(p) AS id',
  'KNOWS'
)
YIELD nodeId, cost
MATCH (n) WHERE id(n) = nodeId
RETURN n.name AS Personne, cost
```

#### **7.2. Exemple Simple d’Appel de Procédure**

**Objectif :** Récupérer tous les labels présents dans la base de données.

```cypher
CALL db.labels()
YIELD label
RETURN label
```

### **8. Mise à Jour et Suppression des Données**

#### **8.1. Mise à Jour des Propriétés d’un Nœud**

**Objectif :** Mettre à jour l'année de sortie du film "Inception" à 2010.

```cypher
MATCH (m:Movie { title: 'Inception' })
SET m.released = 2010
RETURN m.title AS Film, m.released AS NouvelleAnnée
```

#### **8.2. Suppression d’un Nœud**

**Objectif :** Supprimer le film "The Matrix" de la base de données.

```cypher
MATCH (m:Movie { title: 'The Matrix' })
DELETE m
RETURN 'Film supprimé' AS Message
```

#### **8.3. Suppression d’une Relation**

**Objectif :** Supprimer la relation `FOLLOWS` entre "Alice" et "Bob".

```cypher
MATCH (a:Person { name: 'Alice' })-[r:FOLLOWS]->(b:Person { name: 'Bob' })
DELETE r
RETURN 'Relation FOLLOWS supprimée entre Alice et Bob' AS Message
```

### **9. Patterns dans Cypher**

#### **9.1. Pattern Simple**

**Exemple : Création d'un nœud simple**

```cypher
CREATE (p:Person { name: 'Brie Larson', born: 1989 })
RETURN p
```

#### **9.2. Pattern Complexe**

**Exemple : Création de relations multiples entre nœuds**

```cypher
MATCH (a:Person { name: 'Alice' }), 
      (b:Person { name: 'Bob' }), 
      (c:Person { name: 'Charlie' }), 
      (d:Interest { name: 'Graph Databases' })
CREATE (a)-[:FRIEND_OF]->(b)-[:WORKS_WITH]->(c),
       (a)-[:LIKES]->(d)
RETURN a, b, c, d
```

### **10. Création et Mise à Jour avec MERGE**

#### **10.1. Utilisation de MERGE pour Créer ou Mettre à Jour un Nœud**

**Exemple : Créer ou mettre à jour une personne**

```cypher
MERGE (a:Person { name: 'Brie Larson' })
ON CREATE SET a.born = 1989
ON MATCH SET a.stars = COALESCE(a.stars, 0) + 1
RETURN a
```

### **11. Indexation et Contraintes**

#### **11.1. Création d’Index**

**Exemple : Créer un index sur le nom des personnes et le titre des films**

```cypher
CREATE INDEX ON :Person(name)
CREATE INDEX ON :Movie(title)
```

#### **11.2. Création de Contraintes d’Unicité**

**Exemple : Assurer l’unicité des noms des personnes et des titres des films**

```cypher
CREATE CONSTRAINT ON (p:Person) ASSERT p.name IS UNIQUE
CREATE CONSTRAINT ON (m:Movie) ASSERT m.title IS UNIQUE
```

### **12. Utilisation de Paramètres dans les Requêtes**

**Exemple : Requête dynamique avec paramètres**

```cypher
MATCH (p:Person { name: $nom })
RETURN p
```

**Utilisation avec un client Cypher (ex. Neo4j Browser) :**

```json
{
  "nom": "Tom Hanks"
}
```

---

## Explications des Concepts Clés

### **a. MATCH et WHERE**

- **MATCH** : Utilisé pour spécifier le motif (pattern) à rechercher dans le graphe.
- **WHERE** : Ajoute des conditions supplémentaires pour filtrer les résultats.

### **b. RETURN**

- Spécifie les données à retourner après la correspondance du motif.

### **c. CREATE**

- Utilisé pour créer de nouveaux nœuds et relations dans le graphe.

### **d. SET**

- Permet de mettre à jour les propriétés des nœuds ou relations existants.

### **e. DELETE**

- Supprime des nœuds ou relations du graphe.

### **f. UNION**

- Combine les résultats de plusieurs requêtes en éliminant les doublons.

### **g. EXPLAIN et PROFILE**

- **EXPLAIN** : Affiche le plan d'exécution d'une requête sans l'exécuter.
- **PROFILE** : Exécute la requête et fournit des informations détaillées sur son exécution.

### **h. CALL**

- Permet d'appeler des procédures stockées ou des fonctions intégrées dans Neo4j.

### **i. DISTINCT**

- Élimine les doublons dans les résultats retournés.

### **j. Agrégation (COUNT, AVG, SUM, etc.)**

- Utilisée pour effectuer des calculs sur les données, comme compter le nombre de films par réalisateur.

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
