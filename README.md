# PowerView.ps1

PowerView est un outil PowerShell permettant d'acquérir une reconnaissance de la situation du réseau sur les domaines Windows Actice Directory
Il contient un ensemble de remplacements purement PowerShell pour diverses commandes Windows « net * », qui utilisent les hooks PowerShell AD et les fonctions API Win32 sous-jacentes pour exécuter des fonctionnalités de domaine Windows utiles.

Il implémente également diverses métafonctions utiles, y compris certaines fonctions de recherche d'utilisateurs personnalisées qui identifieront l'endroit où des utilisateurs spécifiques du réseau sont connectés. Il peut également vérifier sur quelles machines du domaine l'utilisateur actuel dispose d'un accès administrateur local. Plusieurs fonctions d'énumération et d'abus de confiance de domaine existent également. Voir les descriptions des fonctions pour une utilisation appropriée et les options disponibles. Pour une sortie détaillée des fonctionnalités sous-jacentes, transmettez les indicateurs -Verbose ou -Debug.

Pour les fonctions qui énumèrent plusieurs machines, transmettez l'indicateur -Verbose pour obtenir un état de progression au fur et à mesure que chaque hôte est énuméré. La plupart des fonctions « méta » acceptent un tableau d'hôtes du pipeline.

# Fonctions diverses :
Export-PowerViewCSV       - ajout CSV thread-safe
Resolve-IPAddress         - résout un nom d'hôte en IP
ConvertTo-SID             - convertit un nom d'utilisateur/groupe donné en un identifiant de sécurité (SID)
Convert-ADName            - convertit les noms d'objets entre une variété de formats
ConvertFrom-UACValue      - convertit une valeur UAC int en une forme lisible par l'homme
Add-RemoteConnection      - pseudo "monte" une connexion à un chemin distant à l'aide de l'objet d'identification spécifié
Remove-RemoteConnection   - détruit une connexion créée par New-RemoteConnection
Invoke-UserImpersonation  - crée une nouvelle connexion de type "runas / netonly" et emprunte l'identité du jeton
Invoke-RevertToSelf       - annule toute usurpation d'identité de jeton
Get-DomainSPNTicket       - demande le ticket Kerberos pour un nom principal de service spécifié (SPN)
Invoke-Kerberoast :       demande des tickets de service pour les comptes compatibles Kerberoast et renvoie les hachages de tickets extraits.
Get-PathAcl               - récupère les ACL pour un chemin de fichier local/distant avec récursivité de groupe facultative

# Fonctions de domaine/LDAP :
Get-DomainDNSZone                 - énumère les zones DNS Active Directory pour un domaine donné
Get-DomainDNSRecord               - énumère les enregistrements DNS Active Directory pour une zone donnée
Get-Domain                        - renvoie l'objet de domaine pour le domaine actuel (ou spécifié)
Get-DomainController              - renvoie les contrôleurs de domaine pour le domaine actuel (ou spécifié)
Get-Forest                        - renvoie l'objet forêt pour la forêt actuelle (ou spécifiée)
Get-ForestDomain                  - renvoie tous les domaines de la forêt actuelle (ou spécifiée)
Get-ForestGlobalCatalog           - renvoie tous les catalogues globaux de la forêt actuelle (ou spécifiée)
Find-DomainObjectPropertyOutlier  - identifie les objets utilisateur/groupe/ordinateur dans AD qui ont des propriétés « aberrantes » définies
Get-DomainUser                    - renvoie tous les utilisateurs ou objets utilisateur spécifiques dans AD
New-DomainUser                    - crée un nouvel utilisateur de domaine (en supposant les autorisations appropriées) et renvoie l'objet utilisateur
Set-DomainUserPassword            - définit le mot de passe pour une identité utilisateur donnée et renvoie l'objet utilisateur
Get-DomainUserEvent               - énumère les événements de connexion au compte (ID 4624) et la connexion avec les événements d'informations d'identification explicites
Get-DomainComputer                - renvoie tous les ordinateurs ou objets informatiques spécifiques dans AD
Get-DomainObject                  - renvoie tous les objets de domaine (ou spécifiés) dans AD
Set-DomainObject                  - modifie une propriété gven pour un objet Active Directory spécifié
Get-DomainObjectAcl               - renvoie les ACL associées à un objet Active Directory spécifique
Add-DomainObjectAcl               - ajoute une ACL pour un objet Active Directory spécifique
Find-InterestingDomainAcl         - recherche les ACL d'objets dans le domaine actuel (ou spécifié) avec des droits de modification définis sur des objets non intégrés
Get-DomainOU                      - recherche de toutes les unités d'organisation (OU) ou d'objets d'UO spécifiques dans AD
Get-DomainSite                    - recherche de tous les sites ou objets de site spécifiques dans AD
Get-DomainSubnet                  - recherche tous les sous-réseaux ou objets de sous-réseaux spécifiques dans AD
Get-DomainSID                     - renvoie le SID du domaine actuel ou du domaine spécifié
Get-DomainGroup                   - renvoie tous les groupes ou objets de groupe spécifiques dans AD
New-DomainGroup                   - crée un nouveau groupe de domaine (en supposant les autorisations appropriées) et renvoie l'objet groupe
Get-DomainManagedSecurityGroup    - renvoie tous les groupes de sécurité du domaine actuel (ou cible) pour lesquels un gestionnaire est défini
Get-DomainGroupMember             - renvoie les membres d'un groupe de domaine spécifique
Add-DomainGroupMember             - ajoute un utilisateur (ou un groupe) de domaine à un groupe de domaine existant, en supposant les autorisations appropriées pour le faire
Get-DomainFileServer              - renvoie une liste de serveurs fonctionnant probablement comme serveurs de fichiers
Get-DomainDFSShare                - renvoie une liste de tous les systèmes de fichiers distribués tolérants aux pannes pour le domaine actuel (ou spécifié)

# Fonctions d'énumération 
Get-NetLocalGroup                 - énumère les groupes locaux sur la machine locale (ou distante)
Get-NetLocalGroupMember           - énumère les membres d'un groupe local spécifique sur la machine locale (ou distante)
Get-NetShare                      - renvoie les partages ouverts sur la machine locale (ou distante)
Get-NetLoggedon                   - renvoie les utilisateurs connectés sur la machine locale (ou distante)
Get-NetSession                    - renvoie les informations de session pour la machine locale (ou distante)
Get-RegLoggedOn                   - renvoie qui est connecté à la machine locale (ou distante) via l'énumération des clés de registre distantes
Get-NetRDPSession                 - renvoie les informations de bureau/session à distance pour la machine locale (ou distante)
Test-AdminAccess                  - reste en attente si l'utilisateur actuel dispose d'un accès administratif à la machine locale (ou distante)
Get-NetComputerSiteName           - renvoie le site AD où réside la machine locale (ou distante)
Get-WMIRegProxy                   - énumère le serveur proxy et les contenus WPAD pour l'utilisateur actuel
Get-WMIRegLastLoggedOn            - renvoie le dernier utilisateur qui s'est connecté à la machine locale (ou distante)
Get-WMIRegCachedRDPConnection     - renvoie des informations sur les connexions RDP sortantes de la machine locale (ou distante)
Get-WMIRegMountedDrive            - renvoie des informations sur les lecteurs montés en réseau enregistrés pour la machine locale (ou distante)
Get-WMIProcess                    - renvoie une liste de processus et de leurs propriétaires sur la machine locale ou distante
Find-InterestingFile              - recherche les fichiers sur le chemin donné qui correspondent à une série de critères spécifiés

# Fonctions « méta » filetées
Find-DomainUserLocation           - recherche les machines du domaine auxquelles des utilisateurs spécifiques sont connectés
Find-DomainProcess                - recherche les machines du domaine sur lesquelles des processus spécifiques sont actuellement en cours d'exécution
Find-DomainUserEvent              - recherche les événements de connexion sur le domaine actuel (ou distant) pour les utilisateurs spécifiés
Find-DomainShare                  - recherche les partages accessibles sur les machines du domaine
Find-InterestingDomainShareFile   - recherche des fichiers correspondant à des critères spécifiques sur les partages lisibles dans le domaine
Find-LocalAdminAccess             - recherche les machines sur le domaine local sur lequel l'utilisateur actuel dispose d'un accès administrateur local
Find-DomainLocalGroupMember       - énumère les membres du groupe local spécifié sur les machines du domain

# Fonctions de "Trust" domaine :
Get-DomainTrust                   - renvoie toutes les approbations de domaine pour le domaine actuel ou un domaine spécifié
Get-ForestTrust                   - renvoie toutes les approbations de forêt pour la forêt actuelle ou une forêt spécifiée
Get-DomainForeignUser             - énumère les utilisateurs qui font partie de groupes en dehors du domaine de l'utilisateur
Get-DomainForeignGroupMember      - énumère les groupes avec des utilisateurs en dehors du domaine du groupe et renvoie chaque membre étranger
Get-DomainTrustMapping            - cette fonction énumère toutes les approbations pour le domaine actuel, puis énumère toutes les approbations pour chaque domaine trouvé
