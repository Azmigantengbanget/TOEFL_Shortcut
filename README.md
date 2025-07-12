<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Sinonim kata 'ubiquitous'?", "id": "Ada di mana-mana." },
  { "en": "Sinonim kata 'ambiguous'?", "id": "Memiliki banyak arti, tidak jelas." },
  { "en": "Sinonim kata 'resilient'?", "id": "Tangguh, mampu bangkit kembali." },
  { "en": "Sinonim kata 'mitigate'?", "id": "Mengurangi dampak atau keparahan." },
  { "en": "Sinonim kata 'profound'?", "id": "Sangat mendalam atau hebat." },
  { "en": "Sinonim kata 'ostensibly'?", "id": "Tampaknya, seolah-olah, di permukaan." },
  { "en": "Sinonim kata 'exacerbate'?", "id": "Memperburuk situasi atau kondisi." },
  { "en": "Sinonim kata 'empirical'?", "id": "Berdasarkan pengamatan atau pengalaman." },
  { "en": "Sinonim kata 'anomalous'?", "id": "Tidak normal, menyimpang dari standar." },
  { "en": "Sinonim kata 'adverse'?", "id": "Tidak menguntungkan, berbahaya, berlawanan." },
  { "en": "Fungsi frasa 'consequently'?", "id": "Menunjukkan sebuah hasil atau akibat." },
  { "en": "Fungsi frasa 'furthermore'?", "id": "Menambahkan informasi atau poin baru." },
  { "en": "Fungsi frasa 'in contrast'?", "id": "Menunjukkan perbedaan atau pertentangan." },
  { "en": "Fungsi frasa 'for instance'?", "id": "Untuk memberikan sebuah contoh spesifik." },
  { "en": "Fungsi frasa 'despite this'?", "id": "Menunjukkan kontras dengan pernyataan sebelumnya." },
  { "en": "Apa arti 'photosynthesis'?", "id": "Proses tumbuhan membuat makanan." },
  { "en": "Apa arti 'biodiversity'?", "id": "Keanekaragaman hayati di suatu ekosistem." },
  { "en": "Apa arti 'plate tectonics'?", "id": "Teori pergerakan lempeng bumi." },
  { "en": "Apa arti 'industrial revolution'?", "id": "Perubahan besar ke proses manufaktur." },
  { "en": "Apa arti 'urbanization'?", "id": "Proses perpindahan penduduk ke kota." },
  { "en": "Arti idiom 'a piece of cake'?", "id": "Sesuatu yang sangat mudah dilakukan." },
  { "en": "Arti idiom 'bite the bullet'?", "id": "Menghadapi situasi sulit dengan berani." },
  { "en": "Arti idiom 'break a leg'?", "id": "Ucapan semoga berhasil atau sukses." },
  { "en": "Arti idiom 'call it a day'?", "id": "Berhenti bekerja untuk hari itu." },
  { "en": "Arti idiom 'get out of hand'?", "id": "Menjadi tidak terkendali." },
  { "en": "Arti idiom 'hang in there'?", "id": "Jangan menyerah, tetap bertahan." },
  { "en": "Arti idiom 'it's not rocket science'?", "id": "Sesuatu yang tidak sulit dipahami." },
  { "en": "Arti idiom 'miss the boat'?", "id": "Kehilangan kesempatan atau peluang." },
  { "en": "Arti idiom 'pull yourself together'?", "id": "Tenangkan diri, kontrol emosimu." },
  { "en": "Arti idiom 'so far so good'?", "id": "Sejauh ini semuanya berjalan lancar." },
  { "en": "Arti idiom 'speak of the devil'?", "id": "Orang yang dibicarakan tiba-tiba muncul." },
  { "en": "Arti idiom 'the last straw'?", "id": "Batas akhir kesabaran." },
  { "en": "Arti idiom 'under the weather'?", "id": "Merasa tidak enak badan, sakit." },
  { "en": "Arti idiom 'once in a blue moon'?", "id": "Sesuatu yang terjadi sangat jarang." },
  { "en": "Arti idiom 'the best of both worlds'?", "id": "Mendapat keuntungan dari dua hal." },
  { "en": "Alasan tinggal di desa?", "id": "Udara bersih dan suasana tenang." },
  { "en": "Alasan belajar online?", "id": "Jadwal fleksibel dan lebih hemat." },
  { "en": "Alasan berolahraga setiap hari?", "id": "Menjaga kesehatan fisik dan mental." },
  { "en": "Keuntungan punya hewan peliharaan?", "id": "Mengurangi stres, teman bermain." },
  { "en": "Dampak positif media sosial?", "id": "Terhubung dengan teman dan keluarga." },
  { "en": "Dampak negatif media sosial?", "id": "Menyebabkan kecanduan dan perbandingan sosial." },
  { "en": "Alasan pentingnya membaca buku?", "id": "Menambah wawasan dan imajinasi." },
  { "en": "Manfaat belajar bahasa asing?", "id": "Meningkatkan peluang karir internasional." },
  { "en": "Penyebab utama polusi udara?", "id": "Asap kendaraan dan industri." },
  { "en": "Cara mengurangi sampah plastik?", "id": "Gunakan tas belanja dapat dipakai ulang." },
  { "en": "Pentingnya kerja sama tim?", "id": "Mencapai tujuan lebih cepat, efisien." },
  { "en": "Satu kualitas pemimpin yang baik?", "id": "Mampu menginspirasi dan memotivasi." },
  { "en": "Mengapa tidur cukup itu penting?", "id": "Memulihkan energi dan fungsi otak." },
  { "en": "Satu keuntungan dari berwisata?", "id": "Mempelajari budaya baru yang berbeda." },
  { "en": "Pentingnya menabung sejak dini?", "id": "Untuk persiapan dana darurat, masa depan." },
  { "en": "Perbaiki: 'She don't like coffee'?", "id": "'She doesn't like coffee.'" },
  { "en": "Perbaiki: 'I have saw that movie'?", "id": "'I have seen that movie.'" },
  { "en": "Perbaiki: 'He is more smarter'?", "id": "'He is smarter.'" },
  { "en": "Perbaiki: 'They was happy'?", "id": "'They were happy.'" },
  { "en": "Perbaiki: 'Me and my friend went'?", "id": "'My friend and I went.'" },
  { "en": "Perbaiki: 'Listen! The baby cry'?", "id": "'Listen! The baby is crying.'" },
  { "en": "Perbaiki: 'I am agree with you'?", "id": "'I agree with you.'" },
  { "en": "Perbaiki: 'This is a scissors'?", "id": "'These are scissors.'" },
  { "en": "Perbaiki: 'Everybody are here'?", "id": "'Everybody is here.'" },
  { "en": "Perbaiki: 'I look forward to meet you'?", "id": "'I look forward to meeting you.'" },
  { "en": "Kata transisi untuk sebab-akibat?", "id": "Therefore, as a result, consequently." },
  { "en": "Kata transisi untuk menambahkan ide?", "id": "In addition, moreover, furthermore." },
  { "en": "Kata transisi untuk memberi contoh?", "id": "For example, for instance." },
  { "en": "Kata transisi untuk kesimpulan?", "id": "In conclusion, to sum up." },
  { "en": "Kata transisi untuk urutan?", "id": "First, second, then, finally." },
  { "en": "Bentuk lampau dari 'go'?", "id": "Went." },
  { "en": "Bentuk lampau dari 'eat'?", "id": "Ate." },
  { "en": "Bentuk lampau dari 'see'?", "id": "Saw." },
  { "en": "Bentuk lampau dari 'begin'?", "id": "Began." },
  { "en": "Bentuk lampau dari 'write'?", "id": "Wrote." },
  { "en": "Kapan menggunakan 'much'?", "id": "Untuk kata benda tak terhitung." },
  { "en": "Kapan menggunakan 'many'?", "id": "Untuk kata benda dapat dihitung." },
  { "en": "Contoh benda tak terhitung?", "id": "Water, information, money, time." },
  { "en": "Contoh benda dapat dihitung?", "id": "Books, chairs, ideas, dollars." },
  { "en": "Kapan menggunakan 'a' vs 'an'?", "id": "'An' sebelum bunyi huruf vokal." },
  { "en": "Kalimat pasif dari 'He writes a letter'?", "id": "'A letter is written by him.'" },
  { "en": "Kalimat pasif dari 'They built the house'?", "id": "'The house was built by them.'" },
  { "en": "Bentuk komparatif dari 'good'?", "id": "Better." },
  { "en": "Bentuk superlatif dari 'good'?", "id": "Best." },
  { "en": "Bentuk komparatif dari 'bad'?", "id": "Worse." },
  { "en": "Kalimat pengandaian tipe 1?", "id": "If + simple present, will + verb." },
  { "en": "Kalimat pengandaian tipe 2?", "id": "If + simple past, would + verb." },
  { "en": "Kalimat pengandaian tipe 3?", "id": "If + past perfect, would have + V3." },
  { "en": "Fungsi pengandaian tipe 1?", "id": "Menyatakan kemungkinan nyata di masa depan." },
  { "en": "Fungsi pengandaian tipe 2?", "id": "Menyatakan situasi tidak nyata sekarang." },
  { "en": "Fungsi pengandaian tipe 3?", "id": "Menyatakan penyesalan tentang masa lalu." },
  { "en": "Apa itu 'gerund'?", "id": "Kata kerja yang berfungsi sebagai kata benda." },
  { "en": "Ciri khas 'gerund'?", "id": "Berakhiran -ing (contoh: swimming)." },
  { "en": "Apa itu 'infinitive'?", "id": "Bentuk dasar kata kerja (to + V1)." },
  { "en": "Kata kerja yang diikuti 'gerund'?", "id": "Enjoy, avoid, finish, mind." },
  { "en": "Parafrasa dari 'The discovery was pivotal'?", "id": "Penemuan itu sangat penting." },
  { "en": "Parafrasa dari 'The process is intricate'?", "id": "Prosesnya rumit dan mendetail." },
  { "en": "Parafrasa dari 'The results were inconclusive'?", "id": "Hasilnya tidak memberikan kesimpulan." },
  { "en": "Parafrasa dari 'It has numerous applications'?", "id": "Ini memiliki banyak kegunaan." },
  { "en": "Parafrasa dari 'The theory was refuted'?", "id": "Teori itu telah dibantah." },
  { "en": "Sinonim kata 'subsequently'?", "id": "Setelah itu, kemudian, akibatnya." },
  { "en": "Sinonim kata 'inevitable'?", "id": "Tidak bisa dihindari, pasti terjadi." },
  { "en": "Sinonim kata 'rudimentary'?", "id": "Dasar, belum berkembang, sederhana." },
  { "en": "Sinonim kata 'albeit'?", "id": "Meskipun, biarpun, walaupun." },
  { "en": "Sinonim kata 'cumbersome'?", "id": "Berat, merepotkan, tidak efisien." },
  { "en": "Penulis 'skeptis' berarti ia...", "id": "Merasa ragu atau tidak percaya." },
  { "en": "Penulis 'advocates for' berarti ia...", "id": "Mendukung atau menyarankan sesuatu." },
  { "en": "Sikap penulis yang 'neutral'?", "id": "Menyajikan fakta tanpa memihak." },
  { "en": "Sikap penulis yang 'critical'?", "id": "Menunjukkan kekurangan atau kesalahan." },
  { "en": "Kalimat 'It led to...' menunjukkan apa?", "id": "Hubungan sebab dan akibat." },
  { "en": "Kata 'this process' merujuk ke?", "id": "Proses yang dijelaskan kalimat sebelumnya." },
  { "en": "Kata 'they' sering merujuk ke?", "id": "Kata benda jamak sebelumnya." },
  { "en": "Kata 'its' merujuk ke?", "id": "Kepemilikan dari benda tunggal sebelumnya." },
  { "en": "Frasa 'the former' merujuk ke?", "id": "Hal pertama dari dua yang disebut." },
  { "en": "Frasa 'the latter' merujuk ke?", "id": "Hal kedua dari dua yang disebut." },
  { "en": "Arti idiom 'to be in hot water'?", "id": "Berada dalam masalah serius." },
  { "en": "Arti idiom 'to get cold feet'?", "id": "Menjadi takut melakukan sesuatu." },
  { "en": "Arti idiom 'to see eye to eye'?", "id": "Setuju sepenuhnya dengan seseorang." },
  { "en": "Arti idiom 'to hit the nail on the head'?", "id": "Mengatakan sesuatu yang sangat tepat." },
  { "en": "Arti idiom 'to bite off more than you can chew'?", "id": "Mengambil tugas terlalu berat." },
  { "en": "Arti idiom 'to be on the ball'?", "id": "Cepat mengerti dan bereaksi." },
  { "en": "Arti idiom 'to ring a bell'?", "id": "Terdengar familiar, seperti pernah dengar." },
  { "en": "Arti idiom 'to cut corners'?", "id": "Melakukan sesuatu dengan cara termudah." },
  { "en": "Arti idiom 'to be on the fence'?", "id": "Ragu-ragu, tidak bisa memutuskan." },
  { "en": "Arti idiom 'to cost an arm and a leg'?", "id": "Sesuatu yang harganya sangat mahal." },
  { "en": "Jika dosen berkata 'bear with me'?", "id": "Meminta Anda untuk bersabar." },
  { "en": "Jika dosen berkata 'to recap'?", "id": "Ia akan meringkas poin utama." },
  { "en": "Jika mahasiswa berkata 'I'm lost'?", "id": "Ia tidak mengerti penjelasannya." },
  { "en": "Jika mahasiswa berkata 'I can't make it'?", "id": "Ia tidak bisa datang." },
  { "en": "Hubungan dosen dan mahasiswa?", "id": "Formal, akademis, memberikan bimbingan." },
  { "en": "Hubungan dua mahasiswa mengerjakan tugas?", "id": "Kolaboratif, diskusi, saling membantu." },
  { "en": "Prediksi setelah 'Let's define...'?", "id": "Pembicara akan menjelaskan arti istilah." },
  { "en": "Prediksi setelah 'There are two types...'?", "id": "Pembicara akan menjelaskan dua jenis." },
  { "en": "Jika pembicara meninggikan suara?", "id": "Ia sedang menekankan poin penting." },
  { "en": "Jika pembicara berbicara lebih lambat?", "id": "Ia menjelaskan konsep yang sulit." },
  { "en": "Frasa untuk menyetujui di percakapan?", "id": "'You can say that again.'" },
  { "en": "Frasa untuk tidak setuju (sopan)?", "id": "'I see your point, but...'" },
  { "en": "Frasa untuk menunjukkan keraguan?", "id": "'Are you sure about that?'" },
  { "en": "Frasa untuk meminta klarifikasi?", "id": "'Could you explain that again?'" },
  { "en": "Frasa untuk menyela (sopan)?", "id": "'Sorry to interrupt, but...'" },
  { "en": "Frasa pembuka untuk Speaking Task 1?", "id": "'In my opinion...'" },
  { "en": "Frasa untuk memberi alasan?", "id": "'This is because...'" },
  { "en": "Frasa untuk memberi contoh?", "id": "'For example, I once...'" },
  { "en": "Frasa untuk menutup jawaban Speaking?", "id": "'So, that's why I think...'" },
  { "en": "Frasa untuk memulai jawaban terstruktur?", "id": "'There are two main reasons...'" },
  { "en": "Satu ide untuk 'describe your favorite city'?", "id": "Memiliki sistem transportasi publik efisien." },
  { "en": "Satu ide untuk 'a useful skill'?", "id": "Belajar berbicara di depan umum." },
  { "en": "Satu ide untuk 'a favorite book'?", "id": "Memiliki alur cerita tidak terduga." },
  { "en": "Satu ide untuk 'agree/disagree: online class'?", "id": "Setuju, karena menawarkan fleksibilitas waktu." },
  { "en": "Satu ide untuk 'important quality in a friend'?", "id": "Kejujuran, karena membangun kepercayaan." },
  { "en": "Frasa untuk memulai esai Writing?", "id": "'It is often argued that...'" },
  { "en": "Frasa untuk 'thesis statement'?", "id": "'This essay will argue that...'" },
  { "en": "Frasa untuk paragraf pertama isi?", "id": "'One of the primary reasons...'" },
  { "en": "Frasa untuk paragraf kedua isi?", "id": "'Another significant factor is...'" },
  { "en": "Frasa untuk mengenalkan contoh?", "id": "'A clear example of this is...'" },
  { "en": "Frasa untuk kesimpulan esai?", "id": "'In conclusion, based on these points...'" },
  { "en": "Frasa untuk menghubungkan bacaan & audio?", "id": "'The lecture contradicts the reading passage.'" },
  { "en": "Frasa lain untuk menghubungkan?", "id": "'The speaker reinforces the reading's idea.'" },
  { "en": "Frasa untuk melaporkan poin bacaan?", "id": "'The author of the passage states...'" },
  { "en": "Frasa untuk melaporkan poin audio?", "id": "'The professor in the lecture explains...'" },
  { "en": "Perbaiki: 'The reason is because...'?", "id": "Gunakan 'The reason is that...'" },
  { "en": "Perbaiki: 'In the modern world today'?", "id": "Cukup 'In the modern world.'" },
  { "en": "Perbaiki: 'The people is friendly'?", "id": "'The people are friendly.'" },
  { "en": "Perbaiki: 'The number of student are'?", "id": "'The number of students is...'" },
  { "en": "Perbaiki: 'I enjoyed to watch it'?", "id": "'I enjoyed watching it.'" },
  { "en": "Penggunaan 'its' vs 'it's'?", "id": "'Its' kepemilikan, 'it's' singkatan." },
  { "en": "Penggunaan 'their', 'there', 'they're'?", "id": "Kepemilikan, tempat, singkatan." },
  { "en": "Penggunaan 'affect' vs 'effect'?", "id": "'Affect' kata kerja, 'effect' kata benda." },
  { "en": "Penggunaan 'less' vs 'fewer'?", "id": "'Fewer' untuk benda dapat dihitung." },
  { "en": "Penggunaan 'than' vs 'then'?", "id": "'Than' perbandingan, 'then' urutan waktu." },
  { "en": "Kalimat aktif atau pasif di esai?", "id": "Gunakan kalimat aktif lebih sering." },
  { "en": "Cara membuat kalimat lebih akademis?", "id": "Hindari bahasa gaul dan singkatan." },
  { "en": "Contoh bahasa gaul yang dihindari?", "id": "Gonna, wanna, a lot of." },
  { "en": "Pengganti 'a lot of'?", "id": "Numerous, many, a significant amount." },
  { "en": "Pengganti 'get'?", "id": "Obtain, receive, become." },
  { "en": "Pengganti 'bad'?", "id": "Negative, detrimental, harmful, poor." },
  { "en": "Pengganti 'good'?", "id": "Beneficial, positive, effective, valuable." },
  { "en": "Pengganti 'thing'?", "id": "Factor, aspect, issue, concept." },
  { "en": "Pengganti 'show'?", "id": "Illustrate, demonstrate, reveal, indicate." },
  { "en": "Pengganti 'important'?", "id": "Significant, crucial, vital, essential." },
  { "en": "Cara menghindari pengulangan kata?", "id": "Gunakan sinonim dan kata ganti." },
  { "en": "Tujuan 'thesis statement'?", "id": "Memberi tahu pembaca argumen utama esai." },
  { "en": "Apakah 'thesis statement' harus kuat?", "id": "Ya, harus jelas dan dapat diperdebatkan." },
  { "en": "Tanda baca di akhir kalimat?", "id": "Titik (.), tanda tanya (?), seru (!)." },
  { "en": "Fungsi koma (,) dalam kalimat?", "id": "Memisahkan item dalam daftar." },
  { "en": "Fungsi titik koma (;) ?", "id": "Menggabungkan dua klausa independen terkait." },
  { "en": "Trik utama mendapat skor tinggi?", "id": "Jawab pertanyaan, tunjukkan kemampuan bahasa." },
  { "en": "Tujuan utama paragraf pendahuluan?", "id": "Memberikan konteks dan ide utama." },
  { "en": "Tujuan utama paragraf kesimpulan?", "id": "Meringkas dan menegaskan kembali argumen." },
  { "en": "Frasa 'for instance' menandakan?", "id": "Sebuah contoh akan diberikan." },
  { "en": "Frasa 'in other words' menandakan?", "id": "Sebuah parafrasa atau penjelasan ulang." },
  { "en": "Frasa 'as a result' menandakan?", "id": "Sebuah akibat atau konsekuensi." },
  { "en": "Frasa 'similarly' menandakan?", "id": "Sebuah perbandingan atau kesamaan." },
  { "en": "Frasa 'conversely' menandakan?", "id": "Sebuah pertentangan atau kebalikan." },
  { "en": "Sinonim kata 'subtle'?", "id": "Halus, tidak kentara, tidak langsung." },
  { "en": "Sinonim kata 'feasible'?", "id": "Mungkin dilakukan, layak, memungkinkan." },
  { "en": "Sinonim kata 'abrupt'?", "id": "Tiba-tiba, mendadak, tidak terduga." },
  { "en": "Sinonim kata 'leverage' (verb)?", "id": "Memanfaatkan untuk keuntungan maksimal." },
  { "en": "Sinonim kata 'discrepancy'?", "id": "Perbedaan, ketidaksesuaian, kesenjangan." },
  { "en": "Bedakan: 'Pivotal discovery' vs 'Minor detail'?", "id": "Pivotal mengubah pemahaman, minor mendukung." },
  { "en": "Bedakan: 'Main cause' vs 'Contributing factor'?", "id": "Main cause utama, contributing membantu." },
  { "en": "Bedakan: 'Hypothesis' vs 'Theory'?", "id": "Hypothesis dugaan, theory terbukti." },
  { "en": "Bedakan: 'Correlation' vs 'Causation'?", "id": "Correlation hubungan, causation sebab-akibat." },
  { "en": "Maksud penulis: 'This view is problematic'?", "id": "Penulis tidak setuju dengan pandangan." },
  { "en": "Maksud penulis: 'This laid the groundwork for...'?", "id": "Ini menjadi dasar untuk perkembangan." },
  { "en": "Maksud penulis: 'A common misconception is...'?", "id": "Penulis akan mengoreksi pemahaman salah." },
  { "en": "Maksud penulis: 'It remains to be seen'?", "id": "Hasilnya belum diketahui atau pasti." },
  { "en": "Arti 'go over your notes'?", "id": "Meninjau kembali catatan Anda." },
  { "en": "Arti 'fall behind on work'?", "id": "Tertinggal dalam pekerjaan atau tugas." },
  { "en": "Arti 'to brush up on' a skill?", "id": "Meningkatkan kembali skill yang lama." },
  { "en": "Arti 'to figure something out'?", "id": "Memahami atau menemukan solusi." },
  { "en": "Arti 'to turn in an assignment'?", "id": "Menyerahkan atau mengumpulkan tugas." },
  { "en": "Arti 'to be a straight-A student'?", "id": "Siswa yang selalu dapat nilai A." },
  { "en": "Arti 'to pull an all-nighter'?", "id": "Begadang semalaman untuk belajar/kerja." },
  { "en": "Arti 'to work something out'?", "id": "Menyelesaikan masalah, mencari jalan keluar." },
  { "en": "Arti 'the main takeaway is...'?", "id": "Poin atau pelajaran terpenting adalah..." },
  { "en": "Arti 'to sign up for a class'?", "id": "Mendaftar untuk sebuah kelas." },
  { "en": "Dosen berkata 'Any questions so far'?", "id": "Memberi kesempatan untuk bertanya." },
  { "en": "Dosen berkata 'Let's move on'?", "id": "Akan beralih ke topik selanjutnya." },
  { "en": "Dosen berkata 'That's a good question'?", "id": "Menghargai pertanyaan yang diajukan." },
  { "en": "Dosen berkata 'I'll get back to you'?", "id": "Akan menjawabnya di lain waktu." },
  { "en": "Nada suara 'enthusiastic' menunjukkan?", "id": "Pembicara sangat bersemangat/tertarik." },
  { "en": "Nada suara 'doubtful' menunjukkan?", "id": "Pembicara merasa ragu atau tidak yakin." },
  { "en": "Nada suara 'surprised' menunjukkan?", "id": "Pembicara tidak menduga hal tersebut." },
  { "en": "Struktur kuliah 'problem-solution'?", "id": "Menjelaskan masalah lalu menawarkan solusi." },
  { "en": "Struktur kuliah 'compare-contrast'?", "id": "Menunjukkan persamaan dan perbedaan." },
  { "en": "Struktur kuliah 'chronological'?", "id": "Menjelaskan kejadian berdasarkan urutan waktu." },
  { "en": "Apa yang diimplikasikan pembicara?", "id": "Makna yang tidak diucapkan langsung." },
  { "en": "Tingkat kepastian: 'It seems that...'?", "id": "Tidak terlalu pasti, kemungkinan." },
  { "en": "Tingkat kepastian: 'It's definitively...'?", "id": "Sangat pasti, tidak ada keraguan." },
  { "en": "Tingkat kepastian: 'Perhaps it could...'?", "id": "Sangat tidak pasti, spekulatif." },
  { "en": "Jika pembicara mengulang sebuah kata?", "id": "Kata tersebut sangat penting." },
  { "en": "Frasa pembuka untuk Speaking Task 2 (Campus Situation)?", "id": "'The university plans to...'" },
  { "en": "Frasa untuk menyampaikan opini mahasiswa?", "id": "'The man/woman agrees with the plan.'" },
  { "en": "Frasa untuk alasan pertama opini?", "id": "'His/her first reason is that...'" },
  { "en": "Frasa untuk alasan kedua opini?", "id": "'Secondly, he/she mentions that...'" },
  { "en": "Ide untuk 'advantages of traveling'?", "id": "Memperluas wawasan dan pengalaman budaya." },
  { "en": "Ide untuk 'disadvantages of social media'?", "id": "Menyita waktu dan menyebarkan misinformasi." },
  { "en": "Ide untuk 'importance of teamwork'?", "id": "Menggabungkan beragam keahlian untuk hasil." },
  { "en": "Ide untuk 'the best way to learn'?", "id": "Melalui praktik langsung dan pengalaman." },
  { "en": "Ide untuk 'living in a dorm vs off-campus'?", "id": "Asrama lebih mudah, apartemen lebih bebas." },
  { "en": "Contoh 'general' vs 'specific'?", "id": "'Fruit' (umum), 'a green apple' (spesifik)." },
  { "en": "Mengapa contoh spesifik itu penting?", "id": "Membuat argumen lebih kuat meyakinkan." },
  { "en": "Frasa untuk mengenalkan contoh spesifik?", "id": "'To be more specific...'" },
  { "en": "Frasa untuk Speaking Task 3 (Academic Lecture)?", "id": "'The reading explains the concept of...'" },
  { "en": "Frasa untuk menghubungkan ke contoh dosen?", "id": "'The professor illustrates this with an example.'" },
  { "en": "Frasa untuk menjelaskan contoh dosen?", "id": "'He/she describes a study where...'" },
  { "en": "Frasa untuk kesimpulan Speaking Task 3?", "id": "'This example clearly illustrates the concept.'" },
  { "en": "Frasa untuk memulai paragraf kontra-argumen?", "id": "'Some people might argue that...'" },
  { "en": "Frasa untuk menyanggah kontra-argumen (rebuttal)?", "id": "'However, this view is flawed because...'" },
  { "en": "Frasa untuk memperkuat argumen?", "id": "'This evidence strongly suggests that...'" },
  { "en": "Frasa untuk menunjukkan konsesi (mengakui poin)?", "id": "'While it is true that...'" },
  { "en": "Frasa untuk mengakhiri kesimpulan esai?", "id": "'Therefore, it is clear that...'" },
  { "en": "Kata ganti 'which' digunakan untuk?", "id": "Memberikan informasi tambahan tentang benda." },
  { "en": "Kata ganti 'that' digunakan untuk?", "id": "Memberikan informasi esensial tentang benda." },
  { "en": "Kata ganti 'who' digunakan untuk?", "id": "Merujuk kepada orang (sebagai subjek)." },
  { "en": "Kata ganti 'whom' digunakan untuk?", "id": "Merujuk kepada orang (sebagai objek)." },
  { "en": "Penggunaan 'due to'?", "id": "Diikuti oleh kata benda." },
  { "en": "Penggunaan 'because'?", "id": "Diikuti oleh subjek dan kata kerja." },
  { "en": "Penggunaan 'although'?", "id": "Diikuti oleh subjek dan kata kerja." },
  { "en": "Penggunaan 'despite'?", "id": "Diikuti oleh kata benda atau gerund." },
  { "en": "Contoh kalimat kompleks?", "id": "Memiliki satu klausa independen & dependen." },
  { "en": "Contoh kalimat majemuk?", "id": "Memiliki dua klausa independen." },
  { "en": "Apa itu 'parallel structure'?", "id": "Menggunakan pola grammar yang sama." },
  { "en": "Contoh 'parallel structure'?", "id": "'I like swimming and hiking.'" },
  { "en": "Kesalahan umum 'run-on sentence'?", "id": "Dua kalimat digabung tanpa penghubung." },
  { "en": "Kesalahan umum 'sentence fragment'?", "id": "Kalimat yang tidak lengkap." },
  { "en": "Apa itu 'dangling modifier'?", "id": "Frasa yang tidak jelas menerangkan apa." },
  { "en": "Perbaiki: 'Running fast, the finish line was reached'?", "id": "'Running fast, he reached the finish line.'" },
  { "en": "Transisi untuk 'emphasis' (penekanan)?", "id": "Indeed, in fact, notably." },
  { "en": "Transisi untuk 'clarification' (klarifikasi)?", "id": "In other words, that is to say." },
  { "en": "Transisi untuk 'concession' (konsesi)?", "id": "Admittedly, while it is true." },
  { "en": "Transisi untuk 'summation' (ringkasan)?", "id": "In short, to summarize." },
  { "en": "Cara lain mengatakan 'I think'?", "id": "I believe, I would argue that." },
  { "en": "Inti dari esai yang bagus?", "id": "Argumen yang jelas dan terdukung." },
  { "en": "Inti dari jawaban Speaking yang bagus?", "id": "Jawaban terstruktur dan mudah diikuti." },
  { "en": "Inti dari Reading yang efektif?", "id": "Memahami pertanyaan, menemukan jawaban cepat." },
  { "en": "Inti dari Listening yang efektif?", "id": "Memahami gambaran besar, catat detail." },
  { "en": "Satu kesalahan terbesar saat tes?", "id": "Menghabiskan terlalu banyak waktu." },
  { "en": "Satu kebiasaan baik saat latihan?", "id": "Selalu analisis kesalahan setelahnya." },
  { "en": "Soal 'According to the passage' menanyakan?", "id": "Fakta yang tertulis jelas di teks." },
  { "en": "Soal 'The word X is closest in meaning to' menanyakan?", "id": "Sinonim kata X dalam konteks." },
  { "en": "Soal 'Why does the author mention X' menanyakan?", "id": "Tujuan atau fungsi retoris penulis." },
  { "en": "Soal 'Which of the following is NOT true' menanyakan?", "id": "Satu pernyataan salah dari empat." },
  { "en": "Soal 'The author implies that...' menanyakan?", "id": "Kesimpulan yang tidak tertulis langsung." },
  { "en": "Jawaban pengecoh (distractor) 'terlalu ekstrem'?", "id": "Menggunakan kata seperti 'always', 'never'." },
  { "en": "Jawaban pengecoh 'tidak disebutkan'?", "id": "Informasi yang terdengar logis tapi baru." },
  { "en": "Jawaban pengecoh 'di luar lingkup'?", "id": "Benar tapi tidak relevan dengan pertanyaan." },
  { "en": "Jawaban pengecoh 'kontradiktif'?", "id": "Bertentangan dengan ide utama teks." },
  { "en": "Kata kunci terpenting dalam soal?", "id": "Kata benda, kata kerja, kata sifat." },
  { "en": "Apa yang dilakukan setelah temukan kata kunci?", "id": "Scan teks untuk kata kunci tersebut." },
  { "en": "Fungsi paragraf: mengenalkan teori?", "id": "Paragraf akan menjelaskan sebuah hipotesis." },
  { "en": "Fungsi paragraf: memberikan contoh?", "id": "Paragraf akan mengilustrasikan poin sebelumnya." },
  { "en": "Fungsi paragraf: menyanggah argumen?", "id": "Paragraf akan menunjukkan kelemahan teori." },
  { "en": "Fungsi paragraf: memberikan konteks sejarah?", "id": "Paragraf akan menjelaskan latar belakang waktu." },
  { "en": "Parafrasa 'Geothermal energy is derived from the Earth's core'?", "id": "Energi panas bumi berasal dari inti Bumi." },
  { "en": "Parafrasa 'The Industrial Revolution transformed societies'?", "id": "Revolusi industri mengubah banyak masyarakat." },
  { "en": "Parafrasa 'The evidence is compelling'?", "id": "Buktinya sangat kuat dan meyakinkan." },
  { "en": "Parafrasa 'The population burgeoned'?", "id": "Populasi meningkat dengan sangat pesat." },
  { "en": "Parafrasa 'The effects are detrimental'?", "id": "Dampaknya merusak atau sangat berbahaya." },
  { "en": "Sinyal dosen akan memberi definisi?", "id": "'What we mean by X is...'" },
  { "en": "Sinyal dosen akan memberi contoh?", "id": "'Let's take the case of...'" },
  { "en": "Sinyal dosen akan menyimpulkan?", "id": "'So, in a nutshell...'" },
  { "en": "Sinyal dosen akan menunjukkan kontras?", "id": "'On the other hand...'" },
  { "en": "Sinyal dosen akan memulai topik baru?", "id": "'Okay, now let's turn to...'" },
  { "en": "Simbol catatan untuk sebuah teori?", "id": "Gunakan huruf 'T' dalam lingkaran." },
  { "en": "Simbol catatan untuk sebuah tanggal?", "id": "Gunakan angka dan garis bawahi." },
  { "en": "Simbol catatan untuk pertanyaan dosen?", "id": "Gunakan tanda tanya besar (??)." },
  { "en": "Simbol catatan untuk sebuah proses?", "id": "Gunakan angka dan panah (1->2->3)." },
  { "en": "Simbol catatan untuk nama orang?", "id": "Tulis nama, beri kotak di sekelilingnya." },
  { "en": "Mengapa dosen mengulang sebuah poin?", "id": "Untuk menekankan pentingnya poin tersebut." },
  { "en": "Implikasi dari 'I was wondering if...'?", "id": "Permintaan yang sopan dan tidak langsung." },
  { "en": "Implikasi dari 'That's one way to look at it'?", "id": "Dosen memiliki pandangan lain (tidak setuju)." },
  { "en": "Implikasi dari 'You're on the right track'?", "id": "Jawabanmu hampir benar, tapi belum." },
  { "en": "Implikasi dari 'I'm afraid I can't do that'?", "id": "Penolakan yang sopan." },
  { "en": "Tujuan mahasiswa menemui dosen?", "id": "Meminta perpanjangan waktu, klarifikasi materi." },
  { "en": "Prediksi tindakan mahasiswa setelahnya?", "id": "Akan merevisi paper, mencari sumber." },
  { "en": "Tipe soal 'Double-barreled question'?", "id": "Soal yang menanyakan dua hal sekaligus." },
  { "en": "Struktur jawaban untuk 'organization question'?", "id": "Pilih urutan logis dari penjelasan." },
  { "en": "Jenis jawaban pengecoh di Listening?", "id": "Menyebutkan kata kunci tapi konteks salah." },
  { "en": "Fungsi kalimat pertama di esai Writing?", "id": "Menarik perhatian pembaca (hook)." },
  { "en": "Fungsi kalimat kedua di esai Writing?", "id": "Memberikan konteks atau latar belakang." },
  { "en": "Fungsi kalimat terakhir di paragraf pembuka?", "id": "Thesis statement (pernyataan tesis)." },
  { "en": "Fungsi kalimat pertama di paragraf isi?", "id": "Topic sentence (kalimat topik)." },
  { "en": "Fungsi kalimat terakhir di paragraf isi?", "id": "Kalimat penutup atau transisi." },
  { "en": "Fungsi kalimat pertama di paragraf kesimpulan?", "id": "Menyatakan kembali thesis statement." },
  { "en": "Fungsi kalimat terakhir di esai?", "id": "Memberikan pemikiran akhir atau prediksi." },
  { "en": "Kesalahan logika: 'Overgeneralization'?", "id": "Membuat kesimpulan luas dari bukti minim." },
  { "en": "Kesalahan logika: 'Hasty generalization'?", "id": "Kesimpulan terburu-buru tanpa data cukup." },
  { "en": "Kesalahan logika: 'Circular reasoning'?", "id": "Argumen yang berputar-putar tanpa bukti." },
  { "en": "Frasa template untuk mengenalkan poin bacaan?", "id": "'The reading passage posits that...'" },
  { "en": "Frasa template untuk mengenalkan poin dosen?", "id": "'The lecturer, however, challenges this idea.'" },
  { "en": "Frasa template untuk menunjukkan pertentangan?", "id": "'This directly contradicts the passage's claim.'" },
  { "en": "Frasa template untuk menunjukkan dukungan?", "id": "'This example reinforces the reading's argument.'" },
  { "en": "Parafrasa dari 'The reading states that the project was a failure'?", "id": "Penulis mengklaim proyek itu gagal." },
  { "en": "Parafrasa dari 'The lecturer explains the process of erosion'?", "id": "Dosen menjelaskan bagaimana erosi terjadi." },
  { "en": "Parafrasa dari 'She is skeptical about the results'?", "id": "Dia meragukan hasil-hasil tersebut." },
  { "en": "Parafrasa dari 'He provides a counter-argument'?", "id": "Dia memberikan argumen yang berlawanan." },
  { "en": "Frasa pembuka untuk Speaking Task 4 (Lecture Summary)?", "id": "'The professor discusses the concept of...'" },
  { "en": "Frasa untuk menyebutkan poin pertama dosen?", "id": "'First, he/she explains that...'" },
  { "en": "Frasa untuk menyebutkan poin kedua dosen?", "id": "'He/she then provides an example...'" },
  { "en": "Intonasi untuk kalimat tanya?", "id": "Biasanya naik di akhir kalimat." },
  { "en": "Intonasi untuk kalimat pernyataan?", "id": "Biasanya turun di akhir kalimat." },
  { "en": "Pentingnya 'pausing' saat berbicara?", "id": "Memberi waktu berpikir, menekankan ide." },
  { "en": "Di mana harus berhenti sejenak (pause)?", "id": "Setelah koma atau di akhir ide." },
  { "en": "Apa itu 'thought group'?", "id": "Sekelompok kata yang diucapkan bersama." },
  { "en": "Transisi lanjutan untuk kontras?", "id": "Nonetheless, whereas, conversely." },
  { "en": "Transisi lanjutan untuk sebab-akibat?", "id": "Consequently, thereby, hence." },
  { "en": "Transisi lanjutan untuk penambahan?", "id": "Furthermore, moreover, in addition." },
  { "en": "Transisi lanjutan untuk penekanan?", "id": "Indeed, significantly, notably." },
  { "en": "Cara lain mengatakan 'many'?", "id": "Numerous, a multitude of, a plethora of." },
  { "en": "Cara lain mengatakan 'because of'?", "id": "Due to, owing to, on account of." },
  { "en": "Cara lain mengatakan 'but'?", "id": "However, although, yet, nevertheless." },
  { "en": "Cara lain mengatakan 'so' (akibat)?", "id": "Therefore, thus, for this reason." },
  { "en": "Cara lain mengatakan 'for example'?", "id": "For instance, to illustrate, such as." },
  { "en": "Inti dari skor Reading tinggi?", "id": "Kecepatan, akurasi, dan manajemen waktu." },
  { "en": "Inti dari skor Listening tinggi?", "id": "Fokus, catatan efektif, pemahaman implisit." },
  { "en": "Inti dari skor Speaking tinggi?", "id": "Struktur, kelancaran, dan pengembangan ide." },
  { "en": "Inti dari skor Writing tinggi?", "id": "Organisasi esai, argumen, dan grammar." },
  { "en": "Berapa lama waktu 'proofread' esai?", "id": "Sisakan setidaknya 2-3 menit." },
  { "en": "Apa yang dicari saat 'proofread'?", "id": "Kesalahan ejaan, grammar, dan tanda baca." },
  { "en": "Trik 'proofread' yang efektif?", "id": "Baca dari akhir ke awal." },
  { "en": "Sinonim kata 'paucity'?", "id": "Kekurangan, kelangkaan, jumlah sedikit." },
  { "en": "Sinonim kata 'tenuous'?", "id": "Lemah, tipis, tidak substansial." },
  { "en": "Sinonim kata 'gregarious'?", "id": "Suka bergaul, ramah." },
  { "en": "Sinonim kata 'ephemeral'?", "id": "Berlangsung singkat, sementara, fana." },
  { "en": "Sinonim kata 'proponent'?", "id": "Pendukung, penganjur, pembela." },
  { "en": "Sinonim kata 'conundrum'?", "id": "Teka-teki, masalah yang sulit." },
  { "en": "Sinonim kata 'alleviate'?", "id": "Meringankan, mengurangi rasa sakit." },
  { "en": "Sinonim kata 'lucrative'?", "id": "Menguntungkan, menghasilkan banyak uang." },
  { "en": "Arti frasa 'to shed light on'?", "id": "Menjelaskan, membuat sesuatu lebih jelas." },
  { "en": "Arti frasa 'a double-edged sword'?", "id": "Memiliki keuntungan dan kerugian sekaligus." },
  { "en": "Arti frasa 'to play devil's advocate'?", "id": "Mengambil sisi berlawanan untuk diskusi." },
  { "en": "Arti frasa 'a gray area'?", "id": "Sesuatu yang tidak jelas benar/salah." },
  { "en": "Kata: 'Inherent'?", "id": "Melekat, bawaan, tidak terpisahkan." },
  { "en": "Kata: 'Contrived'?", "id": "Dibuat-buat, tidak alami, direkayasa." },
  { "en": "Kata: 'Prolific'?", "id": "Sangat produktif, menghasilkan banyak." },
  { "en": "Kata: 'Scrutinize'?", "id": "Memeriksa dengan sangat teliti." },
  { "en": "Kata: 'Deterrent'?", "id": "Sesuatu yang mencegah atau menghalangi." },
  { "en": "Kata: 'Inconspicuous'?", "id": "Tidak menarik perhatian, tidak mencolok." },
  { "en": "Kata: 'Juxtaposition'?", "id": "Penempatan dua hal berdampingan." },
  { "en": "Kata: 'Prerequisite'?", "id": "Sesuatu yang menjadi prasyarat." },
  { "en": "Kata: 'Volatile'?", "id": "Tidak stabil, mudah berubah/menguap." },
  { "en": "Kata: 'Eminent'?", "id": "Terkenal, terkemuka, unggul." },
  { "en": "Tujuan frasa: 'granted that...'?", "id": "Mengakui sebuah poin (konsesi)." },
  { "en": "Tujuan frasa: 'correspondingly...'?", "id": "Menunjukkan hubungan yang sepadan." },
  { "en": "Tujuan frasa: 'by the same token...'?", "id": "Menunjukkan kesamaan argumen/logika." },
  { "en": "Tujuan frasa: 'to put it another way...'?", "id": "Menjelaskan ulang dengan cara berbeda." },
  { "en": "Tujuan frasa: 'this underscores...'?", "id": "Ini menekankan atau menggarisbawahi." },
  { "en": "Sikap Penulis: 'merely a catalyst'?", "id": "Hanya sebagai pemicu, bukan penyebab utama." },
  { "en": "Sikap Penulis: 'a pivotal moment'?", "id": "Momen yang sangat penting." },
  { "en": "Sikap Penulis: 'is still debated'?", "id": "Masih menjadi perdebatan (netral)." },
  { "en": "Sikap Penulis: 'an oversimplified view'?", "id": "Pandangan yang terlalu menyederhanakan (kritis)." },
  { "en": "Sikap Penulis: 'compelling evidence'?", "id": "Bukti yang sangat kuat (mendukung)." },
  { "en": "Idiom: 'to be off the hook'?", "id": "Terbebas dari masalah/tanggung jawab." },
  { "en": "Idiom: 'to be in the same boat'?", "id": "Berada dalam situasi sulit yang sama." },
  { "en": "Idiom: 'to not see the forest for the trees'?", "id": "Terlalu fokus detail, lupa gambaran besar." },
  { "en": "Idiom: 'to add insult to injury'?", "id": "Memperburuk situasi yang sudah buruk." },
  { "en": "Idiom: 'to sit on the fence'?", "id": "Tidak mau memihak, netral." },
  { "en": "Idiom: 'a blessing in disguise'?", "id": "Sesuatu yang awalnya buruk jadi baik." },
  { "en": "Idiom: 'to jump on the bandwagon'?", "id": "Ikut-ikutan tren yang sedang populer." },
  { "en": "Idiom: 'to hit the road'?", "id": "Mulai perjalanan, pergi." },
  { "en": "Idiom: 'to be a dime a dozen'?", "id": "Sesuatu yang umum dan murah." },
  { "en": "Idiom: 'to make a long story short'?", "id": "Meringkas cerita, langsung ke intinya." },
  { "en": "Sinyal Dosen: 'Let's take a step back'?", "id": "Akan melihat dari perspektif lebih luas." },
  { "en": "Sinyal Dosen: 'That's beyond the scope of this class'?", "id": "Topik itu tidak akan dibahas." },
  { "en": "Sinyal Dosen: 'Keep in mind that...'?", "id": "Menandakan informasi penting untuk diingat." },
  { "en": "Sinyal Dosen: 'To give you some context...'?", "id": "Akan memberikan informasi latar belakang." },
  { "en": "Sinyal Dosen: 'The key distinction is...'?", "id": "Akan menjelaskan perbedaan utama." },
  { "en": "Implikasi Mahasiswa: 'I didn't catch that'?", "id": "Saya tidak mendengar/mengerti." },
  { "en": "Implikasi Mahasiswa: 'Could you give an example'?", "id": "Meminta ilustrasi untuk memperjelas." },
  { "en": "Implikasi Mahasiswa: 'So, basically...'?", "id": "Mencoba mengkonfirmasi pemahamannya." },
  { "en": "Implikasi Mahasiswa: 'What about the deadline'?", "id": "Menanyakan tenggat waktu pengumpulan." },
  { "en": "Implikasi Mahasiswa: 'Is this going to be on the test'?", "id": "Menanyakan apakah materi akan diujikan." },
  { "en": "Struktur Jawaban Speaking: 'Preference'?", "id": "Pilihan, alasan 1, alasan 2." },
  { "en": "Struktur Jawaban Speaking: 'Agree/Disagree'?", "id": "Setuju/tidak, alasan 1, alasan 2." },
  { "en": "Struktur Jawaban Speaking: 'Paired Choice'?", "id": "Pilih satu, jelaskan kenapa, jelaskan kenapa tidak pilih yang lain." },
  { "en": "Struktur Jawaban Speaking: 'Hypothetical'?", "id": "Jawaban, penjelasan imajinatif, hasil." },
  { "en": "Struktur Jawaban Speaking: 'Description'?", "id": "Jelaskan objek, berikan detail, ceritakan pengalaman." },
  { "en": "Template Inti Speaking: 'I would prefer X over Y'?", "id": "Saya lebih memilih X daripada Y." },
  { "en": "Template Inti Speaking: 'There are a couple of reasons'?", "id": "Ada beberapa alasan untuk ini." },
  { "en": "Template Inti Speaking: 'For one thing...'?", "id": "Alasan yang pertama adalah..." },
  { "en": "Template Inti Speaking: 'On top of that...'?", "id": "Selain itu..." },
  { "en": "Template Inti Speaking: 'All in all, that's why...'?", "id": "Secara keseluruhan, itulah mengapa..." },
  { "en": "Template Inti Writing: 'Thesis Statement'?", "id": "I personally believe that... for two reasons." },
  { "en": "Template Inti Writing: 'Topic Sentence'?", "id": "First and foremost, one major reason is..." },
  { "en": "Template Inti Writing: 'Introduce Example'?", "id": "A compelling example of this is..." },
  { "en": "Template Inti Writing: 'Introduce Counterargument'?", "id": "It could be argued that..." },
  { "en": "Template Inti Writing: 'Rebuttal'?", "id": "However, this argument is not convincing because..." },
  { "en": "Template Inti Writing: 'Conclusion'?", "id": "In conclusion, considering these points..." },
  { "en": "Template Inti Integrated: 'Reading's Point'?", "id": "The passage claims that..." },
  { "en": "Template Inti Integrated: 'Lecture's Point'?", "id": "The lecturer, however, refutes this by stating..." },
  { "en": "Template Inti Integrated: 'Show Contradiction'?", "id": "This challenges the idea presented in the reading." },
  { "en": "Template Inti Integrated: 'Detail from Lecture'?", "id": "He/she provides the example of..." },
  { "en": "Grammar Inti: 'Subject-Verb Agreement'?", "id": "The quality of the apples is..." },
  { "en": "Grammar Inti: 'Pronoun Agreement'?", "id": "Each student must bring his or her book." },
  { "en": "Grammar Inti: 'Conditional Tense'?", "id": "If I had known, I would have come." },
  { "en": "Grammar Inti: 'Gerund after Preposition'?", "id": "He is good at solving problems." },
  { "en": "Grammar Inti: 'Parallelism in Lists'?", "id": "To study, to practice, and to review." },
  { "en": "Grammar Inti: 'Correct Comparison'?", "id": "His car is different from mine." },
  { "en": "Grammar Inti: 'Adjective vs. Adverb'?", "id": "He runs quickly (adverb)." },
  { "en": "Grammar Inti: 'Countable vs. Uncountable'?", "id": "I have less money but fewer coins." },
  { "en": "Grammar Inti: 'Reported Speech Tense Shift'?", "id": "'I am tired' becomes 'he was tired'." },
  { "en": "Grammar Inti: 'Article Use'?", "id": "The sun is a star." },
  { "en": "Pola Soal Reading: 'Inference'?", "id": "Jawaban tidak tertulis, harus disimpulkan." },
  { "en": "Pola Soal Reading: 'Main Idea'?", "id": "Cari 'thesis statement' di paragraf pertama." },
  { "en": "Pola Soal Reading: 'Detail'?", "id": "Cari jawaban di sekitar kata kunci." },
  { "en": "Pola Soal Reading: 'Purpose'?", "id": "Pahami fungsi kalimat/paragraf." },
  { "en": "Pola Soal Reading: 'Vocabulary'?", "id": "Gunakan konteks kalimat untuk menebak." },
  { "en": "Pola Soal Listening: 'Gist-Purpose'?", "id": "Pahami mengapa percakapan itu terjadi." },
  { "en": "Pola Soal Listening: 'Detail'?", "id": "Tangkap nama, angka, atau alasan spesifik." },
  { "en": "Pola Soal Listening: 'Organization'?", "id": "Kenali struktur presentasi dosen." },
  { "en": "Pola Soal Listening: 'Attitude'?", "id": "Perhatikan nada suara dan pilihan kata." },
  { "en": "Pola Soal Listening: 'Inference'?", "id": "Tarik kesimpulan dari apa yang diisyaratkan." },
  { "en": "Pola Speaking: 'Independent Task'?", "id": "Berikan opini pribadi dengan alasan." },
  { "en": "Pola Speaking: 'Integrated Task' (Read-Listen-Speak)?", "id": "Ringkas dan hubungkan bacaan & audio." },
  { "en": "Pola Writing: 'Independent Task'?", "id": "Tulis esai opini yang terstruktur." },
  { "en": "Pola Writing: 'Integrated Task' (Read-Listen-Write)?", "id": "Ringkas dan bandingkan poin bacaan & audio." },
  { "en": "Inti Reading: Mencari pronoun antecedent?", "id": "Lihat kata benda sebelum kata ganti." },
  { "en": "Inti Listening: Menangkap 'digression'?", "id": "Saat dosen menyimpang dari topik utama." },
  { "en": "Inti Speaking: 'Pacing'?", "id": "Berbicara dengan kecepatan yang stabil." },
  { "en": "Inti Writing: 'Cohesion'?", "id": "Ide-ide terhubung secara logis." },
  { "en": "Inti Writing: 'Coherence'?", "id": "Seluruh esai mudah dipahami." },
  { "en": "Inti TOEFL: Apa yang diuji?", "id": "Kemampuan menggunakan Inggris di lingkungan akademis." },
  { "en": "Kata: 'Paradigm'?", "id": "Model, pola, atau kerangka berpikir." },
  { "en": "Kata: 'Synthesis'?", "id": "Penggabungan beberapa elemen menjadi satu." },
  { "en": "Kata: 'Dichotomy'?", "id": "Pemisahan antara dua hal berlawanan." },
  { "en": "Kata: 'Empirical'?", "id": "Berdasarkan bukti dari observasi/eksperimen." },
  { "en": "Kata: 'Pragmatic'?", "id": "Praktis, berdasarkan kenyataan, bukan teori." },
  { "en": "Kata: 'Homogeneous'?", "id": "Terdiri dari jenis yang sama." },
  { "en": "Kata: 'Heterogeneous'?", "id": "Terdiri dari berbagai jenis berbeda." },
  { "en": "Kata: 'Nuance'?", "id": "Perbedaan makna atau ekspresi halus." },
  { "en": "Kata: 'Tentative'?", "id": "Tidak pasti, bersifat sementara, ragu-ragu." },
  { "en": "Kata: 'Corroborate'?", "id": "Mengkonfirmasi atau mendukung dengan bukti." },
  { "en": "Frasa: 'by virtue of'?", "id": "Menunjukkan alasan: 'karena', 'berkat'." },
  { "en": "Frasa: 'in tandem with'?", "id": "Menunjukkan kerja sama: 'bersama dengan'." },
  { "en": "Frasa: 'on the contrary'?", "id": "Menunjukkan pertentangan: 'sebaliknya'." },
  { "en": "Frasa: 'needless to say'?", "id": "Menunjukkan sesuatu yang sudah jelas." },
  { "en": "Frasa: 'that is to say'?", "id": "Menunjukkan klarifikasi: 'yaitu', 'dengan kata lain'." },
  { "en": "Distraktor Reading: 'True but not mentioned'?", "id": "Jawaban benar tapi tak ada di teks." },
  { "en": "Distraktor Reading: 'The extreme answer'?", "id": "Jawaban memakai kata 'all', 'never'." },
  { "en": "Distraktor Reading: 'The partial answer'?", "id": "Jawaban hanya benar sebagian." },
  { "en": "Distraktor Reading: 'The logical-sounding answer'?", "id": "Jawaban masuk akal tapi salah." },
  { "en": "Distraktor Reading: 'The opposite answer'?", "id": "Jawaban yang artinya kebalikan." },
  { "en": "Idiom: 'to be in limbo'?", "id": "Dalam keadaan tidak pasti." },
  { "en": "Idiom: 'to get the ball rolling'?", "id": "Memulai sesuatu (proyek/diskusi)." },
  { "en": "Idiom: 'to go back to the drawing board'?", "id": "Memulai lagi dari awal." },
  { "en": "Idiom: 'to learn the ropes'?", "id": "Mempelajari dasar-dasar suatu pekerjaan." },
  { "en": "Idiom: 'to be on the same page'?", "id": "Memiliki pemahaman yang sama." },
  { "en": "Idiom: 'a rule of thumb'?", "id": "Aturan praktis yang tidak mutlak." },
  { "en": "Idiom: 'the elephant in the room'?", "id": "Masalah besar yang semua orang hindari." },
  { "en": "Idiom: 'to touch base'?", "id": "Menghubungi seseorang secara singkat." },
  { "en": "Idiom: 'a ballpark figure'?", "id": "Angka perkiraan yang kasar." },
  { "en": "Idiom: 'to think outside the box'?", "id": "Berpikir secara kreatif dan inovatif." },
  { "en": "Sinyal Dosen: 'So, what does this all mean'?", "id": "Akan memberikan kesimpulan atau implikasi." },
  { "en": "Sinyal Dosen: 'Let's switch gears for a moment'?", "id": "Akan mengubah topik pembicaraan." },
  { "en": "Sinyal Dosen: 'This is a crucial point'?", "id": "Informasi ini sangat penting." },
  { "en": "Sinyal Dosen: 'You'll see this again'?", "id": "Materi ini kemungkinan akan diuji." },
  { "en": "Sinyal Dosen: 'The main takeaway here is...'?", "id": "Poin terpenting yang harus diingat." },
  { "en": "Implikasi Mahasiswa: 'I'm having trouble with...'?", "id": "Meminta bantuan atau saran." },
  { "en": "Implikasi Mahasiswa: 'I was hoping to...'?", "id": "Menyatakan permintaan dengan sangat sopan." },
  { "en": "Implikasi Mahasiswa: 'I'm not following you'?", "id": "Saya tidak mengerti penjelasan Anda." },
  { "en": "Implikasi Mahasiswa: 'Is it possible to...'?", "id": "Menanyakan apakah ada pengecualian." },
  { "en": "Implikasi Mahasiswa: 'What are the requirements'?", "id": "Menanyakan syarat atau kriteria tugas." },
  { "en": "Template Inti: Menyatakan Opini Kuat?", "id": "'It is my firm belief that...'" },
  { "en": "Template Inti: Menyatakan Opini Lembut?", "id": "'It seems to me that...'" },
  { "en": "Template Inti: Menunjukkan Sebab?", "id": "'This is largely due to the fact that...'" },
  { "en": "Template Inti: Menunjukkan Akibat?", "id": "'This, in turn, leads to...'" },
  { "en": "Template Inti: Spekulasi Masa Depan?", "id": "'It is conceivable that in the future...'" },
  { "en": "Template Inti: Memperkenalkan Solusi?", "id": "'A viable solution to this issue would be...'" },
  { "en": "Template Inti: Memberi Contoh Pribadi?", "id": "'From my personal experience, I can say...'" },
  { "en": "Template Inti: Menutup Paragraf?", "id": "'Thus, it is clear that...'" },
  { "en": "Template Inti: Menghubungkan Ide Serupa?", "id": "'In a similar vein,...'" },
  { "en": "Template Inti: Mengakui Kompleksitas?", "id": "'The issue is not straightforward, as...'" },
  { "en": "Template Integrated: Melaporkan Teori Bacaan?", "id": "'The passage puts forward the theory that...'" },
  { "en": "Template Integrated: Melaporkan Sanggahan Dosen?", "id": "'The professor, however, casts doubt on this.'" },
  { "en": "Template Integrated: Menyebutkan Bukti dari Dosen?", "id": "'To support this, the lecturer cites...'" },
  { "en": "Template Integrated: Kesimpulan Perbedaan?", "id": "'Thus, the lecture undermines the passage's claim.'" },
  { "en": "Template Integrated: Menjelaskan Poin Bacaan?", "id": "'According to the author,...'" },
  { "en": "Sinonim Akademis untuk 'big'?", "id": "Substantial, considerable, significant." },
  { "en": "Sinonim Akademis untuk 'new'?", "id": "Novel, innovative, contemporary." },
  { "en": "Sinonim Akademis untuk 'many'?", "id": "Numerous, a multitude of, various." },
  { "en": "Sinonim Akademis untuk 'show'?", "id": "Illustrate, demonstrate, reveal." },
  { "en": "Sinonim Akademis untuk 'use'?", "id": "Utilize, employ, leverage." },
  { "en": "Sinonim Akademis untuk 'get'?", "id": "Obtain, acquire, procure." },
  { "en": "Sinonim Akademis untuk 'say'?", "id": "State, assert, claim, argue." },
  { "en": "Sinonim Akademis untuk 'wrong'?", "id": "Flawed, erroneous, incorrect." },
  { "en": "Sinonim Akademis untuk 'right'?", "id": "Correct, valid, sound." },
  { "en": "Sinonim Akademis untuk 'about' (topik)?", "id": "Regarding, concerning, pertaining to." },
  { "en": "Pola Speaking: Jawaban terstruktur?", "id": "Poin utama -> Penjelasan -> Contoh." },
  { "en": "Pola Writing: Paragraf terstruktur?", "id": "Kalimat topik -> Penjelasan -> Bukti/Contoh." },
  { "en": "Pola Reading: Pertanyaan Detail?", "id": "Cari jawaban di baris sebelum/sesudah keyword." },
  { "en": "Pola Listening: Pertanyaan Tujuan?", "id": "Biasanya terjawab di awal percakapan." },
  { "en": "Pola Umum: Apa yang diuji?", "id": "Pemahaman dan produksi bahasa Inggris." },
  { "en": "Inti Reading: Tujuan penulis?", "id": "Menginformasikan, meyakinkan, atau membantah." },
  { "en": "Inti Listening: Hubungan antar pembicara?", "id": "Dosen-mahasiswa, penasihat-mahasiswa, sesama mahasiswa." },
  { "en": "Inti Speaking: Kunci kelancaran?", "id": "Bukan kecepatan, tapi aliran yang stabil." },
  { "en": "Inti Writing: Kunci koherensi?", "id": "Setiap kalimat terhubung logis dengan sebelumnya." },
  { "en": "Inti Soal: Bagaimana jika tidak tahu?", "id": "Tebak dengan cerdas, jangan biarkan kosong." },
  { "en": "Inti Belajar: Fokus pada apa?", "id": "Mengenali pola dan melatih kelemahan." },
  { "en": "Inti Waktu: Bagaimana menghemat waktu?", "id": "Jangan habiskan waktu pada satu soal." },
  { "en": "Inti Catatan: Apa yang paling penting?", "id": "Poin utama, bukan setiap kata." },
  { "en": "Inti Jawaban: Apakah harus kompleks?", "id": "Harus jelas dan menjawab pertanyaan." },
  { "en": "Inti Kosakata: Kuantitas atau kualitas?", "id": "Kualitas penggunaan dalam konteks." },
  { "en": "Kata: 'Myriad'?", "id": "Jumlah yang sangat banyak." },
  { "en": "Kata: 'Assert'?", "id": "Menyatakan dengan percaya diri." },
  { "en": "Kata: 'Devoid of'?", "id": "Sama sekali tidak memiliki." },
  { "en": "Kata: 'Susceptible to'?", "id": "Rentan atau mudah terpengaruh oleh." },
  { "en": "Kata: 'Precede'?", "id": "Terjadi atau ada sebelum." },
  { "en": "Kata: 'Succeed' (urutan)?", "id": "Terjadi atau ada setelah." },
  { "en": "Kata: 'Viable'?", "id": "Mampu bekerja atau berhasil." },
  { "en": "Kata: 'Spur'?", "id": "Mendorong atau memacu pertumbuhan." },
  { "en": "Kata: 'Stifle'?", "id": "Menahan atau menghentikan pertumbuhan." },
  { "en": "Kata: 'Consensus'?", "id": "Persetujuan umum atau kesepakatan." },
  { "en": "Frasa: 'a case in point'?", "id": "Contoh yang jelas dari sesuatu." },
  { "en": "Frasa: 'in light of'?", "id": "Mempertimbangkan sesuatu, karena." },
  { "en": "Frasa: 'to be contingent on'?", "id": "Tergantung pada sesuatu yang lain." },
  { "en": "Frasa: 'to fall short of'?", "id": "Gagal mencapai target atau standar." },
  { "en": "Frasa: 'to pave the way for'?", "id": "Membuka jalan untuk sesuatu." },
  { "en": "Kata: 'Ambivalent'?", "id": "Memiliki perasaan campur aduk/bertentangan." },
  { "en": "Kata: 'Perfunctory'?", "id": "Dilakukan sekilas, tanpa minat." },
  { "en": "Kata: 'Egregious'?", "id": "Sangat buruk, mengerikan, mengejutkan." },
  { "en": "Kata: 'Anachronism'?", "id": "Sesuatu yang tidak sesuai zaman." },
  { "en": "Kata: 'Exacerbate'?", "id": "Memperburuk masalah atau perasaan." },
  { "en": "Kata: 'Meticulous'?", "id": "Sangat teliti dan memperhatikan detail." },
  { "en": "Kata: 'Taciturn'?", "id": "Pendiam, tidak banyak bicara." },
  { "en": "Kata: 'Superfluous'?", "id": "Berlebihan, tidak diperlukan lagi." },
  { "en": "Kata: 'Candid'?", "id": "Jujur, terus terang, blak-blakan." },
  { "en": "Kata: 'Ephemeral'?", "id": "Hanya berlangsung untuk waktu singkat." },
  { "en": "Sinyal untuk Kontradiksi?", "id": "However, nevertheless, despite." },
  { "en": "Sinyal untuk Sebab-Akibat?", "id": "Therefore, consequently, as a result." },
  { "en": "Sinyal untuk Penambahan?", "id": "Moreover, furthermore, in addition." },
  { "en": "Sinyal untuk Contoh?", "id": "For instance, to illustrate, such as." },
  { "en": "Sinyal untuk Urutan?", "id": "Initially, subsequently, ultimately." },
  { "en": "Pola Soal: Mencari Definisi?", "id": "Temukan kata kunci, baca kalimatnya." },
  { "en": "Pola Soal: Menyimpulkan Sikap Penulis?", "id": "Perhatikan pilihan kata sifat/adverbia." },
  { "en": "Pola Soal: Memasukkan Kalimat?", "id": "Cocokkan dengan kata ganti/transisi." },
  { "en": "Pola Soal: Menyederhanakan Kalimat?", "id": "Cari subjek-predikat inti, buang detail." },
  { "en": "Pola Soal: Ringkasan Bacaan?", "id": "Pilih hanya ide-ide utama paragraf." },
  { "en": "Idiom: 'to be up in the air'?", "id": "Sesuatu yang belum diputuskan." },
  { "en": "Idiom: 'to get wind of something'?", "id": "Mendengar rumor tentang sesuatu." },
  { "en": "Idiom: 'to be in the red'?", "id": "Mengalami kerugian finansial, berhutang." },
  { "en": "Idiom: 'to be in the black'?", "id": "Mendapat keuntungan finansial." },
  { "en": "Idiom: 'a blessing in disguise'?", "id": "Sesuatu yang buruk tapi hasilnya baik." },
  { "en": "Idiom: 'to throw in the towel'?", "id": "Menyerah, berhenti berusaha." },
  { "en": "Idiom: 'to steal someone's thunder'?", "id": "Mengambil perhatian/kredit dari orang lain." },
  { "en": "Idiom: 'to let the cat out of the bag'?", "id": "Membocorkan sebuah rahasia." },
  { "en": "Idiom: 'to be on thin ice'?", "id": "Dalam situasi berisiko atau berbahaya." },
  { "en": "Idiom: 'to burn the midnight oil'?", "id": "Bekerja atau belajar hingga larut malam." },
  { "en": "Sinyal Dosen: 'Now, this is fascinating...'?", "id": "Akan menjelaskan poin yang menarik." },
  { "en": "Sinyal Dosen: 'But what about...'?", "id": "Akan mengenalkan sebuah pengecualian/masalah." },
  { "en": "Sinyal Dosen: 'Let's put this into perspective'?", "id": "Akan membandingkan dengan gambaran besar." },
  { "en": "Sinyal Dosen: 'The consensus among scholars is...'?", "id": "Akan menyatakan pendapat mayoritas ahli." },
  { "en": "Sinyal Dosen: 'This theory has been challenged'?", "id": "Akan menjelaskan kritik terhadap teori." },
  { "en": "Masalah Mahasiswa: 'I can't access the online portal'?", "id": "Masalah teknis dengan sistem universitas." },
  { "en": "Masalah Mahasiswa: 'The workload is overwhelming'?", "id": "Beban tugas atau belajar terlalu banyak." },
  { "en": "Masalah Mahasiswa: 'I have a scheduling conflict'?", "id": "Dua jadwal bentrok di waktu sama." },
  { "en": "Masalah Mahasiswa: 'I'm not sure what to focus on'?", "id": "Butuh arahan untuk tugas/ujian." },
  { "en": "Masalah Mahasiswa: 'The group project isn't working'?", "id": "Masalah dengan kerja sama tim." },
  { "en": "Template: Menyatakan Tujuan Esai?", "id": "'The purpose of this essay is to...'" },
  { "en": "Template: Mengakui Pandangan Lain?", "id": "'Admittedly, there is some merit to...'" },
  { "en": "Template: Memberi Penekanan Kuat?", "id": "'It cannot be overstated that...'" },
  { "en": "Template: Menunjukkan Hasil Logis?", "id": "'It logically follows that...'" },
  { "en": "Template: Menghubungkan ke Contoh Nyata?", "id": "'This is clearly exemplified by...'" },
  { "en": "Template: Mengajukan Pertanyaan Retoris?", "id": "'But is this always the case?'" },
  { "en": "Template: Menyatakan Kembali Tesis (Kesimpulan)?", "id": "'As has been demonstrated,...'" },
  { "en": "Template: Memberi Implikasi Masa Depan?", "id": "'Looking ahead, it is likely that...'" },
  { "en": "Template: Menawarkan Solusi Kompromi?", "id": "'A balanced approach would be to...'" },
  { "en": "Template: Menutup dengan Pernyataan Kuat?", "id": "'Ultimately, the evidence overwhelmingly supports...'" },
  { "en": "Template Integrated: Menjelaskan Fungsi Contoh Dosen?", "id": "'The professor uses this to illustrate...'" },
  { "en": "Template Integrated: Menunjukkan Bagaimana Dosen Menyanggah?", "id": "'The lecturer refutes this by pointing out...'" },
  { "en": "Template Integrated: Menyebutkan Detail dari Bacaan?", "id": "'The passage specifically mentions that...'" },
  { "en": "Template Integrated: Menyebutkan Keraguan Dosen?", "id": "'The speaker is skeptical about the reading's claim.'" },
  { "en": "Template Integrated: Merangkum Hubungan Bacaan-Dosen?", "id": "'Overall, the lecture casts doubt on the passage.'" },
  { "en": "Alternatif untuk 'important'?", "id": "Crucial, vital, essential, significant." },
  { "en": "Alternatif untuk 'interesting'?", "id": "Fascinating, compelling, intriguing." },
  { "en": "Alternatif untuk 'explains'?", "id": "Elucidates, clarifies, illustrates." },
  { "en": "Alternatif untuk 'says'?", "id": "Argues, posits, contends, asserts." },
  { "en": "Alternatif untuk 'changes' (verb)?", "id": "Alters, modifies, transforms." },
  { "en": "Alternatif untuk 'problem'?", "id": "Issue, challenge, drawback, obstacle." },
  { "en": "Alternatif untuk 'result'?", "id": "Consequence, outcome, repercussion." },
  { "en": "Alternatif untuk 'idea'?", "id": "Concept, notion, theory, perspective." },
  { "en": "Alternatif untuk 'part'?", "id": "Component, element, aspect, factor." },
  { "en": "Alternatif untuk 'different'?", "id": "Distinct, disparate, varied, diverse." },
  { "en": "Inti Reading: Menemukan ide utama?", "id": "Cari kalimat yang merangkum paragraf." },
  { "en": "Inti Listening: Menebak arti kata asing?", "id": "Dengarkan penjelasan dosen setelahnya." },
  { "en": "Inti Speaking: Menghindari jeda panjang?", "id": "Gunakan frasa pengisi seperti 'Well, let me see...'" },
  { "en": "Inti Writing: Membuat esai lebih kuat?", "id": "Gunakan contoh spesifik dan detail." },
  { "en": "Inti Umum: Kesalahan fatal?", "id": "Salah mengerti pertanyaan atau prompt." },
  { "en": "Kata: 'Inherent' vs 'Acquired'?", "id": "Bawaan versus didapat/dipelajari." },
  { "en": "Kata: 'Explicit' vs 'Implicit'?", "id": "Dinyatakan jelas versus tersirat." },
  { "en": "Kata: 'Intrinsic' vs 'Extrinsic'?", "id": "Motivasi dari dalam versus dari luar." },
  { "en": "Kata: 'Static' vs 'Dynamic'?", "id": "Tetap tidak berubah versus berubah-ubah." },
  { "en": "Kata: 'Converge' vs 'Diverge'?", "id": "Menuju satu titik versus menyebar." },
  { "en": "Kata: 'Penultimate'?", "id": "Kedua dari terakhir." },
  { "en": "Kata: 'Antecedent'?", "id": "Sesuatu yang ada/terjadi sebelumnya." },
  { "en": "Kata: 'Proponent' vs 'Opponent'?", "id": "Pendukung versus penentang." },
  { "en": "Kata: 'Precursor'?", "id": "Pendahulu, sesuatu yang datang sebelumnya." },
  { "en": "Kata: 'Ramification'?", "id": "Konsekuensi rumit yang tak terduga." },
  { "en": "Frasa: 'in the grand scheme of things'?", "id": "Dalam gambaran besar, secara keseluruhan." },
  { "en": "Frasa: 'to take with a grain of salt'?", "id": "Tidak mempercayai sesuatu sepenuhnya." },
  { "en": "Frasa: 'the tip of the iceberg'?", "id": "Bagian kecil dari masalah besar." },
  { "en": "Frasa: 'to read between the lines'?", "id": "Memahami makna yang tersirat." },
  { "en": "Frasa: 'to be on the brink of'?", "id": "Di ambang atau hampir terjadi." },
  { "en": "Kata: 'Esoteric'?", "id": "Sulit dipahami, hanya untuk kelompok kecil." },
  { "en": "Kata: 'Pervasive'?", "id": "Menyebar luas, meresap ke mana-mana." },
  { "en": "Kata: 'Juxtapose'?", "id": "Menempatkan berdampingan untuk membandingkan." },
  { "en": "Kata: 'Fortuitous'?", "id": "Terjadi secara kebetulan, beruntung." },
  { "en": "Kata: 'Amalgamate'?", "id": "Menggabungkan menjadi satu kesatuan." },
  { "en": "Kata: 'Elicit'?", "id": "Memancing atau mendapatkan reaksi/jawaban." },
  { "en": "Kata: 'Opaque'?", "id": "Tidak tembus cahaya, sulit dipahami." },
  { "en": "Kata: 'Nascent'?", "id": "Baru mulai ada, dalam tahap awal." },
  { "en": "Kata: 'Salient'?", "id": "Paling penting atau menonjol." },
  { "en": "Kata: 'Underscore'?", "id": "Menekankan, menggarisbawahi pentingnya." },
  { "en": "Frasa: 'in hindsight'?", "id": "Meninjau kembali kejadian masa lalu." },
  { "en": "Frasa: 'a myriad of'?", "id": "Jumlah yang sangat banyak dari." },
  { "en": "Frasa: 'to serve as a catalyst for'?", "id": "Menjadi pemicu untuk sesuatu." },
  { "en": "Frasa: 'a precursor to'?", "id": "Sesuatu yang mendahului hal lain." },
  { "en": "Frasa: 'to be fraught with'?", "id": "Dipenuhi dengan (masalah/risiko)." },
  { "en": "Sikap Penulis: 'a groundbreaking study'?", "id": "Sangat mendukung, menganggapnya terobosan." },
  { "en": "Sikap Penulis: 'a flawed premise'?", "id": "Sangat kritis, menganggap dasarnya salah." },
  { "en": "Sikap Penulis: 'remains ambiguous'?", "id": "Netral, menganggapnya masih belum jelas." },
  { "en": "Sikap Penulis: 'a more nuanced approach is needed'?", "id": "Kritis, menganggapnya terlalu sederhana." },
  { "en": "Sikap Penulis: 'The data suggests...'?", "id": "Hati-hati, berdasarkan data (objektif)." },
  { "en": "Idiom: 'to be back to square one'?", "id": "Harus memulai lagi dari awal." },
  { "en": "Idiom: 'a drop in the bucket'?", "id": "Jumlah sangat kecil, tidak signifikan." },
  { "en": "Idiom: 'to have a chip on your shoulder'?", "id": "Merasa marah karena perlakuan tidak adil." },
  { "en": "Idiom: 'to be in the driver's seat'?", "id": "Memegang kendali atas situasi." },
  { "en": "Idiom: 'to cut to the chase'?", "id": "Langsung ke inti masalah." },
  { "en": "Idiom: 'to face the music'?", "id": "Menerima konsekuensi dari perbuatan." },
  { "en": "Idiom: 'to go down in flames'?", "id": "Gagal total secara spektakuler." },
  { "en": "Idiom: 'the icing on the cake'?", "id": "Sesuatu yang baik di atas situasi baik." },
  { "en": "Idiom: 'to be on the same wavelength'?", "id": "Berpikir dengan cara yang sama." },
  { "en": "Idiom: 'to play it by ear'?", "id": "Melakukan sesuatu tanpa rencana." },
  { "en": "Sinyal Dosen: 'Now, let's dissect this further'?", "id": "Akan menganalisis dengan lebih mendalam." },
  { "en": "Sinyal Dosen: 'This is counterintuitive, but...'?", "id": "Akan menjelaskan sesuatu yang berlawanan logika." },
  { "en": "Sinyal Dosen: 'Let's not get bogged down in details'?", "id": "Ajak fokus pada gambaran besar." },
  { "en": "Sinyal Dosen: 'The overarching theme here is...'?", "id": "Akan menyatakan tema atau ide utama." },
  { "en": "Sinyal Dosen: 'Fast forward to the 20th century...'?", "id": "Melompat ke periode waktu lain." },
  { "en": "Implikasi Mahasiswa: 'I'm a bit overwhelmed'?", "id": "Merasa beban tugas terlalu berat." },
  { "en": "Implikasi Mahasiswa: 'Can you clarify the scope'?", "id": "Meminta batasan tugas yang jelas." },
  { "en": "Implikasi Mahasiswa: 'Is there a rubric for this'?", "id": "Menanyakan kriteria penilaian tugas." },
  { "en": "Implikasi Mahasiswa: 'I'm struggling to find sources'?", "id": "Kesulitan mencari bahan referensi." },
  { "en": "Implikasi Mahasiswa: 'What's the format for citation'?", "id": "Menanyakan gaya sitasi yang digunakan." },
  { "en": "Template: Memulai paragraf dengan pertanyaan?", "id": "'So, what are the implications of this?'" },
  { "en": "Template: Menunjukkan hubungan sebab-akibat langsung?", "id": "'This directly results in...'" },
  { "en": "Template: Membandingkan dua teori?", "id": "'Theory A posits X, whereas Theory B suggests Y.'" },
  { "en": "Template: Menunjukkan sebuah ironi?", "id": "'Ironically, the solution itself created...'" },
  { "en": "Template: Menyatakan pentingnya topik?", "id": "'Understanding this is crucial because...'" },
  { "en": "Template: Memberi peringatan?", "id": "'However, one must be cautious because...'" },
  { "en": "Template: Merujuk ke penelitian?", "id": "'Recent studies have shown that...'" },
  { "en": "Template: Menunjukkan konsensus umum?", "id": "'It is widely accepted that...'" },
  { "en": "Template: Menunjukkan pergeseran paradigma?", "id": "'This marked a shift in thinking about...'" },
  { "en": "Template: Memperkenalkan sebuah definisi?", "id": "'X can be defined as...'" },
  { "en": "Template Integrated: Menyatakan dosen setuju sebagian?", "id": "'The lecturer agrees with the reading on...'" },
  { "en": "Template Integrated: Menyatakan dosen menambah detail?", "id": "'The professor provides additional details about...'" },
  { "en": "Template Integrated: Menyatakan dosen memberi penjelasan alternatif?", "id": "'The speaker offers an alternative explanation for...'" },
  { "en": "Template Integrated: Mengutip sumber daya tarik?", "id": "'The reading's argument is based on...'" },
  { "en": "Template Integrated: Mengutip sumber dari dosen?", "id": "'The lecturer's point relies on...'" },
  { "en": "Alternatif untuk 'think about'?", "id": "Consider, contemplate, ponder." },
  { "en": "Alternatif untuk 'more and more'?", "id": "Increasingly, progressively." },
  { "en": "Alternatif untuk 'find out'?", "id": "Discover, ascertain, determine." },
  { "en": "Alternatif untuk 'deal with'?", "id": "Address, handle, manage." },
  { "en": "Alternatif untuk 'a lot of'?", "id": "A significant amount, a plethora of." },
  { "en": "Alternatif untuk 'in the end'?", "id": "Ultimately, eventually, in the long run." },
  { "en": "Alternatif untuk 'I think'?", "id": "I contend that, I would argue that." },
  { "en": "Alternatif untuk 'make sure'?", "id": "Ensure, verify, confirm." },
  { "en": "Alternatif untuk 'start'?", "id": "Commence, initiate, embark on." },
  { "en": "Alternatif untuk 'stop'?", "id": "Cease, halt, terminate." },
  { "en": "Inti Reading: Tujuan pertanyaan 'inference'?", "id": "Menguji pemahaman makna tersirat." },
  { "en": "Inti Listening: Kunci 'note-taking'?", "id": "Gunakan simbol, jangan tulis kalimat penuh." },
  { "en": "Inti Speaking: Elemen penilaian utama?", "id": "Kelancaran, koherensi, dan pengembangan topik." },
  { "en": "Inti Writing: Kesalahan paling fatal?", "id": "Tidak menjawab prompt (off-topic)." },
  { "en": "Inti Umum: Cara terbaik berlatih?", "id": "Simulasi tes penuh dalam kondisi nyata." },
  { "en": "Kata: 'Axiom'?", "id": "Pernyataan yang diterima sebagai kebenaran." },
  { "en": "Kata: 'Capitulate'?", "id": "Menyerah, mengalah pada lawan." },
  { "en": "Kata: 'Didactic'?", "id": "Bertujuan untuk mengajar atau mendidik." },
  { "en": "Kata: 'Iconoclast'?", "id": "Orang yang menyerang keyakinan umum." },
  { "en": "Kata: 'Laudable'?", "id": "Patut dipuji, terpuji." },
  { "en": "Kata: 'Ostentatious'?", "id": "Suka pamer, mencolok, berlebihan." },
  { "en": "Kata: 'Quixotic'?", "id": "Sangat idealis tapi tidak praktis." },
  { "en": "Kata: 'Soporific'?", "id": "Menyebabkan kantuk atau tidur." },
  { "en": "Kata: 'Truncate'?", "id": "Memotong, memperpendek dengan memotong." },
  { "en": "Kata: 'Zealot'?", "id": "Orang yang fanatik pada keyakinan." },
  { "en": "Frasa: 'a slippery slope'?", "id": "Langkah awal menuju serangkaian masalah." },
  { "en": "Frasa: 'to be on cloud nine'?", "id": "Merasa sangat bahagia atau gembira." },
  { "en": "Frasa: 'a litmus test'?", "id": "Tes yang menentukan untuk sesuatu." },
  { "en": "Frasa: 'to take something at face value'?", "id": "Menerima sesuatu apa adanya." },
  { "en": "Frasa: 'to have a vested interest'?", "id": "Memiliki kepentingan pribadi dalam sesuatu." },
  { "en": "Inti Reading: Cara menebak kata?", "id": "Lihat awalan, akhiran, dan akar kata." },
  { "en": "Inti Listening: Pentingnya prediksi?", "id": "Membantu otak fokus pada informasi relevan." },
  { "en": "Inti Speaking: Bagaimana memulai dengan kuat?", "id": "Langsung jawab pertanyaan, jangan bertele-tele." },
  { "en": "Inti Writing: Apa itu 'unity'?", "id": "Setiap kalimat mendukung satu ide utama." },
  { "en": "Inti Belajar: Apa itu 'active recall'?", "id": "Mengingat informasi tanpa melihat catatan." },
  { "en": "Kata: 'Ubiquitous'?", "id": "Ada di mana-mana, dijumpai di semua tempat." },
  { "en": "Kata: 'Tacit'?", "id": "Dipahami tanpa diucapkan, tersirat." },
  { "en": "Kata: 'Cognizant'?", "id": "Sadar atau mengetahui sesuatu." },
  { "en": "Kata: 'Abate'?", "id": "Berkurang, mereda, menurun kekuatannya." },
  { "en": "Kata: 'Cacophony'?", "id": "Suara yang keras dan tidak harmonis." },
  { "en": "Kata: 'Debacle'?", "id": "Kegagalan total yang memalukan." },
  { "en": "Kata: 'Facetious'?", "id": "Tidak serius, sering bercanda." },
  { "en": "Kata: 'Idiosyncrasy'?", "id": "Ciri khas atau kebiasaan unik." },
  { "en": "Kata: 'Magnanimous'?", "id": "Berjiwa besar, murah hati, pemaaf." },
  { "en": "Kata: 'Nefarious'?", "id": "Jahat, tidak bermoral, keji." },
  { "en": "Frasa: 'a stark contrast'?", "id": "Perbedaan yang sangat jelas." },
  { "en": "Frasa: 'to serve as a proxy for'?", "id": "Menjadi perwakilan atau ukuran untuk." },
  { "en": "Frasa: 'in accordance with'?", "id": "Sesuai dengan (aturan/prinsip)." },
  { "en": "Frasa: 'to be predicated on'?", "id": "Didasarkan pada suatu ide/asumsi." },
  { "en": "Frasa: 'a confluence of factors'?", "id": "Pertemuan atau gabungan beberapa faktor." },
  { "en": "Sikap Penulis: 'a long-overdue reform'?", "id": "Sangat mendukung, menganggapnya sudah seharusnya." },
  { "en": "Sikap Penulis: 'an untenable position'?", "id": "Kritis, menganggap posisi tidak dapat dipertahankan." },
  { "en": "Sikap Penulis: 'The implications are far-reaching'?", "id": "Menekankan pentingnya dampak luas." },
  { "en": "Sikap Penulis: 'a specious argument'?", "id": "Kritis, menganggap argumennya tampak benar tapi salah." },
  { "en": "Sikap Penulis: 'This is well-documented'?", "id": "Menyatakan fakta dengan keyakinan." },
  { "en": "Idiom: 'to be at a crossroads'?", "id": "Di titik harus membuat keputusan penting." },
  { "en": "Idiom: 'to have your hands full'?", "id": "Sangat sibuk dengan banyak pekerjaan." },
  { "en": "Idiom: 'a quantum leap'?", "id": "Kemajuan atau peningkatan yang sangat besar." },
  { "en": "Idiom: 'a litmus test'?", "id": "Ujian penentu untuk suatu hal." },
  { "en": "Idiom: 'to be out of your depth'?", "id": "Dalam situasi yang terlalu sulit." },
  { "en": "Idiom: 'to go against the grain'?", "id": "Melakukan sesuatu yang berlawanan kebiasaan." },
  { "en": "Idiom: 'to be on the back burner'?", "id": "Menjadi prioritas rendah untuk sementara." },
  { "en": "Idiom: 'a steep learning curve'?", "id": "Sesuatu yang sulit dipelajari cepat." },
  { "en": "Idiom: 'to throw caution to the wind'?", "id": "Mengambil risiko, bertindak tanpa hati-hati." },
  { "en": "Idiom: 'to be in a nutshell'?", "id": "Secara singkat, pada intinya." },
  { "en": "Sinyal Dosen: 'This will be on the final'?", "id": "Materi ini pasti akan diujikan." },
  { "en": "Sinyal Dosen: 'Let's table this for now'?", "id": "Akan menunda diskusi topik ini." },
  { "en": "Sinyal Dosen: 'To play devil's advocate...'?", "id": "Akan mengambil sisi argumen berlawanan." },
  { "en": "Sinyal Dosen: 'That's a bit of a tangent'?", "id": "Pembicaraan mulai menyimpang dari topik." },
  { "en": "Sinyal Dosen: 'The crux of the matter is...'?", "id": "Akan menyatakan inti permasalahan." },
  { "en": "Implikasi Mahasiswa: 'I'm not sure I follow'?", "id": "Saya tidak mengerti alur logikanya." },
  { "en": "Implikasi Mahasiswa: 'Can we go over that one more time'?", "id": "Meminta pengulangan penjelasan." },
  { "en": "Implikasi Mahasiswa: 'What's the best way to prepare'?", "id": "Meminta saran belajar untuk ujian." },
  { "en": "Implikasi Mahasiswa: 'Is attendance mandatory'?", "id": "Menanyakan apakah kehadiran itu wajib." },
  { "en": "Implikasi Mahasiswa: 'Do you offer extra credit'?", "id": "Menanyakan adanya tugas nilai tambah." },
  { "en": "Template: Menunjukkan Keterbatasan Studi?", "id": "'A limitation of this study is...'" },
  { "en": "Template: Menyarankan Penelitian Lanjutan?", "id": "'Further research is needed to determine...'" },
  { "en": "Template: Menyatakan Persetujuan dengan Kualifikasi?", "id": "'I agree, but only to a certain extent.'" },
  { "en": "Template: Menghubungkan Bukti ke Klaim?", "id": "'This evidence substantiates the claim that...'" },
  { "en": "Template: Membuat Perbandingan Langsung?", "id": "'When compared to X, Y is clearly...'" },
  { "en": "Template: Menunjukkan Pengecualian Aturan?", "id": "'An exception to this rule is...'" },
  { "en": "Template: Memperkenalkan Perspektif Sejarah?", "id": "'Historically, this has been viewed as...'" },
  { "en": "Template: Menyatakan Implikasi Praktis?", "id": "'In practical terms, this means...'" },
  { "en": "Template: Menolak Argumen Lawan?", "id": "'This line of reasoning is flawed.'" },
  { "en": "Template: Memulai Paragraf Tubuh?", "id": "'To begin with, the primary argument is...'" },
  { "en": "Template Integrated: Melaporkan Detail Spesifik Dosen?", "id": "'The lecturer points to a study on...'" },
  { "en": "Template Integrated: Menyatakan Keraguan Dosen?", "id": "'The speaker questions the validity of...'" },
  { "en": "Template Integrated: Menjelaskan Bagaimana Bacaan Salah?", "id": "'The passage fails to consider that...'" },
  { "en": "Template Integrated: Menjelaskan Bagaimana Dosen Benar?", "id": "'The professor's example demonstrates why...'" },
  { "en": "Template Integrated: Merangkum Konflik Utama?", "id": "'The core of the disagreement lies in...'" },
  { "en": "Alternatif untuk 'important' (konteks mendesak)?", "id": "Imperative, critical, urgent." },
  { "en": "Alternatif untuk 'because'?", "id": "Since, as, given that." },
  { "en": "Alternatif untuk 'but'?", "id": "Yet, however, nevertheless." },
  { "en": "Alternatif untuk 'and'?", "id": "Moreover, additionally, as well as." },
  { "en": "Alternatif untuk 'if'?", "id": "Provided that, assuming that." },
  { "en": "Alternatif untuk 'very'?", "id": "Extremely, highly, remarkably." },
  { "en": "Alternatif untuk 'good' (kualitas)?", "id": "Excellent, superior, high-quality." },
  { "en": "Alternatif untuk 'bad' (dampak)?", "id": "Detrimental, adverse, harmful." },
  { "en": "Alternatif untuk 'big' (skala)?", "id": "Vast, extensive, large-scale." },
  { "en": "Alternatif untuk 'small' (jumlah)?", "id": "Minimal, negligible, minuscule." },
  { "en": "Inti Reading: Menemukan detail pendukung?", "id": "Cari frasa 'for example', 'such as'." },
  { "en": "Inti Listening: Menangkap tujuan pembicara?", "id": "Tanyakan: 'Mengapa dia mengatakan ini?'" },
  { "en": "Inti Speaking: Mengembangkan jawaban?", "id": "Jawab 'mengapa' dan 'bagaimana' dari opinimu." },
  { "en": "Inti Writing: Membuat tesis kuat?", "id": "Harus spesifik dan bisa diperdebatkan." },
  { "en": "Inti Umum: Kesalahan persiapan?", "id": "Hanya fokus pada satu jenis skill." },
  { "en": "Kata: 'Albeit'?", "id": "Meskipun, biarpun, walaupun." },
  { "en": "Kata: 'Bolster'?", "id": "Memperkuat, mendukung, menyokong." },
  { "en": "Kata: 'Concede'?", "id": "Mengakui sesuatu itu benar (dengan enggan)." },
  { "en": "Kata: 'Delineate'?", "id": "Menggambarkan atau menjelaskan secara rinci." },
  { "en": "Kata: 'Garish'?", "id": "Terlalu mencolok, norak." },
  { "en": "Kata: 'Impetus'?", "id": "Dorongan atau motivasi untuk sesuatu." },
  { "en": "Kata: 'Lackluster'?", "id": "Tidak bersemangat, kusam, kurang bersinar." },
  { "en": "Kata: 'Plethora'?", "id": "Jumlah yang sangat berlebihan." },
  { "en": "Kata: 'Rhetoric'?", "id": "Seni berbicara efektif (terkadang kosong)." },
  { "en": "Kata: 'Vex'?", "id": "Membuat kesal, jengkel, atau khawatir." },
  { "en": "Frasa: 'a foregone conclusion'?", "id": "Hasil yang sudah pasti." },
  { "en": "Frasa: 'to be in vogue'?", "id": "Sedang populer atau menjadi mode." },
  { "en": "Frasa: 'a moot point'?", "id": "Poin yang tidak relevan lagi." },
  { "en": "Frasa: 'par for the course'?", "id": "Sesuatu yang normal atau diharapkan." },
  { "en": "Frasa: 'to rest on your laurels'?", "id": "Berpuas diri dengan kesuksesan masa lalu." },
  { "en": "Inti Reading: Membedakan fakta dan opini?", "id": "Fakta dapat diverifikasi, opini tidak." },
  { "en": "Inti Listening: Kunci sukses percakapan?", "id": "Pahami masalah dan solusi yang didiskusikan." },
  { "en": "Inti Speaking: Cara terdengar alami?", "id": "Gunakan kontraksi (don't, it's)." },
  { "en": "Inti Writing: Apa itu 'cohesion'?", "id": "Kalimat dan paragraf mengalir lancar." },
  { "en": "Inti Belajar: Apa itu 'spaced repetition'?", "id": "Mengulang materi dengan jeda waktu." },
  { "en": "Kata: 'Auspicious'?", "id": "Menunjukkan pertanda baik, menjanjikan." },
  { "en": "Kata: 'Belligerent'?", "id": "Suka berperang, agresif, bermusuhan." },
  { "en": "Kata: 'Circumvent'?", "id": "Menghindari (masalah/aturan) dengan cerdik." },
  { "en": "Kata: 'Deleterious'?", "id": "Merusak, berbahaya, merugikan." },
  { "en": "Kata: 'Enervate'?", "id": "Melemahkan, mengurangi kekuatan." },
  { "en": "Kata: 'Fastidious'?", "id": "Sangat teliti, pemilih, sulit dipuaskan." },
  { "en": "Kata: 'Garrulous'?", "id": "Sangat cerewet, banyak bicara." },
  { "en": "Kata: 'Harbinger'?", "id": "Pertanda atau sinyal akan datangnya sesuatu." },
  { "en": "Kata: 'Incumbent'?", "id": "Pihak yang sedang menjabat." },
  { "en": "Kata: 'Languid'?", "id": "Lesu, lemah, tidak bertenaga." },
  { "en": "Frasa: 'an integral part of'?", "id": "Bagian yang sangat penting." },
  { "en": "Frasa: 'to be contingent upon'?", "id": "Tergantung pada suatu syarat." },
  { "en": "Frasa: 'in stark contrast to'?", "id": "Sangat kontras atau berlawanan dengan." },
  { "en": "Frasa: 'to give credence to'?", "id": "Membuat sesuatu tampak dapat dipercaya." },
  { "en": "Frasa: 'a testament to'?", "id": "Bukti nyata dari sesuatu." },
  { "en": "Sikap Penulis: 'a paradigm shift'?", "id": "Menekankan perubahan fundamental yang besar." },
  { "en": "Sikap Penulis: 'a cursory glance'?", "id": "Kritis, menganggap analisisnya tidak mendalam." },
  { "en": "Sikap Penulis: 'an erroneous assumption'?", "id": "Kritis, menganggap asumsinya keliru." },
  { "en": "Sikap Penulis: 'This is corroborated by...'?", "id": "Menyajikan bukti pendukung yang kuat." },
  { "en": "Sikap Penulis: 'The findings are paradoxical'?", "id": "Menunjukkan hasil yang tampak kontradiktif." },
  { "en": "Idiom: 'a double-edged sword'?", "id": "Memiliki sisi positif dan negatif." },
  { "en": "Idiom: 'to be in a bind'?", "id": "Berada dalam situasi yang sulit." },
  { "en": "Idiom: 'to have a change of heart'?", "id": "Mengubah pikiran atau perasaan." },
  { "en": "Idiom: 'to keep someone in the loop'?", "id": "Selalu memberi informasi terbaru." },
  { "en": "Idiom: 'to be on the fence'?", "id": "Ragu-ragu, belum bisa memutuskan." },
  { "en": "Idiom: 'to spill the beans'?", "id": "Membocorkan rahasia secara tidak sengaja." },
  { "en": "Idiom: 'to take something with a grain of salt'?", "id": "Tidak percaya sesuatu sepenuhnya." },
  { "en": "Idiom: 'to get something off your chest'?", "id": "Mengungkapkan sesuatu yang mengganggu." },
  { "en": "Idiom: 'to be out of the woods'?", "id": "Terbebas dari bahaya atau kesulitan." },
  { "en": "Idiom: 'to draw a blank'?", "id": "Tidak bisa mengingat sesuatu." },
  { "en": "Sinyal Dosen: 'Let's take a closer look'?", "id": "Akan membahas detail suatu topik." },
  { "en": "Sinyal Dosen: 'This is a common pitfall'?", "id": "Akan memperingatkan tentang kesalahan umum." },
  { "en": "Sinyal Dosen: 'There are several schools of thought'?", "id": "Akan menjelaskan berbagai sudut pandang." },
  { "en": "Sinyal Dosen: 'For the sake of argument...'?", "id": "Akan membuat sebuah asumsi sementara." },
  { "en": "Sinyal Dosen: 'Does that make sense'?", "id": "Memeriksa pemahaman para mahasiswa." },
  { "en": "Implikasi Mahasiswa: 'I'm a little confused about...'?", "id": "Meminta klarifikasi pada bagian spesifik." },
  { "en": "Implikasi Mahasiswa: 'Could you spell that name out'?", "id": "Meminta ejaan nama yang disebutkan." },
  { "en": "Implikasi Mahasiswa: 'Will this be graded on a curve'?", "id": "Menanyakan sistem penilaian relatif." },
  { "en": "Implikasi Mahasiswa: 'What's the weight of the final exam'?", "id": "Menanyakan persentase nilai ujian akhir." },
  { "en": "Implikasi Mahasiswa: 'I have a prior commitment'?", "id": "Sudah punya janji/acara lain." },
  { "en": "Template: Menyatakan Asumsi?", "id": "'Assuming that X is true, then...'" },
  { "en": "Template: Menunjukkan Signifikansi?", "id": "'What is most significant is...'" },
  { "en": "Template: Memperkenalkan Topik Esai?", "id": "'This essay will examine the issue of...'" },
  { "en": "Template: Menjelaskan Struktur Esai?", "id": "'The first section will discuss...'" },
  { "en": "Template: Membuat Generalisasi Hati-hati?", "id": "'In many cases, it tends to be...'" },
  { "en": "Template: Menunjukkan Kekurangan Argumen?", "id": "'The primary weakness in this argument is...'" },
  { "en": "Template: Menghubungkan ke Isu Lebih Luas?", "id": "'This reflects a broader trend of...'" },
  { "en": "Template: Menyatakan Kembali Argumen Utama?", "id": "'To reiterate the main point,...'" },
  { "en": "Template: Memberi Pernyataan Akhir?", "id": "'Ultimately, it is a matter of...'" },
  { "en": "Template: Menunjukkan Akibat yang Tak Terhindarkan?", "id": "'Inevitably, this leads to...'" },
  { "en": "Template Integrated: Melaporkan Keraguan Dosen?", "id": "'The lecturer questions whether...'" },
  { "en": "Template Integrated: Menjelaskan Contoh Balasan?", "id": "'He offers a counterexample of...'" },
  { "en": "Template Integrated: Menunjukkan Data Berbeda?", "id": "'The speaker presents data that contradicts...'" },
  { "en": "Template Integrated: Mengutip Langsung (dengan parafrasa)?", "id": "'The author's claim that...is challenged.'" },
  { "en": "Template Integrated: Menjelaskan Implikasi dari Sanggahan?", "id": "'This suggests the reading's conclusion is flawed.'" },
  { "en": "Alternatif untuk 'also'?", "id": "Furthermore, moreover, in addition." },
  { "en": "Alternatif untuk 'so' (akibat)?", "id": "Therefore, consequently, hence." },
  { "en": "Alternatif untuk 'for example'?", "id": "To illustrate, for instance." },
  { "en": "Alternatif untuk 'a lot of'?", "id": "A plethora of, numerous, myriad." },
  { "en": "Alternatif untuk 'in conclusion'?", "id": "To sum up, in summary, ultimately." },
  { "en": "Alternatif untuk 'important'?", "id": "Vital, crucial, pivotal, essential." },
  { "en": "Alternatif untuk 'bad'?", "id": "Detrimental, adverse, negative." },
  { "en": "Alternatif untuk 'good'?", "id": "Beneficial, advantageous, favorable." },
  { "en": "Alternatif untuk 'big'?", "id": "Considerable, substantial, significant." },
  { "en": "Alternatif untuk 'show'?", "id": "Demonstrate, illustrate, indicate." },
  { "en": "Inti Reading: Tujuan pertanyaan 'author's purpose'?", "id": "Memahami mengapa penulis menulis bacaan." },
  { "en": "Inti Listening: Menangkap 'sarcasm'?", "id": "Perhatikan nada yang berlawanan makna." },
  { "en": "Inti Speaking: Pentingnya 'pacing'?", "id": "Menjaga ritme bicara agar jelas." },
  { "en": "Inti Writing: Apa itu 'thesis-driven'?", "id": "Setiap bagian esai mendukung tesis." },
  { "en": "Inti Umum: Sikap mental terbaik?", "id": "Tenang, fokus, dan percaya diri." },
  { "en": "Kata: 'Abjure'?", "id": "Meninggalkan keyakinan, menolak." },
  { "en": "Kata: 'Blandishment'?", "id": "Bujukan atau sanjungan manis." },
  { "en": "Kata: 'Coalesce'?", "id": "Menyatu, bergabung menjadi satu." },
  { "en": "Kata: 'Diaphanous'?", "id": "Tipis, ringan, tembus pandang." },
  { "en": "Kata: 'Exigent'?", "id": "Mendesak, membutuhkan tindakan segera." },
  { "en": "Kata: 'Fatuous'?", "id": "Bodoh, konyol, tidak masuk akal." },
  { "en": "Kata: 'Hegemony'?", "id": "Dominasi atau kepemimpinan satu kelompok." },
  { "en": "Kata: 'Inimical'?", "id": "Merugikan, menghalangi, atau bermusuhan." },
  { "en": "Kata: 'Maelstrom'?", "id": "Situasi kacau, pusaran air besar." },
  { "en": "Kata: 'Pellucid'?", "id": "Sangat jernih, mudah dipahami." },
  { "en": "Frasa: 'a fait accompli'?", "id": "Sesuatu yang sudah terjadi." },
  { "en": "Frasa: 'to be in arrears'?", "id": "Terlambat membayar hutang." },
  { "en": "Frasa: 'a cause célèbre'?", "id": "Isu yang menarik perhatian publik." },
  { "en": "Frasa: 'de facto'?", "id": "Pada kenyataannya, meskipun tidak resmi." },
  { "en": "Frasa: 'in situ'?", "id": "Di lokasi aslinya." },
  { "en": "Inti Reading: Menebak arti dari struktur?", "id": "Lihat kata sambung (and, but, so)." },
  { "en": "Inti Listening: Mengapa pengulangan itu penting?", "id": "Menandakan konsep kunci atau jawaban." },
  { "en": "Inti Speaking: Cara menambah detail?", "id": "Jawab pertanyaan 'siapa, apa, kapan, di mana'." },
  { "en": "Inti Writing: Kunci paragraf yang baik?", "id": "Satu ide utama per paragraf." },
  { "en": "Inti Belajar: Apa itu 'interleaving'?", "id": "Mempelajari beberapa topik secara bergantian." },
  { "en": "Kata: 'Abstruse'?", "id": "Sulit dimengerti, rumit, mendalam." },
  { "en": "Kata: 'Bonhomie'?", "id": "Sifat ramah tamah yang bersahabat." },
  { "en": "Kata: 'Circumlocution'?", "id": "Penggunaan kata-kata yang berbelit-belit." },
  { "en": "Kata: 'Desultory'?", "id": "Tidak teratur, acak, tanpa tujuan." },
  { "en": "Kata: 'Equanimity'?", "id": "Ketenangan mental, terutama saat sulit." },
  { "en": "Kata: 'Foment'?", "id": "Menghasut, memicu, atau membangkitkan (masalah)." },
  { "en": "Kata: 'Germane'?", "id": "Relevan atau berhubungan dengan suatu subjek." },
  { "en": "Kata: 'Iconoclast'?", "id": "Orang yang menyerang keyakinan tradisional." },
  { "en": "Kata: 'Jettison'?", "id": "Membuang atau menyingkirkan sesuatu." },
  { "en": "Kata: 'Mendacious'?", "id": "Suka berbohong, tidak jujur." },
  { "en": "Frasa: 'a tacit assumption'?", "id": "Asumsi yang tidak diucapkan." },
  { "en": "Frasa: 'an empirical study'?", "id": "Studi yang berdasarkan data lapangan." },
  { "en": "Frasa: 'a symbiotic relationship'?", "id": "Hubungan saling menguntungkan." },
  { "en": "Frasa: 'a causal link'?", "id": "Hubungan sebab dan akibat." },
  { "en": "Frasa: 'to be mutually exclusive'?", "id": "Tidak bisa terjadi pada saat bersamaan." },
  { "en": "Sikap Penulis: 'a panacea for...'?", "id": "Kritis, menyiratkan itu bukan solusi." },
  { "en": "Sikap Penulis: 'a promising avenue of research'?", "id": "Optimis, mendukung penelitian lebih lanjut." },
  { "en": "Sikap Penulis: 'This is predicated on the assumption...'?", "id": "Kritis, menyoroti dasar argumen." },
  { "en": "Sikap Penulis: 'an overreaching conclusion'?", "id": "Kritis, menganggap kesimpulan berlebihan." },
  { "en": "Sikap Penulis: 'a watershed moment'?", "id": "Menekankan sebuah titik balik penting." },
  { "en": "Idiom: 'the acid test'?", "id": "Tes yang membuktikan nilai sesuatu." },
  { "en": "Idiom: 'to be at loggerheads'?", "id": "Berada dalam perselisihan atau konflik kuat." },
  { "en": "Idiom: 'to have an axe to grind'?", "id": "Memiliki agenda atau motif pribadi." },
  { "en": "Idiom: 'to bite the dust'?", "id": "Gagal, hancur, atau mati." },
  { "en": "Idiom: 'to burn your bridges'?", "id": "Merusak hubungan secara permanen." },
  { "en": "Idiom: 'to chew the fat'?", "id": "Mengobrol santai untuk waktu lama." },
  { "en": "Idiom: 'to have a field day'?", "id": "Sangat menikmati suatu kesempatan." },
  { "en": "Idiom: 'to flog a dead horse'?", "id": "Menyia-nyiakan usaha pada hal sia-sia." },
  { "en": "Idiom: 'to go off on a tangent'?", "id": "Tiba-tiba mengubah topik pembicaraan." },
  { "en": "Idiom: 'to have skin in the game'?", "id": "Memiliki kepentingan pribadi/finansial." },
  { "en": "Sinyal Dosen: 'Let's circle back to that later'?", "id": "Akan membahasnya lagi nanti." },
  { "en": "Sinyal Dosen: 'Without getting too technical...'?", "id": "Akan menjelaskan konsep sulit secara sederhana." },
  { "en": "Sinyal Dosen: 'This is the crux of the argument'?", "id": "Ini adalah inti dari argumennya." },
  { "en": "Sinyal Dosen: 'I'm playing devil's advocate here'?", "id": "Saya akan mengambil sisi berlawanan." },
  { "en": "Sinyal Dosen: 'Let me rephrase that'?", "id": "Akan mengulang dengan kata-kata berbeda." },
  { "en": "Implikasi Mahasiswa: 'I'm not following your logic'?", "id": "Tidak mengerti alur berpikir dosen." },
  { "en": "Implikasi Mahasiswa: 'What is the scope of the paper'?", "id": "Menanyakan batasan dan cakupan tugas." },
  { "en": "Implikasi Mahasiswa: 'Is group submission allowed'?", "id": "Menanyakan bolehkah tugas dikerjakan kelompok." },
  { "en": "Implikasi Mahasiswa: 'Could you elaborate on that point'?", "id": "Meminta penjelasan lebih detail." },
  { "en": "Implikasi Mahasiswa: 'I'd like to get your feedback on...'?", "id": "Meminta masukan atau umpan balik." },
  { "en": "Template: Menunjukkan Kesimpulan Tak Terhindarkan?", "id": "'The logical conclusion, therefore, is that...'" },
  { "en": "Template: Menunjukkan Area Untuk Riset Lanjut?", "id": "'An area for further study would be...'" },
  { "en": "Template: Memperkenalkan Sebuah Paradoks?", "id": "'Paradoxically, while X is true, so is Y.'" },
  { "en": "Template: Membingkai Ulang Sebuah Masalah?", "id": "'The issue is not X, but rather Y.'" },
  { "en": "Template: Menunjukkan Implikasi Luas?", "id": "'This has far-reaching implications for...'" },
  { "en": "Template: Menyatakan Sesuatu Secara Hati-hati?", "id": "'The evidence seems to suggest that...'" },
  { "en": "Template: Menegaskan Kembali Poin Penting?", "id": "'It bears repeating that...'" },
  { "en": "Template: Memulai Paragraf Badan Esai?", "id": "'One of the most compelling arguments is...'" },
  { "en": "Template: Menolak Sebuah Klaim Populer?", "id": "'Contrary to popular belief,...'" },
  { "en": "Template: Menutup Esai Secara Efektif?", "id": "'In light of this evidence, it is clear...'" },
  { "en": "Template Integrated: Melaporkan kesamaan poin?", "id": "'The lecturer echoes the point that...'" },
  { "en": "Template Integrated: Menjelaskan contoh dosen?", "id": "'The professor exemplifies this concept by...'" },
  { "en": "Template Integrated: Menunjukkan kelemahan argumen bacaan?", "id": "'The reading's argument is undermined by...'" },
  { "en": "Template Integrated: Menghubungkan dua ide?", "id": "'The lecturer connects this idea to...'" },
  { "en": "Template Integrated: Menyimpulkan hubungan?", "id": "'Thus, the lecture provides a counter-narrative.'" },
  { "en": "Alternatif untuk 'shows' (data)?", "id": "Indicates, reveals, demonstrates." },
  { "en": "Alternatif untuk 'causes'?", "id": "Leads to, results in, triggers." },
  { "en": "Alternatif untuk 'based on'?", "id": "Derived from, founded on." },
  { "en": "Alternatif untuk 'looks at'?", "id": "Examines, analyzes, investigates." },
  { "en": "Alternatif untuk 'talks about'?", "id": "Discusses, explores, addresses." },
  { "en": "Alternatif untuk 'really'?", "id": "Gunakan kata sifat yang lebih kuat." },
  { "en": "Alternatif untuk 'very important'?", "id": "Crucial, vital, paramount." },
  { "en": "Alternatif untuk 'very big'?", "id": "Immense, vast, enormous." },
  { "en": "Alternatif untuk 'very smart'?", "id": "Brilliant, intelligent, astute." },
  { "en": "Alternatif untuk 'very bad'?", "id": "Abysmal, egregious, detrimental." },
  { "en": "Inti Reading: Pola 'Classification'?", "id": "Teks mengelompokkan hal ke dalam kategori." },
  { "en": "Inti Listening: Kunci 'inference'?", "id": "Gabungkan apa yang didengar dan diketahui." },
  { "en": "Inti Speaking: Elemen 'delivery'?", "id": "Pacing, intonasi, pengucapan, dan kelancaran." },
  { "en": "Inti Writing: Apa itu 'academic tone'?", "id": "Gaya penulisan formal, objektif, dan impersonal." },
  { "en": "Inti Umum: Cara terbaik evaluasi diri?", "id": "Rekam jawaban Speaking, minta feedback esai." },
  { "en": "Kata: 'Quisling'?", "id": "Pengkhianat yang bekerja sama dengan musuh." },
  { "en": "Kata: 'Recalcitrant'?", "id": "Suka membangkang, sulit diatur." },
  { "en": "Kata: 'Salubrious'?", "id": "Bermanfaat bagi kesehatan, menyehatkan." },
  { "en": "Kata: 'Tenebrous'?", "id": "Gelap, suram, atau tidak jelas." },
  { "en": "Kata: 'Unctuous'?", "id": "Terlalu manis, berminyak, tidak tulus." },
  { "en": "Kata: 'Vicissitude'?", "id": "Perubahan nasib atau keadaan." },
  { "en": "Kata: 'Wizened'?", "id": "Keriput dan layu karena usia." },
  { "en": "Kata: 'Xenophobia'?", "id": "Ketakutan atau kebencian pada orang asing." },
  { "en": "Kata: 'Yoke'?", "id": "Sesuatu yang menindas atau membatasi." },
  { "en": "Kata: 'Zephyr'?", "id": "Angin sepoi-sepoi yang lembut." },
  { "en": "Frasa: 'ad nauseam'?", "id": "Mengulang sesuatu sampai memuakkan." },
  { "en": "Frasa: 'bona fide'?", "id": "Asli, tulus, dapat dipercaya." },
  { "en": "Frasa: 'ipso facto'?", "id": "Oleh karena fakta itu sendiri." },
  { "en": "Frasa: 'status quo'?", "id": "Keadaan yang ada sekarang." },
  { "en": "Frasa: 'terra firma'?", "id": "Darat atau tanah yang kokoh." },
  { "en": "Inti Reading: Menjawab soal 'EXCEPT'?", "id": "Temukan tiga pilihan yang ada di teks." },
  { "en": "Inti Listening: Bagaimana tetap fokus?", "id": "Visualisasikan apa yang sedang dibicarakan." },
  { "en": "Inti Speaking: Menghindari keheningan?", "id": "Parafrasakan pertanyaan jika butuh waktu." },
  { "en": "Inti Writing: Pentingnya 'outline'?", "id": "Memastikan esai terstruktur dan logis." },
  { "en": "Inti Terakhir: Kunci sukses?", "id": "Memahami pola, bukan hanya menghafal." }

        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
