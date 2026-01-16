#!/bin/bash

echo "Configuration du routage sur le CLIENT..."

# Nettoyage des règles existantes
sudo ip rule flush
sudo ip rule add from all lookup local pref 0
sudo ip rule add from all lookup main pref 32766
sudo ip rule add from all lookup default pref 32767

# Interfaces du client
INTERFACES=("enp7s0" "enp8s0" "enp9s0" "enp10s0")

# Gateways côté client (routeur)
GW_ROUTER=("10.0.1.1" "10.0.2.1" "10.0.3.1" "10.0.4.1")

# Réseaux du serveur
SERVER_NETS=("192.168.1.0/24" "192.168.2.0/24" "192.168.3.0/24" "192.168.4.0/24")

for i in {0..3}
do
    IF=${INTERFACES[$i]}
    GW=${GW_ROUTER[$i]}
    NET=${SERVER_NETS[$i]}

    echo "Ajout route $NET via $GW dev $IF ..."
    sudo ip route add $NET via $GW dev $IF 2>/dev/null
done

echo "Configuration Client terminée."
