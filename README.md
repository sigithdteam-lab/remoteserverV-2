# 🚀 Remote Server Management V2 (ManageServer)

**Remote Server Management V2** adalah toolkit Bash modular yang dirancang untuk mengelola banyak server Linux secara terpusat via SSH. Dilengkapi dengan Smart Tuning, Auto-Dependency, Safety Guards, dan Audit Log.

## ✨ Fitur Utama
1. **SSH Key & Auth Management:** Generate otomatis 3 tipe key (Ed25519, ECDSA, RSA), sinkronisasi known_hosts, fallback legacy ssh-rsa.
2. **Multi-Server Execution:** Eksekusi paralel/berurutan dengan output terisolasi dan rekap status.
3. **User & Group Management:** Manajemen user/group dengan proteksi berlapis (tidak bisa hapus root atau akun aktif).
4. **Smart Server Tune-Up:** Auto-Tuning (Stability/Performance) & Advanced Tuning (Custom Workers/RAM), deteksi MPM Apache.
5. **Security & Maintenance:** Firewall, ClamAV (Scan only default), Clean All vs Deep Clean.
6. **Farm Server Provisioning:** Simple Load Balancer (HAProxy + Nginx + DB + NFS).

## 🛠️ Prasyarat (Dependencies)
Tool ini membutuhkan paket di Controller. Jika belum terinstall, script akan menawarkan instalasi otomatis:
- openssh-client, nmap, traceroute, dnsutils, whois, jq, openssl, net-tools.

## 🚀 Cara Instalasi
1. Copy folder remoteserver ke server (misal /root/remoteserver).
2. Beri izin eksekusi: `chmod +x remoteserver.sh`
3. Buat file listserver.txt berisi daftar IP:
   Contoh:
   root@192.168.1.10
   admsys@10.0.0.5:2222
   203.119.13.20

## 🖥️ Cara Penggunaan
Jalankan script utama:
`./remoteserver.sh`

### Tampilan Menu Utama
```text
═══════════════════════════════════════════════════════════
  REMOTE SERVER MANAGEMENT V2
═══════════════════════════════════════════════════════════
--- SSH & Otentikasi ---
1. SSH Key Management
2. SSH & Server List Management

--- Server Operation ---
3. Server Maintainer
4. Server Information
5. User & Group Management
6. Security Management
7. Service Server Management

--- Network & Tools ---
8. Basic Tools

--- Advanced ---
9.  Server Tune-Up
10. Instalasi Server Management

0. Exit
═══════════════════════════════════════════════════════════
Pilih (0-10):

### Konfigurasi (Opsional):
   Jika ingin mengubah default user atau menjalankan mode simulasi secara permanen, edit file ~/.remoteserver/config.conf:
DEFAULT_SSH_USER="root"
DRY_RUN="false"      # Ubah ke "true" untuk simulasi (tidak mengeksekusi)
PARALLEL="false"     # Ubah ke "true" untuk eksekusi paralel
