# QUIZ Cypher — 10 Questions & Réponses

---

**1. Quelle différence entre CREATE et MERGE ?**

`CREATE` crée toujours un nouveau nœud/relation, même s'il existe déjà → risque de doublons. `MERGE` vérifie d'abord si l'élément existe, et ne le crée que s'il est absent → idempotent et sûr pour les ETL.

---

**2. Pourquoi Cypher est un langage déclaratif ?**

On décrit **ce qu'on veut** (le résultat), pas **comment l'obtenir** (les étapes). C'est le moteur Neo4j qui détermine le plan d'exécution optimal, comme SQL pour les bases relationnelles.

---

**3. Que signifie MATCH (a)-[:REL]->(b) ?**

On cherche tous les nœuds `a` qui ont une relation de type `REL` pointant vers un nœud `b`. La flèche `->` indique le sens de la relation.

---

**4. Quelle différence entre COUNT(*) et COUNT(DISTINCT) ?**

`COUNT(*)` compte toutes les lignes y compris les doublons. `COUNT(DISTINCT x)` compte uniquement les valeurs uniques de `x`. Exemple : un driver qui fait 3 trajets vers Queens → `COUNT(*)` = 3, `COUNT(DISTINCT zone)` = 1.

---

**5. Pourquoi WITH est-il nécessaire ?**

`WITH` permet de **chaîner des étapes** dans une requête Cypher — il transmet les résultats intermédiaires à la suite de la requête, comme un pipe. Sans lui, on ne peut pas filtrer ou agréger entre deux `MATCH`.

---

**6. Comment éviter les doublons dans un ETL graphe ?**

En utilisant `MERGE` au lieu de `CREATE`, et en créant des **contraintes d'unicité** (`CREATE CONSTRAINT`) sur les propriétés clés. C'est exactement ce qu'on fait dans un pipeline Hop → Neo4j.

---

**7. Quelle différence entre relation SQL et relation graphe ?**

En SQL, une relation est une **jointure implicite** entre tables via des clés étrangères, coûteuse à calculer. En graphe, la relation est un **lien physique direct** entre nœuds, stocké et parcouru nativement sans jointure → beaucoup plus performant pour les données connectées.

---

**8. Pourquoi les graphes sont puissants pour la fraude ?**

La fraude se cache dans les **patterns de connexions** entre entités (comptes, personnes, transactions). Les graphes permettent de traverser ces connexions en quelques étapes là où SQL nécessiterait des dizaines de jointures imbriquées.

---

**9. Que fait DETACH DELETE ?**

Supprime un nœud **et toutes ses relations** en même temps. Sans `DETACH`, Neo4j refuse de supprimer un nœud qui a encore des relations (pour maintenir la cohérence du graphe).

---

**10. Pourquoi un graphe est plus naturel pour modéliser des réseaux ?**

Parce que le monde réel est fait de **connexions** — réseaux sociaux, transports, fraude, recommandations. Un graphe modélise directement ces liens comme des entités de première classe, alors qu'un modèle relationnel les force dans des tables qui ne reflètent pas intuitivement la structure des données.