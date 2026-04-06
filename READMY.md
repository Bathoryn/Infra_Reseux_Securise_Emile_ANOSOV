# 🛡️ Rendu TP1 - Infras Réseau Sécurisées

**Étudiant :** ANOSOV EMILE 
**Promotion :** B3 CS  

---

## 📌 Présentation du TP1 : Base Topology
[cite_start]Ce dépôt contient mon rendu pour le premier TP de mise en place de la topologie de base[cite: 262, 263]. [cite_start]L'infrastructure est virtualisée avec GNS3 (routeurs et switches Cisco) et VirtualBox (VMs Rocky Linux)[cite: 271, 272, 273, 274]. [cite_start]Elle intègre un accès internet via NAT, une segmentation par VLANs et un service DHCP centralisé[cite: 276, 277, 278].

## 🗺️ Topologie Réseau
Voici la capture de l'infrastructure déployée sur GNS3 :

![Topologie du TP1](./chemin/vers/ton/image_topologie.png)
*(Pense à remplacer le chemin par le bon nom de ton fichier image)*

---

## 🚀 Récapitulatif des configurations

### 1. Accès Internet & NAT (Part 2)
[cite_start]Le routeur `r1` récupère une IP publique dynamiquement via l'interface connectée au nuage GNS3[cite: 420, 421, 422]. 
Pour permettre aux machines des LANs de joindre Internet, j'ai configuré :
* [cite_start]**NAT** : Mise en place du NAT sur `r1` avec les interfaces *inside* vers les LANs et *outside* vers Internet[cite: 439, 441, 442].
* [cite_start]**DNS** : Configuration du DNS public de Cloudflare (`1.1.1.1`) sur les clients VPCS pour la résolution de noms (ex: ping efrei.fr)[cite: 459, 461, 462].

### 2. Service DHCP (Part 3)
[cite_start]La distribution des adresses IP est gérée par la machine `dhcp.tp1.efrei` via le service `dnsmasq`[cite: 345, 347].
* [cite_start]**Réseaux gérés** : clients, guests et admins[cite: 350].
* [cite_start]**Plages IP** : Attribution d'adresses comprises entre `.10` et `.100` pour chaque réseau[cite: 353].
* [cite_start]**Options DHCP** : Le serveur fournit aussi l'IP de la passerelle et le DNS (`1.1.1.1`)[cite: 351, 352].
* [cite_start]**DHCP Relay** : Configuration du relais DHCP sur le routeur `r1` pour rediriger les requêtes des clients vers le serveur DHCP (`10.1.30.1`)[cite: 380, 381, 383].

---

## 📂 Fichiers présents dans le dépôt

[cite_start]Comme demandé dans la partie 4, voici la liste des fichiers de configuration et des captures Wireshark fournis dans ce `.zip`[cite: 315, 318, 327]:

### ⚙️ Configurations (Part 4)
* **Équipements réseau** : 
    * [cite_start]`r1-running-config.txt` [cite: 319]
    * [cite_start]`core1-running-config.txt` [cite: 324]
    * [cite_start]`access1-running-config.txt` à `access4-running-config.txt` [cite: 320, 321, 322, 323]
* **Système** : 
    * [cite_start]`dnsmasq.conf` (configuration du serveur DHCP) [cite: 328]

### 🔍 Captures Wireshark
* [cite_start]`p2_no_nat.pcap` : Preuve du ping vers 1.1.1.1 échouant avant le NAT (l'IP source est celle du client)[cite: 432, 435, 437].
* [cite_start]`p2_nat.pcap` : Preuve du ping réussi après configuration du NAT (l'IP source est translatée par le routeur)[cite: 451, 455, 456].
* [cite_start]`p2_routed_ping.pcap` : Requête DNS suivie du ping vers `efrei.fr` depuis un client[cite: 478, 479, 480].
* [cite_start]`p3_dhcp.pcap` : Échange DHCP complet (Discover, Offer, Request, Ack) montrant la récupération d'une IP par un client[cite: 396, 401].