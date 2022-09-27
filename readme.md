## Exercice 1:
---

Vous administrez le réseau interne 172.16.0.0/23 d’une entreprise, et devez gérer un parc de 254 machines
réparties en 7 sous-réseaux. La répartition des machines est la suivante :
- Sous-réseau 1 : 38 machines
- Sous-réseau 2 : 33 machines
- Sous-réseau 3 : 52 machines
- Sous-réseau 4 : 35 machines
- Sous-réseau 5 : 34 machines
- Sous-réseau 6 : 37 machines 
- Sous-réseau 7 : 25 machines
Donnez, pour chaque sous-réseau, l’adresse de sous-réseau, l’adresse de broadcast (multidiffusion) ainsi
que les adresses de la première et dernière machine configurées (précisez si vous utilisez du VLSM ou pas).

Je n'utiliserai pas le VLSM pour ces réseaux.

| Adresse de sous réseau | Adresse de Broadcast | Première machine | Dernière machine |
|------------------------|----------------------|------------------|------------------|
| 127.16.0.0             | 127.16.0.63          | 127.16.0.1       | 127.16.0.62      |
| 127.16.0.64            | 127.16.0.127         | 127.16.0.65      | 127.16.0.126     |
| 127.16.0.128           | 127.16.0.191         | 127.16.0.129     | 127.16.0.190     |
| 127.16.0.192           | 127.16.0.255         | 127.16.0.193     | 127.16.0.254     |
| 127.16.1.0             | 127.16.1.63          | 127.16.1.1       | 127.16.1.62      |
| 127.16.1.64            | 127.16.1.127         | 127.16.1.65      | 127.16.1.126     |
| 127.16.1.128           | 127.16.1.191         | 127.16.1.129     | 127.16.1.190     |
| 127.16.1.192           | 127.16.1.255         | 127.16.1.193     | 127.16.1.254     |


## Exercice 2:
---

2) L'adresse lo correspond à l'adresse de loopback, pour que la machine communique avec elle meme
3) On utilise la commande ```sudo apt remove cloud-init``` pour le supprimer
4) On modifie le fichier /etc/hosts en remplaçant le nom de l'adresse 127.0.0.1 par tpadmin.local et en utilisant la commande ```hostnamectl set-hostname tpadmin.local```

## Exercice 3:
---

1) On utilise la commande ```apt install isc-dhcp-server``` pour installer le paquet, et on voit bien qu'il ne fonctionne pas comme il n'est pas configuré
2) Pour changer l'adresse ip de manière permanente il faut changer la configuration du netplan, 
```network :
version : 2
renderer : networkd
ethernets :
ens224 :
addresses :
− 192.168.100.1/2 4
```
et modifier l'ip avec ```ip addr add 192.168.1.100/24 dev ens224```
3) On met la config dans le fichier dhcpd.conf
```
default-lease-time 120;
max-lease-time 600;
authoritative; #DHCP officiel pour notre réseau
option broadcast-address 192.168.100.255; #informe les clients de l'adresse de broadcast
option domain-name "tpadmin.local"; #tous les hôtes qui se connectent au
#réseau auront ce nom de domaine
subnet 192.168.100.0 netmask 255.255.255.0 { #configuration du sous-réseau 192.168.100.0
range 192.168.100.100 192.168.100.240; #pool d'adresses IP attribuables
option routers 192.168.100.1; #le serveur sert de passerelle par défaut
option domain-name-servers 192.168.100.1; #le serveur sert aussi de serveur DNS
}
```

4) Pour spécifier l'interface sur lequelle le serveur doit on va dans le fichier /etc/default/isc-dhcp-server et on met la ligne ```INTERFACEv4="ens224"```
5) On redemarre le serveur avec la commande ```systemctl restart isc-dhcp-server``` puis on vérifie qu'il fonctionne avec ```systemctl status isc-dhcp-server```
6) Pour désactiver la carte réseau on utilise ```ip link set ens192 down```, on modifie le nom dans le fichier /etc/hosts et avec la commande utiliser avant.
7) DHCPDiscover sert à détecter les serveurs DHCP disponible, DHCPOFFER permet de soummettre une adresse ip au client, DHCPREQUEST réponds au DHCPOFFER en en indiquant quel serveur à été choisi par le client, DHCPACK fait la procedure “IP address allocation/lease”.
8) Le fichier dhcpd.lease permet de stocker l'assignement d'ip a des machines de manière temporaire.
```dhcp-lease-list``` permet de lister les machines et leurs ip associer.
9) J'utilise ```ping 192.168.100.1``` depuis le client et les ping fonctionnent bien.
10) On ajoute dans le dossier /etc/dhcp/dhcp.conf 
```
deny unknown-clients;

host client {
  hardware ethernet 00:50:56:89:49:77
  fixed-address 192.168.100.20
}
```

## Exercice 4:
---

1) Pour pouvoir communiquer en dehors qu'en local, on décommente la ligne ```net.ipv4.ip_forward=1``` dans le fichier /etc/sysctl.conf puis on utilise la commande ```sudo sysctl -p /etc/sysctl.conf``` pour que les changements soit pris en compte.
2) J'utilise la commande ```sudo iptables --table nat --append POSTROUTING --out-interface enp0s3 -j MASQUERADE``` pour pouvoir traduire les adresses.
Le ping 1.1.1.1 fonctionne bien.

## Exercice 5:
---

1) On installe bind9 avec ```apt install bind9``` puis on regarde si il fonctionne avec ```systemctl status bind9```
2) On va dans le fichier /etc/bind/named.conf/options et on décommente forwarder en ajoutant les ip 1.1.1.1 et 8.8.8.8 dedans et on le redémarre avec la commande ```systemctl restart bind9```
3) La commande ```ping www.google.fr``` fonctionne.
4) On utilise la commande ```lynx www.wikipedia.org``` pour pouvoir aller sur le site en ligne de commande.

## Exercice 6:
---

1) J'ajoute les ligne 
```
zone "tpadmin.local" IN {
type master;
file "/etc/bind/db.tpadmin.local";
};
```
dans le fichier /etc/bind/named.conf.options comme demander.
2) On échange 2 fois localhost par tpadmin dans le fichier /etc/bind/db.admin.local
3) On ajoute dans le fichier named.conf.local 
```
zone "100.168.192.in-addr.arpa" {
type master;
file "/etc/bind/db.192.168.100";
};
```
4)



