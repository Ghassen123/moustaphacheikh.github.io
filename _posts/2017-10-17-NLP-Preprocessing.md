---
title: "TALN: Prétraitement du texte arabe dans 10 lignes de codes"
layout: post
date:   2017-10-17
---

## Introduction

Aujourd'hui, les entreprises font face à une énorme quantité et une grande variété de données - les appels des clients, leurs e-mails, les tweets, les données provenant d'applications mobiles et autres. Il faut beaucoup d'efforts et de temps pour rendre ces données utiles. L'une des principales compétences dans l'extraction d'informations à partir de données de texte est le traitement automatique du langage naturel (TALN).

Le traitement automatique du langage naturel (TALN) est un domaine de recherche et d'application qui explore comment les ordinateurs peuvent être utilisés pour comprendre et manipuler le texte en langage naturel. Compte tenu de l'augmentation du contenu sur Internet et les médias sociaux, c'est l'une des compétences incontournables pour tous les spécialistes des données.

Dans chaque tâche de traitement de la langue naturelle, il y a quelques étapes de prétraitement communes que nous devons faire. Je vais expliquer et fournir le code pour les techniques de pré-traitement suivantes en python.

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
    regex = re.compile(r'[\u064B\u064C\u064D\u064E\u064F\u0650\u0651\u0652]')
    return re.sub(regex, '', string)
```
Testons cette méthode sur un exemple :

```python
string = "بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيمِ"
```

```python
print("Avant: ",string)
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
    regex = re.compile(r"(http|https|ftp)://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+")
    return re.sub(regex, ' ', string)
```

Testons cette méthode sur un exemple :

```python
string ="ftp://ift.tt/2xWCmyr مواصفات وسعر هاتف أيفون 8 الجديد https://ift.tt/2xWCmyr"
```

```python
print("Avant: ",string)
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
    regex = re.compile(r"(\d|[\u0660\u0661\u0662\u0663\u0664\u0665\u0666\u0667\u0668\u0669])+")
    return re.sub(regex, ' ', string)
```

Testons cette méthode sur un exemple :

```python
string ="مواصفات وسعر هاتف أيفون الجديد1234567890   ٠‎١‎٢‎٣‎٤‎٥‎٦‎ ٧‎٨‎٩"
```

```python
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
```

```python
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
    return ' '.join([word for word in string.split() if not re.findall(
        r'[^\s\u0621\u0622\u0623\u0624\u0625\u0626\u0627\u0628\u0629\u062A\u062B\u062C\u062D\u062E\u062F\u0630\u0631\u0632\u0633\u0634\u0635\u0636\u0637\u0638\u0639\u063A\u0640\u0641\u0642\u0643\u0644\u0645\u0646\u0647\u0648\u0649\u064A]',
        word)])
```

Testons cette méthode sur un exemple :

```python
string = "مواڞفات وسعر هاتف أيفون 8 الجدي"
```

```python
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
```

```python
print("Avant: ",string)
string = remove_extra_whitespace(string)
print("Après: ",string)
```
```python
before:  مواڞفات       وسعر هاتف     أيفون 8    الجدي
after:  مواڞفات وسعر هاتف أيفون 8 الجدي
```
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
```

```python
print("Avant: ",string)
string = remove_extra_whitespace(string)
print("Après: ",string)
```
Nous pouvons voir ci-dessus que remove_extra_whitespace (string) supprime les espaces blancs supplémentaires d'une chaîne et retourner la version propre.