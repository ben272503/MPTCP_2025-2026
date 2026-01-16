#!/bin/bash

# --- Configuration basée sur vos fichiers ---
# IPs du client 
IP_CLIENTS=("10.0.1.10" "10.0.2.10" "10.0.3.10" "10.0.4.10")
# Passerelles (IPs du routeur côté client) [cite: 6, 10, 14, 18]
GW_ROUTER=("10.0.1.1" "10.0.2.1" "10.0.3.1" "10.0.4.1")
# Interfaces définies dans client_netplan.md 
INTERFACES=("eth0" "eth1" "eth2" "eth3")
# Réseau cible (Serveur)
TARGET_NET="192.168.0.0/16"

echo "Configuration du routage sur le CLIENT..."

# Nettoyage et limites MPTCP
sudo ip mptcp endpoint flush
sudo ip mptcp limits set subflow 4 add_addr_accepted 4

for i in {0..3}
do
    TABLE=$((i + 10))
    IF=${INTERFACES[$i]}
    IP=${IP_CLIENTS[$i]}
    GW=${GW_ROUTER[$i]}

    echo "Configuration de $IF ($IP) via GW $GW (Table $TABLE)..."

    # Création des tables de routage par interface
    sudo ip route add $TARGET_NET dev $IF scope link table $TABLE
    sudo ip route add default via $GW dev $IF table $TABLE
    
    # Règle de politique de routage
    sudo ip rule add from $IP table $TABLE

    # Ajout de l'interface comme endpoint MPTCP (subflow)
    sudo ip mptcp endpoint add $IP dev $IF subflow
done

echo "Configuration Client terminée." | lolcat
