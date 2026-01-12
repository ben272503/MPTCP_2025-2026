#!/bin/bash

# --- Configuration des variables ---
# IPs du client [cite: 4]
IP_CLIENTS=("10.0.1.10" "10.0.2.10" "10.0.3.10" "10.0.4.10")
# Passerelles (IPs du routeur côté client) [cite: 6, 10, 14, 18]
GW_ROUTER=("10.0.1.1" "10.0.2.1" "10.0.3.1" "10.0.4.1")
# Interfaces (à vérifier avec 'ip link')
INTERFACES=("enp1s0" "enp8s0" "enp9s0" "enp10s0")
# Réseau cible (Serveur)
TARGET_NET="192.168.0.0/16"

echo "Configuration du routage sur le CLIENT..."

# 1. Nettoyage de la configuration MPTCP existante
sudo ip mptcp endpoint flush
sudo ip mptcp limits set subflow 4 add_addr_accepted 4

for i in {0..3}
do
    TABLE=$((i + 10))
    IF=${INTERFACES[$i]}
    IP=${IP_CLIENTS[$i]}
    GW=${GW_ROUTER[$i]}

    echo "Configuration de l'interface $IF ($IP) via table $TABLE..."

    # Création d'une table de routage spécifique pour chaque interface
    sudo ip route add $TARGET_NET dev $IF scope link table $TABLE
    sudo ip route add default via $GW dev $IF table $TABLE
    
    # Règle de politique : tout ce qui sort avec l'IP source X utilise la table X
    sudo ip rule add from $IP table $TABLE

    # Ajout de l'endpoint MPTCP pour que le noyau utilise cette interface
    # On met 'subflow' pour que le client initie les connexions secondaires
    sudo ip mptcp endpoint add $IP dev $IF subflow
done

echo "Routage Client terminé."
