# Partie 2 - Manipulation des données avec python
## Manipulation des DataFrames avec Pandas

### Création d'un DataFrame
La manipulation de DataFrames sur python s'effectue grâce à la librarie Pandas. Pour pouvoir reproduire les commandes utilisées, il est nécessaire d'installer le Package Pandas et de l'importer dans le script python
```{python}
import pandas as pd
```
Il est maintenant possible de créer un DataFrame avec la commande suivante 
```{python}
df = pd.DataFrame({'Prenom': ["Prenom1", "Prenom2", "Prenom3",], 'Age': [19,25,53]}, columns = ['Prenom', 'Age'])
```
<img src = "https://user-images.githubusercontent.com/44996564/57060846-23fd5580-6cbb-11e9-9747-8d1aa6467bd8.png">

Mais nous pouvons aussi transformer des objets déja créés en DataFrame
```{python}
# Transformation d'une liste en DataFrame
ma_list = [["Prenom1",19],["Prenom2",25],["Prenom3",53]]
df = pd.DataFrame(ma_list,columns = ['Prenom', 'Age'])

# Transformation d'un vecteur en DataFrame
mon_vecteur = np.array([['Prenom1', '19'],['Prenom2', '25'],['Prenom3', '53']])
df = pd.DataFrame(mon_vecteur,columns = ['Prenom', 'Age'])
```
<img src = "https://user-images.githubusercontent.com/44996564/57060846-23fd5580-6cbb-11e9-9747-8d1aa6467bd8.png">

### Information sur le DataFrame
Il existe plusieurs fonctions permettant d'avoir des informations sur un DataFrame. Voici quelques fonctions couramment utilisées.

```{python}
# Taille du DataFrame
df.shape
```
<img src = "https://user-images.githubusercontent.com/44996564/57061445-1e087400-6cbd-11e9-8afe-383872ad9d72.png">

```{python}
# Apercut des 2 premières lignes
df.head(2)
```
<img src = "https://user-images.githubusercontent.com/44996564/57061505-5f991f00-6cbd-11e9-938d-34cd2615a161.png">

```{python}
# Afficher la liste des variables
df.columns
```
<img src = "https://user-images.githubusercontent.com/44996564/57061551-91aa8100-6cbd-11e9-9f76-23b165972507.png">

Il existe de nombreuses autres fonctions, et beaucoup de documentations diponibles sur internet, en voici un exemple [(Lien vers documentation)](https://pandas.pydata.org/pandas-docs/stable/)

### Sélectionner des données
Voici les 3 princpales méthodes qui permettent de sélectionner des données dans un DataFrame

Voici 3 commandes qui permmettent de sélectionner une variable
```{python}
df.Prenom
df['Prenom']
df.loc[:, 'Prenom']
```
<img src = "https://user-images.githubusercontent.com/44996564/57061757-5c526300-6cbe-11e9-8840-b7cb4d8d016c.png">

Voici 3 commandes permmettant de sélectionner des individus
```{python}
df.Prenom[:2]
df['Prenom'][:2]
df.loc[:1, 'Prenom']
```
<img src = "https://user-images.githubusercontent.com/44996564/57061941-092ce000-6cbf-11e9-9950-78ba0653006c.png">

Voici une dernière fonction permettant de sélectionner des données dans un DataFrame
```{python}
df.iloc[0:2,1]
```
<img src = "https://user-images.githubusercontent.com/44996564/57062036-4f823f00-6cbf-11e9-8d17-60f7ca862bfc.png">

Pour effectuer des filtres sur les données, nous pouvons utiliser les mêmes fonctions
```{python}
df[df['Age']>20]
df[df.Age>20]
```
<img src = "https://user-images.githubusercontent.com/44996564/57062259-f1099080-6cbf-11e9-80cf-44217b2695dc.png">

```{python}
df[df['Prenom']=="Prenom1"]
df[df.Prenom=="Prenom1"]
```
<img src = "https://user-images.githubusercontent.com/44996564/57062335-1f876b80-6cc0-11e9-9d05-8b7d898cb16c.png">

### Format de variables
Pour visualiser le format des variables d'un DataFrame
```{python}
df.dtypes
```
<img src = "https://user-images.githubusercontent.com/44996564/57062501-a3415800-6cc0-11e9-9f88-4029001969c7.png">

Pour modifier le type de variables
```{python}
df.Age = df.Age.astype(str)
df.dtypes
```
<img src = "https://user-images.githubusercontent.com/44996564/57062846-c4567880-6cc1-11e9-9ace-7ccae91d4b29.png">
```{python}
df.Age = df.Age.astype(float)
df.dtypes
```
<img src = "https://user-images.githubusercontent.com/44996564/57062501-a3415800-6cc0-11e9-9f88-4029001969c7.png">

Autres Fonctions utiles :
```{python}
# Transformer un nombre de seconde en datetime :
pd.to_datetime(nb_seconde, unit='s')
```

#### Gestion des valeurs manquantes
Voici quelques techniques permettant de traiter les valeurs manquantes.

La première méthode un peu drastique, consiste à supprimer les lignes qui possèdent des valeurs manquantes. 
Nous pouvons supprimer l'ensemble des lignes qui possèdent au moins une valeur manquante.
```{python}
df = df.dropna()
```

Si nous voulons supprimer les valeurs manqantes pour une seule variable, nous pouvons procéder ainsi
```{python}
df = df[df['variable'].isna()==False]
```

Dans la majeure partie des cas, nous allons recoder ces valeurs manquantes grâce à diverses techniques.

remplacement d'une valeur manquante par 0
```{python}
df['variable'][np.isnan(df['variable'])] = 0
df['variable'][np.isnan(df['variable'])]="MQT"
```

remplacement d'une valeur manquante par la moyenne
```{python}
df['variable'][np.isnan(df['variable'])] = np.mean(df.variable)
```

### Statistique descriptive
```{python}
df['variable'].describe()
df['variable'].count()
df['variable'].max()
df['variable'].min()
df['variable'].mean()
df['variable'].std()
```

### Autres focntions
Voici d'autres fonctions utiles
```{python}
# Transformer un effectif en fréquence
df.value_counts(normalize = True)
# Renommer les variables
df.columns = ['var1', 'var2']
# Trier un DataFrame
df.sort_values(by = ['var1', 'var2'])
# Croisement de deux variables
pd.crosstab(df.variable1,df.variable1)
# Tester l'égalité de deux DataFrames
print(pd.DataFrame.equals(df1, df2))
```


## Manipulation des données avec dplython
Pour manipuler des données sur python, on peut aussi utiliser le package dplython, à condition d'importer le package et de transformer votre dataframe en un objet "DplyFrame"

```{python}
from dplython import (DplyFrame, X, select, sift, arrange, mutate, group_by, summarize) 
data = DplyFrame(data)
```

Il est maintenant possible d'effectuer les opérations suivantes

```{python}
# Sélection
new_data = data >> select(X.variable1,X.variable2,X.variable3)
# Filtres
new_data = data >> sift(X.variable1 > valeur1 , X.variable1 < valeur2)
# Trier
new_data = data >> arrange(X.variable1, X.variable2)
# Ajouter une nouvelle variable
new_data = data >> mutate(new_feature = X.feature1-100, new_feature2 = X.feature1/X.feature2)
# Agrégation
new_data = data >> group_by(X.variable1) >> summarize(new_feature = X.variable2.mean())
# Jointure
inner_join_data = inner_join(data1, data2, on = "ID")
left_join_data = left_join(data1, data2, on = "ID")
```
