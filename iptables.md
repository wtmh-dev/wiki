---
title: iptables
description: 
published: true
date: 2019-07-08T15:16:29.303Z
tags: 
---

# Header

IMPLEMENTACIÓN DE SEGURIDAD
DE LAS REDES CON IPTABLES
Despues de acabar con la instalacion de todos los servicios necesarias y configuracion de las redes, configuramos iptables con siguientes reglas

###Permitimos al informatico con IP 192.168.11.2 el accceso a

#server web (10.5), con mysql, FTP y FTPS, http y https
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 3306 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p udp --dport 3306 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 20:21 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p udp --dport 20:21 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 990 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p udp --dport 990 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p udp --dport 80 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 443 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p udp --dport 443 -j ACCEPT

#a los sistemas Linux (10.4, 10.5) por Webmin y SSH (SSH solo usa tcp)
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 10000 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p udp --dport 10000 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.4 -p tcp --dport 10000 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.4 -p udp --dport 10000 -j ACCEPT

iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 22 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.4 -p tcp --dport 22 -j ACCEPT

iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.5 -p tcp --dport 2222 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.4 -p tcp --dport 2222 -j ACCEPT

#a los Servidores de Microsoft (10.2, 10.3) por Escritorio Remoto (Terminal Server)
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.2 -p tcp --dport 3389 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.2 -p udp --dport 3389 -j ACCEPT

iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.3 -p tcp --dport 3389 -j ACCEPT
iptables -A FORWARD -s 192.168.11.2 -i ens33 -d 192.168.10.3 -p udp --dport 3389 -j ACCEPT


###El resto de la red LAN (192.168.11.0/24) podra acceder a

##Servidor WEB (192.168.10.5) y DNS (192.168.10.2 )
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.5 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.5 -p udp --dport 80 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.5 -p tcp --dport 443 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.5 -p udp --dport 443 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 53 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 53 -j ACCEPT

##Servidor de Dominio (192.168.10.2)

#Puerto 88 para autentificación Kerberos con protocolo UDP
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 88 -j ACCEPT

#UDP y TCP Puerto 135 para las operaciones de controlador de dominio
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 135 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 135 -j ACCEPT

#Puerto TCP 139 y UDP 138 para el Servicio de Replicación de Archivos entre controladores de dominio
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 139 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 138 -j ACCEPT

#Puerto UDP 389 para que LDAP (Lightweight Directory Access Protocol)
#maneje las consultas normales de las computadoras cliente a los controladores de dominio
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 389 -j ACCEPT

#Puerto TCP y UDP 445 para el servicio de comparticion de archivos
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 445 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 445 -j ACCEPT

#TCP y UDP puerto 464 para cambio de contraseña de Kerberos
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 464 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p udp --dport 464 -j ACCEPT

#Los puertos TCP 3268 y 3269 para el catálogo global del cliente al controlador de dominio
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 3268 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.2 -p tcp --dport 3269 -j ACCEPT

##Servidor de correo (192.168.10.3)
#Acceso con el protocolo POP3 y POP3S (110 y 995 SSL/TLS)
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 110 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 110 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 995 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 995 -j ACCEPT

#Acceso con el protocolo IMAP y IMAPS (143 y 993 SSL/TLS)
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 143 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 143 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 993 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 993 -j ACCEPT

#Acceso con el protocolo SMTP y SMTPS (25, 587 y 465 SSL/TLS)
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 25 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 25 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 587 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 587 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p tcp --dport 465 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.3 -p udp --dport 465 -j ACCEPT

#Streaming (servidor multimedia 192.168.10.4) con puerto 32400
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.4 -p tcp --dport 32400 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.4 -p udp --dport 32400 -j ACCEPT

#Permitimos la conexión permanente entre  equipos y la WAN
iptables -A INPUT -m state  --state ESTABLISHED,RELATED -j ACCEPT

#Cerramos el trafico restante hacia DMZ desde la red LAN
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -d 192.168.10.0/29 -j DROP

#Filtramos el trafico desde la LAN a la WAN
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -p udp --dport 80 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -p tcp --dport 443 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -p udp --dport 443 -j ACCEPT

iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -p tcp --dport 53 -j ACCEPT
iptables -A FORWARD -s 192.168.11.0/24 -i ens33 -p udp --dport 53 -j ACCEPT

#Cerramos el resto del trafico desde la LAN a la WAN
iptables -A FORWARD -s 192.168.11.0/24 -i eth2 -o eth0 -j DROP


#Acceso desde la WAN hacia DMZ (http, https, ftp, POP3 y POP3S, IMAP e IMAPS, SMTP y SMTPS, DNS)
iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 80 -j DNAT --to 192.168.10.5:80
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 80 -j DNAT --to 192.168.10.5:80

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 443 -j DNAT --to 192.168.10.5:443
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 443 -j DNAT --to 192.168.10.5:443

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 20 -j DNAT --to 192.168.10.5:20
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 20 -j DNAT --to 192.168.10.5:20

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 21 -j DNAT --to 192.168.10.5:21
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 21 -j DNAT --to 192.168.10.5:21

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 110 -j DNAT --to 192.168.10.3:110
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 110 -j DNAT --to 192.168.10.3:110

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 995 -j DNAT --to 192.168.10.3:995
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 995 -j DNAT --to 192.168.10.3:995
iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 143 -j DNAT --to 192.168.10.3:143
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 143 -j DNAT --to 192.168.10.3:143

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 993 -j DNAT --to 192.168.10.3:993
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 993 -j DNAT --to 192.168.10.3:993

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 25 -j DNAT --to 192.168.10.3:25
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 25 -j DNAT --to 192.168.10.3:25

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 587 -j DNAT --to 192.168.10.3:587
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 587 -j DNAT --to 192.168.10.3:587

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 465 -j DNAT --to 192.168.10.3:465
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 465 -j DNAT --to 192.168.10.3:465

iptables -t nat -A PREROUTING -i ens34 -p tcp --dport 53 -j DNAT --to 192.168.10.2:53
iptables -t nat -A PREROUTING -i ens34 -p udp --dport 53 -j DNAT --to 192.168.10.2:53

##Cerramos el resto de puertos y accesos
iptables -A INPUT -p tcp --dport 1:1024 -j DROP
iptables -A INPUT -p udp --dport 1:1024 -j DROP

iptables -A INPUT -s 0.0.0.0/0 -p tcp --dport 3306 -j DROP
iptables -A INPUT -s 0.0.0.0/0 -p udp --dport 3306 -j DROP

iptables -A INPUT -s 0.0.0.0/0 -p tcp --dport 10000 -j DROP
iptables -A INPUT -s 0.0.0.0/0 -p udp --dport 10000 -j DROP

iptables -A FORWARD  -s 0.0.0.0/0 -p tcp --dport 1:1024 -j DROP
iptables -A FORWARD  -s 0.0.0.0/0 -p udp --dport 1:1024 -j DROP

iptables -A FORWARD -s 0.0.0.0/0 -p tcp --dport 3306 -j DROP
iptables -A FORWARD -s 0.0.0.0/0 -p udp --dport 3306 -j DROP

iptables -A FORWARD -s 0.0.0.0/0 -p tcp --dport 10000 -j DROP
iptables -A FORWARD -s 0.0.0.0/0 -p udp --dport 10000 -j DROP

echo “OK. Verifique el filtrado con: iptables -L -n”

#fin del script

Informacion adicional necesaria para la configuracion de cortafuegos con iptables
Conexion por escritorio remoto:
	Actualmente, Microsoft se refiere a su software de cliente RDP oficial como Conexión de escritorio remoto, anteriormente "Cliente de Servicios de Terminal Server". De forma predeterminada, el servidor escucha en el puerto 3389 (TCP y UDP).

La lista de servicios y sus puertos utilizados para la comunicación de Active Directory:
	La apertura de estos puertos en el Firewall entre los cliente y los controladores de dominio, o entre los controladores de dominio, permitirá que Active Directory funcione correctamente:

– Puerto UDP 88 para autentificación Kerberos
– Puerto 135 (UDP y TCP) para las operaciones de controlador de dominio a controlador de dominio y de cliente a controlador de dominio.
– Puerto TCP 139 y UDP 138 para el Servicio de Replicación de Archivos entre controladores de dominio.
– Puerto UDP 389 para que LDAP maneje las consultas normales de las computadoras cliente a los controladores de dominio.
– Puerto 445 (TCP y UDP) para el servicio de replicación de archivos
– Puerto 464 TCP y UDP para cambio de contraseña de Kerberos
– Los puertos TCP 3268 y 3269 para el catálogo global del cliente al controlador de dominio.
– Puerto 53 (TCP y UDP) para DNS desde cliente a controlador de dominio y controlador de dominio a controlador de dominio.

Los puertos del correo por defecto:
POP3:
Port 110 – non-encrypted port
Port 995 – SSL/TLS port, also known as POP3S
IMAP:
143 – non-encrypted port
993 – SSL/TLS port, also known as IMAPS
SMTP ports:
25 – non-encrypted port
465 – SSL/TLS port, also known as SMTPS

Los puertos adicionales utilizados por Microsoft
587 – SMTP
50636 – EdgeSync Service
