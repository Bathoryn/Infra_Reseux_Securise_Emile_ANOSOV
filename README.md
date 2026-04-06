# 🛡️ Rendu TP1 - Infras Réseau Sécurisées

**Étudiant :** ANOSOV EMILE 
**Promotion :** B3 CS  



## 📌 Présentation du TP1 : Base Topology
Ce dépôt contient mon rendu pour le premier TP de mise en place de la topologie de base. L'infrastructure est virtualisée avec GNS3 (routeurs et switches Cisco) et VirtualBox (VMs Rocky Linux). Elle intègre un accès internet via NAT, une segmentation par VLANs et un service DHCP centralisé.

## 🗺️ Topologie Réseau
Voici la capture de l'infrastructure déployée sur GNS3 :

![Topologie du TP1](.https://raw.githubusercontent.com/Bathoryn/Infra_Reseux_Securise_Emile_ANOSOV/refs/heads/main/tp1.png?token=GHSAT0AAAAAADZEHUKFWRKEIPB3EGVCKS7C2OTWQIQ)

## 🚀 Récapitulatif des configurations

### 1. Accès Internet & NAT (Part 2)
Le routeur `r1` récupère une IP publique dynamiquement via l'interface connectée au nuage GNS3. 
Pour permettre aux machines des LANs de joindre Internet, j'ai configuré :
* **NAT** : Mise en place du NAT sur `r1` avec les interfaces *inside* vers les LANs et *outside* vers Internet.
* **DNS** : Configuration du DNS public de Cloudflare (`1.1.1.1`) sur les clients VPCS pour la résolution de noms (ex: ping efrei.fr).

### 2. Service DHCP (Part 3)
La distribution des adresses IP est gérée par la machine `dhcp.tp1.efrei` via le service `dnsmasq`.
* **Réseaux gérés** : clients, guests et admins.
* **Plages IP** : Attribution d'adresses comprises entre `.10` et `.100` pour chaque réseau.
* **Options DHCP** : Le serveur fournit aussi l'IP de la passerelle et le DNS (`1.1.1.1`).
* **DHCP Relay** : Configuration du relais DHCP sur le routeur `r1` pour rediriger les requêtes des clients vers le serveur DHCP (`10.1.30.1`).

---

## 📂 Fichiers présents dans le dépôt

Comme demandé dans la partie 4, voici la liste des fichiers de configuration et des captures Wireshark fournis dans ce `.zip` :

### ⚙️ Configurations (Part 4)
* **Équipements réseau** : 
    * `r1-running-config.txt`
    * `core1-running-config.txt`
    * `access1-running-config.txt` à `access4-running-config.txt`
* **Système** : 
    * `dnsmasq.conf` (configuration du serveur DHCP)

### 🔍 Captures Wireshark
* `p2_no_nat.pcap` : Preuve du ping vers 1.1.1.1 échouant avant le NAT (l'IP source est celle du client).
* `p2_nat.pcap` : Preuve du ping réussi après configuration du NAT (l'IP source est translatée par le routeur).
* `p2_routed_ping.pcap` : Requête DNS suivie du ping vers `efrei.fr` depuis un client.
* `p3_dhcp.pcap` : Échange DHCP complet (Discover, Offer, Request, Ack) montrant la récupération d'une IP par un client.
