# Classification d'images (Deep Learning)

Modèle de classification d'images par CNN, avec Transfer Learning et fine-tuning.

## Contexte

*[À compléter : quelles images ? Combien de classes ? Quel est l'objectif métier — reconnaissance de produits, tri automatique, détection de défauts ?]*

## Données

- Source : *[nom du dataset / origine]*
- Taille : *[nombre d'images, nombre de classes, répartition train/val/test]*

## Méthodologie

1. **Préparation des données** — redimensionnement, normalisation, augmentation de données (rotation, flip, zoom...).
2. **Transfer Learning** — utilisation d'`EfficientNet` pré-entraîné sur ImageNet comme extracteur de features, couches de classification ré-entraînées sur le dataset cible.
3. **Fine-tuning** — dégel progressif des dernières couches du réseau pré-entraîné pour affiner la représentation aux spécificités du dataset.

## Résultats

| Métrique | Valeur |
|---|---|
| Accuracy (test) | *[valeur]* |
| *[F1-score / autre métrique pertinente]* | *[valeur]* |

*[Ajoutez ici : matrice de confusion, courbe d'apprentissage (loss/accuracy par epoch), exemples de prédictions correctes/erronées]*

## Structure du repo

```
├── data/
├── notebooks/
├── src/
├── requirements.txt
└── README.md
```

## Lancer le projet

```bash
git clone https://github.com/Vortax1247/[nom-du-repo].git
cd [nom-du-repo]
pip install -r requirements.txt
jupyter notebook notebooks/classification_images.ipynb
```

## Pistes d'amélioration

- *[ex. tester d'autres architectures pré-entraînées (ResNet, ViT)]*
- *[ex. déployer le modèle via une API légère (FastAPI, BentoML)]*
