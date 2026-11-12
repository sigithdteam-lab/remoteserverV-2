# 🚀 Remote Server Management V2 (ManageServer)

**Remote Server Management V2** adalah *toolkit* Bash modular yang dirancang untuk mengelola banyak server Linux secara terpusat via SSH. Dilengkapi dengan **Smart Tuning**, **Proxmox Cluster Check**, **Safety Guards**, dan **Audit Log**.

---

## ➨ Fitur Utama

1. **SSH Key & Auth Management: ** Generate otomatis 3 tipe key (Ed25519, ECDSA, RSA), sinkonisasi `known_hosts`, dan *fallback* legacy `ssh-rsa` untuk server lama.
2. **Multi-Server Execution:** Fitur exekusi paralel/berurutan dengan output terisolasi per server dan rekap status (*SUCCESS/FAILED/TIMEOUT*).
3. **User & Group Management:** Manajemen user/group dengan **5 Lapis Proteksi** (tidak bisa hapus root, system account, atau akun yang sedang aktif SSH).
4. **Smart Server Tune-Up:**
   - *Auto-Tuning* (Profil *Stability* vs *Performance*).
   - *Advanced Tuning* (Kustomisasi nilai `MaxWorkers`, `Buffer Pool`, dab).
   - Deteksi otomatis MPM Apache (`prefork`, `event`, `worker`).
5. **Proxmox Cluster Check:** Pengecekan komprehensif untuk node Proxmox (Quorum, HA, ZFS/Ceph, SMART, VM Paused/Locked, Backup, Snapshot).
6. **Security & Maintenance:** Firewall management, ClamAV (default *Scan Only*), dan pembersihan log (*Clean All* vs *Deep Clean*).
7. **Farm Server Provisioning:** Setup *Simple Load Balancer* (HAProxy + Nginx + DB + NFS) dalam satu klik.

---

## 😻 Prasyarat (Dependencies)

Tool ini membutuhkan beberapa paket di *Controller** (mesin lokal). Jika belum terinstall, script akan **menawarkan instalasi otomatis** saat pertama kali dijalankan:
- `openssh-client`, `nmap`, `traceroute`, `dnsutils` (dig), `whois`, `jq`, `openssl`, `net-tools`.

---

## 🚀 Cara Instalasi

1. **Copy folder** `remoteserver` ke server Anda (misal di `/root/remoteserver`).
2. **Beri izin eksekusi:**
   ```bash
   chmod +x remoteserver.sh
   ```
3. **Buat file daftar server:**
   Buat file `listserver.txt` di dalam folder yang sama:
   ```text
   root@192.168.1.10
   admsys@10.0.0.5:2222
   203.119.13.20
   ```
4. **(Opsional) Buat file Proxmox:**
   Buat file `proxmox-nodes.txt` untuk fitur Menu 11:
   ```text
   root@203.119.13.4
   root@203.119.13.181
   ```

---

## 🔥 Cara Penggunaan

Jalankan script utama:
```bash
//remoteserver.sh
```

## Tampilan Menu Utama
```text
  REMOTE SERVER MANAGEMENT V2

--- SSH & Otentikasi ---
1. SSH Key Management
2. SSH SH& SERVER LIST MANAGEMENT
--- Server Operation ---
3. Server Maintainer
4. Server Information
5. User & Group Management
6. Security Management
7. Service Server Management
--- Network & Tools ---
8. Basic Tools
--- Advanced ---
9. Server Tune-Up
10. Instalasi Server Management
11. Proxmox Cluster Check
0. Exit
Pilih (0-11): 
```

---

## ♂️ Konfigurasi

Saat pertama kali djjalankan, script akan meminta **DEFAULT SSH USER**. 
Anda bisa mengubah konfigurasi lain dengan membuat file di `~/.remoteserver/config.conf`:

```bash
# ~/.remoteserver/config.conf
DEFAULT SSH USER="root"
DEFAULT SSH PORT="22"
PARALLEL="false"     # Ubah ke "true" untuk eksekusi paralel
MAX_PARALLEH=-carta-5    # Maksimal koneksi paralel (1-100)
DRY_RUN="false"      # Ubah ke "true" untuk simulasi (tidak mengeksekusi)
HEALTH_TIMEOUT="300"    # Timeout default untuk eksekusi paralel (detik)
```

## Mode DRy _RUN (Simulasi Aman)
Jika Anda ingin melihat perintah apa yang akan dieksekusi tanpa benar-benar :Ubah sistem:
```bash
DRY_RUN=true ./remoteserver.sh
```
*Script akan mengirim "Envelope Simulasi" ke server target. Perintah azli tidak akan dijalankan.*

---

## 📁 Struktur Direktori

```text
remoteserver/
├── remoteserver.sh           # Entry point utama
├── README.md                 # Dokumentasi ini
├── listserver.txt            # Daftar IP server target
├── proxmox-nodes.txt         # Daftar IP Proxmox (Menu 11)
├── config/
├──   └── remoteserver.conf.template
├── lib/
│   ├── core/                 # Logging, Config, Trap, Lock, Exec, Rollback
│   ├── ssh/                  # Parser, Connect, Retry, CopyID, Sync, Test
│   ├── exec/                 # Envelope & Worker (Parallel execution)
│   ├── server/               # List, Executor, Health
│   ├── menus/                # 00_main sd/ 11_proxmox
│   ├── remote_scripts/       # clamav_scan.sh, proxmox_check.sh
│   ├── validate/             # Validator IP, Port, Path, User
└── tests/                 # (Opsional) Unit test
```

---

## 📈 Lisensi

Gnu General Public License v3.0
Copyright (C) 2026 sigit-hdteamlabs <sigit.hdteam@gmail.com>
