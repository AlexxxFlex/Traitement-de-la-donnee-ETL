**QUIZ 1 — LAB 3.1**


**Pipeline robuste Taxi** 


**1. Quelle est la différence fondamentale entre une erreur de transformation et une ligne invalide métier ?** 

Une erreur de transformation est un échec technique empêchant le traitement. Une ligne invalide métier est techniquement traitable mais viole une règle fonctionnelle.

---

**2. Pourquoi est-il dangereux de laisser passer des total\_amount < 0 dans un pipeline de production ?** 

Cela fausse les agrégations financières (sommes, moyennes), corrompt les rapports décisionnels et induit des pertes de revenus invisibles.

---

**3. Explique la différence entre :** 

   **- un rejet technique :** Donnée mal formatée ou illisible. Exemple : Une date reçue sous la forme "2023-02-30" (date inexistante).

 

   **- un rejet métier :** Donnée techniquement correcte mais incohérente. Exemple : Un trajet de taxi avec une distance de 0 km mais un coût de 50€.

---

---

**4. Pourquoi un pipeline doit-il être rejouable (idempotent) ?** 

Donne un scénario réel où cela devient critique. 

Un pipeline doit être idempotent pour garantir que plusieurs exécutions successives sur les mêmes données produisent le même résultat final sans doublons.

**Scénario :** Le serveur plante au milieu du traitement. On doit pouvoir relancer le pipeline complet sans dupliquer les lignes déjà insérées avant le crash.

---

**5. Que se passe-t-il si le schéma du CSV change (nouvelle colonne, colonne supprimée) ?** 

Comment anticiper cela dans Hop ? 

Le pipeline risque d'échouer (erreur de mapping) ou de décaler les valeurs.

**Anticipation Hop :** Utiliser la gestion d'erreurs sur l'étape d'entrée ("Error handling") et éviter le mappage positionnel strict au profit du mappage par nom de colonne.

---

**6. Pourquoi est-il préférable de séparer les flux "clean" et "rejected" plutôt que de simplement supprimer les lignes invalides ?** 

Supprimer les lignes invalides entraîne une perte de données sèche ("data loss"). Séparer les flux permet l'auditabilité, le débogage et la récupération potentielle des données après correction.


**7. Quelle transformation Hop permet de filtrer les lignes selon une condition logique ?** 

La transformation "Filter rows".

---

**8. Dans une architecture professionnelle, qui définit les règles de validation :** 

   1. le data engineer 

 

   1. le métier 

 

   1. les deux ? 

   Explique. 

Les deux. Le métier définit la règle ("le montant ne peut pas être négatif") et le Data Engineer définit l'implémentation technique et les seuils de tolérance. 

---

**9. Pourquoi ajouter un processing\_timestamp peut être stratégique ?** 

Il permet la traçabilité (savoir quand la donnée a été intégrée), facilite l'audit et est indispensable pour les architectures incrémentales ou le chargement de type SCD (Slowly Changing Dimensions).

---

**10. Si ton pipeline traite 10 millions de lignes, quelle étape risque d’être la plus coûteuse en performance ?** 


Les opérations bloquantes nécessitant de stocker tout le dataset en mémoire ou sur disque avant de continuer, principalement le "Sort rows" (Tri) ou les jointures cartésiennes non optimisées.

---