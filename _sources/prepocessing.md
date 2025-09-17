# Data Preprocessing Teks

**Data preprocessing** adalah proses menyiapkan data mentah agar menjadi lebih bersih, rapi, dan siap digunakan dalam analisis atau pemodelan.  

Dalam konteks **teks**, preprocessing sangat penting karena data teks biasanya masih mengandung banyak hal yang tidak relevan, seperti tanda baca, kata-kata umum (stopwords), salah ketik, atau variasi kata yang berbeda-beda tetapi bermakna sama.  

### Tujuan Preprocessing
- Mengurangi noise (gangguan) dalam data.  
- Menstandarkan teks agar seragam.  
- Memperbaiki kualitas data sehingga hasil analisis (misalnya klasifikasi, clustering, TF-IDF, word embedding) menjadi lebih akurat.  

---

## Tahapan Preprocessing Teks yang Digunakan

### 1. Stopword Removal
Menghapus kata-kata umum yang tidak memiliki makna penting, seperti *yang, di, ke, dan, atau*.  

**Contoh:**  
- Sebelum: *“Penelitian ini dilakukan di universitas”*  
- Sesudah: *“Penelitian dilakukan universitas”*  

---

### 2. Menghilangkan Simbol atau Tanda Baca
Membersihkan teks dari tanda baca (.,!?) atau simbol (&, %, #) agar lebih konsisten.  

**Contoh:**  
- Sebelum: *“Data, hasil & analisis!!!”*  
- Sesudah: *“Data hasil analisis”*  

---

### 3. Cek Ejaan dan Pembakuan Kata
Melakukan normalisasi kata dengan cara:  
- Mengoreksi kata salah ketik (*akurasii → akurasi*).  
- Membakukan singkatan (*tp → tetapi*, *utk → untuk*).  

**Contoh:**  
- Sebelum: *“resp yg ikut bnyk tp datanya kurang”*  
- Sesudah: *“responden yang ikut banyak tetapi datanya kurang”*  

---

### 4. Stemming
Mengubah kata ke bentuk dasar. Dengan stemming, kata “penelitian”, “meneliti”, dan “diteliti” dianggap sama karena semuanya menjadi “teliti”.  

**Contoh:**  
- Sebelum: *“Penelitian dilakukan universitas”*  
- Sesudah: *“teliti lakukan universitas”*  

---

### 5. Tokenisasi
Memecah kalimat menjadi unit kata (token) sehingga lebih mudah dianalisis.  

**Contoh:**  
- Sebelum: *“teliti lakukan universitas”*  
- Sesudah: *[“teliti”, “lakukan”, “universitas”]*  

---

## Kesimpulan
Preprocessing teks adalah **fondasi utama** sebelum analisis data.  
Dengan membersihkan, membakukan, dan menstandarkan kata, data menjadi:  
- Lebih **rapi**  
- Lebih **ringkas**  
- Lebih **siap** digunakan untuk algoritma NLP seperti TF-IDF, klasifikasi, clustering, atau deep learning.  
