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
