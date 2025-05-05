# Virtualizzazione

## Indice

1. 🧠 **Cosa sono le macchine virtuali (VM)?**
2. ⚙️ **Architettura: livelli di virtualizzazione**
3. 🧰 **Tipi di hypervisor**
4. 🔄 **Come vengono virtualizzate le risorse**
5. 🌐 **Reti virtuali e modalità di connessione**
6. ✅ **Vantaggi e ❌ Svantaggi**
7. 🎯 **Conclusioni**

---

## 1. 🧠 Cosa sono le macchine virtuali (VM)?

Una **macchina virtuale (VM)** è un'istanza software che riproduce tutte le funzionalità di un computer fisico. Grazie alla virtualizzazione, è possibile far girare più VM sullo stesso server (host), ognuna con:

* Un proprio **sistema operativo guest** (ad es. Linux, Windows, macOS)
* CPU virtuali (vCPU), memoria RAM, storage e interfacce di rete mappate su risorse fisiche
* Dischi virtuali (file .vmdk, .vdi, .qcow2) che simulano hard disk

**Perché usare le VM?**

* **Isolamento**: difendono l’host e altre VM da crash o malfunzionamenti
* **Flessibilità**: creazione, clonazione, snapshot e ripristino in pochi clic
* **Efficienza**: consolidano più carichi di lavoro su un unico hardware
* **Portabilità**: spostamento rapido di VM tra host o cloud diversi

![image](https://github.com/user-attachments/assets/ccd1d6d4-34b5-4b79-98e5-74853e8dd525)

---

## 2. ⚙️ Architettura: livelli di virtualizzazione

Per comprendere l’ecosistema della virtualizzazione, distinguiamo tre livelli principali:

### 2.1 🧱 Hardware (Host)

Il livello più basso: server fisico con:

* Processori (CPU multicore, Hyper-Threading)
* Moduli di memoria RAM
* Controller di storage (SSD, HDD)
* Schede di rete (NIC), GPU e periferiche vari

![image](https://github.com/user-attachments/assets/a6c344a1-e10f-4d7e-9146-0a70fce2a9b6)

### 2.2 🎛️ Hypervisor

L’hypervisor è il cuore della virtualizzazione. Le sue responsabilità principali:

* **Gestione delle risorse**: assegna dinamicamente CPU, RAM, I/O
* **Traduzione delle istruzioni**: intercetta chiamate hardware da VM e le converte per l’host
* **Isolamento**: impedisce che una VM influenzi le altre
* **Interfaccia di controllo**: API e dashboard per operazioni (creazione, snapshot, migrazione)

![image](https://github.com/user-attachments/assets/22c690d7-053e-4f11-83c2-5ba2e5dc916b)

### 2.3 💻 Macchine Virtuali (Guest)

Ogni VM è un'entità autonoma composta da:

* **vCPU**: core virtuali pianificati dall’hypervisor
* **vRAM**: blocchi di memoria dedicati
* **vNIC**: interfacce di rete virtuali
* **Disco virtuale**: file o LUN che fungono da hard disk
* **Sistema operativo e applicazioni** installati come su un PC reale

---

## 3. 🧰 Tipi di hypervisor

### 3.1 🔹 Hypervisor di tipo 1 (bare-metal)

* Installazione **diretta sull'hardware** senza OS intermedio
* Prestazioni elevate e minore latenza
* Utilizzato in datacenter e cloud (infrastrutture di produzione)
* **Esempi**: VMware ESXi, Microsoft Hyper-V, XenServer, KVM (in modalità bare-metal)

### 3.2 🔹 Hypervisor di tipo 2 (hosted)

* Esegue all’interno di un sistema operativo host (Windows, Linux, macOS)
* Setup rapido, ideale per sviluppo e test locali
* Sovraccarico aggiuntivo del sistema host
* **Esempi**: Oracle VirtualBox, VMware Workstation, Parallels Desktop

![image](https://github.com/user-attachments/assets/095928ad-72c3-4952-8c0f-7d33988123df)

---

## 4. 🔄 Come vengono virtualizzate le risorse

### 4.1 🧠 CPU

* **vCPU**: thread pianificati su core fisici
* **Overcommitment**: assegnare più vCPU di core fisici (uso in scenari con carichi inattivi)
* **CPU Pinning**: vincolare vCPU a core specifici per prestazioni critiche

### 4.2 🧮 RAM

* Allocazione statica o dinamica (memory ballooning)
* **Overcommitment** di memoria con ballooning driver nei guest
* Condivisione di pagine identiche (TPS, Transparent Page Sharing)

### 4.3 💾 Disco

* Dischi virtuali:

  * **Thick provisioning**: spazio allocato immediatamente
  * **Thin provisioning**: spazio allocato su richiesta
* **Snapshot**: punti di ripristino che catturano lo stato disco e memoria
* Backend di storage: SAN, NAS, local SSD/RAID

### 4.4 🌐 Schede di rete

* **vNIC** mappate su bridge, NAT o reti interne
* Virtual Switch/Distributed Switch per instradamento interno
* **SR-IOV**: mappatura diretta di una NIC fisica alla VM per latenza minima

![image](https://github.com/user-attachments/assets/97a5087c-b165-4f61-b530-b7625044de95)

---

## 5. 🌐 Reti virtuali e modalità di connessione

| Modalità      | Accesso Internet | Accesso dalla LAN | Isolamento |
| ------------- | ---------------- | ----------------- | ---------- |
| **NAT**       | ✅                | ❌                 | Alto       |
| **Bridge**    | ✅                | ✅                 | Basso      |
| **Host-Only** | ❌                | Solo host         | Medio      |
| **Internal**  | ❌                | Solo altre VM     | Totale     |

### 5.1 NAT (Network Address Translation)

La VM utilizza l’IP e la connessione dell’host. Buono per navigazione, aggiornamenti, ma non riceve connessioni in ingresso.

### 5.2 Bridge

La VM appare come nodo autonomo sulla LAN. Riceve IP dal router e può comunicare bidirezionalmente con altri dispositivi.

![image](https://github.com/user-attachments/assets/7379fc27-0eb3-43c1-9bcd-c1a35cf3aafa)

### 5.3 Host-Only

Rete privata tra host e VM, isolata da Internet e LAN. Perfetta per test sicuri e sviluppo.

### 5.4 Internal Network

Rete esclusiva tra VM sullo stesso host. Utile per cluster di VM e testing di servizi distribuiti senza esporli.

![image](https://github.com/user-attachments/assets/0bd3395f-7c82-4f73-9d1d-ed429950b72e)

---

## 7. ✅ Vantaggi e ❌ Svantaggi

### ✅ Vantaggi

* **Isolamento completo**
  Ogni VM è separata dalle altre e dall’host, per cui crash, malfunzionamenti o attacchi in una VM non si ripercuotono sul sistema fisico né sulle altre macchine virtuali.

* **Consolidamento e ottimizzazione dell’hardware**
  Più carichi di lavoro possono girare sullo stesso server fisico, incrementando l’utilizzo delle risorse e riducendo i costi di infrastruttura.

* **Flessibilità operativa**
  Creazione, clonazione, snapshot e ripristino di VM in modo rapido, semplificando backup, test e deployment di ambienti omogenei.

* **Portabilità e scalabilità**
  VM facilmente “trasportabili” tra host fisici o cloud diversi, garantendo migrazioni live (vMotion, Live Migration) e autoscaling in ambienti virtualizzati.

* **Provisioning rapido**
  Possibilità di distribuire nuove VM in pochi minuti, automatizzando interi stack applicativi con tool di orchestrazione (es. Terraform, Ansible, Kubernetes).

### ❌ Svantaggi

* **Overhead di prestazioni**
  L’hypervisor introduce uno strato di astrazione che può comportare latenze e un utilizzo di risorse leggermente superiore rispetto al bare-metal.

* **Gestione complessa**
  Infrastrutture di virtualizzazione molto ampie richiedono strumenti avanzati di monitoraggio, orchestration e capacity planning per evitare “VM sprawl” e disallineamenti di risorse.

* **Licensing e costi software**
  Alcuni hypervisor commerciali e tool di gestione (vCenter, SCVMM) comportano licenze costose; inoltre, software guest come Windows Server possono richiedere ulteriori permessi.

* **Security boundary condivisa**
  Sebbene isolata, la sicurezza dipende dall’hypervisor: vulnerabilità a livello di virtualizzazione possono potenzialmente compromettere più VM contemporaneamente.

* **Dipendenza dall’hardware**
  Caratteristiche avanzate (SR-IOV, nested virtualization, offload) richiedono CPU, chipset e storage controller compatibili, e un firmware aggiornato.

![image](https://github.com/user-attachments/assets/da6a179e-d85f-4ed2-bdce-7e7b9f0af02f)

---

## 7. 🎯 Conclusioni

La virtualizzazione consente di massimizzare l’efficienza hardware, garantire isolamento e flessibilità e ridurre i costi operativi. Scegliere il giusto hypervisor, configurare correttamente risorse e reti virtuali, sono gli elementi chiave per un’infrastruttura robusta e scalabile.

![image](https://github.com/user-attachments/assets/23d01e6a-15a7-4e36-ab1e-e3af9600dedb)

