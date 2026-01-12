# Configuration du Routage Inter-Machines (MPTCP)
Ce document détaille la configuration réseau permettant la communication multi-chemins entre le Client et le Serveur via le Routeur.

## 1. Architecture du Réseau
Une table de routage classique est établie avec plusieures routes statiques nous permettant d'être sur de quel chemins prennent les données et ainsi faire des opérations sur le réseau.

[📄 Ouvrir La topologie complete en PDF](Description_de_la_Topologie_Réseau_MPTCP.pdf)



Il faut ajouter des interfaces a chaques cartes pour qu'elles puisse les utiliser ensuite.

![Topologie du réseau MPTCP](../images/MPTCP_Topology.png)

### Etape manuelle: 
Il est possible de configurer toutes les interfaces à la main, lors du routage sur nos machines virtuelles personnelles que nous avons fait sur nos ordinateurs respectifs, nous avons optés pour un script de routage.
Cela permet d'accélérer la phase de routage.

**Creer les fichier netplan :**

`sudo nano /etc/netplan/99-mptcp-router.yaml`

Puis on colle le contenu du fichier ci-dessous correspondant à notre machine

1- *Routeur*:



[Retour au Readme](../Readme.md)
