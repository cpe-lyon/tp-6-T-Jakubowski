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
v e r si o n : 2
rende re r : networkd
e th e rn e ts :
ens224 :
addresses :
− 1 9 2 . 1 6 8 . 1 0 0 . 1 / 2 4
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
5) 
