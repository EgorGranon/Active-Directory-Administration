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

D) Déverrouiller un compte utilisateur:

Danc cet exemple on va utiliser le compte Adam Masters (amasters)

En utulisant powershell : <code>PS C:\htb> Unlock-ADAccount -Identity amasters</code>

Nous devons également définir un nouveau mot de passe pour l'utilisateur(1) et les contraindre à le changer lors de la prochaine connexion(2).

(1)<code>PS C:\htb> Set-ADAccountPassword -Identity 'amasters' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rdReset!" -Force)</code>

(2)<code>PS C:\htb> Set-ADUser -Identity amasters -ChangePasswordAtLogon $true</code>

Pour déverrouiller le compte d'utilisateur d'Adam Masters à partir de l'interface graphique (GUI), nous utiliserons ADUC(Active Directory Users & Computers), tout comme lorsque nous avons ajouté un utilisateur au domaine précédemment.

![](https://imgur.com/2losXYj.png)

## 2 - Manage Groups and Other Organizational Units

A) From Powershell

Pour créer une nouvelle unité d'organisation (OU) et un groupe, nous pouvons effectuer les actions suivantes :

<code>PS C:\htb> New-ADOrganizationalUnit -Name "Security Analysts" -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=CORP,DC=INLANEFREIGHT,DC=LOCAL"</code>

Tout d'abord, nous avons créé la nouvelle unité d'organisation (OU) pour regrouper nos analystes et leurs ressources. Ensuite, nous devons créer un groupe de sécurité pour ces utilisateurs.

<code>PS C:\htb> New-ADGroup -Name "Security Analysts" -SamAccountName analysts -GroupCategory Security -GroupScope Global -DisplayName "Security Analysts" -Path "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" -Description "Members of this group are Security Analysts under the IT OU"</code>

B) From MMC Snap-in

Cela sera un processus en deux étapes rapide pour nous. Tout d'abord, nous devons créer une nouvelle unité d'organisation (OU) pour accueillir nos analystes en sécurité. Pour ce faire, nous allons :

Naviguer vers l'OU "Corp > Employés > HQ-NYC > IT". Nous allons créer un nouveau conteneur au sein de l'IT.
Faire un clic droit sur IT et sélectionner "Nouveau > Unité d'organisation". Une nouvelle fenêtre devrait apparaître.
Saisir le nom "Analystes en sécurité" dans le champ Nom et laisser l'option par défaut sélectionnée pour la case à cocher Protéger. Appuyer sur OK, et l'OU devrait être créée.

![](https://imgur.com/HcnPFfd.png)

Maintenant que nous avons notre OU, créons le groupe de sécurité pour nos analystes.
Faites un clic droit sur notre nouvelle OU "Analystes en sécurité" et sélectionnez "Nouveau > Groupe". Une fenêtre contextuelle devrait apparaître.

Saisissez le nom du groupe "Analystes en sécurité".
Sélectionnez la portée du groupe "Domaine local".
Assurez-vous que le type de groupe soit "Sécurité" et non "Distribution".
Une fois que vous avez vérifié les options, appuyez sur OK.

![](https://imgur.com/ivlGvGH.png)

Nous devons déplacer nos nouveaux utilisateurs dans l'OU et les ajouter au groupe de sécurité. Gardez à l'esprit que l'objectif est d'organiser logiquement nos objets AD pour les localiser et les administrer facilement. En utilisant les groupes de sécurité, nous pouvons rapidement attribuer des autorisations et des ressources à des utilisateurs spécifiques au lieu de gérer chaque utilisateur individuellement.

Powershell : 

<code>PS C:\htb> Add-ADGroupMember -Identity analysts -Members ACepheus,OStarchaser,ACallisto</code>

MMC snap-in:

1)Trouvez l'utilisateur que vous souhaitez ajouter.
2)Faites un clic droit sur l'utilisateur et sélectionnez "Ajouter à un groupe". Une nouvelle fenêtre apparaîtra pour spécifier le nom du groupe.
3)Saisissez tout ou partie du groupe auquel vous souhaitez ajouter l'utilisateur. Dans ce cas, nous ajoutons Andromeda au groupe "Analystes en sécurité". Si notre requête correspond à un ou plusieurs groupes, 4)une autre boîte de dialogue apparaîtra, nous fournissant une liste de groupes parmi lesquels choisir. Sélectionnez le groupe nécessaire et appuyez sur "OK".
5)Le choix que vous avez sélectionné sera maintenant mis en évidence dans la fenêtre précédente. Plusieurs groupes peuvent être sélectionnés en même temps si nécessaire. Une fois terminé, appuyez sur "OK".
6)Si aucun problème ne se produit, une nouvelle fenêtre contextuelle vous informera que l'opération est terminée. Pour le valider, vous pouvez afficher les propriétés du groupe ou de l'utilisateur.

![](https://imgur.com/K8MUycs.png)

![](https://imgur.com/pWPKlbL.png)

## 3 - Manage Group Policy Objects

Pour dupliquer un objet de stratégie de groupe (GPO), nous pouvons utiliser la cmdlet Copy-GPO ou le faire depuis la console de gestion des stratégies de groupe.

### Dupliquez l'objet via PowerShell:

<code>Copy-GPO -SourceName "Nom de la GPO d'origine" -TargetName "Nom de la GPO dupliquée"</code>

La commande ci-dessus prendra la GPO "Logon Banner" et la copiera dans un nouvel objet nommé "Security Analyst Control". Cet objet aura toutes les anciennes attributs de la GPO "Logon Banner", mais il ne sera pas appliqué à quoi que ce soit tant que nous ne le lierons pas.

Liez la nouvelle GPO à une OU:

<code>PS C:\htb> New-GPLink -Name "Security Analysts Control" -Target "ou=Security Analysts,ou=IT,OU=HQ-NYC,OU=Employees,OU=Corp,dc=INLANEFREIGHT,dc=LOCAL" -LinkEnabled Yes</code>

La commande ci-dessus prendra la nouvelle GPO que nous avons créée, la liera à l'OU "Security Analysts" et l'activera. Pour l'instant, c'est tout ce que nous allons faire depuis PowerShell. Nous devons encore apporter quelques modifications à la stratégie, mais nous effectuerons ces actions depuis la console de gestion des stratégies de groupe. Modifier les préférences de la GPO depuis PowerShell peut être un peu intimidant et dépasse largement le cadre de ce module.

### Modiier un GPO via GPMC(Group Policy Management console)

1)Pour modifier notre nouvel objet de stratégie :

2)Nous devons ouvrir GPMC (Group Policy Management Console) et développer l'arborescence des objets de stratégie de groupe pour voir quels GPO existent.

3)Faites un clic droit sur l'objet de stratégie que nous souhaitons modifier et sélectionnez "Modifier". L'éditeur de gestion des stratégies de groupe devrait s'ouvrir dans une nouvelle fenêtre.

4)À partir de là, nous avons plusieurs options pour activer ou désactiver des paramètres.

5)Nous devons modifier les paramètres relatifs aux supports amovibles et nous assurer qu'ils sont configurés pour bloquer l'accès à tout support amovible. Nous autoriserons explicitement les analystes en sécurité à accéder à PowerShell et CMD, car leurs tâches quotidiennes l'exigent.

6)Emplacement des paramètres de stratégie pour les supports amovibles = Configuration utilisateur > Stratégies > Modèles d'administration > Système > Accès aux supports amovibles.
Emplacement des paramètres pour l'invite de commandes (Command Prompt) = Configuration utilisateur > Stratégies > Modèles d'administration > Système.
Pour les paramètres de l'ordinateur, nous devons nous assurer que la bannière de connexion (Logon Banner) est appliquée et que les paramètres de la stratégie de mot de passe pour ce groupe sont renforcés.

7)Emplacement des paramètres de la bannière de connexion = Configuration ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies locales > Options de sécurité.
Pour référence, ce paramètre devrait déjà être activé puisque la GPO que nous avons copiée était pour une bannière de connexion. Nous validons les paramètres et nous nous assurons qu'il est activé et appliqué.
Emplacement des paramètres de la stratégie de mot de passe = Configuration ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies de compte > Stratégie de mot de passe.

![](https://imgur.com/vraKiAD.png)

## 4 - Add and Remove Computers To The Domain

PowerShell rejoindre un Domain: 

<code>PS C:\htb> Add-Computer -DomainName INLANEFREIGHT.LOCAL -Credential INLANEFREIGHT\HTB-student_adm -Restart</code>

Cette chaîne utilise le domaine (INLANEFREIGHT.LOCAL) auquel nous souhaitons joindre l'hôte, et nous devons spécifier l'utilisateur dont les informations d'identification seront utilisées pour autoriser la connexion (HTB-student_adm). Spécifier le redémarrage dans la chaîne est nécessaire car la connexion ne se fera pas tant que l'hôte ne redémarrera pas à nouveau, ce qui lui permettra d'acquérir les paramètres et les stratégies du domaine.

