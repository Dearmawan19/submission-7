# Laporan Proyek Machine Learning - Dearmawan

## Project Overview

Proyek ini bertujuan untuk membangun sistem rekomendasi buku. Di era digital dengan jumlah informasi dan pilihan yang melimpah, pengguna seringkali kesulitan menemukan buku yang sesuai dengan minat dan preferensi mereka. Sistem rekomendasi hadir sebagai solusi untuk membantu pengguna menemukan buku-buku baru atau yang relevan secara efisien. Dengan menganalisis data historis interaksi pengguna dengan buku (seperti rating) dan informasi konten buku (seperti judul, penulis, dan penerbit), sistem ini dapat memberikan saran yang dipersonalisasi (Ricci, Rokach, & Shapira, 2011).

Pentingnya Proyek: Masalah penemuan buku yang relevan ini penting untuk diselesaikan karena dapat secara signifikan meningkatkan pengalaman pengguna dalam menjelajahi koleksi buku yang besar. Selain itu, sistem rekomendasi dapat membantu penulis dan penerbit menjangkau audiens yang lebih tepat sasaran, yang berpotensi meningkatkan penjualan dan keterlibatan pembaca pada platform buku digital maupun fisik. Dengan kata lain, sistem ini menciptakan nilai bagi semua pihak dalam ekosistem perbukuan(Aggarwal, 2016).

Riset Terkait: Riset terkait sistem rekomendasi telah banyak dilakukan dan menunjukkan perkembangan yang pesat. Berbagai teknik telah dieksplorasi, mencakup content-based filtering yang menganalisis atribut item, collaborative filtering yang memanfaatkan perilaku kolektif pengguna, dan pendekatan hibrida yang menggabungkan keduanya (Ricci, Rokach, & Shapira, 2011). 

# Referensi:
* Ricci, F., Rokach, L., & Shapira, B. (2011). Introduction to Recommender Systems Handbook. In Recommender Systems Handbook (pp. 1–35). Springer, Boston, MA.
* Aggarwal, C. C. (2016). Recommender Systems: The Textbook. Springer.

## Business Understanding

Proses klarifikasi masalah dalam proyek ini melibatkan pemahaman terhadap kebutuhan inti pengguna, yaitu menemukan buku yang menarik dan relevan bagi mereka di tengah banyaknya pilihan. Tantangan utamanya adalah bagaimana menyediakan rekomendasi yang tidak hanya akurat tetapi juga beragam, dari kumpulan data buku yang sangat besar dan terus bertambah.

### Problem Statements

* Kesulitan Penemuan Buku:Bagaimana cara membantu pengguna menemukan buku yang sesuai dengan preferensi bacaan mereka di antara jutaan judul yang tersedia?
* Personalisasi Rekomendasi Berdasarkan Perilaku Serupa: Bagaimana cara memberikan rekomendasi buku yang dipersonalisasi kepada seorang pengguna, dengan memanfaatkan riwayat bacaan dan rating dari pengguna lain yang memiliki selera atau preferensi serupa?

### Goals

* Rekomendasi Berdasarkan Buku Pilihan (Item-Based/Content-Based): Mengembangkan sistem yang mampu memberikan daftar rekomendasi buku kepada pengguna berdasarkan satu atau lebih buku yang mereka sukai atau pilih sebagai referensi.
* Rekomendasi Berdasarkan Preferensi Pengguna Lain (Collaborative Filtering): Mengembangkan sistem yang mampu memberikan daftar rekomendasi buku kepada pengguna berdasarkan analisis kemiripan preferensi mereka dengan pengguna lain, menggunakan teknik model-based collaborative filtering.

### Solution Approach

Untuk mencapai tujuan-tujuan di atas, dua pendekatan utama sistem rekomendasi akan diimplementasikan dan dievaluasi:

### Solution Statements

1.  **Content-Based Filtering**:
    * Cara Kerja: Pendekatan ini akan merekomendasikan buku berdasarkan kemiripan atribut atau konten buku. Fitur-fitur seperti judul buku, nama penulis, dan nama penerbit akan diekstraksi dan digunakan untuk membuat profil atau representasi dari setiap buku.
    * Teknik: Teknik TF-IDF (Term Frequency-Inverse Document Frequency) akan digunakan untuk mengubah data tekstual (judul, penulis, penerbit) menjadi vektor fitur numerik. Kemiripan antar buku kemudian akan dihitung menggunakan metrik seperti cosine similarity atau linear kernel. Buku-buku dengan skor kemiripan tertinggi terhadap buku referensi akan direkomendasikan.
2.  **Collaborative Filtering (Model-Based - SVD)**:
    * Cara Kerja: Pendekatan ini akan merekomendasikan buku berdasarkan pola rating atau interaksi dari banyak pengguna. Ide dasarnya adalah jika pengguna A memiliki pola rating yang mirip dengan pengguna B, maka buku yang disukai pengguna A (yang belum dilihat pengguna B) kemungkinan besar juga akan disukai oleh pengguna B.
    * Teknik: Algoritma Singular Value Decomposition (SVD), yang merupakan teknik faktorisasi matriks, akan digunakan. SVD akan mencoba mempelajari faktor-faktor laten (preferensi tersembunyi) dari data rating pengguna-item yang ada. Model ini kemudian dapat digunakan untuk memprediksi rating pengguna terhadap buku yang belum pernah ia rating sebelumnya, dan buku dengan prediksi rating tertinggi akan direkomendasikan.

## Data Understanding

Proyek ini menggunakan dataset publik yang terdiri dari tiga file CSV terpisah: Books.csv, Users.csv, dan Ratings.csv.

* Books.csv berisi informasi detail mengenai buku, dengan 271.360 entri dan 8 kolom.
* Users.csv berisi informasi mengenai pengguna, dengan 278.858 entri dan 3 kolom.
* Ratings.csv berisi informasi rating yang diberikan pengguna terhadap buku, dengan 1.149.780 entri dan 3 kolom.

Kondisi data secara umum tergolong baik. Meskipun terdapat beberapa nilai hilang dan data yang memerlukan pembersihan, khususnya pada kolom Year-Of-Publication &  Publisher di Books.csv dan kolom Age di Users.csv, keseluruhan struktur dataset cukup konsisten. Sementara itu, Ratings.csv tidak mengandung nilai yang hilang, sehingga tidak memerlukan proses pembersihan pada bagian tersebut.

Selain itu, ketiga dataset — Books.csv, Users.csv, dan Ratings.csv — telah diperiksa dan tidak mengandung baris duplikat, sehingga tidak diperlukan proses penghapusan duplikasi. Dataset ini tersedia di berbagai repositori publik, salah satunya adalah di Kaggle dengan nama dataset populer “Book-Crossings” yang sering digunakan dalam proyek sistem rekomendasi buku.


Tautan Sumber Data: https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset/data

Variabel-variabel pada dataset adalah sebagai berikut:

1.  **Books.csv**
    * ISBN: International Standard Book Number, kode unik global untuk setiap edisi buku (tipe: object).
    * Book-Title: Judul lengkap buku (tipe: object).
    * Book-Author: Nama penulis atau para penulis buku (tipe: object). Terdapat 2 nilai hilang.
    * Year-Of-Publication: Tahun buku tersebut dipublikasikan (tipe: object, perlu dikonversi ke numerik dan dibersihkan).
    * Publisher: Nama perusahaan atau entitas yang menerbitkan buku (tipe: object). Terdapat 2 nilai hilang.
    * Image-URL-S: URL gambar sampul buku dalam ukuran kecil (tipe: object).
    * Image-URL-M: URL gambar sampul buku dalam ukuran sedang (tipe: object).
    * Image-URL-L: URL gambar sampul buku dalam ukuran besar (tipe: object). Terdapat 3 nilai hilang.
    * Observasi: Dataset buku cukup lengkap untuk kolom-kolom inti yang akan digunakan (ISBN, Judul, Penulis, Penerbit, Tahun). Kolom Year-Of-Publication memiliki beberapa entri yang tidak valid (misalnya, nama penerbit atau judul) yang perlu ditangani. Kolom URL gambar tidak akan digunakan dalam model ini.
2.  **Users.csv**
    * User-ID: Nomor identifikasi unik untuk setiap pengguna (tipe: int64).
    * Location: Lokasi geografis pengguna, biasanya dalam format kota, provinsi/negara bagian, negara (tipe: object).
    * Age: Usia pengguna dalam tahun (tipe: float64). Terdapat 110.762 nilai hilang (sekitar 39.7%).
    * Observasi: Kolom Age memiliki persentase nilai hilang yang signifikan. Selain itu, terdapat nilai usia yang tidak realistis (misalnya 0 atau usia sangat tua seperti 244) yang memerlukan penanganan outlier.
3.  **Ratings.csv**
    * User-ID: ID unik pengguna yang memberikan rating, merujuk ke User-ID di Users.csv (tipe: int64).
    * ISBN: ISBN buku yang dirating, merujuk ke ISBN di Books.csv (tipe: object).
    * Book-Rating: Rating yang diberikan oleh pengguna untuk buku tersebut. Skala rating adalah 0 hingga 10. Rating 0 umumnya menandakan rating implisit (misalnya, buku ada di rak pengguna) atau tidak ada rating eksplisit yang diberikan. (tipe: int64).
    * Observasi: Dataset rating sangat besar dan tidak memiliki nilai hilang pada kolom-kolomnya. Mayoritas (lebih dari 700.000 dari 1.1 juta) rating adalah 0. Ini penting untuk dipertimbangkan karena model collaborative filtering yang akan digunakan (SVD) biasanya bekerja lebih baik dengan rating eksplisit (1-10).

### Visualisasi Data dan Exploratory Data Analysis (EDA):

* Distribusi Rating Buku:
* [Bar chart menunjukkan frekuensi setiap nilai rating (0-10). Rating 0 memiliki frekuensi tertinggi, jauh melebihi rating lainnya. Rating 8, 7, dan 10 adalah rating eksplisit yang paling sering muncul berikutnya.]
  
![image](https://github.com/user-attachments/assets/7d20b9de-cf44-4c12-a2b7-7546a3421fbb)


Insight: Sebagian besar interaksi dalam dataset adalah implisit (rating 0). Untuk analisis yang melibatkan preferensi eksplisit, fokus pada rating 1-10 akan lebih relevan. Rating tinggi (7-10) menunjukkan sentimen positif yang lebih kuat dari pengguna.

* Distribusi Usia Pengguna:
* [Histogram menunjukkan distribusi frekuensi usia pengguna setelah penanganan outlier awal. Puncak distribusi berada di sekitar usia 20-40 tahun, dengan penurunan frekuensi untuk usia yang lebih tua.]

![image](https://github.com/user-attachments/assets/d340d6ba-3317-4722-adaf-3b7a565b8695)


Insight: Mayoritas pengguna aktif dalam dataset ini berada dalam rentang usia dewasa muda hingga paruh baya. Informasi ini bisa berguna untuk segmentasi atau personalisasi lebih lanjut, meskipun dalam proyek ini tidak digunakan secara langsung dalam pemodelan utama.

* Top 10 Buku dengan Jumlah Rating Terbanyak:
* [Bar chart horizontal menampilkan 10 judul buku yang paling banyak menerima rating, dengan "Wild Animus" di posisi teratas dengan jumlah rating signifikan lebih banyak dari buku lainnya.]

![image](https://github.com/user-attachments/assets/631619ae-85ec-46e5-8112-7cf40f407c61)

Insight: Buku "Wild Animus" sangat dominan dalam hal jumlah rating yang diterima, menunjukkan popularitas atau mungkin bias dalam dataset. Buku-buku populer lainnya seperti "The Lovely Bones: A Novel" dan "The Da Vinci Code" juga muncul. Ini bisa menjadi pertimbangan dalam evaluasi, karena buku populer cenderung lebih mudah direkomendasikan.

## Data Preparation

Proses persiapan data dilakukan untuk membersihkan, mentransformasi, dan menyusun data agar optimal untuk digunakan dalam tahap pemodelan. Teknik yang digunakan pada notebook dan laporan ini sudah berurutan.

1.  **Persiapan books_df**:  
    * **Koreksi Year-Of-Publication:**
        Proses: Kolom Year-Of-Publication awalnya bertipe object dan mengandung beberapa nilai non-numerik. Dilakukan konversi ke tipe numerik menggunakan `pd.to_numeric` dengan parameter `errors='coerce'`, yang akan mengubah nilai yang tidak bisa dikonversi menjadi NaN.
    
      Nilai NaN yang dihasilkan (termasuk yang mungkin sudah ada sebelumnya atau akibat konversi) kemudian diisi dengan nilai median dari tahun publikasi yang valid. Akhirnya, tipe data kolom ini diubah menjadi int.
       * *Alasan:* Memastikan konsistensi tipe data tahun menjadi numerik sangat penting untuk analisis temporal atau penggunaan sebagai fitur. Mengisi nilai yang salah format atau hilang dengan median adalah strategi imputasi yang umum untuk menjaga integritas data tanpa memperkenalkan bias ekstrem.

     * **Mengisi Nilai Hilang pada Book-Author dan Publisher:**
        Proses: Nilai NaN pada kolom Book-Author diisi dengan string 'Unknown Author', dan pada kolom Publisher diisi dengan string 'Unknown Publisher'.
        * *Alasan:* Untuk model content-based filtering, informasi penulis dan penerbit akan digunakan sebagai bagian dari konten. Mengisi nilai yang hilang dengan placeholder generik memastikan semua buku memiliki informasi ini, sehingga tidak ada data yang terbuang saat pembuatan fitur konten dan menghindari error pada algoritma yang tidak bisa menangani nilai hilang.
    
    * **Menghapus Kolom URL Gambar:**
        Proses: Kolom Image-URL-S, Image-URL-M, dan Image-URL-L dihapus dari dataframe.
         *  *Alasan:* Kolom-kolom ini berisi URL gambar sampul buku dan tidak akan digunakan sebagai fitur dalam model rekomendasi berbasis teks yang dikembangkan dalam proyek ini. Menghapusnya akan mengurangi dimensi data dan menyederhanakan dataset.
    * **Pembuatan Kolom `content` untuk Content-Based Filtering:**
        Proses: Sebuah kolom baru bernama `content` dibuat dengan menggabungkan informasi dari kolom Book-Title, Book-Author, dan Publisher menjadi satu string teks tunggal untuk setiap buku. Ini dilakukan pada sampel data `books_cb_sample` yang digunakan untuk content-based filtering. Nilai NaN pada kolom-kolom tersebut (jika masih ada setelah penanganan sebelumnya atau pada sampel baru) diisi dengan string kosong sebelum proses penggabungan.
        * *Alasan:* Kolom `content` ini akan menjadi input utama untuk TF-IDF Vectorizer dalam model content-based filtering. Menggabungkan beberapa fitur tekstual relevan ke dalam satu representasi diharapkan dapat menangkap esensi konten buku dengan lebih baik.
2.  **Persiapan users_df**:
    * **Menangani Outlier dan Nilai Hilang pada Age:**
        Proses: Usia pengguna yang tercatat di atas 90 tahun atau di bawah 5 tahun dianggap sebagai outlier atau data yang tidak akurat, sehingga nilai-nilai ini diubah menjadi NaN. Setelah itu, semua nilai NaN pada kolom Age (baik yang berasal dari data asli maupun yang baru diubah karena outlier) diisi dengan nilai median dari usia pengguna yang valid. Terakhir, tipe data kolom Age diubah menjadi int.
        * *Alasan:* Membersihkan data usia dari nilai-nilai ekstrem yang kemungkinan besar salah input sangat penting untuk analisis demografis yang akurat atau jika usia akan digunakan sebagai fitur. Imputasi dengan median membantu mempertahankan distribusi data usia tanpa membuang banyak baris data karena nilai hilang.
3.  **Persiapan ratings_df (untuk Collaborative Filtering)**:
    * **Filter Rating Eksplisit:**
        Proses: Baris data dari `ratings_df` di mana Book-Rating adalah 0 (menandakan rating implisit atau tidak ada rating) dihilangkan. Hasilnya disimpan dalam dataframe baru `ratings_explicit_df`.
        * *Alasan:* Model collaborative filtering berbasis SVD yang akan digunakan umumnya dirancang untuk bekerja dengan preferensi eksplisit yang dinyatakan oleh pengguna (misalnya, rating 1-10). Rating 0 tidak memberikan informasi preferensi yang jelas dan dapat mengganggu proses pembelajaran model.
    * **Penggabungan Data:**
        Proses: Dataframe `ratings_explicit_df` digabungkan (merged) dengan `books_df` (untuk mendapatkan Book-Title berdasarkan ISBN) dan dengan `users_df` (untuk mendapatkan Age berdasarkan User-ID). Hasil penggabungan ini disimpan sebagai `full_data_explicit`.
        * *Alasan:* Menggabungkan data ini memudahkan analisis eksploratif lebih lanjut dan memastikan semua informasi yang relevan (pengguna, item, rating, serta beberapa metadata item dan pengguna) tersedia dalam satu struktur data terpadu sebelum pemfilteran lebih lanjut.
    * **Filtering untuk Mengurangi Sparsity:**
        Proses: Dilakukan dua tahap pemfilteran pada `full_data_explicit` untuk mengurangi masalah sparsity data:
        1.  Hanya pengguna yang telah memberikan minimal 5 rating eksplisit yang dipertahankan.
        2.  Dari hasil langkah pertama, hanya buku yang telah menerima minimal 5 rating eksplisit yang dipertahankan.
        Data yang telah difilter ini disimpan sebagai `filtered_ratings_cf`.
        Alasan: Matriks interaksi pengguna-item dalam sistem rekomendasi seringkali sangat sparse (banyak entri kosong karena pengguna hanya merating sebagian kecil item). Sparsity yang tinggi dapat menurunkan kualitas dan stabilitas model collaborative filtering. Dengan memfilter pengguna dan item yang memiliki sedikit interaksi, kita fokus pada bagian data yang lebih padat, yang diharapkan dapat membantu model mempelajari pola preferensi dengan lebih baik.

Setelah semua tahapan persiapan data ini, `books_df` dan `users_df` menjadi bersih dari nilai hilang pada kolom-kolom yang krusial dan siap untuk analisis lebih lanjut. `filtered_ratings_cf` menjadi dataset inti untuk melatih model collaborative filtering, dan sampel `books_cb_sample` (dengan kolom `content` yang telah dibuat) siap untuk digunakan dalam model content-based filtering.

## Modeling and Result
Dua pendekatan sistem rekomendasi diimplementasikan: Content-Based Filtering dan Collaborative Filtering menggunakan SVD.

1.  **Content-Based Filtering**
    Pendekatan ini merekomendasikan buku berdasarkan kemiripan kontennya (judul, penulis, penerbit).
    * Pengambilan Sampel: Untuk efisiensi komputasi, model ini dilatih pada sampel data yang terdiri dari 10% dari keseluruhan `books_df`, yang disebut `books_cb_sample`.
    * Pembuatan Fitur Konten: Sebuah kolom `content` dibuat dengan menggabungkan teks dari Book-Title, Book-Author, dan Publisher untuk setiap buku dalam sampel.
    * TF-IDF Vectorization: `TfidfVectorizer` dari sklearn digunakan untuk mengubah teks pada kolom `content` menjadi matriks representasi numerik (TF-IDF). Proses ini melibatkan:
        * Penghilangan stop words bahasa Inggris (kata-kata umum seperti "the", "is", "in").
        * Penggunaan n-gram range (1,2) untuk menangkap baik kata tunggal maupun frasa dua kata sebagai fitur, yang dapat menangkap konteks lebih baik daripada kata tunggal saja.

        ```python
        # Inisialisasi TF-IDF Vectorizer
        tfidf = TfidfVectorizer(stop_words='english', ngram_range=(1,2))
        # Membuat matriks TF-IDF dari SAMPEL
        tfidf_matrix_sample = tfidf.fit_transform(books_cb_sample['content'])
        ```

    * Perhitungan Kemiripan: `linear_kernel` dari sklearn.metrics.pairwise digunakan untuk menghitung skor kemiripan antar semua pasangan buku dalam sampel. `linear_kernel` menghasilkan hasil yang sama dengan cosine similarity ketika inputnya adalah vektor TF-IDF (karena TF-IDF biasanya sudah ternormalisasi atau normalisasi L2 adalah bagian dari prosesnya).

        ```python
        # Menghitung KEMIRIPAN pada SAMPEL menggunakan linear_kernel
        cosine_sim_cb_sample = linear_kernel(tfidf_matrix_sample, tfidf_matrix_sample)
        ```

    * Fungsi Rekomendasi: Fungsi `get_content_based_recommendations_sample(isbn, N=10)` dibuat untuk menghasilkan top-N rekomendasi. Fungsi ini mengambil ISBN buku sebagai input, mencari buku tersebut dalam sampel, mendapatkan skor kemiripannya dengan semua buku lain, mengurutkannya, dan mengembalikan N buku teratas yang paling mirip beserta skor kesamaannya.
    * Hasil (Top-N Recommendation): Contoh output untuk buku 'The F Word' (ISBN 0679764275) dari sampel, N=5:
      
|       | ISBN       | Book-Title                                               | Book-Author           | similarity_score |
| :---- | :--------- | :------------------------------------------------------- | :-------------------- | :--------------- |
| 19535 | 0375407413 | Random House Webster's College Dictionary                | Random House          | 0.380672         |
| 22905 | 0679780114 | Random House Webster's American Sign Language Dictionary | Elaine, Ph.D. Costello | 0.290939         |
| 8505  | 0679408797 | Conglomeros                                              | Jesse Browner         | 0.172857         |
| 19407 | 0394487028 | Hers                                                     | A Alvarez             | 0.128790         |
| 15419 | 0394840909 | I Can Count to 100 ... Can You? (Random House Pictureback) | Katherine Howard      | 0.115693         |

* Kelebihan Pendekatan Content-Based:
     * Dapat merekomendasikan item baru yang belum memiliki interaksi pengguna (mengatasi sebagian masalah cold start untuk item).
     * Rekomendasi bersifat transparan dan dapat dijelaskan berdasarkan fitur item (misalnya, "buku ini direkomendasikan karena memiliki penulis yang sama").
     * Tidak bergantung pada data pengguna lain.
* Kekurangan Pendekatan Content-Based:
    * Kualitas rekomendasi sangat bergantung pada kualitas fitur yang diekstraksi. Jika fitur tidak representatif, rekomendasi akan buruk.
    * Cenderung menghasilkan rekomendasi yang serendipity-nya rendah (kurang mengejutkan) karena hanya merekomendasikan item yang sangat mirip dengan yang sudah disukai pengguna.
    * Bisa terjadi over-specialization, di mana pengguna terjebak dalam gelembung filter dan tidak terekspos pada item yang beragam.

2.  **Collaborative Filtering (SVD)**
    Pendekatan ini merekomendasikan buku berdasarkan pola rating dari komunitas pengguna.
    * Persiapan Data untuk Library Surprise: Data rating eksplisit yang telah difilter (`filtered_ratings_cf`) digunakan. `Reader` dari library surprise diinisialisasi untuk skala rating 1 hingga 10. Data kemudian di-load ke dalam format dataset surprise menggunakan `Dataset.load_from_df`.
    * Pembagian Data: Dataset dibagi menjadi set pelatihan (80%) dan set pengujian (20%) menggunakan fungsi `train_test_split` dari `surprise.model_selection`.
    * Model SVD: Algoritma SVD (Singular Value Decomposition) dari library surprise dipilih sebagai model. SVD adalah teknik faktorisasi matriks yang populer untuk collaborative filtering. Parameter yang digunakan dalam notebook adalah: `n_factors=50` (jumlah faktor laten), `n_epochs=20` (jumlah iterasi pelatihan), `lr_all=0.005` (learning rate), dan `reg_all=0.02` (faktor regularisasi).

        ```python
        # Menggunakan SVD dengan parameter yang ditentukan
        algo_svd = SVD(n_factors=50, n_epochs=20, lr_all=0.005, reg_all=0.02, random_state=42)
        algo_svd.fit(trainset_cf) # Melatih model pada set pelatihan
        ```

    * Evaluasi: Setelah pelatihan, model dievaluasi pada set pengujian untuk mengukur seberapa baik ia memprediksi rating yang sebenarnya. Metrik yang digunakan adalah RMSE dan MAE.
    * Fungsi Rekomendasi: Fungsi `get_collaborative_filtering_recommendations(user_id, N=10)` dibuat untuk menghasilkan top-N rekomendasi bagi pengguna tertentu. Fungsi ini:
        * Mengidentifikasi semua buku dalam `filtered_ratings_cf` yang belum pernah dirating oleh `user_id` target.
        * Menggunakan model SVD yang telah dilatih (`algo_svd.test()`) untuk memprediksi rating pengguna target terhadap buku-buku yang belum dirating tersebut.
        * Mengurutkan buku-buku tersebut berdasarkan prediksi rating (estimasi) secara menurun.
        * Mengembalikan N buku teratas beserta detailnya (ISBN, Judul, Penulis) dan prediksi ratingnya.
    * Hasil (Top-N Recommendation): Contoh output untuk User-ID 276747, N=5:

|     | ISBN       | Book-Title                                                | Book-Author      | estimated_rating |
| :-- | :--------- | :-------------------------------------------------------- | :--------------- | :--------------- |
| 0   | 0743454529 | My Sister's Keeper : A Novel (Picoult, Jodi)              | Jodi Picoult     | 9.540725         |
| 1   | 0812550706 | Ender's Game (Ender Wiggins Saga (Paperback))             | Orson Scott Card | 9.432904         |
| 2   | 0439136369 | Harry Potter and the Prisoner of Azkaban (Book 3)         | J. K. Rowling    | 9.430569         |
| 3   | 0836213319 | Dilbert: A Book of Postcards                              | Scott Adams      | 9.373584         |
| 4   | 0439425220 | Harry Potter and the Chamber of Secrets Postcard Book     | J. K. Rowling    | 9.369498         |

* Kelebihan Pendekatan Collaborative Filtering (SVD):
    * Mampu menemukan pola preferensi yang kompleks dan tersembunyi dari data interaksi pengguna-item.
    * Dapat menghasilkan rekomendasi yang serendipitous (mengejutkan namun relevan) karena tidak bergantung pada fitur item secara eksplisit.
    * Tidak memerlukan pengetahuan domain tentang item yang direkomendasikan.
* Kekurangan Pendekatan Collaborative Filtering (SVD):
    * Mengalami masalah cold start: sulit memberikan rekomendasi untuk pengguna baru (yang belum memiliki riwayat rating) atau item baru (yang belum pernah dirating).
    * Kinerja sangat dipengaruhi oleh sparsity data. Jika data rating sangat jarang, model akan kesulitan belajar pola yang signifikan.
    * Kurang transparan; sulit menjelaskan mengapa suatu item direkomendasikan selain karena "pengguna serupa menyukainya".

## Evaluation

1.  **Content-Based Filtering**
    Evaluasi untuk model content-based filtering yang diimplementasikan dalam notebook ini dilakukan secara kualitatif melalui inspeksi visual terhadap hasil rekomendasi.
    * Metrik yang Digunakan: Tidak ada metrik kuantitatif formal seperti presisi atau recall yang dihitung dalam notebook ini, karena hal tersebut memerlukan ground truth (data label mengenai kemiripan buku yang sebenarnya) yang tidak tersedia. Evaluasi dilakukan dengan cara:
        * Memilih satu buku input secara acak dari sampel data (`books_cb_sample`).
        * Menghasilkan daftar top-N (misalnya, 10) buku yang direkomendasikan untuk buku input tersebut.
        * Secara manual memeriksa apakah buku-buku yang direkomendasikan tersebut terlihat mirip atau relevan dengan buku input berdasarkan judul, penulis, atau tema yang mungkin tersirat.
    * Contoh Hasil Evaluasi (untuk buku 'Enterprise One to One: Tools for Competing in the Interactive Age' dengan ISBN 038548755X dari sampel):

      
|       | ISBN       | Book-Title                                                                                              | Book-Author         | similarity_score |
| :---- | :--------- | :------------------------------------------------------------------------------------------------------ | :------------------ | :--------------- |
| 8969  | 0385494092 | One to One B2B: Customer Development Strategies for the Business-To-Business World (One to One)         | Don Peppers         | 0.262420         |
| 11157 | 038549369X | The One to One Fieldbook: The Complete Toolkit for Implementing a 1To1 Marketing Program (One to One) | Don Peppers         | 0.258237         |
| 26088 | 0679790756 | Windows 3.1 Power Tools/Book and Disk (Bantam Power Tools Series)                                       | Geoffrey T. Leblond | 0.086537         |
| 16170 | 0385420579 | The Republic of Tea                                                                                     | Mel Ziegler         | 0.080384         |
| 8037  | 0440425050 | Five Little Peppers and How They Grew                                                                   | Margaret Sidney     | 0.077253         |
| 10711 | 0609608002 | Start with NO...The Negotiating Tools that the Pros Don't Want You to Know                                | Jim Camp            | 0.072519         |
| 5192  | 0448058081 | Five Little Peppers and How They Grew                                                                   | Margaret Sidney     | 0.068112         |
| 4152  | 059042520x | Five Little Peppers and How They Grew (Apple Classics)                                                  | Margaret Sidney     | 0.067535         |
| 22816 | 0385421540 | Managing by Storying Around: A New Method of Leadership                                                 | David Armstrong     | 0.064898         |
| 25547 | 0201144689 | Fundamentals of Interactive Computer Graphics (Systems Programming Series)                                | James D. Foley      | 0.063721         |


* Interpretasi Hasil:
     Berdasarkan contoh output, buku-buku yang direkomendasikan seperti "One to One B2B" dan "The One to One Fieldbook" (keduanya oleh Don Peppers, sama seperti buku input jika penulisnya adalah Don Peppers atau Martha Rogers) tampak relevan secara tematis dengan buku input yang berfokus pada "One to One" marketing. Ini menunjukkan bahwa model mampu menangkap kemiripan berdasarkan penulis dan tema judul. Namun, skor kesamaan yang dihasilkan relatif rendah (mayoritas di bawah 0.1, dengan yang tertinggi sekitar 0.26). Hal ini mengindikasikan bahwa meskipun ada relevansi, kemiripan konten berdasarkan fitur yang digunakan (judul, penulis, penerbit) mungkin tidak terlalu kuat untuk semua rekomendasi, atau representasi TF-IDF dengan n-gram (1,2) belum sepenuhnya menangkap nuansa kemiripan yang lebih dalam. Evaluasi lebih lanjut bisa melibatkan pengumpulan feedback pengguna atau penggunaan metrik seperti precision@N dan recall@N jika ada data ground truth yang sesuai. Metrik ini sesuai dengan konteks data (teks), problem statement (menemukan buku mirip), dan solusi (content-based).

2.  **Collaborative Filtering (SVD)**
    Evaluasi untuk model collaborative filtering berbasis SVD menggunakan metrik kuantitatif standar yang mengukur akurasi prediksi rating.
    * Metrik yang Digunakan:
        * Root Mean Squared Error (RMSE):
            Formula: $RMSE=\sqrt{\frac{1}{N}\sum_{(u,i)\in TestSet}(r_{ui}-\hat{r}_{ui})^2}$
            Cara Kerja: RMSE mengukur akar dari rata-rata kuadrat selisih antara rating aktual ($r_{ui}$) dan rating prediksi ($\hat{r}_{ui}$) untuk semua pasangan pengguna-item (N) dalam set pengujian. Karena selisihnya dikuadratkan, RMSE memberikan bobot yang lebih besar pada kesalahan prediksi yang besar. Nilai yang lebih rendah menunjukkan akurasi prediksi yang lebih baik.
        * Mean Absolute Error (MAE):
            Formula: $MAE=\frac{1}{N}\sum_{(u,i)\in TestSet}|r_{ui}-\hat{r}_{ui}|$
            Cara Kerja: MAE mengukur rata-rata dari selisih absolut antara rating aktual dan rating prediksi. MAE memberikan gambaran langsung mengenai seberapa besar rata-rata kesalahan prediksi tanpa memberikan bobot lebih pada kesalahan besar. Nilai yang lebih rendah juga menunjukkan akurasi prediksi yang lebih baik.
        Di mana N adalah jumlah total rating dalam set pengujian, $r_{ui}$ adalah rating aktual yang diberikan oleh pengguna u untuk item i, dan $\hat{r}_{ui}$ adalah rating yang diprediksi oleh model untuk pengguna u pada item i.
    * Hasil Metrik (dari notebook):
        SVD Model - RMSE: 1.5741
        SVD Model - MAE: 1.2101
    * Interpretasi Hasil:
        Nilai RMSE sebesar 1.5741 menunjukkan bahwa, secara rata-rata, prediksi rating yang dihasilkan oleh model SVD memiliki simpangan sekitar 1.5741 poin dari rating sebenarnya yang diberikan oleh pengguna, pada skala rating 1-10.
        Nilai MAE sebesar 1.2101 menunjukkan bahwa, secara rata-rata, selisih absolut antara rating prediksi dan rating aktual adalah sekitar 1.2101 poin.
        Kedua metrik ini sesuai dengan konteks data (rating numerik), problem statement (memprediksi preferensi pengguna), dan solusi (collaborative filtering SVD). Mengingat skala rating adalah 1-10, nilai kesalahan rata-rata sekitar 1.2 hingga 1.6 poin rating menunjukkan bahwa model memiliki kemampuan prediksi yang cukup baik, namun masih terdapat ruang untuk peningkatan. Performa ini bisa ditingkatkan lebih lanjut melalui hyperparameter tuning yang lebih ekstensif pada model SVD, mencoba algoritma collaborative filtering lainnya, atau menggabungkannya dengan pendekatan lain (misalnya, hibrida). Selain akurasi prediksi rating, untuk sistem yang bertujuan menghasilkan daftar top-N, metrik evaluasi seperti precision@N, recall@N, F1-score@N, atau nDCG juga sangat relevan untuk mengukur kualitas urutan rekomendasi.

 **Penjabaran Hasil Evaluasi**
Berdasarkan implementasi proyek sistem rekomendasi buku yang dikembangkan menggunakan pendekatan content-based filtering dan collaborative filtering (SVD), seluruh problem statement yang telah dirumuskan berhasil dijawab secara sistematis. Permasalahan terkait kesulitan pengguna dalam menemukan buku yang relevan telah diatasi melalui pemanfaatan content-based filtering yang merekomendasikan buku berdasarkan kesamaan fitur seperti judul dan penulis, memungkinkan pengguna tetap mendapatkan rekomendasi meskipun belum pernah memberikan rating sebelumnya. Di sisi lain, collaborative filtering dengan metode Singular Value Decomposition (SVD) mampu mempelajari pola interaksi antar pengguna dan buku, sehingga dapat memberikan rekomendasi yang lebih personal dan kontekstual. Dari sisi pencapaian tujuan (goals), sistem ini terbukti berhasil memberikan rekomendasi yang akurat, yang ditunjukkan oleh nilai Root Mean Square Error (RMSE) sebesar 1.57 pada model SVD, yang merupakan indikator performa prediktif yang cukup baik dalam konteks data rating. Selain itu, solusi-solusi yang dirancang terbukti berdampak signifikan terhadap kualitas sistem rekomendasi. Content-based filtering memberikan fleksibilitas pada sistem untuk tetap memberikan rekomendasi meski data pengguna terbatas, sementara collaborative filtering menambah dimensi personalisasi berdasarkan preferensi kolektif. Dampak keseluruhan dari kedua pendekatan ini menciptakan sistem rekomendasi yang tidak hanya fungsional, namun juga scalable dan relevan untuk diterapkan dalam platform digital literasi atau toko buku daring. Ke depan, hasil ini memberikan landasan yang kuat untuk pengembangan sistem hybrid yang menggabungkan keunggulan kedua metode demi meningkatkan kepuasan pengguna secara menyeluruh.
