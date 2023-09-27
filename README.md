# Active-Directory-Administration

|     | Description |
|-----|-------------|
| 1-  | Manage Users | 
| 2-  | Manage Groups and Other Organizational Units | 
| 3-  | Manage Group Policy Objects |
| 4-  | Add and Remove Computers To The Domain | 

## 1 - Manage Users

Notre première tâche consiste à ajouter quelques utilisateurs nouvellement embauchés dans AD. Nous allons simplement les créer dans la portée "inlanefreight.local", en descendant dans la structure des dossiers "Corp > Employés > HQ-NYC > IT" pour le moment. Une fois que nous aurons créé nos autres groupes, nous les déplacerons dans les nouveaux dossiers. Vous pouvez utiliser le module PowerShell Active Directory (New-ADUser), l'extension Active Directory Utilisateurs et Ordinateurs ou MMC pour effectuer ces actions.

A) Utiliser le module PowerShell Active Directory (New-ADUser)

<code>New-ADUser -Name "Orion Starchaser" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="o.starchaser@inlanefreight.local"}</code>

Après avoir appuyé sur Entrée, une invite apparaîtra. Veuillez entrer un mot de passe sécurisé pour l'utilisateur.

B) Ajouter un utilisateur AD via l'interface graphique (GUI)

Pour ajouter un utilisateur AD via l'interface graphique (GUI), nous devons d'abord ouvrir "Active Directory Utilisateurs et Ordinateurs" via le menu Démarrer, dans le dossier "Outils d'administration".

![](https://imgur.com/QOZ9hAy.png)

Veuillez remplir les informations comme suit :

![](https://imgur.com/ioNUa2l.png)

![](https://imgur.com/C1kHych.png)

C) Supprimer un utilisateur à l'aide de la console MMC (snap-in)

La méthode la plus simple à partir de la console ADUC (Active Directory Users and Computers) sera d'utiliser la fonction de recherche. Inlanefreight compte de nombreux utilisateurs répartis dans plusieurs OU (Unités d'organisation). Voici comment utiliser la fonction de recherche :

1 - Cliquez avec le bouton droit de la souris sur "Employees" et sélectionnez "Rechercher".
2 - Tapez le nom d'utilisateur que vous souhaitez rechercher, dans ce cas, "Paul Valencia", puis cliquez sur "Rechercher maintenant". Si un utilisateur porte ce nom, les résultats de la recherche apparaîtront dans la partie inférieure de la fenêtre de recherche.
3 - Maintenant, cliquez avec le bouton droit de la souris sur l'utilisateur et sélectionnez "Supprimer". Une fenêtre contextuelle apparaîtra pour confirmer la suppression de l'utilisateur. Cliquez sur "Oui".
4 - Pour vérifier que l'utilisateur a bien été supprimé, vous pouvez utiliser à nouveau la fonction de recherche pour rechercher l'utilisateur.

![](https://imgur.com/wMDJvSv.png)

![](https://imgur.com/hpPlr5d.png)

![](https://imgur.com/96GXIt1.png)



