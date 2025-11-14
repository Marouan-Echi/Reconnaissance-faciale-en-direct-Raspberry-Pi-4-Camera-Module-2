# dataset — conventions et bonnes pratiques

Ce dossier contient les photos des personnes connues. Chaque personne doit avoir son propre sous-dossier. Le script principal parcourt `dataset/` pour générer les encodages (fichier `face_encodings.pkl`).

Conventions recommandées

- Structure : `dataset/<NomPersonne>/` (ex. `dataset/Marouan/`).
- Noms de dossiers : utilisez des caractères ASCII simples, sans espaces (ex. `Alice_Smith` ou `Marouan`).
- Formats de fichiers : `.jpg`, `.jpeg`, `.png` (préférez `.jpg` pour économiser l'espace).
- Nombre d'images par personne : 5–20 images pour une bonne couverture d'angles et d'éclairages.
- Qualité des images : visage centré, taille suffisante du visage dans l'image, évitez le flou.

Conseils pratiques

- Variez les photos : face de face, profil léger, différentes expressions et éclairages.
- Évitez les photos de groupe ou recadrées de très loin (le visage doit être identifiable).
- Si vous ajoutez des images depuis `unknown_faces/`, vérifiez manuellement l'identité avant de les déplacer.

Commandes utiles

Linux / Raspberry Pi :

```bash
mkdir -p dataset/Marouan
cp /chemin/vers/photo1.jpg dataset/Marouan/
```

PowerShell (Windows) :

```powershell
New-Item -ItemType Directory -Path dataset\Marouan -Force
Copy-Item C:\chemin\vers\photo1.jpg -Destination dataset\Marouan\
```

Après ajout/modification d'images

1. Supprimez `face_encodings.pkl` si vous souhaitez forcer la régénération des encodages au prochain lancement ; sinon, le script peut proposer de recalculer automatiquement.
2. Lancez `face_recognition_live.py` pour reconstruire le cache si nécessaire.

Respect de la vie privée

Les photos sont des données sensibles. Assurez-vous d'avoir le consentement des personnes avant de stocker ou traiter leurs images.
