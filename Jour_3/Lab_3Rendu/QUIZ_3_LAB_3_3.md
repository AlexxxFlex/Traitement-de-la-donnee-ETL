# QUIZ 3 — LAB 3.3 : Logging avancé & Audit — Réponses

---

**1. ERROR / WARNING / INFO**
- **ERROR** : quelque chose a échoué, le pipeline s'arrête ou rejette des lignes
- **WARNING** : anomalie détectée mais le pipeline continue (ex: valeur nulle)
- **INFO** : message informatif normal, tout se passe bien

---

**2. Pipeline qui "ne plante pas" mais défectueux**

Parce qu'un filtre trop strict ou une jointure qui ne matche pas peut silencieusement éliminer des lignes sans générer d'erreur. Le résultat est incomplet sans que Hop ne se plaigne.

---

**3. Identifier la transformation la plus lente**

Dans l'onglet **Metrics** du panneau d'exécution, regarder la colonne **Duration** — le step avec le temps le plus élevé est le goulot.

---

**4. Chute soudaine de lignes sans erreur**

C'est un filtre trop agressif, une jointure sans correspondance (LEFT JOIN qui exclut), ou une condition de validation qui rejette des lignes silencieusement.

---

**5. Logs indispensables la nuit**

Personne ne surveille le pipeline. En cas d'échec, le log est la seule trace disponible pour comprendre ce qui s'est passé et à quel moment.

---

**6. Log système vs audit métier**
- **Log système** : technique, destiné au dev/ops (erreurs, durées, stack traces)
- **Audit métier** : fonctionnel, destiné au métier (combien de lignes traitées, rejetées, quand, par quel pipeline)

---

**7. Pourquoi simuler une erreur volontairement**

Pour apprendre à lire les messages d'erreur dans un contexte maîtrisé, et savoir exactement où chercher dans les logs quand une vraie erreur survient en production.

---

**8. Détecter un goulot d'étranglement**

Onglet Metrics → colonne Duration → le step le plus lent est le goulot. On peut aussi comparer le ratio lignes entrantes / lignes sortantes par step.

---

**9. Centraliser les logs**

Pour avoir une vue unifiée sur tous les pipelines, pouvoir corréler des événements entre eux, alerter automatiquement, et conserver un historique sans chercher dans des dizaines de fichiers dispersés.

---

**10. Informations minimales dans l'audit (2h du matin)**
- Timestamp de début et de fin
- Nom du pipeline
- Nombre de lignes traitées / rejetées
- Code et message d'erreur
- Step où l'erreur s'est produite
