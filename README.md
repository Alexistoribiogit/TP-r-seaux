# tp 1 réseau

1. Affichage d'informations sur la pile TCP/IP locale

    A. Affichez les infos des cartes réseau de votre PC

    .ipconfig / all (Permet d'afficher la configuration TCP/IP complète de toutes les cartes réseau):

Wifi:
    nom (Description) : MediaTek Wi-Fi 6 MT7921 Wireless LAN Card
    Adresse Mac (Adresse Physique) : 88-A3-84-45-B0-1A
    adresse IP de l'interface WiFi (Adresse IPv4): 10.33.18.92

Ethernet:
    nom (Descrption) : Killer E2600 Gigabit Ethernet Controller
    Adresse Mac (Adresse Physique) : E4-A8-DF-F8-E1-35
    adresse IP de l'interface Ethernet (Adresse IPv4): non
<br>
B. Affichez votre gateway

    .ipconfig / all (Permet d'afficher la configuration TCP/IP complète de toutes les cartes réseau):

    Passerelle par défaut : 10.33.19.254
<br>
C. En graphique (GUI : Graphical User Interface)

    Chemin: panneau de configuration ->réseaux et internet -> connexion réseaux->
![](https://i.imgur.com/oxV2tyf.png)

*Trouvez comment afficher les infiormations sur une carte IP (change selon l'OS)*

    interface Wifi:
    
    -adresse wifi: 10.33.19.254
    -adresse mac : d8:f3:bc:7c:46:1f
    -passerelle : 10.33.19.254
    
D. Question
à quoi sert la gate way dans le réseau d'YNOV.

**2- Modifications des informations**
A- ![](https://i.imgur.com/hvjGdUX.png)

Statistiques Ping pour 192.168.0.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 2ms, Maximum = 2ms, Moyenne = 2ms
    
```
mon pc va être en 192.168.0.2, le pc connecté a internet sera en 192.168.0.1 et la gateway sera en 192.168.0.3 avec un masque 255.255.255.252
```
Envoi d’une requête 'Ping'  192.168.0.1 avec 32 octets de données :
Réponse de 192.168.0.1 : octets=32 temps=344 ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps=1 ms TTL=128
```
je peut ping l'aure pc, testons avec google :
```
Envoi d’une requête 'Ping'  8.8.8.8 avec 32 octets de données :
Réponse de 8.8.8.8 : octets=32 temps=21 ms TTL=113
Réponse de 8.8.8.8 : octets=32 temps=23 ms TTL=113
Réponse de 8.8.8.8 : octets=32 temps=22 ms TTL=113
Réponse de 8.8.8.8 : octets=32 temps=21 ms TTL=113
```

on peut uttiliser un traceroute (tracert 8.8.8.8 (pour dns.google )).

```
Détermination de l’itinéraire vers dns.google [8.8.8.8]
avec un maximum de 30 sauts :

  1    <1 ms    <1 ms    <1 ms  MSI [192.168.0.1]
  2     *        *        *     Délai d’attente de la demande dépassé.
  3     7 ms     7 ms     6 ms  10.33.19.254
  4     5 ms     5 ms     4 ms  137.149.196.77.rev.sfr.net [77.196.149.137]
  5    10 ms     8 ms     9 ms  108.97.30.212.rev.sfr.net [212.30.97.108]
  6    23 ms    22 ms    23 ms  222.172.136.77.rev.sfr.net [77.136.172.222]
  7    24 ms    22 ms    22 ms  221.172.136.77.rev.sfr.net [77.136.172.221]
  8    25 ms    26 ms    25 ms  186.144.6.194.rev.sfr.net [194.6.144.186]
  9    22 ms    27 ms    22 ms  186.144.6.194.rev.sfr.net [194.6.144.186]
 10    24 ms    23 ms    22 ms  72.14.194.30
 11    22 ms    22 ms    22 ms  172.253.69.49
 12    27 ms    22 ms    38 ms  108.170.238.107
 13    23 ms    22 ms    22 ms  dns.google [8.8.8.8]
```

avec netcat on peut lancer un chat 
    - on ecrit "192.168.0.2 8888" dans la console de netcat sur le pc client.
    - on ecrit "-l -p 8888" dans la console de netcat sur le pc server.

```
  TCP    192.168.0.2:9999       192.168.0.1:1055       ESTABLISHED
```

pour pouvoir acceder a ce service via ethernet et wifi, pour pouvoir préciser l'IP d'écoute il faut lancer netcat avec cette commande : 

```
.\nc.exe -l -p 9999 -s 192.168.0.2
```

PS C:\WINDOWS\system32> netstat -a -n -b  | select-string 9999

le ping a déjà une règle nomé **"Analyse de l’ordinateur virtuel (Demande d’écho - Trafic entrant ICMPv4)"** dans le fire-wall qui permet le ping

il suffit après de créer une règle pour associet à netcat le port 9999 au service.
je l'ai appelé **"tcp netcat"**

```
** 1- dhcp
Exploration du DHCP, depuis votre PC

afficher l'adresse IP du serveur DHCP, Trouver la date d'expiration de votre bail DHCP***
```

  Bail obtenu. . . . . . . . . . . . . . : mercredi 28 septembre 2022 15:40:39
   Bail expirant. . . . . . . . . . . . . : jeudi 29 septembre 2022 13:52:46
 
   Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254*
   
```
2-DNS
trouver l'adresse IP du serveur DNS que connaît votre ordinateur
```

Address:  10.2.0.1

```
J'utilise, en ligne de commande l'outil nslookup (Windowspour faire des requêtes DNS à la main
    faites un lookup (lookup = "dis moi à quelle IP se trouve tel nom de domaine")

pour google.com
```
Nom :    google.com
Addresses:  2404:6800:4004:811::200e
          172.217.175.238
 ```         
pour ynov.com
```
Nom :    ynov.com
Addresses:  2606:4700:20::681a:be9
          2606:4700:20::ac43:4ae2
          2606:4700:20::681a:ae9
          104.26.11.233
          172.67.74.226
          104.26.10.233

```
faites un reverse lookup
pour l'adresse 78.74.21.21
```
Serveur :   dns.google
Address:  10.2.0.1

Nom :    host-78-74-21-21.homerun.telia.com
Address:  78.74.21.21
```
pour l'adresse 92.146.54.88
```
Serveur :   dns.google
Address:  10.2.0.1

*** dns.google ne parvient pas à trouver 92.146.54.88 : Non-existent domain
```
Ces  deux adresses appartiennent a google.
![](https://i.imgur.com/KbQN0Se.png)
nous pouvons voir sur le screen que la lan entre nos deux pc il y a des requetes tcp en bleu émant netcat et ichp en violet provenant d'un ping
