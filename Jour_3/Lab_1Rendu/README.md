# LAB 3.1 — Pipeline robuste Taxi

# 📌 Lab 1 — Nettoyage & Préparation des Données

---

🎯 Objectif
- Construire un pipeline Apache Hop pour ingérer, nettoyer, valider et séparer un dataset Taxi (CSV) en flux "clean" et "rejected".

---

📁 Structure attendue
- data/
  - raw/
  - clean/
  - rejected/

---

Créer rapidement :
```bash
mkdir -p data/raw data/clean data/rejected
```

---

⚙️ Étapes clés
1. Ingestion
   - `CSV File Input` → lire depuis data/raw (UTF‑8, séparateur correct).
2. Typage & normalisation
   - Forcer types, convertir strings → dates, normaliser décimales.
3. Validation (Filter rows)
   - Règles minimales :
     - total_amount > 0
     - passenger_count > 0
     - pickup_datetime IS NOT NULL
     - dropoff_datetime IS NOT NULL
   - Flux vrai → data/clean, faux → data/rejected (ajouter error_reason).
4. Gestion d’erreurs
   - Activer Error Handling sur étapes sensibles (conversion).
   - Renvoyer les lignes fautives vers `rejected` avec diagnostic.
5. Export
   - clean → data/clean/taxi_clean.csv
   - rejected → data/rejected/taxi_rejected.csv

---

🧾 Bonnes pratiques
- Séparer clean/rejected pour audit et correction.
- Rendre le pipeline idempotent (éviter doublons).
- Ajouter colonnes de diagnostic : `error_reason`, `error_step`, `processing_timestamp`, `batch_id`.

---

Exécution rapide
1. Ouvrir Apache Hop.
2. Charger la transformation : [Lab_1Rendu/Lab1.hpl](Lab_1Rendu/Lab1.hpl).
3. Lancer. Vérifier `data/clean/` et `data/rejected/`.

Références et notes
- Pipeline source : [Lab_1Rendu/Lab1.hpl](Lab_1Rendu/Lab1.hpl)  
- Quiz, captures et réponses : [Pipeline Lab_1 (capture d'écran)](Lab_1Rendu/pipeline_lab_1.png) — capture d'écran montrant la pipeline ; [resultat_apres_filtrage (capture d'écran)](Lab_1Rendu/resultat_apres_filtrage.png) — capture d'écran montrant le résultat obtenu lors de l'exécution de la pipeline. Réponses au QUIZ : [Lab_1Rendu/QUIZ_1_Lab3_Reponses.md](Lab_1Rendu/QUIZ_1_Lab3_Reponses.md)

---
# Traitement-de-la-donnee-ETL
# Traitement-de-la-donnee-ETL
# Traitement-de-la-donnee-ETL
