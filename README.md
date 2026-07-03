# Multi-Site Network Automation Engine (Python)

Repositori ini berisi skrip otomasi infrastruktur jaringan untuk mengonfigurasi perangkat router di dua kantor cabang (Bekasi dan Depok) secara serentak[cite: 1]. Skrip ini mengotomatiskan pembuatan Multi-VLAN, pembagian DHCP Server, hingga penanganan konflik alamat IP (*Overlapping IP*) lintas jalur VPN[cite: 1].

Proyek ini dibangun secara independen menggunakan metode **Vibe Coding** (proses pengembangan cepat memanfaatkan AI Code Generation) yang dimatangkan melalui serangkaian eksperimen *trial and error* langsung pada simulator jaringan[cite: 1].

---

## 📂 Struktur Berkas

*   **`main.py`**: Skrip utama untuk menyuntikkan seluruh parameter konfigurasi ke router target secara remote via SSH[cite: 1].
*   **`rollback.py`**: Skrip pembersihan otomatis untuk menghapus seluruh konfigurasi yang telah dibuat, mengembalikan router ke kondisi awal (*clean state*).
*   **`Tabel_Excel_A.xlsx`**: File database spreadsheet yang menyimpan parameter teknis jaringan sebagai *source of truth*[cite: 1].
*   **`Dokumentasi_Project.pdf`**: *Slide deck* presentasi yang memuat topologi arsitektur, dokumentasi kode, serta bukti uji konektivitas *Before vs After*[cite: 1].

---

## ⚙️ Alur Kerja Skrip

1.  **Pemisahan Data & Logika**: Skrip `main.py` membaca variabel teknis secara otomatis dari berkas `Tabel_Excel_A.xlsx`[cite: 1].
2.  **Eksekusi Sekuensial**: Python membuka jalur SSH terenkripsi ke setiap router, lalu mengeksekusi perintah pembuatan *bridge*, sub-interface VLAN, alokasi IP, dan aktivasi DHCP secara berurutan[cite: 1].
3.  **Penanganan Overlapping**: Skrip secara otomatis menyuntikkan aturan *Firewall Netmap NAT* untuk menjembatani komunikasi antar cabang yang memiliki subnet IP lokal kembar[cite: 1].
4.  **Fitur Rollback**: Jika diperlukan pengujian ulang, skrip `rollback.py` dapat dijalankan untuk menghapus seluruh *interface* dan *rule* yang telah terpasang tanpa perlu meriset router manual.

---

## 🚀 Panduan Penggunaan

### 1. Instalasi Dependensi 
Pastikan Python telah terpasang, kemudian instal pustaka pendukung via terminal:
```
pip install netmiko pandas openpyxl
```
### 2. Penerapan Konfigurasi (Deploy)
Jalankan perintah berikut untuk memulai injeksi konfigurasi otomatis:
```
python main.py
```
### 3. Pembatalan Konfigurasi (Rollback)
Jalankan perintah berikut jika ingin membersihkan kembali seluruh pengaturan pada perangkat:
```
python rollback.py
```
Pastikan Python telah terpasang, kemudian instal pustaka pendukung via terminal:
```bash
pip install netmiko pandas openpyxl
