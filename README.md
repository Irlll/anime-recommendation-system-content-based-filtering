# Proyek Akhir Anime Recommendation System

#### Disusun oleh : Irbah Labibah Nur Saidah

Ini adalah proyek akhir sistem rekomendasi untuk memenuhi submission dicoding. Proyek ini membangun model berbasis content based filtering yang dapat menentukan top-N rekomendasi anime bagi user.

## Domain Proyek

### Latar Belakang

Animasi merupakan proses menciptakan efek gerakan pada suatu objek dari perubahan kumpulan gambar secara terus-menerus dalam jangka waktu tertentu. Proses ini juga menambahkan efek suara, emosi, serta karakter sesuai dengan cerita sehingga dapat menghasilkan objek yang seolah-olah hidup. Anime tidak memiliki arti khusus dan merupakan cara orang Jepang untuk menyebut *animation*, baik animasi buatan Jepang maupun bukan. Namun, bagi orang-orang di luar Japang, kata anime digunakan untuk membedakan animasi buatan Jepang dengan animasi buatan Amerika dikarenakan adanya perbedaan ciri khas dari animasi kedua negara tersebut.

<br>

<div><img src="https://user-images.githubusercontent.com/107544829/190916199-7df48b2b-556b-41da-a0f1-ae37f91c3aee.png" width="1000"/></div>

[Referensi gambar](https://wall.alphacoders.com/big.php?i=546902)

<br>

Anime sangat populer di dalam dan di luar Jepang sehingga banyak situs atau sistem steaming online yang memungkinkan pengguna untuk menonton anime dimana saja. Namun, jumlah anime yang beredar saat ini sangatlah banyak dan kadang membuat pengguna kesukaran untuk menemukan anime yang sesuai dengan selera mereka. Hal ini juga dapat disebabkan oleh terbatasnya deskripsi dan ulasan pengguna.

Berdasarkan pada masalah tersebut, maka penelitian ini dilakukan untuk memberi rekomendasi anime kepada pengguna berdasarkan kemiripan dari preferensi pengguna terhadap anime sebelumnya. Diharapkan dengan memberikan rekomendasi anime yang sesuai, pengguna dapat terhibur dan menghabiskan waktu lebih lama di dalam sistem dan mendatangkan keuntungan bagi pengembang sebagai penyedia sistem.

Referensi :

- [Anime dan Gaya Hidup Mahasiswa](https://repository.uinjkt.ac.id/dspace/bitstream/123456789/45316/1/Ida%20Aisyah.pdf)
- [Rekomendasi Anime dengan Latent Semantic Indexing Berbasis Sinopsis Genre](https://www.researchgate.net/publication/274712918_Rekomendasi_Anime_dengan_Latent_Semantic_Indexing_Berbasis_Sinopsis_Genre)

## Business Understanding

Proyek ini dibangun untuk perusahaan dengan karakteristik bisnis sebagai berikut :

- Perusahaan pengembang situs atau sistem streaming anime online.
- Perusahaan pengembang situs rekomendasi dan informasi anime.

### Problem Statement

1. Dapatkah sistem memberikan rekomendasi tanpa input dari pengguna baru?
2. Berdasarkan anime yang baru saja disukai pengguna, bagaimana cara membuat daftar rekomendasi anime dengan metode pendekatan content based filtering?

### Goals

1. Menampilkan daftar top rekomendasi anime untuk pengguna baru.
2. Menghasilkan daftar rekomendasi anime berdasarkan anime yang disukai pengguna dengan metode pendekatan content based filtering.

### Solution Statement

1. Menganalisis data dengan melakukan univariate analysis dan multivariate analysis. Memahami data juga dapat dilakukan dengan visualisasi. Tahap ini dapat menyelsaikan *goals* nomor 1.
2. Menyiapkan data agar bisa digunakan dalam membangun model.
3. Melakukan pengembangan model dengan pendekatan content based filtering serta melakukan evaluasi. Tahap ini dapat menyelesaikan *goals* nomor 2.

## Data Understanding & Preprocessing

Dataset yang digunakan dalam proyek ini merupakan data daftar judul anime dengan karakteristik dan jumlah penggemar serta rata-rata rating dari pengguna. Dataset ini dapat diunduh di [Kaggle : House Rent Prediction Dataset](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database).

Berikut informasi pada dataset :
<br>

Terdapat dua dataset, yaitu dataset anime yang berisi setiap judul anime beserta dengan genre, tipe, epidoses, rata-rata rating, dan jumlah anggota komunitas anime tersebut dan dataset anime_rating yang berisi pemberian rating anime dari setiap user.

1. Dataset anime
<br> - Dataset memiliki format CSV.
<br> - Dataset memiliki 12294 sample dengan 7 fitur.
<br> - Dataset memiliki 1 fitur bertipe float(64), 2 fitur bertipe int64, dan 4 fitur bertipe object.
<br> - Terdapat missing value pada dataset.

2. Dataset anime_rating
<br> - Dataset memiliki format CSV.
<br> - Dataset memiliki 7813737 sample dengan 3 fitur.
<br> - Dataset memiliki 1 fitur bertipe int64.
<br> - Tidak ada missing value pada dataset.

### Variable - Variable Pada Dataset

1. Dataset anime
<br> - `anime_id` = ID unik dari setiap anime.
<br> - `name` = Judul anime.
<br> - `genre` =  genre anime.
<br> - `type` = Tipe tayang anime, seperti TV, OVA, etc.
<br> - `episodes` = Banyaknya episode setiap anime.
<br> - `rating` = Rata-rata rating setiap anime terhadap jumlah user yang memberi rating.
<br> - `members` = Jumlah anggota komunitas setiap anime.

2. Dataset anime_rating
<br> - `user_id` = ID unik dari setiap user
<br> - `anime_id` = ID dari anime yang diberi peringkat oleh user
<br> - `rating` = Rating yang diberikan oleh user.

### Data preprocessing

1. Menghapus missing value pada dari dataset anime.
2. Menghapus duplikat sample dari rating anime dataset.
3. Menghapus symbol pada judul anime.

### Univariate Analysis

Univariate Analysis adalah menganalisis setiap fitur secara terpisah.

#### Analisis setiap atribut dataset anime

|       | anime_id | rating   | members    |
|-------|----------|----------|------------|
| count | 12017.00 | 12017.00 | 12017.00   |
| mean  | 13638.00 |     6.48 | 18348.88   |
| std   | 11231.08 | 1.02     | 55372.50   |
| min   | 1.00     | 1.67     | 12.00      |
| 25%   | 3391.00  | 5.89     | 225.00     |
| 50%   | 9959.00  | 6.57     | 1552.00    |
| 75%   | 23729.00 | 7.18     | 9588.00    |
| max   | 34519.00 | 10.00    | 1013917.00 |

Dataset anime memiliki rating anime terendah 1.67 dan rating tertinggi 10 dengan rata-rata 6.48. Dataset ini juga memiliki jumlah anggota komunitas anime terendah 12 dan terbanyak 1013917 dengan rata-rata 18348. Perbedaan nilai min dan max dari jumlah anggota komunitas anime cukup jauh dan hal ini wajar dikarenakan beberapa anime memang sangat populer dan beberapa tidak.

#### Analisis setiap atribut numerik dataset anime_rating

|       | user_id    | anime_id   | rating     |
|-------|------------|------------|------------|
| count | 7813736.00 | 7813736.00 | 7813736.00 |
| mean  |   36727.96 |    8909.07 | 6.14       |
| std   | 20997.95   | 8883.95    | 3.73       |
| min   | 1.00       | 1.00       | -1.00      |
| 25%   | 18974.00   | 1240.00    | 6.00       |
| 50%   | 36791.00   | 6213.00    | 1552.00    |
| 75%   | 54757.00   | 14093.00   | 9.00       |
| max   | 73516.00   | 34519.00   | 10.00      |

Dataset rating anime memiliki rating terendah yang diberikan user pada suatu anime adalah -1 dan rating tertinggi adalah 10. Rating -1 menandakan bahwa user menonton anime, namun tidak memberikan rating. Sample user yang tidak memberikan rating tidak akan digunakan sehingga dihapus.

```
anime_rating = anime_rating[~(anime_rating.rating == -1)]
```

#### Analisis fitur kategorik genre pada dataset anime

<div><img src="https://user-images.githubusercontent.com/107544829/190631304-e27ab156-08a5-474f-9d7d-52fe2e58112f.png" width="400"/></div>
Dataset anime memiliki banyak sekali genre unik. Dapat diperhatikan bahwa 1 judul anime bisa memiliki banyak genre atau hanya memiliki 1 genre. Pemberian genre yang seperti ini wajar dalam anime.

#### Analisis fitur kategorik tipe tayang pada dataset anime

<div><img src="https://user-images.githubusercontent.com/107544829/190632721-2f1bbc4f-8567-4cc9-b49f-2c0a5e28beb1.png" width="350"/></div>

Insights :

- 30.52% anime ditayangkan di TV.
- 27.33% anime tayang dalam bentuk OVA.
- 18.80% anime tayang dalam bentuk movie.

Movie adalah anime yang tayang dalam bentuk film. Original Video Animation (OVA) adalah anime yang diliris dalam bentuk fisik (CD, DVD, HD-DVD, Blu-ray, dll) tanpa penyiaran TV. Original Net Animation (ONA) adalah anime yang tayang lebih dahulu lewat internet. Spesial adalah episode anime yang durasinya hanya beberapa menit dan biasanya tidak memiliki hubungan dengan alur cerita aslinya atau sebagai fan service.

<br>

Referensi :
[Pengertian Episode OVA,ONA,OAD dan SPESIAL Dalam Anime](https://binarycode100.wordpress.com/2018/12/08/pengertian-episode-ovaonaoad-dan-spesial-dalam-anime/).

#### Analisis distribusi data rata-rata rating anime

<div><img src="https://user-images.githubusercontent.com/107544829/190857442-0b12b379-067e-4595-9481-1cd20323f4c0.png" width="300"/></div>

Sebagian besar rata-rata peringkat anime tersebar dari rating 4 hingga 8.

#### Analisis distribusi data peringkat yang diberikan pengguna

<div><img src="https://user-images.githubusercontent.com/107544829/190857570-e8c4fd5e-994c-4519-9792-dca9b66c30db.png" width="300"/></div>

Sebagian besar peringkat yang diberikan pengguna tersebar dari rating 6 hingga 10.

### Multivariate Analysis

Multivariate Analysis menunjukkan hubungan antara dua atau lebih fitur dalam data.

#### Top 10 komunitas anime terbesar

<div><img src="https://user-images.githubusercontent.com/107544829/190857731-c2932dff-c16c-4c4c-af47-29e59bc5de8a.png" width="1000"/></div>

Anime Death Note memiliki anggota komunitas tertinggi, diikuti dengan Shingeki no Kyojin dan Sword Art Online. Informasi ini dapat digunakan pengembang sistem dalam merekomendasikan anime yang populer kepada penggunanya. Banyaknya anggota komunitas anime menandakan bahwa anime cukup favorit dan populer di kalangan pengguna.

#### Top 10 anime berdasarkan rata-rata rating anime

<div><img src="https://user-images.githubusercontent.com/107544829/190858010-98b51998-dde0-4dc2-b978-9284f8d2dc4c.png" width="1000"/></div>

Anime Taka no Tsume 8: Yoshida-kun no X-Files memiliki rata-rata rating anime tertinggi, diikuti dengan Spoon-hime no Swing Kitchen dan Mogura no Motoro. Informasi ini cenderung bias untuk dijadikan rekomendasi dikarenakan rata-rata rating yang dipengaruhi oleh jumlah pengguna yang memberi rating. Misalnya anime X memiliki rata-rata rating tinggi, namun yang memberikan rating ternyata hanya 3 pengguna.

#### Top 10 anime berdasarkan kontribusi rating pengguna

<div><img src="https://user-images.githubusercontent.com/107544829/190858357-7be813d7-67e6-4283-899c-6af270de3fb6.png" width="1000"/></div>

Anime Death Note menyumbang kontribusi peringkat oleh pengguna terbanyak, diikuti oleh anime Sword Art Online dan anime Shingeki no Kyokin. Informasi ini dapat digunakan pengembang sistem dalam merekomendasikan anime yang populer kepada penggunanya. Hal ini dikarenakan semakin banyaknya kontribusi peringkat, semakin banyak pula pengguna yang menonton anime tersebut (populer).

## Content Based Filtering Model & Result

### Content Based Filtering

Sistem yang dibangun oleh proyek ini adalah sistem rekomendasi sederhana berdasarkan genre anime berbasis content based filtering.

Sistem rekomendasi berbasis konten adalah sistem yang merekomendasikan konten yang mirip dengan konten yang disukai pengguna sebelumnya. Apabila suatu konten memiliki karakteristik yang sama atau hampir sama dengan konten lainnya, maka kedua konten tersebut dapat dikatakan mirip.

Misalkan dalam sistem rekomendasi anime, jika pengguna menyukai anime Jujutsu Kaisen, sistem dapat merekomendasikan anime dengan genre action lainnya.

<div><img src="https://user-images.githubusercontent.com/107544829/190860242-6e0e9d61-e54f-46d0-930e-415e73cebab0.png" width="500"/></div>

#### TF-IDF

TF-IDF atau Term Frequency - Inverse Document Frequency, Bahasa adalah ukuran statistik yang menggambarkan pentingnya istilah dalam dokumen dalam kumpulan atau korpus. Metrik ini biasanya digunakan sebagai faktor pembobotan untuk pencarian informasi, penambangan teks, dan pemodelan pengguna. Nilai TF-IDF meningkat secara linier dengan jumlah kemunculan suatu term dan bergantung pada jumlah dokumen dalam korpus yang memuat term tersebut.

TF-IDF digunakan pada sistem rekomendasi anime untuk menentukan representasi fitur penting dari setiap genre anime. Untuk menjalankan TF-IDF digunakan fungsi [tfidfvectorizer()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) dari library sklearn.

Setelah itu hasil TF-IDF tadi ditransformasikan ke dalam bentuk matriks dengan fungsi todense().

Dataframe baru dibuat untuk menunjukkan matriks TF-IDF untuk beberapa anime dan genre. Semakin tinggi nilai matriks menunjukkan semakin erat hubungan antara anime dengan genre tersebut. Misalkan anime Asa da yo! Kaishain merupakan genre comedy terlihat dari nilai matriks 1.0 yang didapat anime tersebut pada genre comedy.

<br>
<div><img src="https://user-images.githubusercontent.com/107544829/190863156-e486ec9c-e784-46db-ab82-d65cd25de13b.png" width="700"/></div>

<br>

Referensi : [Toggle the table of contents
tf–idf](https://id.wikipedia.org/wiki/Tf%E2%80%93idf)

#### Cosine Similarity

Cosine Similarity mengukur kesamaan antara dua vektor dan menentukan apakah kedua vektor menunjuk ke arah yang sama. Teknik ini bekerja dengan menghitung sudut cosinus antara dua vektor. Semakin kecil sudut cosinus antara dua vektor, semakin besar nilai kemiripan cosinusnya.

<br>

<div><img src="https://user-images.githubusercontent.com/107544829/190915969-92ac61ae-b1ac-44d9-9ec1-92f778c19602.png" width="400"/></div>

[Referensi gambar](https://www.tyrrell4innovation.ca/miword-of-the-day-iscosine-distance/)

<br>

Cosine similarity digunakan untuk menghitung derajat kesamaan antar anime. Untuk menjalankan cosine similarity digunakan fungsi [cosine_similarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html) dari library sklearn.  

Tahap ini menghitung cosise similarity pada dataframe tfidf_matrix yang diperoleh dari tahapan TF-IDF sebelumnya.

Dataframe baru dibuat untuk melihat kesamaan antar anime (hasil dari cosine similarity). Dataframe ini menunjukkan nilai kesamaan antar anime. Semakin tinggi nilai cosline similarity, kedua anime akan semakin memiliki kesamaan.Misalnya, Mama Puri!?, Dark Blue, dan Anata dake Konbanwa memiliki kesamaan dengan anime Kisaku Spirit terlihat dari nilai cosine similarity 1.0 antar kedua anime tersebut.

<br>

|                                                                | Kisaku Spirit |
|----------------------------------------------------------------|---------------|
| Hourou Musuko                                                  | 0.000000      |
| Mama Puri!?                                                    | 1.000000      |
| Dark Blue                                                      | 1.000000      |
| Anime Tenchou                                                  | 0.000000      |
| Chocolate Underground                                          | 0.000000      |
| Anata dake Konbawa                                             | 1.000000      |
| Bishoujo ANimerama: Gokkun Doli - Choujigen Pico-chan Toujou!! | 0.547047      |
| Grendizer Giga                                                 | 0.000000      |
| Pink Mizu Dorobou Ame Dorobou                                  | 0.000000      |
| En En Nikoli                                                   | 0.000000      |


<br>

### Result

Fungsi `anime_recommendations` dibuat untuk menemukan rekomendasi anime menggunakan similarity yang telah didefinisikan sebelumnya. Fungsi ini bekerja dengan cara mengambil anime dengan similarity terbesar dari index yang ada.

Selanjutnya adalah menemukan rekomendasi yang mirip dengan anime Naruto :

<br>

| anime_id | name   | genre                                              | type | episodes | rating | members |
|----------|--------|----------------------------------------------------|------|----------|--------|---------|
| 20       | Naruto | Action, Comedy, Martial Arts, Shounen, Super Power | TV   | 220      | 7.81   | 683297  |

Berikut top 5 rekomendasi :

<br>

| name                                                    | genre                                              |
|---------------------------------------------------------|----------------------------------------------------|
| Naruto: Shippuuden Movie 4 - The Lost Tower             | Action, Comedy, Martial Arts, Shounen, Super Power |
| Naruto Shippuuden: Sunny Side Battle                    | Action, Comedy, Martial Arts, Shounen, Super Power |
| Boruto: Naruto the Movie - Naruto ga Hokage ni Natta Hi | Action, Comedy, Martial Arts, Shounen, Super Power |
| Naruto x UT                                             | Action, Comedy, Martial Arts, Shounen, Super Power |
| Naruto: Shippuuden                                      | Action, Comedy, Martial Arts, Shounen, Super Power |

Sistem telah berhasil merekomendasikan top 5 persen anime yang mirip dengan naruto, yaitu beberapa film dan seri dari naruto itu sendiri. Jadi, jika pengguna menyunkai naruto, maka sistem dapat merekomendasikan seri atau movie naruto lainnya.

<br>

## Evaluation
Evaluasi hasil sistem dengan recommender system precision dalam menemukan rekomendasi dari anime naruto adalah sebesar 5/5 atau 100%.

<div><img src="https://user-images.githubusercontent.com/107544829/190915653-cf57d3de-db41-455c-b060-b4dd6630157b.png" width="500"/></div>

<br>
