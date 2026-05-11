# Proxy Zabbix
Appunti per la creazione di un Proxy Zabbiz su hardware dedicato.
## Caratteristiche
- Versione 7.0 (LTS) per avere massimo potenziale e modalità attiva.
- Indispensabile per SNMP (monitoraggio stampanti ed altro)
- Utile per clienti con più sistemi da monitorare 
- Può essere installato su Firewall pfSense
- Può essere installato su RaspberryPi
- Può essere installato su vecchi PC ri recupero (requisito minimo 64 bit)
- NON può essere installato su sistemi Windows

## Installazione sistema operativo
Scelta distribuzione Debian 13 (Trixie)
Creare chiave USB mediante Rufus.

Scegliere installazione minimale con supporto SSH e senza interfaccia grafica.

## Configurazione sistema operativo
Impostare IP fisso
ip addr per ricavare nome interfaccia (es: eth0)
modificare `/etc/network/interfaces`
esempio:
```bash 
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```    
aggiungere dns in `/etc/resolv.conf`
```bash
nameserver=8.8.8.8
nameserver=1.1.1.1
``` 
riavviare.

## Installazione Zabbix Proxy con db Sqlite
```bash
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian13_all.deb
dpkg -i zabbix-release_latest_7.0+debian13_all.deb
apt update 
apt install zabbix-proxy-sqlite3
```

## Configurazione del Proxy
Configurare i parametri nel file `/etc/zabbix/zabbix_proxy.conf`

server: 79.10.137.138
server port: 22251
hostname: PCP-PRX-001
proxy mode: Active
tls psk identity: PSK-PCP-PRX-001
tls psk:
abilitare SNMP trap
controllare path database SQLite

Configurare avvio automatico del proxy:
```bash
systemctl restart zabbix-proxy
systemctl enable zabbix-proxy 
```



