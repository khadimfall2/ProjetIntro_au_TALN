# DEFT2013 Tâche 2 : NOMEQUIPE (optionnel)

Baye Lahad Mbacke M1 ATAL Nantes Universite
Khadim Fall M1 DS Nantes Universite 
## Description de la tâche

	1 ou 2 exemples de documents (avec leur identifiant)

## Statistiques corpus

	Nombre de document de train/dev/test
	Répartition des étiquettes dans chacun des sous-ensemble

## Méthodes proposées

### Run1: baseline (méthode de référence)

	Description de la méthode:
	- descripteurs utilisés:
        Toutes les caractéristiques présentes dans l'ensemble de données à l'exception de la colonne 'type' sont utilisées comme descripteurs.
	- classifieur utilisé:
        Un DummyClassifier de la bibliothèque sklearn.dummy est utilisé. La stratégie 'stratified' est utilisée, qui génère des prédictions en respectant la distribution des classes de l'ensemble d'entraînement.


### Run2: TF-IDF

    Description de la méthode:
    - descripteurs utilisés:
        Les descripteurs sont les mots présents dans les documents. Les mots sont pondérés par leur fréquence dans le document et leur fréquence dans le corpus.
    - classifieur utilisé:
        Un classifieur de la bibliothèque sklearn est utilisé. Le classifieur utilisé est un classifieur de régression logistique.
### Run3: Word2Vec

    Description de la méthode:
    - descripteurs utilisés:
        Les descripteurs sont les vecteurs de mots générés par le modèle Word2Vec.
    - classifieur utilisé:
        Un classifieur de la bibliothèque sklearn est utilisé. Le classifieur utilisé est un classifieur de régression logistique.
### Run4: NOMMETHODE (pour aller plus loin)

## Résultats

| Run      | f1 Score |
| -------- | --------:|
| baseline |  0.33 |
| TF-IDF   |  0.86 |
| Word2Vec |  0.81 |
| CountVectorizer   |  0.84 |

### Analyse de résultats
	
	Pistes d'analyse:
	* Combien de documents ont un score de 0 ? de 0.5 ? de 1 ? (Courbe ROC)
	* Y-a-t-il des régularités dans les document bien/mal classifiés ?
	* Où est-ce que l'approche se trompe ? (matrice de confusion)
	* Si votre méthode le permet: quels sont les descripteurs les plus décisifs ?