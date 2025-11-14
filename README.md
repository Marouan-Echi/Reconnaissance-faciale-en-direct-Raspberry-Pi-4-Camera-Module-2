# Reconnaissance faciale en direct — Raspberry Pi 4 + Camera Module 2

Ce projet permet d'exécuter une reconnaissance faciale en temps réel (Raspberry Pi 4 ou machine locale) en utilisant `Picamera2` (sur Raspberry Pi), `OpenCV` et la bibliothèque `face_recognition`.

Le script principal s'appelle : `face_recognition_live.py`.

## Objectif

- Détecter les visages dans le flux vidéo de la caméra.
- Reconnaître les visages connus à partir d'un répertoire `dataset/` (un dossier par personne).
- Capturer et stocker les visages non reconnus dans `unknown_faces/` pour revue ultérieure.

## Structure recommandée du dépôt

```
project-root/
├─ face_recognition_live.py       # script principal
├─ dataset/                       # dossiers par personne (à créer)
│  ├─ Marouan/
│  │  ├─ img01.jpg
│  │  └─ img02.jpg
│  └─ Alice/
│     ├─ img01.jpg
│     └─ img02.jpg
├─ unknown_faces/                 # images des visages non reconnus
├─ face_encodings.pkl             # encodages générés par le script (cache)
└─ README.md                      # ce fichier
```

> Remarque : `dataset/` doit contenir un dossier par personne; chaque dossier contient les images de cette personne.

## Préparer les dossiers `dataset/` et `unknown_faces/`

1. Créer `dataset/` à la racine du projet si nécessaire.
2. Pour chaque personne, créer un dossier nommé par son identité (ex. `dataset/Marouan/`) et y placer ses photos.
3. Créer `unknown_faces/` (le script peut aussi le créer automatiquement s'il n'existe pas).

Commandes utiles (Linux / Raspberry Pi) :

```bash
mkdir -p dataset/Marouan
mkdir -p unknown_faces
```

Commandes PowerShell (Windows) :

```powershell
New-Item -ItemType Directory -Path dataset\Marouan -Force
New-Item -ItemType Directory -Path unknown_faces -Force
```

Vous pouvez ajouter un fichier `.gitkeep` dans ces dossiers pour les versionner même s'ils sont vides :

```bash
# Linux
touch dataset/.gitkeep
touch unknown_faces/.gitkeep
```

## Bonnes pratiques pour les photos

- Formats acceptés : `.jpg`, `.jpeg`, `.png` (préférez `.jpg` pour l'espace disque).
- Nombre d'images recommandées par personne : 5–20.
- Variété : inclure des photos de face, profils légers, différentes expressions et éclairages.
- Qualité : éviter les images floues, le visage doit occuper une bonne partie de l'image.
- Nom des dossiers : privilégiez des noms simples sans espaces ni caractères spéciaux (ex. `Marouan`, `Alice_Smith`).

## Exemple d'ajout d'une personne

1. Créer le dossier : `dataset/NomPersonne/`.
2. Copier les images : `cp /chemin/vers/*.jpg dataset/NomPersonne/` (ou via l'explorateur Windows).
3. Supprimer `face_encodings.pkl` si vous voulez forcer la reconstruction des encodages au prochain lancement.
4. Lancer le script : `python3 face_recognition_live.py`.

## Installation (Raspberry Pi recommandé)

1. Mettre à jour le système :

```bash
sudo apt update && sudo apt upgrade -y
```

2. Installer les dépendances système (exemple pour Raspberry Pi) :

```bash
sudo apt install -y python3-picamera2 python3-opencv libatlas-base-dev libopenblas-dev liblapack-dev cmake build-essential
```

3. (Optionnel si compilation requise) :

```bash
sudo apt install -y git python3-dev dphys-swapfile
# augmenter temporairement le swap si dlib échoue à compiler
```

4. Créer un environnement Python et installer les dépendances Python :

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install face_recognition dlib imutils opencv-python
```

Sur Windows, adaptez l'activation du venv :

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
pip install --upgrade pip
pip install face_recognition opencv-python imutils
```

Note : l'installation de `dlib` peut requérir la compilation et des dépendances système.

## Lancer le script

Linux / Raspberry Pi :

```bash
source venv/bin/activate   # si vous utilisez un venv
python3 face_recognition_live.py
```

Windows (PowerShell) :

```powershell
venv\Scripts\Activate.ps1
python face_recognition_live.py
```

## Comportement attendu

- Au démarrage, le script parcourt `dataset/`, calcule les encodages des visages et sauvegarde un cache dans `face_encodings.pkl`.
- Pendant l'exécution, les visages détectés sont comparés aux encodages connus ; si un visage n'est pas reconnu, il est capturé dans `unknown_faces/` avec un nom contenant la date/heure.
- Une fenêtre vidéo s'affiche avec les cadres et les noms des personnes reconnues ; appuyez sur `ESC` pour quitter.

## Fichiers et constantes utiles (dans le script)

- `KNOWN_FACES_DIR` : chemin vers `dataset/` (par défaut `dataset`).
- `UNKNOWN_FACES_DIR` : chemin vers `unknown_faces/`.
- `ENCODINGS_FILE` : nom du fichier de cache (par défaut `face_encodings.pkl`).

(Ouvrez `face_recognition_live.py` pour voir les lignes exactes où ces constantes sont définies et ajuster si besoin.)

## Dépannage

- Pas d'image caméra : vérifier le branchement et tester avec `libcamera-hello` (Raspberry Pi).
- Erreurs lors de l'installation de `dlib` : installer `cmake`, `build-essential`, BLAS/LAPACK puis augmenter le swap si nécessaire.
- Fenêtre OpenCV absente via SSH : exécuter depuis l'environnement graphique ou rediriger l'affichage.
- Performances faibles : réduire la résolution de la caméra et la taille des images traitées.

## Confidentialité et conformité

Les images et encodages sont des données biométriques sensibles. Assurez-vous d'obtenir le consentement des personnes et de respecter la législation locale (ex. RGPD).

## Résumé rapide (pour démarrer)

1. Créez `dataset/<NomPersonne>/` et ajoutez 5–20 images par personne.
2. Créez (ou laissez créer) `unknown_faces/`.
3. Installez les dépendances appropriées pour votre plateforme.
4. Lancez : `python3 face_recognition_live.py`.

---

Si vous le souhaitez, je peux :

- ajouter un petit script pour créer automatiquement un dossier `dataset/NomPersonne` et redimensionner des images ;
- générer un rapport qui liste les sous-dossiers présents dans `dataset/` et le nombre d'images par dossier ;
- ajouter un `CONTRIBUTING.md` avec une checklist pour ajouter de nouvelles personnes.

Dites-moi quelle option vous voulez et je l'ajoute.
