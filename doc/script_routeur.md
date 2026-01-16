#!/bin/bash

echo "Configuration du routage sur le ROUTEUR..."

# Réseaux côté client
CLIENT_NETS=("10.0.1.0/24" "10.0.2.0/24" "10.0.3.0/24" "10.0.4.0/24")
CLIENT_IFACES=("enp1s0" "enp8s0" "enp9s0" "enp10s0")

# Réseaux côté serveur
SERVER_NETS=("192.168.1.0/24" "192.168.2.0/24" "192.168.3.0/24" "192.168.4.0/24")
SERVER_IFACES=("enp11s0" "enp12s0" "enp13s0" "enp14s0")

echo "Ajout des routes CLIENT -> SERVEUR..."
for i in {0..3}
do
    sudo ip route add ${SERVER_NETS[$i]} dev ${CLIENT_IFACES[$i]} 2>/dev/null
done

echo "Ajout des routes SERVEUR -> CLIENT..."
for i in {0..3}
do
    sudo ip route add ${CLIENT_NETS[$i]} dev ${SERVER_IFACES[$i]} 2>/dev/null
done

echo "Configuration Routeur terminée."
