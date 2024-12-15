# Atelier Neo4j: Movie Graph

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

## 3. Requêtes de base avec Cypher

Cypher est le langage de requêtes de Neo4j. Voici quelques exemples pour commencer :

### Visualiser le schema du graphe:
```
// What is related, and how
CALL db.schema.visualization()
```
### Découvrir le graphe: 
```
// List node labels
CALL db.labels()
```
```
// List relationship types
CALL db.relationshipTypes()
```
### Trouver un acteur :
```cypher
MATCH (a:Person {name:'Tom Hanks'}) RETURN a
```

### Trouver un film :
```cypher
MATCH (m:Movie {title:'The Matrix'}) RETURN m
```

### Liste des films d'un acteur :
```cypher
MATCH (a:Person {name:'Tom Hanks'})-[:ACTED_IN]->(m:Movie) RETURN m
```

---

<a name="manipulation-des-donnees"></a>

## 4. Manipulation des données

<a name="ajout-de-donnees"></a>
### Ajout de données

#### Ajouter un acteur :
```cypher
CREATE (a:Person {name:'Brie Larson', born:1989}) RETURN a
```

#### Ajouter un film :
```cypher
CREATE (m:Movie {title:'Captain Marvel', released:2019, tagline:'Everything begins with a (her)o.'}) RETURN m
```

#### Créer une relation entre un acteur et un film :
```cypher
MATCH (a:Person {name:'Brie Larson'}), (m:Movie {title:'Captain Marvel'})
MERGE (a)-[r:ACTED_IN]->(m) SET r.roles = ['Carol Danvers'] RETURN a,r,m
```

<a name="suppression-de-donnees"></a>
### Suppression de données

#### Supprimer un acteur :
```cypher
MATCH (a:Person {name:'Brie Larson'}) DETACH DELETE a
```

#### Supprimer un film :
```cypher
MATCH (m:Movie {title:'Captain Marvel'}) DETACH DELETE m
```

<a name="mise-a-jour-et-merge"></a>
### Mise à jour et MERGE

#### Ajouter ou mettre à jour un acteur :
```cypher
MERGE (a:Person {name:'Brie Larson'})
ON CREATE SET a.born = 1989
ON MATCH SET a.stars = COALESCE(a.stars, 0) + 1
RETURN a
```

---

<a name="requetes-avancees"></a>

## 5. Requêtes avancées

<a name="recommandations"></a>
### Recommandations et recherche complexe

#### Recommander de nouveaux co-acteurs :
```cypher
MATCH (a:Person {name:'Tom Hanks'})-[:ACTED_IN]->(m)<-[:ACTED_IN]-(coActors),
      (coActors)-[:ACTED_IN]->(m2)<-[:ACTED_IN]-(cocoActors)
WHERE NOT (a)-[:ACTED_IN]->()<-[:ACTED_IN]-(cocoActors) AND a <> cocoActors
RETURN cocoActors.name AS Recommended, count(*) AS Strength ORDER BY Strength DESC
```

<a name="chemin-de-bacon"></a>
### Chemin de Bacon

#### Trouver le chemin le plus court entre Kevin Bacon et un autre acteur :
```cypher
MATCH p=shortestPath(
              (bacon:Person {name:'Kevin Bacon'})-[*]-(a:Person {name:'Al Pacino'})
            )
RETURN p
```

---

<a name="ressources"></a>

## 6. Ressources supplémentaires

- [Manuel du développeur Neo4j](https://neo4j.com/docs/)
- [Guide Cypher](https://neo4j.com/developer/cypher/)
- [Exemples pratiques](https://neo4j.com/graphgists/)

Amusez-vous bien avec Neo4j et la Movie Database !
