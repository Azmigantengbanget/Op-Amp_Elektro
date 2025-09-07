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


    { "en": "Apa Itu 'Operational Amplifier' (Op-Amp)?", "id": "Penguat Diferensial Tegangan Penguatan Tinggi." },
    { "en": "Apa Simbol Skematik Op-Amp?", "id": "Segitiga Dengan Dua Input Satu Output." },
    { "en": "Apa Nama Input Positif Op-Amp?", "id": "Input Non-Inverting (Tak Membalik)." },
    { "en": "Apa Nama Input Negatif Op-Amp?", "id": "Input Inverting (Membalik)." },
    { "en": "Apa Fungsi Terminal V+ Op-Amp?", "id": "Terminal Catu Daya Positif." },
    { "en": "Apa Fungsi Terminal V- Op-Amp?", "id": "Terminal Catu Daya Negatif." },
    { "en": "Berapa Impedansi Input Op-Amp Ideal?", "id": "Tak Terhingga (Infinite)." },
    { "en": "Berapa Impedansi Output Op-Amp Ideal?", "id": "Nol (Zero)." },
    { "en": "Berapa Penguatan Tegangan Loop Terbuka Ideal?", "id": "Tak Terhingga (Infinite)." },
    { "en": "Berapa Bandwidth Op-Amp Ideal?", "id": "Tak Terhingga (Infinite)." },
    { "en": "Apa Itu 'Virtual Ground'?", "id": "Titik Dengan Tegangan Nol Volt." },
    { "en": "Kapan 'Virtual Ground' Terjadi?", "id": "Saat Umpan Balik Negatif Diterapkan." },
    { "en": "Apa Itu 'Umpan Balik Negatif' (Negative Feedback)?", "id": "Keluaran Terhubung Ke Input Inverting." },
    { "en": "Apa Fungsi Umpan Balik Negatif?", "id": "Menstabilkan Penguatan Dan Operasi Op-Amp." },
    { "en": "Apa Itu 'Penguat Inverting'?", "id": "Menguatkan Dan Membalik Fasa Sinyal." },
    { "en": "Bagaimana Sinyal Masuk Penguat Inverting?", "id": "Melalui Input Inverting (-)." },
    { "en": "Bagaimana Rumus Gain Penguat Inverting?", "id": "Gain = -Rf / Rin." },
    { "en": "Apa Itu 'Penguat Non-Inverting'?", "id": "Menguatkan Tanpa Membalik Fasa Sinyal." },
    { "en": "Bagaimana Sinyal Masuk Penguat Non-Inverting?", "id": "Melalui Input Non-Inverting (+)." },
    { "en": "Bagaimana Rumus Gain Penguat Non-Inverting?", "id": "Gain = 1 + (Rf / Rin)." },
    { "en": "Apa Itu 'Voltage Follower' (Buffer)?", "id": "Penguat Dengan Gain Sama Dengan Satu." },
    { "en": "Apa Fungsi Utama Voltage Follower?", "id": "Penyangga Impedansi (Impedance Buffering)." },
    { "en": "Apa Itu 'Penguat Penjumlah' (Summing Amplifier)?", "id": "Menjumlahkan Beberapa Sinyal Tegangan Masukan." },
    { "en": "Apa Konfigurasi Dasar Penguat Penjumlah?", "id": "Konfigurasi Inverting Dengan Banyak Input." },
    { "en": "Apa Itu 'Penguat Diferensial'?", "id": "Menguatkan Selisih Antara Dua Tegangan." },
    { "en": "Apa Kegunaan Penguat Diferensial?", "id": "Menolak Sinyal Common-mode (Noise)." },
    { "en": "Apa Itu 'CMRR' (Common-Mode Rejection Ratio)?", "id": "Kemampuan Op-Amp Menolak Sinyal Common-mode." },
    { "en": "Berapa Nilai CMRR Op-Amp Ideal?", "id": "Tak Terhingga (Infinite)." },
    { "en": "Apa Itu 'Slew Rate'?", "id": "Laju Perubahan Tegangan Output Maksimum." },
    { "en": "Apa Satuan Slew Rate?", "id": "Volt Per Mikrodetik (V/µs)." },
    { "en": "Apa Akibat Slew Rate Terbatas?", "id": "Distorsi Pada Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu 'Tegangan Offset Input'?", "id": "Tegangan Diferensial Untuk Output Nol." },
    { "en": "Apa Penyebab Tegangan Offset Input?", "id": "Ketidaksempurnaan Internal Transistor." },
    { "en": "Apa Itu 'Arus Bias Input'?", "id": "Arus DC Kecil Ke Terminal Input." },
    { "en": "Mengapa Arus Bias Input Ada?", "id": "Dibutuhkan Untuk Membias Transistor Internal." },
    { "en": "Apa Itu 'Arus Offset Input'?", "id": "Selisih Antara Dua Arus Bias Input." },
    { "en": "Apa Itu 'Gain-Bandwidth Product' (GBP)?", "id": "Hasil Kali Gain Dan Bandwidth." },
    { "en": "Apa Sifat GBP Pada Op-Amp?", "id": "Nilainya Cenderung Konstan." },
    { "en": "Apa Itu 'Saturasi' (Saturation)?", "id": "Kondisi Output Op-Amp Mencapai Batas." },
    { "en": "Kapan Saturasi Terjadi?", "id": "Saat Output Mendekati Tegangan Catu Daya." },
    { "en": "Apa Itu 'Tegangan Swing Output'?", "id": "Rentang Tegangan Output Maksimum." },
    { "en": "Apa Itu 'Komparator' (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
    { "en": "Bagaimana Op-Amp Digunakan Sebagai Komparator?", "id": "Tanpa Menggunakan Umpan Balik Negatif." },
    { "en": "Apa Keluaran Komparator?", "id": "Tegangan Saturasi Positif Atau Negatif." },
    { "en": "Apa Itu 'Integrator'?", "id": "Menghasilkan Output Integral Dari Input." },
    { "en": "Komponen Apa Di Jalur Feedback Integrator?", "id": "Sebuah Kapasitor." },
    { "en": "Apa Itu 'Differentiator'?", "id": "Menghasilkan Output Turunan Dari Input." },
    { "en": "Komponen Apa Di Jalur Input Differentiator?", "id": "Sebuah Kapasitor." },
    { "en": "Masalah Apa Pada Differentiator Ideal?", "id": "Tidak Stabil Pada Frekuensi Tinggi." },
    { "en": "Apa Itu 'Filter Aktif'?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
    { "en": "Apa Keuntungan Filter Aktif?", "id": "Dapat Memberikan Penguatan (Gain)." },
    { "en": "Apa Itu 'Filter Low-Pass'?", "id": "Melewatkan Frekuensi Rendah." },
    { "en": "Apa Itu 'Filter High-Pass'?", "id": "Melewatkan Frekuensi Tinggi." },
    { "en": "Apa Itu 'Filter Band-Pass'?", "id": "Melewatkan Pita Frekuensi Tertentu." },
    { "en": "Apa Itu 'Frekuensi Cutoff'?", "id": "Frekuensi Batas Operasi Sebuah Filter." },
    { "en": "Apa Itu 'Osilator'?", "id": "Rangkaian Pembangkit Sinyal Periodik." },
    { "en": "Apa Syarat Terjadinya Osilasi?", "id": "Umpan Balik Positif Dengan Gain Satu." },
    { "en": "Apa Itu 'Kriteria Barkhausen'?", "id": "Dua Syarat Untuk Terjadinya Osilasi." },
    { "en": "Apa Itu 'PSRR' (Power Supply Rejection Ratio)?", "id": "Kemampuan Menolak Derau Catu Daya." },
    { "en": "Apa Itu 'Open-Loop Gain'?", "id": "Penguatan Tegangan Tanpa Umpan Balik." },
    { "en": "Apa Itu 'Closed-Loop Gain'?", "id": "Penguatan Tegangan Dengan Umpan Balik." },
    { "en": "Apa Hubungan Open-Loop dan Closed-Loop Gain?", "id": "Closed-Loop Selalu Lebih Kecil." },
    { "en": "Apa Itu 'Derau' (Noise) Dalam Op-Amp?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
    { "en": "Apa Satuan Kerapatan Derau Tegangan?", "id": "Nanovolt Per Akar Hertz (nV/√Hz)." },
    { "en": "Apa Itu 'Stabilitas' Dalam Rangkaian Op-Amp?", "id": "Kemampuan Untuk Tidak Berosilasi." },
    { "en": "Apa Itu 'Margin Fasa' (Phase Margin)?", "id": "Ukuran Stabilitas Rangkaian Umpan Balik." },
    { "en": "Berapa Margin Fasa Yang Baik?", "id": "Biasanya Diatas 45 Derajat." },
    { "en": "Apa Itu 'Kompensasi Frekuensi'?", "id": "Teknik Untuk Menstabilkan Op-Amp." },
    { "en": "Apa Itu 'Op-Amp Ideal'?", "id": "Model Teoritis Dengan Karakteristik Sempurna." },
    { "en": "Apa Itu 'Op-Amp Praktis'?", "id": "Op-Amp Nyata Dengan Keterbatasan." },
    { "en": "Apa Itu 'Datasheet' Op-Amp?", "id": "Dokumen Spesifikasi Teknis Dari Pabrikan." },
    { "en": "Apa Tipe Kemasan Op-Amp Umum?", "id": "DIP (Dual In-line Package)." },
    { "en": "Contoh Op-Amp Populer?", "id": "LM741 Atau TL072." },
    { "en": "Apa Itu 'Rail-to-Rail' Op-Amp?", "id": "Output Dapat Mencapai Tegangan Catu Daya." },
    { "en": "Apa Keuntungan Op-Amp Rail-to-Rail?", "id": "Rentang Dinamis Output Yang Lebih Luas." },
    { "en": "Apa Itu 'Op-Amp Presisi'?", "id": "Op-Amp Dengan Offset Sangat Rendah." },
    { "en": "Apa Itu 'Op-Amp Daya Rendah'?", "id": "Op-Amp Dengan Konsumsi Arus Rendah." },
    { "en": "Apa Itu 'Op-Amp Kecepatan Tinggi'?", "id": "Op-Amp Dengan Slew Rate Tinggi." },
    { "en": "Apa Itu 'Instrumentation Amplifier'?", "id": "Penguat Diferensial Presisi Tinggi." },
    { "en": "Berapa Input Pada Instrumentation Amplifier?", "id": "Tiga Op-Amp Dalam Satu Konfigurasi." },
    { "en": "Apa Itu 'Penguat Logaritmik'?", "id": "Outputnya Adalah Logaritma Dari Input." },
    { "en": "Apa Itu 'Penguat Anti-Logaritmik'?", "id": "Outputnya Eksponensial Dari Input." },
    { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Komparator Dengan Histeresis." },
    { "en": "Apa Fungsi Histeresis?", "id": "Mencegah Transisi Cepat Akibat Derau." },
    { "en": "Apa Itu 'Penyearah Presisi' (Precision Rectifier)?", "id": "Menyearahkan Sinyal AC Sangat Kecil." },
    { "en": "Apa Itu 'Zero Crossing Detector'?", "id": "Mendeteksi Saat Sinyal Melewati Nol." },
    { "en": "Apa Itu 'Peak Detector'?", "id": "Menangkap Dan Menahan Nilai Puncak." },
    { "en": "Apa Itu 'Sample-and-Hold Amplifier'?", "id": "Mencuplik Dan Menahan Tegangan Input." },
    { "en": "Apa Itu 'Current-to-Voltage Converter'?", "id": "Mengubah Sinyal Arus Menjadi Tegangan." },
    { "en": "Apa Nama Lain Current-to-Voltage Converter?", "id": "Penguat Transimpedansi." },
    { "en": "Apa Itu 'Voltage-to-Current Converter'?", "id": "Mengubah Sinyal Tegangan Menjadi Arus." },
    { "en": "Apa Nama Lain Voltage-to-Current Converter?", "id": "Penguat Transkonduktansi." },
    { "en": "Apa Itu 'Active Clamp Circuit'?", "id": "Membatasi Tegangan Sinyal Pada Level." },
    { "en": "Apa Itu 'Gyrator'?", "id": "Mensimulasikan Induktor Menggunakan Kapasitor." },
    { "en": "Mengapa Mensimulasikan Induktor?", "id": "Induktor Sulit Dibuat Dalam IC." },
    { "en": "Apa Itu 'Negative Impedance Converter' (NIC)?", "id": "Membuat Impedansi Bernilai Negatif." },
    { "en": "Apa Itu 'Bode Plot'?", "id": "Grafik Respon Frekuensi (Gain/Fasa)." },
    { "en": "Apa Itu 'Decade' Dalam Frekuensi?", "id": "Perubahan Frekuensi Sepuluh Kali Lipat." },
    { "en": "Apa Itu 'Octave' Dalam Frekuensi?", "id": "Perubahan Frekuensi Dua Kali Lipat." },
    { "en": "Apa Itu 'Slope' Dalam Bode Plot?", "id": "Tingkat Perubahan Gain Terhadap Frekuensi." },
    { "en": "Berapa Slope Umum Filter Orde Pertama?", "id": "-20 dB Per Dekade." },
    { "en": "Apa Itu 'Penguatan Diferensial' (Ad)?", "id": "Penguatan Untuk Selisih Sinyal Input." },
    { "en": "Apa Itu 'Penguatan Common-mode' (Ac)?", "id": "Penguatan Untuk Sinyal Yang Sama." },
    { "en": "Bagaimana Rumus CMRR Dalam dB?", "id": "20 log (Ad / Ac)." },
    { "en": "Apa Itu 'Single-Supply' Op-Amp?", "id": "Op-Amp Beroperasi Dengan Satu Catu Daya." },
    { "en": "Apa Itu 'Dual-Supply' Op-Amp?", "id": "Op-Amp Beroperasi Dengan Catu Daya Ganda." },
    { "en": "Apa Itu 'Virtual Short Circuit'?", "id": "Tegangan Antara Input Op-Amp Nol." },
    { "en": "Kapan Aturan Virtual Short Berlaku?", "id": "Dalam Konfigurasi Umpan Balik Negatif." },
    { "en": "Apa Itu 'Pemisahan Kanal' (Channel Separation)?", "id": "Ukuran Isolasi Antar Op-Amp Ganda." },
    { "en": "Apa Itu 'Op-Amp Ganda' (Dual Op-Amp)?", "id": "Dua Op-Amp Dalam Satu Kemasan IC." },
    { "en": "Apa Itu 'Op-Amp Kuad' (Quad Op-Amp)?", "id": "Empat Op-Amp Dalam Satu Kemasan IC." },
    { "en": "Apa Itu 'Total Harmonic Distortion' (THD)?", "id": "Ukuran Distorsi Sinyal Harmonik." },
    { "en": "Apa Itu 'Settling Time'?", "id": "Waktu Output Stabil Setelah Perubahan." },
    { "en": "Apa Itu 'Overshoot'?", "id": "Output Melebihi Nilai Akhir Sesaat." },
    { "en": "Apa Itu 'Ringing'?", "id": "Osilasi Yang Mereda Pada Output." },
    { "en": "Apa Itu 'Sinyal Input Common-Mode'?", "id": "Rata-rata Dari Dua Tegangan Input." },
    { "en": "Apa Itu 'Rentang Tegangan Input Common-Mode'?", "id": "Rentang Tegangan Input Yang Diizinkan." },
    { "en": "Apa Itu 'JFET Input' Op-Amp?", "id": "Op-Amp Menggunakan JFET Di Input." },
    { "en": "Apa Keuntungan Op-Amp Input JFET?", "id": "Arus Bias Input Sangat Rendah." },
    { "en": "Apa Itu 'CMOS' Op-Amp?", "id": "Op-Amp Terbuat Dari Teknologi CMOS." },
    { "en": "Apa Keuntungan Op-Amp CMOS?", "id": "Konsumsi Daya Sangat Rendah." },
    { "en": "Apa Itu 'Bipolar Input' Op-Amp?", "id": "Op-Amp Menggunakan BJT Di Input." },
    { "en": "Apa Keuntungan Op-Amp Input Bipolar?", "id": "Derau Tegangan Umumnya Lebih Rendah." },
    { "en": "Apa Itu 'Pin Null Offset'?", "id": "Pin Untuk Mengatur Tegangan Offset." },
    { "en": "Bagaimana Cara Nulling Offset?", "id": "Menghubungkan Potensiometer Ke Pin Null." },
    { "en": "Apa Itu 'Bootstrap' Dalam Rangkaian?", "id": "Teknik Umpan Balik Positif." },
    { "en": "Apa Itu 'Differential Pair'?", "id": "Jantung Dari Tahap Input Op-Amp." },
    { "en": "Apa Itu 'Current Mirror'?", "id": "Sirkuit Untuk Menyalin Arus." },
    { "en": "Apa Fungsi Current Mirror Di Op-Amp?", "id": "Menyediakan Beban Aktif (Active Load)." },
    { "en": "Apa Itu 'Beban Aktif' (Active Load)?", "id": "Beban Menggunakan Transistor Bukan Resistor." },
    { "en": "Apa Keuntungan Beban Aktif?", "id": "Memberikan Penguatan Tegangan Sangat Tinggi." },
    { "en": "Apa Itu 'Tahap Output Push-Pull'?", "id": "Konfigurasi Output Umum Pada Op-Amp." },
    { "en": "Apa Itu 'Distorsi Crossover'?", "id": "Distorsi Pada Output Push-Pull Kelas B." },
    { "en": "Bagaimana Mengurangi Distorsi Crossover?", "id": "Menggunakan Konfigurasi Kelas AB." },
    { "en": "Apa Itu 'Proteksi Hubung Singkat'?", "id": "Fitur Internal Pelindung Output Op-Amp." },
    { "en": "Apa Itu 'Proteksi Termal' (Thermal Shutdown)?", "id": "Fitur Mematikan Op-Amp Saat Panas." },
    { "en": "Apa Itu 'Kapasitor Bypass Catu Daya'?", "id": "Kapasitor Antara Pin Catu Daya-Ground." },
    { "en": "Apa Fungsi Kapasitor Bypass?", "id": "Menyaring Derau Dari Catu Daya." },
    { "en": "Mengapa Jalur PCB Feedback Harus Pendek?", "id": "Mengurangi Kapasitansi Dan Induktansi Parasitik." },
    { "en": "Apa Itu 'Bidang Tanah' (Ground Plane)?", "id": "Lapisan Tembaga Luas Untuk Ground." },
    { "en": "Apa Keuntungan Ground Plane?", "id": "Mengurangi Derau Dan Impedansi Ground." },
    { "en": "Apa Itu 'Osilator Jembatan Wien'?", "id": "Osilator Gelombang Sinus Populer." },
    { "en": "Apa Itu 'Osilator Geser Fasa'?", "id": "Osilator Menggunakan Jaringan RC Geser Fasa." },
    { "en": "Apa Itu 'Multivibrator Astable'?", "id": "Rangkaian Op-Amp Pembangkit Gelombang Persegi." },
    { "en": "Apa Itu 'Filter Butterworth'?", "id": "Filter Dengan Respon Paling Datar." },
    { "en": "Apa Itu 'Filter Chebyshev'?", "id": "Filter Dengan Transisi Lebih Tajam." },
    { "en": "Apa Kelemahan Filter Chebyshev?", "id": "Memiliki Ripple Di Passband." },
    { "en": "Apa Itu 'Filter Bessel'?", "id": "Filter Dengan Respon Fasa Linear." },
    { "en": "Apa Itu 'Orde Filter'?", "id": "Menentukan Tingkat Ketajaman (Roll-off)." },
    { "en": "Berapa Slope Filter Orde Kedua?", "id": "-40 dB Per Dekade." },
    { "en": "Apa Itu 'Topologi Sallen-Key'?", "id": "Desain Populer Untuk Filter Aktif." },
    { "en": "Apa Itu 'Konverter Impedansi Negatif'?", "id": "Membuat Impedansi Negatif." },
    { "en": "Apa Itu 'Howland Current Source'?", "id": "Sumber Arus Terkendali Tegangan." },
    { "en": "Apa Itu 'Penguat Jembatan' (Bridge Amplifier)?", "id": "Menggandakan Swing Tegangan Output." },
    { "en": "Apa Itu 'Kopling AC' (AC Coupling)?", "id": "Menghalangi Komponen DC Dengan Kapasitor." },
    { "en": "Apa Itu 'Kopling DC' (DC Coupling)?", "id": "Melewatkan Semua Komponen Frekuensi." },
    { "en": "Apa Itu 'Voltage Clamp'?", "id": "Rangkaian Pembatas Level Tegangan." },
    { "en": "Apa Itu 'Window Comparator'?", "id": "Mendeteksi Tegangan Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Absolute Value Circuit'?", "id": "Sama Dengan Penyearah Presisi Gelombang Penuh." },
    { "en": "Apa Itu 'Log Ratio Amplifier'?", "id": "Outputnya Adalah Rasio Logaritmik Input." },
    { "en": "Apa Itu 'Op-Amp Terisolasi'?", "id": "Op-Amp Dengan Isolasi Galvanik." },
    { "en": "Kapan Op-Amp Terisolasi Digunakan?", "id": "Saat Mengukur Tegangan Tinggi Berbahaya." },
    { "en": "Apa Itu 'Chopper-Stabilized' Op-Amp?", "id": "Op-Amp Dengan Drift Offset Sangat Rendah." },
    { "en": "Apa Itu 'Auto-Zero' Op-Amp?", "id": "Op-Amp Yang Mengkalibrasi Offset Otomatis." },
    { "en": "Apa Itu 'Current-Feedback' Op-Amp (CFA)?", "id": "Op-Amp Dengan Arsitektur Berbeda." },
    { "en": "Apa Keuntungan CFA?", "id": "Slew Rate Sangat Tinggi." },
    { "en": "Apa Kelemahan CFA?", "id": "Akurasi DC Kurang Baik." },
    { "en": "Apa Itu 'Voltage-Feedback' Op-Amp (VFA)?", "id": "Jenis Op-Amp Paling Umum." },
    { "en": "Apa Itu 'Derating'?", "id": "Mengurangi Batas Operasi Karena Suhu." },
    { "en": "Apa Itu 'Disipasi Daya' (Power Dissipation)?", "id": "Energi Yang Diubah Op-Amp Jadi Panas." },
    { "en": "Apa Itu 'Resistansi Termal'?", "id": "Ukuran Hambatan Aliran Panas." },
    { "en": "Apa Itu 'Suhu Sambungan' (Junction Temperature)?", "id": "Suhu Internal Die Semikonduktor." },
    { "en": "Apa Itu 'Suhu Sekitar' (Ambient Temperature)?", "id": "Suhu Udara Di Sekitar Perangkat." },
    { "en": "Mengapa Perlu Heat Sink?", "id": "Untuk Membuang Panas Berlebih." },
    { "en": "Apa Itu 'Unity-Gain Stable'?", "id": "Op-Amp Stabil Pada Penguatan Satu." },
    { "en": "Apa Itu 'Decompensated' Op-Amp?", "id": "Tidak Stabil Pada Penguatan Rendah." },
    { "en": "Mengapa Menggunakan Op-Amp Decompensated?", "id": "Bandwidth Lebih Baik Pada Penguatan Tinggi." },
    { "en": "Apa Itu 'Pole' Dalam Respon Frekuensi?", "id": "Frekuensi Dimana Gain Mulai Turun." },
    { "en": "Apa Itu 'Zero' Dalam Respon Frekuensi?", "id": "Frekuensi Dimana Gain Mulai Naik." },
    { "en": "Apa Itu 'Loop Gain'?", "id": "Total Penguatan Dalam Loop Umpan Balik." },
    { "en": "Apa Itu 'Phase Splitter'?", "id": "Menghasilkan Dua Output Beda Fasa 180°." },
    { "en": "Apa Itu 'All-Pass Filter'?", "id": "Menggeser Fasa Tanpa Mengubah Amplitudo." },
    { "en": "Apa Kegunaan All-Pass Filter?", "id": "Koreksi Fasa Atau Penundaan Sinyal." },
    { "en": "Apa Itu 'Tegangan Saturasi Rendah'?", "id": "Saat Output Sangat Dekat V-." },
    { "en": "Apa Itu 'Tegangan Saturasi Tinggi'?", "id": "Saat Output Sangat Dekat V+." },
    { "en": "Apa Itu 'Keluaran Open-Loop'?", "id": "Keluaran Tanpa Koneksi Umpan Balik." },
    { "en": "Apa Itu 'Penguat Terprogram' (PGA)?", "id": "Penguat Dengan Gain Terkendali Digital." },
    { "en": "Apa Itu 'Current Source'?", "id": "Rangkaian Yang Memberikan Arus Konstan." },
    { "en": "Apa Itu 'Current Sink'?", "id": "Rangkaian Yang Menerima Arus Konstan." },
    { "en": "Apa Itu 'Mute Circuit'?", "id": "Rangkaian Untuk Membisukan Sinyal Audio." },
    { "en": "Apa Itu 'Overload Recovery Time'?", "id": "Waktu Pulih Dari Kondisi Saturasi." },
    { "en": "Bagaimana Op-Amp Mengukur Arus?", "id": "Mengukur Jatuh Tegangan Di Resistor Shunt." },
    { "en": "Apa Itu 'Penguat Sisi Tinggi'?", "id": "Mengukur Arus Di Jalur Positif." },
    { "en": "Apa Itu 'Penguat Sisi Rendah'?", "id": "Mengukur Arus Di Jalur Ground." },
    { "en": "Apa Itu 'Input Over-Voltage Protection'?", "id": "Melindungi Input Dari Tegangan Berlebih." },
    { "en": "Apa Itu 'Fully-Differential Amplifier' (FDA)?", "id": "Op-Amp Dengan Input Dan Output Diferensial." },
    { "en": "Apa Keuntungan FDA?", "id": "Kinerja Derau Dan Distorsi Lebih Baik." },
    { "en": "Apa Itu 'Tegangan Output Common-Mode'?", "id": "Tegangan DC Rata-rata Output FDA." },
    { "en": "Apa Itu 'Kapasitor Kompensasi'?", "id": "Kapasitor Internal Untuk Menjamin Stabilitas." },
    { "en": "Apa Itu 'Distorsi Amplitudo'?", "id": "Perubahan Bentuk Gelombang Karena Penguatan Non-linear." },
    { "en": "Apa Itu 'Distorsi Fasa'?", "id": "Pergeseran Fasa Yang Berbeda Tiap Frekuensi." },
    { "en": "Apa Itu 'Clipping'?", "id": "Pemotongan Puncak Sinyal Akibat Saturasi." },
    { "en": "Apa Itu 'Slew-Induced Distortion' (SID)?", "id": "Distorsi Akibat Slew Rate Terbatas." },
    { "en": "Apa Itu 'Tegangan Referensi'?", "id": "Tegangan Stabil Untuk Tujuan Perbandingan." },
    { "en": "Bagaimana Op-Amp Membuat Tegangan Referensi?", "id": "Menggunakan Dioda Zener Atau Bandgap." },
    { "en": "Apa Itu 'Penguat Jembatan Wheatstone'?", "id": "Menguatkan Output Dari Sensor Jembatan." },
    { "en": "Apa Itu 'Konverter Arus AC ke DC'?", "id": "Penyearah Presisi Untuk Mengukur AC." },
    { "en": "Apa Itu 'Pelacakan Umpan Balik Aktif'?", "id": "Teknik Untuk Meningkatkan Akurasi." },
    { "en": "Apa Itu 'Op-Amp Dengan Shutdown'?", "id": "Op-Amp Yang Dapat Dimatikan." },
    { "en": "Apa Tujuan Fitur Shutdown?", "id": "Menghemat Daya Saat Tidak Digunakan." },
    { "en": "Apa Itu 'Skala Logaritmik'?", "id": "Skala Berdasarkan Logaritma (Contohnya dB)." },
    { "en": "Apa Itu 'Arus Diam' (Quiescent Current)?", "id": "Arus Yang Dikonsumsi Tanpa Sinyal." },
    { "en": "Apa Itu 'Efisiensi Op-Amp'?", "id": "Rasio Daya Output Terhadap Daya Input." },
    { "en": "Apa Itu 'Linearitas Penguatan'?", "id": "Seberapa Konstan Penguatan Tetap." },
    { "en": "Apa Itu 'Penguat Isolasi'?", "id": "Memberikan Isolasi Listrik Antar Sirkuit." },
    { "en": "Apa Itu 'Topologi Inverting'?", "id": "Input Diberikan Ke Terminal Inverting." },
    { "en": "Apa Itu 'Topologi Non-Inverting'?", "id": "Input Diberikan Ke Terminal Non-Inverting." },
    { "en": "Apa Itu 'T-network' Dalam Feedback?", "id": "Jaringan Feedback Untuk Penguatan Tinggi." },
    { "en": "Apa Keuntungan T-network?", "id": "Gain Tinggi Tanpa Resistor Besar." },
    { "en": "Apa Itu 'Analisis AC'?", "id": "Menganalisis Perilaku Rangkaian Sinyal AC." },
    { "en": "Apa Itu 'Analisis DC'?", "id": "Menganalisis Perilaku Rangkaian Sinyal DC." },
    { "en": "Apa Itu 'Titik Operasi' (Q-point)?", "id": "Titik Bias DC Sebuah Penguat." },
    { "en": "Apa Itu 'Model Sinyal Kecil'?", "id": "Model Linear Untuk Analisis AC." },
    { "en": "Apa Itu 'Impedansi Loop Tertutup'?", "id": "Impedansi Input/Output Dengan Feedback." },
    { "en": "Apa Impedansi Input Penguat Inverting?", "id": "Sama Dengan Resistor Input (Rin)." },
    { "en": "Apa Impedansi Input Penguat Non-Inverting?", "id": "Sangat Tinggi (Mendekati Ideal)." },
    { "en": "Apa Impedansi Output Penguat Feedback?", "id": "Sangat Rendah." },
    { "en": "Apa Itu 'Rangkaian Penjepit' (Clamping Circuit)?", "id": "Membatasi Amplitudo Sinyal." },
    { "en": "Apa Itu 'Voltage Limiter'?", "id": "Nama Lain Untuk Rangkaian Penjepit." },
    { "en": "Apa Itu 'Precision Clipper'?", "id": "Rangkaian Pemotong Sinyal Akurasi Tinggi." },
    { "en": "Apa Itu 'Dead-Zone Circuit'?", "id": "Output Nol Untuk Input Rentang Tertentu." },
    { "en": "Apa Itu 'Generator Gelombang Segitiga'?", "id": "Menghasilkan Sinyal Output Bentuk Segitiga." },
    { "en": "Bagaimana Membuat Gelombang Segitiga?", "id": "Mengintegrasikan Sinyal Gelombang Persegi." },
    { "en": "Apa Itu 'Generator Gelombang Sinus'?", "id": "Menghasilkan Sinyal Output Sinusoidal." },
    { "en": "Bagaimana Membuat Gelombang Sinus?", "id": "Menggunakan Osilator Seperti Jembatan Wien." },
    { "en": "Apa Itu 'Generator Fungsi'?", "id": "Menghasilkan Berbagai Bentuk Gelombang." },
    { "en": "Apa Itu 'Sumber Arus Terkendali Tegangan'?", "id": "Arus Output Proporsional Tegangan Input." },
    { "en": "Apa Itu 'Bootstrap Current Source'?", "id": "Sumber Arus Dengan Kepatuhan Tinggi." },
    { "en": "Apa Itu 'Penguat JFET'?", "id": "Menggunakan JFET Sebagai Komponen Penguat." },
    { "en": "Apa Itu 'Penguat MOSFET'?", "id": "Menggunakan MOSFET Sebagai Komponen Penguat." },
    { "en": "Apa Itu 'Efek Miller'?", "id": "Meningkatkan Kapasitansi Input Karena Gain." },
    { "en": "Bagaimana Efek Miller Mempengaruhi Bandwidth?", "id": "Dapat Menurunkan Bandwidth Penguat." },
    { "en": "Apa Itu 'Kaskade' (Cascading) Amplifier?", "id": "Menghubungkan Beberapa Tahap Penguat Seri." },
    { "en": "Bagaimana Menghitung Gain Total Kaskade?", "id": "Mengalikan Gain Masing-masing Tahap." },
    { "en": "Apa Itu 'Konfigurasi Darlington'?", "id": "Sepasang Transistor Untuk Penguatan Tinggi." },
    { "en": "Apa Itu 'Slew Rate Limiting'?", "id": "Keterbatasan Laju Perubahan Output." },
    { "en": "Apa Itu 'Full-Power Bandwidth'?", "id": "Bandwidth Maksimum Tanpa Distorsi Slew." },
    { "en": "Apa Itu 'Drift'?", "id": "Perubahan Lambat Karakteristik Karena Suhu." },
    { "en": "Apa Itu 'Koefisien Suhu'?", "id": "Ukuran Perubahan Parameter Terhadap Suhu." },
    { "en": "Apa Itu 'Matching' Komponen?", "id": "Memilih Komponen Dengan Karakteristik Serupa." },
    { "en": "Mengapa Matching Penting?", "id": "Penting Untuk Akurasi Penguat Diferensial." },
    { "en": "Apa Itu 'Layout Simetris'?", "id": "Tata Letak Komponen Simetris." },
    { "en": "Mengapa Layout Simetris Penting?", "id": "Mengurangi Efek Termal Dan Parasitik." },
    { "en": "Apa Itu 'Star Ground'?", "id": "Teknik Menghubungkan Semua Ground Satu Titik." },
    { "en": "Apa Tujuan Star Ground?", "id": "Mencegah Terjadinya Ground Loop." },
    { "en": "Apa Itu 'Kapasitor Decoupling'?", "id": "Kapasitor Untuk Menstabilkan Catu Daya." },
    { "en": "Dimana Kapasitor Decoupling Ditempatkan?", "id": "Sedekat Mungkin Dengan Pin Op-Amp." },
    { "en": "Apa Itu 'Ferrite Bead'?", "id": "Komponen Pasif Penekan Derau Frekuensi Tinggi." },
    { "en": "Apa Itu 'Phantom Zero'?", "id": "Efek Kapasitor Input Pada Respons." },
    { "en": "Apa Itu 'Resistor Noise'?", "id": "Derau Termal Yang Dihasilkan Resistor." },
    { "en": "Bagaimana Mengurangi Derau Resistor?", "id": "Menggunakan Nilai Resistor Lebih Rendah." },
    { "en": "Apa Itu 'Shot Noise'?", "id": "Derau Karena Sifat Diskret Elektron." },
    { "en": "Apa Itu 'Flicker Noise' (1/f)?", "id": "Derau Dominan Pada Frekuensi Rendah." },
    { "en": "Apa Itu 'Noise Figure' (NF)?", "id": "Ukuran Degradasi SNR Oleh Op-Amp." },
    { "en": "Nilai Noise Figure Yang Baik?", "id": "Serendah Mungkin (Mendekati 0 dB)." },
    { "en": "Apa Itu 'SNR' (Signal-to-Noise Ratio)?", "id": "Rasio Daya Sinyal Terhadap Derau." },
    { "en": "Apa Itu 'Averaging' Untuk Mengurangi Derau?", "id": "Mengambil Rata-rata Beberapa Pembacaan." },
    { "en": "Apa Itu 'Band-Limiting' Untuk Mengurangi Derau?", "id": "Membatasi Bandwidth Sesuai Sinyal." },
    { "en": "Apa Itu 'Preamplifier'?", "id": "Penguat Awal Untuk Sinyal Lemah." },
    { "en": "Apa Karakteristik Preamplifier Yang Baik?", "id": "Penguatan Tinggi Dan Derau Rendah." },
    { "en": "Apa Itu 'Error Amplifier'?", "id": "Op-Amp Dalam Loop Umpan Balik Kontrol." },
    { "en": "Apa Itu 'Servo Loop'?", "id": "Sistem Kontrol Umpan Balik." },
    { "en": "Apa Itu 'Phase-Locked Loop' (PLL)?", "id": "Sistem Umpan Balik Sinkronisasi Fasa." },
    { "en": "Apa Itu 'Voltage-Controlled Oscillator' (VCO)?", "id": "Osilator Dengan Frekuensi Terkendali Tegangan." },
    { "en": "Apa Itu 'Charge Pump'?", "id": "Sirkuit Penaik/Penurun Tegangan DC." },
    { "en": "Apa Itu 'Akurasi'?", "id": "Kedekatan Nilai Ukur Ke Nilai Sebenarnya." },
    { "en": "Apa Itu 'Presisi'?", "id": "Kemampuan Memberi Hasil Yang Sama." },
    { "en": "Apa Itu 'Resolusi'?", "id": "Perubahan Terkecil Yang Dapat Dideteksi." },
    { "en": "Apa Itu 'Linearitas'?", "id": "Seberapa Lurus Respon Input-Output." },
    { "en": "Apa Itu 'Kesalahan Non-Linearitas'?", "id": "Penyimpangan Dari Garis Lurus Ideal." },
    { "en": "Apa Itu 'Kesalahan Penguatan' (Gain Error)?", "id": "Penyimpangan Slope Dari Nilai Ideal." },
    { "en": "Apa Itu 'Kesalahan Offset'?", "id": "Output Saat Input Seharusnya Nol." },
    { "en": "Apa Itu 'Kalibrasi'?", "id": "Menyesuaikan Output Agar Sesuai Standar." },
    { "en": "Apa Itu 'Trim'?", "id": "Menyesuaikan Parameter Sirkuit Secara Halus." },
    { "en": "Bagaimana Laser Trimming Dilakukan?", "id": "Memotong Resistor Film Tipis." },
    { "en": "Apa Itu 'Zener Zap'?", "id": "Teknik Pemrograman Elemen Sirkuit." },
    { "en": "Apa Itu 'Virtual Reference'?", "id": "Titik Referensi Dibuat Pembagi Tegangan." },
    { "en": "Apa Itu 'Common-Mode Voltage'?", "id": "Komponen Tegangan Yang Sama Di Kedua Input." },
    { "en": "Apa Efek CMRR Terbatas?", "id": "Kesalahan Muncul Di Output." },
    { "en": "Apa Itu 'Common-Mode Input Range'?", "id": "Rentang Tegangan Input Yang Diizinkan." },
    { "en": "Apa Terjadi Jika Input Melebihi Rentang?", "id": "Op-Amp Tidak Berfungsi Dengan Benar." },
    { "en": "Apa Itu 'Phase Reversal'?", "id": "Fasa Output Tiba-tiba Terbalik." },
    { "en": "Kapan Phase Reversal Bisa Terjadi?", "id": "Saat Input Melebihi Batas Common-mode." },
    { "en": "Apa Itu 'Kapasitansi Beban'?", "id": "Kapasitansi Pada Terminal Output." },
    { "en": "Bagaimana Kapasitansi Beban Mempengaruhi Stabilitas?", "id": "Dapat Menyebabkan Osilasi." },
    { "en": "Bagaimana Cara Mengatasi Beban Kapasitif?", "id": "Menggunakan Resistor Isolasi Seri." },
    { "en": "Apa Itu 'Gain Peaking'?", "id": "Peningkatan Gain Tiba-tiba Dekat Bandwidth." },
    { "en": "Apa Indikasi Dari Gain Peaking?", "id": "Margin Fasa Yang Rendah." },
    { "en": "Apa Itu 'Op-Amp Arus Balik' (Current Feedback)?", "id": "Op-Amp Dengan Arsitektur Berbeda." },
    { "en": "Apa Itu 'Resistor Umpan Balik' (Feedback Resistor)?", "id": "Resistor Penghubung Output Ke Input." },
    { "en": "Apa Itu 'Resistor Input' (Input Resistor)?", "id": "Resistor Penghubung Sinyal Ke Input." },
    { "en": "Apa Peran Rasio Resistor?", "id": "Menentukan Penguatan Rangkaian (Closed-Loop)." },
    { "en": "Apa Itu 'Toleransi Resistor'?", "id": "Penyimpangan Nilai Resistor Dari Nominal." },
    { "en": "Bagaimana Toleransi Mempengaruhi Gain?", "id": "Menyebabkan Ketidakakuratan Pada Penguatan." },
    { "en": "Apa Itu 'Derau Arus Input'?", "id": "Derau Yang Dihasilkan Arus Bias." },
    { "en": "Apa Itu 'Derau Tegangan Input'?", "id": "Derau Tegangan Internal Op-Amp." },
    { "en": "Apa Itu 'Pojok Derau' (Noise Corner)?", "id": "Frekuensi Peralihan Derau Flicker-White." },
    { "en": "Apa Itu 'Derau Putih' (White Noise)?", "id": "Derau Dengan Kerapatan Spektral Seragam." },
    { "en": "Apa Itu 'Derau Merah Jambu' (Pink Noise)?", "id": "Derau Dengan Energi Sama Per Oktaf." },
    { "en": "Apa Itu 'Kopling Elektromagnetik'?", "id": "Gangguan Derau Lewat Medan Elektromagnetik." },
    { "en": "Bagaimana Cara Mengurangi Kopling Elektromagnetik?", "id": "Menggunakan Pelindung (Shielding)." },
    { "en": "Apa Itu 'Kopling Kapasitif'?", "id": "Gangguan Derau Lewat Medan Listrik." },
    { "en": "Apa Itu 'Penjaga' (Guard Ring)?", "id": "Jalur Konduktif Untuk Mencegat Arus Bocor." },
    { "en": "Apa Itu 'Penguat Audio'?", "id": "Op-Amp Yang Dioptimalkan Untuk Sinyal Audio." },
    { "en": "Apa Itu 'Penguat Video'?", "id": "Op-Amp Dengan Bandwidth Sangat Lebar." },
    { "en": "Apa Itu 'Multiplexer Analog'?", "id": "Saklar Elektronik Memilih Satu Sinyal." },
    { "en": "Apa Itu 'Demultiplexer Analog'?", "id": "Menyalurkan Satu Sinyal Ke Banyak Output." },
    { "en": "Apa Itu 'Penguat Terisolasi'?", "id": "Penguat Dengan Isolasi Listrik." },
    { "en": "Apa Itu 'Efek Termoelektrik'?", "id": "Tegangan Timbul Akibat Suhu Berbeda." },
    { "en": "Bagaimana Efek Termoelektrik Menyebabkan Offset?", "id": "Gradien Suhu Pada Kaki Komponen." },
    { "en": "Apa Itu 'Papan Prototipe' (Breadboard)?", "id": "Papan Untuk Membuat Prototipe Rangkaian." },
    { "en": "Apa Kelemahan Breadboard Untuk Op-Amp?", "id": "Kapasitansi Parasitik Tinggi." },
    { "en": "Apa Itu 'SPICE' (Simulation Program with Integrated Circuit Emphasis)?", "id": "Program Simulasi Rangkaian Elektronik." },
    { "en": "Apa Itu 'Model SPICE' Op-Amp?", "id": "Representasi Perilaku Op-Amp Untuk Simulasi." },
    { "en": "Apa Itu 'Analisis Transien'?", "id": "Simulasi Perilaku Rangkaian Terhadap Waktu." },
    { "en": "Apa Itu 'Analisis Monte Carlo'?", "id": "Simulasi Statistik Dengan Variasi Komponen." },
    { "en": "Apa Itu 'Analisis Kasus Terburuk'?", "id": "Analisis Pada Kondisi Komponen Ekstrem." },
    { "en": "Apa Itu 'Op-Amp Otomotif'?", "id": "Op-Amp Yang Memenuhi Standar Otomotif." },
    { "en": "Apa Itu 'AEC-Q100'?", "id": "Standar Kualifikasi Untuk IC Otomotif." },
    { "en": "Apa Itu 'ESD' (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Yang Merusak." },
    { "en": "Bagaimana Melindungi Op-Amp Dari ESD?", "id": "Penanganan Yang Tepat Dan Dioda Proteksi." },
    { "en": "Apa Itu 'Latch-up'?", "id": "Kondisi Hubung Singkat Internal IC CMOS." },
    { "en": "Apa Pemicu Latch-up?", "id": "Tegangan Berlebih Pada Input Atau Output." },
    { "en": "Apa Itu 'Topologi Miller'?", "id": "Teknik Kompensasi Frekuensi Umum." },
    { "en": "Apa Itu 'Tegangan Saturasi Dropout'?", "id": "Selisih Antara V Catu Daya-V Output Maks." },
    { "en": "Apa Itu 'Penguat Dua Tahap'?", "id": "Arsitektur Op-Amp Paling Umum." },
    { "en": "Apa Tahap Pertama Op-Amp Umum?", "id": "Penguat Diferensial." },
    { "en": "Apa Tahap Kedua Op-Amp Umum?", "id": "Tahap Penguatan Tegangan." },
    { "en": "Apa Tahap Ketiga Op-Amp Umum?", "id": "Tahap Buffer Output." },
    { "en": "Apa Itu 'Gain Margin'?", "id": "Ukuran Lain Dari Stabilitas Rangkaian." },
    { "en": "Apa Itu 'Root Locus'?", "id": "Metode Analisis Stabilitas." },
    { "en": "Apa Itu 'Konverter V-ke-F' (Voltage-to-Frequency)?", "id": "Frekuensi Output Proporsional Tegangan Input." },
    { "en": "Apa Itu 'Konverter F-ke-V' (Frequency-to-Voltage)?", "id": "Tegangan Output Proporsional Frekuensi Input." },
    { "en": "Apa Itu 'Penguat Jembatan Terikat-DC'?", "id": "Konfigurasi Penguat Daya Audio." },
    { "en": "Apa Itu 'Power Op-Amp'?", "id": "Op-Amp Yang Dapat Menyalurkan Arus Besar." },
    { "en": "Apa Itu 'Composite Amplifier'?", "id": "Menggabungkan Dua Op-Amp Untuk Kinerja Terbaik." },
    { "en": "Contoh Composite Amplifier?", "id": "Op-Amp Presisi Diikuti Op-Amp Cepat." },
    { "en": "Apa Itu 'Op-Amp Nol-Drift'?", "id": "Op-Amp Dengan Offset Sangat Rendah." },
    { "en": "Apa Itu 'Impedansi Transimpedansi'?", "id": "Impedansi Dari Penguat Transimpedansi." },
    { "en": "Apa Itu 'Impedansi Transkonduktansi'?", "id": "Impedansi Dari Penguat Transkonduktansi." },
    { "en": "Apa Itu 'Ideal Current Source'?", "id": "Sumber Arus Dengan Impedansi Tak Terhingga." },
    { "en": "Apa Itu 'Ideal Voltage Source'?", "id": "Sumber Tegangan Dengan Impedansi Nol." },
    { "en": "Apa Itu 'Teorema Superposisi'?", "id": "Menganalisis Efek Sumber Secara Terpisah." },
    { "en": "Apa Itu 'Teorema Thevenin'?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan." },
    { "en": "Apa Itu 'Teorema Norton'?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus." },
    { "en": "Bagaimana Op-Amp Digunakan di Regulator Tegangan?", "id": "Sebagai Penguat Kesalahan (Error Amplifier)." },
    { "en": "Apa Itu 'Regulator Linear'?", "id": "Regulator Yang Bekerja Di Daerah Linear." },
    { "en": "Apa Itu 'LDO' (Low-Dropout Regulator)?", "id": "Regulator Linear Dengan Dropout Rendah." },
    { "en": "Apa Itu 'SMPS' (Switch-Mode Power Supply)?", "id": "Catu Daya Berbasis Pensaklaran." },
    { "en": "Apa Peran Op-Amp Dalam SMPS?", "id": "Mengontrol Siklus Kerja (Duty Cycle)." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
    { "en": "Apa Itu 'Error Budget Analysis'?", "id": "Menganalisis Total Kesalahan Dalam Sistem." },
    { "en": "Sumber Kesalahan Apa Saja Di Rangkaian Op-Amp?", "id": "Offset, Bias, Derau, Toleransi." },
    { "en": "Apa Itu 'Root Sum Square' (RSS)?", "id": "Metode Menjumlahkan Kesalahan Acak." },
    { "en": "Apa Itu 'Penguat Terdistribusi'?", "id": "Teknik Untuk Mencapai Bandwidth Ultra-Lebar." },
    { "en": "Apa Itu 'Tegangan Mode Diferensial'?", "id": "Selisih Tegangan Antara Dua Input." },
    { "en": "Apa Itu 'Keluaran Rail-to-Rail'?", "id": "Output Dapat Mencapai Catu Daya." },
    { "en": "Apa Itu 'Masukan Rail-to-Rail'?", "id": "Input Dapat Menerima Tegangan Catu Daya." },
    { "en": "Apa Itu 'Class A' Amplifier?", "id": "Penguat Dengan Transistor Selalu Aktif." },
    { "en": "Apa Itu 'Class B' Amplifier?", "id": "Penguat Dengan Dua Transistor Bergantian." },
    { "en": "Apa Itu 'Class AB' Amplifier?", "id": "Kompromi Antara Kelas A Dan B." },
    { "en": "Apa Itu 'Class D' Amplifier?", "id": "Penguat Berbasis Pensaklaran (PWM)." },
    { "en": "Apa Kelas Tahap Output Op-Amp Umum?", "id": "Kelas AB." },
    { "en": "Apa Itu 'Efek Early'?", "id": "Modulasi Lebar Basis Pada Transistor BJT." },
    { "en": "Apa Pengaruh Efek Early?", "id": "Membatasi Penguatan Tegangan Intrinsik." },
    { "en": "Apa Itu 'Giga-ohm Seal'?", "id": "Koneksi Resistansi Sangat Tinggi." },
    { "en": "Apa Itu 'Patch-Clamp Amplifier'?", "id": "Mengukur Arus Lewat Kanal Ion Tunggal." },
    { "en": "Apa Itu 'Respon Impuls'?", "id": "Keluaran Sistem Diberi Input Impuls." },
    { "en": "Apa Itu 'Konvolusi'?", "id": "Operasi Matematis Untuk Mencari Output." },
    { "en": "Apa Itu 'Domain-s'?", "id": "Representasi Rangkaian Menggunakan Transformasi Laplace." },
    { "en": "Apa Itu 'Fungsi Transfer'?", "id": "Rasio Output Terhadap Input Domain-s." },
    { "en": "Apa Itu 'Pole Dominan'?", "id": "Pole Dengan Frekuensi Terendah." },
    { "en": "Apa Fungsi Pole Dominan?", "id": "Mendominasi Respon Frekuensi Op-Amp." },
    { "en": "Apa Itu 'Kapasitor Parasitik'?", "id": "Kapasitansi Tak Diinginkan Dalam Rangkaian." },
    { "en": "Apa Itu 'Induktansi Parasitik'?", "id": "Induktansi Tak Diinginkan Dalam Rangkaian." },
    { "en": "Apa Itu 'Settling Error'?", "id": "Perbedaan Dari Nilai Akhir." },
    { "en": "Apa Itu 'Feed-forward Compensation'?", "id": "Teknik Kompensasi Untuk Meningkatkan Bandwidth." },
    { "en": "Apa Itu 'Input-Referred Noise'?", "id": "Derau Seolah-olah Muncul Di Input." },
    { "en": "Apa Itu 'Output-Referred Noise'?", "id": "Derau Yang Diukur Langsung Di Output." },
    { "en": "Bagaimana Menghitung Output-Referred Noise?", "id": "Input-Referred Noise Dikali Penguatan." },
    { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Sinyal Antar Kanal Berdekatan." },
    { "en": "Kapan Crosstalk Menjadi Masalah?", "id": "Pada Op-Amp Ganda Atau Kuad." },
    { "en": "Apa Itu 'Drift Suhu'?", "id": "Perubahan Offset Akibat Perubahan Suhu." },
    { "en": "Apa Satuan Drift Suhu Offset?", "id": "Mikrovolt Per Derajat Celcius (µV/°C)." },
    { "en": "Apa Itu 'Bootstrap Buffer'?", "id": "Buffer Dengan Impedansi Input Sangat Tinggi." },
    { "en": "Apa Itu 'Rectifier Jembatan Presisi'?", "id": "Menyearahkan Sinyal AC Dengan Akurat." },
    { "en": "Apa Itu 'Phase Detector'?", "id": "Rangkaian Yang Membandingkan Fasa Dua Sinyal." },
    { "en": "Apa Itu 'Lock-In Amplifier'?", "id": "Mengukur Sinyal AC Sangat Kecil." },
    { "en": "Apa Prinsip Lock-In Amplifier?", "id": "Menggunakan Deteksi Sinkron (Homodyne)." },
    { "en": "Apa Itu 'Analisis Fourier'?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensi." },
    { "en": "Apa Itu 'Koefisien Fourier'?", "id": "Amplitudo Dari Setiap Komponen Frekuensi." },
    { "en": "Apa Itu 'Respon Step'?", "id": "Keluaran Saat Diberi Input Step." },
    { "en": "Apa Hubungan Respon Step Dan Slew Rate?", "id": "Kemiringan Awal Dibatasi Slew Rate." },
    { "en": "Apa Itu 'Dielectric Absorption'?", "id": "Efek Memori Pada Kapasitor." },
    { "en": "Bagaimana Dielectric Absorption Mempengaruhi Sirkuit?", "id": "Menyebabkan Kesalahan Pada Integrator." },
    { "en": "Apa Itu 'Guard Trace'?", "id": "Jalur PCB Untuk Melindungi Sinyal Sensitif." },
    { "en": "Apa Itu 'Kelvin Connection'?", "id": "Teknik Pengukuran Empat Kawat." },
    { "en": "Apa Tujuan Kelvin Connection?", "id": "Menghilangkan Efek Resistansi Kabel." },
    { "en": "Apa Itu 'Op-Amp Arus Nol' (Zero-Current Op-Amp)?", "id": "Op-Amp Dengan Arus Input Hampir Nol." },
    { "en": "Apa Itu 'Transkonduktansi' (gm)?", "id": "Ukuran Penguatan Arus-Tegangan." },
    { "en": "Apa Itu 'Tegangan Early'?", "id": "Ukuran Resistansi Output Transistor." },
    { "en": "Apa Itu 'Sirkuit Cascode'?", "id": "Konfigurasi Penguat Untuk Gain Tinggi." },
    { "en": "Apa Itu 'Sirkuit Wilson Current Mirror'?", "id": "Cermin Arus Dengan Kinerja Tinggi." },
    { "en": "Apa Itu 'Bandgap Reference'?", "id": "Sumber Referensi Tegangan Sangat Stabil." },
    { "en": "Apa Itu 'Rasio Aspek' (W/L) Transistor?", "id": "Rasio Lebar Terhadap Panjang Gerbang." },
    { "en": "Bagaimana Rasio Aspek Mempengaruhi Kinerja?", "id": "Mempengaruhi Arus Dan Transkonduktansi." },
    { "en": "Apa Itu 'Efek Tubuh' (Body Effect)?", "id": "Perubahan Tegangan Ambang Transistor." },
    { "en": "Apa Itu 'Tegangan Ambang' (Threshold Voltage)?", "id": "Tegangan Untuk Menghidupkan Transistor." },
    { "en": "Apa Itu 'Sub-Threshold Conduction'?", "id": "Arus Kecil Saat Transistor Mati." },
    { "en": "Apa Itu 'Model Ebers-Moll'?", "id": "Model Matematika Untuk Transistor BJT." },
    { "en": "Apa Itu 'Parameter-h'?", "id": "Model Sinyal Kecil Untuk Transistor." },
    { "en": "Apa Itu 'Hybrid-Pi Model'?", "id": "Model Sinyal Kecil Lainnya." },
    { "en": "Apa Itu 'Desensitisasi'?", "id": "Penurunan Penguatan Karena Umpan Balik." },
    { "en": "Apa Itu 'Faktor Umpan Balik' (Beta)?", "id": "Fraksi Sinyal Output Yang Dikembalikan." },
    { "en": "Apa Itu 'Stabilitas Mutlak'?", "id": "Stabil Untuk Semua Kondisi Pasif." },
    { "en": "Apa Itu 'Stabilitas Kondisional'?", "id": "Stabil Hanya Untuk Kondisi Tertentu." },
    { "en": "Apa Itu 'Nyquist Stability Criterion'?", "id": "Metode Grafis Untuk Menganalisis Stabilitas." },
    { "en": "Apa Itu 'Contour Plot'?", "id": "Representasi Grafis Tiga Dimensi." },
    { "en": "Apa Itu 'Analisis Toleransi'?", "id": "Mempelajari Efek Variasi Nilai Komponen." },
    { "en": "Apa Itu 'Sensitivitas'?", "id": "Ukuran Perubahan Output Akibat Komponen." },
    { "en": "Apa Itu 'Peringkat Daya' (Power Rating)?", "id": "Daya Maksimum Yang Dapat Ditangani." },
    { "en": "Apa Itu 'Safe Operating Area' (SOA)?", "id": "Batas Operasi Aman Untuk Transistor." },
    { "en": "Apa Itu 'Second Breakdown'?", "id": "Kegagalan Transistor Akibat Efek Termal." },
    { "en": "Apa Itu 'Efek Zener'?", "id": "Mekanisme Tembus Dioda Bias Mundur." },
    { "en": "Apa Itu 'Efek Avalanche'?", "id": "Mekanisme Tembus Lainnya Pada Dioda." },
    { "en": "Apa Itu 'Kapasitansi Sambungan'?", "id": "Kapasitansi Internal Dioda Atau Transistor." },
    { "en": "Apa Itu 'Waktu Pemulihan Mundur'?", "id": "Waktu Dioda Berhenti Menghantar." },
    { "en": "Apa Itu 'Arus Balik' (Reverse Current)?", "id": "Arus Bocor Dioda Bias Mundur." },
    { "en": "Apa Itu 'Pengganda Tegangan'?", "id": "Rangkaian Untuk Melipatgandakan Tegangan DC." },
    { "en": "Apa Itu 'Voltage Doubler'?", "id": "Menggandakan Tegangan Input." },
    { "en": "Apa Itu 'Voltage Tripler'?", "id": "Melipatgandakan Tegangan Tiga Kali." },
    { "en": "Apa Itu 'Sirkuit Cockcroft-Walton'?", "id": "Jenis Pengganda Tegangan." },
    { "en": "Apa Itu 'Ripple Factor'?", "id": "Ukuran Sisa Variasi AC." },
    { "en": "Apa Itu 'Regulasi Beban'?", "id": "Perubahan Tegangan Output Akibat Beban." },
    { "en": "Apa Itu 'Regulasi Jalur'?", "id": "Perubahan Output Akibat Input." },
    { "en": "Apa Itu 'Transient Response'?", "id": "Respon Sistem Terhadap Perubahan Input." },
    { "en": "Apa Itu 'Op-Amp Terkopling-AC'?", "id": "Hanya Menguatkan Komponen AC." },
    { "en": "Apa Itu 'Op-Amp Terkopling-DC'?", "id": "Menguatkan Komponen AC Dan DC." },
    { "en": "Apa Itu 'DC Servo'?", "id": "Umpan Balik Untuk Menghilangkan Offset DC." },
    { "en": "Apa Itu 'Skema Warna Resistor'?", "id": "Kode Warna Untuk Menandai Nilai Resistor." },
    { "en": "Apa Itu 'Kode SMD' (Surface-Mount Device)?", "id": "Kode Angka Untuk Menandai Komponen SMD." },
    { "en": "Apa Itu 'Footprint' Komponen?", "id": "Pola Tembaga Untuk Pemasangan Komponen." },
    { "en": "Apa Itu 'Library' Dalam Desain PCB?", "id": "Kumpulan Skematik Dan Footprint." },
    { "en": "Apa Itu 'Netlist'?", "id": "Daftar Koneksi Dalam Rangkaian Skematik." },
    { "en": "Apa Itu 'Autorouting'?", "id": "Proses Otomatis Membuat Jalur PCB." },
    { "en": "Apa Itu 'Thermal Relief Pad'?", "id": "Menghubungkan Pin Ke Ground Plane." },
    { "en": "Apa Itu 'Via Stitching'?", "id": "Menambahkan Banyak Via Untuk Koneksi." },
    { "en": "Apa Itu 'Teardrop' Dalam Desain PCB?", "id": "Bentuk Tetes Air Pada Sambungan." },
    { "en": "Apa Itu 'Impedansi Terkontrol'?", "id": "Jalur PCB Dengan Impedansi Spesifik." },
    { "en": "Kapan Impedansi Terkontrol Diperlukan?", "id": "Untuk Sinyal Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu 'Sinyal Diferensial'?", "id": "Informasi Dikirim Sebagai Selisih." },
    { "en": "Apa Keuntungan Sinyal Diferensial?", "id": "Sangat Tahan Terhadap Derau." },
    { "en": "Apa Itu 'Twisted Pair'?", "id": "Dua Kabel Dipilin Untuk Sinyal Diferensial." },
    { "en": "Apa Itu 'Integritas Sinyal' (Signal Integrity)?", "id": "Kualitas Sinyal Listrik." },
    { "en": "Apa Itu 'Refleksi Sinyal'?", "id": "Sinyal Memantul Akibat Mismatch Impedansi." },
    { "en": "Apa Itu 'Ringing' Akibat Refleksi?", "id": "Osilasi Pada Tepi Sinyal Digital." },
    { "en": "Apa Itu 'Terminasi'?", "id": "Resistor Untuk Mencegah Refleksi." },
    { "en": "Apa Itu 'Terminasi Seri'?", "id": "Resistor Dekat Sumber Sinyal." },
    { "en": "Apa Itu 'Terminasi Paralel'?", "id": "Resistor Dekat Ujung Saluran." },
    { "en": "Apa Itu 'Diagram Mata' (Eye Diagram)?", "id": "Metode Visualisasi Kualitas Sinyal Digital." },
    { "en": "Apa Itu 'Jitter'?", "id": "Variasi Waktu Pada Tepi Sinyal." },
    { "en": "Apa Itu 'Penguat Sampel-dan-Tahan'?", "id": "Sirkuit Yang Mencuplik Dan Menahan Tegangan." },
    { "en": "Apa Itu 'Droop'?", "id": "Penurunan Tegangan Tertahan Seiring Waktu." },
    { "en": "Apa Itu 'Feedthrough'?", "id": "Kebocoran Sinyal Input Ke Output." },
    { "en": "Apa Itu 'Aperture Time'?", "id": "Waktu Peralihan Dari Mode Sampel." },
    { "en": "Apa Itu 'ADC' (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
    { "en": "Apa Itu 'DAC' (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
    { "en": "Apa Peran Op-Amp Dalam ADC/DAC?", "id": "Buffer Input, Filter Anti-aliasing." },
    { "en": "Apa Itu 'Filter Anti-Aliasing'?", "id": "Filter Low-pass Sebelum Proses Sampling." },
    { "en": "Apa Itu 'Filter Rekonstruksi'?", "id": "Filter Low-pass Setelah Proses DAC." },
    { "en": "Apa Itu 'Kuantisasi'?", "id": "Pembulatan Nilai Sampel Ke Level." },
    { "en": "Apa Itu 'Error Kuantisasi'?", "id": "Selisih Antara Nilai Asli Sampel." },
    { "en": "Apa Itu 'Differential Nonlinearity' (DNL)?", "id": "Ukuran Kesalahan Antara Langkah ADC." },
    { "en": "Apa Itu 'Integral Nonlinearity' (INL)?", "id": "Ukuran Penyimpangan Total Dari Ideal." },
    { "en": "Apa Itu 'Flash ADC'?", "id": "ADC Sangat Cepat Menggunakan Komparator." },
    { "en": "Apa Itu 'SAR ADC' (Successive Approximation Register)?", "id": "ADC Dengan Keseimbangan Kecepatan-Resolusi." },
    { "en": "Apa Itu 'Delta-Sigma ADC'?", "id": "ADC Resolusi Sangat Tinggi." },
    { "en": "Apa Itu 'Oversampling'?", "id": "Sampling Pada Frekuensi Jauh Lebih Tinggi." },
    { "en": "Apa Itu 'Noise Shaping'?", "id": "Memindahkan Derau Kuantisasi Keluar Pita." },
    { "en": "Apa Itu 'R-2R Ladder' DAC?", "id": "DAC Menggunakan Jaringan Resistor R-2R." },
    { "en": "Apa Itu 'Binary-Weighted' DAC?", "id": "DAC Dengan Resistor Berbobot Biner." },
    { "en": "Apa Itu 'Monotonisitas' (Monotonicity)?", "id": "Output DAC Selalu Naik Dengan Input." },
    { "en": "Apa Itu 'Glitch' Pada DAC?", "id": "Lonjakan Sesaat Saat Transisi Kode." },
    { "en": "Apa Itu 'Deglitching Circuit'?", "id": "Rangkaian Untuk Menghilangkan Glitch DAC." },
    { "en": "Apa Itu 'Op-Amp Chopper'?", "id": "Op-Amp Dengan Offset Sangat Rendah." },
    { "en": "Bagaimana Cara Kerja Op-Amp Chopper?", "id": "Memodulasi Sinyal Untuk Hilangkan Offset." },
    { "en": "Apa Itu 'THD+N' (Total Harmonic Distortion plus Noise)?", "id": "Ukuran Distorsi Dan Derau Total." },
    { "en": "Apa Itu 'SINAD' (Signal-to-Noise and Distortion)?", "id": "Rasio Sinyal Terhadap Derau-Distorsi." },
    { "en": "Apa Itu 'ENOB' (Effective Number of Bits)?", "id": "Ukuran Kinerja Dinamis ADC." },
    { "en": "Apa Itu 'SFDR' (Spurious-Free Dynamic Range)?", "id": "Rentang Dinamis Bebas Sinyal Palsu." },
    { "en": "Apa Itu 'Intermodulation Distortion' (IMD)?", "id": "Distorsi Akibat Pencampuran Dua Sinyal." },
    { "en": "Apa Itu 'Two-Tone Test'?", "id": "Metode Pengujian Intermodulasi Distortion." },
    { "en": "Apa Itu 'Active Antenna'?", "id": "Antena Dengan Penguat Terintegrasi." },
    { "en": "Apa Peran Op-Amp di Active Antenna?", "id": "Sebagai Penguat Derau Rendah (LNA)." },
    { "en": "Apa Itu 'LNA' (Low-Noise Amplifier)?", "id": "Penguat Awal Dengan Derau Rendah." },
    { "en": "Apa Itu 'PID Controller'?", "id": "Kontroler Umpan Balik (Proportional-Integral-Derivative)." },
    { "en": "Bagaimana Op-Amp Membangun Kontroler PID?", "id": "Menggunakan Rangkaian Penjumlah, Integrator, Differentiator." },
    { "en": "Apa Itu 'Proportional Band'?", "id": "Rentang Error Untuk Output Kontroler." },
    { "en": "Apa Itu 'Integral Windup'?", "id": "Kondisi Saturasi Pada Aksi Integral." },
    { "en": "Apa Itu 'Anti-Windup Circuit'?", "id": "Rangkaian Untuk Mencegah Integral Windup." },
    { "en": "Apa Itu 'Biquad Filter'?", "id": "Topologi Filter Aktif Orde Kedua." },
    { "en": "Apa Itu 'State-Variable Filter'?", "id": "Filter Universal Dengan Output LP/HP/BP." },
    { "en": "Apa Itu 'Switched-Capacitor Filter'?", "id": "Filter Menggunakan Kapasitor Dan Saklar." },
    { "en": "Apa Keuntungan Switched-Capacitor Filter?", "id": "Mudah Diintegrasikan Dalam Chip IC." },
    { "en": "Apa Itu 'GIC' (Generalized Impedance Converter)?", "id": "Rangkaian Untuk Mensimulasikan Elemen." },
    { "en": "Apa Itu 'Simulated Inductor'?", "id": "Induktor Dibuat Dengan Op-Amp." },
    { "en": "Apa Itu 'Frequency-Dependent Negative Resistor' (FDNR)?", "id": "Elemen Sirkuit Sintetis." },
    { "en": "Apa Itu 'Model Boyle'?", "id": "Model Sederhana Untuk Op-Amp Praktis." },
    { "en": "Apa Itu 'Model Makro' (Macromodel)?", "id": "Model SPICE Yang Menyederhanakan Sirkuit." },
    { "en": "Apa Itu 'Analisis Titik Bias DC'?", "id": "Simulasi Untuk Menemukan Titik Operasi DC." },
    { "en": "Apa Itu 'AC Sweep Analysis'?", "id": "Simulasi Respon Frekuensi Rangkaian." },
    { "en": "Apa Itu 'Noise Analysis'?", "id": "Simulasi Untuk Menganalisis Kinerja Derau." },
    { "en": "Apa Itu 'Distortion Analysis'?", "id": "Simulasi Untuk Menganalisis Distorsi." },
    { "en": "Apa Itu 'Parameter Sweep'?", "id": "Simulasi Dengan Mengubah Nilai Komponen." },
    { "en": "Apa Itu 'Simulasi Termal'?", "id": "Menganalisis Efek Suhu Pada Rangkaian." },
    { "en": "Apa Itu 'ESD Human Body Model' (HBM)?", "id": "Model Standar Untuk Pengujian ESD." },
    { "en": "Apa Itu 'ESD Machine Model' (MM)?", "id": "Model ESD Lainnya Yang Lebih Ketat." },
    { "en": "Apa Itu 'Dioda Proteksi Input'?", "id": "Dioda Internal Pelindung Dari ESD." },
    { "en": "Apa Itu 'Resistor Balast'?", "id": "Resistor Untuk Membatasi Arus." },
    { "en": "Apa Itu 'Transistor Lateral PNP'?", "id": "Jenis Transistor PNP Dalam Proses CMOS." },
    { "en": "Apa Itu 'Transistor Vertikal PNP'?", "id": "Jenis Transistor PNP Kinerja Tinggi." },
    { "en": "Apa Itu 'Popcorn Noise' (Burst Noise)?", "id": "Derau Frekuensi Rendah Acak." },
    { "en": "Apa Itu 'Tegangan Keluaran Diferensial'?", "id": "Selisih Antara Vout+ dan Vout-." },
    { "en": "Apa Itu 'Penguat Terkopling Langsung'?", "id": "Penguat Tanpa Kapasitor Kopling." },
    { "en": "Apa Itu 'Penguat Tipe-Inverting'?", "id": "Nama Lain Untuk Penguat Inverting." },
    { "en": "Apa Itu 'Penguat Tipe-Noninverting'?", "id": "Nama Lain Untuk Penguat Non-inverting." },
    { "en": "Apa Itu 'Penguat Jembatan Penuh'?", "id": "Menguatkan Sinyal Dari Sensor Jembatan." },
    { "en": "Apa Itu 'Penguat Setengah Jembatan'?", "id": "Menguatkan Sinyal Dari Setengah Jembatan." },
    { "en": "Apa Itu 'Tegangan Eksitasi Jembatan'?", "id": "Tegangan Yang Diberikan Ke Jembatan Sensor." },
    { "en": "Apa Itu 'Linearisasi'?", "id": "Membuat Respon Sensor Menjadi Linear." },
    { "en": "Bagaimana Op-Amp Melakukan Linearisasi?", "id": "Menggunakan Jaringan Umpan Balik Non-linear." },
    { "en": "Apa Itu 'Kompensasi Cold-Junction'?", "id": "Koreksi Untuk Pengukuran Termokopel." },
    { "en": "Apa Itu 'Penguat Termokopel'?", "id": "Menguatkan Sinyal Tegangan Sangat Kecil." },
    { "en": "Apa Itu 'Penguat Fotodioda'?", "id": "Penguat Transimpedansi Untuk Fotodioda." },
    { "en": "Apa Itu 'Mode Fotokonduktif'?", "id": "Fotodioda Diberi Bias Mundur." },
    { "en": "Apa Itu 'Mode Fotovoltaik'?", "id": "Fotodioda Tanpa Bias Eksternal." },
    { "en": "Apa Itu 'Kapasitansi Gelap'?", "id": "Kapasitansi Fotodioda Tanpa Cahaya." },
    { "en": "Apa Itu 'Arus Gelap'?", "id": "Arus Bocor Fotodioda Tanpa Cahaya." },
    { "en": "Apa Itu 'Responsivitas'?", "id": "Rasio Arus Output Terhadap Daya Cahaya." },
    { "en": "Apa Itu 'Efek Piezoelektrik'?", "id": "Bahan Menghasilkan Tegangan Saat Ditekan." },
    { "en": "Apa Itu 'Penguat Muatan' (Charge Amplifier)?", "id": "Mengukur Muatan Listrik Dari Sensor." },
    { "en": "Sensor Apa Yang Menggunakan Penguat Muatan?", "id": "Sensor Piezoelektrik Dan Akselerometer." },
    { "en": "Apa Itu 'Kabel Triboelektrik Rendah'?", "id": "Kabel Khusus Untuk Sinyal Muatan." },
    { "en": "Apa Itu 'High-Z Probe'?", "id": "Probe Osiloskop Dengan Impedansi Tinggi." },
    { "en": "Apa Itu 'Low-Z Probe'?", "id": "Probe Osiloskop Dengan Impedansi Rendah." },
    { "en": "Apa Itu 'Ground Lead'?", "id": "Kabel Ground Pada Probe Osiloskop." },
    { "en": "Mengapa Ground Lead Harus Pendek?", "id": "Mengurangi Induktansi Dan Ringing." },
    { "en": "Apa Itu 'Compensation' Probe Osiloskop?", "id": "Menyesuaikan Probe Dengan Input Osiloskop." },
    { "en": "Apa Itu 'ADC Pipeline'?", "id": "ADC Kecepatan Tinggi Dengan Tahap Bertingkat." },
    { "en": "Apa Itu 'ADC Ramp'?", "id": "ADC Menggunakan Komparator Dan Ramp." },
    { "en": "Apa Itu 'ADC Dual-Slope'?", "id": "ADC Lambat Dengan Akurasi Tinggi." },
    { "en": "Apa Itu 'Penguat Terisolasi Optik'?", "id": "Menggunakan Optocoupler Untuk Isolasi." },
    { "en": "Apa Itu 'Penguat Terisolasi Kapasitif'?", "id": "Menggunakan Kapasitor Untuk Isolasi." },
    { "en": "Apa Itu 'Penguat Terisolasi Magnetik'?", "id": "Menggunakan Trafo Untuk Isolasi." },
    { "en": "Apa Itu 'Respon Impuls Terbatas' (FIR)?", "id": "Karakteristik Filter Digital." },
    { "en": "Apa Itu 'Respon Impuls Tak Terbatas' (IIR)?", "id": "Karakteristik Filter Digital Lainnya." },
    { "en": "Apa Itu 'Jendela' (Windowing)?", "id": "Teknik Dalam Desain Filter FIR." },
    { "en": "Apa Itu 'Efek Gibbs'?", "id": "Riak Dekat Diskontinuitas Sinyal." },
    { "en": "Apa Itu 'Zero-Order Hold'?", "id": "Metode Rekonstruksi Sinyal Dari DAC." },
    { "en": "Apa Itu 'Sinc Function'?", "id": "Respon Frekuensi Ideal Filter Low-pass." },
    { "en": "Apa Itu 'Decimation'?", "id": "Mengurangi Laju Sampel Sinyal Digital." },
    { "en": "Apa Itu 'Interpolation'?", "id": "Meningkatkan Laju Sampel Sinyal Digital." },
    { "en": "Apa Itu 'Aliasing'?", "id": "Distorsi Akibat Sampling Terlalu Lambat." },
    { "en": "Apa Itu 'Teorema Sampling Nyquist'?", "id": "Syarat Minimum Frekuensi Sampling." },
    { "en": "Apa Itu 'Frekuensi Nyquist'?", "id": "Setengah Dari Frekuensi Sampling." },
    { "en": "Apa Itu 'Roll-off' Filter?", "id": "Tingkat Ketajaman Transisi Filter." },
    { "en": "Apa Itu 'Passband'?", "id": "Rentang Frekuensi Yang Dilewatkan Filter." },
    { "en": "Apa Itu 'Stopband'?", "id": "Rentang Frekuensi Yang Ditolak Filter." },
    { "en": "Apa Itu 'Ripple'?", "id": "Variasi Gain Di Passband." },
    { "en": "Apa Itu 'Transition Band'?", "id": "Rentang Frekuensi Antara Passband-Stopband." },
    { "en": "Apa Itu 'Group Delay'?", "id": "Ukuran Penundaan Rata-rata Filter." },
    { "en": "Apa Itu 'Phase Delay'?", "id": "Ukuran Penundaan Untuk Satu Frekuensi." },
    { "en": "Apa Itu 'Linear Phase'?", "id": "Group Delay Konstan Di Semua Frekuensi." },
    { "en": "Mengapa Linear Phase Penting?", "id": "Menjaga Bentuk Gelombang Tanpa Distorsi." },
    { "en": "Apa Itu 'Equalizer Grafis'?", "id": "Filter Untuk Menyesuaikan Respon Frekuensi Audio." },
    { "en": "Apa Itu 'Equalizer Parametrik'?", "id": "Equalizer Dengan Kontrol Frekuensi, Gain, Q." },
    { "en": "Apa Itu 'Faktor-Q' (Quality Factor)?", "id": "Ukuran Selektivitas Filter Band-pass." },
    { "en": "Apa Itu 'Active Crossover'?", "id": "Membagi Sinyal Audio Sebelum Penguatan." },
    { "en": "Apa Itu 'Passive Crossover'?", "id": "Membagi Sinyal Audio Setelah Penguatan." },
    { "en": "Apa Itu 'Rangkaian Gyrator'?", "id": "Mensimulasikan Induktor Dengan Kapasitor." },
    { "en": "Apa Itu 'Voltage Controlled Current Source' (VCCS)?", "id": "Sumber Arus Yang Dikendalikan Tegangan." },
    { "en": "Apa Itu 'OTA' (Operational Transconductance Amplifier)?", "id": "Penguat Dengan Output Arus." },
    { "en": "Apa Perbedaan OTA dan Op-Amp?", "id": "OTA Adalah Penguat Transkonduktansi." },
    { "en": "Apa Itu 'Arus Bias' OTA?", "id": "Arus Untuk Mengontrol Transkonduktansi (gm)." },
    { "en": "Apa Itu 'Current Conveyor'?", "id": "Blok Bangunan Sirkuit Analog Lainnya." },
    { "en": "Apa Itu 'Logarithmic Conformance'?", "id": "Akurasi Penguat Logaritmik." },
    { "en": "Apa Itu 'Antilog Amplifier'?", "id": "Outputnya Adalah Eksponensial Dari Input." },
    { "en": "Apa Itu 'Multiplier Analog'?", "id": "Outputnya Adalah Perkalian Dua Input." },
    { "en": "Apa Itu 'Divider Analog'?", "id": "Outputnya Adalah Pembagian Dua Input." },
    { "en": "Apa Itu 'Root-Mean-Square' (RMS)?", "id": "Nilai Efektif Dari Sinyal AC." },
    { "en": "Apa Itu 'RMS-to-DC Converter'?", "id": "Menghasilkan Output DC Sesuai Nilai RMS." },
    { "en": "Apa Itu 'Faktor Crest'?", "id": "Rasio Nilai Puncak Terhadap RMS." },
    { "en": "Apa Itu 'Active Guard Drive'?", "id": "Mengurangi Kapasitansi Parasitik Kabel." },
    { "en": "Apa Itu 'Driven Shield'?", "id": "Nama Lain Untuk Active Guard Drive." },
    { "en": "Apa Itu 'Die' (Dalam IC)?", "id": "Potongan Silikon Berisi Sirkuit." },
    { "en": "Apa Itu 'Substrat' IC?", "id": "Material Dasar Wafer Semikonduktor." },
    { "en": "Apa Itu 'Proses Bipolar'?", "id": "Teknologi Fabrikasi IC Berbasis BJT." },
    { "en": "Apa Itu 'Proses CMOS'?", "id": "Teknologi Fabrikasi IC Berbasis MOSFET." },
    { "en": "Apa Itu 'Proses BiCMOS'?", "id": "Menggabungkan Teknologi Bipolar Dan CMOS." },
    { "en": "Apa Keuntungan BiCMOS?", "id": "Kombinasi Kecepatan Dan Kepadatan." },
    { "en": "Apa Itu 'Layout' IC?", "id": "Desain Geometris Sirkuit Pada Chip." },
    { "en": "Apa Itu 'Parasitic Extraction'?", "id": "Menghitung Elemen Parasitik Dari Layout." },
    { "en": "Apa Itu 'Antenna Effect'?", "id": "Kerusakan Gerbang Selama Proses Fabrikasi." },
    { "en": "Apa Itu 'Electromigration'?", "id": "Pergerakan Atom Logam Karena Arus." },
    { "en": "Apa Akibat Electromigration?", "id": "Dapat Menyebabkan Kegagalan Sirkuit." },
    { "en": "Apa Itu 'Wire Bonding'?", "id": "Menyambung Die Ke Kaki Kemasan." },
    { "en": "Apa Itu 'Lead Frame'?", "id": "Kerangka Logam Untuk Kemasan IC." },
    { "en": "Apa Itu 'Die Attach'?", "id": "Proses Melekatkan Die Ke Lead Frame." },
    { "en": "Apa Itu 'Molding'?", "id": "Proses Enkapsulasi Chip Dengan Plastik." },
    { "en": "Apa Itu 'Trimming' Laser?", "id": "Menyesuaikan Nilai Resistor Dengan Laser." },
    { "en": "Apa Itu 'Yield' Fabrikasi?", "id": "Persentase Chip Yang Berfungsi." },
    { "en": "Apa Itu 'Wafer Sort'?", "id": "Pengujian Chip Saat Masih Di Wafer." },
    { "en": "Apa Itu 'Final Test'?", "id": "Pengujian Chip Setelah Dikemas." },
    { "en": "Apa Itu 'Burn-in'?", "id": "Pengujian Di Suhu Tinggi." },
    { "en": "Apa Tujuan Burn-in?", "id": "Menyingkirkan Kegagalan Awal." },
    { "en": "Apa Itu 'MTTF' (Mean Time To Failure)?", "id": "Waktu Rata-rata Hingga Terjadi Kegagalan." },
    { "en": "Apa Itu 'Kurva Bak Mandi' (Bathtub Curve)?", "id": "Grafik Tingkat Kegagalan Terhadap Waktu." },
    { "en": "Apa Itu 'Failure Analysis'?", "id": "Investigasi Penyebab Kegagalan Perangkat." },
    { "en": "Apa Itu 'Decapsulation'?", "id": "Membuka Kemasan IC Untuk Analisis." },
    { "en": "Apa Itu 'FIB' (Focused Ion Beam)?", "id": "Alat Untuk Modifikasi Sirkuit Mikroskopis." },
    { "en": "Apa Itu 'SEM' (Scanning Electron Microscope)?", "id": "Mikroskop Untuk Melihat Struktur Sangat Kecil." },
    { "en": "Apa Itu 'Transfer Function Pole'?", "id": "Akar Dari Polinomial Penyebut." },
    { "en": "Apa Itu 'Transfer Function Zero'?", "id": "Akar Dari Polinomial Pembilang." },
    { "en": "Apa Itu 'Routh-Hurwitz Stability Criterion'?", "id": "Metode Matematis Untuk Cek Stabilitas." },
    { "en": "Apa Itu 'BIBO Stability' (Bounded-Input, Bounded-Output)?", "id": "Output Terbatas Untuk Input Terbatas." },
    { "en": "Apa Itu 'Marginal Stability'?", "id": "Sistem Di Ambang Batas Osilasi." },
    { "en": "Apa Itu 'Absolute Phase'?", "id": "Fasa Sinyal Relatif Terhadap Referensi." },
    { "en": "Apa Itu 'Relative Phase'?", "id": "Perbedaan Fasa Antara Dua Sinyal." },
    { "en": "Apa Itu 'Quadrature'?", "id": "Perbedaan Fasa Sebesar 90 Derajat." },
    { "en": "Apa Itu 'In-Phase'?", "id": "Perbedaan Fasa Sebesar 0 Derajat." },
    { "en": "Apa Itu 'Anti-Phase'?", "id": "Perbedaan Fasa Sebesar 180 Derajat." },
    { "en": "Apa Itu 'Lissajous Figure'?", "id": "Pola Osiloskop Untuk Melihat Hubungan Fasa." },
    { "en": "Apa Itu 'Time Domain Reflectometry' (TDR)?", "id": "Mengukur Refleksi Untuk Karakterisasi Impedansi." },
    { "en": "Apa Itu 'Network Analyzer'?", "id": "Mengukur Parameter Jaringan Listrik." },
    { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Mengukur Magnitudo Sinyal Di Domain Frekuensi." },
    { "en": "Apa Itu 'Logic Analyzer'?", "id": "Menganalisis Banyak Sinyal Digital." },
    { "en": "Apa Itu 'Arbitrary Waveform Generator' (AWG)?", "id": "Pembangkit Gelombang Bentuk Apapun." },
    { "en": "Apa Itu 'Probe Loading'?", "id": "Efek Probe Membebani Rangkaian." },
    { "en": "Apa Itu 'Active Probe'?", "id": "Probe Dengan Penguat Internal." },
    { "en": "Apa Itu 'Differential Probe'?", "id": "Probe Untuk Mengukur Sinyal Diferensial." },
    { "en": "Apa Itu 'Current Probe'?", "id": "Probe Untuk Mengukur Arus." },
    { "en": "Apa Itu 'Near-Field Probe'?", "id": "Mendeteksi Medan Elektromagnetik Jarak Dekat." },
    { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik Pada Sirkuit." },
    { "en": "Apa Itu 'EMC' (Electromagnetic Compatibility)?", "id": "Kemampuan Perangkat Berfungsi Tanpa Gangguan." },
    { "en": "Apa Itu 'Radiated Emissions'?", "id": "Pancaran EMI Yang Tidak Diinginkan." },
    { "en": "Apa Itu 'Conducted Emissions'?", "id": "EMI Yang Merambat Lewat Kabel." },
    { "en": "Apa Itu 'Radiated Immunity'?", "id": "Kekebalan Perangkat Terhadap Radiasi EMI." },
    { "en": "Apa Itu 'Conducted Immunity'?", "id": "Kekebalan Perangkat Terhadap Konduksi EMI." },
    { "en": "Apa Itu 'Faraday Shield'?", "id": "Selungkup Konduktif Untuk Blokir EMI." },
    { "en": "Apa Itu 'Gasket EMI'?", "id": "Segel Konduktif Untuk Menutup Celah." },
    { "en": "Apa Itu 'Common-Mode Choke'?", "id": "Filter Penekan Derau Common-mode." },
    { "en": "Apa Itu 'Differential-Mode Choke'?", "id": "Filter Penekan Derau Differential-mode." },
    { "en": "Apa Itu 'Feedthrough Capacitor'?", "id": "Kapasitor Untuk Menyaring Jalur Sinyal." },
    { "en": "Apa Itu 'Transient Voltage Suppressor' (TVS)?", "id": "Komponen Pelindung Lonjakan Tegangan." },
    { "en": "Apa Itu 'Varistor' (MOV)?", "id": "Resistor Tergantung Tegangan." },
    { "en": "Apa Itu 'Gas Discharge Tube' (GDT)?", "id": "Pelindung Lonjakan Tegangan Berbasis Gas." },
    { "en": "Apa Itu 'Polymeric PTC' (Resettable Fuse)?", "id": "Sekring Yang Dapat Pulih Sendiri." },
    { "en": "Apa Itu 'Inrush Current Limiter'?", "id": "Membatasi Arus Awal Saat Dihidupkan." },
    { "en": "Apa Itu 'Thermistor NTC' (Negative Temperature Coefficient)?", "id": "Resistansi Turun Saat Suhu Naik." },
    { "en": "Apa Itu 'Thermistor PTC' (Positive Temperature Coefficient)?", "id": "Resistansi Naik Saat Suhu Naik." },
    { "en": "Apa Itu 'Crowbar Circuit'?", "id": "Rangkaian Proteksi Tegangan Berlebih." },
    { "en": "Apa Itu 'Snubber Circuit'?", "id": "Rangkaian Peredam Lonjakan Tegangan." },
    { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda Pelindung Dari Lonjakan Induktif." },
    { "en": "Apa Itu 'Bootstrap' Dalam Catu Daya?", "id": "Sirkuit Untuk Menaikkan Tegangan Gate Drive." },
    { "en": "Apa Itu 'Under-Voltage Lockout' (UVLO)?", "id": "Mematikan Sirkuit Saat Tegangan Terlalu Rendah." },
    { "en": "Apa Itu 'Over-Voltage Lockout' (OVLO)?", "id": "Mematikan Sirkuit Saat Tegangan Terlalu Tinggi." },
    { "en": "Apa Itu 'Soft Start'?", "id": "Menaikkan Tegangan Output Secara Perlahan." },
    { "en": "Apa Tujuan Soft Start?", "id": "Membatasi Arus Inrush Saat Dihidupkan." },
    { "en": "Apa Itu 'Power Good' Signal?", "id": "Sinyal Indikator Catu Daya Stabil." },
    { "en": "Apa Itu 'Sequencing' Catu Daya?", "id": "Menghidupkan Beberapa Tegangan Secara Berurutan." },
    { "en": "Apa Itu 'Hot Swap Controller'?", "id": "Mengizinkan Pemasangan Papan Saat Sistem Hidup." },
    { "en": "Apa Itu 'OR-ing Controller'?", "id": "Menggabungkan Beberapa Catu Daya Redundan." },
    { "en": "Apa Itu 'Load Sharing'?", "id": "Membagi Beban Secara Merata Antar Catu Daya." },
    { "en": "Apa Itu 'Dropout Voltage'?", "id": "Beda Tegangan Minimal Input-Output Regulator." },
    { "en": "Apa Itu 'Headroom Voltage'?", "id": "Nama Lain Untuk Dropout Voltage." },
    { "en": "Apa Itu 'Kelvin Sensing'?", "id": "Mengukur Tegangan Langsung Di Beban." },
    { "en": "Apa Tujuan Kelvin Sensing?", "id": "Mengkompensasi Jatuh Tegangan Pada Kabel." },
    { "en": "Apa Itu 'SPICE Model Subcircuit'?", "id": "Model Detail Berbasis Transistor Internal." },
    { "en": "Apa Itu 'SPICE Model Behavioral'?", "id": "Model Berbasis Fungsi Matematis." },
    { "en": "Apa Keuntungan Model Behavioral?", "id": "Simulasi Jauh Lebih Cepat." },
    { "en": "Apa Itu 'Convergence Problem'?", "id": "Masalah Saat Simulasi SPICE Gagal." },
    { "en": "Apa Itu 'Time Step' Dalam Simulasi?", "id": "Interval Waktu Antara Titik Kalkulasi." },
    { "en": "Apa Itu 'Initial Condition'?", "id": "Kondisi Awal Rangkaian Saat Simulasi." },
    { "en": "Apa Itu 'Single-Ended Signaling'?", "id": "Sinyal Direferensikan Terhadap Ground." },
    { "en": "Apa Itu 'Differential Signaling'?", "id": "Sinyal Direferensikan Terhadap Sinyal Lain." },
    { "en": "Apa Itu 'Op-Amp Video'?", "id": "Op-Amp Dengan Bandwidth Untuk Sinyal Video." },
    { "en": "Apa Itu 'Differential Gain'?", "id": "Ukuran Perubahan Penguatan Amplitudo." },
    { "en": "Apa Itu 'Differential Phase'?", "id": "Ukuran Perubahan Fasa Amplitudo." },
    { "en": "Apa Itu 'Bandwidth -3dB'?", "id": "Frekuensi Saat Gain Turun 3dB." },
    { "en": "Apa Itu 'Bandwidth Unity Gain'?", "id": "Frekuensi Saat Gain Turun Jadi Satu." },
    { "en": "Apa Itu 'Tegangan Noise Puncak-ke-Puncak'?", "id": "Ukuran Amplitudo Derau Maksimum." },
    { "en": "Apa Itu 'Noise Spectral Density'?", "id": "Distribusi Derau Di Seluruh Frekuensi." },
    { "en": "Apa Itu 'Respon Step Orde Pertama'?", "id": "Respon Eksponensial Menuju Nilai Akhir." },
    { "en": "Apa Itu 'Respon Step Orde Kedua'?", "id": "Bisa Teredam, Kritis, Atau Kurang Teredam." },
    { "en": "Apa Itu 'Rise Time'?", "id": "Waktu Sinyal Naik Dari 10% ke 90%." },
    { "en": "Apa Itu 'Fall Time'?", "id": "Waktu Sinyal Turun Dari 90% ke 10%." },
    { "en": "Apa Itu 'Delay Time'?", "id": "Waktu Sebelum Sinyal Mulai Merespon." },
    { "en": "Apa Itu 'Propagation Delay'?", "id": "Waktu Sinyal Merambat Melalui Op-Amp." },
    { "en": "Apa Itu 'Slew Rate Enhancement'?", "id": "Teknik Untuk Meningkatkan Slew Rate." },
    { "en": "Apa Itu 'Input Capacitance'?", "id": "Kapasitansi Internal Di Terminal Input." },
    { "en": "Apa Itu 'Common-Mode Input Capacitance'?", "id": "Kapasitansi Dilihat Oleh Sinyal Common-mode." },
    { "en": "Apa Itu 'Differential Input Capacitance'?", "id": "Kapasitansi Antara Dua Terminal Input." },
    { "en": "Apa Itu 'Output Impedance vs. Frequency'?", "id": "Impedansi Output Berubah Dengan Frekuensi." },
    { "en": "Mengapa Impedansi Output Naik?", "id": "Karena Penguatan Loop Terbuka Menurun." },
    { "en": "Apa Itu 'Isolation Amplifier'?", "id": "Penguat Dengan Isolasi Listrik." },
    { "en": "Apa Itu 'Leakage Current' Isolator?", "id": "Arus Kecil Yang Melintasi Penghalang." },
    { "en": "Apa Itu 'Isolation Voltage'?", "id": "Tegangan Maksimum Yang Dapat Ditahan Isolator." },
    { "en": "Apa Itu 'Isolation Mode Rejection'?", "id": "Kemampuan Menolak Sinyal Lintas Isolasi." },
    { "en": "Apa Itu 'Audio Power Amplifier'?", "id": "Op-Amp Untuk Menggerakkan Loudspeaker." },
    { "en": "Apa Itu 'Damping Factor'?", "id": "Ukuran Kemampuan Penguat Mengontrol Speaker." },
    { "en": "Apa Itu 'Bridged Tied Load' (BTL)?", "id": "Konfigurasi Untuk Daya Output Empat Kali Lipat." },
    { "en": "Apa Itu 'Headphone Amplifier'?", "id": "Op-Amp Untuk Menggerakkan Headphone." },
    { "en": "Apa Itu 'RIAA Equalization'?", "id": "Standar Ekualisasi Untuk Piringan Hitam." },
    { "en": "Apa Itu 'Tone Control'?", "id": "Sirkuit Untuk Mengatur Bass Dan Treble." },
    { "en": "Apa Itu 'Baxandall Tone Control'?", "id": "Topologi Sirkuit Kontrol Nada Populer." },
    { "en": "Apa Itu 'Volume Control'?", "id": "Mengatur Amplitudo Sinyal Audio." },
    { "en": "Apa Itu 'Logarithmic Potentiometer'?", "id": "Potensiometer Untuk Kontrol Volume." },
    { "en": "Apa Itu 'Balance Control'?", "id": "Mengatur Volume Antara Kanal Kiri-Kanan." },
    { "en": "Apa Itu 'Loudness Control'?", "id": "Meningkatkan Bass/Treble Volume Rendah." },
    { "en": "Apa Itu 'Pre-emphasis'?", "id": "Meningkatkan Frekuensi Tinggi Sebelum Transmisi." },
    { "en": "Apa Itu 'De-emphasis'?", "id": "Mengembalikan Respon Frekuensi Normal." },
    { "en": "Apa Itu 'Dolby Noise Reduction'?", "id": "Sistem Pengurang Derau Kaset Analog." },
    { "en": "Apa Itu 'dBx Noise Reduction'?", "id": "Sistem Pengurang Derau Lainnya." },
    { "en": "Apa Itu 'VU Meter'?", "id": "Indikator Level Volume Unit." },
    { "en": "Apa Itu 'Peak Program Meter' (PPM)?", "id": "Indikator Level Puncak Sinyal." },
    { "en": "Apa Itu 'Audio Compressor'?", "id": "Mengurangi Rentang Dinamis Sinyal." },
    { "en": "Apa Itu 'Audio Limiter'?", "id": "Mencegah Sinyal Melebihi Batas." },
    { "en": "Apa Itu 'Audio Expander'?", "id": "Meningkatkan Rentang Dinamis Sinyal." },
    { "en": "Apa Itu 'Noise Gate'?", "id": "Mematikan Sinyal Di Bawah Ambang Batas." },
    { "en": "Apa Itu 'DI Box' (Direct Injection)?", "id": "Mengubah Sinyal Instrumen Jadi Sinyal Mikrofon." },
    { "en": "Apa Itu 'Phantom Power'?", "id": "Daya DC Dikirim Lewat Kabel Mikrofon." },
    { "en": "Apa Itu 'Balanced Line'?", "id": "Koneksi Audio Profesional Tiga Kawat." },
    { "en": "Apa Itu 'Unbalanced Line'?", "id": "Koneksi Audio Konsumen Dua Kawat." },
    { "en": "Apa Itu 'Ground Loop Hum'?", "id": "Derau Frekuensi Rendah Akibat Ground Loop." },
    { "en": "Bagaimana Mengatasi Ground Loop Hum?", "id": "Menggunakan Ground Lift Atau Isolator." },
    { "en": "Apa Itu 'Line Level'?", "id": "Level Sinyal Standar Antar Perangkat Audio." },
    { "en": "Apa Itu 'Mic Level'?", "id": "Level Sinyal Sangat Rendah Dari Mikrofon." },
    { "en": "Apa Itu 'Instrument Level'?", "id": "Level Sinyal Dari Gitar Listrik." },
    { "en": "Apa Itu 'Speaker Level'?", "id": "Level Daya Tinggi Untuk Menggerakkan Speaker." },
    { "en": "Apa Itu 'dBu'?", "id": "Satuan Desibel Direferensikan Ke 0.775 Volt." },
    { "en": "Apa Itu 'dBV'?", "id": "Satuan Desibel Direferensikan Ke 1 Volt." },
    { "en": "Apa Itu 'dBm'?", "id": "Satuan Desibel Direferensikan Ke 1 Miliwatt." },
    { "en": "Apa Itu 'dBSPL' (Sound Pressure Level)?", "id": "Ukuran Intensitas Suara." },
    { "en": "Apa Itu 'Phono Preamplifier'?", "id": "Penguat Untuk Sinyal Dari Turntable." },
    { "en": "Apa Itu 'Moving Magnet' (MM) Cartridge?", "id": "Jenis Kartrid Piringan Hitam Umum." },
    { "en": "Apa Itu 'Moving Coil' (MC) Cartridge?", "id": "Jenis Kartrid Piringan Hitam Kinerja Tinggi." },
    { "en": "Apa Itu 'Microphonics'?", "id": "Derau Akibat Getaran Mekanis Komponen." },
    { "en": "Apa Itu 'Potentiometer Noise'?", "id": "Derau Listrik Dari Kontak Potensiometer." },
    { "en": "Apa Itu 'Active Tone Control'?", "id": "Kontrol Nada Menggunakan Op-Amp." },
    { "en": "Apa Itu 'Gyrator Equalizer'?", "id": "Equalizer Berbasis Sirkuit Gyrator." },
    { "en": "Apa Itu 'Constant-Q Equalizer'?", "id": "Equalizer Dengan Bandwidth Tetap." },
    { "en": "Apa Itu 'Mixing Console'?", "id": "Peralatan Untuk Menggabungkan Sinyal Audio." },
    { "en": "Apa Itu 'Summing Amplifier' Dalam Mixer?", "id": "Menjumlahkan Sinyal Dari Berbagai Kanal." },
    { "en": "Apa Itu 'Pan Pot' (Panoramic Potentiometer)?", "id": "Mengatur Posisi Sinyal Di Medan Stereo." },
    { "en": "Apa Itu 'Bus' Dalam Mixer Audio?", "id": "Jalur Sinyal Gabungan." },
    { "en": "Apa Itu 'Auxiliary Send' (Aux Send)?", "id": "Output Tambahan Untuk Efek Atau Monitor." },
    { "en": "Apa Itu 'Insert Point'?", "id": "Titik Untuk Menambahkan Prosesor Sinyal Eksternal." },
    { "en": "Apa Itu 'PFL' (Pre-Fade Listen)?", "id": "Mendengarkan Sinyal Sebelum Diatur Fader." },
    { "en": "Apa Itu 'AFL' (After-Fade Listen)?", "id": "Mendengarkan Sinyal Setelah Diatur Fader." },
    { "en": "Apa Itu 'Solo' Tombol?", "id": "Membisukan Semua Kanal Lain." },
    { "en": "Apa Itu 'Fader'?", "id": "Kontrol Geser Untuk Level Sinyal." },
    { "en": "Apa Itu 'Gain Staging'?", "id": "Mengatur Level Penguatan Setiap Tahap." },
    { "en": "Apa Itu 'Headroom'?", "id": "Ruang Antara Level Sinyal-Clipping." },
    { "en": "Apa Itu 'Noise Floor'?", "id": "Level Derau Dasar Sistem." },
    { "en": "Apa Itu 'Signal-to-Noise Ratio' (SNR)?", "id": "Rasio Antara Sinyal Dan Derau." },
    { "en": "Apa Itu 'Dynamic Range'?", "id": "Rasio Sinyal Terkeras Dan Terlemah." },
    { "en": "Apa Itu 'Unity Gain'?", "id": "Penguatan Sama Dengan Satu (0 dB)." },
    { "en": "Apa Itu 'Line Amplifier'?", "id": "Menguatkan Sinyal Ke Line Level." },
    { "en": "Apa Itu 'Distribution Amplifier'?", "id": "Membagi Satu Sinyal Ke Banyak Output." },
    { "en": "Apa Itu 'Voltage-Controlled Amplifier' (VCA)?", "id": "Penguat Dengan Gain Terkendali Tegangan." },
    { "en": "Bagaimana VCA Digunakan Dalam Otomasi?", "id": "Mengontrol Level Sinyal Secara Otomatis." },
    { "en": "Apa Itu 'Digitally-Controlled Potentiometer' (DCP)?", "id": "Potensiometer Yang Dikontrol Secara Digital." },
    { "en": "Apa Itu 'Sample Rate Converter'?", "id": "Mengubah Laju Sampel Sinyal Digital." },
    { "en": "Apa Itu 'Dithering'?", "id": "Menambahkan Derau Untuk Mengurangi Error Kuantisasi." },
    { "en": "Apa Itu 'Truncation'?", "id": "Membuang Bit Paling Tidak Signifikan." },
    { "en": "Apa Itu 'Floating-Point' Arithmetic?", "id": "Aritmetika Untuk Angka Rentang Lebar." },
    { "en": "Apa Itu 'Fixed-Point' Arithmetic?", "id": "Aritmetika Dengan Jumlah Digit Tetap." },
    { "en": "Apa Itu 'DSP' (Digital Signal Processor)?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
    { "en": "Apa Itu 'MAC' (Multiply-Accumulate) Instruction?", "id": "Operasi Fundamental Dalam DSP." },
    { "en": "Apa Itu 'Convolution Reverb'?", "id": "Efek Gema Berbasis Respon Impuls." },
    { "en": "Apa Itu 'Impulse Response' (IR)?", "id": "Rekaman Karakteristik Akustik Ruang." },
    { "en": "Apa Itu 'FFT' (Fast Fourier Transform)?", "id": "Algoritma Cepat Untuk Analisis Fourier." },
    { "en": "Apa Itu 'Time-Stretching'?", "id": "Mengubah Tempo Audio Tanpa Mengubah Pitch." },
    { "en": "Apa Itu 'Pitch-Shifting'?", "id": "Mengubah Pitch Audio Tanpa Mengubah Tempo." },
    { "en": "Apa Itu 'Auto-Tune'?", "id": "Proses Koreksi Pitch Vokal Otomatis." },
    { "en": "Apa Itu 'Vocoder'?", "id": "Menganalisis Dan Mensintesis Suara Manusia." },
    { "en": "Apa Itu 'Synthesizer'?", "id": "Alat Musik Elektronik Pembangkit Suara." },
    { "en": "Apa Itu 'Subtractive Synthesis'?", "id": "Sintesis Suara Dengan Menyaring Harmonik." },
    { "en": "Apa Itu 'Additive Synthesis'?", "id": "Sintesis Suara Dengan Menjumlahkan Sinusoida." },
    { "en": "Apa Itu 'FM Synthesis'?", "id": "Sintesis Suara Menggunakan Modulasi Frekuensi." },
    { "en": "Apa Itu 'Wavetable Synthesis'?", "id": "Sintesis Suara Menggunakan Sampel Gelombang." },
    { "en": "Apa Itu 'ADSR Envelope'?", "id": "Kontrol (Attack, Decay, Sustain, Release)." },
    { "en": "Apa Itu 'LFO' (Low-Frequency Oscillator)?", "id": "Osilator Lambat Untuk Efek Modulasi." },
    { "en": "Apa Itu 'MIDI' (Musical Instrument Digital Interface)?", "id": "Protokol Komunikasi Untuk Instrumen Musik." },
    { "en": "Apa Itu 'DAW' (Digital Audio Workstation)?", "id": "Perangkat Lunak Untuk Produksi Musik." },
    { "en": "Apa Itu 'Audio Plugin' (VST, AU)?", "id": "Perangkat Lunak Efek Atau Instrumen Tambahan." },
    { "en": "Apa Itu 'Latency' Dalam Audio Digital?", "id": "Penundaan Waktu Pemrosesan Sinyal." },
    { "en": "Apa Itu 'ASIO' (Audio Stream Input/Output)?", "id": "Driver Audio Latency Rendah Untuk Windows." },
    { "en": "Apa Itu 'Core Audio'?", "id": "Framework Audio Latency Rendah Untuk macOS." },
    { "en": "Apa Itu 'Sound Card' (Kartu Suara)?", "id": "Perangkat Keras Untuk Input/Output Audio." },
    { "en": "Apa Itu 'Audio Interface'?", "id": "Perangkat Eksternal Untuk Input/Output Audio." },
    { "en": "Apa Itu 'Preamplifier Mikrofon'?", "id": "Menguatkan Sinyal Lemah Dari Mikrofon." },
    { "en": "Apa Itu 'Mikrofon Kondensor'?", "id": "Mikrofon Yang Membutuhkan Phantom Power." },
    { "en": "Apa Itu 'Mikrofon Dinamis'?", "id": "Mikrofon Yang Tidak Butuh Daya." },
    { "en": "Apa Itu 'Pola Kutub' (Polar Pattern)?", "id": "Sensitivitas Mikrofon Terhadap Arah." },
    { "en": "Apa Itu 'Cardioid'?", "id": "Pola Kutub Berbentuk Hati." },
    { "en": "Apa Itu 'Omnidirectional'?", "id": "Sensitif Terhadap Semua Arah." },
    { "en": "Apa Itu 'Bidirectional' (Figure-8)?", "id": "Sensitif Terhadap Depan Dan Belakang." },
    { "en": "Apa Itu 'Efek Proksimitas'?", "id": "Peningkatan Respon Bass Dekat Sumber." },
    { "en": "Apa Itu 'Pop Filter'?", "id": "Menghilangkan Bunyi Letupan Vokal." },
    { "en": "Apa Itu 'Shock Mount'?", "id": "Mengisolasi Mikrofon Dari Getaran." },
    { "en": "Apa Itu 'Kabel XLR'?", "id": "Konektor Tiga Pin Untuk Audio Profesional." },
    { "en": "Apa Itu 'Kabel TRS' (Tip-Ring-Sleeve)?", "id": "Konektor Untuk Sinyal Balanced Atau Stereo." },
    { "en": "Apa Itu 'Kabel TS' (Tip-Sleeve)?", "id": "Konektor Untuk Sinyal Unbalanced Mono." },
    { "en": "Apa Itu 'Impedansi Mikrofon'?", "id": "Biasanya Rendah (Low-Z)." },
    { "en": "Apa Itu 'Impedansi Input Preamplifier'?", "id": "Harus Jauh Lebih Tinggi Dari Mikrofon." },
    { "en": "Apa Itu 'Impedance Bridging'?", "id": "Koneksi Input Impedansi Tinggi-Rendah." },
    { "en": "Apa Itu 'Noise Gate' Dalam Audio?", "id": "Mematikan Sinyal Di Bawah Ambang Batas." },
    { "en": "Apa Itu 'De-Esser'?", "id": "Mengurangi Bunyi Sibilan (Sss) Berlebih." },
    { "en": "Apa Itu 'Exciter'?", "id": "Menambahkan Harmonik Untuk Kecerahan." },
    { "en": "Apa Itu 'Reverb' (Gema)?", "id": "Simulasi Pantulan Suara Dalam Ruang." },
    { "en": "Apa Itu 'Delay' (Tunda)?", "id": "Mengulang Sinyal Setelah Penundaan Waktu." },
    { "en": "Apa Itu 'Chorus'?", "id": "Efek Penebal Suara." },
    { "en": "Apa Itu 'Flanger'?", "id": "Efek Suara Jet Berbasis Delay." },
    { "en": "Apa Itu 'Phaser'?", "id": "Efek Berbasis Pergeseran Fasa." },
    { "en": "Apa Itu 'Wah-wah'?", "id": "Filter Yang Digeser Secara Manual." },
    { "en": "Apa Itu 'Distortion'?", "id": "Efek Clipping Sinyal (Umumnya Gitar)." },
    { "en": "Apa Itu 'Overdrive'?", "id": "Bentuk Distorsi Yang Lebih Ringan." },
    { "en": "Apa Itu 'Fuzz'?", "id": "Bentuk Distorsi Yang Ekstrem." },
    { "en": "Apa Itu 'Amplifier Gitar'?", "id": "Penguat Khusus Untuk Gitar Listrik." },
    { "en": "Apa Itu 'Cabinet' Speaker?", "id": "Kotak Yang Berisi Driver Speaker." },
    { "en": "Apa Itu 'Speaker Driver'?", "id": "Komponen Transduser Dalam Speaker." },
    { "en": "Apa Itu 'Woofer'?", "id": "Driver Speaker Untuk Frekuensi Rendah." },
    { "en": "Apa Itu 'Mid-range'?", "id": "Driver Speaker Untuk Frekuensi Tengah." },
    { "en": "Apa Itu 'Tweeter'?", "id": "Driver Speaker Untuk Frekuensi Tinggi." },
    { "en": "Apa Itu 'Super Tweeter'?", "id": "Driver Untuk Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu 'Subwoofer'?", "id": "Speaker Khusus Frekuensi Sangat Rendah." },
    { "en": "Apa Itu 'Port' (Pada Speaker)?", "id": "Lubang Untuk Meningkatkan Respon Bass." },
    { "en": "Apa Itu 'Sealed Enclosure'?", "id": "Desain Kotak Speaker Tertutup." },
    { "en": "Apa Itu 'Ported Enclosure' (Bass Reflex)?", "id": "Desain Kotak Speaker Dengan Port." },
    { "en": "Apa Itu 'Passive Radiator'?", "id": "Konus Tanpa Driver Untuk Resonansi." },
    { "en": "Apa Itu 'Transmission Line' Speaker?", "id": "Desain Speaker Dengan Pipa Internal." },
    { "en": "Apa Itu 'Horn Loudspeaker'?", "id": "Speaker Dengan Pemandu Gelombang Corong." },
    { "en": "Apa Itu 'Electrostatic Loudspeaker'?", "id": "Speaker Menggunakan Panel Elektrostatis." },
    { "en": "Apa Itu 'Planar Magnetic' Speaker?", "id": "Speaker Menggunakan Diafragma Datar." },
    { "en": "Apa Itu 'Bi-amping'?", "id": "Menggunakan Penguat Terpisah Untuk Woofer/Tweeter." },
    { "en": "Apa Itu 'Tri-amping'?", "id": "Bi-amping Dengan Tambahan Penguat Mid-range." },
    { "en": "Apa Itu 'Speaker Aktif'?", "id": "Speaker Dengan Penguat Terintegrasi." },
    { "en": "Apa Itu 'Speaker Pasif'?", "id": "Speaker Yang Membutuhkan Penguat Eksternal." },
    { "en": "Apa Itu 'Impedansi Nominal' Speaker?", "id": "Resistansi Rata-rata Speaker." },
    { "en": "Apa Itu 'Sensitivitas Speaker'?", "id": "Ukuran Efisiensi (dB per Watt)." },
    { "en": "Apa Itu 'Respon Frekuensi' Speaker?", "id": "Rentang Frekuensi Yang Dapat Direproduksi." },
    { "en": "Apa Itu 'Room Acoustics'?", "id": "Perilaku Suara Dalam Sebuah Ruangan." },
    { "en": "Apa Itu 'Standing Waves'?", "id": "Gelombang Stasioner Dalam Ruangan." },
    { "en": "Apa Itu 'Room Modes'?", "id": "Frekuensi Resonansi Sebuah Ruangan." },
    { "en": "Apa Itu 'Bass Trap'?", "id": "Perangkat Akustik Penyerap Frekuensi Rendah." },
    { "en": "Apa Itu 'Diffuser'?", "id": "Perangkat Akustik Penyebar Pantulan Suara." },
    { "en": "Apa Itu 'Absorber'?", "id": "Perangkat Akustik Penyerap Energi Suara." },
    { "en": "Apa Itu 'Waktu Dengung' (Reverberation Time)?", "id": "Waktu Gema Menurun 60 dB." },
    { "en": "Apa Itu 'Soundproofing'?", "id": "Mengisolasi Suara Antar Ruangan." },
    { "en": "Apa Itu 'Sound Masking'?", "id": "Menambahkan Derau Latar Untuk Privasi." },
    { "en": "Apa Itu 'Op-Amp Terintegrasi'?", "id": "Op-Amp Dalam Bentuk Sirkuit Terpadu." },
    { "en": "Apa Itu 'Op-Amp Diskrit'?", "id": "Op-Amp Dibangun Dari Komponen Individual." },
    { "en": "Apa Keuntungan Op-Amp Diskrit?", "id": "Kinerja Dapat Dioptimalkan Secara Penuh." },
    { "en": "Apa Kerugian Op-Amp Diskrit?", "id": "Ukuran Besar, Biaya Tinggi." },
    { "en": "Apa Itu 'Dielectric Constant'?", "id": "Ukuran Kemampuan Bahan Simpan Energi." },
    { "en": "Apa Itu 'Loss Tangent'?", "id": "Ukuran Kerugian Energi Dalam Dielektrik." },
    { "en": "Apa Itu 'H-Parameters'?", "id": "Parameter Hibrida Untuk Model Transistor." },
    { "en": "Apa Itu 'Y-Parameters'?", "id": "Parameter Admitansi Untuk Jaringan Dua Port." },
    { "en": "Apa Itu 'Z-Parameters'?", "id": "Parameter Impedansi Untuk Jaringan Dua Port." },
    { "en": "Apa Itu 'S-Parameters'?", "id": "Parameter Hamburan Untuk Analisis Frekuensi Tinggi." },
    { "en": "Apa Itu 'Return Loss'?", "id": "Ukuran Sinyal Yang Dipantulkan Kembali." },
    { "en": "Apa Itu 'Insertion Loss'?", "id": "Ukuran Kehilangan Sinyal Lewati Perangkat." },
    { "en": "Apa Itu 'Smith Chart'?", "id": "Grafik Untuk Analisis Impedansi RF." },
    { "en": "Apa Itu 'VSWR' (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri Tegangan." },
    { "en": "Nilai VSWR Yang Ideal?", "id": "1:1 (Tidak Ada Refleksi)." },
    { "en": "Apa Itu 'Impedance Matching'?", "id": "Menyesuaikan Impedansi Untuk Transfer Daya." },
    { "en": "Apa Itu 'Tuner Antena'?", "id": "Jaringan Penyesuai Impedansi Untuk Antena." },
    { "en": "Apa Itu 'Balun'?", "id": "Mengubah Saluran Seimbang Jadi Tak Seimbang." },
    { "en": "Apa Itu 'Unun'?", "id": "Mengubah Impedansi Saluran Tak Seimbang." },
    { "en": "Apa Itu 'Power Splitter'?", "id": "Membagi Sinyal RF Ke Banyak Jalur." },
    { "en": "Apa Itu 'Power Combiner'?", "id": "Menggabungkan Sinyal RF Dari Banyak Jalur." },
    { "en": "Apa Itu 'Directional Coupler'?", "id": "Mencuplik Sebagian Daya Sinyal RF." },
    { "en": "Apa Itu 'Circulator'?", "id": "Perangkat RF Tiga Port." },
    { "en": "Apa Itu 'Isolator'?", "id": "Memungkinkan Sinyal Lewat Satu Arah." },
    { "en": "Apa Itu 'Attenuator'?", "id": "Mengurangi Kekuatan Sinyal." },
    { "en": "Apa Itu 'RF Switch'?", "id": "Saklar Elektronik Untuk Sinyal RF." },
    { "en": "Apa Itu 'Phase Shifter'?", "id": "Rangkaian Untuk Menggeser Fasa Sinyal." },
    { "en": "Apa Itu 'RF Mixer'?", "id": "Mengalikan Dua Sinyal Frekuensi." },
    { "en": "Apa Itu 'Up-conversion'?", "id": "Mengubah Frekuensi Rendah Ke Tinggi." },
    { "en": "Apa Itu 'Down-conversion'?", "id": "Mengubah Frekuensi Tinggi Ke Rendah." },
    { "en": "Apa Itu 'Superheterodyne Receiver'?", "id": "Arsitektur Penerima Radio Umum." },
    { "en": "Apa Itu 'Image Frequency'?", "id": "Frekuensi Palsu Dalam Penerima Superheterodyne." },
    { "en": "Apa Itu 'Image Rejection Filter'?", "id": "Filter Untuk Menekan Frekuensi Cermin." },
    { "en": "Apa Itu 'Local Oscillator' (LO)?", "id": "Osilator Internal Untuk Pencampuran." },
    { "en": "Apa Itu 'Intermediate Frequency' (IF)?", "id": "Frekuensi Tetap Hasil Pencampuran." },
    { "en": "Apa Itu 'IF Amplifier'?", "id": "Penguat Pada Frekuensi Menengah." },
    { "en": "Apa Itu 'Detector'?", "id": "Mengekstrak Informasi Dari Sinyal Termodulasi." },
    { "en": "Apa Itu 'Envelope Detector'?", "id": "Detektor Untuk Sinyal Modulasi Amplitudo (AM)." },
    { "en": "Apa Itu 'Frequency Discriminator'?", "id": "Detektor Untuk Sinyal Modulasi Frekuensi (FM)." },
    { "en": "Apa Itu 'Phase Detector' Dalam PLL?", "id": "Membandingkan Fasa Sinyal Input-VCO." },
    { "en": "Apa Itu 'Loop Filter'?", "id": "Filter Low-pass Dalam PLL." },
    { "en": "Apa Itu 'Lock Range'?", "id": "Rentang Frekuensi PLL Dapat Kunci." },
    { "en": "Apa Itu 'Capture Range'?", "id": "Rentang Frekuensi PLL Mulai Kunci." },
    { "en": "Apa Itu 'Frequency Synthesizer'?", "id": "Membangkitkan Berbagai Frekuensi Dari Referensi." },
    { "en": "Apa Itu 'Direct Digital Synthesis' (DDS)?", "id": "Sintesis Frekuensi Secara Digital." },
    { "en": "Apa Itu 'Phase Noise'?", "id": "Derau Fasa Jangka Pendek Osilator." },
    { "en": "Apa Itu 'Jitter' Dalam Osilator?", "id": "Variasi Waktu Tepi Sinyal." },
    { "en": "Apa Itu 'Allan Variance'?", "id": "Ukuran Stabilitas Frekuensi Jangka Panjang." },
    { "en": "Apa Itu 'Oven-Controlled Crystal Oscillator' (OCXO)?", "id": "Osilator Kristal Stabilitas Sangat Tinggi." },
    { "en": "Apa Itu 'Temperature-Compensated Crystal Oscillator' (TCXO)?", "id": "Osilator Kristal Dengan Kompensasi Suhu." },
    { "en": "Apa Itu 'Voltage-Controlled Crystal Oscillator' (VCXO)?", "id": "Frekuensi Kristal Dapat Diatur Tegangan." },
    { "en": "Apa Itu 'Ceramic Resonator'?", "id": "Resonator Lebih Murah Dari Kristal." },
    { "en": "Apa Itu 'SAW Resonator'?", "id": "Resonator Gelombang Akustik Permukaan." },
    { "en": "Apa Itu 'Dielectric Resonator'?", "id": "Resonator Berbasis Material Keramik." },
    { "en": "Apa Itu 'Cavity Resonator'?", "id": "Resonator Berbasis Rongga Logam." },
    { "en": "Apa Itu 'Waveguide'?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
    { "en": "Apa Itu 'Microstrip Line'?", "id": "Saluran Transmisi Pada Papan PCB." },
    { "en": "Apa Itu 'Stripline'?", "id": "Saluran Transmisi Diapit Dua Ground." },
    { "en": "Apa Itu 'Coplanar Waveguide'?", "id": "Saluran Transmisi Dengan Ground Sebidang." },
    { "en": "Apa Itu 'Skin Effect'?", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
    { "en": "Apa Itu 'Proximity Effect'?", "id": "Distribusi Arus Akibat Konduktor Berdekatan." },
    { "en": "Apa Itu 'Litz Wire'?", "id": "Kawat Untuk Mengurangi Efek Skin." },
    { "en": "Apa Itu 'Ferrite Core'?", "id": "Inti Magnetik Untuk Induktor/Trafo." },
    { "en": "Apa Itu 'Toroidal Core'?", "id": "Inti Magnetik Berbentuk Donat." },
    { "en": "Apa Itu 'Pot Core'?", "id": "Inti Ferit Yang Melindungi Belitan." },
    { "en": "Apa Itu 'E Core'?", "id": "Inti Ferit Berbentuk Huruf E." },
    { "en": "Apa Itu 'Air Gap'?", "id": "Celah Udara Dalam Inti Magnetik." },
    { "en": "Mengapa Perlu Air Gap?", "id": "Untuk Mencegah Saturasi Inti." },
    { "en": "Apa Itu 'Indeks AL'?", "id": "Karakteristik Inti Ferit." },
    { "en": "Apa Itu 'Hysteresis Loop'?", "id": "Grafik B-H Material Feromagnetik." },
    { "en": "Apa Itu 'Remanence'?", "id": "Sisa Magnetisasi Setelah Medan Dihilangkan." },
    { "en": "Apa Itu 'Coercivity'?", "id": "Kekuatan Medan Balik Untuk Hilangkan Magnetisasi." },
    { "en": "Apa Itu 'Eddy Current Loss'?", "id": "Kerugian Daya Akibat Arus Eddy." },
    { "en": "Apa Itu 'Hysteresis Loss'?", "id": "Kerugian Energi Dalam Inti Magnetik." },
    { "en": "Apa Itu 'Laminated Core'?", "id": "Inti Berlapis Untuk Kurangi Arus Eddy." },
    { "en": "Apa Itu 'Flyback Transformer'?", "id": "Trafo Penyimpan Energi." },
    { "en": "Apa Itu 'Forward Converter'?", "id": "Jenis Konverter DC-DC Terisolasi." },
    { "en": "Apa Itu 'Push-Pull Converter'?", "id": "Konverter Dengan Dua Transistor Saklar." },
    { "en": "Apa Itu 'Half-Bridge Converter'?", "id": "Konverter Menggunakan Setengah Jembatan." },
    { "en": "Apa Itu 'Full-Bridge Converter'?", "id": "Konverter Menggunakan Jembatan Penuh." },
    { "en": "Apa Itu 'Resonant Converter'?", "id": "Konverter Menggunakan Sirkuit Resonansi." },
    { "en": "Apa Itu 'Zero-Voltage Switching' (ZVS)?", "id": "Pensaklaran Saat Tegangan Nol." },
    { "en": "Apa Itu 'Zero-Current Switching' (ZCS)?", "id": "Pensaklaran Saat Arus Nol." },
    { "en": "Apa Keuntungan ZVS/ZCS?", "id": "Mengurangi Kerugian Daya Pensaklaran." },
    { "en": "Apa Itu 'Gate Driver'?", "id": "Sirkuit Untuk Menggerakkan Gerbang MOSFET." },
    { "en": "Apa Itu 'Charge Injection'?", "id": "Efek Tak Diinginkan Pada Saklar Analog." },
    { "en": "Apa Itu 'Clock Feedthrough'?", "id": "Kebocoran Sinyal Clock Ke Sinyal Analog." },
    { "en": "Apa Itu 'Droop Rate'?", "id": "Laju Penurunan Tegangan Tertahan." },
    { "en": "Apa Itu 'Aperture Jitter'?", "id": "Variasi Waktu Pada Momen Sampling." },
    { "en": "Apa Itu 'Metastability'?", "id": "Keadaan Tak Tentu Dalam Sirkuit Digital." },
    { "en": "Apa Itu 'Comparator Hysteresis'?", "id": "Mencegah Osilasi Di Dekat Titik Ambang." },
    { "en": "Apa Itu 'Kickback Noise'?", "id": "Derau Dari Komparator Kembali Ke Input." },
    { "en": "Apa Itu 'Thermal Tail'?", "id": "Error Respon Step Akibat Efek Termal." },
    { "en": "Apa Itu 'Input Bias Current Cancellation'?", "id": "Teknik Mengurangi Error Arus Bias." },
    { "en": "Apa Itu 'Long-Tailed Pair'?", "id": "Tahap Input Diferensial Dasar." },
    { "en": "Apa Itu 'Gilbert Cell'?", "id": "Inti Dari Mixer Analog." },
    { "en": "Apa Itu 'Blackmer Gain Cell'?", "id": "Inti Dari VCA Kinerja Tinggi." },
    { "en": "Apa Itu 'Operational Amplifier Topologies'?", "id": "Berbagai Konfigurasi Rangkaian Op-Amp." },
    { "en": "Apa Itu 'Single-Stage Amplifier'?", "id": "Penguat Dengan Satu Tahap Penguatan." },
    { "en": "Apa Itu 'Telescopic Cascode'?", "id": "Topologi Op-Amp Gain Sangat Tinggi." },
    { "en": "Apa Itu 'Folded Cascode'?", "id": "Topologi Op-Amp Dengan Rentang Input Luas." },
    { "en": "Apa Itu 'Recycling Folded Cascode'?", "id": "Peningkatan Dari Topologi Folded Cascode." },
    { "en": "Apa Itu 'Gain Boosting'?", "id": "Teknik Meningkatkan Penguatan Loop Terbuka." },
    { "en": "Apa Itu 'Output Stage'?", "id": "Tahap Akhir Op-Amp Untuk Menggerakkan Beban." },
    { "en": "Apa Itu 'Quiescent Current Control'?", "id": "Mengatur Arus Diam Tahap Output." },
    { "en": "Apa Itu 'Common-Mode Feedback' (CMFB)?", "id": "Menstabilkan Tegangan Common-mode FDA." },
    { "en": "Apa Itu 'Slewing'?", "id": "Kondisi Output Terbatas Slew Rate." },
    { "en": "Apa Itu 'Linear Settling'?", "id": "Bagian Akhir Dari Respon Step." },
    { "en": "Apa Itu 'DC-Coupled'?", "id": "Melewatkan Komponen DC." },
    { "en": "Apa Itu 'AC-Coupled'?", "id": "Menghalangi Komponen DC." },
    { "en": "Apa Itu 'Decoupling'?", "id": "Mengisolasi Antar Bagian Sirkuit." },
    { "en": "Apa Itu 'Bypass'?", "id": "Memberi Jalur Impedansi Rendah." },
    { "en": "Apa Itu 'Noise Figure Degradation'?", "id": "Peningkatan Noise Figure Sistem Total." },
    { "en": "Apa Itu 'Friis Formula'?", "id": "Menghitung Noise Figure Sistem Kaskade." },
    { "en": "Apa Itu 'Sensitivity' Penerima?", "id": "Sinyal Terlemah Yang Dapat Dideteksi." },
    { "en": "Apa Itu 'Selectivity' Penerima?", "id": "Kemampuan Memilih Sinyal Yang Diinginkan." },
    { "en": "Apa Itu 'Fidelity'?", "id": "Tingkat Akurasi Reproduksi Sinyal." },
    { "en": "Apa Itu 'Head-Related Transfer Function' (HRTF)?", "id": "Fungsi Transfer Telinga Manusia." },
    { "en": "Apa Itu 'Binaural Recording'?", "id": "Merekam Suara Seperti Didengar Manusia." },
    { "en": "Apa Itu 'Ambisonics'?", "id": "Format Audio Medan Suara Penuh." },
    { "en": "Apa Itu 'Psychoacoustics'?", "id": "Studi Persepsi Suara Manusia." },
    { "en": "Apa Itu 'Auditory Masking'?", "id": "Satu Suara Menutupi Suara Lain." },
    { "en": "Apa Itu 'Equal-Loudness Contour'?", "id": "Kurva Sensitivitas Pendengaran Manusia." },
    { "en": "Apa Itu 'Phon'?", "id": "Satuan Tingkat Kekerasan Suara Subjektif." },
    { "en": "Apa Itu 'Sone'?", "id": "Satuan Kekerasan Suara Subjektif." },
    { "en": "Apa Itu 'Sound Intensity'?", "id": "Daya Akustik Per Satuan Luas." },
    { "en": "Apa Itu 'Acoustic Impedance'?", "id": "Ukuran Perlawanan Medium Gelombang Suara." },
    { "en": "Apa Itu 'Reflection' (Pantulan)?", "id": "Gelombang Memantul Dari Permukaan." },
    { "en": "Apa Itu 'Refraction' (Pembiasan)?", "id": "Gelombang Berbelok Saat Lewati Medium." },
    { "en": "Apa Itu 'Diffraction' (Difraksi)?", "id": "Gelombang Menyebar Saat Lewati Celah." },
    { "en": "Apa Itu 'Absorption' (Penyerapan)?", "id": "Energi Gelombang Diubah Jadi Panas." },
    { "en": "Apa Itu 'Doppler Effect'?", "id": "Perubahan Frekuensi Akibat Gerakan." },
    { "en": "Apa Itu 'Beats'?", "id": "Interferensi Dua Frekuensi Berdekatan." },
    { "en": "Apa Itu 'Harmonics'?", "id": "Frekuensi Kelipatan Dari Frekuensi Dasar." },
    { "en": "Apa Itu 'Fundamental Frequency'?", "id": "Frekuensi Terendah Dalam Sinyal Kompleks." },
    { "en": "Apa Itu 'Overtones'?", "id": "Harmonik Selain Frekuensi Fundamental." },
    { "en": "Apa Itu 'Timbre'?", "id": "Kualitas Suara Yang Membedakan Instrumen." },
    { "en": "Apa Itu 'White Point'?", "id": "Referensi Warna Putih Dalam Tampilan." },
    { "en": "Apa Itu 'Color Temperature'?", "id": "Ukuran Karakteristik Warna Cahaya." },
    { "en": "Apa Itu 'Gamma Correction'?", "id": "Koreksi Non-linear Kecerahan Tampilan." },
    { "en": "Apa Itu 'Contrast Ratio'?", "id": "Rasio Antara Putih Terterang-Hitam Tergelap." },
    { "en": "Apa Itu 'Brightness'?", "id": "Ukuran Luminansi Total Tampilan." },
    { "en": "Apa Itu 'Viewing Angle'?", "id": "Sudut Pandang Maksimum Tampilan." },
    { "en": "Apa Itu 'Response Time'?", "id": "Waktu Piksel Berubah Warna." },
    { "en": "Apa Itu 'Refresh Rate'?", "id": "Berapa Kali Layar Diperbarui Per Detik." },
    { "en": "Apa Itu 'Dead Pixel'?", "id": "Piksel Yang Gagal Berfungsi." },
    { "en": "Apa Itu 'Stuck Pixel'?", "id": "Piksel Yang Selalu Menyala." },
    { "en": "Apa Itu 'Backlight'?", "id": "Sumber Cahaya Untuk Layar LCD." },
    { "en": "Apa Itu 'Edge-Lit'?", "id": "Backlight Dengan LED Di Tepi." },
    { "en": "Apa Itu 'Direct-Lit' (Full-Array)?", "id": "Backlight Dengan LED Di Belakang Panel." },
    { "en": "Apa Itu 'Local Dimming'?", "id": "Mengontrol Kecerahan Zona Backlight." },
    { "en": "Apa Itu 'OLED' (Organic Light Emitting Diode)?", "id": "Teknologi Tampilan Self-Emissive." },
    { "en": "Apa Itu 'Burn-in' (Pada OLED)?", "id": "Degradasi Piksel Yang Tidak Merata." },
    { "en": "Apa Itu 'MicroLED'?", "id": "Teknologi Tampilan Self-Emissive Berikutnya." },
    { "en": "Apa Itu 'Quantum Dots'?", "id": "Kristal Nano Untuk Meningkatkan Warna LCD." },
    { "en": "Apa Itu 'HDR' (High Dynamic Range)?", "id": "Rentang Kecerahan Dan Warna Lebih Luas." },
    { "en": "Apa Itu 'Dolby Vision'?", "id": "Standar HDR Dengan Metadata Dinamis." },
    { "en": "Apa Itu 'HDR10+'?", "id": "Standar HDR Lain Dengan Metadata Dinamis." },
    { "en": "Apa Itu 'Color Depth'?", "id": "Jumlah Bit Untuk Merepresentasikan Warna." },
    { "en": "Apa Itu '8-bit Color'?", "id": "Sekitar 16.7 Juta Warna." },
    { "en": "Apa Itu '10-bit Color'?", "id": "Sekitar 1.07 Miliar Warna." },
    { "en": "Apa Itu 'Banding' (Posterization)?", "id": "Transisi Warna Kasar Akibat Color Depth." },
    { "en": "Apa Itu 'Upscaling'?", "id": "Mengubah Video Resolusi Rendah-Tinggi." },
    { "en": "Apa Itu 'Interpolasi'?", "id": "Membuat Piksel Baru Berdasarkan Tetangganya." },
    { "en": "Apa Itu 'Motion Interpolation'?", "id": "Membuat Frame Tambahan Untuk Gerakan Halus." },
    { "en": "Apa Itu 'Soap Opera Effect'?", "id": "Efek Gerakan Terlalu Halus." },
    { "en": "Apa Itu 'Latency' Tampilan (Input Lag)?", "id": "Penundaan Antara Input Dan Tampilan." },
    { "en": "Apa Itu 'Variable Refresh Rate' (VRR)?", "id": "Sinkronisasi Refresh Rate Dengan GPU." },
    { "en": "Apa Itu 'G-Sync'?", "id": "Teknologi VRR Dari Nvidia." },
    { "en": "Apa Itu 'FreeSync'?", "id": "Teknologi VRR Dari AMD." },
    { "en": "Apa Itu 'Tearing'?", "id": "Artefak Visual Akibat Sinkronisasi Buruk." },
    { "en": "Apa Itu 'V-Sync' (Vertical Sync)?", "id": "Membatasi Frame Rate Ke Refresh Rate." },
    { "en": "Apa Itu 'Colorimeter'?", "id": "Alat Untuk Mengukur Dan Mengkalibrasi Warna." },
    { "en": "Apa Itu 'Spectrophotometer'?", "id": "Alat Pengukur Warna Lebih Akurat." },
    { "en": "Apa Itu 'ICC Profile'?", "id": "File Data Karakteristik Warna Perangkat." },
    { "en": "Apa Itu 'Color Management'?", "id": "Menjaga Konsistensi Warna Antar Perangkat." },
    { "en": "Apa Itu 'DCI-P3'?", "id": "Ruang Warna Umum Untuk Sinema Digital." },
    { "en": "Apa Itu 'sRGB'?", "id": "Ruang Warna Standar Untuk Web." },
    { "en": "Apa Itu 'Adobe RGB'?", "id": "Ruang Warna Dengan Gamut Lebih Luas." },
    { "en": "Apa Itu 'Rec. 709'?", "id": "Standar Ruang Warna Untuk HDTV." },
    { "en": "Apa Itu 'Rec. 2020'?", "id": "Standar Ruang Warna Untuk UHDTV." },
    { "en": "Apa Itu 'Chromaticity Diagram'?", "id": "Representasi Grafis Semua Warna Terlihat." },
    { "en": "Apa Itu 'Color Space'?", "id": "Rentang Warna Spesifik." }


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
