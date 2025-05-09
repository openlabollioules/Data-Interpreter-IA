# Projet de Data Interpreter IA

Ce projet est une application REST qui permet de traiter divers types de fichiers (à savoir .xls, .xlsx, .csv, .json, .pdf, .py) et de générer des analyses sur ces fichiers en utilisant un modèle de langage large (LLM). Il intègre des fonctionnalités d'extraction de texte, d'images, de données relationnelles et de code Python. Il utilise l'écosystème LangChain, des bases de données DuckDB, ainsi que FastAPI pour l'interface utilisateur.

### Présentation de l'algorithme du Data Interpreter sous forme de schéma

![Schéma de l'algorithme](./assets/data_interpreter_explain.png)

## Prérequis

- Python 3.11 ou plus récent
- `tesseract` pour l'extraction OCR
- Librairies Python listées dans `requirements.txt`
- `ollama` pour utiliser les LLM

### Installation de Tesseract

Ubuntu/Debian :
```bash
sudo apt install tesseract-ocr
```

macOS :
```bash
brew install tesseract
```

### Installation de Ollama

Pour utiliser les modèles LLM avec Ollama, suivez les instructions ci-dessous :

1. Installez Ollama :

    macOS :
    ```bash
    brew install ollama
    ```

    Linux :
    ```bash
    curl -o- https://ollama.com/download.sh | bash
    ```

2. Lancez le service Ollama :

    ```bash
    ollama serve
    ```

3. Téléchargez les modèles LLM requis :

    ```bash
    ollama pull duckdb-nsql:latest
    ollama pull llama3.2:latest
    ollama pull command-r-plus:latest
    ```
   
4. Installez Ollama :

    macOS :
    ```bash
    brew install ollama
    ```

    Linux :
    ```bash
    curl -o- https://ollama.com/download.sh | bash
    ```

## Installation du Projet

1. Clonez le répertoire du projet :

    ```bash
    git clone <URL_DU_PROJET>
    cd <nom_du_répertoire>
    ```

2. Installez les dépendances nécessaires à l'aide de `requirements.txt` :

    ```bash
    pip install -r requirements.txt
    ```

## Lancer l'Application

Pour exécuter l'application, lancez la commande suivante :

```bash
sh run_terminal.sh <chemin_vers_vos_fichiers>
```

- Remplacez `<chemin_vers_vos_fichiers>` par le chemin des fichiers que vous souhaitez traiter.

L'application se lancera sur `http://0.0.0.0:8000`.

## Pour obtenir des indications de lancement dans le terminal


```bash
sh run_terminal.sh --help
```

## Utilisation de l'API REST

L'API REST est développée avec FastAPI. Voici un exemple d'utilisation :

- Endpoint : `/query/`
- Méthode : `POST`
- Corps de la requête :
  ```json
  {
    "complex_query": "<votre_question>"
  }
  ```
- Réponse :
  ```json
  {
    "analysis_result": "<résultat_de_l'analyse>"
  }
  ```

Vous pouvez tester l'API à l'aide d'un outil comme `Postman` ou `curl`.

### Exemple d'Exécution via Curl

```bash
curl -X POST "http://localhost:8000/query/" -H "Content-Type: application/json" -d '{"complex_query": "Donne-moi les statistiques de ventes"}'
```

## Fonctionnalités Clés

1. **Extraction de Texte et Images des PDF :** Le projet utilise `pdfminer.six` pour extraire le texte et `PyMuPDF` pour extraire les images des fichiers PDF.
2. **Extraction de Texte par OCR :** `pytesseract` est utilisé pour extraire le texte des images présentes dans les PDF.
3. **Analyse de Code Python :** Extraction des fonctions, classes, imports et autres éléments d'un fichier `.py` en utilisant le module `ast`.
4. **Chargement de Données dans une Base DuckDB :** Les fichiers CSV, Excel, JSON, et PDF peuvent être chargés dans DuckDB.
5. **Génération Automatique de Réponses et d'Outils :** Utilisation de modèles LLM (à partir de LangChain) pour générer des plans d'action, des requêtes SQL et des analyses complètes.

## Organisation des Fichiers

- `main.py` : Le script principal pour exécuter l'application.
- `requirements.txt` : Liste des dépendances à installer.
- `README.md` : Ce fichier de documentation.
- `CHANGELOG.md` : Ce fichier récapitulatif des différentes versions du projet.
- `src/`: Ce dossier contient les fichiers sources du projet. 
- `pipelines/`: Ce dossier contient le script python de la sous solution sous forme de pipeline OpenWebui.
- `.env`: Ce fichier contient les variables d'environnement essentielles pour une bonne exécution.
- `docker-compose.yml`: Ce script Docker va permettre de mettre en place l'interface OpenWEBUI ainsi que son serveur Pipeline
- `run_pipelines.sh`: Ce script sh va permettre de lancer l'interface OpenWEBUI.
- `run_terminal.sh`: Ce script va permettre de lancer le data interpreter en mode terminal.
- `version.py`: Ce fichier python référence la version actuelle de la solution déployée.

## Exemples de Fichiers Supportés

- **Excel (.xls, .xlsx, xlsm)** : Chargement de toutes les feuilles disponibles dans une base de données.
- **CSV (.csv)** : Chargement dans une table DuckDB avec traitement préalable.
- **JSON (.json)** : Normalisation des données imbriquées et chargement.
- **PDF (.pdf)** : Extraction de texte et images avec OCR.
- **Python (.py)** : Analyse et extraction du code, des fonctions, classes, et autres éléments Python.

## Exemple de commandes additionnelles

- `sh run_terminal.sh --help` : Permet d'obtenir le helper du programme
- `sh run_terminal.sh --v` : Permet d'obtenir le numéro de version du programme actuel

## Flags à inclure si besoin dans les requêtes

- `#force` : Permet de forcer le traitement d'une question meme si elle a été traitée précédemment.
- `#pass` : Permet de ne pas utiliser le processus de traitement et de questionner simplement le LLM.

### Exemple de requête avec le flag #force

```
{
  "complex_query": "Quel est le nom de la première colonne ? #force"
}
```

### Exemple de requête avec le flag #pass

```
{
  "complex_query": "Quel est le nom de la première colonne ? #pass"
}
```

## Configuration de l'environnement

À la racine du projet, vous allez pouvoir retrouver un fichier `.env` qui vous permettra de configurer les différents paramètres du projet.
- `DB_PATH` : Chemin vers la base de données DuckDB.
- `LLM_MODEL` : Modèle LLM à utiliser (par exemple, `llama3.2`).
- ...

Ces paramètres peuvent être modifiés selon vos besoins.

## Lancement Interface OpenWebui

Pour lancer l'interface OpenWebui, exécutez la commande suivante :

```bash
sh run_pipelines.sh
```

## Les commandes utiles à lancer dans le mode terminal

- Afin d'avoir une documentation détaillée, vous pouvez faire cette commande help au démarrage.
```bash
sh run_terminal.sh --help
```

- Afin de connaitre la version actuelle de la solution : 
```bash
sh run_terminal.sh --v
```

## Avertissement

Ce projet est en cours de développement et peut contenir des bugs. Utilisez-le à vos risques et périls. Les résultats générés par le LLM peuvent ne pas être exacts ou appropriés pour toutes les situations. Veuillez toujours vérifier les résultats avant de les utiliser dans un contexte critique.
