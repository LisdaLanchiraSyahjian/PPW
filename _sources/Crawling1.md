# Web Crawling

Web crawling adalah proses otomatis untuk menjelajahi halaman-halaman di internet dengan bantuan program khusus yang disebut web crawler atau spider. Program ini bekerja seperti “robot penjelajah” yang mengunjungi situs web, membaca isi halamannya, lalu mengikuti tautan (link) yang ada untuk mengumpulkan informasi lebih banyak lagi.

Tujuan utama web crawling adalah mengumpulkan data dari berbagai halaman web agar bisa diolah lebih lanjut, misalnya untuk mesin pencari seperti Google yang menggunakan crawler untuk mengindeks miliaran halaman, atau untuk riset yang membutuhkan data dari website tertentu. Dalam praktiknya, hasil crawling bisa berupa teks, gambar, atau metadata dari sebuah halaman.


```python
pip install sprynger
```

    Collecting sprynger
      Downloading sprynger-0.4.1-py3-none-any.whl.metadata (5.8 kB)
    Requirement already satisfied: lxml in /usr/local/lib/python3.12/dist-packages (from sprynger) (5.4.0)
    Requirement already satisfied: requests in /usr/local/lib/python3.12/dist-packages (from sprynger) (2.32.4)
    Requirement already satisfied: urllib3 in /usr/local/lib/python3.12/dist-packages (from sprynger) (2.5.0)
    Requirement already satisfied: platformdirs in /usr/local/lib/python3.12/dist-packages (from sprynger) (4.3.8)
    Requirement already satisfied: charset_normalizer<4,>=2 in /usr/local/lib/python3.12/dist-packages (from requests->sprynger) (3.4.3)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.12/dist-packages (from requests->sprynger) (3.10)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.12/dist-packages (from requests->sprynger) (2025.8.3)
    Downloading sprynger-0.4.1-py3-none-any.whl (40 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m40.2/40.2 kB[0m [31m1.9 MB/s[0m eta [36m0:00:00[0m
    [?25hInstalling collected packages: sprynger
    Successfully installed sprynger-0.4.1
    


```python
import requests

api_key = "e2f401d8a59dde318fcebacc107ae993"
isbn = "978-3-031-63497-0"  # contoh ISBN valid

url = "https://api.springernature.com/meta/v2/json"
params = {
    "q": f"isbn:{isbn}",
    "api_key": api_key,
    "p": 10
}

response = requests.get(url, params=params)

if response.status_code == 200:
    data = response.json()
    print(f"Total hasil: {data['result'][0]['total']}\n")
    for record in data['records']:
        doi = record.get('doi', 'N/A')
        title = record.get('title', 'No title')
        abstract = record.get('abstract', 'No abstract')
        print(f"DOI: {doi}")
        print(f"Title: {title}")
        print(f"Abstract: {abstract}\n")
else:
    print("Error:", response.status_code, response.text)

```

    Total hasil: 27
    
    DOI: 10.1007/978-3-031-63498-7_20
    Title: Quantifier Shifting for Quantified Boolean Formulas Revisited
    Abstract: Modern solvers for quantified Boolean formulas (QBFs) process formulas in prenex form, which divides each QBF into two parts: the quantifier prefix and the propositional matrix. While this representation does not cover the full language of QBF, every non-prenex formula can be transformed to an equivalent formula in prenex form. This transformation offers several degrees of freedom and blurs structural information that might be useful for the solvers. In a case study conducted 20 years back, it has been shown that the applied transformation strategy heavily impacts solving time. We revisit this work and investigate how sensitive recent QBF solvers perform w.r.t. various prenexing strategies.
    
    DOI: 10.1007/978-3-031-63498-7_9
    Title: First-Order Automatic Literal Model Generation
    Abstract: Given a finite consistent set of ground literals, we present an algorithm that generates a complete first-order logic interpretation, i.e., an interpretation for all ground literals over the signature and not just those in the input set, that is also a model for the input set. The interpretation is represented by first-order linear literals. It can be effectively used to evaluate clauses. A particular application are SCL stuck states. The SCL (Simple Clause Learning) calculus always computes with respect to a finite number of ground literals. It then finds either a contradiction or a stuck state being a model with respect to the considered ground literals. Our algorithm builds a complete literal interpretation out of such a stuck state model that can then be used to evaluate the clause set. If all clauses are satisfied an overall model has been found. If it does not satisfy some clause, this information can be effectively explored to extend the scope of ground literals considered by SCL.
    
    DOI: 10.1007/978-3-031-63498-7_3
    Title: Stepping Stones in the TPTP World
    Abstract: The TPTP World is a well established infrastructure that supports research, development, and deployment of Automated Theorem Proving (ATP) systems. There are key components that help make the TPTP World a success: the TPTP problem library was first released in 1993, the CADE ATP System Competition (CASC) was conceived after CADE-12 in 1994, problem difficulty ratings were added in 1997, the current TPTP language was adopted in 2003, the SZS ontologies were specified in 2004, the TSTP solution library was built starting around 2005, the Specialist Problem Classes (SPCs) have been used to classify problems since 2010, the SystemOnTPTP service has been offered from 2011, the StarExec service was started in 2013, and a world of TPTP users have helped all along. This paper reviews these stepping stones in the development of the TPTP World.
    
    DOI: 10.1007/978-3-031-63498-7_21
    Title: Satisfiability Modulo Exponential Integer Arithmetic
    Abstract: SMT solvers use sophisticated techniques for polynomial (linear or non-linear) integer arithmetic. In contrast, non-polynomial integer arithmetic has mostly been neglected so far. However, in the context of program verification, polynomials are often insufficient to capture the behavior of the analyzed system without resorting to approximations. In the last years, incremental linearization has been applied successfully to satisfiability modulo real arithmetic with transcendental functions. We adapt this approach to an extension of polynomial integer arithmetic with exponential functions. Here, the key challenge is to compute suitable lemmas that eliminate the current model from the search space if it violates the semantics of exponentiation. An empirical evaluation of our implementation shows that our approach is highly effective in practice.
    
    DOI: 10.1007/978-3-031-63498-7_6
    Title: Tableaux for Automated Reasoning in Dependently-Typed Higher-Order Logic
    Abstract: Dependent type theory gives an expressive type system facilitating succinct formalizations of mathematical concepts. In practice, it is mainly used for interactive theorem proving with intensional type theories, with PVS being a notable exception. In this paper, we present native rules for automated reasoning in a dependently-typed version (DHOL) of classical higher-order logic (HOL). DHOL has an extensional type theory with an undecidable type checking problem which contains theorem proving. We implemented the inference rules as well as an automatic type checking mode in Lash, a fork of Satallax, the leading tableaux-based prover for HOL. Our method is sound and complete with respect to provability in DHOL. Completeness is guaranteed by the incorporation of a sound and complete translation from DHOL to HOL recently proposed by Rothgang et al. While this translation can already be used as a preprocessing step to any HOL prover, to achieve better performance, our system directly works in DHOL. Moreover, experimental results show that the DHOL version of Lash can outperform all major HOL provers executed on the translation.
    
    DOI: 10.1007/978-3-031-63498-7_22
    Title: SAT-Based Learning of Computation Tree Logic
    Abstract: The CTL learning problem consists in finding for a given sample of positive and negative Kripke structures a distinguishing CTL formula that is verified by the former but not by the latter. Further constraints may bound the size and shape of the desired formula or even ask for its minimality in terms of syntactic size. This synthesis problem is motivated by explanation generation for dissimilar models, e.g. comparing a faulty implementation with the original protocol. We devise a SAT -based encoding for a fixed size CTL formula, then provide an incremental approach that guarantees minimality. We further report on a prototype implementation whose contribution is twofold: first, it allows us to assess the efficiency of various output fragments and optimizations. Secondly, we can experimentally evaluate this tool by randomly mutating Kripke structures or syntactically introducing errors in higher-level models, then learning CTL distinguishing formulas.
    
    DOI: 10.1007/978-3-031-63498-7_10
    Title: Synthesis of Recursive Programs in Saturation
    Abstract: We turn saturation-based theorem proving into an automated framework for recursive program synthesis. We introduce magic axioms as valid induction axioms and use them together with answer literals in saturation. We introduce new inference rules for induction in saturation and use answer literals to synthesize recursive functions from these proof steps. Our proof-of-concept implementation in the Vampire theorem prover constructs recursive functions over algebraic data types, while proving inductive properties over these types.
    
    DOI: 10.1007/978-3-031-63498-7_23
    Title: MCSat-Based Finite Field Reasoning in the Yices2 SMT Solver (Short Paper)
    Abstract: This system description introduces an enhancement to the Yices2 SMT solver, enabling it to reason over non-linear polynomial systems over finite fields. Our reasoning approach fits into the model-constructing satisfiability (MCSat) framework and is based on zero decomposition techniques, which find finite basis explanations for theory conflicts over finite fields. As the MCSat solver within Yices2 can support (and combine) several theories via theory plugins, we implemented our reasoning approach as a new plugin for finite fields and extended Yices2 ’s frontend to parse finite field problems, making our implementation the first MCSat-based reasoning engine for finite fields. We present its evaluation on finite field benchmarks, comparing it against cvc5 . Additionally, our work leverages the modular architecture of the MCSat solver in Yices2 to provide a foundation for the rapid implementation of further reasoning techniques for this theory.
    
    DOI: 10.1007/978-3-031-63498-7_11
    Title: Synthesizing Strongly Equivalent Logic Programs: Beth Definability for Answer Set Programs via Craig Interpolation in First-Order Logic
    Abstract: We show a projective Beth definability theorem for logic programs under the stable model semantics: For given programs P and Q and vocabulary  V (set of predicates) the existence of a program  R in V such that $$P \cup R$$ P ∪ R and $$P \cup Q$$ P ∪ Q are strongly equivalent can be expressed as a first-order entailment. Moreover, our result is effective: A program  R can be constructed from a Craig interpolant for this entailment, using a known first-order encoding for testing strong equivalence, which we apply in reverse to extract programs from formulas. As a further perspective, this allows transforming logic programs via transforming their first-order encodings. In a prototypical implementation, the Craig interpolation is performed by first-order provers based on clausal tableaux or resolution calculi. Our work shows how definability and interpolation, which underlie modern logic-based approaches to advanced tasks in knowledge representation, transfer to answer set programming.
    
    DOI: 10.1007/978-3-031-63498-7_16
    Title: Model Completeness for Rational Trees
    Abstract: We analyze the theory of rational trees with finitely many constructors, infinitely many atoms and an atomicity predicate. We design a new decision procedure, proving in addition that this theory is model-complete. We also show that the enrichment of the language with selectors and simultaneous parametric fixpoints enjoys quantifier elimination.
    
    


```python
import requests
import pandas as pd

# Masukkan API key dari akun Springer Nature Developer
api_key = "e2f401d8a59dde318fcebacc107ae993"

# Daftar keyword yang mau dicari
keywords = ["web mining", "web crawling", "supervised learning", "unsupervised learning"]

# Endpoint API
url = "https://api.springernature.com/meta/v2/json"

# Simpan hasil di list
all_results = []

for kw in keywords:
    print(f"🔍 Crawling keyword: {kw}")
    params = {
        "q": kw,
        "api_key": api_key,
        "p": 20   # jumlah hasil per query (maks 50 per request)
    }

    response = requests.get(url, params=params)

    if response.status_code == 200:
        data = response.json()
        records = data.get("records", [])
        for record in records:
            all_results.append({
                "keyword": kw,
                "doi": record.get("doi", "N/A"),
                "title": record.get("title", "No title"),
                "publicationName": record.get("publicationName", "N/A"),
                "publicationDate": record.get("publicationDate", "N/A"),
                "abstract": record.get("abstract", "No abstract")
            })
    else:
        print(f"⚠️ Error {response.status_code} untuk keyword: {kw}")

# Simpan ke DataFrame
df = pd.DataFrame(all_results)

# Tampilkan data
print(df.head())

# Simpan ke CSV
df.to_csv("springer_results.csv", index=False, encoding="utf-8")
print("✅ Data berhasil disimpan ke springer_results.csv")

```

    🔍 Crawling keyword: web mining
    🔍 Crawling keyword: web crawling
    🔍 Crawling keyword: supervised learning
    🔍 Crawling keyword: unsupervised learning
          keyword                           doi  \
    0  web mining   10.1007/978-3-032-00983-8_5   
    1  web mining   10.1007/978-3-031-93802-3_7   
    2  web mining   10.1007/978-981-96-7238-7_2   
    3  web mining  10.1007/978-3-031-95296-8_15   
    4  web mining   10.1007/978-3-031-90470-7_6   
    
                                                   title  \
    0  Survey on Data Mining and Machine Learning Met...   
    1  Unveiling Power Laws in Graph Mining: Techniqu...   
    2  Architecture Mining Approach for Systems-of-Sy...   
    3  A Mathematical Model and Algorithm for Data An...   
    4  ‘Internet of Things’ and ‘Social Networking’: ...   
    
                                         publicationName publicationDate  \
    0  Renewable Energy, Green Computing, and Sustain...      2026-01-01   
    1                                       Graph Mining      2026-01-01   
    2  Service-Oriented Computing – ICSOC 2024 Workshops      2026-01-01   
    3  Internet of Things, Smart Spaces, and Next Gen...      2026-01-01   
    4  Collective Principles and the Formation of the...      2026-01-01   
    
                                                abstract  
    0  It is observed that the Mental illness by the ...  
    1  Power laws play a crucial role in understandin...  
    2  Context: Systems of Systems (SoS) constitute a...  
    3  Machine learning methods play an important rol...  
    4  Moving to the post-2000 period, or the post-fo...  
    ✅ Data berhasil disimpan ke springer_results.csv
    
