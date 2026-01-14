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

### 📘 Marche suivie si configuration à la main :
*Ces commandes seront ce qu'on va retrouver dans les fichiers yaml de la configuration netplan*

```console
# exemple de commande d'ajout de route
sudo ip route add 192.168.1.0/24 via 10.0.1.1 dev eth0
```

### 📗 Configuration des interfaces par Netplan (script) - Création des fichiers:

```console
sudo nano /etc/netplan/99-mptcp-router.yaml
```
> 💡 pour la propreté, il est préférable d'adapter le nom du fichier. exemple: **99-mptcp-client.yaml** pour le client

Puis on colle le contenu du fichier ci-dessous correspondant à notre machine

* **Routeur :** [`99-mptcp-router.yaml`](./netplan/routeur_netplan.yaml) — *Gère le transfert de paquets entre les deux sous-réseaux.*
* **Client :** [`99-mptcp-client.yaml`](./netplan/client_netplan.yaml) — *10.0.X.X/24*
* **Serveur :** [`99-mptcp-server.yaml`](./netplan/serveur_netplan.yaml) — *192.168.X.X/24*

Après avoir créé le fichier sur la machine correspondante, on peut vérifier les changement qui seront appliqués (optionnel mais recommandé):

```console
# ne dois rien renvoyer si pas d'erreur/warning
sudo netplan generate
```
> 💡Adapter la configuration en fonction de ces warning. voir sur internet

Puis appliquer les changements pour de bon:

```console
sudo netplan apply
```
* On peut s'assurer du résultat avec `sudo netplan try`

Pour finir, la vérification de l'application de la configuration:

```console
ip a
```
*sortie : vous devriez voir les différentes adresses comme l'exemple ci-dessous:*
*Nous pouvons y voir deux adresse **default** qui viennent d'une connexion par pont avec mon ordinateur*

![Exemple de routage fait](../images/exemple_ipr.png)

### Spécifique Routeur:
Pour que le routeur fasse sont travail de **'pont'** entre les deux machines, il est important d'activer le **transfer de paquet**:

```console
# Activation du pont de routage
sudo sysctl -w net.ipv4.ip_forward=1
```

## 2- Application de script de routage sur client et serveur

On applique des scripts pour faire la configuration des routes statiques spécifiant quel routes sont utilisables.

* **Client :** [`script client`](./script_client.md)
* **Serveur :** [`script serveur`](./script_serveur.md)

## *Vérification:*

**Ping de base**

Sur le client, faire `ping 192.168.1.10` 

**Vérification MPTCP**

utiliser `ip mptcp endpoint show` pour voir si les interfaces sont bien enregistrées

**Tester les multi-chemins exemple (optionnel, puisque prochaines étapes du projet):**

 1- Installer `mptcpize` ou utiliser `iperf3` pour .
 2- Lancer un transfert et vérifiez avec nload ou iftop sur le routeur si les 4 interfaces reçoivent du trafic simultanément.

[⮌ Retour au Readme général](../README.md)




