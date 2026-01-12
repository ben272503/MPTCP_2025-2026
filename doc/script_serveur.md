#!/bin/bash

# --- Configuration des variables ---
# IPs du serveur [cite: 34, 37, 40, 43]
IP_SERVERS=("192.168.1.10" "192.168.2.10" "192.168.3.10" "192.168.4.10")
# Passerelles (IPs du routeur côté serveur) [cite: 22, 25, 28, 31]
GW_ROUTER=("192.168.1.1" "192.168.2.1" "192.168.3.1" "192.168.4.1")
# Interfaces (à vérifier avec 'ip link')
INTERFACES=("enp11s0" "enp12s0" "enp13s0" "enp14s0")
# Réseau cible (Client)
TARGET_NET="10.0.0.0/8"

echo "Configuration du routage sur le SERVEUR..."

# 1. Nettoyage et limites MPTCP
sudo ip mptcp endpoint flush
sudo ip mptcp limits set subflow 4 add_addr_accepted 4

for i in {0..3}
do
    TABLE=$((i + 20))
    IF=${INTERFACES[$i]}
    IP=${IP_SERVERS[$i]}
    GW=${GW_ROUTER[$i]}

    echo "Configuration de l'interface $IF ($IP) via table $TABLE..."

    # Configuration des tables de routage
    sudo ip route add $TARGET_NET dev $IF scope link table $TABLE
    sudo ip route add default via $GW dev $IF table $TABLE
    
    # Règle de politique
    sudo ip rule add from $IP table $TABLE

    # Pour le serveur, on utilise 'signal' pour annoncer ses IPs au client
    sudo ip mptcp endpoint add $IP dev $IF signal
done

echo "Routage Serveur terminé."
