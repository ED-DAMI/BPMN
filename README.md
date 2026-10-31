# TP 2 — Process mining avec PM4Py (demandes de congé)

## Objectif du projet
Ce dépôt contient un rendu pédagogique de process mining avec PM4Py sur un processus de demandes de congé : exploration du journal, découverte de processus, contrôle de conformité et analyse de performance.

## Arborescence

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   ├── demande_conge.csv                # à fournir
│   └── modele_reference_conge.bpmn      # à fournir
├── notebooks/
│   └── tp2_process_mining.ipynb
├── reports/
│   ├── compte_rendu.md
│   └── figures/
└── notbooks/
    └── tp1.ipynb                        # notebook historique conservé
```

## Données nécessaires
Le notebook principal attend :
- `data/demande_conge.csv`
- `data/modele_reference_conge.bpmn`

Le fichier CSV doit contenir les colonnes :
- `case_id`
- `activity`
- `timestamp`
- `resource`

Aucune donnée fictive n'est générée en cas d'absence de ces fichiers.

## Environnement Python

### 1) Créer un environnement virtuel
```bash
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# .venv\Scripts\activate   # Windows PowerShell
```

### 2) Installer les dépendances Python
```bash
pip install -r requirements.txt
```

## Graphviz (système)
`requirements.txt` installe les bibliothèques Python, mais **pas** le binaire système `dot`.

- Windows : installer Graphviz depuis le site officiel puis ajouter `dot` au `PATH`.
- Linux (Debian/Ubuntu) : `sudo apt-get install graphviz`
- Colab : `!apt-get update && apt-get install -y graphviz`

Le notebook vérifie `shutil.which("dot")` et affiche un message clair si Graphviz est absent.

## Lancement du notebook
Depuis la racine du dépôt :
```bash
jupyter notebook notebooks/tp2_process_mining.ipynb
```

Le notebook peut aussi être lancé depuis `notebooks/` : la racine du projet est détectée automatiquement sans changer silencieusement le répertoire courant.

## Ordre d'exécution
1. Redémarrer le noyau.
2. Exécuter toutes les cellules de haut en bas.
3. Vérifier les fichiers produits dans `reports/figures/` et les synthèses dans `reports/compte_rendu.md`.

## Emplacement des résultats
- Figures : `reports/figures/`
- Compte rendu : `reports/compte_rendu.md`

## Limites principales
- Les conclusions dépendent de la complétude du journal et de la qualité des timestamps.
- Le filtrage (`noise_threshold`) influence les modèles découverts.
- Les durées calculées sont calendaires sauf traitement explicite des jours ouvrés.
- Les écarts d'alignement indiquent des divergences au modèle, pas une causalité métier.
