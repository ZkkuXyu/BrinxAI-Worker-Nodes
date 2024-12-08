### Langkah 1: Persiapkan Hardware
Pastikan hardware Anda memenuhi persyaratan minimum:
- **CPU**: Minimal 8 VCPU Cores, direkomendasikan 16+ VCPU Cores.
- **RAM**: Minimal 16GB, direkomendasikan 64GB atau lebih.
- **Storage**: Minimal 300GB SSD, direkomendasikan 1TB SSD atau lebih.
- **GPU**: Opsional, tapi GPU seperti NVIDIA RTX 2060 atau AMD Radeon RX 5600 XT dapat meningkatkan performa dan penghasilan.
- **Port**: Port 5011 perlu terbuka.

### Langkah 2: Instalasi Software
1. **Instal `python3-venv`**:
   ```sh
   sudo apt install python3-venv
   ```
2. **Instal Docker** (Jika belum terinstal):
   ```sh
   sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
   ```

### Langkah 3: Pull Docker Image BrinxAI
Pull Docker image BrinxAI Worker Node:
```sh
docker pull admier/brinxai_nodes-worker:latest
```

### Langkah 4: Jalankan Script Installasi
Clone repository BrinxAI dan jalankan script installasi:
```sh
git clone https://github.com/admier1/BrinxAI-Worker-Nodes
cd BrinxAI-Worker-Nodes
chmod +x install_ubuntu.sh
./install_ubuntu.sh
```

### Langkah 5: Konfigurasi Jaringan
1. **Buka Port 5011**:
   ```sh
   sudo ufw allow 5011/tcp
   sudo ufw enable
   sudo ufw status
   ```
2. **Aktifkan Docker**:
   ```sh
   sudo docker run -d --name brinxai_worker --cap-add=NET_ADMIN admier/brinxai_nodes-worker:latest
   ```

### Langkah 6: Register Node Anda
1. Kunjungi situs BrinxAI Worker Registration.
2. Buat akun dan masuk.
3. Klik "Add Worker Node" dan masukkan nama node dan alamat IP Anda.

## Terdapat tulisan peringatan 

apabila ada tulisan warning seperti ini ketika memasukan kode ini ./install_ubuntu.sh

WARN[0000] /root/BrinxAI-Worker-Nodes/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 

Peringatan ini menunjukkan bahwa atribut `version` dalam file `docker-compose.yml` sudah usang dan akan diabaikan. Ini adalah peringatan yang tidak mempengaruhi jalannya instalasi atau operasional node, namun sebaiknya diatasi untuk mencegah kebingungan

## Cara untuk memperbaikinya:

### Langkah 1: Edit File `docker-compose.yml`
1. Buka file `docker-compose.yml` di editor teks favorit Anda. Jika Anda berada di dalam MobaXterm, Anda bisa menggunakan editor teks seperti `nano` atau `vi`.
   ```sh
   nano /root/BrinxAI-Worker-Nodes/docker-compose.yml
   ```

### Langkah 2: Hapus Atribut `version`
2. Cari atribut `version` di bagian atas file dan hapus baris tersebut. Atribut ini biasanya terlihat seperti ini:
   ```yaml
   version: '3.8'
   ```
   Hapus baris tersebut, sehingga file `docker-compose.yml` Anda tidak lagi memiliki atribut `version`.

Sebelum:

yaml
version: '3.8'
services:
  brinxai_worker:
    image: admier/brinxai_nodes-worker:latest
    ...

Sesudah:

yaml
services:
  brinxai_worker:
    image: admier/brinxai_nodes-worker:latest
    ...
    
### Langkah 3: Simpan Perubahan dan Keluar
3. Simpan perubahan yang Anda buat dan keluar dari editor teks. Di `nano`, Anda bisa melakukan ini dengan menekan `Ctrl+O` untuk menyimpan dan `Ctrl+X` untuk keluar.

### Langkah 4: Jalankan Ulang Script Instalasi
4. Jalankan ulang script instalasi untuk memastikan semua berjalan lancar tanpa peringatan:
   ```sh
   ./install_ubuntu.sh
   ```
5. **Aktifkan Docker**:
   Aktifkan docker kembali
   ```sh
   sudo docker run -d --name brinxai_worker --cap-add=NET_ADMIN admier/brinxai_nodes-worker:latest
   ```
## Ketika sudah login dan port ip anda dockernya tidak di temukan yang bertuliskan seperti ini anda bisa memasukan kode ini 
Inactive No containers available or invalid data.
Pesan ini mengindikasikan bahwa BrinxAI tidak dapat mendeteksi container yang berjalan pada alamat IP yang Anda tambahkan. Berikut beberapa langkah yang bisa Anda lakukan untuk menyelesaikan masalah ini:

### Langkah 1: Verifikasi Status Container
Pastikan container BrinxAI Anda berjalan dan dapat diakses. Jalankan perintah berikut untuk memeriksa status container:
```sh
docker ps
```
Pastikan container yang diperlukan (`brinxai_relay` atau yang lainnya) berada dalam status "Up".

### Langkah 2: Periksa Konfigurasi IP
Pastikan IP address yang Anda tambahkan ke BrinxAI adalah benar dan dapat diakses. Anda bisa menggunakan perintah berikut untuk memeriksa IP address dari container:
```sh
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' brinxai_relay
```

### Langkah 3: Pastikan Port Terbuka
Pastikan port yang dibutuhkan oleh BrinxAI terbuka dan tidak diblokir oleh firewall. Jalankan perintah berikut untuk membuka port yang dibutuhkan:
```sh
sudo ufw allow 5011/tcp
sudo ufw enable
sudo ufw status
```

### Langkah 4: Periksa Log Container
Periksa log container untuk memastikan tidak ada error atau masalah lain. Jalankan perintah berikut untuk melihat log container:
```sh
docker logs brinxai_relay
```

### Langkah 5: Restart Container
Kadang-kadang, merestart container dapat membantu menyelesaikan masalah. Jalankan perintah berikut untuk merestart container:
```sh
docker restart brinxai_relay
```
Refresh web workers.brinxai.com
