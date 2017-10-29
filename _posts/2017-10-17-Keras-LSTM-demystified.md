---
title: "TALN: Prétraitement du texte arabe dans 10 lignes de codes"
categories:
  - NLP
tags:
  - Python
  - Regex
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
* **remove_urls(string)** : supprime toutes les URL d'une chaîne et renvoie la version propre
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
