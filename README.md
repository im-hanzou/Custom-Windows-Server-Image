
## Langkah-langkah Instalasi | [Tutorial Video](https://youtu.be/HS_J5RUBbSw) (Indonesian)

### 1. Unduh dan Instal File Installer
Jalankan perintah berikut untuk mengunduh installer:

```bash
wget https://raw.githubusercontent.com/emuhib/windows.server.DO/main/windows-server-autoinstaller.sh
```

### 2. Berikan Izin Eksekusi pada File
Setelah diunduh, berikan izin agar file dapat dijalankan:

```bash
chmod +x windows-server-autoinstaller.sh
```

### 3. Jalankan Installer
Jalankan installer dengan perintah berikut:

```bash
./windows-server-autoinstaller.sh
```

### 4. Jalankan QEMU
Setelah installer selesai, jalankan QEMU untuk memulai Windows Server. Ganti `xx` dengan versi Windows yang Anda pilih (misal, `windows10`):

```bash
qemu-system-x86_64 \
-m 4G \
-cpu host \
-enable-kvm \
-boot order=d \
-drive file=windowsxx.iso,media=cdrom \
-drive file=windowsxx.img,format=raw,if=virtio \
-drive file=virtio-win.iso,media=cdrom \
-device usb-ehci,id=usb,bus=pci.0,addr=0x4 \
-device usb-tablet \
-vnc :0
```

**Catatan: Tekan Enter dua kali untuk melanjutkan.**

### 5. Akses via VNC
Setelah QEMU berjalan, ikuti langkah berikut untuk mengakses dan mengonfigurasi Windows Server:

1. Aktifkan **Remote Desktop** di pengaturan Windows Server.
2. Nonaktifkan **CTRL+ALT+DEL** di Local Security.
3. Atur agar Windows Server **tidak pernah tidur**.

### 6. Kompres File Windows Server
Setelah konfigurasi selesai, kompres image Windows Server. Ganti `xxxx` dengan versi Windows yang Anda pilih (misal, `windows10`):

```bash
dd if=windowsxxxx.img | gzip -c > windowsxxxx.gz
```

### 7. Instal Apache
Instal Apache untuk melayani file melalui web:

```bash
apt install apache2
```

### 8. Berikan Akses Firewall untuk Apache
Izinkan akses Apache melalui firewall:

```bash
sudo ufw allow 'Apache'
```

### 9. Pindahkan File Windows Server ke Lokasi Web
Salin file Windows Server yang sudah dikompres ke direktori web Apache:

```bash
cp windowsxxxx.gz /var/www/html/
```

### 10. Link Download
Setelah file dipindahkan, akses file tersebut melalui alamat IP droplet Anda:

```
http://[IP_Droplet]/windowsxxxx.gz
```

**Contoh:**
```
http://188.166.190.241/windows10.gz
```

## Menjalankan Windows Server di Droplet Baru

Untuk menjalankan Windows Server di droplet baru, gunakan perintah berikut. Ganti `LINK` dengan link unduhan file yang sudah dikompres sebelumnya:

```bash
wget -O- --no-check-certificate LINK | gunzip | dd of=/dev/vda
```

### Catatan Penting:
- Pastikan Anda mengganti placeholder `xxxx` dengan versi Windows yang benar.
- Jangan lupa untuk mengganti `LINK` dengan URL file Anda yang sebenarnya.
