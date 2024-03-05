# DEFT2013 Tâche 2 : NOMEQUIPE (optionnel)

Baye Lahad Mbacke M1 ATAL Nantes Universite
Khadim Fall M1 DS Nantes Universite 
## Description de la tâche

	1 ou 2 exemples de documents (avec leur identifiant)
    La tâche consiste à classer automatiquement les recettes de cuisine en trois catégories : Plat principal, Entrée ou Dessert, en se basant sur le titre et les instructions de la recette.

## Statistiques corpus

**Nombre de documents de train/validation/test :**
 
-``Entraînement``: 9978
-``Validation``: 2495
-``Test`` : 1388 

**Répartition des étiquettes dans chacun des sous-ensembles :**
**Train:**
        ``Plat principal`` : 4644
        ``Entrée`` : 3036
        ``Dessert`` : 2298
    **Validation :**
        ``Plat principal`` : 1158
        ``Entrée`` : 726
        ``Dessert`` : 611
    **Test :**
        ``Plat principal`` : 644
        ``Entrée`` : 407
        ``Dessert`` : 337

## Méthodes proposées

### Run1: baseline (méthode de référence)

	Nous utilisons un classificateur dummy qui prédit les classes de manière aléatoire. Cette méthode est utilisée comme point de comparaison de base pour évaluer les performances des autres méthodes plus sophistiquées
	Aucun descripteur spécifique n'est utilisé dans cette méthode. Le classificateur dummy se contente de tirer aléatoirement des prédictions sans se baser sur les caractéristiques des données.
	DummyClassifier de scikit-learn avec la stratégie 'stratified'. Cette stratégie génère des prédictions en respectant la distribution des classes dans l'ensemble de données d'entraînement.


## NB: On a nettoyer les données en supprimant  les caractères non alphabétiques,Tokenization,Supprimer les stopwords, convertir en minuscules, lemmatisation
## avec les methodes suivantes nous avons utilisé le classificateur SVM avec le noyau gaussien car c'est celui qui a donnee les meilleurs resultats contre (Regression logistique,Random Forest, Gradient Boosting, AdaBoost, Naive Bayes) avec les parametres gaussien (rbf) et C=10 car on a tester les hyperprametres et c'est celui qui a donnee les meilleurs resultats

### Run2: TF-IDF

    Pour la méthode Run2, nous utilisons également la représentation TF-IDF (Term Frequency-Inverse Document Frequency) pour la vectorisation des données textuelles
    Les descripteurs utilisés sont les valeurs TF-IDF des termes présents dans les recettes de cuisine.
    Nous avons utilise le modèle de Support Vector Machine (SVM)car c'est lui qui a les meilleurs resultats avec les parametres gaussien (rbf) et C=10
### Run3: Word2Vec

    Les descripteurs sont les vecteurs de documents moyens générés à partir du modèle Word2Vec. Ces vecteurs représentent chaque document (dans ce cas, chaque recette de cuisine) sous forme d'un vecteur numérique dense dans un espace vectoriel continu
    Le classifieur utilisé est une machine à vecteurs de support (SVM) avec un noyau gaussien (RBF). Ce choix de classifieur est basé sur l'hypothèse que les vecteurs de documents générés par Word2Vec pourraient bénéficier d'une séparation non linéaire des classes dans l'espace des caractéristiques
### Run4: CountVectorizer
    Les descripteurs sont les vecteurs de compte des termes présents dans les recettes de cuisine. Chaque terme unique dans l'ensemble des données est représenté par un vecteur de caractéristiques, où chaque élément du vecteur représente le nombre d'occurrences du terme correspondant dans le document.
    Le classifieur utilisé est une machine à vecteurs de support (SVM) avec un noyau gaussien (RBF). Comme dans la méthode précédente, ce choix de classifieur est basé sur l'hypothèse que les caractéristiques extraites des recettes de cuisine peuvent bénéficier d'une séparation non linéaire des classes dans l'espace des caractéristiques
## Résultats
``Avec Nettoyage des données``

| Run      | f1 Score |
| -------- | --------:|
| baseline |  0.37 |
| TF-IDF   |  0.87 |
| Word2Vec |  0.86 |
| CountVectorizer   |  0.86 |

``Sans Nettoyage des données``

| Run      | f1 Score |
| -------- | --------:|
| baseline |  0.32 |
| TF-IDF   |  0.87 |
| Word2Vec |  0.82 |
| CountVectorizer   |  0.85 |



### Analyse de résultats
	
	Pistes d'analyse:
	* Combien de documents ont un score de 0 ? de 0.5 ? de 1 ? (Courbe ROC)
	* Y-a-t-il des régularités dans les document bien/mal classifiés ?
	* Où est-ce que l'approche se trompe ? (matrice de confusion)
	* Si votre méthode le permet: quels sont les descripteurs les plus décisifs ?