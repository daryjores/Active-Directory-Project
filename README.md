<h1>Active Directory Home Lab avec création d'utilisateurs en masse</h1>

<h2>Description</h2>
Ce projet est une démonstration de la façon dont j'ai créé un environnement de laboratoire domestique Active Directory à l'aide de VMWare. Je configure un serveur Microsoft pour y exécuter Active Directory. J'ai ensuite configuré un contrôleur de domaine qui me permettra de gérer un domaine. J'ai ensuite exécuté un script Powershell pour créer plus de 1000 utilisateurs dans Active Directory et me connecter à ces comptes nouvellement créés sur un autre client qui utilise le domaine que j'ai configuré pour se connecter à l'internet. Ce laboratoire simule un environnement professionnel. Pour ce laboratoire, j'aurai besoin d'une ISO Microsoft Server 2019, d'une ISO Windows 10 Enterprise, de VMWare et d'un script Powershell.
<br />

<h2>Langues et utilitaires utilisés</h2>

- <b>Active Directory</b> 
- <b>PowerShell</b>
- <b>CMD</b>

<h2>Environnements utilisés </h2>

- <b>VMWare</b>
- <b>Microsoft Server 2019</b>
- <b>Windows 10</b> (21H2)

<h2>Liens</h2>

- <b>VMWare:</b> https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
- <b>Microsoft Server 2019:</b> https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
- <b>Windows 10 ISO:</b> https://www.microsoft.com/en-us/software-download/windows10

<h2 align="center">Program walk-through</h2>

<p align="center">
<b>Le diagramme de réseau que j'utiliserai pour ce projet</b> <br/>
<img src="https://i.imgur.com/IfxvoYS.png" height="80%" width="80%" alt="Network Diagram"/>
<br />
<br />
<b>Pour la machine virtuelle qui hébergera mon contrôleur de domaine, j'ai besoin de deux adaptateurs réseau. J'ai besoin d'un NAT qui utilisera l'adresse IP de mon routeur domestique et d'un adaptateur réseau interne pour que mon DC puisse communiquer avec d'autres machines virtuelles. Pour le réseau interne, j'utiliserai VMnet0. Reportez-vous au diagramme au début</b> <br/>
<img src="https://i.imgur.com/UHBjxOd.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/7CLcFGU.jpg" height="80%" width="80%" alt="Configuring the Network Adapter for the Domain Controller Virtual Machine"/>
<br />
<br />
<b>Après avoir téléchargé Windows Server 2019 sur la machine virtuelle, la première chose que je dois faire est de configurer les deux adaptateurs réseau que j'ai. L'un est le NIC externe et l'autre est le NIC interne.</b> <br/>
<img src="https://i.imgur.com/SwAH8sj.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Je dois maintenant déterminer quel NIC est notre NAT. C'est Ethernet0 parce que son DNS est localdomain.</b> <br/>
<img src="https://i.imgur.com/JE7zCDh.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<img src="https://i.imgur.com/mjczWGf.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Je renomme les adaptateurs pour qu'il me soit plus facile de savoir lequel est lequel et c'est important plus tard lors de la configuration du DC et du DHCP.</b> <br/>
<img src="https://i.imgur.com/iiYpjCy.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Je configure maintenant la carte réseau interne et lui attribue une adresse IP basée sur le schéma ci-dessus (172.16.0.1) et je n'ai pas besoin de lui donner une passerelle par défaut car le contrôleur de domaine est la passerelle. Quant au serveur DNS, je lui attribue une adresse IP basée sur le diagramme car lorsque nous installerons Active Directory, il installera le DNS. Je l'ai configuré en tant qu'adresse loopback afin qu'il s'interroge lui-même.</b> <br/>
<img src="https://i.imgur.com/JIPk50Q.jpg" height="80%" width="80%" alt="Configuring the Network"/>
<br />
<br />
<b>Maintenant que je sais quel adaptateur réseau est notre adaptateur externe et interne, je vais de l'avant et renomme le PC du nom long et compliqué qu'il a maintenant en DC (Domain Controller). Cela force un redémarrage, ce qui est très bien</b> <br/>
<img src="https://i.imgur.com/TklrxzC.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<b>Après avoir redémarré, je lance le processus de téléchargement d'Active Directory. La vidéo est courte mais le téléchargement est réussi.</b> <br/>

https://user-images.githubusercontent.com/108043108/176958623-4276b4d1-3c6e-469c-8875-49007d003aa2.mp4

<br />
<br />
<p align="center">
<b>J'ai installé les services de domaine Active Directory, mais nous n'avons jamais défini le serveur (ou l'ordinateur) comme domaine. Je dois maintenant créer le domaine</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176959446-c4992d67-9bdc-4a5b-8229-e6d18850e29d.mp4

<br />
<br />
<p align="center">
<b>Lorsque le serveur est promu à un domaine, il force un redémarrage. Lorsque je me reconnecte, vous pouvez voir que le domaine a été créé avec succès car mon compte administrateur a maintenant MYDOMAIN devant lui!</b> <br/>
</p>
<img src="https://i.imgur.com/BmLk2Gc.jpg" height="80%" width="80%" alt="Renaming the PC"/>
<br />
<br />
<p align="center">
<b>Maintenant, au lieu d'utiliser le compte d'administration intégré, je vais créer un compte d'administration dédié au domaine </b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176960581-a0934287-267c-464e-8eee-8b85ae523910.mp4

<br />
<br />
<p align="center">
<b>J'ai créé un compte d'administrateur spécifique au domaine, mais il n'a pas de privilèges d'administrateur. Je dois aller dans Active Directory et promouvoir ce nouveau compte en tant qu'administrateur. Lorsque je fais cela, je me déconnecte du compte d'administrateur intégré et je me connecte à mon compte d'administrateur de domaine nouvellement créé !</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961105-3f8df2e0-f1e2-490b-a457-e1dd3fc2470a.mp4

<br />
<br />
<p align="center">
<b>Je dois maintenant installer et configurer le RAS/NAT pour que mon ordinateur client Windows 10 puisse accéder à Internet via le réseau interne par l'intermédiaire du contrôleur de domaine.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961534-0afd1ae0-c421-4af7-ae76-7b2cefe43bdd.mp4

<br />
<br />
<p align="center">
<b>NMaintenant que le rôle est installé, je dois configurer le Routage et l'Accès à distance.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176961801-c6ed71d1-4f12-4775-82d5-853cac5260da.mp4

<br />
<br />
<p align="center">
<b>C'est très bien ! Maintenant que l'accès à distance est installé et configuré, il est temps d'installer un serveur DHCP. Cela permettra d'attribuer une adresse IP à nos clients Windows 10 et de leur permettre de naviguer sur Internet..</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176962485-229ae237-b9e9-4178-b857-4741d318d33e.mp4

<br />
<br />
<p align="center">
<b>Nous allons maintenant configurer le DHCP et définir une portée. L'objectif du DHCP est de permettre aux ordinateurs du réseau de se voir attribuer automatiquement une adresse IP. L'étendue que je vais créer attribuera des adresses IP dans une plage, la plage étant 172.16.0.100-200, de sorte que le DHCP sera effectivement en mesure d'attribuer 100 adresses IP. J'ai également fixé à 20 jours la durée pendant laquelle les adresses IP peuvent être louées. La raison de la location est que lorsqu'une adresse IP est attribuée, elle ne peut pas être utilisée par d'autres appareils. Ainsi, si je ne dispose que de 100 adresses IP et que 100 d'entre elles sont utilisées, les nouveaux appareils ne peuvent se voir attribuer une adresse IP sur le réseau, ce qui signifie qu'ils ne peuvent pas se connecter à l'internet. Un bail est simplement la durée pendant laquelle une adresse IP peut être détenue (louée) par un appareil avant d'être recyclée. S'il s'agissait, par exemple, d'un café qui offre le wifi et que le temps moyen passé par une personne à l'intérieur de ce café était de 2 heures, il serait absurde de lui louer une adresse IP pour 20 jours. Cela reviendrait à verrouiller cette adresse IP pour cette durée et personne d'autre ne pourrait l'utiliser. S'il s'agissait d'un café, je recommanderais de fixer la durée du bail à moins de 4 heures et de donner une fourchette plus large. Comme il s'agit uniquement d'une VM, la durée du bail n'a pas d'importance.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176962663-866da4fd-de06-4549-9b86-3925a674bb3d.mp4

<br />
<br />
<p align="center">
<b>Pour obtenir mon script powershell à partir d'Internet, je dois pouvoir naviguer sur le Web. Je dois désactiver les fonctions de sécurité du contrôleur de domaine. S'il s'agissait d'un environnement de production, je ne ferais jamais cela, car il y a un risque de sécurité. Comme il ne s'agit que d'un environnement de laboratoire pour moi, ce n'est pas un problème. Je pourrais naviguer sur Internet sans faire cette étape, mais c'est ennuyeux parce qu'il nous enverra des avertissements pour chaque page Web que nous visiterons.</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176964588-4b7ab303-c338-4037-8142-996bde30cac3.mp4

<br />
<br />
<p align="center">
<b>Maintenant qu'Active Directory est configuré et que mon contrôleur de domaine l'est également, j'utilise un script Powershell pour créer plus de 1000 comptes d'utilisateurs.</b> <br/>
</p>
<img src="https://i.imgur.com/ISI6fPb.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>Voici une vidéo de l'exécution du script !</b> <br/>
</p>

https://user-images.githubusercontent.com/108043108/176953922-60b62f24-fd3f-41b4-ae0c-8780afc7b708.mp4

<br />
<br />
<p align="center">
<b>Le script s'est exécuté avec succès et les résultats confirmant que les comptes d'utilisateurs ont été créés sont étonnants. Certains doublons n'ont pas été créés, mais cela peut être facilement résolu en ajoutant au script Powershell quelques lignes de code qui lui indiqueront ce qu'il faut faire en cas de doublons. Quelque chose comme "En cas de doublon, ajouter un 1 à la fin du nom du compte", par exemple. Si vous voulez voir le code complet utilisé, référez-vous au début de ce dépôt. Le script se trouve sous CREATE_USERS.ps1</b> <br/>
</p>
<img src="https://i.imgur.com/MhlDg1o.jpg" height="80%" width="80%" alt="Using Powershell to create 1000 user accounts in bulk"/>
<br />
<br />
<p align="center">
<b>Il est maintenant temps de créer une nouvelle machine virtuelle qui agira comme un utilisateur dans le domaine. Je nomme cette machine CLIENT1</b> <br/>
</p>
<img src="https://i.imgur.com/wvBRBWf.jpg" height="80%" width="80%" alt="Setting up new Virtual Machine"/>
<br />
<br />
<p align="center">
<b>Je configure l'adaptateur réseau de manière à ce qu'il ne soit pas NAT et qu'il ne puisse pas se connecter à Internet sur mon réseau local. La seule façon pour cette machine virtuelle de se connecter à Internet est de se voir attribuer une IP par le DC sur le serveur VM. Reportez-vous au diagramme au début. Je dois changer l'adaptateur réseau pour qu'il soit sur le même réseau interne que le contrôleur de domaine, dans ce cas VMnet0.</b>  <br/>
</p>
<img src="https://i.imgur.com/6IjDUEj.jpg" height="80%" width="80%" alt="Configuring the VM Network Adapter"/>
<br />
<br />
<p align="center">
<b>Après avoir configuré une machine virtuelle distincte qui simulera un employé se connectant au domaine. Faisons d'une pierre deux coups en renommant l'ordinateur CLIENT1 et en cliquant sur la case pour devenir membre du domaine mydomain.com. Je suis invité à donner mon identifiant de connexion et j'ai choisi d'utiliser le compte d'administrateur que j'ai configuré plus tôt.</b>  <br/>
</p>
<img src="https://i.imgur.com/ceB3tDJ.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>J'ai réussi à rejoindre le domaine en tant que membre !</b>  <br/>
</p>
<img src="https://i.imgur.com/euBgIXf.jpg" height="80%" width="80%" alt="Configuring the Client VM"/>
<br />
<br />
<p align="center">
<b>Je me connecte à un compte d'utilisateur que j'ai créé à partir du script Powershell pour tester si tout est configuré correctement. Au lieu de me connecter au compte d'utilisateur créé lors de la création de la machine virtuelle, j'essaie de me connecter à un compte d'utilisateur créé dans MYDOMAIN.</b>  <br/>
</p>
<img src="https://i.imgur.com/dPeaySX.gif" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>Exécution de la commande promt pour voir si la VM cliente reçoit l'adresse IP correctement assignée par le contrôleur de domaine. Nous pouvons voir que le contrôleur de domaine m'a correctement loué une adresse IP (encerclé en rouge) et que lorsque j'envoie un ping au domaine, cela fonctionne (encerclé en jaune).</b>  <br/>
</p>
<img src="https://i.imgur.com/QBWuCS9.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>Un dernier test pour vérifier que l'environnement de travail et les utilisateurs en masse que j'ai créés fonctionnent.</b>  <br/>
</p>
<img src="https://i.imgur.com/j6ZRHPz.jpg" height="80%" width="80%" alt="Testing The Environment"/>
<br />
<br />
<p align="center">
<b>Je retourne sur mon serveur VM et je vérifie le DCHP pour voir combien d'adresses ont été louées. Nous pouvons voir ici, encerclée en rouge, que ma machine virtuelle CLIENT1 s'est vu louer une adresse. S'il s'agissait d'un véritable environnement d'entreprise, il y aurait des centaines, voire des milliers d'adresses louées dans ce dossier, en fonction de la durée du bail, bien entendu ! Dans cet environnement, j'ai fixé la mienne à 20 jours</b>  <br/>
</p>
<img src="https://i.imgur.com/qvNKa7v.jpg" height="80%" width="80%" alt="Checking leased addresses"/>
<br />
<br />
<p align="center">
<b>Voici une autre façon de vérifier combien d'ordinateurs ou de périphériques sont actuellement connectés au domaine. Nous pouvons voir que mon ordinateur CLIENT1 est correctement reconnu dans Active Directory. Encore une fois, s'il s'agissait d'un environnement réel, il y aurait probablement des milliers d'appareils dans ce dossier.</b>  <br/>
</p>
<img src="https://i.imgur.com/A2dMovv.jpg" height="80%" width="80%" alt="Checking the computers in Active Dirctory"/>
<br />
<br />
<p align="center">
<b>Ici, je fais défiler tous les comptes d'utilisateurs que j'ai créés avec Powershell. Plus de 1000 ont été créés!</b>  <br/>
</p>
<img src="https://i.imgur.com/POpjnf9.gif" height="80%" width="80%" alt="Checking the Users created by Powershell"/>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

