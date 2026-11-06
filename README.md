# 🚀 Remote Server Management V2 (ManageServer) - (Fixing patching bug)

**Remote Server Management V2** adalah toolkit Bash modular yang dirancang untuk mengelola banyak server Linux secara terpusat via SSH. Dilengkapi dengan Smart Tuning, Auto-Dependency, Safety Guards, dan Audit Log.

## ✨ Fitur Utama
1. **SSH Key & Auth Management:** Generate otomatis 3 tipe key (Ed25519, ECDSA, RSA), sinkronisasi known_hosts, fallback legacy ssh-rsa.
2. **Multi-Server Execution:** Eksekusi paralel/berurutan dengan output terisolasi dan rekap status.
3. **User & Group Management:** Manajemen user/group dengan proteksi berlapis (tidak bisa hapus root atau akun aktif).
4. **Smart Server Tune-Up:** Auto-Tuning (Stability/Performance) & Advanced Tuning (Custom Workers/RAM), deteksi MPM Apache.
5. **Security & Maintenance:** Firewall, ClamAV (Scan only default), Clean All vs Deep Clean.
6. **Farm Server Provisioning:** Simple Load Balancer (HAProxy + Nginx + DB + NFS).

### ⚙️ Analisis Fungsi (Engine & Keamanan)

Di balik layar, script ini menggunakan arsitektur modular (bukan satu file besar):

· Central Execution Layer (CEL): Semua perintah ke server target wajib lewat fungsi cel_remote_exec (untuk modifikasi) atau cel_remote_read (untuk baca saja). Ini mencegah eksekusi liar dan memastikan audit log tercatat.
· Sudo Policy (NOPASSWD): Jika login SSH sebagai root, perintah jalan langsung. Jika login sebagai user biasa, script menggunakan sudo -n. Jika user tidak punya NOPASSWD, proses langsung berhenti (tidak menggantung minta password).
· Host Key Policy: Menggunakan StrictHostKeyChecking=accept-new. Host baru diterima otomatis, tapi jika fingerprint berubah (indikasi MITM), koneksi ditolak dengan peringatan merah.
· Legacy SSH Fallback: Otomatis mendeteksi jika server lama menolak algoritma modern, lalu beralih menggunakan ssh-rsa (SHA-1) secara aman.
· Validasi Ketat (Validators): Semua input user (IP, Port, Username, Path ClamAV) divalidasi sebelum dieksekusi untuk mencegah Command Injection.

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

### Konfigurasi (Opsional):
   Jika ingin mengubah default user atau menjalankan mode simulasi secara permanen, edit file ~/.remoteserver/config.conf(jika belum ada, buat file config.conf) isi berikut:
   
DEFAULT_SSH_USER="root"
DRY_RUN="false"      # Ubah ke "true" untuk simulasi (tidak mengeksekusi)
PARALLEL="false"     # Ubah ke "true" untuk eksekusi paralel

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



### Tree menu:
remoteserver.sh (Main Menu)
│
├── 1. SSH Key Management
│   ├── 1. Generate SSH Keys (ed25519 + ecdsa + rsa)
│   ├── 2. Import Public Keys (semua ke remote)
│   ├── 3. Test Passwordless SSH
│   ├── 4. Regenerate SSH Keys
│   ├── 5. Repair SSH Key (local permissions)
│   └── 6. Remove SSH Key (from remote)
│
├── 2. SSH & Server List Management
│   ├── 1. Lihat list server
│   ├── 2. Tambah server
│   ├── 3. Hapus server (cleanup auth lokal)
│   ├── 4. Verifikasi konsistensi (list vs known_hosts)
│   ├── 5. Remote SSH (manual)
│   └── 6. Remote SSH (from list)
│
├── 3. Server Maintainer
│   ├── 1. Check pending updates
│   ├── 2. Check pending reboot
│   ├── 3. Update package lists
│   ├── 4. Upgrade packages
│   ├── 5. Update & Upgrade
│   ├── 6. Repair package management
│   ├── 7. Clean All (SAFE)
│   ├── 8. Deep Clean (dry-run preview)
│   └── 9. Deep Clean (REAL)
│
├── 4. Server Information
│   ├── 1. Info Server (full system monitor)
│   ├── 2. Check disk structure (lsblk)
│   ├── 3. Check partition disk (fdisk)
│   ├── 4. Check UUID & filesystem (blkid)
│   └── 5. Health Check Dashboard
│
├── 5. User & Group Management
│   ├── 1. User List
│   ├── 2. Add user
│   ├── 3. Delete user (5 lapis proteksi)
│   ├── 4. Change password
│   ├── 5. Lock/Unlock user (termasuk Lock+Kill)
│   ├── 6. Group List
│   ├── 7. Add group
│   ├── 8. Delete group
│   ├── 9. Add user to group
│   └── 10. Remove user from group
│
├── 6. Security Management
│   ├── 1. Firewall status
│   ├── 2. Open ports
│   ├── 3. Failed logins
│   ├── 4. Firewall add rule (Allow/Drop/Reject)
│   └── 5. Virus scan (ClamAV)
│
├── 7. Service Server Management
│   ├── 1. Service status
│   ├── 2. Stop service
│   ├── 3. Start service
│   └── 4. Restart service
│
├── 8. Basic Tools
│   ├── 1. Ping subnet / IP / Range
│   ├── 2. Traceroute
│   ├── 3. dig
│   ├── 4. nslookup
│   ├── 5. whois
│   ├── 6. Prefix Whois (APNIC)
│   ├── 7. ASN Info
│   ├── 8. Check MX
│   ├── 9. Check PTR
│   ├── 10. Check SPF
│   └── 11. Check DMARC
│
├── 9. Server Tune-Up (Smart Tuning)
│   ├── 1. Apache Auto-Tune (Stability / Performance)
│   ├── 2. Nginx Auto-Tune (Stability / Performance)
│   ├── 3. Database Auto-Tune (Shared / Dedicated)
│   ├── 4. Apache Advanced Tune (Custom Workers)
│   ├── 5. Nginx Advanced Tune (Custom Workers/Conn)
│   ├── 6. Database Advanced Tune (Custom RAM MB)
│   └── 7. Database Repair/Optimize
│
├── 10. Instalasi Server Management
│   ├── 1. Install Apache + PHP
│   ├── 2. Install Nginx + PHP-FPM
│   ├── 3. Install MariaDB
│   ├── 4. Install PostgreSQL
│   ├── 5. Install LAMP
│   ├── 6. Install LEMP
│   ├── 7. Install LEPP
│   └── 8. Farm Server Management (Sub-menu)
│       ├── 1. True High Availability (HA)
│       └── 2. Simple Load Balancer
│
└── 0. Exit
