# UTS Analisa Clustering

## Analisa Clustering Dokumen pada Data Email

## Import Library


```python
import pandas as pd
import re
import string
import nltk
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Unduh stopwords jika belum ada
nltk.download('stopwords')

```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Unzipping corpora/stopwords.zip.
    




    True



## Input Data


```python
from google.colab import drive
drive.mount('/content/drive')

file_path = '/content/drive/MyDrive/Semester 7/spam.csv'

```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
    


```python
import pandas as pd
from IPython.display import display

# Baca file CSV dengan encoding latin-1
df = pd.read_csv('/content/drive/MyDrive/Semester 7/spam.csv', encoding='latin-1')

# Hapus semua kolom kosong dan kolom Unnamed
df = df.loc[:, ~df.columns.str.contains('^Unnamed')]

# Tampilkan nama kolom setelah dibersihkan
print(df.columns)

# Tampilkan 5 data teratas dengan tampilan tabel
display(df.head())

# Tampilkan jumlah total data
print("\n===== Jumlah Total Data =====")
print("Jumlah data:", len(df))

```

    Index(['id', 'Text'], dtype='object')
    



  <div id="df-2961e41d-ff81-40fc-92fa-6211692c4ae8" class="colab-df-container">
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
      <th>Text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Go until jurong point, crazy.. Available only ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Ok lar... Joking wif u oni...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>U dun say so early hor... U c already then say...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Nah I don't think he goes to usf, he lives aro...</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-2961e41d-ff81-40fc-92fa-6211692c4ae8')"
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
        document.querySelector('#df-2961e41d-ff81-40fc-92fa-6211692c4ae8 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-2961e41d-ff81-40fc-92fa-6211692c4ae8');
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


    <div id="df-4da6c07b-3999-423c-be23-cb1fa06aa965">
      <button class="colab-df-quickchart" onclick="quickchart('df-4da6c07b-3999-423c-be23-cb1fa06aa965')"
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
            document.querySelector('#df-4da6c07b-3999-423c-be23-cb1fa06aa965 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>



    
    ===== Jumlah Total Data =====
    Jumlah data: 5572
    

## Prepocessing


```python
import re
import string
import nltk
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer
from IPython.display import display

# Unduh stopwords (jika belum)
nltk.download('stopwords')

# Inisialisasi stopwords dan stemmer
stop_words = set(stopwords.words('english'))
stemmer = PorterStemmer()

# --- Fungsi Preprocessing ---
def preprocess(text):
    text = str(text).lower()                                       # ubah ke huruf kecil
    text = re.sub(r'http\S+|www\S+|https\S+', '', text)           # hapus URL
    text = re.sub(r'\d+', '', text)                                # hapus angka
    text = text.translate(str.maketrans('', '', string.punctuation))  # hapus tanda baca
    tokens = text.split()                                          # pisahkan kata
    tokens = [stemmer.stem(word) for word in tokens if word not in stop_words]  # hapus stopword & stemming
    return " ".join(tokens)

# --- Terapkan Preprocessing ke Seluruh Data ---
df['clean_text'] = df['Text'].apply(preprocess)

# --- Tampilkan 5 Data Teratas Sebelum & Sesudah ---
print("===== Contoh 5 Data Sebelum & Sesudah Preprocessing =====")
display(df[['Text', 'clean_text']].head())

```

    [nltk_data] Downloading package stopwords to /root/nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    

    ===== Contoh 5 Data Sebelum & Sesudah Preprocessing =====
    



  <div id="df-5824ce01-94ce-46bb-b2ce-5de80e14662f" class="colab-df-container">
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
      <th>Text</th>
      <th>clean_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Go until jurong point, crazy.. Available only ...</td>
      <td>go jurong point crazi avail bugi n great world...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ok lar... Joking wif u oni...</td>
      <td>ok lar joke wif u oni</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>
      <td>free entri wkli comp win fa cup final tkt st m...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>U dun say so early hor... U c already then say...</td>
      <td>u dun say earli hor u c alreadi say</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nah I don't think he goes to usf, he lives aro...</td>
      <td>nah dont think goe usf live around though</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-5824ce01-94ce-46bb-b2ce-5de80e14662f')"
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
        document.querySelector('#df-5824ce01-94ce-46bb-b2ce-5de80e14662f button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-5824ce01-94ce-46bb-b2ce-5de80e14662f');
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


    <div id="df-fece6a7f-da0b-4127-a834-cadb6d54e471">
      <button class="colab-df-quickchart" onclick="quickchart('df-fece6a7f-da0b-4127-a834-cadb6d54e471')"
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
            document.querySelector('#df-fece6a7f-da0b-4127-a834-cadb6d54e471 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>



- Lowercasing – Mengubah semua huruf menjadi huruf kecil supaya kata yang sama tapi berbeda kapitalisasi dianggap identik.

- Hapus URL – Menghilangkan tautan atau alamat web yang biasanya tidak relevan untuk analisis teks.

- Hapus angka – Menghapus semua angka karena sering tidak berpengaruh pada makna inti teks.

- Hapus tanda baca – Menghilangkan simbol seperti titik, koma, tanda tanya, dll., agar analisis fokus pada kata saja.

- Tokenisasi – Memecah kalimat menjadi kata-kata individual agar bisa dianalisis satu per satu.

- Hapus stopwords & Stemming – Stopwords dihapus karena kata-kata umum  tidak informatif, dan stemming mengubah kata ke bentuk dasar agar kata turunan dianggap sama dengan kata dasarnya.

## Ekstraksi Fitur Menggunakan TF-IDF


```python
from sklearn.feature_extraction.text import TfidfVectorizer

# --- Inisialisasi TF-IDF Vectorizer ---
tfidf = TfidfVectorizer(
    max_features=1000,      # ambil 1000 kata paling penting
    ngram_range=(1, 2),     # unigram + bigram
    min_df=2,               # kata muncul minimal di 2 dokumen
    max_df=0.8,             # kata muncul di >80% dokumen diabaikan
    stop_words='english'    # hapus stopwords bahasa Inggris
)

# --- Terapkan TF-IDF ke kolom clean_text ---
tfidf_matrix = tfidf.fit_transform(df['clean_text'])

# --- Ukuran matriks TF-IDF ---
print("===== Ukuran Matriks TF-IDF =====")
print(tfidf_matrix.shape)

# --- Contoh 10 fitur (kata) teratas ---
print("\n===== Contoh 10 Fitur (Kata) TF-IDF =====")
print(tfidf.get_feature_names_out()[:10])

```

    ===== Ukuran Matriks TF-IDF =====
    (5572, 1000)
    
    ===== Contoh 10 Fitur (Kata) TF-IDF =====
    ['abiola' 'abl' 'abt' 'accept' 'access' 'account' 'account statement'
     'activ' 'actual' 'ad']
    


```python
from sklearn.feature_extraction.text import TfidfVectorizer
import pandas as pd

# --- Inisialisasi TF-IDF Vectorizer ---
vectorizer = TfidfVectorizer(
    stop_words='english',   # hapus stopwords bahasa Inggris
    lowercase=True,         # ubah semua ke huruf kecil
    max_features=1000       # ambil 1000 kata paling penting
)

# --- Hitung TF-IDF dari kolom 'clean_text' ---
tfidf_matrix = vectorizer.fit_transform(df['clean_text'])

# --- Ubah hasilnya ke DataFrame ---
tfidf_df = pd.DataFrame(
    tfidf_matrix.toarray(),
    columns=vectorizer.get_feature_names_out()
)

# --- Tampilkan hasil ---
print("🔹 Ukuran matriks TF-IDF:", tfidf_df.shape)
print("\n🔹 5 Data Pertama Hasil TF-IDF:\n")
print(tfidf_df.head())

# --- Lihat 10 kata dengan bobot TF-IDF tertinggi rata-rata ---
mean_tfidf = tfidf_df.mean().sort_values(ascending=False)
print("\n🔹 10 Kata dengan Bobot TF-IDF Tertinggi:\n")
print(mean_tfidf.head(10))

```

    🔹 Ukuran matriks TF-IDF: (5572, 1000)
    
    🔹 5 Data Pertama Hasil TF-IDF:
    
       abiola  abl  abt  accept  access  account  activ  actual   ad  add  ...  \
    0     0.0  0.0  0.0     0.0     0.0      0.0    0.0     0.0  0.0  0.0  ...   
    1     0.0  0.0  0.0     0.0     0.0      0.0    0.0     0.0  0.0  0.0  ...   
    2     0.0  0.0  0.0     0.0     0.0      0.0    0.0     0.0  0.0  0.0  ...   
    3     0.0  0.0  0.0     0.0     0.0      0.0    0.0     0.0  0.0  0.0  ...   
    4     0.0  0.0  0.0     0.0     0.0      0.0    0.0     0.0  0.0  0.0  ...   
    
       yesterday   yo  yoga  youd  youll  youv   yr  yup   ìï   ûò  
    0        0.0  0.0   0.0   0.0    0.0   0.0  0.0  0.0  0.0  0.0  
    1        0.0  0.0   0.0   0.0    0.0   0.0  0.0  0.0  0.0  0.0  
    2        0.0  0.0   0.0   0.0    0.0   0.0  0.0  0.0  0.0  0.0  
    3        0.0  0.0   0.0   0.0    0.0   0.0  0.0  0.0  0.0  0.0  
    4        0.0  0.0   0.0   0.0    0.0   0.0  0.0  0.0  0.0  0.0  
    
    [5 rows x 1000 columns]
    
    🔹 10 Kata dengan Bobot TF-IDF Tertinggi:
    
    im      0.023752
    ok      0.020941
    come    0.018365
    ur      0.015828
    ill     0.015607
    dont    0.014569
    know    0.014518
    ltgt    0.014502
    like    0.014183
    time    0.013879
    dtype: float64
    

5572 merupakan banyak data dan 1000 merupakan banyaknya kata unik


```python
print("Jumlah fitur TF-IDF:", len(vectorizer.get_feature_names_out()))
print("Contoh fitur:", vectorizer.get_feature_names_out()[:20])

```

    Jumlah fitur TF-IDF: 1000
    Contoh fitur: ['abiola' 'abl' 'abt' 'accept' 'access' 'account' 'activ' 'actual' 'ad'
     'add' 'address' 'admir' 'aft' 'afternoon' 'age' 'ago' 'ah' 'aha' 'aight'
     'aint']
    

## Clustering Dokumen Menggunakan K-Means


```python
from sklearn.cluster import KMeans
from IPython.display import display

# --- Tentukan jumlah cluster (misal 2: spam & non-spam) ---
num_clusters = 2

# --- Inisialisasi model KMeans ---
kmeans = KMeans(n_clusters=num_clusters, random_state=42, n_init=10)

# --- Latih model KMeans dengan data TF-IDF ---
kmeans.fit(tfidf_matrix)

# --- Simpan hasil cluster ke dataframe ---
df['cluster'] = kmeans.labels_

# --- Tampilkan 10 data pertama beserta cluster ---
print("===== Contoh 10 Data dengan Label Cluster =====")
display(df[['Text', 'clean_text', 'cluster']].head(10))

# --- Lihat jumlah data per cluster ---
print("\n===== Jumlah Data per Cluster =====")
print(df['cluster'].value_counts())

```

    ===== Contoh 10 Data dengan Label Cluster =====
    



  <div id="df-8d8b37cf-e391-4c5b-bae3-64f9369ac14b" class="colab-df-container">
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
      <th>Text</th>
      <th>clean_text</th>
      <th>cluster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Go until jurong point, crazy.. Available only ...</td>
      <td>go jurong point crazi avail bugi n great world...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ok lar... Joking wif u oni...</td>
      <td>ok lar joke wif u oni</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>
      <td>free entri wkli comp win fa cup final tkt st m...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>U dun say so early hor... U c already then say...</td>
      <td>u dun say earli hor u c alreadi say</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nah I don't think he goes to usf, he lives aro...</td>
      <td>nah dont think goe usf live around though</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>FreeMsg Hey there darling it's been 3 week's n...</td>
      <td>freemsg hey darl week word back id like fun st...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Even my brother is not like to speak with me. ...</td>
      <td>even brother like speak treat like aid patent</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>As per your request 'Melle Melle (Oru Minnamin...</td>
      <td>per request mell mell oru minnaminungint nurun...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>WINNER!! As a valued network customer you have...</td>
      <td>winner valu network custom select receivea å£ ...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Had your mobile 11 months or more? U R entitle...</td>
      <td>mobil month u r entitl updat latest colour mob...</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-8d8b37cf-e391-4c5b-bae3-64f9369ac14b')"
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
        document.querySelector('#df-8d8b37cf-e391-4c5b-bae3-64f9369ac14b button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-8d8b37cf-e391-4c5b-bae3-64f9369ac14b');
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


    <div id="df-f66cdd38-95be-4553-b7c9-0a9475394ad6">
      <button class="colab-df-quickchart" onclick="quickchart('df-f66cdd38-95be-4553-b7c9-0a9475394ad6')"
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
            document.querySelector('#df-f66cdd38-95be-4553-b7c9-0a9475394ad6 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>



    
    ===== Jumlah Data per Cluster =====
    cluster
    0    5171
    1     401
    Name: count, dtype: int64
    

Dilakukan clustering dengan dua cluster yaitu 0 dan 1, pada code diatas dapat dilihat banyanya data cluster 0 dan 1


```python
import numpy as np

terms = vectorizer.get_feature_names_out()
order_centroids = kmeans.cluster_centers_.argsort()[:, ::-1]

for i in range(num_clusters):
    print(f"\nCluster {i} - Top 10 Kata:")
    print(", ".join([terms[ind] for ind in order_centroids[i, :10]]))

```

    
    Cluster 0 - Top 10 Kata:
    ok, come, ur, ill, ltgt, dont, know, like, got, time
    
    Cluster 1 - Top 10 Kata:
    im, home, gonna, want, ill, work, sorri, realli, know, ok
    

Code diatas menampilkan pengelompokan kata yang mirip

## Evaluasi Clustering


```python
from sklearn.metrics import silhouette_score

# Hitung skor silhouette
score = silhouette_score(tfidf_matrix, kmeans.labels_)

print("===== Evaluasi Clustering =====")
print("Silhouette Score:", score)

```

    ===== Evaluasi Clustering =====
    Silhouette Score: 0.012418589964713801
    

Silhouette Score mengukur kualitas hasil clustering

## Visualisasi


```python
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Reduksi dimensi ke 2 komponen utama
pca = PCA(n_components=2)
reduced_data = pca.fit_transform(tfidf_matrix.toarray())

# Plot hasil clustering
plt.figure(figsize=(8, 6))
plt.scatter(reduced_data[:, 0], reduced_data[:, 1], c=kmeans.labels_, cmap='viridis', s=20)
plt.title('Visualisasi Clustering Email (TF-IDF + KMeans)')
plt.xlabel('PCA Component 1')
plt.ylabel('PCA Component 2')
plt.show()

```


    
![png](UTS__Clustering_files/UTS__Clustering_24_0.png)
    


- Component 1 (PC1) → arah yang menangkap perbedaan kata-kata paling signifikan antar dokumen.
- Component 2 (PC2) → arah kedua yang menangkap perbedaan penting berikutnya yang tidak ditangkap PC1.
