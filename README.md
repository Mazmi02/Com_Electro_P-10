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


  {
    "en": "Apa Kepanjangan VLSI (Very Large Scale Integration)?",
    "id": "Very Large Scale Integration."
  },
  {
    "en": "Apa Itu Alur Desain (Design Flow) VLSI?",
    "id": "Proses Pembuatan Chip IC (Integrated Circuit)."
  },
  {
    "en": "Apa Tahap Awal Desain VLSI (Very Large Scale Integration)?",
    "id": "Spesifikasi Dan Arsitektur."
  },
  {
    "en": "Apa Kepanjangan RTL (Register Transfer Level)?",
    "id": "Register Transfer Level."
  },
  {
    "en": "Apa Itu Deskripsi RTL (Register Transfer Level)?",
    "id": "Desain Fungsional (VHDL (VHSIC Hardware Description Language)/Verilog)."
  },
  {
    "en": "Apa Itu Sintesis (Synthesis) Logika?",
    "id": "Proses Mengubah RTL (Register Transfer Level) Ke Netlist."
  },
  {
    "en": "Apa Itu Netlist (Daftar Jaring) Gerbang?",
    "id": "Kumpulan Gerbang Logika Interkoneksi."
  },
  {
    "en": "Apa Itu Desain Tata Letak Fisik (Physical Layout)?",
    "id": "Proses Menggambar Geometri Fisik Chip."
  },
  {
    "en": "Apa Itu Tahap Penempatan (Placement)?",
    "id": "Menentukan Lokasi Fisik Gerbang Logika."
  },
  {
    "en": "Apa Itu Tahap Perutean (Routing)?",
    "id": "Menghubungkan Gerbang Logika (Jalur Logam)."
  },
  {
    "en": "Apa Kepanjangan P&R (Place and Route)?",
    "id": "Place and Route (Penempatan Perutean)."
  },
  {
    "en": "Apa Kepanjangan STA (Static Timing Analysis)?",
    "id": "Static Timing Analysis."
  },
  {
    "en": "Apa Fungsi STA (Static Timing Analysis)?",
    "id": "Memverifikasi Waktu (Timing) Desain Digital."
  },
  {
    "en": "Apa Itu Verifikasi (Verification) Desain?",
    "id": "Memastikan Desain Berfungsi Sesuai Spesifikasi."
  },
  {
    "en": "Apa Itu Simulasi (Simulation) Fungsional?",
    "id": "Memverifikasi Logika RTL (Register Transfer Level) (Tanpa Tunda)."
  },
  {
    "en": "Apa Itu Simulasi (Simulation) Gerbang (Gate-Level)?",
    "id": "Simulasi Netlist (Dengan Tunda Waktu)."
  },
  {
    "en": "Apa Kepanjangan GDSII (Graphic Data System II)?",
    "id": "Format File Standar Masker Pabrikasi."
  },
  {
    "en": "Apa Itu Tape-Out (Pita Keluar)?",
    "id": "Proses Final Desain Dikirim Ke Pabrik."
  },
  {
    "en": "Apa Itu Wafer (Wafer)?",
    "id": "Lempengan Silikon Dasar Pembuatan Chip."
  },
  {
    "en": "Apa Itu Fab (Foundry/Pabrik)?",
    "id": "Pabrik Pembuatan Semikonduktor."
  },
  {
    "en": "Apa Kepanjangan CAN (Controller Area Network)?",
    "id": "Controller Area Network."
  },
  {
    "en": "Dimana CAN (Controller Area Network) Bus Umum Digunakan?",
    "id": "Otomotif (Mobil) Dan Industri."
  },
  {
    "en": "Apa Sifat Protokol CAN (Controller Area Network)?",
    "id": "Multi-Master, Berbasis Pesan."
  },
  {
    "en": "Apa Itu Arbitrasi (Arbitration) CAN (Controller Area Network)?",
    "id": "Prioritas Pesan (ID (Identifier) Rendah Menang)."
  },
  {
    "en": "Apa Kepanjangan LIN (Local Interconnect Network)?",
    "id": "Local Interconnect Network."
  },
  {
    "en": "Kapan LIN (Local Interconnect Network) Bus Digunakan?",
    "id": "Subsistem Otomotif Biaya Rendah (Sensor)."
  },
  {
    "en": "Apa Sifat LIN (Local Interconnect Network) Bus?",
    "id": "Master Tunggal, Slave Banyak."
  },
  {
    "en": "Apa Kepanjangan MOST (Media Oriented Systems Transport)?",
    "id": "Media Oriented Systems Transport."
  },
  {
    "en": "Kapan MOST (Media Oriented Systems Transport) Bus Digunakan?",
    "id": "Sistem Infotainment Multimedia Mobil."
  },
  {
    "en": "Media Transmisi MOST (Media Oriented Systems Transport)?",
    "id": "Serat Optik Plastik (POF (Plastic Optical Fiber))."
  },
  {
    "en": "Apa Kepanjangan FlexRay (FlexRay)?",
    "id": "Protokol Otomotif Kecepatan Tinggi (Deterministik)."
  },
  {
    "en": "Apa Itu Fisika Kuantum?",
    "id": "Studi Materi Energi Level Atom."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization)?",
    "id": "Energi Bersifat Diskret (Paket)."
  },
  {
    "en": "Apa Itu Foton (Photon)?",
    "id": "Kuantum (Paket) Energi Cahaya."
  },
  {
    "en": "Apa Itu Dualitas Gelombang-Partikel?",
    "id": "Materi Cahaya Punya Sifat Keduanya."
  },
  {
    "en": "Apa Itu Prinsip Ketidakpastian (Uncertainty) Heisenberg?",
    "id": "Tidak Bisa Ukur Posisi Momentum Akurat."
  },
  {
    "en": "Apa Itu Fungsi Gelombang (Wave Function)?",
    "id": "Deskripsi Probabilitas Keadaan Kuantum."
  },
  {
    "en": "Apa Itu Persamaan Schrödinger (Schrödinger Equation)?",
    "id": "Persamaan Evolusi Fungsi Gelombang."
  },
  {
    "en": "Apa Itu Spin (Kuantum)?",
    "id": "Momentum Sudut Intrinsik Partikel."
  },
  {
    "en": "Apa Itu Prinsip Eksklusi (Exclusion) Pauli?",
    "id": "Dua Fermion Tidak Bisa Keadaan Sama."
  },
  {
    "en": "Apa Itu Fermion (Fermion)?",
    "id": "Partikel Spin Setengah Bulat (Elektron)."
  },
  {
    "en": "Apa Itu Boson (Boson)?",
    "id": "Partikel Spin Bulat (Foton)."
  },
  {
    "en": "Apa Itu Superposisi (Superposition) Kuantum?",
    "id": "Sistem Ada Banyak Keadaan Sekaligus."
  },
  {
    "en": "Apa Itu Keterikatan (Entanglement) Kuantum?",
    "id": "Koneksi Dua Partikel (Korelasi Instan)."
  },
  {
    "en": "Apa Itu Qubit (Quantum Bit)?",
    "id": "Unit Dasar Informasi Kuantum."
  },
  {
    "en": "Perbedaan Bit Dan Qubit?",
    "id": "Qubit (Quantum Bit) Bisa Superposisi (0 Dan 1)."
  },
  {
    "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?",
    "id": "Komputasi Berbasis Mekanika Kuantum."
  },
  {
    "en": "Apa Itu Gerbang (Gate) Kuantum?",
    "id": "Operasi Logika Dasar Pada Qubit (Quantum Bit)."
  },
  {
    "en": "Apa Itu Algoritma Shor (Shor's Algorithm)?",
    "id": "Algoritma Kuantum (Faktorisasi Prima)."
  },
  {
    "en": "Apa Itu Algoritma Grover (Grover's Algorithm)?",
    "id": "Algoritma Kuantum (Pencarian Database)."
  },
  {
    "en": "Apa Itu Dekohorensi (Decoherence) Kuantum?",
    "id": "Hilangnya Sifat Kuantum (Gangguan Lingkungan)."
  },
  {
    "en": "Apa Itu Kriptografi (Cryptography) Kuantum?",
    "id": "Enkripsi Aman Berbasis Mekanika Kuantum."
  },
  {
    "en": "Apa Kepanjangan QKD (Quantum Key Distribution)?",
    "id": "Quantum Key Distribution (Distribusi Kunci)."
  },
  {
    "en": "Apa Itu Terowongan (Tunneling) Kuantum?",
    "id": "Partikel Menembus Penghalang Potensial."
  },
  {
    "en": "Apa Itu Efek Josephson (Josephson Effect)?",
    "id": "Arus Superkonduktor Lewati Isolator."
  },
  {
    "en": "Apa Itu Sambungan Josephson (Josephson Junction)?",
    "id": "Dua Superkonduktor Dipisah Isolator Tipis."
  },
  {
    "en": "Aplikasi Sambungan Josephson (Josephson Junction)?",
    "id": "SQUID (Superconducting Quantum Interference Device), Qubit (Quantum Bit)."
  },
  {
    "en": "Apa Itu Titik Kuantum (Quantum Dot)?",
    "id": "Nanokristal Semikonduktor (Sifat Kuantum)."
  },
  {
    "en": "Aplikasi Titik Kuantum (Quantum Dot)?",
    "id": "Layar QLED (Quantum Dot LED), Pencitraan Medis."
  },
  {
    "en": "Apa Itu Efek Hall (Hall Effect) Kuantum?",
    "id": "Efek Hall (TerkKuantisasi) Medan Magnet Kuat."
  },
  {
    "en": "Apa Itu Model Standar (Fisika Partikel)?",
    "id": "Teori Klasifikasi Partikel Elementer."
  },
  {
    "en": "Apa Itu Quark (Quark)?",
    "id": "Partikel Elementer (Penyusun Proton Neutron)."
  },
  {
    "en": "Apa Itu Lepton (Lepton)?",
    "id": "Partikel Elementer (Contoh: Elektron)."
  },
  {
    "en": "Apa Itu Boson (Boson) Higgs?",
    "id": "Partikel Terkait Medan Higgs (Massa)."
  },
  {
    "en": "Apa Itu Isolator Topologi (Topological Insulator)?",
    "id": "Isolator (Internal), Konduktor (Permukaan)."
  },
  {
    "en": "Apa Itu Elektrodinamika Kuantum (QED)?",
    "id": "Teori Kuantum Interaksi Elektromagnetik."
  },
  {
    "en": "Apa Itu Kromodinamika Kuantum (QCD)?",
    "id": "Teori Kuantum Interaksi Kuat (Quark)."
  },
  {
    "en": "Apa Itu Motor Brushless DC (BLDC)?",
    "id": "Motor DC (Direct Current) Tanpa Sikat (Brush)."
  },
  {
    "en": "Bagaimana Komutasi Motor BLDC (Brushless DC)?",
    "id": "Secara Elektronik (Inverter Sensor Hall)."
  },
  {
    "en": "Apa Itu Komutasi Trapesoidal (Trapezoidal)?",
    "id": "Metode Komutasi Sederhana Motor BLDC (Brushless DC)."
  },
  {
    "en": "Apa Itu Komutasi Sinusoidal (Sinusoidal)?",
    "id": "Metode Komutasi Halus Motor BLDC (Brushless DC)."
  },
  {
    "en": "Apa Kepanjangan PMSM (Permanent Magnet Synchronous Motor)?",
    "id": "Permanent Magnet Synchronous Motor."
  },
  {
    "en": "Bentuk GGL (Gaya Gerak Listrik) Balik Motor BLDC (Brushless DC)?",
    "id": "Trapesoidal (Trapezoidal)."
  },
  {
    "en": "Bentuk GGL (Gaya Gerak Listrik) Balik Motor PMSM (Permanent Magnet Synchronous Motor)?",
    "id": "Sinusoidal (Sinusoidal)."
  },
  {
    "en": "Kontrol Apa Yang Dipakai Motor PMSM (Permanent Magnet Synchronous Motor)?",
    "id": "Kontrol Berorientasi Medan (FOC (Field Oriented Control))."
  },
  {
    "en": "Apa Itu Motor Sinkron (Synchronous Motor)?",
    "id": "Kecepatan Rotor Sinkron Frekuensi Stator."
  },
  {
    "en": "Apa Itu Motor Asinkron (Asynchronous Motor)?",
    "id": "Kecepatan Rotor Dibawah Kecepatan Sinkron."
  },
  {
    "en": "Nama Lain Motor Asinkron (Asynchronous Motor)?",
    "id": "Motor Induksi (Induction Motor)."
  },
  {
    "en": "Apa Itu Motor Induksi Fasa Tunggal?",
    "id": "Motor Induksi (Satu Fasa Catu Daya)."
  },
  {
    "en": "Apa Itu Motor Induksi Tiga Fasa?",
    "id": "Motor Induksi (Tiga Fasa Catu Daya)."
  },
  {
    "en": "Apa Itu Rotor Sangkar Tupai (Squirrel Cage)?",
    "id": "Rotor (Batang Konduktor Hubung Singkat)."
  },
  {
    "en": "Apa Itu Rotor Lilit (Wound Rotor)?",
    "id": "Rotor (Belitan Tiga Fasa, Cincin Geser)."
  },
  {
    "en": "Apa Keuntungan Rotor Lilit (Wound Rotor)?",
    "id": "Kontrol Kecepatan Torsi (Resistor Eksternal)."
  },
  {
    "en": "Apa Itu Starting (Pengasutan) Langsung (DOL)?",
    "id": "Menghubungkan Motor Langsung Ke Jala-Jala."
  },
  {
    "en": "Kelemahan Starting (Pengasutan) Langsung (DOL)?",
    "id": "Arus Awal Sangat Tinggi."
  },
  {
    "en": "Apa Itu Starting (Pengasutan) Bintang-Segitiga (Star-Delta)?",
    "id": "Mengurangi Tegangan Awal Motor."
  },
  {
    "en": "Apa Itu Starting (Pengasutan) Autotransformator?",
    "id": "Menggunakan Autotrafo (Menurunkan Tegangan Awal)."
  },
  {
    "en": "Apa Itu Soft Starter (Pengasut Lunak)?",
    "id": "Starting (Pengasutan) Berbasis Elektronika Daya (SCR (Silicon Controlled Rectifier))."
  },
  {
    "en": "Apa Kepanjangan VFD (Variable Frequency Drive)?",
    "id": "Variable Frequency Drive."
  },
  {
    "en": "Apa Fungsi VFD (Variable Frequency Drive)?",
    "id": "Mengatur Kecepatan Motor Induksi (Frekuensi)."
  },
  {
    "en": "Apa Itu Kontrol V/f (Volt per Hertz)?",
    "id": "Kontrol VFD (Variable Frequency Drive) (Menjaga Fluks Konstan)."
  },
  {
    "en": "Apa Itu Penerima (Receiver) Superheterodyne?",
    "id": "Arsitektur Penerima (Konversi Frekuensi IF)."
  },
  {
    "en": "Apa Kepanjangan IF (Intermediate Frequency)?",
    "id": "Intermediate Frequency (Frekuensi Antara)."
  },
  {
    "en": "Apa Komponen Utama Penerima Superheterodyne?",
    "id": "LNA (Low Noise Amplifier), Mixer, Osilator Lokal, Filter IF."
  },
  {
    "en": "Apa Itu Osilator Lokal (Local Oscillator/LO)?",
    "id": "Pembangkit Sinyal (Frekuensi Pencampuran)."
  },
  {
    "en": "Apa Itu Mixer (Pencampur) RF (Radio Frequency)?",
    "id": "Perangkat Non-Linear (Pengali Sinyal)."
  },
  {
    "en": "Apa Output (Keluaran) Mixer RF (Radio Frequency)?",
    "id": "Jumlah (RF+LO) Selisih (RF-LO) Frekuensi."
  },
  {
    "en": "Apa Itu Frekuensi Gambar (Image Frequency)?",
    "id": "Frekuensi Palsu (Terima Selain RF (Radio Frequency))."
  },
  {
    "en": "Bagaimana Cara Menolak Frekuensi Gambar?",
    "id": "Filter RF (Radio Frequency) (Filter Lolos Pita)."
  },
  {
    "en": "Apa Itu Filter IF (Intermediate Frequency)?",
    "id": "Filter Selektif (Menentukan Bandwidth Saluran)."
  },
  {
    "en": "Apa Kepanjangan LNA (Low Noise Amplifier)?",
    "id": "Low Noise Amplifier (Penguat Derau Rendah)."
  },
  {
    "en": "Dimana Posisi LNA (Low Noise Amplifier)?",
    "id": "Tahap Pertama (Paling Depan) Penerima."
  },
  {
    "en": "Mengapa LNA (Low Noise Amplifier) Ditaruh Di Depan?",
    "id": "Memperbaiki Noise Figure (NF) Sistem."
  },
  {
    "en": "Apa Itu Noise Figure (NF) (Angka Derau)?",
    "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio) Sistem."
  },
  {
    "en": "Apa Arti Noise Figure (NF) Rendah?",
    "id": "Kinerja Baik (Noise (Derau) Ditambahkan Sedikit)."
  },
  {
    "en": "Apa Itu Noise Factor (F) (Faktor Derau)?",
    "id": "Rasio SNR (Signal-to-Noise Ratio) Input Output (Linear)."
  },
  {
    "en": "Hubungan Noise Figure (NF) Dan Noise Factor (F)?",
    "id": "NF (dB) = 10 * log10(F)."
  },
  {
    "en": "Apa Itu Rumus Friis (Noise Bertingkat)?",
    "id": "Menghitung Total Noise Factor (NF) Sistem Kaskade."
  },
  {
    "en": "Tahap Mana Paling Mempengaruhi Total NF (Noise Figure)?",
    "id": "Tahap Pertama (Gain (Penguatan) Tahap Pertama)."
  },
  {
    "en": "Apa Itu Penerima (Receiver) Direct Conversion?",
    "id": "Arsitektur Penerima (Konversi Langsung RF-Baseband)."
  },
  {
    "en": "Apa Nama Lain Penerima Direct Conversion?",
    "id": "Homodyne Atau Zero-IF (ZIF)."
  },
  {
    "en": "Berapa Frekuensi IF (Intermediate Frequency) Penerima Zero-IF (ZIF)?",
    "id": "Nol Hertz (Hz)."
  },
  {
    "en": "Apa Frekuensi Osilator Lokal (LO) Penerima Zero-IF (ZIF)?",
    "id": "Sama Dengan Frekuensi RF (Radio Frequency) Masuk."
  },
  {
    "en": "Keuntungan Penerima Zero-IF (ZIF)?",
    "id": "Sederhana, Integrasi Tinggi, Tanpa Filter Gambar."
  },
  {
    "en": "Kelemahan Penerima Zero-IF (ZIF)?",
    "id": "Kebocoran LO (LO Leakage), DC (Direct Current) Offset, Flicker Noise."
  },
  {
    "en": "Apa Itu Kebocoran LO (LO Leakage)?",
    "id": "Sinyal Osilator Lokal (LO) Bocor Ke Input."
  },
  {
    "en": "Apa Itu DC (Direct Current) Offset (Zero-IF)?",
    "id": "Komponen DC (Direct Current) Palsu Di Output Baseband."
  },
  {
    "en": "Apa Itu Arsitektur Penerima Low-IF (IF Rendah)?",
    "id": "Kompromi (Superheterodyne Dan Zero-IF (ZIF))."
  },
  {
    "en": "Apa Itu Konversi Ganda (Dual Conversion)?",
    "id": "Penerima Superheterodyne (Dua Tahap IF (Intermediate Frequency))."
  },
  {
    "en": "Apa Itu Desensitisasi (Desensitization) Penerima?",
    "id": "Penurunan Sensitivitas Akibat Sinyal Kuat."
  },
  {
    "en": "Apa Itu Pemblokiran (Blocking) Penerima?",
    "id": "Sinyal Kuat (Luar Pita) Menjenuhkan LNA (Low Noise Amplifier)."
  },
  {
    "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?",
    "id": "Ukuran Linearitas Penerima (Intermodulasi)."
  },
  {
    "en": "Apa Kepanjangan IIP3 (Input IP3)?",
    "id": "Third-Order Intercept Point (Referensi Input)."
  },
  {
    "en": "Apa Kepanjangan OIP3 (Output IP3)?",
    "id": "Third-Order Intercept Point (Referensi Output)."
  },
  {
    "en": "Apa Itu Distorsi Intermodulasi (IMD)?",
    "id": "Sinyal Palsu Akibat Non-Linearitas."
  },
  {
    "en": "Apa Itu Produk IMD3 (Intermodulation Distortion 3)?",
    "id": "Produk Intermodulasi Orde Ketiga."
  },
  {
    "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?",
    "id": "Titik Gain (Penguatan) Turun 1 dB (Saturasi)."
  },
  {
    "en": "Apa Itu Rentang Dinamis (Dynamic Range)?",
    "id": "Rentang Sinyal (Terlemah Terkuat) Diterima."
  },
  {
    "en": "Apa Kepanjangan SFDR (Spurious-Free Dynamic Range)?",
    "id": "Spurious-Free Dynamic Range."
  },
  {
    "en": "Apa Itu Sensitivitas (Sensitivity) Penerima?",
    "id": "Sinyal Input Terlemah (Dapat Dideteksi)."
  },
  {
    "en": "Apa Itu Lantai Derau (Noise Floor)?",
    "id": "Level Daya Noise (Derau) Internal Sistem."
  },
  {
    "en": "Apa Itu Daya Noise (Derau) Termal Ideal?",
    "id": "k * T * B (Boltzmann * Suhu * Bandwidth)."
  },
  {
    "en": "Apa Itu Konstanta Boltzmann (k)?",
    "id": "Konstanta Fisika (Termodinamika)."
  },
  {
    "en": "Apa Itu Suhu (Kelvin) Standar Noise (Derau)?",
    "id": "Dua Ratus Sembilan Puluh Kelvin (290 K)."
  },
  {
    "en": "Apa Kepanjangan AGC (Automatic Gain Control)?",
    "id": "Automatic Gain Control (Pengatur Gain (Penguatan) Otomatis)."
  },
  {
    "en": "Fungsi AGC (Automatic Gain Control) Penerima?",
    "id": "Menjaga Level Sinyal Output Konstan."
  },
  {
    "en": "Apa Kepanjangan RSSI (Received Signal Strength Indicator)?",
    "id": "Received Signal Strength Indicator."
  },
  {
    "en": "Apa Itu Mixer (Pencampur) Pasif?",
    "id": "Mixer (Pencampur) (Tanpa Gain (Penguatan)) (Dioda)."
  },
  {
    "en": "Apa Itu Mixer (Pencampur) Aktif?",
    "id": "Mixer (Pencampur) (Dengan Gain (Penguatan)) (Transistor)."
  },
  {
    "en": "Apa Itu Mixer (Pencampur) Seimbang (Balanced Mixer)?",
    "id": "Mixer (Pencampur) (Meredam Kebocoran Port)."
  },
  {
    "en": "Apa Itu Mixer (Pencampur) Seimbang Ganda (DBM)?",
    "id": "Mixer (Pencampur) (Isolasi Port Tinggi)."
  },
  {
    "en": "Apa Itu Cincin Dioda (Diode Ring)?",
    "id": "Inti Mixer (Pencampur) Seimbang Ganda (DBM)."
  },
  {
    "en": "Apa Itu Rugi Konversi (Conversion Loss) Mixer?",
    "id": "Pelemahan Sinyal (Mixer (Pencampur) Pasif)."
  },
  {
    "en": "Apa Itu Gain (Penguatan) Konversi (Conversion Gain) Mixer?",
    "id": "Penguatan Sinyal (Mixer (Pencampur) Aktif)."
  },
  {
    "en": "Apa Itu Isolasi LO-RF (Local Oscillator-Radio Frequency) Mixer?",
    "id": "Pelemahan Kebocoran Sinyal LO (Osilator Lokal) Ke RF (Radio Frequency)."
  },
  {
    "en": "Apa Itu Isolasi RF-IF (Radio Frequency-Intermediate Frequency) Mixer?",
    "id": "Pelemahan Kebocoran Sinyal RF (Radio Frequency) Ke IF (Frekuensi Antara)."
  },
  {
    "en": "Apa Itu Port RF (Radio Frequency) (Mixer)?",
    "id": "Port Input Sinyal Radio Frekuensi."
  },
  {
    "en": "Apa Itu Port LO (Local Oscillator) (Mixer)?",
    "id": "Port Input Sinyal Osilator Lokal."
  },
  {
    "en": "Apa Itu Port IF (Intermediate Frequency) (Mixer)?",
    "id": "Port Output Sinyal Frekuensi Antara."
  },
  {
    "en": "Apa Itu Pemancar (Transmitter) Heterodyne?",
    "id": "Pemancar (Konversi Ke Atas Melalui IF)."
  },
  {
    "en": "Apa Itu Pemancar (Transmitter) Direct Conversion?",
    "id": "Pemancar (Konversi Langsung Baseband Ke RF)."
  },
  {
    "en": "Apa Itu Modulator I/Q (I/Q Modulator)?",
    "id": "Komponen Utama Pemancar Direct Conversion."
  },
  {
    "en": "Apa Kepanjangan PA (Power Amplifier)?",
    "id": "Power Amplifier (Penguat Daya)."
  },
  {
    "en": "Dimana Posisi PA (Power Amplifier)?",
    "id": "Tahap Terakhir Pemancar (Sebelum Antena)."
  },
  {
    "en": "Apa Itu Efisiensi (PAE (Power Added Efficiency)) Penguat Daya?",
    "id": "Ukuran Efisiensi Penguat Daya RF (Radio Frequency)."
  },
  {
    "en": "Apa Itu Linearitas (Linearity) Penguat Daya?",
    "id": "Kemampuan Menguatkan Sinyal Tanpa Distorsi."
  },
  {
    "en": "Apa Itu Kompresi Gain (Gain Compression)?",
    "id": "Penurunan Gain (Penguatan) Pada Daya Tinggi (Saturasi)."
  },
  {
    "en": "Apa Itu Linearizer (Linearizer) Penguat Daya?",
    "id": "Sirkuit Perbaikan Linearitas Penguat Daya."
  },
  {
    "en": "Apa Itu Penguat Daya (PA) Kelas A?",
    "id": "Linearitas Tinggi, Efisiensi Sangat Rendah."
  },
  {
    "en": "Apa Itu Penguat Daya (PA) Kelas B?",
    "id": "Efisiensi Sedang (Distorsi Crossover)."
  },
  {
    "en": "Apa Itu Penguat Daya (PA) Kelas AB?",
    "id": "Kompromi (Linearitas Baik, Efisiensi Baik)."
  },
  {
    "en": "Apa Itu Penguat Daya (PA) Kelas C?",
    "id": "Efisiensi Tinggi (Sangat Non-Linear)."
  },
  {
    "en": "Aplikasi Penguat Daya (PA) Kelas C?",
    "id": "Pemancar FM (Frequency Modulation) (Modulasi Frekuensi) (Sinyal Konstan)."
  },
  {
    "en": "Apa Itu Penguat Daya (PA) Switching (Kelas D, E, F)?",
    "id": "Efisiensi Sangat Tinggi (Mode Saklar)."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Doherty?",
    "id": "Teknik Peningkatan Efisiensi Penguat Daya (Daya Puncak)."
  },
  {
    "en": "Apa Kepanjangan LDMOS (Laterally Diffused Metal Oxide Semiconductor)?",
    "id": "Laterally Diffused Metal Oxide Semiconductor."
  },
  {
    "en": "Aplikasi LDMOS (Laterally Diffused Metal Oxide Semiconductor)?",
    "id": "Penguat Daya RF (Radio Frequency) (Stasiun Pemancar Seluler)."
  },
  {
    "en": "Apa Kepanjangan GaN (Gallium Nitride)?",
    "id": "Gallium Nitride (Galium Nitrida)."
  },
  {
    "en": "Keunggulan GaN (Gallium Nitride) (RF (Radio Frequency))?",
    "id": "Daya Tinggi, Frekuensi Tinggi, Efisien."
  },
  {
    "en": "Apa Kepanjangan GaAs (Gallium Arsenide)?",
    "id": "Gallium Arsenide (Galium Arsenida)."
  },
  {
    "en": "Keunggulan GaAs (Gallium Arsenide) (RF (Radio Frequency))?",
    "id": "Frekuensi Tinggi, Noise (Derau) Rendah (LNA (Low Noise Amplifier))."
  },
  {
    "en": "Apa Itu Penjejakan Selubung (Envelope Tracking)?",
    "id": "Teknik Efisiensi Penguat Daya (VCC (Voltage Common Collector) Dinamis)."
  },
  {
    "en": "Apa Itu Digital Pre-Distortion (DPD)?",
    "id": "Teknik Digital (Kompensasi Non-Linearitas PA (Power Amplifier))."
  },
  {
    "en": "Apa Itu Transceiver (Transceiver)?",
    "id": "Gabungan Pemancar (Transmitter) Penerima (Receiver)."
  },
  {
    "en": "Apa Itu Saklar Antena (Antenna Switch)?",
    "id": "Memilih Antara Transmit (Kirim) Receive (Terima)."
  },
  {
    "en": "Apa Itu Duplekser (Duplexer) (RF (Radio Frequency))?",
    "id": "Memungkinkan Transmit (Kirim) Receive (Terima) Bersamaan (Satu Antena)."
  },
  {
    "en": "Apa Itu Diplexer (Diplexer) (RF (Radio Frequency))?",
    "id": "Memisah/Menggabung Sinyal (Beda Pita Frekuensi)."
  },
  {
    "en": "Apa Itu Psikoakustika (Psychoacoustics)?",
    "id": "Studi Persepsi Suara Manusia."
  },
  {
    "en": "Apa Itu Efek Haas (Haas Effect)?",
    "id": "Efek Precedence (Suara Pertama Dominan)."
  },
  {
    "en": "Apa Itu Masking (Penyelubungan) Frekuensi?",
    "id": "Suara Keras Menutupi Suara Lemah."
  },
  {
    "en": "Apa Pola Polar (Polar Pattern) Kardioid?",
    "id": "Pola Mikrofon (Bentuk Hati)."
  },
  {
    "en": "Pola Polar Kardioid Sensitif Dimana?",
    "id": "Sensitif Di Depan (Menolak Belakang)."
  },
  {
    "en": "Apa Pola Polar (Polar Pattern) Omni?",
    "id": "Sensitif Segala Arah (360 Derajat)."
  },
  {
    "en": "Apa Pola Polar (Polar Pattern) Figure-8?",
    "id": "Sensitif Depan Belakang (Tolak Samping)."
  },
  {
    "en": "Apa Itu Efek Proksimitas (Proximity Effect) Mikrofon?",
    "id": "Peningkatan Bass (Dekat Sumber)."
  },
  {
    "en": "Mikrofon Apa Punya Efek Proksimitas?",
    "id": "Mikrofon Directional (Kardioid, Figure-8)."
  },
  {
    "en": "Apa Itu Rumble (Gemuruh) Audio?",
    "id": "Noise (Derau) Frekuensi Sangat Rendah."
  },
  {
    "en": "Apa Itu Filter High-Pass (HPF) Audio?",
    "id": "Memotong Frekuensi Rendah (Rumble)."
  },
  {
    "en": "Apa Itu Sibilance (Sibilansi) Audio?",
    "id": "Suara Desis 'S' Keras (Vokal)."
  },
  {
    "en": "Apa Itu De-Esser (De-Esser)?",
    "id": "Kompresor Meredam Sibilansi."
  },
  {
    "en": "Apa Itu Pink Noise (Derau Merah Jambu)?",
    "id": "Energi Sama Per Oktaf."
  },
  {
    "en": "Apa Itu White Noise (Derau Putih)?",
    "id": "Energi Sama Per Frekuensi."
  },
  {
    "en": "Apa Itu Plot Root Locus (Kedudukan Akar)?",
    "id": "Graf Lokasi Kutub (Pole) (Variasi Gain)."
  },
  {
    "en": "Kemana Cabang Root Locus (Kedudukan Akar) Bergerak?",
    "id": "Dari Kutub (Pole) Menuju Nol (Zero)."
  },
  {
    "en": "Apa Itu Asimtot (Asymptote) Root Locus?",
    "id": "Garis Pandu Cabang (Menuju Tak Hingga)."
  },
  {
    "en": "Apa Itu Titik Pisah (Breakaway Point) Root Locus?",
    "id": "Titik Cabang Meninggalkan Sumbu Real."
  },
  {
    "en": "Apa Itu Titik Masuk (Break-in Point) Root Locus?",
    "id": "Titik Cabang Masuk Sumbu Real."
  },
  {
    "en": "Kapan Sistem Stabil (Root Locus)?",
    "id": "Semua Kutub (Pole) Di Sisi Kiri (LHP)."
  },
  {
    "en": "Apa Itu Dekade (Decade) (Bode Plot)?",
    "id": "Perubahan Frekuensi Sepuluh Kali Lipat."
  },
  {
    "en": "Apa Itu Oktaf (Octave) (Bode Plot)?",
    "id": "Perubahan Frekuensi Dua Kali Lipat."
  },
  {
    "en": "Berapa Kemiringan (Slope) Integrator (Bode Plot)?",
    "id": "Minus Dua Puluh dB Per Dekade."
  },
  {
    "en": "Berapa Kemiringan (Slope) Diferensiator (Bode Plot)?",
    "id": "Plus Dua Puluh dB Per Dekade."
  },
  {
    "en": "Berapa Pergeseran Fasa Integrator?",
    "id": "Minus Sembilan Puluh Derajat."
  },
  {
    "en": "Berapa Pergeseran Fasa Diferensiator?",
    "id": "Plus Sembilan Puluh Derajat."
  },
  {
    "en": "Apa Itu Frekuensi Pojok (Corner Frequency)?",
    "id": "Frekuensi Titik Respon Turun 3dB."
  },
  {
    "en": "Apa Itu Respon Transien (Transient Response)?",
    "id": "Respon Sistem Sesaat (Sebelum Tunak)."
  },
  {
    "en": "Apa Itu Respon Keadaan Tunak (Steady-State)?",
    "id": "Respon Sistem Setelah Waktu Lama."
  },
  {
    "en": "Apa Itu Konverter Buck (Buck Converter)?",
    "id": "Konverter Penurun Tegangan DC (Direct Current)."
  },
  {
    "en": "Apa Itu Konverter Boost (Boost Converter)?",
    "id": "Konverter Penaik Tegangan DC (Direct Current)."
  },
  {
    "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?",
    "id": "Konverter Penaik/Penurun (Polaritas Terbalik)."
  },
  {
    "en": "Apa Itu Konverter Cuk (Cuk Converter)?",
    "id": "Konverter Penaik/Penurun (Polaritas Terbalik)."
  },
  {
    "en": "Apa Itu Konverter SEPIC (SEPIC Converter)?",
    "id": "Konverter Penaik/Penurun (Polaritas Sama)."
  },
  {
    "en": "Apa Itu Konverter Zeta (Zeta Converter)?",
    "id": "Konverter Penaik/Penurun (Polaritas Sama)."
  },
  {
    "en": "Konverter Apa Yang Non-Inverting Buck-Boost?",
    "id": "SEPIC (SEPIC Converter) Dan Zeta (Zeta Converter)."
  },
  {
    "en": "Apa Itu Mode Konduksi Kontinu (CCM)?",
    "id": "Arus Induktor Tidak Pernah Nol."
  },
  {
    "en": "Apa Itu Mode Konduksi Dis-Kontinu (DCM)?",
    "id": "Arus Induktor Mencapai Nol."
  },
  {
    "en": "Apa Itu Mode Konduksi Kritis (CRM/BCM)?",
    "id": "Batas Antara CCM (Mode Konduksi Kontinu) DCM (Mode Konduksi Dis-Kontinu)."
  },
  {
    "en": "Apa Itu Ripple (Riak) Arus Induktor?",
    "id": "Fluktuasi Arus Puncak-Ke-Puncak Induktor."
  },
  {
    "en": "Apa Itu Ripple (Riak) Tegangan Output?",
    "id": "Fluktuasi Tegangan DC (Direct Current) Output."
  },
  {
    "en": "Apa Fungsi Kapasitor Output (SMPS (Switch Mode Power Supply))?",
    "id": "Filter (Menghaluskan) Ripple (Riak) Tegangan."
  },
  {
    "en": "Apa Fungsi Induktor (SMPS (Switch Mode Power Supply))?",
    "id": "Menyimpan Menyalurkan Energi (Membatasi Arus)."
  },
  {
    "en": "Apa Itu Rangkaian Penjepit (Clamp) Aktif?",
    "id": "Rangkaian Snubber (Peredam) (Pakai Transistor)."
  },
  {
    "en": "Apa Itu Rangkaian Snubber (Peredam) RCD (Resistor-Capacitor-Diode)?",
    "id": "Snubber (Peredam) Disipatif (Membuang Panas)."
  },
  {
    "en": "Apa Itu Konverter Flyback (Flyback Converter)?",
    "id": "Topologi Isolasi (Berbasis Buck-Boost)."
  },
  {
    "en": "Apa Itu Konverter Forward (Forward Converter)?",
    "id": "Topologi Isolasi (Berbasis Buck)."
  },
  {
    "en": "Apa Itu Reset (Reset) Inti Trafo (Flyback)?",
    "id": "Energi Dikosongkan Saat Saklar OFF."
  },
  {
    "en": "Apa Itu Reset (Reset) Inti Trafo (Forward)?",
    "id": "Perlu Lilitan Tersier (Reset)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Gelombang Linear?",
    "id": "Vektor Medan Listrik (Osilasi Satu Bidang)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Gelombang Vertikal?",
    "id": "Medan Listrik (Osilasi Bidang Vertikal)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Gelombang Horizontal?",
    "id": "Medan Listrik (Osilasi Bidang Horizontal)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Gelombang Sirkular?",
    "id": "Medan Listrik (Berputar, Magnitudo Konstan)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Gelombang Eliptikal?",
    "id": "Medan Listrik (Berputar, Magnitudo Variasi)."
  },
  {
    "en": "Apa Itu Rasio Aksial (Axial Ratio) (AR)?",
    "id": "Rasio Sumbu Utama Minor (Polarisasi Eliptikal)."
  },
  {
    "en": "Berapa Rasio Aksial (AR) Polarisasi Sirkular Ideal?",
    "id": "Satu (1) (Atau Nol dB)."
  },
  {
    "en": "Apa Kepanjangan RHCP (Right-Hand Circular Polarization)?",
    "id": "Polarisasi Sirkular Putar Kanan."
  },
  {
    "en": "Apa Kepanjangan LHCP (Left-Hand Circular Polarization)?",
    "id": "Polarisasi Sirkular Putar Kiri."
  },
  {
    "en": "Apa Itu Rugi Mismatch (Mismatch) Polarisasi?",
    "id": "Rugi Daya (Antena Beda Polarisasi)."
  },
  {
    "en": "Apa Itu Kondisi Batas (Boundary) Konduktor Sempurna?",
    "id": "Medan E Tangensial = Nol."
  },
  {
    "en": "Apa Itu Kondisi Batas (Boundary) Dielektrik Sempurna?",
    "id": "Komponen Tangensial E Kontinu."
  },
  {
    "en": "Apa Itu Kerapatan Arus Permukaan (J_s)?",
    "id": "Arus Mengalir Di Permukaan Konduktor."
  },
  {
    "en": "Apa Itu Teorema Poynting (Poynting's Theorem)?",
    "id": "Teorema Konservasi Energi Elektromagnetik."
  },
  {
    "en": "Apa Itu Vektor Poynting (Poynting Vector)?",
    "id": "Vektor Kerapatan Daya Sesaat."
  },
  {
    "en": "Apa Itu VHDL (VHSIC Hardware Description Language)?",
    "id": "Bahasa Deskripsi Perangkat Keras."
  },
  {
    "en": "Apa Itu Verilog (Verilog)?",
    "id": "Bahasa Deskripsi Perangkat Keras (Lainnya)."
  },
  {
    "en": "Apa Itu Sintesis (Synthesis)?",
    "id": "Proses Mengubah Kode HDL (Hardware Description Language) Ke Gerbang."
  },
  {
    "en": "Apa Itu Simulasi (Simulation)?",
    "id": "Proses Verifikasi Fungsionalitas Kode HDL (Hardware Description Language)."
  },
  {
    "en": "Apa Itu Testbench (Testbench)?",
    "id": "Kode Simulasi (Pemberi Stimulus Pengecek Output)."
  },
  {
    "en": "Apa Itu Entitas (Entity) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Deklarasi Port (Antarmuka) Desain."
  },
  {
    "en": "Apa Itu Arsitektur (Architecture) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Deskripsi Fungsionalitas (Isi) Entitas."
  },
  {
    "en": "Apa Itu Proses (Process) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Blok Kode Sekuensial (Eksekusi Berurutan)."
  },
  {
    "en": "Apa Itu Daftar Sensitivitas (Sensitivity List)?",
    "id": "Daftar Sinyal Pemicu Eksekusi Proses."
  },
  {
    "en": "Apa Itu Sinyal (Signal) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Kawat (Wire) Penghubung (Level Arsitektur)."
  },
  {
    "en": "Apa Itu Variabel (Variable) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Penyimpanan Sementara (Dalam Proses)."
  },
  {
    "en": "Apa Itu Modul (Module) (Verilog)?",
    "id": "Blok Desain Dasar (Setara Entitas+Arsitektur)."
  },
  {
    "en": "Apa Itu 'wire' (Kawat) (Verilog)?",
    "id": "Tipe Data Jaring (Net) (Koneksi Pasif)."
  },
  {
    "en": "Apa Itu 'reg' (Register) (Verilog)?",
    "id": "Tipe Data Variabel (Penyimpan Nilai)."
  },
  {
    "en": "Apa Itu Blok 'always' (Verilog)?",
    "id": "Blok Kode (Eksekusi Saat Sinyal Pemicu)."
  },
  {
    "en": "Apa Itu '@(posedge clk)' (Verilog)?",
    "id": "Pemicu Tepi Naik (Positive Edge) Clock."
  },
  {
    "en": "Apa Itu '@(*)' (Verilog)?",
    "id": "Pemicu Kombinasional (Semua Input Sensitif)."
  },
  {
    "en": "Apa Itu 'assign' (Verilog)?",
    "id": "Pernyataan Konkuren (Terus Menerus)."
  },
  {
    "en": "Apa Itu Blocking Assignment (=) (Verilog)?",
    "id": "Eksekusi Berurutan (Memblokir)."
  },
  {
    "en": "Apa Itu Non-Blocking Assignment (<=) (Verilog)?",
    "id": "Eksekusi Paralel (Terjadwal)."
  },
  {
    "en": "Kapan Menggunakan Non-Blocking (<=) (Verilog)?",
    "id": "Pemodelan Rangkaian Sekuensial (Flip-Flop)."
  },
  {
    "en": "Kapan Menggunakan Blocking (=) (Verilog)?",
    "id": "Pemodelan Rangkaian Kombinasional."
  },
  {
    "en": "Apa Itu Instansiasi (Instantiation)?",
    "id": "Penggunaan Komponen (Modul/Entitas) Dalam Desain."
  },
  {
    "en": "Apa Itu Generic (VHDL (VHSIC Hardware Description Language)) / Parameter (Verilog)?",
    "id": "Konstanta Konfigurasi (Ukuran Bus, Dll)."
  },
  {
    "en": "Apa Itu Tipe Data STD_LOGIC_VECTOR?",
    "id": "Vektor (Bus) Tipe STD_LOGIC."
  },
  {
    "en": "Apa Itu Logika Tiga Keadaan (Tri-State)?",
    "id": "Output (1, 0, Atau Z (Impedansi Tinggi))."
  },
  {
    "en": "Apa Fungsi Buffer Tri-State (Tri-State Buffer)?",
    "id": "Mengizinkan Banyak Perangkat Berbagi Bus."
  },
  {
    "en": "Apa Itu Penahan Bus (Bus Holder)?",
    "id": "Menjaga Nilai Logika Bus (Mencegah Float)."
  },
  {
    "en": "Apa Itu Logika Emitter-Coupled (ECL)?",
    "id": "Keluarga Logika Sangat Cepat (Boros Daya)."
  },
  {
    "en": "Apa Itu Logika TTL (Transistor-Transistor Logic)?",
    "id": "Keluarga Logika Berbasis BJT (Bipolar Junction Transistor) (5V)."
  },
  {
    "en": "Apa Itu Logika CMOS (Complementary Metal-Oxide-Semiconductor)?",
    "id": "Keluarga Logika (Daya Rendah) (MOSFET (Metal Oxide Semiconductor FET))."
  },
  {
    "en": "Apa Itu Taksonomi Flynn (Flynn's Taxonomy)?",
    "id": "Klasifikasi Arsitektur Komputer Paralel."
  },
  {
    "en": "Apa Kepanjangan SISD (Single Instruction, Single Data)?",
    "id": "Single Instruction, Single Data."
  },
  {
    "en": "Apa Contoh Arsitektur SISD (Single Instruction, Single Data)?",
    "id": "Prosesor Unicore (Tunggal) Generasi Awal."
  },
  {
    "en": "Apa Kepanjangan SIMD (Single Instruction, Multiple Data)?",
    "id": "Single Instruction, Multiple Data."
  },
  {
    "en": "Aplikasi SIMD (Single Instruction, Multiple Data)?",
    "id": "Pemrosesan Grafis (GPU (Graphics Processing Unit)), Vektor."
  },
  {
    "en": "Apa Kepanjangan MISD (Multiple Instruction, Single Data)?",
    "id": "Multiple Instruction, Single Data."
  },
  {
    "en": "Contoh Arsitektur MISD (Multiple Instruction, Single Data)?",
    "id": "Jarang Ditemui (Beberapa Sistem Toleran Gagal)."
  },
  {
    "en": "Apa Kepanjangan MIMD (Multiple Instruction, Multiple Data)?",
    "id": "Multiple Instruction, Multiple Data."
  },
  {
    "en": "Contoh Arsitektur MIMD (Multiple Instruction, Multiple Data)?",
    "id": "Prosesor Multi-Core Modern."
  },
  {
    "en": "Apa Itu Koherensi (Coherence) Cache?",
    "id": "Konsistensi Data Antar Beberapa Cache."
  },
  {
    "en": "Apa Masalah Koherensi (Coherence) Cache?",
    "id": "Data Cache Menjadi Basi (Stale Data)."
  },
  {
    "en": "Apa Itu Protokol Koherensi (Coherence)?",
    "id": "Aturan Menjaga Konsistensi Data Cache."
  },
  {
    "en": "Apa Itu Protokol Berbasis Snooping (Snooping)?",
    "id": "Cache Memantau (Snoop) Transaksi Bus."
  },
  {
    "en": "Apa Itu Protokol Berbasis Direktori (Directory-Based)?",
    "id": "Direktori Terpusat Melacak Status Cache."
  },
  {
    "en": "Apa Kepanjangan Protokol MESI?",
    "id": "Modified, Exclusive, Shared, Invalid."
  },
  {
    "en": "Apa Status 'Modified' (MESI)?",
    "id": "Cache Diubah, Beda Memori, Salinan Tunggal."
  },
  {
    "en": "Apa Status 'Exclusive' (MESI)?",
    "id": "Cache Bersih, Sama Memori, Salinan Tunggal."
  },
  {
    "en": "Apa Status 'Shared' (MESI)?",
    "id": "Cache Bersih, Sama Memori, Banyak Salinan."
  },
  {
    "en": "Apa Status 'Invalid' (MESI)?",
    "id": "Baris Cache Tidak Valid (Basi)."
  },
  {
    "en": "Apa Itu False Sharing (Berbagi Palsu)?",
    "id": "Dua Data Beda, Satu Baris Cache (Invalidasi)."
  },
  {
    "en": "Apa Itu Kisi (Lattice) Kristal?",
    "id": "Susunan Atom Periodik Tiga Dimensi."
  },
  {
    "en": "Apa Itu Sel (Cell) Satuan (Unit Cell)?",
    "id": "Unit Berulang Terkecil Kisi Kristal."
  },
  {
    "en": "Struktur Kristal Silikon (Si)?",
    "id": "Kubik Intan (Diamond Cubic)."
  },
  {
    "en": "Struktur Kristal Galium Arsenida (GaAs)?",
    "id": "Zincblende (Mirip Intan)."
  },
  {
    "en": "Apa Itu Indeks Miller (Miller Indices)?",
    "id": "Notasi Bidang Orientasi Kristal."
  },
  {
    "en": "Apa Arti Bidang (100) (Silikon)?",
    "id": "Bidang Tegak Lurus Sumbu X."
  },
  {
    "en": "Apa Arti Arah <100> (Kristal)?",
    "id": "Arah Sepanjang Sumbu X."
  },
  {
    "en": "Apa Itu Proses Czochralski (CZ)?",
    "id": "Metode Penumbuhan Ingot Kristal Tunggal."
  },
  {
    "en": "Apa Itu Ingot (Ingot) Silikon?",
    "id": "Batangan Silikon Kristal Tunggal."
  },
  {
    "en": "Apa Itu Proses Float-Zone (FZ)?",
    "id": "Metode Pemurnian Ingot Silikon."
  },
  {
    "en": "Apa Itu Epitaksi (Epitaxy)?",
    "id": "Penumbuhan Lapisan Tipis Kristal Tunggal."
  },
  {
    "en": "Apa Kepanjangan MOCVD (Metalorganic CVD)?",
    "id": "Metalorganic Chemical Vapor Deposition."
  },
  {
    "en": "Apa Kepanjangan MBE (Molecular Beam Epitaxy)?",
    "id": "Molecular Beam Epitaxy."
  },
  {
    "en": "Apa Itu Cacat (Defect) Titik (Kristal)?",
    "id": "Cacat Pada Level Atom (Vakansi)."
  },
  {
    "en": "Apa Itu Vakansi (Vacancy) Kristal?",
    "id": "Kekosongan Atom Di Posisi Kisi."
  },
  {
    "en": "Apa Itu Interstisial (Interstitial) Kristal?",
    "id": "Atom Menyisip Di Antara Kisi."
  },
  {
    "en": "Apa Itu Dislokasi (Dislocation) Kristal?",
    "id": "Cacat Garis (Linear) Pada Kisi."
  },
  {
    "en": "Apa Itu Batas Butir (Grain Boundary)?",
    "id": "Batas Antar Orientasi Kristal Berbeda."
  },
  {
    "en": "Apa Itu Silikon Monokristalin?",
    "id": "Kristal Tunggal (Satu Orientasi Utuh)."
  },
  {
    "en": "Apa Itu Silikon Polikristalin?",
    "id": "Banyak Butir Kristal (Orientasi Acak)."
  },
  {
    "en": "Apa Itu Silikon Amorf (Amorphous)?",
    "id": "Tanpa Susunan Kisi Teratur (Gelas)."
  },
  {
    "en": "Apa Itu Celah Udara (Air Gap) Induktor?",
    "id": "Celah Non-Magnetik Pada Inti."
  },
  {
    "en": "Mengapa Induktor Perlu Celah Udara?",
    "id": "Menyimpan Energi, Mencegah Saturasi."
  },
  {
    "en": "Apa Itu Saturasi (Saturation) Inti Magnetik?",
    "id": "Inti Jenuh (Fluks Maksimal)."
  },
  {
    "en": "Apa Kurva B-H (B-H Loop)?",
    "id": "Kurva Histeresis (B Vs H)."
  },
  {
    "en": "Apa Itu Retentivitas (Retentivity) (Br)?",
    "id": "Sisa Kepadatan Fluks (Saat H=0)."
  },
  {
    "en": "Apa Itu Koersivitas (Coercivity) (Hc)?",
    "id": "Kekuatan Medan (Menghapus Sisa Magnet)."
  },
  {
    "en": "Apa Itu Rugi Inti (Core Loss)?",
    "id": "Rugi Histeresis Dan Rugi Arus Eddy."
  },
  {
    "en": "Apa Itu Rugi Histeresis (Hysteresis Loss)?",
    "id": "Energi Hilang Akibat Siklus Magnetisasi."
  },
  {
    "en": "Apa Itu Rugi Arus Eddy (Eddy Current Loss)?",
    "id": "Arus Pusar Terinduksi Di Inti."
  },
  {
    "en": "Bagaimana Mengurangi Rugi Arus Eddy?",
    "id": "Menggunakan Inti Laminasi (Berlapis)."
  },
  {
    "en": "Mengapa Ferit (Ferrite) Dipakai Frekuensi Tinggi?",
    "id": "Resistivitas Tinggi (Rugi Eddy Rendah)."
  },
  {
    "en": "Apa Itu Bahan Magnetik Lunak (Soft)?",
    "id": "Koersivitas Rendah (Mudah Dimagnetkan)."
  },
  {
    "en": "Apa Itu Bahan Magnetik Keras (Hard)?",
    "id": "Koersivitas Tinggi (Magnet Permanen)."
  },
  {
    "en": "Apa Itu Kontrol Kaskade (Cascade Control)?",
    "id": "Kontrol Bertingkat (Loop Luar Dalam)."
  },
  {
    "en": "Contoh Kontrol Kaskade (Cascade Control)?",
    "id": "Kontrol Posisi (Luar) Kontrol Kecepatan (Dalam)."
  },
  {
    "en": "Apa Itu Kontrol Umpan Maju (Feedforward)?",
    "id": "Kontrol Antisipatif (Mengukur Gangguan)."
  },
  {
    "en": "Apa Itu Kontrol Rasio (Ratio Control)?",
    "id": "Menjaga Rasio Antara Dua Variabel."
  },
  {
    "en": "Apa Itu Kontrol Jangkauan Terpisah (Split-Range)?",
    "id": "Satu Kontroler Mengatur Dua Aktuator."
  },
  {
    "en": "Apa Itu Kontrol Pengabaian (Override Control)?",
    "id": "Kontroler Selektif (Pilih Input Terendah/Tertinggi)."
  },
  {
    "en": "Apa Itu Kontroler Bang-Bang (On-Off)?",
    "id": "Kontroler Dua Posisi (Aktif/Mati)."
  },
  {
    "en": "Apa Itu Histeresis (Hysteresis) Kontroler On-Off?",
    "id": "Mencegah Osilasi Cepat (Chattering)."
  },
  {
    "en": "Apa Itu Penguat Transimpedansi (TIA)?",
    "id": "Penguat (Mengubah Arus Menjadi Tegangan)."
  },
  {
    "en": "Aplikasi Penguat Transimpedansi (TIA)?",
    "id": "Penerima Fotodioda."
  },
  {
    "en": "Apa Itu Penguat Transkonduktansi (OTA)?",
    "id": "Penguat (Mengubah Tegangan Menjadi Arus)."
  },
  {
    "en": "Apa Kepanjangan OTA (Operational Transconductance Amplifier)?",
    "id": "Operational Transconductance Amplifier."
  },
  {
    "en": "Apa Itu Komparator (Comparator) Jendela?",
    "id": "Mendeteksi Sinyal Dalam Rentang Tegangan."
  },
  {
    "en": "Apa Itu Penyearah (Rectifier) Presisi?",
    "id": "Penyearah (Tanpa Volt Drop) (Op-Amp)."
  },
  {
    "en": "Apa Itu Pengikut (Follower) Tegangan?",
    "id": "Op-Amp (Gain=1) (Penyangga Impedansi)."
  },
  {
    "en": "Apa Itu Penyangga (Buffer) Impedansi?",
    "id": "Impedansi Input Tinggi, Output Rendah."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Audio Kelas G?",
    "id": "Efisiensi Ditingkatkan (Rel Catu Daya Ganda)."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Audio Kelas H?",
    "id": "Efisiensi Ditingkatkan (Modulasi Rel Catu Daya)."
  },
  {
    "en": "Apa Itu Modulasi Pelacakan Selubung (Envelope Tracking)?",
    "id": "Catu Daya Dinamis (Efisiensi PA RF)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex d'?",
    "id": "Explosion Proof (Tahan Ledakan)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex e'?",
    "id": "Increased Safety (Keamanan Ditingkatkan)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex i'?",
    "id": "Intrinsic Safety (Aman Intrinsik)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex m'?",
    "id": "Encapsulation (Enkapsulasi)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex p'?",
    "id": "Pressurized (Bertekanan)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex o'?",
    "id": "Oil Immersion (Rendaman Minyak)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) 'Ex q'?",
    "id": "Powder Filling (Isian Pasir)."
  },
  {
    "en": "Apa Kepanjangan ATEX (ATmosphères EXplosibles)?",
    "id": "Direktif Eropa (Area Berbahaya)."
  },
  {
    "en": "Apa Kepanjangan IECEx (IEC Explosive)?",
    "id": "Sertifikasi Global (Area Berbahaya)."
  },
  {
    "en": "Apa Itu Grup (Group) Gas (ATEX)?",
    "id": "Klasifikasi Gas (Daya Ledak)."
  },
  {
    "en": "Apa Itu Kelas (Class) Suhu (ATEX)?",
    "id": "Batas Suhu Permukaan Maksimum."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe K?",
    "id": "Chromel - Alumel (Umum)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe J?",
    "id": "Iron (Besi) - Constantan (Konstantan)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe T?",
    "id": "Copper (Tembaga) - Constantan (Konstantan)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe E?",
    "id": "Chromel - Constantan (Konstantan) (Sensitif)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe R/S/B?",
    "id": "Platinum-Rhodium (Suhu Sangat Tinggi)."
  },
  {
    "en": "Apa Itu Termopil (Thermopile)?",
    "id": "Sensor (Gabungan Termokopel Seri)."
  },
  {
    "en": "Apa Itu RTD (Resistance Temperature Detector) Pt100?",
    "id": "Bahan Platinum (Hambatan 100 Ohm Di 0°C)."
  },
  {
    "en": "Apa Itu RTD (Resistance Temperature Detector) Pt1000?",
    "id": "Bahan Platinum (Hambatan 1000 Ohm Di 0°C)."
  },
  {
    "en": "Apa Itu Termistor (Thermistor) NTC (Negative Temperature Coefficient)?",
    "id": "Hambatan Turun Saat Suhu Naik."
  },
  {
    "en": "Apa Itu Termistor (Thermistor) PTC (Positive Temperature Coefficient)?",
    "id": "Hambatan Naik Saat Suhu Naik."
  },
  {
    "en": "Apa Itu Sensor Suhu (Sensor) IC (Integrated Circuit)?",
    "id": "Sensor Suhu (Output Tegangan/Digital)."
  },
  {
    "en": "Contoh Sensor Suhu (Sensor) IC (Integrated Circuit)?",
    "id": "LM35 (Tegangan Linear), DS18B20 (Digital)."
  },
  {
    "en": "Apa Itu Sintesis Pohon Clock (CTS)?",
    "id": "Proses Distribusi Clock (VLSI)."
  },
  {
    "en": "Tujuan Sintesis Pohon Clock (CTS)?",
    "id": "Meminimalkan Skew (Kemiringan) Clock."
  },
  {
    "en": "Apa Itu Buffer (Penyangga) Clock?",
    "id": "Penguat Sinyal Clock (Menjaga Slew)."
  },
  {
    "en": "Apa Itu Penjagaan Clock (Clock Gating)?",
    "id": "Mematikan Clock Modul Tidak Aktif."
  },
  {
    "en": "Tujuan Penjagaan Clock (Clock Gating)?",
    "id": "Mengurangi Disipasi Daya Dinamis."
  },
  {
    "en": "Apa Itu Sel Pengisi (Filler Cell)?",
    "id": "Sel Kosong (Mengisi Celah Layout)."
  },
  {
    "en": "Tujuan Sel Pengisi (Filler Cell)?",
    "id": "Konektivitas Rel Daya, Kepadatan Manufaktur."
  },
  {
    "en": "Apa Itu Verifikasi Fisik (Physical Verification)?",
    "id": "Pengecekan Akhir Desain Tata Letak."
  },
  {
    "en": "Apa Kepanjangan DRC (Design Rule Check)?",
    "id": "Design Rule Check (Aturan Manufaktur)."
  },
  {
    "en": "Apa Kepanjangan LVS (Layout Versus Schematic)?",
    "id": "Layout Versus Schematic."
  },
  {
    "en": "Fungsi LVS (Layout Versus Schematic)?",
    "id": "Memastikan Tata Letak Sesuai Skematik."
  },
  {
    "en": "Apa Kepanjangan ERC (Electrical Rule Check)?",
    "id": "Electrical Rule Check (Aturan Kelistrikan)."
  },
  {
    "en": "Apa Itu Ekstraksi Parasitik (Parasitic Extraction)?",
    "id": "Menghitung R C Parasitik Tata Letak."
  },
  {
    "en": "Apa Itu Simulasi (Simulation) Pasca-Tata Letak?",
    "id": "Simulasi Netlist (Termasuk Parasitik)."
  },
  {
    "en": "Apa Itu Sel (Cell) Standar (VLSI)?",
    "id": "Blok Logika Dasar (Gerbang, Flip-Flop)."
  },
  {
    "en": "Apa Itu Pitch (Kisar) Metal?",
    "id": "Jarak Antar Pusat Jalur Logam."
  },
  {
    "en": "Apa Itu Kontrol Skalar (Scalar Control)?",
    "id": "Kontrol V/f (Volt per Hertz) Motor AC (Alternating Current)."
  },
  {
    "en": "Apa Itu Kontrol Vektor (Vector Control)?",
    "id": "Kontrol FOC (Field Oriented Control) (Fluks Torsi)."
  },
  {
    "en": "Keunggulan Kontrol Vektor (Vector Control)?",
    "id": "Respon Torsi Cepat, Akurasi Tinggi."
  },
  {
    "en": "Kelemahan Kontrol Skalar (Scalar Control)?",
    "id": "Respon Torsi Lambat (Performa Rendah)."
  },
  {
    "en": "Apa Itu Estimator (Estimator) Fluks?",
    "id": "Estimasi Fluks Rotor (Kontrol Vektor)."
  },
  {
    "en": "Apa Itu Estimator (Estimator) Kecepatan?",
    "id": "Estimasi Kecepatan (Kontrol Tanpa Sensor)."
  },
  {
    "en": "Apa Itu Kerugian (Loss) Konduksi MOSFET?",
    "id": "I Kuadrat Kali RDS(on) (I² * RDS(on))."
  },
  {
    "en": "Apa Itu Kerugian (Loss) Switching MOSFET?",
    "id": "Kerugian Saat Transisi (Turn-On Turn-Off)."
  },
  {
    "en": "Apa Itu Kerugian (Loss) Dioda Bodi?",
    "id": "Kerugian Konduksi Dioda Internal MOSFET."
  },
  {
    "en": "Apa Itu Kerugian (Loss) Pemulihan Balik (Reverse Recovery)?",
    "id": "Kerugian Dioda (Trr)."
  },
  {
    "en": "Apa Itu Kerugian (Loss) Drive Gerbang?",
    "id": "Energi Mengisi Muatan Gerbang (Qg)."
  },
  {
    "en": "Apa Itu Kerugian (Loss) Konduksi IGBT?",
    "id": "Tegangan Saturasi (Vce_sat) Kali Arus (Ic)."
  },
  {
    "en": "Apa Itu Energi (Energy) Turn-On (E_on)?",
    "id": "Energi Hilang Saat Transisi ON."
  },
  {
    "en": "Apa Itu Energi (Energy) Turn-Off (E_off)?",
    "id": "Energi Hilang Saat Transisi OFF."
  },
  {
    "en": "Apa Kepanjangan IEC (International Electrotechnical Commission) 61000-4-2?",
    "id": "Standar Uji Imunitas ESD (Electrostatic Discharge)."
  },
  {
    "en": "Apa Kepanjangan IEC (International Electrotechnical Commission) 61000-4-4?",
    "id": "Standar Uji Imunitas EFT (Electrical Fast Transient)."
  },
  {
    "en": "Apa Kepanjangan IEC (International Electrotechnical Commission) 61000-4-5?",
    "id": "Standar Uji Imunitas Surja (Surge)."
  },
  {
    "en": "Apa Itu EFT (Electrical Fast Transient) / Burst?",
    "id": "Pulsa Transien Cepat Beruntun."
  },
  {
    "en": "Apa Itu Surja (Surge)?",
    "id": "Pulsa Transien Energi Tinggi (Petir)."
  },
  {
    "en": "Apa Kepanjangan LISN (Line Impedance Stabilization Network)?",
    "id": "Line Impedance Stabilization Network."
  },
  {
    "en": "Fungsi LISN (Line Impedance Stabilization Network)?",
    "id": "Alat Uji Emisi Terkonduksi (EMI)."
  },
  {
    "en": "Apa Itu Probe (Probe) Arus (EMI (Electromagnetic Interference))?",
    "id": "Sensor (Clamp-On) Ukur Arus Noise (Derau)."
  },
  {
    "en": "Apa Itu Probe (Probe) Medan Dekat (Near-Field)?",
    "id": "Mendeteksi Sumber Radiasi EMI (Electromagnetic Interference) Lokal."
  },
  {
    "en": "Apa Itu Antena Bikonikal (Biconical Antenna)?",
    "id": "Antena Uji EMC (Electromagnetic Compatibility) (Frekuensi Rendah)."
  },
  {
    "en": "Apa Itu Antena Log-Periodik (Log-Periodic)?",
    "id": "Antena Uji EMC (Electromagnetic Compatibility) (Bandwidth Lebar)."
  },
  {
    "en": "Apa Itu Antena Tanduk (Horn Antenna)?",
    "id": "Antena Uji EMC (Electromagnetic Compatibility) (Frekuensi Tinggi/Microwave)."
  },
  {
    "en": "Apa Itu Sel (Cell) GTEM (Gigahertz Transverse Electromagnetic)?",
    "id": "Ruang Uji EMC (Electromagnetic Compatibility) (Alternatif Ruang Anechoic)."
  },
  {
    "en": "Apa Itu Stirrer (Pengaduk) Mode (Ruang Gema)?",
    "id": "Baling-Baling Logam (Acak Medan RF)."
  },
  {
    "en": "Apa Itu Absorber (Penyerap) RF (Radio Frequency)?",
    "id": "Material Busa (Peredam Pantulan RF)."
  },
  {
    "en": "Apa Itu Elektroluminesensi (Electroluminescence)?",
    "id": "Emisi Cahaya Akibat Arus Listrik."
  },
  {
    "en": "Apa Itu Katodoluminesensi (Cathodoluminescence)?",
    "id": "Emisi Cahaya Akibat Tembakan Elektron."
  },
  {
    "en": "Contoh Katodoluminesensi (Cathodoluminescence)?",
    "id": "Layar Tabung CRT (Cathode Ray Tube)."
  },
  {
    "en": "Apa Itu Fosforesensi (Phosphorescence)?",
    "id": "Pendaran Cahaya Tertunda (Glow-In-The-Dark)."
  },
  {
    "en": "Apa Itu Fluoresensi (Fluorescence)?",
    "id": "Pendaran Cahaya Sesaat (Lampu TL)."
  },
  {
    "en": "Apa Itu Termoluminesensi (Thermoluminescence)?",
    "id": "Emisi Cahaya Saat Dipanaskan."
  },
  {
    "en": "Apa Itu Tipe (Type) Sistem Kontrol?",
    "id": "Jumlah Integrator Murni Di Loop."
  },
  {
    "en": "Apa Itu Sistem Tipe (Type) 0?",
    "id": "Tidak Ada Integrator (Error Step Konstan)."
  },
  {
    "en": "Apa Itu Sistem Tipe (Type) 1?",
    "id": "Satu Integrator (Error Step Nol)."
  },
  {
    "en": "Apa Itu Sistem Tipe (Type) 2?",
    "id": "Dua Integrator (Error Ramp Nol)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) Error Posisi Statis (Kp)?",
    "id": "Ukuran Error Step (Tipe 0)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) Error Kecepatan Statis (Kv)?",
    "id": "Ukuran Error Ramp (Tipe 1)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) Error Percepatan Statis (Ka)?",
    "id": "Ukuran Error Parabola (Tipe 2)."
  },
  {
    "en": "Error (Galat) Tunak (Input Step, Tipe 0)?",
    "id": "Konstan (1 / (1 + Kp))."
  },
  {
    "en": "Error (Galat) Tunak (Input Step, Tipe 1)?",
    "id": "Nol (0)."
  },
  {
    "en": "Error (Galat) Tunak (Input Ramp, Tipe 1)?",
    "id": "Konstan (1 / Kv)."
  },
  {
    "en": "Error (Galat) Tunak (Input Ramp, Tipe 2)?",
    "id": "Nol (0)."
  },
  {
    "en": "Apa Itu Aksi Kontrol Proporsional (P)?",
    "id": "Output Proporsional Error Saat Ini."
  },
  {
    "en": "Apa Itu Aksi Kontrol Integral (I)?",
    "id": "Output Proporsional Akumulasi Error Lalu."
  },
  {
    "en": "Apa Itu Aksi Kontrol Derivatif (D)?",
    "id": "Output Proporsional Laju Perubahan Error."
  },
  {
    "en": "Apa Itu Penyetelan (Tuning) PID (Proportional Integral Derivative)?",
    "id": "Proses Penentuan Parameter Kp, Ki, Kd."
  },
  {
    "en": "Apa Itu Osilasi (Oscillation) Sistem Kontrol?",
    "id": "Fluktuasi Output Di Sekitar Setpoint."
  },
  {
    "en": "Apa Itu Waktu Tunda (Dead Time) Proses?",
    "id": "Tundaan Waktu (Input Berubah Output Respon)."
  },
  {
    "en": "Apa Itu Arus (Current) Saturasi (Transistor BJT (Bipolar Junction Transistor))?",
    "id": "Arus Kolektor Maksimum (Vce Rendah)."
  },
  {
    "en": "Apa Itu Daerah (Region) Cut-Off (Transistor BJT (Bipolar Junction Transistor))?",
    "id": "Transistor Mati (Arus Kolektor Nol)."
  },
  {
    "en": "Apa Itu Daerah (Region) Aktif (Transistor BJT (Bipolar Junction Transistor))?",
    "id": "Transistor Bekerja Sebagai Penguat."
  },
  {
    "en": "Apa Itu Daerah (Region) Saturasi (Transistor BJT (Bipolar Junction Transistor))?",
    "id": "Transistor Bekerja Sebagai Saklar Tertutup."
  },
  {
    "en": "Apa Itu Daerah (Region) Aktif Mundur (Reverse-Active)?",
    "id": "Fungsi Emitor Kolektor Dibalik."
  },
  {
    "en": "Apa Itu Beta (β) Transistor BJT (Bipolar Junction Transistor)?",
    "id": "Penguatan Arus (Ic / Ib)."
  },
  {
    "en": "Apa Nama Lain Beta (β) BJT (Bipolar Junction Transistor)?",
    "id": "Hfe (hFE)."
  },
  {
    "en": "Apa Itu Alpha (α) Transistor BJT (Bipolar Junction Transistor)?",
    "id": "Rasio Arus (Ic / Ie)."
  },
  {
    "en": "Apa Hubungan Alpha (α) Dan Beta (β)?",
    "id": "Alpha = Beta / (Beta + 1)."
  },
  {
    "en": "Apa Itu Konfigurasi Common Emitter (CE)?",
    "id": "Input Basis, Output Kolektor (Gain Tinggi)."
  },
  {
    "en": "Apa Itu Konfigurasi Common Collector (CC)?",
    "id": "Input Basis, Output Emitor (Buffer)."
  },
  {
    "en": "Apa Nama Lain Konfigurasi Common Collector (CC)?",
    "id": "Pengikut Emitor (Emitter Follower)."
  },
  {
    "en": "Apa Itu Konfigurasi Common Base (CB)?",
    "id": "Input Emitor, Output Kolektor (Frekuensi Tinggi)."
  },
  {
    "en": "Apa Itu Garis Beban (Load Line) DC (Direct Current)?",
    "id": "Grafik Titik Operasi DC (Direct Current) Transistor."
  },
  {
    "en": "Apa Itu Titik-Q (Quiescent Point)?",
    "id": "Titik Operasi Transistor (Tanpa Sinyal)."
  },
  {
    "en": "Apa Itu Stabilitas Termal (Thermal Stability) Titik-Q?",
    "id": "Titik-Q Stabil Terhadap Perubahan Suhu."
  },
  {
    "en": "Apa Itu Pelarian Termal (Thermal Runaway)?",
    "id": "Kenaikan Suhu Rusak Transistor (Positif Feedback)."
  },
  {
    "en": "Apa Itu Bias (Biasing) Pembagi Tegangan?",
    "id": "Metode Pembiasan Titik-Q Paling Stabil."
  },
  {
    "en": "Apa Itu Bias (Biasing) Umpan Balik Kolektor?",
    "id": "Metode Pembiasan (Stabilitas Sedang)."
  },
  {
    "en": "Apa Itu Bias (Biasing) Tetap (Fixed Bias)?",
    "id": "Metode Pembiasan Paling Tidak Stabil."
  },
  {
    "en": "Apa Itu Model Hybrid-Pi (h-pi)?",
    "id": "Model Sinyal Kecil Frekuensi Tinggi BJT."
  },
  {
    "en": "Apa Itu Model Parameter-H (h-parameter)?",
    "id": "Model Sinyal Kecil BJT (Black Box)."
  },
  {
    "en": "Apa Itu Transkonduktansi (gm) (BJT (Bipolar Junction Transistor))?",
    "id": "Rasio Perubahan Ic Terhadap Vbe."
  },
  {
    "en": "Apa Itu Resistansi Emitor (re) Sinyal Kecil?",
    "id": "Resistansi Dinamis Persambungan Emitor."
  },
  {
    "en": "Apa Itu Efek Tembok Awal (Early Effect)?",
    "id": "Modulasi Lebar Basis (Tegangan Kolektor)."
  },
  {
    "en": "Apa Itu Tegangan Tembok Awal (Early Voltage) (VA)?",
    "id": "Parameter Kuantifikasi Efek Tembok Awal."
  },
  {
    "en": "Apa Itu Kapasitansi Difusi (Diffusion Capacitance) (BJT (Bipolar Junction Transistor))?",
    "id": "Kapasitansi Sambungan (Forward Bias)."
  },
  {
    "en": "Apa Itu Kapasitansi Transisi (Transition Capacitance) (BJT (Bipolar Junction Transistor))?",
    "id": "Kapasitansi Sambungan (Reverse Bias)."
  },
  {
    "en": "Apa Itu Frekuensi Transisi (fT)?",
    "id": "Frekuensi Saat Gain (Penguatan) Arus Beta = 1."
  },
  {
    "en": "Apa Itu Struktur Data (Data Structure)?",
    "id": "Cara Mengorganisasi Data Di Memori."
  },
  {
    "en": "Apa Itu Algoritma (Algorithm)?",
    "id": "Langkah Logis Menyelesaikan Masalah."
  },
  {
    "en": "Apa Itu Kompleksitas Waktu (Time Complexity)?",
    "id": "Ukuran Efisiensi Waktu Algoritma."
  },
  {
    "en": "Apa Itu Notasi Big-O (Big-O Notation)?",
    "id": "Notasi Asimtotik (Kasus Terburuk)."
  },
  {
    "en": "Apa Itu Kompleksitas O(1)?",
    "id": "Waktu Konstan (Sangat Cepat)."
  },
  {
    "en": "Apa Itu Kompleksitas O(n)?",
    "id": "Waktu Linear (Proporsional Input)."
  },
  {
    "en": "Apa Itu Kompleksitas O(n²)?",
    "id": "Waktu Kuadratik (Contoh: Bubble Sort)."
  },
  {
    "en": "Apa Itu Kompleksitas O(log n)?",
    "id": "Waktu Logaritmik (Contoh: Binary Search)."
  },
  {
    "en": "Apa Itu Kompleksitas O(n log n)?",
    "id": "Waktu (Contoh: Merge Sort, Quick Sort)."
  },
  {
    "en": "Apa Itu Struktur Data Array (Larik)?",
    "id": "Kumpulan Data (Indeks Berurutan)."
  },
  {
    "en": "Apa Itu Struktur Data Linked List (Daftar Tertaut)?",
    "id": "Kumpulan Data (Node, Pointer)."
  },
  {
    "en": "Apa Itu Struktur Data Stack (Tumpukan)?",
    "id": "LIFO (Last-In, First-Out)."
  },
  {
    "en": "Apa Itu Struktur Data Queue (Antrian)?",
    "id": "FIFO (First-In, First-Out)."
  },
  {
    "en": "Apa Itu Struktur Data Tree (Pohon)?",
    "id": "Struktur Hierarkis (Node, Edge)."
  },
  {
    "en": "Apa Itu Pohon Biner (Binary Tree)?",
    "id": "Pohon (Maksimal Dua Anak)."
  },
  {
    "en": "Apa Itu Pohon Pencarian Biner (BST)?",
    "id": "Pohon Biner (Kiri < Induk < Kanan)."
  },
  {
    "en": "Apa Itu Pohon AVL (AVL Tree)?",
    "id": "BST (Binary Search Tree) Seimbang Mandiri (Tinggi)."
  },
  {
    "en": "Apa Itu Pohon Merah-Hitam (Red-Black Tree)?",
    "id": "BST (Binary Search Tree) Seimbang Mandiri (Warna Node)."
  },
  {
    "en": "Apa Itu Struktur Data Hash Table (Tabel Hash)?",
    "id": "Struktur (Pencarian Cepat Kunci-Nilai)."
  },
  {
    "en": "Apa Itu Fungsi Hash (Hash Function)?",
    "id": "Memetakan Kunci Ke Indeks Array."
  },
  {
    "en": "Apa Itu Kolisi (Collision) Hash?",
    "id": "Dua Kunci Menghasilkan Indeks Sama."
  },
  {
    "en": "Apa Itu Struktur Data Graf (Graph)?",
    "id": "Kumpulan Simpul (Vertex) Sisi (Edge)."
  },
  {
    "en": "Apa Itu Algoritma BFS (Breadth-First Search)?",
    "id": "Penelusuran Graf (Level Per Level)."
  },
  {
    "en": "Apa Itu Algoritma DFS (Depth-First Search)?",
    "id": "Penelusuran Graf (Mendalam Dahulu)."
  },
  {
    "en": "Apa Itu Algoritma Dijkstra (Dijkstra)?",
    "id": "Algoritma Pencarian Jalur Terpendek."
  },
  {
    "en": "Apa Kepanjangan SoC (System on a Chip)?",
    "id": "System on a Chip."
  },
  {
    "en": "Apa Beda SoC (System on a Chip) Dan Mikrokontroler?",
    "id": "SoC (System on a Chip) (Lebih Kompleks, CPU (Central Processing Unit) Aplikasi)."
  },
  {
    "en": "Apa Itu Hard Processor Core (SoC)?",
    "id": "Core (Inti) Prosesor Fisik (Contoh: ARM (Advanced RISC Machine))."
  },
  {
    "en": "Apa Itu Soft Processor Core (SoC)?",
    "id": "Core (Inti) Prosesor Logika (Implementasi FPGA (Field Programmable Gate Array))."
  },
  {
    "en": "Apa Itu Bus (Bus) Interkoneksi SoC?",
    "id": "Jalur Komunikasi Internal Chip."
  },
  {
    "en": "Contoh Bus (Bus) Interkoneksi SoC?",
    "id": "AMBA (Advanced Microcontroller Bus Architecture) (AXI (Advanced eXtensible Interface), APB (Advanced Peripheral Bus))."
  },
  {
    "en": "Apa Kepanjangan IP (Intellectual Property) Core (VLSI)?",
    "id": "Intellectual Property Core (Blok Desain Siap Pakai)."
  },
  {
    "en": "Apa Itu Hard IP (Intellectual Property) Core?",
    "id": "IP (Intellectual Property) Fisik (Tata Letak Tetap)."
  },
  {
    "en": "Apa Itu Soft IP (Intellectual Property) Core?",
    "id": "IP (Intellectual Property) Logika (Kode RTL (Register Transfer Level))."
  },
  {
    "en": "Beda Utama CPLD (Complex Programmable Logic Device) FPGA (Field Programmable Gate Array)?",
    "id": "Arsitektur (CPLD (Complex Programmable Logic Device) (SOP (Sum of Products)), FPGA (Field Programmable Gate Array) (LUT (Look-Up Table)))."
  },
  {
    "en": "Mana Lebih Baik (Kepadatan Logika)?",
    "id": "FPGA (Field Programmable Gate Array) (Kepadatan Sangat Tinggi)."
  },
  {
    "en": "Mana Lebih Baik (Prediktabilitas Waktu)?",
    "id": "CPLD (Complex Programmable Logic Device) (Tunda Waktu Lebih Deterministik)."
  },
  {
    "en": "Mana Menyimpan Konfigurasi (Volatil)?",
    "id": "FPGA (Field Programmable Gate Array) (Perlu Boot ROM (Read Only Memory))."
  },
  {
    "en": "Mana Menyimpan Konfigurasi (Non-Volatil)?",
    "id": "CPLD (Complex Programmable Logic Device) (EEPROM (Electrically Erasable Programmable Read Only Memory)/Flash Internal)."
  },
  {
    "en": "Apa Arsitektur CPLD (Complex Programmable Logic Device)?",
    "id": "Blok Logika (Macrocell) Terhubung Matriks."
  },
  {
    "en": "Apa Arsitektur FPGA (Field Programmable Gate Array)?",
    "id": "Array Blok Logika (CLB (Configurable Logic Block)) Matriks Perutean."
  },
  {
    "en": "Apa Itu Medium Lossless (Tanpa Rugi)?",
    "id": "Medium (Konduktivitas Nol, σ=0)."
  },
  {
    "en": "Apa Itu Medium Lossy (Berugi)?",
    "id": "Medium (Konduktivitas Tidak Nol, σ≠0)."
  },
  {
    "en": "Apa Itu Konduktor Sempurna (Perfect Conductor)?",
    "id": "Medium (Konduktivitas Tak Terhingga)."
  },
  {
    "en": "Apa Itu Konstanta Atenuasi (α)?",
    "id": "Ukuran Pelemahan Gelombang (Medium Rugi)."
  },
  {
    "en": "Apa Itu Konstanta Fasa (β)?",
    "id": "Ukuran Pergeseran Fasa Gelombang."
  },
  {
    "en": "Apa Itu Konstanta Propagasi (γ)?",
    "id": "Ukuran Total (γ = α + jβ)."
  },
  {
    "en": "Apa Itu Impedansi Intrinsik (η)?",
    "id": "Rasio Medan Listrik (E) Medan Magnet (H)."
  },
  {
    "en": "Impedansi Intrinsik Ruang Hampa (η0)?",
    "id": "Sekitar 377 Ohm (120π)."
  },
  {
    "en": "Impedansi Intrinsik Konduktor Sempurna?",
    "id": "Nol (0)."
  },
  {
    "en": "Apa Itu Kedalaman Kulit (Skin Depth) (δ)?",
    "id": "Kedalaman Penetrasi Gelombang (Medium Rugi)."
  },
  {
    "en": "Kedalaman Kulit (Skin Depth) (Konduktor Baik)?",
    "id": "Berbanding Terbalik Akar Frekuensi."
  },
  {
    "en": "Apa Itu Gelombang Bidang (Plane Wave)?",
    "id": "Gelombang (Medan E H Seragam Bidang Konstan)."
  },
  {
    "en": "Apa Itu Gelombang TEM (Transverse Electromagnetic)?",
    "id": "Medan E H Tegak Lurus Arah Rambat."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Gelombang?",
    "id": "Orientasi Osilasi Vektor Medan Listrik (E)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Linear?",
    "id": "Osilasi Medan E Satu Garis Lurus."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Sirkular?",
    "id": "Osilasi Medan E (Magnitudo Konstan, Berputar)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Eliptikal?",
    "id": "Osilasi Medan E (Magnitudo Variabel, Berputar)."
  },
  {
    "en": "Apa Itu Efek Doppler (Doppler Effect)?",
    "id": "Pergeseran Frekuensi (Gerakan Sumber/Pengamat)."
  },
  {
    "en": "Apa Itu Pergeseran Biru (Blueshift) (Doppler)?",
    "id": "Frekuensi Naik (Mendekat)."
  },
  {
    "en": "Apa Itu Pergeseran Merah (Redshift) (Doppler)?",
    "id": "Frekuensi Turun (Menjauh)."
  },
  {
    "en": "Apa Itu Inverter Multi-Level (MLI)?",
    "id": "Inverter (Output Bentuk Tangga)."
  },
  {
    "en": "Keuntungan Inverter Multi-Level (MLI)?",
    "id": "Distorsi (THD) Rendah, Tegangan Tinggi."
  },
  {
    "en": "Apa Topologi NPC (Neutral Point Clamped)?",
    "id": "MLI (Multi-Level Inverter) (Dioda Penjepit Netral)."
  },
  {
    "en": "Apa Topologi Kapasitor Terbang (Flying Capacitor)?",
    "id": "MLI (Multi-Level Inverter) (Kapasitor Transfer Muatan)."
  },
  {
    "en": "Apa Topologi CHB (Cascaded H-Bridge)?",
    "id": "MLI (Multi-Level Inverter) (Jembatan-H Seri)."
  },
  {
    "en": "Keuntungan Topologi CHB (Cascaded H-Bridge)?",
    "id": "Modular, Mudah Diperluas Levelnya."
  },
  {
    "en": "Apa Teknik Modulasi (Modulation) MLI?",
    "id": "PWM (Pulse Width Modulation) Sinusoidal Multi-Level."
  },
  {
    "en": "Apa Kepanjangan SVM (Space Vector Modulation)?",
    "id": "Space Vector Modulation."
  },
  {
    "en": "Apa Kepanjangan SHE (Selective Harmonic Elimination)?",
    "id": "Selective Harmonic Elimination (Eliminasi Harmonisa)."
  },
  {
    "en": "Apa Itu Jembatan (Bridge) Pengukur LCR?",
    "id": "Jembatan Wheatstone Versi AC (Alternating Current)."
  },
  {
    "en": "Apa Prinsip Ukur LCR (Inductance Capacitance Resistance) Meter?",
    "id": "Mengukur Impedansi Kompleks (Z)."
  },
  {
    "en": "Apa Model Seri (LCR (Inductance Capacitance Resistance))?",
    "id": "Resistansi Seri (ESR (Equivalent Series Resistance)) Kapasitansi/Induktansi."
  },
  {
    "en": "Apa Model Paralel (LCR (Inductance Capacitance Resistance))?",
    "id": "Resistansi Paralel (EPR (Equivalent Parallel Resistance)) Kapasitansi/Induktansi."
  },
  {
    "en": "Kapan Model Seri Dipakai?",
    "id": "Komponen Impedansi Rendah (Kapasitor Besar)."
  },
  {
    "en": "Kapan Model Paralel Dipakai?",
    "id": "Komponen Impedansi Tinggi (Kapasitor Kecil)."
  },
  {
    "en": "Apa Itu Faktor Disipasi (DF)?",
    "id": "Ukuran Kerugian (Loss) Kapasitor."
  },
  {
    "en": "Apa Rumus Faktor Disipasi (DF)?",
    "id": "DF = ESR (Equivalent Series Resistance) / Xc (Reaktansi Kapasitif)."
  },
  {
    "en": "Apa Itu Faktor Kualitas (Q)?",
    "id": "Ukuran Kualitas Induktor (Kebalikan DF)."
  },
  {
    "en": "Apa Rumus Faktor Kualitas (Q) Induktor?",
    "id": "Q = Xl (Reaktansi Induktif) / ESR (Equivalent Series Resistance)."
  },
  {
    "en": "Apa Itu Pengukuran 4-Terminal (Kelvin)?",
    "id": "Metode Ukur (Eliminasi Resistansi Kabel Probe)."
  },
  {
    "en": "Apa Itu Frekuensi Uji (Test Frequency)?",
    "id": "Frekuensi AC (Alternating Current) (Ukur Impedansi)."
  },
  {
    "en": "Apa Itu Level Uji (Test Level)?",
    "id": "Amplitudo Tegangan Uji AC (Alternating Current)."
  },
  {
    "en": "Apa Itu Bias DC (Direct Current) (LCR (Inductance Capacitance Resistance) Meter)?",
    "id": "Menambah Arus/Tegangan DC (Direct Current) (Uji Kondisi Aktual)."
  },
  {
    "en": "Apa Kepanjangan THD+N (Total Harmonic Distortion + Noise)?",
    "id": "Total Harmonic Distortion plus Noise."
  },
  {
    "en": "Apa Itu SINAD (Signal-to-Noise and Distortion)?",
    "id": "Signal-to-Noise and Distortion Ratio."
  },
  {
    "en": "Apa Hubungan THD+N (Total Harmonic Distortion + Noise) SINAD (Signal-to-Noise and Distortion)?",
    "id": "Berbanding Terbalik (SINAD (Signal-to-Noise and Distortion) = 1 / (THD+N))."
  },
  {
    "en": "Apa Itu Crosstalk (Silang Bicara) (Audio)?",
    "id": "Kebocoran Sinyal Antar Kanal (Kiri Kanan)."
  },
  {
    "en": "Apa Itu Respon Frekuensi (Frequency Response)?",
    "id": "Pengukuran Gain (Penguatan) (Rentang Frekuensi Audio)."
  },
  {
    "en": "Apa Itu IMD (Intermodulation Distortion) (Audio)?",
    "id": "Distorsi Akibat Interaksi Dua Nada (Frek Baru)."
  },
  {
    "en": "Apa Itu Dynamic Range (Rentang Dinamis) (Audio)?",
    "id": "Rasio Sinyal Terkuat Terhadap Noise (Derau) Floor."
  },
  {
    "en": "Apa Itu SQL (Structured Query Language)?",
    "id": "Structured Query Language (Bahasa Kueri)."
  },
  {
    "en": "Apa Kepanjangan DDL (Data Definition Language)?",
    "id": "Data Definition Language (Bahasa Definisi Data)."
  },
  {
    "en": "Apa Kepanjangan DML (Data Manipulation Language)?",
    "id": "Data Manipulation Language (Bahasa Manipulasi Data)."
  },
  {
    "en": "Apa Kepanjangan DCL (Data Control Language)?",
    "id": "Data Control Language (Bahasa Kontrol Data)."
  },
  {
    "en": "Apa Itu Perintah SELECT (SQL)?",
    "id": "Perintah Mengambil Data Dari Tabel."
  },
  {
    "en": "Apa Itu Perintah INSERT (SQL)?",
    "id": "Perintah Memasukkan Baris Baru Data."
  },
  {
    "en": "Apa Itu Perintah UPDATE (SQL)?",
    "id": "Perintah Memodifikasi Data Yang Ada."
  },
  {
    "en": "Apa Itu Perintah DELETE (SQL)?",
    "id": "Perintah Menghapus Baris Data."
  },
  {
    "en": "Apa Itu Kunci Primer (Primary Key)?",
    "id": "Kolom Identifikasi Unik Baris Tabel."
  },
  {
    "en": "Apa Itu Kunci Asing (Foreign Key)?",
    "id": "Kolom Penghubung Relasi Antar Tabel."
  },
  {
    "en": "Apa Itu Normalisasi (Normalization) Database?",
    "id": "Proses Mengurangi Redundansi Data."
  },
  {
    "en": "Apa Itu Bentuk Normal Pertama (1NF)?",
    "id": "Setiap Sel Tabel Bernilai Atomik."
  },
  {
    "en": "Apa Itu Bentuk Normal Kedua (2NF)?",
    "id": "Memenuhi 1NF (Dependensi Penuh Kunci Primer)."
  },
  {
    "en": "Apa Itu Bentuk Normal Ketiga (3NF)?",
    "id": "Memenuhi 2NF (Tanpa Dependensi Transitif)."
  },
  {
    "en": "Apa Itu Properti ACID (Database)?",
    "id": "Atomicity, Consistency, Isolation, Durability."
  },
  {
    "en": "Apa Itu Atomisitas (Atomicity) (ACID)?",
    "id": "Transaksi (Berhasil Semua Atau Gagal Semua)."
  },
  {
    "en": "Apa Itu Konsistensi (Consistency) (ACID)?",
    "id": "Transaksi (Database Tetap Valid)."
  },
  {
    "en": "Apa Itu Isolasi (Isolation) (ACID)?",
    "id": "Transaksi (Tidak Saling Mengganggu)."
  },
  {
    "en": "Apa Itu Durabilitas (Durability) (ACID)?",
    "id": "Transaksi (Perubahan Data Persisten)."
  },
  {
    "en": "Apa Itu SQL (Structured Query Language) JOIN (Gabungan)?",
    "id": "Menggabungkan Baris Dari Dua Tabel."
  },
  {
    "en": "Apa Itu INNER (Dalam) JOIN?",
    "id": "Hanya Baris Yang Cocok Di Kedua Tabel."
  },
  {
    "en": "Apa Itu LEFT (Kiri) JOIN?",
    "id": "Semua Baris Tabel Kiri (Cocok Kanan)."
  },
  {
    "en": "Apa Itu RIGHT (Kanan) JOIN?",
    "id": "Semua Baris Tabel Kanan (Cocok Kiri)."
  },
  {
    "en": "Apa Itu FULL OUTER (Luar Penuh) JOIN?",
    "id": "Semua Baris (Kiri Kanan, Cocok Maupun Tidak)."
  },
  {
    "en": "Apa Itu Indeks (Index) Database?",
    "id": "Struktur Data (Mempercepat Pencarian Kueri)."
  },
  {
    "en": "Apa Itu Database NoSQL (NoSQL)?",
    "id": "Database Non-Relasional."
  },
  {
    "en": "Apa Itu Database Dokumen (Document Database)?",
    "id": "Database NoSQL (NoSQL) (Menyimpan Dokumen JSON (JavaScript Object Notation))."
  },
  {
    "en": "Apa Itu Penyimpanan Kunci-Nilai (Key-Value Store)?",
    "id": "Database NoSQL (NoSQL) (Kamus Sederhana)."
  },
  {
    "en": "Apa Itu Database Graf (Graph Database)?",
    "id": "Database NoSQL (NoSQL) (Node Relasi)."
  },
  {
    "en": "Apa Itu Database Kolom (Columnar Database)?",
    "id": "Database NoSQL (NoSQL) (Menyimpan Data Per Kolom)."
  },
  {
    "en": "Apa Kepanjangan SDLC (Software Development Life Cycle)?",
    "id": "Software Development Life Cycle."
  },
  {
    "en": "Apa Itu Model Air Terjun (Waterfall Model)?",
    "id": "Model SDLC (Software Development Life Cycle) (Linear Sekuensial)."
  },
  {
    "en": "Apa Itu Model Tangkas (Agile Model)?",
    "id": "Model SDLC (Software Development Life Cycle) (Iteratif Inkremental)."
  },
  {
    "en": "Apa Itu Scrum (Scrum)?",
    "id": "Kerangka Kerja (Framework) Manajemen Agile."
  },
  {
    "en": "Apa Itu Sprint (Scrum)?",
    "id": "Iterasi Waktu Tetap (Pengembangan Fitur)."
  },
  {
    "en": "Apa Itu Tumpukan (Backlog) Produk?",
    "id": "Daftar Fitur Kebutuhan Produk."
  },
  {
    "en": "Apa Itu Tumpukan (Backlog) Sprint?",
    "id": "Pekerjaan Yang Dipilih Untuk Satu Sprint."
  },
  {
    "en": "Apa Itu Scrum Master (Scrum Master)?",
    "id": "Fasilitator Pemimpin Pelayan Tim Scrum."
  },
  {
    "en": "Apa Itu Pemilik Produk (Product Owner)?",
    "id": "Perwakilan Pemangku Kepentingan (Visi Produk)."
  },
  {
    "en": "Apa Itu Model V (V-Model)?",
    "id": "Model SDLC (Software Development Life Cycle) (Verifikasi Validasi)."
  },
  {
    "en": "Apa Itu DevOps (DevOps)?",
    "id": "Kultur Filosofi (Development Operations)."
  },
  {
    "en": "Apa Itu CI/CD (Continuous Integration/Continuous Delivery)?",
    "id": "Integrasi Pengiriman Berkelanjutan."
  },
  {
    "en": "Apa Itu Pengujian Unit (Unit Testing)?",
    "id": "Pengujian Bagian Kode Terkecil."
  },
  {
    "en": "Apa Itu Pengujian Integrasi (Integration Testing)?",
    "id": "Pengujian Gabungan Antar Modul."
  },
  {
    "en": "Apa Itu Pengujian Sistem (System Testing)?",
    "id": "Pengujian Sistem Lengkap Terintegrasi."
  },
  {
    "en": "Apa Itu Pengujian Penerimaan (Acceptance Testing)?",
    "id": "Pengujian Validasi Kebutuhan Pengguna."
  },
  {
    "en": "Apa Itu Pengujian Regresi (Regression Testing)?",
    "id": "Memastikan Perubahan Tidak Merusak Fitur Lama."
  },
  {
    "en": "Apa Itu Pengujian Kotak Hitam (Black-Box)?",
    "id": "Pengujian Fungsionalitas (Tanpa Kode Internal)."
  },
  {
    "en": "Apa Itu Pengujian Kotak Putih (White-Box)?",
    "id": "Pengujian Struktural (Melihat Kode Internal)."
  },
  {
    "en": "Apa Itu Algoritma Pencarian A* (A-Star)?",
    "id": "Algoritma Pencarian Graf (Estimasi Heuristik)."
  },
  {
    "en": "Apa Itu Fungsi Heuristik (Heuristic Function)?",
    "id": "Fungsi Estimasi Biaya Menuju Target."
  },
  {
    "en": "Apa Kepanjangan SLAM (Simultaneous Localization and Mapping)?",
    "id": "Simultaneous Localization and Mapping (Robotika)."
  },
  {
    "en": "Apa Itu Filter Partikel (Particle Filter)?",
    "id": "Algoritma Estimasi Non-Linear (Monte Carlo)."
  },
  {
    "en": "Apa Kepanjangan LIDAR (Light Detection and Ranging)?",
    "id": "Light Detection and Ranging (Sensor Laser)."
  },
  {
    "en": "Apa Itu Odometri (Odometry)?",
    "id": "Estimasi Posisi Berdasar Gerakan (Roda)."
  },
  {
    "en": "Apa Itu Pemrograman Berorientasi Objek (OOP)?",
    "id": "Object-Oriented Programming (Paradigma)."
  },
  {
    "en": "Apa Itu Kelas (Class) (OOP (Object-Oriented Programming))?",
    "id": "Cetakan (Blueprint) Pembuat Objek."
  },
  {
    "en": "Apa Itu Objek (Object) (OOP (Object-Oriented Programming))?",
    "id": "Instans (Wujud Nyata) Dari Kelas."
  },
  {
    "en": "Apa Itu Enkapsulasi (Encapsulation) (OOP (Object-Oriented Programming))?",
    "id": "Pembungkusan Data Metode (Satu Unit)."
  },
  {
    "en": "Apa Itu Pewarisan (Inheritance) (OOP (Object-Oriented Programming))?",
    "id": "Pewarisan Sifat (Kelas Induk Anak)."
  },
  {
    "en": "Apa Itu Polimorfisme (Polymorphism) (OOP (Object-Oriented Programming))?",
    "id": "Konsep 'Banyak Bentuk' (Metode Beda Perilaku)."
  },
  {
    "en": "Apa Itu Abstraksi (Abstraction) (OOP (Object-Oriented Programming))?",
    "id": "Menyembunyikan Implementasi Kompleks."
  },
  {
    "en": "Apa Itu Perangkat Tegar (Firmware)?",
    "id": "Perangkat Lunak Tertanam (Hardware)."
  },
  {
    "en": "Apa Itu Pemuat Boot (Bootloader)?",
    "id": "Program Inisialisasi Pemuat Sistem Operasi."
  },
  {
    "en": "Apa Itu Sistem Operasi (Operating System)?",
    "id": "Perangkat Lunak Pengelola Sumber Daya."
  },
  {
    "en": "Apa Itu Kernel (Kernel) (OS (Operating System))?",
    "id": "Inti Utama Sistem Operasi."
  },
  {
    "en": "Apa Kepanjangan RTOS (Real-Time Operating System)?",
    "id": "Real-Time Operating System."
  },
  {
    "en": "Apa Itu Waktu Nyata Keras (Hard Real-Time)?",
    "id": "Batas Waktu (Deadline) Absolut (Tidak Boleh Gagal)."
  },
  {
    "en": "Apa Itu Waktu Nyata Lunak (Soft Real-Time)?",
    "id": "Batas Waktu (Deadline) Prioritas (Boleh Gagal)."
  },
  {
    "en": "Apa Itu Utas (Thread) (Komputasi)?",
    "id": "Unit Eksekusi Terkecil (Dalam Proses)."
  },
  {
    "en": "Apa Itu Proses (Process) (Komputasi)?",
    "id": "Program Yang Sedang Dieksekusi."
  },
  {
    "en": "Apa Itu Konkurensi (Concurrency)?",
    "id": "Banyak Tugas Berjalan (Tampak Bersamaan)."
  },
  {
    "en": "Apa Itu Paralelisme (Parallelism)?",
    "id": "Banyak Tugas Berjalan (Aktual Bersamaan)."
  },
  {
    "en": "Apa Itu Kebuntuan (Deadlock)?",
    "id": "Kondisi Saling Menunggu (Terkunci)."
  },
  {
    "en": "Apa Itu Semafor (Semaphore)?",
    "id": "Variabel Sinkronisasi (Kontrol Akses Sumber Daya)."
  },
  {
    "en": "Apa Itu Muteks (Mutex/Mutual Exclusion)?",
    "id": "Kunci (Lock) Sinkronisasi (Eksklusi Mutual)."
  },
  {
    "en": "Apa Itu Kondisi Pacu (Race Condition)?",
    "id": "Hasil Akhir Tergantung Urutan Eksekusi."
  },
  {
    "en": "Apa Itu Efek Faraday (Faraday Effect)?",
    "id": "Rotasi Polarisasi Cahaya (Medan Magnet)."
  },
  {
    "en": "Apa Itu Efek Magneto-Optik (Magneto-Optic)?",
    "id": "Interaksi Cahaya Bahan Termagnetisasi."
  },
  {
    "en": "Apa Itu Efek Akusto-Optik (Acousto-Optic)?",
    "id": "Interaksi Cahaya Gelombang Akustik."
  },
  {
    "en": "Apa Itu Modulator Akusto-Optik (AOM)?",
    "id": "Perangkat Modulasi Cahaya (Gelombang Suara)."
  },
  {
    "en": "Apa Itu Modulator Elektro-Optik (EOM)?",
    "id": "Perangkat Modulasi Cahaya (Medan Listrik)."
  },
  {
    "en": "Apa Itu Fototransistor (Phototransistor)?",
    "id": "Transistor (Basis Peka Cahaya)."
  },
  {
    "en": "Apa Itu Motor DC (Direct Current) Ber-Sikat (Brushed)?",
    "id": "Motor (Komutasi Mekanis Sikat Karbon)."
  },
  {
    "en": "Apa Itu Motor DC (Direct Current) Tanpa Sikat (Brushless)?",
    "id": "Motor (Komutasi Elektronik Sensor Hall)."
  },
  {
    "en": "Apa Itu Komutasi Trapesoidal (Trapezoidal)?",
    "id": "Metode Drive BLDC (Brushless DC) (Sederhana)."
  },
  {
    "en": "Apa Itu Komutasi Sinusoidal (Sinusoidal)?",
    "id": "Metode Drive PMSM (Permanent Magnet Synchronous Motor) (Halus)."
  },
  {
    "en": "Bentuk GGL (Gaya Gerak Listrik) Balik (Back-EMF) BLDC (Brushless DC)?",
    "id": "Trapesoidal (Trapezoidal)."
  },
  {
    "en": "Bentuk GGL (Gaya Gerak Listrik) Balik (Back-EMF) PMSM (Permanent Magnet Synchronous Motor)?",
    "id": "Sinusoidal (Sinusoidal)."
  },
  {
    "en": "Apa Itu Motor Sinkron (Synchronous Motor)?",
    "id": "Kecepatan Rotor = Kecepatan Sinkron."
  },
  {
    "en": "Apa Itu Motor Asinkron (Asynchronous Motor)?",
    "id": "Kecepatan Rotor < Kecepatan Sinkron (Slip)."
  },
  {
    "en": "Nama Lain Motor Asinkron (Asynchronous Motor)?",
    "id": "Motor Induksi (Induction Motor)."
  },
  {
    "en": "Apa Itu Rotor Sangkar Tupai (Squirrel Cage)?",
    "id": "Rotor (Batang Konduktor Hubung Singkat)."
  },
  {
    "en": "Apa Itu Rotor Lilit (Wound Rotor)?",
    "id": "Rotor (Belitan Tiga Fasa, Cincin Geser)."
  },
  {
    "en": "Apa Itu Starting (Pengasutan) Langsung (DOL)?",
    "id": "Direct On-Line (Arus Start Tinggi)."
  },
  {
    "en": "Apa Itu Starting (Pengasutan) Bintang-Segitiga (Star-Delta)?",
    "id": "Mengurangi Tegangan Arus Awal."
  },
  {
    "en": "Apa Itu Soft Starter (Pengasut Lunak)?",
    "id": "Starting (Pengasutan) Elektronik (Ramp Tegangan)."
  },
  {
    "en": "Apa Kepanjangan VFD (Variable Frequency Drive)?",
    "id": "Variable Frequency Drive."
  },
  {
    "en": "Apa Fungsi VFD (Variable Frequency Drive)?",
    "id": "Mengatur Kecepatan Motor (Frekuensi)."
  },
  {
    "en": "Apa Struktur Sel SRAM (Static RAM) 6T?",
    "id": "Enam Transistor (Dua Inverter Silang)."
  },
  {
    "en": "Apa Fungsi Transistor Akses (Access) SRAM?",
    "id": "Menghubungkan Bit Line Ke Latch."
  },
  {
    "en": "Apa Struktur Sel DRAM (Dynamic RAM) 1T1C?",
    "id": "Satu Transistor Satu Kapasitor."
  },
  {
    "en": "Apa Fungsi Kapasitor DRAM (Dynamic RAM)?",
    "id": "Menyimpan Muatan (Bit 0 Atau 1)."
  },
  {
    "en": "Mengapa DRAM (Dynamic RAM) Perlu Refresh (Penyegaran)?",
    "id": "Kapasitor Kehilangan Muatan (Bocor)."
  },
  {
    "en": "Apa Itu Siklus Refresh (Refresh Cycle) DRAM?",
    "id": "Proses Membaca Menulis Ulang Data."
  },
  {
    "en": "Apa Topologi Konverter Forward (Forward)?",
    "id": "Konverter Isolasi (Buck (Penurun) Primer)."
  },
  {
    "en": "Apa Fungsi Lilitan Reset (Reset Winding) Forward?",
    "id": "Mengosongkan Fluks Magnetik Inti."
  },
  {
    "en": "Apa Topologi Konverter Jembatan-Setengah (Half-Bridge)?",
    "id": "Konverter Isolasi (Dua Saklar)."
  },
  {
    "en": "Apa Topologi Konverter Jembatan-Penuh (Full-Bridge)?",
    "id": "Konverter Isolasi (Empat Saklar)."
  },
  {
    "en": "Apa Keuntungan Jembatan-Penuh (Full-Bridge)?",
    "id": "Daya Tinggi (Pemanfaatan Trafo Penuh)."
  },
  {
    "en": "Apa Itu Konverter Push-Pull (Push-Pull)?",
    "id": "Konverter Isolasi (Trafo Center-Tap)."
  },
  {
    "en": "Apa Itu Diskretisasi (Discretization) Kontroler?",
    "id": "Mengubah Kontroler Kontinu Ke Digital."
  },
  {
    "en": "Apa Metode Tustin (Transformasi Bilinear)?",
    "id": "Metode Diskretisasi (Trapezoidal)."
  },
  {
    "en": "Apa Metode Euler (Euler's Method) (Diskretisasi)?",
    "id": "Metode Diskretisasi (Beda Maju/Mundur)."
  },
  {
    "en": "Apa Itu Zero-Order Hold (ZOH)?",
    "id": "Menahan Sampel Sinyal (Bentuk Tangga)."
  },
  {
    "en": "Apa Itu Algoritma PID (Proportional Integral Derivative) Digital?",
    "id": "Implementasi PID (Proportional Integral Derivative) (Persamaan Beda)."
  },
  {
    "en": "Apa Prinsip PMMC (Permanent Magnet Moving Coil)?",
    "id": "Gaya Lorentz (Koil Di Medan Magnet)."
  },
  {
    "en": "Apa Fungsi Pegas (Spring) PMMC?",
    "id": "Memberikan Torsi Lawan (Mengembalikan Jarum)."
  },
  {
    "en": "Apa Prinsip Meter Moving-Iron (Besi Bergerak)?",
    "id": "Tolakan/Tarikan (Besi Dimagnetisasi)."
  },
  {
    "en": "Meter Moving-Iron (Besi Bergerak) (AC/DC)?",
    "id": "Bisa AC (Alternating Current) Dan DC (Direct Current) (RMS)."
  },
  {
    "en": "PMMC (Permanent Magnet Moving Coil) (AC/DC)?",
    "id": "Hanya DC (Direct Current) (Nilai Rata-Rata)."
  },
  {
    "en": "Bagaimana PMMC (Permanent Magnet Moving Coil) Mengukur AC (Alternating Current)?",
    "id": "Menggunakan Dioda Penyearah Internal."
  },
  {
    "en": "Prinsip DVM (Digital Voltmeter) Integrasi (Dual-Slope)?",
    "id": "Mengubah Tegangan Menjadi Interval Waktu."
  },
  {
    "en": "Apa Itu Refraksi (Refraction) Gelombang?",
    "id": "Pembelokan Gelombang (Beda Medium)."
  },
  {
    "en": "Apa Itu Difraksi (Diffraction) Gelombang?",
    "id": "Penyebaran Gelombang (Celah Sempit)."
  },
  {
    "en": "Apa Itu Atenuasi (Attenuation) Gelombang?",
    "id": "Pelemahan Amplitudo Gelombang."
  },
  {
    "en": "Apa Itu Impedansi Gelombang (Wave Impedance)?",
    "id": "Rasio Total Medan Listrik (E) Magnetik (H)."
  },
  {
    "en": "Apa Itu Hamburan (Scattering) Gelombang?",
    "id": "Penyebaran Gelombang (Objek Kecil)."
  },
  {
    "en": "Apa Itu Hamburan Rayleigh (Rayleigh Scattering)?",
    "id": "Hamburan (Ukuran Partikel Jauh Lebih Kecil Lambda)."
  },
  {
    "en": "Mengapa Langit Berwarna Biru?",
    "id": "Hamburan Rayleigh (Cahaya Biru)."
  },
  {
    "en": "Apa Itu Injeksi Pembawa Muatan (Carrier Injection)?",
    "id": "Peningkatan Muatan (Forward Bias PN)."
  },
  {
    "en": "Arus Apa Dominan (Forward Bias)?",
    "id": "Arus Difusi (Diffusion Current)."
  },
  {
    "en": "Arus Apa Dominan (Reverse Bias)?",
    "id": "Arus Drift (Drift Current) (Sangat Kecil)."
  },
  {
    "en": "Apa Itu Mobilitas (Mobility) Elektron (μe)?",
    "id": "Kemudahan Elektron Bergerak (Medan E)."
  },
  {
    "en": "Apa Itu Mobilitas (Mobility) Hole (μh)?",
    "id": "Kemudahan Hole (Lubang) Bergerak (Medan E)."
  },
  {
    "en": "Mana Lebih Besar (Mobilitas Elektron/Hole)?",
    "id": "Mobilitas Elektron (Umumnya)."
  },
  {
    "en": "Apa Itu Doping (Doping) Tipe-N?",
    "id": "Menambah Atom Donor (Pentavalen)."
  },
  {
    "en": "Apa Itu Doping (Doping) Tipe-P?",
    "id": "Menambah Atom Akseptor (Trivalen)."
  },
  {
    "en": "Apa Itu Atom Donor (Donor Atom)?",
    "id": "Atom (Kelebihan Satu Elektron Valensi)."
  },
  {
    "en": "Apa Itu Atom Akseptor (Acceptor Atom)?",
    "id": "Atom (Kekurangan Satu Elektron Valensi)."
  },
  {
    "en": "Apa Itu Level Donor (Donor Level)?",
    "id": "Level Energi (Dekat Pita Konduksi)."
  },
  {
    "en": "Apa Itu Level Akseptor (Acceptor Level)?",
    "id": "Level Energi (Dekat Pita Valensi)."
  },
  {
    "en": "Apa Itu Hukum Aksi Massa (Mass Action Law)?",
    "id": "n * p = ni Kuadrat (ni²)."
  },
  {
    "en": "Apa Itu Potensial Kontak (Built-in Potential) (Vbi)?",
    "id": "Beda Potensial Internal Sambungan PN."
  },
  {
    "en": "Apa Itu Daerah Deplesi (Depletion Region)?",
    "id": "Area Tanpa Pembawa Muatan Bebas."
  },
  {
    "en": "Apa Itu Arus Saturasi Balik (Is)?",
    "id": "Arus Bocor (Drift) Saat Reverse Bias."
  },
  {
    "en": "Apa Itu Persamaan Dioda Shockley?",
    "id": "Model Matematis Kurva I-V Dioda."
  },
  {
    "en": "Apa Itu Rangkaian SR (Set-Reset) Latch?",
    "id": "Elemen Memori Dasar (Dua Gerbang Silang)."
  },
  {
    "en": "Apa Itu Kondisi Terlarang (Forbidden) SR (Set-Reset) Latch?",
    "id": "S = 1, R = 1 (Output Tak Tentu)."
  },
  {
    "en": "Apa Itu SR (Set-Reset) Latch Ber-Clock (Gated)?",
    "id": "SR (Set-Reset) Latch (Input Clock (Gerbang))."
  },
  {
    "en": "Apa Itu D (Data) Latch (Transparan)?",
    "id": "Output Mengikuti D (Data) (Clock (Gerbang) Tinggi)."
  },
  {
    "en": "Apa Itu D (Data) Flip-Flop (Edge-Triggered)?",
    "id": "Output Menyalin D (Data) (Saat Tepi Clock)."
  },
  {
    "en": "Apa Itu JK (Jack-Kilby) Flip-Flop?",
    "id": "Flip-Flop (J=K=1 Mode Toggle)."
  },
  {
    "en": "Apa Itu T (Toggle) Flip-Flop?",
    "id": "Flip-Flop (Output Berubah (Toggle) Tepi Clock)."
  },
  {
    "en": "Apa Itu Register (Register)?",
    "id": "Kumpulan Flip-Flop (Penyimpan Data)."
  },
  {
    "en": "Apa Itu Pencacah (Counter) Asinkron (Ripple)?",
    "id": "Flip-Flop Dipicu Output Sebelumnya."
  },
  {
    "en": "Apa Itu Pencacah (Counter) Sinkron (Synchronous)?",
    "id": "Semua Flip-Flop Dipicu Clock (Gerbang) Sama."
  },
  {
    "en": "Kelemahan Pencacah (Counter) Ripple (Asinkron)?",
    "id": "Tunda Propagasi (Propagation Delay) (Glitch)."
  },
  {
    "en": "Apa Itu Pencacah (Counter) Dekade (Decade)?",
    "id": "Pencacah (Menghitung Nol Hingga Sembilan)."
  },
  {
    "en": "Apa Itu Kriteria Area Sama (Equal Area Criterion)?",
    "id": "Analisis Grafis Kestabilan Transien."
  },
  {
    "en": "Kapan Kriteria Area Sama (Equal Area Criterion) Digunakan?",
    "id": "Sistem Dua Mesin (Satu Mesin-Bus Tak Hingga)."
  },
  {
    "en": "Apa Itu Area Percepatan (Accelerating Area) (A1)?",
    "id": "Area (Daya Mekanik > Daya Listrik)."
  },
  {
    "en": "Apa Itu Area Perlambatan (Decelerating Area) (A2)?",
    "id": "Area (Daya Listrik > Daya Mekanik)."
  },
  {
    "en": "Apa Syarat Stabil (Kriteria Area Sama)?",
    "id": "Area Perlambatan (A2) > Area Percepatan (A1)."
  },
  {
    "en": "Apa Itu Sudut Rotor (Rotor Angle) (δ)?",
    "id": "Sudut Antara Rotor Generator Bus Tak Hingga."
  },
  {
    "en": "Apa Itu Persamaan Ayunan (Swing Equation)?",
    "id": "Model Matematis Dinamika Sudut Rotor."
  },
  {
    "en": "Apa Itu Waktu Pembersihan Gangguan Kritis (CCT)?",
    "id": "Waktu Maks Gangguan (Sistem Masih Stabil)."
  },
  {
    "en": "Apa Itu Sudut Kritis (Critical Clearing Angle)?",
    "id": "Sudut Maksimum (Sistem Masih Stabil)."
  },
  {
    "en": "Apa Itu Ikatan (Bonding) Ekuipotensial?",
    "id": "Menghubungkan Semua Konduktor (Potensial Sama)."
  },
  {
    "en": "Apa Itu Tegangan Sentuh (Touch Voltage)?",
    "id": "Beda Potensial (Peralatan Gagal Ground)."
  },
  {
    "en": "Apa Itu Tegangan Langkah (Step Voltage)?",
    "id": "Beda Potensial (Dua Kaki Di Tanah)."
  },
  {
    "en": "Apa Kepanjangan GFCI (Ground Fault Circuit Interrupter)?",
    "id": "Ground Fault Circuit Interrupter."
  },
  {
    "en": "Prinsip Kerja GFCI (Ground Fault Circuit Interrupter)?",
    "id": "Mendeteksi Beda Arus (Fasa Netral)."
  },
  {
    "en": "Apa Kepanjangan AFCI (Arc Fault Circuit Interrupter)?",
    "id": "Arc Fault Circuit Interrupter."
  },
  {
    "en": "Prinsip Kerja AFCI (Arc Fault Circuit Interrupter)?",
    "id": "Mendeteksi Sinyal Busur Api (Arcing)."
  },
  {
    "en": "Apa Rating (Rating) Motor ODP (Open Drip Proof)?",
    "id": "Motor Terbuka (Perlindungan Tetesan Vertikal)."
  },
  {
    "en": "Apa Rating (Rating) Motor TEFC (Totally Enclosed Fan Cooled)?",
    "id": "Motor Tertutup (Pendingin Kipas Eksternal)."
  },
  {
    "en": "Apa Rating (Rating) Motor TENV (Totally Enclosed Non-Ventilated)?",
    "id": "Motor Tertutup (Tanpa Kipas Pendingin)."
  },
  {
    "en": "Apa Rating (Rating) Motor EXPL (Explosion Proof)?",
    "id": "Motor Tahan Ledakan (Area Berbahaya)."
  },
  {
    "en": "Apa Itu Kelas (Class) Isolasi Motor?",
    "id": "Batas Suhu Maksimum Lilitan Motor."
  },
  {
    "en": "Apa Itu Kelas (Class) Isolasi F (Motor)?",
    "id": "Batas Suhu 155 Derajat Celcius."
  },
  {
    "en": "Apa Itu Kelas (Class) Isolasi H (Motor)?",
    "id": "Batas Suhu 180 Derajat Celcius."
  },
  {
    "en": "Apa Itu Service Factor (SF) Motor?",
    "id": "Kemampuan Beban Lebih (Jangka Pendek)."
  },
  {
    "en": "Apa Arti Service Factor (SF) 1.15?",
    "id": "Bisa Beroperasi 15% Di Atas Rating."
  },
  {
    "en": "Apa Itu NEMA (National Electrical Manufacturers Association) Frame (Rangka)?",
    "id": "Standar Dimensi Fisik Motor."
  },
  {
    "en": "Apa Itu Pengujian (Test) Tanpa Beban (No-Load) Motor?",
    "id": "Mengukur Rugi Inti Gesekan Angin."
  },
  {
    "en": "Apa Itu Pengujian (Test) Rotor Terkunci (Locked-Rotor)?",
    "id": "Mengukur Rugi Tembaga Arus Start."
  },
  {
    "en": "Apa Itu Efisiensi (Efficiency) Motor?",
    "id": "Rasio Daya Mekanik Output Daya Listrik Input."
  },
  {
    "en": "Apa Itu Standar Efisiensi (Efficiency) IE1?",
    "id": "Efisiensi Standar."
  },
  {
    "en": "Apa Itu Standar Efisiensi (Efficiency) IE2?",
    "id": "Efisiensi Tinggi (High)."
  },
  {
    "en": "Apa Itu Standar Efisiensi (Efficiency) IE3?",
    "id": "Efisiensi Premium (Premium)."
  },
  {
    "en": "Apa Itu Standar Efisiensi (Efficiency) IE4?",
    "id": "Efisiensi Super Premium (Super Premium)."
  },
  {
    "en": "Apa Itu Standar Efisiensi (Efficiency) IE5?",
    "id": "Efisiensi Ultra Premium (Ultra Premium)."
  },
  {
    "en": "Apa Itu Transformasi-Z (Z-Transform) Bilateral?",
    "id": "Transformasi (Penjumlahan Dari Minus Tak Hingga)."
  },
  {
    "en": "Apa Itu Transformasi-Z (Z-Transform) Unilateral?",
    "id": "Transformasi (Penjumlahan Dari Nol)."
  },
  {
    "en": "Apa Kepanjangan ROC (Region of Convergence)?",
    "id": "Region of Convergence (Wilayah Konvergensi)."
  },
  {
    "en": "Apa Itu ROC (Region of Convergence) Sinyal Kausal?",
    "id": "Area Di Luar Lingkaran Terluar."
  },
  {
    "en": "Apa Itu ROC (Region of Convergence) Sinyal Anti-Kausal?",
    "id": "Area Di Dalam Lingkaran Terdalam."
  },
  {
    "en": "Apa Pemetaan (Mapping) Sisi Kiri Bidang-S Ke Bidang-Z?",
    "id": "Bagian Dalam Lingkaran Satuan."
  },
  {
    "en": "Apa Pemetaan (Mapping) Sumbu Imajiner Bidang-S Ke Bidang-Z?",
    "id": "Tepi Lingkaran Satuan."
  },
  {
    "en": "Apa Pemetaan (Mapping) Sisi Kanan Bidang-S Ke Bidang-Z?",
    "id": "Bagian Luar Lingkaran Satuan."
  },
  {
    "en": "Apa Properti Pergeseran Waktu (Time Shift) Z-Transform?",
    "id": "Perkalian Dengan (Z Pangkat -N)."
  },
  {
    "en": "Apa Itu Teorema Nilai Awal (Initial Value)?",
    "id": "Mencari Nilai Awal (X[0])."
  },
  {
    "en": "Apa Itu Teorema Nilai Akhir (Final Value)?",
    "id": "Mencari Nilai Akhir (X[Tak Hingga])."
  },
  {
    "en": "Apa Syarat Teorema Nilai Akhir?",
    "id": "Kutub (Pole) Harus Stabil."
  },
  {
    "en": "Apa Itu Breakdown (Tembus) Termal Dielektrik?",
    "id": "Kegagalan Isolasi Akibat Panas Internal."
  },
  {
    "en": "Apa Itu Breakdown (Tembus) Intrinsik Dielektrik?",
    "id": "Kegagalan Akibat Medan Listrik Ekstrem."
  },
  {
    "en": "Apa Itu Breakdown (Tembus) Elektrokimia Dielektrik?",
    "id": "Degradasi Isolasi Akibat Reaksi Kimia."
  },
  {
    "en": "Apa Itu Pelepasan Muatan Sebagian (Partial Discharge)?",
    "id": "Pelepasan Listrik Kecil Rongga Isolator."
  },
  {
    "en": "Apa Itu Pohon (Treeing) Listrik (Isolasi)?",
    "id": "Jalur Karbonisasi Permanen (Kegagalan)."
  },
  {
    "en": "Apa Itu Hukum Paschen (Paschen's Law)?",
    "id": "Tegangan Breakdown (Tembus) Gas (Fungsi Tekanan Jarak)."
  },
  {
    "en": "Apa Itu Pelepasan (Discharge) Korona (Corona)?",
    "id": "Ionisasi Udara Sekitar Konduktor Tegangan Tinggi."
  },
  {
    "en": "Apa Tanda-Tanda Korona (Corona)?",
    "id": "Desis Audio, Cahaya Ungu, Produksi Ozon."
  },
  {
    "en": "Dampak Korona (Corona)?",
    "id": "Rugi Daya, Interferensi Radio (RFI)."
  },
  {
    "en": "Apa Itu Model Kegagalan (Failure) Arrhenius?",
    "id": "Laju Kegagalan Tergantung Suhu."
  },
  {
    "en": "Apa Itu Model Kegagalan (Failure) Eyring?",
    "id": "Model (Suhu Dan Stres Non-Termal)."
  },
  {
    "en": "Apa Itu Model Kegagalan (Failure) Coffin-Manson?",
    "id": "Kegagalan Akibat Kelelahan Siklus Termal."
  },
  {
    "en": "Apa Itu Uji Bakar (Burn-In Test)?",
    "id": "Operasi Awal (Filter Kegagalan Dini)."
  },
  {
    "en": "Apa Itu Pelapukan (Weathering) Isolator?",
    "id": "Degradasi Akibat Paparan Lingkungan (UV)."
  },
  {
    "en": "Apa Itu Loncatan Api (Flashover) Isolator?",
    "id": "Busur Api Lewat Permukaan Isolator."
  },
  {
    "en": "Apa Itu Tusukan (Puncture) Isolator?",
    "id": "Busur Api Menembus Badan Isolator."
  },
  {
    "en": "Apa Itu Gardu Induk (Substation) Tipe AIS?",
    "id": "Air Insulated Substation (Isolasi Udara)."
  },
  {
    "en": "Apa Itu Gardu Induk (Substation) Tipe GIS?",
    "id": "Gas Insulated Substation (Isolasi Gas SF6)."
  },
  {
    "en": "Keunggulan Gardu Induk (Substation) GIS (Gas Insulated Substation)?",
    "id": "Sangat Kompak, Tahan Polusi."
  },
  {
    "en": "Apa Konfigurasi Busbar (Busbar) Tunggal?",
    "id": "Sederhana, Murah, Andal Rendah."
  },
  {
    "en": "Apa Konfigurasi Busbar (Busbar) Ganda?",
    "id": "Fleksibel (Bisa Transfer Beban)."
  },
  {
    "en": "Apa Konfigurasi Busbar (Busbar) Cincin (Ring)?",
    "id": "Sangat Andal (CB (Circuit Breaker) Gagal Tetap Aman)."
  },
  {
    "en": "Apa Konfigurasi Busbar (Busbar) Satu Setengah (Breaker-and-a-Half)?",
    "id": "Sangat Andal (Tiga CB (Circuit Breaker) Per Dua Bay)."
  },
  {
    "en": "Apa Itu Bay (Teluk) Gardu Induk?",
    "id": "Unit Fungsional (Saluran Atau Trafo)."
  },
  {
    "en": "Apa Fungsi Saklar Pentanahan (Earthing Switch)?",
    "id": "Membumikan Peralatan (Keamanan Perawatan)."
  },
  {
    "en": "Apa Fungsi Pemisah (Disconnector/PMS)?",
    "id": "Memutus Rangkaian (Kondisi Tanpa Beban)."
  },
  {
    "en": "Apa Fungsi Pemutus Tenaga (CB (Circuit Breaker))?",
    "id": "Memutus Rangkaian (Kondisi Berbeban/Gangguan)."
  },
  {
    "en": "Apa Itu Latch (Kait) Earle (Earle Latch)?",
    "id": "Latch (Kait) D (Data) Tahan Hazard (Data Latch)."
  },
  {
    "en": "Apa Itu Sinkroniser (Synchronizer) FIFO (First-In First-Out)?",
    "id": "Metode Aman Transmisi CDC (Clock Domain Crossing)."
  },
  {
    "en": "Mengapa FIFO (First-In First-Out) Asinkron Pakai Pointer (Penunjuk) Gray?",
    "id": "Hanya Satu Bit Berubah (Cegah Metastabilitas)."
  },
  {
    "en": "Apa Itu Sel (Cell) Gated Clock (Clock Bergerbang) Terintegrasi?",
    "id": "Metode Aman Hemat Daya (Cegah Glitch)."
  },
  {
    "en": "Apa Itu Jaringan Distribusi Clock (Clock Tree)?",
    "id": "Jaringan Distribusi Sinyal Clock (Rata)."
  },
  {
    "en": "Apa Kepanjangan CTS (Clock Tree Synthesis)?",
    "id": "Clock Tree Synthesis."
  },
  {
    "en": "Tujuan CTS (Clock Tree Synthesis)?",
    "id": "Meminimalkan Skew (Kemiringan) Jitter (Getaran) Clock."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Fisik (Physical) (OSI (Open Systems Interconnection) L1)?",
    "id": "Media Transmisi Fisik (Bit)."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Tautan Data (Data Link) (OSI (Open Systems Interconnection) L2)?",
    "id": "Frame (Bingkai), Alamat MAC (Media Access Control)."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Jaringan (Network) (OSI (Open Systems Interconnection) L3)?",
    "id": "Paket (Packet), Alamat IP (Internet Protocol), Routing."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Transportasi (Transport) (OSI (Open Systems Interconnection) L4)?",
    "id": "Segmen (Segment), Port (TCP (Transmission Control Protocol)/UDP (User Datagram Protocol))."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Sesi (Session) (OSI (Open Systems Interconnection) L5)?",
    "id": "Manajemen Dialog (Sesi)."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Presentasi (Presentation) (OSI (Open Systems Interconnection) L6)?",
    "id": "Translasi Format Data (Enkripsi)."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Aplikasi (Application) (OSI (Open Systems Interconnection) L7)?",
    "id": "Antarmuka Pengguna (HTTP (Hypertext Transfer Protocol), FTP (File Transfer Protocol))."
  },
  {
    "en": "Apa Itu Jabat Tangan (Handshake) Tiga Arah TCP (Transmission Control Protocol)?",
    "id": "SYN (Synchronize), SYN-ACK (Synchronize-Acknowledge), ACK (Acknowledge)."
  },
  {
    "en": "Apa Fungsi Jendela Geser (Sliding Window) TCP?",
    "id": "Kontrol Aliran Data (Flow Control)."
  },
  {
    "en": "Apa Fungsi Kontrol Kemacetan (Congestion Control) TCP?",
    "id": "Mencegah Kolaps Jaringan (Algoritma)."
  },
  {
    "en": "Apa Itu Awal Lambat (Slow Start) TCP?",
    "id": "Menaikkan Jendela Kemacetan Eksponensial."
  },
  {
    "en": "Apa Itu Time To Live (TTL) Paket IP (Internet Protocol)?",
    "id": "Batas Jumlah Lompatan (Hop) Router."
  },
  {
    "en": "Apa Itu Alamat (Address) IPv4 (Internet Protocol version 4)?",
    "id": "Alamat Jaringan 32-bit."
  },
  {
    "en": "Apa Itu Alamat (Address) IPv6 (Internet Protocol version 6)?",
    "id": "Alamat Jaringan 128-bit."
  },
  {
    "en": "Apa Itu Alamat (Address) Loopback?",
    "id": "127.0.0.1 (Localhost)."
  },
  {
    "en": "Apa Kepanjangan NAT (Network Address Translation)?",
    "id": "Network Address Translation."
  },
  {
    "en": "Apa Fungsi NAT (Network Address Translation)?",
    "id": "Mengubah IP (Internet Protocol) Privat Ke Publik."
  },
  {
    "en": "Apa Itu Alamat (Address) IP (Internet Protocol) Privat Kelas A?",
    "id": "10.0.0.0 /8."
  },
  {
    "en": "Apa Itu Alamat (Address) IP (Internet Protocol) Privat Kelas B?",
    "id": "172.16.0.0 /12."
  },
  {
    "en": "Apa Itu Alamat (Address) IP (Internet Protocol) Privat Kelas C?",
    "id": "192.168.0.0 /16."
  },
  {
    "en": "Apa Kepanjangan APIPA (Automatic Private IP Addressing)?",
    "id": "Automatic Private IP Addressing."
  },
  {
    "en": "Kapan APIPA (Automatic Private IP Addressing) Aktif?",
    "id": "Saat DHCP (Dynamic Host Configuration Protocol) Gagal (Rentang 169.254)."
  },
  {
    "en": "Apa Itu Alamat (Address) MAC (Media Access Control) Broadcast?",
    "id": "FF:FF:FF:FF:FF:FF."
  },
  {
    "en": "Apa Kepanjangan ARP (Address Resolution Protocol)?",
    "id": "Address Resolution Protocol."
  },
  {
    "en": "Fungsi ARP (Address Resolution Protocol)?",
    "id": "Memetakan Alamat IP (Internet Protocol) Ke Alamat MAC (Media Access Control)."
  },
  {
    "en": "Apa Kepanjangan RARP (Reverse Address Resolution Protocol)?",
    "id": "Reverse Address Resolution Protocol."
  },
  {
    "en": "Apa Itu VLSM (Variable Length Subnet Masking)?",
    "id": "Subnetting (Sub-jaringan) Dengan Ukuran Bervariasi."
  },
  {
    "en": "Apa Itu CIDR (Classless Inter-Domain Routing)?",
    "id": "Metode Alokasi IP (Internet Protocol) (Notasi Slash /)."
  },
  {
    "en": "Apa Itu Protokol (Protocol) Routing (Perutean) Internal (IGP)?",
    "id": "Routing (Perutean) Dalam Satu Sistem Otonom (OSPF (Open Shortest Path First))."
  },
  {
    "en": "Apa Itu Protokol (Protocol) Routing (Perutean) Eksternal (EGP)?",
    "id": "Routing (Perutean) Antar Sistem Otonom (BGP (Border Gateway Protocol))."
  },
  {
    "en": "Apa Kepanjangan OSPF (Open Shortest Path First)?",
    "id": "Open Shortest Path First."
  },
  {
    "en": "Apa Kepanjangan BGP (Border Gateway Protocol)?",
    "id": "Border Gateway Protocol."
  },
  {
    "en": "Apa Itu MTU (Maximum Transmission Unit)?",
    "id": "Ukuran Paket Data Maksimum Jaringan."
  },
  {
    "en": "Apa Itu Fragmentasi (Fragmentation) IP (Internet Protocol)?",
    "id": "Memecah Paket Besar (Sesuaikan MTU (Maximum Transmission Unit))."
  },
  {
    "en": "Apa Itu Port (Port) 80 (TCP (Transmission Control Protocol))?",
    "id": "HTTP (Hypertext Transfer Protocol) (Web Server)."
  },
  {
    "en": "Apa Itu Port (Port) 443 (TCP (Transmission Control Protocol))?",
    "id": "HTTPS (Hypertext Transfer Protocol Secure) (Web Server Aman)."
  },
  {
    "en": "Apa Itu Port (Port) 22 (TCP (Transmission Control Protocol))?",
    "id": "SSH (Secure Shell) (Remote Login Aman)."
  },
  {
    "en": "Apa Itu Port (Port) 25 (TCP (Transmission Control Protocol))?",
    "id": "SMTP (Simple Mail Transfer Protocol) (Pengiriman Email)."
  },
  {
    "en": "Apa Itu Port (Port) 53 (UDP (User Datagram Protocol))?",
    "id": "DNS (Domain Name System) (Kueri Nama Domain)."
  },
  {
    "en": "Apa Itu Port (Port) 21 (TCP (Transmission Control Protocol))?",
    "id": "FTP (File Transfer Protocol) (Kontrol File Transfer)."
  },
  {
    "en": "Apa Itu Port (Port) 67 68 (UDP (User Datagram Protocol))?",
    "id": "DHCP (Dynamic Host Configuration Protocol) (Alokasi IP (Internet Protocol))."
  },
  {
    "en": "Apa Itu Penyangga Jitter (Jitter Buffer)?",
    "id": "Menyimpan Sementara Paket (VoIP (Voice over Internet Protocol))."
  },
  {
    "en": "Apa Kepanjangan VoIP (Voice over Internet Protocol)?",
    "id": "Voice over Internet Protocol (Suara Lewat IP)."
  },
  {
    "en": "Apa Kepanjangan RTP (Real-time Transport Protocol)?",
    "id": "Real-time Transport Protocol (Media Streaming)."
  },
  {
    "en": "Apa Kepanjangan RTCP (RTP Control Protocol)?",
    "id": "RTP (Real-time Transport Protocol) Control Protocol (Statistik Kualitas)."
  },
  {
    "en": "Apa Kepanjangan SIP (Session Initiation Protocol)?",
    "id": "Session Initiation Protocol (Sinyal VoIP (Voice over Internet Protocol))."
  },
  {
    "en": "Apa Kepanjangan H.323 (H.323)?",
    "id": "Standar Protokol Multimedia (Video Konferensi)."
  },
  {
    "en": "Apa Itu Kodek (Codec) Audio?",
    "id": "Kompresi Dekompresi Sinyal Audio Digital."
  },
  {
    "en": "Contoh Kodek (Codec) Audio (VoIP (Voice over Internet Protocol))?",
    "id": "G.711, G.729, Opus."
  },
  {
    "en": "Apa Itu Kodek (Codec) Video?",
    "id": "Kompresi Dekompresi Sinyal Video Digital."
  },
  {
    "en": "Contoh Kodek (Codec) Video?",
    "id": "H.264, H.265 (HEVC), VP9, AV1."
  },
  {
    "en": "Apa Itu Kompresi Lossy (Berugi)?",
    "id": "Kompresi (Membuang Sebagian Data)."
  },
  {
    "en": "Apa Itu Kompresi Lossless (Tanpa Rugi)?",
    "id": "Kompresi (Data Bisa Dikembalikan Sempurna)."
  },
  {
    "en": "Apa Kepanjangan SIL (Safety Integrity Level)?",
    "id": "Safety Integrity Level."
  },
  {
    "en": "Apa Tujuan SIL (Safety Integrity Level)?",
    "id": "Kuantifikasi Keandalan Sistem Keselamatan."
  },
  {
    "en": "Ada Berapa Level SIL (Safety Integrity Level)?",
    "id": "Empat Level (SIL 1 Hingga SIL 4)."
  },
  {
    "en": "Level SIL (Safety Integrity Level) Mana Paling Kritis (Tertinggi)?",
    "id": "SIL 4 (Sangat Kritis)."
  },
  {
    "en": "Level SIL (Safety Integrity Level) Mana Paling Rendah (Dasar)?",
    "id": "SIL 1 (Risiko Rendah)."
  },
  {
    "en": "Apa Kepanjangan SIF (Safety Instrumented Function)?",
    "id": "Safety Instrumented Function."
  },
  {
    "en": "Apa Itu SIF (Safety Instrumented Function)?",
    "id": "Fungsi Proteksi (Sensor, Logika, Aktuator)."
  },
  {
    "en": "Apa Kepanjangan PFD (Probability of Failure on Demand)?",
    "id": "Probability of Failure on Demand."
  },
  {
    "en": "Kapan PFD (Probability of Failure on Demand) Digunakan?",
    "id": "Mode Operasi Permintaan Rendah (Low Demand)."
  },
  {
    "en": "Apa Kepanjangan PFH (Probability of Failure per Hour)?",
    "id": "Probability of Failure per Hour."
  },
  {
    "en": "Kapan PFH (Probability of Failure per Hour) Digunakan?",
    "id": "Mode Operasi Permintaan Tinggi (High Demand)."
  },
  {
    "en": "Apa Itu Toleransi Kesalahan (Fault Tolerance)?",
    "id": "Kemampuan Sistem (Operasi Saat Gagal)."
  },
  {
    "en": "Apa Itu Redundansi (Redundancy) 1oo2 (One-out-of-Two)?",
    "id": "Dua Komponen (Satu Gagal Tetap Jalan)."
  },
  {
    "en": "Apa Itu Redundansi (Redundancy) 2oo3 (Two-out-of-Three)?",
    "id": "Tiga Komponen (Voting (Pemungutan Suara) (Dua Benar))."
  },
  {
    "en": "Apa Itu Standar IEC (International Electrotechnical Commission) 61508?",
    "id": "Standar Induk Keselamatan Fungsional."
  },
  {
    "en": "Apa Itu Standar IEC (International Electrotechnical Commission) 61511?",
    "id": "Standar Keselamatan (Industri Proses)."
  },
  {
    "en": "Apa Itu GGM (Gaya Gerak Magnet) (MMF)?",
    "id": "Gaya Pendorong Fluks Magnetik."
  },
  {
    "en": "Apa Satuan GGM (Gaya Gerak Magnet) (MMF)?",
    "id": "Amper-Lilitan (Ampere-Turn)."
  },
  {
    "en": "Apa Itu Reluktansi (Reluctance) Magnetik (R)?",
    "id": "Hambatan Terhadap Aliran Fluks Magnetik."
  },
  {
    "en": "Apa Satuan Reluktansi (Reluctance) Magnetik?",
    "id": "Amper-Lilitan Per Weber (A-t/Wb)."
  },
  {
    "en": "Apa Itu Hukum Hopkinson (Hopkinson's Law)?",
    "id": "GGM (Gaya Gerak Magnet) = Fluks Kali Reluktansi."
  },
  {
    "en": "Apa Analogi Hukum Hopkinson (Hukum Ohm)?",
    "id": "Tegangan = Arus Kali Hambatan."
  },
  {
    "en": "Apa Itu Permeansi (Permeance)?",
    "id": "Kebalikan Dari Reluktansi (Reluctance)."
  },
  {
    "en": "Apa Rumus Reluktansi (Reluctance) Inti (Core)?",
    "id": "Panjang Dibagi (Permeabilitas Kali Area)."
  },
  {
    "en": "Apa Itu Efek Fringing (Tepi)?",
    "id": "Fluks Magnetik Melebar Di Celah Udara."
  },
  {
    "en": "Apa Itu Induktansi Diri (Self-Inductance) (L)?",
    "id": "GGL (Gaya Gerak Listrik) Induksi Diri (Perubahan Arus)."
  },
  {
    "en": "Apa Itu Induktansi Mutual (Mutual Inductance) (M)?",
    "id": "GGL (Gaya Gerak Listrik) Induksi (Antar Dua Kumparan)."
  },
  {
    "en": "Apa Itu Faktor Kopling (Coupling) (k)?",
    "id": "Ukuran Fraksi Fluks Terhubung."
  },
  {
    "en": "Berapa Nilai k (Kopling Sempurna)?",
    "id": "Satu (1)."
  },
  {
    "en": "Hubungan M (Induktansi Mutual), L1, L2, k?",
    "id": "M = k * Akar(L1 * L2)."
  },
  {
    "en": "Apa Itu Koneksi Seri Penambah (Aiding)?",
    "id": "Fluks Saling Memperkuat (Ltotal = L1+L2+2M)."
  },
  {
    "en": "Apa Itu Koneksi Seri Penentang (Opposing)?",
    "id": "Fluks Saling Melemahkan (Ltotal = L1+L2-2M)."
  },
  {
    "en": "Apa Itu Notasi Titik (Dot Convention)?",
    "id": "Menandai Polaritas Lilitan Induktif."
  },
  {
    "en": "Apa Itu Energi Tersimpan (Induktor)?",
    "id": "W = Setengah L Kali I Kuadrat (½LI²)."
  },
  {
    "en": "Apa Itu Topologi Resonansi Seri (SRC)?",
    "id": "Rangkaian Tangki LC (Induktor-Kapasitor) Seri Saklar."
  },
  {
    "en": "Apa Itu Topologi Resonansi Paralel (PRC)?",
    "id": "Rangkaian Tangki LC (Induktor-Kapasitor) Paralel Beban."
  },
  {
    "en": "Apa Itu Topologi Resonansi LCC (LCC)?",
    "id": "Resonansi (Induktor, Kapasitor, Kapasitor) (Tiga Elemen)."
  },
  {
    "en": "Apa Itu Kontrol Frekuensi Variabel (VFC)?",
    "id": "Metode Kontrol Konverter Resonansi."
  },
  {
    "en": "Kapan ZVS (Zero Voltage Switching) (Saklar Tegangan Nol) Terjadi (Resonansi)?",
    "id": "Operasi Di Atas Frekuensi Resonansi."
  },
  {
    "en": "Kapan ZCS (Zero Current Switching) (Saklar Arus Nol) Terjadi (Resonansi)?",
    "id": "Operasi Di Bawah Frekuensi Resonansi."
  },
  {
    "en": "Apa Itu Penyeimbangan Tegangan Kapasitor (MLI)?",
    "id": "Tantangan Utama MLI (Multi-Level Inverter) (NPC (Neutral Point Clamped)/FC (Flying Capacitor))."
  },
  {
    "en": "Apa Itu Modulasi Pergeseran Fasa (Phase-Shifted PWM)?",
    "id": "Metode Kontrol MLI (Multi-Level Inverter) Kaskade (CHB)."
  },
  {
    "en": "Apa Itu Modulasi Disposisi Level (Level-Shifted PWM)?",
    "id": "Metode Kontrol MLI (Multi-Level Inverter) NPC (Neutral Point Clamped)/FC (Kapasitor Terbang)."
  },
  {
    "en": "Apa Itu Penyearah Vienna (Vienna Rectifier)?",
    "id": "Penyearah Tiga Fasa (Input PFC (Power Factor Correction))."
  },
  {
    "en": "Apa Itu Jaringan (Network) Z-Source (Z-Source)?",
    "id": "Jaringan LC (Induktor-Kapasitor) (Bentuk-Z) (Impedansi)."
  },
  {
    "en": "Apa Itu Glitch (Kutu) DAC (Digital to Analog Converter)?",
    "id": "Transien Palsu Saat Perubahan Kode Input."
  },
  {
    "en": "Kapan Glitch (Kutu) DAC (Digital to Analog Converter) Terparah?",
    "id": "Transisi Titik Tengah (Contoh: 0111 Ke 1000)."
  },
  {
    "en": "Apa Itu Energi (Energy) Glitch (Kutu)?",
    "id": "Area (Luas) Dari Pulsa Glitch (Kutu)."
  },
  {
    "en": "Apa Itu Rangkaian Deglitching (Deglitcher)?",
    "id": "Rangkaian (Sample-and-Hold) (Peredam Glitch (Kutu))."
  },
  {
    "en": "Apa Itu Waktu Penetapan (Settling Time) DAC?",
    "id": "Waktu Output (Stabil Dalam Batas Error)."
  },
  {
    "en": "Apa Itu Latensi (Latency) ADC (Analog to Digital Converter)?",
    "id": "Tunda Waktu (Sinyal Masuk Data Keluar)."
  },
  {
    "en": "Apa Itu Throughput (Throughput) ADC (Analog to Digital Converter)?",
    "id": "Laju Konversi (Sampel Per Detik)."
  },
  {
    "en": "Apa Itu ADC (Analog to Digital Converter) Pipeline (Pipa Salur)?",
    "id": "ADC (Analog to Digital Converter) Cepat (Tahapan Bertingkat)."
  },
  {
    "en": "Apa Itu MASH (Multi-Stage Noise Shaping)?",
    "id": "Arsitektur Sigma-Delta (Bertingkat)."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization) Vektor?",
    "id": "Kuantisasi (Blok Data Sekaligus)."
  },
  {
    "en": "Apa Itu Operasi Bitwise (Bitwise) AND (&)?",
    "id": "Operasi Logika AND (Dan) Per Bit."
  },
  {
    "en": "Apa Itu Operasi Bitwise (Bitwise) OR (|)?",
    "id": "Operasi Logika OR (Atau) Per Bit."
  },
  {
    "en": "Apa Itu Operasi Bitwise (Bitwise) XOR (^)?",
    "id": "Operasi Logika XOR (Exclusive OR) Per Bit."
  },
  {
    "en": "Apa Itu Operasi Bitwise (Bitwise) NOT (~)?",
    "id": "Operasi Logika NOT (Inversi) Per Bit."
  },
  {
    "en": "Apa Itu Operasi Geser Kiri (Left Shift) (<<)?",
    "id": "Menggeser Bit Ke Kiri (Kali Dua)."
  },
  {
    "en": "Apa Itu Operasi Geser Kanan (Right Shift) (>>)?",
    "id": "Menggeser Bit Ke Kanan (Bagi Dua)."
  },
  {
    "en": "Apa Itu Bit Masking (Penopeng Bit)?",
    "id": "Menggunakan Bitwise (Mengisolasi/Mengubah Bit)."
  },
  {
    "en": "Bagaimana Cara Mengatur (Set) Satu Bit?",
    "id": "Menggunakan Operasi OR (|) Dengan Mask."
  },
  {
    "en": "Bagaimana Cara Menghapus (Clear) Satu Bit?",
    "id": "Menggunakan Operasi AND (&) Inversi Mask."
  },
  {
    "en": "Bagaimana Cara Membalik (Toggle) Satu Bit?",
    "id": "Menggunakan Operasi XOR (^) Dengan Mask."
  },
  {
    "en": "Bagaimana Cara Mengecek (Check) Satu Bit?",
    "id": "Menggunakan Operasi AND (&) Dengan Mask."
  },
  {
    "en": "Apa Itu Penjumlahan Komplemen Dua (2's Complement)?",
    "id": "Metode Penjumlahan Bilangan Bertanda."
  },
  {
    "en": "Apa Itu Pengurangan Komplemen Dua (2's Complement)?",
    "id": "Ubah Pengurang, Lalu Tambahkan."
  },
  {
    "en": "Apa Itu Bilangan Titik Tetap (Fixed-Point)?",
    "id": "Representasi Bilangan (Posisi Koma Tetap)."
  },
  {
    "en": "Apa Format Bilangan Q (Q Format)?",
    "id": "Notasi Bilangan Titik Tetap (Q(m.n))."
  },
  {
    "en": "Apa Itu Bilangan Titik Mengambang (Floating-Point)?",
    "id": "Representasi Bilangan (Sign, Exponent, Mantissa)."
  },
  {
    "en": "Apa Itu Format Presisi Tunggal (Single Precision)?",
    "id": "Floating-Point (32-bit) (IEEE 754)."
  },
  {
    "en": "Apa Itu Format Presisi Ganda (Double Precision)?",
    "id": "Floating-Point (64-bit) (IEEE 754)."
  },
  {
    "en": "Apa Itu Nilai Denormalized (Denormal)?",
    "id": "Bilangan Floating-Point (Sangat Kecil)."
  },
  {
    "en": "Apa Itu Inf (Tak Hingga) (Floating Point)?",
    "id": "Hasil (Pembagian Dengan Nol)."
  },
  {
    "en": "Apa Itu NaN (Not a Number)?",
    "id": "Hasil Operasi Tidak Valid (0/0)."
  },
  {
    "en": "Apa Itu Komputasi Lunak (Soft Computing)?",
    "id": "Metode Komputasi (Toleran Ketidakpastian)."
  },
  {
    "en": "Apa Saja Pilar Komputasi Lunak?",
    "id": "Logika Fuzzy, Jaringan Saraf, Komputasi Evolusioner."
  },
  {
    "en": "Apa Itu Logika Fuzzy (Fuzzy Logic)?",
    "id": "Logika Berbasis Derajat Kebenaran."
  },
  {
    "en": "Apa Itu Himpunan Fuzzy (Fuzzy Set)?",
    "id": "Himpunan (Keanggotaan Bertingkat)."
  },
  {
    "en": "Apa Itu Fungsi Keanggotaan (Membership Function)?",
    "id": "Kurva (Derajat Keanggotaan Himpunan Fuzzy)."
  },
  {
    "en": "Apa Itu Fuzifikasi (Fuzzification)?",
    "id": "Proses Mengubah Input Crisp Ke Fuzzy."
  },
  {
    "en": "Apa Itu Mesin Inferensi (Inference Engine) Fuzzy?",
    "id": "Mengevaluasi Aturan Fuzzy (IF-THEN)."
  },
  {
    "en": "Apa Itu Defuzifikasi (Defuzzification)?",
    "id": "Proses Mengubah Hasil Fuzzy Ke Crisp."
  },
  {
    "en": "Metode Defuzifikasi (Defuzzification) Umum?",
    "id": "Metode Sentroid (Center of Gravity)."
  },
  {
    "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?",
    "id": "Model Komputasi (Meniru Otak Biologis)."
  },
  {
    "en": "Apa Itu Neuron (Neuron) (ANN (Artificial Neural Network))?",
    "id": "Unit Pemroses Dasar Jaringan Saraf."
  },
  {
    "en": "Apa Itu Fungsi Aktivasi (Activation Function)?",
    "id": "Fungsi Non-Linear Output Neuron."
  },
  {
    "en": "Apa Itu Fungsi Aktivasi Sigmoid?",
    "id": "Fungsi Aktivasi (Bentuk S)."
  },
  {
    "en": "Apa Itu Fungsi Aktivasi ReLU (Rectified Linear Unit)?",
    "id": "Fungsi Aktivasi (Maks(0, x))."
  },
  {
    "en": "Apa Itu Perceptron (Perceptron)?",
    "id": "Model Jaringan Saraf Paling Sederhana."
  },
  {
    "en": "Apa Itu Jaringan Saraf Umpan Maju (Feedforward)?",
    "id": "Jaringan (Informasi Maju, Tanpa Loop)."
  },
  {
    "en": "Apa Itu Jaringan Saraf Berulang (RNN)?",
    "id": "Jaringan (Koneksi Umpan Balik, Memori)."
  },
  {
    "en": "Apa Itu Pembelajaran Terbimbing (Supervised Learning)?",
    "id": "Belajar (Data Input Berlabel Output)."
  },
  {
    "en": "Apa Itu Pembelajaran Tak Terbimbing (Unsupervised Learning)?",
    "id": "Belajar (Data Input Tanpa Label)."
  },
  {
    "en": "Apa Itu Algoritma Genetik (Genetic Algorithm)?",
    "id": "Algoritma Optimasi (Evolusi Biologis)."
  },
  {
    "en": "Apa Itu Pembangkitan Awal (Initial Population)?",
    "id": "Kumpulan Solusi Awal (Algoritma Genetik)."
  },
  {
    "en": "Apa Itu Fungsi Kebugaran (Fitness Function)?",
    "id": "Ukuran Kualitas Solusi (Algoritma Genetik)."
  },
  {
    "en": "Apa Itu Seleksi (Selection) (Algoritma Genetik)?",
    "id": "Memilih Individu Terbaik (Reproduksi)."
  },
  {
    "en": "Apa Itu Pindah Silang (Crossover) (Algoritma Genetik)?",
    "id": "Kombinasi Genetik Dua Induk."
  },
  {
    "en": "Apa Itu Mutasi (Mutation) (Algoritma Genetik)?",
    "id": "Perubahan Acak Gen (Diversitas)."
  },
  {
    "en": "Apa Itu Kriptografi (Cryptography)?",
    "id": "Ilmu Teknik Enkripsi Dekripsi Pesan."
  },
  {
    "en": "Apa Itu Kriptanalisis (Cryptanalysis)?",
    "id": "Ilmu Teknik Memecahkan Kode Terenkripsi."
  },
  {
    "en": "Apa Itu Plaintext (Teks Polos)?",
    "id": "Pesan Asli (Sebelum Enkripsi)."
  },
  {
    "en": "Apa Itu Ciphertext (Teks Sandi)?",
    "id": "Pesan Terenkripsi (Setelah Enkripsi)."
  },
  {
    "en": "Apa Itu Enkripsi (Encryption)?",
    "id": "Proses Mengubah Plaintext Menjadi Ciphertext."
  },
  {
    "en": "Apa Itu Dekripsi (Decryption)?",
    "id": "Proses Mengubah Ciphertext Menjadi Plaintext."
  },
  {
    "en": "Apa Itu Kunci (Key) Kriptografi?",
    "id": "Informasi Rahasia (Algoritma Enkripsi)."
  },
  {
    "en": "Apa Itu Kriptografi Kunci Simetris?",
    "id": "Satu Kunci (Sama) Enkripsi Dekripsi."
  },
  {
    "en": "Apa Itu Kriptografi Kunci Asimetris?",
    "id": "Dua Kunci (Publik Privat) Berbeda."
  },
  {
    "en": "Nama Lain Kriptografi Kunci Asimetris?",
    "id": "Kriptografi Kunci Publik."
  },
  {
    "en": "Apa Itu Kunci Publik (Public Key)?",
    "id": "Kunci (Disebarluaskan) (Enkripsi Verifikasi)."
  },
  {
    "en": "Apa Itu Kunci Privat (Private Key)?",
    "id": "Kunci (Dirahasiakan) (Dekripsi Tanda Tangan)."
  },
  {
    "en": "Contoh Algoritma Simetris?",
    "id": "AES (Advanced Encryption Standard), DES (Data Encryption Standard), RC4."
  },
  {
    "en": "Contoh Algoritma Asimetris?",
    "id": "RSA (Rivest-Shamir-Adleman), ECC (Elliptic Curve Cryptography)."
  },
  {
    "en": "Apa Kepanjangan AES (Advanced Encryption Standard)?",
    "id": "Advanced Encryption Standard."
  },
  {
    "en": "Apa Kepanjangan RSA (Rivest-Shamir-Adleman)?",
    "id": "Rivest-Shamir-Adleman."
  },
  {
    "en": "Apa Kepanjangan ECC (Elliptic Curve Cryptography)?",
    "id": "Elliptic Curve Cryptography."
  },
  {
    "en": "Apa Itu Fungsi Hash (Hash Function)?",
    "id": "Fungsi Satu Arah (Ukuran Tetap)."
  },
  {
    "en": "Sifat Fungsi Hash Kriptografis?",
    "id": "Tahan Kolisi, Tahan Preimage."
  },
  {
    "en": "Apa Itu Kolisi (Collision) Hash?",
    "id": "Dua Input Berbeda Menghasilkan Hash Sama."
  },
  {
    "en": "Apa Kepanjangan MD5 (Message Digest 5)?",
    "id": "Message Digest 5 (Hash Kuno)."
  },
  {
    "en": "Apa Kepanjangan SHA (Secure Hash Algorithm)?",
    "id": "Secure Hash Algorithm (Contoh: SHA-256)."
  },
  {
    "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?",
    "id": "Validasi Keaslian Integritas Data (Kunci Privat)."
  },
  {
    "en": "Apa Kepanjangan CA (Certificate Authority)?",
    "id": "Certificate Authority (Otoritas Sertifikat)."
  },
  {
    "en": "Apa Itu Sertifikat Digital (X.509)?",
    "id": "File (Mengikat Kunci Publik Identitas)."
  },
  {
    "en": "Apa Kepanjangan PKI (Public Key Infrastructure)?",
    "id": "Public Key Infrastructure."
  },
  {
    "en": "Apa Kepanjangan SSL (Secure Sockets Layer)?",
    "id": "Secure Sockets Layer (Pendahulu TLS (Transport Layer Security))."
  },
  {
    "en": "Apa Kepanjangan TLS (Transport Layer Security)?",
    "id": "Transport Layer Security (HTTPS (Hypertext Transfer Protocol Secure))."
  },
  {
    "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?",
    "id": "Serangan (Penyadapan Di Tengah Komunikasi)."
  },
  {
    "en": "Apa Itu Serangan Brute Force (Brute Force Attack)?",
    "id": "Serangan (Mencoba Semua Kombinasi Kunci)."
  },
  {
    "en": "Apa Itu Serangan Kamus (Dictionary Attack)?",
    "id": "Serangan (Mencoba Kata Sandi Kamus)."
  },
  {
    "en": "Apa Itu Nonce (Nonce)?",
    "id": "Angka (Digunakan Sekali Saja) (Proteksi Replay)."
  },
  {
    "en": "Apa Itu Salt (Garam) (Hash)?",
    "id": "Data Acak (Ditambah Password Sebelum Hash)."
  },
  {
    "en": "Apa Itu Mode Operasi (Mode of Operation) Sandi Blok?",
    "id": "Cara Mengenkripsi Blok Data Berurutan."
  },
  {
    "en": "Apa Itu Mode ECB (Electronic Codebook)?",
    "id": "Mode (Setiap Blok Dienkripsi Sama) (Tidak Aman)."
  },
  {
    "en": "Apa Itu Mode CBC (Cipher Block Chaining)?",
    "id": "Mode (Ciphertext Sebelumnya Di-XOR Plaintext)."
  },
  {
    "en": "Apa Itu Vektor Inisialisasi (Initialization Vector/IV)?",
    "id": "Blok Data Acak Awal (Mode CBC (Cipher Block Chaining))."
  },
  {
    "en": "Apa Itu Padding (Bantalan) Kriptografi?",
    "id": "Menambah Data (Agar Sesuai Ukuran Blok)."
  },
  {
    "en": "Apa Itu Pertukaran Kunci (Key Exchange) Diffie-Hellman?",
    "id": "Metode Aman (Bertukar Kunci Simetris)."
  },
  {
    "en": "Apa Itu Pengolahan Citra (Image Processing)?",
    "id": "Manipulasi Analisis Citra Digital."
  },
  {
    "en": "Apa Itu Piksel (Pixel)?",
    "id": "Elemen Gambar Terkecil."
  },
  {
    "en": "Apa Itu Resolusi (Resolution) Citra?",
    "id": "Jumlah Piksel (Lebar Kali Tinggi)."
  },
  {
    "en": "Apa Itu Kedalaman Warna (Color Depth)?",
    "id": "Jumlah Bit Per Piksel (Menentukan Warna)."
  },
  {
    "en": "Apa Itu Citra Grayscale (Skala Abu-abu)?",
    "id": "Citra (Hanya Informasi Intensitas Hitam-Putih)."
  },
  {
    "en": "Apa Itu Citra Biner (Binary Image)?",
    "id": "Citra (Hanya Hitam Atau Putih)."
  },
  {
    "en": "Apa Itu Model Warna RGB (Red Green Blue)?",
    "id": "Model Aditif (Merah, Hijau, Biru)."
  },
  {
    "en": "Apa Itu Model Warna CMYK (Cyan Magenta Yellow Key)?",
    "id": "Model Subtraktif (Cyan, Magenta, Kuning, Hitam)."
  },
  {
    "en": "Apa Itu Model Warna HSV (Hue Saturation Value)?",
    "id": "Model (Rona, Saturasi, Nilai Kecerahan)."
  },
  {
    "en": "Apa Itu Histogram (Histogram) Citra?",
    "id": "Grafik Distribusi Intensitas Piksel."
  },
  {
    "en": "Apa Itu Ekualisasi (Equalization) Histogram?",
    "id": "Metode Peningkatan Kontras Citra."
  },
  {
    "en": "Apa Itu Operasi Titik (Point Operation)?",
    "id": "Operasi (Nilai Piksel Baru = Fungsi Piksel Lama)."
  },
  {
    "en": "Contoh Operasi Titik (Point Operation)?",
    "id": "Kecerahan (Brightness), Kontras (Contrast)."
  },
  {
    "en": "Apa Itu Koreksi Gamma (Gamma Correction)?",
    "id": "Koreksi Non-Linear (Kecerahan Citra)."
  },
  {
    "en": "Apa Itu Konvolusi (Convolution) Citra?",
    "id": "Operasi Filter (Menggunakan Kernel)."
  },
  {
    "en": "Apa Itu Kernel (Kernel) Konvolusi?",
    "id": "Matriks Kecil (Filter Spasial)."
  },
  {
    "en": "Apa Itu Filter Spasial (Spatial Filter)?",
    "id": "Filter (Bekerja Pada Lingkungan Piksel)."
  },
  {
    "en": "Apa Itu Filter Rata-Rata (Mean Filter)?",
    "id": "Filter Penghalus (Mengaburkan Citra)."
  },
  {
    "en": "Apa Itu Filter Gaussian (Gaussian Blur)?",
    "id": "Filter Penghalus (Bobot Gaussian)."
  },
  {
    "en": "Apa Itu Filter Median (Median Filter)?",
    "id": "Filter Non-Linear (Penghilang Noise Salt-Pepper)."
  },
  {
    "en": "Apa Itu Deteksi Tepi (Edge Detection)?",
    "id": "Menemukan Batas Objek (Perubahan Intensitas)."
  },
  {
    "en": "Apa Itu Operator Sobel (Sobel Operator)?",
    "id": "Kernel Deteksi Tepi (Gradien)."
  },
  {
    "en": "Apa Itu Operator Prewitt (Prewitt Operator)?",
    "id": "Kernel Deteksi Tepi (Mirip Sobel)."
  },
  {
    "en": "Apa Itu Operator Laplacian (Laplacian)?",
    "id": "Kernel Deteksi Tepi (Turunan Kedua)."
  },
  {
    "en": "Apa Itu Detektor Tepi Canny (Canny Edge Detector)?",
    "id": "Algoritma Deteksi Tepi Multi-Tahap."
  },
  {
    "en": "Apa Itu Penajaman (Sharpening) Citra?",
    "id": "Menonjolkan Tepi (Menggunakan Laplacian)."
  },
  {
    "en": "Apa Itu Unsharp Masking (USM)?",
    "id": "Metode Penajaman (Citra Asli - Blur)."
  },
  {
    "en": "Apa Itu Segmentasi (Segmentation) Citra?",
    "id": "Mempartisi Citra Menjadi Region."
  },
  {
    "en": "Apa Itu Pengambangan (Thresholding)?",
    "id": "Segmentasi (Memisahkan Berdasar Nilai Piksel)."
  },
  {
    "en": "Apa Itu Metode Otsu (Otsu's Method)?",
    "id": "Algoritma Penentuan Ambang Batas Otomatis."
  },
  {
    "en": "Apa Itu Operasi Morfologi (Morphological Operations)?",
    "id": "Operasi (Struktur Bentuk Citra Biner)."
  },
  {
    "en": "Apa Itu Dilasi (Dilation) (Morfologi)?",
    "id": "Memperbesar (Menggemukkan) Objek."
  },
  {
    "en": "Apa Itu Erosi (Erosion) (Morfologi)?",
    "id": "Mengecilkan (Mengikis) Objek."
  },
  {
    "en": "Apa Itu Pembukaan (Opening) (Morfologi)?",
    "id": "Erosi Diikuti Dilasi (Hapus Noise Kecil)."
  },
  {
    "en": "Apa Itu Penutupan (Closing) (Morfologi)?",
    "id": "Dilasi Diikuti Erosi (Tutup Lubang Kecil)."
  },
  {
    "en": "Apa Itu Elemen Struktur (Structuring Element)?",
    "id": "Kernel (Inti) Operasi Morfologi."
  },
  {
    "en": "Apa Itu Kompresi Lossless (Tanpa Rugi) Citra?",
    "id": "Kompresi (Citra Bisa Kembali Sempurna)."
  },
  {
    "en": "Contoh Kompresi Lossless (Tanpa Rugi) Citra?",
    "id": "PNG (Portable Network Graphics), BMP (Bitmap) (RLE (Run Length Encoding))."
  },
  {
    "en": "Apa Itu Kompresi Lossy (Berugi) Citra?",
    "id": "Kompresi (Membuang Informasi Visual)."
  },
  {
    "en": "Contoh Kompresi Lossy (Berugi) Citra?",
    "id": "JPEG (Joint Photographic Experts Group)."
  },
  {
    "en": "Apa Kepanjangan DCT (Discrete Cosine Transform)?",
    "id": "Discrete Cosine Transform (Dasar JPEG (Joint Photographic Experts Group))."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization) (JPEG (Joint Photographic Experts Group))?",
    "id": "Proses Pembuangan Informasi (Lossy)."
  },
  {
    "en": "Apa Itu Interpolasi (Interpolation) Piksel?",
    "id": "Estimasi Nilai Piksel (Resize Citra)."
  },
  {
    "en": "Apa Itu Interpolasi Nearest Neighbor (Tetangga Terdekat)?",
    "id": "Interpolasi (Cepat, Kualitas Rendah)."
  },
  {
    "en": "Apa Itu Interpolasi Bilinear (Bilinear)?",
    "id": "Interpolasi (Rata-Rata 4 Piksel)."
  },
  {
    "en": "Apa Itu Interpolasi Bicubic (Bicubic)?",
    "id": "Interpolasi (Rata-Rata 16 Piksel, Halus)."
  },
  {
    "en": "Apa Itu Transformasi Affine (Affine Transform)?",
    "id": "Transformasi Geometris (Translasi, Rotasi, Skala)."
  },
  {
    "en": "Apa Itu Transformasi Perspektif (Perspective Transform)?",
    "id": "Transformasi (Memetakan Persegi Ke Segiempat)."
  },
  {
    "en": "Apa Itu Ruang (Domain) Frekuensi (Citra)?",
    "id": "Representasi Citra (Perubahan Frekuensi)."
  },
  {
    "en": "Apa Itu Filter Lolos Tinggi (High-Pass) (Frekuensi)?",
    "id": "Meloloskan Frekuensi Tinggi (Menajamkan Tepi)."
  },
  {
    "en": "Apa Itu Filter Lolos Rendah (Low-Pass) (Frekuensi)?",
    "id": "Meloloskan Frekuensi Rendah (Menghaluskan/Blur)."
  },
  {
    "en": "Apa Kepanjangan VHDL (VHSIC Hardware Description Language)?",
    "id": "VHSIC (Very High Speed Integrated Circuit) Hardware Description Language."
  },
  {
    "en": "Apa Itu Pustaka (Library) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Kumpulan Paket (Package) Komponen."
  },
  {
    "en": "Apa Pustaka (Library) Standar VHDL (VHSIC Hardware Description Language)?",
    "id": "Pustaka (IEEE (Institute of Electrical Engineers)), Paket (STD_LOGIC_1164)."
  },
  {
    "en": "Apa Itu Entitas (Entity) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Deklarasi Port (Input Output) Desain."
  },
  {
    "en": "Apa Itu Arsitektur (Architecture) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Deskripsi Implementasi Fungsionalitas Entitas."
  },
  {
    "en": "Apa Itu Pemodelan (Modeling) Perilaku (Behavioral)?",
    "id": "Deskripsi Fungsionalitas (Level Tinggi)."
  },
  {
    "en": "Apa Itu Pemodelan (Modeling) Aliran Data (Dataflow)?",
    "id": "Deskripsi Aliran Sinyal (Konkuren)."
  },
  {
    "en": "Apa Itu Pemodelan (Modeling) Struktural (Structural)?",
    "id": "Deskripsi Interkoneksi Komponen."
  },
  {
    "en": "Apa Itu Proses (Process) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Blok Kode Sekuensial (Eksekusi Berurutan)."
  },
  {
    "en": "Apa Itu Daftar Sensitivitas (Sensitivity List)?",
    "id": "Sinyal Pemicu Eksekusi Blok Proses."
  },
  {
    "en": "Apa Itu Sinyal (Signal) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Objek (Penyimpan Nilai, Level Arsitektur)."
  },
  {
    "en": "Apa Itu Variabel (Variable) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Objek (Penyimpan Nilai, Level Proses)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) (VHDL (VHSIC Hardware Description Language))?",
    "id": "Objek (Nilai Tetap)."
  },
  {
    "en": "Apa Tipe Data (Data Type) STD_LOGIC?",
    "id": "Tipe Logika Standar 9-Level."
  },
  {
    "en": "Apa Tipe Data (Data Type) STD_LOGIC_VECTOR?",
    "id": "Array (Vektor) Satu Dimensi STD_LOGIC."
  },
  {
    "en": "Apa Tipe Data (Data Type) BIT (BIT)?",
    "id": "Tipe Logika Standar 2-Level ('0', '1')."
  },
  {
    "en": "Apa Tipe Data (Data Type) SIGNED (SIGNED)?",
    "id": "Vektor (Bilangan Aritmatika Bertanda)."
  },
  {
    "en": "Apa Tipe Data (Data Type) UNSIGNED (UNSIGNED)?",
    "id": "Vektor (Bilangan Aritmatika Tak Bertanda)."
  },
  {
    "en": "Paket (Package) Apa (Menggunakan SIGNED/UNSIGNED)?",
    "id": "Paket (NUMERIC_STD) Pustaka IEEE (Institute of Electrical Engineers)."
  },
  {
    "en": "Apa Itu Operator Konkatenasi (Concatenation) (&)?",
    "id": "Menggabungkan Dua Vektor Atau Sinyal."
  },
  {
    "en": "Apa Itu Generic (VHDL (VHSIC Hardware Description Language))?",
    "id": "Parameter (Konfigurasi Entitas)."
  },
  {
    "en": "Apa Itu Port (VHDL (VHSIC Hardware Description Language))?",
    "id": "Antarmuka (Mode IN (Masuk), OUT (Keluar), INOUT (Masuk Keluar), BUFFER (Penyangga))."
  },
  {
    "en": "Apa Itu Instansiasi (Instantiation) Komponen?",
    "id": "Penggunaan Komponen (Entitas) Dalam Desain Lain."
  },
  {
    "en": "Apa Itu Pernyataan (Statement) 'WAIT' (VHDL (VHSIC Hardware Description Language))?",
    "id": "Menunda Eksekusi (Hanya Dalam Proses)."
  },
  {
    "en": "Apa Itu 'rising_edge(clk)'?",
    "id": "Fungsi (Deteksi Tepi Naik Sinyal Clock)."
  },
  {
    "en": "Siapa Yang Mengusulkan Hukum Elektromagnetisme?",
    "id": "James Clerk Maxwell."
  },
  {
    "en": "Siapa Penemu Induksi Elektromagnetik?",
    "id": "Michael Faraday."
  },
  {
    "en": "Siapa Penemu Baterai (Tumpukan Volta)?",
    "id": "Alessandro Volta."
  },
  {
    "en": "Siapa Yang Mendefinisikan Hubungan V, I, R?",
    "id": "Georg Simon Ohm."
  },
  {
    "en": "Siapa Penemu Arus Bolak-Balik (AC)?",
    "id": "Nikola Tesla (Pengembang Utama)."
  },
  {
    "en": "Siapa Pendukung Utama Arus Searah (DC)?",
    "id": "Thomas Alva Edison."
  },
  {
    "en": "Apa Itu 'Perang Arus' (War of Currents)?",
    "id": "Persaingan Antara Sistem AC (Alternating Current) Dan DC (Direct Current)."
  },
  {
    "en": "Siapa Penemu Transistor (Bersama)?",
    "id": "Bardeen, Brattain, Shockley."
  },
  {
    "en": "Siapa Penemu Gelombang Radio (Elektromagnetik)?",
    "id": "Heinrich Hertz."
  },
  {
    "en": "Satuan Apa Yang Dinamai Dari André-Marie Ampère?",
    "id": "Ampere (Satuan Arus Listrik)."
  },
  {
    "en": "Satuan Apa Yang Dinamai Dari Michael Faraday?",
    "id": "Farad (Satuan Kapasitansi)."
  },
  {
    "en": "Satuan Apa Yang Dinamai Dari Joseph Henry?",
    "id": "Henry (Satuan Induktansi)."
  },
  {
    "en": "Satuan Apa Yang Dinamai Dari James Watt?",
    "id": "Watt (Satuan Daya Listrik)."
  },
  {
    "en": "Satuan Apa Yang Dinamai Dari Gustav Kirchhoff?",
    "id": "Hukum Kirchhoff (Arus Tegangan)."
  },
  {
    "en": "Apa Itu Leyden Jar (Guci Leyden)?",
    "id": "Bentuk Awal Kapasitor."
  },
  {
    "en": "Apa Itu Telegraf (Telegraph)?",
    "id": "Sistem Komunikasi Jarak Jauh (Morse)."
  },
  {
    "en": "Siapa Penemu Kode Morse (Morse Code)?",
    "id": "Samuel Morse."
  },
  {
    "en": "Apa Itu Dinamo (Dynamo)?",
    "id": "Generator Listrik DC (Direct Current) Awal."
  },
  {
    "en": "Apa Itu Tabung Vakum (Vacuum Tube)?",
    "id": "Komponen Elektronik (Penguat/Saklar Awal)."
  },
  {
    "en": "Apa Itu Dioda Tabung Vakum (Fleming Valve)?",
    "id": "Penyearah (Rectifier) Tabung Vakum."
  },
  {
    "en": "Apa Itu Trioda (Triode) Tabung Vakum?",
    "id": "Penguat (Amplifier) Tabung Vakum Pertama."
  },
  {
    "en": "Siapa Penemu Trioda (Triode)?",
    "id": "Lee De Forest."
  },
  {
    "en": "Apa Itu Resistor Komposisi Karbon (Carbon Comp)?",
    "id": "Resistor Kuno (Campuran Karbon Perekat)."
  },
  {
    "en": "Apa Itu Resistor Film Karbon (Carbon Film)?",
    "id": "Resistor (Lapisan Tipis Karbon)."
  },
  {
    "en": "Apa Itu Resistor Film Logam (Metal Film)?",
    "id": "Resistor Presisi (Lapisan Tipis Logam)."
  },
  {
    "en": "Apa Itu Resistor Kawat Lilit (Wirewound)?",
    "id": "Resistor (Kawat Resistif Dililitkan)."
  },
  {
    "en": "Karakteristik Resistor Kawat Lilit (Wirewound)?",
    "id": "Daya Tinggi, Presisi, Induktif."
  },
  {
    "en": "Apa Itu Resistor Foil Logam (Metal Foil)?",
    "id": "Resistor Presisi Sangat Tinggi (Stabilitas)."
  },
  {
    "en": "Apa Itu Resistor Oksida Logam (Metal Oxide)?",
    "id": "Resistor (Tahan Suhu Tinggi)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe J?",
    "id": "Besi (Iron) - Konstantan (Constantan)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe K?",
    "id": "Chromel (Kromel) - Alumel (Alumel)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe T?",
    "id": "Tembaga (Copper) - Konstantan (Constantan)."
  },
  {
    "en": "Apa Itu Termokopel (Thermocouple) Tipe E?",
    "id": "Chromel (Kromel) - Konstantan (Constantan)."
  },
  {
    "en": "Termokopel (Thermocouple) Tipe Apa Paling Sensitif?",
    "id": "Tipe E (Perubahan Tegangan/Suhu Besar)."
  },
  {
    "en": "Termokopel (Thermocouple) Tipe Apa Paling Umum?",
    "id": "Tipe K (Rentang Lebar, Murah)."
  },
  {
    "en": "Termokopel (Thermocouple) Tipe Apa (Suhu Sangat Tinggi)?",
    "id": "Tipe R, S, B (Platinum-Rhodium)."
  },
  {
    "en": "Apa Warna Kabel Tipe J (Standar ANSI)?",
    "id": "Putih (Positif) - Merah (Negatif)."
  },
  {
    "en": "Apa Warna Kabel Tipe K (Standar ANSI)?",
    "id": "Kuning (Positif) - Merah (Negatif)."
  },
  {
    "en": "Apa Warna Kabel Tipe T (Standar ANSI)?",
    "id": "Biru (Positif) - Merah (Negatif)."
  },
  {
    "en": "Apa Itu Sambungan Referensi (Cold Junction)?",
    "id": "Titik Sambungan Termokopel Ke Voltmeter."
  },
  {
    "en": "Apa Kepanjangan CJC (Cold Junction Compensation)?",
    "id": "Cold Junction Compensation (Kompensasi Suhu)."
  },
  {
    "en": "Apa Itu Permitivitas (Permittivity) (ε)?",
    "id": "Kemampuan Material (Menyimpan Energi Listrik)."
  },
  {
    "en": "Apa Itu Permitivitas Relatif (Dielectric Constant)?",
    "id": "Rasio Permitivitas Bahan Terhadap Vakum."
  },
  {
    "en": "Apa Itu Tangen Rugi (Loss Tangent) (Tan δ)?",
    "id": "Ukuran Rugi (Disipasi) Energi Dielektrik."
  },
  {
    "en": "Apa Arti Tangen Rugi (Loss Tangent) Rendah?",
    "id": "Bahan Isolator Dielektrik Yang Baik."
  },
  {
    "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?",
    "id": "Medan Listrik Maksimum (Sebelum Tembus)."
  },
  {
    "en": "Apa Satuan Kekuatan Dielektrik?",
    "id": "Volt Per Meter (V/m) (Atau KV/mm)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Dielektrik?",
    "id": "Pergeseran Muatan Internal (Medan E)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Elektronik?",
    "id": "Pergeseran Awan Elektron (Sangat Cepat)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Ionik?",
    "id": "Pergeseran Ion Positif Negatif (Kristal)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Orientasi (Dipolar)?",
    "id": "Pelurusan Momen Dipol Permanen (Air)."
  },
  {
    "en": "Apa Itu Bahan Dielektrik Non-Polar?",
    "id": "Tidak Ada Momen Dipol Permanen (Teflon)."
  },
  {
    "en": "Apa Itu Bahan Dielektrik Polar?",
    "id": "Memiliki Momen Dipol Permanen (Air)."
  },
  {
    "en": "Apa Itu Rugi Dielektrik (Dielectric Loss)?",
    "id": "Energi Hilang (Panas) Akibat Polarisasi AC."
  },
  {
    "en": "Apa Itu Relaksasi (Relaxation) Dielektrik?",
    "id": "Waktu Tunda Polarisasi (Mengikuti Medan AC)."
  },
  {
    "en": "Apa Itu Bahan Piroelektrik (Pyroelectric)?",
    "id": "Polarisasi Berubah Akibat Perubahan Suhu."
  },
  {
    "en": "Apa Itu Bahan Ferroelektrik (Ferroelectric)?",
    "id": "Polarisasi Spontan (Bisa Dibalik Medan E)."
  },
  {
    "en": "Contoh Bahan Ferroelektrik (Ferroelectric)?",
    "id": "Barium Titanat (BaTiO3), PZT."
  },
  {
    "en": "Apa Itu Histeresis (Hysteresis) Ferroelektrik?",
    "id": "Kurva (Polarisasi Vs Medan E) (Loop)."
  },
  {
    "en": "Apa Itu Permeabilitas (Permeability) Magnetik (μ)?",
    "id": "Kemampuan Material (Dilewati Fluks Magnetik)."
  },
  {
    "en": "Apa Itu Bahan Diamagnetik (Diamagnetic)?",
    "id": "Menolak Lemah Medan Magnet (μ < μ0)."
  },
  {
    "en": "Contoh Bahan Diamagnetik (Diamagnetic)?",
    "id": "Tembaga, Air, Bismut."
  },
  {
    "en": "Apa Itu Bahan Paramagnetik (Paramagnetic)?",
    "id": "Tertarik Lemah Medan Magnet (μ > μ0)."
  },
  {
    "en": "Contoh Bahan Paramagnetik (Paramagnetic)?",
    "id": "Aluminium, Platinum, Oksigen."
  },
  {
    "en": "Apa Itu Bahan Feromagnetik (Ferromagnetic)?",
    "id": "Tertarik Kuat Medan Magnet (μ >> μ0)."
  },
  {
    "en": "Contoh Bahan Feromagnetik (Ferromagnetic)?",
    "id": "Besi, Nikel, Kobalt."
  },
  {
    "en": "Apa Itu Domain (Domain) Magnetik?",
    "id": "Area Momen Magnetik Searah (Feromagnetik)."
  },
  {
    "en": "Apa Itu Titik Curie (Curie Temperature)?",
    "id": "Suhu (Feromagnetik Kehilangan Sifatnya)."
  },
  {
    "en": "Apa Itu Bahan Antiferomagnetik (Antiferromagnetic)?",
    "id": "Momen Magnetik (Berlawanan, Saling Menghapus)."
  },
  {
    "en": "Apa Itu Bahan Ferimagnetik (Ferrimagnetic)?",
    "id": "Momen Magnetik (Berlawanan, Tidak Seimbang)."
  },
  {
    "en": "Contoh Bahan Ferimagnetik (Ferrimagnetic)?",
    "id": "Ferit (Ferrite) (Magnetit Fe3O4)."
  },
  {
    "en": "Apa Itu Magnetostriksi (Magnetostriction)?",
    "id": "Perubahan Bentuk Material (Medan Magnet)."
  },
  {
    "en": "Apa Itu Model Jaringan (Network Model) OSI?",
    "id": "Model Referensi 7 Lapisan (Layer)."
  },
  {
    "en": "Apa Itu Model Jaringan (Network Model) TCP/IP?",
    "id": "Model Praktis 4 Lapisan (Layer)."
  },
  {
    "en": "Lapisan (Layer) Apa (Physical) Di TCP/IP?",
    "id": "Lapisan Akses Jaringan (Network Access)."
  },
  {
    "en": "Lapisan (Layer) Apa (Data Link) Di TCP/IP?",
    "id": "Lapisan Akses Jaringan (Network Access)."
  },
  {
    "en": "Lapisan (Layer) Apa (Network) Di TCP/IP?",
    "id": "Lapisan Internet (Internet Layer)."
  },
  {
    "en": "Lapisan (Layer) Apa (Transport) Di TCP/IP?",
    "id": "Lapisan Transportasi (Transport Layer)."
  },
  {
    "en": "Lapisan (Layer) Apa (Session, Presentation, Application) Di TCP/IP?",
    "id": "Lapisan Aplikasi (Application Layer)."
  },
  {
    "en": "Protokol (Protocol) Apa Di Lapisan Internet TCP/IP?",
    "id": "IP (Internet Protocol), ICMP (Internet Control Message Protocol), ARP (Address Resolution Protocol)."
  },
  {
    "en": "Protokol (Protocol) Apa Di Lapisan Transportasi TCP/IP?",
    "id": "TCP (Transmission Control Protocol), UDP (User Datagram Protocol)."
  },
  {
    "en": "Protokol (Protocol) Apa Di Lapisan Aplikasi TCP/IP?",
    "id": "HTTP (Hypertext Transfer Protocol), FTP (File Transfer Protocol), SMTP (Simple Mail Transfer Protocol), DNS (Domain Name System)."
  },
  {
    "en": "Apa Itu Sinyal Baseband (Baseband)?",
    "id": "Sinyal Frekuensi Asli (Belum Modulasi)."
  },
  {
    "en": "Apa Itu Sinyal Broadband (Broadband)?",
    "id": "Transmisi Multi-Sinyal (Beda Frekuensi)."
  },
  {
    "en": "Apa Itu Sinyal Passband (Passband)?",
    "id": "Sinyal (Modulasi) Terpusat Frekuensi Pembawa."
  },
  {
    "en": "Apa Itu Pengkodean (Coding) Saluran (Channel Coding)?",
    "id": "Penambahan Redundansi (Deteksi Koreksi Error)."
  },
  {
    "en": "Apa Itu Pengkodean (Coding) Sumber (Source Coding)?",
    "id": "Kompresi Data (Menghapus Redundansi Sumber)."
  },
  {
    "en": "Apa Itu Kompresi (Compression) Huffman?",
    "id": "Metode Kompresi Lossless (Statistik Simbol)."
  },
  {
    "en": "Apa Itu Kompresi (Compression) LZW (Lempel-Ziv-Welch)?",
    "id": "Metode Kompresi Lossless (Berbasis Kamus)."
  },
  {
    "en": "Apa Itu Run-Length Encoding (RLE)?",
    "id": "Kompresi Lossless (Data Berulang)."
  },
  {
    "en": "Apa Itu Pulse Code Modulation (PCM)?",
    "id": "Metode Konversi Sinyal Analog Ke Digital."
  },
  {
    "en": "Apa Tiga Langkah PCM (Pulse Code Modulation)?",
    "id": "Sampling (Pencuplikan), Kuantisasi (Quantization), Pengkodean (Coding)."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization) Seragam (Uniform)?",
    "id": "Level Kuantisasi (Berjarak Sama)."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization) Non-Seragam?",
    "id": "Level Kuantisasi (Berjarak Tidak Sama)."
  },
  {
    "en": "Apa Itu Companding (Companding)?",
    "id": "Kompresi Ekspansi (Kuantisasi Non-Seragam)."
  },
  {
    "en": "Apa Itu Hukum-A (A-Law) (Companding)?",
    "id": "Standar Companding (Eropa)."
  },
  {
    "en": "Apa Itu Hukum-Mu (Mu-Law) (Companding)?",
    "id": "Standar Companding (Amerika Utara, Jepang)."
  },
  {
    "en": "Apa Itu Delta Modulation (DM)?",
    "id": "Metode Konversi (Hanya Mengirim Beda 1-Bit)."
  },
  {
    "en": "Apa Itu Distorsi (Distortion) Slope Overload (DM)?",
    "id": "Sinyal Input Berubah Terlalu Cepat."
  },
  {
    "en": "Apa Itu Distorsi (Distortion) Granular Noise (DM)?",
    "id": "Sinyal Input Berubah Terlalu Lambat."
  },
  {
    "en": "Apa Itu Adaptive Delta Modulation (ADM)?",
    "id": "Modulasi Delta (Ukuran Langkah Adaptif)."
  },
  {
    "en": "Apa Kepanjangan DPCM (Differential Pulse Code Modulation)?",
    "id": "Differential Pulse Code Modulation."
  },
  {
    "en": "Apa Prinsip DPCM (Differential Pulse Code Modulation)?",
    "id": "Mengkodekan Selisih (Prediksi Nilai Sampel)."
  },
  {
    "en": "Apa Kepanjangan ADPCM (Adaptive DPCM)?",
    "id": "Adaptive Differential Pulse Code Modulation."
  },
  {
    "en": "Apa Itu Analisis Aliran Daya (Load Flow)?",
    "id": "Kalkulasi Tegangan, Sudut Fasa, Aliran Daya Jaringan."
  },
  {
    "en": "Apa Itu Sistem Per-Unit (Per-Unit System)?",
    "id": "Normalisasi Besaran Listrik (Menggunakan Nilai Basis)."
  },
  {
    "en": "Apa Itu Daya Basis (Base MVA/VA)?",
    "id": "Nilai Daya Referensi (Umum) Sistem Per-Unit."
  },
  {
    "en": "Apa Itu Tegangan Basis (Base Voltage)?",
    "id": "Nilai Tegangan Referensi (Tiap Zona) Sistem Per-Unit."
  },
  {
    "en": "Apa Itu Bus (Bus) Slack (Swing)?",
    "id": "Bus Referensi (V=1.0 pu, δ=0) (Menyeimbangkan Rugi Daya)."
  },
  {
    "en": "Apa Itu Bus (Bus) PQ (Load Bus)?",
    "id": "Bus Beban (P Dan Q Diketahui, V Dan δ Dicari)."
  },
  {
    "en": "Apa Itu Bus (Bus) PV (Generator Bus)?",
    "id": "Bus Pembangkit (P Dan |V| Diketahui, Q Dan δ Dicari)."
  },
  {
    "en": "Apa Itu Metode Gauss-Seidel (Aliran Daya)?",
    "id": "Metode Iteratif Sederhana (Solusi Aliran Daya)."
  },
  {
    "en": "Apa Itu Metode Newton-Raphson (Aliran Daya)?",
    "id": "Metode Iteratif Cepat (Solusi Aliran Daya)."
  },
  {
    "en": "Apa Itu Matriks Admitansi Bus (Ybus)?",
    "id": "Matriks (Hubungan Arus Injeksi Tegangan Simpul)."
  },
  {
    "en": "Apa Itu Matriks Jacobian (Jacobian Matrix) (Aliran Daya)?",
    "id": "Matriks Turunan Parsial (Metode Newton-Raphson)."
  },
  {
    "en": "Apa Itu Rugi-Rugi Saluran (Line Losses)?",
    "id": "Energi Hilang (Panas I²R) Di Saluran Transmisi."
  },
  {
    "en": "Apa Itu Bus (Bus) Tak Terhingga (Infinite Bus)?",
    "id": "Sumber Ideal (Tegangan Frekuensi Konstan Sempurna)."
  },
  {
    "en": "Apa Itu Kontrol Aliran Daya (Power Flow Control)?",
    "id": "Mengatur Aliran Daya (Menggunakan Perangkat FACTS)."
  },
  {
    "en": "Apa Itu Analisis Kontingensi (Contingency Analysis)?",
    "id": "Simulasi Efek Kegagalan (Satu Komponen)."
  },
  {
    "en": "Apa Itu Memori Virtual (Virtual Memory)?",
    "id": "Teknik (RAM (Random Access Memory) Terlihat Lebih Besar Dari Fisik)."
  },
  {
    "en": "Apa Itu Alamat (Address) Virtual?",
    "id": "Alamat Logis (Dihasilkan CPU (Central Processing Unit))."
  },
  {
    "en": "Apa Itu Alamat (Address) Fisik?",
    "id": "Alamat Aktual (Di Memori RAM (Random Access Memory) Fisik)."
  },
  {
    "en": "Apa Kepanjangan MMU (Memory Management Unit)?",
    "id": "Memory Management Unit."
  },
  {
    "en": "Apa Fungsi MMU (Memory Management Unit)?",
    "id": "Menerjemahkan Alamat Virtual Ke Fisik."
  },
  {
    "en": "Apa Itu Tabel Halaman (Page Table)?",
    "id": "Tabel (Pemetaan Halaman Virtual Fisik)."
  },
  {
    "en": "Apa Itu Entri (Entry) Tabel Halaman (PTE)?",
    "id": "Satu Baris (Informasi Pemetaan Halaman)."
  },
  {
    "en": "Apa Itu Halaman (Page) Memori?",
    "id": "Blok Memori Virtual (Ukuran Tetap)."
  },
  {
    "en": "Apa Itu Bingkai (Frame) Memori?",
    "id": "Blok Memori Fisik (Ukuran Tetap)."
  },
  {
    "en": "Apa Itu Page Fault (Kesalahan Halaman)?",
    "id": "Halaman Virtual (Dicari) Tidak Ada Di RAM (Random Access Memory)."
  },
  {
    "en": "Apa Itu Penanganan (Handling) Page Fault?",
    "id": "Proses OS (Operating System) (Ambil Halaman Dari Disk)."
  },
  {
    "en": "Apa Itu Paging (Paging) Memori?",
    "id": "Teknik Manajemen Memori (Berbasis Halaman)."
  },
  {
    "en": "Apa Itu Paging (Paging) Sesuai Permintaan (Demand Paging)?",
    "id": "Halaman (Dimuat Ke RAM (Random Access Memory) Saat Dibutuhkan)."
  },
  {
    "en": "Apa Itu Algoritma Penggantian Halaman?",
    "id": "Menentukan Halaman (Diusir Dari RAM (Random Access Memory))."
  },
  {
    "en": "Contoh Algoritma Penggantian Halaman?",
    "id": "FIFO (First-In First-Out), LRU (Least Recently Used), Optimal."
  },
  {
    "en": "Apa Kepanjangan LRU (Least Recently Used)?",
    "id": "Least Recently Used (Paling Jarang Digunakan)."
  },
  {
    "en": "Apa Itu Thrashing (Thrashing) Memori?",
    "id": "Kinerja Buruk (Terlalu Sering Page Fault)."
  },
  {
    "en": "Apa Kepanjangan TLB (Translation Lookaside Buffer)?",
    "id": "Translation Lookaside Buffer."
  },
  {
    "en": "Apa Fungsi TLB (Translation Lookaside Buffer)?",
    "id": "Cache (Memori Cepat) Untuk Terjemahan Halaman."
  },
  {
    "en": "Apa Itu TLB (Translation Lookaside Buffer) Hit (Trefo)?",
    "id": "Terjemahan Ditemukan Di TLB (Translation Lookaside Buffer)."
  },
  {
    "en": "Apa Itu TLB (Translation Lookaside Buffer) Miss (Gagal)?",
    "id": "Terjemahan (Harus Dicari Di Page Table)."
  },
  {
    "en": "Apa Itu Segmentasi (Segmentation) Memori?",
    "id": "Manajemen Memori (Segmen Logis)."
  },
  {
    "en": "Apa Itu Bit Kotor (Dirty Bit) (Halaman)?",
    "id": "Menandai Halaman (Telah Diubah/Ditulis)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) Memori?",
    "id": "Mencegah Proses (Mengakses Memori Ilegal)."
  },
  {
    "en": "Apa Kepanjangan RADAR (Radio Detection and Ranging)?",
    "id": "Radio Detection and Ranging."
  },
  {
    "en": "Apa Prinsip Dasar Radar?",
    "id": "Memancarkan Sinyal, Menerima Pantulan (Echo)."
  },
  {
    "en": "Apa Itu Radar Pulsa (Pulse Radar)?",
    "id": "Radar (Memancarkan Pulsa Sinyal Singkat)."
  },
  {
    "en": "Apa Itu Jangkauan (Range) Radar?",
    "id": "Jarak Ke Target."
  },
  {
    "en": "Apa Kepanjangan PRF (Pulse Repetition Frequency)?",
    "id": "Pulse Repetition Frequency (Frekuensi Pengulangan Pulsa)."
  },
  {
    "en": "Apa Kepanjangan PRI (Pulse Repetition Interval)?",
    "id": "Pulse Repetition Interval (Waktu Antar Pulsa)."
  },
  {
    "en": "Apa Hubungan PRF (Pulse Repetition Frequency) Dan PRI (Pulse Repetition Interval)?",
    "id": "PRI (Pulse Repetition Interval) = 1 / PRF (Pulse Repetition Frequency)."
  },
  {
    "en": "Apa Itu Ambiguitas (Ambiguity) Jangkauan (Radar)?",
    "id": "Ketidakmampuan (Menentukan Jangkauan Pasti)."
  },
  {
    "en": "Apa Itu Jangkauan (Range) Maksimum Tak Ambigu?",
    "id": "Jarak (Ditentukan Oleh PRI (Pulse Repetition Interval))."
  },
  {
    "en": "Apa Itu Radar Doppler (Doppler Radar)?",
    "id": "Radar (Mengukur Kecepatan Target)."
  },
  {
    "en": "Apa Itu Pergeseran (Shift) Doppler?",
    "id": "Perubahan Frekuensi (Akibat Gerakan Target)."
  },
  {
    "en": "Apa Itu Radar Pulsa-Doppler (Pulse-Doppler)?",
    "id": "Radar (Mengukur Jangkauan Dan Kecepatan)."
  },
  {
    "en": "Apa Itu Ambiguitas (Ambiguity) Kecepatan (Radar)?",
    "id": "Ketidakmampuan (Menentukan Kecepatan Pasti)."
  },
  {
    "en": "Apa Itu Kecepatan (Velocity) Buta (Blind Speed)?",
    "id": "Kecepatan (Target Tidak Terdeteksi Radar Doppler)."
  },
  {
    "en": "Apa Kepanjangan MTI (Moving Target Indication)?",
    "id": "Moving Target Indication (Indikasi Target Bergerak)."
  },
  {
    "en": "Apa Fungsi MTI (Moving Target Indication)?",
    "id": "Memfilter Objek Diam (Clutter)."
  },
  {
    "en": "Apa Itu Clutter (Kekacauan) (Radar)?",
    "id": "Pantulan Sinyal Tak Diinginkan (Tanah, Hujan)."
  },
  {
    "en": "Apa Kepanjangan FMCW (Frequency Modulated Continuous Wave)?",
    "id": "Frequency Modulated Continuous Wave."
  },
  {
    "en": "Apa Itu Radar CW (Continuous Wave)?",
    "id": "Radar (Memancarkan Sinyal Terus Menerus)."
  },
  {
    "en": "Apa Kepanjangan RCS (Radar Cross-Section)?",
    "id": "Radar Cross-Section (Penampang Lintang Radar)."
  },
  {
    "en": "Apa Arti RCS (Radar Cross-Section)?",
    "id": "Ukuran (Seberapa Terlihat Target Oleh Radar)."
  },
  {
    "en": "Apa Itu Teknologi Siluman (Stealth)?",
    "id": "Teknologi (Mengurangi RCS (Radar Cross-Section))."
  },
  {
    "en": "Apa Itu Filter Pencocok (Matched Filter) (Radar)?",
    "id": "Filter (Memaksimalkan SNR (Signal-to-Noise Ratio) Sinyal Pantulan)."
  },
  {
    "en": "Apa Itu Kompresi Pulsa (Pulse Compression)?",
    "id": "Teknik (Meningkatkan Resolusi Jangkauan)."
  },
  {
    "en": "Apa Itu Resolusi (Resolution) Jangkauan (Radar)?",
    "id": "Kemampuan (Memisahkan Dua Target Dekat)."
  },
  {
    "en": "Apa Itu Resolusi (Resolution) Sudut (Angular) (Radar)?",
    "id": "Kemampuan (Memisahkan Target Sudut Sama)."
  },
  {
    "en": "Apa Itu Lebar Sinar (Beamwidth) Antena Radar?",
    "id": "Lebar Sudut Pancaran Utama Antena."
  },
  {
    "en": "Apa Kepanjangan SAR (Synthetic Aperture Radar)?",
    "id": "Synthetic Aperture Radar (Pencitraan Resolusi Tinggi)."
  },
  {
    "en": "Apa Kepanjangan ISAR (Inverse SAR)?",
    "id": "Inverse Synthetic Aperture Radar."
  },
  {
    "en": "Apa Itu Radar GPR (Ground Penetrating Radar)?",
    "id": "Radar (Mendeteksi Objek Bawah Tanah)."
  },
  {
    "en": "Apa Itu Fabrikasi (Fabrication) Semikonduktor?",
    "id": "Proses (Membuat IC (Integrated Circuit) Di Wafer)."
  },
  {
    "en": "Apa Itu Perakitan (Assembly) IC (Integrated Circuit)?",
    "id": "Proses (Memotong Die (Chip) Mengemasnya)."
  },
  {
    "en": "Apa Itu Pengujian (Testing) IC (Integrated Circuit)?",
    "id": "Pengujian Fungsional Chip (Chip) Selesai."
  },
  {
    "en": "Apa Itu Die (Chip) (IC (Integrated Circuit))?",
    "id": "Potongan Silikon (Sirkuit Terpadu) Individual."
  },
  {
    "en": "Apa Itu Dicing (Pemotongan Dadu) Wafer?",
    "id": "Proses (Memotong Wafer Menjadi Die (Chip))."
  },
  {
    "en": "Apa Itu Pemasangan (Mounting) Die (Chip)?",
    "id": "Menempelkan Die (Chip) Ke Substrat Kemasan."
  },
  {
    "en": "Apa Itu Bahan (Material) Pemasang (Attach) Die (Chip)?",
    "id": "Perekat Epoksi Atau Solder Eutektik."
  },
  {
    "en": "Apa Itu Ikatan Kawat (Wire Bonding)?",
    "id": "Menghubungkan Pad (Bantalan) Die (Chip) Ke Kemasan (Kawat Emas)."
  },
  {
    "en": "Apa Itu Ikatan Bola (Ball Bonding)?",
    "id": "Teknik Ikatan Kawat (Menggunakan Bola Emas)."
  },
  {
    "en": "Apa Itu Ikatan Baji (Wedge Bonding)?",
    "id": "Teknik Ikatan Kawat (Menggunakan Baji)."
  },
  {
    "en": "Apa Itu Flip-Chip (Chip Terbalik)?",
    "id": "Metode Interkoneksi (Die (Chip) Dibalik)."
  },
  {
    "en": "Apa Itu Benjolan (Bump) Solder (Flip-Chip)?",
    "id": "Tonjolan Solder (Penghubung Die (Chip) Ke Substrat)."
  },
  {
    "en": "Apa Itu Leadframe (Rangka Kaki)?",
    "id": "Rangka Logam (Kaki Pin Kemasan IC (Integrated Circuit))."
  },
  {
    "en": "Apa Itu Substrat (Substrate) Kemasan?",
    "id": "Papan Sirkuit Mini (Dasar Kemasan BGA (Ball Grid Array))."
  },
  {
    "en": "Apa Itu Enkapsulasi (Encapsulation) (IC (Integrated Circuit))?",
    "id": "Proses (Menutup Die (Chip) (Cetakan Plastik))."
  },
  {
    "en": "Apa Kepanjangan DIP (Dual In-line Package)?",
    "id": "Dual In-line Package (Kemasan Kaki Dua Sisi)."
  },
  {
    "en": "Apa Kepanjangan SOP (Small Outline Package)?",
    "id": "Small Outline Package (Versi SMD (Surface Mount Device) Dari DIP (Dual In-line Package))."
  },
  {
    "en": "Apa Kepanjangan QFP (Quad Flat Package)?",
    "id": "Quad Flat Package (Kemasan Kaki Empat Sisi)."
  },
  {
    "en": "Apa Itu Kaki (Lead) Gull-Wing (Sayap Camar)?",
    "id": "Bentuk Kaki (Lead) (QFP (Quad Flat Package), SOIC (Small Outline Integrated Circuit))."
  },
  {
    "en": "Apa Itu Kaki (Lead) J-Lead (J-Lead)?",
    "id": "Bentuk Kaki (Lead) (PLCC (Plastic Leaded Chip Carrier))."
  },
  {
    "en": "Apa Kepanjangan QFN (Quad Flat No-leads)?",
    "id": "Quad Flat No-leads (Kemasan Tanpa Kaki, Pad (Bantalan) Bawah)."
  },
  {
    "en": "Apa Kepanjangan BGA (Ball Grid Array)?",
    "id": "Ball Grid Array (Kemasan Bola Solder Bawah)."
  },
  {
    "en": "Apa Itu Underfill (Isian Bawah) (Flip-Chip)?",
    "id": "Epoksi (Mengisi Celah Die (Chip) Substrat)."
  },
  {
    "en": "Apa Itu Hermetic Seal (Segel Hermetik)?",
    "id": "Segel Kedap Udara (Militer/Medis)."
  },
  {
    "en": "Apa Itu Koefisien Muai Panas (CTE)?",
    "id": "Ukuran Pemuaian Material (Suhu)."
  },
  {
    "en": "Apa Itu Stres (Stress) Termomekanis?",
    "id": "Stres (Akibat Beda CTE (Coefficient of Thermal Expansion))."
  },
  {
    "en": "Apa Itu Kemasan (Package) 3D (Tiga Dimensi) IC (Integrated Circuit)?",
    "id": "Menumpuk Beberapa Die (Chip) Vertikal."
  },
  {
    "en": "Apa Kepanjangan TSV (Through-Silicon Via)?",
    "id": "Through-Silicon Via (Interkoneksi Vertikal Die (Chip))."
  },
  {
    "en": "Apa Itu Pengujian (Test) ATE (Automated Test Equipment)?",
    "id": "Automated Test Equipment (Pengujian IC (Integrated Circuit) Otomatis)."
  },
  {
    "en": "Apa Itu Pembagian (Binning) Chip?",
    "id": "Sortir Chip (Berdasar Kinerja/Kecepatan)."
  },
  {
    "en": "Apa Itu Yield (Hasil) Pabrikasi?",
    "id": "Persentase Die (Chip) Fungsional Per Wafer."
  },
  {
    "en": "Apa Itu Integritas Daya (Power Integrity/PI)?",
    "id": "Analisis Kualitas Jaringan Distribusi Daya (PDN)."
  },
  {
    "en": "Apa Itu Jaringan Distribusi Daya (PDN)?",
    "id": "Jalur (Rel, Bidang) Penghantar Daya PCB (Printed Circuit Board)."
  },
  {
    "en": "Apa Itu Impedansi Target (Target Impedance) PDN?",
    "id": "Impedansi Maksimum (PDN) Di Frekuensi Tertentu."
  },
  {
    "en": "Apa Itu Riak (Noise) Catu Daya?",
    "id": "Fluktuasi (Noise) Pada Rel Catu Daya."
  },
  {
    "en": "Apa Itu Kapasitor Bulk (Bulk Capacitor)?",
    "id": "Menyuplai Arus Frekuensi Rendah (Besar)."
  },
  {
    "en": "Apa Itu Kapasitor Decoupling (Decoupling Capacitor)?",
    "id": "Menyuplai Arus Frekuensi Menengah (Lokal)."
  },
  {
    "en": "Apa Itu Kapasitor Bypass (Bypass Capacitor)?",
    "id": "Menyuplai Arus Frekuensi Tinggi (Sangat Dekat Pin)."
  },
  {
    "en": "Apa Itu Induktansi Penyebaran (Spreading Inductance)?",
    "id": "Induktansi Jalur Bidang Catu Daya."
  },
  {
    "en": "Apa Itu Resonansi (Resonance) PDN (Power Distribution Network)?",
    "id": "Puncak Impedansi (Kapasitor Induktansi Parasitik)."
  },
  {
    "en": "Bagaimana Meredam Resonansi (Resonance) PDN (Power Distribution Network)?",
    "id": "Kapasitor Nilai Berbeda, Redaman (ESR (Equivalent Series Resistance))."
  },
  {
    "en": "Apa Itu Kapasitor Interplane (Antar Bidang)?",
    "id": "Kapasitansi Parasitik (Antara VCC (Voltage Common Collector) Ground Plane)."
  },
  {
    "en": "Apa Itu Efek Samping Kapasitor Bypass?",
    "id": "Membentuk Resonansi (Dengan Induktansi Parasitik)."
  },
  {
    "en": "Apa Itu Sistem Berkas (File System)?",
    "id": "Struktur Logis (Organisasi File Disk)."
  },
  {
    "en": "Apa Kepanjangan FAT (File Allocation Table)?",
    "id": "File Allocation Table."
  },
  {
    "en": "Apa Itu FAT (File Allocation Table) 16?",
    "id": "Sistem Berkas (Ukuran Cluster 16-bit)."
  },
  {
    "en": "Apa Itu FAT (File Allocation Table) 32?",
    "id": "Sistem Berkas (Dukungan Volume Besar)."
  },
  {
    "en": "Apa Kepanjangan exFAT (Extended FAT)?",
    "id": "Extended File Allocation Table (Flash Drive)."
  },
  {
    "en": "Apa Kepanjangan NTFS (New Technology File System)?",
    "id": "New Technology File System (Windows)."
  },
  {
    "en": "Fitur Utama NTFS (New Technology File System)?",
    "id": "Jurnaling, Keamanan (ACL (Access Control List)), Kompresi."
  },
  {
    "en": "Apa Itu Jurnaling (Journaling) File System?",
    "id": "Mencatat Perubahan (Pemulihan Cepat)."
  },
  {
    "en": "Apa Itu Ext4 (Fourth Extended File System)?",
    "id": "Sistem Berkas Umum Linux."
  },
  {
    "en": "Apa Kepanjangan APFS (Apple File System)?",
    "id": "Apple File System (MacOS (Macintosh Operating System), iOS (iPhone Operating System))."
  },
  {
    "en": "Apa Itu Blok (Block) (File System)?",
    "id": "Unit Alokasi Penyimpanan Terkecil."
  },
  {
    "en": "Apa Itu Inode (Inode) (File System)?",
    "id": "Struktur Data (Metadata File)."
  },
  {
    "en": "Apa Itu Fragmentasi (Fragmentation) Eksternal?",
    "id": "Ruang Kosong Tersebar (Blok Kecil)."
  },
  {
    "en": "Apa Itu Fragmentasi (Fragmentation) Internal?",
    "id": "Ruang Sisa (Blok Alokasi Terakhir)."
  },
  {
    "en": "Apa Itu Defragmentasi (Defragmentation)?",
    "id": "Menyusun Ulang File (Agar Kontinu)."
  },
  {
    "en": "Apa Kepanjangan SSD (Solid State Drive)?",
    "id": "Solid State Drive (Memori Flash)."
  },
  {
    "en": "Apa Kepanjangan HDD (Hard Disk Drive)?",
    "id": "Hard Disk Drive (Piringan Magnetik)."
  },
  {
    "en": "Apa Itu Wear Leveling (SSD (Solid State Drive))?",
    "id": "Distribusi Penulisan (Meratakan Keausan Flash)."
  },
  {
    "en": "Apa Itu Garbage Collection (SSD (Solid State Drive))?",
    "id": "Mengatur Ulang Blok (Menyiapkan Halaman)."
  },
  {
    "en": "Apa Itu Perintah TRIM (TRIM Command)?",
    "id": "Perintah OS (Operating System) (Halaman Tidak Valid)."
  },
  {
    "en": "Apa Kepanjangan S.M.A.R.T. (SMART)?",
    "id": "Self-Monitoring, Analysis, Reporting Technology."
  },
  {
    "en": "Apa Itu Bad Sector (Sektor Rusak)?",
    "id": "Area Disk (Tidak Dapat Dibaca/Ditulis)."
  },
  {
    "en": "Apa Kepanjangan RAID (Redundant Array of Independent Disks)?",
    "id": "Redundant Array of Independent Disks."
  },
  {
    "en": "Apa Itu RAID 0 (Striping)?",
    "id": "Data Dibagi (Tanpa Redundansi)."
  },
  {
    "en": "Apa Itu RAID 1 (Mirroring)?",
    "id": "Data Diduplikasi (Redundansi Penuh)."
  },
  {
    "en": "Apa Itu RAID 5 (Striping with Parity)?",
    "id": "Striping (Data) Paritas Terdistribusi."
  },
  {
    "en": "Apa Itu RAID 6 (Striping with Double Parity)?",
    "id": "Striping (Data) Paritas Ganda (Toleran 2 Gagal)."
  },
  {
    "en": "Apa Itu RAID 10 (1+0)?",
    "id": "Gabungan Mirroring (Cermin) Dan Striping (Pita)."
  },
  {
    "en": "Apa Itu Avionik (Avionics)?",
    "id": "Sistem Elektronik Pesawat Terbang."
  },
  {
    "en": "Apa Kepanjangan ARINC (Aeronautical Radio, Incorporated)?",
    "id": "Aeronautical Radio, Incorporated."
  },
  {
    "en": "Apa Itu Standar ARINC 429?",
    "id": "Bus Data Avionik (Satu Arah)."
  },
  {
    "en": "Apa Itu Standar ARINC 664?",
    "id": "Ethernet Switched (Avionik Modern)."
  },
  {
    "en": "Apa Kepanjangan AFDX (Avionics Full-Duplex Switched Ethernet)?",
    "id": "Avionics Full-Duplex Switched Ethernet."
  },
  {
    "en": "Apa Itu Standar MIL-STD-1553?",
    "id": "Bus Data Serial (Militer, Avionik)."
  },
  {
    "en": "Apa Topologi Bus (Bus) MIL-STD-1553?",
    "id": "Bus (Kabel Ganda) Terterminasi."
  },
  {
    "en": "Apa Tiga Tipe Terminal (Terminal) 1553?",
    "id": "Bus Controller (BC), Remote Terminal (RT), Bus Monitor (BM)."
  },
  {
    "en": "Apa Itu Bus Controller (BC) (1553)?",
    "id": "Master (Pengendali) Tunggal Bus (Bus)."
  },
  {
    "en": "Apa Itu Remote Terminal (RT) (1553)?",
    "id": "Slave (Periferal Sensor/Aktuator)."
  },
  {
    "en": "Apa Itu Bus Monitor (BM) (1553)?",
    "id": "Perekam Data (Pasif)."
  },
  {
    "en": "Apa Itu Pengkodean (Encoding) Manchester (1553)?",
    "id": "Pengkodean Biphase (Sinkron Mandiri)."
  },
  {
    "en": "Apa Itu Pesan (Message) Perintah (Command) (1553)?",
    "id": "Instruksi Dari Bus Controller (BC) Ke Remote Terminal (RT)."
  },
  {
    "en": "Apa Itu Pesan (Message) Data (Data) (1553)?",
    "id": "Transfer Data (Antar Terminal)."
  },
  {
    "en": "Apa Itu Pesan (Message) Status (Status) (1553)?",
    "id": "Respon (Tanggapan) Dari Remote Terminal (RT)."
  },
  {
    "en": "Apa Itu SpaceWire (SpaceWire)?",
    "id": "Standar Jaringan Data (Pesawat Luar Angkasa)."
  },
  {
    "en": "Apa Itu Motor Sinkron (Synchronous Motor)?",
    "id": "Kecepatan Rotor = Kecepatan Sinkron."
  },
  {
    "en": "Apa Itu Motor Asinkron (Asynchronous Motor)?",
    "id": "Kecepatan Rotor < Kecepatan Sinkron (Slip)."
  },
  {
    "en": "Apa Itu Motor Reluktansi (Reluctance Motor)?",
    "id": "Torsi (Dihasilkan) Variasi Reluktansi."
  },
  {
    "en": "Apa Itu Motor Histeresis (Hysteresis Motor)?",
    "id": "Torsi (Dihasilkan) Rugi Histeresis Rotor."
  },
  {
    "en": "Apa Itu Eksitasi (Excitation) Motor Sinkron?",
    "id": "Pemberian Arus DC (Direct Current) Ke Lilitan Rotor."
  },
  {
    "en": "Apa Itu Kondisi Over-Excited (Eksitasi Berlebih)?",
    "id": "Motor (Menyuplai Daya Reaktif) (Leading PF)."
  },
  {
    "en": "Apa Itu Kondisi Under-Excited (Eksitasi Kurang)?",
    "id": "Motor (Menyerap Daya Reaktif) (Lagging PF)."
  },
  {
    "en": "Apa Itu Kondensor (Condenser) Sinkron?",
    "id": "Motor Sinkron (Tanpa Beban) (Koreksi PF)."
  },
  {
    "en": "Apa Itu Kurva-V (V-Curves) Motor Sinkron?",
    "id": "Grafik (Arus Jangkar Vs Arus Medan)."
  },
  {
    "en": "Bagaimana Motor Sinkron (Synchronous Motor) Start (Mulai)?",
    "id": "Lilitan Damper (Induksi) Atau VFD (Variable Frequency Drive)."
  },
  {
    "en": "Apa Itu Lilitan Damper (Amortisseur)?",
    "id": "Batang (Rotor) (Start Induksi Meredam Osilasi)."
  },
  {
    "en": "Apa Itu Osilasi (Hunting) Motor Sinkron?",
    "id": "Osilasi Kecepatan Rotor (Sekitar Sinkron)."
  },
  {
    "en": "Apa Itu Sudut Torsi (Torque Angle) (δ)?",
    "id": "Sudut (Antara Fluks Stator Rotor)."
  },
  {
    "en": "Apa Itu Torsi Tarik-Keluar (Pull-Out Torque)?",
    "id": "Torsi Maksimum (Motor Kehilangan Sinkronisasi)."
  },
  {
    "en": "Apa Itu Sinkronoskop (Synchroscope)?",
    "id": "Alat (Visualisasi Sinkronisasi Generator)."
  },
  {
    "en": "Apa Itu Perambatan (Propagation) Gelombang Radio?",
    "id": "Cara Gelombang (Bergerak Dari Pemancar Penerima)."
  },
  {
    "en": "Apa Itu Gelombang Tanah (Ground Wave)?",
    "id": "Gelombang (Mengikuti Kelengkungan Bumi)."
  },
  {
    "en": "Kapan Gelombang Tanah (Ground Wave) Efektif?",
    "id": "Frekuensi Rendah (LF (Low Frequency)/MF (Medium Frequency)) (Radio AM (Amplitude Modulation))."
  },
  {
    "en": "Apa Itu Gelombang Langit (Skywave)?",
    "id": "Gelombang (Dipantulkan Lapisan Ionosfer)."
  },
  {
    "en": "Kapan Gelombang Langit (Skywave) Efektif?",
    "id": "Frekuensi Tinggi (HF (High Frequency)) (Radio Gelombang Pendek)."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Ionosfer?",
    "id": "Lapisan Atmosfer (Terionisasi Sinar UV)."
  },
  {
    "en": "Apa Itu Gelombang Ruang (Space Wave) (LOS)?",
    "id": "Gelombang (Garis Pandang Lurus)."
  },
  {
    "en": "Kapan Gelombang Ruang (Space Wave) Efektif?",
    "id": "Frekuensi Sangat Tinggi (VHF (Very High Frequency)/UHF (Ultra High Frequency)/Microwave)."
  },
  {
    "en": "Apa Itu Fading (Fading) Multipath?",
    "id": "Interferensi (Gelombang Tiba Lewat Banyak Jalur)."
  },
  {
    "en": "Apa Itu Zona Fresnel (Fresnel Zone)?",
    "id": "Area Elips (Antara Pemancar Penerima)."
  },
  {
    "en": "Apa Itu Redaman (Attenuation) Ruang Bebas?",
    "id": "Pelemahan Sinyal (Penyebaran Energi)."
  },
  {
    "en": "Apa Itu Redaman (Attenuation) Hujan?",
    "id": "Pelemahan Sinyal (Diserap Tetesan Hujan)."
  },
  {
    "en": "Apa Itu Hamburan (Ducting) Troposferik?",
    "id": "Perambatan Gelombang (Jauh) (Lapisan Troposfer)."
  },
  {
    "en": "Apa Kepanjangan MUF (Maximum Usable Frequency)?",
    "id": "Maximum Usable Frequency (Frekuensi Maks (HF (High Frequency)) Gelombang Langit)."
  },
  {
    "en": "Apa Kepanjangan LUF (Lowest Usable Frequency)?",
    "id": "Lowest Usable Frequency (Frekuensi Min (HF (High Frequency)) Gelombang Langit)."
  },
  {
    "en": "Apa Itu Jarak Lompatan (Skip Distance)?",
    "id": "Jarak (Pemancar Ke Titik Balik Gelombang Langit)."
  },
  {
    "en": "Apa Itu Zona Hening (Skip Zone)?",
    "id": "Area (Tidak Terjangkau Gelombang Tanah Langit)."
  },
  {
    "en": "Apa Itu Sudut Kritis (Critical Angle) (Ionosfer)?",
    "id": "Sudut (Maksimum Agar Gelombang Kembali)."
  },
  {
    "en": "Apa Itu Frekuensi Kritis (Critical Frequency)?",
    "id": "Frekuensi (Maksimum (Vertikal) Kembali Ke Bumi)."
  },
  {
    "en": "Apa Itu Pengelabuan (Jamming) Sinyal?",
    "id": "Interferensi Sengaja (Menghalangi Komunikasi)."
  },
  {
    "en": "Apa Itu Anti-Jamming (Anti-Jamming)?",
    "id": "Teknik (Melawan Pengelabuan) (Frequency Hopping)."
  },
  {
    "en": "Apa Itu Frequency Hopping (Lompatan Frekuensi)?",
    "id": "Mengubah Frekuensi Pembawa Secara Cepat."
  },
  {
    "en": "Apa Kepanjangan DSSS (Direct-Sequence Spread Spectrum)?",
    "id": "Direct-Sequence Spread Spectrum."
  },
  {
    "en": "Apa Itu Kode Chip (Chipping Code) (DSSS)?",
    "id": "Kode (Penyebar Sinyal) (Bandwidth Lebar)."
  },
  {
    "en": "Apa Itu Logika Tangga (Ladder Logic)?",
    "id": "Bahasa Pemrograman Grafis PLC (Programmable Logic Controller)."
  },
  {
    "en": "Apa Itu Rung (Anak Tangga) (Ladder)?",
    "id": "Satu Baris Logika Horizontal."
  },
  {
    "en": "Apa Itu Rel (Rail) (Ladder)?",
    "id": "Garis Vertikal (Sumber Daya L R)."
  },
  {
    "en": "Apa Itu Kontak (Contact) NO (Normally Open)?",
    "id": "Terbuka (Logika 0) Jika Input 0."
  },
  {
    "en": "Apa Itu Kontak (Contact) NC (Normally Closed)?",
    "id": "Tertutup (Logika 1) Jika Input 0."
  },
  {
    "en": "Apa Itu Koil (Coil) (Ladder)?",
    "id": "Output (Penggerak Relai Internal/Eksternal)."
  },
  {
    "en": "Apa Itu Rangkaian Logika AND (Ladder)?",
    "id": "Dua Kontak (Contact) NO (Normally Open) Seri."
  },
  {
    "en": "Apa Itu Rangkaian Logika OR (Ladder)?",
    "id": "Dua Kontak (Contact) NO (Normally Open) Paralel."
  },
  {
    "en": "Apa Itu Rangkaian Logika NOT (Ladder)?",
    "id": "Satu Kontak (Contact) NC (Normally Closed)."
  },
  {
    "en": "Apa Itu Rangkaian Latch (Pengunci)?",
    "id": "Kontak (Contact) Output Paralel Input (Self-Holding)."
  },
  {
    "en": "Apa Itu Timer (Pewaktu) On-Delay (TON)?",
    "id": "Menunda Output ON (Setelah Input ON)."
  },
  {
    "en": "Apa Itu Timer (Pewaktu) Off-Delay (TOF)?",
    "id": "Menunda Output OFF (Setelah Input OFF)."
  },
  {
    "en": "Apa Itu Timer (Pewaktu) Retentive (RTO)?",
    "id": "Menyimpan Akumulasi Waktu (Input Mati)."
  },
  {
    "en": "Apa Itu Bit (Bit) Enable (EN) Timer?",
    "id": "Aktif Saat Timer (Pewaktu) Diaktifkan."
  },
  {
    "en": "Apa Itu Bit (Bit) Done (DN) Timer?",
    "id": "Aktif Saat Akumulasi = Preset."
  },
  {
    "en": "Apa Itu Bit (Bit) Timer-Timing (TT)?",
    "id": "Aktif Saat Timer (Pewaktu) Menghitung."
  },
  {
    "en": "Apa Itu Counter (Pencacah) Up (CTU)?",
    "id": "Menghitung Naik (Pulsa Tepi Naik)."
  },
  {
    "en": "Apa Itu Counter (Pencacah) Down (CTD)?",
    "id": "Menghitung Turun (Pulsa Tepi Naik)."
  },
  {
    "en": "Apa Itu Bit (Bit) Done (DN) Counter (Pencacah)?",
    "id": "Aktif Saat Akumulasi = Preset."
  },
  {
    "en": "Apa Itu Perintah (Command) Reset (RES)?",
    "id": "Mengatur Ulang Timer (Pewaktu) Counter (Pencacah)."
  },
  {
    "en": "Apa Itu One-Shot Rising (OSR)?",
    "id": "Menghasilkan Pulsa Satu Scan (Tepi Naik)."
  },
  {
    "en": "Apa Itu One-Shot Falling (OSF)?",
    "id": "Menghasilkan Pulsa Satu Scan (Tepi Turun)."
  },
  {
    "en": "Apa Itu Komparasi (Compare) (EQU)?",
    "id": "Sama Dengan (=)."
  },
  {
    "en": "Apa Itu Komparasi (Compare) (NEQ)?",
    "id": "Tidak Sama Dengan (≠)."
  },
  {
    "en": "Apa Itu Komparasi (Compare) (GRT)?",
    "id": "Lebih Besar Dari (>)."
  },
  {
    "en": "Apa Itu Komparasi (Compare) (LES)?",
    "id": "Kurang Dari (<)."
  },
  {
    "en": "Apa Itu Komparasi (Compare) (GEQ)?",
    "id": "Lebih Besar Atau Sama (≥)."
  },
  {
    "en": "Apa Itu Komparasi (Compare) (LEQ)?",
    "id": "Kurang Dari Atau Sama (≤)."
  },
  {
    "en": "Apa Itu Perintah (Command) JUMP (JMP)?",
    "id": "Melompat Ke Label (Rung)."
  },
  {
    "en": "Apa Itu Master Control Relay (MCR)?",
    "id": "Mengaktifkan/Menonaktifkan Zona Logika."
  },
  {
    "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?",
    "id": "Alat Ukur Sinyal Domain Frekuensi."
  },
  {
    "en": "Apa Sumbu Horizontal Penganalisis Spektrum?",
    "id": "Frekuensi (Frequency)."
  },
  {
    "en": "Apa Sumbu Vertikal Penganalisis Spektrum?",
    "id": "Amplitudo (Daya, dBm)."
  },
  {
    "en": "Apa Itu Frekuensi Tengah (Center Frequency)?",
    "id": "Titik Tengah Tampilan Frekuensi."
  },
  {
    "en": "Apa Itu Rentang (Span) (Spectrum Analyzer)?",
    "id": "Lebar Pita Frekuensi Yang Ditampilkan."
  },
  {
    "en": "Apa Itu Zero Span (Rentang Nol)?",
    "id": "Menganalisis Sinyal Domain Waktu (Satu Frekuensi)."
  },
  {
    "en": "Apa Kepanjangan RBW (Resolution Bandwidth)?",
    "id": "Resolution Bandwidth (Lebar Pita Resolusi)."
  },
  {
    "en": "Apa Fungsi RBW (Resolution Bandwidth)?",
    "id": "Filter IF (Menentukan Resolusi Frekuensi)."
  },
  {
    "en": "RBW (Resolution Bandwidth) Sempit (Narrow) Menghasilkan Apa?",
    "id": "Resolusi Baik, Sapuan Lambat."
  },
  {
    "en": "RBW (Resolution Bandwidth) Lebar (Wide) Menghasilkan Apa?",
    "id": "Resolusi Buruk, Sapuan Cepat."
  },
  {
    "en": "Apa Kepanjangan VBW (Video Bandwidth)?",
    "id": "Video Bandwidth (Lebar Pita Video)."
  },
  {
    "en": "Apa Fungsi VBW (Video Bandwidth)?",
    "id": "Menghaluskan Noise (Derau) Tampilan."
  },
  {
    "en": "Kapan VBW (Video Bandwidth) Dibuat Kecil?",
    "id": "Mengurangi Variasi Noise (Derau) (Merata-rata)."
  },
  {
    "en": "Apa Itu Waktu Sapu (Sweep Time) (SWT)?",
    "id": "Waktu Menyapu (Scan) Rentang (Span)."
  },
  {
    "en": "Apa Itu Detektor (Detector) Puncak (Peak)?",
    "id": "Menampilkan Nilai Puncak (Maksimum) Sinyal."
  },
  {
    "en": "Apa Itu Detektor (Detector) Sampel (Sample)?",
    "id": "Mengambil Sampel Sinyal (Tampilan Cepat)."
  },
  {
    "en": "Apa Itu Detektor (Detector) Rata-Rata (Average)?",
    "id": "Merata-ratakan Daya Sinyal."
  },
  {
    "en": "Apa Itu Lantai Derau (Noise Floor) Tampilan?",
    "id": "Level Noise (Derau) Internal Alat Ukur."
  },
  {
    "en": "Apa Itu Pre-Amplifier (Preamplifier) (Spectrum Analyzer)?",
    "id": "Penguat Internal (Menurunkan Noise Floor (NF))."
  },
  {
    "en": "Apa Itu Atenuator (Attenuator) Input (Spectrum Analyzer)?",
    "id": "Melemahkan Sinyal Input (Cegah Saturasi)."
  },
  {
    "en": "Apa Itu Penanda (Marker) (Spectrum Analyzer)?",
    "id": "Alat Baca Nilai (Frekuensi Amplitudo) Puncak."
  },
  {
    "en": "Apa Itu Tampilan Max Hold (Tahan Maks)?",
    "id": "Menyimpan Menampilkan Nilai Puncak Terlama."
  },
  {
    "en": "Apa Itu Tampilan Min Hold (Tahan Min)?",
    "id": "Menyimpan Menampilkan Nilai Minimum Terlama."
  },
  {
    "en": "Apa Itu Spurious (Palsu) Sinyal?",
    "id": "Sinyal Tak Diinginkan (Harmonisa, Intermodulasi)."
  },
  {
    "en": "Apa Itu Phase Noise (Derau Fasa) (Spectrum Analyzer)?",
    "id": "Noise (Derau) Di Sekitar Sinyal Pembawa (Carrier)."
  },
  {
    "en": "Apa Itu Boiler (Ketel Uap)?",
    "id": "Pembangkit Uap (Steam Generator)."
  },
  {
    "en": "Apa Itu Boiler (Ketel Uap) Pipa Air (Water Tube)?",
    "id": "Air Dalam Pipa, Api Diluar."
  },
  {
    "en": "Apa Itu Boiler (Ketel Uap) Pipa Api (Fire Tube)?",
    "id": "Api Dalam Pipa, Air Diluar."
  },
  {
    "en": "Mana (Boiler) Untuk Tekanan Tinggi?",
    "id": "Boiler (Ketel Uap) Pipa Air (Water Tube)."
  },
  {
    "en": "Apa Itu Superheater (Superheater)?",
    "id": "Memanaskan Uap Jenuh (Jadi Uap Kering)."
  },
  {
    "en": "Apa Itu Uap Jenuh (Saturated Steam)?",
    "id": "Uap (Mengandung Titik Air)."
  },
  {
    "en": "Apa Itu Uap Kering (Superheated Steam)?",
    "id": "Uap (Suhu Diatas Titik Didih)."
  },
  {
    "en": "Apa Itu Reheater (Pemanas Ulang)?",
    "id": "Memanaskan Ulang Uap (Antar Turbin)."
  },
  {
    "en": "Apa Itu Economizer (Economizer)?",
    "id": "Memanaskan Air Umpan (Gas Buang)."
  },
  {
    "en": "Apa Itu Air Preheater (Pemanas Udara)?",
    "id": "Memanaskan Udara Pembakaran (Gas Buang)."
  },
  {
    "en": "Apa Itu Turbin (Turbine) Impuls (Impulse)?",
    "id": "Turbin (Energi Kinetik (Pancaran))."
  },
  {
    "en": "Apa Itu Turbin (Turbine) Reaksi (Reaction)?",
    "id": "Turbin (Energi Kinetik Tekanan)."
  },
  {
    "en": "Contoh Turbin (Turbine) Impuls (Air)?",
    "id": "Turbin Pelton."
  },
  {
    "en": "Contoh Turbin (Turbine) Reaksi (Air)?",
    "id": "Turbin Francis, Kaplan."
  },
  {
    "en": "Apa Itu Kondensor (Condenser) (PLTU (Pembangkit Listrik Tenaga Uap))?",
    "id": "Mengubah Uap Bekas Menjadi Air."
  },
  {
    "en": "Apa Itu Menara Pendingin (Cooling Tower)?",
    "id": "Mendinginkan Air Pendingin Kondensor."
  },
  {
    "en": "Apa Itu Pompa (Pump) Air Umpan (Boiler Feed Pump)?",
    "id": "Memompa Air Ke Boiler (Tekanan Tinggi)."
  },
  {
    "en": "Apa Itu Deaerator (Deaerator)?",
    "id": "Menghilangkan Gas Terlarut (Oksigen) Air Umpan."
  },
  {
    "en": "Apa Itu Kipas (Fan) Tekan Paksa (Forced Draft)?",
    "id": "Memasok Udara Pembakaran Ke Boiler."
  },
  {
    "en": "Apa Itu Kipas (Fan) Isap Paksa (Induced Draft)?",
    "id": "Menghisap Gas Buang Dari Boiler."
  },
  {
    "en": "Apa Itu Electrostatic Precipitator (ESP)?",
    "id": "Penangkap Debu Abu (Gas Buang)."
  },
  {
    "en": "Apa Kepanjangan FGD (Flue Gas Desulfurization)?",
    "id": "Flue Gas Desulfurization (Penghilang Sulfur)."
  },
  {
    "en": "Apa Itu Generator (Generator) (Pembangkit)?",
    "id": "Mengubah Energi Mekanik Ke Listrik."
  },
  {
    "en": "Apa Itu Eksiter (Exciter) Generator?",
    "id": "Pembangkit Arus DC (Direct Current) (Medan Magnet Rotor)."
  },
  {
    "en": "Apa Itu AVR (Automatic Voltage Regulator) (Generator)?",
    "id": "Mengatur Tegangan Output (Kontrol Eksitasi)."
  },
  {
    "en": "Apa Itu Elektrolit (Electrolyte) Padat (Solid-State)?",
    "id": "Konduktor Ion Berfasa Padat."
  },
  {
    "en": "Aplikasi Elektrolit (Electrolyte) Padat?",
    "id": "Baterai Solid-State, Sensor Gas."
  },
  {
    "en": "Apa Itu Baterai (Battery) Solid-State?",
    "id": "Baterai (Tanpa Elektrolit Cair)."
  },
  {
    "en": "Apa Itu Memristor (Memristor)?",
    "id": "Komponen Pasif (Resistansi Memori)."
  },
  {
    "en": "Siapa Teoris Memristor (Memristor)?",
    "id": "Leon Chua."
  },
  {
    "en": "Apa Itu Spintronik (Spintronics)?",
    "id": "Elektronik Berbasis Spin Elektron."
  },
  {
    "en": "Apa Kepanjangan MRAM (Magnetoresistive RAM)?",
    "id": "Magnetoresistive Random Access Memory."
  },
  {
    "en": "Apa Kepanjangan GMR (Giant Magnetoresistance)?",
    "id": "Giant Magnetoresistance (Magnetoresistansi Raksasa)."
  },
  {
    "en": "Apa Kepanjangan TMR (Tunnel Magnetoresistance)?",
    "id": "Tunnel Magnetoresistance."
  },
  {
    "en": "Apa Itu Efek (Effect) Spin Hall (Spin Hall)?",
    "id": "Arus Spin (Akibat Arus Listrik)."
  },
  {
    "en": "Apa Itu Metamaterial (Metamaterial)?",
    "id": "Material (Struktur Buatan, Properti Unik)."
  },
  {
    "en": "Apa Itu Indeks (Index) Refraksi Negatif?",
    "id": "Sifat (Metamaterial, Pembelokan Cahaya Unik)."
  },
  {
    "en": "Apa Itu Jubah (Cloaking) Tak Terlihat?",
    "id": "Aplikasi Potensial Metamaterial."
  },
  {
    "en": "Apa Itu Plasmonik (Plasmonics)?",
    "id": "Studi Interaksi Cahaya Elektron Logam."
  },
  {
    "en": "Apa Itu Plasmon (Plasmon) Permukaan?",
    "id": "Osilasi Elektron Kolektif Permukaan Logam."
  },
  {
    "en": "Apa Itu Fotonik (Photonics)?",
    "id": "Ilmu Teknologi Berbasis Foton (Cahaya)."
  },
  {
    "en": "Apa Itu Fotonik (Photonics) Silikon?",
    "id": "Integrasi Komponen Fotonik Di Chip Silikon."
  },
  {
    "en": "Apa Itu Komputasi (Computing) Neuromorfik?",
    "id": "Komputasi (Meniru Struktur Otak Biologis)."
  },
  {
    "en": "Apa Itu Komputasi (Computing) Optik?",
    "id": "Komputasi (Menggunakan Foton, Bukan Elektron)."
  },
  {
    "en": "Apa Itu Efek (Effect) Fotovoltaik (Photovoltaic)?",
    "id": "Pembangkitan Tegangan Akibat Cahaya."
  },
  {
    "en": "Apa Itu Medan Listrik Seragam (Uniform)?",
    "id": "Medan (Nilai Arah Sama) Di Semua Titik."
  },
  {
    "en": "Apa Itu Garis Fluks Listrik?",
    "id": "Garis Imajiner (Representasi Medan E)."
  },
  {
    "en": "Arah Garis Fluks Listrik?",
    "id": "Dari Muatan Positif Ke Negatif."
  },
  {
    "en": "Apa Itu Kerapatan Fluks Listrik (D)?",
    "id": "Fluks Listrik Total Per Satuan Area."
  },
  {
    "en": "Apa Satuan Kerapatan Fluks Listrik (D)?",
    "id": "Coulomb Per Meter Persegi (C/m²)."
  },
  {
    "en": "Apa Bunyi Hukum Gauss (Listrik)?",
    "id": "Fluks Total = Muatan Total Dilingkupi."
  },
  {
    "en": "Apa Itu Permukaan Gaussian (Gaussian Surface)?",
    "id": "Permukaan Tertutup Imajiner (Analisis Gauss)."
  },
  {
    "en": "Apa Itu Gradien Potensial (Potential Gradient)?",
    "id": "Laju Perubahan Potensial Terhadap Jarak."
  },
  {
    "en": "Apa Hubungan Medan (E) Potensial (V)?",
    "id": "E = Negatif Gradien V (E = -∇V)."
  },
  {
    "en": "Apa Itu Dwikutub (Dipole) Listrik?",
    "id": "Sepasang Muatan (Sama Berlawanan) Dekat."
  },
  {
    "en": "Apa Itu Momen (Moment) Dwikutub (Dipole) Listrik (p)?",
    "id": "Muatan Dikalikan Jarak Pemisah (Qd)."
  },
  {
    "en": "Apa Itu Persamaan Laplace (Laplace's Equation)?",
    "id": "Del Kuadrat V = Nol (∇²V = 0)."
  },
  {
    "en": "Kapan Persamaan Laplace (Laplace's Equation) Digunakan?",
    "id": "Daerah Tanpa Muatan Bebas (ρv = 0)."
  },
  {
    "en": "Apa Itu Persamaan Poisson (Poisson's Equation)?",
    "id": "Del Kuadrat V = -ρv / ε."
  },
  {
    "en": "Kapan Persamaan Poisson (Poisson's Equation) Digunakan?",
    "id": "Daerah Yang Mengandung Muatan Bebas."
  },
  {
    "en": "Apa Itu Teorema Keunikan (Uniqueness Theorem)?",
    "id": "Solusi (Laplace/Poisson) Adalah Unik."
  },
  {
    "en": "Apa Itu Metode Pencitraan (Method of Images)?",
    "id": "Metode (Solusi Elektrostatik) Dekat Konduktor."
  },
  {
    "en": "Apa Itu Medan Magnetostatik (Magnetostatic)?",
    "id": "Medan Magnet (Akibat Arus DC (Direct Current) Konstan)."
  },
  {
    "en": "Apa Bunyi Hukum Biot-Savart?",
    "id": "Menghitung Medan Magnet (Elemen Arus dI)."
  },
  {
    "en": "Apa Bunyi Hukum Ampere (Sirkuit)?",
    "id": "Integral Garis H = Arus Dilingkupi (I_enc)."
  },
  {
    "en": "Apa Itu Kerapatan Arus (J)?",
    "id": "Arus Listrik Per Satuan Luas Area."
  },
  {
    "en": "Apa Satuan Kerapatan Arus (J)?",
    "id": "Ampere Per Meter Persegi (A/m²)."
  },
  {
    "en": "Hukum Ampere (Bentuk Titik/Diferensial)?",
    "id": "Curl H = J (∇ × H = J)."
  },
  {
    "en": "Apa Itu Potensial Vektor Magnetik (A)?",
    "id": "Vektor (Curl A = B) (∇ × A = B)."
  },
  {
    "en": "Apa Itu Potensial Skalar Magnetik (Vm)?",
    "id": "Skalar (H = -Gradien Vm) (Daerah J=0)."
  },
  {
    "en": "Apa Itu Induktansi (Inductance) (L)?",
    "id": "Rasio Fluks Terkait (λ) Terhadap Arus (I)."
  },
  {
    "en": "Apa Itu Fluks (Flux) Terkait (Linked Flux) (λ)?",
    "id": "Jumlah Lilitan (N) Kali Fluks (Φ)."
  },
  {
    "en": "Apa Itu Induktansi Internal (Internal Inductance)?",
    "id": "Induktansi (Fluks Di Dalam Konduktor)."
  },
  {
    "en": "Apa Itu Induktansi Eksternal (External Inductance)?",
    "id": "Induktansi (Fluks Di Luar Konduktor)."
  },
  {
    "en": "Apa Itu Energi (Energy) Tersimpan (Medan Magnet)?",
    "id": "Energi (Tersimpan Dalam Ruang Medan B H)."
  },
  {
    "en": "Apa Itu Torsi (Torque) Magnetik?",
    "id": "Gaya Putar (Loop Arus Di Medan B)."
  },
  {
    "en": "Apa Itu Momen (Moment) Dipol Magnetik (m)?",
    "id": "Arus (I) Kali Luas Area (A) Loop."
  },
  {
    "en": "Apa Itu Resistivitas (Resistivity) (ρ)?",
    "id": "Hambatan Spesifik Intrinsik Material."
  },
  {
    "en": "Apa Satuan Resistivitas (Resistivity)?",
    "id": "Ohm-Meter (Ω·m)."
  },
  {
    "en": "Apa Itu Konduktivitas (Conductivity) (σ)?",
    "id": "Kemudahan Material Menghantarkan Arus."
  },
  {
    "en": "Apa Satuan Konduktivitas (Conductivity)?",
    "id": "Siemens Per Meter (S/m)."
  },
  {
    "en": "Hubungan Resistivitas (Resistivity) Dan Konduktivitas (Conductivity)?",
    "id": "Saling Berbanding Terbalik (σ = 1/ρ)."
  },
  {
    "en": "Apa Itu Koefisien (Coefficient) Suhu Resistansi (TCR)?",
    "id": "Perubahan Resistansi Per Derajat Suhu."
  },
  {
    "en": "Apa Itu TCR (Temperature Coefficient of Resistance) Positif (PTC)?",
    "id": "Hambatan Naik Saat Suhu Naik (Logam)."
  },
  {
    "en": "Apa Itu TCR (Temperature Coefficient of Resistance) Negatif (NTC)?",
    "id": "Hambatan Turun Saat Suhu Naik (Semikonduktor)."
  },
  {
    "en": "Apa Itu Superkonduktor (Superconductor)?",
    "id": "Material (Hambatan Listrik Nol Sempurna)."
  },
  {
    "en": "Apa Itu Pita (Band) Valensi?",
    "id": "Pita Energi (Level Energi Elektron Terikat)."
  },
  {
    "en": "Apa Itu Pita (Band) Konduksi?",
    "id": "Pita Energi (Level Energi Elektron Bebas)."
  },
  {
    "en": "Apa Itu Celah (Gap) Energi (Energy Gap)?",
    "id": "Energi (Pemisah Pita Valensi Konduksi)."
  },
  {
    "en": "Bagaimana Celah (Gap) Energi (Energy Gap) Konduktor?",
    "id": "Tumpang Tindih (Overlap) Atau Nol."
  },
  {
    "en": "Bagaimana Celah (Gap) Energi (Energy Gap) Isolator?",
    "id": "Sangat Lebar (Besar)."
  },
  {
    "en": "Bagaimana Celah (Gap) Energi (Energy Gap) Semikonduktor?",
    "id": "Sempit (Kecil) (Sekitar 1 eV)."
  },
  {
    "en": "Apa Itu Level (Level) Energi Fermi?",
    "id": "Level Energi (Probabilitas Elektron 50%)."
  },
  {
    "en": "Dimana Level (Level) Fermi Semikonduktor Intrinsik?",
    "id": "Di Tengah Celah (Gap) Energi."
  },
  {
    "en": "Dimana Level (Level) Fermi Semikonduktor Tipe-N?",
    "id": "Dekat Pita Konduksi."
  },
  {
    "en": "Dimana Level (Level) Fermi Semikonduktor Tipe-P?",
    "id": "Dekat Pita Valensi."
  },
  {
    "en": "Apa Itu Permitivitas (Permittivity) Relatif (εr)?",
    "id": "Konstanta Dielektrik (Rasio ε / ε0)."
  },
  {
    "en": "Apa Itu Permeabilitas (Permeability) Relatif (μr)?",
    "id": "Rasio Permeabilitas (μ / μ0)."
  },
  {
    "en": "Apa Itu Suseptibilitas (Susceptibility) Listrik (χe)?",
    "id": "Ukuran Polarisasi Material Dielektrik."
  },
  {
    "en": "Apa Itu Suseptibilitas (Susceptibility) Magnetik (χm)?",
    "id": "Ukuran Magnetisasi Material."
  },
  {
    "en": "Apa Itu Rugi (Loss) Dielektrik?",
    "id": "Disipasi Energi (Panas) Dielektrik (Medan AC)."
  },
  {
    "en": "Apa Itu Tangen (Tangent) Rugi (Loss) (Tan δ)?",
    "id": "Rasio (Kerugian Terhadap Energi Tersimpan)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Dielektrik?",
    "id": "Pergeseran Pusat Muatan (Medan Listrik)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Elektronik?",
    "id": "Pergeseran Awan Elektron Relatif Inti."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Ionik?",
    "id": "Pergeseran Ion Positif Negatif (Kristal)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Orientasi (Dipolar)?",
    "id": "Pelurusan Arah Momen Dipol Permanen."
  },
  {
    "en": "Apa Itu Relaksasi (Relaxation) Dielektrik?",
    "id": "Waktu Tunda Polarisasi (Mengikuti Medan)."
  },
  {
    "en": "Apa Itu Bahan (Material) Piezoelektrik?",
    "id": "Material (Hasilkan Listrik Jika Ditekan)."
  },
  {
    "en": "Apa Itu Efek (Effect) Piezoelektrik Balik?",
    "id": "Material (Berubah Bentuk Jika Diberi Listrik)."
  },
  {
    "en": "Apa Itu Bahan (Material) Piroelektrik?",
    "id": "Material (Hasilkan Listrik Jika Suhu Berubah)."
  },
  {
    "en": "Apa Itu Bahan (Material) Ferroelektrik?",
    "id": "Material (Polarisasi Spontan Bisa Dibalik)."
  },
  {
    "en": "Apa Itu Arus (Current) Perpindahan (Displacement) (Id)?",
    "id": "Arus (Akibat Perubahan Kerapatan Fluks D)."
  },
  {
    "en": "Siapa Yang Mengusulkan Arus (Current) Perpindahan?",
    "id": "James Clerk Maxwell."
  },
  {
    "en": "Apa Hukum Ampere-Maxwell?",
    "id": "Curl H = J + Turunan D Terhadap T."
  },
  {
    "en": "Apa Itu Persamaan (Equation) Kontinuitas?",
    "id": "Divergensi J = - Turunan Rho Terhadap T."
  },
  {
    "en": "Apa Arti Persamaan (Equation) Kontinuitas?",
    "id": "Hukum Konservasi (Kekekalan) Muatan."
  },
  {
    "en": "Apa Itu Persamaan (Equation) Gelombang (Wave)?",
    "id": "Persamaan (Turunan Persamaan Maxwell)."
  },
  {
    "en": "Apa Itu Kecepatan (Velocity) Rambat (Gelombang EM)?",
    "id": "Satu Dibagi Akar (Permeabilitas Kali Permitivitas)."
  },
  {
    "en": "Berapa Kecepatan (Velocity) Cahaya (Ruang Hampa)?",
    "id": "Sekitar 3 x 10 Pangkat 8 m/s."
  },
  {
    "en": "Apa Itu Impedansi (Impedance) Intrinsik (η)?",
    "id": "Rasio Amplitudo Medan E Terhadap H."
  },
  {
    "en": "Impedansi (Impedance) Intrinsik Ruang Hampa (η0)?",
    "id": "Sekitar 377 Ohm (120π)."
  },
  {
    "en": "Apa Itu Gelombang (Wave) Bidang (Plane)?",
    "id": "Gelombang (Medan E H Konstan Bidang Fasa)."
  },
  {
    "en": "Apa Itu Gelombang (Wave) Seragam (Uniform)?",
    "id": "Gelombang (Amplitudo Konstan Bidang Fasa)."
  },
  {
    "en": "Apa Itu Gelombang (Wave) TEM (Transverse Electro-Magnetic)?",
    "id": "Medan E H Tegak Lurus Arah Rambat."
  },
  {
    "en": "Apa Itu Vektor (Vector) Poynting (S)?",
    "id": "Vektor Kerapatan Daya Sesaat (S = E x H)."
  },
  {
    "en": "Apa Satuan Vektor (Vector) Poynting?",
    "id": "Watt Per Meter Persegi (W/m²)."
  },
  {
    "en": "Apa Itu Daya (Power) Rata-Rata (Gelombang EM)?",
    "id": "Integral Rata-Rata Vektor Poynting."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Linear?",
    "id": "Osilasi Medan E (Satu Garis Lurus)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Sirkular?",
    "id": "Osilasi Medan E (Berputar Magnitudo Konstan)."
  },
  {
    "en": "Apa Itu Polarisasi (Polarization) Eliptikal?",
    "id": "Osilasi Medan E (Berputar Magnitudo Variabel)."
  },
  {
    "en": "Apa Itu Refleksi (Reflection) (Gelombang)?",
    "id": "Pantulan Gelombang (Batas Dua Medium)."
  },
  {
    "en": "Apa Itu Transmisi (Transmission) (Gelombang)?",
    "id": "Penerusan Gelombang (Lewati Batas Medium)."
  },
  {
    "en": "Apa Itu Koefisien (Coefficient) Refleksi (Pantulan) (Γ)?",
    "id": "Rasio Amplitudo (Gelombang Pantul Datang)."
  },
  {
    "en": "Apa Itu Koefisien (Coefficient) Transmisi (T)?",
    "id": "Rasio Amplitudo (Gelombang Transmisi Datang)."
  },
  {
    "en": "Apa Itu Hukum Snell (Snell's Law)?",
    "id": "Hubungan Sudut (Datang Bias) (Refraksi)."
  },
  {
    "en": "Apa Itu Sudut (Angle) Brewster?",
    "id": "Sudut (Refleksi Nol) (Polarisasi Paralel)."
  },
  {
    "en": "Apa Itu Refleksi (Reflection) Total Internal?",
    "id": "Pantulan Sempurna (Medium Rapat Optik)."
  },
  {
    "en": "Apa Itu Saluran (Line) Transmisi?",
    "id": "Struktur (Pemandu Gelombang EM)."
  },
  {
    "en": "Contoh Saluran (Line) Transmisi?",
    "id": "Kabel Koaksial, Mikrostrip, Kawat Paralel."
  },
  {
    "en": "Apa Parameter (Parameter) Saluran Transmisi (RLCG)?",
    "id": "R (Resistansi), L (Induktansi), C (Kapasitansi), G (Konduktansi)."
  },
  {
    "en": "Apa Itu Impedansi (Impedance) Karakteristik (Z0)?",
    "id": "Impedansi Input (Saluran Panjang Tak Hingga)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) Propagasi (γ)?",
    "id": "Ukuran (Atenuasi Fasa) (γ = α + jβ)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) Atenuasi (α)?",
    "id": "Pelemahan Sinyal (Per Satuan Panjang)."
  },
  {
    "en": "Apa Itu Konstanta (Constant) Fasa (β)?",
    "id": "Pergeseran Fasa (Per Satuan Panjang)."
  },
  {
    "en": "Apa Itu Saluran (Line) Tanpa Rugi (Lossless)?",
    "id": "Saluran Ideal (R = 0, G = 0)."
  },
  {
    "en": "Apa Itu Saluran (Line) Tanpa Distorsi (Distortionless)?",
    "id": "Kondisi (R/L = G/C)."
  },
  {
    "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?",
    "id": "Cabang AI (Artificial Intelligence) (Sistem Belajar Dari Data)."
  },
  {
    "en": "Apa Itu Pembelajaran Terbimbing (Supervised)?",
    "id": "Belajar (Data Input Memiliki Label Output)."
  },
  {
    "en": "Apa Itu Pembelajaran Tak Terbimbing (Unsupervised)?",
    "id": "Belajar (Data Input Tanpa Label Output)."
  },
  {
    "en": "Apa Itu Pembelajaran Penguatan (Reinforcement)?",
    "id": "Belajar (Berdasarkan Umpan Balik (Reward/Punishment))."
  },
  {
    "en": "Apa Itu Masalah Klasifikasi (Classification)?",
    "id": "Masalah (Memprediksi Kategori Diskret)."
  },
  {
    "en": "Apa Itu Masalah Regresi (Regression)?",
    "id": "Masalah (Memprediksi Nilai Kontinu)."
  },
  {
    "en": "Apa Itu Masalah Pengelompokan (Clustering)?",
    "id": "Masalah (Menemukan Pola Grup Data)."
  },
  {
    "en": "Apa Itu Data Latih (Training Data)?",
    "id": "Data (Digunakan Melatih Model)."
  },
  {
    "en": "Apa Itu Data Uji (Test Data)?",
    "id": "Data (Digunakan Evaluasi Kinerja Model)."
  },
  {
    "en": "Apa Itu Overfitting (Overfitting)?",
    "id": "Model (Terlalu Hafal Data Latih, Buruk Di Data Uji)."
  },
  {
    "en": "Apa Itu Underfitting (Underfitting)?",
    "id": "Model (Terlalu Sederhana, Gagal Menangkap Pola)."
  },
  {
    "en": "Apa Itu Fitur (Feature) (ML)?",
    "id": "Variabel Input Yang Digunakan Model."
  },
  {
    "en": "Apa Itu Rekayasa Fitur (Feature Engineering)?",
    "id": "Proses (Seleksi Transformasi Fitur)."
  },
  {
    "en": "Apa Itu Fungsi Kerugian (Loss Function)?",
    "id": "Mengukur Error (Kesalahan) Prediksi Model."
  },
  {
    "en": "Apa Itu Regresi (Regression) Linear?",
    "id": "Model Regresi (Mencari Garis Lurus Terbaik)."
  },
  {
    "en": "Apa Itu Regresi (Regression) Logistik?",
    "id": "Model Klasifikasi (Probabilitas Biner)."
  },
  {
    "en": "Apa Itu Pohon Keputusan (Decision Tree)?",
    "id": "Model (Struktur Mirip Pohon IF-THEN)."
  },
  {
    "en": "Apa Itu Support Vector Machine (SVM)?",
    "id": "Model (Mencari Hyperplane Pemisah Terbaik)."
  },
  {
    "en": "Apa Itu Algoritma K-Means (K-Means Clustering)?",
    "id": "Algoritma Clustering (Partisi Ke K Cluster)."
  },
  {
    "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?",
    "id": "Sub-Bidang ML (Jaringan Saraf Tiruan Dalam)."
  },
  {
    "en": "Apa Itu Propagasi Balik (Backpropagation)?",
    "id": "Algoritma (Menghitung Gradien (Loss) (ANN))."
  },
  {
    "en": "Apa Itu Penurunan Gradien (Gradient Descent)?",
    "id": "Algoritma Optimasi (Minimalkan Loss Function)."
  },
  {
    "en": "Apa Itu Laju Pembelajaran (Learning Rate)?",
    "id": "Ukuran Langkah (Penurunan Gradien)."
  },
  {
    "en": "Apa Itu Epok (Epoch) (ML)?",
    "id": "Satu Siklus Penuh (Melewati Data Latih)."
  },
  {
    "en": "Apa Itu Ukuran Batch (Batch Size)?",
    "id": "Jumlah Sampel Data (Satu Iterasi Latih)."
  },
  {
    "en": "Apa Kepanjangan CNN (Convolutional Neural Network)?",
    "id": "Convolutional Neural Network."
  },
  {
    "en": "Kapan CNN (Convolutional Neural Network) Digunakan?",
    "id": "Pemrosesan Citra (Computer Vision)."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Konvolusi (CNN)?",
    "id": "Lapisan (Ekstraksi Fitur Citra (Filter))."
  },
  {
    "en": "Apa Itu Lapisan (Layer) Pooling (CNN)?",
    "id": "Lapisan (Reduksi Dimensi Spasial)."
  },
  {
    "en": "Apa Kepanjangan RNN (Recurrent Neural Network)?",
    "id": "Recurrent Neural Network."
  },
  {
    "en": "Kapan RNN (Recurrent Neural Network) Digunakan?",
    "id": "Data Sekuensial (Teks, Waktu)."
  },
  {
    "en": "Apa Kepanjangan LSTM (Long Short-Term Memory)?",
    "id": "Long Short-Term Memory (Varian RNN (Recurrent Neural Network))."
  },
  {
    "en": "Apa Masalah Gradien Lenyap (Vanishing Gradient)?",
    "id": "Gradien (Sangat Kecil) (Sulit Melatih RNN (Recurrent Neural Network))."
  },
  {
    "en": "Apa Itu Filter FIR (Finite Impulse Response)?",
    "id": "Filter Digital (Respon Impuls Berhingga)."
  },
  {
    "en": "Apa Itu Filter IIR (Infinite Impulse Response)?",
    "id": "Filter Digital (Respon Impuls Tak Berhingga)."
  },
  {
    "en": "Kelebihan Utama Filter FIR (Finite Impulse Response)?",
    "id": "Selalu Stabil, Fase Linear Sempurna."
  },
  {
    "en": "Kekurangan Filter FIR (Finite Impulse Response)?",
    "id": "Orde (Komputasi) Tinggi."
  },
  {
    "en": "Kelebihan Utama Filter IIR (Infinite Impulse Response)?",
    "id": "Orde (Komputasi) Rendah (Sangat Efisien)."
  },
  {
    "en": "Kekurangan Filter IIR (Infinite Impulse Response)?",
    "id": "Fase Non-Linear, Potensi Tidak Stabil."
  },
  {
    "en": "Metode Desain Filter FIR (Finite Impulse Response)?",
    "id": "Metode Jendela (Windowing Method)."
  },
  {
    "en": "Metode Desain Filter IIR (Infinite Impulse Response)?",
    "id": "Transformasi Bilinear, Invariansi Impuls."
  },
  {
    "en": "Apa Itu Transformasi Bilinear (Bilinear Transform)?",
    "id": "Mengubah Filter Analog (Domain-S) Ke Digital (Domain-Z)."
  },
  {
    "en": "Apa Itu Warping (Pembelokan) Frekuensi?",
    "id": "Distorsi Frekuensi (Efek Transformasi Bilinear)."
  },
  {
    "en": "Apa Itu Filter Butterworth (Butterworth)?",
    "id": "Filter (Respon Passband Paling Datar)."
  },
  {
    "en": "Apa Itu Filter Chebyshev (Chebyshev) (Tipe I)?",
    "id": "Filter (Roll-Off Cepat, Riak Di Passband)."
  },
  {
    "en": "Apa Itu Filter Chebyshev (Chebyshev) (Tipe II)?",
    "id": "Filter (Roll-Off Cepat, Riak Di Stopband)."
  },
  {
    "en": "Apa Itu Filter Elliptic (Cauer)?",
    "id": "Filter (Roll-Off Paling Tajam, Riak Di Keduanya)."
  },
  {
    "en": "Apa Itu Filter Bessel (Bessel)?",
    "id": "Filter (Tunda Grup (Fase) Paling Linear)."
  },
  {
    "en": "Apa Itu Tunda Grup (Group Delay)?",
    "id": "Ukuran Distorsi Fasa Filter."
  },
  {
    "en": "Apa Itu Pola Desain (Design Pattern)?",
    "id": "Solusi Umum (Masalah Desain Perangkat Lunak)."
  },
  {
    "en": "Tiga Kategori Pola Desain (GoF)?",
    "id": "Creational (Penciptaan), Structural (Struktural), Behavioral (Perilaku)."
  },
  {
    "en": "Apa Itu Pola (Pattern) Singleton?",
    "id": "Memastikan Kelas (Hanya Punya Satu Instans)."
  },
  {
    "en": "Apa Itu Pola (Pattern) Factory (Pabrik)?",
    "id": "Membuat Objek (Tanpa Ekspos Logika Penciptaan)."
  },
  {
    "en": "Apa Itu Pola (Pattern) Observer (Pengamat)?",
    "id": "Objek (Berlangganan Notifikasi Perubahan)."
  },
  {
    "en": "Apa Itu Pola (Pattern) Strategy (Strategi)?",
    "id": "Enkapsulasi Algoritma (Bisa Ditukar)."
  },
  {
    "en": "Apa Itu Pola (Pattern) Decorator (Dekorator)?",
    "id": "Menambah Fungsionalitas Objek (Dinamis)."
  },
  {
    "en": "Apa Itu Pola (Pattern) Adapter (Adaptor)?",
    "id": "Menghubungkan Antarmuka (Interface) Tidak Kompatibel."
  },
  {
    "en": "Apa Itu Pola (Pattern) Facade (Fasad)?",
    "id": "Antarmuka Sederhana (Sistem Kompleks)."
  },
  {
    "en": "Apa Itu Pola (Pattern) MVC (Model-View-Controller)?",
    "id": "Pola Arsitektur (Pemisahan Logika UI)."
  },
  {
    "en": "Apa Itu Model (Model) (MVC)?",
    "id": "Logika Data Bisnis (Inti)."
  },
  {
    "en": "Apa Itu View (Tampilan) (MVC)?",
    "id": "Representasi Visual (Antarmuka Pengguna)."
  },
  {
    "en": "Apa Itu Controller (Pengontrol) (MVC)?",
    "id": "Menerima Input (Jembatan Model View)."
  },
  {
    "en": "Apa Itu Bahan (Material) Anoda Baterai Li-Ion?",
    "id": "Grafit (Karbon)."
  },
  {
    "en": "Apa Itu Bahan (Material) Katoda Baterai Li-Ion?",
    "id": "Oksida Logam Lithium (LCO, NMC, LFP)."
  },
  {
    "en": "Apa Itu Bahan (Material) Elektrolit Baterai Li-Ion?",
    "id": "Garam Lithium (Pelarut Organik)."
  },
  {
    "en": "Proses Saat Pengisian (Charge) Baterai Li-Ion?",
    "id": "Ion Lithium (Pindah Dari Katoda Ke Anoda)."
  },
  {
    "en": "Proses Saat Pengosongan (Discharge) Baterai Li-Ion?",
    "id": "Ion Lithium (Pindah Dari Anoda Ke Katoda)."
  },
  {
    "en": "Apa Itu Interkalasi (Intercalation)?",
    "id": "Penyisipan Ion Lithium (Ke Struktur Kisi)."
  },
  {
    "en": "Apa Kepanjangan SEI (Solid Electrolyte Interphase)?",
    "id": "Solid Electrolyte Interphase (Lapisan Antarmuka)."
  },
  {
    "en": "Apa Fungsi Lapisan SEI (Solid Electrolyte Interphase)?",
    "id": "Melindungi Anoda (Grafit) Dari Reaksi Elektrolit."
  },
  {
    "en": "Penyebab Utama Pelarian Termal (Thermal Runaway)?",
    "id": "Hubung Singkat Internal (Pertumbuhan Dendrit)."
  },
  {
    "en": "Apa Itu Dendrit (Dendrite) Lithium?",
    "id": "Struktur Mirip Jarum Logam Lithium."
  },
  {
    "en": "Apa Kepanjangan BMS (Battery Management System)?",
    "id": "Battery Management System."
  },
  {
    "en": "Fungsi Utama BMS (Battery Management System)?",
    "id": "Proteksi (Overcharge, Overdischarge), Penyeimbangan."
  },
  {
    "en": "Apa Itu Penyeimbangan (Balancing) Sel Pasif?",
    "id": "Membuang Energi Berlebih (Via Resistor)."
  },
  {
    "en": "Apa Itu Penyeimbangan (Balancing) Sel Aktif?",
    "id": "Memindahkan Energi (Sel Kuat Ke Lemah)."
  },
  {
    "en": "Apa Itu Baterai Aliran (Flow Battery)?",
    "id": "Baterai (Energi Disimpan Elektrolit Cair Eksternal)."
  },
  {
    "en": "Apa Itu Anolit (Anolyte) (Baterai Aliran)?",
    "id": "Elektrolit Cair Sisi Anoda."
  },
  {
    "en": "Apa Itu Katolit (Catholyte) (Baterai Aliran)?",
    "id": "Elektrolit Cair Sisi Katoda."
  },
  {
    "en": "Contoh Baterai Aliran (Flow Battery)?",
    "id": "Vanadium Redox Flow Battery (VRFB)."
  },
  {
    "en": "Keuntungan Baterai Aliran (Flow Battery)?",
    "id": "Skalabilitas (Daya Energi Terpisah)."
  },
  {
    "en": "Apa Itu Baterai Logam-Udara (Metal-Air)?",
    "id": "Baterai (Katoda Menggunakan Oksigen Udara)."
  },
  {
    "en": "Contoh Baterai Logam-Udara (Metal-Air)?",
    "id": "Baterai Seng-Udara (Zinc-Air)."
  },
  {
    "en": "Apa Itu Kepadatan Energi (Energy Density)?",
    "id": "Energi Tersimpan Per Satuan Volume/Berat."
  },
  {
    "en": "Apa Itu Kepadatan Daya (Power Density)?",
    "id": "Kemampuan Melepas Energi Cepat."
  },
  {
    "en": "Apa Itu Efisiensi (Efficiency) Coulombik?",
    "id": "Rasio Muatan (Charge) Keluar Masuk."
  },
  {
    "en": "Apa Itu C-Rate (Laju-C)?",
    "id": "Ukuran Laju Pengisian/Pengosongan Baterai."
  },
  {
    "en": "Apa Arti 1C (Satu C-Rate)?",
    "id": "Mengisi Penuh Baterai Dalam 1 Jam."
  },
  {
    "en": "Apa Arti 5C (Lima C-Rate)?",
    "id": "Mengisi Penuh Baterai Dalam 12 Menit."
  },
  {
    "en": "Apa Arti 0.5C (Setengah C-Rate)?",
    "id": "Mengisi Penuh Baterai Dalam 2 Jam."
  },
  {
    "en": "Apa Itu Fotolitografi (Photolithography)?",
    "id": "Proses Pencetakan Pola Sirkuit (IC)."
  },
  {
    "en": "Apa Itu Photoresist (Resis Foto)?",
    "id": "Material Polimer Peka Cahaya (UV)."
  },
  {
    "en": "Apa Itu Resis (Resist) Positif?",
    "id": "Area Terekspos (UV) Menjadi Larut."
  },
  {
    "en": "Apa Itu Resis (Resist) Negatif?",
    "id": "Area Terekspos (UV) Menjadi Keras."
  },
  {
    "en": "Apa Itu Masker (Mask) Fotolitografi?",
    "id": "Pola Kromium (Cr) Pada Kaca Kuarsa."
  },
  {
    "en": "Apa Itu Reticle (Reticle)?",
    "id": "Nama Lain Masker (Mask) (Pembesaran)."
  },
  {
    "en": "Apa Itu Stepper (Stepper) Litografi?",
    "id": "Mesin Proyeksi Pola Masker (Step-Repeat)."
  },
  {
    "en": "Apa Itu Etsa (Etching)?",
    "id": "Proses Penghilangan Material (Non-Resis)."
  },
  {
    "en": "Apa Itu Etsa (Etching) Basah (Wet)?",
    "id": "Menggunakan Bahan Kimia Cair (Isotropik)."
  },
  {
    "en": "Apa Itu Etsa (Etching) Kering (Dry)?",
    "id": "Menggunakan Gas Plasma (Anisotropik)."
  },
  {
    "en": "Apa Itu Etsa (Etching) Isotropik?",
    "id": "Mengikis Ke Segala Arah (Undercut)."
  },
  {
    "en": "Apa Itu Etsa (Etching) Anisotropik?",
    "id": "Mengikis Satu Arah (Vertikal)."
  },
  {
    "en": "Apa Kepanjangan RIE (Reactive Ion Etching)?",
    "id": "Reactive Ion Etching (Etsa Kering)."
  },
  {
    "en": "Apa Itu Deposisi (Deposition)?",
    "id": "Proses Penambahan Lapisan Material."
  },
  {
    "en": "Apa Kepanjangan CVD (Chemical Vapor Deposition)?",
    "id": "Chemical Vapor Deposition."
  },
  {
    "en": "Apa Kepanjangan PVD (Physical Vapor Deposition)?",
    "id": "Physical Vapor Deposition."
  },
  {
    "en": "Apa Itu Sputtering (PVD)?",
    "id": "Deposisi (Bombardir Target Dengan Ion)."
  },
  {
    "en": "Apa Itu Oksidasi (Oxidation) Termal?",
    "id": "Menumbuhkan Silikon Dioksida (SiO2)."
  },
  {
    "en": "Apa Fungsi Oksida (Oxide) Gerbang (Gate)?",
    "id": "Isolator (Gate) Transistor MOSFET."
  },
  {
    "en": "Apa Itu Implantasi (Implantation) Ion?",
    "id": "Proses Doping (Menembakkan Ion Dopant)."
  },
  {
    "en": "Apa Itu Kriteria Kestabilan (Stability) Nyquist?",
    "id": "Kestabilan (Plot Nyquist) Titik (-1, 0)."
  },
  {
    "en": "Apa Itu Plot (Plot) Nyquist?",
    "id": "Plot (Fungsi Alih Loop) Domain Frekuensi Polar."
  },
  {
    "en": "Apa Itu Prinsip (Principle) Argumen Cauchy?",
    "id": "Dasar Matematika Kriteria Nyquist."
  },
  {
    "en": "Apa Itu Jumlah (N) (Nyquist)?",
    "id": "Jumlah (Kutub RHP) Loop Terbuka."
  },
  {
    "en": "Apa Itu Jumlah (Z) (Nyquist)?",
    "id": "Jumlah (Kutub RHP) Loop Tertutup."
  },
  {
    "en": "Apa Itu Jumlah (P) (Nyquist)?",
    "id": "Jumlah (Putaran Searah Jarum Jam) Titik -1."
  },
  {
    "en": "Apa Rumus (Formula) Kestabilan Nyquist?",
    "id": "Z = N + P."
  },
  {
    "en": "Kapan Sistem (Nyquist) Stabil?",
    "id": "Z = 0 (Tidak Ada Kutub RHP)."
  },
  {
    "en": "Apa Itu Gain (Gain) Margin (GM)?",
    "id": "Faktor (Gain) Untuk Mencapai Ketidakstabilan."
  },
  {
    "en": "Bagaimana (GM) (Gain Margin) Diukur (Plot Nyquist)?",
    "id": "Invers (Magnitudo) Saat Fasa -180 Derajat."
  },
  {
    "en": "Apa Itu Fasa (Phase) Margin (PM)?",
    "id": "Sudut (Fasa) Tambahan (Mencapai Ketidakstabilan)."
  },
  {
    "en": "Bagaimana (PM) (Phase Margin) Diukur (Plot Nyquist)?",
    "id": "Sudut (Saat Magnitudo Memotong Lingkaran Satuan)."
  },
  {
    "en": "Apa Itu Plot (Plot) Nichols?",
    "id": "Plot (Log-Magnitudo Vs Fasa) (Kontrol)."
  },
  {
    "en": "Apa Itu Kontrol (Control) Kokoh (Robust)?",
    "id": "Kontrol (Tahan Ketidakpastian Model)."
  },
  {
    "en": "Apa Itu Keterkendalian (Controllability)?",
    "id": "Kemampuan (Input Mengontrol State Internal)."
  },
  {
    "en": "Apa Itu Keteramatan (Observability)?",
    "id": "Kemampuan (Output Mengukur State Internal)."
  },
  {
    "en": "Apa Itu Matriks (Matrix) Keterkendalian (Controllability)?",
    "id": "Matriks (Uji Keterkendalian) (Kalman)."
  },
  {
    "en": "Apa Itu Matriks (Matrix) Keteramatan (Observability)?",
    "id": "Matriks (Uji Keteramatan) (Kalman)."
  },
  {
    "en": "Kapan Sistem (Sistem) Terkendali (Controllable)?",
    "id": "Rank (Peringkat) Matriks Keterkendalian Penuh."
  },
  {
    "en": "Kapan Sistem (Sistem) Teramati (Observable)?",
    "id": "Rank (Peringkat) Matriks Keteramatan Penuh."
  },
  {
    "en": "Apa Itu Penempatan (Placement) Kutub (Pole)?",
    "id": "Desain (Kontroler Umpan Balik State)."
  },
  {
    "en": "Apa Itu Estimator (Estimator) Keadaan (State)?",
    "id": "Pengamat (Observer) (Estimasi State Internal)."
  },
  {
    "en": "Apa Itu Pengamat (Observer) Luenberger?",
    "id": "Estimator (State) Berbasis Model."
  },
  {
    "en": "Apa Itu Prinsip (Principle) Separasi (Separation)?",
    "id": "Desain (Kontroler Estimator) Terpisah."
  },
  {
    "en": "Apa Kepanjangan LQR (Linear Quadratic Regulator)?",
    "id": "Linear Quadratic Regulator (Kontrol Optimal)."
  },
  {
    "en": "Apa Itu Struktur (Structure) Filter Digital?",
    "id": "Implementasi (Diagram Blok) Persamaan Beda."
  },
  {
    "en": "Apa Itu Struktur (Structure) Direct Form I?",
    "id": "Implementasi (Langsung) (Dua Rantai Tundaan)."
  },
  {
    "en": "Apa Itu Struktur (Structure) Direct Form II?",
    "id": "Implementasi (Tundaan Terpusat) (Hemat Memori)."
  },
  {
    "en": "Apa Itu Struktur (Structure) Transposed Direct Form II?",
    "id": "Implementasi (Balikan) (Struktur Direct Form II)."
  },
  {
    "en": "Apa Itu Struktur (Structure) Kaskade (Cascade)?",
    "id": "Implementasi (Filter Orde Dua Seri)."
  },
  {
    "en": "Apa Itu Struktur (Structure) Paralel (Parallel)?",
    "id": "Implementasi (Filter Orde Dua Paralel)."
  },
  {
    "en": "Apa Itu Struktur (Structure) Lattice (Kisi)?",
    "id": "Implementasi (Kokoh Terhadap Kuantisasi)."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization) Koefisien?",
    "id": "Error (Pembulatan) Nilai Koefisien Filter."
  },
  {
    "en": "Apa Itu Efek (Effect) Kuantisasi (Quantization)?",
    "id": "Error (Akibat Representasi Bit Terbatas)."
  },
  {
    "en": "Apa Itu Limpahan (Overflow) Aritmatika?",
    "id": "Hasil (Perhitungan) Melebihi Batas Bit."
  },
  {
    "en": "Apa Itu Siklus (Cycle) Batas (Limit)?",
    "id": "Osilasi (Frekuensi Rendah) (Filter IIR)."
  },
  {
    "en": "Apa Itu Penyekalaan (Scaling) (Filter Digital)?",
    "id": "Mencegah (Limpahan) (Overflow) (Aritmatika)."
  },
  {
    "en": "Apa Itu Filter (Filter) Multirate?",
    "id": "Filter (Laju Sampling Berubah)."
  },
  {
    "en": "Apa Itu Desimasi (Decimation)?",
    "id": "Proses (Menurunkan Laju Sampling)."
  },
  {
    "en": "Apa Itu Interpolasi (Interpolation) (DSP)?",
    "id": "Proses (Menaikkan Laju Sampling)."
  },
  {
    "en": "Apa Itu Filter (Filter) Anti-Aliasing?",
    "id": "Filter LPF (Low Pass Filter) (Sebelum Desimasi)."
  },
  {
    "en": "Apa Itu Filter (Filter) Anti-Imaging?",
    "id": "Filter LPF (Low Pass Filter) (Setelah Interpolasi)."
  },
  {
    "en": "Apa Itu Bank (Bank) Filter?",
    "id": "Kumpulan (Filter Lolos Pita) (Analisis Spektrum)."
  },
  {
    "en": "Apa Itu Bank (Bank) Filter Quadrature Mirror (QMF)?",
    "id": "Bank (Filter) (Rekonstruksi Sempurna)."
  },
  {
    "en": "Apa Itu Transformasi (Transform) Wavelet?",
    "id": "Analisis (Sinyal) (Domain Waktu-Frekuensi)."
  },
  {
    "en": "Apa Beda (Fourier Vs Wavelet)?",
    "id": "Fourier (Frekuensi), Wavelet (Waktu Frekuensi)."
  },
  {
    "en": "Apa Itu Mother (Induk) Wavelet?",
    "id": "Fungsi (Gelombang) Dasar (Transformasi Wavelet)."
  },
  {
    "en": "Apa Kepanjangan DWT (Discrete Wavelet Transform)?",
    "id": "Discrete Wavelet Transform."
  },
  {
    "en": "Apa Kepanjangan CWT (Continuous Wavelet Transform)?",
    "id": "Continuous Wavelet Transform."
  },
  {
    "en": "Apa Itu Wavelet (Wavelet) Haar?",
    "id": "Mother (Induk) Wavelet (Paling Sederhana) (Kotak)."
  },
  {
    "en": "Apa Itu Analisis (Analysis) Multiresolusi?",
    "id": "Analisis (Sinyal) (Berbagai Skala Detail)."
  },
  {
    "en": "Apa Itu Koefisien (Coefficient) Aproksimasi (DWT)?",
    "id": "Komponen (Frekuensi Rendah) (Skala Besar)."
  },
  {
    "en": "Apa Itu Koefisien (Coefficient) Detail (DWT)?",
    "id": "Komponen (Frekuensi Tinggi) (Detail Kecil)."
  },
  {
    "en": "Apa Itu Transformasi (Transform) Hilbert?",
    "id": "Menggeser (Fasa Sinyal) 90 Derajat."
  },
  {
    "en": "Apa Itu Sinyal (Signal) Analitik?",
    "id": "Sinyal (Kompleks) (Tanpa Frekuensi Negatif)."
  },
  {
    "en": "Apa Itu Relai (Relay) Jarak (Distance) Zona 1?",
    "id": "Proteksi (80-90% Saluran) (Seketika)."
  },
  {
    "en": "Apa Itu Relai (Relay) Jarak (Distance) Zona 2?",
    "id": "Proteksi (Cadangan Saluran Depan) (Tunda Waktu)."
  },
  {
    "en": "Apa Itu Relai (Relay) Jarak (Distance) Zona 3?",
    "id": "Proteksi (Cadangan Saluran Berikutnya) (Tunda Waktu Lama)."
  },
  {
    "en": "Apa Itu Power (Daya) Swing (Ayunan)?",
    "id": "Osilasi (Daya) Antar Generator."
  },
  {
    "en": "Apa Itu Blok (Block) Ayunan Daya (PSB)?",
    "id": "Fitur (Relai) (Mencegah Trip Saat Ayunan)."
  },
  {
    "en": "Apa Itu Arus (Current) Inrush (Trafo)?",
    "id": "Lonjakan (Arus Magnetisasi) Saat Start."
  },
  {
    "en": "Apa Itu Penahan (Restraint) Harmonisa (Relai 87)?",
    "id": "Mencegah (Trip Palsu) Akibat Arus Inrush."
  },
  {
    "en": "Apa Itu Proteksi (Protection) Busbar (Busbar)?",
    "id": "Proteksi (Zona Kritis) Gardu Induk."
  },
  {
    "en": "Apa Itu Skema (Scheme) Pailang Kawat (Pilot Wire)?",
    "id": "Proteksi (Diferensial) Saluran Transmisi."
  },
  {
    "en": "Apa Itu Skema (Scheme) POTT (Permissive Overreach Transfer Trip)?",
    "id": "Skema (Teleproteksi) Relai Jarak."
  },
  {
    "en": "Apa Itu Skema (Scheme) DCB (Directional Comparison Blocking)?",
    "id": "Skema (Teleproteksi) (Blokir Sinyal)."
  },
  {
    "en": "Apa Itu Auto-Recloser (Penutup Balik Otomatis)?",
    "id": "Memulihkan (Saluran) (Gangguan Temporer)."
  },
  {
    "en": "Apa Itu Kegagalan (Failure) Pemutus (Breaker Failure)?",
    "id": "Proteksi (Cadangan Lokal) (CB (Circuit Breaker) Gagal Buka)."
  },
  {
    "en": "Apa Itu Sinkro-Cek (Synchro-Check) Relai?",
    "id": "Memastikan (Sinkron) Sebelum Menutup CB (Circuit Breaker)."
  },
  {
    "en": "Apa Itu Relai (Relay) Impedansi Tanah (Ground Impedance)?",
    "id": "Relai (Jarak) (Deteksi Gangguan Tanah)."
  },
  {
    "en": "Apa Itu Relai (Relay) Arah (Directional) Tanah?",
    "id": "Relai (Deteksi Arah Gangguan Tanah)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) Generator (Lengkap)?",
    "id": "Melindungi (Stator, Rotor, Eksitasi)."
  },
  {
    "en": "Apa Itu Relai (Relay) Tekanan (Pressure) Tiba-Tiba?",
    "id": "Proteksi (Mekanis Trafo) (Lonjakan Tekanan)."
  },
  {
    "en": "Apa Itu Proteksi (Protection) Saluran (Line) Transmisi?",
    "id": "Menggunakan (Relai Jarak) (Diferensial)."
  },
  {
    "en": "Apa Itu Relai (Relay) Diferensial (Differential) Persentase?",
    "id": "Relai (Diferensial) (Karakteristik Kemiringan)."
  },
  {
    "en": "Apa Itu Relai (Relay) Impedansi (Impedance)?",
    "id": "Relai (Jarak) (Karakteristik Lingkaran)."
  },
  {
    "en": "Apa Itu Relai (Relay) Reaktansi (Reactance)?",
    "id": "Relai (Jarak) (Karakteristik Garis Lurus)."
  },
  {
    "en": "Apa Itu Relai (Relay) Mho (Mho)?",
    "id": "Relai (Jarak) (Karakteristik Lingkaran Lewat Origin)."
  },
  {
    "en": "Apa Itu Batas (Boundary) Arc Flash?",
    "id": "Jarak Aman Insiden Energi Arc Flash."
  },
  {
    "en": "Apa Itu Batas (Boundary) Kilat Terbatas (Limited Approach)?",
    "id": "Jarak (Hanya Petugas Berwenang Boleh Masuk)."
  },
  {
    "en": "Apa Itu Batas (Boundary) Kilat Terlarang (Restricted Approach)?",
    "id": "Jarak (Risiko Kejut Listrik Tinggi, Wajib APD)."
  },
  {
    "en": "Apa Itu Batas (Boundary) Kilat Dilarang (Prohibited Approach)?",
    "id": "Jarak (Sama Seperti Kontak Langsung)."
  },
  {
    "en": "Apa Itu Energi (Energy) Insiden (Incident)?",
    "id": "Energi Termal (Panas) Dihasilkan Arc Flash."
  },
  {
    "en": "Apa Satuan Energi (Energy) Insiden?",
    "id": "Kalori Per Sentimeter Persegi (cal/cm²)."
  },
  {
    "en": "Apa Kategori (Category) PPE (Personal Protective Equipment) Arc Flash 0?",
    "id": "Risiko Sangat Rendah (Pakaian Katun Biasa)."
  },
  {
    "en": "Apa Kategori (Category) PPE (Personal Protective Equipment) Arc Flash 1?",
    "id": "Risiko Rendah (Minimal 4 cal/cm²)."
  },
  {
    "en": "Apa Kategori (Category) PPE (Personal Protective Equipment) Arc Flash 2?",
    "id": "Risiko Sedang (Minimal 8 cal/cm²)."
  },
  {
    "en": "Apa Kategori (Category) PPE (Personal Protective Equipment) Arc Flash 3?",
    "id": "Risiko Tinggi (Minimal 25 cal/cm²)."
  },
  {
    "en": "Apa Kategori (Category) PPE (Personal Protective Equipment) Arc Flash 4?",
    "id": "Risiko Sangat Tinggi (Minimal 40 cal/cm²)."
  },
  {
    "en": "Apa Itu Studi (Study) Arc Flash?",
    "id": "Analisis Perhitungan Bahaya Arc Flash."
  },
  {
    "en": "Apa Itu Sel (Cell) Surya Monokristalin?",
    "id": "Panel (Sel) (Kristal Silikon Tunggal Murni)."
  },
  {
    "en": "Apa Ciri (Appearance) Sel (Cell) Monokristalin?",
    "id": "Hitam Seragam, Sudut Terpotong."
  },
  {
    "en": "Apa Itu Sel (Cell) Surya Polikristalin?",
    "id": "Panel (Sel) (Banyak Kristal Silikon)."
  },
  {
    "en": "Apa Ciri (Appearance) Sel (Cell) Polikristalin?",
    "id": "Biru Berpola (Pecahan Kaca)."
  },
  {
    "en": "Mana Lebih Efisien (Mono/Poli)?",
    "id": "Monokristalin (Lebih Efisien)."
  },
  {
    "en": "Mana Lebih Murah (Mono/Poli)?",
    "id": "Polikristalin (Lebih Murah)."
  },
  {
    "en": "Apa Itu Sel (Cell) Surya Film Tipis (Thin-Film)?",
    "id": "Sel (Lapisan Tipis Semikonduktor)."
  },
  {
    "en": "Contoh (Example) Material Sel (Cell) Film Tipis?",
    "id": "CdTe (Cadmium Telluride), CIGS (Copper Indium Gallium Selenide), Amorf."
  },
  {
    "en": "Apa Kepanjangan PERC (Passivated Emitter and Rear Cell)?",
    "id": "Passivated Emitter and Rear Cell."
  },
  {
    "en": "Tujuan (Purpose) Teknologi PERC (Passivated Emitter and Rear Cell)?",
    "id": "Meningkatkan Efisiensi (Sel) Panel Surya."
  },
  {
    "en": "Apa Itu Panel (Panel) Surya Bifacial (Bifacial)?",
    "id": "Panel (Menyerap Cahaya Dari Dua Sisi)."
  },
  {
    "en": "Apa Itu Inverter (Inverter) Jaringan (Grid-Tie)?",
    "id": "Inverter (Sinkronisasi Otomatis Jaringan PLN)."
  },
  {
    "en": "Apa Itu Inverter (Inverter) Hibrida (Hybrid)?",
    "id": "Inverter (Grid-Tie Dan Baterai)."
  },
  {
    "en": "Apa Itu Inverter (Inverter) Off-Grid (Off-Grid)?",
    "id": "Inverter (Sistem Mandiri Tanpa PLN)."
  },
  {
    "en": "Apa Itu Inverter (Inverter) Mikro (Microinverter)?",
    "id": "Inverter Kecil (Satu Per Panel Surya)."
  },
  {
    "en": "Apa Itu Pengoptimal (Optimizer) Daya (Power Optimizer)?",
    "id": "Perangkat MPPT (Maximum Power Point Tracking) Level Panel (DC (Direct Current)-DC (Direct Current))."
  },
  {
    "en": "Apa Itu Anti-Islanding (Anti-Pulau)?",
    "id": "Fitur Keamanan (Inverter Mati Saat PLN Padam)."
  },
  {
    "en": "Apa Kepanjangan BOS (Balance of System) (PV)?",
    "id": "Balance of System (Sistem Keseimbangan)."
  },
  {
    "en": "Apa (Example) Komponen BOS (Balance of System)?",
    "id": "Kabel, Mounting, Inverter, Baterai."
  },
  {
    "en": "Apa Itu Sel (Cell) Surya Heterojunction (HJT)?",
    "id": "Gabungan (Kristalin Dan Amorf Silikon)."
  },
  {
    "en": "Apa Itu Memori (Memory) Flash (Flash) NAND (Not AND)?",
    "id": "Memori (Struktur Sel Seri) (Penyimpanan Data)."
  },
  {
    "en": "Apa Itu Memori (Memory) Flash (Flash) NOR (Not OR)?",
    "id": "Memori (Struktur Sel Paralel) (Eksekusi Kode)."
  },
  {
    "en": "Mana (Flash) Lebih Cepat (Membaca)?",
    "id": "NOR (Not OR) Flash (Akses Acak Cepat)."
  },
  {
    "en": "Mana (Flash) Lebih Cepat (Menulis Hapus)?",
    "id": "NAND (Not AND) Flash (Operasi Blok)."
  },
  {
    "en": "Mana (Flash) Kepadatan (Density) Lebih Tinggi?",
    "id": "NAND (Not AND) Flash (Biaya Per Bit Murah)."
  },
  {
    "en": "Aplikasi (Application) Memori (Memory) Flash (Flash) NAND (Not AND)?",
    "id": "SSD (Solid State Drive), USB (Universal Serial Bus) Drive, Kartu Memori."
  },
  {
    "en": "Aplikasi (Application) Memori (Memory) Flash (Flash) NOR (Not OR)?",
    "id": "Penyimpanan Kode Boot (Firmware) BIOS (Basic Input Output System)."
  },
  {
    "en": "Apa Itu Sel (Cell) Level Tunggal (SLC)?",
    "id": "Satu Bit Per Sel (Paling Tahan Lama)."
  },
  {
    "en": "Apa Itu Sel (Cell) Multi Level (MLC)?",
    "id": "Dua Bit Per Sel."
  },
  {
    "en": "Apa Itu Sel (Cell) Tiga Level (TLC)?",
    "id": "Tiga Bit Per Sel (Kepadatan Tinggi)."
  },
  {
    "en": "Apa Itu Sel (Cell) Empat Level (QLC)?",
    "id": "Empat Bit Per Sel (Kapasitas Maksimal)."
  },
  {
    "en": "Apa Itu Siklus (Cycle) P/E (Program/Erase)?",
    "id": "Ukuran Daya Tahan (Endurance) Flash."
  },
  {
    "en": "Mana (SLC/MLC/TLC) Paling Tahan Lama?",
    "id": "SLC (Single-Level Cell) (Siklus P/E Tertinggi)."
  },
  {
    "en": "Apa Itu Wear Leveling (Perataan Keausan)?",
    "id": "Distribusi Penulisan (Meratakan Umur Sel)."
  },
  {
    "en": "Apa Itu Amplifikasi (Amplification) Tulis (Write)?",
    "id": "Penulisan Data Aktual (Lebih Besar Data Host)."
  },
  {
    "en": "Apa Itu Pengumpulan (Collection) Sampah (Garbage Collection)?",
    "id": "Proses (Manajemen Blok Flash) (SSD (Solid State Drive))."
  },
  {
    "en": "Apa Itu Perintah (Command) TRIM (TRIM)?",
    "id": "Perintah OS (Operating System) (Halaman Data Dihapus)."
  },
  {
    "en": "Apa Itu Memori (Memory) RRAM (Resistive RAM) (ReRAM)?",
    "id": "Memori (Resistansi Berubah) (Filamen Konduktif)."
  },
  {
    "en": "Apa Itu Memori (Memory) PCM (Phase Change Memory)?",
    "id": "Memori (Fasa Material Amorf/Kristalin)."
  },
  {
    "en": "Apa Itu Pseudokapasitor (Pseudocapacitor)?",
    "id": "Superkapasitor (Penyimpanan Muatan (Faradaic))."
  },
  {
    "en": "Perbedaan (Difference) EDLC (Electric Double-Layer Capacitor) Pseudokapasitor?",
    "id": "EDLC (Electric Double-Layer Capacitor) (Fisik), Pseudo (Kimia Redoks)."
  },
  {
    "en": "Apa Itu Kapasitor (Capacitor) Hibrida (Hybrid)?",
    "id": "Gabungan (Baterai Superkapasitor)."
  },
  {
    "en": "Material (Material) Elektroda EDLC (Electric Double-Layer Capacitor)?",
    "id": "Karbon Aktif (Luas Permukaan Tinggi)."
  },
  {
    "en": "Apa Itu Rangkaian (Circuit) Squelch (Peredam)?",
    "id": "Membungkam (Mute) Audio (Saat Sinyal Lemah)."
  },
  {
    "en": "Dimana (Where) Rangkaian (Circuit) Squelch (Peredam) Digunakan?",
    "id": "Penerima Radio (FM (Frequency Modulation), Walkie-Talkie)."
  },
  {
    "en": "Apa Itu Detektor (Detector) Kuadratur (Quadrature)?",
    "id": "Metode Demodulasi FM (Frequency Modulation) (Geser Fasa)."
  },
  {
    "en": "Apa Itu Diskriminator (Discriminator) Foster-Seeley?",
    "id": "Sirkuit Demodulasi FM (Frequency Modulation) (Peka Amplitudo)."
  },
  {
    "en": "Apa Itu Detektor (Detector) Rasio (Ratio)?",
    "id": "Sirkuit Demodulasi FM (Frequency Modulation) (Tahan Noise AM)."
  },
  {
    "en": "Apa Kepanjangan AGC (Automatic Gain Control)?",
    "id": "Automatic Gain Control (Pengatur Gain Otomatis)."
  },
  {
    "en": "Apa Kepanjangan AFC (Automatic Frequency Control)?",
    "id": "Automatic Frequency Control (Pengatur Frekuensi Otomatis)."
  },
  {
    "en": "Fungsi (Function) AFC (Automatic Frequency Control) (Radio)?",
    "id": "Mengunci Frekuensi Osilator Lokal (LO)."
  },
  {
    "en": "Apa Itu Filter (Filter) Keramik (Ceramic)?",
    "id": "Filter IF (Intermediate Frequency) (Murah, Ukuran Kecil)."
  },
  {
    "en": "Apa Itu Filter (Filter) Kristal (Crystal)?",
    "id": "Filter IF (Intermediate Frequency) (Sangat Selektif, Faktor Q Tinggi)."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) RF (Radio Frequency) Tuned (Tala)?",
    "id": "Penguat (Selektif) (Pita Frekuensi Sempit)."
  },
  {
    "en": "Apa Itu Filter (Filter) Pelacakan (Tracking)?",
    "id": "Filter (Frekuensi Tengah) (Mengikuti Sinyal Input)."
  },
  {
    "en": "Apa Itu Sirkuit (Circuit) Mute (Bisu)?",
    "id": "Mematikan (Output Audio) (Seketika)."
  },
  {
    "en": "Apa Itu S-Meter (S-Meter)?",
    "id": "Indikator Kekuatan Sinyal (Radio)."
  },
  {
    "en": "Apa Itu Graphene (Grafena)?",
    "id": "Lapisan (Tunggal Atom Karbon) (Struktur Honeycomb)."
  },
  {
    "en": "Apa Itu Carbon Nanotube (CNT)?",
    "id": "Tabung (Silinder) Atom Karbon."
  },
  {
    "en": "Apa Itu CNT (Carbon Nanotube) Dinding Tunggal (SWCNT)?",
    "id": "Single-Walled Carbon Nanotube."
  },
  {
    "en": "Apa Itu CNT (Carbon Nanotube) Dinding Ganda (MWCNT)?",
    "id": "Multi-Walled Carbon Nanotube."
  },
  {
    "en": "Sifat (Property) Listrik Graphene (Grafena)?",
    "id": "Konduktivitas Sangat Tinggi (Mobilitas Tinggi)."
  },
  {
    "en": "Apa Itu Efek (Effect) Balistik (Ballistic)?",
    "id": "Transpor Elektron (Tanpa Hamburan)."
  },
  {
    "en": "Apa Itu Efek (Effect) Termionik (Thermionic)?",
    "id": "Emisi Elektron (Akibat Energi Panas)."
  },
  {
    "en": "Apa Itu Emisi (Emission) Medan (Field Emission)?",
    "id": "Emisi Elektron (Akibat Medan Listrik Kuat)."
  },
  {
    "en": "Apa Itu Emisi (Emission) Fotoelektrik (Photoelectric)?",
    "id": "Emisi Elektron (Akibat Paparan Foton)."
  },
  {
    "en": "Apa Itu Emisi (Emission) Sekunder (Secondary Emission)?",
    "id": "Emisi Elektron (Akibat Tumbukan Partikel)."
  },
  {
    "en": "Apa Itu Anoda (Anode)?",
    "id": "Elektroda (Tempat Arus Masuk) (Elektron Keluar)."
  },
  {
    "en": "Apa Itu Katoda (Cathode)?",
    "id": "Elektroda (Tempat Arus Keluar) (Elektron Masuk)."
  },
  {
    "en": "Apa Itu Muatan (Charge) Ruang (Space Charge)?",
    "id": "Awan Elektron (Sekitar Katoda Panas)."
  },
  {
    "en": "Apa Itu Tabung (Tube) Sinar Katoda (CRT)?",
    "id": "Cathode Ray Tube (Tabung Gambar)."
  },
  {
    "en": "Apa Itu Senapan (Gun) Elektron (Electron Gun)?",
    "id": "Pembangkit (Fokus) Berkas Elektron (CRT (Cathode Ray Tube))."
  },
  {
    "en": "Apa Itu Kumparan (Coil) Defleksi (Deflection Yoke)?",
    "id": "Pembelok Berkas Elektron (Medan Magnet)."
  },
  {
    "en": "Apa Itu Pelat (Plate) Defleksi (Deflection Plates)?",
    "id": "Pembelok Berkas Elektron (Medan Listrik)."
  },
  {
    "en": "Apa Itu Lapisan (Screen) Fosfor (Phosphor)?",
    "id": "Lapisan (Berpendar) (Ditembak Elektron)."
  },
  {
    "en": "Apa Itu Masker (Mask) Bayangan (Shadow Mask)?",
    "id": "Pelat (Logam Berlubang) (Memisahkan Warna RGB)."
  },
  {
    "en": "Apa Itu Degaussing (Degaussing)?",
    "id": "Proses (Menghilangkan Sisa Magnet) (CRT (Cathode Ray Tube))."
  },
  {
    "en": "Apa Itu Tampilan (Display) Plasma (Plasma Display)?",
    "id": "Tampilan (Sel Gas Plasma (UV) Fosfor)."
  },
  {
    "en": "Apa Itu Tampilan (Display) VFD (Vacuum Fluorescent Display)?",
    "id": "Vacuum Fluorescent Display (Katodoluminesensi)."
  },
  {
    "en": "Apa Itu Tampilan (Display) Nixie (Nixie Tube)?",
    "id": "Tabung (Gas Neon) (Penampil Angka Kawat)."
  },
  {
    "en": "Apa Itu Tampilan (Display) Elektroluminesen (EL)?",
    "id": "Tampilan (Bahan Berpendar Medan Listrik)."
  },
  {
    "en": "Apa Itu Layar (Screen) Sentuh (Touchscreen) Resistif?",
    "id": "Layar (Sensor Tekanan) (Dua Lapisan Konduktif)."
  },
  {
    "en": "Apa Itu Layar (Screen) Sentuh (Touchscreen) Kapasitif?",
    "id": "Layar (Sensor Medan Listrik Tubuh)."
  },
  {
    "en": "Apa Itu Layar (Screen) Sentuh (Touchscreen) SAW (Surface Acoustic Wave)?",
    "id": "Layar (Sensor Getaran Akustik Permukaan)."
  },
  {
    "en": "Apa Itu Layar (Screen) Sentuh (Touchscreen) Inframerah (Infrared)?",
    "id": "Layar (Sensor Jaringan Sinar Inframerah)."
  }



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
