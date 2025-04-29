# Virtualizzazione

## Indice

1. 🧠 **Cosa sono le macchine virtuali (VM)?**  
2. ⚙️ **Architettura: livelli di virtualizzazione**  
   2.1. 🧱 Hardware (Host)  
   2.2. 🎛️ Hypervisor  
   2.3. 💻 Macchine virtuali  
3. 🧰 **Tipi di hypervisor**  
   3.1. 🔹 Hypervisor di tipo 1 (bare-metal)  
   3.2. 🔹 Hypervisor di tipo 2 (hosted)  
4. 🔄 **Come le risorse vengono virtualizzate**  
   4.1. 🧠 CPU  
   4.2. 🧮 RAM  
   4.3. 💾 Disco  
   4.4. 🌐 Schede di rete  
5. 🌍 **Modalità di rete delle VM**  
   5.1. 🔹 NAT  
   5.2. 🔹 Bridge  
   5.3. 🔹 Host-Only  
   5.4. 🔹 Internal Network  
6. 🎯 **Conclusioni**  

## 🧠 COSA SONO LE MACCHINE VIRTUALI (VM)?

Una **macchina virtuale** è un'istanza software che emula l’intero funzionamento di un computer fisico, eseguendo un sistema operativo e applicazioni come se fosse una macchina reale.

Ma dietro le quinte, la virtualizzazione si basa su un processo complesso che coinvolge:

- **La separazione tra hardware fisico e risorse logiche**
- **L'astrazione e la gestione delle risorse da parte di un hypervisor**

---

## ⚙️ ARCHITETTURA: LIVELLI DI VIRTUALIZZAZIONE

Per capire bene come funzionano le VM, possiamo dividerle in **livelli**:

### 🧱 1. Hardware (Host)
Il livello fisico: CPU, RAM, disco, schede di rete, ecc.

### 🎛️ 2. Hypervisor
È lo strato di virtualizzazione che crea e gestisce le VM. Si occupa di:

- Allocare risorse (CPU, RAM, disco) alle macchine virtuali
- Intercettare e tradurre le istruzioni hardware (es. accessi alla memoria o alla rete)
- I/O (Input/Output) virtualizzato

### 💻 3. Macchine Virtuali
Ogni VM ha:

- Una **CPU virtuale** (vCPU)
- RAM virtuale
- Schede di rete, dischi, e anche dispositivi USB virtuali
- Un **sistema operativo guest** che crede di essere su una macchina fisica

---

## 🧰 TIPI DI HYPERVISOR

### 🔹 Hypervisor di tipo 1 (bare-metal)
- Si installa **direttamente sull’hardware**, senza bisogno di un sistema operativo host.
- Più performante e sicuro.
- Usato in ambienti server/produttivi.
- Esempi: VMware ESXi, Microsoft Hyper-V, Xen

### 🔹 Hypervisor di tipo 2 (hosted)
- Gira **sopra un sistema operativo esistente** (Windows, Linux, macOS).
- È più semplice da usare, ma leggermente meno efficiente.
- Esempi: Oracle VirtualBox, VMware Workstation, Parallels

---

## 🔄 COME LE RISORSE VENGONO VIRTUALIZZATE

### 🧠 CPU
L’hypervisor può assegnare **vCPU** alle VM. Queste sono core virtuali mappati su core fisici (o thread via Hyper-Threading).

### 🧮 RAM
Ogni VM riceve una porzione di RAM. Alcuni hypervisor supportano **overcommitment** (allocare più RAM virtuale di quella realmente disponibile).

### 💾 Disco
Le VM usano **dischi virtuali** (file con estensione .vdi, .vmdk, .qcow2, ecc.) che simulano veri dischi rigidi.

### 🌐 Schede di rete
Ogni VM può avere una o più interfacce di rete virtuali (vNIC), che si collegano a varie modalità di rete:

---

## 🌍 MODALITÀ DI RETE DELLE VM

### 🔹 NAT (Network Address Translation)
- La VM condivide la connessione dell’host verso l’esterno.
- L’hypervisor fa da "router": assegna un IP privato alla VM e traduce gli indirizzi.
- La VM **può accedere a Internet**, ma **non può essere raggiunta dall'esterno**.

### 🔹 Bridge
- La VM è **collegata direttamente alla rete fisica** come un dispositivo autonomo.
- Riceve un indirizzo IP dal router/switch di rete.
- **Può essere raggiunta da altri dispositivi**, utile per server o test di rete.

### 🔹 Host-Only
- La VM comunica **solo con l’host**, tramite una rete privata virtuale.
- Non c'è accesso a Internet, ma utile per ambienti di test.

### 🔹 Internal Network
- Le VM comunicano **solo tra loro**, sulla stessa rete virtuale.
- Isolate sia dall’host che da Internet.
 

| Modalità rete | Accesso a Internet | Accesso dalla LAN | Isolamento |
|---------------|-------------------|-------------------|------------|
| NAT           | ✅                | ❌                | Alto       |
| Bridge        | ✅                | ✅                | Nessuno    |
| Host-Only     | ❌                | Solo host         | Medio      |
| Internal      | ❌                | Solo altre VM     | Totale     |

---

## 🎯 CONCLUSIONI

Una **VM** è un sistema isolato che emula un computer fisico grazie a un **hypervisor**, che gestisce e traduce le richieste verso l’hardware reale. Le modalità di rete permettono di controllare il livello di connessione e visibilità della VM, adattandola allo scenario d’uso.