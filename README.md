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


  { "en": "Apa itu sistem?", "id": "Sekumpulan komponen yang bekerja bersama." },
  { "en": "Apa itu sistem kontrol?", "id": "Sistem yang mengelola perintah atau mengatur sistem lain." },
  { "en": "Apa itu plant?", "id": "Objek atau proses fisik yang dikontrol." },
  { "en": "Apa itu input?", "id": "Sinyal atau stimulus yang masuk ke sistem." },
  { "en": "Apa itu output?", "id": "Respon aktual yang dihasilkan oleh sistem." },
  { "en": "Apa itu referensi (setpoint)?", "id": "Nilai output yang diinginkan." },
  { "en": "Apa itu gangguan (disturbance)?", "id": "Sinyal tak diinginkan yang mempengaruhi output." },
  { "en": "Apa itu sistem loop terbuka?", "id": "Aksi kontrol tidak bergantung pada output." },
  { "en": "Contoh sistem loop terbuka?", "id": "Pemanggang roti, kipas angin." },
  { "en": "Kelebihan sistem loop terbuka?", "id": "Sederhana, stabil, dan murah." },
  { "en": "Kekurangan sistem loop terbuka?", "id": "Tidak akurat, tidak bisa mengatasi gangguan." },
  { "en": "Apa itu sistem loop tertutup?", "id": "Aksi kontrol bergantung pada output sistem." },
  { "en": "Nama lain sistem loop tertutup?", "id": "Sistem kontrol umpan balik (feedback)." },
  { "en": "Contoh sistem loop tertutup?", "id": "Pendingin ruangan (AC), autopilot pesawat." },
  { "en": "Kelebihan sistem loop tertutup?", "id": "Akurat, dapat menolak gangguan." },
  { "en": "Kekurangan sistem loop tertutup?", "id": "Kompleks, mahal, potensi tidak stabil." },
  { "en": "Apa itu umpan balik (feedback)?", "id": "Mengukur output untuk dibandingkan dengan referensi." },
  { "en": "Apa itu sinyal error?", "id": "Perbedaan antara sinyal referensi dan umpan balik." },
  { "en": "Apa itu kontroler (controller)?", "id": "Komponen yang memproses error dan menghasilkan aksi." },
  { "en": "Apa itu aktuator?", "id": "Perangkat yang mengubah sinyal kontrol menjadi aksi." },
  { "en": "Contoh aktuator?", "id": "Motor listrik, katup (valve), pemanas." },
  { "en": "Apa itu sensor?", "id": "Perangkat yang mengukur variabel output." },
  { "en": "Contoh sensor?", "id": "Termometer, takometer, sensor tekanan." },
  { "en": "Apa itu umpan balik negatif?", "id": "Umpan balik yang mengurangi sinyal error." },
  { "en": "Tujuan umpan balik negatif?", "id": "Meningkatkan stabilitas dan akurasi sistem." },
  { "en": "Apa itu umpan balik positif?", "id": "Umpan balik yang memperkuat sinyal error." },
  { "en": "Efek umpan balik positif?", "id": "Menyebabkan ketidakstabilan atau osilasi." },
  { "en": "Apa itu sistem SISO?", "id": "Single-Input, Single-Output." },
  { "en": "Apa itu sistem MIMO?", "id": "Multiple-Input, Multiple-Output." },
  { "en": "Apa itu sistem LTI (Linear Time-Invariant)?", "id": "Sistem linear yang perilakunya tidak berubah." },
  { "en": "Apa itu linearitas?", "id": "Mematuhi prinsip superposisi dan homogenitas." },
  { "en": "Apa itu time-invariance?", "id": "Respons tidak bergantung pada waktu mulai." },
  { "en": "Apa itu sistem non-linear?", "id": "Sistem yang tidak memenuhi prinsip linearitas." },
  { "en": "Apa itu stabilitas?", "id": "Kemampuan sistem kembali ke kesetimbangan." },
  { "en": "Apa itu sistem stabil?", "id": "Output terbatas untuk input yang terbatas." },
  { "en": "Apa itu sistem tidak stabil?", "id": "Output tidak terbatas untuk input terbatas." },
  { "en": "Apa itu sistem marginally stable?", "id": "Output berosilasi konstan untuk input terbatas." },
  { "en": "Apa itu respons transien?", "id": "Perilaku sistem saat menuju keadaan baru." },
  { "en": "Apa itu respons steady-state?", "id": "Perilaku sistem setelah waktu yang lama." },
  { "en": "Apa itu akurasi?", "id": "Seberapa dekat output dengan nilai referensi." },
  { "en": "Apa itu error steady-state?", "id": "Error yang tersisa setelah respons transien." },
  { "en": "Apa itu sensitivitas?", "id": "Ukuran seberapa besar sistem terpengaruh parameter." },
  { "en": "Apa itu robustnes?", "id": "Kemampuan sistem bekerja baik meski ada ketidakpastian." },
  { "en": "Apa itu diagram blok?", "id": "Representasi grafis dari fungsi-fungsi sistem." },
  { "en": "Apa itu summing junction?", "id": "Titik penjumlahan atau pengurangan sinyal." },
  { "en": "Apa itu take-off point?", "id": "Titik percabangan sinyal." },
  { "en": "Apa itu sistem servo?", "id": "Sistem kontrol untuk posisi mekanik." },
  { "en": "Apa itu sistem regulator?", "id": "Sistem kontrol untuk menjaga output konstan." },
  { "en": "Apa itu sistem process control?", "id": "Kontrol variabel seperti suhu, tekanan, aliran." },
  { "en": "Apa itu otomasi?", "id": "Penggunaan sistem kontrol untuk menggantikan manusia." },
  { "en": "Apa itu sistem waktu kontinu?", "id": "Sinyal didefinisikan pada setiap titik waktu." },
  { "en": "Apa itu sistem waktu diskrit?", "id": "Sinyal didefinisikan pada interval waktu tertentu." },
  { "en": "Apa itu kontrol digital?", "id": "Kontroler diimplementasikan menggunakan komputer digital." },
  { "en": "Apa itu variabel terkontrol?", "id": "Besaran atau kuantitas yang diukur/dikontrol." },
  { "en": "Apa itu variabel termanipulasi?", "id": "Besaran yang diubah kontroler untuk mempengaruhi plant." },
  { "en": "Tujuan utama sistem kontrol?", "id": "Mencapai stabilitas, akurasi, dan respons cepat." },
  { "en": "Contoh sistem biologis?", "id": "Pengaturan suhu tubuh manusia." },
  { "en": "Contoh sistem ekonomi?", "id": "Penawaran dan permintaan di pasar." },
  { "en": "Apa itu model matematis?", "id": "Deskripsi sistem menggunakan persamaan matematika." },
  { "en": "Apa itu white box modeling?", "id": "Model dibuat berdasarkan prinsip-prinsip fisika." },
  { "en": "Apa itu black box modeling?", "id": "Model dibuat dari data input-output." },
  { "en": "Apa itu grey box modeling?", "id": "Kombinasi model white box dan black box." },
  { "en": "Apa itu simulasi?", "id": "Meniru perilaku sistem menggunakan model." },
  { "en": "Apa itu identifikasi sistem?", "id": "Membangun model dari data eksperimen." },
  { "en": "Apa itu on-off control?", "id": "Jenis kontrol paling sederhana (nyala/mati)." },
  { "en": "Contoh on-off control?", "id": "Termostat setrika, kulkas." },
  { "en": "Apa itu feedforward control?", "id": "Aksi kontrol berdasarkan prediksi gangguan." },
  { "en": "Apa itu gain?", "id": "Faktor penguatan sinyal." },
  { "en": "Apa itu atenuasi?", "id": "Pelemahan sinyal." },
  { "en": "Apa itu sinyal step?", "id": "Input yang tiba-tiba berubah dan konstan." },
  { "en": "Apa itu sinyal ramp?", "id": "Input yang berubah secara linear terhadap waktu." },
  { "en": "Apa itu sinyal impuls?", "id": "Input sesaat dengan energi yang besar." },
  { "en": "Apa itu sinyal sinusoidal?", "id": "Input yang berosilasi secara periodik." },
  { "en": "Apa itu sistem kausal?", "id": "Output tidak bergantung pada input masa depan." },
  { "en": "Apa itu sistem deterministik?", "id": "Respons dapat diprediksi tanpa keacakan." },
  { "en": "Apa itu sistem stokastik?", "id": "Sistem yang memiliki unsur keacakan." },
  { "en": "Apa itu open-loop gain?", "id": "Gain total dari path umpan maju." },
  { "en": "Apa itu closed-loop gain?", "id": "Gain sistem dengan umpan balik tertutup." },
  { "en": "Apa itu atenuasi?", "id": "Pengurangan amplitudo sinyal." },
  { "en": "Apa itu sinyal analog?", "id": "Sinyal dengan nilai kontinu." },
  { "en": "Apa itu sinyal digital?", "id": "Sinyal dengan nilai diskrit." },
  { "en": "Apa itu sampling?", "id": "Mengubah sinyal kontinu menjadi diskrit." },
  { "en": "Apa itu kuantisasi?", "id": "Mengubah nilai kontinu menjadi level diskrit." },
  { "en": "Apa itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah sinyal analog menjadi digital." },
  { "en": "Apa itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah sinyal digital menjadi analog." },
  { "en": "Apa itu sistem manual?", "id": "Sistem yang membutuhkan operator manusia." },
  { "en": "Apa itu sistem otomatis?", "id": "Sistem yang bekerja tanpa intervensi manusia." },
  { "en": "Apa itu time delay (dead time)?", "id": "Waktu tunda antara input dan output." },
  { "en": "Apa itu superposisi?", "id": "Prinsip dasar dalam sistem linear." },
  { "en": "Apa itu orde sistem?", "id": "Orde tertinggi turunan dalam persamaan diferensial." },
  { "en": "Apa itu proses?", "id": "Operasi yang akan dikontrol." },
  { "en": "Apa itu node?", "id": "Titik percabangan dalam diagram blok." },
  { "en": "Apa itu cabang?", "id": "Garis yang merepresentasikan sistem/sinyal." },
  { "en": "Apa itu persamaan diferensial?", "id": "Mendeskripsikan dinamika sistem dalam domain waktu." },
  { "en": "Mengapa butuh model matematis?", "id": "Untuk analisis dan desain sistem kontrol." },
  { "en": "Model sistem mekanik translasi?", "id": "Menggunakan massa, pegas, dan peredam." },
  { "en": "Hukum Newton Kedua?", "id": "Dasar untuk pemodelan sistem mekanik." },
  { "en": "Model sistem mekanik rotasi?", "id": "Menggunakan momen inersia, pegas torsi." },
  { "en": "Model sistem elektrik?", "id": "Menggunakan resistor, induktor, dan kapasitor." },
  { "en": "Hukum Tegangan Kirchhoff (KVL) dalam pemodelan?", "id": "Dasar pemodelan rangkaian listrik." },
  { "en": "Apa itu linearisasi?", "id": "Aproksimasi sistem non-linear menjadi linear." },
  { "en": "Kapan linearisasi digunakan?", "id": "Saat sistem beroperasi di sekitar titik kerja." },
  { "en": "Apa itu titik kerja (operating point)?", "id": "Titik kesetimbangan dari sistem." },
  { "en": "Apa itu deret Taylor?", "id": "Dasar matematis untuk proses linearisasi." },
  { "en": "Apa itu transformasi Laplace?", "id": "Mengubah persamaan diferensial menjadi aljabar." },
  { "en": "Apa itu domain-s?", "id": "Domain frekuensi kompleks dari transformasi Laplace." },
  { "en": "Variabel 's' merepresentasikan apa?", "id": "Variabel frekuensi kompleks (sigma + j*omega)." },
  { "en": "Mengapa menggunakan transformasi Laplace?", "id": "Menyederhanakan analisis sistem LTI." },
  { "en": "Apa itu fungsi transfer?", "id": "Rasio transformasi Laplace output terhadap input." },
  { "en": "Asumsi fungsi transfer?", "id": "Kondisi awal sistem adalah nol." },
  { "en": "Apa itu pole dari sistem?", "id": "Akar dari penyebut fungsi transfer." },
  { "en": "Pole menentukan apa?", "id": "Respons natural dan stabilitas sistem." },
  { "en": "Apa itu zero dari sistem?", "id": "Akar dari pembilang fungsi transfer." },
  { "en": "Zero menentukan apa?", "id": "Karakteristik bentuk dari respons sistem." },
  { "en": "Apa itu plot pole-zero?", "id": "Visualisasi lokasi pole dan zero." },
  { "en": "Sistem stabil berdasarkan pole?", "id": "Semua pole di setengah bidang kiri." },
  { "en": "Sistem tidak stabil berdasarkan pole?", "id": "Ada pole di setengah bidang kanan." },
  { "en": "Sistem marginally stable berdasarkan pole?", "id": "Pole berada di sumbu imajiner." },
  { "en": "Apa itu mode sistem?", "id": "Komponen respons natural terkait setiap pole." },
  { "en": "Pole real menentukan mode apa?", "id": "Mode eksponensial (naik atau turun)." },
  { "en": "Pole kompleks menentukan mode apa?", "id": "Mode osilasi teredam atau tumbuh." },
  { "en": "Bagian real dari pole menentukan apa?", "id": "Laju peluruhan atau pertumbuhan eksponensial." },
  { "en": "Bagian imajiner dari pole menentukan apa?", "id": "Frekuensi osilasi dari respons." },
  { "en": "Apa itu fungsi transfer loop terbuka?", "id": "Hasil kali fungsi transfer di path maju." },
  { "en": "Apa itu fungsi transfer loop tertutup?", "id": "Fungsi transfer keseluruhan sistem umpan balik." },
  { "en": "Apa itu persamaan karakteristik?", "id": "Penyebut dari fungsi transfer loop tertutup." },
  { "en": "Apa itu impedansi?", "id": "Konsep resistansi di domain frekuensi." },
  { "en": "Impedansi resistor di domain-s?", "id": "Tetap R." },
  { "en": "Impedansi induktor di domain-s?", "id": "s*L." },
  { "en": "Impedansi kapasitor di domain-s?", "id": "1 / (s*C)." },
  { "en": "Analogi mekanik-elektrik?", "id": "Menghubungkan sistem mekanik dengan rangkaian listrik." },
  { "en": "Analogi gaya-tegangan?", "id": "Gaya analog dengan tegangan." },
  { "en": "Analogi gaya-arus?", "id": "Gaya analog dengan arus." },
  { "en": "Massa analog dengan apa?", "id": "Induktor (analogi gaya-tegangan)." },
  { "en": "Peredam (damper) analog dengan apa?", "id": "Resistor." },
  { "en": "Pegas (spring) analog dengan apa?", "id": "Kapasitor (analogi gaya-tegangan)." },
  { "en": "Apa itu sistem orde pertama?", "id": "Sistem dengan satu pole." },
  { "en": "Contoh sistem orde pertama?", "id": "Rangkaian RC, sistem termal." },
  { "en": "Apa itu sistem orde kedua?", "id": "Sistem dengan dua pole." },
  { "en": "Contoh sistem orde kedua?", "id": "Rangkaian RLC, sistem massa-pegas-peredam." },
  { "en": "Apa itu sistem proper?", "id": "Orde penyebut lebih besar dari pembilang." },
  { "en": "Apa itu sistem strictly proper?", "id": "Orde penyebut > orde pembilang." },
  { "en": "Apa itu sistem improper?", "id": "Orde pembilang > orde penyebut." },
  { "en": "Sistem fisik biasanya proper atau improper?", "id": "Proper atau strictly proper." },
  { "en": "Apa itu DC gain?", "id": "Gain sistem pada frekuensi nol (s=0)." },
  { "en": "Bagaimana menghitung DC gain?", "id": "Substitusi s = 0 pada fungsi transfer." },
  { "en": "Apa itu reduksi diagram blok?", "id": "Menyederhanakan diagram blok menjadi satu fungsi transfer." },
  { "en": "Aturan blok kaskade?", "id": "Fungsi transfer dikalikan." },
  { "en": "Aturan blok paralel?", "id": "Fungsi transfer dijumlahkan." },
  { "en": "Aturan loop umpan balik?", "id": "Menggunakan formula G / (1 + GH)." },
  { "en": "Apa itu signal-flow graph?", "id": "Representasi grafis lain dari sistem." },
  { "en": "Apa itu node input?", "id": "Node yang hanya memiliki cabang keluar." },
  { "en": "Apa itu node output?", "id": "Node yang hanya memiliki cabang masuk." },
  { "en": "Apa itu path?", "id": "Urutan cabang searah." },
  { "en": "Apa itu forward path?", "id": "Path dari input ke output." },
  { "en": "Apa itu loop?", "id": "Path yang dimulai dan diakhiri di node sama." },
  { "en": "Apa itu loop gain?", "id": "Hasil kali gain di sepanjang loop." },
  { "en": "Apa itu non-touching loops?", "id": "Loop yang tidak memiliki node bersama." },
  { "en": "Apa itu rumus gain Mason?", "id": "Metode untuk mencari fungsi transfer dari SFG." },
  { "en": "Apa itu variabel keadaan?", "id": "Set variabel terkecil yang mendeskripsikan sistem." },
  { "en": "Apa itu representasi state-space?", "id": "Model sistem menggunakan matriks (A, B, C, D)." },
  { "en": "Matriks A merepresentasikan apa?", "id": "Dinamika internal sistem." },
  { "en": "Matriks B merepresentasikan apa?", "id": "Bagaimana input mempengaruhi keadaan." },
  { "en": "Matriks C merepresentasikan apa?", "id": "Bagaimana keadaan menghasilkan output." },
  { "en": "Matriks D merepresentasikan apa?", "id": "Jalur langsung dari input ke output." },
  { "en": "Keuntungan state-space?", "id": "Dapat menangani sistem MIMO dan non-linear." },
  { "en": "Orde sistem dalam state-space?", "id": "Dimensi dari matriks A." },
  { "en": "Apa itu eigenvalue dari matriks A?", "id": "Pole dari sistem." },
  { "en": "Model motor DC?", "id": "Menggabungkan domain elektrik dan mekanik." },
  { "en": "Konstanta GGL (Back EMF) motor?", "id": "Menghubungkan kecepatan dengan tegangan induksi." },
  { "en": "Konstanta torsi motor?", "id": "Menghubungkan arus jangkar dengan torsi." },
  { "en": "Fungsi transfer motor DC?", "id": "Menghubungkan tegangan input dengan kecepatan output." },
  { "en": "Apa itu sistem non-minimum phase?", "id": "Sistem dengan zero di bidang kanan." },
  { "en": "Efek zero non-minimum phase?", "id": "Menyebabkan respons awal yang berlawanan." },
  { "en": "Apa itu pembatalan pole-zero?", "id": "Pole dan zero berada di lokasi sama." },
  { "en": "Masalah pembatalan pole-zero?", "id": "Mode tersembunyi dapat menyebabkan masalah." },
  { "en": "Apa itu respons impuls?", "id": "Output sistem saat diberi input impuls." },
  { "en": "Hubungan respons impuls dan fungsi transfer?", "id": "Transformasi Laplace dari respons impuls." },
  { "en": "Apa itu konvolusi?", "id": "Operasi matematis untuk mencari output sistem." },
  { "en": "Integral konvolusi?", "id": "Menghitung output sistem LTI di domain waktu." },
  { "en": "Teorema konvolusi?", "id": "Konvolusi waktu menjadi perkalian frekuensi." },
  { "en": "Apa itu model fisik?", "id": "Model berdasarkan pemahaman fisika sistem." },
  { "en": "Apa itu model empiris?", "id": "Model berdasarkan observasi dan data." },
  { "en": "Apa itu derajat kebebasan?", "id": "Jumlah gerakan independen yang mungkin." },
  { "en": "Apa itu sistem terdistribusi?", "id": "Sistem yang propertinya bergantung pada ruang." },
  { "en": "Apa itu sistem lumped-parameter?", "id": "Sistem diasumsikan sebagai titik tunggal." },
  { "en": "Apa itu momen inersia?", "id": "Analogi massa untuk gerak rotasi." },
  { "en": "Apa itu pegas torsi?", "id": "Analogi pegas untuk gerak rotasi." },
  { "en": "Apa itu gesekan viskos?", "id": "Gaya gesek sebanding dengan kecepatan." },
  { "en": "Apa itu pemodelan hidrolik?", "id": "Pemodelan sistem berbasis aliran fluida." },
  { "en": "Apa itu pemodelan termal?", "id": "Pemodelan sistem berbasis transfer panas." },
  { "en": "Apa itu time constant?", "id": "Ukuran kecepatan respons sistem." },
  { "en": "Apa itu ekspansi fraksi parsial?", "id": "Metode untuk melakukan transformasi Laplace invers." },
  { "en": "Apa itu analisis domain waktu?", "id": "Mempelajari perilaku sistem terhadap waktu." },
  { "en": "Input tes standar?", "id": "Step, ramp, impulse, dan sinusoidal." },
  { "en": "Mengapa menggunakan input tes?", "id": "Untuk mengevaluasi dan membandingkan performa sistem." },
  { "en": "Apa itu respons step?", "id": "Output sistem saat diberi input step." },
  { "en": "Karakteristik apa yang diukur dari respons step?", "id": "Kecepatan, osilasi, dan error." },
  { "en": "Apa itu rise time (Tr)?", "id": "Waktu dari 10% ke 90% nilai akhir." },
  { "en": "Rise time mengindikasikan apa?", "id": "Kecepatan respons sistem." },
  { "en": "Apa itu peak time (Tp)?", "id": "Waktu untuk mencapai puncak pertama." },
  { "en": "Apa itu settling time (Ts)?", "id": "Waktu agar respons masuk dalam batas toleransi." },
  { "en": "Batas toleransi umum untuk settling time?", "id": "Biasanya 2% atau 5% dari nilai akhir." },
  { "en": "Settling time mengindikasikan apa?", "id": "Berapa lama transien berlangsung." },
  { "en": "Apa itu overshoot?", "id": "Puncak maksimum melampaui nilai akhir." },
  { "en": "Penyebab overshoot?", "id": "Penyimpanan energi dalam sistem." },
  { "en": "Overshoot mengindikasikan apa?", "id": "Stabilitas relatif sistem." },
  { "en": "Apa itu delay time (Td)?", "id": "Waktu untuk mencapai 50% nilai akhir." },
  { "en": "Respons sistem orde pertama?", "id": "Naik secara eksponensial tanpa overshoot." },
  { "en": "Konstanta waktu (tau) menentukan apa?", "id": "Kecepatan respons sistem orde pertama." },
  { "en": "Hubungan settling time dan tau?", "id": "Ts kira-kira 4 kali tau." },
  { "en": "Apa itu rasio redaman (zeta)?", "id": "Parameter kunci sistem orde kedua." },
  { "en": "Rasio redaman menentukan apa?", "id": "Sifat osilasi dari respons." },
  { "en": "Apa itu frekuensi natural (omega_n)?", "id": "Frekuensi osilasi tanpa redaman." },
  { "en": "Frekuensi natural menentukan apa?", "id": "Kecepatan intrinsik respons sistem." },
  { "en": "Sistem underdamped (zeta < 1)?", "id": "Respons berosilasi sebelum mencapai kestabilan." },
  { "en": "Sistem critically damped (zeta = 1)?", "id": "Respons tercepat tanpa adanya overshoot." },
  { "en": "Sistem overdamped (zeta > 1)?", "id": "Respons lambat tanpa osilasi." },
  { "en": "Sistem undamped (zeta = 0)?", "id": "Respons berosilasi terus-menerus." },
  { "en": "Bagaimana lokasi pole mempengaruhi respons?", "id": "Sangat menentukan bentuk dan kecepatan respons." },
  { "en": "Pole dominan?", "id": "Pole yang paling dekat dengan sumbu imajiner." },
  { "en": "Mengapa pole dominan penting?", "id": "Karena paling lambat meluruh." },
  { "en": "Efek penambahan zero?", "id": "Dapat meningkatkan overshoot dan kecepatan." },
  { "en": "Efek penambahan pole?", "id": "Dapat memperlambat respons sistem." },
  { "en": "Apa itu error steady-state (e_ss)?", "id": "Error setelah sistem mencapai kestabilan." },
  { "en": "Penyebab error steady-state?", "id": "Tipe sistem dan tipe input." },
  { "en": "Apa itu tipe sistem?", "id": "Jumlah integrator (1/s) di loop terbuka." },
  { "en": "Sistem Tipe 0?", "id": "Tidak memiliki integrator." },
  { "en": "Sistem Tipe 1?", "id": "Memiliki satu integrator." },
  { "en": "Sistem Tipe 2?", "id": "Memiliki dua integrator." },
  { "en": "Error steady-state (input step, Tipe 0)?", "id": "Konstan dan berhingga." },
  { "en": "Error steady-state (input step, Tipe 1)?", "id": "Nol." },
  { "en": "Error steady-state (input ramp, Tipe 0)?", "id": "Tak terhingga." },
  { "en": "Error steady-state (input ramp, Tipe 1)?", "id": "Konstan dan berhingga." },
  { "en": "Error steady-state (input ramp, Tipe 2)?", "id": "Nol." },
  { "en": "Apa itu konstanta error statis?", "id": "Kp, Kv, Ka." },
  { "en": "Konstanta error posisi (Kp)?", "id": "Berkaitan dengan error untuk input step." },
  { "en": "Konstanta error kecepatan (Kv)?", "id": "Berkaitan dengan error untuk input ramp." },
  { "en": "Konstanta error akselerasi (Ka)?", "id": "Berkaitan dengan error untuk input parabola." },
  { "en": "Cara mengurangi error steady-state?", "id": "Meningkatkan gain atau tipe sistem." },
  { "en": "Efek samping meningkatkan gain?", "id": "Dapat mengurangi stabilitas sistem." },
  { "en": "Apa itu analisis sensitivitas?", "id": "Melihat efek perubahan parameter pada performa." },
  { "en": "Apa itu respons frekuensi?", "id": "Respons steady-state terhadap input sinusoidal." },
  { "en": "Apa itu magnitudo respons?", "id": "Rasio amplitudo output terhadap input." },
  { "en": "Apa itu fasa respons?", "id": "Perbedaan fasa antara output dan input." },
  { "en": "Apa itu plot Bode?", "id": "Grafik magnitudo dan fasa vs frekuensi." },
  { "en": "Sumbu magnitudo Bode plot?", "id": "Desibel (dB)." },
  { "en": "Sumbu frekuensi Bode plot?", "id": "Skala logaritmik." },
  { "en": "Satu dekade (decade)?", "id": "Perubahan frekuensi 10 kali lipat." },
  { "en": "Satu oktaf (octave)?", "id": "Perubahan frekuensi 2 kali lipat." },
  { "en": "Slope pada Bode plot?", "id": "Menunjukkan orde dari pole atau zero." },
  { "en": "Pole simpel menyebabkan slope?", "id": "-20 dB per dekade." },
  { "en": "Zero simpel menyebabkan slope?", "id": "+20 dB per dekade." },
  { "en": "Frekuensi pojok (corner frequency)?", "id": "Lokasi dimana slope mulai berubah." },
  { "en": "Apa itu plot Nyquist?", "id": "Plot respons frekuensi dalam koordinat polar." },
  { "en": "Plot Nyquist digunakan untuk apa?", "id": "Menganalisis stabilitas sistem loop tertutup." },
  { "en": "Apa itu gain margin?", "id": "Ukuran stabilitas relatif terkait gain." },
  { "en": "Apa itu phase margin?", "id": "Ukuran stabilitas relatif terkait fasa." },
  { "en": "Phase margin yang baik?", "id": "Biasanya antara 30 hingga 60 derajat." },
  { "en": "Gain margin yang baik?", "id": "Biasanya lebih dari 6 dB." },
  { "en": "Hubungan respons waktu dan frekuensi?", "id": "Saling berkaitan erat." },
  { "en": "Bandwidth yang lebar mengindikasikan apa?", "id": "Rise time yang cepat." },
  { "en": "Puncak resonansi mengindikasikan apa?", "id": "Overshoot pada respons waktu." },
  { "en": "Apa itu puncak resonansi (Mr)?", "id": "Nilai maksimum dari magnitudo respons." },
  { "en": "Apa itu frekuensi resonansi (wr)?", "id": "Frekuensi saat puncak resonansi terjadi." },
  { "en": "Apa itu bandwidth?", "id": "Rentang frekuensi dimana gain signifikan." },
  { "en": "Apa itu filter low-pass?", "id": "Melewatkan frekuensi rendah, meredam frekuensi tinggi." },
  { "en": "Apa itu filter high-pass?", "id": "Melewatkan frekuensi tinggi, meredam frekuensi rendah." },
  { "en": "Apa itu filter band-pass?", "id": "Melewatkan rentang frekuensi tertentu." },
  { "en": "Apa itu plot Nichols?", "id": "Plot magnitudo vs fasa." },
  { "en": "Spesifikasi performa sistem?", "id": "Kriteria desain (rise time, overshoot, dll)." },
  { "en": "Apa itu trade-off dalam desain?", "id": "Memperbaiki satu spesifikasi dapat memperburuk yang lain." },
  { "en": "Contoh trade-off?", "id": "Kecepatan vs stabilitas." },
  { "en": "Apa itu respons sistem non-linear?", "id": "Dapat menunjukkan perilaku kompleks (limit cycles)." },
  { "en": "Apa itu limit cycle?", "id": "Osilasi periodik yang stabil." },
  { "en": "Apa itu analisis bidang fasa?", "id": "Metode grafis untuk sistem orde kedua." },
  { "en": "Apa itu trajectory?", "id": "Lintasan keadaan sistem di bidang fasa." },
  { "en": "Apa itu singular point?", "id": "Titik kesetimbangan di bidang fasa." },
  { "en": "Tipe singular point?", "id": "Node, saddle, focus, center." },
  { "en": "Apa itu fungsi deskriptif?", "id": "Metode untuk menganalisis osilasi non-linear." },
  { "en": "Apa itu dead-zone?", "id": "Area dimana input tidak menghasilkan output." },
  { "en": "Apa itu saturasi?", "id": "Output terbatas pada level maksimum." },
  { "en": "Apa itu histeresis?", "id": "Output bergantung pada arah perubahan input." },
  { "en": "Respons sistem dengan waktu tunda?", "id": "Menyebabkan pergeseran fasa dan mengurangi stabilitas." },
  { "en": "Bagaimana menganalisis sistem waktu diskrit?", "id": "Menggunakan transformasi-Z dan bidang-z." },
  { "en": "Apa itu frekuensi sampling?", "id": "Seberapa sering sinyal diukur." },
  { "en": "Teorema sampling Nyquist?", "id": "Frekuensi sampling harus minimal 2 kali." },
  { "en": "Apa itu aliasing?", "id": "Distorsi akibat frekuensi sampling terlalu rendah." },
  { "en": "Apa itu analisis domain frekuensi?", "id": "Mempelajari respons sistem terhadap input sinusoidal." },
  { "en": "Mengapa analisis domain frekuensi penting?", "id": "Memberikan wawasan tentang stabilitas dan performa." },
  { "en": "Apa itu respons frekuensi?", "id": "Respons steady-state terhadap input sinusoidal." },
  { "en": "Respons frekuensi terdiri dari apa?", "id": "Magnitudo dan fasa." },
  { "en": "Bagaimana mendapatkan respons frekuensi dari G(s)?", "id": "Substitusi s dengan j*omega." },
  { "en": "Apa itu plot Bode?", "id": "Plot logaritmik dari magnitudo dan fasa." },
  { "en": "Keuntungan plot Bode?", "id": "Mudah digambar dan diinterpretasikan." },
  { "en": "Magnitudo pada plot Bode dalam satuan apa?", "id": "Desibel (dB)." },
  { "en": "Apa itu desibel (dB)?", "id": "20 kali logaritma basis 10 dari magnitudo." },
  { "en": "Frekuensi pada plot Bode dalam skala apa?", "id": "Logaritmik (radian/detik atau Hertz)." },
  { "en": "Plot magnitudo Bode menunjukkan apa?", "id": "Penguatan (gain) sistem pada berbagai frekuensi." },
  { "en": "Plot fasa Bode menunjukkan apa?", "id": "Pergeseran fasa sistem pada berbagai frekuensi." },
  { "en": "Faktor konstanta K pada plot Bode?", "id": "Garis horizontal pada 20*log(K)." },
  { "en": "Faktor integrator (1/s) pada plot Bode?", "id": "Slope -20 dB/dekade, fasa -90 derajat." },
  { "en": "Faktor differensiator (s) pada plot Bode?", "id": "Slope +20 dB/dekade, fasa +90 derajat." },
  { "en": "Faktor pole orde pertama pada plot Bode?", "id": "Slope berubah menjadi -20 dB/dekade." },
  { "en": "Faktor zero orde pertama pada plot Bode?", "id": "Slope berubah menjadi +20 dB/dekade." },
  { "en": "Faktor pole kuadratik pada plot Bode?", "id": "Slope berubah menjadi -40 dB/dekade." },
  { "en": "Faktor zero kuadratik pada plot Bode?", "id": "Slope berubah menjadi +40 dB/dekade." },
  { "en": "Apa itu frekuensi pojok (corner frequency)?", "id": "Frekuensi dimana aproksimasi asimtotik bertemu." },
  { "en": "Koreksi pada frekuensi pojok?", "id": "Sekitar 3 dB untuk faktor orde pertama." },
  { "en": "Apa itu plot Nyquist?", "id": "Plot respons frekuensi dalam bidang kompleks." },
  { "en": "Sumbu plot Nyquist?", "id": "Sumbu real dan sumbu imajiner." },
  { "en": "Plot Nyquist juga disebut?", "id": "Plot polar." },
  { "en": "Bagaimana plot Nyquist dibuat?", "id": "Plot G(jw) untuk omega dari 0 hingga tak hingga." },
  { "en": "Kriteria stabilitas Nyquist?", "id": "Menganalisis putaran plot mengelilingi titik -1." },
  { "en": "Apa itu stabilitas relatif?", "id": "Seberapa 'jauh' sistem dari ketidakstabilan." },
  { "en": "Apa itu gain margin (GM)?", "id": "Berapa banyak gain bisa ditambah." },
  { "en": "Di mana GM diukur pada plot Bode?", "id": "Pada frekuensi saat fasa -180 derajat." },
  { "en": "GM positif (dalam dB) berarti?", "id": "Sistem stabil." },
  { "en": "Apa itu phase margin (PM)?", "id": "Berapa banyak fasa bisa dikurangi." },
  { "en": "Di mana PM diukur pada plot Bode?", "id": "Pada frekuensi saat gain 0 dB." },
  { "en": "PM positif berarti?", "id": "Sistem stabil." },
  { "en": "Hubungan PM dan redaman?", "id": "PM yang lebih besar berarti redaman lebih baik." },
  { "en": "Aproksimasi redaman dari PM?", "id": "zeta kira-kira PM (derajat) / 100." },
  { "en": "Apa itu frekuensi crossover gain?", "id": "Frekuensi saat gain magnitudo 0 dB." },
  { "en": "Apa itu frekuensi crossover fasa?", "id": "Frekuensi saat pergeseran fasa -180 derajat." },
  { "en": "Apa itu plot Nichols?", "id": "Plot gain (dB) vs fasa (derajat)." },
  { "en": "Keuntungan plot Nichols?", "id": "Stabilitas dan performa loop tertutup mudah dilihat." },
  { "en": "Apa itu M-circles?", "id": "Kontur magnitudo konstan loop tertutup." },
  { "en": "Apa itu N-circles?", "id": "Kontur fasa konstan loop tertutup." },
  { "en": "Apa itu bandwidth?", "id": "Rentang frekuensi dimana gain sistem signifikan." },
  { "en": "Definisi bandwidth (-3dB)?", "id": "Frekuensi saat magnitudo turun 3 dB." },
  { "en": "Hubungan bandwidth dan rise time?", "id": "Bandwidth lebih lebar, rise time lebih cepat." },
  { "en": "Apa itu puncak resonansi (Mr)?", "id": "Gain maksimum pada respons frekuensi." },
  { "en": "Puncak resonansi tinggi menandakan apa?", "id": "Overshoot besar pada respons waktu." },
  { "en": "Apa itu frekuensi resonansi (wr)?", "id": "Frekuensi saat Mr terjadi." },
  { "en": "Respons frekuensi sistem orde pertama?", "id": "Seperti filter low-pass." },
  { "en": "Respons frekuensi sistem orde kedua?", "id": "Dapat menunjukkan puncak resonansi." },
  { "en": "Apa itu filter?", "id": "Sistem yang mengubah karakteristik frekuensi sinyal." },
  { "en": "Filter low-pass ideal?", "id": "Melewatkan semua frekuensi di bawah cutoff." },
  { "en": "Filter high-pass ideal?", "id": "Melewatkan semua frekuensi di atas cutoff." },
  { "en": "Filter band-pass ideal?", "id": "Melewatkan frekuensi dalam suatu rentang." },
  { "en": "Filter band-stop (notch) ideal?", "id": "Meredam frekuensi dalam suatu rentang." },
  { "en": "Apa itu zero-pole placement?", "id": "Mendesain filter dengan menempatkan pole dan zero." },
  { "en": "Filter Butterworth?", "id": "Filter dengan respons magnitudo paling datar." },
  { "en": "Filter Chebyshev?", "id": "Respons lebih tajam dengan ripple di passband." },
  { "en": "Filter Bessel?", "id": "Memiliki respons fasa paling linear." },
  { "en": "Tujuan utama respons fasa linear?", "id": "Menghindari distorsi bentuk gelombang." },
  { "en": "Apa itu delai grup (group delay)?", "id": "Ukuran distorsi fasa." },
  { "en": "Respons frekuensi dari sistem waktu tunda?", "id": "Magnitudo konstan, fasa menurun linear." },
  { "en": "Respons frekuensi sistem non-minimum phase?", "id": "Pergeseran fasa lebih besar dari sistem minimum phase." },
  { "en": "Apa itu analisis spektral?", "id": "Menguraikan sinyal menjadi komponen frekuensinya." },
  { "en": "Apa itu Fast Fourier Transform (FFT)?", "id": "Algoritma efisien untuk menghitung spektrum." },
  { "en": "Apa itu power spectral density (PSD)?", "id": "Distribusi daya sinyal pada berbagai frekuensi." },
  { "en": "Apa itu white noise?", "id": "Noise dengan spektrum daya yang datar." },
  { "en": "Bagaimana gain margin di plot Nyquist?", "id": "Berhubungan dengan perpotongan sumbu real negatif." },
  { "en": "Bagaimana phase margin di plot Nyquist?", "id": "Sudut antara titik -1 dan perpotongan lingkaran satuan." },
  { "en": "Apa itu sistem minimum phase?", "id": "Tidak memiliki pole atau zero di RHP." },
  { "en": "Apa itu sistem all-pass?", "id": "Gain magnitudo konstan untuk semua frekuensi." },
  { "en": "Sistem kausalitas dalam domain frekuensi?", "id": "Hubungan antara bagian real dan imajiner." },
  { "en": "Hubungan Kramers-Kronig?", "id": "Dasar matematis untuk kausalitas." },
  { "en": "Respons frekuensi sistem loop tertutup?", "id": "Dapat ditentukan dari respons loop terbuka." },
  { "en": "Sensitivitas dalam domain frekuensi?", "id": "Menunjukkan seberapa baik sistem menolak gangguan." },
  { "en": "Apa itu fungsi sensitivitas?", "id": "1 / (1 + G(s)H(s))." },
  { "en": "Apa itu fungsi sensitivitas komplementer?", "id": "G(s)H(s) / (1 + G(s)H(s))." },
  { "en": "Sistem Tipe 1 pada plot Bode?", "id": "Memiliki slope -20 dB/dekade pada frekuensi rendah." },
  { "en": "Sistem Tipe 2 pada plot Bode?", "id": "Memiliki slope -40 dB/dekade pada frekuensi rendah." },
  { "en": "Bagaimana sistem tipe mempengaruhi gain frekuensi rendah?", "id": "Gain menjadi tak hingga (secara teori)." },
  { "en": "Apa itu kestabilan robust?", "id": "Stabilitas terjaga meskipun ada ketidakpastian model." },
  { "en": "Apa itu performa robust?", "id": "Performa terjaga meskipun ada ketidakpastian model." },
  { "en": "Apa itu loop shaping?", "id": "Mendesain kontroler dengan membentuk plot Bode." },
  { "en": "Apa itu kontrol aktif noise?", "id": "Membatalkan noise dengan menghasilkan sinyal anti-noise." },
  { "en": "Apa itu kontrol aktif getaran?", "id": "Meredam getaran mekanis secara aktif." },
  { "en": "Eksperimen respons frekuensi?", "id": "Memberikan input sinus dan mengukur output." },
  { "en": "Apa itu signal analyzer?", "id": "Alat untuk mengukur respons frekuensi." },
  { "en": "Apa itu Bode's gain-phase relationship?", "id": "Hubungan antara slope gain dan fasa." },
  { "en": "Respons frekuensi sistem diskrit?", "id": "Dievaluasi pada lingkaran satuan di bidang-z." },
  { "en": "Apa itu warping frekuensi?", "id": "Efek distorsi saat mengubah sistem kontinu ke diskrit." },
  { "en": "Apa itu filter digital?", "id": "Implementasi filter menggunakan prosesor digital." },
  { "en": "Filter FIR (Finite Impulse Response)?", "id": "Filter digital yang selalu stabil." },
  { "en": "Filter IIR (Infinite Impulse Response)?", "id": "Filter digital yang lebih efisien." },
  { "en": "Apa itu windowing?", "id": "Teknik yang digunakan dalam desain filter FIR." },
  { "en": "Desain filter dengan metode penempatan pole-zero?", "id": "Menempatkan pole/zero untuk bentuk respons." },
  { "en": "Apa itu frekuensi Nyquist?", "id": "Setengah dari frekuensi sampling." },
  { "en": "Apa tujuan desain sistem kontrol?", "id": "Memenuhi spesifikasi performa yang diinginkan." },
  { "en": "Spesifikasi performa apa saja?", "id": "Stabilitas, respons transien, dan error steady-state." },
  { "en": "Apa itu kompensasi?", "id": "Menambahkan komponen (kompensator) untuk memperbaiki performa." },
  { "en": "Apa itu kompensator?", "id": "Kontroler tambahan dalam loop umpan balik." },
  { "en": "Jenis kompensasi?", "id": "Lead, lag, dan lag-lead." },
  { "en": "Apa itu kontroler PID?", "id": "Proportional-Integral-Derivative." },
  { "en": "Mengapa PID sangat populer?", "id": "Sederhana, efektif, dan mudah dipahami." },
  { "en": "Apa itu aksi kontrol proporsional (P)?", "id": "Output sebanding dengan error saat ini." },
  { "en": "Fungsi utama kontroler P?", "id": "Mempercepat respons sistem." },
  { "en": "Efek samping kontroler P?", "id": "Meningkatkan gain dapat mengurangi stabilitas." },
  { "en": "Error steady-state dengan kontroler P?", "id": "Biasanya masih memiliki error." },
  { "en": "Apa itu aksi kontrol integral (I)?", "id": "Output sebanding dengan akumulasi error." },
  { "en": "Fungsi utama kontroler I?", "id": "Menghilangkan error steady-state." },
  { "en": "Efek samping kontroler I?", "id": "Dapat memperlambat respons dan mengurangi stabilitas." },
  { "en": "Apa itu integral windup?", "id": "Masalah saturasi pada kontroler integral." },
  { "en": "Apa itu aksi kontrol derivatif (D)?", "id": "Output sebanding dengan laju perubahan error." },
  { "en": "Fungsi utama kontroler D?", "id": "Meningkatkan redaman, memprediksi masa depan." },
  { "en": "Efek samping kontroler D?", "id": "Memperkuat noise frekuensi tinggi." },
  { "en": "Kontroler PD?", "id": "Menggabungkan aksi proporsional dan derivatif." },
  { "en": "Kontroler PI?", "id": "Menggabungkan aksi proporsional dan integral." },
  { "en": "Tujuan utama kontroler PI?", "id": "Mempercepat respons dan menghilangkan error." },
  { "en": "Apa itu tuning kontroler?", "id": "Proses menentukan parameter (Kp, Ki, Kd)." },
  { "en": "Metode tuning Ziegler-Nichols?", "id": "Metode heuristik untuk tuning PID." },
  { "en": "Metode loop tertutup Ziegler-Nichols?", "id": "Berdasarkan gain dan periode osilasi." },
  { "en": "Metode loop terbuka Ziegler-Nichols?", "id": "Berdasarkan respons step sistem." },
  { "en": "Apa itu kompensator lead?", "id": "Memberikan pergeseran fasa positif (lead)." },
  { "en": "Tujuan kompensator lead?", "id": "Meningkatkan phase margin dan kecepatan." },
  { "en": "Kompensator lead mirip kontroler apa?", "id": "Kontroler PD." },
  { "en": "Apa itu kompensator lag?", "id": "Memberikan atenuasi gain pada frekuensi tinggi." },
  { "en": "Tujuan kompensator lag?", "id": "Memperbaiki error steady-state tanpa mengorbankan stabilitas." },
  { "en": "Kompensator lag mirip kontroler apa?", "id": "Kontroler PI." },
  { "en": "Apa itu kompensator lag-lead?", "id": "Menggabungkan efek dari kompensator lag dan lead." },
  { "en": "Desain dengan metode Root Locus?", "id": "Membentuk lintasan akar untuk memenuhi spesifikasi." },
  { "en": "Bagaimana kompensator mengubah Root Locus?", "id": "Dengan menambahkan pole dan zero baru." },
  { "en": "Zero kompensator lead diletakkan di mana?", "id": "Lebih dekat ke origin daripada pole-nya." },
  { "en": "Pole kompensator lag diletakkan di mana?", "id": "Sangat dekat dengan origin." },
  { "en": "Desain dengan metode respons frekuensi?", "id": "Membentuk plot Bode untuk memenuhi spesifikasi." },
  { "en": "Bagaimana kompensator mengubah plot Bode?", "id": "Mengubah kurva gain dan fasa." },
  { "en": "Apa itu umpan balik keadaan (state feedback)?", "id": "Menggunakan semua variabel keadaan untuk umpan balik." },
  { "en": "Apa itu penempatan pole (pole placement)?", "id": "Menempatkan pole loop tertutup di lokasi manapun." },
  { "en": "Syarat untuk penempatan pole?", "id": "Sistem harus dapat dikontrol (controllable)." },
  { "en": "Apa itu keterkontrolan (controllability)?", "id": "Kemampuan untuk menggerakkan sistem ke keadaan manapun." },
  { "en": "Apa itu estimator keadaan (observer)?", "id": "Memperkirakan variabel keadaan yang tidak terukur." },
  { "en": "Mengapa butuh observer?", "id": "Karena tidak semua variabel keadaan bisa diukur." },
  { "en": "Syarat untuk desain observer?", "id": "Sistem harus dapat diamati (observable)." },
  { "en": "Apa itu keteramatan (observability)?", "id": "Kemampuan untuk menentukan keadaan dari output." },
  { "en": "Observer Luenberger?", "id": "Jenis umum dari estimator keadaan." },
  { "en": "Prinsip separasi?", "id": "Desain kontroler dan observer bisa dilakukan terpisah." },
  { "en": "Apa itu kontrol optimal?", "id": "Desain kontroler yang meminimalkan indeks performa." },
  { "en": "Apa itu indeks performa?", "id": "Ukuran kuantitatif dari 'kebaikan' respons." },
  { "en": "Contoh indeks performa?", "id": "Integral dari kuadrat error (ISE)." },
  { "en": "Apa itu LQR (Linear-Quadratic Regulator)?", "id": "Kontroler optimal untuk sistem linear." },
  { "en": "Apa itu filter Kalman?", "id": "Estimator optimal untuk sistem dengan noise." },
  { "en": "Apa itu LQG (Linear-Quadratic-Gaussian)?", "id": "Kombinasi dari kontroler LQR dan filter Kalman." },
  { "en": "Apa itu kontrol robust?", "id": "Desain kontrol yang tahan terhadap ketidakpastian." },
  { "en": "Apa itu H-infinity control?", "id": "Metode desain kontrol robust." },
  { "en": "Apa itu kontrol adaptif?", "id": "Kontroler yang menyesuaikan dirinya secara otomatis." },
  { "en": "Kapan kontrol adaptif digunakan?", "id": "Saat parameter plant tidak diketahui atau berubah." },
  { "en": "Apa itu Model Reference Adaptive Control (MRAC)?", "id": "Membuat plant mengikuti model referensi." },
  { "en": "Apa itu Self-Tuning Regulator (STR)?", "id": "Mengidentifikasi parameter plant secara online." },
  { "en": "Apa itu gain scheduling?", "id": "Mengubah parameter kontroler berdasarkan variabel penjadwalan." },
  { "en": "Apa itu kontrol non-linear?", "id": "Desain kontroler untuk sistem non-linear." },
  { "en": "Apa itu feedback linearization?", "id": "Membatalkan non-linearitas sistem." },
  { "en": "Apa itu sliding mode control?", "id": "Teknik kontrol non-linear yang sangat robust." },
  { "en": "Apa itu kontrol cerdas?", "id": "Menggunakan metode AI seperti fuzzy logic." },
  { "en": "Apa itu fuzzy logic control?", "id": "Kontrol berbasis aturan 'if-then'." },
  { "en": "Apa itu neural network control?", "id": "Menggunakan jaringan saraf sebagai kontroler." },
  { "en": "Apa itu on-off control?", "id": "Jenis kontroler paling sederhana." },
  { "en": "Kelemahan on-off control?", "id": "Menyebabkan osilasi dan keausan mekanis." },
  { "en": "Apa itu kontrol kaskade?", "id": "Struktur kontrol dengan loop dalam dan luar." },
  { "en": "Apa itu kontrol feedforward?", "id": "Mengantisipasi dan mengatasi gangguan sebelum terjadi." },
  { "en": "Apa itu kontrol rasio?", "id": "Menjaga rasio antara dua variabel tetap konstan." },
  { "en": "Apa itu dead time compensation?", "id": "Teknik untuk menangani sistem dengan waktu tunda." },
  { "en": "Apa itu Smith Predictor?", "id": "Kompensator waktu tunda yang populer." },
  { "en": "Apa itu implementasi digital kontroler?", "id": "Menggunakan mikroprosesor atau DSP." },
  { "en": "Apa itu discretisasi?", "id": "Proses mengubah kontroler kontinu menjadi diskrit." },
  { "en": "Efek discretisasi?", "id": "Dapat mempengaruhi stabilitas dan performa." },
  { "en": "Apa itu anti-windup?", "id": "Strategi untuk mengatasi saturasi integral." },
  { "en": "Apa itu bumpless transfer?", "id": "Peralihan mulus antara mode manual dan otomatis." },
  { "en": "Apa itu actuator saturation?", "id": "Aktuator mencapai batas maksimumnya." },
  { "en": "Apa itu sensor noise?", "id": "Gangguan acak pada sinyal pengukuran." },
  { "en": "Efek sensor noise pada kontroler D?", "id": "Aksi kontrol menjadi sangat berfluktuasi." },
  { "en": "Solusi untuk noise pada kontroler D?", "id": "Menambahkan filter low-pass." },
  { "en": "Apa itu real-time control?", "id": "Perhitungan kontrol harus selesai dalam batas waktu." },
  { "en": "Apa itu sistem terdistribusi?", "id": "Kontrol dilakukan oleh banyak unit terkoordinasi." },
  { "en": "Apa itu perangkat keras kontrol?", "id": "PLC, DCS, mikrokontroler." },
  { "en": "Apa itu PLC (Programmable Logic Controller)?", "id": "Komputer industri untuk kontrol otomasi." },
  { "en": "Apa itu DCS (Distributed Control System)?", "id": "Sistem kontrol untuk pabrik skala besar." },
  { "en": "Apa itu SCADA?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Apa itu HMI (Human-Machine Interface)?", "id": "Antarmuka grafis untuk operator." },
  { "en": "Apa itu sistem real-time?", "id": "Sistem dengan batasan waktu yang ketat." },
  { "en": "Desain kontrol untuk sistem MIMO?", "id": "Lebih kompleks, mempertimbangkan interaksi antar loop." },
  { "en": "Apa itu decoupling?", "id": "Mendesain agar input hanya mempengaruhi satu output." },
  { "en": "Langkah pertama dalam desain kontrol?", "id": "Memahami proses dan menentukan tujuan." },
  { "en": "Langkah kedua dalam desain kontrol?", "id": "Membuat model matematis dari plant." },
  { "en": "Langkah ketiga dalam desain kontrol?", "id": "Menganalisis model dan memilih strategi kontrol." },
  { "en": "Langkah keempat dalam desain kontrol?", "id": "Mendesain dan melakukan tuning kontroler." },
  { "en": "Langkah kelima dalam desain kontrol?", "id": "Simulasi dan implementasi." },
  { "en": "Apa itu robot?", "id": "Mesin terprogram untuk melakukan tugas otomatis." },
  { "en": "Apa itu robotika?", "id": "Ilmu dan teknologi tentang robot." },
  { "en": "Tiga hukum robotika Asimov?", "id": "Konsep fiksi tentang etika robot." },
  { "en": "Apa itu manipulator?", "id": "Lengan robot yang dirancang untuk memanipulasi objek." },
  { "en": "Apa itu robot mobile?", "id": "Robot yang dapat bergerak di lingkungannya." },
  { "en": "Apa itu robot humanoid?", "id": "Robot yang dirancang menyerupai manusia." },
  { "en": "Apa itu link?", "id": "Bagian kaku dari sebuah manipulator." },
  { "en": "Apa itu joint (sendi)?", "id": "Koneksi antara dua link yang memungkinkan gerakan." },
  { "en": "Apa itu sendi revolute?", "id": "Sendi yang memungkinkan gerakan rotasi." },
  { "en": "Apa itu sendi prismatic?", "id": "Sendi yang memungkinkan gerakan translasi (linear)." },
  { "en": "Apa itu rantai kinematik?", "id": "Rangkaian link dan sendi." },
  { "en": "Apa itu rantai kinematik terbuka?", "id": "Memiliki ujung bebas (misalnya, lengan robot)." },
  { "en": "Apa itu rantai kinematik tertutup?", "id": "Tidak memiliki ujung bebas (misalnya, mekanisme paralel)." },
  { "en": "Apa itu end-effector?", "id": "Alat di ujung lengan robot." },
  { "en": "Contoh end-effector?", "id": "Gripper, alat las, kamera." },
  { "en": "Apa itu base (dasar)?", "id": "Bagian robot yang terpasang tetap." },
  { "en": "Apa itu derajat kebebasan (DOF)?", "id": "Jumlah gerakan independen yang dimiliki robot." },
  { "en": "Berapa DOF yang dibutuhkan untuk mencapai posisi manapun?", "id": "Minimal tiga DOF (x, y, z)." },
  { "en": "Berapa DOF yang dibutuhkan untuk mencapai orientasi manapun?", "id": "Minimal tiga DOF (roll, pitch, yaw)." },
  { "en": "Berapa DOF lengan manusia (kira-kira)?", "id": "Tujuh DOF." },
  { "en": "Apa itu workspace (ruang kerja)?", "id": "Area yang dapat dijangkau oleh end-effector." },
  { "en": "Apa itu reachable workspace?", "id": "Semua titik yang dapat dijangkau end-effector." },
  { "en": "Apa itu dexterous workspace?", "id": "Titik yang dapat dijangkau dengan berbagai orientasi." },
  { "en": "Apa itu konfigurasi robot?", "id": "Spesifikasi lengkap posisi semua link." },
  { "en": "Apa itu ruang konfigurasi (C-space)?", "id": "Ruang dari semua konfigurasi robot yang mungkin." },
  { "en": "Apa itu singularitas?", "id": "Konfigurasi dimana robot kehilangan derajat kebebasan." },
  { "en": "Apa itu akurasi robot?", "id": "Seberapa dekat robot mencapai titik target." },
  { "en": "Apa itu repeatability (keterulangan)?", "id": "Kemampuan robot kembali ke posisi yang sama." },
  { "en": "Mana yang biasanya lebih baik, akurasi atau repeatability?", "id": "Repeatability." },
  { "en": "Apa itu resolusi?", "id": "Gerakan terkecil yang dapat dibuat robot." },
  { "en": "Apa itu payload?", "id": "Beban maksimum yang dapat dibawa robot." },
  { "en": "Struktur robot serial?", "id": "Link dan sendi terhubung secara berurutan." },
  { "en": "Struktur robot paralel?", "id": "End-effector terhubung ke dasar oleh beberapa rantai." },
  { "en": "Kelebihan robot paralel?", "id": "Kecepatan tinggi, akurasi, dan kekakuan." },
  { "en": "Kekurangan robot paralel?", "id": "Workspace lebih kecil." },
  { "en": "Apa itu robot Cartesian?", "id": "Menggunakan tiga sendi prismatic (X, Y, Z)." },
  { "en": "Apa itu robot Cylindrical?", "id": "Menggunakan sendi revolute dan dua prismatic." },
  { "en": "Apa itu robot Spherical (Polar)?", "id": "Menggunakan dua sendi revolute dan satu prismatic." },
  { "en": "Apa itu robot Articulated (Antropomorfik)?", "id": "Struktur mirip lengan manusia dengan sendi revolute." },
  { "en": "Apa itu robot SCARA?", "id": "Selective Compliance Articulated Robot for Assembly." },
  { "en": "Apa itu robot mobile beroda?", "id": "Robot yang bergerak menggunakan roda." },
  { "en": "Apa itu differential drive?", "id": "Mengontrol kecepatan dua roda secara independen." },
  { "en": "Apa itu robot mobile berkaki?", "id": "Robot yang berjalan menggunakan kaki." },
  { "en": "Apa itu robot terbang?", "id": "Drone atau Unmanned Aerial Vehicle (UAV)." },
  { "en": "Apa itu robot bawah air?", "id": "Autonomous Underwater Vehicle (AUV)." },
  { "en": "Apa itu sistem penggerak robot?", "id": "Motor yang menggerakkan sendi (aktuator)." },
  { "en": "Jenis motor yang umum?", "id": "Motor DC servo, stepper, brushless DC." },
  { "en": "Apa itu transmisi?", "id": "Mekanisme untuk mentransfer daya (misalnya, gearbox)." },
  { "en": "Apa itu sensor robot?", "id": "Memberikan informasi tentang robot dan lingkungannya." },
  { "en": "Apa itu sensor internal?", "id": "Mengukur keadaan internal robot (posisi sendi)." },
  { "en": "Contoh sensor internal?", "id": "Encoder, resolver." },
  { "en": "Apa itu sensor eksternal?", "id": "Mengukur lingkungan di sekitar robot." },
  { "en": "Contoh sensor eksternal?", "id": "Kamera, LIDAR, sonar, sensor sentuh." },
  { "en": "Apa itu visi komputer?", "id": "Memungkinkan robot untuk 'melihat'." },
  { "en": "Apa itu LIDAR?", "id": "Light Detection and Ranging." },
  { "en": "Apa itu sonar?", "id": "Menggunakan gelombang suara untuk mendeteksi objek." },
  { "en": "Apa itu sensor gaya/torsi?", "id": "Mengukur gaya dan torsi yang diterapkan." },
  { "en": "Apa itu proprioception?", "id": "Rasa posisi dan gerakan tubuh robot." },
  { "en": "Apa itu kontroler robot?", "id": "Otak dari robot yang menjalankan program." },
  { "en": "Apa itu teleoperation?", "id": "Mengontrol robot dari jarak jauh." },
  { "en": "Apa itu robot otonom?", "id": "Robot yang beroperasi tanpa campur tangan manusia." },
  { "en": "Apa itu robot industri?", "id": "Robot yang digunakan dalam manufaktur." },
  { "en": "Tugas umum robot industri?", "id": "Pengelasan, pengecatan, perakitan, pick-and-place." },
  { "en": "Apa itu robot kolaboratif (cobot)?", "id": "Robot yang dirancang untuk bekerja bersama manusia." },
  { "en": "Apa itu robot medis?", "id": "Robot yang digunakan dalam prosedur bedah." },
  { "en": "Apa itu robot luar angkasa?", "id": "Robot untuk eksplorasi planet (misalnya, Mars Rover)." },
  { "en": "Apa itu robot domestik?", "id": "Robot untuk tugas rumah tangga (misalnya, vacuum cleaner)." },
  { "en": "Apa itu robotika swarm?", "id": "Koordinasi sejumlah besar robot sederhana." },
  { "en": "Apa itu bio-robotika?", "id": "Robot yang terinspirasi oleh sistem biologis." },
  { "en": "Apa itu robotika lunak (soft robotics)?", "id": "Robot yang terbuat dari material fleksibel." },
  { "en": "Apa itu robotika evolusioner?", "id": "Menggunakan evolusi untuk mendesain robot." },
  { "en": "Apa itu etika robot?", "id": "Pertimbangan moral dalam desain dan penggunaan robot." },
  { "en": "Apa itu pemrograman robot?", "id": "Memberikan instruksi kepada robot." },
  { "en": "Apa itu 'teach pendant'?", "id": "Perangkat genggam untuk memprogram robot." },
  { "en": "Apa itu pemrograman offline?", "id": "Membuat program robot di komputer." },
  { "en": "Apa itu kalibrasi robot?", "id": "Menyempurnakan model robot agar sesuai kenyataan." },
  { "en": "Apa itu frame (kerangka acuan)?", "id": "Sistem koordinat yang melekat pada objek." },
  { "en": "Apa itu base frame?", "id": "Kerangka acuan di dasar robot." },
  { "en": "Apa itu tool frame?", "id": "Kerangka acuan di end-effector." },
  { "en": "Apa itu kinematika?", "id": "Studi tentang gerakan tanpa mempertimbangkan gaya." },
  { "en": "Apa itu dinamika?", "id": "Studi tentang gerakan yang mempertimbangkan gaya." },
  { "en": "Apa itu joint space?", "id": "Ruang yang dideskripsikan oleh sudut-sudut sendi." },
  { "en": "Apa itu Cartesian space (task space)?", "id": "Ruang yang dideskripsikan oleh posisi dan orientasi." },
  { "en": "Apa itu pose?", "id": "Kombinasi dari posisi dan orientasi." },
  { "en": "Apa itu frame homogen?", "id": "Matriks 4x4 untuk merepresentasikan pose." },
  { "en": "Apa itu matriks rotasi?", "id": "Matriks 3x3 untuk merepresentasikan orientasi." },
  { "en": "Apa itu redundant manipulator?", "id": "Lengan robot dengan lebih banyak DOF." },
  { "en": "Apa itu lokalisasi?", "id": "Menentukan posisi robot di lingkungannya." },
  { "en": "Apa itu pemetaan (mapping)?", "id": "Membangun peta dari lingkungan." },
  { "en": "Apa itu SLAM?", "id": "Simultaneous Localization and Mapping." },
  { "en": "Apa itu sistem penggerak (drivetrain)?", "id": "Sistem yang menghasilkan gerakan pada robot." },
  { "en": "Apa itu holonomic?", "id": "Robot yang dapat bergerak ke segala arah." },
  { "en": "Apa itu non-holonomic?", "id": "Robot dengan batasan gerakan (misalnya, mobil)." },
  { "en": "Apa itu kinematika?", "id": "Studi geometri gerak." },
  { "en": "Apa itu kinematika maju (forward)?", "id": "Menghitung pose end-effector dari sudut sendi." },
  { "en": "Input kinematika maju?", "id": "Nilai-nilai variabel sendi." },
  { "en": "Output kinematika maju?", "id": "Posisi dan orientasi end-effector." },
  { "en": "Apakah solusi kinematika maju unik?", "id": "Ya, selalu unik." },
  { "en": "Apa itu kinematika balik (inverse)?", "id": "Menghitung sudut sendi dari pose end-effector." },
  { "en": "Input kinematika balik?", "id": "Posisi dan orientasi end-effector yang diinginkan." },
  { "en": "Output kinematika balik?", "id": "Nilai-nilai variabel sendi." },
  { "en": "Apakah solusi kinematika balik unik?", "id": "Tidak, bisa tidak ada, satu, atau banyak." },
  { "en": "Mengapa kinematika balik lebih sulit?", "id": "Persamaan non-linear dan banyak solusi." },
  { "en": "Solusi analitik (closed-form)?", "id": "Solusi kinematika balik dalam bentuk persamaan." },
  { "en": "Solusi numerik?", "id": "Solusi kinematika balik menggunakan metode iteratif." },
  { "en": "Apa itu parameter Denavit-Hartenberg (DH)?", "id": "Metodologi standar untuk kinematika maju." },
  { "en": "Berapa parameter DH per link?", "id": "Empat parameter." },
  { "en": "Apa saja parameter DH?", "id": "a, alpha, d, dan theta." },
  { "en": "Apa itu matriks transformasi homogen?", "id": "Matriks 4x4 yang merepresentasikan frame." },
  { "en": "Fungsi matriks transformasi homogen?", "id": "Mengubah koordinat dari satu frame ke frame lain." },
  { "en": "Bagian rotasi dari matriks homogen?", "id": "Sub-matriks 3x3 di kiri atas." },
  { "en": "Bagian translasi dari matriks homogen?", "id": "Vektor 3x1 di kanan atas." },
  { "en": "Apa itu Jacobian?", "id": "Matriks yang menghubungkan kecepatan sendi dan end-effector." },
  { "en": "Jacobian digunakan untuk apa?", "id": "Analisis kecepatan, singularitas, dan statika." },
  { "en": "Apa itu singularitas?", "id": "Konfigurasi dimana Jacobian kehilangan rank." },
  { "en": "Efek singularitas?", "id": "Robot kehilangan kemampuan bergerak ke arah tertentu." },
  { "en": "Contoh singularitas lengan?", "id": "Saat lengan terentang lurus sepenuhnya." },
  { "en": "Apa itu manipulability ellipsoid?", "id": "Visualisasi kemampuan robot bergerak ke berbagai arah." },
  { "en": "Apa itu perencanaan gerak (motion planning)?", "id": "Menemukan jalur gerak dari awal ke tujuan." },
  { "en": "Tujuan utama motion planning?", "id": "Menghindari tabrakan dengan halangan." },
  { "en": "Apa itu ruang konfigurasi (C-space)?", "id": "Ruang dari semua kemungkinan konfigurasi robot." },
  { "en": "Apa itu halangan di C-space?", "id": "Daerah di C-space yang merepresentasikan tabrakan." },
  { "en": "Apa itu path?", "id": "Kurva kontinu di C-space." },
  { "en": "Apa itu trajectory?", "id": "Path yang memiliki informasi waktu (kecepatan)." },
  { "en": "Apa itu perencanaan jalur (path planning)?", "id": "Hanya mencari geometri jalur." },
  { "en": "Apa itu perencanaan trajektori?", "id": "Menentukan profil kecepatan di sepanjang jalur." },
  { "en": "Apa itu bug algorithm?", "id": "Algoritma path planning sederhana dengan mengikuti halangan." },
  { "en": "Apa itu potential field method?", "id": "Robot ditarik ke tujuan dan ditolak oleh halangan." },
  { "en": "Masalah potential field method?", "id": "Dapat terjebak di minimum lokal." },
  { "en": "Apa itu visibility graph?", "id": "Metode path planning berbasis graf." },
  { "en": "Apa itu cell decomposition?", "id": "Membagi C-space menjadi sel-sel sederhana." },
  { "en": "Apa itu Rapidly-exploring Random Tree (RRT)?", "id": "Algoritma sampling-based untuk path planning." },
  { "en": "Keuntungan RRT?", "id": "Efisien untuk masalah berdimensi tinggi." },
  { "en": "Apa itu Probabilistic Roadmap (PRM)?", "id": "Metode sampling-based lain yang membangun peta." },
  { "en": "Apa itu interpolasi?", "id": "Membuat titik-titik antara di sepanjang path." },
  { "en": "Interpolasi linear?", "id": "Gerakan garis lurus di joint space." },
  { "en": "Interpolasi linear Cartesian?", "id": "Gerakan garis lurus di task space." },
  { "en": "Apa itu polynomial interpolation?", "id": "Menggunakan polinomial untuk jalur yang lebih mulus." },
  { "en": "Apa itu 'spline'?", "id": "Kurva mulus yang melewati titik-titik tertentu." },
  { "en": "Apa itu trajectory 'bang-bang'?", "id": "Trajektori dengan akselerasi maksimum atau minimum." },
  { "en": "Apa itu trajectory berbentuk-S?", "id": "Trajektori dengan profil kecepatan berbentuk lonceng." },
  { "en": "Apa itu optimisasi trajektori?", "id": "Mencari trajektori terbaik (misalnya, tercepat)." },
  { "en": "Apa itu kontrol posisi?", "id": "Mengontrol posisi setiap sendi secara independen." },
  { "en": "Apa itu kontrol gaya?", "id": "Mengontrol gaya yang diterapkan oleh end-effector." },
  { "en": "Kapan kontrol gaya dibutuhkan?", "id": "Saat berinteraksi dengan permukaan (menulis, menggosok)." },
  { "en": "Apa itu kontrol impedansi?", "id": "Mengontrol hubungan dinamis antara posisi dan gaya." },
  { "en": "Apa itu kontrol hibrida posisi/gaya?", "id": "Mengontrol posisi di beberapa arah, gaya di arah lain." },
  { "en": "Apa itu dinamika robot?", "id": "Studi tentang gerak robot yang melibatkan gaya." },
  { "en": "Apa itu dinamika maju?", "id": "Menghitung akselerasi dari torsi yang diberikan." },
  { "en": "Apa itu dinamika balik?", "id": "Menghitung torsi yang dibutuhkan untuk akselerasi." },
  { "en": "Dinamika balik digunakan untuk apa?", "id": "Kontrol robot (computed torque control)." },
  { "en": "Formulasi Lagrange?", "id": "Metode berbasis energi untuk menurunkan dinamika." },
  { "en": "Formulasi Newton-Euler?", "id": "Metode berbasis gaya dan torsi." },
  { "en": "Apa itu momen inersia?", "id": "Resistansi objek terhadap percepatan angular." },
  { "en": "Apa itu gaya sentrifugal?", "id": "Gaya fiktif yang muncul dalam gerak melingkar." },
  { "en": "Apa itu gaya Coriolis?", "id": "Gaya akibat interaksi antara gerak rotasi dan linear." },
  { "en": "Apa itu kompensasi gravitasi?", "id": "Memberikan torsi untuk melawan efek gravitasi." },
  { "en": "Apa itu 'computed torque control'?", "id": "Menggunakan model dinamik untuk membatalkan non-linearitas." },
  { "en": "Apa itu lokalisasi?", "id": "Masalah menentukan di mana robot berada." },
  { "en": "Apa itu pemetaan (mapping)?", "id": "Masalah membangun peta lingkungan." },
  { "en": "Apa itu SLAM?", "id": "Simultaneous Localization and Mapping." },
  { "en": "Mengapa SLAM sulit?", "id": "Masalah 'ayam dan telur'." },
  { "en": "Apa itu filter Kalman (dalam robotika)?", "id": "Digunakan untuk estimasi posisi dan lokalisasi." },
  { "en": "Apa itu EKF SLAM?", "id": "SLAM menggunakan Extended Kalman Filter." },
  { "en": "Apa itu Particle Filter SLAM (FastSLAM)?", "id": "SLAM menggunakan metode partikel." },
  { "en": "Apa itu GraphSLAM?", "id": "Memformulasikan SLAM sebagai masalah optimisasi graf." },
  { "en": "Apa itu loop closure?", "id": "Mengenali kembali tempat yang pernah dikunjungi." },
  { "en": "Mengapa loop closure penting?", "id": "Untuk mengoreksi akumulasi error." },
  { "en": "Apa itu navigasi?", "id": "Proses bergerak dari satu titik ke titik lain." },
  { "en": "Apa itu obstacle avoidance?", "id": "Menghindari tabrakan saat bergerak." },
  { "en": "Apa itu 'dead reckoning'?", "id": "Estimasi posisi hanya berdasarkan sensor internal." },
  { "en": "Masalah 'dead reckoning'?", "id": "Error terakumulasi seiring waktu." },
  { "en": "Apa itu odometry?", "id": "Menggunakan putaran roda untuk mengestimasi gerakan." },
  { "en": "Apa itu Inertial Measurement Unit (IMU)?", "id": "Sensor yang mengukur percepatan dan kecepatan angular." },
  { "en": "Apa itu sensor fusion?", "id": "Menggabungkan data dari berbagai sensor." },
  { "en": "Apa itu kalibrasi?", "id": "Menentukan parameter dari model (misalnya, robot, kamera)." },
  { "en": "Apa itu 'hand-eye' calibration?", "id": "Menentukan hubungan antara kamera dan gripper." },
  { "en": "Apa itu 'pick and place'?", "id": "Operasi robot industri yang paling umum." },
  { "en": "Apa itu Computer-Integrated Manufacturing (CIM)?", "id": "Integrasi total proses manufaktur dengan komputer." },
  { "en": "Apa itu 'lights-out' manufacturing?", "id": "Pabrik yang beroperasi sepenuhnya secara otomatis." },
  { "en": "Apa itu sistem visi?", "id": "Sistem yang memberikan kemampuan 'melihat' pada robot." },
  { "en": "Apa itu pemrosesan citra?", "id": "Menganalisis dan memanipulasi gambar digital." },
  { "en": "Apa itu segmentasi?", "id": "Memisahkan objek dari latar belakang." },
  { "en": "Apa itu pengenalan objek?", "id": "Mengidentifikasi objek apa yang ada dalam gambar." },
  { "en": "Apa itu 'bin picking'?", "id": "Tugas mengambil objek acak dari sebuah wadah." },
  { "en": "Apa itu artificial intelligence (AI) dalam robotika?", "id": "Memberikan kemampuan belajar dan mengambil keputusan." },
  { "en": "Apa itu machine learning dalam robotika?", "id": "Robot belajar dari pengalaman atau data." },
  { "en": "Apa itu reinforcement learning (RL)?", "id": "Belajar melalui trial-and-error dengan reward." },
  { "en": "Apa itu 'learning from demonstration'?", "id": "Robot belajar dengan meniru manusia." },
  { "en": "Apa itu robotika awan (cloud robotics)?", "id": "Memanfaatkan komputasi awan untuk robot." },
  { "en": "Apa konsep stabilitas terpenting?", "id": "Kemampuan sistem untuk kembali ke kesetimbangan." },
  { "en": "Apa itu stabilitas BIBO?", "id": "Bounded-Input, Bounded-Output." },
  { "en": "Definisi stabilitas BIBO?", "id": "Setiap input terbatas menghasilkan output terbatas." },
  { "en": "Apa itu stabilitas internal?", "id": "Semua sinyal internal dalam sistem terbatas." },
  { "en": "Apa itu stabilitas asimtotik?", "id": "Sistem kembali ke kesetimbangan dari kondisi awal." },
  { "en": "Hubungan stabilitas dan lokasi pole?", "id": "Pole menentukan stabilitas sistem." },
  { "en": "Kondisi stabilitas (pole)?", "id": "Semua pole berada di setengah bidang kiri." },
  { "en": "Pole di sumbu jw menyebabkan apa?", "id": "Sistem marginally stable atau osilasi." },
  { "en": "Pole di bidang kanan menyebabkan apa?", "id": "Sistem tidak stabil." },
  { "en": "Bagaimana jika ada pole ganda di sumbu jw?", "id": "Sistem tidak stabil." },
  { "en": "Apa itu Kriteria Stabilitas Routh-Hurwitz?", "id": "Metode aljabar untuk mengecek stabilitas." },
  { "en": "Keuntungan Routh-Hurwitz?", "id": "Tidak perlu menghitung akar (pole)." },
  { "en": "Informasi apa yang diberikan Routh-Hurwitz?", "id": "Jumlah pole di setiap setengah bidang." },
  { "en": "Bagaimana cara membuat tabel Routh?", "id": "Dari koefisien persamaan karakteristik." },
  { "en": "Kondisi perlu untuk stabilitas?", "id": "Semua koefisien harus ada dan positif." },
  { "en": "Kondisi cukup untuk stabilitas (Routh)?", "id": "Tidak ada perubahan tanda di kolom pertama." },
  { "en": "Apa arti perubahan tanda di kolom pertama?", "id": "Ada pole di setengah bidang kanan." },
  { "en": "Kasus spesial Routh: nol di kolom pertama?", "id": "Ganti nol dengan epsilon kecil." },
  { "en": "Kasus spesial Routh: seluruh baris nol?", "id": "Ada pole simetris terhadap titik origin." },
  { "en": "Apa itu persamaan bantu (auxiliary)?", "id": "Dibuat dari baris sebelum baris nol." },
  { "en": "Akar dari persamaan bantu adalah?", "id": "Pole simetris dari sistem." },
  { "en": "Apa itu Root Locus?", "id": "Plot lintasan pole loop tertutup." },
  { "en": "Root Locus diplot terhadap apa?", "id": "Perubahan satu parameter, biasanya gain K." },
  { "en": "Tujuan Root Locus?", "id": "Menganalisis stabilitas dan respons transien." },
  { "en": "Awal Root Locus (K=0)?", "id": "Berawal dari pole loop terbuka." },
  { "en": "Akhir Root Locus (K=tak hingga)?", "id": "Berakhir di zero loop terbuka." },
  { "en": "Berapa jumlah cabang Root Locus?", "id": "Sama dengan jumlah pole loop terbuka." },
  { "en": "Apakah Root Locus simetris?", "id": "Ya, simetris terhadap sumbu real." },
  { "en": "Bagian sumbu real mana yang termasuk locus?", "id": "Di sebelah kiri jumlah ganjil pole/zero." },
  { "en": "Apa itu asimtot?", "id": "Garis lurus yang didekati cabang locus." },
  { "en": "Kapan asimtot ada?", "id": "Jika jumlah pole lebih besar dari zero." },
  { "en": "Jumlah asimtot?", "id": "Selisih antara jumlah pole dan zero." },
  { "en": "Apa itu titik pusat (centroid)?", "id": "Titik potong asimtot di sumbu real." },
  { "en": "Apa itu titik pisah (breakaway point)?", "id": "Titik dimana locus meninggalkan sumbu real." },
  { "en": "Apa itu titik masuk (break-in point)?", "id": "Titik dimana locus memasuki sumbu real." },
  { "en": "Bagaimana mencari titik pisah/masuk?", "id": "Turunan gain K terhadap s sama dengan nol." },
  { "en": "Apa itu sudut datang (angle of arrival)?", "id": "Sudut locus saat tiba di zero kompleks." },
  { "en": "Apa itu sudut berangkat (angle of departure)?", "id": "Sudut locus saat meninggalkan pole kompleks." },
  { "en": "Bagaimana mencari perpotongan sumbu jw?", "id": "Menggunakan tabel Routh atau substitusi s=jw." },
  { "en": "Gain K pada perpotongan sumbu jw menunjukkan apa?", "id": "Batas stabilitas sistem." },
  { "en": "Frekuensi pada perpotongan sumbu jw menunjukkan apa?", "id": "Frekuensi osilasi saat sistem marginal." },
  { "en": "Bagaimana Root Locus membantu desain?", "id": "Menentukan gain K untuk respons yang diinginkan." },
  { "en": "Titik pada Root Locus merepresentasikan apa?", "id": "Pole loop tertutup untuk suatu nilai K." },
  { "en": "Bagaimana menentukan K di suatu titik?", "id": "Menggunakan kondisi magnitudo." },
  { "en": "Apa itu Kriteria Stabilitas Nyquist?", "id": "Metode grafis lain untuk mengecek stabilitas." },
  { "en": "Dasar dari plot Nyquist?", "id": "Respons frekuensi loop terbuka." },
  { "en": "Apa itu contour Nyquist?", "id": "Lintasan di bidang-s yang melingkupi bidang kanan." },
  { "en": "Apa itu prinsip argumen?", "id": "Dasar matematis dari kriteria Nyquist." },
  { "en": "Kriteria Nyquist menghubungkan apa?", "id": "Putaran, zero, dan pole." },
  { "en": "Putaran N pada plot Nyquist?", "id": "Jumlah putaran mengelilingi titik -1." },
  { "en": "Pole P pada kriteria Nyquist?", "id": "Jumlah pole loop terbuka di bidang kanan." },
  { "en": "Zero Z pada kriteria Nyquist?", "id": "Jumlah pole loop tertutup di bidang kanan." },
  { "en": "Kondisi stabilitas Nyquist?", "id": "Z = 0 (tidak ada pole loop tertutup di RHP)." },
  { "en": "Keuntungan Nyquist?", "id": "Dapat menangani sistem non-minimum phase." },
  { "en": "Apa itu stabilitas relatif?", "id": "Seberapa jauh sistem dari ketidakstabilan." },
  { "en": "Apa itu Gain Margin (GM)?", "id": "Faktor penguatan sebelum sistem tidak stabil." },
  { "en": "Bagaimana membaca GM dari plot Bode?", "id": "Jarak dari 0 dB saat fasa -180." },
  { "en": "Bagaimana membaca GM dari plot Nyquist?", "id": "Kebalikan dari perpotongan sumbu real negatif." },
  { "en": "Apa itu Phase Margin (PM)?", "id": "Tambahan fasa sebelum sistem tidak stabil." },
  { "en": "Bagaimana membaca PM dari plot Bode?", "id": "Jarak dari -180 saat gain 0 dB." },
  { "en": "Bagaimana membaca PM dari plot Nyquist?", "id": "Sudut dari sumbu real negatif." },
  { "en": "PM yang kecil menunjukkan apa?", "id": "Respons yang sangat berosilasi." },
  { "en": "PM yang besar menunjukkan apa?", "id": "Respons yang lambat dan teredam baik." },
  { "en": "Apa itu stabilitas kondisional?", "id": "Sistem stabil hanya untuk rentang gain tertentu." },
  { "en": "Sistem absolut stabil?", "id": "Stabil untuk semua nilai gain K." },
  { "en": "Apa itu stabilitas Lyapunov?", "id": "Metode analisis stabilitas sistem non-linear." },
  { "en": "Apa itu fungsi Lyapunov?", "id": "Fungsi skalar seperti energi." },
  { "en": "Kondisi stabilitas Lyapunov?", "id": "Fungsi positif dan turunannya negatif." },
  { "en": "Apa itu domain atraksi?", "id": "Wilayah dimana sistem akan kembali stabil." },
  { "en": "Apa itu stabilitas eksponensial?", "id": "Sistem konvergen ke kesetimbangan secara eksponensial." },
  { "en": "Apa itu stabilitas untuk sistem waktu diskrit?", "id": "Pole harus berada di dalam lingkaran satuan." },
  { "en": "Lingkaran satuan di bidang-z analog dengan apa?", "id": "Setengah bidang kiri di bidang-s." },
  { "en": "Bagaimana jika pole di luar lingkaran satuan?", "id": "Sistem waktu diskrit tidak stabil." },
  { "en": "Bagaimana jika pole di atas lingkaran satuan?", "id": "Sistem waktu diskrit marginal." },
  { "en": "Kriteria Stabilitas Jury?", "id": "Analogi Routh-Hurwitz untuk sistem diskrit." },
  { "en": "Apa itu zero-input response?", "id": "Respons sistem hanya dari kondisi awal." },
  { "en": "Stabilitas zero-input?", "id": "Respons konvergen ke nol tanpa input." },
  { "en": "Apa itu zero-state response?", "id": "Respons sistem hanya dari input." },
  { "en": "Apa itu sistem minimum phase?", "id": "Semua zero berada di setengah bidang kiri." },
  { "en": "Apa itu sistem all-pass?", "id": "Sistem yang hanya mengubah fasa sinyal." },
  { "en": "Lokasi pole sistem all-pass?", "id": "Simetris terhadap sumbu imajiner." },
  { "en": "Magnitudo sistem all-pass?", "id": "Selalu konstan (biasanya 1)." },
  { "en": "Stabilitas dari sistem state-space?", "id": "Ditentukan oleh eigenvalue dari matriks A." },
  { "en": "Kondisi stabilitas (state-space)?", "id": "Semua eigenvalue A memiliki bagian real negatif." },
  { "en": "Apa itu mode terkontrol?", "id": "Mode sistem yang dapat dipengaruhi input." },
  { "en": "Apa itu mode teramati?", "id": "Mode sistem yang dapat dilihat di output." },
  { "en": "Pembatalan pole-zero tidak stabil?", "id": "Jika pole dan zero di bidang kanan." },
  { "en": "Stabilitas struktural?", "id": "Stabilitas tidak berubah oleh variasi parameter kecil." },
  { "en": "Apa itu osilator?", "id": "Sistem yang dirancang untuk menjadi marginally stable." },
  { "en": "Contoh osilator?", "id": "Rangkaian LC, pendulum." },
  { "en": "Bagaimana gain mempengaruhi stabilitas?", "id": "Gain yang terlalu tinggi seringkali menyebabkan ketidakstabilan." },
  { "en": "Bagaimana waktu tunda mempengaruhi stabilitas?", "id": "Waktu tunda selalu mengurangi stabilitas." },
  { "en": "Efek waktu tunda pada fasa?", "id": "Menyebabkan pergeseran fasa negatif yang besar." },
  { "en": "Apa itu 'gain' pada Root Locus?", "id": "Parameter K yang bervariasi dari 0 hingga tak hingga." },
  { "en": "Apa itu 'akar' pada Root Locus?", "id": "Pole dari sistem loop tertutup." },
  { "en": "Apa itu 'locus' pada Root Locus?", "id": "Jalur atau lintasan yang dilalui akar." },
  { "en": "Root Locus untuk umpan balik positif?", "id": "Aturan sudutnya menjadi 0 derajat." },
  { "en": "Kapan stabilitas tidak jadi perhatian utama?", "id": "Pada beberapa sistem loop terbuka." }



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
