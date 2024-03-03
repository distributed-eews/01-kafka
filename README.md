# Deploy Kafka dengan Docker Compose

## Overview

Apache Kafka adalah sistem messaging yang terdistribusi dan dirancang untuk menyediakan throughput tinggi, penyimpanan persisten, dan latensi rendah. Kafka sering digunakan untuk membangun solusi streaming data yang mampu menangani volume data yang sangat besar.

## Requirements

- VM dengan Docker dan Docker Compose terinstal.
- VM harus memiliki setidaknya 1.2GB RAM untuk Kafka, ditambah dengan kebutuhan memori untuk Zookeeper.
- Firewall pada VM harus mengizinkan traffic ingress pada port `29092` untuk memungkinkan komunikasi dengan Kafka.

## Deployment Steps

1. **Persiapan VM**

   - Pastikan Docker dan Docker Compose sudah terinstal pada VM.
   - Buka port `29092` pada firewall VM untuk memungkinkan akses ke Kafka.

2. **Konfigurasi Kafka**

   - Sesuaikan nilai `KAFKA_ZOOKEEPER_CONNECT` dengan IP dan port Zookeeper yang sesuai.
   - Sesuaikan `KAFKA_ADVERTISED_LISTENERS` dengan IP VM Anda agar Kafka bisa diakses dari luar.
     - Ganti `<ip-vm>` dengan IP publik dari VM Anda.
   - Pastikan nilai `KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR` tidak melebihi jumlah broker yang akan dideploy.

3. **Menjalankan Kafka**

   - Navigasikan ke direktori tempat file `docker-compose.yaml` berada.
   - Jalankan perintah berikut untuk mendeploy Kafka dan Zookeeper:

     ```sh
     docker-compose up -d
     ```

   - Periksa status container untuk memastikan Kafka berjalan dengan baik:

     ```sh
     docker-compose ps
     ```

## Mendeploy Lebih dari 1 Kafka Broker

Untuk mendeploy lebih dari satu Kafka broker menggunakan Docker Compose, Anda perlu menambahkan lebih banyak service dalam `docker-compose.yaml` dengan konfigurasi yang mirip namun dengan `KAFKA_BROKER_ID` dan port yang unik untuk setiap broker.

- Duplikasi konfigurasi service `kafka1` untuk setiap broker baru.
- Untuk setiap broker baru, ubah `container_name`, `KAFKA_BROKER_ID`, dan sesuaikan port mapping.

Pastikan untuk mengupdate firewall dan konfigurasi jaringan VM sesuai dengan port yang digunakan oleh setiap Kafka broker untuk memastikan komunikasi yang lancar.

## Catatan

- Pastikan konfigurasi pada `KAFKA_ADVERTISED_LISTENERS` tidak ada yang sama di broker lainnya.
  - Misal di broker 1 terdapat value `DOCKER://kafka1:9092` maka tidak boleh ada lagi value tersebut di broker lainnya.
