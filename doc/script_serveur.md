#!/bin/bash

# --- Configuration basée sur vos fichiers ---
# IPs du serveur 
IP_SERVERS=("192.168.1.10" "192.168.2.10" "192.168.3.10" "192.168.4.10")
# Passerelles (IPs du routeur côté serveur)
GW_ROUTER=("192.168.1.1" "192.168.2.1" "192.168.3.1" "192.168.4.1")
# Interfaces définies dans serveur_netplan.md 
INTERFACES=("enp7s0" "enp8s0" "enp9s0" "enp10s0")
# Réseau cible (Client)
TARGET_NET="10.0.0.0/8"

echo "Configuration du routage sur le SERVEUR..."

# Nettoyage et limites MPTCP
sudo ip mptcp endpoint flush
sudo ip mptcp limits set subflow 4 add_addr_accepted 4

for i in {0..3}
do
    TABLE=$((i + 20))
    IF=${INTERFACES[$i]}
    IP=${IP_SERVERS[$i]}
    GW=${GW_ROUTER[$i]}

    echo "Configuration de $IF ($IP) via GW $GW (Table $TABLE)..."

    # Configuration des tables de routage
    # Route vers le réseau client via la passerelle du routeur
    sudo ip route add $TARGET_NET via $GW dev $IF table $TABLE
    # Route par défaut via la même passerelle
    sudo ip route add default via $GW dev $IF table $TABLE
    
    # Règle de politique de routage
    sudo ip rule add from $IP table $TABLE

    # Annonce des adresses via MPTCP (signal)
    sudo ip mptcp endpoint add $IP dev $IF signal
done

echo "Configuration Serveur terminée."
