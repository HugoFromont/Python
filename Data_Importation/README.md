# Collecte de données avec python
## Importer un ficher CSV
Pour importer un fichier csv, nous utilisons la commande read_csv de la library Pandas
```{python}
import pandas as pd
df = pd.read_csv("chemin/data.csv")
```
Grâce à la fonction précédante nous avons stocké notre CSV dans un DataFrame. Nous verrons plus loin comment manipuler les données d'un DataFrame

## Importer un fichier dbf
pour importer un fichier dbf, nous utilisons la librarie dbf
```{python}
import dbf
table = dbf.Table('chemin.dbf')
```
nous pouvons ensuite naviguer dans la table à l'aide du code suivant
```{python}
table.open()
for ligne in table:
	print(ligne.variable1,ligne.variable2)
table.close()
```

## Connection à une DataBase MySQL

Avec python, il est possible d'établir une connection avec une base de données MySQL, et d'effectuer tout type d'opération (Create Table, Insert into, SELECT ...). Pour cela,  nous devons d'abord connecter python à une DatBase de MySQL.
```{python}
import mysql.connector

conn = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd="#########",
  database="DataBase_Name"
)

cursor = conn.cursor()
```
Nous pouvons maintenant éxécuter des requètes SQL.
Par exemple pour créer une Table dans notre DataBase, nous pouvons utiliser la commande suivante :
```{python}
cursor.execute("""
	CREATE TABLE IF NOT EXISTS nom_table (
		id_user int(5) NOT NULL AUTO_INCREMENT,
		user_name varchar(40) NOT NULL,
		user_age int(3) DEFAULT NULL,
		PRIMARY KEY(id_user)
	);
	""")
```
Nous pouvons ensuite insérer des données dans la Table. 
Voici une fonction qui permet d'insérer des données dans la table créée au dessus. Cette fonction utilise la fonction try afin de savoir si l'opération à été correctement éxécutée.
```{python}
def insert_users(data):
    query = 'INSERT INTO nom_table(user_name,user_age) '\
    'VALUES(%s,"%i");'%(data)
    try:
        cursor.execute(query)
        conn.commit()
        print('successful insertion')
    except :
        print('failled insertion')
	
insert_users(('Hugo',22))
```
Enfin nous pouvons bien évidement récupérer les informations stockées dans la base de données. Voici comment réaliser cette opération  

```{python}
cursor.execute("""SELECT * from nom_table""")
rows = cursor.fetchall()
print(rows)
```

## Récupérer les données de l'API Twitter

Twitter dispose d'une API avec laquelle il est possible d'intéragir (récupérer des tweets, publier des tweets...).
Afin  de récupérer des données Twitter, Twitter nous demande de nous créer un compte développeur [(Twitter Developer Platform)](https://developer.twitter.com/)
Une fois le compte créé, nous disposons d'identifiants qui nous permettent de nous connecter à l'API.
Grâce à ces identifiants, nous allons configurer notre connection à l'API
```{python}
import tweepy

access_token =        "##########################################"
access_token_secret = "##########################################"
consumer_key =        "##########################################"
consumer_secret =     "##########################################"

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)
```
Nous pouvons maintenant faire une recherche par mot clef pour récupérer des tweets précis

```{python}
query = "#python"
max_tweets = 100
public_tweets = tweepy.Cursor(api.search, q=query).items(max_tweets)
for tweet in public_tweets:
    user_name = tweet.user.name
    followers_count = tweet.user.followers_count
    location = tweet.user.location
    id_tweet = tweet.id
    text = tweet.text
    lang = tweet.lang
    date = tweet.created_at
    nb_retweet = tweet.retweet_count
    nb_likes = tweet.favorite_count
    print(text)
```
Le code au dessus permet d'afficher 100 tweets qui contiennent le hashtag "python". Il est possible de sauvegarder ces informations dans un DataFrame, vecteur...
De nombreuses informations peuvent être récupérées, une liste complète est disponible sur le site de [(Twitter Developer Platform)](https://developer.twitter.com/). Quelques exemples des informations qu'il est possible de récupérer sont disponibles dans le code au dessus.

## Collecte de données à l'aide de Scrapping Web
Avec python il est possible d'utiliser des techiniques de Scrapping afin de récupérer les données d'une page Web.

### Web-Scraping avec driver
Dans un premier temps nous allons voir comment il est possible d'utiliser un driver pour naviguer sur une page Web.
Tout d'abord il est nécessaire de télécharger un driver pour le moteur de recherche que vous souhaitez utiliser. Et de le configurer pour pouvoir l'utiliser dans un script python. Sur internet, il existe de nombreurses documentations sur le sujet.

Une fois votre driver installé, nous allons utiliser la librarie Selenium pour controler notre driver.

```{python}
from selenium import webdriver
import os
```
Nous devons renseigner l'emplacement du driver avec la commande suivante et indiquer l'url que le driver doit ouvrir, ici j'ai pris l'url du film inception sur allociné
```{python}
os.environ["PATH"] = "C:/WebScraping/"
url = "http://www.allocine.fr/film/fichefilm_gen_cfilm=143692.html"
```
Nous allons maintenant dire à notre driver d'ouvrir la page web demandée
```{python}
driver = webdriver.Chrome()
driver.get(url)
headers = {}
headers['User-Agent'] = "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2228.0 Safari/537.36"
```
Nous pouvons maintenant récupérer un élément de la page web avec du language Xpath
```{python}
html_element = driver.find_elements_by_xpath('language_Xpath')
for element in html_element:
	print(element.get_attribute('innerHTML'))
```
Mais nous pouvons aussi naviguer au sein de la page web et même cliquer sur des éléments qui ont des liens url.
exemple du code pour scroller la page web 
```{python}
driver.execute_script("window.scrollBy(0, 1000000)")
```
exemple d'une commande qui permet de cliquer sur le bouton d'une page web
```{python}
driver.find_element_by_xpath("chemin_du_boutton_en_Xpath").click()
```
Grâce à cette technique, il est possible de récupérer tout type de données (text, images...)

### Web-Scraping avec la library BeautifulSoup
Voici une deuxième méthode de Scapping qui ne nécessite pas de driver. Et qui est plus simple à utiliser.
Nous allons dans un premier temps récupérer le code source d'une page html avec la commande suivante
```{python}
from requests import get
url = "http://www.allocine.fr/film/fichefilm_gen_cfilm=143692.html"
code_source = get(url)
```
Puis grâce à la librarie BeautifulSoup nous allons pouvoir récupérer les informations qui nous intéressent
```{python}
from bs4 import BeautifulSoup
object_BeautifulSoup = BeautifulSoup(code_source.text, 'html.parser')
element = object_BeautifulSoup.find_all('div', class_ = 'name_class')
result = element.h2
print(resulat)
```
