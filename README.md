# Atelier Préparation de Données Images

Préparation d'un dataset d'images de déchets (`cardboard`, `glass`, `metal`, `paper`, `plastic`, `trash`) en vue de l'entraînement d'un modèle de classification pour améliorer le tri sélectif.

Les images brutes proviennent de plusieurs sources et présentent des problèmes typiques d'un jeu de données réel : dimensions et formats différents, images en niveaux de gris ou RGBA, images trop petites, corrompues, vides, dupliquées ou mal classées, et classes déséquilibrées. L'objectif est de construire, à partir de ce jeu brut, un dataset propre et homogène.

## Structure du projet

```
atelier_prepa_donnees_images/
│
├── notebooks/
│   └── atelier_prepa_donnees_images.ipynb   # tout le pipeline de préparation
├── reports/
│   └── audit_images.csv                     # résultats de l'audit du dataset
└── data/
    ├── raw/          # dataset original, en lecture seule (cardboard, glass, metal, paper, plastic, trash)
    ├── cleaned/       # images nettoyées : redimensionnées 224x224, uniformisées en RGB
    └── augmented/     # images générées par data augmentation pour la classe minoritaire
```

`data/raw/` ne doit jamais être modifié : c'est la référence brute. Les dossiers `data/cleaned/` et `data/augmented/` sont générés par le notebook.

## Contenu du notebook

Le notebook `atelier_prepa_donnees_images.ipynb` suit les étapes suivantes :

1. **Exploration** : extraction pour chaque image de son nom, sa classe, son format, son mode, ses dimensions, l'écart-type de ses pixels, son nombre de canaux et sa taille.
2. **Images corrompues** : détection des fichiers illisibles.
3. **Images vides** : détection des images entièrement noires, blanches, ou à très faible variation.
4. **Résolutions** : analyse des dimensions (min, max, les plus fréquentes) et détection des images sous 64×64 pixels.
5. **Canaux** : répartition des images selon leur nombre de canaux (grayscale, RGB, RGBA).
6. **Doublons** : détection des images strictement identiques (même contenu pixel), même si leur nom de fichier diffère.
7. **Images mal classées** : contrôle visuel ciblé sur les doublons présents dans plusieurs classes à la fois.
8. **Déséquilibre des classes** : nombre d'images par classe.
9. **Redimensionnement** : toutes les images ramenées à 224×224, proportions conservées, padding ajouté si besoin.
10. **Uniformisation des canaux** : toutes les images converties en RGB.
11. **Normalisation** : valeurs des pixels ramenées entre 0 et 1.
12. **Découpage train/validation/test** : split stratifié (70/15/15) du dataset nettoyé.
13. **Data augmentation** : rotation, retournement, zoom, translation, luminosité, contraste appliqués (avec Keras) à la classe minoritaire, uniquement sur le train.

## Installation

```bash
pip install pillow numpy pandas scikit-learn tensorflow
```

## Utilisation

1. Placer les images du dataset dans `data/raw/<classe>/`.
2. Ouvrir et exécuter `notebooks/atelier_prepa_donnees_images.ipynb` dans l'ordre des cellules.
3. Les résultats de l'audit sont consignés dans `reports/audit_images.csv`, les images nettoyées dans `data/cleaned/`.
