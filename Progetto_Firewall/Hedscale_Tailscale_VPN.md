# Headscale e Tailscale VPN

Questa implementazione permette di raggiungere i firewall per consentirne la manutenzione.
Il funzionamento prevede la presenza di un server Headscale sempre raggiungibile, che gestisce la sicurezza delle vpn e l’instradamento delle connessioni.
Dal momento che la VPN è instaurata dal client, non è necessario che il firewall abbia l’IP pubblico. 

## Situazione attuale (in produzione ma TEMPORANEA)
il server headscale HS-M3-01 è in un docker su DK-M3-01 (macchina fisica ex server vega)
Risponde all’indirizzo https://HS-M3-01.ddnsfree.com:8443

La WebGUI per la gestione è in un docker su DK-M3-01
Risponde all’indirizzo https://HS-M3-01.ddnsfree.com:8443/web
Non ha user e password perchè usa una chiave di connessione (API key) che salva nel browser
In KeePass ce n’è una salvata e per generare una nuova si usa il comando:

```bash
docker exec headscale headscale apikeys create --expiration 90d
```

Anomalia sulle pre-authentication key: si possono generare ma poi non si “vedono”. 
Trucco: All’atto della generazione, usare F12 per aprire console di debug e sotto la scheda network identificare la voce preauthenticate


## Come funziona
### Lato client - il firewall - Tailscale
Installare il pacchetto ufficiale tailscale
Definire una regola che permetta l’accesso solo al firewall
Configurare Tailscale
Login server: [il server headscale - deve essere un dominio con certificato]
Pre-authentication Key: [la chiave generata dal server]

### Lato server -Headscale
Sul server bisogna generare una chiave di autenticazione pre-authentication-key:
```bash
	docker exec headscale headscale --user 1 preauthkeys create --expiration 1h
```
oppure crearla sulla WebGUI facendo attenzione all’anomalia citata sopra.

## Installazione client per Windows
Installare Tailscale
Non è possibile configurarlo tramite GUI
Usare powershell con privilegi admin
Digitare il comando:
```bash 
tailscale login --login-server https://HS-M3-01.ddnsfree.com:8443
```
oppure se non è la prima volta:
```bash
tailscale up --reset --login-server https://HS-M3-01.ddnsfree.com:8443
```

Compare un messaggio con un link da aprire nel browswer.
Sulla pagina che si apre, c’è un codice da copiare
Andare sul server e digitare il comando:
```bash
docker exec headscale headscale -u 1 nodes register --key [+ la chave ricevuta]
```
oppure usare l’interfaccia grafica per aggiungere il nodo

Se tutto va bene, nella finestra powershell compare la scritta Registered.

## Comandi utili:
```bash
docker exec headscale headscale nodes list
docker exec headscale headscale apikeys list
docker exec headscale headscale preauthkeys list
```





