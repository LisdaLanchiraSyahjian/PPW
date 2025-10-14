# Klasifikasi Berita menggunakan Fitur Latent Dirichlet Allocation





```python
from google.colab import drive
drive.mount('/content/drive')

```

    Mounted at /content/drive
    


```python
import pandas as pd

# Baca dataset
df = pd.read_csv('/content/drive/MyDrive/Semester 7/berita_fiks.csv')

# Atur supaya semua kolom & baris ditampilkan
pd.set_option('display.max_columns', None)  # tampilkan semua kolom
pd.set_option('display.max_rows', None)     # tampilkan semua baris

# Tampilkan dalam bentuk tabel (scrollable di Colab)
df

```





  <div id="df-568f4277-d9fe-4f1a-a835-96198e39bfd5" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>judul</th>
      <th>kategori_baru</th>
      <th>isi</th>
      <th>before</th>
      <th>after_lower_no_symbol</th>
      <th>after_stopword</th>
      <th>after_corrected</th>
      <th>after_stemmed</th>
      <th>tokens_final</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Dua Tahun Berlalu, Korban Gempa Maroko Masih H...</td>
      <td>news</td>
      <td>Maroko - Dua tahun pascagempa, ribuan warga Ma...</td>
      <td>Maroko - Dua tahun pascagempa, ribuan warga Ma...</td>
      <td>maroko dua tahun pascagempa ribuan warga marok...</td>
      <td>maroko pascagempa ribuan warga maroko tinggal ...</td>
      <td>maroko pascagempa ribuan harga maroko tinggal ...</td>
      <td>maroko pascagempa ribu harga maroko tinggal te...</td>
      <td>['maroko', 'pascagempa', 'ribu', 'harga', 'mar...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Alvi Pemutilasi Pacar Diamuk dan Diumpat Warga...</td>
      <td>news</td>
      <td>Rekonstruksi kasus Alvi Maulana (24) yang muti...</td>
      <td>Rekonstruksi kasus Alvi Maulana (24) yang muti...</td>
      <td>rekonstruksi kasus alvi maulana yang mutilasi ...</td>
      <td>rekonstruksi alvi maulana mutilasi tiara angel...</td>
      <td>rekonstruksi alvi maulana mutilasi tiara angel...</td>
      <td>rekonstruksi alvi maulana mutilasi tiara angel...</td>
      <td>['rekonstruksi', 'alvi', 'maulana', 'mutilasi'...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Truk Seruduk 2 Angkot Lagi Ngetem di Bogor, 3 ...</td>
      <td>olahraga</td>
      <td>Kecelakaan lalu lintas melibatkan truk dan dua...</td>
      <td>Kecelakaan lalu lintas melibatkan truk dan dua...</td>
      <td>kecelakaan lalu lintas melibatkan truk dan dua...</td>
      <td>kecelakaan lintas melibatkan truk unit angkot ...</td>
      <td>kecelakaan lintas melibatkan truk tni angkot a...</td>
      <td>celaka lintas libat truk tni angkot angkut kot...</td>
      <td>['celaka', 'lintas', 'libat', 'truk', 'tni', '...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>MK Gelar Sidang Putusan 5 Gugatan UU TNI Hari Ini</td>
      <td>news</td>
      <td>Mahkamah Konstitusi (MK) akan menggelar sidang...</td>
      <td>Mahkamah Konstitusi (MK) akan menggelar sidang...</td>
      <td>mahkamah konstitusi mk akan menggelar sidang g...</td>
      <td>mahkamah konstitusi mk menggelar sidang gugata...</td>
      <td>mahkamah konstitusi kpk menggelar sidang gugat...</td>
      <td>mahkamah konstitusi kpk gelar sidang gugat uu ...</td>
      <td>['mahkamah', 'konstitusi', 'kpk', 'gelar', 'si...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Gelar Razia, Pemprov Banten Temukan 86 Kendara...</td>
      <td>politik</td>
      <td>Pemerintah Provinsi Banten menggelar razia ken...</td>
      <td>Pemerintah Provinsi Banten menggelar razia ken...</td>
      <td>pemerintah provinsi banten menggelar razia ken...</td>
      <td>pemerintah provinsi banten menggelar razia ken...</td>
      <td>pemerintah provinsi banten menggelar razia ken...</td>
      <td>perintah provinsi banten gelar razia kendara b...</td>
      <td>['perintah', 'provinsi', 'banten', 'gelar', 'r...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Bareskrim Usul Ada LO Polri di LPSK demi Perku...</td>
      <td>politik</td>
      <td>Bareskrim Polri memberikan sejumlah usulan ter...</td>
      <td>Bareskrim Polri memberikan sejumlah usulan ter...</td>
      <td>bareskrim polri memberikan sejumlah usulan ter...</td>
      <td>bareskrim polri usulan terkait ruu lpsk baresk...</td>
      <td>bareskrim polisi usulan terkait ruu kpk baresk...</td>
      <td>bareskrim polisi usul kait ruu kpk bareskrim u...</td>
      <td>['bareskrim', 'polisi', 'usul', 'kait', 'ruu',...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Pemobil di Pekanbaru Jadi Tersangka Usai Pukul...</td>
      <td>olahraga</td>
      <td>Pengendara mobil di Pekanbaru , Riau, Olvi Pra...</td>
      <td>Pengendara mobil di Pekanbaru , Riau, Olvi Pra...</td>
      <td>pengendara mobil di pekanbaru riau olvi praiya...</td>
      <td>pengendara mobil pekanbaru riau olvi praiyan m...</td>
      <td>pengendara mobil pekanbaru riau olvi praiyan m...</td>
      <td>kendara mobil pekanbaru riau olvi praiyan ania...</td>
      <td>['kendara', 'mobil', 'pekanbaru', 'riau', 'olv...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Pimpinan Komisi I DPR Ungkap Djamari Chaniago ...</td>
      <td>olahraga</td>
      <td>Presiden Prabowo Subianto akan melantik Menter...</td>
      <td>Presiden Prabowo Subianto akan melantik Menter...</td>
      <td>presiden prabowo subianto akan melantik menter...</td>
      <td>presiden prabowo subianto melantik menteri koo...</td>
      <td>presiden prabowo subianto melantik menteri koo...</td>
      <td>presiden prabowo subianto lantik menteri koord...</td>
      <td>['presiden', 'prabowo', 'subianto', 'lantik', ...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>China Kumpulkan Sekutunya, Bentuk Tatanan Glob...</td>
      <td>politik</td>
      <td>Forum Xiangshan adalah pertemuan keamanan inte...</td>
      <td>Forum Xiangshan adalah pertemuan keamanan inte...</td>
      <td>forum xiangshan adalah pertemuan keamanan inte...</td>
      <td>forum xiangshan pertemuan keamanan internasion...</td>
      <td>forum xiangshan pertemuan keamanan internasion...</td>
      <td>forum xiangshan temu aman internasional tahun ...</td>
      <td>['forum', 'xiangshan', 'temu', 'aman', 'intern...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Wamentrans Sebut Pengiriman Transmigran Tergan...</td>
      <td>politik</td>
      <td>Wakil Menteri Transmigrasi Viva Yoga Mauladi m...</td>
      <td>Wakil Menteri Transmigrasi Viva Yoga Mauladi m...</td>
      <td>wakil menteri transmigrasi viva yoga mauladi m...</td>
      <td>wakil menteri transmigrasi viva yoga mauladi k...</td>
      <td>wakil menteri transmigrasi liga liga mauladi k...</td>
      <td>wakil menteri transmigrasi liga liga mauladi m...</td>
      <td>['wakil', 'menteri', 'transmigrasi', 'liga', '...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>Ini Sosok Ken Otak Penculikan Kacab Bank demi ...</td>
      <td>news</td>
      <td>Polisi mengungkap C alias Ken merupakan otak a...</td>
      <td>Polisi mengungkap C alias Ken merupakan otak a...</td>
      <td>polisi mengungkap c alias ken merupakan otak a...</td>
      <td>polisi mengungkap c alias ken otak dalang penc...</td>
      <td>polisi mengungkap c alias kpk otak dalang penc...</td>
      <td>polisi ungkap c alias kpk otak dalang culi kep...</td>
      <td>['polisi', 'ungkap', 'c', 'alias', 'kpk', 'ota...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>Berkas Sidang Etik 5 Anggota Brimob Pelindas A...</td>
      <td>news</td>
      <td>Lima anggota Brimob lainnya terkait kasus ojek...</td>
      <td>Lima anggota Brimob lainnya terkait kasus ojek...</td>
      <td>lima anggota brimob lainnya terkait kasus ojek...</td>
      <td>anggota brimob terkait ojek online affan kurni...</td>
      <td>anggota brimob terkait ojek online affan kurni...</td>
      <td>anggota brimob kait ojek online affan kurniawa...</td>
      <td>['anggota', 'brimob', 'kait', 'ojek', 'online'...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>Filipina vs China di Laut China Selatan, 1 Awa...</td>
      <td>travel</td>
      <td>Kapal-kapal Filipina dan China terlibat inside...</td>
      <td>Kapal-kapal Filipina dan China terlibat inside...</td>
      <td>kapalkapal filipina dan china terlibat insiden...</td>
      <td>kapalkapal filipina china terlibat insiden ter...</td>
      <td>kapalkapal filipina china terlibat insiden ter...</td>
      <td>kapalkapal filipina china libat insiden baru a...</td>
      <td>['kapalkapal', 'filipina', 'china', 'libat', '...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>14</td>
      <td>Polres Meranti Telah Distribusikan 115 Ton Ber...</td>
      <td>news</td>
      <td>Polres Kepulauan Meranti terus menggencarkan g...</td>
      <td>Polres Kepulauan Meranti terus menggencarkan g...</td>
      <td>polres kepulauan meranti terus menggencarkan g...</td>
      <td>polres kepulauan meranti menggencarkan gerakan...</td>
      <td>polres kepulauan meranti menggencarkan gerakan...</td>
      <td>polres pulau meranti gencar gera pangan rumah ...</td>
      <td>['polres', 'pulau', 'meranti', 'gencar', 'gera...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>15</td>
      <td>BNPT: Keberagaman Harus Dijaga Sebagai Perekat...</td>
      <td>news</td>
      <td>Badan Nasional Penanggulangan Terorisme (BNPT)...</td>
      <td>Badan Nasional Penanggulangan Terorisme (BNPT)...</td>
      <td>badan nasional penanggulangan terorisme bnpt m...</td>
      <td>badan nasional penanggulangan terorisme bnpt m...</td>
      <td>badan nasional penanggulangan terorisme bnpt m...</td>
      <td>badan nasional tanggulang terorisme bnpt gelar...</td>
      <td>['badan', 'nasional', 'tanggulang', 'terorisme...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>16</td>
      <td>RUU Perlindungan Saksi Korban, Kejagung Usul K...</td>
      <td>politik</td>
      <td>Jaksa Agung Muda Tindak Pidana Umum Kejaksaan ...</td>
      <td>Jaksa Agung Muda Tindak Pidana Umum Kejaksaan ...</td>
      <td>jaksa agung muda tindak pidana umum kejaksaan ...</td>
      <td>jaksa agung muda tindak pidana kejaksaan agung...</td>
      <td>jaksa agung muda tindak pidana kejaksaan agung...</td>
      <td>jaksa agung muda tindak pidana jaksa agung jam...</td>
      <td>['jaksa', 'agung', 'muda', 'tindak', 'pidana',...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>Rapat Bareng Kajati Sulsel, Legislator Tanya K...</td>
      <td>politik</td>
      <td>Komisi III DPR RI menggelar rapat dengar penda...</td>
      <td>Komisi III DPR RI menggelar rapat dengar penda...</td>
      <td>komisi iii dpr ri menggelar rapat dengar penda...</td>
      <td>komisi iii dpr ri menggelar rapat dengar penda...</td>
      <td>polisi tni dpr tni menggelar rapat dengar pend...</td>
      <td>polisi tni dpr tni gelar rapat dengar dapat dp...</td>
      <td>['polisi', 'tni', 'dpr', 'tni', 'gelar', 'rapa...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>Antusiasnya Ibu-ibu Borong Sembako di Gerakan ...</td>
      <td>news</td>
      <td>Polri menggelar Gerakan Pangan Murah (GPM) Pol...</td>
      <td>Polri menggelar Gerakan Pangan Murah (GPM) Pol...</td>
      <td>polri menggelar gerakan pangan murah gpm polri...</td>
      <td>polri menggelar gerakan pangan murah gpm polri...</td>
      <td>polisi menggelar gerakan pangan rumah dpr poli...</td>
      <td>polisi gelar gera pangan rumah dpr polisi lapa...</td>
      <td>['polisi', 'gelar', 'gera', 'pangan', 'rumah',...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>19</td>
      <td>Ibas: Maulid Nabi Inspirasi Peradaban Akhlak d...</td>
      <td>olahraga</td>
      <td>Wakil Ketua MPR RI dari Partai Demokrat Edhie ...</td>
      <td>Wakil Ketua MPR RI dari Partai Demokrat Edhie ...</td>
      <td>wakil ketua mpr ri dari partai demokrat edhie ...</td>
      <td>wakil ketua mpr ri partai demokrat edhie basko...</td>
      <td>wakil ketua dpr tni partai demokrat edhie bask...</td>
      <td>wakil ketua dpr tni partai demokrat edhie bask...</td>
      <td>['wakil', 'ketua', 'dpr', 'tni', 'partai', 'de...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>Pencuri Kabel Grounding Whoosh Ditangkap Saat ...</td>
      <td>news</td>
      <td>Seorang pria pencuri kabel grounding di jalur ...</td>
      <td>Seorang pria pencuri kabel grounding di jalur ...</td>
      <td>seorang pria pencuri kabel grounding di jalur ...</td>
      <td>pria pencuri kabel grounding jalur whoosh pada...</td>
      <td>pria pencuri kabel grounding jalur whoosh pada...</td>
      <td>pria curi kabel grounding jalur whoosh padalar...</td>
      <td>['pria', 'curi', 'kabel', 'grounding', 'jalur'...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>21</td>
      <td>Pekan Tuli Internasional 2025: Tema dan Cara M...</td>
      <td>news</td>
      <td>Pekan Tuli Internasional atau International We...</td>
      <td>Pekan Tuli Internasional atau International We...</td>
      <td>pekan tuli internasional atau international we...</td>
      <td>pekan tuli internasional international week of...</td>
      <td>pekan tni internasional internasional week gol...</td>
      <td>pekan tni internasional internasional week gol...</td>
      <td>['pekan', 'tni', 'internasional', 'internasion...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>22</td>
      <td>MK Tak Terima Gugatan PSU Pilbup Barito Utara,...</td>
      <td>news</td>
      <td>Mahkamah Konstitusi (MK) tidak menerima gugata...</td>
      <td>Mahkamah Konstitusi (MK) tidak menerima gugata...</td>
      <td>mahkamah konstitusi mk tidak menerima gugatan ...</td>
      <td>mahkamah konstitusi mk menerima gugatan terkai...</td>
      <td>mahkamah konstitusi kpk menerima gugatan terka...</td>
      <td>mahkamah konstitusi kpk terima gugat kait mung...</td>
      <td>['mahkamah', 'konstitusi', 'kpk', 'terima', 'g...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>23</td>
      <td>Waskita Karya Kembali Masuk Top 50 Emiten dala...</td>
      <td>health</td>
      <td>PT Waskita Karya (Persero) Tbk kembali mencata...</td>
      <td>PT Waskita Karya (Persero) Tbk kembali mencata...</td>
      <td>pt waskita karya persero tbk kembali mencatat ...</td>
      <td>pt waskita karya persero tbk mencatat prestasi...</td>
      <td>dpr waskita harga persero kpk mencatat prestas...</td>
      <td>dpr waskita harga persero kpk catat prestasi b...</td>
      <td>['dpr', 'waskita', 'harga', 'persero', 'kpk', ...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>24</td>
      <td>Aksi Damai di Depan Polda Metro Minta Bebaskan...</td>
      <td>news</td>
      <td>Aliansi Perempuan Indonesia menggelar aksi dam...</td>
      <td>Aliansi Perempuan Indonesia menggelar aksi dam...</td>
      <td>aliansi perempuan indonesia menggelar aksi dam...</td>
      <td>aliansi perempuan indonesia menggelar aksi dam...</td>
      <td>aliansi perempuan indonesia menggelar jaksa da...</td>
      <td>aliansi perempuan indonesia gelar jaksa damai ...</td>
      <td>['aliansi', 'perempuan', 'indonesia', 'gelar',...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>25</td>
      <td>Kapolda Riau Apel Satkamling Garda Terdepan Pe...</td>
      <td>health</td>
      <td>Kepolisian Daerah (Polda) Riau mengaktifkan ke...</td>
      <td>Kepolisian Daerah (Polda) Riau mengaktifkan ke...</td>
      <td>kepolisian daerah polda riau mengaktifkan kemb...</td>
      <td>kepolisian daerah polda riau mengaktifkan satu...</td>
      <td>kepolisian daerah polda riau mengaktifkan satu...</td>
      <td>polisi daerah polda riau aktif satu aman lingk...</td>
      <td>['polisi', 'daerah', 'polda', 'riau', 'aktif',...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>Dilelang KPK Lagi, Baju Sutra Goceng Kini Laku...</td>
      <td>news</td>
      <td>KPK menggelar lelang barang rampasan kasus kor...</td>
      <td>KPK menggelar lelang barang rampasan kasus kor...</td>
      <td>kpk menggelar lelang barang rampasan kasus kor...</td>
      <td>kpk menggelar lelang barang rampasan korupsi s...</td>
      <td>kpk menggelar lelang barang rampasan korupsi s...</td>
      <td>kpk gelar lelang barang rampas korupsi salah l...</td>
      <td>['kpk', 'gelar', 'lelang', 'barang', 'rampas',...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>27</td>
      <td>Syarat dan Cara Daftar BPJS Ketenagakerjaan un...</td>
      <td>news</td>
      <td>Orang yang bekerja di luar negeri (pekerja mig...</td>
      <td>Orang yang bekerja di luar negeri (pekerja mig...</td>
      <td>orang yang bekerja di luar negeri pekerja migr...</td>
      <td>orang negeri pekerja migran bpjs ketenagakerja...</td>
      <td>orang negeri pekerja migran bpjs ketenagakerja...</td>
      <td>orang negeri kerja migran bpjs ketenagakerjaan...</td>
      <td>['orang', 'negeri', 'kerja', 'migran', 'bpjs',...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>29</td>
      <td>Cerita Warga Pilih Naik MRT Hindari Macet di F...</td>
      <td>news</td>
      <td>Dalam rangka memperingati Hari Perhubungan Nas...</td>
      <td>Dalam rangka memperingati Hari Perhubungan Nas...</td>
      <td>dalam rangka memperingati hari perhubungan nas...</td>
      <td>rangka memperingati perhubungan nasional kesel...</td>
      <td>rangka memperingati perhubungan nasional kesel...</td>
      <td>rangka ingat hubung nasional selamat lintas an...</td>
      <td>['rangka', 'ingat', 'hubung', 'nasional', 'sel...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>30</td>
      <td>Otak Penculikan Kacab Bank Berkelit soal Sosok...</td>
      <td>news</td>
      <td>Polisi mengungkap C alias Ken, yang merupakan ...</td>
      <td>Polisi mengungkap C alias Ken, yang merupakan ...</td>
      <td>polisi mengungkap c alias ken yang merupakan d...</td>
      <td>polisi mengungkap c alias ken dalang otak penc...</td>
      <td>polisi mengungkap c alias kpk dalang otak penc...</td>
      <td>polisi ungkap c alias kpk dalang otak culi kep...</td>
      <td>['polisi', 'ungkap', 'c', 'alias', 'kpk', 'dal...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>32</td>
      <td>Mengapa Komunikasi Pemerintah Lewat Layar Bios...</td>
      <td>politik</td>
      <td>Penayangan pesan Presiden di bioskop baru-baru...</td>
      <td>Penayangan pesan Presiden di bioskop baru-baru...</td>
      <td>penayangan pesan presiden di bioskop barubaru ...</td>
      <td>penayangan pesan presiden bioskop barubaru mem...</td>
      <td>penayangan pasar presiden bioskop barubaru mem...</td>
      <td>tayang pasar presiden bioskop barubaru picu de...</td>
      <td>['tayang', 'pasar', 'presiden', 'bioskop', 'ba...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>33</td>
      <td>Siswi Bogor Terjebak di Toilet Bimbel, Damkar ...</td>
      <td>news</td>
      <td>Seorang siswi terjebak di dalam toilet sebuah ...</td>
      <td>Seorang siswi terjebak di dalam toilet sebuah ...</td>
      <td>seorang siswi terjebak di dalam toilet sebuah ...</td>
      <td>siswi terjebak toilet bimbingan belajar bimbel...</td>
      <td>siswi terjebak toilet bimbingan belajar bimbel...</td>
      <td>siswi jebak toilet bimbing ajar bimbel cibinon...</td>
      <td>['siswi', 'jebak', 'toilet', 'bimbing', 'ajar'...</td>
    </tr>
    <tr>
      <th>31</th>
      <td>34</td>
      <td>Cetakan Tangan Emas Presiden Pertama Kazakhsta...</td>
      <td>politik</td>
      <td>Menara Bayterek berdiri gagah di Kota Astana, ...</td>
      <td>Menara Bayterek berdiri gagah di Kota Astana, ...</td>
      <td>menara bayterek berdiri gagah di kota astana k...</td>
      <td>menara bayterek berdiri gagah kota astana kaza...</td>
      <td>menara bayterek berdiri gagah kota astana kaza...</td>
      <td>menara bayterek diri gagah kota astana kazakhs...</td>
      <td>['menara', 'bayterek', 'diri', 'gagah', 'kota'...</td>
    </tr>
    <tr>
      <th>32</th>
      <td>35</td>
      <td>Makin Panas! Israel Bombardir Pelabuhan Yaman ...</td>
      <td>travel</td>
      <td>Militer Israel menyerang pelabuhan Hodeida yan...</td>
      <td>Militer Israel menyerang pelabuhan Hodeida yan...</td>
      <td>militer israel menyerang pelabuhan hodeida yan...</td>
      <td>militer israel menyerang pelabuhan hodeida dik...</td>
      <td>militer israel menyerang pelabuhan hodeida dik...</td>
      <td>militer israel serang labuh hodeida asai kelom...</td>
      <td>['militer', 'israel', 'serang', 'labuh', 'hode...</td>
    </tr>
    <tr>
      <th>33</th>
      <td>36</td>
      <td>Identitas Kerangka Manusia dalam Pohon Aren Ma...</td>
      <td>news</td>
      <td>Polres Serdang Bedagai tak ingin berspekulasi ...</td>
      <td>Polres Serdang Bedagai tak ingin berspekulasi ...</td>
      <td>polres serdang bedagai tak ingin berspekulasi ...</td>
      <td>polres serdang bedagai berspekulasi identitas ...</td>
      <td>polres serdang bedagai berspekulasi identitas ...</td>
      <td>polres serdang bedagai spekulasi identitas ker...</td>
      <td>['polres', 'serdang', 'bedagai', 'spekulasi', ...</td>
    </tr>
    <tr>
      <th>34</th>
      <td>37</td>
      <td>MK Tolak Gugatan Hasil PSU Pilgub Papua</td>
      <td>politik</td>
      <td>Mahkamah Konstitusi menolak gugatan Benhur Tom...</td>
      <td>Mahkamah Konstitusi menolak gugatan Benhur Tom...</td>
      <td>mahkamah konstitusi menolak gugatan benhur tom...</td>
      <td>mahkamah konstitusi menolak gugatan benhur tom...</td>
      <td>mahkamah konstitusi menolak gugatan benhur tni...</td>
      <td>mahkamah konstitusi tolak gugat benhur tni man...</td>
      <td>['mahkamah', 'konstitusi', 'tolak', 'gugat', '...</td>
    </tr>
    <tr>
      <th>35</th>
      <td>38</td>
      <td>KIP Kuliah 2025: Jadwal, Besaran Bantuan hingg...</td>
      <td>news</td>
      <td>Pendaftaran KIP Kuliah 2025 masih berlangsung ...</td>
      <td>Pendaftaran KIP Kuliah 2025 masih berlangsung ...</td>
      <td>pendaftaran kip kuliah masih berlangsung untuk...</td>
      <td>pendaftaran kip kuliah jalur seleksi mandiri j...</td>
      <td>pendaftaran kpk kuliah jalur seleksi mandiri j...</td>
      <td>daftar kpk kuliah jalur seleksi mandiri jalur ...</td>
      <td>['daftar', 'kpk', 'kuliah', 'jalur', 'seleksi'...</td>
    </tr>
    <tr>
      <th>36</th>
      <td>39</td>
      <td>Senangnya Warga Tarif MRT Rp 1 Hari Ini: Sisan...</td>
      <td>travel</td>
      <td>Warga mengaku sangat senang dengan tarif Rp 1 ...</td>
      <td>Warga mengaku sangat senang dengan tarif Rp 1 ...</td>
      <td>warga mengaku sangat senang dengan tarif rp na...</td>
      <td>warga mengaku senang tarif rp transportasi mrt...</td>
      <td>harga mengaku senang tarif dpr transportasi mr...</td>
      <td>harga aku senang tarif dpr transportasi mrt ra...</td>
      <td>['harga', 'aku', 'senang', 'tarif', 'dpr', 'tr...</td>
    </tr>
    <tr>
      <th>37</th>
      <td>40</td>
      <td>Penertiban Lahan Reaktivasi KA Rangkasbitung-P...</td>
      <td>politik</td>
      <td>Pemerintah Provinsi Banten menjelaskan perkemb...</td>
      <td>Pemerintah Provinsi Banten menjelaskan perkemb...</td>
      <td>pemerintah provinsi banten menjelaskan perkemb...</td>
      <td>pemerintah provinsi banten perkembangan rencan...</td>
      <td>pemerintah provinsi banten perkembangan rencan...</td>
      <td>perintah provinsi banten kembang rencana aktif...</td>
      <td>['perintah', 'provinsi', 'banten', 'kembang', ...</td>
    </tr>
    <tr>
      <th>38</th>
      <td>42</td>
      <td>Prabowo Lantik Menko Polkam-Menpora di Istana ...</td>
      <td>politik</td>
      <td>Presiden Prabowo Subianto bakal melantik sejum...</td>
      <td>Presiden Prabowo Subianto bakal melantik sejum...</td>
      <td>presiden prabowo subianto bakal melantik sejum...</td>
      <td>presiden prabowo subianto melantik menteri kab...</td>
      <td>presiden prabowo subianto melantik menteri kab...</td>
      <td>presiden prabowo subianto lantik menteri kabin...</td>
      <td>['presiden', 'prabowo', 'subianto', 'lantik', ...</td>
    </tr>
    <tr>
      <th>39</th>
      <td>43</td>
      <td>Penampakan Predator Seks yang Perkosa 8 Korban...</td>
      <td>news</td>
      <td>Polres Maluku Tenggara menangkap KT alias Konv...</td>
      <td>Polres Maluku Tenggara menangkap KT alias Konv...</td>
      <td>polres maluku tenggara menangkap kt alias konv...</td>
      <td>polres maluku tenggara menangkap kt alias konv...</td>
      <td>polres maluku tenggara menangkap kpk alias kon...</td>
      <td>polres malu tenggara tangkap kpk alias konven ...</td>
      <td>['polres', 'malu', 'tenggara', 'tangkap', 'kpk...</td>
    </tr>
    <tr>
      <th>40</th>
      <td>44</td>
      <td>Kronologi Mahasiswi di Ciracas Dibunuh Pacar A...</td>
      <td>news</td>
      <td>Seorang mahasiswi berinisial IM (23) ditemukan...</td>
      <td>Seorang mahasiswi berinisial IM (23) ditemukan...</td>
      <td>seorang mahasiswi berinisial im ditemukan tewa...</td>
      <td>mahasiswi berinisial im ditemukan tewas kos ka...</td>
      <td>mahasiswi berinisial im ditemukan tewas gol ka...</td>
      <td>mahasiswi inisial im temu tewas gol kawasan ci...</td>
      <td>['mahasiswi', 'inisial', 'im', 'temu', 'tewas'...</td>
    </tr>
    <tr>
      <th>41</th>
      <td>45</td>
      <td>Tenang, Mental Health Dijamin BPJS Kesehatan</td>
      <td>health</td>
      <td>Badan Penyelenggara Jaminan Sosial Kesehatan (...</td>
      <td>Badan Penyelenggara Jaminan Sosial Kesehatan (...</td>
      <td>badan penyelenggara jaminan sosial kesehatan b...</td>
      <td>badan penyelenggara jaminan sosial kesehatan b...</td>
      <td>badan penyelenggara jaminan sosial kesehatan b...</td>
      <td>badan selenggara jamin sosial sehat bpjs sehat...</td>
      <td>['badan', 'selenggara', 'jamin', 'sosial', 'se...</td>
    </tr>
    <tr>
      <th>42</th>
      <td>46</td>
      <td>Pramono Jawab Curhat Komeng soal Banjir di Jak...</td>
      <td>news</td>
      <td>Gubernur DKI Jakarta Pramono Anung menjawab an...</td>
      <td>Gubernur DKI Jakarta Pramono Anung menjawab an...</td>
      <td>gubernur dki jakarta pramono anung menjawab an...</td>
      <td>gubernur dki jakarta pramono anung anggota kom...</td>
      <td>gubernur dpr jakarta pramono anung anggota kom...</td>
      <td>gubernur dpr jakarta pramono anung anggota kom...</td>
      <td>['gubernur', 'dpr', 'jakarta', 'pramono', 'anu...</td>
    </tr>
    <tr>
      <th>43</th>
      <td>47</td>
      <td>Bank bjb Ajak Nasabah Nabung Sekaligus Ikut La...</td>
      <td>olahraga</td>
      <td>Bank bjb menghadirkan terobosan baru yang meny...</td>
      <td>Bank bjb menghadirkan terobosan baru yang meny...</td>
      <td>bank bjb menghadirkan terobosan baru yang meny...</td>
      <td>bank bjb menghadirkan terobosan menyelaraskan ...</td>
      <td>bank bjb menghadirkan terobosan menyelaraskan ...</td>
      <td>bank bjb hadir terobos selaras layan finansial...</td>
      <td>['bank', 'bjb', 'hadir', 'terobos', 'selaras',...</td>
    </tr>
    <tr>
      <th>44</th>
      <td>48</td>
      <td>Cara Cek Tarif Listrik 2025 Lewat Situs PLN</td>
      <td>news</td>
      <td>Masyarakat dapat mengecek tarif listrik 2025 s...</td>
      <td>Masyarakat dapat mengecek tarif listrik 2025 s...</td>
      <td>masyarakat dapat mengecek tarif listrik secara...</td>
      <td>masyarakat mengecek tarif listrik online laman...</td>
      <td>masyarakat mengecek tarif listrik online laman...</td>
      <td>masyarakat ecek tarif listrik online laman res...</td>
      <td>['masyarakat', 'ecek', 'tarif', 'listrik', 'on...</td>
    </tr>
    <tr>
      <th>45</th>
      <td>49</td>
      <td>Kapolres Pelalawan Cek Pos Kamling, Ajak Warga...</td>
      <td>news</td>
      <td>Kapolres Pelalawan, AKBP John Louis Letedara, ...</td>
      <td>Kapolres Pelalawan, AKBP John Louis Letedara, ...</td>
      <td>kapolres pelalawan akbp john louis letedara me...</td>
      <td>kapolres pelalawan akbp john louis letedara pe...</td>
      <td>kapolres pelalawan akbp john louis letedara pe...</td>
      <td>kapolres pelalawan akbp john louis letedara ke...</td>
      <td>['kapolres', 'pelalawan', 'akbp', 'john', 'lou...</td>
    </tr>
    <tr>
      <th>46</th>
      <td>50</td>
      <td>Walkot Prabumulih Minta Maaf, Klarifikasi Penc...</td>
      <td>news</td>
      <td>Wali Kota Prabumulih, Arlan, meminta maaf kepa...</td>
      <td>Wali Kota Prabumulih, Arlan, meminta maaf kepa...</td>
      <td>wali kota prabumulih arlan meminta maaf kepada...</td>
      <td>wali kota prabumulih arlan maaf kepala smpn pr...</td>
      <td>wakil kota prabumulih arlan maaf kepala smpn p...</td>
      <td>wakil kota prabumulih arlan maaf kepala smpn p...</td>
      <td>['wakil', 'kota', 'prabumulih', 'arlan', 'maaf...</td>
    </tr>
    <tr>
      <th>47</th>
      <td>51</td>
      <td>Trump Mulai Kunjungan Bersejarah di Inggris, A...</td>
      <td>politik</td>
      <td>Presiden Amerika Serikat (AS) Donald Trump tib...</td>
      <td>Presiden Amerika Serikat (AS) Donald Trump tib...</td>
      <td>presiden amerika serikat as donald trump tiba ...</td>
      <td>presiden amerika serikat as donald trump inggr...</td>
      <td>presiden amerika serikat as donald trump inggr...</td>
      <td>presiden amerika serikat as donald trump inggr...</td>
      <td>['presiden', 'amerika', 'serikat', 'as', 'dona...</td>
    </tr>
    <tr>
      <th>48</th>
      <td>52</td>
      <td>Terungkap Pacar Bunuh Mahasiswi di Kos Ciracas...</td>
      <td>news</td>
      <td>Polisi mengungkap motif pria berinisial FF (16...</td>
      <td>Polisi mengungkap motif pria berinisial FF (16...</td>
      <td>polisi mengungkap motif pria berinisial ff yan...</td>
      <td>polisi mengungkap motif pria berinisial ff men...</td>
      <td>polisi mengungkap motif pria berinisial ff men...</td>
      <td>polisi ungkap motif pria inisial ff aniaya pac...</td>
      <td>['polisi', 'ungkap', 'motif', 'pria', 'inisial...</td>
    </tr>
    <tr>
      <th>49</th>
      <td>53</td>
      <td>Kronologi Mobil Boks Ditabrak Avanza hingga Te...</td>
      <td>news</td>
      <td>Polisi menjelaskan kronologi mobil boks tergul...</td>
      <td>Polisi menjelaskan kronologi mobil boks tergul...</td>
      <td>polisi menjelaskan kronologi mobil boks tergul...</td>
      <td>polisi kronologi mobil boks terguling tol jago...</td>
      <td>polisi kronologi mobil boks terguling gol jago...</td>
      <td>polisi kronologi mobil boks guling gol jagoraw...</td>
      <td>['polisi', 'kronologi', 'mobil', 'boks', 'guli...</td>
    </tr>
    <tr>
      <th>50</th>
      <td>54</td>
      <td>Hadiri The Taste of Papua, Papeda Buat Fatma S...</td>
      <td>politik</td>
      <td>Wakil Ketua Bidang 3 Solidaritas Perempuan Unt...</td>
      <td>Wakil Ketua Bidang 3 Solidaritas Perempuan Unt...</td>
      <td>wakil ketua bidang solidaritas perempuan untuk...</td>
      <td>wakil ketua bidang solidaritas perempuan indon...</td>
      <td>wakil ketua bidang solidaritas perempuan indon...</td>
      <td>wakil ketua bidang solidaritas perempuan indon...</td>
      <td>['wakil', 'ketua', 'bidang', 'solidaritas', 'p...</td>
    </tr>
    <tr>
      <th>51</th>
      <td>55</td>
      <td>Apa Itu Subjek Data Pribadi? Simak Penjelasann...</td>
      <td>politik</td>
      <td>Istilah Subjek Data Pribadi menjadi salah satu...</td>
      <td>Istilah Subjek Data Pribadi menjadi salah satu...</td>
      <td>istilah subjek data pribadi menjadi salah satu...</td>
      <td>istilah subjek data pribadi salah undangundang...</td>
      <td>istilah subjek data pribadi salah undangundang...</td>
      <td>istilah subjek data pribadi salah undangundang...</td>
      <td>['istilah', 'subjek', 'data', 'pribadi', 'sala...</td>
    </tr>
    <tr>
      <th>52</th>
      <td>56</td>
      <td>Muncul Isu Kementerian BUMN Akan Dihapus, Legi...</td>
      <td>politik</td>
      <td>Anggota Komisi VI DPR RI , Mufti Anam, menyika...</td>
      <td>Anggota Komisi VI DPR RI , Mufti Anam, menyika...</td>
      <td>anggota komisi vi dpr ri mufti anam menyikapi ...</td>
      <td>anggota komisi vi dpr ri mufti anam menyikapi ...</td>
      <td>anggota polisi tni dpr tni mufti anam menyikap...</td>
      <td>anggota polisi tni dpr tni mufti anam sikap is...</td>
      <td>['anggota', 'polisi', 'tni', 'dpr', 'tni', 'mu...</td>
    </tr>
    <tr>
      <th>53</th>
      <td>57</td>
      <td>600 Mobil Melintas di Hari Kedua Uji Coba Jalu...</td>
      <td>health</td>
      <td>Uji coba pengoperasian pintu Tol Fatmawati 2 ,...</td>
      <td>Uji coba pengoperasian pintu Tol Fatmawati 2 ,...</td>
      <td>uji coba pengoperasian pintu tol fatmawati jak...</td>
      <td>uji coba pengoperasian pintu tol fatmawati jak...</td>
      <td>tni coba pengoperasian pintu gol fatmawati jak...</td>
      <td>tni coba operasi pintu gol fatmawati jakarta s...</td>
      <td>['tni', 'coba', 'operasi', 'pintu', 'gol', 'fa...</td>
    </tr>
    <tr>
      <th>54</th>
      <td>58</td>
      <td>Danantara, Strategic Flexibility dan Risk Culture</td>
      <td>travel</td>
      <td>Presiden Prabowo resmi meluncurkan Badan Penge...</td>
      <td>Presiden Prabowo resmi meluncurkan Badan Penge...</td>
      <td>presiden prabowo resmi meluncurkan badan penge...</td>
      <td>presiden prabowo resmi meluncurkan badan penge...</td>
      <td>presiden prabowo resmi meluncurkan badan penge...</td>
      <td>presiden prabowo resmi luncur badan kelola inv...</td>
      <td>['presiden', 'prabowo', 'resmi', 'luncur', 'ba...</td>
    </tr>
    <tr>
      <th>55</th>
      <td>59</td>
      <td>AHY Tegaskan Kepastian Hukum &amp; Dorong Pemanfaa...</td>
      <td>politik</td>
      <td>Sebagai wujud nyata komitmen pemerintah dalam ...</td>
      <td>Sebagai wujud nyata komitmen pemerintah dalam ...</td>
      <td>sebagai wujud nyata komitmen pemerintah dalam ...</td>
      <td>wujud nyata komitmen pemerintah kepastian huku...</td>
      <td>wujud nyata komitmen pemerintah kepastian huku...</td>
      <td>wujud nyata komitmen perintah pasti hukum mili...</td>
      <td>['wujud', 'nyata', 'komitmen', 'perintah', 'pa...</td>
    </tr>
    <tr>
      <th>56</th>
      <td>60</td>
      <td>Predator Seks di Maluku Ancam Sebar Foto Bugil...</td>
      <td>news</td>
      <td>Pria berinisial KT di Maluku ditangkap polisi....</td>
      <td>Pria berinisial KT di Maluku ditangkap polisi....</td>
      <td>pria berinisial kt di maluku ditangkap polisi ...</td>
      <td>pria berinisial kt maluku ditangkap polisi did...</td>
      <td>pria berinisial kpk maluku ditangkap polisi di...</td>
      <td>pria inisial kpk malu tangkap polisi duga anca...</td>
      <td>['pria', 'inisial', 'kpk', 'malu', 'tangkap', ...</td>
    </tr>
    <tr>
      <th>57</th>
      <td>61</td>
      <td>Tinjau Proyek Pengendalian Banjir, AHY Ungkap ...</td>
      <td>politik</td>
      <td>Menteri Koordinator Bidang Infrastruktur dan P...</td>
      <td>Menteri Koordinator Bidang Infrastruktur dan P...</td>
      <td>menteri koordinator bidang infrastruktur dan p...</td>
      <td>menteri koordinator bidang infrastruktur pemba...</td>
      <td>menteri koordinator bidang infrastruktur pemba...</td>
      <td>menteri koordinator bidang infrastruktur bangu...</td>
      <td>['menteri', 'koordinator', 'bidang', 'infrastr...</td>
    </tr>
    <tr>
      <th>58</th>
      <td>62</td>
      <td>Kapan Fenomena Gerhana Terakhir di Tahun 2025?...</td>
      <td>news</td>
      <td>Tahun 2025 menjadi salah satu tahun dengan fen...</td>
      <td>Tahun 2025 menjadi salah satu tahun dengan fen...</td>
      <td>tahun menjadi salah satu tahun dengan fenomena...</td>
      <td>salah fenomena gerhana padat masyarakat menyak...</td>
      <td>salah fenomena gerhana pasar masyarakat menyak...</td>
      <td>salah fenomena gerhana pasar masyarakat saksi ...</td>
      <td>['salah', 'fenomena', 'gerhana', 'pasar', 'mas...</td>
    </tr>
    <tr>
      <th>59</th>
      <td>63</td>
      <td>Ipda Kadek Sumerta, Sosok Peduli 100 Penyandan...</td>
      <td>news</td>
      <td>Ipda Kadek Sumerta tak hanya menjalankan tugas...</td>
      <td>Ipda Kadek Sumerta tak hanya menjalankan tugas...</td>
      <td>ipda kadek sumerta tak hanya menjalankan tugas...</td>
      <td>ipda kadek sumerta menjalankan tugasnya polisi...</td>
      <td>ipda kadek sumerta menjalankan tugasnya polisi...</td>
      <td>ipda kadek sumerta jalan tugas polisi abdi bin...</td>
      <td>['ipda', 'kadek', 'sumerta', 'jalan', 'tugas',...</td>
    </tr>
    <tr>
      <th>60</th>
      <td>64</td>
      <td>Partner In Crime Juga Ditipu Dukun Pengganda U...</td>
      <td>news</td>
      <td>Polisi mengungkap bahwa dukun pengganda uang, ...</td>
      <td>Polisi mengungkap bahwa dukun pengganda uang, ...</td>
      <td>polisi mengungkap bahwa dukun pengganda uang p...</td>
      <td>polisi mengungkap dukun pengganda uang pria be...</td>
      <td>polisi mengungkap hukum pengganda uang pria be...</td>
      <td>polisi ungkap hukum ganda uang pria inisial h ...</td>
      <td>['polisi', 'ungkap', 'hukum', 'ganda', 'uang',...</td>
    </tr>
    <tr>
      <th>61</th>
      <td>66</td>
      <td>Alvi Sempat Tertidur di Tangga Kos Usai Mutila...</td>
      <td>news</td>
      <td>Terungkap fakta baru dari pemeriksaan Alvi Mau...</td>
      <td>Terungkap fakta baru dari pemeriksaan Alvi Mau...</td>
      <td>terungkap fakta baru dari pemeriksaan alvi mau...</td>
      <td>terungkap fakta pemeriksaan alvi maulana membu...</td>
      <td>terungkap jaksa pemeriksaan alvi maulana membu...</td>
      <td>ungkap jaksa periksa alvi maulana bunuh mutila...</td>
      <td>['ungkap', 'jaksa', 'periksa', 'alvi', 'maulan...</td>
    </tr>
    <tr>
      <th>62</th>
      <td>67</td>
      <td>Patuhi Regulasi WLLP, Perusahaan Bakal Terima ...</td>
      <td>politik</td>
      <td>Kementerian Ketenagakerjaan (Kemnaker) menging...</td>
      <td>Kementerian Ketenagakerjaan (Kemnaker) menging...</td>
      <td>kementerian ketenagakerjaan kemnaker mengingat...</td>
      <td>kementerian ketenagakerjaan kemnaker perusahaa...</td>
      <td>kementerian ketenagakerjaan kemnaker perusahaa...</td>
      <td>menteri ketenagakerjaan kemnaker usaha indones...</td>
      <td>['menteri', 'ketenagakerjaan', 'kemnaker', 'us...</td>
    </tr>
    <tr>
      <th>63</th>
      <td>68</td>
      <td>Pendaftaran KJMU 2025 Tahap 2: Jadwal hingga P...</td>
      <td>politik</td>
      <td>Pemprov DKI Jakarta melalui Dinas Pendidikan J...</td>
      <td>Pemprov DKI Jakarta melalui Dinas Pendidikan J...</td>
      <td>pemprov dki jakarta melalui dinas pendidikan j...</td>
      <td>pemprov dki jakarta dinas pendidikan jakarta m...</td>
      <td>pemprov dpr jakarta dinas pendidikan jakarta m...</td>
      <td>pemprov dpr jakarta dinas didik jakarta buka d...</td>
      <td>['pemprov', 'dpr', 'jakarta', 'dinas', 'didik'...</td>
    </tr>
    <tr>
      <th>64</th>
      <td>69</td>
      <td>Netanyahu Bilang Serangan Israel 'Dibenarkan' ...</td>
      <td>politik</td>
      <td>Perdana Menteri (PM) Israel Benjamin Netanyahu...</td>
      <td>Perdana Menteri (PM) Israel Benjamin Netanyahu...</td>
      <td>perdana menteri pm israel benjamin netanyahu m...</td>
      <td>perdana menteri pm israel benjamin netanyahu s...</td>
      <td>perdana menteri dpr israel benjamin netanyahu ...</td>
      <td>perdana menteri dpr israel benjamin netanyahu ...</td>
      <td>['perdana', 'menteri', 'dpr', 'israel', 'benja...</td>
    </tr>
    <tr>
      <th>65</th>
      <td>70</td>
      <td>Polda Banten Ungkap 577 Kasus Narkoba Sepanjan...</td>
      <td>news</td>
      <td>Wakapolda Banten Brigjen Hendra Wirawan menyam...</td>
      <td>Wakapolda Banten Brigjen Hendra Wirawan menyam...</td>
      <td>wakapolda banten brigjen hendra wirawan menyam...</td>
      <td>wakapolda banten brigjen hendra wirawan polda ...</td>
      <td>wakapolda banten brigjen hendra wirawan polda ...</td>
      <td>wakapolda banten brigjen hendra wirawan polda ...</td>
      <td>['wakapolda', 'banten', 'brigjen', 'hendra', '...</td>
    </tr>
    <tr>
      <th>66</th>
      <td>72</td>
      <td>Legislator Sebut RUU Perampasan Aset Bisa Dipe...</td>
      <td>politik</td>
      <td>Anggota Komisi III DPR RI, Sarifuddin Sudding,...</td>
      <td>Anggota Komisi III DPR RI, Sarifuddin Sudding,...</td>
      <td>anggota komisi iii dpr ri sarifuddin sudding m...</td>
      <td>anggota komisi iii dpr ri sarifuddin sudding d...</td>
      <td>anggota polisi tni dpr tni sarifuddin sudding ...</td>
      <td>anggota polisi tni dpr tni sarifuddin sudding ...</td>
      <td>['anggota', 'polisi', 'tni', 'dpr', 'tni', 'sa...</td>
    </tr>
    <tr>
      <th>67</th>
      <td>73</td>
      <td>Kapolda Riau ke Jajaran: Senjata Kita Bukan Se...</td>
      <td>politik</td>
      <td>Kapolda Riau Irjen Herry Heryawan meminta kepa...</td>
      <td>Kapolda Riau Irjen Herry Heryawan meminta kepa...</td>
      <td>kapolda riau irjen herry heryawan meminta kepa...</td>
      <td>kapolda riau irjen herry heryawan jajarannya p...</td>
      <td>kapolda riau irjen herry heryawan jajarannya p...</td>
      <td>kapolda riau irjen herry heryawan jajar layan ...</td>
      <td>['kapolda', 'riau', 'irjen', 'herry', 'heryawa...</td>
    </tr>
    <tr>
      <th>68</th>
      <td>74</td>
      <td>Jalan ke Gunung Salak Bogor Longsor Saat Sedan...</td>
      <td>travel</td>
      <td>Tanah longsor terjadi di Jalan Raya Cikampak-G...</td>
      <td>Tanah longsor terjadi di Jalan Raya Cikampak-G...</td>
      <td>tanah longsor terjadi di jalan raya cikampakgu...</td>
      <td>tanah longsor jalan raya cikampakgunung salak ...</td>
      <td>tanah longsor jalan raya cikampakgunung salak ...</td>
      <td>tanah longsor jalan raya cikampakgunung salak ...</td>
      <td>['tanah', 'longsor', 'jalan', 'raya', 'cikampa...</td>
    </tr>
    <tr>
      <th>69</th>
      <td>75</td>
      <td>Gubernur Banten: Bus Koridor Kota-Kabupaten Se...</td>
      <td>politik</td>
      <td>Gubernur Banten Andra Soni memaparkan rencana ...</td>
      <td>Gubernur Banten Andra Soni memaparkan rencana ...</td>
      <td>gubernur banten andra soni memaparkan rencana ...</td>
      <td>gubernur banten andra soni memaparkan rencana ...</td>
      <td>gubernur banten andra tni memaparkan rencana p...</td>
      <td>gubernur banten andra tni papar rencana bangun...</td>
      <td>['gubernur', 'banten', 'andra', 'tni', 'papar'...</td>
    </tr>
    <tr>
      <th>70</th>
      <td>76</td>
      <td>Penembak Charlie Kirk Sempat Tulis Catatan Ren...</td>
      <td>news</td>
      <td>Penembak Charlie Kirk, Tyler Robinson , ternya...</td>
      <td>Penembak Charlie Kirk, Tyler Robinson , ternya...</td>
      <td>penembak charlie kirk tyler robinson ternyata ...</td>
      <td>penembak charlie kirk tyler robinson pesan tem...</td>
      <td>penembak charlie kpk tyler robinson pasar tema...</td>
      <td>tembak charlie kpk tyler robinson pasar teman ...</td>
      <td>['tembak', 'charlie', 'kpk', 'tyler', 'robinso...</td>
    </tr>
    <tr>
      <th>71</th>
      <td>77</td>
      <td>Komisi Penyelidik PBB Nyatakan Israel Lakukan ...</td>
      <td>politik</td>
      <td>Komisi Penyelidik PBB menyatakan Israel telah ...</td>
      <td>Komisi Penyelidik PBB menyatakan Israel telah ...</td>
      <td>komisi penyelidik pbb menyatakan israel telah ...</td>
      <td>komisi penyelidik pbb israel genosida warga pa...</td>
      <td>polisi penyelidik pbb israel genosida harga pa...</td>
      <td>polisi selidik pbb israel genosida harga pales...</td>
      <td>['polisi', 'selidik', 'pbb', 'israel', 'genosi...</td>
    </tr>
    <tr>
      <th>72</th>
      <td>78</td>
      <td>Video: Israel Lancarkan Serangan Darat-Udara k...</td>
      <td>politik</td>
      <td>Sekjen PBB Antonio Guterres menyebut penghancu...</td>
      <td>Sekjen PBB Antonio Guterres menyebut penghancu...</td>
      <td>sekjen pbb antonio guterres menyebut penghancu...</td>
      <td>sekjen pbb antonio guterres menyebut penghancu...</td>
      <td>sekjen pbb antonio guterres menyebut penghancu...</td>
      <td>sekjen pbb antonio guterres sebut hancur siste...</td>
      <td>['sekjen', 'pbb', 'antonio', 'guterres', 'sebu...</td>
    </tr>
    <tr>
      <th>73</th>
      <td>79</td>
      <td>Misteri Sosok Pembisik Rekening Dormant di Bal...</td>
      <td>news</td>
      <td>Motif kasus penculikan dan pembunuhan kepala c...</td>
      <td>Motif kasus penculikan dan pembunuhan kepala c...</td>
      <td>motif kasus penculikan dan pembunuhan kepala c...</td>
      <td>motif penculikan pembunuhan kepala cabang kaca...</td>
      <td>motif penculikan pembunuhan kepala cabang kaca...</td>
      <td>motif culi bunuh kepala cabang kacab bank nama...</td>
      <td>['motif', 'culi', 'bunuh', 'kepala', 'cabang',...</td>
    </tr>
    <tr>
      <th>74</th>
      <td>80</td>
      <td>Pelindo Sabet Juara Umum Lomba Harhubnas Kemen...</td>
      <td>olahraga</td>
      <td>PT Pelabuhan Indonesia (Persero) atau Pelindo ...</td>
      <td>PT Pelabuhan Indonesia (Persero) atau Pelindo ...</td>
      <td>pt pelabuhan indonesia persero atau pelindo be...</td>
      <td>pt pelabuhan indonesia persero pelindo berhasi...</td>
      <td>dpr pelabuhan indonesia persero pelindo berhas...</td>
      <td>dpr labuh indonesia persero pelindo hasil raih...</td>
      <td>['dpr', 'labuh', 'indonesia', 'persero', 'peli...</td>
    </tr>
    <tr>
      <th>75</th>
      <td>81</td>
      <td>Video: Peran 2 Prajurit Kopassus dalam Penculi...</td>
      <td>news</td>
      <td>Pomdam Jaya menetapkan dua anggota Kopassus, S...</td>
      <td>Pomdam Jaya menetapkan dua anggota Kopassus, S...</td>
      <td>pomdam jaya menetapkan dua anggota kopassus se...</td>
      <td>pomdam jaya menetapkan anggota kopassus serka ...</td>
      <td>pomdam jaksa menetapkan anggota kopassus serka...</td>
      <td>pomdam jaksa tetap anggota kopassus serka tni ...</td>
      <td>['pomdam', 'jaksa', 'tetap', 'anggota', 'kopas...</td>
    </tr>
    <tr>
      <th>76</th>
      <td>82</td>
      <td>Promo Gratis Sambungan Baru Air PAM Jaya untuk...</td>
      <td>travel</td>
      <td>PAM Jaya terus menunjukkan komitmennya dalam m...</td>
      <td>PAM Jaya terus menunjukkan komitmennya dalam m...</td>
      <td>pam jaya terus menunjukkan komitmennya dalam m...</td>
      <td>pam jaya komitmennya menghadirkan layanan air ...</td>
      <td>pam jaksa komitmennya menghadirkan layanan dpr...</td>
      <td>pam jaksa komitmen hadir layan dpr pipa aman s...</td>
      <td>['pam', 'jaksa', 'komitmen', 'hadir', 'layan',...</td>
    </tr>
    <tr>
      <th>77</th>
      <td>83</td>
      <td>Menhan Israel Usai IDF Luncurkan Serangan Dara...</td>
      <td>politik</td>
      <td>Militer Israel melancarkan serangan darat ke w...</td>
      <td>Militer Israel melancarkan serangan darat ke w...</td>
      <td>militer israel melancarkan serangan darat ke w...</td>
      <td>militer israel melancarkan serangan darat wila...</td>
      <td>militer israel melancarkan serangan darat wila...</td>
      <td>militer israel lancar serang darat wilayah gaz...</td>
      <td>['militer', 'israel', 'lancar', 'serang', 'dar...</td>
    </tr>
    <tr>
      <th>78</th>
      <td>84</td>
      <td>Kerugian Israel Jika Negara Arab dan Muslim Pu...</td>
      <td>news</td>
      <td>Para pemimpin Arab dan Muslim menyerukan untuk...</td>
      <td>Para pemimpin Arab dan Muslim menyerukan untuk...</td>
      <td>para pemimpin arab dan muslim menyerukan untuk...</td>
      <td>pemimpin arab muslim menyerukan meninjau ulang...</td>
      <td>pemimpin arab muslim menyerukan meninjau ulang...</td>
      <td>pimpin arab muslim seru tinjau ulang hubung di...</td>
      <td>['pimpin', 'arab', 'muslim', 'seru', 'tinjau',...</td>
    </tr>
    <tr>
      <th>79</th>
      <td>85</td>
      <td>Adaptasi Jadi Kunci Kejaksaan Rote Ndao Jaga K...</td>
      <td>news</td>
      <td>Penegakan hukum di daerah perbatasan sering be...</td>
      <td>Penegakan hukum di daerah perbatasan sering be...</td>
      <td>penegakan hukum di daerah perbatasan sering be...</td>
      <td>penegakan hukum daerah perbatasan berhadapan k...</td>
      <td>penegakan hukum daerah perbatasan berhadapan k...</td>
      <td>tega hukum daerah batas hadap batas fasilitas ...</td>
      <td>['tega', 'hukum', 'daerah', 'batas', 'hadap', ...</td>
    </tr>
    <tr>
      <th>80</th>
      <td>86</td>
      <td>Wamensos Agus Jabo Serahkan Bantuan ke Korban ...</td>
      <td>politik</td>
      <td>Wakil Menteri Sosial (Wamensos), Agus Jabo Pri...</td>
      <td>Wakil Menteri Sosial (Wamensos), Agus Jabo Pri...</td>
      <td>wakil menteri sosial wamensos agus jabo priyon...</td>
      <td>wakil menteri sosial wamensos agus jabo priyon...</td>
      <td>wakil menteri sosial wamensos agus jabo priyon...</td>
      <td>wakil menteri sosial wamensos agus jabo priyon...</td>
      <td>['wakil', 'menteri', 'sosial', 'wamensos', 'ag...</td>
    </tr>
    <tr>
      <th>81</th>
      <td>87</td>
      <td>10 Fakta Perkara Kacab Bank Dibunuh: Motif hin...</td>
      <td>news</td>
      <td>Fakta-fakta terkait kasus penculikan dan pembu...</td>
      <td>Fakta-fakta terkait kasus penculikan dan pembu...</td>
      <td>faktafakta terkait kasus penculikan dan pembun...</td>
      <td>faktafakta terkait penculikan pembunuhan kepal...</td>
      <td>faktafakta terkait penculikan pembunuhan kepal...</td>
      <td>faktafakta kait culi bunuh kepala cabang kacab...</td>
      <td>['faktafakta', 'kait', 'culi', 'bunuh', 'kepal...</td>
    </tr>
    <tr>
      <th>82</th>
      <td>88</td>
      <td>Program Jaksa Jaga Desa, Upaya Preventif Tekan...</td>
      <td>news</td>
      <td>Simeulue, salah satu pulau terluar Aceh, menja...</td>
      <td>Simeulue, salah satu pulau terluar Aceh, menja...</td>
      <td>simeulue salah satu pulau terluar aceh menjadi...</td>
      <td>simeulue salah pulau terluar aceh wilayah pele...</td>
      <td>simeulue salah pulau terluar aceh wilayah pele...</td>
      <td>simeulue salah pulau luar aceh wilayah leceh s...</td>
      <td>['simeulue', 'salah', 'pulau', 'luar', 'aceh',...</td>
    </tr>
    <tr>
      <th>83</th>
      <td>90</td>
      <td>6.118 Personel Gabungan Dikerahkan Kawal Demo ...</td>
      <td>politik</td>
      <td>Asosiasi pengemudi ojek online ( ojol ) akan m...</td>
      <td>Asosiasi pengemudi ojek online ( ojol ) akan m...</td>
      <td>asosiasi pengemudi ojek online ojol akan mengg...</td>
      <td>asosiasi pengemudi ojek online ojol menggelar ...</td>
      <td>asosiasi pengemudi ojek online gol menggelar d...</td>
      <td>asosiasi kemudi ojek online gol gelar demonstr...</td>
      <td>['asosiasi', 'kemudi', 'ojek', 'online', 'gol'...</td>
    </tr>
    <tr>
      <th>84</th>
      <td>91</td>
      <td>Israel Serang Gaza: 91 Orang Tewas dan 17 Bang...</td>
      <td>travel</td>
      <td>Tentara Israel terus menggempur wilayah Kota G...</td>
      <td>Tentara Israel terus menggempur wilayah Kota G...</td>
      <td>tentara israel terus menggempur wilayah kota g...</td>
      <td>tentara israel menggempur wilayah kota gaza br...</td>
      <td>tentara israel menggempur wilayah kota gaza br...</td>
      <td>tentara israel gempur wilayah kota gaza brutal...</td>
      <td>['tentara', 'israel', 'gempur', 'wilayah', 'ko...</td>
    </tr>
    <tr>
      <th>85</th>
      <td>92</td>
      <td>Pimpinan Ponpes Tampar Santri gegara Tak Disal...</td>
      <td>news</td>
      <td>Pimpinan pondok pesantren (ponpes) di Palopo, ...</td>
      <td>Pimpinan pondok pesantren (ponpes) di Palopo, ...</td>
      <td>pimpinan pondok pesantren ponpes di palopo sul...</td>
      <td>pimpinan pondok pesantren ponpes palopo sulawe...</td>
      <td>pimpinan pondok pesantren ponpes palopo sulawe...</td>
      <td>pimpin pondok pesantren ponpes palopo sulawesi...</td>
      <td>['pimpin', 'pondok', 'pesantren', 'ponpes', 'p...</td>
    </tr>
    <tr>
      <th>86</th>
      <td>93</td>
      <td>Mobil Boks Terbalik di Tol Jagorawi Arah Bogor...</td>
      <td>travel</td>
      <td>Kecelakaan lalu lintas melibatkan mobil boks d...</td>
      <td>Kecelakaan lalu lintas melibatkan mobil boks d...</td>
      <td>kecelakaan lalu lintas melibatkan mobil boks d...</td>
      <td>kecelakaan lintas melibatkan mobil boks tol ja...</td>
      <td>kecelakaan lintas melibatkan mobil boks gol ja...</td>
      <td>celaka lintas libat mobil boks gol jagorawi kp...</td>
      <td>['celaka', 'lintas', 'libat', 'mobil', 'boks',...</td>
    </tr>
    <tr>
      <th>87</th>
      <td>95</td>
      <td>Hangatnya Sambutan Dasco ke Sjafrie di Senayan</td>
      <td>politik</td>
      <td>Sambutan hangat diberikan Wakil Ketua DPR Sufm...</td>
      <td>Sambutan hangat diberikan Wakil Ketua DPR Sufm...</td>
      <td>sambutan hangat diberikan wakil ketua dpr sufm...</td>
      <td>sambutan hangat wakil ketua dpr sufmi dasco ah...</td>
      <td>sambutan harga wakil ketua dpr sufmi dasco ahm...</td>
      <td>sambut harga wakil ketua dpr sufmi dasco ahmad...</td>
      <td>['sambut', 'harga', 'wakil', 'ketua', 'dpr', '...</td>
    </tr>
    <tr>
      <th>88</th>
      <td>96</td>
      <td>Tinjau SRMA di Bengkulu, Menko AHY Pastikan Ak...</td>
      <td>politik</td>
      <td>Menteri Koordinator Bidang Infrastruktur dan P...</td>
      <td>Menteri Koordinator Bidang Infrastruktur dan P...</td>
      <td>menteri koordinator bidang infrastruktur dan p...</td>
      <td>menteri koordinator bidang infrastruktur pemba...</td>
      <td>menteri koordinator bidang infrastruktur pemba...</td>
      <td>menteri koordinator bidang infrastruktur bangu...</td>
      <td>['menteri', 'koordinator', 'bidang', 'infrastr...</td>
    </tr>
    <tr>
      <th>89</th>
      <td>97</td>
      <td>Maju Mundur KPU soal Aturan Kerahasiaan Dokume...</td>
      <td>politik</td>
      <td>Komisi Pemilihan Umum (KPU) membatalkan Keputu...</td>
      <td>Komisi Pemilihan Umum (KPU) membatalkan Keputu...</td>
      <td>komisi pemilihan umum kpu membatalkan keputusa...</td>
      <td>komisi pemilihan kpu membatalkan keputusan kpu...</td>
      <td>polisi pemilihan kpk membatalkan keputusan kpk...</td>
      <td>polisi pilih kpk batal putus kpk tni nomor tet...</td>
      <td>['polisi', 'pilih', 'kpk', 'batal', 'putus', '...</td>
    </tr>
    <tr>
      <th>90</th>
      <td>98</td>
      <td>Titik-titik Kepadatan Lalin di Tol Arah Jakart...</td>
      <td>news</td>
      <td>Lalu lintas di sejumlah ruas tol arah Jakarta ...</td>
      <td>Lalu lintas di sejumlah ruas tol arah Jakarta ...</td>
      <td>lalu lintas di sejumlah ruas tol arah jakarta ...</td>
      <td>lintas ruas tol arah jakarta pagi mengalami ke...</td>
      <td>lintas rumah gol arah jakarta pagi mengalami k...</td>
      <td>lintas rumah gol arah jakarta pagi alami padat...</td>
      <td>['lintas', 'rumah', 'gol', 'arah', 'jakarta', ...</td>
    </tr>
    <tr>
      <th>91</th>
      <td>99</td>
      <td>800 Ribu Orang Akan Demo Protes Kebijakan Pres...</td>
      <td>politik</td>
      <td>Sebanyak 800.000 orang diperkirakan menggelar ...</td>
      <td>Sebanyak 800.000 orang diperkirakan menggelar ...</td>
      <td>sebanyak orang diperkirakan menggelar aksi dem...</td>
      <td>orang menggelar aksi demonstrasi prancis pemer...</td>
      <td>orang menggelar jaksa demonstrasi prancis peme...</td>
      <td>orang gelar jaksa demonstrasi prancis perintah...</td>
      <td>['orang', 'gelar', 'jaksa', 'demonstrasi', 'pr...</td>
    </tr>
    <tr>
      <th>92</th>
      <td>100</td>
      <td>Keluarga Minta Penculik-Pembunuh Kacab Bank Di...</td>
      <td>news</td>
      <td>Polda Metro Jaya telah menetapkan 15 orang seb...</td>
      <td>Polda Metro Jaya telah menetapkan 15 orang seb...</td>
      <td>polda metro jaya telah menetapkan orang sebaga...</td>
      <td>polda metro jaya menetapkan orang tersangka pe...</td>
      <td>polda metro jaksa menetapkan orang tersangka p...</td>
      <td>polda metro jaksa tetap orang sangka culi bunu...</td>
      <td>['polda', 'metro', 'jaksa', 'tetap', 'orang', ...</td>
    </tr>
    <tr>
      <th>93</th>
      <td>101</td>
      <td>Pembunuh Charlie Kirk Terancam Hukuman Mati</td>
      <td>politik</td>
      <td>Pembunuh Charlie Kirk , Tyler Robinson, didakw...</td>
      <td>Pembunuh Charlie Kirk , Tyler Robinson, didakw...</td>
      <td>pembunuh charlie kirk tyler robinson didakwa d...</td>
      <td>pembunuh charlie kirk tyler robinson didakwa t...</td>
      <td>pembunuh charlie kpk tyler robinson didakwa tu...</td>
      <td>bunuh charlie kpk tyler robinson dakwa tujuh d...</td>
      <td>['bunuh', 'charlie', 'kpk', 'tyler', 'robinson...</td>
    </tr>
    <tr>
      <th>94</th>
      <td>103</td>
      <td>Aturan Heboh KPU Rahasiakan Ijazah Capres-Cawa...</td>
      <td>politik</td>
      <td>Publik dihebohkan dengan aturan KPU yang menye...</td>
      <td>Publik dihebohkan dengan aturan KPU yang menye...</td>
      <td>publik dihebohkan dengan aturan kpu yang menye...</td>
      <td>publik dihebohkan aturan kpu ijazah calon pres...</td>
      <td>publik dihebohkan aturan kpk ijazah calon pres...</td>
      <td>publik heboh atur kpk ijazah calon presiden ca...</td>
      <td>['publik', 'heboh', 'atur', 'kpk', 'ijazah', '...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>104</td>
      <td>Polemik Ijazah Capres, Komisi II DPR Minta KPU...</td>
      <td>politik</td>
      <td>KPU resmi membatalkan aturan ijazah calon pres...</td>
      <td>KPU resmi membatalkan aturan ijazah calon pres...</td>
      <td>kpu resmi membatalkan aturan ijazah calon pres...</td>
      <td>kpu resmi membatalkan aturan ijazah calon pres...</td>
      <td>kpk resmi membatalkan aturan ijazah calon pres...</td>
      <td>kpk resmi batal atur ijazah calon presiden cap...</td>
      <td>['kpk', 'resmi', 'batal', 'atur', 'ijazah', 'c...</td>
    </tr>
    <tr>
      <th>96</th>
      <td>105</td>
      <td>Jembatan Apung Ini Jadi Penyelamat Akses Antar...</td>
      <td>news</td>
      <td>Bekasi - Warga Sukamekar, Bekasi, bangun jemba...</td>
      <td>Bekasi - Warga Sukamekar, Bekasi, bangun jemba...</td>
      <td>bekasi warga sukamekar bekasi bangun jembatan ...</td>
      <td>bekasi warga sukamekar bekasi bangun jembatan ...</td>
      <td>bekasi harga sukamekar bekasi bangun jembatan ...</td>
      <td>bekas harga sukamekar bekas bangun jembatan ap...</td>
      <td>['bekas', 'harga', 'sukamekar', 'bekas', 'bang...</td>
    </tr>
    <tr>
      <th>97</th>
      <td>106</td>
      <td>Buka-bukaan KPK Balas Rudy Tanoesoedibjo yang ...</td>
      <td>news</td>
      <td>KPK buka-bukaan membalas Bambang Rudijanto Tan...</td>
      <td>KPK buka-bukaan membalas Bambang Rudijanto Tan...</td>
      <td>kpk bukabukaan membalas bambang rudijanto tano...</td>
      <td>kpk bukabukaan membalas bambang rudijanto tano...</td>
      <td>kpk bukabukaan membalas bambang rudijanto tano...</td>
      <td>kpk bukabukaan balas bambang rudijanto tanoeso...</td>
      <td>['kpk', 'bukabukaan', 'balas', 'bambang', 'rud...</td>
    </tr>
    <tr>
      <th>98</th>
      <td>107</td>
      <td>Ada Demo Ojol di DPR-Kemenhub Hari Ini, Rekaya...</td>
      <td>politik</td>
      <td>Asosiasi pengemudi ojek online ( ojol ) akan m...</td>
      <td>Asosiasi pengemudi ojek online ( ojol ) akan m...</td>
      <td>asosiasi pengemudi ojek online ojol akan mengg...</td>
      <td>asosiasi pengemudi ojek online ojol menggelar ...</td>
      <td>asosiasi pengemudi ojek online gol menggelar d...</td>
      <td>asosiasi kemudi ojek online gol gelar demonstr...</td>
      <td>['asosiasi', 'kemudi', 'ojek', 'online', 'gol'...</td>
    </tr>
    <tr>
      <th>99</th>
      <td>108</td>
      <td>Trump Perpanjang Tunda Blokir TikTok di AS hin...</td>
      <td>olahraga</td>
      <td>Presiden Amerika Serikat (AS) Donald Trump mem...</td>
      <td>Presiden Amerika Serikat (AS) Donald Trump mem...</td>
      <td>presiden amerika serikat as donald trump mempe...</td>
      <td>presiden amerika serikat as donald trump mempe...</td>
      <td>presiden amerika serikat as donald trump mempe...</td>
      <td>presiden amerika serikat as donald trump panja...</td>
      <td>['presiden', 'amerika', 'serikat', 'as', 'dona...</td>
    </tr>
    <tr>
      <th>100</th>
      <td>110</td>
      <td>Teror Video Seks AI Sasar Menteri-Politisi Mal...</td>
      <td>olahraga</td>
      <td>Sejumlah menteri dan politisi Malaysia mendapa...</td>
      <td>Sejumlah menteri dan politisi Malaysia mendapa...</td>
      <td>sejumlah menteri dan politisi malaysia mendapa...</td>
      <td>menteri politisi malaysia teror video sex pals...</td>
      <td>menteri polisi malaysia teror video sex palsu ...</td>
      <td>menteri polisi malaysia teror video sex palsu ...</td>
      <td>['menteri', 'polisi', 'malaysia', 'teror', 'vi...</td>
    </tr>
    <tr>
      <th>101</th>
      <td>111</td>
      <td>Video Netanyahu Sebut Israel Serang Pelabuhan ...</td>
      <td>politik</td>
      <td>Israel melancarkan serangan ke Pelabuhan Hodei...</td>
      <td>Israel melancarkan serangan ke Pelabuhan Hodei...</td>
      <td>israel melancarkan serangan ke pelabuhan hodei...</td>
      <td>israel melancarkan serangan pelabuhan hodeidah...</td>
      <td>israel melancarkan serangan pelabuhan hodeidah...</td>
      <td>israel lancar serang labuh hodeidah laut merah...</td>
      <td>['israel', 'lancar', 'serang', 'labuh', 'hodei...</td>
    </tr>
    <tr>
      <th>102</th>
      <td>113</td>
      <td>Video: Israel Lancarkan Serangan Darat-Udara k...</td>
      <td>health</td>
      <td>Militer Israel mulai melancarkan serangan dara...</td>
      <td>Militer Israel mulai melancarkan serangan dara...</td>
      <td>militer israel mulai melancarkan serangan dara...</td>
      <td>militer israel melancarkan serangan darat kota...</td>
      <td>militer israel melancarkan serangan darat kota...</td>
      <td>militer israel lancar serang darat kota gaza s...</td>
      <td>['militer', 'israel', 'lancar', 'serang', 'dar...</td>
    </tr>
    <tr>
      <th>103</th>
      <td>115</td>
      <td>Video: Viral Warga Adang Truk Hingga Kejar Pet...</td>
      <td>health</td>
      <td>Viral di media sosial sejumlah warga mengadang...</td>
      <td>Viral di media sosial sejumlah warga mengadang...</td>
      <td>viral di media sosial sejumlah warga mengadang...</td>
      <td>viral media sosial warga mengadang truk tamban...</td>
      <td>viral medali sosial harga mengadang truk tamba...</td>
      <td>viral medali sosial harga adang truk tambang l...</td>
      <td>['viral', 'medali', 'sosial', 'harga', 'adang'...</td>
    </tr>
    <tr>
      <th>104</th>
      <td>116</td>
      <td>Mesir Kutuk Serangan Darat Israel ke Kota Gaza...</td>
      <td>health</td>
      <td>Mesir mengutuk serangan darat militer Israel y...</td>
      <td>Mesir mengutuk serangan darat militer Israel y...</td>
      <td>mesir mengutuk serangan darat militer israel y...</td>
      <td>mesir mengutuk serangan darat militer israel d...</td>
      <td>mesir mengutuk serangan darat militer israel d...</td>
      <td>mesir kutuk serang darat militer israel lancar...</td>
      <td>['mesir', 'kutuk', 'serang', 'darat', 'militer...</td>
    </tr>
    <tr>
      <th>105</th>
      <td>117</td>
      <td>Kongres Pluralisme di Kazakhstan, Kasus Perusa...</td>
      <td>news</td>
      <td>Rangkaian VIII Congress of Leaders of World an...</td>
      <td>Rangkaian VIII Congress of Leaders of World an...</td>
      <td>rangkaian viii congress of leaders of world an...</td>
      <td>rangkaian viii congress of leaders of world an...</td>
      <td>rangkaian viii congress gol leaders gol world ...</td>
      <td>rangkai viii congress gol leaders gol world tn...</td>
      <td>['rangkai', 'viii', 'congress', 'gol', 'leader...</td>
    </tr>
    <tr>
      <th>106</th>
      <td>120</td>
      <td>Israel Lancarkan Serangan Darat ke Kota Gaza, ...</td>
      <td>health</td>
      <td>Militer Israel mulai melancarkan serangan dara...</td>
      <td>Militer Israel mulai melancarkan serangan dara...</td>
      <td>militer israel mulai melancarkan serangan dara...</td>
      <td>militer israel melancarkan serangan darat kota...</td>
      <td>militer israel melancarkan serangan darat kota...</td>
      <td>militer israel lancar serang darat kota gaza p...</td>
      <td>['militer', 'israel', 'lancar', 'serang', 'dar...</td>
    </tr>
    <tr>
      <th>107</th>
      <td>121</td>
      <td>HUT ke-15 BNPP, Mendagri: Terus Jaga Komitmen ...</td>
      <td>news</td>
      <td>Badan Nasional Pengelola Perbatasan ( BNPP ) m...</td>
      <td>Badan Nasional Pengelola Perbatasan ( BNPP ) m...</td>
      <td>badan nasional pengelola perbatasan bnpp mengg...</td>
      <td>badan nasional pengelola perbatasan bnpp mengg...</td>
      <td>badan nasional pengelola perbatasan bnpp mengg...</td>
      <td>badan nasional kelola batas bnpp gelar sakit s...</td>
      <td>['badan', 'nasional', 'kelola', 'batas', 'bnpp...</td>
    </tr>
    <tr>
      <th>108</th>
      <td>122</td>
      <td>Video: Alasan Tersangka Penculikan Kacab Bank ...</td>
      <td>news</td>
      <td>Polisi mengungkap alasan para tersangka pencul...</td>
      <td>Polisi mengungkap alasan para tersangka pencul...</td>
      <td>polisi mengungkap alasan para tersangka pencul...</td>
      <td>polisi mengungkap alasan tersangka penculikan ...</td>
      <td>polisi mengungkap alasan tersangka penculikan ...</td>
      <td>polisi ungkap alas sangka culi ujung kacab ban...</td>
      <td>['polisi', 'ungkap', 'alas', 'sangka', 'culi',...</td>
    </tr>
    <tr>
      <th>109</th>
      <td>123</td>
      <td>Koalisi Sipil Kritik Menhan soal TNI Jaga Gedu...</td>
      <td>politik</td>
      <td>Menteri Pertahanan ( Menhan ) Sjafri Sjamsuddi...</td>
      <td>Menteri Pertahanan ( Menhan ) Sjafri Sjamsuddi...</td>
      <td>menteri pertahanan menhan sjafri sjamsuddin me...</td>
      <td>menteri pertahanan menhan sjafri sjamsuddin me...</td>
      <td>menteri pertahanan menhan sjafri sjamsuddin me...</td>
      <td>menteri tahan menhan sjafri sjamsuddin aku tuj...</td>
      <td>['menteri', 'tahan', 'menhan', 'sjafri', 'sjam...</td>
    </tr>
    <tr>
      <th>110</th>
      <td>124</td>
      <td>Diterjang Puting Beliung, Masjid dan 14 Rumah ...</td>
      <td>news</td>
      <td>Bencana puting beliung menerjang kawasan permu...</td>
      <td>Bencana puting beliung menerjang kawasan permu...</td>
      <td>bencana puting beliung menerjang kawasan permu...</td>
      <td>bencana puting beliung menerjang kawasan permu...</td>
      <td>bencana puting beliung menerjang kawasan permu...</td>
      <td>bencana puting beliung terjang kawasan mukim k...</td>
      <td>['bencana', 'puting', 'beliung', 'terjang', 'k...</td>
    </tr>
    <tr>
      <th>111</th>
      <td>125</td>
      <td>KPU Batalkan Aturan Ijazah Capres Dokumen Raha...</td>
      <td>politik</td>
      <td>Anggota Komisi II DPR RI Muhammad Khozin menya...</td>
      <td>Anggota Komisi II DPR RI Muhammad Khozin menya...</td>
      <td>anggota komisi ii dpr ri muhammad khozin menya...</td>
      <td>anggota komisi ii dpr ri muhammad khozin menya...</td>
      <td>anggota polisi tni dpr tni muhammad khozin men...</td>
      <td>anggota polisi tni dpr tni muhammad khozin sam...</td>
      <td>['anggota', 'polisi', 'tni', 'dpr', 'tni', 'mu...</td>
    </tr>
    <tr>
      <th>112</th>
      <td>126</td>
      <td>Mobil Ugal-ugalan Seruduk 3 Motor Berakhir Dia...</td>
      <td>news</td>
      <td>Mobil jenis LCGC Ayla F-1814-JK menyeruduk tig...</td>
      <td>Mobil jenis LCGC Ayla F-1814-JK menyeruduk tig...</td>
      <td>mobil jenis lcgc ayla f jk menyeruduk tiga mot...</td>
      <td>mobil jenis lcgc ayla f jk menyeruduk motor su...</td>
      <td>mobil jenis liga ayla f kpk menyeruduk motor s...</td>
      <td>mobil jenis liga ayla f kpk seruduk motor suka...</td>
      <td>['mobil', 'jenis', 'liga', 'ayla', 'f', 'kpk',...</td>
    </tr>
    <tr>
      <th>113</th>
      <td>127</td>
      <td>Melihat Republic Kazakhstan Grand Mosque, Masj...</td>
      <td>politik</td>
      <td>Bangunan Republic Kazakhstan Grand Mosque berd...</td>
      <td>Bangunan Republic Kazakhstan Grand Mosque berd...</td>
      <td>bangunan republic kazakhstan grand mosque berd...</td>
      <td>bangunan republic kazakhstan grand mosque berd...</td>
      <td>bangunan republic kazakhstan grand mosque berd...</td>
      <td>bangun republic kazakhstan grand mosque diri k...</td>
      <td>['bangun', 'republic', 'kazakhstan', 'grand', ...</td>
    </tr>
    <tr>
      <th>114</th>
      <td>128</td>
      <td>1 Orang Tewas dalam Insiden Mobil Ugal-ugalan ...</td>
      <td>travel</td>
      <td>Mobil jenis LCGC menabrak sejumlah motor di Su...</td>
      <td>Mobil jenis LCGC menabrak sejumlah motor di Su...</td>
      <td>mobil jenis lcgc menabrak sejumlah motor di su...</td>
      <td>mobil jenis lcgc menabrak motor sukaraja bogor...</td>
      <td>mobil jenis liga menabrak motor sukaraja bogor...</td>
      <td>mobil jenis liga tabrak motor sukaraja bogor j...</td>
      <td>['mobil', 'jenis', 'liga', 'tabrak', 'motor', ...</td>
    </tr>
    <tr>
      <th>115</th>
      <td>129</td>
      <td>Penipu Asal Israel Simon Leviev 'Tinder Swindl...</td>
      <td>politik</td>
      <td>Penipu asal Israel Simon Leviev (34) ditangkap...</td>
      <td>Penipu asal Israel Simon Leviev (34) ditangkap...</td>
      <td>penipu asal israel simon leviev ditangkap di g...</td>
      <td>penipu israel simon leviev ditangkap georgia a...</td>
      <td>penipu israel simon leviev ditangkap georgia a...</td>
      <td>tipu israel simon leviev tangkap georgia ameri...</td>
      <td>['tipu', 'israel', 'simon', 'leviev', 'tangkap...</td>
    </tr>
    <tr>
      <th>116</th>
      <td>130</td>
      <td>Warga AS Gelar Doa dan Lilin untuk Aktivis Kon...</td>
      <td>news</td>
      <td>Amerika Serikat - Suasana haru menyelimuti hal...</td>
      <td>Amerika Serikat - Suasana haru menyelimuti hal...</td>
      <td>amerika serikat suasana haru menyelimuti halam...</td>
      <td>amerika serikat suasana haru menyelimuti halam...</td>
      <td>amerika serikat suasana harga menyelimuti hala...</td>
      <td>amerika serikat suasana harga limut halaman ge...</td>
      <td>['amerika', 'serikat', 'suasana', 'harga', 'li...</td>
    </tr>
    <tr>
      <th>117</th>
      <td>131</td>
      <td>Baleg DPR Bicara Kehati-hatian dalam Pembahasa...</td>
      <td>politik</td>
      <td>Wakil Ketua Badan Legislasi (Baleg) DPR RI Stu...</td>
      <td>Wakil Ketua Badan Legislasi (Baleg) DPR RI Stu...</td>
      <td>wakil ketua badan legislasi baleg dpr ri sturm...</td>
      <td>wakil ketua badan legislasi baleg dpr ri sturm...</td>
      <td>wakil ketua badan legislasi baleg dpr tni stur...</td>
      <td>wakil ketua badan legislasi baleg dpr tni stur...</td>
      <td>['wakil', 'ketua', 'badan', 'legislasi', 'bale...</td>
    </tr>
    <tr>
      <th>118</th>
      <td>132</td>
      <td>Korban Tewas Kecelakaan Bus Rombongan Nakes di...</td>
      <td>travel</td>
      <td>Korban tewas kecelakaan bus yang membawa rombo...</td>
      <td>Korban tewas kecelakaan bus yang membawa rombo...</td>
      <td>korban tewas kecelakaan bus yang membawa rombo...</td>
      <td>korban tewas kecelakaan bus membawa rombongan ...</td>
      <td>korban tewas kecelakaan bus membawa rombongan ...</td>
      <td>korban tewas celaka bus bawa rombong rumah sak...</td>
      <td>['korban', 'tewas', 'celaka', 'bus', 'bawa', '...</td>
    </tr>
    <tr>
      <th>119</th>
      <td>133</td>
      <td>Dirut BUMD di Serang Jadi Tersangka Kasus Koru...</td>
      <td>news</td>
      <td>Kejaksaan Negeri (Kejari) Serang menetapkan Di...</td>
      <td>Kejaksaan Negeri (Kejari) Serang menetapkan Di...</td>
      <td>kejaksaan negeri kejari serang menetapkan dire...</td>
      <td>kejaksaan negeri kejari serang menetapkan dire...</td>
      <td>kejaksaan negeri kejari serang menetapkan dire...</td>
      <td>jaksa negeri kejar serang tetap direktur utama...</td>
      <td>['jaksa', 'negeri', 'kejar', 'serang', 'tetap'...</td>
    </tr>
    <tr>
      <th>120</th>
      <td>134</td>
      <td>Viral Pria Pukul-pukul Mobil gegara Parkir di ...</td>
      <td>news</td>
      <td>Rekaman video yang dinarasikan dua pria memuku...</td>
      <td>Rekaman video yang dinarasikan dua pria memuku...</td>
      <td>rekaman video yang dinarasikan dua pria memuku...</td>
      <td>rekaman video dinarasikan pria memukul mobil g...</td>
      <td>rekaman video dinarasikan pria memukul mobil g...</td>
      <td>rekam video narasi pria pukul mobil garagara p...</td>
      <td>['rekam', 'video', 'narasi', 'pria', 'pukul', ...</td>
    </tr>
    <tr>
      <th>121</th>
      <td>135</td>
      <td>Kejati Banten Tangkap Buron Penipuan ke Perusa...</td>
      <td>news</td>
      <td>Kejaksaan Tinggi (Kejati) Banten menangkap Joh...</td>
      <td>Kejaksaan Tinggi (Kejati) Banten menangkap Joh...</td>
      <td>kejaksaan tinggi kejati banten menangkap johnn...</td>
      <td>kejaksaan kejati banten menangkap johnny kaind...</td>
      <td>kejaksaan kejati banten menangkap johnny kaind...</td>
      <td>jaksa kejat banten tangkap johnny kainde alias...</td>
      <td>['jaksa', 'kejat', 'banten', 'tangkap', 'johnn...</td>
    </tr>
    <tr>
      <th>122</th>
      <td>136</td>
      <td>Hadiri Maulid Nabi, Fadli Zon Tekankan Nilai B...</td>
      <td>politik</td>
      <td>Kementerian Kebudayaan, Kementerian Pendidikan...</td>
      <td>Kementerian Kebudayaan, Kementerian Pendidikan...</td>
      <td>kementerian kebudayaan kementerian pendidikan ...</td>
      <td>kementerian kebudayaan kementerian pendidikan ...</td>
      <td>kementerian kebudayaan kementerian pendidikan ...</td>
      <td>menteri budaya menteri didik pasar tengah ment...</td>
      <td>['menteri', 'budaya', 'menteri', 'didik', 'pas...</td>
    </tr>
    <tr>
      <th>123</th>
      <td>137</td>
      <td>Mengemuka Sosok Dukun Pengganda Duit di Balik ...</td>
      <td>news</td>
      <td>Polisi membongkar gudang uang dolar palsu di A...</td>
      <td>Polisi membongkar gudang uang dolar palsu di A...</td>
      <td>polisi membongkar gudang uang dolar palsu di a...</td>
      <td>polisi membongkar gudang uang dolar palsu apar...</td>
      <td>polisi membongkar gudang uang dolar palsu apar...</td>
      <td>polisi bongkar gudang uang dolar palsu apartem...</td>
      <td>['polisi', 'bongkar', 'gudang', 'uang', 'dolar...</td>
    </tr>
    <tr>
      <th>124</th>
      <td>138</td>
      <td>Viral Warga di Parung Panjang Hadang Truk Tamb...</td>
      <td>health</td>
      <td>Video sejumlah warga mengejar petugas Dinas Pe...</td>
      <td>Video sejumlah warga mengejar petugas Dinas Pe...</td>
      <td>video sejumlah warga mengejar petugas dinas pe...</td>
      <td>video warga mengejar petugas dinas perhubungan...</td>
      <td>video harga mengejar petugas dinas perhubungan...</td>
      <td>video harga kejar tugas dinas hubung dishub ka...</td>
      <td>['video', 'harga', 'kejar', 'tugas', 'dinas', ...</td>
    </tr>
    <tr>
      <th>125</th>
      <td>139</td>
      <td>Green Satkamling di Dumai, Wujudkan Keamanan d...</td>
      <td>news</td>
      <td>Polres Dumai melalui Polsek Batu Kapur menghid...</td>
      <td>Polres Dumai melalui Polsek Batu Kapur menghid...</td>
      <td>polres dumai melalui polsek batu kapur menghid...</td>
      <td>polres dumai polsek batu kapur menghidupkan sa...</td>
      <td>polres rumah polsek batu kapur menghidupkan sa...</td>
      <td>polres rumah polsek batu kapur hidup satkamlin...</td>
      <td>['polres', 'rumah', 'polsek', 'batu', 'kapur',...</td>
    </tr>
    <tr>
      <th>126</th>
      <td>140</td>
      <td>Perlawanan Terakhir Kacab Bank Saat Dianiaya d...</td>
      <td>news</td>
      <td>Polisi mengungkap kepala cabang (kacab) bank ,...</td>
      <td>Polisi mengungkap kepala cabang (kacab) bank ,...</td>
      <td>polisi mengungkap kepala cabang kacab bank m i...</td>
      <td>polisi mengungkap kepala cabang kacab bank m i...</td>
      <td>polisi mengungkap kepala cabang kacab bank m i...</td>
      <td>polisi ungkap kepala cabang kacab bank m ilham...</td>
      <td>['polisi', 'ungkap', 'kepala', 'cabang', 'kaca...</td>
    </tr>
    <tr>
      <th>127</th>
      <td>141</td>
      <td>Embung Kemang Utara Dibangun Rp10,9 Miliar unt...</td>
      <td>politik</td>
      <td>Jakarta - Pemerintah Kota Administrasi Jakarta...</td>
      <td>Jakarta - Pemerintah Kota Administrasi Jakarta...</td>
      <td>jakarta pemerintah kota administrasi jakarta s...</td>
      <td>jakarta pemerintah kota administrasi jakarta s...</td>
      <td>jakarta pemerintah kota administrasi jakarta s...</td>
      <td>jakarta perintah kota administrasi jakarta sel...</td>
      <td>['jakarta', 'perintah', 'kota', 'administrasi'...</td>
    </tr>
    <tr>
      <th>128</th>
      <td>142</td>
      <td>Video Peran Lengkap Tersangka Pembunuhan Kacab...</td>
      <td>politik</td>
      <td>Polda Metro Jaya mengungkapkan peran 15 tersan...</td>
      <td>Polda Metro Jaya mengungkapkan peran 15 tersan...</td>
      <td>polda metro jaya mengungkapkan peran tersangka...</td>
      <td>polda metro jaya peran tersangka penculikan me...</td>
      <td>polda metro jaksa peran tersangka penculikan m...</td>
      <td>polda metro jaksa peran sangka culi akibat kep...</td>
      <td>['polda', 'metro', 'jaksa', 'peran', 'sangka',...</td>
    </tr>
    <tr>
      <th>129</th>
      <td>143</td>
      <td>AHY Pastikan Percepatan Konektivitas Bengkulu-...</td>
      <td>politik</td>
      <td>Menteri Koordinator Bidang Infrastruktur dan P...</td>
      <td>Menteri Koordinator Bidang Infrastruktur dan P...</td>
      <td>menteri koordinator bidang infrastruktur dan p...</td>
      <td>menteri koordinator bidang infrastruktur pemba...</td>
      <td>menteri koordinator bidang infrastruktur pemba...</td>
      <td>menteri koordinator bidang infrastruktur bangu...</td>
      <td>['menteri', 'koordinator', 'bidang', 'infrastr...</td>
    </tr>
    <tr>
      <th>130</th>
      <td>144</td>
      <td>Video: 2 Kopassus Terlibat Pembunuhan Kacab Ba...</td>
      <td>health</td>
      <td>Polisi Militer Kodam Jaya mengungkap bahwa ter...</td>
      <td>Polisi Militer Kodam Jaya mengungkap bahwa ter...</td>
      <td>polisi militer kodam jaya mengungkap bahwa ter...</td>
      <td>polisi militer kodam jaya mengungkap prajurit ...</td>
      <td>polisi militer kodam jaksa mengungkap prajurit...</td>
      <td>polisi militer kodam jaksa ungkap prajurit tni...</td>
      <td>['polisi', 'militer', 'kodam', 'jaksa', 'ungka...</td>
    </tr>
    <tr>
      <th>131</th>
      <td>145</td>
      <td>Jaga Stabilitas Harga Pangan, Polres Rohul Sal...</td>
      <td>news</td>
      <td>Polres Rokan Hulu melalui Sat Intelkam menggel...</td>
      <td>Polres Rokan Hulu melalui Sat Intelkam menggel...</td>
      <td>polres rokan hulu melalui sat intelkam menggel...</td>
      <td>polres rokan hulu sat intelkam menggelar kegia...</td>
      <td>polres rokan hukum sakit intelkam menggelar ke...</td>
      <td>polres rok hukum sakit intelkam gelar giat ger...</td>
      <td>['polres', 'rok', 'hukum', 'sakit', 'intelkam'...</td>
    </tr>
    <tr>
      <th>132</th>
      <td>146</td>
      <td>Komeng Curhat ke Menhut soal Deforestasi di Ja...</td>
      <td>health</td>
      <td>Anggota Komite II DPD RI, Alfiansyah Komeng , ...</td>
      <td>Anggota Komite II DPD RI, Alfiansyah Komeng , ...</td>
      <td>anggota komite ii dpd ri alfiansyah komeng cur...</td>
      <td>anggota komite ii dpd ri alfiansyah komeng cur...</td>
      <td>anggota komite tni dpr tni alfiansyah komeng c...</td>
      <td>anggota komite tni dpr tni alfiansyah komeng c...</td>
      <td>['anggota', 'komite', 'tni', 'dpr', 'tni', 'al...</td>
    </tr>
    <tr>
      <th>133</th>
      <td>147</td>
      <td>Purbaya Bakal Ajak Prabowo 'Patroli' Cek Penye...</td>
      <td>politik</td>
      <td>Menteri Keuangan (Menkeu) Purbaya Yudhi Sadewa...</td>
      <td>Menteri Keuangan (Menkeu) Purbaya Yudhi Sadewa...</td>
      <td>menteri keuangan menkeu purbaya yudhi sadewa m...</td>
      <td>menteri keuangan menkeu purbaya yudhi sadewa m...</td>
      <td>menteri keuangan menkeu purbaya yudhi sadewa m...</td>
      <td>menteri uang menkeu purbaya yudhi sadewa aku t...</td>
      <td>['menteri', 'uang', 'menkeu', 'purbaya', 'yudh...</td>
    </tr>
    <tr>
      <th>134</th>
      <td>148</td>
      <td>Kader PPP Tolak Eksternal Duduki Jabatan Ketum...</td>
      <td>politik</td>
      <td>Menjelang Muktamar Partai Persatuan Pembanguna...</td>
      <td>Menjelang Muktamar Partai Persatuan Pembanguna...</td>
      <td>menjelang muktamar partai persatuan pembanguna...</td>
      <td>menjelang muktamar partai persatuan pembanguna...</td>
      <td>menjelang muktamar partai persatuan pembanguna...</td>
      <td>jelang muktamar partai satu bangun dpr tni pol...</td>
      <td>['jelang', 'muktamar', 'partai', 'satu', 'bang...</td>
    </tr>
    <tr>
      <th>135</th>
      <td>149</td>
      <td>Komisi V DPR-Kemendes Sepakat Bebaskan Desa-La...</td>
      <td>olahraga</td>
      <td>Komisi V DPR R I dan Kementerian Desa dan Pemb...</td>
      <td>Komisi V DPR R I dan Kementerian Desa dan Pemb...</td>
      <td>komisi v dpr r i dan kementerian desa dan pemb...</td>
      <td>komisi v dpr r i kementerian desa pembangunan ...</td>
      <td>polisi v dpr dpr tni kementerian desa pembangu...</td>
      <td>polisi v dpr dpr tni menteri desa bangun daera...</td>
      <td>['polisi', 'v', 'dpr', 'dpr', 'tni', 'menteri'...</td>
    </tr>
    <tr>
      <th>136</th>
      <td>150</td>
      <td>Polda NTT Kerahkan Tim SAR K9 Cari Korban Hila...</td>
      <td>news</td>
      <td>Kepolisian Daerah Nusa Tenggara Timur ( Polda ...</td>
      <td>Kepolisian Daerah Nusa Tenggara Timur ( Polda ...</td>
      <td>kepolisian daerah nusa tenggara timur polda nt...</td>
      <td>kepolisian daerah nusa tenggara timur polda nt...</td>
      <td>kepolisian daerah nusa tenggara timur polda tn...</td>
      <td>polisi daerah nusa tenggara timur polda tni te...</td>
      <td>['polisi', 'daerah', 'nusa', 'tenggara', 'timu...</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-568f4277-d9fe-4f1a-a835-96198e39bfd5')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-568f4277-d9fe-4f1a-a835-96198e39bfd5 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-568f4277-d9fe-4f1a-a835-96198e39bfd5');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-893fab87-5f6e-40f0-aaee-efab5f0c188a">
      <button class="colab-df-quickchart" onclick="quickchart('df-893fab87-5f6e-40f0-aaee-efab5f0c188a')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-893fab87-5f6e-40f0-aaee-efab5f0c188a button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

  <div id="id_a11cd5b5-3e0f-480b-961c-c7e9c1f9a42d">
    <style>
      .colab-df-generate {
        background-color: #E8F0FE;
        border: none;
        border-radius: 50%;
        cursor: pointer;
        display: none;
        fill: #1967D2;
        height: 32px;
        padding: 0 0 0 0;
        width: 32px;
      }

      .colab-df-generate:hover {
        background-color: #E2EBFA;
        box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
        fill: #174EA6;
      }

      [theme=dark] .colab-df-generate {
        background-color: #3B4455;
        fill: #D2E3FC;
      }

      [theme=dark] .colab-df-generate:hover {
        background-color: #434B5C;
        box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
        filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
        fill: #FFFFFF;
      }
    </style>
    <button class="colab-df-generate" onclick="generateWithVariable('df')"
            title="Generate code using this dataframe."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M7,19H8.4L18.45,9,17,7.55,7,17.6ZM5,21V16.75L18.45,3.32a2,2,0,0,1,2.83,0l1.4,1.43a1.91,1.91,0,0,1,.58,1.4,1.91,1.91,0,0,1-.58,1.4L9.25,21ZM18.45,9,17,7.55Zm-12,3A5.31,5.31,0,0,0,4.9,8.1,5.31,5.31,0,0,0,1,6.5,5.31,5.31,0,0,0,4.9,4.9,5.31,5.31,0,0,0,6.5,1,5.31,5.31,0,0,0,8.1,4.9,5.31,5.31,0,0,0,12,6.5,5.46,5.46,0,0,0,6.5,12Z"/>
  </svg>
    </button>
    <script>
      (() => {
      const buttonEl =
        document.querySelector('#id_a11cd5b5-3e0f-480b-961c-c7e9c1f9a42d button.colab-df-generate');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      buttonEl.onclick = () => {
        google.colab.notebook.generateWithVariable('df');
      }
      })();
    </script>
  </div>

    </div>
  </div>





```python
data_text = df[:300000][['tokens_final']]

data_text.head()
```





  <div id="df-ce1d43b1-49b7-4c78-9729-c555bfbb1298" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tokens_final</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>['maroko', 'pascagempa', 'ribu', 'harga', 'mar...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>['rekonstruksi', 'alvi', 'maulana', 'mutilasi'...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>['celaka', 'lintas', 'libat', 'truk', 'tni', '...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>['mahkamah', 'konstitusi', 'kpk', 'gelar', 'si...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>['perintah', 'provinsi', 'banten', 'gelar', 'r...</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-ce1d43b1-49b7-4c78-9729-c555bfbb1298')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-ce1d43b1-49b7-4c78-9729-c555bfbb1298 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-ce1d43b1-49b7-4c78-9729-c555bfbb1298');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-92086bd8-3669-438c-ba33-2321e3b62202">
      <button class="colab-df-quickchart" onclick="quickchart('df-92086bd8-3669-438c-ba33-2321e3b62202')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-92086bd8-3669-438c-ba33-2321e3b62202 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
data_text['index'] = data_text.index

documents = data_text
documents.head()
```





  <div id="df-02de13aa-f36a-405b-b000-63391906d5f5" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tokens_final</th>
      <th>index</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>['maroko', 'pascagempa', 'ribu', 'harga', 'mar...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>['rekonstruksi', 'alvi', 'maulana', 'mutilasi'...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>['celaka', 'lintas', 'libat', 'truk', 'tni', '...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>['mahkamah', 'konstitusi', 'kpk', 'gelar', 'si...</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>['perintah', 'provinsi', 'banten', 'gelar', 'r...</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-02de13aa-f36a-405b-b000-63391906d5f5')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-02de13aa-f36a-405b-b000-63391906d5f5 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-02de13aa-f36a-405b-b000-63391906d5f5');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-b376d5d6-aede-4e7a-b4dc-ce1c8d10f54b">
      <button class="colab-df-quickchart" onclick="quickchart('df-b376d5d6-aede-4e7a-b4dc-ce1c8d10f54b')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-b376d5d6-aede-4e7a-b4dc-ce1c8d10f54b button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
len(documents)
```




    137




```python
!pip install gensim==4.3.3 --upgrade

```

    Collecting gensim==4.3.3
      Using cached gensim-4.3.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (8.1 kB)
    Collecting numpy<2.0,>=1.18.5 (from gensim==4.3.3)
      Using cached numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
    Collecting scipy<1.14.0,>=1.7.0 (from gensim==4.3.3)
      Using cached scipy-1.13.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
    Requirement already satisfied: smart-open>=1.8.1 in /usr/local/lib/python3.12/dist-packages (from gensim==4.3.3) (7.3.1)
    Requirement already satisfied: wrapt in /usr/local/lib/python3.12/dist-packages (from smart-open>=1.8.1->gensim==4.3.3) (1.17.3)
    Downloading gensim-4.3.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (26.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m26.6/26.6 MB[0m [31m99.0 MB/s[0m eta [36m0:00:00[0m
    [?25hUsing cached numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.0 MB)
    Using cached scipy-1.13.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (38.2 MB)
    Installing collected packages: numpy, scipy, gensim
      Attempting uninstall: numpy
        Found existing installation: numpy 2.0.2
        Uninstalling numpy-2.0.2:
          Successfully uninstalled numpy-2.0.2
      Attempting uninstall: scipy
        Found existing installation: scipy 1.16.2
        Uninstalling scipy-1.16.2:
          Successfully uninstalled scipy-1.16.2
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    opencv-python 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.4 which is incompatible.
    thinc 8.3.6 requires numpy<3.0.0,>=2.0.0, but you have numpy 1.26.4 which is incompatible.
    tsfresh 0.21.1 requires scipy>=1.14.0; python_version >= "3.10", but you have scipy 1.13.1 which is incompatible.
    opencv-contrib-python 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.4 which is incompatible.
    opencv-python-headless 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.4 which is incompatible.[0m[31m
    [0mSuccessfully installed gensim-4.3.3 numpy-1.26.4 scipy-1.13.1
    




```python
!pip install Sastrawi

```

    Collecting Sastrawi
      Downloading Sastrawi-1.0.1-py2.py3-none-any.whl.metadata (909 bytes)
    Downloading Sastrawi-1.0.1-py2.py3-none-any.whl (209 kB)
    [?25l   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m0.0/209.7 kB[0m [31m?[0m eta [36m-:--:--[0m
[2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m209.7/209.7 kB[0m [31m7.1 MB/s[0m eta [36m0:00:00[0m
    [?25hInstalling collected packages: Sastrawi
    Successfully installed Sastrawi-1.0.1
    


```python
import gensim
from gensim.utils import simple_preprocess
from nltk.corpus import stopwords
from nltk.stem.porter import *
import numpy as np
```


```python
import nltk
nltk.download('wordnet')
```

    [nltk_data] Downloading package wordnet to /root/nltk_data...
    [nltk_data]   Package wordnet is already up-to-date!
    




    True




```python
import nltk
nltk.download('stopwords')

# 🔹 Tambahkan ini
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory

# 🔹 Baru bikin stemmer-nya
factory = StemmerFactory()
stemmer = factory.create_stemmer()

```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    


```python
nltk.download('stopwords')

factory = StemmerFactory()
stemmer = factory.create_stemmer()

stop_words = stopwords.words('indonesian')

def stemming_indonesia(text):
    return stemmer.stem(text)

def preprocess(text):
    result = []
    for token in gensim.utils.simple_preprocess(str(text)):  # tokenisasi
        if token not in stop_words and len(token) > 3:
            result.append(stemming_indonesia(token))
    return result
```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    


```python
nltk.download('stopwords')

factory = StemmerFactory()
stemmer = factory.create_stemmer()

stop_words = stopwords.words('indonesian')

def stemming_indonesia(text):
    return stemmer.stem(text)

def preprocess(text):
    result = []
    for token in gensim.utils.simple_preprocess(str(text)):  # tokenisasi
        if token not in stop_words and len(token) > 3:
            result.append(stemming_indonesia(token))
    return result
```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    


```python
import pandas as pd

df = pd.read_csv('/content/drive/MyDrive/Semester 7/berita_fiks.csv')

print("\nTampilan kolom 'tokens_final' dan 'after_stemmed':")
print(df[['tokens_final', 'after_stemmed']].head())

print("\nJumlah data:", df.shape)

```

    
    Tampilan kolom 'tokens_final' dan 'after_stemmed':
                                            tokens_final  \
    0  ['maroko', 'pascagempa', 'ribu', 'harga', 'mar...   
    1  ['rekonstruksi', 'alvi', 'maulana', 'mutilasi'...   
    2  ['celaka', 'lintas', 'libat', 'truk', 'tni', '...   
    3  ['mahkamah', 'konstitusi', 'kpk', 'gelar', 'si...   
    4  ['perintah', 'provinsi', 'banten', 'gelar', 'r...   
    
                                           after_stemmed  
    0  maroko pascagempa ribu harga maroko tinggal te...  
    1  rekonstruksi alvi maulana mutilasi tiara angel...  
    2  celaka lintas libat truk tni angkot angkut kot...  
    3  mahkamah konstitusi kpk gelar sidang gugat uu ...  
    4  perintah provinsi banten gelar razia kendara b...  
    
    Jumlah data: (137, 10)
    


```python
document_num = 136

# Pastikan indexnya tidak lebih besar dari jumlah baris
if document_num < len(df):
    doc_sample = df.iloc[document_num]['tokens_final']

    print("\nOriginal document (tokens final):")
    print(doc_sample)

    # Split kata berdasarkan spasi
    words = doc_sample.split()
    print("\nTokenized words:")
    print(words)

    # Karena sudah di-stemming, kita hanya tampilkan kolom hasil stemming
    print("\nHasil stemming (after_stemmed):")
    print(df.iloc[document_num]['after_stemmed'])
else:
    print(f"\n⚠️ Nomor {document_num} melebihi jumlah baris ({len(df)})")
```

    
    Original document (tokens final):
    ['polisi', 'daerah', 'nusa', 'tenggara', 'timur', 'polda', 'tni', 'terjun', 'personel', 'tambah', 'daerah', 'camat', 'mauponggo', 'kabupaten', 'nagekeo', 'kena', 'dampak', 'banjir', 'bandang', 'tugas', 'berangkat', 'pagi', 'terjun', 'anggota', 'lapang', 'kirim', 'bantu', 'personel', 'tambah', 'skor', 'polda', 'kuat', 'kerah', 'polres', 'nagekeo', 'polres', 'ende', 'polrespolres', 'kapolda', 'tni', 'irjen', 'rudi', 'darmoko', 'wartawan', 'selasa', 'scroll', 'gol', 'continue', 'with', 'content', 'kuat', 'personel', 'polda', 'tni', 'turun', 'tni', 'anjing', 'lacak', 'kpk', 'cepat', 'proses', 'cari', 'korban', 'korban', 'temu']
    
    Tokenized words:
    ["['polisi',", "'daerah',", "'nusa',", "'tenggara',", "'timur',", "'polda',", "'tni',", "'terjun',", "'personel',", "'tambah',", "'daerah',", "'camat',", "'mauponggo',", "'kabupaten',", "'nagekeo',", "'kena',", "'dampak',", "'banjir',", "'bandang',", "'tugas',", "'berangkat',", "'pagi',", "'terjun',", "'anggota',", "'lapang',", "'kirim',", "'bantu',", "'personel',", "'tambah',", "'skor',", "'polda',", "'kuat',", "'kerah',", "'polres',", "'nagekeo',", "'polres',", "'ende',", "'polrespolres',", "'kapolda',", "'tni',", "'irjen',", "'rudi',", "'darmoko',", "'wartawan',", "'selasa',", "'scroll',", "'gol',", "'continue',", "'with',", "'content',", "'kuat',", "'personel',", "'polda',", "'tni',", "'turun',", "'tni',", "'anjing',", "'lacak',", "'kpk',", "'cepat',", "'proses',", "'cari',", "'korban',", "'korban',", "'temu']"]
    
    Hasil stemming (after_stemmed):
    polisi daerah nusa tenggara timur polda tni terjun personel tambah daerah camat mauponggo kabupaten nagekeo kena dampak banjir bandang tugas berangkat pagi terjun anggota lapang kirim bantu personel tambah skor polda kuat kerah polres nagekeo polres ende polrespolres kapolda tni irjen rudi darmoko wartawan selasa scroll gol continue with content kuat personel polda tni turun tni anjing lacak kpk cepat proses cari korban korban temu
    


```python
print(df.columns.tolist())
```

    ['id', 'judul', 'kategori_baru', 'isi', 'before', 'after_lower_no_symbol', 'after_stopword', 'after_corrected', 'after_stemmed', 'tokens_final']
    


```python
process_docs = df['after_stemmed'].map(preprocess)
```


```python
process_docs[:10]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>after_stemmed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[maroko, pascagempa, ribu, harga, maroko, ting...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[rekonstruksi, alvi, maulana, mutilasi, tiara,...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[celaka, lintas, libat, truk, angkot, angkut, ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[mahkamah, konstitusi, gelar, sidang, gugat, g...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[perintah, provinsi, banten, gelar, razia, ken...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>[bareskrim, polisi, usul, kait, bareskrim, usu...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>[kendara, mobil, pekanbaru, riau, olvi, praiya...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>[presiden, prabowo, subianto, lantik, menteri,...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>[forum, xiangshan, temu, aman, internasional, ...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>[wakil, menteri, transmigrasi, liga, liga, mau...</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> object</label>



**Get a BOW Dict from data**


```python
dic = gensim.corpora.Dictionary(process_docs)
count = 0
for k, v in dic.items():
    print(k, v)
    count += 1
    if count > 10:
        break
```

    0 bantu
    1 darurat
    2 gencar
    3 harga
    4 maroko
    5 pascagempa
    6 proyek
    7 ribu
    8 stadion
    9 tenda
    10 tinggal
    

**filter the dict**


```python
dic.filter_extremes(no_below=15, no_above=0.1, keep_n=100000)
```

**Convert document into BOW format by doc2bow**


```python
bow_corpus = [dic.doc2bow(doc) for doc in process_docs]
```


```python
import ast
df['tokens_final'] = df['tokens_final'].apply(lambda x: ast.literal_eval(x) if isinstance(x, str) else x)

```


```python
from gensim.corpora import Dictionary

dictionary = Dictionary(df['tokens_final'])
bow_corpus = [dictionary.doc2bow(text) for text in df['tokens_final']]

```


```python
document_num = 112

bow_doc = bow_corpus[document_num]

if len(bow_doc) == 0:
    print(f"Dokumen ke-{document_num} kosong.")
else:
    print(f"=== Isi dokumen ke-{document_num} ===\n")
    for word_id, freq in bow_doc:
        print(f"Word ID {word_id} (\"{dictionary[word_id]}\") appears {freq} time(s).")
```

    === Isi dokumen ke-112 ===
    
    Word ID 19 ("content") appears 1 time(s).
    Word ID 20 ("continue") appears 1 time(s).
    Word ID 25 ("gol") appears 1 time(s).
    Word ID 27 ("jadi") appears 1 time(s).
    Word ID 34 ("kpk") appears 1 time(s).
    Word ID 37 ("liga") appears 1 time(s).
    Word ID 53 ("scroll") appears 1 time(s).
    Word ID 62 ("wib") appears 1 time(s).
    Word ID 63 ("with") appears 1 time(s).
    Word ID 68 ("barat") appears 1 time(s).
    Word ID 70 ("bogor") appears 1 time(s).
    Word ID 75 ("f") appears 1 time(s).
    Word ID 79 ("jaksa") appears 3 time(s).
    Word ID 93 ("peristiwa") appears 1 time(s).
    Word ID 137 ("kaca") appears 1 time(s).
    Word ID 150 ("pasar") appears 1 time(s).
    Word ID 211 ("bodi") appears 1 time(s).
    Word ID 222 ("medali") appears 1 time(s).
    Word ID 223 ("mobil") appears 5 time(s).
    Word ID 238 ("sosial") appears 1 time(s).
    Word ID 241 ("video") appears 2 time(s).
    Word ID 242 ("viral") appears 2 time(s).
    Word ID 309 ("september") appears 1 time(s).
    Word ID 325 ("batu") appears 1 time(s).
    Word ID 392 ("detik") appears 1 time(s).
    Word ID 428 ("insiden") appears 1 time(s).
    Word ID 519 ("rusak") appears 1 time(s).
    Word ID 520 ("senin") appears 1 time(s).
    Word ID 888 ("motor") appears 1 time(s).
    Word ID 1339 ("jenis") appears 1 time(s).
    Word ID 1907 ("kayu") appears 1 time(s).
    Word ID 2046 ("amuk") appears 1 time(s).
    Word ID 2047 ("ayla") appears 1 time(s).
    Word ID 2048 ("kabur") appears 1 time(s).
    Word ID 2049 ("penyok") appears 1 time(s).
    Word ID 2050 ("seruduk") appears 1 time(s).
    Word ID 2051 ("sukaraja") appears 1 time(s).
    

**TF-IDF on our document set**


```python
tfidf = gensim.models.TfidfModel(bow_corpus)
```


```python
corpus_tfidf = tfidf[bow_corpus]
```


```python
for docu in corpus_tfidf:
    print(docu)
    break
```

    [(0, 0.173600748952043), (1, 0.22530500554517213), (2, 0.24364738247705112), (3, 0.07849403698715895), (4, 0.6273880320041186), (5, 0.3136940160020593), (6, 0.24364738247705112), (7, 0.19945287724860752), (8, 0.3136940160020593), (9, 0.3136940160020593), (10, 0.1811105003167285), (11, 0.1811105003167285)]
    

**Running LDA using Bag of Words data**


```python
import ast
df['tokens_final'] = df['tokens_final'].apply(lambda x: ast.literal_eval(x) if isinstance(x, str) else x)

```


```python
from gensim.corpora import Dictionary

dic = Dictionary(df['tokens_final'])
bow_corpus = [dic.doc2bow(text) for text in df['tokens_final']]

```


```python
print(len(dic))  # jumlah kata unik
print(len(bow_corpus))  # jumlah dokumen
print(bow_corpus[0][:10])  # contoh isi 10 kata pertama di dokumen 0

```

    2303
    137
    [(0, 1), (1, 1), (2, 1), (3, 1), (4, 2), (5, 1), (6, 1), (7, 1), (8, 1), (9, 1)]
    


```python
lda_model = gensim.models.LdaMulticore(bow_corpus, num_topics=10, id2word = dic, passes = 2, workers=2)
```

    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    


```python
for idx, topic in lda_model.print_topics():
    print("Topic: {} \nWords: {}".format(idx, topic))
    print("\n")
```

    Topic: 0 
    Words: 0.026*"dpr" + 0.021*"gol" + 0.020*"tni" + 0.013*"polisi" + 0.012*"continue" + 0.012*"scroll" + 0.011*"content" + 0.011*"rumah" + 0.011*"with" + 0.011*"harga"
    
    
    Topic: 1 
    Words: 0.016*"polisi" + 0.014*"jakarta" + 0.014*"gol" + 0.012*"kpk" + 0.012*"dpr" + 0.012*"menteri" + 0.010*"harga" + 0.009*"content" + 0.009*"with" + 0.009*"tni"
    
    
    Topic: 2 
    Words: 0.015*"harga" + 0.012*"gol" + 0.011*"sehat" + 0.011*"liga" + 0.010*"jalan" + 0.010*"continue" + 0.010*"content" + 0.010*"with" + 0.009*"scroll" + 0.009*"hakim"
    
    
    Topic: 3 
    Words: 0.018*"gol" + 0.016*"kpk" + 0.012*"continue" + 0.012*"content" + 0.012*"with" + 0.012*"scroll" + 0.010*"laku" + 0.010*"mobil" + 0.009*"israel" + 0.009*"korban"
    
    
    Topic: 4 
    Words: 0.024*"gol" + 0.021*"tni" + 0.011*"scroll" + 0.011*"with" + 0.011*"continue" + 0.011*"content" + 0.009*"presiden" + 0.008*"data" + 0.008*"dpr" + 0.007*"pribadi"
    
    
    Topic: 5 
    Words: 0.018*"pasar" + 0.017*"gol" + 0.013*"jaksa" + 0.012*"continue" + 0.012*"scroll" + 0.012*"with" + 0.012*"polisi" + 0.012*"tni" + 0.012*"content" + 0.011*"kpk"
    
    
    Topic: 6 
    Words: 0.014*"dpr" + 0.013*"serang" + 0.012*"gol" + 0.012*"kpk" + 0.012*"scroll" + 0.012*"content" + 0.012*"with" + 0.011*"continue" + 0.010*"banten" + 0.010*"israel"
    
    
    Topic: 7 
    Words: 0.022*"tni" + 0.015*"budaya" + 0.010*"masyarakat" + 0.009*"gol" + 0.009*"menteri" + 0.008*"tanah" + 0.008*"with" + 0.008*"content" + 0.008*"prabumulih" + 0.008*"scroll"
    
    
    Topic: 8 
    Words: 0.016*"gol" + 0.014*"jaksa" + 0.012*"tni" + 0.011*"content" + 0.011*"scroll" + 0.010*"with" + 0.010*"continue" + 0.009*"pasar" + 0.008*"hubung" + 0.008*"masyarakat"
    
    
    Topic: 9 
    Words: 0.019*"dpr" + 0.015*"kpk" + 0.010*"gol" + 0.010*"tni" + 0.010*"scroll" + 0.010*"continue" + 0.010*"with" + 0.010*"content" + 0.009*"publik" + 0.009*"gerhana"
    
    
    

**Topic coherence**


```python
from gensim.models import CoherenceModel

coherence_model_lda = CoherenceModel(model=lda_model, texts=process_docs, dictionary=dic, coherence='c_v')
coherence_lda = coherence_model_lda.get_coherence()
print('\nCoherence Score: ', coherence_lda)
```

    
    Coherence Score:  nan
    

    /usr/local/lib/python3.12/dist-packages/gensim/topic_coherence/direct_confirmation_measure.py:204: RuntimeWarning: divide by zero encountered in scalar divide
      m_lr_i = np.log(numerator / denominator)
    /usr/local/lib/python3.12/dist-packages/gensim/topic_coherence/indirect_confirmation_measure.py:323: RuntimeWarning: invalid value encountered in scalar divide
      return cv1.T.dot(cv2)[0, 0] / (_magnitude(cv1) * _magnitude(cv2))
    


```python
from gensim.models import CoherenceModel

# Gunakan kolom tokens_final sebagai dokumen terproses
processed_docs = df['tokens_final']

coherence_model_lda = CoherenceModel(model=lda_model,
                                     texts=processed_docs,
                                     dictionary=dic,
                                     coherence='u_mass')

coherence_lda = coherence_model_lda.get_coherence()
print('\nCoherence Score:', coherence_lda)

```

    
    Coherence Score: -3.920143727976317
    

**find the optimal number of topics**


```python
def compute_coherence_values(dictionary, corpus, texts, limit, start=2, step=3):

    coherence_values = []
    model_list = []
    for num_topics in range(start, limit, step):
        model=gensim.models.LdaMulticore(corpus=corpus, id2word=dic, num_topics=num_topics)
        model_list.append(model)
        coherencemodel = CoherenceModel(model=model, texts=texts, dictionary=dic, coherence='c_v')
        coherence_values.append(coherencemodel.get_coherence())

    return model_list, coherence_values
```


```python
from gensim.models import CoherenceModel
import matplotlib.pyplot as plt

coherence_values = []
limit = 40
start = 2
step = 6
x = range(start, limit, step)

for num_topics in x:
    lda_model = gensim.models.LdaMulticore(bow_corpus,
                                           num_topics=num_topics,
                                           id2word=dic,
                                           passes=2,
                                           workers=2)

    coherence_model = CoherenceModel(model=lda_model,
                                     texts=df['tokens_final'],
                                     dictionary=dic,
                                     coherence='c_v')
    coherence_values.append(coherence_model.get_coherence())

# Setelah itu baru diplot:
plt.plot(x, coherence_values)
plt.xlabel("Num Topics")
plt.ylabel("Coherence score")
plt.title("Grafik Coherence vs Jumlah Topik")
plt.legend(["Coherence Score"], loc='best')
plt.show()

```

    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    


    
![png](Klasifikasi_Berita_files/Klasifikasi_Berita_42_1.png)
    


**Running LDA using TF-IDF**


```python
lda_model_tfidf = gensim.models.LdaMulticore(corpus_tfidf, num_topics=10, id2word = dic, passes = 2, workers=4)
```

    WARNING:gensim.models.ldamulticore:too few updates, training might not converge; consider increasing the number of passes or iterations to improve accuracy
    


```python
for idx, topic in lda_model_tfidf.print_topics(-1):
    print("Topic: {} Word: {}".format(idx, topic))
    print("\n")
```

    Topic: 0 Word: 0.004*"israel" + 0.003*"kuliah" + 0.002*"pbb" + 0.002*"daftar" + 0.002*"gaza" + 0.002*"khozin" + 0.002*"aniaya" + 0.002*"genosida" + 0.002*"sbm" + 0.002*"sikap"
    
    
    Topic: 1 Word: 0.003*"poskamling" + 0.002*"filipina" + 0.002*"investasi" + 0.002*"kendara" + 0.002*"tambang" + 0.002*"china" + 0.002*"tugas" + 0.002*"kapal" + 0.002*"akhlak" + 0.002*"ibas"
    
    
    Topic: 2 Word: 0.003*"lelang" + 0.003*"didik" + 0.003*"hamas" + 0.003*"meranti" + 0.002*"bumn" + 0.002*"gaza" + 0.002*"mufti" + 0.002*"israel" + 0.002*"simeulue" + 0.002*"mohon"
    
    
    Topic: 3 Word: 0.003*"korban" + 0.003*"laku" + 0.003*"gerhana" + 0.003*"tangkap" + 0.002*"as" + 0.002*"rumah" + 0.002*"serikat" + 0.002*"hukum" + 0.002*"inisial" + 0.002*"data"
    
    
    Topic: 4 Word: 0.002*"mohon" + 0.002*"maroko" + 0.002*"kpk" + 0.002*"gugat" + 0.002*"transmigrasi" + 0.002*"jakarta" + 0.002*"harga" + 0.002*"menara" + 0.002*"jaksa" + 0.002*"putus"
    
    
    Topic: 5 Word: 0.003*"rekening" + 0.003*"dormant" + 0.003*"narkoba" + 0.003*"sangka" + 0.002*"ilham" + 0.002*"bakar" + 0.002*"pomdam" + 0.002*"senen" + 0.002*"bank" + 0.002*"culi"
    
    
    Topic: 6 Word: 0.003*"harga" + 0.002*"sumerta" + 0.002*"tugas" + 0.002*"percaya" + 0.002*"djamari" + 0.002*"senjata" + 0.002*"masjid" + 0.002*"pramono" + 0.002*"polisi" + 0.002*"labuh"
    
    
    Topic: 7 Word: 0.003*"polisi" + 0.002*"tewas" + 0.002*"hubung" + 0.002*"demonstrasi" + 0.002*"ketenagakerjaan" + 0.002*"jembatan" + 0.002*"situasional" + 0.002*"kemudi" + 0.002*"gaza" + 0.002*"dpr"
    
    
    Topic: 8 Word: 0.003*"sipil" + 0.003*"tni" + 0.002*"menteri" + 0.002*"budaya" + 0.002*"ad" + 0.002*"sjafrie" + 0.002*"mesir" + 0.002*"polisi" + 0.002*"dasco" + 0.002*"libat"
    
    
    Topic: 9 Word: 0.003*"dokumen" + 0.003*"korban" + 0.003*"sehat" + 0.003*"longsor" + 0.003*"publik" + 0.002*"tanah" + 0.002*"tinggal" + 0.002*"kecuali" + 0.002*"pipa" + 0.002*"asep"
    
    
    


```python
coherence_model_lda_idf = CoherenceModel(model=lda_model_tfidf, texts=processed_docs, dictionary=dic, coherence='c_v')

```


```python
from gensim.models import CoherenceModel

# pastikan variabel sesuai
texts = df['tokens_final'].apply(lambda x: eval(x) if isinstance(x, str) else x).tolist()

# Hitung coherence score
coherence_model_lda_idf = CoherenceModel(
    model=lda_model_tfidf,
    texts=texts,
    dictionary=dic,
    coherence='c_v'  # bisa diganti 'u_mass' kalau masih NaN
)

coherence_score = coherence_model_lda_idf.get_coherence()
print('Coherence Score (TF-IDF LDA):', coherence_score)

```

    Coherence Score (TF-IDF LDA): 0.5192768359015442
    

**classifying sample document using LDA Bag of Words model**


```python
process_docs[document_num]
```




    ['mobil',
     'jenis',
     'liga',
     'ayla',
     'seruduk',
     'motor',
     'sukaraja',
     'bogor',
     'jaksa',
     'barat',
     'pasar',
     'mobil',
     'amuk',
     'jaksa',
     'kabur',
     'peristiwa',
     'video',
     'viral',
     'medali',
     'sosial',
     'insiden',
     'senin',
     'september',
     'video',
     'viral',
     'kaca',
     'mobil',
     'detik',
     'bodi',
     'mobil',
     'penyok',
     'jaksa',
     'rusak',
     'mobil',
     'kayu',
     'batu',
     'scroll',
     'continue',
     'with',
     'content']




```python
for index, score in sorted(lda_model[bow_corpus[document_num]], key=lambda tup: tup[1], reverse=True):
    print("\nScore: {}\t Topic: {}".format(score, lda_model.print_topic(index, 5)))
```

    
    Score: 0.9788310527801514	 Topic: 0.028*"gol" + 0.020*"meranti" + 0.018*"mobil" + 0.018*"tni" + 0.015*"pulau"
    


```python
lda_model[bow_corpus[document_num]]
```




    [(30, 0.97883105)]




```python
sorted(lda_model[bow_corpus[document_num]], key=lambda tup: tup[1], reverse=True)
```




    [(30, 0.97883105)]




```python
lda_model.print_topic(index, 10)
```




    '0.028*"gol" + 0.020*"meranti" + 0.018*"mobil" + 0.018*"tni" + 0.015*"pulau" + 0.014*"korban" + 0.012*"sphp" + 0.012*"beras" + 0.012*"polres" + 0.011*"continue"'



**classifying sample document using LDA TF-IDF model**


```python
for index, score in sorted(lda_model_tfidf[bow_corpus[document_num]], key=lambda tup: tup[1], reverse=True):
    print("\nScore: {}\t Topic: {}".format(score, lda_model_tfidf.print_topic(index, 5)))
```

    
    Score: 0.9802947044372559	 Topic: 0.003*"korban" + 0.003*"laku" + 0.003*"gerhana" + 0.003*"tangkap" + 0.002*"as"
    

**Testing model on unseen document**


```python
unseen_document = "Perkenalkan saya Lisda"

bow_vector = dic.doc2bow(preprocess(unseen_document))
for index, score in sorted(lda_model[bow_vector], key = lambda tup : tup[1], reverse=True):
  print('Score: {}\t Topik {}'.format(score, lda_model.print_topic(index, 5)))
```

    Score: 0.5131370425224304	 Topik 0.018*"gol" + 0.013*"menteri" + 0.013*"bangun" + 0.012*"budaya" + 0.012*"pasar"
    Score: 0.013158458285033703	 Topik 0.043*"jaksa" + 0.023*"gol" + 0.017*"kpk" + 0.016*"metro" + 0.015*"jakarta"
    Score: 0.013158458285033703	 Topik 0.034*"jakarta" + 0.023*"gol" + 0.023*"dpr" + 0.019*"tni" + 0.014*"polisi"
    Score: 0.013158458285033703	 Topik 0.032*"prancis" + 0.019*"protes" + 0.015*"layan" + 0.014*"nasional" + 0.013*"jalan"
    Score: 0.013158458285033703	 Topik 0.029*"gol" + 0.020*"israel" + 0.019*"harga" + 0.019*"serang" + 0.018*"gaza"
    Score: 0.013158458285033703	 Topik 0.023*"percaya" + 0.020*"publik" + 0.019*"senjata" + 0.011*"gol" + 0.011*"riau"
    Score: 0.013158458285033703	 Topik 0.038*"kpk" + 0.022*"gerhana" + 0.022*"mohon" + 0.019*"berita" + 0.017*"dpr"
    Score: 0.013158458285033703	 Topik 0.019*"israel" + 0.014*"gol" + 0.014*"tni" + 0.012*"polisi" + 0.012*"hubung"
    Score: 0.013158458285033703	 Topik 0.027*"tni" + 0.020*"bangsa" + 0.014*"dpr" + 0.013*"jaksa" + 0.013*"korban"
    Score: 0.013158458285033703	 Topik 0.031*"budaya" + 0.020*"dpr" + 0.020*"menteri" + 0.013*"gol" + 0.013*"tni"
    Score: 0.013158458285033703	 Topik 0.017*"sumerta" + 0.015*"as" + 0.015*"trump" + 0.014*"harga" + 0.014*"menteri"
    Score: 0.013158458285033703	 Topik 0.009*"korban" + 0.005*"gol" + 0.004*"polisi" + 0.003*"laku" + 0.003*"israel"
    Score: 0.013158458285033703	 Topik 0.022*"satkamling" + 0.013*"kapur" + 0.012*"pohon" + 0.011*"aman" + 0.009*"lestari"
    Score: 0.013158458285033703	 Topik 0.019*"gol" + 0.018*"kpk" + 0.016*"kerja" + 0.013*"with" + 0.013*"continue"
    Score: 0.013158458285033703	 Topik 0.023*"publik" + 0.019*"dokumen" + 0.017*"presiden" + 0.016*"putus" + 0.016*"kpk"
    Score: 0.013158458285033703	 Topik 0.031*"tni" + 0.031*"liga" + 0.026*"transmigrasi" + 0.026*"gol" + 0.016*"harga"
    Score: 0.013158458285033703	 Topik 0.018*"data" + 0.017*"gol" + 0.016*"pribadi" + 0.013*"continue" + 0.013*"content"
    Score: 0.013158458285033703	 Topik 0.033*"israel" + 0.022*"gaza" + 0.020*"serang" + 0.017*"gol" + 0.015*"tipu"
    Score: 0.013158458285033703	 Topik 0.026*"gol" + 0.025*"polisi" + 0.019*"rumah" + 0.015*"dpr" + 0.014*"korban"
    Score: 0.013158458285033703	 Topik 0.049*"korban" + 0.045*"laku" + 0.030*"foto" + 0.020*"kpk" + 0.019*"medali"
    Score: 0.013158458285033703	 Topik 0.020*"pasar" + 0.016*"jalan" + 0.016*"longsor" + 0.013*"scroll" + 0.012*"rumah"
    Score: 0.013158458285033703	 Topik 0.023*"bogor" + 0.019*"celaka" + 0.012*"akibat" + 0.012*"angkot" + 0.011*"peristiwa"
    Score: 0.013158458285033703	 Topik 0.033*"uu" + 0.033*"gugat" + 0.029*"kpk" + 0.025*"tni" + 0.025*"nomor"
    Score: 0.013158458285033703	 Topik 0.045*"dpr" + 0.028*"kawasan" + 0.020*"tni" + 0.019*"polisi" + 0.019*"desa"
    Score: 0.013158458285033703	 Topik 0.029*"harga" + 0.028*"tugas" + 0.016*"uang" + 0.016*"jakarta" + 0.015*"ganda"
    Score: 0.013158458285033703	 Topik 0.027*"sangka" + 0.026*"jaksa" + 0.018*"bunuh" + 0.018*"pasar" + 0.017*"laku"
    Score: 0.013158458285033703	 Topik 0.033*"sehat" + 0.024*"tni" + 0.023*"dpr" + 0.017*"liga" + 0.016*"prabumulih"
    Score: 0.013158458285033703	 Topik 0.045*"narkoba" + 0.039*"banten" + 0.016*"hendra" + 0.015*"lintas" + 0.015*"wilayah"
    Score: 0.013158458285033703	 Topik 0.035*"anggar" + 0.023*"purbaya" + 0.021*"mobil" + 0.019*"boks" + 0.019*"gol"
    Score: 0.013158458285033703	 Topik 0.032*"tni" + 0.021*"gol" + 0.016*"sangka" + 0.011*"usaha" + 0.010*"scroll"
    Score: 0.013158458285033703	 Topik 0.028*"gol" + 0.020*"meranti" + 0.018*"mobil" + 0.018*"tni" + 0.015*"pulau"
    Score: 0.013158458285033703	 Topik 0.038*"tni" + 0.031*"kpk" + 0.022*"polisi" + 0.022*"dpr" + 0.016*"gol"
    Score: 0.013158458285033703	 Topik 0.024*"banten" + 0.023*"tni" + 0.018*"pasar" + 0.014*"kendara" + 0.013*"masyarakat"
    Score: 0.013158458285033703	 Topik 0.019*"gol" + 0.014*"china" + 0.014*"israel" + 0.014*"serang" + 0.013*"hamas"
    Score: 0.013158458285033703	 Topik 0.036*"jaksa" + 0.017*"polisi" + 0.016*"asep" + 0.016*"korban" + 0.016*"bupati"
    Score: 0.013158458285033703	 Topik 0.058*"dpr" + 0.024*"tni" + 0.021*"harga" + 0.019*"lelang" + 0.017*"pipa"
    Score: 0.013158458285033703	 Topik 0.032*"harga" + 0.023*"gol" + 0.023*"kpk" + 0.018*"boks" + 0.014*"libat"
    Score: 0.013158458285033703	 Topik 0.019*"tanah" + 0.018*"menteri" + 0.014*"forum" + 0.014*"tni" + 0.011*"sertifikat"
    
