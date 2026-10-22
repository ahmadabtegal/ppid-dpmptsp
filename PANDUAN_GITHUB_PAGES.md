# PPIDK DPMPTSP Kota Tegal — GitHub Pages

Paket ini menyesuaikan frontend agar dapat di-host di GitHub Pages gratis dan tetap menggunakan Google Apps Script + Google Sheets sebagai backend/database.

## Isi paket
- `index.html` — frontend untuk GitHub Pages.
- `Code.gs` — backend Apps Script dengan API bridge POST.
- `Template_Database_PPIDK.xlsx` — template struktur database.
- `PANDUAN_GITHUB_PAGES.md` — panduan pemasangan.

## 1. Siapkan database
1. Buat Google Spreadsheet baru dari akun resmi instansi.
2. Impor `Template_Database_PPIDK.xlsx` ke Google Sheets (atau gunakan spreadsheet kosong).
3. Buka **Extensions > Apps Script** dari spreadsheet tersebut.
4. Ganti isi `Code.gs` dengan file `Code.gs` dari paket.
5. Jalankan `setupDatabase()` satu kali dan berikan izin yang diminta.
6. Jalankan `setInitialAdminPassword("PASSWORD-KUAT-ANDA")` dari editor dengan password unik minimal 14 karakter. Jangan menaruh password pada file HTML.

## 2. Deploy backend Apps Script
1. Klik **Deploy > New deployment**.
2. Pilih tipe **Web app**.
3. **Execute as:** Me.
4. **Who has access:** Anyone (agar frontend publik dapat mengakses API).
5. Deploy dan salin URL yang berakhiran `/exec`.
6. Jika mengubah kode backend setelah deploy, buat versi deployment baru atau edit deployment aktif agar memakai versi terbaru.

## 3. Hubungkan frontend
1. Buka `index.html`.
2. Cari `const API_URL = 'PASTE_APPS_SCRIPT_WEB_APP_URL_HERE';`
3. Ganti placeholder tersebut dengan URL Web App `/exec`, contoh format `https://script.google.com/macros/s/ID_DEPLOYMENT/exec`.
4. Simpan file.

## 4. Publikasikan ke GitHub Pages
1. Buat repository GitHub, misalnya `ppidk-dpmptsp-tegal`.
2. Unggah `index.html` ke root repository.
3. Buka **Settings > Pages**.
4. Pada **Build and deployment**, pilih **Deploy from a branch**.
5. Pilih branch `main` dan folder `/(root)`, lalu **Save**.
6. Tunggu sampai GitHub Pages memberikan URL, biasanya `https://USERNAME.github.io/ppidk-dpmptsp-tegal/`.

## 5. Pengujian wajib sebelum publikasi
- Buka website dari URL GitHub Pages, bukan file lokal.
- Pastikan daftar dokumen dan FAQ muncul.
- Uji pencarian dan filter kategori/tahun.
- Kirim permohonan uji dan pastikan nomor registrasi tampil.
- Lacak permohonan menggunakan nomor registrasi dan email yang sama.
- Uji pengajuan keberatan.
- Login admin, tambah/edit/hapus dokumen, dan ubah status permohonan.
- Periksa sheet `LogAktivitas` dan pastikan data pemohon hanya dapat dilihat petugas berwenang.

## Catatan teknis dan keamanan
- Frontend GitHub Pages bersifat publik. Jangan pernah memasukkan password admin, token permanen, atau kredensial Google ke dalam `index.html`.
- API bridge menggunakan POST ke Apps Script dan respons `postMessage` melalui iframe untuk menghindari penggunaan `google.script.run` pada hosting statis.
- Apps Script web app dan browser dapat berperilaku berbeda karena redirect dan pembatasan iframe. **Uji alur end-to-end setelah deploy**; jika bridge tidak berfungsi pada lingkungan akun/domain instansi, gunakan proxy backend tepercaya (misalnya Cloudflare Worker) daripada memindahkan data pribadi ke URL GET.
- Akses `Anyone` membuat endpoint dapat dipanggil publik. Sebelum produksi, tambahkan mitigasi spam/rate-limit yang sesuai, tinjau kebijakan privasi, masa simpan data, SOP, dan dasar hukum pemrosesan data.
- Data identitas pemohon jangan diminta kecuali benar-benar diperlukan oleh persyaratan resmi.
- Jangan mengunggah spreadsheet database yang berisi data pemohon ke repository GitHub.
- Isi kontak, SOP, batas waktu, dan dokumen resmi harus diverifikasi oleh pejabat berwenang sebelum portal digunakan.
