<div align="right">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/logo inp Polytech grenoble.png" alt="Logo" width=30% height=30%>
  </a>
</div>

<!-- title -->

# projet-LR1110    



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table des matières</summary>
  <ol>
    <li><a href="#1- Analyse du marché des produits commerciaux concurrents">Analyse du marché des produits commerciaux concurrents</a></li>
    <li><a href="#2- Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud))">Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud))</a>
      <ul>
        <li><a href="#2-1- 1er bloc: acquisition">1er bloc: acquisition</a></li>
        <li><a href="#2-2- 2eme bloc: connectivité">2eme bloc: connectivité</a></li>
        <li><a href="#2-3- 3eme bloc: traitement des données">3eme bloc: traitement des données</a></li>
        <li><a href="#2-4- 4eme bloc: présentation des données">4eme bloc: présentation des données</a></li>
      </ul> 
    </li>
   <li><a href="#3- Définition de la sécurité globale (clé de chiffrage)">Définition de la sécurité globale (clé de chiffrage</a>
      <li><a href="#4- Respect de la vie privée du service (RGPD)">Respect de la vie privée du service (RGPD)</a>
      <li><a href="#5- Estimation du coût de la BOM du produit pour 5000 unités produites et estimation de la durée de vie de la batterie de l’objet">Estimation du coût de la BOM du produit pour 5000 unités produites et estimation de la durée de vie de la batterie de l’objet</a>
      <li><a href="#6- Analyse (brève) du cycle de vie du produit “durable” et “sobre” (ACV)">Réaliser une analyse (brève) du cycle de vie du produit “durable” et “sobre” (ACV)</a>   
       <li><a href="#7- Analyse d’avantages et inconvénients des produits concurrents"> Analyse d’avantages et inconvénients des produits concurrents</a>
  </ol>
</details>




Notre projet porte sur l’étude d’un tracker Lora  permettant le suivi de troupeaux d’animaux (bovins...) et la détection d’anomalies tel qu’une attaque de prédateur par exemple.  L’objectif est de pouvoir récupérer les coordonnées GPS émis constamment par celui-ci et pouvoir les lire.
Dans un premier temps,  nous avons commencé par travailler avec le tracker GPS Dragino LoRaWAN LGT-92-LI, basé sur le microcontrôleur STM32L072, qui envoie des données de géolocalisation et permet d’atteindre de longues portées à de faibles débits de données en minimisant la consommation électrique. Nous utilisons la version de tracker rechargeable, avec le boîtier de protection et tracking en temps réel et nous avons également la possibilité d’utiliser le port USB.Ceci dit, l’étude du lgt92 a été relativement rapide avant que nous soyons passé au LR1110.

<!-- Analyse du marché des produits commerciaux concurrents -->
## 1- Analyse du marché des produits commerciaux concurrents


 
Les produits de tracking LoRa concurrents au LR1110 sont nombreux et ils se distinguent par leur caractéristique qu’ils apportent. La principale fonctionnalité est bien évidemment la transmission de données de géolocalisation mais certains possèdent des fonctions annexes tel que retransmettre les données envoyées par un capteur de mouvement (capteur à 9 axes) ou encore l’émission du niveau de batterie de l’appareil afin d’en informer l’utilisateur. Selon certains modèles, de remonter beaucoup plus de données tel que la température, l’humidité, la lumière , le choc, inclinaison, Mouvement et Activité.     	
Les différents tracker concurrents se différencient aussi bien par la clientèle touchée : Entreprise de transports logistiques permettant le suivi de grosses marchandises (conteneurs)  continuellement et éviter les vols et égarement, par les engins des entreprises minières , utilisé aussi pour le suivi de animaux (incluant notre projet) aussi bien dans une ferme ou animal compagnie , d’autres sont spécialisés dans le suivi de véhicules (véhicules personnels, ou suivi de flotte de véhicules de locations).
Nous pouvons citer les trackers :  G62 ou Oyster de chez Digital matter ou les tracker d’activité personnelle (suivi de personnes âgées ou de personnes malades parkinson, Alzheimer) de chez IOT Factory. Ces trackers sont de l’ordre unitaire d’environ 100€





<!-- Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud)) -->
## 2- Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud))


Généralement l’architecture des systèmes IOT se ressemblent se. on a souvent au début de la chaîne un device pour l’acquisition des données ( capteurs, trackers…) , et puis on trouve le bloc de connectivité qui peut différer selon l'intérêt du projet(LORA, LTE-M, NBIOT…), ensuite on trouve le bloc du traitement des données afin de les rendre  exploitable et présentable, cette partie se fait dans un serveur et généralement sur une machine virtuelle ou on peut mettre des programmes tourner derrière (python,c++,c…) ou des outils de gestion des flux comme Node-RED qu’on a adopté comme solution dans notre projet.

Finalement, on trouve le dernier bloc dans la chaîne IOT qui est la présentation des données, Ce dernier bloc peut varier entre la présentation et le stockage des données qui seront reboucler vers le bloc de traitement, encore une fois l'architecture dépend de l'intérêt du projet. 

Dans le cas de  notre projet, on essaye de remonter la donnée d’un tracker de type LR1110 fournis par semtech. Vu que le tracker fonctionne en lora, on s’attend à une architecture globale qui ressemble à la figure ci-dessous.


<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/LoRa-architecture-20.jpg" alt="imagelora" width=60% height=60%>
  </a>
</div>


<div align="center">
 Architecture globale du systeme  
  </a>
</div>  
<br />

On fait communiquer le tracker en lora avec la gateway fournis par fablab, récupérer les donnée en temps réel depuis le serveur TTN et finalement faire une intégration MQTT qui nous permettra de récupérer les données sur notre propre serveur afin de les traiter et les présenter sur un autre endpoint ( qu’il soit un fichier .log sur notre machine virtuelle ou une application sur un téléphone).



<!-- 1er bloc: acquisition -->
###2-1- 1er bloc: acquisition:

<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/141188110.png" alt="image" width=30% height=30%>
  </a>
</div>
<div align="center">
 LR1110
  </a>
</div>  


<br />
Pour le bloc d’acquisition on a un tracker LR1110 fournis de Semtech est un module de traqueur GPS/GNSS ultra-basse consommation qui intègre un récepteur GPS/GNSS haute sensibilité, une horloge temps réel (RTC), un processeur Arm Cortex-M0+ et une mémoire flash. Il prend en charge les signaux GPS, GLONASS, BeiDou, Galileo et QZSS et peut fonctionner avec une alimentation de seulement 1.8V à 3.3V. Le module peut être utilisé dans des applications telles que les trackers de localisation pour animaux, les suiveurs de vélos et les dispositifs de suivi de personnes.




<!-- 2eme bloc: connectivité -->
### 2-2- 2eme bloc: connectivité:

Pour le bloc de connectivité, Le tracker était déjà réclamée par l’utilisateur sur un serveur ttn, pour qu’on puisse l’utiliser  on a partagé les droits d'accès à ce device avec nous. Les droits partagés étaient restreints or on n'avait pas accès à tout ( partie intégration, partie de décodage encodage…). Donc, pour satisfaire cette partie de connectivité, et pouvoir récupérer les données pour pouvoir les traiter ensuite, nous avons implémenter un client MQTT sur notre propre serveur et se souscrire sur le topic de ce device qui est lui même considéré comme un client sur le broker implémenter sur le serveur TTN.




<!-- 3eme bloc: traitement des données -->
### 2-3- 3eme bloc: traitement des données

Pour le bloc de traitement des données on va s'intéresser à la configuration de notre device, au format des messages uplink et downlink échangés et le filtrage des messages. Notre Devise LR1110, comme mentionné précédemment, est déjà réclamé sur TTN, donc il réussit de faire le Join et envoyer son message à base64 qui sera décodé et transformé en JSON sur le serveur TTN, notre rôle c’est récupérer ce message qui sera sous forme de buffer string, le rendre sous format JSON encore une fois, le filtrer et puis stocker les donnée dans un fichier .log ou les présenter sur une interface graphique.



<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/join reussi.PNG" alt="image" width=60% height=60%>
  </a>
</div>

</div>
<div align="center">
join
  </a>
</div>  


<br />

On a pu créer notre propre serveur en créant une instance élastique sur AWS de type linux debian. Sur notre machine virtuelle implémentée dans notre serveur, on a installé les outils nécessaires pour établir une communication avec le broker tels que Node-RED, TLS…) 




<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/instance.jpg" alt="image" width=60% height=60%>
  </a>
</div>

</div>
<div align="center">
 instance elastique: Linux Debian
  </a>
</div>  


<br />

<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/cnction.jpg" alt="image" width=60% height=60%>
  </a>
</div>

</div>
<div align="center">
 connexion au serveur
  </a>
</div>  


<br />

<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/nodeee.png" alt="image" width=60% height=60%>
  </a>
</div>


</div>
<div align="center">
 Lancement de Node
  </a>
</div>  


<br />

Il est important de mentionner les métriques logiciels dans cette partie.
Les codes nécessaires pour réussir cette communication sont : 
-le code du encodeur/décodeur sur le serveur TTN pour transformer les messages de la base64 vers JSON.
- Le code de notre fonction sur NODE pour filtrer L’objet JSON afin d’avoir que la latitude et longitude ainsi que l’accuracy qui sera responsable de déterminer le rayon de la position du tracker.Vous pouvez trouvez les codes dans les fichiers dans le projet. Le fait de se baser sur des outils comme NODE pour gérer les flux nous permet d’utiliser moins de codes donc avoir une implémentation simple et robuste.



<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/flux.PNG" alt="image" width=60% height=60%>
  </a>
</div>


</div>
<div align="center">
Gestion des flux sur Node
  </a>
</div>  


<br />

<!-- 4eme bloc: présentation des données -->
### 4eme bloc: présentation des données

Pour le dernier bloc on a choisi de présenter la position du tracker sur un plan en utilisant le package Worldmap et puis mettre un end point sur red-remote ce qui nous permet de visualiser la position sur votre téléphone.





<br />
<div align="center">
  <a href="https://github.com//Khalilsaidi-polybot/projet-LR1110/blob/main/README.md">
    <img src="images/position.jpg" alt="image" width=40% height=40%>
  </a>
</div>

</div>
<div align="center">
Position du Tracker
  </a>
</div>  


<br />

<!-- Définition de la sécurité globale (clé de chiffrage) -->
## Définition de la sécurité globale (clé de chiffrage)

  Sur l’aspect matériel, le tracker LR1110 dispose d’une suite d’octets définissant le Dev EUI ET LE JoinEUI. Ces codes sont attribués à titre unique pour identifier un appareil dans un réseau Lora : le JoinEUI est utilisé pour l’inscription du tracker dans le réseau et le DevEUI permet d’identifier de manière unique le tracker dans le réseau Lora.

On l’utilise pour établir une communication sécurisée. Le tracker est basé sur un algorithme de chiffrement par blocs AES-128, les données sont traitées par blocs de 128 bits. Il permet de sécuriser les données de localisations et les communications établies. Il prend en charge l’authentification entre appareils à l’aide de clés partagés (via protocole LoraWAN) et permet la confidentialité des informations pour empêcher les accès non autorisés.
 
#### -Sur l’aspect logiciel :
 
La sécurité des gateways Lora repose sur deux protocoles de sécurité : AES-128 et OTAA, permettant de chiffrer les données les données transmises et de gérer la configuration des appareils connectés.
TTN (The Things Networks) est un réseau d’IOT qui utilise aussi le protocole de sécurité AES-128 qui va protéger en chiffrant les données transmises. Le protocole de communication IOT utilisé est : MQTT (Message Queue Telemetry Transport) et permet aux devices IoT (le tracker) de se connecter, dans la finalité, au serveur aws et recevoir les données de géolocalisation. Nous utilisons le protocole de chiffrement TLS pour chiffrer les données transmises



<!-- Respect de la vie privée du service (RGPD) -->
## 4- Respect de la vie privée du service (RGPD)


 
D’après la RGPD, les dispositifs de traceurs GPS utilisés pour suivre une personne, un objet, un animal ou un véhicule, se fait uniquement si celles-ci ont été prévenues (ou leurs propriétaires). Autrement, c’est considéré comme une entrave aux libertés. La loi prévoit une peine de prison pouvant aller jusqu’à 5 ans avec amendes qui varie dépendamment de si l’on est une personne morale ou physique.                	    
Cependant les normes sont différentes sur les balises gps relevant du domaine militaire et civil, offrant de meilleures  précisions sur les applications militaires. Cependant, il n’est pas indiqué le rayon de précision (en mètres) sur la localisation.
 
Notre projet traite des données personnelles en l’occurrence avec la remontée des informations de localisations qu’il transmet continuellement ce qui signifie que toutes 10 minutes la position est remise à jour avec les nouvelles données. Il existe bel et bien des risques d’atteintes au respect de la vie privée au dépend de notre projet IOT.
 
Nous l’avons utilisé à des fins pédagogiques dans le but de simuler le tracking de bovins. Les risques d’atteintes à la vie privée du projet IOT:
 
#### -          La collecte de données personnelles sur la position de localisation du trackeur à des fins malveillantes ou
#### -          Commerciales pour la revente des données à des services Tiers sans consentement des utilisateurs,
Tout cela entraine une violation de la vie privée.
 
Durant nos phases de tests et de configuration du lr1110, nous avons modifié  les paramètres de durée de transmission de données, et sur l’accuracy en testant plusieurs scénarios différents :tel que la diminution du rayon afin d’avoir une meilleure précision dans la réception des données de localisation et la réduction du temps de communication permettant la mise à jour des données de géolocalisation plus fréquemment.


<!-- Estimation du coût de la BOM du produit pour 5000 unités produites et estimation de la durée de vie de la batterie de l’objet -->
## 5- Estimation du coût de la BOM du produit pour 5000 unités produites et estimation de la durée de vie de la batterie de l’objet








Nous avons utilisé le tracker tel qu’il est vendu et nous n’avons pas eu besoin de réaliser le PCB , ou le boîtier contenant le microcontrôleur, connecteurs , et module RF. Nous avons uniquement interagit avec la partie logicielle du tracker pour le faire fonctionner. De plus nous n’avons pas accès aux fournisseurs de semtech, Il sera donc difficile d’en faire une estimation du coût de la BOM.
 
Le coût du traceur se retrouve au prix d’environ 80 €. Cela dit, nous avons identifié les éléments qui composent l’intérieur du traceur puisque nous l’avons démonté à plusieurs reprises lorsque celui-ci ne voulait pas démarrer. Ainsi, il est composé du module Lora semtech LR1110, le MCU, la panoplie des composants électroniques, la batteries (2 piles ½ AA), les connecteurs et la PCB.
 
A partir de ces éléments, on estime le cout d’achat pour chaque composant : le MCU à environ 5-10 €,mémoire flash à 2€,module Lora semetech lr1110 à 20€, composant électronique à environ 4€, batterie environ 3€, boîtier environ 4€ . On peut estimer le cout total de la BOM pour 5000 unités produites à environ 75000€ - 110000€. Les prix incluent également le coût de tests et de certification ETSI du produit.
 
Ici, la batterie est de type demi-AA et la durée de vie de la batterie dépend de plusieurs paramètres tel que la fréquence et conditions d'utilisation. D’après la documentation technique, elle stipule que pour une utilisation normale (device non altéré par la température de stockage par exemple), la durée est de 2 à 3ans.




<!-- Réaliser une analyse (brève) du cycle de vie du produit “durable” et “sobre” (ACV) -->
## 6- Aanalyse (brève) du cycle de vie du produit “durable” et “sobre” (ACV)



L’analyse du cycle de vie d’un produit correspond à l’analyse des impacts environnementaux du LR1110 depuis la production de celui-ci où sont pris en compte l’impact des matières premières tel que le silicium permettant la réalisation des chips, le plastique pour l’enveloppe ou d’autres matières pour le boitier. La phase de production est celle ou il faut prendre le plus en compte l’empreinte environnementale (énergie dépensée, pollution émise lors de l’impression et assemblage des produits composant le tracker…).

Enfin, la consommation d’énergie est un autre des facteurs importants à prendre en compte. Cependant il n’est pas facile de justifier ces étapes avec des nombres concrètement puisqu’il est quasi impossible de remonter toute la chaîne de production, fabrication et d’assemblage.


 Analyse d’avantages et inconvénients des produits concurrents
<!-- Analyse d’avantages et inconvénients des produits concurrents -->
## 7- Analyse d’avantages et inconvénients des produits concurrents

| Types de Trackers | Technologie Lora | Longue portée | Faible consommation d’énergie | Transmission rapide des données | Prix |
|:-----------------:|:----------------:|:-------------:|:-----------------------------:|:-------------------------------:|:----:|
|       LR1110      |         X        |       ++      |               ++              |                ++               |   +  |
|       G62 DM      |         X        |       ++      |               ++              |               +++               |  ++  |
|    IOT Factory    |         X        |       ++      |               ++              |                ++               |   +  |
|   Track Abeeway   |         X        |      +++      |              +++              |                ++               |  +++ |


