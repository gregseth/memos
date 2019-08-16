Problèmes récurrents et procédures utiles
=========================================

Word 2007+ : Correction orthographique 
--------------------------------------

### Symptômes ####

La vérification lors de la frappe ne fonctionne plus (le zigzag rouge 
n'apparaît plus) alors que l'option est bien activée.

### Solutions envisageables ###

#### Problème de langue ####

- Sélectionner le texte du document (Ctrl-A)
- Révision > Langue > Définir la langue de vérification... 
- Sélectionner la langue voulue et décocher les cases _[ ] Ne pas vérifier..._ 
    et _[ ] Détecter automatiquement la langue_

#### Paramétrage du document ####

- Fichier > Options > Vérification
- Décocher les cases _[ ] Masquer les fautes d'orthographe..._ et 
    _[ ] Masquer les erreurs grammaticales..._


Word 2007+ : Styles vérouillés
------------------------------

### Symptômes ###

Impossible de modifier les styles : tout est grisé.

### Solution ###

Aller dans l'onglet _Révision_ et cliquer sur _Restreindre la modification_ et 
tout décocher.


Word 2003+ : Supprimer un renvoi tout en conservant le texte
------------------------------------------------------------

Sélectionner le renvoi et faire <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F9</kbd>.
    

PDF : Suppression de la protection par mot de passe
---------------------------------------------------

### Symptômes ###

Impossibilité d'ouvrir un PDF protégé par mot de passe contre l'édition dans 
Adobe Illustrator (ou tout autre logiciel permettant d'éditer un PDF).

### Solution ###

- Ouvrir le PDF avec _Utilitaire ColorSync_
- Enregistrer sous...
- Si la fonction est désactivée, Imprimer... et utiliser la fonction 
    d'enregistrement native.
	
Autre possibilité (Beuh style) : 
    
    pdf2ps "locked_file.pdf" | ps2pdf - "unlocked_file.pdf"


Visual Studio : Afficher un marqueur de colonne
-----------------------------------------------

- En base de registre, ajouter la valeur chaîne `Guides` à la clé 
    `[HKCU]\Software\Microsoft\VisualStudio\X.X\TextEditor` où `X.X` correspond
    à la version de Visual à modifier. 
- Lui donner une valeur de la forme : `RGB(rr,gg,bb) xx1 xx2 ...` où les `xx` 
    désignent la colonne où placer la marque -- la ligne est placée _après_ la
    colonne `xx` (e.g.: `RGB(255,0,0) 80 128`).


Visual Code : Proxy et bugs graphiques
--------------------------------------

### Symptômes ###

La fenêtre d'authentification du proxy s'affiche à chaque démarrage (voire 
plus). Des problèmes d'affichage surviennent, notament dans la mini vue à droite
des documents.

### Solution ###

Passer des options supplémentaires au lancement de VSCode. Pour le proxy :

    code --proxy-server="http=$http_proxy;https=$https_proxy"

Pour l'affichage, désactiver l'accélération matérielle avec :

    code --disable-gpu

Avec l'environnement Gnome, il peut s'avérer judicieux d'ajouter ces options au 
fichier `/usr/share/applications/code.desktop`, pour une utilisation
automatique en mode graphique.


AFP : Le répertoire partagé ne monte pas
----------------------------------------

### Sympômes ###

Un disque ou un répertoire partagé en AFP ne monte pas et fait freezer le 
Finder (peut aller jusqu'au plantage).
Éventuellement un message d'erreur indique que le version de protocole n'est
pas prise en charge, lors d'une connexion en resaisissant les informations 
d'identification. 

### Solution ###

Accéder au répertoire partagé autrement (SMB, accès local, ...) et supprimer
les répertoires `.AppleDB` et `.AppleDesktop`  se trouvant à la racine.


macOS / Spotlight
-----------------

### Symptômes ###

Le raccourci clavir défini pour afficher la barre de recherche Spotlight ne fonctionne plus (rien ne s'affiche).

### Solution ####

Supprimer le fichier :

    $HOME/Library/Preferences/com.apple.HIToolbox.plist


Ouverture de session automatique sous Windows 7
-----------------------------------------------

### Solution ###

    control userpasswords2

Puis décocher la case "les utilisateur doivent entrer un login et un mot de 
passe" et saisir le nom et le mot de passe de la session qui sera ouverte
automatiquement.


Windows Vista/7 : désactiver ClearType sans avoir à saigner des yeux
--------------------------------------------------------------------

### Problème ###

L'utilisation de ClearType est problématique à de nombreux niveaux. Mais s'en
passer l'est aussi : les polices Microsoft optimisées pour ClearType (comme 
Segoe UI, Calibri, Cambria, Consolas, ...) sont absolument illisibles 
(pixellisées ET floues) une fois celui-ci désactivé. Le plupart du temps le 
choix des polices est laissé à l'utilisateur. Mais malgré tous les réglages,
la police Segoe UI reste à beaucoup d'endroits : comment s'en débarasser ?

### Solution ###

Tout d'abord pour l'interface générale de Windows (et des application qui 
permettent de le définir) changer la police Segoe UI 9pt pour du Tahoma 8pt.

Ensuite coller les lignes suivantes dans un fichier .reg et l'ajouter au 
registre. Cela permet de simuler l'absence de Segoe UI et force la police de 
fallback à Arial (n'importe quelle autre police peut être utilisée, mais 
attention à bien utiliser le nom de celle-ci -- comme affiché dans les listes
de polices des programmes -- et non le nom du fichier TTF du dossier Fonts).

    Windows Registry Editor Version 5.00
    
    [HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts]
    
    "Segoe UI (TrueType)"=""
    "Segoe UI Bold (TrueType)"=""
    "Segoe UI Italic (TrueType)"=""
    "Segoe UI Bold Italic (TrueType)"=""
    
    [HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontSubstitutes]
    
    "Segoe UI"="Arial"
	
Pour restaurer l'utilisation du Segoe UI, faire de même avec :

    Windows Registry Editor Version 5.00
    
    [HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts]
    "Segoe UI (TrueType)"="segoeui.ttf"
    "Segoe UI Bold (TrueType)"="segoeuib.ttf"
    "Segoe UI Italic (TrueType)"="segoeuii.ttf"
    "Segoe UI Bold Italic (TrueType)"="segoeuiz.ttf"
    
    [HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontSubstitutes]
    
    "Segoe UI"=-

Source : [How To Get Rid of Segoe UI Font][1]


Windows : Désactiver les programmes au démarrage dans le registre
-----------------------------------------------------------------

### Problème ###

Des programmes se lancent à l'ouverture de session alors qu'ils sont absents
du dossier « Démarrage » (ou « Startup ») dans le menu Démarrer.

### Solution ###

Vérifier les clés de registre suivantes :

    [HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run]
    [HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce]
    [HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServices]
    [HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServicesOnce]
    
    [HKCU\Software\Microsoft\Windows\CurrentVersion\Run]
    [HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce]
    [HKCU\Software\Microsoft\Windows\CurrentVersion\RunServices]
    [HKCU\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce]

Source : [Manage the Programs Run at Windows Startup][2]


Illustrator : Inverser le sens de l'axe vertical (> CS5)
--------------------------------------------------------

### Problème ###

Dans Illustrator, à partir de la CS5, l'axe des ordonnées est orienté vers le
bas. 

### Solution ###

Pour revenir à un axe vertical orienté vers le haut, comme dans la CS4 et
précédentes, il faut éditer le fichier de configuration d'Illustrator. Celui-ci
se trouve dans :

    ~/Library/Preferences/Adobe Illustrator CS<X> Settings/<locale>/Adobe Illustrator Prefs

Modifier les valeurs suivantes :

    /isRulerIn4thQuad 1
	/isRulerOriginTopLeft 1


Windows : Caractères spéciaux
-----------------------------

Windows propose une série de caractères spéciaux accessibles avec la combinaison
de touches `Alt` + `<NUM>`, à choisir parmi : 

 1. ☺ 
 2. ☻ 
 3. ♥  
 4. ♦ 
 5. ♣ 
 6. ♠ 
 7. • 
 8. ◘ 
 9. ○ 
 10. ◙ 
 11. ♂ 
 12.  ♀  
 13.  ♪  
 14. ♫ 
 15. ☼ 
 16. ► 
 17. ◄ 
 18. ↕ 
 19. ‼ 
 20. ¶ 
 21. § 
 22. ▬ 
 23. ↨ 
 24.  ↑  
 25.  ↓  
 26. → 
 27. ← 
 28. ∟ 
 29. ↔ 
 30. ▲ 
 31. ▼ 


Chemins intéressants à retenir
------------------------------

### Visual Studio ###

- user keywords

    %PROGAM_FILES%/Microsoft Visual Studio 2008/Common7/IDE/usertype.dat

### Eclipse ###

#### Configuration files ####

    .metadata/.plugins/org.eclipse.core.runtime/.settings/

#### syntax coloring ####

    org.eclipse.jdt.ui.prefs

#### text editors ####

    org.eclipse.ui.editors.prefs

in a general manner add any *ui*, *core* or *jdt* file from this folder.

#### Project files ###

    .metadata/.plugins/org.eclipse.core.resources/.projects



  [1]: http://forums.whirlpool.net.au/archive/938337
  [2]: http://www.pctools.com/guides/registry/detail/109/

