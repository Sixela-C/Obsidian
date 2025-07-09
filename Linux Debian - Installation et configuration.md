---
up:
  - "[[]]"
---
## 1. Installation et configuration de la VM sur VMWare Worsktation :

1. Lancer le logiciel de virtualisation VMWare Workstation

2. Dans la barre de menu en haut à gauche, cliquer sur **File** puis sur **New Virtual Machine**

3. Dans la fenêtre **New Virtual Machine Wizard**, sélectionner **Typical**, puis cliquer sur **Next** :
![[image-1.png]]

4. Dans le menu **Guest Operating System Installation**, cocher **Installer disc image file (iso)** et dans le menu déroulant sélectionner le fichier **.iso** souhaité, puis cliquer sur **Next** :
![[image-2.png]]

5. Dans le menu **Name the Virtual Machine**, nommer la VM comme souhaité et choisir l'emplacement de son enregistrement, puis cliquer sur **Next** :
![[image-3.png]]

6. Dans la fenêtre **Specify Disk Capacity**, sélectionner la taille maximum pour le disque (cette taille peut être ajusté ultérieurement) et cocher **Store virtual disk as a single file**, puis cliquer sur **Next** :
![[image-4.png]]

7. Dans la fenêtre **Ready to creat Virtual Machine**, vérifier l'ensemble des paramètres, puis cliquer sur **Customize Hardware** :
![[image-5.png]]

Dans la fenêtre **Hardware**, cliquer sur **Network Adapter** et cocher **Bridged: Connected directly to the physical network** puis cliquer sur **Close** :
![[image-6.png]]

De retour dans la fenêtre **New Virtual Machine Wizard**, cliquer sur **Finish**.
La VM se lance automatiquement, sinon double-cliquer sur le nom de la VM dans le menu latéral gauche de VMWare Workstation.

# 2. Installation et configuration de Debian :

1. Démarrer la VM Debian
2. Sélectionner **Graphical install** ou **Install**
![[image-7.png]]

3. Sélectionner la langue de votre choix puis appuyer sur la touche **Entrée**
![[image-8.png]]

4. Sélectionner le pays de votre localisation géographique puis appuyer sur la touche **Entrée**
![[image-9.png]]

5. Sélectionner la disposition du clavier en fonction de la langue choisie puis appuyer sur la touche **Entrée**
![[image-10.png]]

6. Inscrire le nom de la machine choisi puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-11.png]]

7. Inscrire le nom de domaine qu'intégrera la machine (le cas échéant ) puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-12.png]]

8. Créer le mot de passe _**root**_ et noter-le, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-13.png]]

9. Confirmer le mot de passe _**root**_ en le renseignant une seconde fois, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-15.png]]

10. Créer un compte utilisateur courant en renseignant le nom complet choisi pour cet utilisateur, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-16.png]]

11. Poursuivre la création du compte utilisateur courant en renseignant le login choisi pour cet utilisateur, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-17.png]]

12. Poursuivre la création du compte utilisateur courant en renseignant le mot de passe choisi (différent de celui de _**root**_), puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-18.png]]

13. Confirmer le mot de passe choisi pour l'utilisateur courant en le renseignant à nouveau, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-19.png]]

14. Choisir le mode de partitionnement du disque puis appuyer sur la touche **Entrée**
![[image-20.png]]

15. Sélectionner le disque à partitionner, qui sera effacé, puis appuyer sur la touche **Entrée**
![[image-21.png]]

16. Sélectionner le schéma de partitionnement, idéalement avec des partitions séparées, puis appuyer sur la touche **Entrée**
![[image-22.png]]

17. Confirmer les informations sur le partitionnement des disques,  en appuyant sur la touche **TAB** jusqu'à ce que **Oui** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-23.png]]

18. Modifier (le cas échéant) l'espace utilisé pour le partitionnement des disques, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-24.png]]

19. Valider les informations relatives au partitionnement des disques, en appuyant sur la touche **TAB** jusqu'à ce que **Oui** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-25.png]]

20. Patienter durant l'**Installation du système de base**
21.  Pour la configuration de l'outil de gestion des paquets, sélectionner **Non** en surbrillance rouge en appuyant sur la touche **TAB** puis en appuyant sur la touche **Entrée**
![[image-26.png]]

22. Sélectionner le pays du miroir pour les mises à jour des paquets en fonction de votre localisation, puis appuyer sur la touche **Entrée**
![[image-27.png]]

23. Sélectionner le miroir de votre choix, puis appuyer sur la touche **Entrée**
![[image-28.png]]

24. Renseigner le cas échéant l'adresse du proxy, puis appuyer sur la touche **TAB** jusqu'à ce que **Continuer** soit en surbrillance rouge puis appuyer sur la touche **Entrée**
![[image-29.png]]

25. Patienter durant le téléchargement des mises à jour
26. Refuser de fournir anonymement des informations statistiques en appuyant sur la touche **TAB** jusqu'à avoir **Non** en surbrillance rouge puis en appuyant sur la touche **Entrée**
![[image-31.png]]

27. Sélectionner les logiciels à installer (le cas échéant) en appuyant sur la touche **Espace**, puis appuyer sur la touche **TAB** jusqu'à avoir **Continuer** en surbrillance rouge et appuyer sur la touche **Entrée**
![[image-32.png]]

28. Confirmer l'installation de **Grub** en appuyant sur la touche **Entrée**
![[image-33.png]]

29. Choisir le périphérique d'installation pour **Grub** puis appuyer sur la touche **Entrée**
![[image-34.png]]

30. Patienter durant le processus de finalisation de l'installation
31. Pour terminer l'installation, appuyer sur la touche **Entrée**
![[image-35.png]]

32. Le système redémarre automatiquement. Félicitation, vous avez installé Debian avec succès