Exemples de fichiers G-Code
===========================

Table des matières

**JavaScript must be enabled in your browser to display the table of contents.**

Après l’installation de Machinekit, plusieurs exemples de fichiers G-code se trouveront dans le répertoire /nc\_files du dossier d’installation. Seuls les fichiers adaptés au type de la machine sont utilisable.

Exemples pour une fraiseuse
---------------------------

### Fraisage hélicoïdal d’un orifice

-   Nom du fichier: useful-subroutines.ngc

-   Description: Programme pour usiner une poche ou un alésage avec utilisation des paramètres.

### Rainurage

-   Nom du fichier: useful-subroutines.ngc

-   Decription: Programme pour fraiser une rainure avec utilisation des paramètres.

![](images/useful-subroutines-ngc.png)

Figure 1. Les différents usinages de l’exemple

### Palpage d’une grille rectangulaire de points

-   Nom du fichier: gridprobe.ngc

-   Description: Relevé d’une grille rectangulaire de points au palpeur.

Ce programme palpe de manière répétitive, en suivant une grille régulière en XY et écrit les points mesurés dans le fichier *probe-results.txt* situé dans le même répertoire que le fichier .ini.

![](images/gridprobe-ngc.png)

Figure 2. La grille de palpage

### Amélioration du palpage d’une grille rectangulaire de points

-   Nom du fichier: smartprobe.ngc

-   Description: Relevé d’une grille rectangulaire de points au palpeur.

Ce programme est une amélioration du précédent. Il palpe de manière répétitive, en suivant une grille régulière en XY et écrit les points mesurés dans le fichier *probe-results.txt* situé dans le même répertoire que le fichier .ini.

![](images/smartprobe-ngc.png)

Figure 3. La grille de palpage plus fine

### Mesure de longueur d’outil

-   Nom du fichier: tool-lenght-probe.ngc

-   Description: Mesure automatique de la longueur de l’outil.

Ce programme donne un exemple de la mesure automatique de longueur d’outil en utilisant un contact raccordé à l’entrée sonde. C’est très pratique pour une machine sur laquelle la longueur des outils est différente à chaque montage.

### Mesure d’un alésage au palpeur

-   Nom du fichier: probe-hole.ngc

-   Description: Mesure le centre et le diamètre d’un alésage.

Ce programme montre comment trouver le centre d’un alésage, comment calculer son diamètre et enregistrer les mesures dans un fichier.

### Compensation de rayon d’outil

-   Nom du fichier: comp-g1.ngc

-   Description: Mouvements d’entrée et de sortie avec la compensation de rayon d’outil.

Ce programme démontre la particularité du chemin d’outil sans et avec la compensation de rayon d’outil. Le rayon d’outil est pris dans la table d’outils.

Exemples pour un tour
---------------------

### Filetage

-   Nom du fichier: lathe-g76.ngc

-   Description: Dressage, filetage et tronçonnage.

Ce programme donne un exemple de filetage automatique sur un tour avec utilisation des paramètres. Il demande quelques adaptations pour fonctionner, ces adaptations seront un excellent exercice.

------------------------------------------------------------------------

Dernière mise à jour 2015-12-20 22:15:56 CET


