<br />
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
    <li><a href="#Analyse du marché des produits commerciaux concurrents">Analyse du marché des produits commerciaux concurrents</a></li>
    <li><a href="#Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud))">Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud))</a>
      <ul>
        <li><a href="#1er bloc: acquisition">1er bloc: acquisition</a></li>
        <li><a href="#2eme bloc: connectivité">2 bloc: connectivité</a></li>
        <li><a href="#3eme bloc: traitement des données">3eme bloc: traitement des données</a></li>
        <li><a href="#4eme bloc: présentation des données">4eme bloc: présentation des données</a></li>
      </ul> 
    </li>
   <li><a href="#Définition de la sécurité globale (clé de chiffrage)">Définition de la sécurité globale (clé de chiffrage</a>
      <li><a href="#Respect de la vie privée du service (RGPD)">Respect de la vie privée du service (RGPD)</a>
      <li><a href="#Estimation du coût de la BOM du produit pour 5000 unités produites et estimation de la durée de vie de la batterie de l’objet">Estimation du coût de la BOM du produit pour 5000 unités produites et estimation de la durée de vie de la batterie de l’objet</a>
       <li><a href="#Réaliser une analyse (brève) du cycle de vie du produit “durable” et “sobre” (ACV)">Réaliser une analyse (brève) du cycle de vie du produit “durable” et “sobre” (ACV)</a>
  </ol>
</details>




Notre projet porte sur l’étude d’un tracker Lora  permettant le suivi de troupeaux d’animaux (bovins...) et la détection d’anomalies tel qu’une attaque de prédateur par exemple.  L’objectif est de pouvoir récupérer les coordonnées GPS émis constamment par celui-ci et pouvoir les lire.
Dans un premier temps,  nous avons commencé par travailler avec le tracker GPS Dragino LoRaWAN LGT-92-LI, basé sur le microcontrôleur STM32L072, qui envoie des données de géolocalisation et permet d’atteindre de longues portées à de faibles débits de données en minimisant la consommation électrique. Nous utilisons la version de tracker rechargeable, avec le boîtier de protection et tracking en temps réel et nous avons également la possibilité d’utiliser le port USB.Ceci dit, l’étude du lgt92 a été relativement rapide avant que nous soyons passé au LR1110.

<!-- Analyse du marché des produits commerciaux concurrents -->
## Analyse du marché des produits commerciaux concurrents


 
Les produits de tracking LoRa concurrents au LR1110 sont nombreux et ils se distinguent par leur caractéristique qu’ils apportent. La principale fonctionnalité est bien évidemment la transmission de données de géolocalisation mais certains possèdent des fonctions annexes tel que retransmettre les données envoyées par un capteur de mouvement (capteur à 9 axes) ou encore l’émission du niveau de batterie de l’appareil afin d’en informer l’utilisateur. Selon certains modèles, de remonter beaucoup plus de données tel que la température, l’humidité, la lumière , le choc, inclinaison, Mouvement et Activité.     	
Les différents tracker concurrents se différencient aussi bien par la clientèle touchée : Entreprise de transports logistiques permettant le suivi de grosses marchandises (conteneurs)  continuellement et éviter les vols et égarement, par les engins des entreprises minières , utilisé aussi pour le suivi de animaux (incluant notre projet) aussi bien dans une ferme ou animal compagnie , d’autres sont spécialisés dans le suivi de véhicules (véhicules personnels, ou suivi de flotte de véhicules de locations).
Nous pouvons citer les trackers :  G62 ou Oyster de chez Digital matter ou les tracker d’activité personnelle (suivi de personnes âgées ou de personnes malades parkinson, Alzheimer) de chez IOT Factory. Ces trackers sont de l’ordre unitaire d’environ 100€





<!-- Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud)) -->
## Définition de l’architecture globale du systèmes (ensemble d’objets, service en ligne (cloud))












