# Operació Nord Tec

**Alumne:** Zain Ali  
**Grup classe:** 1r ASIX  

## Índex
 [Estat del projecte](#-estat-del-projecte)
 [Arquitectura de xarxa](#-arquitectura-de-xarxa)
 [Configuracions](#-configuracions)
 [Incidències i solucions](#-incidències-i-solucions)
 [Decisions tècniques](#-decisions-tècniques)
 [Reflexions tècniques](#-reflexions-tècniques)

## Estat del projecte

### Done (Fet)
 [x] Creació i configuració inicial del repositori a GitHub.
 [x] Configuració de les targetes de xarxa estàtiques en Linux.
 [x] Instal·lació del servei de SSH per a connexions remotes.

### Todo (Pendent)
 Implementació de regles complexes de Firewall (UFW).
 Resolució de problemes d'enrutament de la base de dades.

## Arquitectura de xarxa

### Esquema de la xarxa
![Diagrama de Xarxa](imatges/network_topology.png) 

### Informació rellevant i Adreçament IP

| Dispositiu / Servidor | Interfície | Adreça IP | Màscara | Passarel·la (GW) |
| :--- | :--- | :--- | :--- | :--- |
| **Servidor Principal** | eth0 (LAN) | `192.168.10.5` | `255.255.255.0` | `192.168.10.1` |
| **Client Ubuntu** | eth0 | `192.168.10.20` | `255.255.255.0` | `192.168.10.1` |
| **Router Principal** | eth1 (WAN) | `DHCP Publica`| - | - |

## Configuracions

En aquest apartat s'expliquen els principals fitxers de configuració utilitzats en el desplegament:

 **`/etc/netplan/01-netcfg.yaml`**: Fitxer utilitzat per a la configuració estàtica de les targetes de xarxa en entorns Ubuntu/Debian.
 **`/etc/ssh/sshd_config`**: Fitxer utilitzat per a securitzar el servei SSH cambiant el port per defecte.

## Incidències i solucions

### Error de connexió en el servei SSH
 **Missatge d'error exacte:** 
  ```text
  ssh: connect to host 192.168.10.5 port 22: Connection refused
  ```
 **Quan:** Fase de post-configuració del servidor, intentant accedir de forma remota des del client Ubuntu.
 **Causa:** El servei `sshd` no estava actiu i el port 22 estava blocat pel tallafocs actiu del sistema (`ufw`).
 **Solució:** S'ha arrencat el servei amb `sudo systemctl enable --now ssh` i s'ha configurat la regla de tallafocs amb `sudo ufw allow ssh`.
 **Detectada per:** @zainali


## Decisions tècniques

 **Tecnologia triada:** Ús de Netplan per a adreçament IP estàtic.
   **Per què:** Perquè els servidors d'infraestructura requereixen tenir una IP fixa i coneguda per evitar fallades de connectivitat.
   **Alternatives:** S'ha valorat fer un servidor DHCP dinàmic, però s'ha descartat per seguretat.


## Reflexions tècniques

Durant aquesta pràctica he après la importància de documentar de forma estructurada cada configuració del sistema. El disseny correcte de la xarxa evita futures incidències i ens ajuda a tenir un millor control en entorns professionals d'administració de sistemes.

