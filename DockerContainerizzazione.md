# **Docker e Containerizzazione**

## Indice

- [Cos'è la Containerizzazione?](#cos%C3%A8-la-containerizzazione)
- [Breve Storia ed Evoluzione dei Container](#breve-storia-ed-evoluzione-dei-container)
- [A Cosa Serve Docker?](#a-cosa-serve-docker)
- [Componenti Chiave di Docker](#componenti-chiave-di-docker)
- [Concetti Fondamentali: Immagini e Container](#concetti-fondamentali-immagini-e-container)
- [Esercitazione 1: Primi Passi con Docker](#esercitazione-1-primi-passi-con-docker)
- [Esercitazione 2: Creazione e Gestione di Immagini Docker](#esercitazione-2-creazione-e-gestione-di-immagini-docker)
- [Esercitazione 3: Networking e Persistenza dei Dati con Docker](#esercitazione-3-networking-e-persistenza-dei-dati-con-docker)
- [Comandi utili](#comandi-utili)
- [Conclusioni](#conclusioni)

---

## Cos'è la Containerizzazione?
La **containerizzazione** è una tecnologia che permette di **creare ambienti isolati** in cui far girare applicazioni. Più precisamente, è una forma di **virtualizzazione a livello di sistema operativo** che consente di **impacchettare un'applicazione insieme a tutto ciò di cui ha bisogno per funzionare**: codice, runtime, librerie, variabili d'ambiente e file di configurazione. Tutto questo viene racchiuso in un'unità chiamata **container**.

A differenza delle tradizionali **macchine virtuali (VM)**, che virtualizzano l'intero hardware e includono un sistema operativo completo, i container **condividono il kernel** del sistema operativo dell'host. Questo li rende **molto più leggeri**, **veloci da avviare** e **meno esigenti in termini di risorse**.

In pratica, grazie ai container si evita il classico problema del "**funzionava sul mio computer**", perché l'applicazione viene eseguita sempre nello stesso ambiente, sia in fase di sviluppo che di test o produzione. In questo modo, si ottiene maggiore **affidabilità** e **coerenza** tra i diversi ambienti di lavoro.

![image](https://github.com/user-attachments/assets/55d10382-3c40-42c3-be79-a3ed6e5b0fc4)

> **Sunto per chi non ha voglia di leggere:** I container **non si riservano una porzione fissa di hardware** (CPU, RAM, disco) come fanno le macchine virtuali.
> Invece, condividono le risorse del sistema operativo host dinamicamente, usando ciò che serve quando serve.

---

## Breve Storia ed Evoluzione dei Container
La containerizzazione ha radici profonde nei sistemi Unix:

- **1979**: La funzionalità **chroot** di Unix introduce un primo concetto di isolamento del filesystem.
- **2000**: **FreeBSD jail** evolve chroot offrendo maggiore isolamento.
- **2005**: **Solaris Containers (Zones)** forniscono un ambiente virtualizzato più completo.
- **2008**: **LXC (Linux Containers)** introducono **namespace e cgroups** per l'isolamento.
- **2013**: **Docker** viene rilasciato, **semplificando drasticamente l'utilizzo dei container** rendendoli accessibili a un vasto numero di sviluppatori.

![image](https://github.com/user-attachments/assets/c37a5eaa-2096-46f2-9001-90d738aea995)

- **2015**: Nasce la **Open Container Initiative (OCI)** per standardizzare i formati dei container.
- **Oggi**: Un ecosistema maturo con strumenti di **orchestrazione** come **Kubernetes**.

---

## A Cosa Serve Docker?
Docker ha rivoluzionato il modo in cui le applicazioni vengono sviluppate, distribuite ed eseguite. I suoi principali vantaggi includono:

- **Consistenza ambientale**: Elimina le differenze tra ambienti, assicurando che l'applicazione funzioni allo stesso modo ovunque.
- **Portabilità**: I container possono essere eseguiti su qualsiasi sistema che supporti Docker.
- **Leggerezza e velocità**: I container consumano meno risorse e si avviano molto più rapidamente rispetto alle VM.
- **Isolamento**: Le applicazioni all'interno dei container sono isolate l'una dall'altra, evitando interferenze.
- **Scalabilità**: È facile replicare e scalare i container per gestire carichi di lavoro variabili.
- **Efficienza delle risorse**: Permette una maggiore densità di applicazioni per server.

---

## Componenti Chiave di Docker
L'**architettura di Docker** si basa su alcuni elementi chiave che lavorano insieme:

- **Docker Engine**: È il motore principale di Docker, responsabile di creare e gestire container. Include:
  - **dockerd**: Il demone (programma che gira in background) che si occupa di gestire immagini, container, reti e volumi.
  - **REST API**: Il canale attraverso cui applicazioni e strumenti esterni possono comunicare con Docker.
  - **CLI (Command Line Interface)**: La riga di comando che consente agli utenti di impartire comandi al demone.
- **Docker CLI**: È l'interfaccia che usi scrivendo comandi (`docker run`, `docker build`, ecc.) per interagire facilmente con Docker senza bisogno di programmare sulla REST API.
- **Docker Registry**: È il "magazzino" dove sono archiviate le **immagini Docker**. Il registry pubblico più conosciuto è **Docker Hub**, ma è possibile creare anche registry privati per gestire immagini aziendali o personali.

![image](https://github.com/user-attachments/assets/f43676d6-1bac-4709-bfec-96e9967848ca)

> In breve, Docker Engine gestisce i container, la CLI ti permette di controllarlo, e il Registry fornisce le immagini pronte da usare o su cui basarsi.

---

## Concetti Fondamentali: Immagini e Container

- **Immagine Docker**: Un **template di sola lettura** con le istruzioni per creare un container. È composta da **layer sovrapposti** (filesystem a strati), dove ogni layer rappresenta un'istruzione nel **Dockerfile** (il file che contiene le istruzioni per costruire l'immagine). Le immagini sono **immutabili** e possono essere basate su altre immagini (ereditarietà). Pensa a un'immagine come a una **classe** in programmazione orientata agli oggetti.
- **Container Docker**: Un'**istanza eseguibile di un'immagine**. Aggiunge un **layer scrivibile** sopra l'immagine di base. Ha un ciclo di vita ben definito (creazione, avvio, esecuzione, pausa, stop, eliminazione). I container sono isolati ma condividono risorse con l'host e possono essere connessi a reti e volumi.

![image](https://github.com/user-attachments/assets/661a53bd-ec0d-46c1-a7f4-200ebcba2d67)

---

## Esercitazione 1: Primi Passi con Docker
Questa esercitazione ti introdurrà all'installazione e all'esecuzione dei primi comandi Docker. Assicurati di avere un ambiente Alpine Linux pronto (che trovate già pronto nella dir `machine` - usate `alpineNAT`).

- Assicurati di soddisfare i **requisiti di sistema**: Alpine Linux 3.13+, Kernel 3.10+, accesso root/sudo, connessione internet.

- Esegui i **comandi di installazione**:
  ```bash
  # Aggiornare i repository
  apk update

  # Installare Docker
  apk add docker

  # Abilitare e avviare il servizio Docker all'avvio
  rc-update add docker boot
  service docker start

  # Verificare che Docker sia in esecuzione
  service docker status
  ```

  - Verifica l'installazione eseguendo `docker version`.
  > Noi lavoreremo sempre in amm. quindi saltiamo i prossimi due comandi, ricorda che per passare in amm. usa `su root`.

  - Se necessario, aggiungi il tuo utente al gruppo `docker` per evitare l'uso di `sudo` con i comandi Docker:
    ```bash
    sudo addgroup docker <tuo_utente>
    sudo usermod -aG docker <tuo_utente>
    newgrp docker # oppure disconnettiti e riconnettiti
    ```
  - Verifica lo stato del servizio Docker con `sudo rc-status docker` e, se non in esecuzione, avvialo con `sudo rc-service docker start`.

1. **Esecuzione del Container "Hello World"**:
   - Esegui il comando:
     ```bash
     docker run hello-world
     ```
   > Analizza l'output: Docker cercherà l'immagine `hello-world` localmente, se non la trova la scaricherà da Docker Hub, creerà un container da essa, lo eseguirà (stampando un messaggio) e terminerà.

2. **Container Interattivo**:
   - Esegui un container interattivo basato su un'immagine Alpine Linux:
     ```bash
     docker run -it alpine sh
     ```
     - `-it`: Alloca uno pseudo-terminale interattivo.
     - `alpine`: L'immagine da utilizzare.
     - `sh`: Il comando da eseguire all'interno del container.
   - Esplora l'ambiente con comandi Linux di base (es. `ls`, `cat /etc/os-release`).
   - Digita `exit` per uscire dal container.
   - Esegui `docker ps -a` per vedere che il container esiste ancora ma è fermato.

3. **Container in Background**:
   - Esegui un container NGINX in background:
     ```bash
     docker run -d -p 8080:80 nginx
     ```
     - `-d`: Esegue il container in modalità detached (background).
     - `-p 8080:80`: Mappa la porta 80 del container alla porta 8080 dell'host.
     - `nginx`: L'immagine da utilizzare.
   - Verifica che il container sia in esecuzione con `docker ps`.
   - Apri un browser sull'host e vai su `http://localhost` per visualizzare la pagina predefinita di NGINX.
   - Visualizza i log del container con `docker logs <ID_DEL_CONTAINER>` (l'ID lo trovi con `docker ps`).
   > Essendo che lavori su una VM ricorda di mappare le porte anche sull'hypervisor. (8080:8080)
   
  **Vuoi seguire la vibe?** esegui il comando di prima in modalità interattiva, e modifica la pagina di nginx.
      ```bash
      docker run -it -p 8080:80 nginx sh
      # Dentro la modalità interattiva
      cd usr/share/nginx/html
      # Modifica
      vi index.html
      ```
      
  **NB: Vuoi installare Vi usa:**
    ```bash
    apt update
    apt install vim
    ```

4. **Gestione del Ciclo di Vita dei Container**:
   - **Creare** un container senza avviarlo: `docker create <immagine>`
   - **Avviare** un container creato: `docker start <ID_DEL_CONTAINER>`
   - **Fermare** un container in esecuzione: `docker stop <ID_DEL_CONTAINER>` (invia SIGTERM, poi SIGKILL se necessario)
   - **Eliminare** un container fermato: `docker rm <ID_DEL_CONTAINER>`
   - Sperimenta con questi comandi utilizzando i container creati nei passaggi precedenti. Ricorda che i container sono effimeri (nell'ultimo layer) e le modifiche non salvate in volumi vengono perse all'eliminazione.

5. **Esplorazione di Docker Hub**:
   - Visita il sito web di Docker Hub (`hub.docker.com`).
   - Cerca immagini ufficiali (contrassegnate come "Official Image") di software come `nginx`, `alpine`, `ubuntu`, `nodejs`.
   - Leggi la documentazione delle immagini per capire come utilizzarle, quali tag sono disponibili (e il significato di tag come `latest` e versioni specifiche), e come configurarle.

---

# Esercitazione 2: Creazione e Gestione di Immagini Docker

## 1. Introduzione ai Dockerfile

* Un **Dockerfile** è un file di testo che contiene una sequenza di istruzioni per costruire un'immagine Docker in modo **automatico e ripetibile**.
* L'istruzione iniziale dev'essere **FROM**, che definisce l'**immagine base**.
* Le istruzioni Dockerfile più comuni sono:

  * **FROM**: Definisce l'immagine di partenza.
  * **RUN**: Esegue comandi durante la build.
  * **COPY** e **ADD**: Copiano file nel container.
  * **WORKDIR**: Imposta la directory di lavoro.
  * **ENV**: Definisce variabili d'ambiente.
  * **EXPOSE**: Indica le porte usate dal container.
  * **CMD**/**ENTRYPOINT**: Comando da eseguire quando il container parte.
  * **VOLUME**: Crea punti di montaggio per dati persistenti.

---

## 2. Preparazione di una Directory per Node-RED

### Crea la directory di progetto

Apri il terminale e crea una cartella dove inseriremo il `Dockerfile`:

```bash
mkdir nodered-alpine
cd nodered-alpine
```

### Crea un file `.dockerignore`

Per evitare di includere file inutili nella build:

```bash
echo "node_modules\nnpm-debug.log" > .dockerignore
```

---

## 3. Creazione del Dockerfile per Node-RED

### Crea un file `Dockerfile`

All'interno della directory, crea un file `Dockerfile` con il contenuto seguente:

```Dockerfile
FROM node:18-alpine

# Installa strumenti di compilazione necessari per alcuni node
RUN apk add --no-cache \
    python3 \
    make \
    g++ \
    bash

# Installa Node-RED
RUN npm install -g --unsafe-perm node-red

# Crea una cartella per i flussi
WORKDIR /data

# Espone la porta 1880 usata da Node-RED
EXPOSE 1880

# Comando di avvio
CMD ["node-red"]
```

### Spiegazione delle istruzioni

* `FROM node:18-alpine`: Usa un'immagine base leggera con Node.js.
* `apk add`: Installa tool di sistema essenziali per compilare eventuali dipendenze.
* `npm install -g node-red`: Installa Node-RED globalmente.
* `WORKDIR /data`: Imposta la directory dove Node-RED salva i flussi.
* `EXPOSE 1880`: Porta su cui gira l'interfaccia web di Node-RED.
* `CMD ["node-red"]`: Avvia Node-RED all'avvio del container.

---

## 4. Costruzione dell’Immagine Docker

Dalla directory in cui si trova il Dockerfile, esegui:

```bash
docker build -t my-nodered-alpine .
```

* `-t my-nodered-alpine`: Dai un nome all’immagine.
* `.`: Usa la directory corrente come contesto di build.

Verifica che l'immagine sia stata creata:

```bash
docker images
```

---

## 5. Avvio del Container Node-RED

Esegui il container:

```bash
docker run -d -p 1880:1880 --name nodered my-nodered-alpine
```

* `-p 1880:1880`: Espone la porta web di Node-RED.
* `--name nodered`: Dai un nome al container.

Apri il browser su:

```
http://localhost:1880
```

Dovresti vedere l'interfaccia di Node-RED pronta per l'uso.

---

# Esercitazione 3: Networking e Persistenza dei Dati con Docker

Questa esercitazione si concentra su due aspetti fondamentali di Docker:

1. **Networking**: come isolare e mettere in comunicazione tra loro più container  
2. **Persistenza**: come fare in modo che i dati sopravvivano al ciclo di vita effimero di un container  

---

### 1. Networking Docker

#### **Concetti teorici**
- **Namespace di rete**: ogni rete Docker crea un “namespace” isolato, cioè un piccolo mondo virtuale in cui i container possono avere interfacce, tabelle di routing e firewall indipendenti dall’host o da altre reti.
- **Isolamento e sicurezza**: scegliendo reti diverse si controlla chi può scoprire e raggiungere chi. Questo è alla base dell’architettura “zero trust”: di default un container su una rete non vede gli altri sulle reti diverse.

#### **Reti predefinite**
- **bridge**  
  - È la rete “di casa” di Docker.  
  - Ogni container lanciato senza specificare rete finisce qui.  
  - I container su `bridge` possono comunicare tra loro usando IP privati, ma per collegarsi dall’esterno serve il _port mapping_.
- **host**  
  - Il container condivide **tutto** lo stack di rete dell’host: stessa interfaccia di rete, stessi IP e porte.  
  - Vantaggio: latenza minima, nessun NAT.  
  - Svantaggio: nessun isolamento (quindi da usare con cautela).
- **none**  
  - Il container non ha alcuna interfaccia di rete.  
  - Serve per scenari ultra-isolati, dove la rete non serve o viene gestita esternamente.

#### **Reti personalizzate**
- Creando una rete di tipo _bridge_ custom si ottiene:
  - **DNS interno**: i container si scoprono per nome (es. `redis`, `web`).
  - **Maggiore isolamento**: solo chi è collegato a quella rete può comunicare.
- Comando di base:
  ```bash
  docker network create my-network
  ```
- Esempio di avvio:
  ```bash
  docker run -d --name my-app   --network my-network my-nodejs-app:1.0
  docker run -d --name my-postgres --network my-network postgres:15-alpine
  ```
  – Ora `my-app` può raggiungere il database all’hostname `my-postgres` senza configurare IP.

#### **Esposizione di porte**
- To expose a service outside the host, si usa:
  ```bash
  docker run -p <porta_host>:<porta_container> ...
  ```
- Teoria del NAT: Docker crea regole iptables sull’host che mappano `host:porta_host` → `container_ip:porta_container`.

---

### 2. Comunicazione tra Container in Rete (con Docker Compose)

#### **Concetti teorici**
- **Orchestrazione leggera**: Docker Compose permette di descrivere in un unico YAML più servizi, reti e volumi, e di avviarli/fermarli insieme.
- **Service discovery**: grazie alla rete Compose, ogni servizio è raggiungibile col nome definito sotto `services:`.

#### **Esempio base (`docker-compose.yml`)**
```yaml
version: '3.8'
services:
  redis:
    image: redis:alpine
    networks:
      - my-app-net

  web:
    image: your-node-app-image:latest
    ports:
      - "8080:3000"
    networks:
      - my-app-net
    depends_on:
      - redis

networks:
  my-app-net:
    driver: bridge
```

- **`depends_on`** garantisce l’ordine di avvio (ma non verifica che Redis sia “pronto”: per quello servono _healthchecks_).
- Tutti i container finiscono sulla rete `my-app-net` e si scambiano richieste via DNS interno.

---

### 3. Persistenza dei Dati con Volumi Docker

#### **Concetti teorici**
- I **container** sono basati su file system **copy-on-write** e sono per definizione **stateless**: quando lo elimini, tutto dentro scompare.
- I **volumi** sono gestiti dal demone Docker:
  - Risiedono fuori dal file system di ogni singolo container.
  - Hanno un driver (di default `local`): puoi anche usare driver remoti o cifrati.
  - Sono indipendenti dal ciclo di vita dei container: restano esistenti finché non li cancelli esplicitamente.

#### **Creazione e utilizzo di un volume**
1. **Crea il volume**  
   ```bash
   docker volume create my-data-volume
   ```
2. **Esegui un container montando il volume**  
   ```bash
   docker run -d \
     --name my-db \
     -v my-data-volume:/var/lib/postgresql/data \
     postgres:15-alpine
   ```
   - `/var/lib/postgresql/data` è il path interno in cui PostgreSQL salva i dati.
3. **Vantaggi**  
   - Se elimini e ricrei il container, i dati restano intatti.  
   - Puoi collegare lo stesso volume a più container di backup, analisi, migrazione, ecc.

#### **Bind mount vs Volume**
- **Volume**  
  - Gestito da Docker, portabile e più performante su Linux.  
  - Non dipende da percorsi host espliciti.
- **Bind mount**  
  - Mappa una cartella qualsiasi dell’host (`/home/user/data`) dentro il container.  
  - Utile in sviluppo (vedi live-reload dei sorgenti), ma meno portable e potenzialmente meno safe (dipende da permessi host).
---
## Comandi Utili

```bash
# Lista dei container attivi
docker ps

# Lista di tutti i container (anche fermi)
docker ps -a

# Avviare un container fermo
docker start nome_container

# Fermare un container in esecuzione
docker stop nome_container

# Riavviare un container
docker restart nome_container

# Cancellare un container fermo
docker rm nome_container

# Cancellare tutti i container fermi
docker container prune

# Vedere i log di un container
docker logs nome_container

# Controllare lo stato dettagliato di un container
docker inspect nome_container

# Entrare nella shell del container (se bash è disponibile)
docker exec -it nome_container bash

# Entrare nella shell del container (se solo sh è disponibile)
docker exec -it nome_container sh

# Eseguire un comando dentro il container
docker exec nome_container comando

# Creare e avviare un nuovo container in background
docker run -d --name mio_container nginx

#Avvio automatico del container all'avvio della macchina virtuale
docker run --restart=always -d --name mio_container nginx

```

---
## Conclusioni

Queste tre esercitazioni coprono gli aspetti fondamentali di Docker, partendo dall'esecuzione di container semplici, passando alla creazione di immagini personalizzate e concludendo con la gestione del networking e della persistenza dei dati. Ricorda di consultare la documentazione ufficiale di Docker per approfondire ulteriormente ogni argomento e per i comandi specifici relativi all'installazione e alla configurazione su Alpine Linux.
