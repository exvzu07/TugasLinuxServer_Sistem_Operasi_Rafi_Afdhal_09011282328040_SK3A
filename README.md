# 09011282328040_Rafi_Afdhal_SK3A_TugasLinuxServer_Sistem_Operasi

   **Nama: Rafi Afdhal**
   
   **NIM: 09011282328040**
   
   **Kelas: SK3A**
   
   **Dosen Pengampu Mata Kuliah: 	Adi Hermansyah, S.Kom., M.T**


---

# Implementasi Akses Server Base OS dengan SSH
## 1. Defenisi
SSH (Secure Shell) merupakan protokol kriptografi yang memungkinkan akses, kontrol, dan modifikasi pada sistem server jarak jauh. Ini Merujuk pada rangkaian utilitas yang mengimplementasikan protokol SSH. Secure Shell menyediakan autentikasi kata sandi yang kuat dan autentikasi kunci publik, serta komunikasi data terenkripsi antara dua komputer yang terhubung melalui jaringan terbuka, seperti internet. SSH penting untuk menjaga keamanan sistem, karena protokol ini bertindak sebagai cara yang aman untuk menyediakan akses dan manajemen sistem jaringan. Cara Protokol ssh bekerja dapat dilihat pada [link ini](https://www.ipxo.com/app/uploads/2022/02/How-does-SSH-work.jpg) 

## 2. Alat dan Bahan 
### 2.1 Host Side Operation System

Untuk host side yang dibutuhkann merupakan sebuah operation system berbasis server, dalam hal ini saya menggunakan Ubuntu LTS server yang dapat diunduh di [Link Ubuntu LTS](https://ubuntu.com/download/server).
Diperlukan penginstalan secara manual jika menggunakan server ini. User dapat mengikuti konfigurasi seperti berikut :
![Screenshot (958) copy](https://github.com/user-attachments/assets/037d17c6-7bf5-4470-9fce-f7d0f408cc6e)

#### Selanjutnya, lakukan konfigurasi yang terdapat pada linux server tersebut. Jika mengalami Error seperti ini : 
![Screenshot (962) copy](https://github.com/user-attachments/assets/fbc9fafd-adf6-4f82-963d-3b3ef83cd234)

#### Maka, user diharuskan memasukkan file .iso yang telah diunduh sebelumnya

![Screenshot (961) copy](https://github.com/user-attachments/assets/d1d0a3b5-0ca0-4a8b-b7ae-fb5bcf999edf)

#### Setelah melakukan konfigurasi, User akan perlu melakukan restart sebelum dapat menjalankan Linux servernya. Setelah melakukan restart dan login kembali, user akan mendapati tampilan linux servernya sebagai berikut :
![Screenshot 2024-10-31 000116](https://github.com/user-attachments/assets/e596b566-e800-445e-b25a-1b33ca3fd1dd)

### 2.2 Client Side Operation System
  
Untuk client side, User dapat memilih Sistem Operasi yang hendak digunakan. Karena untuk melakukan koneski yang paling dibutuhkan adalah kemampuan dari operasi sistem untuk melakukan koneksi ke server dengan protokol ssh. Dalam hal ini, saya menggunakan Sistem Operasi Linux Mint (Cinnamon Edition) untuk melakukan koneksi ke Server, yang dapat diunduh dengan mengeklik [Link Linux Mint](https://www.linuxmint.com/edition.php?id=316). User dapat menggunakan [Tools Open-SSH](https://ubuntu.com/server/docs/openssh-server) untuk melakukan koneksi tampilan awal dari openssh seperti berikut:

![Screenshot 2024-10-31 000823](https://github.com/user-attachments/assets/ad9348fe-9cf0-4251-8951-c9ce6e1db974)


## 3. Cara Kerja
### 3.1 Koneksi default port
#### 3.1.1 Konfigurasi awal

Setelah melakukan inisialiasi pada proses sebelumnya, User perlu menginstall tools yang diperlukan sebelum menjalankan perintah check ip address
```
$ sudo apt install net-tools
```
![Screenshot 2024-10-31 000711](https://github.com/user-attachments/assets/d9c2808d-5c4e-4660-9cae-02f7aa839a9d)

Setelah menginstall Tools yang diperlukan, User dapat melakukan check ip address pada host server untuk melakukan koneksi dengan memasukkan perintah berikut 
```
$ ifconfig
```
![Screenshot 2024-10-30 222109](https://github.com/user-attachments/assets/5c212fec-9036-4411-b967-6ed98280ac1f)

Pada Linux Server nya, aktifkan ssh nya dengan perintah berikut :
```
$ sudo systemctl enable ssh
```
![Screenshot 2024-10-31 000601](https://github.com/user-attachments/assets/2b61fdcb-5808-4821-8967-e0dd09ffeb9e)

## NOTABENE #1 :
Pastikan Konfigurasi terhubung ke Bridge Adapter (Jika Menggunakan VirtualBox)
![Screenshot 2024-10-31 004326](https://github.com/user-attachments/assets/d31c15db-c0f1-444f-afd8-30e357c8053f)

#### Pada Linux Mint nya, Install Tools yang diperlukan :
```
$ sudo apt install nmap
```

#### 3.1.2 Memeriksa open port pada host server
Untuk memeriksa alamat port yang terbuka pada host server, pada client side, User dapat melakukan *Port Scanning* menggunakan tools [nmap](https://nmap.org/). Lalu dengan memasukkan perintah berikut ini
```
$ nmap <alamat ip Host Server>
```
![Screenshot 2024-10-30 222447](https://github.com/user-attachments/assets/c6d90898-25bd-4cf3-80da-faf673b2ea07)

#### 3.1.3 Melakukan koneksi pada default port untuk protokol ssh
Untuk melakukan koneksi ke host server pada default port (22) dengan menggunakan protokol ssh, User dapat menggunakan perintah berikut ini
```
$ ssh -p 22 <usernameHost@alamatiphost>
```
![Screenshot 2024-10-30 222724](https://github.com/user-attachments/assets/4689f595-a0b7-41a8-9936-48186888f0e2)

### 3.2 Konfigurasi untuk port 40 pada host server
Masuk ke mode superuser pada Linux Server agar user dapat leluasa melakukan modifikasi, bisa dilakukan dengan perintah berikut ini:
```
$ sudo su
```
![Screenshot 2024-10-30 223005](https://github.com/user-attachments/assets/0708bd95-d22d-46d1-a174-4b6f771b7b0a)

#### 3.2.1 Modifikasi file *sshd_config* 
Sebelum melakukan konfigurasi maka kita harus melakukan file backup agar file sistem utama tidak corrupt jika terjadi hal yang tidak diinginkan dengan memasukkan perintah berikut ini:
```
$ cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```
setelah melakukan backup, maka kita akan melakukan modifikasi pada file *sshd_config* dengan memasukkan perintah berikut ini:
```
$ nano /etc/ssh/sshd_config
```
![Screenshot 2024-10-30 224710](https://github.com/user-attachments/assets/865a70b0-4242-489c-b253-a5a3d561c0c7)

setelah membuka file *sshd_config* menggunakan perintah nano ubah konfigurasi dengan beberapa hal ini dengan mengubah baris yang terdapat unsur port 22:
```
Protocol 2
Port 40
```
![Screenshot 2024-10-30 224734](https://github.com/user-attachments/assets/d8864206-5ac3-4167-9bd3-f23904a5ed65)

setelah melakukan modifikasi maka perubahan pada file harus disimpan dengan memasukkan kombinasi keyboard *ctrl+x* dan masukkan opsi *Yes*

#### 3.2.2 Menerapkan perubahan pada port
Untuk Mengubah konfigurasi server SSH dan Memeriksa kebenaran konfigurasi yang dilakukan pada file *sshd_config* sebelumnya, perlu melakukan perintah ini:
```
$ sed -i 's/#Port22/Port40/' /etc/ssh/sshd_config
$ sshd -t
```
![Screenshot 2024-10-30 225927](https://github.com/user-attachments/assets/93a5a711-c5ea-4430-8d6a-86488b3d9f59)

#### 3.2.3 Menerapkan perubahan pada ufw
Untuk konfigurasi firewall Uncomplicated Firewall (UFW) pada sistem Linux, User perlu memblokir lalu lintas SSH pada port 22 dan hanya mengizinkan lalu lintas pada port 40 dengan perintah berikut :
```
$ ufw allow 40/tcp
$ ufw deny 22/tcp
$ ufw enable
```
dan untuk mengecek status ufw dapat menggunakan perintah berikut ini:
```
$ ufw status
```
![Screenshot 2024-10-30 230231](https://github.com/user-attachments/assets/8a408eaf-740d-43d2-a35e-0c7ca76aa98c)

Sebelum lanjut, User dapat melakukan reboot terlebih dahulu sebelum kembali menyalakan ssh :
```
$ reboot
```

#### 3.2.4 Memeriksa Sshd yang telah diset
User dapat menyalakan ssh terlebih dahulu sebelum masuk ke sshd dengan perintah berikut :
```
$ systemctl enable ssh
```
![Screenshot 2024-10-30 234605](https://github.com/user-attachments/assets/ccad01e9-9ea6-4a99-86e7-ee57f9f3e7ca)

User perlu melakukan restart sshd dan memeriksa ulang status sshd untuk memastikan apakah port sudah berubah atau belum :
```
$ systemctl restart sshd
$ systemctl status sshd
```
![Screenshot 2024-10-30 234730](https://github.com/user-attachments/assets/de8e82cf-3400-4b7f-bfac-d4a770d0e0ba)

## NOTABENE #2 :
User akan mendapatkan pemberitahuan bahwa koneksi terhadap server telah terputus pada Terminal Linux Mint nya
![Screenshot 2024-10-30 234831](https://github.com/user-attachments/assets/b4de85c8-fae9-447e-962d-f1f603699b8b)

### 3.3 Koneksi ke port 40
Setelah konfigurasi port selesai, kita bisa melakukan koneksi ke port 40 seperti sebelumnya dengan perintah :
```
$ ssh -p 40 <usernameHost@alamatiphost>
```
![Screenshot 2024-10-30 235133](https://github.com/user-attachments/assets/5bf647e1-252e-4ffd-a2eb-47039b2374b8)


