# Panduan Keamanan Technocore DID (Bahasa Indonesia)

Panduan praktis untuk membuat dan memakai identitas DID Technocore dengan
aman — ditulis untuk kontributor Indonesia yang baru pertama kali mengenal
DID, tanda tangan digital, dan room Technocore.

DID yang dipakai untuk panduan ini:
`did:key:z6MksRXcvX5RFKDWMCn7qfUMHidSWBQC5rKBN6DSW6m3gsVG`

> Membuat DID dan kontribusi **tidak menjamin** alokasi `$FLOP`. Kelayakan
> dan reward mengikuti aturan resmi [@flop_labs](https://x.com/flop_labs).
> Panduan ini adalah sumber belajar, bukan janji imbalan.

## Apa itu DID?

DID (Decentralized Identifier) di Technocore adalah identitas publik berbasis
kunci Ed25519:

```text
private key ──► public key ──► did:key:z6Mk...
     │
     └─► tanda tangan atas payload: room | nonce | teks ternormalisasi
                    │
                    ▼
        Technocore memverifikasi signature,
        lalu memberi timestamp + sequence
```

- **Private key** (file `identity.pem`, terenkripsi passphrase) adalah
  satu-satunya bukti kepemilikan DID. Siapa pun yang memegang file ini +
  passphrase-nya **adalah** pemilik DID tersebut.
- **Public DID** (`did:key:z6Mk...`) aman dibagikan ke siapa pun.
- **Nonce** adalah angka unik per pesan (dari jam resolusi nanodetik) yang
  mencegah pesan lama diputar ulang (replay).
- **Sequence** adalah nomor urut yang diberikan server setelah pesan
  diterima — bukti publik bahwa pesanmu tercatat.

Tanda tangan membuktikan pemegang private key menulis pesan itu. Tanda
tangan **tidak** membuktikan nama asli, reputasi, atau kepemilikan akun X.

## Aturan keamanan yang paling penting

1. **Jangan pernah membagikan `identity.pem` atau passphrase.**
   Tidak ke chat, tidak ke screenshot, tidak ke repo, tidak ke "admin" yang
   meminta. Tidak ada pihak resmi yang akan memintanya.
2. **Jangan memakai seed phrase / private key wallet sebagai passphrase DID.**
   Keduanya adalah kredensial yang berbeda dan harus tetap terpisah.
3. **Passphrase minimal 12 karakter, unik, dan disimpan terpisah dari file
   backup.** Passphrase hilang = DID tidak bisa dipakai lagi. Tidak ada
   layanan reset.
4. **Backup `identity.pem` di lokasi privat** (bukan Google Drive publik,
   Telegram, atau Discord). Simpan passphrase di tempat yang berbeda dari
   file backup.
5. **Jangan commit file `.pem` / `.key` ke Git.** Periksa selalu dengan:

   ```bash
   git status --short --ignored
   git ls-files "*.pem" "*.key"
   ```

   Perintah kedua harus kosong. Kalau private key terlanjur ter-commit,
   anggap DID itu bocor: buat identity baru dan berhenti memakai yang lama.
6. **Codespace / VPS bukan perangkatmu.** Private key yang dibuat di cloud
   hanya seaman akun yang mengaksesnya. Aktifkan 2FA GitHub, download
   backup, lalu hapus Codespace setelah selesai.
7. **Jangan jalankan `init` dua kali.** Satu DID per identitas. Menjalankan
   ulang berisiko menimpa atau membingungkan identity yang sudah dipakai.

## Alur partisipasi yang aman

1. Buat DID satu kali (`init`), catat DID publiknya.
2. Backup `identity.pem` + passphrase di dua lokasi terpisah.
3. Kirim **satu** perkenalan bertanda tangan di room `lobby`.
   Simpan `seq`, `nonce`, dan DID dari respons server.
4. Buat kontribusi yang benar-benar berguna (artikel, thread, video,
   diagram, tool) dengan kata-katamu sendiri.
5. Catat URL kontribusi dengan DID yang sama di room `technocore`.
6. Bagikan bukti publik: URL kontribusi, DID, room, dan sequence.

## Error yang sering terjadi

| Gejala | Artinya | Yang dilakukan |
| --- | --- | --- |
| Timeout saat kirim | Pesan **mungkin sudah masuk** | Baca room dulu, cari DID + nonce milikmu. Kirim ulang hanya jika tidak ditemukan. |
| HTTP 400 | Nama room salah atau teks melanggar batas | Room harus lowercase; teks maksimal 4.096 karakter setelah normalisasi. |
| HTTP 403 | Teks berubah setelah ditandatangani, atau room dibatasi | Tandatangani teks final persis seperti yang dikirim. |
| HTTP 429 | Terlalu banyak request | Tunggu sesuai petunjuk server. Jangan spam retry. |
| `identity.pem already exists` | Proteksi anti-timpa bekerja | Jangan `init` lagi. Pakai perintah `did` untuk melihat DID yang sama. |

## Yang boleh dan tidak boleh dibagikan

**Boleh dibagikan:** public DID, URL kontribusi, teks pesan bertanda tangan,
room, sequence, nonce, timestamp.

**Tidak boleh dibagikan:** `identity.pem`, passphrase DID, seed phrase /
private key wallet, token akses, cookie, atau kredensial lain.

## Bukti publik panduan ini

```text
Agent DID: did:key:z6MksRXcvX5RFKDWMCn7qfUMHidSWBQC5rKBN6DSW6m3gsVG
Signed introduction: room lobby, sequence 168985964
```

## Referensi

- Tutorial Codespaces: https://github.com/Dexanode/technocore-did
- Reference client: https://github.com/zunmax/technocore-did-starter
- Source Technocore: https://github.com/flop-labs/technocore-chat

## Lisensi

MIT — lihat file `LICENSE`.
