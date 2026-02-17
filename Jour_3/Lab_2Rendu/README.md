# Lab_2 — Calcul du taux de rejet

Résumé rapide :
- Pipeline Hop : [`Lab2`](Lab2.hpl) — calcule le nombre d'entrées propres / rejetées et le taux de rejet.
- Données d'entrée : [data.csv](data.csv)
- Questionnaire / notes : [QUIZ 2 — LAB 3.md](Jour_3/Lab_2Rendu/QUIZ 2 — LAB 3.md)

Exécution :
- Ouvrir [`Lab2`](Lab2.hpl) dans Apache Hop.
- Lancer la pipeline. Les sorties sont écrites dans les répertoires de destination définis dans la pipeline (clean / rejected / totals).

Points importants :
- Le filtrage se fait dans la transform "Filter rows" (conditions sur total_amount, passenger_count, dates).
- Les agrégations utilisent des transforms Group By pour total_clean / total_rejected.
- Le taux est calculé par deux transforms Calculator (somme → division).

Astuce :
- Vérifier les chemins ${PROJECT_HOME} dans le fichier si les fichiers de sortie ne s'écrivent pas au bon endroit.

Fin.

Ressources :
- Capture d'écran du pipeline : [`Lab2`](pipeline_Lab_2.png)

- Réponses au quiz : [`Lab2`](QUIZ_2_Lab_3.md)