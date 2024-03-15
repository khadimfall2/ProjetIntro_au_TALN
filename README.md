# DEFT2013 Tâche 2 : YNWA


| **Baye Lahad Mbacke M1 ATAL** | **Khadim Fall M1 DS**
|----------------|----------------|
## Description de la tâche
    La tâche consiste à classer automatiquement les recettes de cuisine en trois catégories : Plat principal, Entrée ou Dessert, en se basant sur le titre et les instructions de la recette.
## Exemple de document 1 :
**Identifiant** : recette_170277.xml  
**Titre** : Langue de boeuf sauce piquante aux cornichons  
**Type** : Plat principal  
**Difficulté** : Facile  
**Coût** : Moyen  
**Ingrédients** :
- 1 langue de 1,5 kg - 20 cl de vinaigre d'alcool  - 1 oignon piqué de 3 - clous de girofle - 2 bouillons cubes - 1 branche de thym - 5 feuilles de laurier - sel - poivre - 50 g de beurre - 1 oignon émincé - 3 cuillères à soupe de farine - 100 g de cornichons - 1 cuillère à café d'arôme Patrelle ou du concentré de tomate"
 
**Recette** : Faire tremper la langue toute une nuit dans une cocotte remplie d'eau pour la dégorger. Dans une cocotte minute, mettre la langue, couvrir d'eau, ajouter la moitié du vinaigre. Faire bouillir pendant 15 min. Egouter. Pendant ce temps, faire bouillir de l'eau. Remettre la langue dans la cocotte, y verser l'eau bouillante, le reste du vinaigre, 1 oignon piqué de 3 clous de girofle, 2 bouillon cubes, le thym, le laurier, sel et poivre. Laisser cuire 1h30 à partir de l'ébullition. Retirer la langue de la cocotte et ôter la peau, garder le bouillon. Sauce : faire fondre le beurre dans une grande casserole, ajouter un oignon émincé, faire fondre, ajouter la farine. Puis ajouter petit à petit environ 1 l de bouillon de cuisson, 1 cuillère à café d'arôme Patrelle pour coloré la sauce. Goûter la sauce. Ajouter sel et poivre si besoin, les cornichons coupés en rondelles. Dans un plat de service, déposer la langue coupée en tranches et napper de sauce. Bonne appétit.Ce plat est encore meilleur réchauffé.


## Exemple de document 2 :
**Identifiant** : recette_21519.xml  
**Titre** : Tarte aux raisins et amandes  
**Type** : Dessert  
**Difficulté** : Facile  
**Coût** : Moyen  
**Ingrédients** :
- 1 pâte brisée - 150 g de raisins bien mûrs - 3 oeufs - 100 g de sucre - 3 cuillères à soupe de crème fraîche - 60 g de poudre d'amandes"

**Recette** : Garnissez une tourtière avec la pâte étalée. Faites-la cuire à blanc, 10 min à 180°C (thermostat 6). Répartissez les grains de raisins lavés et séchés dessus. Fouettez les oeufs avec le sucre, la crème et la poudre d'amandes. Versez sur les raisins et faites cuire 30 min au four à 200°C (thermostat 6-7).



## Statistiques corpus

**Nombre de documents de train/dev/test :**

|   Dataset     | nombre :|
|---------------|--------|
| Entraînement  |  9978  |
| dev           |  2495  |
| Test          |  1388  |

**Répartition des étiquettes dans chacun des sous-ensembles :**
|   Dataset   | Plat principal (%) |Dessert (%)  | Entrée (%)  |
|-------------|----------------|--------|---------|
| Entraînement| 4644 (46.54%)         | 3036 (30.43%)  | 2298 (23.03%)   |
| dev         | 1158 (46.41% )        | 726 (29.10% )  | 611 (24.49% )   |
| Test        | 644 (46.40% )         | 407 (29.32%)   | 337 (24.28%)   |

## Méthodes proposées

### Run1: baseline (méthode de référence)

	Nous utilisons un classificateur dummy qui prédit les classes de manière aléatoire. Cette méthode est utilisée comme point de comparaison de base pour évaluer les performances des autres méthodes plus sophistiquées
	Aucun descripteur spécifique n'est utilisé dans cette méthode. Le classificateur dummy se contente de tirer aléatoirement des prédictions sans se baser sur les caractéristiques des données.
	DummyClassifier de scikit-learn avec la stratégie 'stratified'. Cette stratégie génère des prédictions en respectant la distribution des classes dans l'ensemble de données d'entraînement.


## NB:(Nettoyage et Hyperparametres)
On a nettoyé les données en supprimant les caractères non alphabétiques, en tokenisant et en enlevant la casse avec simple_preprocess. On avait créé notre propre fonction pour tokenizer et supprimer la casse, mais notre fonction n'était pas trop efficace. On a aussi enlevé les stop words avec stop_words de nltk, on a également appliqué le stemming des mots avec FrenchStemmer de nltk, et enfin, on a lemmatisé les mots avec la librairie Spacy.

Avec les méthodes suivantes, nous avons utilisé le classificateur SVM avec le noyau gaussien car c'est celui qui a donné les meilleurs résultats par rapport à la régression logistique, Random Forest, Gradient Boosting, AdaBoost et Naive Bayes que nous avons testés, avec les paramètres gaussien (rbf) et C=5, car après avoir testé les hyperparamètres avec GridSearchCV de scikit-learn, c'est celui qui a donné les meilleurs résultats.
### Run2: TF-IDF

    Pour la méthode Run2, nous utilisons la représentation TF-IDF pour la vectorisation des données textuelles.
    Les descripteurs utilisés sont les valeurs TF-IDF des termes présents dans les titres et les recettes de cuisine.
    Le classifieur utilisé est le SVM avec un noyau gaussien (RBF).

### Run3: Word2Vec

    Descripteurs : Word2Vec.
    Le classifieur utilisé SVM avec un noyau gaussien (RBF). On a ajuster les parametres avec GridSearchCV de scikit-learn pour trouver les meilleurs hyperparametres de Word2Vec et on vecteur_size=150, le nombre de mots avant et apres le mots actuels (window=10), et le minimum de la frequence du mot qu'on doit garder doit etre 2 (min_count=2), on aussi utiliseer le skip-gram (sg=1) et aussi on a fixe le nombre de thread a 1 par avoir la reproducibilite des resultats (workers=1)

### Run4: Bag of Words
    Descripteurs : Bag of Words.
    Le classifieur utilisé SVM avec un noyau gaussien (RBF).
## Résultats


``Sans Nettoyage des données``

| Run      | f1 Score |
| -------- | --------:|
| baseline |  0.356  |
| TF-IDF   |  0.865 |
| Word2Vec |  0.838  |
| Bag of Words   | 0.851 |


``Avec Nettoyage des données``

| Run      | f1 Score |                       
| -------- | --------:|
| baseline |  0.356 |
| TF-IDF   |  0.877 |
| Word2Vec |  0.867 |
| Bag of Words  | 0.865 |


Comme vous l'avez remarqué, les résultats sont meilleurs avec le Nettoyage des données. Cela s'explique par le fait que le nettoyage des données permet de réduire le bruit et d'améliorer la qualité des représentations vectorielles des titres et des recettes de cuisine, facilitant ainsi la tâche de classification pour les modèles. Lors du nettoyage, nous avons commencé par tokenizer et mettre en minuscule les mots avec une fonction simple que nous avions créée, mais cela n'a pas été très concluant. Nous avons donc décidé d'utiliser simple_preprocess de Gensim, qui est plus performant et plus simple ; cela a légèrement amélioré nos résultats. Nous avons également enlevé les stop words avec stop_words et avons stemmé les mots avec FrenchStemmer. Enfin, nous avons lemmatisé les mots avec la librairie Spacy. Nous avons constaté aussi que si nous réalisons le stemming avant la lemmatisation, nous obtenons des résultats moins bons (0.866) que si nous effectuons uniquement le stemming (0.874). Et si nous réalisons la lemmatisation avant le stemming, cela donne les meilleurs résultats que nous avons obtenus avec le nettoyage des données (0.877). Ceci peut être expliqué par le fait que le stemmer enlève des lettres, et donc des informations qui peuvent être utiles pour la classification. On a également noté qu'avec la fonction simple_preprocess et le paramètre min_len=1, qui permet de garder les mots de taille 1, on obtient de meilleurs résultats avec le TF-IDF et le Bag of Words.

On peut également remarquer que le Nettoyage des données améliore les résultats sur toutes les autres méthodes. Avec Word2Vec, par exemple, la macro avg passe de 0.838 à 0.867, ce qui représente une amélioration de 0.029, et avec Bag of Words, elle passe de 0.851 à 0.861, soit une amélioration de 0.014. Ainsi, avec le nettoyage des données, c'est Word2Vec qui s'est le plus amélioré par rapport aux autres méthodes, suivi par le TF-IDF.

### Analyse des resultats

### TF-IDF :
``Matrice de Confusion :``

|         | Pred Dessert | Pred Entrée | Pred Plat principal |
|---------|--------------|-------------|---------------------|
| Dessert | 405          | 1           | 1                   |
| Entrée  | 3            | 253         | 81                  |
| Plat principal | 6    | 69          | 569                 |


### Word2Vec
``Matrice de Confusion :``

|         | Pred Dessert | Pred Entrée | Pred Plat principal |
|---------|--------------|-------------|---------------------|
| Dessert | 404          | 1           | 2                   |
| Entrée  | 5            | 229         | 103                  |
| Plat principal | 4    | 54          | 586                 |


### Bag of Words :
``Matrice de Confusion :``

|         | Pred Dessert | Pred Entrée | Pred Plat principal |
|---------|--------------|-------------|---------------------|
| Dessert | 405          | 1           | 1                   |
| Entrée  | 5            | 244         | 88                  |
| Plat principal | 7    | 75          | 562                 |


Parmi les méthodes que nous avons proposées, on remarque que la classe dessert est très bien prédite par les trois méthodes, ce qui peut s'expliquer par le fait que les desserts ont des recettes spécifiques. Cependant, les méthodes ont du mal à prédire les classes plat principal et entrée. On peut dire que les erreurs de classification sont principalement dues à la confusion entre les entrées et les plats principaux. Cela peut s'expliquer par le fait que les entrées et les plats principaux ont souvent des recettes ou des titres similaires, ce qui rend la tâche de classification plus difficile, ou certains plats peuvent se servir indifféremment en entrée ou en plat principal.

### References
- slides du cours et les codes des TP
- https://deft.lisn.upsaclay.fr/actes/2013/pdf/deft13_submission_0.pdf
- https://www.nltk.org/
- https://spacy.io/models/fr
- https://tedboy.github.io/nlps/generated/generated/gensim.utils.simple_preprocess.html 
- https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html
- https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html
- https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html
- https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html
- https://scikit-learn.org/stable/modules/generated/sklearn.dummy.DummyClassifier.html
- https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html
