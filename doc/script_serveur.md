#!/bin/bash

IP_SERVERS=("192.168.1.10" "192.168.2.10" "192.168.3.10" "192.168.4.10")
GW_ROUTER=("192.168.1.1" "192.168.2.1" "192.168.3.1" "192.168.4.1")
INTERFACES=("enp7s0" "enp8s0" "enp9s0" "enp10s0")
TARGET_NET="10.0.0.0/8"

echo "Configuration du routage sur le SERVEUR..."

sudo ip rule flush
sudo ip rule add from all lookup local pref 0
sudo ip rule add from all lookup main pref 32766
sudo ip rule add from all lookup default pref 32767

for T in 20 21 22 23
do
    sudo ip route flush table $T 2>/dev/null
done

for i in {0..3}
do
    TABLE=$((i + 20))
    IF=${INTERFACES[$i]}
    IP=${IP_SERVERS[$i]}
    GW=${GW_ROUTER[$i]}

    echo "Configuration de $IF ($IP) via GW $GW (Table $TABLE)..."

    sudo ip route add $TARGET_NET via $GW dev $IF table $TABLE
    sudo ip route add default via $GW dev $IF table $TABLE

    sudo ip rule add from $IP table $TABLE pref $((1000 + i))
done

echo "Configuration Serveur terminée."
