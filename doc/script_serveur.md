#!/bin/bash

# --- Configuration du SERVEUR ---
# IPs du serveur
IP_SERVERS=("192.168.1.10" "192.168.2.10" "192.168.3.10" "192.168.4.10")

# Passerelles (routeur côté serveur)
GW_ROUTER=("192.168.1.1" "192.168.2.1" "192.168.3.1" "192.168.4.1")

# Interfaces du serveur
INTERFACES=("enp7s0" "enp8s0" "enp9s0" "enp10s0")

# Réseau du client
TARGET_NET="10.0.0.0/8"

echo "Configuration du routage sur le SERVEUR..."

# Configuration des tables et règles
for i in {0..3}
do
    TABLE=$((i + 20))
    IF=${INTERFACES[$i]}
    IP=${IP_SERVERS[$i]}
    GW=${GW_ROUTER[$i]}

    echo "Configuration de $IF ($IP) via GW $GW (Table $TABLE)..."

    # Route vers le réseau client via la passerelle du routeur
    sudo ip route add $TARGET_NET via $GW dev $IF table $TABLE

    # Route par défaut via la même passerelle
    sudo ip route add default via $GW dev $IF table $TABLE

    # Règle de routage basée sur l'IP source
    sudo ip rule add from $IP table $TABLE
done

echo "Configuration Serveur terminée."
