# 🧭 Guida: Installare Alpine Linux su VirtualBox

## 📥 1. Scarica Alpine Linux

* Vai su: [https://alpinelinux.org/downloads/](https://alpinelinux.org/downloads/)
* Scarica l’immagine ISO **Standard** (es. `alpine-standard-3.19.1-x86_64.iso`)

---

## 🖥️ 2. Crea una nuova macchina virtuale su VirtualBox

1. Apri **Oracle VirtualBox**
2. Clicca su **Nuova**
3. Imposta:

   * **Nome**: AlpineLinux
   * **Tipo**: Linux
   * **Versione**: Other Linux (64-bit)
4. **Memoria**: almeno **256 MB** (meglio 512 MB o più)
5. **Disco fisso**:

   * Crea un nuovo disco VDI (virtuale dinamico)
   * Dimensione: **2 GB o più**

---

## 📁 3. Monta la ISO

1. Vai su **Impostazioni** della VM → **Archiviazione**
2. Seleziona il controller IDE → clicca sull’icona del disco → **Scegli un file di disco...**
3. Seleziona la ISO scaricata

---

## 🌐 4. Configurazione di rete (NAT o Bridge)

Vai su **Impostazioni → Rete**, poi scegli una delle seguenti modalità:

### 🔹 Modalità NAT (Default)

* Permette alla VM di accedere a Internet tramite l'host.
* Utile per la maggior parte delle situazioni.

Impostazioni:

* **Scheda 1** → **Abilitata**
* **Allegata a**: NAT

Puoi anche configurare **port forwarding** (es. per SSH):

* Clic su **Avanzate → Port Forwarding**

  * Nome: SSH
  * Protocollo: TCP
  * Porta host: 2222
  * Porta guest: 22

### 🔸 Modalità Bridge

* La VM è visibile nella rete locale come un host a sé stante.
* Utile per testare server accessibili da altri dispositivi.

Impostazioni:

* **Scheda 1** → **Abilitata**
* **Allegata a**: Scheda con bridge
* Seleziona la scheda di rete fisica del tuo PC (es. Wi-Fi o Ethernet)

---

## 🚀 5. Avvia la VM e installa Alpine

1. Avvia la macchina virtuale
2. Dopo il boot, accedi come:

   * **Login**: `root` (nessuna password, quindi \n\r)
3. Avvia il processo guidato:

```sh
setup-alpine
```

### Durante il setup:

* **Selezione tastiera**: it (oppure us)
* **Hostname**: alpine-vm (puoi lasciare quello proposto)
* **Interfaccia di rete**:

  * Solitamente sarà `eth0`
  * Se usi NAT o Bridge, scegli **dhcp**
* **Mirror Alpine**: puoi accettare quello proposto o selezionarne uno più vicino
* **Timezone**: es. Europe/Rome
* **Password root**: imposta una password sicura
* **SSHD**: consigliato sì (per accesso remoto)
* **Setup user?**: opzionale (puoi creare un utente non-root)
* **Which disk use for installation**: es. `sda`
* **How to use it**: `sys` (installazione su disco)

---

## 💾 6. Rimuovi la ISO e riavvia

* Dopo l’installazione, **spegni la VM**
* Vai su **Impostazioni → Archiviazione**, rimuovi l’ISO dal lettore
* Riavvia la macchina: Alpine partirà dal disco

---

## 🌐 7. Verifica la rete

Dopo il reboot, accedi con `root` e la password che hai impostato.

### Verifica IP:

```sh
ip a
```

Dovresti vedere un indirizzo IP su `eth0`.

### Test connessione:

```sh
ping -c 4 google.com
```

### In caso di Bridge:

Controlla che il router assegni l’IP correttamente, oppure configura manualmente con:

```sh
vi /etc/network/interfaces
```

E aggiungi:

```ini
auto eth0
iface eth0 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
```

Poi riavvia il networking:

```sh
/etc/init.d/networking restart
```

---

> ### NB: In caso di ssh ripetuto sulla stessa porta in Windows dovrai eliminare l'host conosciuto per l'ssh su quella porta

```sh
notepad "C:\Users\NomeUserSocrate\.ssh\known_hosts"
```

## ✅ Conclusione

Hai ora una VM Alpine Linux minimal ma potente, pronta per lo sviluppo, test di server o container.
