## Création de graphiques avec Matplotlib
### Diagramme en bar

```{python}
df['variable_quali'].value_counts().plot.bar()
```
### histogramme
```{python}
df['variable_quanti'].plot.hist()
```
### Nuage de points
### Histogramme
### Boxplot
Pour afficher des boxplots les uns à côté des autres :
```{python}
var = ['Axe1','Axe2','Axe3','Axe4','Axe5']
graph=1
fig = plt.figure(figsize=(20,5))
for axe in var:
    plt.subplot(1, 5, graph)
    plt.boxplot(df[axe])
    graph += 1
plt.show()
```
pour afficher un boxplot par catégorie :
```{python}
fig = plt.figure(figsize=(20,5))
var = ['Axe1','Axe2','Axe3','Axe4','Axe5']
i=1
for axe in var:
    plt.subplot(1, 5, i)
    plt.boxplot([
    df[df['Cluster']=='1'][axe],
    df[df['Cluster']=='2'][axe],
    df[df['Cluster']=='3'][axe],
    df[df['Cluster']=='4'][axe],
    df[df['Cluster']=='5'][axe],
    df[df['Cluster']=='6'][axe]])
    i +=1
plt.show()
```
### Afficher plusieurs graphiques
