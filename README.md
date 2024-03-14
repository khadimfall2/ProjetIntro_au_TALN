# DEFT2013 Tâche 2 : Mouride


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
- 1 langue de 1,5 kg
- 20 cl de vinaigre d'alc...  

**Recette** : Faire tremper la langue toute une nuit dans un...

## Exemple de document 2 :
**Identifiant** : recette_21519.xml  
**Titre** : Tarte aux raisins et amandes  
**Type** : Dessert  
**Difficulté** : Facile  
**Coût** : Moyen  
**Ingrédients** :
- 1 pâte brisée
- 150 g de raisins bien mûrs

## Statistiques corpus

**Nombre de documents de train/dev/test :**

|   Dataset     | nombre :|
|---------------|--------|
| Entraînement  |  9978  |
| dev:          |  2495  |
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


## NB:
 On a nettoyer les données en supprimant  les caractères non alphabétiques,tokeniser et enlever la casse avec ``simple_preprocess``on a aussi enlever les stop words avec ``stop_words`` nltk, on a aussi stemmer les mots avec ``FrenchStemmer`` de nltk , enfin on lemmentizer les mots avec la librairie ``Spacy``
 avec les methodes suivantes nous avons utilisé le classificateur ``SVM`` avec le noyau gaussien car c'est celui qui a donnee les meilleurs resultats contre (Regression logistique,Random Forest, Gradient Boosting, AdaBoost, Naive Bayes) avec les parametres gaussien (rbf) et C=5 car on a supposer que tester les  hyperprametres avec GridSearchCV de scikit-learn c'est celui qui a donnee les meilleurs resultats.

### Run2: TF-IDF

    Pour la méthode Run2, nous utilisons également la représentation TF-IDF (Term Frequency-Inverse Document Frequency) pour la vectorisation des données textuelles
    Les descripteurs utilisés sont les valeurs TF-IDF des termes présents dans les recettes de cuisine.
    Nous avons utilise le modèle de Support Vector Machine (SVM)
### Run3: Word2Vec

    Les descripteurs sont les vecteurs de documents moyens générés à partir du modèle Word2Vec. Ces vecteurs représentent chaque document (dans ce cas, chaque recette de cuisine) sous forme d'un vecteur numérique dense dans un espace vectoriel continu
    Le classifieur utilisé SVM avec un noyau gaussien (RBF).. On a ajuster les parametres avec GridSearchCV de scikit-learn pour trouver les meilleurs hyperparametres de Word2Vec et on vecteur_size=150,window=10, min_count=2, sg=1 (skip-gram)
### Run4: Bag of Words
    Les descripteurs sont les vecteurs de compte des termes présents dans les recettes de cuisine. Chaque terme unique dans l'ensemble des données est représenté par un vecteur de caractéristiques, où chaque élément du vecteur représente le nombre d'occurrences du terme correspondant dans le document.
    Le classifieur utilisé est une machine à vecteurs de support (SVM) avec un noyau gaussien (RBF).

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
| Bag of Words  | 0.861 |


Comme vous l'avez remarqué, les résultats sont meilleurs avec le ``Nettoyage des données``, en particulier pour la méthode ``TF-IDF``. Cela s'explique par le fait que le nettoyage des données permet de réduire le bruit et d'améliorer la qualité des représentations vectorielles des titres et des recettes de cuisine, ce qui facilite la tâche de classification pour les modèles. Lors du nettoyage des données, nous avons commencé par tokenizer et mettre en minuscule les mots avec une fonction simple que nous avons créée, mais cela n'a pas donné grand-chose. Nous avons donc décidé d'utiliser ``simple_preprocess`` de Gensim, qui est plus performant car cela a un peu amélioré les résultats. Nous avons également enlevé les stop words avec ``stop_words`` de NLTK, et nous avons aussi stemmé les mots avec ``FrenchStemmer`` de NLTK. Enfin, nous avons lemmatisé les mots avec la librairie ``Spacy``. Nous avons constaté aussi que si nous faisons le stemming avant la lemmatisation, nous obtenons des résultats (0.866) moins bons que si nous faisons seulement le stemming (0.874), et si nous faisons la lemmatisation avant le stemming, cela donne les meilleurs résultats que nous avons eus avec le nettoyage des données (0.877). Ceci peut être expliqué par le fait que le stemmer enlève des lettres et donc des informations qui peuvent être 
utiles pour la classification.

On peut également remarquer que le ``Nettoyage des données `` donne les meilleurs résultats sur toutes les autres méthodes. Avec ``Word2Vec``, par exemple, la  macro avg passe de 0.838 à 0.867, ce qui représente une différence de ``0.029``, et avec ``Bag of Words``, elle passe de 0.851 à 0.861, soit une différence de ``0.010``. Ainsi, avec le nettoyage des données, c'est Word2Vec qui s'est beaucoup amélioré par rapport aux autres méthodes, suivi par le ``TF-IDF`` 


### Analyse des resultats

### TF-IDF :
``Matrice de Confusion :`

|         | Pred Dessert | Pred Entrée | Pred Plat principal |
|---------|--------------|-------------|---------------------|
| Dessert | 405          | 1           | 1                   |
| Entrée  | 3            | 250         | 84                  |
| Plat principal | 6    | 70          | 568                 |

Le modèle a du mal à distinguer les classes "Entrée" et "Plat principal", comme indiqué par les 84 exemples de la classe "Entrée" classés à tort comme "Plat principal" et les 70 exemples de la classe "Plat principal" classés à tort comme "Entrée".

Les termes fréquents partagés entre les recettes d'entrées et de plats principaux peuvent conduire à une similarité dans les vecteurs TF-IDF, ce qui complique la distinction pour le modèle.


### Word2Vec
``Matrice de Confusion :`

|         | Pred Dessert | Pred Entrée | Pred Plat principal |
|---------|--------------|-------------|---------------------|
| Dessert | 402          | 2           | 3                   |
| Entrée  | 3            | 223         | 111                  |
| Plat principal | 5    | 76          | 523                 |


Des erreurs significatives sont observées, notamment dans la classification des recettes de la classe "Plat principal" où 111 exemples sont classés à tort comme "Entrée".

Word2Vec capture la sémantique des mots, mais la similarité sémantique entre les recettes de "Plat principal" et "Entrée" peut conduire à des confusions, en particulier si certaines entrées ressemblent à des plats principaux et vice versa.

### Bag of Words :
``Matrice de Confusion :`

|         | Pred Dessert | Pred Entrée | Pred Plat principal |
|---------|--------------|-------------|---------------------|
| Dessert | 402          | 1           | 1                   |
| Entrée  | 4            | 244         | 89                  |
| Plat principal | 8    | 75          | 561                 |


Le modèle semble avoir des difficultés similaires à distinguer les classes "Entrée" et "Plat principal" comme observé dans les autres méthodes.

Comme avec TF-IDF, la représentation BoW par CountVectorizer peut également souffrir de la similarité entre les termes fréquents des titre ou recettes d'entrées et de plats principaux.