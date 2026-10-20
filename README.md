# assignment-semantic-search-Ahmad-Reza-Fadlan

1. Deskripsi Project

Project ini merupakan prototype aplikasi semantic search yang terintegrasi dengan Large Language Model (LLM) untuk membantu menjawab pertanyaan customer service e-commerce berdasarkan knowledge base FAQ.

Sistem menggunakan Gemini Embeddings API untuk mengubah FAQ menjadi representasi vektor, FAISS untuk mencari informasi yang relevan, dan Gemini LLM untuk menghasilkan jawaban berdasarkan hasil retrieval.

2. Tujuan Project

Mengimplementasikan semantic search untuk menemukan FAQ berdasarkan kemiripan makna.

Mengintegrasikan Gemini Embeddings API dengan FAISS sebagai vector database.

Menggunakan hasil retrieval sebagai context untuk menghasilkan jawaban LLM.

Mengevaluasi relevansi hasil pencarian dan kualitas jawaban AI.

3. Dataset dan Batasan

Dataset yang digunakan adalah ecommerce_faq_knowledge_base.csv yang disediakan oleh ReWork.

Dataset terdiri dari 59 entri FAQ dengan kolom faq_id, category, question, dan answer. Informasi mencakup berbagai kebutuhan customer service e-commerce, seperti pemesanan, pembayaran, dan pengiriman.

Batasan sistem:

Jawaban hanya berdasarkan informasi FAQ yang tersedia.

Setiap pasangan pertanyaan dan jawaban diperlakukan sebagai satu unit retrievable.

Sistem mengambil tiga FAQ terdekat menggunakan FAISS.

Jika informasi tidak tersedia, LLM diarahkan untuk menyarankan pengguna menghubungi customer service.

4. Teknologi yang Digunakan

Python 3.12 – Bahasa pemrograman

Pandas & NumPy – Pengolahan data dan embedding

Gemini Embeddings API – Menghasilkan embedding FAQ dan query

FAISS – Penyimpanan vektor dan semantic search

Gemini LLM API – Menghasilkan jawaban berdasarkan context

Jupyter Notebook – Implementasi dan pengujian

5. Alur Sistem

Ingest: Membaca 59 entri FAQ dari dataset menggunakan Pandas.

Embed: Menggabungkan question dan answer, kemudian menghasilkan embedding menggunakan gemini-embedding-2 dengan dimensi 3.072.

Store: Menyimpan 59 embedding ke FAISS menggunakan IndexFlatL2.

Retrieve: Mengubah query pengguna menjadi embedding dan mengambil tiga FAQ terdekat berdasarkan squared Euclidean distance.

Generate: Menyusun prompt dengan Persona, Task, Context, dan Format, kemudian mengirimkannya ke Gemini LLM untuk menghasilkan jawaban.

6. Hasil Pengujian

Pengujian dilakukan menggunakan lima query dengan tiga kategori, yaitu pertanyaan langsung, parafrase, dan pertanyaan di luar knowledge base.

Pertanyaan langsung:

Bagaimana cara melakukan pemesanan di platform ini?

Apakah saya bisa memesan tanpa membuat akun?

Kedua pertanyaan berhasil dijawab sesuai informasi FAQ yang tersedia.

Pertanyaan parafrase:

Paket saya kok belum nyampe padahal sudah lewat perkiraan?

Gimana caranya supaya pesanan yang sudah dibuat dibatalkan?

Sistem mampu memahami maksud pertanyaan meskipun menggunakan kalimat yang berbeda dari FAQ asli. Jawaban yang dihasilkan sesuai dengan kebijakan pengiriman dan pembatalan pesanan dalam knowledge base.

Pertanyaan di luar knowledge base:

Apakah perusahaan menyediakan layanan penyewaan mobil?

FAISS tetap mengembalikan tiga FAQ terdekat meskipun tidak berkaitan langsung dengan penyewaan mobil. Namun, LLM tidak mengarang informasi dan menyampaikan bahwa informasi tersebut belum tersedia.

Analisis Relevansi

Secara keseluruhan, hasil pengujian menunjukkan bahwa semantic search mampu mengambil informasi yang relevan, termasuk pada pertanyaan dengan parafrase. Pada contoh keterlambatan paket, FAQ yang paling sesuai berhasil ditemukan pada peringkat pertama.

Keterbatasan sistem terdapat pada pencarian pertanyaan di luar knowledge base. FAISS tetap mengembalikan FAQ terdekat meskipun tidak relevan karena belum diterapkan mekanisme penyaringan berdasarkan threshold. Oleh karena itu, instruksi prompt berperan penting dalam membatasi jawaban LLM agar tetap sesuai dengan context.

7. Struktur Project

semantic-search-assignment/
├── data/
│   ├── ecommerce_faq_knowledge_base.csv
│   └── faq_index.faiss
├── notebook/
│   └── starter_notebook_semantic_search.ipynb
├── documentation/
├── .gitignore
├── README.md
└── requirements.txt

File .env digunakan secara lokal untuk menyimpan API key dan tidak disertakan dalam repository.

8. Cara Menjalankan

1. Clone repository

git clone https://github.com/ahmdrezaa/assignment-semantic-search-Ahmad-Reza-Fadlan.git
cd assignment-semantic-search-Ahmad-Reza-Fadlan

2. Siapkan environment Python

conda create -n semantic-search python=3.12 pip
conda activate semantic-search

3. Install dependencies

python -m pip install -r requirements.txt

4. Siapkan Gemini API key

Buat API key melalui Google AI Studio, lalu atur environment variable GEMINI_API_KEY sebelum menjalankan notebook.

5. Jalankan Jupyter Notebook

cd notebook
jupyter notebook starter_notebook_semantic_search.ipynb

Pilih kernel Python dari environment semantic-search, kemudian jalankan setiap cell secara berurutan.

Proses pembuatan embedding dan pemanggilan LLM memerlukan koneksi internet serta akses Gemini API.

9. Kesimpulan

Prototype berhasil mengimplementasikan alur semantic search mulai dari pemrosesan knowledge base, pembuatan embedding, penyimpanan FAISS, hingga generasi jawaban menggunakan LLM.

Berdasarkan lima query pengujian, sistem dapat menjawab pertanyaan langsung dan parafrase dengan baik serta menunjukkan respons yang sesuai ketika informasi tidak tersedia dalam knowledge base. Namun, diperlukan mekanisme filtering tambahan agar hasil retrieval yang tidak relevan dapat ditangani dengan lebih baik.