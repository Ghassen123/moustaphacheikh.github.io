---
title: "TALN: Prétraitement du texte arabe en 10 lignes"
layout: post
date:   2017-10-17
---
<center>
<img src="{{ '/assets/img/nlp.png' | prepend: site.baseurl }}" alt=""> 
<figcaption><a href="https://ontotext.com/top-5-semantic-technology-trends-2017/">This image from here</a></figcaption>
</center>

Aujourd'hui, les nous sommes en face à une énorme quantité et une grande variété de données - les e-mails, les tweets, les données provenant d'applications mobiles et autres. 
Il faut beaucoup d'efforts et de temps pour rendre ces données utiles. L'une des principales compétences dans l'extraction d'informations à partir de données de texte est le traitement automatique du langage naturel (TALN).

Le traitement automatique du langage naturel (TALN) est un domaine de recherche et d'application qui explore comment les ordinateurs peuvent être utilisés pour comprendre et manipuler le texte en langage naturel. Compte tenu de l'augmentation du contenu sur Internet et les médias sociaux, c'est l'une des compétences incontournables pour tous les spécialistes des données.
<center>

</center>
Les mots sont souvent considérés comme les constituants de base des textes. Le premier module d'un pipeline TALN est souvent un tokenizer qui transforme les textes enséquences de mots. Cependant, différentes techniques de prétraitement peuvent être (et sont) davantage utilisées dans la pratique. Ceux-ci incluent la lemmatisation ou POS.

Mais dans la vraie vie avant tout cela, nous devons préparer le texte avant de commencer. Ce processus est appelé nettoyage de texte.

Dans ce tutoriel, je vais écrire quelques méthodes génériques pour le nettoyage de texte que vous pouvez combiner comme vous le souhaitez, pour obtenir le résultat final.


* **Nettoyage** 

    * Remove numbers
    * Remove diacritics
    * Remove replacated carracters
    * Remove non arabic words
            .
            .
            .
    * Remove ponctuatons
   
 
Dans les prochains articles, nous allons voir les titres suivants:

* **Stemming**
* **Tokenization**
* **Part-Of-Speech Tagging**
* **Word Embeddings**

## Méthodes de prétraitement
Maintenant nous allons mettre en œuvre chaque méthode séparément et voir le travail sur un exemple,
et après cela, nous allons les combiner pour obtenir le résultat final.
### Importer les bibliothèques requises
***

Tout d'abord, importons les bibliothèques requises

```python
import re
```
Nous allons implémenter les méthodes suivantes:

* **remove_diacritics(string)**: supprime tous les signes diacritiques d'une chaîne et renvoie la version nettoyée

* **remove_urls(string)** : supprime toutes les URLs d'une chaîne et renvoie la version propre

* **remove_numbers(string)**: supprime tous les nombres d'une chaîne et renvoie la version propre

* **noramlize(string)**: normalise une chaîne (voir l'implémentation )

* **remove_non_arabic_words(string)**: supprime tous les mots non arabes (ont un symbole non arabe) d'une chaîne et renvoie la version nettoyée

* **remove_extra_whitespace(string)**: supprime les espaces supplémentaires d'une chaîne et renvoie la version propre

* **remove_non_arabic_symbols(string)**: supprime tous les symboles non arabes d'une chaîne et renvoie la version propre

* **remove_ponctuations(string)**: supprime toutes les ponctuations d'une chaîne et renvoie la version propre

* **remove_stop_words(string, stop_words=['empthy'])**: supprime la liste des mots vides d'une chaîne et renvoie la version nettoyée

* **remove_dubplicated_letters(string)**: supprime les lettres en double et renvoie la chaîne de résultat

### Supprimer les signes diacritique
***

```python
def remove_diacritics(string):
    regex = re.compile(r'[\u064B\u064C\u064D\
    \u064E\u064F\u0650\u0651\u0652]')
    return re.sub(regex, '', string)
```
Testons cette méthode sur un exemple :

```python
text = "بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيمِ"
print("Avant: ",text)
string = remove_diacritics(string)
print("Après: ",string)
```
```python
Avant:  بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيمِ
Après:  بسم الله الرحمٰن الرحيم
```

Nous pouvons voir ci-dessus que remove_diacritics(string) supprime tous les signes diacritiques d'une chaîne et renvoie la version propre.

### supprimer les URLs
***

```python
def remove_urls(string):
    regex = re.compile(r"(http|https|ftp)://\
    (?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|\
    (?:%[0-9a-fA-F][0-9a-fA-F]))+")
    return re.sub(regex, ' ', string)
```

Testons cette méthode sur un exemple :

```python
text ="ftp://ift.tt/2xWCmyr مواصفات وسعر هاتف أيفون 8 الجديد https://ift.tt/2xWCmyr"
print("Avant: ",text)
string = remove_urls(string)
print("Après: ",string)
```
```python
Avant:  ftp://ift.tt/2xWCmyr مواصفات وسعر هاتف أيفون 8 الجديد https://ift.tt/2xWCmyr
Après:    مواصفات وسعر هاتف أيفون 8 الجديد  
```

Donc, nous pouvons voir ci-dessus que remove_urls(string) supprime toutes les URLs qui commencent avec http (s) ou ftp d'une chaîne et retourne la version propre.

### Supprimer les nombres
***

```python
def remove_numbers(string):
    regex = re.compile(r"(\d|[\u0660\u0661\u0662\
    \u0663\u0664\u0665\u0666\u0667\u0668\u0669])+")
    return re.sub(regex, ' ', string)
```

Testons cette méthode sur un exemple :

```python
string ="مواصفات وسعر هاتف أيفون الجديد1234567890   ٠‎١‎٢‎٣‎٤‎٥‎٦‎ ٧‎٨‎٩"
print("Avant: ",string)
string = remove_numbers(string)
print("Après: ",string)
```

```python
Avant:  مواصفات وسعر هاتف أيفون الجديد1234567890   ٠‎١‎٢‎٣‎٤‎٥‎٦‎ ٧‎٨‎٩
Après:  مواصفات وسعر هاتف أيفون الجديد 
```

Donc, nous pouvons voir ci-dessus que remove_numbers(string) supprime tous les nombres d'une chaîne et renvoie la version propre.

### Normalise une chaîne
***

```python
def noramlize(string):
    regex = re.compile(r'[إأٱآا]')
    string = re.sub(regex, 'ا', string)
    regex = re.compile(r'[ى]')
    string = re.sub(regex, 'ي', string)
    regex = re.compile(r'[ؤئ]')
    string = re.sub(regex, 'ء', string)
    return string
```

Testons cette méthode sur un exemple :

```python
string = " ؤ ئ إ أ ٱ آ ا ى ي"
print("Avant: ",string)
string = noramlize(string)
print("Après: ",string)
```
```python
Avant:   ؤ ئ إ أ ٱ آ ا ى ي
Après:   ء ء ا ا ا ا ا ي ي
```

### Supprimer les mots non arabes
***

```python
def remove_non_arabic_words(string):
    return ' '.join([word for word in \
            string.split() if not re.findall(
        r'[^\s\u0621\u0622\u0623\u0624\u0625\
        \u0626\u0627\u0628\u0629\u062A\u062B\
        \u062C\u062D\u062E\u062F\u0630\u0631\
        \u0632\u0633\u0634\u0635\u0636\u0637\
        \u0638\u0639\u063A\u0640\u0641\u0642\
        \u0643\u0644\u0645\u0646\u0647\u0648\
        \u0649\u064A]',
        word)])
```

Testons cette méthode sur un exemple :

```python
string = "مواڞفات وسعر هاتف أيفون 8 الجدي"
print("Avant: ",string)
string = remove_non_arabic_words(string)
print("Après: ",string)
```
```python
Avant:  مواڞفات وسعر هاتف أيفون 8 الجدي
Après:  وسعر هاتف أيفون الجدي
```
Parfois, nous pouvons trouver un mot qui a des symboles non arabes. On peut voir ci-dessus que remove_non_arabic_words (string) supprime tous les mots non arabes (ont un symbole non arabe) d'une chaîne et retourne la version propre. par exemple مواڞفات

### Supprimer les espaces supplémentaires
***

```python
def remove_extra_whitespace(string):
    string = re.sub(r'\s+', ' ', string)
    return re.sub(r"\s{2,}", " ", string).strip()
```

Testons cette méthode sur un exemple :

```python
string = "مواڞفات       وسعر هاتف     أيفون 8    الجدي"
print("Avant: ",string)
string = remove_extra_whitespace(string)
print("Après: ",string)
```
```python
before:  مواڞفات       وسعر هاتف     أيفون 8    الجدي
after:  مواڞفات وسعر هاتف أيفون 8 الجدي
```
### Supprimer les symboles non arabes
***

```python
def remove_non_arabic_symbols(string):
    return re.sub(r'[^\u0600-\u06FF]', ' ', string)
```

Testons cette méthode sur un exemple :

```python
string = "مواڞفات       وسعر هاتف     أيفون 8    الجدي"
print("Avant: ",string)
string = remove_non_arabic_symbols(string)
print("Après: ",string)
```
```python
Avant:  مواصفات   وسعر هاتف     أيفون 8    الجدي☯ ☸ ☹ ☺ ☻ ☼ ☽ ☾ ☿ ♀ ♁ ♂ ♃ ♄ ♅ ♆ ♇  । ॥ ᜵ ᜶ ჻ ⅋ 〽 ॰ ℄ ︕ ︖ ︗ ︘ ︙ 𝚤 𝚥
Après:  مواصفات وسعر هاتف أيفون الجدي
```
comme prévu nous pouvons voir ci-dessus que remove_non_arabic_symbols(string) supprime tous les symboles non arabes d'une chaîne et retourner la version propre.

### Supprimer les ponctuations
***

```python
def remove_punctiation(string):
    return re.sub(
        r'[\u060C\u061B\u2024\u003A\u2026\
        \u061F\u0021\u005D\u005B\u0029\
        \u0028\u002D\u00BB\u00AB\u0022]',
        ' ', string)
```

Testons cette méthode sur un exemple :

```python
text = "  مواصفات   وسعر هاتف     أيفون 8    الجدي ،؛․:…؟! ] [ ) ( - » « "
print("Avant: \n",remove_extra_whitespace(text))
string = remove_punctiation(text)
print("Après: \n",remove_extra_whitespace(string))
```

```python
Avant:  مواصفات وسعر هاتف أيفون 8 الجدي ،؛․:…؟! ] [ ) ( - » «
Après:  مواصفات وسعر هاتف أيفون 8 الجدي
```
comme prévu nous pouvons voir ci-dessus que remove_punctiation(string) supprime tous les ponctuations d'une chaîne et retourner la version propre.

### Supprimer les lettre doubles
***

```python
def remove_dubplicated_letters(string):
    return re.sub(r'(.)\1{2,}', r'\1', string)
```

Testons cette méthode sur un exemple :

```python
text = "مواصفات وسعر من هاتف أيفون   مممن الجديد"
print("Avant: ",remove_extra_whitespace(text))
string = remove_dubplicated_letters(text)
print("Après: ",remove_extra_whitespace(string))
```
```python
Avant:  مواصفات وسعر من هاتف أيفون مممن الجديد
Après:  مواصفات وسعر من هاتف أيفون من الجديد
```
comme prévu nous pouvons voir ci-dessus que remove_dubplicated_letters(string) supprime tous les lettre double d'une chaîne et retourner la version propre.

# Le prétraitement en tant que collection d'étapes

Nous pouvons utiliser la méthode ci-dessus pour créer un pipeline de prétraitement.

### Supprimer les lettre doubles
***

```python
def preprocessing(string):
    # remove diacritics
    string = remove_diacritics(string)
    # remove non arabic symbols
    string = remove_non_arabic_symbols(string)
    # remove punctiations
    string = remove_punctiation(string)
    # remove dubplicated letters
    string = remove_dubplicated_letters(string)
    # remove non arabic words
    string = remove_non_arabic_words(string)
    # normelize the text
    string = noramlize(string)
    # remove extra white spaces
    string = remove_extra_whitespace(string)
    return string
```

Testons cette méthode sur un exemple :

```python
text = "مواڞفات وسعر هاتف أيفون  fq fddfd 8 الجدي"
print("Avant: ",remove_extra_whitespace(text))
string = preprocessing(text)
print("Après: ",remove_extra_whitespace(string))
```
```python
Avant:  بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيمِftp://ift.tt/2xWCmyr مواصفات وسسسسسعر هاتف أيفون 8 الجديد https://ift.tt/2xWCmyr
Après:  بسم الله الرحيم مواصفات وسعر هاتف ايفون الجديد
```

En général, nous ajoutons ce que nous voulons dans la méthode de prétraitement.
