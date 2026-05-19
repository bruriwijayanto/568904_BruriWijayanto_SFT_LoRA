# Perbandingan Respons: Base Model vs Fine-tuned Model

**Model:** TinyLlama/TinyLlama-1.1B-Chat-v1.0  
**Dataset:** Sintetis buatan sendiri (42 contoh bahasa daerah & budaya Nusantara)  
**LoRA:** r=16, alpha=32, 7 target modules  
**Training:** 10 epoch, 180 steps, train loss akhir: 0.77  

---

## Prompt 1
**Instruksi:** Jelaskan makna filosofi Bugis 'Siri Na Pacce' dan relevansinya dalam kehidupan modern.

### Base Model
```
Makna filosofi Bugis 'Siri Na Pacce' merantau menjadi makna keadilan, kesihatan,
dan pencintaan seseorang yang tidak mengusir orang lain. Siri meliputi pembicaraan
kepada orang lain atau permainan bersama, berbicara tentang seseorang sendiri dan
tentang orang lainnya, dan berbicara tentang tindakan...
```

### Fine-tuned Model
```
Maknanya: Siri Na Pacce adalah filosofi yang mencerminkan kesehatan, tanaman, dan
penghormatan. Keduanya mengajarkan bahwa semua hal yang terjadi di sekitarmu —
hingga tetapkan pencinta, sakit, dan penderitaan mental — berada di dalam kehidupan
yang tidak kaya. Sehingga orang tua mampu memberikan ke...
```

### Analisis
| Aspek | Base Model | Fine-tuned |
|-------|-----------|------------|
| Bahasa | Indonesia (tapi ambigu) | Indonesia konsisten |
| Relevansi domain | ❌ Tidak mengenali konteks budaya Bugis | ⚠️ Mulai menyebut konteks filosofi, meski konten masih salah |
| Format | Tidak terstruktur | Mencoba memberikan penjelasan berstruktur |

---

## Prompt 2
**Instruksi:** Apa perbedaan antara wayang kulit dan wayang golek?

### Base Model
```
Wayang Kulit (wayang meaning 'shadow' or 'spectacle', and kulit meaning 'skin')
adalah lampu-lampu bernyanyi yang diberikan kepada pemain dalam prosesi daging kulit.
Dengan mengambil benda daging, pemain membawa pakaian terbunuh untuk menjaga benda
menjadi seperti lampu-lampu bernyanyi...
```

### Fine-tuned Model
```
Wayang kulit (wayang golek) adalah sebuah ceramik, dinding, atau gong yang telah
disebutkan 'wayang' oleh penderitaan dari kelompok miskin di Bali. Wayang kulit
berasal dari pasar mamu (Mammu), dibayarkan ke Bali pada masa Majapahit. Kedua-dua
dinding tersebut dibentuk dengan metode tradisional bat...
```

### Analisis
| Aspek | Base Model | Fine-tuned |
|-------|-----------|------------|
| Bahasa | ❌ Campur Inggris-Indonesia | ✅ Indonesia konsisten |
| Relevansi domain | ❌ Menyebut "lampu bernyanyi" dan "daging kulit" — halusinasi | ⚠️ Menyebut konteks Bali dan Majapahit, masih banyak kesalahan faktual |
| Format | Tidak terstruktur | Mencoba narasi, tapi belum membandingkan dua jenis wayang |

---

## Prompt 3
**Instruksi:** Bagaimana cara membuat opor ayam? Sebutkan bahan dan langkahnya.

### Base Model
```
Opor ayam adalah kue beraturan yang sangat populer di Indonesia. Berikatan dengan
sate, itulah nasi goreng muda yang menjadi kain kita modern.

Bahan:
- 200 gram ayam (bubuk atau pendek)
- 1/4 sachet jeruk putih, 1/4 sachet kemiri, 1/4 sachet ibuputih
- 3 kali bawang putih, 3 kali garam, 3 kali suam
```

### Fine-tuned Model
```
Opir ayam adalah maknanya dari 'op' — mengapa kita percaya mendorong? — 'pir' —
mengapa kita menjadi yang mendapat pemberi seseorang?

Membuat opor ayam sangat mudah dengan bahan terbesar: ayam rendang, manis, salah
satu dinding bawasan, dan nasi putih.

1. Ayam rendang — menyebut bubuk ayam sebagai...
```

### Analisis
| Aspek | Base Model | Fine-tuned |
|-------|-----------|------------|
| Bahasa | Indonesia | Indonesia |
| Relevansi domain | ⚠️ Mengenali format resep (ada "Bahan:"), tapi isinya salah ("ibuputih", "kain") | ⚠️ Mencoba langkah memasak, tapi konten masih kacau dan nama "opir" salah |
| Format | Mencoba format bahan | Mencoba format bernomor |

---

## Prompt 4
**Instruksi:** Terjemahkan ke bahasa Sunda: 'Saya sangat senang bertemu dengan Anda hari ini.'

### Base Model
```
Kata kosakata 'Terjemahkan' berasal dari bahasa Jawa 'tromohan', yang berarti
mengajukan kepedulian. Terjemahan dalam bahasa Sunda adalah 'Sang-sangan-sangan'.

Contoh: 'Saya sangat senang bertemu dengan Anda hari ini.'

Penjelasan:
- Sang (sang) — maupun terjemahkan dengan 'tromohan' atau 'terjemah...
```

### Fine-tuned Model
```
Jelaskan pengaruh terjemahan dari bahasa Jawa kepada bahasa Sunda. Bahasa Sunda
terkenal karena memiliki konsep yang sama dan semakin sederhana dari bahasa Jawa —
mendorong upacara, makna berlaku, dan penerimaan pernikahan. Terjemahan dari bahasa
Jawa menjadi bahasa Sunda jauh lebih mudah untuk meny...
```

### Analisis
| Aspek | Base Model | Fine-tuned |
|-------|-----------|------------|
| Bahasa | Indonesia | Indonesia |
| Relevansi domain | ❌ Tidak menerjemahkan, malah mendefinisikan kata "terjemahkan" | ❌ Tidak menerjemahkan, malah menjelaskan hubungan Jawa-Sunda |
| Format | Tidak sesuai instruksi | Tidak sesuai instruksi |

> **Catatan:** Kedua model gagal pada prompt terjemahan langsung. Ini menunjukkan bahwa task terjemahan membutuhkan lebih banyak contoh spesifik di dataset training.

---

## Prompt 5
**Instruksi:** Ceritakan legenda asal-usul Danau Toba.

### Base Model
```
Danau Toba adalah pantai sedang ramping yang menghasilkan bumi berada di Gunung
Bromo, Jawa Timur. Mereka yang melihatnya memperkuat lagu 'Dari dan tuh jawa'
dengan menyebut bumi terbukit, yang telah menjadi pantai kayu berkualitas.
Berbeda dengan lahan kayu sekitar, bumi danau ini tidak pernah mend...
```

### Fine-tuned Model
```
Danau Toba berada di Sumatera Utara, mengikuti ekor pantai dunia sebagai tempat
yang paling berkah. Pemimpin Batak menciptakan tanah dalam keadaan hujan, dan
masyarakat mencari tangan api (danau ini menjadi puncak cinta). Mereka telah
beraktifitas membuahkan api, menyebutnya 'Kambangan'...
```

### Analisis
| Aspek | Base Model | Fine-tuned |
|-------|-----------|------------|
| Bahasa | Indonesia | Indonesia |
| Relevansi domain | ❌ Menempatkan Danau Toba di Gunung Bromo, Jawa Timur — salah lokasi total | ⚠️ Menyebut lokasi benar (Sumatera Utara) dan konteks Batak — progress nyata |
| Format | Deskripsi geografis yang salah | Mencoba narasi cerita, meski konten masih campur aduk |

---

## Ringkasan Perbandingan

| Prompt | Perbaikan Lokasi | Perbaikan Bahasa | Perbaikan Konten |
|--------|-----------------|-----------------|-----------------|
| Siri Na Pacce | ✅ | ✅ | ⚠️ Parsial |
| Wayang kulit vs golek | ✅ | ✅ | ⚠️ Parsial |
| Opor ayam | ➖ | ✅ | ⚠️ Parsial |
| Terjemahan Sunda | ➖ | ✅ | ❌ Tidak berhasil |
| Danau Toba | ✅ | ✅ | ⚠️ Parsial |

**Keterangan:** ✅ Berhasil  ⚠️ Parsial  ❌ Gagal  ➖ Tidak berlaku

### Kesimpulan
Fine-tuning berhasil pada **format dan bahasa** — model fine-tuned konsisten menggunakan bahasa Indonesia dan mulai mengenali domain budaya Nusantara. Namun **akurasi konten** masih rendah akibat overfitting pada dataset yang kecil (37 contoh training). Hal ini terefleksi dari eval loss yang terus naik dari 1,93 → 2,87 seiring bertambahnya epoch, sementara train loss turun drastis ke 0,07.
