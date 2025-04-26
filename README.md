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
     docker run -d -p 80:80 nginx
     ```
     - `-d`: Esegue il container in modalità detached (background).
     - `-p 80:80`: Mappa la porta 80 del container alla porta 80 dell'host.
     - `nginx`: L'immagine da utilizzare.
   - Verifica che il container sia in esecuzione con `docker ps`.
   - Apri un browser sull'host e vai su `http://localhost` per visualizzare la pagina predefinita di NGINX.
   - Visualizza i log del container con `docker logs <ID_DEL_CONTAINER>` (l'ID lo trovi con `docker ps`).
   > Essendo che lavori su una VM ricorda di mappare le porte anche sull'hypervisor.

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

Questa esercitazione si concentra sulla creazione di **immagini Docker personalizzate** utilizzando i **Dockerfile**.

## 1. Introduzione ai Dockerfile

- Un **Dockerfile** è un file di testo contenente una serie di **istruzioni per costruire un'immagine Docker in modo automatizzato**.
- La prima istruzione non commentata deve essere **FROM**, che definisce l'**immagine base** da cui partire.
- Altre istruzioni principali includono:
  - **RUN**: Esegue comandi all'interno del container durante la fase di build (ogni `RUN` crea un nuovo layer).
  - **COPY**: Copia file e directory locali nell'immagine.
  - **ADD**: Simile a `COPY`, ma può anche estrarre archivi e scaricare file da URL.
  - **WORKDIR**: Imposta la directory di lavoro per le istruzioni successive.
  - **ENV**: Imposta variabili d'ambiente.
  - **EXPOSE**: Indica le porte che il container ascolterà a runtime (non le pubblica effettivamente).
  - **CMD**: Specifica il comando predefinito da eseguire all'avvio del container (può essere sovrascritto).
  - **ENTRYPOINT**: Specifica l'eseguibile principale del container (più difficile da sovrascrivere di `CMD`).
  - **VOLUME**: Crea un punto di montaggio per volumi esterni.
  - **USER**: Imposta l'utente per eseguire i comandi successivi e il container.

![image](https://github.com/user-attachments/assets/10ccb528-728e-4c9b-b133-993245f2c0df)

---

## 2. Creazione di un Dockerfile per un'Applicazione Node.js Semplice

### Crea una directory per l'applicazione

Apri il terminale e crea una cartella, ad esempio:

```bash
mkdir mia-app-node
cd mia-app-node
```

### Crea il file `package.json`

Dentro la directory, crea un file `package.json` con questo contenuto base:

```json
{
  "name": "mia-app-node",
  "version": "1.0.0",
  "description": "Una semplice applicazione Node.js",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
*(Qui usiamo `express` per creare un server HTTP velocemente.)*

### Crea il file `app.js`

Ora crea `app.js` con un semplice server Express:

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Ciao dal container Docker!');
});

app.listen(port, () => {
  console.log(`App in ascolto su http://localhost:${port}`);
});
```

### Crea il file `.dockerignore`

> Importantissimo per non mandare file inutili nella build!  
> Crea un file `.dockerignore` con dentro ad esempio:
> 
> ```
> node_modules
> npm-debug.log
> ```
> 
> Così Docker non includerà la cartella `node_modules` (che verrà ricreata nel container) e i file di log.

### Crea il file `Dockerfile`

Nella stessa directory crea un file chiamato `Dockerfile` con un contenuto simile a questo:

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD [ "npm", "start" ]
```

- **Spiegazione delle istruzioni**:
  - `FROM node:18-alpine`: Utilizza l'immagine ufficiale Node.js basata su Alpine Linux come immagine base.
  - `WORKDIR /app`: Imposta `/app` come directory di lavoro all'interno del container.
  - `COPY package*.json ./`: Copia i file `package.json` e `package-lock.json` (se presente) nella directory `/app`.
  - `RUN npm install`: Installa le dipendenze Node.js.
  - `COPY . .`: Copia il resto dei file dell'applicazione nella directory `/app`.
  - `EXPOSE 3000`: Indica che l'applicazione ascolterà sulla porta 3000.
  - `CMD [ "npm", "start" ]`: Specifica il comando per avviare l'applicazione.

---

## 3. Costruzione dell'Immagine Docker

- Naviga nella directory contenente il `Dockerfile` e la tua applicazione.
- Esegui il comando `docker build`:

  ```bash
  docker build -t my-nodejs-app:1.0 .
  ```

  - `-t my-nodejs-app:1.0`: Assegna un nome (`my-nodejs-app`) e un tag (`1.0`) all'immagine.
  - `.`: Specifica il contesto di build (la directory corrente).

- Verifica che l'immagine sia stata creata con:

  ```bash
  docker images
  ```

---

## 4. Esecuzione di un Container dall'Immagine

- Esegui un container basato sulla tua immagine:

  ```bash
  docker run -d -p 8080:3000 my-nodejs-app:1.0
  ```

  - `-p 8080:3000`: Mappa la porta 3000 del container alla porta 8080 dell'host.

- Verifica che l'applicazione funzioni aprendo un browser su:

  ```
  http://localhost:8080
  ```

- Visualizza i log del container:

  ```bash
  docker logs <ID_DEL_CONTAINER>
  ```

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
Queste tre esercitazioni coprono gli aspetti fondamentali di Docker, partendo dall'esecuzione di container semplici, passando alla creazione di immagini personalizzate e concludendo con la gestione del networking e della persistenza dei dati. Ricorda di consultare la documentazione ufficiale di Docker per approfondire ulteriormente ogni argomento e per i comandi specifici relativi all'installazione e alla configurazione su Alpine Linux.
