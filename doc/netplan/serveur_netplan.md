network:
  version: 2
  renderer: networkd
  ethernets:
    # Interface d'administration
    enp7s0:
      dhcp4: true
      dhcp4-overrides:
        use-routes: false
    
    # ----------------------------------------------------
    # CHEMINS MPTCP (Côté serveur: 192.168.X.10)
    # ----------------------------------------------------

    # Interfaces MPTCP
    eth0:
      [cite_start]addresses: [192.168.1.10/24] # [cite: 34, 35]
    eth1:
      [cite_start]addresses: [192.168.2.10/24] # [cite: 37, 38]
    eth2:
      [cite_start]addresses: [192.168.3.10/24] # [cite: 40, 41]
    eth3:
      [cite_start]addresses: [192.168.4.10/24] # [cite: 43, 44]
