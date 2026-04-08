# Trieur de Fichiers Automatique

Ce projet est un Gestionnaire de Fichiers Intelligent. Il permet de trier, renommer et organiser automatiquement les fichiers d'un dossier donné selon leurs extensions.

## Guide d'Utilisation (Mode d'Emploi)

Vous avez un dossier en désordre rempli d'images, de factures PDF et de vidéos ? Laissez l'application tout ranger pour vous !

1. **Lancement :** Double-cliquez sur l'icone du programme (ou lancez `python main.py` depuis votre terminal).
2. **Sélection :** Cliquez sur le bouton "Parcourir..." pour cibler votre dossier en désordre.
3. **Options de renommage (Facultatif) :**
   *(Ces options vous permettent de personnaliser au maximum les noms de vos fichiers)*
   - **Préfixe personnalisé** : Tapez un mot qui s'ajoutera au début de chaque fichier. 
     *Exemple : Si vous tapez `Projet_`, le fichier `facture.pdf` deviendra `Projet_facture.pdf`.*
   - **Ajouter la date du jour** : Cochez cette case pour préfixer vos fichiers avec la date du jour.
     *Exemple : Le fichier `photo.jpg` deviendra `2026-03-24_photo.jpg`.*
     *(Si vous utilisez les deux options, la date précède le préfixe : `2026-03-24_Projet_photo.jpg`)*
   - **Générer un fichier historique_tri.txt** : Laissez cette case cochée pour obtenir un compte-rendu textuel (placé dans de dossier source) qui trace tous les déplacements effectués. Très utile en cas de doute !
4. **Action :** Cliquez sur le gros bouton bleu **"LANCER LE TRI INTELLIGENT"**.
5. **C'est fini !** La fenêtre vous affiche les actions réalisées. Vos fichiers sont à présent rangés dans des sous-dossiers classés par types précis (**Word, Excel, PowerPoint, Access, PDF, Images, Vidéos, Musiques, Code, etc.**). 


---

##  Pour aller plus loin

*   **Compiler en .exe :** Si vous souhaitez transformer ce script en un logiciel indépendant, consultez le fichier [CONVERSION.md](CONVERSION.md).

---


## 1. Architecture du Projet (MVC Simplifié)

L'application est découpée en 3 fichiers principaux pour séparer l'interface de la logique :

*   **`main.py` (Le chef d'orchestre) :** C'est le point d'entrée. Son seul rôle est d'importer l'interface graphique et de lancer l'application.
*   **`gui.py` (La vue / L'interface) :** Contient le code Tkinter. Ce fichier gère la fenêtre, les boutons, les champs de texte et les barres de progression. Il récolte les informations saisies par l'utilisateur.
*   **`logic.py` (Le cerveau / La logique) :** Contient l'algorithme de tri. Il reçoit les ordres de l'interface, effectue les opérations sur les fichiers du disque dur, et renvoie un rapport d'exécution.

---

# Utilisation de l'application

## I Télecharger le fichier TrieurFichiers.exe
## II. Lancer TrieurFichiers.exe avec un double clic

## 2. Fonctionnement de l'Application (Pas à Pas)

Voici ce qui se déroule lorsque vous utilisez l'application :

### Étape A : L'Interface (`gui.py`)
1.  L'utilisateur sélectionne un **dossier cible** via le bouton "Parcourir".
2.  Il peut paramétrer le tri : ajout d'un préfixe, ajout de la date du jour, génération ou non d'un log.
3.  Au clic sur **"Lancer le tri intelligent"**, l'interface valide les données et les transmet à la fonction métier `trier_et_renommer` située dans `logic.py`.

### Étape B : Le Cerveau (`logic.py`)
C'est ici que le vrai traitement est effectué :
1.  **Vérification :** Le programme s'assure que le dossier source existe bien.
2.  **Scan :** Il liste tous les fichiers du dossier (`os.listdir`).
3.  **Catégorisation :** Il extrait l'extension de chaque fichier (`.jpg`, `.pdf`, etc.) et la compare à un dictionnaire (`CATEGORIES`).
4.  **Création :** Si le sous-dossier (ex: "Images", "Documents") n'existe pas, il le crée (`os.makedirs`).
5.  **Renommage et Sécurité :** Le fichier est renommé en ajoutant la date et/ou le préfixe. S'il y a un conflit (un fichier avec ce nom exact existe déjà dans le dossier de destination), le script ajoute un compteur `(1)`, `(2)`, etc. afin de garantir qu'**aucun fichier ne soit supprimé par mégarde**.
    *Exemple : Si `Image_01.jpg` existe déjà dans le dossier "Images", le programme renommera automatiquement votre fichier entrant en `Image_01(1).jpg`.*
6.  **Déplacement :** Le fichier est finalement déplacé (`shutil.move`) dans son sous-dossier.
7.  **Historique :** Toutes ces actions sont gardées en mémoire dans un journal, puis enregistrées dans `historique_tri.txt` si l'option est cochée.

### Étape C : Le Résultat (`gui.py`)
1.  `logic.py` termine son traitement et renvoie le journal complet à l'interface.
2.  L'interface affiche ce résumé d'exécution dans la zone de logs.
3.  Une fenêtre popup informe du succès final de l'opération.
