# Progetto Firewall pfSense

## Introduzione
Scopo di questo progetto è creare un processo ottimizzato per la preparazione, configurazione ed installazione di Firewall presso i clienti.
Il progetto si compone di fasi ben definite che lo rendono flessibile e robusto.

Le prime tre fasi sono a carico di LM-InnoTech e portano ad avere una installazione generica ma standardizzata. Si effettua con una chiavetta USB che contiene anche il file di configurazione base per tutti i FireWall che verranno gestiti e monitorati da LM-InnoTech.
Fase preparazione
Fase di prima installazione
Fase di prima configurazione

La fase successiva è a carico di LM-InnoTech e comporta la personalizzazione del FireWall per quanto riguarda la sua univocità nell’ecosistema di monitoraggio e gestione. 
Fase di personalizzazione identità del firewall e del gestore

Le fasi successive sono a carico del Gestore e comportano la personalizzazione dell’installazione per il singolo cliente e l’attivazione e configurazione di servizi accessori richiesti.
Fase di configurazione Rete.
Apertura porte - Opzionale.
Configurazione VPN - Opzionale.


Scelta dell’hardware
	Requisiti
Scelta del software
	Requisiti
Preparazione dell’hardware
	Montaggio
Controllo batteria
	Configurazione Bios
Sempre acceso
Resto default
Preparazione del software
	Creazione chiavetta USB
	Impostazione configurazione standard

Installazione Prototipo per clonazione
	Inserire chiavetta “cruzer”
	Accendere e F8 per scelta boot
	Scegliere “Cruzer”
	Accettare licenza
	Install senza cambiare nulla
	Configurare rete - vale solo per installazione
WAN usare Re0 Realtek perchè piu’ scarsa
 accettare tutto (DHCP)
LAN usare Em0 Intel perchè più affidabile
Scegliere STATIC e Impostare un IP che non dia conflitto con il router
192.168.10.1   255.255.255.0  
attivare DHCP da 101 a 200
confermare impostazioni di rete
	Scegliere INSTALL CE (Community Edition)
	Filesystem accettare
	Stripe accettare
	Scegliere disco su cui installare (attenzione a non scegliere la chiavetta)
	Confermare cancellazione disco
	Scegliere versione 2.8.1 [NON SCEGLIERE ALTRO PER COMPATIBILITA’]
	Attendere 6/7 minuti la fine dell’installazione
	Confermare con OK.
	Al reboot TOGLIERE LA CHIAVETTA.
	
## Configurazione Prototipo per clonazione
- Wizard
	- Welcome: next
	- Netgate… : Next
	- General: lasciare tutto com’è tranne DNS e mettere 8.8.8.8 (?!?!?!?!?!?!?!?!) 
		- Timeserver: Impostare timezone Europe/Roma
	- Wan: Next
	- Lan: Next
	- Admin password: Next
	- Reload
	- Finish
	- Il sistema si riavvia
- Accettare Licenza
- Installare pacchetti
	- ShellCommand
	- Sudo
	- OpenVPN Client Export
	- Tailscale
	- Zabbix Agent 7
	- Zabbix Proxy 7
- Configurare ShellCommand
	- Aggiungere comando per Tastiera Italiana
		- Command: kbdcontrol -l it.kbd 
		- Description: Impostazione tastiera italiana per console
	- Aggiungere comando per rendere eseguibile smartctl da utenti non-root
		- Command: chmod +s /usr/local/sbin/smartctl
		- Description: Abilita esecuzione di smartctl agli utenti senza privilegi sufficienti
- Configurare Sudo
	- Group: admins: spuntare No Password
	- il resto default
- Creare utenti Admin (MA NON I CERTIFICATI !!!)
	- Username: pcpoint
	- Password: F!rep@55
	- Full Name: PC-Point Admin
	- Group membership: admin
	- Effective privileges: 
	- Assigned privileges:  User - System - Shell Account Access
- Installare Certificato (Certificate Authority) per Headscale
	- Descriptive name: Headscale-Server-CA
	- Method: Import an existing Certificate Authority
	- Trust store: spuntato
	- Randomize Serial: spuntato
	- Certificate data: [copiare il contenuto completo del certificato per Headscale]
Next certificate serial: 1
- Configurazione Tailscale
	- Authentication
	- Login server: https://HS-M3-01.ddnsfree.com:8443
	- Pre-authentication Key: hskey-auth-3mSXycJISZ9K-ubWBNcV9KPGIMwpIENB_bxncoCJ2ZGJOPI_n_9OCBz8g5F4YmYY_mRUQjseATYgY
	- Settings
		- Enable: spuntato !!!!!ATTENZIONE!!!!!!!
		- il resto default
- Configurazione Zabbix Agent
	- Server: 79.10.137.138
	- Server Active: 79.10.137.138:22251
	- Hostname: pfsense
	- TLS Connect:: psk
	- TLS Accept: psk
	- TLS PSK Identity: PSK-identity
	- TLS PSK: 000000000000000000000000
	- Advanced options: [VEDI QUI IN FONDO]
	- il resto default
- Configurazione Zabbix Proxy
	- Server: 79.10.137.138
	- Server Port: 22251
	- Hostname: pfsense
	- Proxy mode: Active
	- TLS Connect:: psk
	- TLS Accept: psk
	- TLS PSK Identity: PSK-identity
	- TLS PSK: 000000000000000000000000
	- Start SNMP Trapper: Enabled
	- il resto default
- Configurazioni Generali
	- System - Advanced - Admin Access
		- Serial Terminal: Spuntare
		- Console menu: Password protect the console menu
	- System - Advanced - Networking
		- Server Backend: Kea DHCP
	- System - Advanced - Miscellaneous
		- Cryptographic hardware: AES-NI CPU-based Acceleration
		- Thermal Sensor: Intel Core* CPU on-die thermal sensor
- Configurare Dashboard
	| Sinistra | Destra |
	|---|---|
	| System Information | Interfaces |
	| | Gateways |
	| | Dynamic DNS Status |
	| | IPsec |
	| | OpenVPN |
	| | Thermal Sensors |
	| | S.M.A.R.T. Status |
	| | Disks |

## Installazione FireWall di produzione
usare chiavetta USB con installer pfsense + file config.xml

## Configurazione Firewall di produzione
- Generare certificato per  Authority (CA)
	- Nome: CA PCP-FW-005
	- Country: IT
	- Province: Varese
	- City: Fagnano Olona
	- Organization: LM-InnoTech s.r.l.
	- Organization Unit: PC-Point 
	- il resto default 
- Configurazione DNS Dinamico
	- Service Type: Custom
	- Update URL: https://api.dynu.com/nic/update?hostame=PCP-FW-001.ddnsfree.com&password=Dynup@55
	- Result Match: good %IP%
	- il resto default
- Configurazione OpenVPN
	- Type of server: Local User Account
	- Certificate Authority: CA FW-PCP-001
	- Server Certificate: Add New
		- Name: Cert OpenVPN
		- Common Name: OpenVPN FW-PCP-001
		- il resto default
	- Configurazione del server
		- Description: OpenVPN Server
		- IPv4 Tunnel Network: 192.168.250.0/24
		- il resto default
		- salvare
	- Rientrare in modifica
		- Redirect IPv4 Gateway: Spuntato
		- Inter-Client Communication: Spuntato
		- NetBIOS enable: Spuntato
		- Gateway creation: Only IPv4
		- il resto default
		- Salvare
		- Regole FireWall
			- entrambe spuntate
		- Finish

- Configurazione Dashboard
	- Impostare Dashboard in modo analogo al prototipo
	- Thermal Sensors
	- S.M.A.R.T. Status
	- Disks


## Installazione FireWall di produzione
	usare chiavetta USB con installer pfsense + file config.xml
Configurazione Firewall di produzione
	Generare certificato per  Authority (CA)
Nome: CA PCP-FW-005
Country: IT
Province: Varese
City: Fagnano Olona
Organization: LM-InnoTech s.r.l.
Organization Unit: PC-Point 
resto tutto default 
Configurazione DNS Dinamico
Service Type: Custom
Update URL: https://api.dynu.com/nic/update?hostame=PCP-FW-001.ddnsfree.com&password=Dynup@55
Result Match: good %IP%
Description: Dynu DDNS
Configurazione OpenVPN server con Wizard
Type of server: Local User Account
Certificate Authority: CA FW-PCP-001
Server Certificate: Add New
Name: Cert OpenVPN
Common Name: OpenVPN FW-PCP-001
il resto default
Configurazione del server
Description: OpenVPN Server
IPv4 Tunnel Network: 192.168.250.0/24
il resto default
salvare
Rientrare in modifica
Redirect IPv4 Gateway: Spuntato
Inter-Client Communication: Spuntato
NetBIOS enable: Spuntato
Gateway creation: Only IPv4
il resto default
Regole FireWall: entrambe spuntate
Finish
	Creare Certificati per utenti Admin
Common name: impostare come name: lminnotech o pcpoint
	Esportare Certificati per utenti Admin
Hostname resolution: other
Hostname: PCP-FW-001.ddnsfree.com
Salvare come default
esportare i certificati 
	Impostare hostname e PSK su Zabbix Agent
Hostname: FW-PCP-001
TLS PSK Identity: PSK-FW-PCP-001
TLS PSK: [psk generata con KeePass]
il resto default
	Impostare hostname e PSK su Zabbix Proxy
Hostname: PRX-PCP-001
TLS PSK Identity: PSK-PRX-PCP-001
TLS PSK: [psk generata con KeePass]
il resto default

## Personalizzazione presso il cliente
	Wizard per impostare WAN e LAN
	Impostare Rete WAN
IPV6: disabilitare
	Impostare Rete LAN
IPV6 disabilitare
	Routing
Eliminare GW non utilizzati
Impostare Monitoraggio Gateway 1.1.1.1
	Utenti
Disabilitare Admin
	Impostare nome e dominio del firewall (opzionale)
	Aprire porte (opzionale)
	Impostare VPN IpSec LAN to LAN (opzionale)
	Creare utenti (solo per OpenVPN)
	Verifiche finali
		Navigazione
		OpenVPN
		Zabbix



TODO - Da implementare
Cavo null modem USB-USB
Console tramite adattatore USB-Seriale
Autenticazione doppio fattore per OpenVPN
 

## NOTE
ADVANCED OPTIONS:

`Codice`
```bash
UserParameter=hw.uuid,(kenv -q smbios.system.uuid || /usr/local/sbin/dmidecode -s system-uuid || echo "UNKNOWN") | cut -c 1-36
UserParameter=hw.mac.primary,ifconfig -l | tr ' ' '\n' | grep -E '^(em|igb|re|ix|vtnet)[0-9]' | head -n 1 | xargs ifconfig | grep ether | awk '{print $2}'
### Temperatura CPU ###
# Discovery dei Core (Versione PHP stabile)
UserParameter=cpu.discovery,php -r '$n=(int)`sysctl -n kern.smp.cpus`;$d=[];for($i=0;$i<$n;$i++){$d[]=["{#CPUCORE}"=>(string)$i];}echo json_encode(["data"=>$d]);'
# Lettura Temperatura
UserParameter=cpu.temp[*],sysctl -n dev.cpu.$1.temperature | tr -d "C"
### Temperatura Motherboard ###
# Discovery Zone Termiche Motherboard
UserParameter=mb.discovery,php -r '$o=`sysctl -a hw.acpi.thermal | grep temperature`; preg_match_all("/hw\.acpi\.thermal\.(tz\d+)\.temperature/", $o, $m); $d=[]; foreach($m[1] as $z){$d[]=["{#TZONE}"=>$z];} echo json_encode(["data"=>$d]);'
# Lettura Temperatura della Zona specifica
UserParameter=mb.temp[*],sysctl -n hw.acpi.thermal.$1.temperature | tr -d "C"
### STATO DEI DISCHI con S.M.A.R.T. ###
# Discovery dei dischi fisici
UserParameter=disk.discovery,/usr/local/bin/php -r '$o=`/sbin/geom disk list | grep "Geom name:"`; preg_match_all("/Geom name: (\w+)/", $o, $m); $d=[]; foreach(array_unique($m[1]) as $dev){ if(preg_match("/^(cd|da\d+)/", $dev)) continue; $d[]=["{#DISKNAME}"=>"/dev/".$dev, "{#SHORTNAME}"=>$dev]; } echo json_encode(["data"=>$d]);'
# Master Item: Recupera tutto lo SMART in un colpo solo
UserParameter=disk.smart.json[*],smartctl -j -i -H -A $1
```

