# Configuration du Routage Inter-Machines (MPTCP)
Ce document détaille la configuration réseau permettant la communication multi-chemins entre le Client et le Serveur via le Routeur.

## 1. Architecture du Réseau
Une table de routage classique est établie avec plusieures routes statiques nous permettant d'être sur de quel chemins prennent les données et ainsi faire des opérations sur le réseau.

[📄 Ouvrir La topologie complete en PDF](Description_de_la_Topologie_Réseau_MPTCP.pdf)



Il faut ajouter des interfaces a chaques cartes pour qu'elles puisse les utiliser ensuite.

![Topologie du réseau MPTCP](../images/MPTCP_Topology.png)

### Etape manuelle: 
Il est possible de configurer toutes les interfaces à la main, lors du routage sur nos machines virtuelles personnelles que nous avons fait sur nos ordinateurs respectifs, nous avons optés pour une configuration netplan, une application de toute la configuration réseau via un fichier.
Cela permet d'accélérer la phase de routage.

> ❗ Le routage suivant ne permet pas aux machines d'utiliser internet. Elles doivent en être coupées pour être dans un réseau fermé lors des tests MPTCP.

### Configuration des interfaces (Netplan) - Création des fichiers:

```
sudo nano /etc/netplan/99-mptcp-router.yaml
```
> 💡 pour la propreté, il est préférable d'adapter le nom du fichier. exemple: **99-mptcp-client.yaml** pour le client

Puis on colle le contenu du fichier ci-dessous correspondant à notre machine

* **Routeur :** [`99-mptcp-router.yaml`](./netplan/routeur_netplan.md) — *Gère le transfert de paquets entre les deux sous-réseaux.*
* **Client :** [`99-mptcp-client.yaml`](./netplan/client_netplan.md) — *10.0.X.X/24*
* **Serveur :** [`99-mptcp-server.yaml`](./netplan/serveur_netplan.md) — *192.168.X.X/24*

Après avoir créé le fichier sur la machine correspondante, on applique les changements:

```
sudo netplan apply
```

Pour finir, la vérification de l'application de la configuration:

```
ip a
```
*sortie : vous devriez voir les différentes adresses comme l'exemple ci-dessous:*
*Nous pouvons y voir deux adresse **default** qui viennent d'une connexion par pont avec mon ordinateur*

![Exemple de routage fait](../images/exemple_ipr.png)

[⮌ Retour au Readme général](../README.md)




