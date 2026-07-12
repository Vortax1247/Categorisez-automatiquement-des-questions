Catégorisation automatique de questions Stack Overflow (NLP)

🔗 Repo : github.com/Vortax1247/Categorisez-automatiquement-des-questions

Système de suggestion automatique de tags pour Stack Overflow, sous forme d'un algorithme de machine learning multi-label assignant des tags pertinents à une question, avec comparaison d'approches supervisées et non supervisées.

Contexte

Sur Stack Overflow, poser une question nécessite de renseigner plusieurs tags pour la rendre facilement retrouvable. Si cet exercice ne pose pas de problème aux utilisateurs expérimentés, les nouveaux utilisateurs bénéficieraient d'une suggestion automatique de tags pertinents.

Objectif : développer un système suggérant automatiquement plusieurs tags pertinents à partir du texte d'une question (titre + corps).

Contraintes du projet :


Extraction de features spécifiques aux données textuelles
Comparaison d'une approche non supervisée (proposition de mots-clés) et d'une approche supervisée
Pour l'approche supervisée, comparaison d'au moins 4 méthodes d'extraction de features : bag-of-words (TF-IDF), puis 3 approches d'embedding — Word2Vec, BERT, USE (Universal Sentence Encoder)
Évaluation avec séparation propre train/test
Gestion de versions du code final avec Git
Mise à disposition d'un point d'entrée d'API pour tester le modèle en production


Données


Source : export StackExchange Data Explorer, requêtes SQL sur la base Stack Overflow
Filtrage appliqué : questions avec au moins 5 tags, ayant reçu une réponse, avec un score, un nombre de vues et de favoris significatifs (pour ne conserver que des questions pertinentes et bien taguées) 

'SELECT TOP 500000 Title, Body, Tags, Id, Score, ViewCount, FavoriteCount, AnswerCount

FROM Posts

WHERE PostTypeId = 1 AND ViewCount > 10 AND FavoriteCount > 10

AND Score > 5 AND AnswerCount > 0 AND LEN(Tags) - LEN(REPLACE(Tags, '<','')) >= 5'

Variables : titre, corps de la question, tags associés


Méthodologie


Analyse exploratoire — analyse univariée et multivariée, réduction dimensionnelle pour visualiser les données textuelles à grande dimension.
Approche non supervisée — proposition de mots-clés à partir du contenu de la question (ex. topic modeling).
Approche supervisée — comparaison de 4 méthodes d'extraction de features :

TF-IDF (bag-of-words)
Word2Vec
BERT
USE (Universal Sentence Encoder)


Pour chaque méthode d'embedding, plusieurs classifieurs testés (SVC, SGD, régression logistique, Random Forest), optimisés par GridSearchCV avec une stratégie One-vs-Rest (classification multi-label). Évaluation par Hamming score.
Déploiement — mise en place d'un point d'entrée d'API pour tester le modèle final.


Résultats

Comparaison des méthodes d'extraction de features (train vs test score) :

MéthodeScore entraînementScore testTF-IDF~0.82~0.39Word2Vec~0.60~0.57BERT~0.90~0.54USE~0.81~0.68

USE (Universal Sentence Encoder) est le modèle d'embedding le plus performant, avec le meilleur score en test et le meilleur compromis entraînement/test (moins de sur-apprentissage que BERT, bien plus performant que TF-IDF). Le classifieur retenu sur cette base est un Linear SVC.

Test de plusieurs classifieurs avec TF-IDF (SVC, SGD, régression logistique, Random Forest) : le SVC obtient le meilleur score d'entraînement, mais souffre d'un fort sur-apprentissage (score test nettement inférieur) — confirmant l'intérêt de passer par un embedding plus riche que le TF-IDF plutôt que de complexifier le classifieur sur des features bag-of-words.

Structure du repo

├── HUMMEL_Adrien_1_notebook_exploration_042023.ipynb   # analyse exploratoire (non nettoyé)
├── HUMMEL_Adrien_2_notebook_test_052023.ipynb           # test des différentes approches (non nettoyé)
├── HUMMEL_Adrien_3_code_052023.py                       # code final à déployer
├── HUMMEL_Adrien_4_point_entree_API_052023              # point d'entrée de l'API
├── Présentation StackOverflow.pdf                       # support de soutenance
└── README.md

Lancer le projet

bashgit clone https://github.com/Vortax1247/Categorisez-automatiquement-des-questions.git
cd Categorisez-automatiquement-des-questions
pip install -r requirements.txt
jupyter notebook HUMMEL_Adrien_1_notebook_exploration_042023.ipynb

Pistes d'amélioration


fine-tuner BERT plutôt que l'utiliser en extracteur de features figé, pour réduire l'écart train/test observé
élargir le jeu de données au-delà des contraintes de filtrage SQL pour améliorer la généralisation
