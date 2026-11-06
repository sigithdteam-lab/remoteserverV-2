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

