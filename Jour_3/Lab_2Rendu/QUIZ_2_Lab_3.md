**QUIZ 2 --- LAB 3.2**

**Data Quality Dashboard**

---

**1.  Pourquoi mesurer le taux de rejet est plus important que le nombre brut de lignes rejetées ?**

Le nombre brut n'a aucun contexte. 1000 lignes rejetées sur 10 000 =
10% de rejet, c'est grave. 1000 lignes rejetées sur 1 000 000 = 0.1%,
c'est acceptable.Le taux permet de comparer entre les jours, les
sources, les pays. Le nombre brut ne permet pas de comparaison
pertinente.

---

**2.  Si le taux de rejet passe de 3 % à 18 % en une journée, quelles hypothèses peux-tu formuler ?**

**Changement de source :**

-   Nouveau format de fichier, nouveau fournisseur, changement de schéma

**Bug upstream :**

-   Une application a commencé à envoyer des données malformées ou
    incomplètes

**Règle de validation trop stricte :**

-   Une règle a été modifiée/ajoutée côté pipeline et rejette désormais
    des cas légitimes

**Problème ponctuel :**

-   Export partiel, troncature de fichier, problème réseau en cours de
    transfert

**Données de test :**

-   Mélangées avec les données de production

---

**3.  Quelle transformation Hop permet de calculer des agrégations comme AVG, COUNT ou MAX ?**

La transformation Group By dans Apache Hop permet de regrouper les
lignes par une ou plusieurs clés et de calculer des fonctions
d'agrégation : COUNT, AVG, SUM, MIN, MAX, FIRST, LAST, etc. C'est
l'équivalent d'un GROUP BY SQL, appliqué dans le pipeline de
transformation.

---

**4.  Quelle différence entre :**

    -   COUNT(*) = compte toutes les lignes du résultat, qu'elles
        contiennent des NULL ou non. C'est le nombre de lignes total.

    -   COUNT(colonne) = compte uniquement les lignes où la colonne est
        non NULL. La différence entre les deux donne donc directement le
        nombre de valeurs manquantes dans cette colonne, ce qui en fait
        un indicateur de qualité très utile.

---

**5.  Pourquoi un dashboard de qualité doit-il être historisé ?**

Une photo instantanée ne suffit pas. L'historisation permet de :

-   **Détecter des dérives progressives** (un taux de null qui monte
    lentement de 1% à 15% en 2 semaines)

-   **Comparer avant/après** une modification du pipeline ou de la
    source

-   **Auditer** la qualité des données dans le temps pour des raisons de
    conformité

-   **Anticiper** des problèmes récurrents (ex : dégradation chaque
    lundi matin)

Sans historique, on ne sait pas si un problème est nouveau ou chronique.

---

**6.  Quel est le risque d'utiliser uniquement la moyenne pour analyser total_amount ?**

La moyenne est non robuste aux outliers. Si 99% des transactions
font entre 10€ et 100€ mais qu'une seule transaction vaut 500 000€, la
moyenne sera fortement tirée vers le haut sans refléter la réalité de la
distribution. On peut alors croire que "tout va bien en moyenne" alors
qu'il y a des anomalies majeures. Il faut compléter avec la médiane
(robuste aux extrêmes), le min/max (pour repérer les valeurs
aberrantes) et l'écart-type (pour mesurer la dispersion).

---

**7.  À partir de quel seuil une donnée devient-elle un problème ?**

Qui décide ce seuil ?

**Seuil :** Dépend du contexte métier (ex: 1% de rejets peut être acceptable
pour des logs, mais critique pour des transactions financières).

**Décision :** C'est une collaboration entre les équipes data et métiers,
en fonction des impacts (coût, risque, réglementation).

---

**8.  Quelle différence entre :**

    -   Métrique descriptive : elle observe et documente l'état des
        données sans jugement.
        Ex : "Le champ total_amount a une moyenne de 42€, un max de 9
        800€, et 2,3% de valeurs nulles." Elle sert à comprendre et
        explorer.

    -   Métrique d'alerte : elle déclenche une action (notification,
        blocage du pipeline, ticket) lorsqu'un seuil prédéfini est
        franchi.
        Ex : "Si le taux de null dépasse 5% → envoyer une alerte
        Slack." Elle sert à réagir.

---

**9.  Pourquoi exporter les métriques dans une table SQL peut être préférable à un CSV ?**

Le CSV est simple mais limité : il est difficile à requêter, non
sécurisé, écrasé à chaque export et peu adapté à l'historisation. Une
table SQL offre :

-   **Requêtes et filtres :** directement sur l'historique (ex :
    "donne-moi le taux de rejet des 30 derniers jours")

-   **Jointures :** avec d'autres tables (ex : corréler la qualité avec
    un événement métier)

-   **Gestion des droits d'accès :** fine par rôle

-   **Intégration native :** dans d'autres pipelines ou outils de BI
    (Metabase, Grafana, etc.)

-   **Scalabilité :** pour des millions de métriques sans perte de
    performance

---

**10. Si les données sont propres mais incohérentes d'un point de vue métier, le dashboard le détectera-t-il ? Pourquoi ?**

Non, pas automatiquement. Le dashboard de qualité technique détecte des
anomalies structurelles : valeurs nulles, mauvais formats, types
incorrects, doublons. Mais des incohérences métier comme "un
remboursement supérieur au montant de la commande", "une date de
livraison antérieure à la date de commande" ou "un client avec un âge
de 4 ans qui a souscrit un crédit" sont techniquement valides --- les
champs sont renseignés, les types sont corrects. Pour les détecter, il
faut coder explicitement des règles métier dans le pipeline (ex:
refund_amount <= order_amount). Cela souligne que la qualité des
données est autant une responsabilité métier que technique.

---