# Web Usage


```python
import pandas as pd
from IPython.display import display

# Baca file CSV
df = pd.read_csv("webuage.csv")

# Filter hanya yang mengandung '.html' dan status = 200
filtered_df = df[df['Request URI'].str.contains('.html', na=False)]

 # --- Hitung total data setelah difilter ---
print("Total data setelah difilter:", len(filtered_df))

# Tampilkan 20 baris teratas
display(filtered_df.head(20))


```

    Total data setelah difilter: 79176
    



  <div id="df-990e3bb9-2dcb-4461-9a8c-c01515f1e3b9" class="colab-df-container">
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
      <th>Remote host</th>
      <th>Remote logname</th>
      <th>Remote user</th>
      <th>Request time</th>
      <th>Request method</th>
      <th>Request URI</th>
      <th>Request Protocol</th>
      <th>Status</th>
      <th>Size of response (incl. headers)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>65.55.147.227</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:00:24Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>21878</td>
    </tr>
    <tr>
      <th>1</th>
      <td>65.55.86.34</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:00:58Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1416</td>
    </tr>
    <tr>
      <th>2</th>
      <td>148.188.55.88</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:01:41Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>10946</td>
    </tr>
    <tr>
      <th>4</th>
      <td>66.249.139.233</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:09Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>17247</td>
    </tr>
    <tr>
      <th>5</th>
      <td>72.30.50.248</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:13Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.0</td>
      <td>200</td>
      <td>7883</td>
    </tr>
    <tr>
      <th>8</th>
      <td>65.55.80.97</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:51Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1416</td>
    </tr>
    <tr>
      <th>9</th>
      <td>65.55.161.41</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:54Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>37122</td>
    </tr>
    <tr>
      <th>10</th>
      <td>65.55.119.204</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:55Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>64380</td>
    </tr>
    <tr>
      <th>11</th>
      <td>65.55.58.168</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:56Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1202</td>
    </tr>
    <tr>
      <th>12</th>
      <td>65.55.35.29</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:02:55Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>269198</td>
    </tr>
    <tr>
      <th>14</th>
      <td>65.55.34.249</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:18Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>6775</td>
    </tr>
    <tr>
      <th>16</th>
      <td>99.249.191.8</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:26Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>788</td>
    </tr>
    <tr>
      <th>17</th>
      <td>99.249.180.68</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:26Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>205</td>
    </tr>
    <tr>
      <th>19</th>
      <td>99.249.82.166</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:26Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>671</td>
    </tr>
    <tr>
      <th>20</th>
      <td>99.249.139.155</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:26Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>757</td>
    </tr>
    <tr>
      <th>21</th>
      <td>99.249.91.194</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:26Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>935</td>
    </tr>
    <tr>
      <th>26</th>
      <td>65.55.153.28</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:39Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1416</td>
    </tr>
    <tr>
      <th>28</th>
      <td>65.55.107.149</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:40Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>64380</td>
    </tr>
    <tr>
      <th>29</th>
      <td>65.55.247.6</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:41Z</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1202</td>
    </tr>
    <tr>
      <th>31</th>
      <td>65.55.22.3</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15T02:03:50Z</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>4225</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-990e3bb9-2dcb-4461-9a8c-c01515f1e3b9')"
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
        document.querySelector('#df-990e3bb9-2dcb-4461-9a8c-c01515f1e3b9 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-990e3bb9-2dcb-4461-9a8c-c01515f1e3b9');
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


    <div id="df-88440404-164f-42ca-9538-1ff0810ace55">
      <button class="colab-df-quickchart" onclick="quickchart('df-88440404-164f-42ca-9538-1ff0810ace55')"
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
            document.querySelector('#df-88440404-164f-42ca-9538-1ff0810ace55 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




```python
import pandas as pd
from IPython.display import display

# Baca file CSV
df = pd.read_csv("webuage.csv")

# Ubah kolom Request time ke format datetime
df['Request time'] = pd.to_datetime(df['Request time'], errors='coerce')

# Filter hanya yang mengandung '.html' dan status = 200
filtered_df = df[df['Request URI'].str.contains('.html', na=False) & (df['Status'] == 200)]

# --- Hitung total data setelah difilter ---
print("Total data setelah difilter:", len(filtered_df))

# Tampilkan 20 baris teratas
display(filtered_df.head(20))

```

    Total data setelah difilter: 75718
    



  <div id="df-e271cedb-13b8-4883-8d87-6f713eba5de8" class="colab-df-container">
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
      <th>Remote host</th>
      <th>Remote logname</th>
      <th>Remote user</th>
      <th>Request time</th>
      <th>Request method</th>
      <th>Request URI</th>
      <th>Request Protocol</th>
      <th>Status</th>
      <th>Size of response (incl. headers)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>65.55.147.227</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:00:24+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>21878</td>
    </tr>
    <tr>
      <th>1</th>
      <td>65.55.86.34</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:00:58+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1416</td>
    </tr>
    <tr>
      <th>2</th>
      <td>148.188.55.88</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:01:41+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>10946</td>
    </tr>
    <tr>
      <th>4</th>
      <td>66.249.139.233</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:09+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>17247</td>
    </tr>
    <tr>
      <th>5</th>
      <td>72.30.50.248</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:13+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.0</td>
      <td>200</td>
      <td>7883</td>
    </tr>
    <tr>
      <th>8</th>
      <td>65.55.80.97</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:51+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1416</td>
    </tr>
    <tr>
      <th>9</th>
      <td>65.55.161.41</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:54+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>37122</td>
    </tr>
    <tr>
      <th>10</th>
      <td>65.55.119.204</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:55+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>64380</td>
    </tr>
    <tr>
      <th>11</th>
      <td>65.55.58.168</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:56+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1202</td>
    </tr>
    <tr>
      <th>12</th>
      <td>65.55.35.29</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:02:55+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>269198</td>
    </tr>
    <tr>
      <th>14</th>
      <td>65.55.34.249</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:18+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>6775</td>
    </tr>
    <tr>
      <th>16</th>
      <td>99.249.191.8</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:26+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>788</td>
    </tr>
    <tr>
      <th>17</th>
      <td>99.249.180.68</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:26+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>205</td>
    </tr>
    <tr>
      <th>19</th>
      <td>99.249.82.166</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:26+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>671</td>
    </tr>
    <tr>
      <th>20</th>
      <td>99.249.139.155</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:26+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>757</td>
    </tr>
    <tr>
      <th>21</th>
      <td>99.249.91.194</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:26+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>935</td>
    </tr>
    <tr>
      <th>26</th>
      <td>65.55.153.28</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:39+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1416</td>
    </tr>
    <tr>
      <th>28</th>
      <td>65.55.107.149</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:40+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>64380</td>
    </tr>
    <tr>
      <th>29</th>
      <td>65.55.247.6</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:41+00:00</td>
      <td>GET</td>
      <td>/index.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>1202</td>
    </tr>
    <tr>
      <th>31</th>
      <td>65.55.22.3</td>
      <td>-</td>
      <td>-</td>
      <td>2009-10-15 02:03:50+00:00</td>
      <td>GET</td>
      <td>/faq.html</td>
      <td>HTTP/1.1</td>
      <td>200</td>
      <td>4225</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-e271cedb-13b8-4883-8d87-6f713eba5de8')"
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
        document.querySelector('#df-e271cedb-13b8-4883-8d87-6f713eba5de8 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-e271cedb-13b8-4883-8d87-6f713eba5de8');
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


    <div id="df-0f31aa37-6da4-479b-8510-74b8076be73c">
      <button class="colab-df-quickchart" onclick="quickchart('df-0f31aa37-6da4-479b-8510-74b8076be73c')"
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
            document.querySelector('#df-0f31aa37-6da4-479b-8510-74b8076be73c button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




```python
# Simpan hasil filter ke file CSV baru
filtered_df.to_csv("filter_webuage.csv", index=False)
print("File hasil filter berhasil disimpan sebagai 'filter_webuage.csv'")

```

    File hasil filter berhasil disimpan sebagai 'filter_webuage.csv'
    


```python
import pandas as pd
from IPython.display import display

# --- Baca file CSV ---
df = pd.read_csv("webuage.csv")

# --- Ubah kolom waktu jadi datetime ---
df['Request time'] = pd.to_datetime(df['Request time'], errors='coerce')

# --- Filter hanya request .html dan status 200 ---
filtered_df = df[df['Request URI'].str.contains('.html', na=False) & (df['Status'] == 200)]

# --- Hitung jumlah user unik berdasarkan Remote host ---
unique_hosts = filtered_df['Remote host'].nunique()
print(f"Jumlah user unik berdasarkan Remote host: {unique_hosts}")

# --- Hitung kombinasi unik Remote host + Request Protocol ---
unique_combinations = filtered_df[['Remote host', 'Request Protocol']].drop_duplicates()
print(f"Jumlah kombinasi unik host + protocol: {len(unique_combinations)}")

# --- Hitung jumlah request tiap user per protocol ---
user_request_counts = (
    filtered_df.groupby(['Remote host', 'Request Protocol'])
    .size()
    .reset_index(name='Jumlah Request')
    .sort_values(by='Jumlah Request', ascending=False)
)

# --- Tampilkan hasil dalam bentuk tabel ---
print("\nDaftar 10 user teratas berdasarkan jumlah request:")
display(user_request_counts.head(10))

```

    Jumlah user unik berdasarkan Remote host: 74895
    Jumlah kombinasi unik host + protocol: 74905
    
    Daftar 10 user teratas berdasarkan jumlah request:
    



  <div id="df-48e34cc7-d57b-4033-9c93-ddc1432a8ab6" class="colab-df-container">
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
      <th>Remote host</th>
      <th>Request Protocol</th>
      <th>Jumlah Request</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10056</th>
      <td>134.34.67.219</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>44471</th>
      <td>66.249.122.225</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>54927</th>
      <td>77.49.91.139</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6733</th>
      <td>134.34.108.221</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>41659</th>
      <td>65.55.251.19</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>44058</th>
      <td>66.249.106.25</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>40735</th>
      <td>65.55.213.89</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>48995</th>
      <td>66.249.60.199</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>40085</th>
      <td>65.55.186.103</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>49791</th>
      <td>66.249.89.78</td>
      <td>HTTP/1.1</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-48e34cc7-d57b-4033-9c93-ddc1432a8ab6')"
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
        document.querySelector('#df-48e34cc7-d57b-4033-9c93-ddc1432a8ab6 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-48e34cc7-d57b-4033-9c93-ddc1432a8ab6');
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


    <div id="df-26a79712-4962-4f34-8860-13c6fe8fbd2d">
      <button class="colab-df-quickchart" onclick="quickchart('df-26a79712-4962-4f34-8860-13c6fe8fbd2d')"
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
            document.querySelector('#df-26a79712-4962-4f34-8860-13c6fe8fbd2d button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>


