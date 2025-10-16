# UTS Analisa Klasifikasi Berita

## Analisa Klasifikasi Berita dengan Ekstraksi Fitur Model dengan Classifier Naive Bayes dan SVM

## Input Data


```python
from google.colab import drive
drive.mount('/content/drive')

file_path = '/content/drive/MyDrive/Semester 7/Berita.csv'

```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
    


```python
import pandas as pd

df = pd.read_csv(file_path)
print("Jumlah data:", len(df))
df.head()
```

    Jumlah data: 1500
    





  <div id="df-92fc8b8d-9d4c-4218-90ee-ab62c54dba1a" class="colab-df-container">
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
      <th>No</th>
      <th>judul</th>
      <th>berita</th>
      <th>tanggal</th>
      <th>kategori</th>
      <th>link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Airlangga Harap Kenaikan UMP Tingkatkan Daya B...</td>
      <td>Menteri Koordinator (Menko) Bidang Perekonomia...</td>
      <td>Minggu, 01 Des 2024 23:40 WIB</td>
      <td>Ekonomi</td>
      <td>https://www.cnnindonesia.com/ekonomi/202412012...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>PT SIER Beri Penghargaan untuk 50 Tenant Terba...</td>
      <td>Dalam rangka memeriahkan hari jadi ke-50, PT S...</td>
      <td>Minggu, 01 Des 2024 20:45 WIB</td>
      <td>Ekonomi</td>
      <td>https://www.cnnindonesia.com/ekonomi/202412012...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Prabowo Bakal Bentuk Kementerian Penerimaan Ne...</td>
      <td>Wacana Presiden Prabowo Subianto akan membentu...</td>
      <td>Minggu, 01 Des 2024 19:40 WIB</td>
      <td>Ekonomi</td>
      <td>https://www.cnnindonesia.com/ekonomi/202412011...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Sinergi Kemenag &amp; BPJS Ketenagakerjaan Lindung...</td>
      <td>BPJS Ketenagakerjaan dan Kementerian Agama (Ke...</td>
      <td>Minggu, 01 Des 2024 19:03 WIB</td>
      <td>Ekonomi</td>
      <td>https://www.cnnindonesia.com/ekonomi/202412011...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Pemerintah Segera Bentuk Satgas PHK Usai Tetap...</td>
      <td>Pemerintah akan segera membentuk Satuan Tugas ...</td>
      <td>Minggu, 01 Des 2024 19:00 WIB</td>
      <td>Ekonomi</td>
      <td>https://www.cnnindonesia.com/ekonomi/202412011...</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-92fc8b8d-9d4c-4218-90ee-ab62c54dba1a')"
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
        document.querySelector('#df-92fc8b8d-9d4c-4218-90ee-ab62c54dba1a button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-92fc8b8d-9d4c-4218-90ee-ab62c54dba1a');
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


    <div id="df-77f96d69-019f-46a1-b4ed-18dd7602c2be">
      <button class="colab-df-quickchart" onclick="quickchart('df-77f96d69-019f-46a1-b4ed-18dd7602c2be')"
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
            document.querySelector('#df-77f96d69-019f-46a1-b4ed-18dd7602c2be button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




### Cek Jumlah dan Sebaran Data per Kategori


```python
# Hitung jumlah data di setiap kategori
kategori_counts = df['kategori'].value_counts()

# Tampilkan hasil
print("📊 Jumlah data per kategori:\n")
print(kategori_counts)

# Tampilkan juga total kategori unik
print("\n🔢 Total kategori unik:", df['kategori'].nunique())

```

    📊 Jumlah data per kategori:
    
    kategori
    Ekonomi          375
    Olahraga         375
    Nasional         375
    Internasional    375
    Name: count, dtype: int64
    
    🔢 Total kategori unik: 4
    


```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(8,5))
sns.barplot(x=kategori_counts.index, y=kategori_counts.values, palette='viridis')
plt.title("Sebaran Jumlah Data per Kategori Berita")
plt.xlabel("Kategori")
plt.ylabel("Jumlah Data")
plt.xticks(rotation=45)
plt.show()

```

    /tmp/ipython-input-1445065985.py:5: FutureWarning: 
    
    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `x` variable to `hue` and set `legend=False` for the same effect.
    
      sns.barplot(x=kategori_counts.index, y=kategori_counts.values, palette='viridis')
    


    
![png](UTS_Klasifikasi_files/UTS_Klasifikasi_7_1.png)
    


## Prepocessing


```python
pip install sastrawi
```

    Collecting sastrawi
      Downloading Sastrawi-1.0.1-py2.py3-none-any.whl.metadata (909 bytes)
    Downloading Sastrawi-1.0.1-py2.py3-none-any.whl (209 kB)
    [?25l   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m0.0/209.7 kB[0m [31m?[0m eta [36m-:--:--[0m[2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m209.7/209.7 kB[0m [31m8.3 MB/s[0m eta [36m0:00:00[0m
    [?25hInstalling collected packages: sastrawi
    Successfully installed sastrawi-1.0.1
    


```python
import re
import nltk
from nltk.corpus import stopwords
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory

# Download stopwords Bahasa Indonesia
nltk.download('stopwords')
stop_words = set(stopwords.words('indonesian'))

# Inisialisasi stemmer Bahasa Indonesia
factory = StemmerFactory()
stemmer = factory.create_stemmer()

# Fungsi preprocessing
def preprocess(text):
    text = str(text).lower()                              # Case folding
    text = re.sub(r'http\S+|www.\S+', '', text)           # Hapus URL
    text = re.sub(r'[^a-z\s]', '', text)                  # Hapus simbol dan angka
    tokens = text.split()                                 # Tokenisasi
    tokens = [t for t in tokens if t not in stop_words]   # Stopword removal
    tokens = [stemmer.stem(t) for t in tokens]            # Stemming
    return ' '.join(tokens)                               # Gabungkan kembali

# Terapkan ke kolom berita
df['cleaned'] = df['berita'].apply(preprocess)
df[['berita', 'cleaned']].head()

```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Unzipping corpora/stopwords.zip.
    





  <div id="df-8c4f20dd-a8eb-4eb9-8b98-878864b799ca" class="colab-df-container">
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
      <th>berita</th>
      <th>cleaned</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Menteri Koordinator (Menko) Bidang Perekonomia...</td>
      <td>menteri koordinator menko bidang ekonomi airla...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dalam rangka memeriahkan hari jadi ke-50, PT S...</td>
      <td>rangka riah pt surabaya industrial estate rung...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wacana Presiden Prabowo Subianto akan membentu...</td>
      <td>wacana presiden prabowo subianto bentuk bentuk...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BPJS Ketenagakerjaan dan Kementerian Agama (Ke...</td>
      <td>bpjs ketenagakerjaan menteri agama kemenag lin...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pemerintah akan segera membentuk Satuan Tugas ...</td>
      <td>perintah bentuk satu tugas putus hubung kerja ...</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-8c4f20dd-a8eb-4eb9-8b98-878864b799ca')"
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
        document.querySelector('#df-8c4f20dd-a8eb-4eb9-8b98-878864b799ca button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-8c4f20dd-a8eb-4eb9-8b98-878864b799ca');
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


    <div id="df-7ac57849-b034-4d43-aefb-5bb1b9312048">
      <button class="colab-df-quickchart" onclick="quickchart('df-7ac57849-b034-4d43-aefb-5bb1b9312048')"
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
            document.querySelector('#df-7ac57849-b034-4d43-aefb-5bb1b9312048 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
# Tampilkan 10 contoh pertama biar kelihatan jelas
pd.set_option('display.max_colwidth', 100)  # biar teksnya gak terpotong

print("📊 Perbandingan Sebelum dan Sesudah Preprocessing:\n")
display(df[['berita', 'cleaned']].head(10))
```

    📊 Perbandingan Sebelum dan Sesudah Preprocessing:
    
    



  <div id="df-e837d526-cd21-4fd9-aab6-eca5665ebfd8" class="colab-df-container">
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
      <th>berita</th>
      <th>cleaned</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Menteri Koordinator (Menko) Bidang Perekonomian Airlangga Hartarto berharap kenaikan upah minimu...</td>
      <td>menteri koordinator menko bidang ekonomi airlangga hartarto harap naik upah minimum provinsi ump...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dalam rangka memeriahkan hari jadi ke-50, PT Surabaya Industrial Estate Rungkut (PT SIER) mengge...</td>
      <td>rangka riah pt surabaya industrial estate rungkut pt sier gelar acara tajuk green industrial awa...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wacana Presiden Prabowo Subianto akan membentuk akan membentuk Kementerian Penerimaan Negara kem...</td>
      <td>wacana presiden prabowo subianto bentuk bentuk menteri terima negara santer beredarsinyal bentuk...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BPJS Ketenagakerjaan dan Kementerian Agama (Kemenag) memberikan perlindungan jaminan sosial kete...</td>
      <td>bpjs ketenagakerjaan menteri agama kemenag lindung jamin sosial ketenagakerjaan ribu guru tenaga...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pemerintah akan segera membentuk Satuan Tugas Pemutusan Hubungan Kerja (Satgas PHK) usai menetap...</td>
      <td>perintah bentuk satu tugas putus hubung kerja satgas phk tetap upah minimum provinsi ump persenr...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Menko Bidang Infrastruktur dan Pembangunan Kewilayahan Agus Harimurti Yudhoyono (AHY) buka-bukaa...</td>
      <td>menko bidang infrastruktur bangun wilayah agus harimurti yudhoyono ahy bukabukaan nasib bangun k...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Kepala Badan Gizi Nasional Dadan Hindayana menyebut anggaran makan bergizi gratis sebesar Rp10 r...</td>
      <td>kepala badan gizi nasional dad hindayana sebut anggar makan gizi gratis rp ribu anak harga ratar...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Menteri Koordinator Bidang Pangan Zulkifli Hasan meminta tambahan anggaran Rp510 miliar kepada D...</td>
      <td>menteri koordinator bidang pangan zulkifli hasan tambah anggar rp miliar dpr tambah anggar mente...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Uji coba alias commissioning pembangkit listrik tenaga surya (PLTS) di IKN Nusantara bakal dilak...</td>
      <td>uji coba alias commissioning bangkit listrik tenaga surya plts ikn nusantara desember direktur u...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Anak crazy rich pengusaha sawit Kalimantan Samsudin Andi Arsyad alias Haji Isam, Jhony Saputra m...</td>
      <td>anak crazy rich usaha sawit kalimantan samsudin andi arsyad alias haji isam jhony saputra sorot ...</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-e837d526-cd21-4fd9-aab6-eca5665ebfd8')"
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
        document.querySelector('#df-e837d526-cd21-4fd9-aab6-eca5665ebfd8 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-e837d526-cd21-4fd9-aab6-eca5665ebfd8');
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


    <div id="df-7504a54d-7d33-4fbd-bac8-a45964b6a708">
      <button class="colab-df-quickchart" onclick="quickchart('df-7504a54d-7d33-4fbd-bac8-a45964b6a708')"
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
            document.querySelector('#df-7504a54d-7d33-4fbd-bac8-a45964b6a708 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>



langkah-langkah preprocessing:

1. **Case folding** – Mengubah semua huruf menjadi huruf kecil supaya kata yang sama tapi berbeda kapitalisasi dianggap identik.

2. **Hapus URL** – Menghilangkan alamat web atau link karena biasanya tidak relevan untuk analisis teks.

3. **Hapus simbol dan angka** – Menghapus tanda baca dan angka agar fokus pada kata yang bermakna.

4. **Tokenisasi** – Memecah teks menjadi kata-kata individual agar bisa dianalisis satu per satu.

5. **Stopword removal** – Menghapus kata-kata umum seperti “dan”, “yang”, “di” yang tidak memberikan informasi penting.

6. **Stemming** – Mengubah kata ke bentuk dasar agar kata turunan dianggap sama, misal “berlari” → “lari”.


## Ekstraksi Fitur dengan Topic Modeling (LDA)


```python
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.decomposition import LatentDirichletAllocation

# Ubah teks bersih menjadi representasi bag-of-words
vectorizer = CountVectorizer(max_features=1000)  # batasi 1000 kata agar efisien
X_counts = vectorizer.fit_transform(df['cleaned'])

# Gunakan LDA untuk menemukan topik
lda = LatentDirichletAllocation(n_components=10, random_state=42)  # 10 topik
X_topics = lda.fit_transform(X_counts)

# Cek bentuk data hasil ekstraksi fitur
print("Bentuk fitur topik:", X_topics.shape)
```

    Bentuk fitur topik: (1500, 10)
    

Kode ini mengubah teks menjadi fitur topik numerik sehingga dokumen bisa dianalisis, diklasifikasikan, atau divisualisasi berdasarkan topik yang terkandung.Digunakan 1000 kata untuk dihtung dan diambil 10 topik berdasarkan kata yang paling dominan.

## Split Data untuk Training dan Testing


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X_topics, y, test_size=0.2, random_state=42, stratify=y
)
```

## Klasifikasi dengan Naïve Bayes


```python
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

nb = MultinomialNB()
nb.fit(X_train, y_train)
y_pred_nb = nb.predict(X_test)

print("🎯 Akurasi Naïve Bayes:", accuracy_score(y_test, y_pred_nb))
print("\n📄 Classification Report (Naïve Bayes):\n", classification_report(y_test, y_pred_nb, target_names=label_encoder.classes_))
```

    🎯 Akurasi Naïve Bayes: 0.8533333333333334
    
    📄 Classification Report (Naïve Bayes):
                    precision    recall  f1-score   support
    
          Ekonomi       0.79      0.93      0.85        75
    Internasional       0.84      0.87      0.86        75
         Nasional       0.80      0.63      0.70        75
         Olahraga       0.99      0.99      0.99        75
    
         accuracy                           0.85       300
        macro avg       0.85      0.85      0.85       300
     weighted avg       0.85      0.85      0.85       300
    
    

## Klasifikasi dengan SVM


```python
from sklearn.svm import LinearSVC

svm = LinearSVC(random_state=42)
svm.fit(X_train, y_train)
y_pred_svm = svm.predict(X_test)

print("🎯 Akurasi SVM:", accuracy_score(y_test, y_pred_svm))
print("\n📄 Classification Report (SVM):\n", classification_report(y_test, y_pred_svm, target_names=label_encoder.classes_))
```

    🎯 Akurasi SVM: 0.83
    
    📄 Classification Report (SVM):
                    precision    recall  f1-score   support
    
          Ekonomi       0.76      0.93      0.84        75
    Internasional       0.88      0.69      0.78        75
         Nasional       0.71      0.69      0.70        75
         Olahraga       0.99      1.00      0.99        75
    
         accuracy                           0.83       300
        macro avg       0.84      0.83      0.83       300
     weighted avg       0.84      0.83      0.83       300
    
    

## Visualisasi Confusion Matrix


```python
import matplotlib.pyplot as plt
import seaborn as sns

def plot_confusion(y_true, y_pred, title):
    cm = confusion_matrix(y_true, y_pred)
    plt.figure(figsize=(8,6))
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
                xticklabels=label_encoder.classes_,
                yticklabels=label_encoder.classes_)
    plt.xlabel('Predicted')
    plt.ylabel('Actual')
    plt.title(title)
    plt.show()

plot_confusion(y_test, y_pred_nb, "Confusion Matrix - Naïve Bayes")
plot_confusion(y_test, y_pred_svm, "Confusion Matrix - SVM")

```


    
![png](UTS_Klasifikasi_files/UTS_Klasifikasi_23_0.png)
    



    
![png](UTS_Klasifikasi_files/UTS_Klasifikasi_23_1.png)
    

