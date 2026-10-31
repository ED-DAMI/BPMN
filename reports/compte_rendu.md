# TP 2 — Process mining avec PM4Py

## Introduction
Ce travail analyse un processus de demandes de congé à partir d'un journal d'événements et d'un modèle BPMN de référence.
Objectifs : décrire le journal, découvrir le processus réel, mesurer la conformité et analyser la performance.
Fichiers utilisés : `data/demande_conge.csv` et `data/modele_reference_conge.bpmn`.

## Méthodologie
1. Chargement et contrôle qualité des données (colonnes, valeurs manquantes, timestamps, doublons).
2. Découverte de processus (DFG et BPMN avec/sans filtrage).
3. Vérification de conformité (fitness, précision, alignements).
4. Analyse de performance (durées, DFG de performance, délais avant validation manager).

## Q1 — Statistiques du journal
Nombre de cas, événements, activités et variantes : calculés dans le notebook (Partie A).
Les nombres exacts doivent provenir d'une exécution avec les fichiers présents dans `data/`.

Activités absentes du modèle de référence : à vérifier par confrontation explicite entre activités observées et modèle, sans confondre :
- activité absente du modèle ;
- activité présente mais exécutée à un endroit inattendu.

## Q2 — Deux variantes les plus fréquentes
Effectif et proportion des deux premières variantes : calculés en Partie A.4.
Comparer les séquences réellement observées.
Si la différence principale porte sur l'ordre de « Enregistrer le congé » et « Mettre à jour le planning », interpréter cela comme un possible parallélisme opérationnel, à confirmer par le modèle découvert.

## Q3 — Fitness et conformité
Référence annoncée (à comparer au recalcul) :
- Fitness : 0.9129
- Traces parfaitement conformes : 63.8 %

Rappel : la fitness globale et le pourcentage de cas parfaitement conformes sont deux indicateurs différents.

## Q4 — Classification des écarts
- Validation absente avant enregistrement : écart potentiellement grave, sous hypothèse de complétude du journal.
- Relances : potentiellement acceptables (gestion d'attente).
- Refus, modification et revalidation : potentiellement acceptables selon règles métier.
- Annulations : à vérifier selon les effets sur enregistrement/planning.

Prévention côté Camunda (principe) :
- tâche d'examen manager obligatoire ;
- contrôle explicite de la décision ;
- seule la branche approuvée autorise l'enregistrement ;
- refus dans une branche dédiée ;
- pas de contournement externe de l'état du processus.

Terminer une tâche ne suffit pas à prouver une approbation : la décision doit être vérifiée explicitement.

## Q5 — Performance et amélioration
Références annoncées (à comparer au recalcul) :
- Moyenne : 2.69 jours
- Médiane : 1.81 jour
- Maximum : 12.2 jours

Interprétation attendue :
- objectif de 2 jours non respecté en moyenne si 2.69 jours est confirmé ;
- attente avant intervention manager comme goulot potentiel ;
- délai moyen de M3 supérieur aux autres sans conclusion causale.

Amélioration proposée : minuterie non interruptive sur la tâche de validation
(rappel à 24h, puis escalade/délégation), en conservant l'obligation d'approbation.

## Limites
- Complétude du journal non garantie.
- Qualité et granularité des timestamps.
- Effets du filtrage sur les modèles découverts.
- Limites d'interprétation des alignements et des indicateurs temporels.

## Conclusion
Le notebook fournit un pipeline reproductible pour répondre aux Q1–Q5, avec séparation claire entre valeurs recalculées et valeurs de référence communiquées.

Si les fichiers source sont absents :
**« Valeur communiquée lors d’une exécution précédente, non recalculée dans cette session. »**
