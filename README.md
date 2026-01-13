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


    { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Dioda Terkendali Arus Satu Arah." },
    { "en": "Apa Itu 'TRIAC' (Triode For Alternating Current)?", "id": "Saklar Elektronik Arus Bolak Balik." },
    { "en": "Apa Itu 'DIAC' (Diode For Alternating Current)?", "id": "Pemicu Dua Arah Tegangan Dadal." },
    { "en": "Apa Itu 'IGBT' (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya Tinggi Saklar Cepat." },
    { "en": "Apa Itu 'MOSFET' (Metal Oxide Semiconductor)?", "id": "Transistor Efek Medan Kendali Tegangan." },
    { "en": "Apa Itu 'GTO' (Gate Turn-Off Thyristor)?", "id": "Thyristor Yang Bisa Dimatikan Gerbang." },
    { "en": "Apa Itu 'Power Diode' (Dioda Daya)?", "id": "Penyearah Arus Besar Frekuensi Rendah." },
    { "en": "Apa Itu 'Schottky Diode' (Dioda Schottky)?", "id": "Dioda Saklar Cepat Tegangan Rendah." },
    { "en": "Apa Itu 'Freewheeling Diode' (Dioda Freewheeling)?", "id": "Pelepas Energi Induktif Saat Mati." },
    { "en": "Apa Itu 'Rectifier' (Penyearah)?", "id": "Pengubah Arus AC Menjadi DC." },
    { "en": "Apa Itu 'Inverter' (Inverter)?", "id": "Pengubah Arus DC Menjadi AC." },
    { "en": "Apa Itu 'Chopper' (Pencacah DC)?", "id": "Pengubah Level Tegangan Arus Searah." },
    { "en": "Apa Itu 'Cycloconverter' (Siklokonverter)?", "id": "Pengubah Frekuensi Arus Bolak Balik." },
    { "en": "Apa Itu 'Controlled Rectifier' (Penyearah Terkendali)?", "id": "Penyearah Menggunakan Sudut Picu Thyristor." },
    { "en": "Apa Itu 'Firing Angle' (Sudut Picu)?", "id": "Sudut Mulai Menghantar Arus Thyristor." },
    { "en": "Apa Itu 'Commutation' (Komutasi)?", "id": "Proses Mematikan Aliran Arus Thyristor." },
    { "en": "Apa Itu 'Natural Commutation' (Komutasi Alami)?", "id": "Pemutusan Arus Melalui Titik Nol." },
    { "en": "Apa Itu 'Forced Commutation' (Komutasi Paksa)?", "id": "Pemutusan Arus Menggunakan Komponen Tambahan." },
    { "en": "Apa Itu 'Snubber Circuit' (Sirkuit Snubber)?", "id": "Peredam Lonjakan Tegangan Transien Saklar." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Pengaturan Daya Melalui Lebar Pulsa." },
    { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Rasio Waktu On Terhadap Periode." },
    { "en": "Apa Itu 'Buck Converter' (Konverter Buck)?", "id": "Rangkaian Penurun Tegangan DC Efisien." },
    { "en": "Apa Itu 'Boost Converter' (Konverter Boost)?", "id": "Rangkaian Penaik Tegangan DC Efisien." },
    { "en": "Apa Itu 'Buck-Boost Converter' (Konverter Buck-Boost)?", "id": "Rangkaian Penaik Penurun Tegangan DC." },
    { "en": "Apa Itu 'SMPS' (Switched-Mode Power Supply)?", "id": "Catu Daya Berbasis Pensakelaran Cepat." },
    { "en": "Apa Itu 'Total Harmonic Distortion' (THD)?", "id": "Persentase Distorsi Gelombang Akibat Harmonisa." },
    { "en": "Apa Itu 'Power Factor' (Faktor Daya)?", "id": "Rasio Daya Nyata Terhadap Semu." },
    { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Pengatur Kecepatan Motor Melalui Frekuensi." },
    { "en": "Apa Itu 'Soft Starter' (Starter Lunak)?", "id": "Alat Pengurang Lonjakan Arus Motor." },
    { "en": "Apa Itu 'Contactor' (Kontaktor)?", "id": "Saklar Magnetik Pengendali Arus Besar." },
    { "en": "Apa Itu 'Overload Relay' (Relay Beban Lebih)?", "id": "Proteksi Panas Berlebih Pada Motor." },
    { "en": "Apa Itu 'DOL' (Direct On Line Starter)?", "id": "Starter Motor Dengan Tegangan Penuh." },
    { "en": "Apa Itu 'Star-Delta' (Starter Bintang Segitiga)?", "id": "Starter Pengurang Arus Awal Motor." },
    { "en": "Apa Itu 'Busbar' (Rel Tembaga)?", "id": "Penghantar Utama Arus Besar Panel." },
    { "en": "Apa Itu 'LVMDP' (Low Voltage Main Distribution Panel)?", "id": "Panel Distribusi Utama Tegangan Rendah." },
    { "en": "Apa Itu 'MCC' (Motor Control Center)?", "id": "Pusat Kendali Motor Listrik Industri." },
    { "en": "Apa Itu 'Capacitor Bank' (Bank Kapasitor)?", "id": "Kumpulan Kapasitor Perbaikan Faktor Daya." },
    { "en": "Apa Itu 'Cable Gland' (Kelen Kabel)?", "id": "Alat Pengunci Kabel Pada Panel." },
    { "en": "Apa Itu 'Cable Tray' (Baki Kabel)?", "id": "Struktur Penyangga Jalur Kabel Industri." },
    { "en": "Apa Itu 'Cable Ladder' (Tangga Kabel)?", "id": "Penyangga Kabel Berat Bentuk Tangga." },
    { "en": "Apa Itu 'Conduit' (Pipa Pelindung)?", "id": "Saluran Pelindung Kabel Instalasi Listrik." },
    { "en": "Apa Itu 'Junction Box' (Kotak Sambung)?", "id": "Tempat Percabangan Dan Penyambungan Kabel." },
    { "en": "Apa Itu 'Terminal Block' (Blok Terminal)?", "id": "Titik Sambungan Kabel Dalam Panel." },
    { "en": "Apa Itu 'Limit Switch' (Saklar Pembatas)?", "id": "Sensor Batas Gerak Mekanik Objek." },
    { "en": "Apa Itu 'Proximity Sensor' (Sensor Jarak)?", "id": "Pendeteksi Objek Tanpa Kontak Fisik." },
    { "en": "Apa Itu 'Push Button' (Tombol Tekan)?", "id": "Alat Kendali Manual Sirkuit Kontrol." },
    { "en": "Apa Itu 'Emergency Stop' (Tombol Darurat)?", "id": "Pemutus Daya Cepat Saat Bahaya." },
    { "en": "Apa Itu 'Pilot Light' (Lampu Indikator)?", "id": "Lampu Penanda Status Operasi Mesin." },
    { "en": "Apa Itu 'Cam Switch' (Saklar Putar)?", "id": "Saklar Pilih Operasi Manual Bertingkat." },
    { "en": "Apa Itu 'MCB' (Miniature Circuit Breaker)?", "id": "Pemutus Arus Beban Lebih Kecil." },
    { "en": "Apa Itu 'MCCB' (Molded Case Circuit Breaker)?", "id": "Pemutus Arus Kapasitas Menengah Industri." },
    { "en": "Apa Itu 'ACB' (Air Circuit Breaker)?", "id": "Pemutus Arus Besar Media Udara." },
    { "en": "Apa Itu 'ELCB' (Earth Leakage Circuit Breaker)?", "id": "Pemutus Arus Deteksi Kebocoran Tanah." },
    { "en": "Apa Itu 'NGR' (Neutral Grounding Resistor)?", "id": "Resistor Pembatas Arus Gangguan Tanah." },
    { "en": "Apa Itu 'Isolator' (Saklar Pemisah)?", "id": "Pemisah Rangkaian Tanpa Beban Listrik." },
    { "en": "Apa Itu 'Current Transformer' (CT)?", "id": "Transformator Pengubah Arus Skala Besar." },
    { "en": "Apa Itu 'Potential Transformer' (PT)?", "id": "Transformator Pengubah Tegangan Skala Besar." },
    { "en": "Apa Itu 'Earth Tester' (Alat Ukur Arde)?", "id": "Alat Ukur Resistansi Pentanahan Bumi." },
    { "en": "Apa Itu 'Insulation Tester' (Megger)?", "id": "Alat Ukur Tahanan Isolasi Kabel." },
    { "en": "Apa Itu 'Clamp Meter' (Tang Ampere)?", "id": "Alat Ukur Arus Tanpa Memutus." },
    { "en": "Apa Itu 'Phase Rotation' (Urutan Fase)?", "id": "Arah Putaran Gelombang Tiga Fase." },
    { "en": "Apa Itu 'Harmonic Filter' (Filter Harmonisa)?", "id": "Penyaring Gangguan Gelombang Listrik Kotor." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Penurunan Tegangan Akibat Hambatan Kabel." },
    { "en": "Apa Itu 'Ampacity' (KHA Kabel)?", "id": "Kapasitas Maksimal Arus Kabel Listrik." },
    { "en": "Apa Itu 'IP Rating' (Ingress Protection)?", "id": "Tingkat Perlindungan Terhadap Debu, Air." },
    { "en": "Apa Itu 'IK Rating' (Impact Protection)?", "id": "Tingkat Perlindungan Terhadap Benturan Mekanik." },
    { "en": "Apa Itu 'Explosion Proof' (Tahan Ledakan)?", "id": "Peralatan Aman Untuk Area Berbahaya." },
    { "en": "Apa Itu 'Intrinsically Safe' (Aman Intrinsik)?", "id": "Sistem Energi Rendah Pencegah Ledakan." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Prosedur Isolasi Energi Berbahaya Aman." },
    { "en": "Apa Itu 'Arc Flash' (Busur Api)?", "id": "Ledakan Cahaya Panas Akibat Arus." },
    { "en": "Apa Itu 'PPE' (Personal Protective Equipment)?", "id": "Alat Pelindung Diri Kerja Listrik." },
    { "en": "Apa Itu 'Earthing' (Pembumian)?", "id": "Hubungan Bagian Logam Ke Bumi." },
    { "en": "Apa Itu 'Grounding' (Pentanahan)?", "id": "Hubungan Titik Netral Ke Bumi." },
    { "en": "Apa Itu 'Lightning Rod' (Penangkal Petir)?", "id": "Batang Logam Penyalur Petir Bumi." },
    { "en": "Apa Itu 'Faraday Cage' (Sangkar Faraday)?", "id": "Ruang Terlindung Dari Medan Listrik." },
    { "en": "Apa Itu 'Interlock' (Saling Kunci)?", "id": "Sistem Pencegah Operasi Salah Peralatan." },
    { "en": "Apa Itu 'Relay' (Relay)?", "id": "Saklar Elektromagnetik Kendali Arus Kecil." },
    { "en": "Apa Itu 'Timer' (Relay Tunda)?", "id": "Relay Dengan Pengaturan Waktu Kerja." },
    { "en": "Apa Itu 'Counter' (Penghitung Digital)?", "id": "Alat Penghitung Jumlah Kejadian Sinyal." },
    { "en": "Apa Itu 'Thermocouple' (Termokopel)?", "id": "Sensor Suhu Berdasarkan Tegangan Logam." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berdasarkan Hambatan Logam." },
    { "en": "Apa Itu 'Pressure Switch' (Saklar Tekanan)?", "id": "Saklar Berdasarkan Batas Tekanan Fluida." },
    { "en": "Apa Itu 'Float Switch' (Saklar Pelampung)?", "id": "Saklar Berdasarkan Ketinggian Level Cairan." },
    { "en": "Apa Itu 'Solenoid' (Solenoida)?", "id": "Kumparan Pembangkit Gerak Mekanik Listrik." },
    { "en": "Apa Itu 'Servo Motor' (Motor Servo)?", "id": "Motor Dengan Kendali Posisi Presisi." },
    { "en": "Apa Itu 'Stepper Motor' (Motor Langkah)?", "id": "Motor Bergerak Dalam Sudut Bertahap." },
    { "en": "Apa Itu 'Synchronous Motor' (Motor Sinkron)?", "id": "Motor Dengan Kecepatan Tetap Sinkron." },
    { "en": "Apa Itu 'Induction Motor' (Motor Induksi)?", "id": "Motor Paling Umum Industri AC." },
    { "en": "Apa Itu 'Slip' (Slip Motor)?", "id": "Selisih Kecepatan Medan Dan Rotor." },
    { "en": "Apa Itu 'Torque' (Torsi)?", "id": "Gaya Putar Yang Dihasilkan Motor." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi)?", "id": "Rasio Daya Keluar Terhadap Masuk." },
    { "en": "Apa Itu 'Phase Failure' (Gagal Fase)?", "id": "Hilangnya Salah Satu Fase Listrik." },
    { "en": "Apa Itu 'Short Circuit' (Hubung Singkat)?", "id": "Aliran Arus Listrik Tanpa Hambatan." },
    { "en": "Apa Itu 'Voltage Swell' (Lonjakan Tegangan)?", "id": "Kenaikan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Voltage Sag' (Kedipan Tegangan)?", "id": "Penurunan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Blackout' (Padam Total)?", "id": "Kegagalan Total Penyaluran Energi Listrik." },
    { "en": "Apa Itu 'Smart Meter' (Meteran Pintar)?", "id": "Alat Ukur Listrik Digital Komunikasi." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Kontroler Logika Industri Terprogram Digital." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control Data Acquisition)?", "id": "Sistem Pengawasan Dan Akuisisi Data." },
    { "en": "Apa Itu 'Uptime' (Waktu Aktif)?", "id": "Durasi Sistem Beroperasi Tanpa Gangguan." },
    { "en": "Apa Itu 'State Space' (Ruang Keadaan)?", "id": "Representasi Matematika Sistem Dinamis Modern." },
    { "en": "Apa Itu 'State Vector' (Vektor Keadaan)?", "id": "Kumpulan Variabel Status Internal Sistem." },
    { "en": "Apa Itu 'Controllability' (Keterkendalian)?", "id": "Kemampuan Mengarahkan Status Sistem Tertentu." },
    { "en": "Apa Itu 'Observability' (Keteramatan)?", "id": "Kemampuan Mengetahui Status Dari Output." },
    { "en": "Apa Itu 'Eigenvalue' (Nilai Eigen)?", "id": "Penentu Karakteristik Respon Dinamis Sistem." },
    { "en": "Apa Itu 'LQR' (Linear Quadratic Regulator)?", "id": "Algoritma Kendali Optimal Berbasis Gain." },
    { "en": "Apa Itu 'LQG' (Linear Quadratic Gaussian)?", "id": "Gabungan LQR Dan Estimator Kalman." },
    { "en": "Apa Itu 'Kalman Filter' (Penyaring Kalman)?", "id": "Algoritma Estimasi Status Dalam Derau." },
    { "en": "Apa Itu 'MPC' (Model Predictive Control)?", "id": "Kendali Berdasarkan Prediksi Masa Depan." },
    { "en": "Apa Itu 'Robust Control' (Kendali Kokoh)?", "id": "Kendali Tahan Terhadap Ketidakpastian Sistem." },
    { "en": "Apa Itu 'Adaptive Control' (Kendali Adaptif)?", "id": "Kendali Dengan Parameter Berubah Otomatis." },
    { "en": "Apa Itu 'Fuzzy Logic' (Logika Kabur)?", "id": "Kendali Berdasarkan Derajat Kebenaran Variabel." },
    { "en": "Apa Itu 'Fuzzification' (Fasifikasi)?", "id": "Pengubahan Nilai Tegas Menjadi Kabur." },
    { "en": "Apa Itu 'Defuzzification' (Defasifikasi)?", "id": "Pengubahan Nilai Kabur Menjadi Tegas." },
    { "en": "Apa Itu 'Neural Network' (Jaringan Saraf)?", "id": "Model Kendali Berbasis Pembelajaran Data." },
    { "en": "Apa Itu 'Deep Learning' (Pembelajaran Mendalam)?", "id": "Jaringan Saraf Dengan Banyak Lapisan." },
    { "en": "Apa Itu 'Reinforcement Learning' (Pembelajaran Penguatan)?", "id": "Belajar Melalui Sistem Hadiah Hukuman." },
    { "en": "Apa Itu 'Genetic Algorithm' (Algoritma Genetik)?", "id": "Optimasi Berdasarkan Teori Evolusi Alami." },
    { "en": "Apa Itu 'Nonlinear Control' (Kendali Tak-Linier)?", "id": "Kendali Untuk Sistem Dinamis Kompleks." },
    { "en": "Apa Itu 'Sliding Mode Control' (SMC)?", "id": "Kendali Kokoh Dengan Permukaan Peralihan." },
    { "en": "Apa Itu 'Lyapunov Stability' (Stabilitas Lyapunov)?", "id": "Analisis Stabilitas Sistem Tanpa Solusi." },
    { "en": "Apa Itu 'H-Infinity Control' (Kendali H-Tak-Hingga)?", "id": "Minimasi Dampak Gangguan Terburuk Sistem." },
    { "en": "Apa Itu 'Digital Control' (Kendali Digital)?", "id": "Sistem Kendali Menggunakan Komputer Digital." },
    { "en": "Apa Itu 'Sampling Period' (Periode Cuplikan)?", "id": "Interval Waktu Antar Dua Data." },
    { "en": "Apa Itu 'Z-Transform' (Transformasi Z)?", "id": "Analisis Sistem Linier Waktu Diskrit." },
    { "en": "Apa Itu 'Discretization' (Diskretisasi)?", "id": "Pengubahan Model Kontinu Menjadi Diskrit." },
    { "en": "Apa Itu 'Aliasing' (Aliasing Sinyal)?", "id": "Distorsi Akibat Sampling Terlalu Lambat." },
    { "en": "Apa Itu 'Zero-Order Hold' (ZOH)?", "id": "Penahan Nilai Sinyal Antar Cuplikan." },
    { "en": "Apa Itu 'Deadbeat Control' (Kendali Deadbeat)?", "id": "Respon Cepat Dalam Langkah Terbatas." },
    { "en": "Apa Itu 'System Identification' (Identifikasi Sistem)?", "id": "Pembuatan Model Berdasarkan Data Eksperimen." },
    { "en": "Apa Itu 'Transfer Function' (Fungsi Transfer)?", "id": "Rasio Output Input Domain Laplace." },
    { "en": "Apa Itu 'Pole' (Kutub Sistem)?", "id": "Nilai Penyebab Fungsi Transfer Tak-Hingga." },
    { "en": "Apa Itu 'Zero' (Nol Sistem)?", "id": "Nilai Penyebab Fungsi Transfer Menjadi." },
    { "en": "Apa Itu 'Stability Margin' (Margin Stabilitas)?", "id": "Jarak Aman Sistem Dari Ketidakstabilan." },
    { "en": "Apa Itu 'Phase Margin' (Margin Fase)?", "id": "Batas Perubahan Fase Sebelum Goyah." },
    { "en": "Apa Itu 'Gain Margin' (Margin Penguatan)?", "id": "Batas Penambahan Penguatan Sebelum Goyah." },
    { "en": "Apa Itu 'SISO' (Single Input Single Output)?", "id": "Sistem Dengan Satu Masukan Keluaran." },
    { "en": "Apa Itu 'MIMO' (Multiple Input Multiple Output)?", "id": "Sistem Dengan Banyak Masukan Keluaran." },
    { "en": "Apa Itu 'Causality' (Kausalitas)?", "id": "Output Hanya Tergantung Input Sekarang." },
    { "en": "Apa Itu 'Time-Invariant' (Tak-Ubah Waktu)?", "id": "Karakteristik Sistem Tetap Seiring Waktu." },
    { "en": "Apa Itu 'Linearity' (Linieritas)?", "id": "Sistem Memenuhi Prinsip Superposisi Homogenitas." },
    { "en": "Apa Itu 'Hysteresis' (Histeresis Kendali)?", "id": "Keterlambatan Respon Terhadap Perubahan Input." },
    { "en": "Apa Itu 'Backlash' (Spasi Mekanik)?", "id": "Celah Kosong Antar Roda Gigi." },
    { "en": "Apa Itu 'Dead Zone' (Zona Mati)?", "id": "Rentang Input Tanpa Perubahan Output." },
    { "en": "Apa Itu 'Saturation' (Saturasi)?", "id": "Batas Maksimal Kemampuan Output Perangkat." },
    { "en": "Apa Itu 'Anti-Windup' (Anti-Windup)?", "id": "Pencegah Akumulasi Error Pada Integrator." },
    { "en": "Apa Itu 'Feedforward Control' (Kendali Umpan-Maju)?", "id": "Aksi Kendali Berdasarkan Gangguan Terukur." },
    { "en": "Apa Itu 'Bumpless Transfer' (Perpindahan Mulus)?", "id": "Transisi Manual Ke Otomatis Tanpa." },
    { "en": "Apa Itu 'Distributed Control' (Kendali Terdistribusi)?", "id": "Sistem Kendali Dengan Banyak Kontroler." },
    { "en": "Apa Itu 'Centralized Control' (Kendali Terpusat)?", "id": "Seluruh Kendali Dilakukan Satu Unit." },
    { "en": "Apa Itu 'Embedded Control' (Kendali Tertanam)?", "id": "Sistem Kendali Di Dalam Mikrokontroler." },
    { "en": "Apa Itu 'IoT Control' (Kendali IoT)?", "id": "Kendali Perangkat Melalui Jaringan Internet." },
    { "en": "Apa Itu 'Edge Intelligence' (Kecerdasan Tepi)?", "id": "Pemrosesan Data Cerdas Dekat Sensor." },
    { "en": "Apa Itu 'Cloud Automation' (Otomasi Awan)?", "id": "Kendali Skala Besar Melalui Server." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Simulasi Virtual Real-Time Objek Fisik." },
    { "en": "Apa Itu 'Cyber-Physical System' (CPS)?", "id": "Integrasi Komputasi Jaringan Proses Fisik." },
    { "en": "Apa Itu 'Mechatronics' (Mekatronika)?", "id": "Sinergi Mekanik Elektronika Dan Komputer." },
    { "en": "Apa Itu 'Robotics' (Robotika)?", "id": "Desain Konstruksi Operasi Robot Cerdas." },
    { "en": "Apa Itu 'DOF' (Degrees Of Freedom)?", "id": "Jumlah Arah Gerakan Mandiri Robot." },
    { "en": "Apa Itu 'Kinematics' (Kinematika)?", "id": "Studi Gerak Tanpa Memperhatikan Gaya." },
    { "en": "Apa Itu 'Dynamics' (Dinamika)?", "id": "Studi Gerak Dengan Memperhatikan Gaya." },
    { "en": "Apa Itu 'Trajectory Planning' (Perencanaan Lintasan)?", "id": "Penentuan Rute Gerak Robot Halus." },
    { "en": "Apa Itu 'Jacobian Matrix' (Matriks Jacobian)?", "id": "Hubungan Kecepatan Sendi Dan Ujung." },
    { "en": "Apa Itu 'Singularity' (Singularitas Robot)?", "id": "Kondisi Robot Kehilangan Derajat Kebebasan." },
    { "en": "Apa Itu 'Force Control' (Kendali Gaya)?", "id": "Kendali Interaksi Fisik Robot Lingkungan." },
    { "en": "Apa Itu 'Vision Control' (Kendali Visual)?", "id": "Kendali Berdasarkan Informasi Kamera Sensor." },
    { "en": "Apa Itu 'Haptic Feedback' (Umpan Balik Haptik)?", "id": "Indera Sentuhan Buatan Pada Kendali." },
    { "en": "Apa Itu 'Teleoperation' (Teleoperasi)?", "id": "Kendali Perangkat Dari Jarak Jauh." },
    { "en": "Apa Itu 'Swarm Intelligence' (Kecerdasan Kawanan)?", "id": "Perilaku Kolektif Sistem Terdesentralisasi Mandiri." },
    { "en": "Apa Itu 'Autonomous System' (Sistem Otonom)?", "id": "Sistem Beroperasi Tanpa Intervensi Manusia." },
    { "en": "Apa Itu 'Soft Robotics' (Robotika Lunak)?", "id": "Robot Dari Bahan Fleksibel Kenyal." },
    { "en": "Apa Itu 'Exoskeleton' (Eksoskeleton)?", "id": "Rangka Luar Pembantu Kekuatan Manusia." },
    { "en": "Apa Itu 'Humanoid' (Robot Humanoid)?", "id": "Robot Dengan Bentuk Tubuh Manusia." },
    { "en": "Apa Itu 'Cobot' (Collaborative Robot)?", "id": "Robot Bekerja Bersama Manusia Aman." },
    { "en": "Apa Itu 'Machine Learning' (Pembelajaran Mesin)?", "id": "Algoritma Belajar Dari Pengalaman Data." },
    { "en": "Apa Itu 'Feature Extraction' (Ekstraksi Fitur)?", "id": "Pemilihan Informasi Penting Dari Data." },
    { "en": "Apa Itu 'Supervised Learning' (Pembelajaran Terawasi)?", "id": "Belajar Dari Data Berlabel Jelas." },
    { "en": "Apa Itu 'Unsupervised Learning' (Tanpa Pengawasan)?", "id": "Mencari Pola Dalam Data Mentah." },
    { "en": "Apa Itu 'Regression' (Regresi)?", "id": "Prediksi Nilai Kontinu Berdasarkan Data." },
    { "en": "Apa Itu 'Classification' (Klasifikasi)?", "id": "Pengelompokan Data Ke Dalam Kategori." },
    { "en": "Apa Itu 'Overfitting' (Pengepasan Berlebih)?", "id": "Model Terlalu Fokus Data Latih." },
    { "en": "Apa Itu 'Underfitting' (Kurang Pengepasan)?", "id": "Model Gagal Menangkap Pola Data." },
    { "en": "Apa Itu 'Data Mining' (Penggalian Data)?", "id": "Proses Menemukan Informasi Dalam Database." },
    { "en": "Apa Itu 'Big Data' (Data Besar)?", "id": "Analisis Kumpulan Data Volume Sangat." },
    { "en": "Apa Itu 'Predictive Analytics' (Analitik Prediktif)?", "id": "Ramalan Kejadian Berdasarkan Data Historis." },
    { "en": "Apa Itu 'Anomaly Detection' (Deteksi Anomali)?", "id": "Pencarian Pola Tidak Lazim Data." },
    { "en": "Apa Itu 'Fault Diagnosis' (Diagnosis Gangguan)?", "id": "Identifikasi Lokasi Penyebab Kerusakan Mesin." },
    { "en": "Apa Itu 'Condition Monitoring' (Pemantauan Kondisi)?", "id": "Pengawasan Kesehatan Mesin Secara Kontinu." },
    { "en": "Apa Itu 'OEE' (Overall Equipment Effectiveness)?", "id": "Ukuran Efisiensi Total Peralatan Industri." },
    { "en": "Apa Itu 'Smart Factory' (Pabrik Cerdas)?", "id": "Pabrik Terkoneksi Dengan Otomasi Tinggi." },
    { "en": "Apa Itu 'Green Automation' (Otomasi Hijau)?", "id": "Otomasi Demi Efisiensi Energi Berkelanjutan." },
    { "en": "Apa Itu 'Standard ISO 50001' (Manajemen Energi)?", "id": "Standar Internasional Sistem Manajemen Energi." },
    { "en": "Apa Itu 'Energy Audit' (Audit Energi)?", "id": "Evaluasi Pola Konsumsi Energi Bangunan." },
    { "en": "Apa Itu 'Demand Side Management' (DSM)?", "id": "Optimasi Konsumsi Listrik Sisi Konsumen." },
    { "en": "Apa Itu 'Smart Metering' (Meteran Pintar)?", "id": "Alat Ukur Listrik Komunikasi Digital." },
    { "en": "Apa Itu 'Renewable Integration' (Integrasi Terbarukan)?", "id": "Penggabungan Energi Bersih Ke Jaringan." },
    { "en": "Apa Itu 'Microgrid Control' (Kendali Mikrogrid)?", "id": "Pengatur Keseimbangan Daya Lokal Mandiri." },
    { "en": "Apa Itu 'Frequency Regulation' (Regulasi Frekuensi)?", "id": "Menjaga Frekuensi Jaringan Tetap Stabil." },
    { "en": "Apa Itu 'Voltage Regulation' (Regulasi Tegangan)?", "id": "Menjaga Tegangan Jaringan Tetap Stabil." },
    { "en": "Apa Itu 'Project Management' (Manajemen Proyek)?", "id": "Aplikasi Ilmu Mencapai Tujuan Proyek." },
    { "en": "Apa Itu 'Waterfall' (Metodologi Air Terjun)?", "id": "Pengembangan Proyek Urutan Linier Kaku." },
    { "en": "Apa Itu 'Agile' (Metodologi Agile)?", "id": "Pengembangan Proyek Iteratif Dan Fleksibel." },
    { "en": "Apa Itu 'Scrum' (Kerangka Kerja Scrum)?", "id": "Metode Kelola Proyek Dengan Sprint." },
    { "en": "Apa Itu 'Sprint' (Iterasi Sprint)?", "id": "Periode Waktu Singkat Selesaikan Tugas." },
    { "en": "Apa Itu 'Kanban' (Metode Kanban)?", "id": "Visualisasi Alur Kerja Menggunakan Papan." },
    { "en": "Apa Itu 'PMBOK' (Project Management Body Of Knowledge)?", "id": "Standar Global Panduan Manajemen Proyek." },
    { "en": "Apa Itu 'PMP' (Project Management Professional)?", "id": "Sertifikasi Internasional Manajer Proyek Profesional." },
    { "en": "Apa Itu 'Stakeholder' (Pemangku Kepentingan)?", "id": "Pihak Yang Mempengaruhi Hasil Proyek." },
    { "en": "Apa Itu 'Scope Creep' (Penyimpangan Lingkup)?", "id": "Perubahan Proyek Tanpa Kontrol Formal." },
    { "en": "Apa Itu 'WBS' (Work Breakdown Structure)?", "id": "Pembagian Tugas Proyek Menjadi Kecil." },
    { "en": "Apa Itu 'Gantt Chart' (Bagan Gantt)?", "id": "Visualisasi Jadwal Proyek Terhadap Waktu." },
    { "en": "Apa Itu 'CPM' (Critical Path Method)?", "id": "Metode Jalur Terpanjang Durasi Proyek." },
    { "en": "Apa Itu 'PERT' (Program Evaluation Review Technique)?", "id": "Analisis Statistik Estimasi Waktu Proyek." },
    { "en": "Apa Itu 'Milestone' (Pencapaian Penting)?", "id": "Titik Penanda Kemajuan Signifikan Proyek." },
    { "en": "Apa Itu 'Kick-off Meeting' (Rapat Perdana)?", "id": "Pertemuan Awal Peresmian Mulai Proyek." },
    { "en": "Apa Itu 'KPI' (Key Performance Indicator)?", "id": "Ukuran Keberhasilan Target Kinerja Proyek." },
    { "en": "Apa Itu 'SLA' (Service Level Agreement)?", "id": "Perjanjian Tingkat Layanan Antar Pihak." },
    { "en": "Apa Itu 'Risk Management' (Manajemen Risiko)?", "id": "Identifikasi Dan Mitigasi Ketidakpastian Proyek." },
    { "en": "Apa Itu 'Risk Mitigation' (Mitigasi Risiko)?", "id": "Tindakan Mengurangi Dampak Buruk Risiko." },
    { "en": "Apa Itu 'Contingency Plan' (Rencana Kontinjensi)?", "id": "Rencana Cadangan Menghadapi Kejadian Darurat." },
    { "en": "Apa Itu 'SWOT' (Strengths Weaknesses Opportunities Threats)?", "id": "Analisis Internal Dan Eksternal Proyek." },
    { "en": "Apa Itu 'Feasibility Study' (Studi Kelayakan)?", "id": "Analisis Menilai Kelayakan Teknis Ekonomi." },
    { "en": "Apa Itu 'CAPEX' (Capital Expenditure)?", "id": "Biaya Modal Pengadaan Aset Tetap." },
    { "en": "Apa Itu 'OPEX' (Operating Expenditure)?", "id": "Biaya Operasional Rutin Harian Proyek." },
    { "en": "Apa Itu 'ROI' (Return On Investment)?", "id": "Rasio Pengembalian Atas Modal Investasi." },
    { "en": "Apa Itu 'TCO' (Total Cost Of Ownership)?", "id": "Total Biaya Kepemilikan Aset Selamanya." },
    { "en": "Apa Itu 'Procurement' (Pengadaan Barang)?", "id": "Proses Pembelian Sumber Daya Proyek." },
    { "en": "Apa Itu 'RFP' (Request For Proposal)?", "id": "Undangan Penawaran Solusi Kepada Vendor." },
    { "en": "Apa Itu 'RFQ' (Request For Quotation)?", "id": "Permintaan Harga Spesifik Barang Jasa." },
    { "en": "Apa Itu 'Change Request' (Permintaan Perubahan)?", "id": "Usulan Formal Modifikasi Lingkup Proyek." },
    { "en": "Apa Itu 'Quality Assurance' (Penjaminan Kualitas)?", "id": "Prosedur Menjaga Standar Hasil Proyek." },
    { "en": "Apa Itu 'Quality Control' (Pengendalian Kualitas)?", "id": "Pemeriksaan Akhir Kesesuaian Hasil Produk." },
    { "en": "Apa Itu 'Six Sigma' (Metodologi Six Sigma)?", "id": "Metode Pengurangan Kecacatan Proses Industri." },
    { "en": "Apa Itu 'Lean' (Manajemen Lean)?", "id": "Efisiensi Melalui Penghapusan Pemborosan Proses." },
    { "en": "Apa Itu 'Kaizen' (Perbaikan Berkelanjutan)?", "id": "Filosofi Perbaikan Kecil Terus Menerus." },
    { "en": "Apa Itu 'Backlog' (Daftar Tugas)?", "id": "Daftar Pekerjaan Yang Belum Diselesaikan." },
    { "en": "Apa Itu 'User Story' (Cerita Pengguna)?", "id": "Deskripsi Fitur Dari Perspektif Pengguna." },
    { "en": "Apa Itu 'Velocity' (Kecepatan Tim)?", "id": "Ukuran Produktivitas Tim Dalam Sprint." },
    { "en": "Apa Itu 'Burn-down Chart' (Grafik Burn-down)?", "id": "Visualisasi Sisa Pekerjaan Terhadap Waktu." },
    { "en": "Apa Itu 'Resource Levelling' (Pemerataan Sumber Daya)?", "id": "Optimasi Penjadwalan Hindari Beban Berlebih." },
    { "en": "Apa Itu 'Deliverable' (Hasil Serahan)?", "id": "Produk Nyata Yang Dihasilkan Proyek." },
    { "en": "Apa Itu 'Project Charter' (Piagam Proyek)?", "id": "Dokumen Pengesahan Resmi Keberadaan Proyek." },
    { "en": "Apa Itu 'Crashing' (Pemercepatan Proyek)?", "id": "Penambahan Biaya Persingkat Durasi Proyek." },
    { "en": "Apa Itu 'Fast Tracking' (Jalur Cepat)?", "id": "Eksekusi Tugas Paralel Persingkat Waktu." },
    { "en": "Apa Itu 'Sunk Cost' (Biaya Hangus)?", "id": "Biaya Masa Lalu Tidak Kembali." },
    { "en": "Apa Itu 'Value Engineering' (Rekayasa Nilai)?", "id": "Optimasi Fungsi Produk Dengan Biaya." },
    { "en": "Apa Itu 'Product Lifecycle' (Siklus Hidup Produk)?", "id": "Tahapan Produk Dari Ide Hingga." },
    { "en": "Apa Itu 'BIM' (Building Information Modeling)?", "id": "Pemodelan Digital Terpadu Proyek Konstruksi." },
    { "en": "Apa Itu 'Persetujuan Insinyur' (Engineer's Approval)?", "id": "Legalitas Teknis Oleh Tenaga Ahli." },
    { "en": "Apa Itu 'Etika Profesi' (Professional Ethics)?", "id": "Aturan Perilaku Benar Dalam Pekerjaan." },
    { "en": "Apa Itu 'Integritas' (Integrity Insinyur)?", "id": "Kejujuran Dan Konsistensi Prinsip Moral." },
    { "en": "Apa Itu 'Conflict Of Interest' (Konflik Kepentingan)?", "id": "Situasi Kepentingan Pribadi Rugikan Tugas." },
    { "en": "Apa Itu 'Whistleblowing' (Pelaporan Pelanggaran)?", "id": "Pengungkapan Praktik Salah Dalam Organisasi." },
    { "en": "Apa Itu 'Confidentiality' (Kerahasiaan Data)?", "id": "Kewajiban Menjaga Informasi Rahasia Klien." },
    { "en": "Apa Itu 'Public Safety' (Keselamatan Publik)?", "id": "Prioritas Utama Insinyur Melindungi Masyarakat." },
    { "en": "Apa Itu 'Competence' (Kompetensi Profesional)?", "id": "Bekerja Sesuai Bidang Keahlian Miliknya." },
    { "en": "Apa Itu 'Continuing Development' (CPD)?", "id": "Pengembangan Pengetahuan Profesional Secara Berkelanjutan." },
    { "en": "Apa Itu 'Duty Of Care' (Kewajiban Menjaga)?", "id": "Tanggung Jawab Moral Hindari Kerusakan." },
    { "en": "Apa Itu 'Professional Liability' (Tanggung Jawab Hukum)?", "id": "Konsekuensi Hukum Atas Kelalaian Kerja." },
    { "en": "Apa Itu 'Sustainability' (Keberlanjutan Lingkungan)?", "id": "Desain Mempertimbangkan Dampak Masa Depan." },
    { "en": "Apa Itu 'Intellectual Property' (IP)?", "id": "Hak Kekayaan Atas Karya Intelektual." },
    { "en": "Apa Itu 'Patent' (Paten Invensi)?", "id": "Hak Eksklusif Atas Penemuan Teknologi." },
    { "en": "Apa Itu 'Copyright' (Hak Cipta)?", "id": "Perlindungan Karya Tulis Dan Seni." },
    { "en": "Apa Itu 'Trade Secret' (Rahasia Dagang)?", "id": "Informasi Bisnis Rahasia Bernilai Ekonomis." },
    { "en": "Apa Itu 'Code Of Ethics' (Kode Etik)?", "id": "Dokumen Standar Perilaku Anggota Profesi." },
    { "en": "Apa Itu 'IEEE' (Institute Of Electrical Electronics Engineers)?", "id": "Organisasi Profesi Teknik Elektro Global." },
    { "en": "Apa Itu 'ANSI' (American National Standards Institute)?", "id": "Lembaga Standar Nasional Amerika Serikat." },
    { "en": "Apa Itu 'ISO' (International Organization For Standardization)?", "id": "Organisasi Standar Internasional Seluruh Bidang." },
    { "en": "Apa Itu 'NEMA' (National Electrical Manufacturers Association)?", "id": "Standar Peralatan Listrik Amerika Utara." },
    { "en": "Apa Itu 'SNI' (Standar Nasional Indonesia)?", "id": "Standar Teknis Berlaku Di Indonesia." },
    { "en": "Apa Itu 'Occupational Health' (Kesehatan Kerja)?", "id": "Upaya Menjaga Kesehatan Fisik Pekerja." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Prosedur Keamanan Isolasi Energi Berbahaya." },
    { "en": "Apa Itu 'Work Permit' (Izin Kerja)?", "id": "Dokumen Persetujuan Melakukan Pekerjaan Berbahaya." },
    { "en": "Apa Itu 'Root Cause Analysis' (RCA)?", "id": "Metode Pencarian Akar Penyebab Masalah." },
    { "en": "Apa Itu 'FMEA' (Failure Mode Effects Analysis)?", "id": "Analisis Potensi Kegagalan Suatu Desain." },
    { "en": "Apa Itu 'Standard Operating Procedure' (SOP)?", "id": "Langkah Kerja Baku Selesaikan Tugas." },
    { "en": "Apa Itu 'Peer Review' (Tinjauan Sejawat)?", "id": "Evaluasi Karya Oleh Rekan Profesi." },
    { "en": "Apa Itu 'Technical Audit' (Audit Teknis)?", "id": "Pemeriksaan Kepatuhan Terhadap Standar Teknik." },
    { "en": "Apa Itu 'Compliance' (Kepatuhan Regulasi)?", "id": "Ketaatan Terhadap Hukum Dan Aturan." },
    { "en": "Apa Itu 'Corporate Responsibility' (CSR)?", "id": "Tanggung Jawab Perusahaan Terhadap Masyarakat." },
    { "en": "Apa Itu 'Bribery' (Penyuapan)?", "id": "Pemberian Sesuatu Untuk Mempengaruhi Keputusan." },
    { "en": "Apa Itu 'Facilitation Payment' (Pembayaran Fasilitasi)?", "id": "Uang Pelicin Percepat Urusan Birokrasi." },
    { "en": "Apa Itu 'Quality Policy' (Kebijakan Kualitas)?", "id": "Pernyataan Komitmen Organisasi Terhadap Mutu." },
    { "en": "Apa Itu 'Documentation' (Dokumentasi Teknik)?", "id": "Rekaman Data Proses Dan Desain." },
    { "en": "Apa Itu 'Blueprinting' (Pembuatan Cetak Biru)?", "id": "Dokumen Rencana Detail Teknis Proyek." },
    { "en": "Apa Itu 'As-Built Drawing' (Gambar Terpasang)?", "id": "Gambar Akhir Sesuai Hasil Lapangan." },
    { "en": "Apa Itu 'O&M Manual' (Operations Maintenance)?", "id": "Panduan Operasi Dan Perawatan Alat." },
    { "en": "Apa Itu 'Vendor Management' (Manajemen Vendor)?", "id": "Pengelolaan Hubungan Dengan Pemasok Barang." },
    { "en": "Apa Itu 'Kickback' (Uang Komisi)?", "id": "Penerimaan Ilegal Atas Penggunaan Vendor." },
    { "en": "Apa Itu 'Green Engineering' (Teknik Hijau)?", "id": "Desain Ramah Lingkungan Hemat Energi." },
    { "en": "Apa Itu 'Safety Culture' (Budaya Keselamatan)?", "id": "Kesadaran Kolektif Pentingnya Keamanan Kerja." },
    { "en": "Apa Itu 'Mentoring' (Mentoring Teknik)?", "id": "Bimbingan Insinyur Senior Kepada Junior." },
    { "en": "Apa Itu 'Professionalism' (Profesionalisme)?", "id": "Sikap Dan Kualitas Kerja Ahli." },
    { "en": "Apa Itu 'Accountability' (Akuntabilitas)?", "id": "Kewajiban Menanggung Hasil Keputusan Sendiri." },
    { "en": "Apa Itu 'Standardization' (Standardisasi)?", "id": "Penyeragaman Teknis Demi Interoperabilitas Produk." },
    { "en": "Apa Itu 'Validation' (Validasi)?", "id": "Pembuktian Produk Memenuhi Kebutuhan Pengguna." },
    { "en": "Apa Itu 'Verification' (Verifikasi)?", "id": "Pembuktian Produk Memenuhi Spesifikasi Teknis." },
    { "en": "Apa Itu 'Optimization' (Optimasi Proyek)?", "id": "Pencarian Keseimbangan Biaya Waktu Kualitas." },
    { "en": "Apa Itu 'DC Motor' (Motor Arus Searah)?", "id": "Mesin Pengubah Listrik Menjadi Gerak." },
    { "en": "Apa Itu 'Stator' (Bagian Diam Motor)?", "id": "Komponen Magnetik Diam Dalam Motor." },
    { "en": "Apa Itu 'Rotor' (Bagian Berputar Motor)?", "id": "Komponen Inti Motor Yang Berputar." },
    { "en": "Apa Itu 'Commutator' (Komutator Motor DC)?", "id": "Pembalik Arus Pada Motor Searah." },
    { "en": "Apa Itu 'Brushes' (Sikat Karbon)?", "id": "Penghubung Listrik Bagian Berputar Motor." },
    { "en": "Apa Itu 'Armature' (Angker Motor)?", "id": "Tempat Terjadinya Gaya Elektromagnetik Motor." },
    { "en": "Apa Itu 'Back EMF' (Gaya Gerak Listrik Balik)?", "id": "Tegangan Melawan Arus Masuk Motor." },
    { "en": "Apa Itu 'Shunt Motor' (Motor Shunt)?", "id": "Motor Dengan Lilitan Medan Paralel." },
    { "en": "Apa Itu 'Series Motor' (Motor Seri)?", "id": "Motor Dengan Lilitan Medan Seri." },
    { "en": "Apa Itu 'Compound Motor' (Motor Kompon)?", "id": "Motor Gabungan Seri Dan Paralel." },
    { "en": "Apa Itu 'Torque' (Torsi Mekanik)?", "id": "Gaya Putar Pada Poros Motor." },
    { "en": "Apa Itu 'Starting Torque' (Torsi Awal)?", "id": "Gaya Putar Saat Motor Mulai." },
    { "en": "Apa Itu 'Rated Speed' (Kecepatan Rating)?", "id": "Kecepatan Putar Aman Kondisi Normal." },
    { "en": "Apa Itu 'Induction Motor' (Motor Induksi)?", "id": "Motor AC Berdasarkan Induksi Elektromagnetik." },
    { "en": "Apa Itu 'Squirrel Cage' (Sangkar Tupai)?", "id": "Jenis Rotor Motor Induksi Sederhana." },
    { "en": "Apa Itu 'Slip' (Slip Motor Induksi)?", "id": "Selisih Kecepatan Medan Dan Rotor." },
    { "en": "Apa Itu 'Synchronous Speed' (Kecepatan Sinkron)?", "id": "Kecepatan Putar Medan Magnet Stator." },
    { "en": "Apa Itu 'Synchronous Motor' (Motor Sinkron)?", "id": "Motor Dengan Kecepatan Putar Tetap." },
    { "en": "Apa Itu 'Excitation' (Eksitasi Medan)?", "id": "Pemberian Arus Penguat Medan Magnet." },
    { "en": "Apa Itu 'Permanent Magnet Motor' (Motor Magnet Permanen)?", "id": "Motor Menggunakan Magnet Tetap Alami." },
    { "en": "Apa Itu 'BLDC' (Brushless DC Motor)?", "id": "Motor Searah Tanpa Sikat Karbon." },
    { "en": "Apa Itu 'Universal Motor' (Motor Universal)?", "id": "Motor Untuk Arus Searah Bolak-Balik." },
    { "en": "Apa Itu 'Reluctance Motor' (Motor Reluktansi)?", "id": "Motor Berdasarkan Prinsip Hambatan Magnetik." },
    { "en": "Apa Itu 'Hysteresis Motor' (Motor Histeresis)?", "id": "Motor Berdasarkan Efek Lag Magnetik." },
    { "en": "Apa Itu 'Generator' (Generator Listrik)?", "id": "Mesin Pengubah Gerak Menjadi Listrik." },
    { "en": "Apa Itu 'Alternator' (Alternator)?", "id": "Generator Penghasil Arus Bolak Balik." },
    { "en": "Apa Itu 'Prime Mover' (Penggerak Mula)?", "id": "Sumber Energi Pemutar Poros Generator." },
    { "en": "Apa Itu 'Voltage Regulation' (Regulasi Tegangan)?", "id": "Kemampuan Menjaga Tegangan Tetap Stabil." },
    { "en": "Apa Itu 'Magnetic Flux' (Fluks Magnetik)?", "id": "Jumlah Garis Gaya Medan Magnet." },
    { "en": "Apa Itu 'Magnetic Field' (Medan Magnet)?", "id": "Ruang Disekitar Magnet Berpengaruh Gaya." },
    { "en": "Apa Itu 'Permeability' (Permeabilitas)?", "id": "Kemampuan Bahan Menghantarkan Fluks Magnetik." },
    { "en": "Apa Itu 'Reluctance' (Reluktansi Magnetik)?", "id": "Hambatan Terhadap Aliran Fluks Magnetik." },
    { "en": "Apa Itu 'Hysteresis Loop' (Kurva Histeresis)?", "id": "Grafik Hubungan Intensitas Dan Induksi." },
    { "en": "Apa Itu 'Remanence' (Remanensi Magnetik)?", "id": "Sisa Magnetisme Setelah Medan Dihilangkan." },
    { "en": "Apa Itu 'Coercivity' (Koersivitas)?", "id": "Kekuatan Medan Penghilang Magnetisme Sisa." },
    { "en": "Apa Itu 'Eddy Current' (Arus Eddy)?", "id": "Arus Pusar Penyebab Panas Inti." },
    { "en": "Apa Itu 'Lamination' (Laminasi Inti)?", "id": "Lapisan Tipis Pengurang Arus Pusar." },
    { "en": "Apa Itu 'Ferromagnetic' (Feromagnetik)?", "id": "Bahan Yang Sangat Kuat Ditarik Magnet." },
    { "en": "Apa Itu 'Paramagnetic' (Paramagnetik)?", "id": "Bahan Yang Lemah Ditarik Magnet." },
    { "en": "Apa Itu 'Diamagnetic' (Diamagnetik)?", "id": "Bahan Yang Menolak Medan Magnet." },
    { "en": "Apa Itu 'Curie Temperature' (Suhu Curie)?", "id": "Suhu Hilangnya Sifat Magnetik Bahan." },
    { "en": "Apa Itu 'Soft Magnetic' (Magnet Lunak)?", "id": "Bahan Mudah Dimagnetkan Dan Dihilangkan." },
    { "en": "Apa Itu 'Hard Magnetic' (Magnet Keras)?", "id": "Bahan Sulit Dihilangkan Sifat Magnetnya." },
    { "en": "Apa Itu 'Semiconductor' (Semikonduktor)?", "id": "Bahan Antara Konduktor Dan Isolator." },
    { "en": "Apa Itu 'Intrinsic Semiconductor' (Semikonduktor Intrinsik)?", "id": "Bahan Semikonduktor Murni Tanpa Campuran." },
    { "en": "Apa Itu 'Extrinsic Semiconductor' (Semikonduktor Ekstrinsik)?", "id": "Bahan Semikonduktor Dengan Campuran Tertentu." },
    { "en": "Apa Itu 'Doping' (Doping Semikonduktor)?", "id": "Proses Penambahan Ketidakmurnian Ke Semikonduktor." },
    { "en": "Apa Itu 'N-Type' (Tipe-N)?", "id": "Semikonduktor Dengan Kelebihan Elektron Negatif." },
    { "en": "Apa Itu 'P-Type' (Tipe-P)?", "id": "Semikonduktor Dengan Kelebihan Lubang Positif." },
    { "en": "Apa Itu 'Hole' (Lubang Elektron)?", "id": "Kekosongan Elektron Bertindak Muatan Positif." },
    { "en": "Apa Itu 'PN Junction' (Sambungan PN)?", "id": "Titik Temu Bahan Tipe P, N." },
    { "en": "Apa Itu 'Depletion Region' (Daerah Kosong)?", "id": "Area Tanpa Muatan Bebas Sambungan." },
    { "en": "Apa Itu 'Forward Bias' (Prasikap Maju)?", "id": "Pemberian Tegangan Agar Arus Mengalir." },
    { "en": "Apa Itu 'Reverse Bias' (Prasikap Balik)?", "id": "Pemberian Tegangan Agar Arus Terhambat." },
    { "en": "Apa Itu 'Breakdown Voltage' (Tegangan Dadal)?", "id": "Batas Tegangan Penyebab Arus Bocor." },
    { "en": "Apa Itu 'Band Gap' (Celah Pita)?", "id": "Energi Pemisah Pita Valensi Konduksi." },
    { "en": "Apa Itu 'Valence Band' (Pita Valensi)?", "id": "Pita Energi Terisi Elektron Terikat." },
    { "en": "Apa Itu 'Conduction Band' (Pita Konduksi)?", "id": "Pita Energi Tempat Elektron Bebas." },
    { "en": "Apa Itu 'Fermi Level' (Tingkat Fermi)?", "id": "Tingkat Energi Probabilitas Elektron Setengah." },
    { "en": "Apa Itu 'Resistivity' (Resistivitas)?", "id": "Kemampuan Bahan Menahan Arus Listrik." },
    { "en": "Apa Itu 'Conductivity' (Konduktivitas)?", "id": "Kemampuan Bahan Menghantarkan Arus Listrik." },
    { "en": "Apa Itu 'Superconductor' (Superkonduktor)?", "id": "Bahan Tanpa Hambatan Suhu Rendah." },
    { "en": "Apa Itu 'Meissner Effect' (Efek Meissner)?", "id": "Pengusiran Medan Magnet Oleh Superkonduktor." },
    { "en": "Apa Itu 'Critical Temperature' (Suhu Kritis)?", "id": "Suhu Bahan Menjadi Superkonduktor Murni." },
    { "en": "Apa Itu 'Dielectric' (Dielektrik)?", "id": "Bahan Isolasi Penyimpan Medan Listrik." },
    { "en": "Apa Itu 'Permittivity' (Permitivitas)?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
    { "en": "Apa Itu 'Dielectric Strength' (Kekuatan Dielektrik)?", "id": "Tegangan Maksimal Sebelum Isolasi Tembus." },
    { "en": "Apa Itu 'Polarization' (Polarisasi Dielektrik)?", "id": "Pemisahan Muatan Dalam Bahan Listrik." },
    { "en": "Apa Itu 'Piezoelectric' (Piezoelektrik)?", "id": "Pembangkit Listrik Akibat Tekanan Mekanik." },
    { "en": "Apa Itu 'Thermoelectric' (Termoelektrik)?", "id": "Pembangkit Listrik Akibat Perbedaan Suhu." },
    { "en": "Apa Itu 'Seebeck Effect' (Efek Seebeck)?", "id": "Munculnya Tegangan Akibat Perbedaan Panas." },
    { "en": "Apa Itu 'Peltier Effect' (Efek Peltier)?", "id": "Perpindahan Panas Akibat Arus Listrik." },
    { "en": "Apa Itu 'Photoconductivity' (Fotokonduktivitas)?", "id": "Peningkatan Konduktivitas Akibat Cahaya Datang." },
    { "en": "Apa Itu 'Work Function' (Fungsi Kerja)?", "id": "Energi Minimal Melepas Elektron Permukaan." },
    { "en": "Apa Itu 'Hall Effect' (Efek Hall)?", "id": "Munculnya Tegangan Akibat Medan Magnet." },
    { "en": "Apa Itu 'Mobility' (Mobilitas Pembawa)?", "id": "Kecepatan Gerak Muatan Dalam Bahan." },
    { "en": "Apa Itu 'Diffusion' (Difusi Muatan)?", "id": "Gerak Muatan Akibat Perbedaan Konsentrasi." },
    { "en": "Apa Itu 'Drift' (Hanyutan Muatan)?", "id": "Gerak Muatan Akibat Medan Listrik." },
    { "en": "Apa Itu 'Capacitance' (Kapasitansi)?", "id": "Kemampuan Komponen Menyimpan Muatan Listrik." },
    { "en": "Apa Itu 'Inductance' (Induktansi)?", "id": "Kemampuan Komponen Menyimpan Medan Magnet." },
    { "en": "Apa Itu 'Mutual Inductance' (Induktansi Bersama)?", "id": "Induksi Antara Dua Kumparan Berdekatan." },
    { "en": "Apa Itu 'Self Inductance' (Induktansi Diri)?", "id": "Induksi Pada Kumparan Oleh Dirinya." },
    { "en": "Apa Itu 'Transformer' (Transformator)?", "id": "Alat Pengubah Level Tegangan AC." },
    { "en": "Apa Itu 'Core Loss' (Rugi Inti)?", "id": "Daya Hilang Pada Inti Besi." },
    { "en": "Apa Itu 'Copper Loss' (Rugi Tembaga)?", "id": "Daya Hilang Pada Lilitan Kawat." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi Mesin)?", "id": "Rasio Daya Keluar Terhadap Masuk." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Penurunan Tegangan Akibat Hambatan Dalam." },
    { "en": "Apa Itu 'Open Circuit Test' (Uji Beban Nol)?", "id": "Tes Mencari Nilai Rugi Inti." },
    { "en": "Apa Itu 'Short Circuit Test' (Uji Hubung Singkat)?", "id": "Tes Mencari Nilai Rugi Tembaga." },
    { "en": "Apa Itu 'Impedance' (Impedansi Mesin)?", "id": "Hambatan Total Aliran Arus AC." },
    { "en": "Apa Itu 'Saturation' (Kejenuhan Magnetik)?", "id": "Kondisi Fluks Tidak Lagi Bertambah." },
    { "en": "Apa Itu 'Air Gap' (Celah Udara)?", "id": "Jarak Antara Stator Dan Rotor." },
    { "en": "Apa Itu 'Cooling System' (Sistem Pendingin)?", "id": "Metode Pembuang Panas Operasi Mesin." },
    { "en": "Apa Itu 'Forced Air' (Udara Paksa)?", "id": "Pendinginan Menggunakan Kipas Aliran Udara." },
    { "en": "Apa Itu 'Oil Cooling' (Pendinginan Minyak)?", "id": "Pendinginan Menggunakan Media Minyak Isolasi." },
    { "en": "Apa Itu 'Vibration' (Getaran Mesin)?", "id": "Gerakan Periodik Akibat Ketidakseimbangan Mekanik." },
    { "en": "Apa Itu 'Insulation Class' (Kelas Isolasi)?", "id": "Batas Ketahanan Suhu Bahan Isolator." },
    { "en": "Apa Itu 'IC' (Integrated Circuit)?", "id": "Kumpulan Komponen Elektronika Dalam Chip." },
    { "en": "Apa Itu 'SSI' (Small Scale Integration)?", "id": "Integrasi Hingga Puluhan Komponen Elektronika." },
    { "en": "Apa Itu 'MSI' (Medium Scale Integration)?", "id": "Integrasi Hingga Ratusan Komponen Elektronika." },
    { "en": "Apa Itu 'LSI' (Large Scale Integration)?", "id": "Integrasi Hingga Ribuan Komponen Elektronika." },
    { "en": "Apa Itu 'VLSI' (Very Large Scale Integration)?", "id": "Integrasi Jutaan Transistor Dalam Chip." },
    { "en": "Apa Itu 'ULSI' (Ultra Large Scale Integration)?", "id": "Integrasi Miliaran Transistor Dalam Chip." },
    { "en": "Apa Itu 'Wafer' (Wafer Silikon)?", "id": "Kepingan Tipis Bahan Dasar Semikonduktor." },
    { "en": "Apa Itu 'Ingot' (Batangan Silikon)?", "id": "Silikon Murni Berbentuk Batang Silinder." },
    { "en": "Apa Itu 'Photolithography' (Fotolitografi)?", "id": "Proses Mencetak Pola Sirkuit Chip." },
    { "en": "Apa Itu 'Photoresist' (Fotoresis)?", "id": "Lapisan Polimer Peka Terhadap Cahaya." },
    { "en": "Apa Itu 'Etching' (Etsa)?", "id": "Proses Pengikisan Lapisan Material Kimia." },
    { "en": "Apa Itu 'Ion Implantation' (Implantasi Ion)?", "id": "Proses Penambahan Pengotor Ke Semikonduktor." },
    { "en": "Apa Itu 'Diffusion' (Difusi)?", "id": "Penyebaran Atom Pengotor Melalui Panas." },
    { "en": "Apa Itu 'Metallization' (Metalisasi)?", "id": "Proses Pembentukan Jalur Penghantar Logam." },
    { "en": "Apa Itu 'Passivation' (Pasivasi)?", "id": "Lapisan Pelindung Permukaan Chip IC." },
    { "en": "Apa Itu 'Die' (Dadu Chip)?", "id": "Potongan Kecil Individual Dari Wafer." },
    { "en": "Apa Itu 'Cleanroom' (Ruang Bersih)?", "id": "Laboratorium Bebas Partikel Debu Kontaminasi." },
    { "en": "Apa Itu 'CMOS' (Complementary Metal Oxide Semiconductor)?", "id": "Teknologi IC Dengan Konsumsi Daya." },
    { "en": "Apa Itu 'NMOS' (N-channel Metal Oxide Semiconductor)?", "id": "Transistor Berbasis Aliran Elektron Negatif." },
    { "en": "Apa Itu 'PMOS' (P-channel Metal Oxide Semiconductor)?", "id": "Transistor Berbasis Aliran Lubang Positif." },
    { "en": "Apa Itu 'FinFET' (Fin Field Effect Transistor)?", "id": "Transistor Tiga Dimensi Efisiensi Tinggi." },
    { "en": "Apa Itu 'SOI' (Silicon On Insulator)?", "id": "Lapisan Silikon Diatas Bahan Isolator." },
    { "en": "Apa Itu 'Gate' (Gerbang Transistor)?", "id": "Terminal Pengendali Arus Utama Transistor." },
    { "en": "Apa Itu 'Source' (Sumber Transistor)?", "id": "Terminal Masuknya Muatan Dalam Transistor." },
    { "en": "Apa Itu 'Drain' (Saluran Keluar Transistor)?", "id": "Terminal Keluarnya Muatan Dalam Transistor." },
    { "en": "Apa Itu 'Threshold Voltage' (Tegangan Ambang)?", "id": "Tegangan Minimum Untuk Mengaktifkan Transistor." },
    { "en": "Apa Itu 'Moore's Law' (Hukum Moore)?", "id": "Prediksi Penggandaan Jumlah Transistor IC." },
    { "en": "Apa Itu 'SoC' (System On Chip)?", "id": "Seluruh Sistem Komputer Dalam Chip." },
    { "en": "Apa Itu 'NoC' (Network On Chip)?", "id": "Sistem Komunikasi Antar Komponen Chip." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Chip Khusus Untuk Fungsi Tertentu." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Chip Digital Yang Dapat Diprogram." },
    { "en": "Apa Itu 'IP Core' (Intellecual Property Core)?", "id": "Desain Blok Logika Siap Pakai." },
    { "en": "Apa Itu 'VHDL' (Hardware Description Language)?", "id": "Bahasa Pemodelan Perangkat Keras Digital." },
    { "en": "Apa Itu 'Verilog' (Bahasa Verilog)?", "id": "Bahasa Standar Deskripsi Perangkat Keras." },
    { "en": "Apa Itu 'Logic Synthesis' (Sintesis Logika)?", "id": "Konversi Kode Menjadi Gerbang Logika." },
    { "en": "Apa Itu 'Place And Route' (Tempat Jalur)?", "id": "Penentuan Lokasi Dan Koneksi Fisik." },
    { "en": "Apa Itu 'GDSII' (Graphic Data System)?", "id": "Format Berkas Standar Desain Layout." },
    { "en": "Apa Itu 'Design Rule Check' (DRC)?", "id": "Verifikasi Kesesuaian Aturan Manufaktur Chip." },
    { "en": "Apa Itu 'Layout Versus Schematic' (LVS)?", "id": "Pengecekan Kesamaan Layout Dan Skematik." },
    { "en": "Apa Itu 'Parasitic Extraction' (Ekstraksi Parasitik)?", "id": "Analisis Efek Hambatan Kapasitansi Liar." },
    { "en": "Apa Itu 'Static Timing Analysis' (STA)?", "id": "Verifikasi Kecepatan Kerja Sirkuit Digital." },
    { "en": "Apa Itu 'Power Integrity' (Integritas Daya)?", "id": "Kualitas Distribusi Tegangan Dalam Chip." },
    { "en": "Apa Itu 'Signal Integrity' (Integritas Sinyal)?", "id": "Kualitas Transmisi Sinyal Tanpa Distorsi." },
    { "en": "Apa Itu 'Package' (Kemasan IC)?", "id": "Wadah Pelindung Chip Dan Konektor." },
    { "en": "Apa Itu 'DIP' (Dual In-line Package)?", "id": "Kemasan IC Dua Baris Kaki." },
    { "en": "Apa Itu 'QFP' (Quad Flat Package)?", "id": "Kemasan Chip Kotak Empat Sisi." },
    { "en": "Apa Itu 'BGA' (Ball Grid Array)?", "id": "Kemasan Chip Dengan Bola Solder." },
    { "en": "Apa Itu 'Flip Chip' (Chip Terbalik)?", "id": "Metode Pemasangan Chip Langsung Terbalik." },
    { "en": "Apa Itu 'Wire Bonding' (Penyambungan Kawat)?", "id": "Koneksi Kawat Emas Chip Ke." },
    { "en": "Apa Itu 'TSV' (Through Silicon Via)?", "id": "Lubang Penghubung Antar Lapisan Chip." },
    { "en": "Apa Itu '3D IC' (Sirkuit Terpadu 3D)?", "id": "Penumpukan Beberapa Chip Secara Vertikal." },
    { "en": "Apa Itu 'Mixed Signal' (Sinyal Campuran)?", "id": "Chip Berisi Sirkuit Analog Digital." },
    { "en": "Apa Itu 'RFIC' (Radio Frequency IC)?", "id": "Chip Khusus Komunikasi Frekuensi Radio." },
    { "en": "Apa Itu 'MEMS' (Micro Electro Mechanical Systems)?", "id": "Sistem Mekanik Dan Elektronik Mikro." },
    { "en": "Apa Itu 'Optoelectronics' (Optoelektronika)?", "id": "Perangkat Interaksi Cahaya Dan Listrik." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Analog Menjadi Digital." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Sinyal Digital Menjadi Analog." },
    { "en": "Apa Itu 'SRAM' (Static Random Access Memory)?", "id": "Memori Akses Cepat Dalam Chip." },
    { "en": "Apa Itu 'DRAM' (Dynamic Random Access Memory)?", "id": "Memori Kapasitas Besar Perlu Penyegaran." },
    { "en": "Apa Itu 'ROM' (Read Only Memory)?", "id": "Memori Penyimpan Data Permanen Chip." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Memory)?", "id": "Memori Permanen Hapus Melalui Listrik." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Non-Volatil Cepat Dan Padat." },
    { "en": "Apa Itu 'Sense Amplifier' (Penguat Pengindra)?", "id": "Sirkuit Pembaca Data Kecil Memori." },
    { "en": "Apa Itu 'Charge Pump' (Pompa Muatan)?", "id": "Sirkuit Penaik Tegangan Dalam Chip." },
    { "en": "Apa Itu 'Voltage Regulator' (Regulator Tegangan)?", "id": "Penjaga Tegangan Operasi Chip Stabil." },
    { "en": "Apa Itu 'PLL' (Phase Locked Loop)?", "id": "Pembangkit Sinyal Detak Frekuensi Stabil." },
    { "en": "Apa Itu 'VCO' (Voltage Controlled Oscillator)?", "id": "Osilator Terkendali Level Tegangan Masuk." },
    { "en": "Apa Itu 'Current Mirror' (Cermin Arus)?", "id": "Sirkuit Pengganda Nilai Arus Referensi." },
    { "en": "Apa Itu 'Bandgap Reference' (Referensi Celah-Pita)?", "id": "Penyedia Tegangan Referensi Stabil Suhu." },
    { "en": "Apa Itu 'Operational Amplifier' (Op-Amp)?", "id": "Penguat Sinyal Analog Serbaguna IC." },
    { "en": "Apa Itu 'Comparator' (Komparator)?", "id": "Sirkuit Pembanding Dua Level Tegangan." },
    { "en": "Apa Itu 'Latch-up' (Kancingan)?", "id": "Kondisi Hubung Singkat Berbahaya CMOS." },
    { "en": "Apa Itu 'Electrostatic Discharge' (ESD)?", "id": "Lonjakan Listrik Statis Perusak Chip." },
    { "en": "Apa Itu 'Antenna Effect' (Efek Antena)?", "id": "Akumulasi Muatan Saat Proses Etsa." },
    { "en": "Apa Itu 'Electromigration' (Elektromigrasi)?", "id": "Perpindahan Atom Logam Akibat Arus." },
    { "en": "Apa Itu 'Hot Carrier Injection' (HCI)?", "id": "Degradasi Transistor Akibat Elektron Berenergi." },
    { "en": "Apa Itu 'NBTI' (Negative Bias Temperature Instability)?", "id": "Penurunan Kinerja Transistor Akibat Panas." },
    { "en": "Apa Itu 'Soft Error' (Kesalahan Lunak)?", "id": "Kesalahan Data Akibat Radiasi Partikel." },
    { "en": "Apa Itu 'BIST' (Built-In Self Test)?", "id": "Sirkuit Pengujian Mandiri Dalam Chip." },
    { "en": "Apa Itu 'Scan Chain' (Rantai Pindai)?", "id": "Metode Pengujian Logika Internal Chip." },
    { "en": "Apa Itu 'Yield' (Hasil Produksi)?", "id": "Persentase Chip Berfungsi Dalam Wafer." },
    { "en": "Apa Itu 'Foundry' (Pabrik Chip)?", "id": "Perusahaan Manufaktur Sirkuit Terpadu Eksternal." },
    { "en": "Apa Itu 'Fabless' (Tanpa Pabrik)?", "id": "Perusahaan Desain Chip Tanpa Manufaktur." },
    { "en": "Apa Itu 'Tape-out' (Rilis Desain)?", "id": "Tahap Akhir Desain Siap Manufaktur." },
    { "en": "Apa Itu 'Photomask' (Masker Foto)?", "id": "Template Pola Sirkuit Untuk Fotolitografi." },
    { "en": "Apa Itu 'Stepper' (Mesin Stepper)?", "id": "Alat Proyeksi Pola Ke Wafer." },
    { "en": "Apa Itu 'CMP' (Chemical Mechanical Planarization)?", "id": "Proses Perataan Permukaan Wafer Silikon." },
    { "en": "Apa Itu 'Doping Profile' (Profil Doping)?", "id": "Distribusi Konsentrasi Pengotor Dalam Bahan." },
    { "en": "Apa Itu 'Sheet Resistance' (Resistansi Lembar)?", "id": "Ukuran Hambatan Lapisan Tipis Material." },
    { "en": "Apa Itu 'Epitaxy' (Epitaksi)?", "id": "Pertumbuhan Kristal Baru Diatas Wafer." },
    { "en": "Apa Itu 'Polysilicon' (Polisilikon)?", "id": "Silikon Kristal Banyak Untuk Gerbang." },
    { "en": "Apa Itu 'Gate Oxide' (Oksida Gerbang)?", "id": "Lapisan Isolasi Tipis Bawah Gerbang." },
    { "en": "Apa Itu 'High-K Dielectric' (Dielektrik K-Tinggi)?", "id": "Material Isolator Pengurang Arus Bocor." },
    { "en": "Apa Itu 'Copper Interconnect' (Interkoneksi Tembaga)?", "id": "Penggunaan Tembaga Untuk Jalur Sinyal." },
    { "en": "Apa Itu 'Via' (Lubang Jalur)?", "id": "Koneksi Vertikal Antar Lapisan Logam." },
    { "en": "Apa Itu 'Packaging Reliability' (Keandalan Kemasan)?", "id": "Ketahanan Kemasan Terhadap Kondisi Lingkungan." },
    { "en": "Apa Itu 'Thermal Dissipation' (Disipasi Panas)?", "id": "Pembuangan Energi Panas Dari Chip." },
    { "en": "Apa Itu 'System-In-Package' (SiP)?", "id": "Integrasi Berbagai Chip Dalam Kemasan." },
    { "en": "Apa Itu 'Combinational Logic' (Logika Kombinasional)?", "id": "Output Hanya Tergantung Input Sekarang." },
    { "en": "Apa Itu 'Sequential Logic' (Logika Sekuensial)?", "id": "Output Tergantung Input Dan Memori." },
    { "en": "Apa Itu 'Propagation Delay' (Tunda Rambat)?", "id": "Waktu Sinyal Melalui Gerbang Logika." },
    { "en": "Apa Itu 'Fan-In' (Jumlah Masukan)?", "id": "Jumlah Input Maksimal Gerbang Logika." },
    { "en": "Apa Itu 'Fan-Out' (Jumlah Keluaran)?", "id": "Jumlah Beban Output Gerbang Logika." },
    { "en": "Apa Itu 'Noise Margin' (Margin Derau)?", "id": "Batas Toleransi Gangguan Sinyal Digital." },
    { "en": "Apa Itu 'Race Condition' (Kondisi Balapan)?", "id": "Ketergantungan Output Pada Urutan Sinyal." },
    { "en": "Apa Itu 'Logic Hazard' (Bahaya Logika)?", "id": "Transisi Sinyal Output Yang Tidak Diinginkan." },
    { "en": "Apa Itu 'Glitch' (Denyut Liar)?", "id": "Lonjakan Sinyal Singkat Penyebab Kesalahan." },
    { "en": "Apa Itu 'Karnaugh Map' (Peta Karnaugh)?", "id": "Metode Grafis Penyederhanaan Fungsi Logika." },
    { "en": "Apa Itu 'Quine-McCluskey' (Algoritma Quine-McCluskey)?", "id": "Metode Tabulasi Penyederhanaan Fungsi Logika." },
    { "en": "Apa Itu 'Half Adder' (Penjumlah Setengah)?", "id": "Sirkuit Penjumlah Dua Bit Digital." },
    { "en": "Apa Itu 'Full Adder' (Penjumlah Penuh)?", "id": "Sirkuit Penjumlah Tiga Bit Digital." },
    { "en": "Apa Itu 'CLA' (Carry Lookahead Adder)?", "id": "Penjumlah Cepat Dengan Prediksi Bawaan." },
    { "en": "Apa Itu 'Multiplexer' (Multiplekser)?", "id": "Pemilih Satu Jalur Dari Banyak." },
    { "en": "Apa Itu 'Demultiplexer' (Demultipleksper)?", "id": "Penyalur Satu Jalur Ke Banyak." },
    { "en": "Apa Itu 'Priority Encoder' (Enkoder Prioritas)?", "id": "Enkoder Berdasarkan Urutan Kepentingan Input." },
    { "en": "Apa Itu 'Magnitude Comparator' (Komparator Magnitudo)?", "id": "Sirkuit Pembanding Besar Dua Bilangan." },
    { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Unit Pemroses Matematika Dan Logika." },
    { "en": "Apa Itu 'Flip-Flop' (Flip-Flop)?", "id": "Elemen Penyimpan Data Satu Bit." },
    { "en": "Apa Itu 'SR Flip-Flop' (Set-Reset)?", "id": "Flip-Flop Dasar Dengan Kondisi Terlarang." },
    { "en": "Apa Itu 'JK Flip-Flop' (Jack-Kilby)?", "id": "Flip-Flop Universal Dengan Fitur Toggle." },
    { "en": "Apa Itu 'D Flip-Flop' (Data)?", "id": "Flip-Flop Penunda Data Satu Detak." },
    { "en": "Apa Itu 'T Flip-Flop' (Toggle)?", "id": "Flip-Flop Pembalik Status Setiap Detak." },
    { "en": "Apa Itu 'Edge Triggered' (Pemicuan Tepi)?", "id": "Aktivasi Berdasarkan Perubahan Sinyal Detak." },
    { "en": "Apa Itu 'Level Triggered' (Pemicuan Level)?", "id": "Aktivasi Berdasarkan Nilai Sinyal Detak." },
    { "en": "Apa Itu 'Master-Slave' (Tuan-Hamba)?", "id": "Konfigurasi Flip-Flop Pencegah Balapan Sinyal." },
    { "en": "Apa Itu 'Shift Register' (Register Geser)?", "id": "Rangkaian Flip-Flop Pemindah Data Seri." },
    { "en": "Apa Itu 'PISO' (Parallel In Serial Out)?", "id": "Input Paralel Menjadi Output Seri." },
    { "en": "Apa Itu 'SIPO' (Serial In Parallel Out)?", "id": "Input Seri Menjadi Output Paralel." },
    { "en": "Apa Itu 'Binary Counter' (Pencacah Biner)?", "id": "Sirkuit Penghitung Urutan Bilangan Biner." },
    { "en": "Apa Itu 'Asynchronous Counter' (Pencacah Asinkron)?", "id": "Pencacah Dengan Sinyal Detak Berantai." },
    { "en": "Apa Itu 'Synchronous Counter' (Pencacah Sinkron)?", "id": "Pencacah Dengan Sinyal Detak Bersamaan." },
    { "en": "Apa Itu 'Ring Counter' (Pencacah Cincin)?", "id": "Register Geser Dengan Output Kembali." },
    { "en": "Apa Itu 'Johnson Counter' (Pencacah Johnson)?", "id": "Pencacah Cincin Dengan Output Terbalik." },
    { "en": "Apa Itu 'FSM' (Finite State Machine)?", "id": "Model Perubahan Status Logika Digital." },
    { "en": "Apa Itu 'State Diagram' (Diagram Status)?", "id": "Visualisasi Perpindahan Status Sistem Digital." },
    { "en": "Apa Itu 'State Table' (Tabel Status)?", "id": "Representasi Tabulasi Logika Perpindahan Status." },
    { "en": "Apa Itu 'Moore Machine' (Mesin Moore)?", "id": "Output Hanya Tergantung Status Sekarang." },
    { "en": "Apa Itu 'Mealy Machine' (Mesin Mealy)?", "id": "Output Tergantung Status Dan Input." },
    { "en": "Apa Itu 'Instruction Set' (Kumpulan Instruksi)?", "id": "Daftar Operasi Dasar Sebuah Prosesor." },
    { "en": "Apa Itu 'Opcode' (Operation Code)?", "id": "Bagian Instruksi Penentu Jenis Operasi." },
    { "en": "Apa Itu 'Operand' (Operand)?", "id": "Data Yang Diolah Oleh Instruksi." },
    { "en": "Apa Itu 'Addressing Mode' (Mode Pengalamatan)?", "id": "Cara Menentukan Lokasi Data Memori." },
    { "en": "Apa Itu 'Immediate Addressing' (Pengalamatan Langsung)?", "id": "Data Berada Langsung Dalam Instruksi." },
    { "en": "Apa Itu 'Register Addressing' (Pengalamatan Register)?", "id": "Data Berada Di Dalam Register." },
    { "en": "Apa Itu 'Program Counter' (Penghitung Program)?", "id": "Register Penunjuk Alamat Instruksi Selanjutnya." },
    { "en": "Apa Itu 'Stack Pointer' (Penunjuk Tumpukan)?", "id": "Register Alamat Atas Memori Tumpukan." },
    { "en": "Apa Itu 'Accumulator' (Akumulator)?", "id": "Register Penyimpan Hasil Operasi ALU." },
    { "en": "Apa Itu 'Instruction Cycle' (Siklus Instruksi)?", "id": "Proses Ambil, Dekode, Dan Eksekusi." },
    { "en": "Apa Itu 'Fetch' (Ambil Instruksi)?", "id": "Proses Pengambilan Instruksi Dari Memori." },
    { "en": "Apa Itu 'Decode' (Dekode Instruksi)?", "id": "Proses Penerjemahan Instruksi Menjadi Kontrol." },
    { "en": "Apa Itu 'Execute' (Eksekusi Instruksi)?", "id": "Proses Pelaksanaan Operasi Oleh Prosesor." },
    { "en": "Apa Itu 'Write Back' (Tulis Balik)?", "id": "Penyimpanan Hasil Eksekusi Ke Memori." },
    { "en": "Apa Itu 'Pipelining' (Pemrosesan Jalur Pipa)?", "id": "Eksekusi Banyak Instruksi Secara Simultan." },
    { "en": "Apa Itu 'Pipeline Hazard' (Bahaya Jalur Pipa)?", "id": "Kondisi Penghambat Kelancaran Eksekusi Pipa." },
    { "en": "Apa Itu 'Data Hazard' (Bahaya Data)?", "id": "Instruksi Menunggu Hasil Instruksi Sebelumnya." },
    { "en": "Apa Itu 'Control Hazard' (Bahaya Kontrol)?", "id": "Gangguan Pipa Akibat Instruksi Cabang." },
    { "en": "Apa Itu 'Branch Prediction' (Prediksi Cabang)?", "id": "Teknik Perkiraan Arah Alur Program." },
    { "en": "Apa Itu 'CISC' (Complex Instruction Set Computer)?", "id": "Prosesor Dengan Instruksi Kompleks Banyak." },
    { "en": "Apa Itu 'RISC' (Reduced Instruction Set Computer)?", "id": "Prosesor Dengan Instruksi Sederhana Efisien." },
    { "en": "Apa Itu 'VLIW' (Very Long Instruction Word)?", "id": "Instruksi Panjang Berisi Banyak Operasi." },
    { "en": "Apa Itu 'Superscalar' (Arsitektur Superskalar)?", "id": "Kemampuan Eksekusi Banyak Instruksi Per-Detak." },
    { "en": "Apa Itu 'Out Of Order Execution' (Eksekusi Tak-Berurutan)?", "id": "Menjalankan Instruksi Berdasarkan Ketersediaan Data." },
    { "en": "Apa Itu 'Microarchitecture' (Mikroarsitektur)?", "id": "Implementasi Fisik Dari Arsitektur Prosesor." },
    { "en": "Apa Itu 'Control Unit' (Unit Kontrol)?", "id": "Pengatur Aliran Data Dalam Prosesor." },
    { "en": "Apa Itu 'Hardwired Control' (Kontrol Terprogram Keras)?", "id": "Unit Kontrol Berbasis Sirkuit Logika." },
    { "en": "Apa Itu 'Microprogrammed Control' (Kontrol Ter-Mikroprogram)?", "id": "Unit Kontrol Berbasis Memori Instruksi." },
    { "en": "Apa Itu 'Bus' (Bus Data)?", "id": "Jalur Komunikasi Antar Komponen Komputer." },
    { "en": "Apa Itu 'Address Bus' (Bus Alamat)?", "id": "Jalur Penentu Lokasi Memori Tujuan." },
    { "en": "Apa Itu 'Data Bus' (Bus Data)?", "id": "Jalur Pengirim Informasi Antar Perangkat." },
    { "en": "Apa Itu 'Control Bus' (Bus Kontrol)?", "id": "Jalur Pengirim Sinyal Perintah Sistem." },
    { "en": "Apa Itu 'Memory Hierarchy' (Hierarki Memori)?", "id": "Urutan Memori Berdasarkan Kecepatan Kapasitas." },
    { "en": "Apa Itu 'Cache Memory' (Memori Cache)?", "id": "Memori Cepat Antara CPU Memori." },
    { "en": "Apa Itu 'L1 Cache' (Cache Level 1)?", "id": "Cache Tercepat Terintegrasi Dalam Inti." },
    { "en": "Apa Itu 'L2 Cache' (Cache Level 2)?", "id": "Cache Kapasitas Menengah Dekat Prosesor." },
    { "en": "Apa Itu 'Cache Hit' (Berhasil Cache)?", "id": "Data Ditemukan Di Dalam Cache." },
    { "en": "Apa Itu 'Cache Miss' (Gagal Cache)?", "id": "Data Tidak Ditemukan Dalam Cache." },
    { "en": "Apa Itu 'Virtual Memory' (Memori Virtual)?", "id": "Ekstensi Memori Fisik Menggunakan Disk." },
    { "en": "Apa Itu 'MMU' (Memory Management Unit)?", "id": "Perangkat Pengelola Alamat Memori Virtual." },
    { "en": "Apa Itu 'TLB' (Translation Lookaside Buffer)?", "id": "Cache Untuk Mempercepat Translasi Alamat." },
    { "en": "Apa Itu 'Page Fault' (Kesalahan Halaman)?", "id": "Data Tidak Ada Di Memori Utama." },
    { "en": "Apa Itu 'Direct Memory Access' (DMA)?", "id": "Transfer Data Memori Tanpa CPU." },
    { "en": "Apa Itu 'Interrupt' (Interupsi)?", "id": "Sinyal Penghenti Sementara Alur Prosesor." },
    { "en": "Apa Itu 'Vectored Interrupt' (Interupsi Ter-Vektor)?", "id": "Interupsi Dengan Alamat Penanganan Spesifik." },
    { "en": "Apa Itu 'Polling' (Polling)?", "id": "Pengecekan Status Perangkat Secara Rutin." },
    { "en": "Apa Itu 'I/O Mapping' (Pemetaan Input-Output)?", "id": "Cara Mengakses Perangkat Melalui Alamat." },
    { "en": "Apa Itu 'SIMD' (Single Instruction Multiple Data)?", "id": "Satu Instruksi Untuk Banyak Data." },
    { "en": "Apa Itu 'MIMD' (Multiple Instruction Multiple Data)?", "id": "Banyak Instruksi Untuk Banyak Data." },
    { "en": "Apa Itu 'Multi-core' (Multi-inti)?", "id": "Banyak Unit Pemroses Dalam Chip." },
    { "en": "Apa Itu 'Hyper-threading' (Hyper-threading)?", "id": "Satu Inti Fisik Dua Logis." },
    { "en": "Apa Itu 'ECC Memory' (Error Correction Code)?", "id": "Memori Dengan Pendeteksi Perbaikan Kesalahan." },
    { "en": "Apa Itu 'Parity Bit' (Bit Paritas)?", "id": "Bit Tambahan Pendeteksi Kesalahan Biner." },
    { "en": "Apa Itu 'Hamming Code' (Kode Hamming)?", "id": "Metode Koreksi Kesalahan Data Digital." },
    { "en": "Apa Itu 'Baud Rate' (Laju Baud)?", "id": "Kecepatan Simbol Per-Detik Komunikasi Serial." },
    { "en": "Apa Itu 'Bit Rate' (Laju Bit)?", "id": "Kecepatan Bit Per-Detik Komunikasi Digital." },
    { "en": "Apa Itu 'Grey Code' (Kode Grey)?", "id": "Urutan Biner Dengan Satu Perubahan." },
    { "en": "Apa Itu 'BCD' (Binary Coded Decimal)?", "id": "Representasi Angka Desimal Dalam Biner." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Sirkuit Digital Yang Dapat Diprogram." },
    { "en": "Apa Itu 'Fiber Optic' (Serat Optik)?", "id": "Saluran Transmisi Cahaya Kecepatan Tinggi." },
    { "en": "Apa Itu 'Core' (Inti Serat Optik)?", "id": "Pusat Perambatan Cahaya Serat Kaca." },
    { "en": "Apa Itu 'Cladding' (Selubung Serat Optik)?", "id": "Lapisan Pembias Cahaya Kembali Inti." },
    { "en": "Apa Itu 'Coating' (Lapisan Pelindung)?", "id": "Pelindung Mekanis Luar Serat Optik." },
    { "en": "Apa Itu 'TIR' (Total Internal Reflection)?", "id": "Pemantulan Cahaya Sempurna Dalam Inti." },
    { "en": "Apa Itu 'Single Mode' (Mode Tunggal)?", "id": "Serat Untuk Transmisi Jarak Jauh." },
    { "en": "Apa Itu 'Multi Mode' (Banyak Mode)?", "id": "Serat Untuk Transmisi Jarak Pendek." },
    { "en": "Apa Itu 'Refractive Index' (Indeks Bias)?", "id": "Rasio Kecepatan Cahaya Dalam Medium." },
    { "en": "Apa Itu 'Attenuation' (Redaman Serat Optik)?", "id": "Pengurangan Kekuatan Sinyal Cahaya Transmisi." },
    { "en": "Apa Itu 'Dispersion' (Dispersi Sinyal Cahaya)?", "id": "Pelebaran Pulsa Cahaya Selama Transmisi." },
    { "en": "Apa Itu 'Chromatic Dispersion' (Dispersi Kromatik)?", "id": "Pelebaran Akibat Perbedaan Panjang Gelombang." },
    { "en": "Apa Itu 'Modal Dispersion' (Dispersi Modal)?", "id": "Pelebaran Akibat Perbedaan Jalur Cahaya." },
    { "en": "Apa Itu 'PMD' (Polarization Mode Dispersion)?", "id": "Pelebaran Akibat Perbedaan Polarisasi Cahaya." },
    { "en": "Apa Itu 'Numerical Aperture' (Apertur Numerik)?", "id": "Kemampuan Serat Menangkap Sinyal Cahaya." },
    { "en": "Apa Itu 'Acceptance Angle' (Sudut Penerimaan)?", "id": "Sudut Maksimal Cahaya Masuk Inti." },
    { "en": "Apa Itu 'Step Index' (Indeks Berjenjang)?", "id": "Perubahan Indeks Bias Secara Mendadak." },
    { "en": "Apa Itu 'Graded Index' (Indeks Gradual)?", "id": "Perubahan Indeks Bias Secara Bertahap." },
    { "en": "Apa Itu 'Laser Diode' (Dioda Laser)?", "id": "Sumber Cahaya Koheren Intensitas Tinggi." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode) Optik?", "id": "Sumber Cahaya Inkoheren Biaya Rendah." },
    { "en": "Apa Itu 'Wavelength' (Panjang Gelombang Cahaya)?", "id": "Jarak Antar Puncak Gelombang Cahaya." },
    { "en": "Apa Itu 'Optical Window' (Jendela Optik)?", "id": "Rentang Frekuensi Dengan Redaman Rendah." },
    { "en": "Apa Itu 'Photodiode' (Fotodioda)?", "id": "Pengubah Sinyal Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'APD' (Avalanche Photodiode)?", "id": "Detektor Cahaya Dengan Penguatan Internal." },
    { "en": "Apa Itu 'PIN Diode' (Dioda PIN)?", "id": "Detektor Cahaya Sensitivitas Tinggi Standar." },
    { "en": "Apa Itu 'Optical Amplifier' (Penguat Optik)?", "id": "Penguat Sinyal Cahaya Tanpa Konversi." },
    { "en": "Apa Itu 'EDFA' (Erbium Doped Fiber Amplifier)?", "id": "Penguat Sinyal Cahaya Berbasis Erbium." },
    { "en": "Apa Itu 'Raman Amplifier' (Penguat Raman)?", "id": "Penguat Berdasarkan Efek Hamburan Raman." },
    { "en": "Apa Itu 'WDM' (Wavelength Division Multiplexing)?", "id": "Pengiriman Banyak Sinyal Beda Warna." },
    { "en": "Apa Itu 'DWDM' (Dense WDM)?", "id": "Multiplexing Cahaya Dengan Jarak Rapat." },
    { "en": "Apa Itu 'CWDM' (Coarse WDM)?", "id": "Multiplexing Cahaya Dengan Jarak Renggang." },
    { "en": "Apa Itu 'Optical Add-Drop Multiplexer' (OADM)?", "id": "Alat Penambah Atau Pengurang Sinyal." },
    { "en": "Apa Itu 'Optical Switch' (Saklar Optik)?", "id": "Perangkat Pengalih Jalur Sinyal Cahaya." },
    { "en": "Apa Itu 'Optical Coupler' (Kopler Optik)?", "id": "Alat Pembagi Atau Penggabung Cahaya." },
    { "en": "Apa Itu 'Optical Isolator' (Isolator Optik)?", "id": "Pencegah Aliran Cahaya Arah Balik." },
    { "en": "Apa Itu 'Optical Circulator' (Sirkulator Optik)?", "id": "Penyalur Cahaya Ke Port Berurutan." },
    { "en": "Apa Itu 'Fiber Bragg Grating' (FBG)?", "id": "Penyaring Cahaya Dalam Serat Optik." },
    { "en": "Apa Itu 'Optical Attenuator' (Peredam Optik)?", "id": "Alat Penurun Intensitas Cahaya Terkontrol." },
    { "en": "Apa Itu 'Splice' (Sambungan Serat Optik)?", "id": "Penyambungan Permanen Dua Ujung Serat." },
    { "en": "Apa Itu 'Fusion Splicing' (Penyambungan Lebur)?", "id": "Penyambungan Serat Menggunakan Panas Listrik." },
    { "en": "Apa Itu 'Mechanical Splicing' (Penyambungan Mekanis)?", "id": "Penyambungan Serat Menggunakan Penjepit Mekanik." },
    { "en": "Apa Itu 'Splice Loss' (Rugi Sambungan)?", "id": "Daya Hilang Akibat Titik Sambungan." },
    { "en": "Apa Itu 'Optical Connector' (Konektor Optik)?", "id": "Penghubung Serat Optik Tidak Permanen." },
    { "en": "Apa Itu 'SC Connector' (Subscriber Connector)?", "id": "Konektor Standar Bentuk Kotak Dorong." },
    { "en": "Apa Itu 'LC Connector' (Lucent Connector)?", "id": "Konektor Ukuran Kecil Kunci Mekanik." },
    { "en": "Apa Itu 'ST Connector' (Straight Tip)?", "id": "Konektor Bulat Dengan Kunci Bayonet." },
    { "en": "Apa Itu 'FC Connector' (Ferrule Connector)?", "id": "Konektor Bulat Dengan Sistem Ulir." },
    { "en": "Apa Itu 'OTDR' (Optical Time Domain Reflectometer)?", "id": "Alat Analisis Karakteristik Jalur Serat." },
    { "en": "Apa Itu 'Optical Power Meter' (OPM)?", "id": "Alat Ukur Kekuatan Sinyal Cahaya." },
    { "en": "Apa Itu 'Light Source' (Sumber Cahaya Uji)?", "id": "Alat Pembangkit Cahaya Untuk Pengujian." },
    { "en": "Apa Itu 'Visual Fault Locator' (VFL)?", "id": "Laser Merah Pendeteksi Kebocoran Serat." },
    { "en": "Apa Itu 'Optical Spectrum Analyzer' (OSA)?", "id": "Alat Analisis Spektrum Daya Cahaya." },
    { "en": "Apa Itu 'Bit Error Rate' (BER) Optik?", "id": "Rasio Kesalahan Bit Transmisi Cahaya." },
    { "en": "Apa Itu 'Optical Signal To Noise Ratio' (OSNR)?", "id": "Perbandingan Sinyal Cahaya Dan Derau." },
    { "en": "Apa Itu 'Dark Fiber' (Serat Gelap)?", "id": "Kabel Serat Optik Belum Digunakan." },
    { "en": "Apa Itu 'FTTH' (Fiber To The Home)?", "id": "Jaringan Serat Optik Sampai Rumah." },
    { "en": "Apa Itu 'FTTX' (Fiber To The X)?", "id": "Istilah Umum Jaringan Serat Optik." },
    { "en": "Apa Itu 'PON' (Passive Optical Network)?", "id": "Jaringan Optik Tanpa Komponen Aktif." },
    { "en": "Apa Itu 'GPON' (Gigabit PON)?", "id": "Standar Jaringan Optik Kecepatan Gigabit." },
    { "en": "Apa Itu 'EPON' (Ethernet PON)?", "id": "Jaringan Optik Berbasis Protokol Ethernet." },
    { "en": "Apa Itu 'Optical Splitter' (Pembagi Optik)?", "id": "Pemisah Satu Sinyal Menjadi Banyak." },
    { "en": "Apa Itu 'OLT' (Optical Line Terminal)?", "id": "Perangkat Utama Di Pusat Penyedia." },
    { "en": "Apa Itu 'ONU' (Optical Network Unit)?", "id": "Perangkat Penerima Di Sisi Pelanggan." },
    { "en": "Apa Itu 'ONT' (Optical Network Terminal)?", "id": "Terminal Akhir Jaringan Di Pelanggan." },
    { "en": "Apa Itu 'Photonics' (Fotonika)?", "id": "Ilmu Pemanfaatan Cahaya Sebagai Informasi." },
    { "en": "Apa Itu 'Silicon Photonics' (Fotonika Silikon)?", "id": "Sirkuit Optik Di Atas Chip." },
    { "en": "Apa Itu 'Li-Fi' (Light Fidelity)?", "id": "Komunikasi Data Nirkabel Melalui Cahaya." },
    { "en": "Apa Itu 'FSO' (Free Space Optics)?", "id": "Transmisi Cahaya Melalui Udara Terbuka." },
    { "en": "Apa Itu 'Laser' (Light Amplification By Stimulated Emission)?", "id": "Penguatan Cahaya Melalui Emisi Terstimulasi." },
    { "en": "Apa Itu 'Coherence' (Koherensi Cahaya)?", "id": "Keselarasan Fase Dan Frekuensi Cahaya." },
    { "en": "Apa Itu 'Monochromatic' (Monokromatik)?", "id": "Cahaya Dengan Satu Warna Spesifik." },
    { "en": "Apa Itu 'Spontaneous Emission' (Emisi Spontan)?", "id": "Pelepasan Foton Secara Alami Acak." },
    { "en": "Apa Itu 'Stimulated Emission' (Emisi Terstimulasi)?", "id": "Pelepasan Foton Akibat Pemicu Luar." },
    { "en": "Apa Itu 'Population Inversion' (Inversi Populasi)?", "id": "Kondisi Elektron Level Atas Lebih." },
    { "en": "Apa Itu 'Optical Cavity' (Rongga Optik)?", "id": "Ruang Pemantulan Cahaya Dalam Laser." },
    { "en": "Apa Itu 'Pumping' (Pemompaan Laser)?", "id": "Pemberian Energi Untuk Memicu Laser." },
    { "en": "Apa Itu 'Gas Laser' (Laser Gas)?", "id": "Laser Dengan Medium Berupa Gas." },
    { "en": "Apa Itu 'Solid State Laser' (Laser Padat)?", "id": "Laser Dengan Medium Kristal Padat." },
    { "en": "Apa Itu 'Fiber Laser' (Laser Serat)?", "id": "Laser Dengan Medium Serat Optik." },
    { "en": "Apa Itu 'MOPA' (Master Oscillator Power Amplifier)?", "id": "Arsitektur Laser Daya Tinggi Stabil." },
    { "en": "Apa Itu 'Nonlinear Optics' (Optik Tak-Linier)?", "id": "Studi Interaksi Cahaya Intensitas Tinggi." },
    { "en": "Apa Itu 'Four Wave Mixing' (FWM)?", "id": "Interaksi Empat Gelombang Cahaya Berbeda." },
    { "en": "Apa Itu 'Self Phase Modulation' (SPM)?", "id": "Perubahan Fase Akibat Intensitas Sendiri." },
    { "en": "Apa Itu 'Cross Phase Modulation' (XPM)?", "id": "Perubahan Fase Akibat Intensitas Lain." },
    { "en": "Apa Itu 'Optical Sensor' (Sensor Optik)?", "id": "Sensor Berbasis Perubahan Sifat Cahaya." },
    { "en": "Apa Itu 'Fiber Optic Gyroscope' (FOG)?", "id": "Sensor Rotasi Berbasis Serat Optik." },
    { "en": "Apa Itu 'Distributed Temperature Sensing' (DTS)?", "id": "Pengukur Suhu Sepanjang Kabel Optik." },
    { "en": "Apa Itu 'Distributed Acoustic Sensing' (DAS)?", "id": "Pengukur Getaran Sepanjang Kabel Optik." },
    { "en": "Apa Itu 'Optical Soliton' (Soliton Optik)?", "id": "Pulsa Cahaya Stabil Tanpa Dispersi." },
    { "en": "Apa Itu 'Mode Locking' (Penguncian Mode)?", "id": "Teknik Penghasil Pulsa Laser Singkat." },
    { "en": "Apa Itu 'Q-Switching' (Pensakelaran-Q)?", "id": "Teknik Penghasil Pulsa Laser Energi." },
    { "en": "Apa Itu 'Birefringence' (Pembiasan Ganda)?", "id": "Perbedaan Indeks Bias Berdasarkan Polarisasi." },
    { "en": "Apa Itu 'Polarization Controller' (Pengendali Polarisasi)?", "id": "Alat Pengubah Status Polarisasi Cahaya." },
    { "en": "Apa Itu 'Optical Interconnect' (Interkoneksi Optik)?", "id": "Penghubung Data Antar Chip Cahaya." },
    { "en": "Apa Itu 'Photonic Crystal' (Kristal Fotonik)?", "id": "Struktur Nano Pengendali Aliran Cahaya." },
    { "en": "Apa Itu 'Waveguide' (Pemandu Gelombang Optik)?", "id": "Struktur Fisik Pengarah Jalur Cahaya." },
    { "en": "Apa Itu 'Modal Noise' (Derau Modal)?", "id": "Gangguan Akibat Interferensi Banyak Mode." },
    { "en": "Apa Itu 'Speckle Pattern' (Pola Speckle)?", "id": "Pola Bintik Akibat Interferensi Laser." },
    { "en": "Apa Itu 'Acousto-Optic Modulator' (AOM)?", "id": "Pengendali Cahaya Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'Electro-Optic Modulator' (EOM)?", "id": "Pengendali Cahaya Menggunakan Medan Listrik." },
    { "en": "Apa Itu 'Radar' (Radio Detection And Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
    { "en": "Apa Itu 'Pulse Radar' (Radar Pulsa)?", "id": "Radar Pengirim Sinyal Pulsa Pendek." },
    { "en": "Apa Itu 'Doppler Effect' (Efek Doppler)?", "id": "Perubahan Frekuensi Akibat Pergerakan Objek." },
    { "en": "Apa Itu 'MTI' (Moving Target Indicator)?", "id": "Radar Pendeteksi Objek Yang Bergerak." },
    { "en": "Apa Itu 'CW Radar' (Continuous Wave)?", "id": "Radar Pengirim Gelombang Radio Kontinu." },
    { "en": "Apa Itu 'FMCW' (Frequency Modulated Continuous Wave)?", "id": "Radar Pengukur Jarak Gelombang Kontinu." },
    { "en": "Apa Itu 'Transmitter' (Pemancar Radar)?", "id": "Sumber Pembangkit Energi Gelombang Mikro." },
    { "en": "Apa Itu 'Duplexer' (Duplekser Radar)?", "id": "Saklar Pemisah Jalur Antena Radar." },
    { "en": "Apa Itu 'Receiver' (Penerima Radar)?", "id": "Pengolah Sinyal Pantulan Dari Objek." },
    { "en": "Apa Itu 'Magnetron' (Tabung Magnetron)?", "id": "Osilator Daya Tinggi Gelombang Mikro." },
    { "en": "Apa Itu 'Klystron' (Tabung Klystron)?", "id": "Penguat Sinyal Radar Daya Tinggi." },
    { "en": "Apa Itu 'TWT' (Traveling Wave Tube)?", "id": "Penguat Pita Lebar Gelombang Mikro." },
    { "en": "Apa Itu 'Radar Cross Section' (RCS)?", "id": "Ukuran Kemampuan Objek Memantulkan Radar." },
    { "en": "Apa Itu 'Clutter' (Derau Clutter)?", "id": "Pantulan Sinyal Dari Objek Pengganggu." },
    { "en": "Apa Itu 'Phased Array' (Larik Berfase)?", "id": "Antena Dengan Pemindaian Berkas Elektronik." },
    { "en": "Apa Itu 'SAR' (Synthetic Aperture Radar)?", "id": "Radar Pencitraan Resolusi Tinggi Pesawat." },
    { "en": "Apa Itu 'Lidar' (Light Detection And Ranging)?", "id": "Sistem Deteksi Menggunakan Pulsa Cahaya." },
    { "en": "Apa Itu 'Sonar' (Sound Navigation And Ranging)?", "id": "Sistem Deteksi Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'Primary Radar' (Radar Primer)?", "id": "Radar Tanpa Bantuan Respon Objek." },
    { "en": "Apa Itu 'Secondary Radar' (Radar Sekunder)?", "id": "Radar Berdasarkan Jawaban Transponder Objek." },
    { "en": "Apa Itu 'IFF' (Identification Friend Or Foe)?", "id": "Sistem Identifikasi Kawan Atau Lawan." },
    { "en": "Apa Itu 'Blind Speed' (Kecepatan Buta)?", "id": "Kecepatan Objek Yang Tidak Terdeteksi." },
    { "en": "Apa Itu 'Maximum Unambiguous Range' (Jarak Maksimal)?", "id": "Jarak Terjauh Tanpa Kebingungan Pulsa." },
    { "en": "Apa Itu 'Beamwidth' (Lebar Berkas Antena)?", "id": "Sudut Cakupan Energi Radiasi Radar." },
    { "en": "Apa Itu 'Azimuth' (Sudut Azimut)?", "id": "Posisi Sudut Horizontal Objek Radar." },
    { "en": "Apa Itu 'Elevation' (Sudut Elevasi)?", "id": "Posisi Sudut Vertikal Objek Radar." },
    { "en": "Apa Itu 'PPI' (Plan Position Indicator)?", "id": "Tampilan Peta Radar Melingkar Standar." },
    { "en": "Apa Itu 'A-Scope' (Tampilan A-Scope)?", "id": "Tampilan Amplitudo Sinyal Terhadap Jarak." },
    { "en": "Apa Itu 'GNSS' (Global Navigation Satellite System)?", "id": "Sistem Navigasi Satelit Cakupan Global." },
    { "en": "Apa Itu 'GPS' (Global Positioning System)?", "id": "Sistem Navigasi Satelit Milik Amerika." },
    { "en": "Apa Itu 'GLONASS' (Globalnaya Navigatsionnaya Sputnikovaya)?", "id": "Sistem Navigasi Satelit Milik Rusia." },
    { "en": "Apa Itu 'Galileo' (Sistem Galileo)?", "id": "Sistem Navigasi Satelit Milik Eropa." },
    { "en": "Apa Itu 'BeiDou' (Sistem BeiDou)?", "id": "Sistem Navigasi Satelit Milik Tiongkok." },
    { "en": "Apa Itu 'Triangulation' (Triangulasi)?", "id": "Penentuan Posisi Berdasarkan Sudut Segitiga." },
    { "en": "Apa Itu 'Trilateration' (Trilaterasi)?", "id": "Penentuan Posisi Berdasarkan Jarak Satelit." },
    { "en": "Apa Itu 'Ephemeris Data' (Data Efemeris)?", "id": "Informasi Posisi Orbit Satelit Presisi." },
    { "en": "Apa Itu 'Almanac Data' (Data Almanak)?", "id": "Informasi Orbit Kasar Seluruh Satelit." },
    { "en": "Apa Itu 'Dilution Of Precision' (DOP)?", "id": "Indikator Penurunan Akurasi Geometris Satelit." },
    { "en": "Apa Itu 'Multipath Error' (Kesalahan Multipath)?", "id": "Gangguan Akibat Pantulan Sinyal Satelit." },
    { "en": "Apa Itu 'Differential GPS' (DGPS)?", "id": "Teknik Peningkatan Akurasi Posisi GPS." },
    { "en": "Apa Itu 'INS' (Inertial Navigation System)?", "id": "Navigasi Berdasarkan Sensor Gerak Mandiri." },
    { "en": "Apa Itu 'IMU' (Inertial Measurement Unit)?", "id": "Modul Sensor Akselerometer Dan Giroskop." },
    { "en": "Apa Itu 'Accelerometer' (Akselerometer)?", "id": "Sensor Pengukur Percepatan Gerak Linear." },
    { "en": "Apa Itu 'Gyroscope' (Giroskop)?", "id": "Sensor Pengukur Kecepatan Sudut Rotasi." },
    { "en": "Apa Itu 'Magnetometer' (Magnetometer)?", "id": "Sensor Pengukur Kekuatan Medan Magnet." },
    { "en": "Apa Itu 'Dead Reckoning' (Navigasi Estimasi)?", "id": "Perhitungan Posisi Berdasarkan Data Terakhir." },
    { "en": "Apa Itu 'Altimeter' (Altimeter)?", "id": "Alat Pengukur Ketinggian Dari Permukaan." },
    { "en": "Apa Itu 'Barometer' (Barometer)?", "id": "Sensor Pengukur Tekanan Udara Atmosfer." },
    { "en": "Apa Itu 'Biomedical Engineering' (Teknik Biomedis)?", "id": "Aplikasi Teknik Dalam Bidang Medis." },
    { "en": "Apa Itu 'Bioinstrumentation' (Bioinstrumentasi)?", "id": "Alat Elektronik Pengukur Sinyal Biologis." },
    { "en": "Apa Itu 'Biosensor' (Biosensor)?", "id": "Sensor Pendeteksi Zat Kimia Biologis." },
    { "en": "Apa Itu 'ECG' (Electrocardiogram)?", "id": "Alat Rekam Aktivitas Listrik Jantung." },
    { "en": "Apa Itu 'EEG' (Electroencephalogram)?", "id": "Alat Rekam Aktivitas Listrik Otak." },
    { "en": "Apa Itu 'EMG' (Electromyogram)?", "id": "Alat Rekam Aktivitas Listrik Otot." },
    { "en": "Apa Itu 'EOG' (Electrooculogram)?", "id": "Alat Rekam Aktivitas Listrik Mata." },
    { "en": "Apa Itu 'Biopotential' (Biopotensial)?", "id": "Tegangan Listrik Yang Dihasilkan Sel." },
    { "en": "Apa Itu 'Electrode' (Elektroda Medis)?", "id": "Antarmuka Penghubung Tubuh Dan Alat." },
    { "en": "Apa Itu 'Isolation Amplifier' (Penguat Isolasi)?", "id": "Penguat Pelindung Pasien Dari Kebocoran." },
    { "en": "Apa Itu 'Artifact' (Artefak Sinyal)?", "id": "Gangguan Non-Biologis Pada Sinyal Medis." },
    { "en": "Apa Itu 'MRI' (Magnetic Resonance Imaging)?", "id": "Pencitraan Medis Berbasis Medan Magnet." },
    { "en": "Apa Itu 'CT Scan' (Computed Tomography)?", "id": "Pencitraan Medis Berbasis Sinar-X Digital." },
    { "en": "Apa Itu 'Ultrasound' (Ultrasonografi)?", "id": "Pencitraan Medis Berbasis Gelombang Suara." },
    { "en": "Apa Itu 'X-Ray' (Sinar-X)?", "id": "Radiasi Elektromagnetik Penembus Jaringan Tubuh." },
    { "en": "Apa Itu 'Pacemaker' (Alat Pacu Jantung)?", "id": "Perangkat Elektronik Pengatur Ritme Jantung." },
    { "en": "Apa Itu 'Defibrillator' (Defibrilator)?", "id": "Alat Pemberi Kejut Listrik Jantung." },
    { "en": "Apa Itu 'Pulse Oximeter' (Oksimeter Nadi)?", "id": "Alat Ukur Kadar Oksigen Darah." },
    { "en": "Apa Itu 'Patient Monitor' (Monitor Pasien)?", "id": "Tampilan Tanda Vital Pasien Real-Time." },
    { "en": "Apa Itu 'Hemodialysis' (Hemodialisis)?", "id": "Mesin Pencuci Darah Pengganti Ginjal." },
    { "en": "Apa Itu 'Ventilator' (Ventilator Medis)?", "id": "Alat Bantu Pernapasan Mekanis Pasien." },
    { "en": "Apa Itu 'Infusion Pump' (Pompa Infus)?", "id": "Alat Pengatur Laju Cairan Infus." },
    { "en": "Apa Itu 'Audiometer' (Audiometer)?", "id": "Alat Uji Tingkat Pendengaran Manusia." },
    { "en": "Apa Itu 'Spirometer' (Spirometer)?", "id": "Alat Ukur Kapasitas Udara Paru-Paru." },
    { "en": "Apa Itu 'Tonometer' (Tonometer)?", "id": "Alat Ukur Tekanan Bola Mata." },
    { "en": "Apa Itu 'Glucometer' (Glukometer)?", "id": "Alat Ukur Kadar Gula Darah." },
    { "en": "Apa Itu 'Endoscope' (Endoskop)?", "id": "Kamera Medis Pemeriksa Rongga Tubuh." },
    { "en": "Apa Itu 'Thermography' (Termografi Medis)?", "id": "Pencitraan Medis Berdasarkan Suhu Tubuh." },
    { "en": "Apa Itu 'Plethysmograph' (Pletismograf)?", "id": "Alat Ukur Perubahan Volume Organ." },
    { "en": "Apa Itu 'Biomaterials' (Biomaterial)?", "id": "Bahan Sintetis Pengganti Jaringan Tubuh." },
    { "en": "Apa Itu 'Prosthetics' (Prostetik)?", "id": "Perangkat Buatan Pengganti Anggota Tubuh." },
    { "en": "Apa Itu 'Neural Interface' (Antarmuka Saraf)?", "id": "Penghubung Sistem Elektronik Dan Saraf." },
    { "en": "Apa Itu 'Telemedicine' (Telemedis)?", "id": "Layanan Medis Melalui Jarak Jauh." },
    { "en": "Apa Itu 'Medical Imaging' (Pencitraan Medis)?", "id": "Teknik Visualisasi Bagian Dalam Tubuh." },
    { "en": "Apa Itu 'Safety Grounding' (Pentanahan Medis)?", "id": "Sistem Keamanan Listrik Peralatan Medis." },
    { "en": "Apa Itu 'Leakage Current' (Arus Bocor)?", "id": "Aliran Arus Listrik Tidak Diinginkan." },
    { "en": "Apa Itu 'Microshock' (Kejut Mikro)?", "id": "Sengatan Listrik Kecil Langsung Jantung." },
    { "en": "Apa Itu 'Macroshock' (Kejut Makro)?", "id": "Sengatan Listrik Besar Melalui Kulit." },
    { "en": "Apa Itu 'Galvanic Skin Response' (GSR)?", "id": "Perubahan Konduktansi Kulit Akibat Emosi." },
    { "en": "Apa Itu 'Capnography' (Kapnografi)?", "id": "Pemantauan Kadar Karbondioksida Saat Napas." },
    { "en": "Apa Itu 'Sphygmomanometer' (Tensimeter)?", "id": "Alat Ukur Tekanan Darah Manusia." },
    { "en": "Apa Itu 'Phonocardiogram' (Fonokardiogram)?", "id": "Rekaman Grafik Suara Detak Jantung." },
    { "en": "Apa Itu 'Biofeedback' (Biofeedback)?", "id": "Kendali Fungsi Tubuh Berdasarkan Data." },
    { "en": "Apa Itu 'Heuristic Analysis' (Analisis Heuristik)?", "id": "Metode Pemecahan Masalah Berdasarkan Pengalaman." },
    { "en": "Apa Itu 'Calibration' (Kalibrasi Medis)?", "id": "Penyesuaian Akurasi Alat Ukur Medis." },
    { "en": "Apa Itu 'Standard IEC 60601' (Standar Keamanan)?", "id": "Aturan Internasional Keamanan Peralatan Medis." },
    { "en": "Apa Itu 'Signal Conditioning' (Pengkondisian Sinyal)?", "id": "Pemrosesan Sinyal Sensor Sebelum Diolah." },
    { "en": "Apa Itu 'Digital Signal Processing' (DSP) Medis?", "id": "Manipulasi Sinyal Biologis Secara Digital." },
    { "en": "Apa Itu 'Feature Extraction' (Ekstraksi Fitur)?", "id": "Pengambilan Informasi Penting Sinyal Medis." },
    { "en": "Apa Itu 'Wearable Devices' (Perangkat Pakai)?", "id": "Alat Pemantau Kesehatan Yang Dipakai." },
    { "en": "Apa Itu 'Implantable Devices' (Perangkat Tanam)?", "id": "Alat Elektronik Di Dalam Tubuh." },
    { "en": "Apa Itu 'IoT' (Internet Of Things)?", "id": "Jaringan Perangkat Terkoneksi Secara Global." },
    { "en": "Apa Itu 'IIoT' (Industrial Internet Of Things)?", "id": "Penerapan Teknologi IoT Pada Industri." },
    { "en": "Apa Itu 'Smart Sensor' (Sensor Cerdas)?", "id": "Sensor Dengan Kemampuan Pemrosesan Data." },
    { "en": "Apa Itu 'MQTT' (Message Queuing Telemetry Transport)?", "id": "Protokol Komunikasi Ringan Untuk IoT." },
    { "en": "Apa Itu 'CoAP' (Constrained Application Protocol)?", "id": "Protokol Transfer Web Perangkat Terbatas." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Sumber Informasi." },
    { "en": "Apa Itu 'Cloud Computing' (Komputasi Awan)?", "id": "Layanan Penyimpanan Dan Pemrosesan Daring." },
    { "en": "Apa Itu 'Gateway' (Gerbang Jaringan IoT)?", "id": "Penghubung Antar Perangkat Dan Awan." },
    { "en": "Apa Itu 'Sensor Node' (Simpul Sensor)?", "id": "Unit Terkecil Pengumpul Data Sensor." },
    { "en": "Apa Itu 'Actuator' (Aktuator)?", "id": "Perangkat Pengubah Sinyal Menjadi Aksi." },
    { "en": "Apa Itu 'Smart City' (Kota Cerdas)?", "id": "Integrasi Teknologi Untuk Efisiensi Kota." },
    { "en": "Apa Itu 'Smart Home' (Rumah Cerdas)?", "id": "Otomasi Perangkat Elektronik Dalam Rumah." },
    { "en": "Apa Itu 'WSN' (Wireless Sensor Network)?", "id": "Jaringan Kumpulan Sensor Nirkabel Terdistribusi." },
    { "en": "Apa Itu 'Mesh Network' (Jaringan Mesh)?", "id": "Topologi Jaringan Saling Terhubung Mandiri." },
    { "en": "Apa Itu 'Star Topology' (Topologi Bintang)?", "id": "Perangkat Terhubung Ke Pusat Tunggal." },
    { "en": "Apa Itu 'Latency' (Latensi)?", "id": "Waktu Tunda Dalam Pengiriman Data." },
    { "en": "Apa Itu 'Throughput' (Throughput)?", "id": "Laju Kecepatan Transfer Data Sebenarnya." },
    { "en": "Apa Itu 'Payload' (Muatan Data)?", "id": "Bagian Utama Berisi Informasi Sinyal." },
    { "en": "Apa Itu 'LPWAN' (Low Power Wide Area Network)?", "id": "Jaringan Jarak Jauh Konsumsi Daya." },
    { "en": "Apa Itu 'LoRa' (Long Range)?", "id": "Teknologi Komunikasi Nirkabel Jarak Jauh." },
    { "en": "Apa Itu 'LoRaWAN' (LoRa Wide Area Network)?", "id": "Protokol Jaringan Berbasis Teknologi LoRa." },
    { "en": "Apa Itu 'NB-IoT' (Narrowband Internet Of Things)?", "id": "Standar Komunikasi Seluler Khusus IoT." },
    { "en": "Apa Itu 'Sigfox' (Teknologi Sigfox)?", "id": "Operator Jaringan Global Khusus IoT." },
    { "en": "Apa Itu 'Zigbee' (Protokol Zigbee)?", "id": "Komunikasi Nirkabel Hemat Energi Jarak." },
    { "en": "Apa Itu 'BLE' (Bluetooth Low Energy)?", "id": "Bluetooth Dengan Konsumsi Daya Rendah." },
    { "en": "Apa Itu 'Wi-Fi' (Wireless Fidelity)?", "id": "Jaringan Lokal Nirkabel Kecepatan Tinggi." },
    { "en": "Apa Itu 'RFID' (Radio Frequency Identification)?", "id": "Identifikasi Objek Melalui Gelombang Radio." },
    { "en": "Apa Itu 'NFC' (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
    { "en": "Apa Itu 'Accelerometer' (Akselerometer)?", "id": "Sensor Pengukur Percepatan Gerak Objek." },
    { "en": "Apa Itu 'Ultrasonic Sensor' (Sensor Ultrasonik)?", "id": "Pengukur Jarak Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'PIR' (Passive Infrared Sensor)?", "id": "Sensor Pendeteksi Gerakan Melalui Panas." },
    { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Hambatan Berubah Sesuai Intensitas Cahaya." },
    { "en": "Apa Itu 'Thermistor' (Termistor)?", "id": "Resistor Peka Terhadap Perubahan Suhu." },
    { "en": "Apa Itu 'Capacitive Touch' (Sentuhan Kapasitif)?", "id": "Sensor Pendeteksi Sentuhan Muatan Listrik." },
    { "en": "Apa Itu 'Hall Effect Sensor' (Sensor Efek Hall)?", "id": "Pendeteksi Keberadaan Medan Magnet Dekat." },
    { "en": "Apa Itu 'Gas Sensor' (Sensor Gas)?", "id": "Pendeteksi Konsentrasi Gas Dalam Udara." },
    { "en": "Apa Itu 'Humidity Sensor' (Sensor Kelembapan)?", "id": "Pengukur Kadar Uap Air Lingkungan." },
    { "en": "Apa Itu 'Barometer' (Sensor Barometer)?", "id": "Pengukur Tekanan Udara Suatu Lokasi." },
    { "en": "Apa Itu 'Proximity Sensor' (Sensor Jarak)?", "id": "Sensor Pendeteksi Objek Tanpa Sentuhan." },
    { "en": "Apa Itu 'LiDAR' (Light Detection And Ranging)?", "id": "Pencitraan Jarak Menggunakan Pulsa Cahaya." },
    { "en": "Apa Itu 'Load Cell' (Sel Beban)?", "id": "Sensor Pengukur Berat Melalui Tekanan." },
    { "en": "Apa Itu 'Strain Gauge' (Pengukur Regangan)?", "id": "Pengukur Perubahan Bentuk Benda Fisik." },
    { "en": "Apa Itu 'Photodiode' (Fotodioda)?", "id": "Pengubah Energi Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'IR Receiver' (Penerima Inframerah)?", "id": "Sensor Penerima Sinyal Cahaya Inframerah." },
    { "en": "Apa Itu 'RTC' (Real Time Clock)?", "id": "Modul Penghitung Waktu Dan Tanggal." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Programmable Memory)?", "id": "Memori Penyimpan Data Permanen Digital." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Data Non-Volatil Kecepatan Tinggi." },
    { "en": "Apa Itu 'OTA' (Over The Air)?", "id": "Pembaruan Perangkat Lunak Secara Nirkabel." },
    { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Aplikasi Digital." },
    { "en": "Apa Itu 'REST' (Representational State Transfer)?", "id": "Arsitektur Layanan Web Berbasis HTTP." },
    { "en": "Apa Itu 'JSON' (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan Teks." },
    { "en": "Apa Itu 'Pub/Sub' (Publish Subscribe)?", "id": "Model Komunikasi Berbasis Topik Data." },
    { "en": "Apa Itu 'Broker' (Broker MQTT)?", "id": "Server Pusat Pengatur Pesan IoT." },
    { "en": "Apa Itu 'Topic' (Topik Data)?", "id": "Alamat Penyaluran Informasi Dalam MQTT." },
    { "en": "Apa Itu 'QoS' (Quality Of Service)?", "id": "Tingkat Jaminan Pengiriman Pesan Data." },
    { "en": "Apa Itu 'Keep Alive' (Tetap Hidup)?", "id": "Sinyal Penjaga Koneksi Tetap Terhubung." },
    { "en": "Apa Itu 'Encryption' (Enkripsi)?", "id": "Pengamanan Data Melalui Kode Rahasia." },
    { "en": "Apa Itu 'TLS' (Transport Layer Security)?", "id": "Protokol Keamanan Komunikasi Jaringan Internet." },
    { "en": "Apa Itu 'AES' (Advanced Encryption Standard)?", "id": "Standar Algoritma Enkripsi Data Simetris." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Representasi Virtual Dari Objek Fisik." },
    { "en": "Apa Itu 'Machine Learning' (Pembelajaran Mesin)?", "id": "Algoritma Belajar Dari Pola Data." },
    { "en": "Apa Itu 'Big Data' (Data Besar)?", "id": "Kumpulan Data Volume Sangat Besar." },
    { "en": "Apa Itu 'Data Analytics' (Analitik Data)?", "id": "Proses Pengolahan Data Menjadi Informasi." },
    { "en": "Apa Itu 'Dashboard' (Dasbor Visual)?", "id": "Tampilan Visual Pemantauan Data IoT." },
    { "en": "Apa Itu 'Firmware' (Perangkat Tegar)?", "id": "Software Permanen Di Dalam Hardware." },
    { "en": "Apa Itu 'Debugging' (Pengawa-kutu)?", "id": "Proses Pencarian Dan Perbaikan Kesalahan." },
    { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Jalur Penghubung Komponen Elektronika." },
    { "en": "Apa Itu 'SMT' (Surface Mount Technology)?", "id": "Teknologi Pemasangan Komponen Permukaan Papan." },
    { "en": "Apa Itu 'Through Hole' (Lubang Tembus)?", "id": "Pemasangan Komponen Melalui Lubang Papan." },
    { "en": "Apa Itu 'Microcontroller' (Mikrokontroler)?", "id": "Komputer Kecil Untuk Kendali Perangkat." },
    { "en": "Apa Itu 'Arduino' (Platform Arduino)?", "id": "Papan Mikrokontroler Mudah Untuk Pemula." },
    { "en": "Apa Itu 'ESP32' (Chip ESP32)?", "id": "Mikrokontroler Dengan Fitur Wi-Fi Bluetooth." },
    { "en": "Apa Itu 'Raspberry Pi' (Komputer Papan Tunggal)?", "id": "Komputer Kecil Berbasis Sistem Linux." },
    { "en": "Apa Itu 'GPIO' (General Purpose Input Output)?", "id": "Pin Digital Untuk Berbagai Fungsi." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Sinyal Kotak Pengatur Daya Listrik." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Komunikasi Serial Dua Kabel Sederhana." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Komunikasi Serial Sinkron Kecepatan Tinggi." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Komunikasi Serial Data Tanpa Detak." },
    { "en": "Apa Itu 'Baud Rate' (Laju Baud)?", "id": "Kecepatan Bit Per Detik Serial." },
    { "en": "Apa Itu 'Logic Level' (Level Logika)?", "id": "Standar Tegangan Sinyal Digital Chip." },
    { "en": "Apa Itu 'Pull-Up' (Resistor Tarik Atas)?", "id": "Penjaga Status Logika Tinggi Jalur." },
    { "en": "Apa Itu 'Pull-Down' (Resistor Tarik Bawah)?", "id": "Penjaga Status Logika Rendah Jalur." },
    { "en": "Apa Itu 'Debouncing' (Penghilang Pantulan)?", "id": "Pembersihan Sinyal Dari Tombol Mekanik." },
    { "en": "Apa Itu 'ISR' (Interrupt Service Routine)?", "id": "Fungsi Khusus Penanganan Sinyal Interupsi." },
    { "en": "Apa Itu 'Sleep Mode' (Mode Tidur)?", "id": "Kondisi Hemat Daya Pada Mikrokontroler." },
    { "en": "Apa Itu 'Deep Sleep' (Tidur Lelap)?", "id": "Konsumsi Daya Minimal Saat Standby." },
    { "en": "Apa Itu 'Wakeup Source' (Sumber Bangun)?", "id": "Pemicu Untuk Mengaktifkan Mikrokontroler Kembali." },
    { "en": "Apa Itu 'Power Consumption' (Konsumsi Daya)?", "id": "Jumlah Energi Digunakan Suatu Perangkat." },
    { "en": "Apa Itu 'Solar Harvesting' (Pemanenan Surya)?", "id": "Penggunaan Energi Matahari Untuk Sensor." },
    { "en": "Apa Itu 'Precision' (Presisi Sensor)?", "id": "Konsistensi Hasil Pengukuran Berulang Kali." },
    { "en": "Apa Itu 'Accuracy' (Akurasi Sensor)?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
    { "en": "Apa Itu 'Sensitivity' (Sensitivitas)?", "id": "Rasio Perubahan Output Terhadap Input." },
    { "en": "Apa Itu 'Calibration' (Kalibrasi)?", "id": "Proses Penyesuaian Akurasi Alat Ukur." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan)?", "id": "Proses Pengambilan Data Sinyal Analog." },
    { "en": "Apa Itu 'Network Security' (Keamanan Jaringan)?", "id": "Sistem Perlindungan Akses Jaringan Komputer." },
    { "en": "Apa Itu 'Cryptography' (Kriptografi)?", "id": "Ilmu Rahasia Pengamanan Pesan Data." },
    { "en": "Apa Itu 'Encryption' (Enkripsi)?", "id": "Pengubahan Pesan Menjadi Kode Rahasia." },
    { "en": "Apa Itu 'Decryption' (Dekripsi)?", "id": "Pengubahan Kode Kembali Menjadi Pesan." },
    { "en": "Apa Itu 'Plaintext' (Teks Terang)?", "id": "Pesan Asli Yang Dapat Dibaca." },
    { "en": "Apa Itu 'Ciphertext' (Teks Sandi)?", "id": "Pesan Hasil Enkripsi Tidak Terbaca." },
    { "en": "Apa Itu 'Symmetric Encryption' (Enkripsi Simetris)?", "id": "Enkripsi Dengan Satu Kunci Sama." },
    { "en": "Apa Itu 'Asymmetric Encryption' (Enkripsi Asimetris)?", "id": "Enkripsi Dengan Pasangan Kunci Berbeda." },
    { "en": "Apa Itu 'Public Key' (Kunci Publik)?", "id": "Kunci Yang Disebarkan Secara Umum." },
    { "en": "Apa Itu 'Private Key' (Kunci Pribadi)?", "id": "Kunci Rahasia Milik Pemegang Sah." },
    { "en": "Apa Itu 'Hashing' (Hashing Data)?", "id": "Pengubahan Data Menjadi Nilai Unik." },
    { "en": "Apa Itu 'MD5' (Message Digest 5)?", "id": "Algoritma Hash Fungsi Ringkasan Pesan." },
    { "en": "Apa Itu 'SHA' (Secure Hash Algorithm)?", "id": "Standar Algoritma Hash Keamanan Data." },
    { "en": "Apa Itu 'Digital Signature' (Tanda Tangan Digital)?", "id": "Verifikasi Keaslian Dan Integritas Dokumen." },
    { "en": "Apa Itu 'Digital Certificate' (Sertifikat Digital)?", "id": "Identitas Digital Validasi Kunci Publik." },
    { "en": "Apa Itu 'CA' (Certificate Authority)?", "id": "Lembaga Penerbit Sertifikat Keamanan Digital." },
    { "en": "Apa Itu 'Firewall' (Tembok Api Jaringan)?", "id": "Sistem Penyaring Lalu Lintas Jaringan." },
    { "en": "Apa Itu 'IDS' (Intrusion Detection System)?", "id": "Sistem Pendeteksi Percobaan Serangan Jaringan." },
    { "en": "Apa Itu 'IPS' (Intrusion Prevention System)?", "id": "Sistem Pencegah Serangan Jaringan Langsung." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Koneksi Jaringan Pribadi Jalur Terenkripsi." },
    { "en": "Apa Itu 'SSL' (Secure Sockets Layer)?", "id": "Protokol Pengamanan Komunikasi Data Internet." },
    { "en": "Apa Itu 'TLS' (Transport Layer Security)?", "id": "Evolusi Protokol Keamanan Jalur Transportasi." },
    { "en": "Apa Itu 'HTTPS' (Hypertext Transfer Protocol Secure)?", "id": "Protokol Web Dengan Enkripsi Keamanan." },
    { "en": "Apa Itu 'SSH' (Secure Shell)?", "id": "Akses Kendali Komputer Jarak Terenkripsi." },
    { "en": "Apa Itu 'IPsec' (Internet Protocol Security)?", "id": "Kumpulan Protokol Pengamanan Jalur Internet." },
    { "en": "Apa Itu 'DDoS' (Distributed Denial Of Service)?", "id": "Serangan Membanjiri Trafik Hingga Lumpuh." },
    { "en": "Apa Itu 'Malware' (Malicious Software)?", "id": "Perangkat Lunak Perusak Sistem Komputer." },
    { "en": "Apa Itu 'Virus' (Virus Komputer)?", "id": "Program Perusak Yang Menjangkiti Berkas." },
    { "en": "Apa Itu 'Worm' (Worm Jaringan)?", "id": "Malware Yang Menyebar Secara Mandiri." },
    { "en": "Apa Itu 'Trojan' (Trojan Horse)?", "id": "Malware Penyamar Sebagai Program Berguna." },
    { "en": "Apa Itu 'Ransomware' (Perangkat Pemeras)?", "id": "Malware Pengunci Data Meminta Tebusan." },
    { "en": "Apa Itu 'Spyware' (Perangkat Pengintai)?", "id": "Malware Pencuri Informasi Secara Sembunyi." },
    { "en": "Apa Itu 'Phishing' (Phishing)?", "id": "Penipuan Pencurian Identitas Melalui Tautan." },
    { "en": "Apa Itu 'Spoofing' (Pemalsuan Identitas)?", "id": "Penyamaran Identitas Sebagai Pihak Tepercaya." },
    { "en": "Apa Itu 'Brute Force' (Serangan Brute Force)?", "id": "Percobaan Menebak Kata Sandi Terus-Menerus." },
    { "en": "Apa Itu 'Dictionary Attack' (Serangan Kamus)?", "id": "Penebakan Sandi Menggunakan Daftar Kata." },
    { "en": "Apa Itu 'SQL Injection' (Injeksi SQL)?", "id": "Serangan Manipulasi Basis Data Web." },
    { "en": "Apa Itu 'Cross-Site Scripting' (XSS)?", "id": "Penyisipan Skrip Berbahaya Halaman Web." },
    { "en": "Apa Itu 'Man-In-The-Middle' (MITM)?", "id": "Pencegatan Komunikasi Antar Dua Pihak." },
    { "en": "Apa Itu 'Zero-Day Attack' (Serangan Hari Nol)?", "id": "Serangan Eksploitasi Celah Keamanan Terbaru." },
    { "en": "Apa Itu 'HoneyPot' (HoneyPot)?", "id": "Sistem Jebakan Untuk Menjebak Peretas." },
    { "en": "Apa Itu 'DMZ' (Demilitarized Zone)?", "id": "Area Jaringan Antara Lokal Luar." },
    { "en": "Apa Itu 'Vulnerability Scanner' (Pemindai Celah)?", "id": "Alat Pencari Kelemahan Sistem Komputer." },
    { "en": "Apa Itu 'Penetration Testing' (Uji Penetrasi)?", "id": "Simulasi Serangan Legal Evaluasi Keamanan." },
    { "en": "Apa Itu 'MFA' (Multi-Factor Authentication)?", "id": "Verifikasi Identitas Lebih Satu Metode." },
    { "en": "Apa Itu '2FA' (Two-Factor Authentication)?", "id": "Verifikasi Identitas Menggunakan Dua Tahap." },
    { "en": "Apa Itu 'OTP' (One-Time Password)?", "id": "Kata Sandi Sekali Pakai Keamanan." },
    { "en": "Apa Itu 'Biometrics' (Biometrik)?", "id": "Identifikasi Berdasarkan Ciri Fisik Tubuh." },
    { "en": "Apa Itu 'RBAC' (Role-Based Access Control)?", "id": "Pengaturan Akses Berdasarkan Peran Pengguna." },
    { "en": "Apa Itu 'Least Privilege' (Hak Istimewa Terkecil)?", "id": "Pemberian Hak Akses Seperlunya Saja." },
    { "en": "Apa Itu 'Audit Trail' (Jejak Audit)?", "id": "Catatan Log Aktivitas Pengguna Sistem." },
    { "en": "Apa Itu 'Zero Trust' (Arsitektur Tanpa Kepercayaan)?", "id": "Model Keamanan Verifikasi Setiap Akses." },
    { "en": "Apa Itu 'Social Engineering' (Rekayasa Sosial)?", "id": "Manipulasi Psikologis Untuk Mencuri Informasi." },
    { "en": "Apa Itu 'Botnet' (Jaringan Bot)?", "id": "Kumpulan Komputer Terinfeksi Kendali Peretas." },
    { "en": "Apa Itu 'Keylogger' (Keylogger)?", "id": "Perekam Ketukan Tombol Keyboard Komputer." },
    { "en": "Apa Itu 'Rootkit' (Rootkit)?", "id": "Software Sembunyi Penyedia Akses Root." },
    { "en": "Apa Itu 'Backdoor' (Pintu Belakang)?", "id": "Akses Tersembunyi Melewati Mekanisme Keamanan." },
    { "en": "Apa Itu 'Steganography' (Steganografi)?", "id": "Seni Menyembunyikan Pesan Dalam Gambar." },
    { "en": "Apa Itu 'Quantum Cryptography' (Kriptografi Kuantum)?", "id": "Pengamanan Data Menggunakan Fisika Kuantum." },
    { "en": "Apa Itu 'Blockchain' (Rantai Blok)?", "id": "Buku Kas Digital Terdesentralisasi Aman." },
    { "en": "Apa Itu 'Antivirus' (Antivirus)?", "id": "Program Pendeteksi Dan Penghapus Malware." },
    { "en": "Apa Itu 'Endpoint Security' (Keamanan Titik Akhir)?", "id": "Perlindungan Perangkat Akhir Seperti Laptop." },
    { "en": "Apa Itu 'IAM' (Identity And Access Management)?", "id": "Sistem Pengelola Identitas Akses Pengguna." },
    { "en": "Apa Itu 'SIEM' (Security Information Event Management)?", "id": "Analisis Log Keamanan Real-Time Terpusat." },
    { "en": "Apa Itu 'Incident Response' (Respon Insiden)?", "id": "Prosedur Penanganan Saat Terjadi Pelanggaran." },
    { "en": "Apa Itu 'Disaster Recovery' (Pemulihan Bencana)?", "id": "Rencana Pengembalian Sistem Setelah Gangguan." },
    { "en": "Apa Itu 'Backup' (Cadangan Data)?", "id": "Salinan Data Untuk Menghindari Kehilangan." },
    { "en": "Apa Itu 'Cold Site' (Cold Site)?", "id": "Lokasi Cadangan Darurat Tanpa Perangkat." },
    { "en": "Apa Itu 'Hot Site' (Hot Site)?", "id": "Lokasi Cadangan Darurat Siap Pakai." },
    { "en": "Apa Itu 'Compliance' (Kepatuhan Keamanan)?", "id": "Kesesuaian Sistem Dengan Standar Regulasi." },
    { "en": "Apa Itu 'ISO 27001' (Standar Keamanan Informasi)?", "id": "Standar Internasional Manajemen Keamanan Informasi." },
    { "en": "Apa Itu 'GDPR' (General Data Protection Regulation)?", "id": "Regulasi Perlindungan Data Pribadi Eropa." },
    { "en": "Apa Itu 'Data Sovereignty' (Kedaulatan Data)?", "id": "Aturan Lokasi Penyimpanan Data Negara." },
    { "en": "Apa Itu 'Deep Web' (Web Dalam)?", "id": "Bagian Web Tidak Terindeks Google." },
    { "en": "Apa Itu 'Dark Web' (Web Gelap)?", "id": "Bagian Web Untuk Aktivitas Anonim." },
    { "en": "Apa Itu 'Tor' (The Onion Router)?", "id": "Jaringan Anonimitas Menjaga Privasi Pengguna." },
    { "en": "Apa Itu 'Onion Routing' (Perutean Bawang)?", "id": "Teknik Enkripsi Berlapis Jalur Komunikasi." },
    { "en": "Apa Itu 'MAC Filtering' (Penyaringan Alamat MAC)?", "id": "Pembatasan Akses Berdasarkan Identitas Hardware." },
    { "en": "Apa Itu 'Port Scanning' (Pemindaian Port)?", "id": "Pencarian Port Terbuka Jaringan Komputer." },
    { "en": "Apa Itu 'WAF' (Web Application Firewall)?", "id": "Pelindung Khusus Serangan Aplikasi Web." },
    { "en": "Apa Itu 'Proxy Server' (Server Proksi)?", "id": "Perantara Komunikasi Antar Jaringan Berbeda." },
    { "en": "Apa Itu 'Sandboxing' (Sandboxing)?", "id": "Menjalankan Program Dalam Lingkungan Terisolasi." },
    { "en": "Apa Itu 'Air Gap' (Celah Udara)?", "id": "Isolasi Fisik Komputer Dari Internet." },
    { "en": "Apa Itu 'Sniffing' (Penyadapan Data)?", "id": "Pengintaian Lalu Lintas Data Jaringan." },
    { "en": "Apa Itu 'Wardriving' (Wardriving)?", "id": "Pencarian Jaringan Wi-Fi Tidak Aman." },
    { "en": "Apa Itu 'Evil Twin' (Evil Twin)?", "id": "Titik Akses Wi-Fi Palsu Berbahaya." },
    { "en": "Apa Itu 'Salt' (Garam Kriptografi)?", "id": "Data Acak Tambahan Pengaman Hash." },
    { "en": "Apa Itu 'Rainbow Table' (Tabel Pelangi)?", "id": "Database Hash Untuk Memecahkan Sandi." },
    { "en": "Apa Itu 'Key Derivation Function' (KDF)?", "id": "Fungsi Penghasil Kunci Dari Sandi." },
    { "en": "Apa Itu 'Entropy' (Entropi Kriptografi)?", "id": "Ukuran Ketidakpastian Atau Keacakan Data." },
    { "en": "Apa Itu 'Nonce' (Nomor Sekali Pakai)?", "id": "Angka Acak Pencegah Serangan Replay." },
    { "en": "Apa Itu 'Perfect Forward Secrecy' (PFS)?", "id": "Perlindungan Sesi Masa Lalu Tetap." },
    { "en": "Apa Itu 'Homomorphic Encryption' (Enkripsi Homomorfik)?", "id": "Komputasi Data Tanpa Membuka Enkripsi." },
    { "en": "Apa Itu 'Tokenization' (Tokenisasi Data)?", "id": "Penggantian Data Sensitif Dengan Token." },
    { "en": "Apa Itu 'Data Masking' (Penyamaran Data)?", "id": "Penyembunyian Data Sensitif Dari Pengguna." },
    { "en": "Apa Itu 'Access Control List' (ACL)?", "id": "Daftar Aturan Izin Akses Jaringan." },
    { "en": "Apa Itu 'Packet Filtering' (Penyaringan Paket)?", "id": "Pemeriksaan Header Paket Data Jaringan." },
    { "en": "Apa Itu 'Stateful Inspection' (Inspeksi Stateful)?", "id": "Pemantauan Status Koneksi Aktif Jaringan." },
    { "en": "Apa Itu 'Data Science' (Sains Data)?", "id": "Ilmu Ekstraksi Pengetahuan Dari Data." },
    { "en": "Apa Itu 'Statistics' (Statistik)?", "id": "Ilmu Pengumpulan Dan Analisis Data." },
    { "en": "Apa Itu 'Descriptive Statistics' (Statistik Deskriptif)?", "id": "Metode Ringkasan Karakteristik Kumpulan Data." },
    { "en": "Apa Itu 'Inferential Statistics' (Statistik Inferensial)?", "id": "Pengambilan Kesimpulan Populasi Dari Sampel." },
    { "en": "Apa Itu 'Population' (Populasi Data)?", "id": "Seluruh Kelompok Objek Yang Diteliti." },
    { "en": "Apa Itu 'Sample' (Sampel Data)?", "id": "Bagian Kecil Mewakili Seluruh Populasi." },
    { "en": "Apa Itu 'Probability' (Probabilitas)?", "id": "Ukuran Kemungkinan Terjadinya Suatu Peristiwa." },
    { "en": "Apa Itu 'Normal Distribution' (Distribusi Normal)?", "id": "Pola Data Simetris Berbentuk Lonceng." },
    { "en": "Apa Itu 'Standard Deviation' (Deviasi Standar)?", "id": "Ukuran Persebaran Data Terhadap Rata-Rata." },
    { "en": "Apa Itu 'Variance' (Varians Data)?", "id": "Kuadrat Dari Nilai Deviasi Standar." },
    { "en": "Apa Itu 'Correlation' (Korelasi Data)?", "id": "Hubungan Antara Dua Variabel Berbeda." },
    { "en": "Apa Itu 'Regression' (Regresi)?", "id": "Prediksi Hubungan Antar Variabel Numerik." },
    { "en": "Apa Itu 'Hypothesis Testing' (Uji Hipotesis)?", "id": "Prosedur Pembuktian Kebenaran Asumsi Statistik." },
    { "en": "Apa Itu 'P-Value' (Nilai P)?", "id": "Probabilitas Penolakan Terhadap Hipotesis Nol." },
    { "en": "Apa Itu 'Null Hypothesis' (Hipotesis Nol)?", "id": "Asumsi Tidak Ada Perubahan Signifikan." },
    { "en": "Apa Itu 'Type I Error' (Galat Tipe Satu)?", "id": "Kesalahan Menolak Hipotesis Yang Benar." },
    { "en": "Apa Itu 'Type II Error' (Galat Tipe Dua)?", "id": "Kesalahan Menerima Hipotesis Yang Salah." },
    { "en": "Apa Itu 'Confidence Interval' (Interval Kepercayaan)?", "id": "Rentang Estimasi Nilai Parameter Populasi." },
    { "en": "Apa Itu 'Outlier' (Pencilan Data)?", "id": "Nilai Data Sangat Jauh Berbeda." },
    { "en": "Apa Itu 'Data Cleaning' (Pembersihan Data)?", "id": "Proses Penghapusan Data Tidak Akurat." },
    { "en": "Apa Itu 'EDA' (Exploratory Data Analysis)?", "id": "Analisis Awal Untuk Menemukan Pola." },
    { "en": "Apa Itu 'Data Visualization' (Visualisasi Data)?", "id": "Representasi Grafis Informasi Dan Data." },
    { "en": "Apa Itu 'Feature Engineering' (Rekayasa Fitur)?", "id": "Pembuatan Variabel Baru Dari Data." },
    { "en": "Apa Itu 'Dimensionality Reduction' (Reduksi Dimensi)?", "id": "Pengurangan Jumlah Variabel Tanpa Informasi." },
    { "en": "Apa Itu 'PCA' (Principal Component Analysis)?", "id": "Teknik Penyederhanaan Data Dimensi Tinggi." },
    { "en": "Apa Itu 'Clustering' (Pengelompokan)?", "id": "Pengelompokan Data Berdasarkan Kesamaan Karakteristik." },
    { "en": "Apa Itu 'K-Means' (Algoritma K-Means)?", "id": "Metode Klasterisasi Data Berbasis Jarak." },
    { "en": "Apa Itu 'Unsupervised Learning' (Tanpa Pengawasan)?", "id": "Belajar Menemukan Pola Data Mentah." },
    { "en": "Apa Itu 'Neural Network' (Jaringan Saraf)?", "id": "Model Komputasi Peniru Cara Otak." },
    { "en": "Apa Itu 'Overfitting' (Pengepasan Berlebih)?", "id": "Model Terlalu Fokus Pada Data Latih." },
    { "en": "Apa Itu 'Underfitting' (Kurang Pengepasan)?", "id": "Model Gagal Menangkap Pola Utama." },
    { "en": "Apa Itu 'Bias' (Bias Model)?", "id": "Kesalahan Akibat Asumsi Terlalu Sederhana." },
    { "en": "Apa Itu 'Variance' (Varians Model)?", "id": "Sensitivitas Model Terhadap Fluktuasi Data." },
    { "en": "Apa Itu 'Cross Validation' (Validasi Silang)?", "id": "Teknik Evaluasi Peforma Model Statistik." },
    { "en": "Apa Itu 'Confusion Matrix' (Matriks Kebingungan)?", "id": "Tabel Evaluasi Kinerja Klasifikasi Model." },
    { "en": "Apa Itu 'Precision' (Presisi)?", "id": "Rasio Prediksi Positif Yang Benar." },
    { "en": "Apa Itu 'Recall' (Pemanggilan Kembali)?", "id": "Rasio Seluruh Kasus Positif Terdeteksi." },
    { "en": "Apa Itu 'F1-Score' (Skor F1)?", "id": "Rata-Rata Harmonik Presisi Dan Recall." },
    { "en": "Apa Itu 'ROC Curve' (Receiver Operating Characteristic)?", "id": "Grafik Penentu Performa Klasifikasi Biner." },
    { "en": "Apa Itu 'AUC' (Area Under Curve)?", "id": "Luasan Di Bawah Kurva ROC." },
    { "en": "Apa Itu 'Decision Tree' (Pohon Keputusan)?", "id": "Model Prediksi Struktur Percabangan Logika." },
    { "en": "Apa Itu 'Random Forest' (Hutan Acak)?", "id": "Kumpulan Banyak Pohon Keputusan Prediksi." },
    { "en": "Apa Itu 'Gradient Boosting' (Gradient Boosting)?", "id": "Teknik Peningkatan Akurasi Model Sekuensial." },
    { "en": "Apa Itu 'NLP' (Natural Language Processing)?", "id": "Pemrosesan Bahasa Manusia Oleh Komputer." },
    { "en": "Apa Itu 'Sentiment Analysis' (Analisis Sentimen)?", "id": "Pendeteksian Opini Dalam Teks Digital." },
    { "en": "Apa Itu 'Data Warehouse' (Gudang Data)?", "id": "Penyimpanan Terpusat Data Besar Organisasi." },
    { "en": "Apa Itu 'Data Lake' (Danau Data)?", "id": "Penyimpanan Data Mentah Berbagai Format." },
    { "en": "Apa Itu 'ETL' (Extract Transform Load)?", "id": "Proses Integrasi Data Ke Database." },
    { "en": "Apa Itu 'Pixel' (Piksel)?", "id": "Elemen Terkecil Gambar Digital Grafis." },
    { "en": "Apa Itu 'Resolution' (Resolusi Gambar)?", "id": "Jumlah Piksel Per Satuan Luas." },
    { "en": "Apa Itu 'Grayscale' (Skala Abu)?", "id": "Gambar Dengan Variasi Intensitas Hitam." },
    { "en": "Apa Itu 'RGB' (Red Green Blue)?", "id": "Model Warna Dasar Cahaya Digital." },
    { "en": "Apa Itu 'Bit Depth' (Kedalaman Bit)?", "id": "Jumlah Informasi Warna Per Piksel." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan Citra)?", "id": "Konversi Citra Kontinu Ke Spasial." },
    { "en": "Apa Itu 'Quantization' (Kuantisasi Citra)?", "id": "Konversi Intensitas Cahaya Menjadi Digital." },
    { "en": "Apa Itu 'Histogram' (Histogram Citra)?", "id": "Grafik Distribusi Frekuensi Intensitas Warna." },
    { "en": "Apa Itu 'Histogram Equalization' (Ekualisasi Histogram)?", "id": "Teknik Peningkatan Kontras Gambar Digital." },
    { "en": "Apa Itu 'Spatial Filtering' (Filter Spasial)?", "id": "Operasi Perubahan Piksel Berdasarkan Tetangga." },
    { "en": "Apa Itu 'Low Pass Filter' (Filter Lolos Rendah)?", "id": "Penghalusan Gambar Dan Pengurang Derau." },
    { "en": "Apa Itu 'High Pass Filter' (Filter Lolos Tinggi)?", "id": "Penajaman Tepi Dan Detail Gambar." },
    { "en": "Apa Itu 'Median Filter' (Filter Median)?", "id": "Peredam Derau Dengan Nilai Tengah." },
    { "en": "Apa Itu 'Noise' (Derau Citra)?", "id": "Gangguan Tak Diinginkan Pada Gambar." },
    { "en": "Apa Itu 'Salt And Pepper Noise' (Derau Garam Merica)?", "id": "Gangguan Berupa Bintik Hitam Putih." },
    { "en": "Apa Itu 'Gaussian Noise' (Derau Gaussian)?", "id": "Gangguan Intensitas Dengan Distribusi Normal." },
    { "en": "Apa Itu 'Edge Detection' (Deteksi Tepi)?", "id": "Pencarian Batas Objek Dalam Citra." },
    { "en": "Apa Itu 'Sobel Operator' (Operator Sobel)?", "id": "Algoritma Deteksi Tepi Berbasis Gradien." },
    { "en": "Apa Itu 'Canny Edge Detection' (Deteksi Canny)?", "id": "Metode Deteksi Tepi Paling Optimal." },
    { "en": "Apa Itu 'Thresholding' (Ambang Batas)?", "id": "Pemisahan Objek Berdasarkan Nilai Intensitas." },
    { "en": "Apa Itu 'Segmentation' (Segmentasi Citra)?", "id": "Proses Pembagian Citra Menjadi Bagian." },
    { "en": "Apa Itu 'Morphological Operations' (Operasi Morfologi)?", "id": "Pengolahan Bentuk Struktur Objek Citra." },
    { "en": "Apa Itu 'Dilation' (Dilasi)?", "id": "Penambahan Piksel Pada Batas Objek." },
    { "en": "Apa Itu 'Erosion' (Erosi)?", "id": "Pengurangan Piksel Pada Batas Objek." },
    { "en": "Apa Itu 'Opening' (Pembukaan Morfologi)?", "id": "Erosi Diikuti Dilasi Penghilang Derau." },
    { "en": "Apa Itu 'Closing' (Penutupan Morfologi)?", "id": "Dilasi Diikuti Erosi Penutup Lubang." },
    { "en": "Apa Itu 'Object Recognition' (Pengenalan Objek)?", "id": "Identifikasi Jenis Benda Dalam Citra." },
    { "en": "Apa Itu 'Pattern Matching' (Pencocokan Pola)?", "id": "Pencarian Kemiripan Pola Dalam Gambar." },
    { "en": "Apa Itu 'Image Compression' (Kompresi Citra)?", "id": "Pengurangan Ukuran Berkas Gambar Digital." },
    { "en": "Apa Itu 'Lossy Compression' (Kompresi Hilang)?", "id": "Kompresi Dengan Pengurangan Kualitas Gambar." },
    { "en": "Apa Itu 'Lossless Compression' (Kompresi Tanpa Hilang)?", "id": "Kompresi Tanpa Mengurangi Data Asli." },
    { "en": "Apa Itu 'JPEG' (Joint Photographic Experts Group)?", "id": "Format Gambar Kompresi Standar Populer." },
    { "en": "Apa Itu 'PNG' (Portable Network Graphics)?", "id": "Format Gambar Tanpa Kehilangan Data." },
    { "en": "Apa Itu 'Fourier Transform' (Transformasi Fourier)?", "id": "Analisis Citra Dalam Domain Frekuensi." },
    { "en": "Apa Itu 'Frequency Domain' (Domain Frekuensi)?", "id": "Representasi Citra Berdasarkan Laju Perubahan." },
    { "en": "Apa Itu 'Spatial Domain' (Domain Spasial)?", "id": "Representasi Citra Berdasarkan Posisi Piksel." },
    { "en": "Apa Itu 'Convolution' (Konvolusi Citra)?", "id": "Operasi Perkalian Matriks Filter Piksel." },
    { "en": "Apa Itu 'Kernel' (Kernel Filter)?", "id": "Matriks Kecil Untuk Operasi Citra." },
    { "en": "Apa Itu 'Padding' (Bantalan Citra)?", "id": "Penambahan Piksel Tepi Menjaga Ukuran." },
    { "en": "Apa Itu 'Stride' (Langkah)?", "id": "Besar Pergeseran Filter Pada Citra." },
    { "en": "Apa Itu 'CNN' (Convolutional Neural Network)?", "id": "Arsitektur Jaringan Saraf Analisis Citra." },
    { "en": "Apa Itu 'Pooling Layer' (Lapisan Pengumpulan)?", "id": "Pengurangan Dimensi Data Citra Digital." },
    { "en": "Apa Itu 'Feature Map' (Peta Fitur)?", "id": "Hasil Ekstraksi Karakteristik Dari Citra." },
    { "en": "Apa Itu 'Image Restoration' (Restorasi Citra)?", "id": "Perbaikan Citra Akibat Kerusakan Sensor." },
    { "en": "Apa Itu 'Deconvolution' (Dekonvolusi)?", "id": "Proses Pembalikan Efek Kabur Gambar." },
    { "en": "Apa Itu 'Motion Blur' (Kabur Gerak)?", "id": "Efek Kabur Akibat Pergerakan Objek." },
    { "en": "Apa Itu 'Color Gamut' (Gamut Warna)?", "id": "Rentang Warna Yang Dapat Direproduksi." },
    { "en": "Apa Itu 'Contrast Ratio' (Rasio Kontras)?", "id": "Perbedaan Antara Terang Dan Gelap." },
    { "en": "Apa Itu 'Dynamic Range' (Rentang Dinamis)?", "id": "Rasio Intensitas Cahaya Terkecil Terbesar." },
    { "en": "Apa Itu '1G' (First Generation)?", "id": "Generasi Pertama Telepon Seluler Analog." },
    { "en": "Apa Itu '2G' (Second Generation)?", "id": "Generasi Kedua Telepon Seluler Digital." },
    { "en": "Apa Itu '3G' (Third Generation)?", "id": "Generasi Ketiga Komunikasi Data Seluler." },
    { "en": "Apa Itu '4G' (Fourth Generation)?", "id": "Generasi Keempat Jaringan Pita Lebar." },
    { "en": "Apa Itu '5G' (Fifth Generation)?", "id": "Generasi Kelima Teknologi Seluler Modern." },
    { "en": "Apa Itu 'LTE' (Long Term Evolution)?", "id": "Standar Komunikasi Nirkabel Kecepatan Tinggi." },
    { "en": "Apa Itu 'Volte' (Voice Over Long Term Evolution)?", "id": "Layanan Suara Melalui Jaringan LTE." },
    { "en": "Apa Itu 'OFDMA' (Orthogonal Frequency Division Multiple Access)?", "id": "Metode Akses Banyak Pengguna Efisien." },
    { "en": "Apa Itu 'SC-FDMA' (Single Carrier Frequency Division Multiple Access)?", "id": "Metode Transmisi Sinyal Unggah LTE." },
    { "en": "Apa Itu 'MIMO' (Multiple Input Multiple Output)?", "id": "Teknologi Banyak Antena Pengirim Penerima." },
    { "en": "Apa Itu 'Massive MIMO' (Massive Multiple Input Multiple Output)?", "id": "Penggunaan Ratusan Antena Secara Bersamaan." },
    { "en": "Apa Itu 'Beamforming' (Pembentukan Berkas Sinyal)?", "id": "Pengarahan Sinyal Secara Presisi Elektronik." },
    { "en": "Apa Itu 'Small Cell' (Sel Kecil)?", "id": "Node Akses Jangkauan Area Terbatas." },
    { "en": "Apa Itu 'Macro Cell' (Sel Makro)?", "id": "Stasiun Pemancar Jangkauan Area Luas." },
    { "en": "Apa Itu 'Base Station' (Stasiun Basis)?", "id": "Pusat Komunikasi Antar Perangkat Seluler." },
    { "en": "Apa Itu 'BTS' (Base Transceiver Station)?", "id": "Perangkat Pengirim Penerima Sinyal Seluler." },
    { "en": "Apa Itu 'Enodeb' (Evolved Node B)?", "id": "Perangkat Stasiun Basis Jaringan LTE." },
    { "en": "Apa Itu 'Gnodeb' (Next Generation Node B)?", "id": "Perangkat Stasiun Basis Jaringan 5G." },
    { "en": "Apa Itu 'Core Network' (Jaringan Inti)?", "id": "Bagian Pusat Pemroses Data Jaringan." },
    { "en": "Apa Itu 'EPC' (Evolved Packet Core)?", "id": "Arsitektur Jaringan Inti Sistem LTE." },
    { "en": "Apa Itu '5GC' (5G Core Network)?", "id": "Jaringan Inti Canggih Berbasis Layanan." },
    { "en": "Apa Itu 'Network Slicing' (Irisan Jaringan)?", "id": "Pembagian Jaringan Virtual Secara Spesifik." },
    { "en": "Apa Itu 'URLLC' (Ultra-Reliable Low Latency Communications)?", "id": "Komunikasi Latensi Rendah Keandalan Tinggi." },
    { "en": "Apa Itu 'EMBB' (Enhanced Mobile Broadband)?", "id": "Layanan Pita Lebar Kecepatan Ekstrem." },
    { "en": "Apa Itu 'MMTC' (Massive Machine Type Communications)?", "id": "Koneksi Masif Jutaan Perangkat IoT." },
    { "en": "Apa Itu 'Mmwave' (Millimeter Wave)?", "id": "Gelombang Frekuensi Tinggi Kapasitas Besar." },
    { "en": "Apa Itu 'Sub-6 GHz' (Di Bawah 6 Gigahertz)?", "id": "Frekuensi Menengah Jangkauan Luas 5G." },
    { "en": "Apa Itu 'Latency' (Latensi Jaringan)?", "id": "Waktu Tunda Respon Data Seluler." },
    { "en": "Apa Itu 'Throughput' (Throughput Data)?", "id": "Kecepatan Transfer Data Berhasil Sebenarnya." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Kapasitas Maksimal Jalur Komunikasi Data." },
    { "en": "Apa Itu 'Backhaul' (Jaringan Punggung)?", "id": "Penghubung Stasiun Basis Ke Pusat." },
    { "en": "Apa Itu 'Fronthaul' (Jaringan Depan)?", "id": "Penghubung Unit Radio Ke Pemroses." },
    { "en": "Apa Itu 'Handover' (Serah Terima)?", "id": "Perpindahan Koneksi Antar Sel Berjalan." },
    { "en": "Apa Itu 'Roaming' (Jelajah Jaringan)?", "id": "Penggunaan Layanan Di Luar Area." },
    { "en": "Apa Itu 'SIM Card' (Subscriber Identity Module)?", "id": "Kartu Identitas Pengguna Jaringan Seluler." },
    { "en": "Apa Itu 'Esim' (Embedded SIM)?", "id": "Identitas Digital Tertanam Dalam Perangkat." },
    { "en": "Apa Itu 'IMEI' (International Mobile Equipment Identity)?", "id": "Nomor Identitas Unik Perangkat Seluler." },
    { "en": "Apa Itu 'IMSI' (International Mobile Subscriber Identity)?", "id": "Nomor Identitas Unik Pelanggan Jaringan." },
    { "en": "Apa Itu 'Carrier Aggregation' (Agregasi Pembawa)?", "id": "Penggabungan Banyak Pita Frekuensi Data." },
    { "en": "Apa Itu 'Duplexing' (Dupleksing)?", "id": "Metode Komunikasi Dua Arah Sekaligus." },
    { "en": "Apa Itu 'FDD' (Frequency Division Duplexing)?", "id": "Pemisahan Jalur Berdasarkan Pita Frekuensi." },
    { "en": "Apa Itu 'TDD' (Time Division Duplexing)?", "id": "Pemisahan Jalur Berdasarkan Slot Waktu." },
    { "en": "Apa Itu 'Spectrum' (Spektrum Frekuensi)?", "id": "Rentang Gelombang Radio Komunikasi Nirkabel." },
    { "en": "Apa Itu 'Licensed Band' (Pita Berlisensi)?", "id": "Frekuensi Khusus Milik Operator Tertentu." },
    { "en": "Apa Itu 'Unlicensed Band' (Pita Tak Berlisensi)?", "id": "Frekuensi Bebas Digunakan Tanpa Izin." },
    { "en": "Apa Itu 'CBRS' (Citizens Broadband Radio Service)?", "id": "Layanan Berbagi Pita Frekuensi Radio." },
    { "en": "Apa Itu 'Network Topology' (Topologi Jaringan)?", "id": "Struktur Hubungan Antar Node Jaringan." },
    { "en": "Apa Itu 'Mesh Network' (Jaringan Mesh)?", "id": "Topologi Perangkat Saling Terhubung Mandiri." },
    { "en": "Apa Itu 'SDN' (Software Defined Networking)?", "id": "Manajemen Jaringan Berbasis Perangkat Lunak." },
    { "en": "Apa Itu 'NFV' (Network Function Virtualization)?", "id": "Virtualisasi Fungsi Perangkat Keras Jaringan." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Pengguna Akhir." },
    { "en": "Apa Itu 'MEC' (Multi-access Edge Computing)?", "id": "Komputasi Tepi Untuk Jaringan Seluler." },
    { "en": "Apa Itu 'Cloud RAN' (Radio Access Network)?", "id": "Virtualisasi Unit Pengolah Sinyal Radio." },
    { "en": "Apa Itu 'Open RAN' (Open Radio Access Network)?", "id": "Arsitektur Jaringan Radio Terbuka Interoperabel." },
    { "en": "Apa Itu 'CA' (Carrier Aggregation)?", "id": "Teknik Peningkatan Kecepatan Data LTE." },
    { "en": "Apa Itu 'Spectral Efficiency' (Efisiensi Spektral)?", "id": "Laju Data Per Lebar Pita." },
    { "en": "Apa Itu 'QAM' (Quadrature Amplitude Modulation)?", "id": "Metode Modulasi Kapasitas Data Tinggi." },
    { "en": "Apa Itu 'SINR' (Signal To Interference Plus Noise Ratio)?", "id": "Kualitas Sinyal Terhadap Gangguan Derau." },
    { "en": "Apa Itu 'RSSI' (Received Signal Strength Indicator)?", "id": "Ukuran Kekuatan Sinyal Radio Diterima." },
    { "en": "Apa Itu 'RSRP' (Reference Signal Received Power)?", "id": "Level Kekuatan Sinyal Referensi LTE." },
    { "en": "Apa Itu 'RSRQ' (Reference Signal Received Quality)?", "id": "Indikator Kualitas Sinyal Referensi LTE." },
    { "en": "Apa Itu 'CQI' (Channel Quality Indicator)?", "id": "Laporan Kualitas Saluran Oleh Perangkat." },
    { "en": "Apa Itu 'Mcs' (Modulation And Coding Scheme)?", "id": "Pengaturan Modulasi Berdasarkan Kondisi Sinyal." },
    { "en": "Apa Itu 'Path Loss' (Rugi Jalur)?", "id": "Penurunan Daya Sinyal Akibat Jarak." },
    { "en": "Apa Itu 'Shadowing' (Pembayangan Sinyal)?", "id": "Penurunan Sinyal Akibat Hambatan Bangunan." },
    { "en": "Apa Itu 'Multipath Fading' (Pudar Banyak Jalur)?", "id": "Gangguan Akibat Pantulan Sinyal Radio." },
    { "en": "Apa Itu 'Doppler Spread' (Penyebaran Doppler)?", "id": "Perubahan Frekuensi Akibat Kecepatan Tinggi." },
    { "en": "Apa Itu 'Interference' (Interferensi)?", "id": "Gangguan Sinyal Dari Sumber Lain." },
    { "en": "Apa Itu 'CCI' (Co-channel Interference)?", "id": "Gangguan Dari Sel Frekuensi Sama." },
    { "en": "Apa Itu 'ACI' (Adjacent Channel Interference)?", "id": "Gangguan Dari Sel Frekuensi Bersebelahan." },
    { "en": "Apa Itu 'Guard Band' (Pita Penjaga)?", "id": "Celah Frekuensi Pencegah Gangguan Antar." },
    { "en": "Apa Itu 'Synchronization' (Sinkronisasi)?", "id": "Penyelarasan Waktu Antar Node Jaringan." },
    { "en": "Apa Itu 'Handoff' (Handoff)?", "id": "Proses Pengalihan Koneksi Tanpa Putus." },
    { "en": "Apa Itu 'Hard Handoff' (Handoff Keras)?", "id": "Putus Sebelum Terhubung Ke Sel." },
    { "en": "Apa Itu 'Soft Handoff' (Handoff Lunak)?", "id": "Terhubung Ke Banyak Sel Sekaligus." },
    { "en": "Apa Itu 'Paging' (Paging)?", "id": "Proses Pencarian Perangkat Oleh Jaringan." },
    { "en": "Apa Itu 'Registration' (Registrasi)?", "id": "Proses Pengenalan Perangkat Ke Jaringan." },
    { "en": "Apa Itu 'Authentication' (Autentikasi)?", "id": "Verifikasi Validitas Identitas Pengguna Jaringan." },
    { "en": "Apa Itu 'Encryption' (Enkripsi Data)?", "id": "Pengamanan Komunikasi Melalui Kode Rahasia." },
    { "en": "Apa Itu 'Firewall' (Tembok Api Jaringan)?", "id": "Sistem Perlindungan Akses Jaringan Ilegal." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Jalur Komunikasi Aman Melalui Internet." },
    { "en": "Apa Itu 'Ipsec' (Internet Protocol Security)?", "id": "Protokol Keamanan Data Lapisan Internet." },
    { "en": "Apa Itu 'DDoS' (Distributed Denial Of Service)?", "id": "Serangan Pelumpuhan Jaringan Trafik Masif." },
    { "en": "Apa Itu 'SLA' (Service Level Agreement)?", "id": "Kontrak Jaminan Kualitas Layanan Operator." },
    { "en": "Apa Itu 'KPI' (Key Performance Indicator) Jaringan?", "id": "Ukuran Kinerja Kualitas Jaringan Seluler." },
    { "en": "Apa Itu 'Accessibility' (Aksesibilitas Jaringan)?", "id": "Kemudahan Perangkat Terhubung Ke Jaringan." },
    { "en": "Apa Itu 'Retainability' (Mempertahankan Koneksi)?", "id": "Kemampuan Jaringan Menjaga Sesi Aktif." },
    { "en": "Apa Itu 'Drop Call' (Panggilan Putus)?", "id": "Kegagalan Mempertahankan Panggilan Suara Aktif." },
    { "en": "Apa Itu 'Jitter' (Variasi Tunda)?", "id": "Ketidakkonsistenan Waktu Kedatangan Paket Data." },
    { "en": "Apa Itu 'Packet Loss' (Kehilangan Paket)?", "id": "Kegagalan Data Sampai Ke Tujuan." },
    { "en": "Apa Itu 'Coverage' (Cakupan Area)?", "id": "Wilayah Yang Terjangkau Sinyal Jaringan." },
    { "en": "Apa Itu 'Dead Zone' (Zona Buta)?", "id": "Area Tanpa Sinyal Jaringan Seluler." },
    { "en": "Apa Itu 'Indoor Coverage' (Cakupan Dalam Ruangan)?", "id": "Kualitas Sinyal Di Dalam Bangunan." },
    { "en": "Apa Itu 'DAS' (Distributed Antenna System)?", "id": "Sistem Antena Penyebar Sinyal Gedung." },
    { "en": "Apa Itu 'Repeater' (Pengulang Sinyal)?", "id": "Alat Penguat Jangkauan Sinyal Radio." },
    { "en": "Apa Itu 'Microwave Link' (Tautan Gelombang Mikro)?", "id": "Koneksi Radio Titik Ke Titik." },
    { "en": "Apa Itu 'Fiber To The Tower' (FTTT)?", "id": "Koneksi Serat Optik Ke Menara." },
    { "en": "Apa Itu 'Network Monitoring' (Pemantauan Jaringan)?", "id": "Pengawasan Kondisi Jaringan Secara Kontinu." },
    { "en": "Apa Itu 'Optimization' (Optimasi Jaringan)?", "id": "Peningkatan Kualitas Layanan Secara Teknis." },
    { "en": "Apa Itu 'PUIL' (Persyaratan Umum Instalasi Listrik)?", "id": "Standar Utama Instalasi Listrik Indonesia." },
    { "en": "Apa Itu 'SNI' (Standar Nasional Indonesia) Listrik?", "id": "Standar Keamanan Perangkat Listrik Nasional." },
    { "en": "Apa Itu 'KWH Meter' (Kilowatt Hour Meter)?", "id": "Alat Ukur Konsumsi Energi Listrik." },
    { "en": "Apa Itu 'MCB' (Miniature Circuit Breaker)?", "id": "Pemutus Arus Saat Beban Lebih." },
    { "en": "Apa Itu 'RCBO' (Residual Current Breaker Overcurrent)?", "id": "Pelindung Hubung Singkat Dan Bocor." },
    { "en": "Apa Itu 'APP' (Alat Pembatas Dan Pengukur)?", "id": "Panel KWH Meter Milik PLN." },
    { "en": "Apa Itu 'Grounding' (Pentanahan Rumah)?", "id": "Jalur Pembuangan Arus Ke Bumi." },
    { "en": "Apa Itu 'NFA' (Neutral From Air)?", "id": "Kabel Udara Menuju Sambungan Rumah." },
    { "en": "Apa Itu 'Kabel NYM' (Kabel Inti Tembaga)?", "id": "Kabel Putih Untuk Instalasi Dalam." },
    { "en": "Apa Itu 'Kabel NYA' (Kabel Tunggal)?", "id": "Kabel Inti Tunggal Dengan Isolasi." },
    { "en": "Apa Itu 'Kabel NYY' (Kabel Luar)?", "id": "Kabel Hitam Untuk Tanam Tanah." },
    { "en": "Apa Itu 'Conduit' (Pipa Kabel)?", "id": "Pipa Pelindung Jalur Kabel Dinding." },
    { "en": "Apa Itu 'Junction Box' (Kotak Sambung)?", "id": "Tempat Percabangan Sambungan Kabel Listrik." },
    { "en": "Apa Itu 'Saklar' (Switch)?", "id": "Alat Penyambung Dan Pemutus Arus." },
    { "en": "Apa Itu 'Stopkontak' (Socket Outlet)?", "id": "Titik Penghubung Listrik Perangkat Portabel." },
    { "en": "Apa Itu 'Steker' (Plug)?", "id": "Pencolok Listrik Pada Ujung Kabel." },
    { "en": "Apa Itu 'Fitting' (Tempat Lampu)?", "id": "Dudukan Pemasangan Bola Lampu Listrik." },
    { "en": "Apa Itu 'Ballast' (Balas Lampu)?", "id": "Pengatur Arus Pada Lampu Tabung." },
    { "en": "Apa Itu 'Starter' (Starter Lampu)?", "id": "Pemicu Nyala Lampu Neon Lama." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Lampu Hemat Energi Teknologi Semikonduktor." },
    { "en": "Apa Itu 'CFL' (Compact Fluorescent Lamp)?", "id": "Lampu Neon Bentuk Spiral Kompak." },
    { "en": "Apa Itu 'Inbow' (Pemasangan Dalam)?", "id": "Pemasangan Alat Listrik Dalam Dinding." },
    { "en": "Apa Itu 'Outbow' (Pemasangan Luar)?", "id": "Pemasangan Alat Listrik Permukaan Dinding." },
    { "en": "Apa Itu 'T-Dus' (Kotak Sambung T)?", "id": "Kotak Sambung Tiga Jalur Kabel." },
    { "en": "Apa Itu 'Cross-Dus' (Kotak Sambung Silang)?", "id": "Kotak Sambung Empat Jalur Kabel." },
    { "en": "Apa Itu 'Lasdop' (Penyambung Kabel)?", "id": "Penutup Putar Isolasi Sambungan Kabel." },
    { "en": "Apa Itu 'Tespen' (Test Pen)?", "id": "Alat Cek Keberadaan Tegangan Listrik." },
    { "en": "Apa Itu 'Multimeter' (Alat Ukur Listrik)?", "id": "Alat Ukur Tegangan Arus Hambatan." },
    { "en": "Apa Itu 'Insulation Tester' (Megger)?", "id": "Alat Ukur Ketahanan Isolasi Kabel." },
    { "en": "Apa Itu 'Earth Tester' (Alat Ukur Arde)?", "id": "Alat Ukur Nilai Resistansi Tanah." },
    { "en": "Apa Itu 'Tang Ampere' (Clamp Meter)?", "id": "Alat Ukur Arus Tanpa Putus." },
    { "en": "Apa Itu 'Phasa' (Kabel Fase)?", "id": "Kabel Bertegangan Sumber Listrik Utama." },
    { "en": "Apa Itu 'Netral' (Kabel Netral)?", "id": "Kabel Pengembali Arus Tanpa Tegangan." },
    { "en": "Apa Itu 'Arde' (Kabel Arde)?", "id": "Kabel Pengaman Pembuangan Arus Bocor." },
    { "en": "Apa Itu 'Korsleting' (Hubung Singkat)?", "id": "Pertemuan Langsung Kabel Fase Netral." },
    { "en": "Apa Itu 'Overload' (Beban Lebih)?", "id": "Pemakaian Daya Melebihi Kapasitas Kabel." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Penurunan Tegangan Akibat Jarak Kabel." },
    { "en": "Apa Itu 'Ampacity' (Kuat Hantar Arus)?", "id": "Batas Maksimal Arus Kabel Aman." },
    { "en": "Apa Itu 'KHA' (Kuat Hantar Arus)?", "id": "Kemampuan Kabel Mengalirkan Arus Listrik." },
    { "en": "Apa Itu 'Daya Listrik' (Electric Power)?", "id": "Besar Energi Listrik Yang Digunakan." },
    { "en": "Apa Itu 'VA' (Volt Ampere)?", "id": "Satuan Daya Semu Instalasi Listrik." },
    { "en": "Apa Itu 'Watt' (Satuan Daya Aktif)?", "id": "Satuan Daya Listrik Yang Nyata." },
    { "en": "Apa Itu 'Faktor Daya' (Power Factor)?", "id": "Perbandingan Antara Daya Nyata Semu." },
    { "en": "Apa Itu 'Cos Phi' (Faktor Daya)?", "id": "Nilai Efisiensi Penggunaan Daya Listrik." },
    { "en": "Apa Itu 'Kapasitor' (Capacitor)?", "id": "Komponen Penyeimbang Daya Reaktif Motor." },
    { "en": "Apa Itu 'Stabilizer' (Penstabil Tegangan)?", "id": "Alat Penjaga Tegangan Listrik Tetap." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply)?", "id": "Catu Daya Cadangan Saat Padam." },
    { "en": "Apa Itu 'Genset' (Generator Set)?", "id": "Mesin Pembangkit Listrik Cadangan Mandiri." },
    { "en": "Apa Itu 'ATS' (Automatic Transfer Switch)?", "id": "Saklar Pindah Sumber Daya Otomatis." },
    { "en": "Apa Itu 'AMF' (Automatic Main Failure)?", "id": "Sistem Menyalakan Genset Secara Otomatis." },
    { "en": "Apa Itu 'Panel Hubung Bagi' (PHB)?", "id": "Pusat Pembagian Grup Sirkuit Rumah." },
    { "en": "Apa Itu 'Box Sigring' (Fuse Box)?", "id": "Kotak Tempat Pemasangan Sekering Lama." },
    { "en": "Apa Itu 'Sekering' (Fuse)?", "id": "Pengaman Listrik Menggunakan Kawat Lebur." },
    { "en": "Apa Itu 'Patron' (Inti Sekering)?", "id": "Bagian Dalam Sekering Yang Diganti." },
    { "en": "Apa Itu 'Diagram Garis Tunggal' (Single Line Diagram)?", "id": "Gambar Sederhana Rangkaian Instalasi Listrik." },
    { "en": "Apa Itu 'Gambar Situasi' (Situation Map)?", "id": "Letak Bangunan Terhadap Jaringan Listrik." },
    { "en": "Apa Itu 'Gambar Tata Letak' (Layout Map)?", "id": "Posisi Penempatan Komponen Listrik Rumah." },
    { "en": "Apa Itu 'Saklar Tunggal' (Single Switch)?", "id": "Saklar Pengendali Satu Lampu Saja." },
    { "en": "Apa Itu 'Saklar Seri' (Double Switch)?", "id": "Saklar Pengendali Dua Lampu Terpisah." },
    { "en": "Apa Itu 'Saklar Tukar' (Two-Way Switch)?", "id": "Saklar Pengendali Satu Lampu Dua." },
    { "en": "Apa Itu 'Saklar Silang' (Intermediate Switch)?", "id": "Saklar Pengendali Lampu Tiga Tempat." },
    { "en": "Apa Itu 'Saklar Hotel' (Two-Way System)?", "id": "Sistem Kendali Lampu Tangga Praktis." },
    { "en": "Apa Itu 'Dimmer' (Pengatur Cahaya)?", "id": "Alat Pengatur Terang Redup Lampu." },
    { "en": "Apa Itu 'Sensor Cahaya' (Photo Cell)?", "id": "Saklar Otomatis Berdasarkan Terang Gelap." },
    { "en": "Apa Itu 'Sensor Gerak' (Motion Sensor)?", "id": "Saklar Otomatis Berdasarkan Gerakan Objek." },
    { "en": "Apa Itu 'Smart Switch' (Saklar Pintar)?", "id": "Saklar Kendali Jarak Jauh Internet." },
    { "en": "Apa Itu 'Smart Plug' (Steker Pintar)?", "id": "Steker Kendali Aplikasi Ponsel Pintar." },
    { "en": "Apa Itu 'Ground Rod' (Batang Arde)?", "id": "Batang Logam Yang Ditanam Tanah." },
    { "en": "Apa Itu 'Terminal Arde' (Grounding Terminal)?", "id": "Titik Kumpul Kabel Arde Panel." },
    { "en": "Apa Itu 'Elektroda Bumi' (Earth Electrode)?", "id": "Bagian Logam Penghantar Arus Tanah." },
    { "en": "Apa Itu 'Resistansi Tanah' (Soil Resistance)?", "id": "Nilai Hambatan Aliran Arus Tanah." },
    { "en": "Apa Itu 'Arus Bocor' (Leakage Current)?", "id": "Aliran Listrik Keluar Jalur Normal." },
    { "en": "Apa Itu 'Sentuh Langsung' (Direct Contact)?", "id": "Menyentuh Bagian Bertegangan Secara Sengaja." },
    { "en": "Apa Itu 'Sentuh Tak Langsung' (Indirect Contact)?", "id": "Menyentuh Bodi Alat Yang Bocor." },
    { "en": "Apa Itu 'Isolasi Kabel' (Cable Insulation)?", "id": "Lapisan Pelindung Kabel Dari Sentuhan." },
    { "en": "Apa Itu 'Selongsong Kabel' (Cable Sleeve)?", "id": "Pelindung Tambahan Sambungan Kabel Listrik." },
    { "en": "Apa Itu 'Skun Kabel' (Cable Lug)?", "id": "Penyambung Ujung Kabel Ke Terminal." },
    { "en": "Apa Itu 'Ties' (Pengikat Kabel)?", "id": "Tali Plastik Pengatur Kerapian Kabel." },
    { "en": "Apa Itu 'Klem' (Cable Clip)?", "id": "Penjepit Kabel Pada Permukaan Dinding." },
    { "en": "Apa Itu 'Tang Kombinasi' (Combination Pliers)?", "id": "Tang Serbaguna Memotong Memegang Menarik." },
    { "en": "Apa Itu 'Tang Potong' (Diagonal Pliers)?", "id": "Alat Khusus Memotong Kabel Listrik." },
    { "en": "Apa Itu 'Tang Lancip' (Long Nose Pliers)?", "id": "Tang Menjangkau Tempat Sempit Dalam." },
    { "en": "Apa Itu 'Tang Kupas' (Wire Stripper)?", "id": "Alat Pengelupas Isolasi Kabel Cepat." },
    { "en": "Apa Itu 'Obeng Plus' (Phillips Screwdriver)?", "id": "Alat Pemutar Sekrup Kepala Silang." },
    { "en": "Apa Itu 'Obeng Minus' (Slotted Screwdriver)?", "id": "Alat Pemutar Sekrup Kepala Datar." },
    { "en": "Apa Itu 'Solder' (Soldering Iron)?", "id": "Alat Pemanas Penyambung Logam Elektronika." },
    { "en": "Apa Itu 'Timah Solder' (Solder Wire)?", "id": "Logam Lunak Pengikat Sambungan Kabel." },
    { "en": "Apa Itu 'Isolasi Hitam' (Electrical Tape)?", "id": "Pita Perekat Pelindung Sambungan Kabel." },
    { "en": "Apa Itu 'Heat Shrink' (Selongsong Bakar)?", "id": "Isolator Kabel Yang Menyusut Dipanaskan." },
    { "en": "Apa Itu 'Arde Rumah' (Residential Grounding)?", "id": "Sistem Keamanan Listrik Lingkup Rumah." },
    { "en": "Apa Itu 'Instalasi 1 Fase' (Single Phase)?", "id": "Sistem Listrik Dengan Dua Kabel." },
    { "en": "Apa Itu 'Instalasi 3 Fase' (Three Phase)?", "id": "Sistem Listrik Dengan Empat Kabel." },
    { "en": "Apa Itu 'Pembagian Grup' (Circuit Grouping)?", "id": "Pemisahan Beban Listrik Sesuai Ruangan." },
    { "en": "Apa Itu 'Beban Induktif' (Inductive Load)?", "id": "Beban Listrik Berupa Kumparan Motor." },
    { "en": "Apa Itu 'Beban Resistif' (Resistive Load)?", "id": "Beban Listrik Berupa Panas Lampu." },
    { "en": "Apa Itu 'Beban Kapasitif' (Capacitive Load)?", "id": "Beban Listrik Berupa Kapasitor Penyeimbang." },
    { "en": "Apa Itu 'Surge Arrester' (Penangkap Surja)?", "id": "Pelindung Perangkat Dari Lonjakan Tegangan." },
    { "en": "Apa Itu 'Penangkal Petir' (Lightning Protection)?", "id": "Sistem Penyalur Petir Langsung Bumi." },
    { "en": "Apa Itu 'SLO' (Sertifikat Laik Operasi)?", "id": "Bukti Instalasi Aman Digunakan Resmi." },
    { "en": "Apa Itu 'NIDI' (Nomor Identitas Instalasi)?", "id": "Nomor Unik Pendataan Instalasi Listrik." },
    { "en": "Apa Itu 'RE' (Renewable Energy)?", "id": "Energi Dari Sumber Alam Terbarukan." },
    { "en": "Apa Itu 'PV' (Photovoltaic)?", "id": "Sel Pengubah Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'Solar Cell' (Sel Surya)?", "id": "Komponen Utama Pembangkit Listrik Matahari." },
    { "en": "Apa Itu 'Inverter' (Pembalik Arus) Surya?", "id": "Pengubah Arus Searah Menjadi Bolak-Balik." },
    { "en": "Apa Itu 'MPPT' (Maximum Power Point Tracking)?", "id": "Optimasi Pengambilan Daya Panel Surya." },
    { "en": "Apa Itu 'Grid-Tied' (Terhubung Jaringan)?", "id": "Sistem Terkoneksi Langsung Jaringan PLN." },
    { "en": "Apa Itu 'Off-Grid' (Luar Jaringan)?", "id": "Sistem Listrik Mandiri Tanpa PLN." },
    { "en": "Apa Itu 'Hybrid System' (Sistem Hibrida)?", "id": "Gabungan Dua Sumber Energi Berbeda." },
    { "en": "Apa Itu 'Net Metering' (Meteran Netto)?", "id": "Ekspor Impor Listrik Antar Konsumen." },
    { "en": "Apa Itu 'Solar Irradiance' (Iradiansi Matahari)?", "id": "Besar Daya Cahaya Per Satuan." },
    { "en": "Apa Itu 'Monocrystalline' (Monokristalin)?", "id": "Panel Surya Efisiensi Tinggi Silikon." },
    { "en": "Apa Itu 'Polycrystalline' (Polikristalin)?", "id": "Panel Surya Silikon Harga Ekonomis." },
    { "en": "Apa Itu 'Thin Film' (Lapisan Tipis)?", "id": "Sel Surya Fleksibel Lapisan Tipis." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi Panel)?", "id": "Rasio Cahaya Menjadi Energi Listrik." },
    { "en": "Apa Itu 'SCC' (Solar Charge Controller)?", "id": "Pengatur Pengisian Baterai Tenaga Surya." },
    { "en": "Apa Itu 'Wind Turbine' (Turbin Angin)?", "id": "Pengubah Energi Angin Menjadi Listrik." },
    { "en": "Apa Itu 'HAWT' (Horizontal Axis Wind Turbine)?", "id": "Turbin Angin Dengan Poros Horizontal." },
    { "en": "Apa Itu 'VAWT' (Vertical Axis Wind Turbine)?", "id": "Turbin Angin Dengan Poros Vertikal." },
    { "en": "Apa Itu 'Nacelle' (Nasel Turbin)?", "id": "Rumah Komponen Utama Atas Turbin." },
    { "en": "Apa Itu 'Anemometer' (Anemometer)?", "id": "Alat Pengukur Kecepatan Angin Lokasi." },
    { "en": "Apa Itu 'Cut-In Speed' (Kecepatan Mulai)?", "id": "Angin Minimal Untuk Memutar Turbin." },
    { "en": "Apa Itu 'Cut-Out Speed' (Kecepatan Berhenti)?", "id": "Angin Maksimal Untuk Keamanan Turbin." },
    { "en": "Apa Itu 'Wind Farm' (Ladang Angin)?", "id": "Kumpulan Turbin Angin Area Luas." },
    { "en": "Apa Itu 'Offshore Wind' (Angin Lepas Pantai)?", "id": "Pembangkit Angin Di Tengah Laut." },
    { "en": "Apa Itu 'Hydroelectric' (Pembangkit Hidroelektrik)?", "id": "Energi Listrik Dari Aliran Air." },
    { "en": "Apa Itu 'PLTA' (Pembangkit Listrik Tenaga Air)?", "id": "Pembangkit Listrik Kapasitas Besar Air." },
    { "en": "Apa Itu 'Micro-Hydro' (Mikrohidro)?", "id": "Pembangkit Listrik Air Skala Kecil." },
    { "en": "Apa Itu 'Penstock' (Pipa Pesat)?", "id": "Pipa Penyalur Air Menuju Turbin." },
    { "en": "Apa Itu 'Draft Tube' (Pipa Lepas)?", "id": "Saluran Keluar Air Setelah Turbin." },
    { "en": "Apa Itu 'Head' (Tinggi Terjun)?", "id": "Beda Ketinggian Air Sumber Turbin." },
    { "en": "Apa Itu 'Pelton Turbine' (Turbin Pelton)?", "id": "Turbin Air Tekanan Tinggi Impulse." },
    { "en": "Apa Itu 'Francis Turbine' (Turbin Francis)?", "id": "Turbin Air Paling Umum Digunakan." },
    { "en": "Apa Itu 'Kaplan Turbine' (Turbin Kaplan)?", "id": "Turbin Air Untuk Head Rendah." },
    { "en": "Apa Itu 'Pumped Storage' (Penyimpanan Pompa)?", "id": "Penyimpanan Energi Melalui Pemompaan Air." },
    { "en": "Apa Itu 'Geothermal' (Energi Panas Bumi)?", "id": "Panas Bumi Sebagai Sumber Listrik." },
    { "en": "Apa Itu 'PLTP' (Pembangkit Listrik Tenaga Panas Bumi)?", "id": "Pembangkit Menggunakan Uap Panas Bumi." },
    { "en": "Apa Itu 'Dry Steam' (Uap Kering)?", "id": "Teknologi Panas Bumi Uap Langsung." },
    { "en": "Apa Itu 'Flash Steam' (Uap Flash)?", "id": "Penguapan Cairan Panas Tekanan Tinggi." },
    { "en": "Apa Itu 'Binary Cycle' (Siklus Biner)?", "id": "Pemanfaatan Cairan Sekunder Titik Didih." },
    { "en": "Apa Itu 'Biomass' (Biomassa)?", "id": "Bahan Organik Sebagai Sumber Energi." },
    { "en": "Apa Itu 'Biofuel' (Bahan Bakar Nabati)?", "id": "Bahan Bakar Dari Materi Tumbuhan." },
    { "en": "Apa Itu 'Biogas' (Biogas)?", "id": "Gas Metana Dari Fermentasi Limbah." },
    { "en": "Apa Itu 'Gasification' (Gasifikasi)?", "id": "Proses Pengubahan Biomassa Menjadi Gas." },
    { "en": "Apa Itu 'Co-Firing' (Pembakaran Bersama)?", "id": "Pembakaran Campuran Batubara Dan Biomassa." },
    { "en": "Apa Itu 'Tidal Energy' (Energi Pasang Surut)?", "id": "Listrik Dari Pergerakan Air Laut." },
    { "en": "Apa Itu 'Wave Energy' (Energi Gelombang)?", "id": "Listrik Dari Gerakan Ombak Laut." },
    { "en": "Apa Itu 'OTEC' (Ocean Thermal Energy Conversion)?", "id": "Listrik Dari Perbedaan Suhu Laut." },
    { "en": "Apa Itu 'Hydrogen Fuel Cell' (Sel Bahan Bakar)?", "id": "Reaksi Kimia Hidrogen Menjadi Listrik." },
    { "en": "Apa Itu 'Electrolyzer' (Elektroliser)?", "id": "Alat Pemisah Air Menjadi Hidrogen." },
    { "en": "Apa Itu 'Green Hydrogen' (Hidrogen Hijau)?", "id": "Hidrogen Dari Energi Terbarukan Murni." },
    { "en": "Apa Itu 'Blue Hydrogen' (Hidrogen Biru)?", "id": "Hidrogen Gas Alam Dengan Karbon." },
    { "en": "Apa Itu 'Energy Storage' (Penyimpanan Energi)?", "id": "Sistem Cadangan Energi Listrik Terpadu." },
    { "en": "Apa Itu 'BESS' (Battery Energy Storage System)?", "id": "Penyimpanan Energi Menggunakan Paket Baterai." },
    { "en": "Apa Itu 'Li-Ion' (Lithium Ion)?", "id": "Teknologi Baterai Kerapatan Energi Tinggi." },
    { "en": "Apa Itu 'LiFePO4' (Lithium Iron Phosphate)?", "id": "Baterai Litium Keamanan Dan Umur." },
    { "en": "Apa Itu 'Flow Battery' (Baterai Alir)?", "id": "Penyimpanan Energi Melalui Cairan Elektrolit." },
    { "en": "Apa Itu 'Flywheel' (Roda Gila)?", "id": "Penyimpanan Energi Kinetik Putaran Cepat." },
    { "en": "Apa Itu 'Supercapacitor' (Superkapasitor)?", "id": "Penyimpanan Daya Kecepatan Isi Tinggi." },
    { "en": "Apa Itu 'Smart Grid' (Jaringan Cerdas)?", "id": "Jaringan Listrik Digital Dua Arah." },
    { "en": "Apa Itu 'Microgrid' (Jaringan Mikro)?", "id": "Sistem Energi Lokal Mandiri Terpadu." },
    { "en": "Apa Itu 'VPP' (Virtual Power Plant)?", "id": "Kumpulan Pembangkit Tersebar Terintegrasi Software." },
    { "en": "Apa Itu 'Distributed Generation' (Pembangkit Tersebar)?", "id": "Pembangkit Kecil Dekat Titik Beban." },
    { "en": "Apa Itu 'Feed-In Tariff' (Tarif Masuk)?", "id": "Kebijakan Harga Pembelian Energi Terbarukan." },
    { "en": "Apa Itu 'Carbon Footprint' (Jejak Karbon)?", "id": "Total Emisi Gas Rumah Kaca." },
    { "en": "Apa Itu 'Decarbonization' (Dekarbonisasi)?", "id": "Proses Pengurangan Emisi Karbon Global." },
    { "en": "Apa Itu 'Net Zero' (Emisi Nol Bersih)?", "id": "Keseimbangan Emisi Dan Penyerapan Karbon." },
    { "en": "Apa Itu 'Energy Efficiency' (Efisiensi Energi)?", "id": "Penggunaan Energi Lebih Sedikit Hasil." },
    { "en": "Apa Itu 'Demand Response' (Respon Permintaan)?", "id": "Penyesuaian Konsumsi Beban Waktu Puncak." },
    { "en": "Apa Itu 'Power Quality' (Kualitas Daya)?", "id": "Kestabilan Tegangan Arus Sumber Listrik." },
    { "en": "Apa Itu 'Harmonics' (Harmonisa)?", "id": "Gangguan Gelombang Akibat Beban Elektronik." },
    { "en": "Apa Itu 'Voltage Sag' (Tegangan Kedip)?", "id": "Penurunan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Voltage Swell' (Tegangan Lonjak)?", "id": "Kenaikan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Frequency Control' (Kendali Frekuensi)?", "id": "Penjaga Keseimbangan Daya Dan Beban." },
    { "en": "Apa Itu 'Inertia' (Inersia Sistem)?", "id": "Kemampuan Melawan Perubahan Frekuensi Mendadak." },
    { "en": "Apa Itu 'Island Mode' (Mode Berpulau)?", "id": "Operasi Mandiri Tanpa Grid Utama." },
    { "en": "Apa Itu 'Black Start' (Mulai Gelap)?", "id": "Menyalakan Pembangkit Tanpa Sumber Luar." },
    { "en": "Apa Itu 'Synchrophasor' (Sinkrofasor)?", "id": "Pengukuran Tegangan Berbasis Waktu Presisi." },
    { "en": "Apa Itu 'PMU' (Phasor Measurement Unit)?", "id": "Alat Ukur Fasor Real-Time Jaringan." },
    { "en": "Apa Itu 'Transmission Loss' (Rugi Transmisi)?", "id": "Energi Hilang Selama Penyaluran Jauh." },
    { "en": "Apa Itu 'Transformer' (Transformator)?", "id": "Alat Pengubah Level Tegangan Listrik." },
    { "en": "Apa Itu 'Substation' (Gardu Induk)?", "id": "Pusat Pengaturan Dan Distribusi Daya." },
    { "en": "Apa Itu 'Circuit Breaker' (Pemutus Sirkuit)?", "id": "Pengaman Arus Gangguan Otomatis Jaringan." },
    { "en": "Apa Itu 'Relay Proteksi' (Relay Proteksi)?", "id": "Alat Deteksi Gangguan Pemutus Arus." },
    { "en": "Apa Itu 'Capacitor Bank' (Bank Kapasitor)?", "id": "Kompensasi Daya Reaktif Jaringan Listrik." },
    { "en": "Apa Itu 'Load Factor' (Faktor Beban)?", "id": "Rasio Beban Rata-Rata Terhadap Puncak." },
    { "en": "Apa Itu 'Base Load' (Beban Dasar)?", "id": "Kebutuhan Minimum Listrik Kontinu Jaringan." },
    { "en": "Apa Itu 'Peak Load' (Beban Puncak)?", "id": "Konsumsi Listrik Maksimal Waktu Tertentu." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Berputar)?", "id": "Pembangkit Siap Pakai Jaga Frekuensi." },
    { "en": "Apa Itu 'HVDC' (High Voltage Direct Current)?", "id": "Transmisi Arus Searah Tegangan Tinggi." },
    { "en": "Apa Itu 'Flexible AC Transmission' (FACTS)?", "id": "Sistem Elektronik Kendali Aliran Daya." },
    { "en": "Apa Itu 'Supergrid' (Jaringan Super)?", "id": "Jaringan Transmisi Luas Antar Negara." },
    { "en": "Apa Itu 'Renewable Certificate' (Sertifikat Hijau)?", "id": "Bukti Penggunaan Energi Terbarukan Sah." },
    { "en": "Apa Itu 'Carbon Credit' (Kredit Karbon)?", "id": "Izin Mengeluarkan Jumlah Emisi Tertentu." },
    { "en": "Apa Itu 'Circular Economy' (Ekonomi Sirkular)?", "id": "Sistem Minim Limbah Maksimalkan Sumber." },
    { "en": "Apa Itu 'Life Cycle Assessment' (LCA)?", "id": "Analisis Dampak Lingkungan Produk Total." },
    { "en": "Apa Itu 'Sustainability' (Keberlanjutan)?", "id": "Pemenuhan Kebutuhan Tanpa Merusak Alam." },
    { "en": "Apa Itu 'Resilience' (Ketahanan Sistem)?", "id": "Kemampuan Pulih Dari Gangguan Jaringan." },
    { "en": "Apa Itu 'Automation' (Otomasi Pembangkit)?", "id": "Kendali Operasi Mesin Secara Otomatis." },
    { "en": "Apa Itu 'AI' (Artificial Intelligence)?", "id": "Kecerdasan Buatan Simulasi Proses Manusia." },
    { "en": "Apa Itu 'ML' (Machine Learning)?", "id": "Algoritma Belajar Dari Pola Data." },
    { "en": "Apa Itu 'ANN' (Artificial Neural Network)?", "id": "Jaringan Saraf Tiruan Peniru Otak." },
    { "en": "Apa Itu 'CNN' (Convolutional Neural Network)?", "id": "Jaringan Saraf Khusus Analisis Citra." },
    { "en": "Apa Itu 'RNN' (Recurrent Neural Network)?", "id": "Jaringan Saraf Untuk Data Berurutan." },
    { "en": "Apa Itu 'Deep Learning' (Pembelajaran Mendalam)?", "id": "Pembelajaran Mesin Dengan Banyak Lapisan." },
    { "en": "Apa Itu 'Supervised Learning' (Pembelajaran Terawasi)?", "id": "Belajar Menggunakan Data Berlabel Jelas." },
    { "en": "Apa Itu 'Unsupervised Learning' (Tanpa Pengawasan)?", "id": "Mencari Struktur Dalam Data Mentah." },
    { "en": "Apa Itu 'Data Set' (Kumpulan Data)?", "id": "Koleksi Informasi Untuk Pelatihan Model." },
    { "en": "Apa Itu 'Training Data' (Data Latih)?", "id": "Data Untuk Membangun Model Kecerdasan." },
    { "en": "Apa Itu 'Testing Data' (Data Uji)?", "id": "Data Untuk Mengevaluasi Akurasi Model." },
    { "en": "Apa Itu 'Weight' (Bobot Saraf)?", "id": "Nilai Kepentingan Koneksi Antar Saraf." },
    { "en": "Apa Itu 'Bias' (Bias Saraf)?", "id": "Nilai Pergeseran Aktivasi Saraf Tiruan." },
    { "en": "Apa Itu 'Activation Function' (Fungsi Aktivasi)?", "id": "Penentu Keluaran Saraf Sesuai Input." },
    { "en": "Apa Itu 'ReLU' (Rectified Linear Unit)?", "id": "Fungsi Aktivasi Non-Linier Paling Populer." },
    { "en": "Apa Itu 'Sigmoid' (Fungsi Sigmoid)?", "id": "Fungsi Aktivasi Berbentuk Kurva S." },
    { "en": "Apa Itu 'Softmax' (Fungsi Softmax)?", "id": "Fungsi Aktivasi Untuk Klasifikasi Multi-Kelas." },
    { "en": "Apa Itu 'Backpropagation' (Propagasi Balik)?", "id": "Metode Pelatihan Jaringan Saraf Tiruan." },
    { "en": "Apa Itu 'Gradient Descent' (Penurunan Gradien)?", "id": "Algoritma Optimasi Pencari Nilai Minimum." },
    { "en": "Apa Itu 'Learning Rate' (Laju Pembelajaran)?", "id": "Besar Langkah Perubahan Bobot Model." },
    { "en": "Apa Itu 'Overfitting' (Pengepasan Berlebih)?", "id": "Model Terlalu Menghafal Data Latih." },
    { "en": "Apa Itu 'Epoch' (Epoche)?", "id": "Satu Putaran Penuh Pelatihan Data." },
    { "en": "Apa Itu 'Batch Size' (Ukuran Batch)?", "id": "Jumlah Sampel Data Dalam Satu." },
    { "en": "Apa Itu 'Loss Function' (Fungsi Kerugian)?", "id": "Ukuran Kesalahan Prediksi Model Kecerdasan." },
    { "en": "Apa Itu 'Accuracy' (Akurasi Model)?", "id": "Persentase Prediksi Benar Dari Total." },
    { "en": "Apa Itu 'Precision' (Presisi Model)?", "id": "Rasio Prediksi Positif Yang Tepat." },
    { "en": "Apa Itu 'Recall' (Recall Model)?", "id": "Rasio Kasus Positif Yang Terdeteksi." },
    { "en": "Apa Itu 'F1 Score' (Skor F1)?", "id": "Rata-Rata Harmonik Presisi Dan Recall." },
    { "en": "Apa Itu 'Feature Extraction' (Ekstraksi Fitur)?", "id": "Pengambilan Karakteristik Penting Dari Data." },
    { "en": "Apa Itu 'Label' (Label Data)?", "id": "Target Jawaban Dalam Data Terawasi." },
    { "en": "Apa Itu 'Clustering' (Klasterisasi)?", "id": "Pengelompokan Data Tanpa Label Jelas." },
    { "en": "Apa Itu 'K-Nearest Neighbors' (K-NN)?", "id": "Klasifikasi Berdasarkan Kedekatan Jarak Data." },
    { "en": "Apa Itu 'Support Vector Machine' (SVM)?", "id": "Pemisah Data Menggunakan Bidang Hiper." },
    { "en": "Apa Itu 'Computer Vision' (Visi Komputer)?", "id": "Analisis Informasi Visual Melalui Algoritma." },
    { "en": "Apa Itu 'Edge AI' (Kecerdasan Tepi)?", "id": "Pemrosesan Kecerdasan Langsung Pada Perangkat." },
    { "en": "Apa Itu 'Hyperparameter' (Hiperparameter)?", "id": "Parameter Pengatur Proses Pelatihan Model." },
    { "en": "Apa Itu 'Optimization' (Optimasi)?", "id": "Proses Mencari Hasil Terbaik Model." },
    { "en": "Apa Itu 'Dropout' (Drop-out)?", "id": "Teknik Pencegah Overfitting Jaringan Saraf." },
    { "en": "Apa Itu 'Transfer Learning' (Pembelajaran Transfer)?", "id": "Penggunaan Model Terlatih Untuk Tugas." },
    { "en": "Apa Itu 'Inference' (Inferensi)?", "id": "Proses Penggunaan Model Untuk Prediksi." },
    { "en": "Apa Itu 'Big Data' (Data Besar)?", "id": "Analisis Data Volume Sangat Masif." },
    { "en": "Apa Itu 'Predictive Maintenance' (Pemeliharaan Prediktif)?", "id": "Prediksi Kerusakan Mesin Menggunakan AI." },
    { "en": "Apa Itu 'Smart Grid AI' (Kecerdasan Jaringan)?", "id": "Optimasi Jaringan Listrik Melalui AI." },
    { "en": "Apa Itu 'Load Forecasting' (Peramalan Beban)?", "id": "Prediksi Kebutuhan Listrik Masa Depan." },
    { "en": "Apa Itu 'Anomaly Detection' (Deteksi Anomali)?", "id": "Identifikasi Pola Tidak Lazim Data." },
    { "en": "Apa Itu 'Fuzzy Logic' (Logika Kabur)?", "id": "Sistem Kendali Berbasis Derajat Kebenaran." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Model Virtual Real-Time Sistem Fisik." },
    { "en": "Apa Itu 'MPC' (Model Predictive Control)?", "id": "Kendali Berdasarkan Prediksi Respon Sistem." },
    { "en": "Apa Itu 'Heuristics' (Heuristik)?", "id": "Strategi Pemecahan Masalah Secara Praktis." },
    { "en": "Apa Itu 'Algorithm' (Algoritma)?", "id": "Langkah Logis Selesaikan Masalah Tertentu." },
    { "en": "Apa Itu 'Turing Test' (Uji Turing)?", "id": "Uji Kemampuan Mesin Meniru Manusia." },
    { "en": "Apa Itu 'Natural Language Generation' (NLG)?", "id": "Pembuatan Teks Bahasa Manusia Otomatis." },
    { "en": "Apa Itu 'Speech Recognition' (Pengenalan Suara)?", "id": "Konversi Suara Menjadi Teks Digital." },
    { "en": "Apa Itu 'Generative AI' (AI Generatif)?", "id": "Kecerdasan Penghasil Konten Baru Unik." },
    { "en": "Apa Itu 'LLM' (Large Language Model)?", "id": "Model Bahasa Besar Berbasis Transformer." },
    { "en": "Apa Itu 'Transformer' (Arsitektur Transformer)?", "id": "Model Saraf Berbasis Mekanisme Atensi." },
    { "en": "Apa Itu 'Attention Mechanism' (Mekanisme Atensi)?", "id": "Fokus Pada Bagian Data Penting." },
    { "en": "Apa Itu 'Bias In AI' (Bias AI)?", "id": "Ketidakadilan Hasil Akibat Data Miring." },
    { "en": "Apa Itu 'Explainable AI' (XAI)?", "id": "Kecerdasan Yang Dapat Dijelaskan Logikanya." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Sumber Sensor." },
    { "en": "Apa Itu 'Robotics' (Robotika)?", "id": "Integrasi Mekanik Elektronika Dan AI." },
    { "en": "Apa Itu 'Autonomous Vehicle' (Kendaraan Otonom)?", "id": "Mobil Yang Berjalan Tanpa Supir." },
    { "en": "Apa Itu 'Perceptron' (Perseptron Dasar)?", "id": "Model Jaringan Saraf Paling Sederhana." },
    { "en": "Apa Itu 'Multi-Layer Perceptron' (MLP)?", "id": "Jaringan Saraf Dengan Lapisan Tersembunyi." },
    { "en": "Apa Itu 'Hidden Layer' (Lapisan Tersembunyi)?", "id": "Lapisan Antara Input Dan Output." },
    { "en": "Apa Itu 'Weight Initialization' (Inisialisasi Bobot)?", "id": "Pemberian Nilai Awal Bobot Saraf." },
    { "en": "Apa Itu 'Normalization' (Normalisasi Data)?", "id": "Penyeragaman Rentang Nilai Data Masuk." },
    { "en": "Apa Itu 'Standardization' (Standarisasi Data)?", "id": "Pengubahan Data Menjadi Distribusi Normal." },
    { "en": "Apa Itu 'One-Hot Encoding' (Pengkodean Satu)?", "id": "Representasi Data Kategorikal Menjadi Biner." },
    { "en": "Apa Itu 'PCA' (Principal Component Analysis)?", "id": "Analisis Komponen Utama Reduksi Data." },
    { "en": "Apa Itu 'Cross Validation' (Validasi Silang)?", "id": "Teknik Pengujian Akurasi Model Berulang." },
    { "en": "Apa Itu 'K-Fold' (Validasi K-Fold)?", "id": "Pembagian Data Menjadi K Bagian." },
    { "en": "Apa Itu 'Stochastic Gradient Descent' (SGD)?", "id": "Optimasi Gradien Secara Acak Cepat." },
    { "en": "Apa Itu 'Adam Optimizer' (Pengoptimal Adam)?", "id": "Algoritma Optimasi Laju Pembelajaran Adaptif." },
    { "en": "Apa Itu 'Momentum' (Momentum Optimasi)?", "id": "Teknik Mempercepat Penurunan Gradien Stabil." },
    { "en": "Apa Itu 'Early Stopping' (Penghentian Dini)?", "id": "Penghentian Latih Sebelum Terjadi Overfitting." },
    { "en": "Apa Itu 'Hyperparameter Tuning' (Penyetelan Parameter)?", "id": "Pencarian Kombinasi Parameter Model Terbaik." },
    { "en": "Apa Itu 'Data Augmentation' (Augmentasi Data)?", "id": "Penambahan Variasi Data Secara Buatan." },
    { "en": "Apa Itu 'Semantic Segmentation' (Segmentasi Semantik)?", "id": "Klasifikasi Setiap Piksel Dalam Gambar." },
    { "en": "Apa Itu 'Object Detection' (Deteksi Objek)?", "id": "Lokalisasi Dan Klasifikasi Benda Gambar." },
    { "en": "Apa Itu 'Bounding Box' (Kotak Pembatas)?", "id": "Area Kotak Penanda Lokasi Objek." },
    { "en": "Apa Itu 'Sentiment Analysis' (Analisis Sentimen)?", "id": "Identifikasi Emosi Dalam Teks Digital." },
    { "en": "Apa Itu 'Chatbot' (Robot Percakapan)?", "id": "Program Simulasi Percakapan Dengan Manusia." },
    { "en": "Apa Itu 'Recommendation System' (Sistem Rekomendasi)?", "id": "Saran Produk Berdasarkan Preferensi Pengguna." },
    { "en": "Apa Itu 'Expert System' (Sistem Pakar)?", "id": "Program Peniru Keputusan Pakar Manusia." },
    { "en": "Apa Itu 'Bio-Inspired AI' (AI Inspirasi Bio)?", "id": "Kecerdasan Berdasarkan Sistem Alam Hayati." },
    { "en": "Apa Itu 'Cybersecurity AI' (AI Keamanan Siber)?", "id": "AI Untuk Deteksi Serangan Digital." },
    { "en": "Apa Itu 'Energy Management AI' (Manajemen Energi)?", "id": "Efisiensi Penggunaan Energi Melalui AI." },
    { "en": "Apa Itu 'Smart Sensor AI' (Sensor Pintar)?", "id": "Sensor Dengan Analisis Data Internal." },
    { "en": "Apa Itu 'Robotic Process Automation' (RPA)?", "id": "Otomasi Tugas Rutin Melalui Software." },
    { "en": "Apa Itu 'FACTS' (Flexible AC Transmission Systems)?", "id": "Sistem Elektronik Kendali Aliran Daya." },
    { "en": "Apa Itu 'STATCOM' (Static Synchronous Compensator)?", "id": "Penstabil Tegangan Berbasis Inverter Cerdas." },
    { "en": "Apa Itu 'SVC' (Static Var Compensator)?", "id": "Kompensator Daya Reaktif Cepat Industri." },
    { "en": "Apa Itu 'UPFC' (Unified Power Flow Controller)?", "id": "Pengatur Aliran Daya Terpadu Jaringan." },
    { "en": "Apa Itu 'PMU' (Phasor Measurement Unit)?", "id": "Alat Ukur Fasor Waktu Nyata." },
    { "en": "Apa Itu 'WAMS' (Wide Area Monitoring System)?", "id": "Sistem Monitoring Wilayah Luas Digital." },
    { "en": "Apa Itu 'PDC' (Phasor Data Concentrator)?", "id": "Pusat Pengumpul Data Fasor Jaringan." },
    { "en": "Apa Itu 'AGC' (Automatic Generation Control)?", "id": "Kendali Output Pembangkit Secara Otomatis." },
    { "en": "Apa Itu 'LFC' (Load Frequency Control)?", "id": "Kendali Keseimbangan Beban Dan Frekuensi." },
    { "en": "Apa Itu 'Transient Stability' (Stabilitas Transien)?", "id": "Ketahanan Sistem Terhadap Gangguan Besar." },
    { "en": "Apa Itu 'Small Signal Stability' (Stabilitas Sinyal Kecil)?", "id": "Ketahanan Terhadap Osilasi Gangguan Kecil." },
    { "en": "Apa Itu 'Voltage Stability' (Stabilitas Tegangan)?", "id": "Kemampuan Mempertahankan Level Tegangan Nominal." },
    { "en": "Apa Itu 'Frequency Stability' (Stabilitas Frekuensi)?", "id": "Kemampuan Menjaga Frekuensi Tetap Stabil." },
    { "en": "Apa Itu 'N-1 Criterion' (Kriteria N-1)?", "id": "Ketahanan Sistem Meski Satu Komponen." },
    { "en": "Apa Itu 'Contingency Analysis' (Analisis Kontinjensi)?", "id": "Studi Dampak Kegagalan Komponen Jaringan." },
    { "en": "Apa Itu 'Load Flow' (Aliran Beban)?", "id": "Analisis Distribusi Daya Listrik Jaringan." },
    { "en": "Apa Itu 'Newton-Raphson Method' (Metode Newton-Raphson)?", "id": "Algoritma Numerik Solusi Aliran Daya." },
    { "en": "Apa Itu 'Gauss-Seidel Method' (Metode Gauss-Seidel)?", "id": "Metode Iteratif Hitung Aliran Daya." },
    { "en": "Apa Itu 'Optimal Power Flow' (OPF)?", "id": "Aliran Daya Dengan Biaya Minimal." },
    { "en": "Apa Itu 'Power System Stabilizer' (PSS)?", "id": "Alat Peredam Osilasi Daya Generator." },
    { "en": "Apa Itu 'AVR' (Automatic Voltage Regulator) Generator?", "id": "Pengatur Tegangan Terminal Generator Otomatis." },
    { "en": "Apa Itu 'Governor' (Gubernur Turbin)?", "id": "Pengatur Kecepatan Putar Turbin Mekanik." },
    { "en": "Apa Itu 'Droop Control' (Kendali Droop)?", "id": "Pengaturan Pembagian Beban Antar Pembangkit." },
    { "en": "Apa Itu 'Black Start' (Mulai Gelap)?", "id": "Pemulihan Pembangkit Tanpa Daya Luar." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Berputar)?", "id": "Kapasitas Pembangkit Siap Pakai Segera." },
    { "en": "Apa Itu 'Operating Reserve' (Cadangan Operasi)?", "id": "Total Daya Cadangan Jaga Keandalan." },
    { "en": "Apa Itu 'Unit Commitment' (Komitmen Unit)?", "id": "Penjadwalan Pengoperasian Unit Pembangkit Listrik." },
    { "en": "Apa Itu 'Economic Dispatch' (Pengiriman Ekonomis)?", "id": "Alokasi Beban Dengan Biaya Terendah." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control And Data Acquisition)?", "id": "Sistem Pengawasan Dan Kendali Jarak." },
    { "en": "Apa Itu 'EMS' (Energy Management System)?", "id": "Sistem Kontrol Operasi Jaringan Transmisi." },
    { "en": "Apa Itu 'DMS' (Distribution Management System)?", "id": "Sistem Kontrol Operasi Jaringan Distribusi." },
    { "en": "Apa Itu 'ADMS' (Advanced DMS)?", "id": "Manajemen Distribusi Canggih Terintegrasi Digital." },
    { "en": "Apa Itu 'Microgrid' (Jaringan Mikro)?", "id": "Sistem Energi Lokal Mandiri Terintegrasi." },
    { "en": "Apa Itu 'Islanded Operation' (Operasi Terisolasi)?", "id": "Kondisi Mikrogrid Berjalan Tanpa Grid." },
    { "en": "Apa Itu 'Grid-Forming Inverter' (Inverter Pembentuk Jaringan)?", "id": "Inverter Pengatur Tegangan Dan Frekuensi." },
    { "en": "Apa Itu 'Grid-Following Inverter' (Inverter Pengikut Jaringan)?", "id": "Inverter Penyesuai Fase Tegangan Jaringan." },
    { "en": "Apa Itu 'Inertia Emulation' (Emulasi Inersia)?", "id": "Simulasi Inersia Mekanik Melalui Inverter." },
    { "en": "Apa Itu 'VPP' (Virtual Power Plant)?", "id": "Kumpulan Pembangkit Terintegrasi Secara Digital." },
    { "en": "Apa Itu 'DER' (Distributed Energy Resources)?", "id": "Sumber Energi Kapasitas Kecil Tersebar." },
    { "en": "Apa Itu 'Demand Response' (Respon Permintaan)?", "id": "Penyesuaian Beban Berdasarkan Sinyal Harga." },
    { "en": "Apa Itu 'Load Shedding' (Pelepasan Beban)?", "id": "Pemutusan Beban Darurat Jaga Frekuensi." },
    { "en": "Apa Itu 'Under Frequency Relay' (UFR)?", "id": "Relay Pemutus Beban Saat Frekuensi." },
    { "en": "Apa Itu 'Fault Analysis' (Analisis Gangguan)?", "id": "Studi Karakteristik Arus Hubung Singkat." },
    { "en": "Apa Itu 'Symmetrical Component' (Komponen Simetris)?", "id": "Metode Analisis Gangguan Tidak Seimbang." },
    { "en": "Apa Itu 'Zero Sequence' (Urutan Nol)?", "id": "Komponen Arus Gangguan Ke Tanah." },
    { "en": "Apa Itu 'Positive Sequence' (Urutan Positif)?", "id": "Komponen Kondisi Kerja Normal Jaringan." },
    { "en": "Apa Itu 'Negative Sequence' (Urutan Negatif)?", "id": "Komponen Akibat Ketidakseimbangan Beban Fase." },
    { "en": "Apa Itu 'Distance Protection' (Proteksi Jarak)?", "id": "Proteksi Berdasarkan Nilai Impedansi Jalur." },
    { "en": "Apa Itu 'Differential Protection' (Proteksi Diferensial)?", "id": "Proteksi Berdasarkan Selisih Arus Masuk." },
    { "en": "Apa Itu 'Overcurrent Protection' (Proteksi Arus Lebih)?", "id": "Proteksi Saat Arus Melebihi Batas." },
    { "en": "Apa Itu 'Directional Protection' (Proteksi Berarah)?", "id": "Proteksi Berdasarkan Arah Aliran Gangguan." },
    { "en": "Apa Itu 'Busbar Protection' (Proteksi Rel)?", "id": "Sistem Pengaman Titik Kumpul Daya." },
    { "en": "Apa Itu 'Transformer Protection' (Proteksi Trafo)?", "id": "Sistem Pengaman Gangguan Internal Transformator." },
    { "en": "Apa Itu 'Generator Protection' (Proteksi Generator)?", "id": "Sistem Pengaman Gangguan Mesin Pembangkit." },
    { "en": "Apa Itu 'Line Trap' (Perangkap Jalur)?", "id": "Penyaring Sinyal Komunikasi Pada Transmisi." },
    { "en": "Apa Itu 'PLC' (Power Line Carrier)?", "id": "Komunikasi Melalui Kabel Tenaga Listrik." },
    { "en": "Apa Itu 'OPGW' (Optical Ground Wire)?", "id": "Kawat Tanah Berisi Serat Optik." },
    { "en": "Apa Itu 'Digital Substation' (Gardu Induk Digital)?", "id": "Gardu Dengan Komunikasi Data Fiber." },
    { "en": "Apa Itu 'IEC 61850' (Standar Komunikasi Gardu)?", "id": "Protokol Standar Internasional Otomasi Gardu." },
    { "en": "Apa Itu 'GOOSE' (Generic Object Oriented Substation Event)?", "id": "Pesan Cepat Antar Perangkat Proteksi." },
    { "en": "Apa Itu 'Sampled Values' (Nilai Sampel)?", "id": "Data Digital Tegangan Arus Real-Time." },
    { "en": "Apa Itu 'Merging Unit' (Unit Penggabung)?", "id": "Alat Konversi Analog Ke Digital." },
    { "en": "Apa Itu 'Intelligent Electronic Device' (IED)?", "id": "Relay Proteksi Digital Multifungsi Pintar." },
    { "en": "Apa Itu 'State Estimation' (Estimasi Keadaan)?", "id": "Perhitungan Kondisi Jaringan Berdasarkan Pengukuran." },
    { "en": "Apa Itu 'Dynamic Security Assessment' (DSA)?", "id": "Evaluasi Keamanan Jaringan Secara Dinamis." },
    { "en": "Apa Itu 'Voltage Collapse' (Runtuh Tegangan)?", "id": "Kegagalan Sistem Mempertahankan Tegangan Stabil." },
    { "en": "Apa Itu 'Angular Stability' (Stabilitas Sudut)?", "id": "Kemampuan Generator Tetap Sinkron Jaringan." },
    { "en": "Apa Itu 'Phase Shifting Transformer' (PST)?", "id": "Pengatur Aliran Daya Melalui Fase." },
    { "en": "Apa Itu 'Series Compensation' (Kompensasi Seri)?", "id": "Kapasitor Seri Penurun Impedansi Transmisi." },
    { "en": "Apa Itu 'Shunt Compensation' (Kompensasi Shunt)?", "id": "Penyerap Atau Penyedia Daya Reaktif." },
    { "en": "Apa Itu 'Power Quality' (Kualitas Daya)?", "id": "Parameter Tegangan Arus Sesuai Standar." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Ukuran Total Distorsi Gelombang Listrik." },
    { "en": "Apa Itu 'Voltage Flicker' (Kedipan Tegangan)?", "id": "Variasi Tegangan Cepat Mengganggu Cahaya." },
    { "en": "Apa Itu 'BESS' (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai Besar." },
    { "en": "Apa Itu 'Flywheel Energy Storage' (FES)?", "id": "Penyimpanan Energi Berbasis Putaran Mekanik." },
    { "en": "Apa Itu 'CAES' (Compressed Air Energy Storage)?", "id": "Penyimpanan Energi Melalui Udara Bertekanan." },
    { "en": "Apa Itu 'Pumped Hydro Storage' (PHS)?", "id": "Penyimpanan Energi Melalui Pemompaan Air." },
    { "en": "Apa Itu 'Energy Density' (Kerapatan Energi)?", "id": "Jumlah Energi Per Volume Material." },
    { "en": "Apa Itu 'Power Density' (Kerapatan Daya)?", "id": "Laju Energi Per Volume Material." },
    { "en": "Apa Itu 'AMI' (Advanced Metering Infrastructure)?", "id": "Infrastruktur Jaringan Meteran Listrik Pintar." },
    { "en": "Apa Itu 'Grid Congestion' (Kemacetan Jaringan)?", "id": "Beban Transmisi Melebihi Kapasitas Aman." },
    { "en": "Apa Itu 'Ancillary Services' (Layanan Tambahan)?", "id": "Fungsi Pendukung Kestabilan Jaringan Listrik." },
    { "en": "Apa Itu 'Regulating Reserve' (Cadangan Regulasi)?", "id": "Daya Untuk Koreksi Frekuensi Menengah." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Berputar)?", "id": "Kapasitas Unit Sinkron Siap Menambah." },
    { "en": "Apa Itu 'Non-Spinning Reserve' (Cadangan Tidak Berputar)?", "id": "Pembangkit Cepat Start Jaga Keandalan." },
    { "en": "Apa Itu 'Operating Limit' (Batas Operasi)?", "id": "Parameter Maksimal Aman Peralatan Jaringan." },
    { "en": "Apa Itu 'Thermal Limit' (Batas Termal)?", "id": "Kapasitas Maksimal Kabel Berdasarkan Panas." },
    { "en": "Apa Itu 'Stability Limit' (Batas Stabilitas)?", "id": "Daya Maksimal Sebelum Sistem Goyah." },
    { "en": "Apa Itu 'Protection Coordination' (Koordinasi Proteksi)?", "id": "Pengaturan Urutan Kerja Perangkat Pengaman." },
    { "en": "Apa Itu 'Selectivity' (Selektivitas Proteksi)?", "id": "Kemampuan Mengisolasi Area Gangguan Terkecil." },
    { "en": "Apa Itu 'Reliability' (Keandalan Sistem)?", "id": "Kemampuan Sistem Melayani Tanpa Putus." },
    { "en": "Apa Itu 'Adequacy' (Kecukupan Sistem)?", "id": "Kemampuan Memenuhi Seluruh Beban Pelanggan." },
    { "en": "Apa Itu 'Security' (Keamanan Sistem Tenaga)?", "id": "Kemampuan Menahan Gangguan Mendadak Jaringan." },
    { "en": "Apa Itu 'Resilience' (Ketahanan Jaringan)?", "id": "Kemampuan Pulih Dari Gangguan Ekstrem." },
    { "en": "Apa Itu 'Self-Healing Grid' (Jaringan Mandiri)?", "id": "Jaringan Listrik Otomatis Memulihkan Gangguan." },
    { "en": "Apa Itu 'Autonomous Vehicle' (Kendaraan Otonom)?", "id": "Kendaraan Mampu Beroperasi Tanpa Manusia." },
    { "en": "Apa Itu 'SAE Level 0' (Otonomi Level 0)?", "id": "Kendali Kendaraan Sepenuhnya Oleh Manusia." },
    { "en": "Apa Itu 'SAE Level 1' (Bantuan Pengemudi)?", "id": "Sistem Membantu Satu Fungsi Kemudi." },
    { "en": "Apa Itu 'SAE Level 2' (Otonomi Parsial)?", "id": "Kendali Otomatis Kemudi Dan Kecepatan." },
    { "en": "Apa Itu 'SAE Level 3' (Otonomi Bersyarat)?", "id": "Sistem Mengemudi Dalam Kondisi Tertentu." },
    { "en": "Apa Itu 'SAE Level 4' (Otonomi Tinggi)?", "id": "Mengemudi Sendiri Tanpa Perlu Intervensi." },
    { "en": "Apa Itu 'SAE Level 5' (Otonomi Penuh)?", "id": "Kendaraan Otomatis Dalam Segala Kondisi." },
    { "en": "Apa Itu 'Lidar' (Light Detection And Ranging)?", "id": "Sensor Jarak Menggunakan Pantulan Cahaya." },
    { "en": "Apa Itu 'Radar' (Radio Detection And Ranging) Kendaraan?", "id": "Deteksi Objek Menggunakan Gelombang Radio." },
    { "en": "Apa Itu 'Ultrasonic Sensor' (Sensor Ultrasonik)?", "id": "Deteksi Objek Jarak Dekat Suara." },
    { "en": "Apa Itu 'Computer Vision' (Visi Komputer)?", "id": "Pemrosesan Gambar Digital Secara Cerdas." },
    { "en": "Apa Itu 'Sensor Fusion' (Fusi Sensor)?", "id": "Penggabungan Data Dari Berbagai Sensor." },
    { "en": "Apa Itu 'SLAM' (Simultaneous Localization And Mapping)?", "id": "Pemetaan Lingkungan Sambil Menentukan Posisi." },
    { "en": "Apa Itu 'HD Maps' (Peta Definisi Tinggi)?", "id": "Peta Presisi Untuk Kendaraan Otonom." },
    { "en": "Apa Itu 'Path Planning' (Perencanaan Jalur)?", "id": "Penentuan Rute Gerak Paling Aman." },
    { "en": "Apa Itu 'Obstacle Avoidance' (Penghindaran Rintangan)?", "id": "Kemampuan Menghindari Objek Di Jalan." },
    { "en": "Apa Itu 'Dead Reckoning' (Navigasi Estimasi)?", "id": "Perhitungan Posisi Berdasarkan Data Gerak." },
    { "en": "Apa Itu 'GNSS' (Global Navigation Satellite System)?", "id": "Sistem Penentu Posisi Berbasis Satelit." },
    { "en": "Apa Itu 'IMU' (Inertial Measurement Unit)?", "id": "Sensor Pengukur Percepatan Dan Rotasi." },
    { "en": "Apa Itu 'RTK' (Real-Time Kinematic)?", "id": "Teknik Peningkatan Akurasi GPS Presisi." },
    { "en": "Apa Itu 'Object Detection' (Deteksi Objek)?", "id": "Identifikasi Benda Di Sekitar Kendaraan." },
    { "en": "Apa Itu 'Lane Detection' (Deteksi Lajur)?", "id": "Identifikasi Garis Markah Jalan Otomatis." },
    { "en": "Apa Itu 'Traffic Sign Recognition' (TSR)?", "id": "Identifikasi Rambu Lalu Lintas Digital." },
    { "en": "Apa Itu 'Pedestrian Detection' (Deteksi Pejalan Kaki)?", "id": "Sistem Keamanan Mengenali Manusia Menyeberang." },
    { "en": "Apa Itu 'V2V' (Vehicle To Vehicle)?", "id": "Komunikasi Antar Kendaraan Secara Nirkabel." },
    { "en": "Apa Itu 'V2I' (Vehicle To Infrastructure)?", "id": "Komunikasi Kendaraan Dengan Infrastruktur Jalan." },
    { "en": "Apa Itu 'V2X' (Vehicle To Everything)?", "id": "Komunikasi Kendaraan Dengan Semua Objek." },
    { "en": "Apa Itu 'DSRC' (Dedicated Short Range Communications)?", "id": "Komunikasi Nirkabel Jarak Pendek Khusus." },
    { "en": "Apa Itu 'C-V2X' (Cellular V2X)?", "id": "Komunikasi Kendaraan Berbasis Jaringan Seluler." },
    { "en": "Apa Itu 'Telematics' (Telematika Kendaraan)?", "id": "Integrasi Telekomunikasi Dan Informatika Kendaraan." },
    { "en": "Apa Itu 'Drive-By-Wire' (Kendali Melalui Kabel)?", "id": "Sistem Kendali Elektronik Tanpa Mekanik." },
    { "en": "Apa Itu 'Steer-By-Wire' (Kemudi Melalui Kabel)?", "id": "Kendali Belok Roda Secara Elektronik." },
    { "en": "Apa Itu 'Brake-By-Wire' (Rem Melalui Kabel)?", "id": "Sistem Pengereman Terkendali Sinyal Listrik." },
    { "en": "Apa Itu 'Throttle-By-Wire' (Gas Melalui Kabel)?", "id": "Pengaturan Kecepatan Mesin Secara Digital." },
    { "en": "Apa Itu 'ADAS' (Advanced Driver Assistance Systems)?", "id": "Sistem Elektronik Pembantu Keamanan Berkendara." },
    { "en": "Apa Itu 'ACC' (Adaptive Cruise Control)?", "id": "Pengatur Kecepatan Menyesuaikan Jarak Depan." },
    { "en": "Apa Itu 'AEB' (Autonomous Emergency Braking)?", "id": "Pengereman Otomatis Saat Keadaan Darurat." },
    { "en": "Apa Itu 'LKA' (Lane Keeping Assist)?", "id": "Penjaga Kendaraan Tetap Di Lajur." },
    { "en": "Apa Itu 'BSD' (Blind Spot Detection)?", "id": "Peringatan Objek Di Area Buta." },
    { "en": "Apa Itu 'Parking Assist' (Bantuan Parkir)?", "id": "Sistem Membantu Parkir Kendaraan Otomatis." },
    { "en": "Apa Itu 'Ego Vehicle' (Kendaraan Utama)?", "id": "Istilah Kendaraan Yang Sedang Diteliti." },
    { "en": "Apa Itu 'Point Cloud' (Awan Titik Lidar)?", "id": "Kumpulan Titik Data Hasil Lidar." },
    { "en": "Apa Itu 'Semantic Segmentation' (Segmentasi Semantik)?", "id": "Klasifikasi Setiap Piksel Gambar Digital." },
    { "en": "Apa Itu 'Kalman Filter' (Penyaring Kalman) Otonom?", "id": "Estimasi Posisi Kendaraan Dalam Derau." },
    { "en": "Apa Itu 'Odometry' (Ometri)?", "id": "Estimasi Posisi Berdasarkan Putaran Roda." },
    { "en": "Apa Itu 'Visual Odometry' (Ometri Visual)?", "id": "Estimasi Posisi Berdasarkan Analisis Gambar." },
    { "en": "Apa Itu 'Point Of Interest' (POI)?", "id": "Lokasi Spesifik Penting Dalam Peta." },
    { "en": "Apa Itu 'Overshoot' (Loncatan Jalur)?", "id": "Kendaraan Melewati Garis Target Jalur." },
    { "en": "Apa Itu 'Control Loop' (Loop Kendali) Otonom?", "id": "Sirkuit Pengaturan Aksi Kendaraan Kontinu." },
    { "en": "Apa Itu 'Latency' (Latensi Sistem Otonom)?", "id": "Waktu Tunda Respon Perintah Komputer." },
    { "en": "Apa Itu 'GPU' (Graphics Processing Unit) Otonom?", "id": "Pemroses Data Visual Kecepatan Tinggi." },
    { "en": "Apa Itu 'NPU' (Neural Processing Unit)?", "id": "Chip Khusus Untuk Algoritma AI." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi) Otonom?", "id": "Pemrosesan Data Langsung Di Kendaraan." },
    { "en": "Apa Itu 'Over-The-Air Update' (OTA)?", "id": "Pembaruan Perangkat Lunak Secara Nirkabel." },
    { "en": "Apa Itu 'Cybersecurity' (Keamanan Siber) Kendaraan?", "id": "Perlindungan Sistem Dari Peretasan Luar." },
    { "en": "Apa Itu 'Ethernet Automotive' (Ethernet Otomotif)?", "id": "Jaringan Data Kecepatan Tinggi Kendaraan." },
    { "en": "Apa Itu 'CAN FD' (Flexible Data-Rate)?", "id": "Protokol Komunikasi Data Otomotif Cepat." },
    { "en": "Apa Itu 'FlexRay' (Protokol FlexRay)?", "id": "Komunikasi Data Kendaraan Keamanan Tinggi." },
    { "en": "Apa Itu 'Gateway ECU' (Electronic Control Unit)?", "id": "Pusat Penghubung Berbagai Jaringan Kendaraan." },
    { "en": "Apa Itu 'Redundancy' (Redundansi Sistem)?", "id": "Cadangan Komponen Jaga Keamanan Operasi." },
    { "en": "Apa Itu 'Fail-Safe' (Aman Gagal)?", "id": "Sistem Kembali Kondisi Aman Gangguan." },
    { "en": "Apa Itu 'Fail-Operational' (Operasional Gagal)?", "id": "Tetap Beroperasi Meski Komponen Rusak." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface) Otonom?", "id": "Antarmuka Interaksi Penumpang Dan Mobil." },
    { "en": "Apa Itu 'Teleoperation' (Teleoperasi)?", "id": "Kendali Kendaraan Dari Jarak Jauh." },
    { "en": "Apa Itu 'Drive Cycle' (Siklus Berkendara)?", "id": "Profil Kecepatan Untuk Pengujian Kendaraan." },
    { "en": "Apa Itu 'Hardware-In-The-Loop' (HIL)?", "id": "Pengujian Perangkat Keras Melalui Simulasi." },
    { "en": "Apa Itu 'Software-In-The-Loop' (SIL)?", "id": "Pengujian Perangkat Lunak Melalui Simulasi." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital) Mobil?", "id": "Simulasi Virtual Akurat Kendaraan Fisik." },
    { "en": "Apa Itu 'Deep Reinforcement Learning' (DRL)?", "id": "AI Belajar Mengemudi Melalui Simulasi." },
    { "en": "Apa Itu 'End-To-End Learning' (Pembelajaran Ujung)?", "id": "Pemetaan Gambar Langsung Ke Kemudi." },
    { "en": "Apa Itu 'Behavior Prediction' (Prediksi Perilaku)?", "id": "Memperkirakan Gerakan Pengguna Jalan Lain." },
    { "en": "Apa Itu 'Trolley Problem' (Masalah Troli)?", "id": "Dilema Etika Keputusan Kecelakaan AI." },
    { "en": "Apa Itu 'Regulatory Framework' (Kerangka Regulasi)?", "id": "Aturan Hukum Pengoperasian Kendaraan Otonom." },
    { "en": "Apa Itu 'Type Approval' (Persetujuan Tipe)?", "id": "Sertifikasi Kendaraan Layak Jalan Raya." },
    { "en": "Apa Itu 'Black Box' (Kotak Hitam) Kendaraan?", "id": "Perekam Data Kejadian Sebelum Kecelakaan." },
    { "en": "Apa Itu 'Ride Hailing' (Layanan Berbagi)?", "id": "Bisnis Transportasi Menggunakan Mobil Otonom." },
    { "en": "Apa Itu 'Platooning' (Pleton Kendaraan)?", "id": "Kumpulan Truk Berjalan Beriringan Rapat." },
    { "en": "Apa Itu 'Last Mile Delivery' (Pengiriman Akhir)?", "id": "Logistik Otonom Sampai Depan Rumah." },
    { "en": "Apa Itu 'Agv' (Automated Guided Vehicle)?", "id": "Robot Pengangkut Barang Industri Otomatis." },
    { "en": "Apa Itu 'Amr' (Autonomous Mobile Robot)?", "id": "Robot Bergerak Mandiri Tanpa Jalur." },
    { "en": "Apa Itu 'Vision Sensor' (Sensor Visi)?", "id": "Kamera Sebagai Pengindra Utama Sistem." },
    { "en": "Apa Itu 'Depth Camera' (Kamera Kedalaman)?", "id": "Kamera Pengukur Jarak Setiap Piksel." },
    { "en": "Apa Itu 'Thermal Camera' (Kamera Termal)?", "id": "Deteksi Objek Berdasarkan Radiasi Panas." },
    { "en": "Apa Itu 'Stereo Vision' (Visi Stereo)?", "id": "Estimasi Jarak Menggunakan Dua Kamera." },
    { "en": "Apa Itu 'ToF' (Time Of Flight)?", "id": "Pengukur Jarak Berdasarkan Waktu Cahaya." },
    { "en": "Apa Itu 'Solid State Lidar' (Lidar Padat)?", "id": "Lidar Tanpa Bagian Mekanik Berputar." },
    { "en": "Apa Itu 'Fov' (Field Of View)?", "id": "Sudut Pandang Maksimal Sebuah Sensor." },
    { "en": "Apa Itu 'Resolution' (Resolusi Sensor)?", "id": "Tingkat Kerapatan Data Hasil Pengukuran." },
    { "en": "Apa Itu 'Sample Rate' (Laju Sampel)?", "id": "Kecepatan Pengambilan Data Per Detik." },
    { "en": "Apa Itu 'Interference' (Interferensi Sensor)?", "id": "Gangguan Sinyal Antar Sensor Berdekatan." },
    { "en": "Apa Itu 'Blind Spot' (Titik Buta)?", "id": "Area Yang Tidak Terjangkau Sensor." },
    { "en": "Apa Itu 'Calibration' (Kalibrasi Sensor)?", "id": "Penyelarasan Koordinat Antar Berbagai Sensor." },
    { "en": "Apa Itu 'Intrinsic Parameters' (Parameter Intrinsik)?", "id": "Karakteristik Internal Lensa Kamera Digital." },
    { "en": "Apa Itu 'Extrinsic Parameters' (Parameter Ekstrinsik)?", "id": "Posisi Dan Orientasi Sensor Kendaraan." },
    { "en": "Apa Itu 'Point Cloud Processing' (Pemrosesan Titik)?", "id": "Analisis Data Geometri Hasil Lidar." },
    { "en": "Apa Itu 'Data Fusion' (Fusi Data)?", "id": "Penggabungan Informasi Menjadi Keputusan Tunggal." },
    { "en": "Apa Itu 'Functional Safety' (Keselamatan Fungsional)?", "id": "Standar Keamanan Sistem Elektronik Otomotif." },
    { "en": "Apa Itu 'ISO 26262' (Standar Otomotif)?", "id": "Standar Internasional Keamanan Fungsional Kendaraan." },
    { "en": "Apa Itu 'ASIL' (Automotive Safety Integrity Level)?", "id": "Klasifikasi Tingkat Risiko Kegagalan Sistem." },
  { "en": "Apa Itu 'Data Fusion' (Fusi Data)?", "id": "Penggabungan Informasi Menjadi Keputusan Tunggal." },
  { "en": "Apa Itu 'Functional Safety' (Keselamatan Fungsional)?", "id": "Standar Keamanan Sistem Elektronik Otomotif." },
  { "en": "Apa Itu 'ISO 26262' (Standar Otomotif)?", "id": "Standar Internasional Keamanan Fungsional Kendaraan." },
  { "en": "Apa Itu 'ASIL' (Automotive Safety Integrity Level)?", "id": "Klasifikasi Tingkat Risiko Kegagalan Sistem." },
  { "en": "Apa Itu 'Uptime' (Waktu Aktif)?", "id": "Durasi Sistem Beroperasi Tanpa Gangguan." },
  { "en": "Apa Itu 'Fail-Safe' (Aman-Gagal)?", "id": "Desain Sistem yang Kembali ke Kondisi Aman Saat Gagal." },
  { "en": "Apa Itu 'ECU' (Electronic Control Unit)?", "id": "Komputer Pengendali Fungsi Elektronik di Kendaraan." },
  { "en": "Apa Itu 'CAN Bus' (Controller Area Network)?", "id": "Protokol Komunikasi Antar Perangkat di Dalam Kendaraan." },
  { "en": "Apa Itu 'Redundansi' (Redundancy)?", "id": "Duplikasi Komponen Kritis untuk Meningkatkan Keandalan." },
  { "en": "Apa Itu 'Hardware-in-the-Loop' (HiL)?", "id": "Metode Pengujian ECU Menggunakan Simulasi Real-Time." },
  { "en": "Apa Itu 'Smart Grid' (Jaringan Cerdas)?", "id": "Jaringan Listrik yang Terintegrasi dengan Teknologi Digital." },
  { "en": "Apa Itu 'Power Factor' (Faktor Daya)?", "id": "Rasio Antara Daya Aktif dengan Daya Semu." },
  { "en": "Apa Itu 'Harmonisa' (Harmonics)?", "id": "Gangguan Gelombang Akibat Beban Non-Linear." },
  { "en": "Apa Itu 'SCADA'?", "id": "Sistem Kendali dan Akuisisi Data Jarak Jauh." },
  { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Komputer Industri untuk Otomatisasi Proses." },
  { "en": "Apa Itu 'Inverter' (Pembalik)?", "id": "Perangkat Pengubah Arus DC Menjadi Arus AC." },
  { "en": "Apa Itu 'Rectifier' (Penyearah)?", "id": "Perangkat Pengubah Arus AC Menjadi Arus DC." },
  { "en": "Apa Itu 'Transformer' (Transformator)?", "id": "Alat untuk Menaikkan atau Menurunkan Tegangan AC." },
  { "en": "Apa Itu 'Capacitor Bank' (Bank Kapasitor)?", "id": "Kumpulan Kapasitor untuk Memperbaiki Faktor Daya." },
  { "en": "Apa Itu 'Circuit Breaker' (Pemutus Sirkuit)?", "id": "Sakelar Otomatis untuk Melindungi Rangkaian dari Arus Lebih." },
  { "en": "Apa Itu 'Relay' (Relai)?", "id": "Sakelar Elektromagnetik yang Dikontrol Arus Listrik." },
  { "en": "Apa Itu 'Back EMF' (GGL Lawan)?", "id": "Tegangan yang Melawan Arus Utama pada Motor Listrik." },
  { "en": "Apa Itu 'Step-Up Transformer'?", "id": "Transformator untuk Menaikkan Level Tegangan." },
  { "en": "Apa Itu 'Step-Down Transformer'?", "id": "Transformator untuk Menurunkan Level Tegangan." },
  { "en": "Apa Itu 'Busbar'?", "id": "Batang Logam Penghantar Arus Besar di Panel Listrik." },
  { "en": "Apa Itu 'Earthing' (Pembumian)?", "id": "Penyaluran Arus Bocor Langsung ke Tanah." },
  { "en": "Apa Itu 'Short Circuit' (Hubung Singkat)?", "id": "Kondisi Arus Mengalir Melalui Jalur Hambatan Sangat Rendah." },
  { "en": "Apa Itu 'Load Cell' (Sel Beban)?", "id": "Sensor yang Mengubah Gaya Menjadi Sinyal Listrik." },
  { "en": "Apa Itu 'Induktansi' (Inductance)?", "id": "Sifat Rangkaian yang Menentang Perubahan Arus." },
  { "en": "Apa Itu 'Kapasitansi' (Capacitance)?", "id": "Kemampuan Komponen untuk Menyimpan Muatan Listrik." },
  { "en": "Apa Itu 'Impedansi' (Impedance)?", "id": "Total Hambatan Arus AC yang Meliputi Resistansi dan Reaktansi." },
  { "en": "Apa Itu 'Oscilloscope' (Osiloskop)?", "id": "Alat Visualisasi Bentuk Gelombang Tegangan Listrik." },
  { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Tegangan, Arus, dan Hambatan Listrik." },
  { "en": "Apa Itu 'Buck Converter'?", "id": "Konverter DC-ke-DC untuk Menurunkan Tegangan." },
  { "en": "Apa Itu 'Boost Converter'?", "id": "Konverter DC-ke-DC untuk Menaikkan Tegangan." },
  { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Persentase Waktu Aktif Sinyal dalam Satu Periode." },
  { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Metode Pengaturan Daya dengan Mengubah Lebar Pulsa." },
  { "en": "Apa Itu 'Zener Diode'?", "id": "Dioda yang Menjaga Tegangan Tetap pada Kondisi Bias Mundur." },
  { "en": "Apa Itu 'MOSFET'?", "id": "Transistor yang Menggunakan Medan Listrik untuk Kontrol Arus." },
  { "en": "Apa Itu 'IGBT'?", "id": "Transistor Daya Tinggi untuk Aplikasi Switching Cepat." },
  { "en": "Apa Itu 'Thyristor'?", "id": "Komponen Semikonduktor yang Berfungsi Sebagai Sakelar Terkendali." },
  { "en": "Apa Itu 'Optocoupler'?", "id": "Isolator Sinyal yang Menggunakan Cahaya Sebagai Penghubung." },
  { "en": "Apa Itu 'Analog to Digital Converter' (ADC)?", "id": "Pengubah Sinyal Kontinu Menjadi Data Digital." },
  { "en": "Apa Itu 'Digital to Analog Converter' (DAC)?", "id": "Pengubah Data Digital Menjadi Sinyal Kontinu." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Transmisi Data dalam Komunikasi Serial." },
  { "en": "Apa Itu 'PID Controller'?", "id": "Algoritma Kontrol untuk Menjaga Kestabilan Output Sistem." },
  { "en": "Apa Itu 'Deadband'?", "id": "Rentang Nilai Input di Mana Tidak Ada Aksi Kontrol." },
  { "en": "Apa Itu 'Histeresis'?", "id": "Perbedaan Nilai Saat Aktivasi dan Deaktivasi Sakelar." },
  { "en": "Apa Itu 'Three-Phase System' (Sistem Tiga Fasa)?", "id": "Sistem Distribusi Listrik Menggunakan Tiga Kawat Berfasa Beda." },
  { "en": "Apa Itu 'Star Connection' (Koneksi Bintang)?", "id": "Hubungan Kumparan yang Memiliki Titik Netral Bersama." },
  { "en": "Apa Itu 'Delta Connection' (Koneksi Segitiga)?", "id": "Hubungan Kumparan di Mana Ujung Satu Bertemu Pangkal Lainnya." },
  { "en": "Apa Itu 'Current Transformer' (CT)?", "id": "Trafo untuk Mengukur Arus Listrik Skala Besar." },
  { "en": "Apa Itu 'Potential Transformer' (PT)?", "id": "Trafo untuk Mengukur Tegangan Listrik Skala Tinggi." },
  { "en": "Apa Itu 'Lightning Arrester'?", "id": "Alat Proteksi Terhadap Lonjakan Tegangan Akibat Petir." },
  { "en": "Apa Itu 'Voltage Sag'?", "id": "Penurunan Tegangan Sesaat pada Sistem Tenaga." },
  { "en": "Apa Itu 'Voltage Swell'?", "id": "Kenaikan Tegangan Sesaat pada Sistem Tenaga." },
  { "en": "Apa Itu 'Ferranti Effect'?", "id": "Kenaikan Tegangan di Ujung Saluran Transmisi Tanpa Beban." },
  { "en": "Apa Itu 'Skin Effect'?", "id": "Kecenderungan Arus AC Mengalir di Permukaan Penghantar." },
  { "en": "Apa Itu 'Corona Discharge'?", "id": "Pelepasan Muatan Listrik ke Udara di Sekitar Kabel Tegangan Tinggi." },
  { "en": "Apa Itu 'Isolator'?", "id": "Bahan yang Menahan Aliran Arus Listrik." },
  { "en": "Apa Itu 'Konduktor'?", "id": "Bahan yang Memudahkan Aliran Arus Listrik." },
  { "en": "Apa Itu 'Semikonduktor'?", "id": "Bahan yang Daya Hantarnya di Antara Isolator dan Konduktor." },
  { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Alat Pengatur Kecepatan Motor dengan Mengubah Frekuensi." },
  { "en": "Apa Itu 'Soft Starter'?", "id": "Alat untuk Mengurangi Lonjakan Arus Saat Motor Mulai Berjalan." },
  { "en": "Apa Itu 'Slip Ring'?", "id": "Cincin Logam untuk Menyalurkan Listrik ke Bagian Berputar." },
  { "en": "Apa Itu 'Brushes' (Sikat Arang)?", "id": "Komponen yang Menghubungkan Arus ke Komutator Motor." },
  { "en": "Apa Itu 'Encoder'?", "id": "Sensor untuk Mendeteksi Posisi dan Kecepatan Putaran." },
  { "en": "Apa Itu 'Hall Effect Sensor'?", "id": "Sensor yang Mendeteksi Keberadaan Medan Magnet." },
  { "en": "Apa Itu 'Thermocouple'?", "id": "Sensor Suhu yang Menghasilkan Tegangan Berdasarkan Perbedaan Panas." },
  { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berdasarkan Perubahan Hambatan Logam." },
  { "en": "Apa Itu 'LVDT'?", "id": "Sensor Presisi untuk Mengukur Pergeseran Linier." },
  { "en": "Apa Itu 'Active Power' (Daya Aktif)?", "id": "Daya yang Benar-benar Diubah Menjadi Usaha (Watt)." },
  { "en": "Apa Itu 'Reactive Power' (Daya Reaktif)?", "id": "Daya yang Diperlukan untuk Membangkitkan Medan Magnet (VAR)." },
  { "en": "Apa Itu 'Apparent Power' (Daya Semu)?", "id": "Total Daya Gabungan Aktif dan Reaktif (VA)." },
  { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply)?", "id": "Penyedia Listrik Cadangan Saat Sumber Utama Padam." },
  { "en": "Apa Itu 'Inrush Current'?", "id": "Lonjakan Arus Sesaat Saat Peralatan Pertama Kali Dinyalakan." },
  { "en": "Apa Itu 'Leakage Current' (Arus Bocor)?", "id": "Arus yang Mengalir Lewat Isolasi yang Tidak Sempurna." },
  { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Sinyal Akibat Radiasi Elektromagnetik." },
  { "en": "Apa Itu 'ESD' (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis yang Dapat Merusak Komponen." },
  { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Tempat Memasang dan Menghubungkan Komponen Elektronik." },
  { "en": "Apa Itu 'Surface Mount Technology' (SMT)?", "id": "Metode Pemasangan Komponen Langsung di Permukaan PCB." },
  { "en": "Apa Itu 'Through-Hole Technology'?", "id": "Metode Pemasangan Komponen Melalui Lubang pada PCB." },
  { "en": "Apa Itu 'Soldering' (Menyolder)?", "id": "Proses Penyambungan Logam Menggunakan Timah Cair." },
  { "en": "Apa Itu 'Logic Gate' (Gerbang Logika)?", "id": "Blok Dasar Pemroses Sinyal Digital." },
  { "en": "Apa Itu 'Flip-Flop'?", "id": "Rangkaian Digital yang Dapat Menyimpan Satu Bit Data." },
  { "en": "Apa Itu 'Microcontroller'?", "id": "Chip Terpadu yang Berfungsi Sebagai Komputer Mini Pengendali." },
  { "en": "Apa Itu 'EEPROM'?", "id": "Memori yang Dapat Dihapus dan Ditulis Ulang Secara Elektrik." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak yang Ditanam Permanen pada Perangkat Keras." },
  { "en": "Apa Itu 'Kernel'?", "id": "Inti dari Sistem Operasi yang Mengelola Sumber Daya." },
  { "en": "Apa Itu 'Watchdog Timer'?", "id": "Sirkuit Pengaman untuk Mereset Sistem Jika Terjadi Freeze." },
  { "en": "Apa Itu 'BMS' (Battery Management System)?", "id": "Sistem Pengelola dan Pelindung Baterai Isi Ulang." },
  { "en": "Apa Itu 'State of Charge' (SoC)?", "id": "Level Kapasitas Baterai yang Tersisa." },
  { "en": "Apa Itu 'State of Health' (SoH)?", "id": "Kondisi Kesehatan dan Umur Pakai Baterai." },
  { "en": "Apa Itu 'Regenerative Braking'?", "id": "Sistem Pengereman yang Mengubah Energi Gerak Menjadi Listrik." },
  { "en": "Apa Itu 'Telematics'?", "id": "Gabungan Teknologi Telekomunikasi dan Informatika pada Kendaraan." },
  { "en": "Apa Itu 'ADAS'?", "id": "Sistem Elektronik Bantuan Pengemudi untuk Meningkatkan Keamanan." },
  { "en": "Apa Itu 'LiDAR'?", "id": "Teknologi Deteksi Jarak Menggunakan Pulsa Cahaya Laser." },
  { "en": "Apa Itu 'Ultrasonic Sensor'?", "id": "Sensor Jarak yang Menggunakan Gelombang Suara Frekuensi Tinggi." },
  { "en": "Apa Itu 'Infrared Sensor'?", "id": "Sensor yang Menggunakan Radiasi Cahaya Infra Merah." },
  { "en": "Apa Itu 'Absorbed Glass Mat' (AGM)?", "id": "Teknologi Baterai Kering dengan Efisiensi Tinggi." },
  { "en": "Apa Itu 'Hydroplaning'?", "id": "Kondisi Ban Kehilangan Traksi Akibat Lapisan Air di Jalan." },
  { "en": "Apa Itu 'Torque Vectoring'?", "id": "Sistem Distribusi Torsi Secara Independen ke Setiap Roda." },
  { "en": "Apa Itu 'Drive-by-Wire'?", "id": "Sistem Kendali Kendaraan Menggunakan Sinyal Elektronik Tanpa Kabel Mekanik." },
  { "en": "Apa Itu 'Brake-by-Wire'?", "id": "Sistem Pengereman yang Diaktifkan Melalui Aktuator Elektronik." },
  { "en": "Apa Itu 'Active Suspension'?", "id": "Sistem Suspensi yang Menyesuaikan Diri Secara Otomatis Terhadap Kondisi Jalan." },
  { "en": "Apa Itu 'Chassis Control'?", "id": "Sistem Elektronik yang Mengelola Stabilitas dan Dinamika Kendaraan." },
  { "en": "Apa Itu 'Traction Control System' (TCS)?", "id": "Sistem Pencegah Roda Slip Saat Akselerasi." },
  { "en": "Apa Itu 'Electronic Stability Control' (ESC)?", "id": "Teknologi Komputer untuk Meningkatkan Stabilitas Kendaraan dengan Mengerem Roda Tertentu." },
  { "en": "Apa Itu 'Oversteer'?", "id": "Kondisi Bagian Belakang Mobil Bergeser Lebih Jauh Saat Menikung." },
  { "en": "Apa Itu 'Understeer'?", "id": "Kondisi Mobil Sulit Berbelok dan Cenderung Lurus Saat Menikung." },
  { "en": "Apa Itu 'Pre-Safe System'?", "id": "Fitur Keamanan yang Menyiapkan Kendaraan Sebelum Terjadi Benturan." },
  { "en": "Apa Itu 'Crumple Zone'?", "id": "Area Kendaraan yang Dirancang untuk Hancur Guna Menyerap Energi Tabrakan." },
  { "en": "Apa Itu 'Blind Spot Detection'?", "id": "Sensor untuk Memperingatkan Adanya Objek di Area yang Tidak Terlihat Pengemudi." },
  { "en": "Apa Itu 'Lane Keep Assist' (LKA)?", "id": "Fitur yang Menjaga Mobil Tetap Berada di Jalurnya Secara Otomatis." },
  { "en": "Apa Itu 'Adaptive Cruise Control' (ACC)?", "id": "Sistem Pengatur Kecepatan yang Menjaga Jarak Aman dengan Kendaraan di Depan." },
  { "en": "Apa Itu 'E-Axle'?", "id": "Sistem Penggerak Listrik yang Menyatukan Motor, Transmisi, dan Elektronika Daya." },
  { "en": "Apa Itu 'In-Wheel Motor'?", "id": "Motor Listrik yang Dipasang Langsung di Dalam Velg Roda." },
  { "en": "Apa Itu 'Solid-State Battery'?", "id": "Baterai yang Menggunakan Elektrolit Padat untuk Keamanan dan Densitas Lebih Tinggi." },
  { "en": "Apa Itu 'Range Anxiety'?", "id": "Kekhawatiran Pengguna Kendaraan Listrik Akan Kehabisan Daya Sebelum Sampai Tujuan." },
  { "en": "Apa Itu 'V2G' (Vehicle-to-Grid)?", "id": "Teknologi yang Memungkinkan Kendaraan Listrik Mengirim Daya Kembali ke Jaringan Listrik." },
  { "en": "Apa Itu 'V2L' (Vehicle-to-Load)?", "id": "Fitur Kendaraan Listrik Sebagai Sumber Listrik untuk Peralatan Luar." },
  { "en": "Apa Itu 'Thermal Management System'?", "id": "Sistem Pengatur Suhu untuk Baterai dan Komponen Elektronik Kendaraan." },
  { "en": "Apa Itu 'Fast Charging'?", "id": "Metode Pengisian Daya Baterai dengan Arus DC Tegangan Tinggi." },
  { "en": "Apa Itu 'On-Board Charger' (OBC)?", "id": "Perangkat di Dalam Mobil yang Mengubah Listrik AC Menjadi DC untuk Baterai." },
  { "en": "Apa Itu 'CCS' (Combined Charging System)?", "id": "Standar Soket Pengisian Daya Kendaraan Listrik yang Mendukung AC dan DC." },
  { "en": "Apa Itu 'Wallbox'?", "id": "Stasiun Pengisian Daya AC yang Dipasang di Dinding Rumah." },
  { "en": "Apa Itu 'Inverter Daya' (Power Inverter)?", "id": "Alat yang Mengubah Arus DC Baterai Menjadi AC untuk Menggerakkan Motor." },
  { "en": "Apa Itu 'DCDC Converter'?", "id": "Alat yang Menurunkan Tegangan Tinggi Baterai Utama ke Tegangan Rendah 12V." },
  { "en": "Apa Itu 'Inductive Charging'?", "id": "Pengisian Daya Nirkabel Menggunakan Medan Elektromagnetik." },
  { "en": "Apa Itu 'Supercharger'?", "id": "Jaringan Pengisian Daya Sangat Cepat yang Dikembangkan Khusus." },
  { "en": "Apa Itu 'Single Pedal Driving'?", "id": "Metode Mengemudi di Mana Akselerasi dan Pengereman Dilakukan dengan Satu Pedal." },
  { "en": "Apa Itu 'Creep Mode'?", "id": "Fitur Mobil Listrik yang Bergerak Pelan Saat Rem Dilepas Tanpa Menginjak Gas." },
  { "en": "Apa Itu 'Synchronous Motor'?", "id": "Motor yang Kecepatan Putarnya Selaras dengan Frekuensi Arus Listrik." },
  { "en": "Apa Itu 'Asynchronous Motor'?", "id": "Motor Induksi yang Putaran Rotornya Lebih Lambat dari Medan Magnet Stator." },
  { "en": "Apa Itu 'Permanent Magnet Motor'?", "id": "Motor Listrik yang Menggunakan Magnet Tetap pada Rotornya." },
  { "en": "Apa Itu 'Rare Earth Metals'?", "id": "Logam Tanah Jarang yang Digunakan Sebagai Bahan Baku Magnet Motor Listrik." },
  { "en": "Apa Itu 'Stator Winding'?", "id": "Kumparan Kawat pada Bagian Diam Motor Listrik." },
  { "en": "Apa Itu 'Rotor Core'?", "id": "Inti Besi pada Bagian Berputar dari Mesin Listrik." },
  { "en": "Apa Itu 'Air Gap' (Celah Udara)?", "id": "Jarak Sempit Antara Stator dan Rotor dalam Motor Listrik." },
  { "en": "Apa Itu 'Magnetic Flux' (Fluks Magnet)?", "id": "Ukuran Kekuatan Medan Magnet yang Melewati Suatu Area." },
  { "en": "Apa Itu 'Reluctance Motor'?", "id": "Motor yang Bergerak Berdasarkan Prinsip Hambatan Magnetik Terendah." },
  { "en": "Apa Itu 'Field Weakening'?", "id": "Teknik Mengurangi Medan Magnet untuk Mencapai Kecepatan Motor yang Lebih Tinggi." },
  { "en": "Apa Itu 'Cogging Torque'?", "id": "Tahanan Magnetik yang Terasa Saat Rotor Motor Diputar Secara Manual." },
  { "en": "Apa Itu 'Isolasi Kelas F'?", "id": "Standar Ketahanan Panas Isolasi Motor Hingga 155 Derajat Celcius." },
  { "en": "Apa Itu 'IP Rating' (Ingress Protection)?", "id": "Standar Ketahanan Perangkat Terhadap Debu dan Air." },
  { "en": "Apa Itu 'Short Circuit Current'?", "id": "Nilai Maksimum Arus yang Mengalir Saat Terjadi Hubung Singkat." },
  { "en": "Apa Itu 'Breaking Capacity'?", "id": "Kemampuan Pemutus Sirkuit untuk Memutuskan Arus Gangguan Tanpa Rusak." },
  { "en": "Apa Itu 'Arc Flash'?", "id": "Ledakan Cahaya dan Panas Akibat Arus Listrik yang Melompat Lewat Udara." },
  { "en": "Apa Itu 'Galvanic Isolation'?", "id": "Pemisahan Sirkuit Elektrik untuk Mencegah Aliran Arus Langsung Namun Tetap Bisa Bertukar Informasi." },
  { "en": "Apa Itu 'Common Mode Noise'?", "id": "Gangguan Elektromagnetik yang Muncul pada Kedua Jalur Kabel Secara Bersamaan." },
  { "en": "Apa Itu 'Differential Mode Noise'?", "id": "Gangguan yang Muncul di Antara Dua Jalur Kabel dengan Arah Berlawanan." },
  { "en": "Apa Itu 'Shielded Cable'?", "id": "Kabel yang Dibungkus Lapisan Logam untuk Melindungi dari Gangguan Frekuensi Radio." },
  { "en": "Apa Itu 'Twisted Pair'?", "id": "Sepasang Kabel yang Dipilin untuk Mengurangi Interferensi Elektromagnetik." },
  { "en": "Apa Itu 'Signal Integrity'?", "id": "Kualitas dan Kejernihan Sinyal Elektrik Saat Melewati Jalur Komunikasi." },
  { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Sinyal dari Satu Kabel ke Kabel Lain yang Berdekatan." },
  { "en": "Apa Itu 'Pull-Down Resistor'?", "id": "Resistor yang Memastikan Input Digital Tetap Low Saat Tidak Ada Sinyal." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Sirkuit Komparator yang Menggunakan Histeresis untuk Menghilangkan Noise Sinyal." },
  { "en": "Apa Itu 'Level Shifter'?", "id": "Rangkaian untuk Menghubungkan Dua Perangkat dengan Level Tegangan Logika Berbeda." },
  { "en": "Apa Itu 'Voltage Divider'?", "id": "Rangkaian Sederhana untuk Mendapatkan Tegangan Lebih Rendah Menggunakan Resistor Seri." },
  { "en": "Apa Itu 'Current Limiter'?", "id": "Rangkaian yang Membatasi Arus Maksimum yang Mengalir ke Beban." },
  { "en": "Apa Itu 'Zener Voltage'?", "id": "Tegangan Spesifik Di Mana Dioda Zener Mulai Menghantarkan Arus Mundur." },
  { "en": "Apa Itu 'Saturation Region'?", "id": "Kondisi Transistor Saat Bekerja Sebagai Sakelar Tertutup Penuh." },
  { "en": "Apa Itu 'Cut-off Region'?", "id": "Kondisi Transistor Saat Bekerja Sebagai Sakelar Terbuka Penuh." },
  { "en": "Apa Itu 'Quiescent Current'?", "id": "Arus yang Dikonsumsi Perangkat Saat Dalam Kondisi Siaga (Idle)." },
  { "en": "Apa Itu 'Ripple Voltage'?", "id": "Variasi Kecil Tegangan DC yang Tersisa Setelah Proses Penyearahan." },
  { "en": "Apa Itu 'Linear Regulator'?", "id": "Penurun Tegangan DC yang Membuang Kelebihan Energi Menjadi Panas." },
  { "en": "Apa Itu 'Switching Regulator'?", "id": "Penurun Tegangan DC yang Efisien dengan Menggunakan Metode On-Off Cepat." },
  { "en": "Apa Itu 'Snubber Circuit'?", "id": "Rangkaian untuk Meredam Lonjakan Tegangan Saat Sakelar Dimatikan." },
  { "en": "Apa Itu 'Flyback Diode'?", "id": "Dioda yang Dipasang Paralel dengan Beban Induktif untuk Melindungi dari GGL Lawan." },
  { "en": "Apa Itu 'Soft Start Circuit'?", "id": "Rangkaian yang Membatasi Arus Inrush Saat Perangkat Mulai Menyala." },
  { "en": "Apa Itu 'Crowbar Circuit'?", "id": "Rangkaian Proteksi yang Menghubung-Singkatkan Jalur Jika Terjadi Overvoltage." },
  { "en": "Apa Itu 'Total Harmonic Distortion' (THD)?", "id": "Persentase Penyimpangan Gelombang dari Bentuk Sinus Murni." },
  { "en": "Apa Itu 'Power Factor Correction' (PFC)?", "id": "Teknik untuk Membuat Beban Terlihat Seperti Resistor Murni bagi Jaringan Listrik." },
  { "en": "Apa Itu 'Active Filter'?", "id": "Filter yang Menggunakan Komponen Aktif Seperti Op-Amp untuk Menghilangkan Noise." },
  { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Rentang Frekuensi yang Dapat Dilewati oleh Suatu Sistem atau Filter." },
  { "en": "Apa Itu 'Sampling Rate'?", "id": "Seberapa Sering Sinyal Analog Diambil Datanya untuk Menjadi Digital." },
  { "en": "Apa Itu 'Aliasing'?", "id": "Distorsi Sinyal Akibat Laju Pengambilan Data (Sampling) yang Terlalu Rendah." },
  { "en": "Apa Itu 'Nyquist Theorem'?", "id": "Aturan Bahwa Laju Sampling Harus Minimal Dua Kali Frekuensi Tertinggi Sinyal." },
  { "en": "Apa Itu 'Resolution' (Resolusi ADC)?", "id": "Jumlah Bit yang Digunakan untuk Mewakili Nilai Analog (Misal 10-bit)." },
  { "en": "Apa Itu 'Quantization Error'?", "id": "Pembulatan Nilai Saat Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu 'Gain' (Penguatan)?", "id": "Faktor Perkalian Sinyal Output Terhadap Sinyal Input." },
  { "en": "Apa Itu 'Feedback' (Umpan Balik)?", "id": "Pengembalian Sebagian Sinyal Output ke Input untuk Kontrol Kestabilan." },
  { "en": "Apa Itu 'Open Loop System'?", "id": "Sistem Kendali yang Bekerja Tanpa Memperhatikan Hasil Outputnya." },
  { "en": "Apa Itu 'Closed Loop System'?", "id": "Sistem Kendali yang Menggunakan Sensor untuk Memperbaiki Kesalahan Output." },
  { "en": "Apa Itu 'Set Point'?", "id": "Nilai Target yang Ingin Dicapai oleh Suatu Sistem Kontrol." },
  { "en": "Apa Itu 'Error Signal'?", "id": "Selisih Antara Set Point dan Nilai Aktual yang Terbaca Sensor." },
  { "en": "Apa Itu 'Rise Time'?", "id": "Waktu yang Dibutuhkan Sinyal untuk Naik dari 10% Menuju 90% Nilai Akhir." },
  { "en": "Apa Itu 'Settling Time'?", "id": "Waktu yang Dibutuhkan Sistem untuk Stabil Setelah Terjadi Gangguan." },
  { "en": "Apa Itu 'Overshoot'?", "id": "Kondisi Di Mana Nilai Output Melebihi Target Sebelum Akhirnya Stabil." },
  { "en": "Apa Itu 'Damping' (Peredaman)?", "id": "Efek yang Mengurangi Osilasi pada Sistem Dinamis." },
  { "en": "Apa Itu 'Critical Damping'?", "id": "Kondisi Peredaman Tercepat untuk Mencapai Stabil Tanpa Osilasi." },
  { "en": "Apa Itu 'Ladder Diagram'?", "id": "Bahasa Pemrograman Visual yang Digunakan untuk PLC." },
  { "en": "Apa Itu 'Human Machine Interface' (HMI)?", "id": "Panel Layar yang Menampilkan Data dan Kontrol Mesin Bagi Operator." },
  { "en": "Apa Itu 'Proximity Sensor'?", "id": "Sensor yang Mendeteksi Kehadiran Objek Tanpa Sentuhan Fisik." },
  { "en": "Apa Itu 'Capacitive Sensor'?", "id": "Sensor yang Bekerja Berdasarkan Perubahan Medan Listrik." },
  { "en": "Apa Itu 'Inductive Sensor'?", "id": "Sensor yang Hanya Mendeteksi Objek Berbahan Logam." },
  { "en": "Apa Itu 'Limit Switch'?", "id": "Sakelar Fisik yang Aktif Saat Tertekan oleh Gerakan Mekanis Mesin." },
  { "en": "Apa Itu 'Servo Motor'?", "id": "Motor yang Dapat Dikontrol Posisi Sudutnya dengan Sangat Presisi." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor yang Berputar dalam Langkah-langkah Diskret per Pulsa Listrik." },
  { "en": "Apa Itu 'Machine Vision'?", "id": "Sistem Kamera Industri untuk Inspeksi Otomatis Produk." }



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
