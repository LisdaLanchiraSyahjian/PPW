# TF-IDF dan Word Embeding Data Berita

```python
from google.colab import drive
drive.mount('/content/drive')

```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
    


```python
import pandas as pd

# Baca dataset hasil preprocessing
df = pd.read_csv('/content/drive/MyDrive/Semester 7/berita_detik_preprocessed.csv')

# Fungsi labeling berdasarkan isi berita
def label_kategori_isi(teks):
    teks = str(teks).lower()

    # Sport / Olahraga
    if any(k in teks for k in [
        "bola", "sepak", "liga", "pemain", "timnas", "olahraga", "pertandingan", "skor", "gol",
        "badminton", "basket", "voli", "tenis", "turnamen", "kompetisi", "olimpiade", "pelatih"
    ]):
        return "olahraga"

    # Travel / Wisata
    elif any(k in teks for k in [
        "wisata", "jalan-jalan", "pantai", "gunung", "destinasi", "liburan", "tiket", "hotel",
        "resor", "candi", "monumen", "pariwisata", "objek wisata", "travel", "perjalanan", "pesawat"
    ]):
        return "travel"

    # Health / Kesehatan
    elif any(k in teks for k in [
        "vaksin", "dokter", "rumah sakit", "penyakit", "kesehatan", "diet", "covid", "obat",
        "virus", "flu", "demam", "operasi", "gejala", "terapi", "gizi", "imunisasi", "paru"
    ]):
        return "health"

    # Politik
    elif any(k in teks for k in [
        "politik", "pilpres", "pemilu", "presiden", "menteri", "dpr", "parlemen", "partai", "capres",
        "cawapres", "kampanye", "kabinet", "politisi", "caleg", "koalisi", "oposisi", "pemerintah",
        "wakil rakyat", "pemilihan", "undang-undang"
    ]):
        return "politik"

    # Default
    else:
        return "news"

# Tambahkan kolom kategori_baru (pakai isi berita asli)
df["kategori_baru"] = df["isi"].apply(label_kategori_isi)

# Pilih kolom sesuai permintaan
df_new = df[["id", "judul", "kategori_baru", "isi",
             "before", "after_lower_no_symbol", "after_stopword",
             "after_corrected", "after_stemmed", "tokens_final"]]

# Simpan ke file CSV baru di folder yang sama di Drive
df_new.to_csv('/content/drive/MyDrive/Semester 7/berita_kategori_baru.csv', index=False)

# Tampilkan distribusi berita per kategori
print("Distribusi berita per kategori:")
print(df_new["kategori_baru"].value_counts())

```

    Distribusi berita per kategori:
    kategori_baru
    news        72
    politik     48
    health      11
    travel      10
    olahraga     9
    Name: count, dtype: int64
    


```python
import pandas as pd
from IPython.display import display

# Baca dataset preprocessed
df = pd.read_csv('/content/drive/MyDrive/Semester 7/berita_kategori_baru.csv')

# Tampilkan 10 data teratas dengan semua kolom
display(df.head(10))

# Tampilkan info dataframe (biar kelihatan semua kolom & tipe datanya)
print("\nInfo Dataset:")
print(df.info())

```



  <div id="df-c48777e8-e8ad-4b67-9531-19566ce8450e" class="colab-df-container">
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
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-c48777e8-e8ad-4b67-9531-19566ce8450e')"
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
        document.querySelector('#df-c48777e8-e8ad-4b67-9531-19566ce8450e button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-c48777e8-e8ad-4b67-9531-19566ce8450e');
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


    <div id="df-59996446-a174-4c62-9d3f-a7e671bf6414">
      <button class="colab-df-quickchart" onclick="quickchart('df-59996446-a174-4c62-9d3f-a7e671bf6414')"
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
            document.querySelector('#df-59996446-a174-4c62-9d3f-a7e671bf6414 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>



    
    Info Dataset:
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 150 entries, 0 to 149
    Data columns (total 10 columns):
     #   Column                 Non-Null Count  Dtype 
    ---  ------                 --------------  ----- 
     0   id                     150 non-null    int64 
     1   judul                  150 non-null    object
     2   kategori_baru          150 non-null    object
     3   isi                    137 non-null    object
     4   before                 137 non-null    object
     5   after_lower_no_symbol  137 non-null    object
     6   after_stopword         137 non-null    object
     7   after_corrected        137 non-null    object
     8   after_stemmed          137 non-null    object
     9   tokens_final           150 non-null    object
    dtypes: int64(1), object(9)
    memory usage: 11.8+ KB
    None
    


```python
# Hapus baris yang kolom 'isi'-nya kosong (NaN)
df_clean = df.dropna(subset=['isi'])

# Simpan hasil bersih ke file baru di folder yang sama
df_clean.to_csv('/content/drive/MyDrive/Semester 7/berita_fiks.csv', index=False)

# Tampilkan info hasil pembersihan
print("Jumlah data sebelum dibersihkan:", len(df))
print("Jumlah data sesudah dibersihkan:", len(df_clean))
print("\nFile hasil bersih sudah disimpan sebagai: berita_fiks.csv")

```

    Jumlah data sebelum dibersihkan: 150
    Jumlah data sesudah dibersihkan: 137
    
    File hasil bersih sudah disimpan sebagai: berita_fiks.csv
    

#TF-IDF


```python
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer

# Baca dataset preprocessed
df = pd.read_csv('/content/drive/MyDrive/Semester 7/berita_fiks.csv')

# Ambil teks dari kolom hasil preprocessing (tokens_final)
corpus = df["tokens_final"].astype(str)

# Inisialisasi TF-IDF Vectorizer
vectorizer = TfidfVectorizer()

# Fit dan transform data
tfidf_matrix = vectorizer.fit_transform(corpus)

# Ubah jadi DataFrame biar lebih mudah dilihat
tfidf_df = pd.DataFrame(
    tfidf_matrix.toarray(),
    columns=vectorizer.get_feature_names_out(),
    index=df["id"]
)

# Tampilkan 10 dokumen pertama dengan 10 kata fitur pertama
print("Contoh hasil TF-IDF:")
print(tfidf_df.iloc[:10, :10])

# Simpan hasil TF-IDF ke file CSV
tfidf_df.to_csv("berita_tfidf.csv")
print("\nHasil TF-IDF disimpan di 'berita_tfidf.csv'")
print(tfidf_df.iloc[:5, :10])

```

    Contoh hasil TF-IDF:
        abadi  abai  abdi  abdul  abh     acara  aceh        ad  ada  adab
    id                                                                    
    1     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    2     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    3     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    4     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    5     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    6     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    7     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.000000  0.0   0.0
    8     0.0   0.0   0.0    0.0  0.0  0.000000   0.0  0.106124  0.0   0.0
    9     0.0   0.0   0.0    0.0  0.0  0.063151   0.0  0.000000  0.0   0.0
    10    0.0   0.0   0.0    0.0  0.0  0.085693   0.0  0.000000  0.0   0.0
    
    Hasil TF-IDF disimpan di 'berita_tfidf.csv'
        abadi  abai  abdi  abdul  abh  acara  aceh   ad  ada  adab
    id                                                            
    1     0.0   0.0   0.0    0.0  0.0    0.0   0.0  0.0  0.0   0.0
    2     0.0   0.0   0.0    0.0  0.0    0.0   0.0  0.0  0.0   0.0
    3     0.0   0.0   0.0    0.0  0.0    0.0   0.0  0.0  0.0   0.0
    4     0.0   0.0   0.0    0.0  0.0    0.0   0.0  0.0  0.0   0.0
    5     0.0   0.0   0.0    0.0  0.0    0.0   0.0  0.0  0.0   0.0
    


```python
from sklearn.feature_extraction.text import TfidfVectorizer

# Pakai tokens_final
df["tokens_joined"] = df["tokens_final"].apply(lambda x: " ".join(eval(x)))

vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(df["tokens_joined"])

# jumlah kata unik
print("Jumlah kata unik (vocabulary TF-IDF):", len(vectorizer.get_feature_names_out()))

```

    Jumlah kata unik (vocabulary TF-IDF): 2290
    

#Word Embeding


```python
!pip install gensim
```

    Requirement already satisfied: gensim in /usr/local/lib/python3.12/dist-packages (4.3.2)
    Requirement already satisfied: numpy>=1.18.5 in /usr/local/lib/python3.12/dist-packages (from gensim) (1.26.4)
    Requirement already satisfied: scipy>=1.7.0 in /usr/local/lib/python3.12/dist-packages (from gensim) (1.16.2)
    Requirement already satisfied: smart_open>=1.8.1 in /usr/local/lib/python3.12/dist-packages (from gensim) (7.3.1)
    Requirement already satisfied: wrapt in /usr/local/lib/python3.12/dist-packages (from smart_open>=1.8.1->gensim) (1.17.3)
    


```python
!pip install --upgrade --force-reinstall numpy gensim
```

    Collecting numpy
      Downloading numpy-2.3.3-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata (62 kB)
    [?25l     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m0.0/62.1 kB[0m [31m?[0m eta [36m-:--:--[0m
[2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m62.1/62.1 kB[0m [31m1.7 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting gensim
      Downloading gensim-4.3.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (8.1 kB)
    Collecting numpy
      Using cached numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
    Collecting scipy<1.14.0,>=1.7.0 (from gensim)
      Downloading scipy-1.13.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m60.6/60.6 kB[0m [31m3.7 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting smart-open>=1.8.1 (from gensim)
      Downloading smart_open-7.3.1-py3-none-any.whl.metadata (24 kB)
    Collecting wrapt (from smart-open>=1.8.1->gensim)
      Downloading wrapt-1.17.3-cp312-cp312-manylinux1_x86_64.manylinux_2_28_x86_64.manylinux_2_5_x86_64.whl.metadata (6.4 kB)
    Downloading gensim-4.3.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (26.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m26.6/26.6 MB[0m [31m55.8 MB/s[0m eta [36m0:00:00[0m
    [?25hUsing cached numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.0 MB)
    Downloading scipy-1.13.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (38.2 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m38.2/38.2 MB[0m [31m20.0 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading smart_open-7.3.1-py3-none-any.whl (61 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m61.7/61.7 kB[0m [31m2.8 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading wrapt-1.17.3-cp312-cp312-manylinux1_x86_64.manylinux_2_28_x86_64.manylinux_2_5_x86_64.whl (88 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m88.0/88.0 kB[0m [31m5.8 MB/s[0m eta [36m0:00:00[0m
    [?25hInstalling collected packages: wrapt, numpy, smart-open, scipy, gensim
      Attempting uninstall: wrapt
        Found existing installation: wrapt 1.17.3
        Uninstalling wrapt-1.17.3:
          Successfully uninstalled wrapt-1.17.3
      Attempting uninstall: numpy
        Found existing installation: numpy 1.26.4
        Uninstalling numpy-1.26.4:
          Successfully uninstalled numpy-1.26.4
      Attempting uninstall: smart-open
        Found existing installation: smart_open 7.3.1
        Uninstalling smart_open-7.3.1:
          Successfully uninstalled smart_open-7.3.1
      Attempting uninstall: scipy
        Found existing installation: scipy 1.16.2
        Uninstalling scipy-1.16.2:
          Successfully uninstalled scipy-1.16.2
      Attempting uninstall: gensim
        Found existing installation: gensim 4.3.2
        Uninstalling gensim-4.3.2:
          Successfully uninstalled gensim-4.3.2
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    opencv-python 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.4 which is incompatible.
    tsfresh 0.21.1 requires scipy>=1.14.0; python_version >= "3.10", but you have scipy 1.13.1 which is incompatible.
    opencv-python-headless 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.4 which is incompatible.
    opencv-contrib-python 4.12.0.88 requires numpy<2.3.0,>=2; python_version >= "3.9", but you have numpy 1.26.4 which is incompatible.
    thinc 8.3.6 requires numpy<3.0.0,>=2.0.0, but you have numpy 1.26.4 which is incompatible.[0m[31m
    [0mSuccessfully installed gensim-4.3.3 numpy-1.26.4 scipy-1.13.1 smart-open-7.3.1 wrapt-1.17.3
    




```python
import pandas as pd
import ast
from gensim.models import Word2Vec
```


```python
# Baca CSV dari path Drive
df = pd.read_csv('/content/drive/MyDrive/Semester 7/berita_fiks.csv')
df.head()
```





  <div id="df-e07e0363-096f-4c71-9aca-da600f4a01d1" class="colab-df-container">
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
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-e07e0363-096f-4c71-9aca-da600f4a01d1')"
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
        document.querySelector('#df-e07e0363-096f-4c71-9aca-da600f4a01d1 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-e07e0363-096f-4c71-9aca-da600f4a01d1');
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


    <div id="df-cb03b7d0-8303-4d68-98d9-b40bb37ac32d">
      <button class="colab-df-quickchart" onclick="quickchart('df-cb03b7d0-8303-4d68-98d9-b40bb37ac32d')"
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
            document.querySelector('#df-cb03b7d0-8303-4d68-98d9-b40bb37ac32d button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
corpus = df["tokens_final"].apply(
    lambda x: ast.literal_eval(x) if isinstance(x, str) else []
).tolist()
```


```python
model = Word2Vec(
    sentences=corpus,
    vector_size=100,   # dimensi vektor
    window=5,          # konteks window
    min_count=2,       # kata muncul minimal 2 kali
    sg=1,              # 1=skip-gram, 0=CBOW
    workers=4
)
```


```python
model.save("/content/drive/MyDrive/Semester 7/word2vec_berita.model")

```


```python
# Lihat total dan contoh kata dalam vocab
print("Total kata dalam model:", len(model.wv.index_to_key))
print("\n10 kata pertama dalam vocab:")
print(model.wv.index_to_key[:10])

```

    Total kata dalam model: 1224
    
    10 kata pertama dalam vocab:
    ['gol', 'tni', 'dpr', 'with', 'scroll', 'content', 'continue', 'kpk', 'polisi', 'pasar']
    


```python
# === Cek hasil ===
# Ganti kata target di sini sesuai kata yang ingin dilihat
kata_target = "masyarakat"

print(f"\nVektor untuk kata '{kata_target}':")
print(model.wv[kata_target][:10])  # tampilkan 10 nilai pertama dari vektor

print(f"\nKata yang mirip dengan '{kata_target}':")
for kata, skor in model.wv.most_similar(kata_target, topn=5):
    print(f"{kata} ({skor:.4f})")

```

    
    Vektor untuk kata 'masyarakat':
    [-0.03845457  0.08990767  0.12875776  0.02771006 -0.0289501  -0.18374357
      0.13039254  0.38831323 -0.10114109 -0.1788716 ]
    
    Kata yang mirip dengan 'masyarakat':
    kpk (0.9987)
    temu (0.9985)
    bangun (0.9985)
    kait (0.9985)
    orang (0.9985)
    
