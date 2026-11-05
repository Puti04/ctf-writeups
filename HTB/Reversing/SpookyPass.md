Nama CTF: SpookyPass

Difficulty: Very Easy

Deskripsi: File zip terenkripsi yang berisi ELF berupa pass. Perlu mencari password tersembunyi di dalamnya untuk menemukan flag

Step by Step: 

1. Menganalisis nama file zip dengan format UUID 8-4-4-4-12, sempat mengaitkan dengan challenge tapi setelah di cek version yaitu v4=random, disimpulkan ini hanya identifier acak  
2. Cek izi zip dengan unzip \-l ketemu struktur rev\_spookypass yang kosong 0 byte dan tentu tidak terenkripsi lalu rev\_spooky/pass berukuran 15.000 KB, terenkripsi. Folder dan file terenkripsi independen  
3. Ek metode enkripsi dengan 7z l \-slt ketemu ZipCrypto dimana secara teori vulnerable ke known-plaintext attack, sedangkan AES nyaris gak bisa dibobol tanpa password asli  
4. Crack pakai zip2john/fcrackzip \+ rockyou.txt, gagal  
5. Cek 2 WU publik tidak ada yang membahas password file zip, mengindikasi password default platform, bukan password yang perlu dicrack  
6. Pakai hackthebox, berhasil  
7. Kasih izin eksekusi menggunakan chmod \+x, strings pas \| grep \-iE “pass\|secret\|s3cr3t”, ketemu s3cr3t\_p455\_f0r\_gh05t5\_4nd\_gh0ul5  
8. Jalankan ./pass masukkan password tersebut lalu ketemu flag HTB{un0bfu5c4t3d\_5tr1ng5}

Kendala: Instalasi zip2john versi jumbo bermasalah sehingga ketika menggunakan which tidak muncul meski apt install john sukses

Kesalahan Saya: 

1. Berasumsi nama file UUID bisa jadi kunci password tanpa validasi bit version dulu  
2. Tidak mencoba password default yaitu hackthebox sehingga memakan waktu dan resource

Real Life: Banyak developer lupa menghapus password/API dimana ditulis langsung di dalam kode yang sebelumnya untuk testing terus di compile menjadi file executable. Teknik strings untuk menemukan informasi penting tersebut untuk menemukan kebocoran untuk mengecek aplikasi atau menganalisa malware (sebelum buka di sandbox, intip file malware untuk mengetahui alamat server buat C2, nama file/path yang malware target atau tulis di komputer, pesan error yang bisa membocorkan kinerja malware dan info penting lainnya)

Konsep dan Relevansi Teori:UUID v1 ada timestamp+MAC sedangkan UUID v4 hanya angka acak murni. Di file zip folder maupun file bisa terenkripsi masing-masing tanpa berkaitan. ZipCrypto dan AES adalah metode mengenkripsi zip dimana ZipCrypto gampang dibobol, tidak semudah AES, penting cek metode enkripsi untuk men-crack. Strings dicoba duluan karena cara paling gampang sebelum ke tahap selanjutnya (disassembler dimana executable → assembly), disebut least effort attack path. Bak reconnaissance sebelum main serang buta. 

Framework / Standard Terkait: CWE-798: Use of Hard-coded Credentials

Kata Kunci: UUID v4, ZipCrypto, strings, ELF binary, static analysis, hardcoded credential, default credential

Tools yang Digunakan: zipinfo/7z l \-slt, zip2john, fcrackzip, unzip, strings, chmod, file

Pelajaran Terpenting: Cek paling hal dasar sebelum ke tahap selanjutnya, akibatnya membuang banyak waktu dan resource. Static analysis sering menyelesaikan challenge "easy" tanpa perlu disassembler. Kalau satu WU kelihatan gak lengkap, cek sumber lain dulu. 

Referensi Belajar Lanjutan: Materi dasar ZIP file format & known-plaintext attack (bkcrack) untuk kasus ZipCrypto yang password-nya gak default; dasar Ghidra/IDA untuk kasus rev yang lebih kompleks (password gak muncul plain di strings); CWE database untuk kategori hardcoded credential.  
