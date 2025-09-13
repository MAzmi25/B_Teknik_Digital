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


  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Dengan Dua Nilai Diskrit." },
  { "en": "Apa Dua Nilai Dalam Sinyal Digital?", "id": "0 (Rendah) Dan 1 (Tinggi)." },
  { "en": "Apa Beda Sinyal 'Digital' Dan 'Analog'?", "id": "Digital Diskrit, Sedangkan Analog Kontinu." },
  { "en": "Apa Itu 'Bit'?", "id": "Unit Informasi Terkecil Dalam Sistem Digital." },
  { "en": "Apa Itu 'Logika Tinggi (High)'?", "id": "Merepresentasikan Nilai Biner 1." },
  { "en": "Apa Itu 'Logika Rendah (Low)'?", "id": "Merepresentasikan Nilai Biner 0." },
  { "en": "Apa Dasar Matematika Untuk Teknik Digital?", "id": "Aljabar Boolean." },
  { "en": "Siapa Penemu 'Aljabar Boolean'?", "id": "George Boole." },
  { "en": "Apa Tiga Operasi Dasar 'Aljabar Boolean'?", "id": "AND, OR, Dan NOT." },
  { "en": "Apa Itu 'Gerbang Logika'?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Fungsi Gerbang 'AND'?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Kapan Output Gerbang 'AND' Bernilai 0?", "id": "Jika Salah Satu Input Bernilai 0." },
  { "en": "Apa Fungsi Gerbang 'OR'?", "id": "Output 1 Jika Salah Satu Input 1." },
  { "en": "Kapan Output Gerbang 'OR' Bernilai 0?", "id": "Jika Semua Input Bernilai 0." },
  { "en": "Apa Fungsi Gerbang 'NOT'?", "id": "Membalik Nilai Input Logika." },
  { "en": "Apa Nama Lain Gerbang 'NOT'?", "id": "Inverter (Pembalik)." },
  { "en": "Apa Itu 'Tabel Kebenaran'?", "id": "Tabel Output Untuk Semua Kombinasi Input." },
  { "en": "Apa Itu Gerbang 'NAND'?", "id": "Gerbang AND Diikuti Oleh Gerbang NOT." },
  { "en": "Kapan Output Gerbang 'NAND' Bernilai 0?", "id": "Hanya Jika Semua Input Bernilai 1." },
  { "en": "Apa Itu Gerbang 'NOR'?", "id": "Gerbang OR Diikuti Oleh Gerbang NOT." },
  { "en": "Kapan Output Gerbang 'NOR' Bernilai 1?", "id": "Hanya Jika Semua Input Bernilai 0." },
  { "en": "Gerbang Apa Yang Disebut 'Gerbang Universal'?", "id": "NAND Dan NOR." },
  { "en": "Mengapa 'NAND' Dan 'NOR' Universal?", "id": "Bisa Membentuk Semua Gerbang Lainnya." },
  { "en": "Apa Kepanjangan 'XOR (Exclusive OR)'?", "id": "Gerbang OR Eksklusif." },
  { "en": "Apa Fungsi Gerbang 'XOR (Exclusive OR)'?", "id": "Output 1 Jika Jumlah Input Ganjil." },
  { "en": "Apa Kepanjangan 'XNOR (Exclusive NOR)'?", "id": "Gerbang NOR Eksklusif." },
  { "en": "Apa Fungsi Gerbang 'XNOR (Exclusive NOR)'?", "id": "Output 1 Jika Input Sama." },
  { "en": "Apa Itu 'Buffer'?", "id": "Memperkuat Sinyal Tanpa Mengubah Logika." },
  { "en": "Apa Itu 'Sistem Bilangan Biner'?", "id": "Sistem Angka Berbasis Dua." },
  { "en": "Apa Itu 'Sistem Bilangan Heksadesimal'?", "id": "Sistem Angka Berbasis Enam Belas." },
  { "en": "Apa Itu 'Sistem Bilangan Oktal'?", "id": "Sistem Angka Berbasis Delapan." },
  { "en": "Apa Kepanjangan 'IC (Integrated Circuit)'?", "id": "Sirkuit Terpadu." },
  { "en": "Apa Fungsi 'IC (Integrated Circuit)' Logika?", "id": "Berisi Kumpulan Gerbang Logika." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Sekelompok IC (Integrated Circuit) Dengan Teknologi Sama." },
  { "en": "Apa Kepanjangan 'TTL (Transistor-Transistor Logic)'?", "id": "Logika Transistor-Transistor." },
  { "en": "Apa Kepanjangan 'CMOS (Complementary Metal-Oxide-Semiconductor)'?", "id": "MOS (Metal-Oxide-Semiconductor) Komplementer." },
  { "en": "Apa Itu 'Rangkaian Kombinasional'?", "id": "Rangkaian Tanpa Elemen Memori." },
  { "en": "Output 'Rangkaian Kombinasional' Bergantung Pada Apa?", "id": "Hanya Bergantung Pada Input Saat Ini." },
  { "en": "Apa Itu 'Rangkaian Sekuensial'?", "id": "Rangkaian Dengan Elemen Memori." },
  { "en": "Output 'Rangkaian Sekuensial' Bergantung Pada Apa?", "id": "Input Saat Ini Dan Keadaan Sebelumnya." },
  { "en": "Apa Itu 'Sinyal Clock'?", "id": "Sinyal Periodik Untuk Sinkronisasi." },
  { "en": "Apa Fungsi 'Sinyal Clock'?", "id": "Menyelaraskan Operasi Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Byte'?", "id": "Sekelompok Delapan Bit." },
  { "en": "Apa Itu 'MSB (Most Significant Bit)'?", "id": "Bit Dengan Bobot Paling Tinggi." },
  { "en": "Apa Itu 'LSB (Least Significant Bit)'?", "id": "Bit Dengan Bobot Paling Rendah." },
  { "en": "Apa Itu 'Ekspresi Boolean'?", "id": "Persamaan Matematis Untuk Rangkaian Logika." },
  { "en": "Apa Itu 'Penyederhanaan Logika'?", "id": "Mengurangi Jumlah Gerbang Dalam Rangkaian." },
  { "en": "Metode Apa Untuk 'Penyederhanaan Logika'?", "id": "Peta Karnaugh Atau Aljabar Boolean." },
  { "en": "Apa Itu 'Peta Karnaugh'?", "id": "Metode Grafis Untuk Menyederhanakan Logika." },
  { "en": "Apa Itu 'Minterm'?", "id": "Ekspresi AND Untuk Satu Kombinasi Input." },
  { "en": "Apa Itu 'Maxterm'?", "id": "Ekspresi OR Untuk Satu Kombinasi Input." },
  { "en": "Apa Itu 'Sum of Products (SOP)'?", "id": "Bentuk Logika Penjumlahan Dari Perkalian." },
  { "en": "Apa Itu 'Product of Sums (POS)'?", "id": "Bentuk Logika Perkalian Dari Penjumlahan." },
  { "en": "Apa Itu 'Logika Positif'?", "id": "Tegangan Tinggi Didefinisikan Sebagai Logika 1." },
  { "en": "Apa Itu 'Logika Negatif'?", "id": "Tegangan Rendah Didefinisikan Sebagai Logika 1." },
  { "en": "Apa Itu 'Fan-In'?", "id": "Jumlah Maksimum Input Sebuah Gerbang." },
  { "en": "Apa Itu 'Fan-Out'?", "id": "Jumlah Gerbang Yang Bisa Digerakkan Output." },
  { "en": "Apa Itu 'Tegangan Suplai'?", "id": "Tegangan Yang Dibutuhkan IC Agar Bekerja." },
  { "en": "Apa 'Tegangan Suplai' Untuk TTL?", "id": "Biasanya 5 Volt." },
  { "en": "Apa Itu 'Noise Margin'?", "id": "Ketahanan Sirkuit Terhadap Gangguan Derau." },
  { "en": "Apa Itu 'Tundaan Propagasi (Propagation Delay)'?", "id": "Waktu Sinyal Merambat Melalui Gerbang." },
  { "en": "Apa Itu 'Disipasi Daya'?", "id": "Daya Yang Dikonsumsi Oleh Gerbang." },
  { "en": "Keluarga Logika Mana Yang Hemat Daya?", "id": "CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Apa Itu 'Input Mengambang (Floating)'?", "id": "Input Yang Tidak Terhubung Ke Apapun." },
  { "en": "Apa Masalah 'Input Mengambang'?", "id": "Menyebabkan Perilaku Rangkaian Tidak Terduga." },
  { "en": "Apa Itu 'Pull-Up Resistor'?", "id": "Resistor Yang Menarik Input Ke Tinggi." },
  { "en": "Apa Itu 'Pull-Down Resistor'?", "id": "Resistor Yang Menarik Input Ke Rendah." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Teknis Sebuah Komponen." },
  { "en": "Apa Itu 'Skematik'?", "id": "Diagram Simbolik Sebuah Rangkaian Elektronik." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Pengujian Perilaku Rangkaian Secara Virtual." },
  { "en": "Apa Itu 'Prototipe'?", "id": "Versi Awal Rangkaian Untuk Pengujian Fisik." },
  { "en": "Apa Itu 'PCB (Printed Circuit Board)'?", "id": "Papan Sirkuit Tercetak." },
  { "en": "Apa Itu 'Gerbang Tri-State'?", "id": "Gerbang Dengan Tiga Keadaan Output." },
  { "en": "Apa Keadaan Ketiga 'Gerbang Tri-State'?", "id": "Impedansi Tinggi (High-Z)." },
  { "en": "Apa Fungsi Keadaan 'Impedansi Tinggi'?", "id": "Memungkinkan Berbagi Bus Data." },
  { "en": "Apa Itu 'Bus'?", "id": "Sekumpulan Jalur Konduktor Untuk Komunikasi." },
  { "en": "Apa Itu 'Encoder Prioritas'?", "id": "Encoder Yang Punya Prioritas Input." },
  { "en": "Apa Itu 'Komparator'?", "id": "Rangkaian Pembanding Dua Nilai Biner." },
  { "en": "Apa Itu 'Paritas'?", "id": "Metode Sederhana Untuk Pengecekan Kesalahan." },
  { "en": "Apa Itu 'Bit Paritas'?", "id": "Bit Tambahan Untuk Menjaga Paritas." },
  { "en": "Apa Itu 'Paritas Ganjil (Odd Parity)'?", "id": "Jumlah Bit 1 Selalu Ganjil." },
  { "en": "Apa Itu 'Paritas Genap (Even Parity)'?", "id": "Jumlah Bit 1 Selalu Genap." },
  { "en": "Apa Itu 'Generator Paritas'?", "id": "Rangkaian Untuk Menghasilkan Bit Paritas." },
  { "en": "Apa Itu 'Pemeriksa Paritas'?", "id": "Rangkaian Untuk Memeriksa Kesalahan Paritas." },
  { "en": "Apa Itu 'Kode Biner'?", "id": "Representasi Angka Menggunakan Bit." },
  { "en": "Apa Itu 'BCD (Binary-Coded Decimal)'?", "id": "Desimal Yang Dikodekan Secara Biner." },
  { "en": "Bagaimana 'BCD (Binary-Coded Decimal)' Bekerja?", "id": "Setiap Digit Desimal Diwakili 4 Bit." },
  { "en": "Apa Itu 'Kode Gray'?", "id": "Kode Biner Yang Hanya Ubah Satu Bit." },
  { "en": "Apa Keuntungan 'Kode Gray'?", "id": "Mengurangi Kesalahan Transisi." },
  { "en": "Apa Itu 'Kode ASCII (American Standard Code for Information Interchange)'?", "id": "Kode Standar Untuk Karakter Teks." },
  { "en": "Apa Itu 'Adder'?", "id": "Rangkaian Untuk Penjumlahan." },
  { "en": "Apa Itu 'Half Adder'?", "id": "Menjumlahkan Dua Bit." },
  { "en": "Apa Itu 'Full Adder'?", "id": "Menjumlahkan Tiga Bit (Dengan Carry-in)." },
  { "en": "Apa Itu 'Carry'?", "id": "Bit Simpanan Dari Penjumlahan." },
  { "en": "Apa Itu 'Borrow'?", "id": "Bit Pinjaman Dari Pengurangan." },
  { "en": "Apa Itu 'Level Tegangan'?", "id": "Rentang Tegangan Untuk Logika 0/1." },
  { "en": "Apa Elemen Dasar Rangkaian Sekuensial?", "id": "Flip-Flop Atau Latch." },
  { "en": "Apa Itu 'Flip-Flop'?", "id": "Elemen Memori Yang Menyimpan Satu Bit." },
  { "en": "Apa Perbedaan 'Flip-Flop' Dan 'Latch'?", "id": "Flip-Flop Edge-Triggered, Latch Level-Triggered." },
  { "en": "Apa Arti 'Edge-Triggered'?", "id": "Berubah Keadaan Saat Tepi Sinyal Clock." },
  { "en": "Apa Arti 'Level-Triggered'?", "id": "Aktif Selama Level Sinyal Clock Tertentu." },
  { "en": "Apa Kepanjangan 'SR Flip-Flop'?", "id": "Set-Reset Flip-Flop." },
  { "en": "Apa Kondisi Terlarang 'SR Flip-Flop'?", "id": "Saat S = 1 Dan R = 1." },
  { "en": "Apa Kepanjangan 'JK Flip-Flop'?", "id": "Jack-Kilby Flip-Flop." },
  { "en": "Bagaimana 'JK Flip-Flop' Mengatasi Kondisi Terlarang?", "id": "Dengan Mode Toggle (Membalik Keadaan)." },
  { "en": "Apa Kepanjangan 'D Flip-Flop'?", "id": "Data Atau Delay Flip-Flop." },
  { "en": "Apa Fungsi 'D Flip-Flop'?", "id": "Menyimpan Nilai Input Saat Clock Aktif." },
  { "en": "Apa Kepanjangan 'T Flip-Flop'?", "id": "Toggle Flip-Flop." },
  { "en": "Apa Fungsi 'T Flip-Flop'?", "id": "Membalik Keadaan Output (Toggle)." },
  { "en": "Apa Itu 'Register'?", "id": "Kumpulan Flip-Flop Untuk Menyimpan Data." },
  { "en": "Apa Fungsi 'Register Geser (Shift Register)'?", "id": "Menggeser Data Bit Per Bit." },
  { "en": "Apa Itu 'Register Penyimpan (Storage Register)'?", "id": "Menyimpan Data Biner Secara Paralel." },
  { "en": "Apa Itu 'Counter (Penghitung)'?", "id": "Rangkaian Yang Menghitung Pulsa Clock." },
  { "en": "Apa Itu 'Counter Asinkron'?", "id": "Output Satu Flip-Flop Jadi Clock Berikutnya." },
  { "en": "Apa Nama Lain 'Counter Asinkron'?", "id": "Ripple Counter." },
  { "en": "Apa Itu 'Counter Sinkron'?", "id": "Semua Flip-Flop Dikendalikan Clock Yang Sama." },
  { "en": "Mana Yang Lebih Cepat, 'Sinkron' Atau 'Asinkron'?", "id": "Counter Sinkron Lebih Cepat." },
  { "en": "Apa Itu 'Counter Naik (Up Counter)'?", "id": "Menghitung Dari Nilai Rendah Ke Tinggi." },
  { "en": "Apa Itu 'Counter Turun (Down Counter)'?", "id": "Menghitung Dari Nilai Tinggi Ke Rendah." },
  { "en": "Apa Itu 'State (Keadaan)'?", "id": "Informasi Yang Tersimpan Dalam Sirkuit Sekuensial." },
  { "en": "Apa Itu 'State Machine (Mesin Keadaan)'?", "id": "Model Abstrak Untuk Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Diagram State'?", "id": "Representasi Grafis Dari Sebuah State Machine." },
  { "en": "Apa Itu 'Multiplexer (MUX)'?", "id": "Sirkuit Pemilih Satu Dari Banyak Input." },
  { "en": "Apa Nama Lain 'Multiplexer'?", "id": "Data Selector." },
  { "en": "Apa Itu 'Demultiplexer (DEMUX)'?", "id": "Sirkuit Pengirim Input Ke Banyak Output." },
  { "en": "Apa Nama Lain 'Demultiplexer'?", "id": "Data Distributor." },
  { "en": "Apa Itu 'Encoder'?", "id": "Mengubah Input Aktif Menjadi Kode Biner." },
  { "en": "Apa Itu 'Decoder'?", "id": "Mengubah Kode Biner Menjadi Output Aktif." },
  { "en": "Apa Fungsi 'Decoder BCD ke 7-Segmen'?", "id": "Menampilkan Angka Desimal Pada Layar." },
  { "en": "Apa Itu 'Komparator Magnitudo'?", "id": "Membandingkan Dua Bilangan Biner." },
  { "en": "Apa Tiga Output 'Komparator'?", "id": "A > B, A < B, A = B." },
  { "en": "Apa Itu 'Rangkaian Aritmetika'?", "id": "Rangkaian Untuk Operasi Matematika." },
  { "en": "Apa Itu 'Carry Look-Ahead'?", "id": "Teknik Mempercepat Penjumlahan Biner." },
  { "en": "Apa Itu 'ALU (Arithmetic Logic Unit)'?", "id": "Unit Aritmetika Dan Logika." },
  { "en": "Di Mana 'ALU (Arithmetic Logic Unit)' Ditemukan?", "id": "Di Dalam CPU (Central Processing Unit)." },
  { "en": "Apa Itu 'Memori Digital'?", "id": "Komponen Penyimpan Data Biner." },
  { "en": "Apa Kepanjangan 'RAM (Random-Access Memory)'?", "id": "Memori Akses Acak." },
  { "en": "Apa Sifat 'RAM (Random-Access Memory)'?", "id": "Volatil (Data Hilang Tanpa Daya)." },
  { "en": "Apa Kepanjangan 'ROM (Read-Only Memory)'?", "id": "Memori Hanya Baca." },
  { "en": "Apa Sifat 'ROM (Read-Only Memory)'?", "id": "Non-Volatil (Data Tetap Tanpa Daya)." },
  { "en": "Apa Kepanjangan 'PROM (Programmable ROM)'?", "id": "ROM (Read-Only Memory) Yang Dapat Diprogram." },
  { "en": "Apa Kepanjangan 'EPROM (Erasable PROM)'?", "id": "PROM (Programmable ROM) Yang Bisa Dihapus." },
  { "en": "Bagaimana Cara Menghapus 'EPROM'?", "id": "Dengan Sinar Ultraviolet (UV)." },
  { "en": "Apa Kepanjangan 'EEPROM (Electrically EPROM)'?", "id": "EPROM (Erasable PROM) Yang Dihapus Listrik." },
  { "en": "Apa Itu 'Memori Flash'?", "id": "Jenis EEPROM Yang Dihapus Per Blok." },
  { "en": "Di Mana 'Memori Flash' Digunakan?", "id": "SSD (Solid-State Drive), USB (Universal Serial Bus) Drive." },
  { "en": "Apa Kepanjangan 'SRAM (Static RAM)'?", "id": "RAM (Random-Access Memory) Statis." },
  { "en": "Apa Kepanjangan 'DRAM (Dynamic RAM)'?", "id": "RAM (Random-Access Memory) Dinamis." },
  { "en": "Apa Komponen Penyimpan 'SRAM'?", "id": "Flip-Flop." },
  { "en": "Apa Komponen Penyimpan 'DRAM'?", "id": "Kapasitor." },
  { "en": "Memori Mana Yang Perlu Di-Refresh?", "id": "DRAM (Dynamic RAM)." },
  { "en": "Memori Mana Yang Lebih Cepat?", "id": "SRAM (Static RAM)." },
  { "en": "Memori Mana Yang Lebih Padat?", "id": "DRAM (Dynamic RAM)." },
  { "en": "Apa Itu 'PLD (Programmable Logic Device)'?", "id": "Perangkat Logika Yang Dapat Diprogram." },
  { "en": "Apa Kepanjangan 'FPGA (Field-Programmable Gate Array)'?", "id": "Array Gerbang Terprogram Lapangan." },
  { "en": "Apa Keuntungan 'FPGA (Field-Programmable Gate Array)'?", "id": "Fleksibel Dan Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Kepanjangan 'ASIC (Application-Specific Integrated Circuit)'?", "id": "IC (Integrated Circuit) Untuk Aplikasi Spesifik." },
  { "en": "Apa Keuntungan 'ASIC (Application-Specific Integrated Circuit)'?", "id": "Kinerja Tinggi Dan Efisiensi Daya." },
  { "en": "Apa Kepanjangan 'ADC (Analog-to-Digital Converter)'?", "id": "Konverter Analog Ke Digital." },
  { "en": "Apa Fungsi 'ADC (Analog-to-Digital Converter)'?", "id": "Mengubah Sinyal Analog Menjadi Biner." },
  { "en": "Apa Kepanjangan 'DAC (Digital-to-Analog Converter)'?", "id": "Konverter Digital Ke Analog." },
  { "en": "Apa Fungsi 'DAC (Digital-to-Analog Converter)'?", "id": "Mengubah Sinyal Biner Menjadi Analog." },
  { "en": "Apa Itu 'Resolusi' ADC/DAC?", "id": "Jumlah Bit Yang Digunakan." },
  { "en": "Apa Itu 'Sampling'?", "id": "Mengukur Sinyal Analog Pada Interval Waktu." },
  { "en": "Apa Itu 'Kuantisasi'?", "id": "Membulatkan Nilai Sampel Ke Level Diskrit." },
  { "en": "Apa Itu 'Antarmuka (Interface)'?", "id": "Jalur Komunikasi Antara Dua Sistem." },
  { "en": "Apa Itu 'Protokol'?", "id": "Aturan Komunikasi." },
  { "en": "Apa Itu 'Komunikasi Serial'?", "id": "Mengirim Data Satu Bit Sekaligus." },
  { "en": "Apa Itu 'Komunikasi Paralel'?", "id": "Mengirim Banyak Bit Secara Bersamaan." },
  { "en": "Apa Itu 'Hazard' Dalam Rangkaian?", "id": "Potensi Glitch Akibat Tundaan Gerbang." },
  { "en": "Apa Itu 'Glitch'?", "id": "Output Sesaat Yang Salah." },
  { "en": "Apa Itu 'Race Condition'?", "id": "Hasil Bergantung Pada Urutan Sinyal Tiba." },
  { "en": "Apa Itu 'Metastabilitas'?", "id": "Keadaan Output Yang Tidak Stabil Atau Menentu." },
  { "en": "Kapan 'Metastabilitas' Terjadi?", "id": "Saat Melanggar Waktu Setup Atau Hold." },
  { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Input Stabil Sebelum Clock." },
  { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Input Stabil Setelah Clock." },
  { "en": "Apa Itu 'Mikroprosesor'?", "id": "Unit Pemroses Pusat (CPU) Dalam Satu Chip." },
  { "en": "Apa Itu 'Mikrokontroler'?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Beda 'Mikroprosesor' Dan 'Mikrokontroler'?", "id": "Mikrokontroler Punya RAM, ROM, Dan I/O." },
  { "en": "Apa Kepanjangan 'CPU (Central Processing Unit)'?", "id": "Unit Pemroses Pusat." },
  { "en": "Apa Kepanjangan 'I/O (Input/Output)'?", "id": "Masukan/Keluaran." },
  { "en": "Apa Itu 'Bahasa Assembly'?", "id": "Bahasa Pemrograman Level Rendah." },
  { "en": "Apa Itu 'Bahasa Mesin'?", "id": "Instruksi Biner Yang Dipahami CPU." },
  { "en": "Apa Kepanjangan 'HDL (Hardware Description Language)'?", "id": "Bahasa Deskripsi Perangkat Keras." },
  { "en": "Apa Contoh Bahasa 'HDL'?", "id": "VHDL Atau Verilog." },
  { "en": "Apa Itu 'Sintesis'?", "id": "Menerjemahkan Kode HDL Menjadi Gerbang Logika." },
  { "en": "Apa Itu 'Simulasi Logika'?", "id": "Menguji Perilaku Desain HDL." },
  { "en": "Apa Itu 'Keluaran Open-Collector'?", "id": "Output TTL Yang Membutuhkan Pull-up." },
  { "en": "Apa Itu 'Keluaran Open-Drain'?", "id": "Output CMOS Yang Membutuhkan Pull-up." },
  { "en": "Apa Itu 'Wired-AND'?", "id": "Menghubungkan Output Open-Collector Bersama." },
  { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Struktur Output Standar Pada TTL." },
  { "en": "Apa Itu 'Buffer Inverting'?", "id": "Buffer Yang Juga Membalikkan Sinyal." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Input Dengan Histeresis Untuk Sinyal Bising." },
  { "en": "Apa Itu 'Penyederhanaan Peta Karnaugh'?", "id": "Mengelompokkan Angka 1 Untuk Sederhanakan Logika." },
  { "en": "Aturan Apa Untuk Mengelompokkan Di 'Peta Karnaugh'?", "id": "Kelompok Harus Berbentuk Persegi (2, 4, 8)." },
  { "en": "Apa Itu 'Don't Care Condition'?", "id": "Kondisi Input Yang Tidak Pernah Terjadi." },
  { "en": "Bagaimana 'Don't Care' Membantu Penyederhanaan?", "id": "Bisa Dianggap 1 Atau 0." },
  { "en": "Apa Itu 'Decoder 2-ke-4'?", "id": "Mengaktifkan Satu Dari Empat Output." },
  { "en": "Berapa Input Pilihan 'Multiplexer 8-ke-1'?", "id": "Tiga Input Pilihan (Select)." },
  { "en": "Apa Itu 'Enable Input'?", "id": "Input Untuk Mengaktifkan Atau Menonaktifkan Chip." },
  { "en": "Apa Itu 'Rangkaian Ripple Carry Adder'?", "id": "Penjumlah Dengan Carry Yang Merambat." },
  { "en": "Apa Kelemahan 'Ripple Carry Adder'?", "id": "Lambat Untuk Penjumlahan Bit Banyak." },
  { "en": "Apa Itu 'Signed Number'?", "id": "Bilangan Biner Yang Bisa Positif/Negatif." },
  { "en": "Apa Itu 'Representasi Two's Complement'?", "id": "Metode Umum Untuk Bilangan Bertanda." },
  { "en": "Bagaimana Cara Mendapatkan 'Two's Complement'?", "id": "Inversi Semua Bit Lalu Tambah Satu." },
  { "en": "Apa Itu 'Aritmetika Biner'?", "id": "Operasi Matematika Pada Bilangan Biner." },
  { "en": "Apa Itu 'Overflow' Aritmetika?", "id": "Hasil Melebihi Kapasitas Bit." },
  { "en": "Apa Itu 'State' Dalam Mesin Keadaan?", "id": "Representasi Memori Sistem." },
  { "en": "Apa Itu 'Transisi State'?", "id": "Perpindahan Dari Satu Keadaan Ke Keadaan Lain." },
  { "en": "Apa Yang Memicu 'Transisi State'?", "id": "Input Dan Sinyal Clock." },
  { "en": "Apa Itu 'Siklus State'?", "id": "Urutan Keadaan Yang Berulang." },
  { "en": "Apa Itu 'Register Paralel (Parallel Register)'?", "id": "Register Yang Memuat Semua Bit Sekaligus." },
  { "en": "Apa Itu 'Register Serial'?", "id": "Register Yang Memuat Bit Satu Per Satu." },
  { "en": "Apa Kepanjangan 'SISO' Shift Register?", "id": "Serial-In, Serial-Out." },
  { "en": "Apa Kepanjangan 'PIPO' Shift Register?", "id": "Parallel-In, Parallel-Out." },
  { "en": "Apa Kegunaan 'Shift Register'?", "id": "Konversi Data Serial-Paralel." },
  { "en": "Apa Itu 'Counter Modulo-N'?", "id": "Counter Yang Menghitung Sampai N-1." },
  { "en": "Bagaimana Cara Membuat 'Counter Modulo-10'?", "id": "Dengan Mereset Counter Pada Hitungan 10." },
  { "en": "Apa Nama Lain 'Counter Modulo-10'?", "id": "Decade Counter." },
  { "en": "Apa Itu 'Ring Counter'?", "id": "Shift Register Dengan Umpan Balik." },
  { "en": "Apa Itu 'Johnson Counter'?", "id": "Varian Ring Counter Dengan Umpan Balik Terbalik." },
  { "en": "Apa Itu 'Keluarga Logika Bipolar'?", "id": "Keluarga Logika Berbasis Transistor BJT." },
  { "en": "Apa Itu 'Keluarga Logika MOS'?", "id": "Keluarga Logika Berbasis Transistor MOSFET." },
  { "en": "Apa Kerugian 'Logika TTL'?", "id": "Konsumsi Daya Relatif Tinggi." },
  { "en": "Apa Kerugian 'Logika CMOS'?", "id": "Rentan Terhadap Kerusakan ESD (Electrostatic Discharge)." },
  { "en": "Apa Itu 'Antarmuka Keluarga Logika'?", "id": "Menghubungkan Dua Jenis Keluarga Logika." },
  { "en": "Apa Itu 'Level Shifter'?", "id": "Sirkuit Pengubah Level Tegangan Logika." },
  { "en": "Apa Itu 'Kapasitansi Beban'?", "id": "Total Kapasitansi Yang Digerakkan Output." },
  { "en": "Bagaimana 'Kapasitansi Beban' Mempengaruhi Kecepatan?", "id": "Beban Besar, Rangkaian Lebih Lambat." },
  { "en": "Apa Itu 'Sirkuit Terbuka (Open Circuit)'?", "id": "Kondisi Jalur Yang Tidak Terhubung." },
  { "en": "Apa Itu 'Hubung Singkat (Short Circuit)'?", "id": "Kondisi Jalur Terhubung Langsung." },
  { "en": "Apa Itu 'Sistem Digital'?", "id": "Sistem Yang Memproses Informasi Digital." },
  { "en": "Contoh 'Sistem Digital'?", "id": "Komputer, Kalkulator, Ponsel." },
  { "en": "Apa Itu 'Clock Edge'?", "id": "Tepi Naik Atau Turun Sinyal Clock." },
  { "en": "Apa Itu 'Frekuensi Clock'?", "id": "Jumlah Siklus Clock Per Detik." },
  { "en": "Apa Itu 'Siklus Tugas (Duty Cycle)'?", "id": "Persentase Waktu Sinyal Dalam Keadaan Tinggi." },
  { "en": "Apa Itu 'Sirkuit Sinkron'?", "id": "Operasi Dikendalikan Oleh Sinyal Clock." },
  { "en": "Apa Itu 'Sirkuit Asinkron'?", "id": "Operasi Tanpa Sinyal Clock Global." },
  { "en": "Apa Itu 'Arsitektur Komputer'?", "id": "Desain Konseptual Sistem Komputer." },
  { "en": "Apa Itu 'Unit Kontrol'?", "id": "Bagian CPU (Central Processing Unit) Pengatur Operasi." },
  { "en": "Apa Itu 'Bus Alamat'?", "id": "Bus Untuk Mentransfer Alamat Memori." },
  { "en": "Apa Itu 'Bus Data'?", "id": "Bus Untuk Mentransfer Data." },
  { "en": "Apa Itu 'Bus Kontrol'?", "id": "Bus Untuk Sinyal Kontrol (Baca/Tulis)." },
  { "en": "Apa Itu 'Siklus Instruksi'?", "id": "Proses Pengambilan Dan Eksekusi Instruksi." },
  { "en": "Apa Itu 'Fetch-Decode-Execute'?", "id": "Tiga Tahap Dasar Siklus Instruksi." },
  { "en": "Apa Itu 'Memori Cache'?", "id": "Memori Kecil Cepat Antara CPU Dan RAM." },
  { "en": "Apa Prinsip Kerja 'Cache'?", "id": "Prinsip Lokalitas Referensi." },
  { "en": "Apa Itu 'Lokalitas Temporal'?", "id": "Data Yang DiAkses Akan DiAkses Lagi." },
  { "en": "Apa Itu 'Lokalitas Spasial'?", "id": "Data Dekat Akan DiAkses Berdekatan." },
  { "en": "Apa Itu 'DMA (Direct Memory Access)'?", "id": "Akses Memori Langsung Tanpa Melewati CPU." },
  { "en": "Apa Keuntungan 'DMA (Direct Memory Access)'?", "id": "Membebaskan CPU (Central Processing Unit) Untuk Tugas Lain." },
  { "en": "Apa Itu 'Interrupt'?", "id": "Sinyal Yang Menginterupsi Eksekusi CPU." },
  { "en": "Apa Fungsi 'Interrupt'?", "id": "Menangani Kejadian Penting Secara Cepat." },
  { "en": "Apa Itu 'Vektor Interrupt'?", "id": "Tabel Alamat Rutin Penanganan Interrupt." },
  { "en": "Apa Itu 'Polling'?", "id": "CPU (Central Processing Unit) Terus-menerus Memeriksa Status." },
  { "en": "Apa Beda 'Interrupt' Dan 'Polling'?", "id": "Interrupt Efisien, Polling Boros Waktu CPU." },
  { "en": "Apa Itu 'Boolean Postulate'?", "id": "Aturan Dasar Dalam Aljabar Boolean." },
  { "en": "Apa Itu 'Hukum De Morgan'?", "id": "Aturan Untuk Menginversi Ekspresi Boolean." },
  { "en": "Apa Itu 'Gerbang Buffer Tri-State'?", "id": "Buffer Dengan Output Bisa Impedansi Tinggi." },
  { "en": "Apa Itu 'Floating Bus'?", "id": "Bus Yang Tidak Digerakkan Oleh Output." },
  { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Struktur Output Khas Pada Logika TTL." },
  { "en": "Apa Itu 'Rangkaian Logika Programmable'?", "id": "Nama Lain Untuk PLD." },
  { "en": "Apa Itu 'Antifuse'?", "id": "Koneksi Sekali Program Dalam FPGA." },
  { "en": "Apa Itu 'SRAM Cell'?", "id": "Elemen Penyimpan Konfigurasi FPGA." },
  { "en": "Apa Itu 'Bitstream'?", "id": "File Konfigurasi Untuk FPGA." },
  { "en": "Apa Itu 'JTAG (Joint Test Action Group)'?", "id": "Standar Untuk Pengujian Dan Pemrograman Chip." },
  { "en": "Apa Itu 'Verifikasi Formal'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Itu 'LVS (Layout Versus Schematic)'?", "id": "Membandingkan Layout Dengan Skematik Asli." },
  { "en": "Apa Itu 'DRC (Design Rule Check)'?", "id": "Memeriksa Apakah Layout Mematuhi Aturan." },
  { "en": "Apa Itu 'Transceiver'?", "id": "Perangkat Yang Bisa Mengirim Dan Menerima." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Menggabungkan Beberapa Sinyal Menjadi Satu." },
  { "en": "Apa Itu 'Arus Sumber (Source Current)'?", "id": "Arus Yang Keluar Dari Output Logika." },
  { "en": "Apa Itu 'Arus Tenggelam (Sink Current)'?", "id": "Arus Yang Masuk Ke Output Logika." },
  { "en": "Apa Itu 'Kompatibilitas Logika'?", "id": "Kemampuan IC Berkomunikasi Satu Sama Lain." },
  { "en": "Apa Itu 'Latch-Up'?", "id": "Kondisi Arus Tinggi Abnormal Pada CMOS." },
  { "en": "Apa Penyebab 'Latch-Up'?", "id": "Terpicunya Struktur SCR (Silicon Controlled Rectifier) Parasit." },
  { "en": "Apa Itu 'Guard Ring'?", "id": "Struktur Layout Untuk Mencegah Latch-Up." },
  { "en": "Apa Itu 'Sistem Tertanam (Embedded System)'?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Tertanam Dalam ROM." },
  { "en": "Apa Itu 'Waktu Tahan (Hold Time)'?", "id": "Waktu Input Harus Stabil Setelah Clock." },
  { "en": "Apa Itu 'Waktu Siap (Setup Time)'?", "id": "Waktu Input Harus Stabil Sebelum Clock." },
  { "en": "Apa Itu 'Penundaan Gerbang (Gate Delay)'?", "id": "Nama Lain Untuk Tundaan Propagasi." },
  { "en": "Apa Itu 'Clock Skew'?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu 'Clock Jitter'?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Itu 'Binary Adder'?", "id": "Nama Lain Untuk Rangkaian Penjumlah." },
  { "en": "Apa Itu 'Magnitude'?", "id": "Nilai Atau Besaran Sebuah Bilangan." },
  { "en": "Apa Itu 'MSI (Medium Scale Integration)'?", "id": "Integrasi Skala Menengah." },
  { "en": "Apa Itu 'Register Buffer'?", "id": "Register Untuk Menyimpan Data Sementara." },
  { "en": "Apa Itu 'Mode Toggle' Pada Flip-Flop?", "id": "Keadaan Output Berubah Setiap Pulsa Clock." },
  { "en": "Flip-Flop Apa Yang Punya 'Mode Toggle'?", "id": "JK Flip-Flop Dan T Flip-Flop." },
  { "en": "Apa Itu 'Asynchronous Input' Pada Flip-Flop?", "id": "Input Yang Bekerja Tanpa Clock." },
  { "en": "Sebutkan Contoh 'Asynchronous Input'?", "id": "Preset Dan Clear." },
  { "en": "Apa Fungsi 'Preset'?", "id": "Memaksa Output Flip-Flop Menjadi 1." },
  { "en": "Apa Fungsi 'Clear'?", "id": "Memaksa Output Flip-Flop Menjadi 0." },
  { "en": "Apa Itu 'Counter Biner'?", "id": "Counter Yang Menghitung Dalam Urutan Biner." },
  { "en": "Apa Itu 'State Assignment'?", "id": "Memberi Kode Biner Pada Setiap Keadaan." },
  { "en": "Apa Itu 'State Table'?", "id": "Tabel Yang Menjelaskan Transisi State." },
  { "en": "Apa Itu 'Excitation Table'?", "id": "Tabel Input Yang Dibutuhkan Flip-Flop." },
  { "en": "Apa Kegunaan 'Decoder 3-ke-8'?", "id": "Memilih Satu Dari Delapan Perangkat." },
  { "en": "Apa Itu 'Priority Encoder'?", "id": "Encoder Dengan Tingkat Prioritas Input." },
  { "en": "Apa Itu 'Arithmetic Overflow'?", "id": "Hasil Operasi Melebihi Kapasitas Bit." },
  { "en": "Bagaimana Mendeteksi 'Overflow' Pada Penjumlahan?", "id": "Jika Carry-in Dan Carry-out Berbeda." },
  { "en": "Apa Itu 'Rangkaian Pengurang (Subtractor)'?", "id": "Rangkaian Untuk Operasi Pengurangan Biner." },
  { "en": "Apa Itu 'Half Subtractor'?", "id": "Mengurangkan Dua Bit." },
  { "en": "Apa Itu 'Full Subtractor'?", "id": "Mengurangkan Tiga Bit (Dengan Borrow-in)." },
  { "en": "Apa Itu 'Representasi Sign-Magnitude'?", "id": "Bit Paling Kiri Sebagai Tanda." },
  { "en": "Apa Kelemahan 'Sign-Magnitude'?", "id": "Punya Dua Representasi Untuk Nol." },
  { "en": "Apa Itu 'One's Complement'?", "id": "Inversi Dari Setiap Bit Bilangan Biner." },
  { "en": "Apa Itu 'Bus Multiplexing'?", "id": "Menggunakan Bus Yang Sama Untuk Berbagai Tujuan." },
  { "en": "Apa Itu 'Arbiter Bus'?", "id": "Sirkuit Yang Mengatur Akses Ke Bus." },
  { "en": "Apa Itu 'Handshaking'?", "id": "Protokol Sinyal Untuk Komunikasi Asinkron." },
  { "en": "Apa Itu 'Logic Probe'?", "id": "Alat Sederhana Untuk Melihat Level Logika." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Untuk Menganalisis Sinyal Digital." },
  { "en": "Apa Beda 'Logic Analyzer' Dan 'Osiloskop'?", "id": "Analyzer Untuk Digital, Osiloskop Analog." },
  { "en": "Apa Itu 'Timing Diagram'?", "id": "Grafik Sinyal Digital Terhadap Waktu." },
  { "en": "Apa Itu 'Glitches'?", "id": "Pulsa Sesaat Yang Tidak Diinginkan." },
  { "en": "Apa Itu 'Debouncing'?", "id": "Menghilangkan Pantulan Sinyal Dari Saklar." },
  { "en": "Mengapa Saklar Mekanis Perlu 'Debouncing'?", "id": "Karena Kontaknya Memantul Secara Fisik." },
  { "en": "Apa Itu 'Sirkuit RC' Untuk Debouncing?", "id": "Menggunakan Resistor Dan Kapasitor." },
  { "en": "Apa Itu 'Sirkuit Latch SR' Untuk Debouncing?", "id": "Menggunakan Latch Untuk Menghilangkan Pantulan." },
  { "en": "Apa Itu 'Boolean Variable'?", "id": "Variabel Dengan Dua Nilai (Benar/Salah)." },
  { "en": "Apa Itu 'Boolean Function'?", "id": "Fungsi Yang Menghasilkan Nilai Boolean." },
  { "en": "Apa Itu 'Kanonis'?", "id": "Bentuk Standar Sebuah Ekspresi." },
  { "en": "Apa Itu 'Bentuk Kanonis SOP'?", "id": "Penjumlahan Dari Semua Minterm." },
  { "en": "Apa Itu 'Bentuk Kanonis POS'?", "id": "Perkalian Dari Semua Maxterm." },
  { "en": "Apa Itu 'Teorema Dualitas'?", "id": "Menukar AND/OR Dan 0/1." },
  { "en": "Apa Itu 'Bubble' Pada Simbol Gerbang?", "id": "Merepresentasikan Operasi Inversi (NOT)." },
  { "en": "Apa Itu 'Gerbang Logika Tiga Input'?", "id": "Gerbang Yang Menerima Tiga Sinyal Input." },
  { "en": "Apa Itu 'Sistem Kombinasional'?", "id": "Nama Lain Untuk Rangkaian Kombinasional." },
  { "en": "Apa Itu 'Sistem Sekuensial'?", "id": "Nama Lain Untuk Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Finite State Machine (FSM)'?", "id": "Nama Formal Untuk Mesin Keadaan." },
  { "en": "Apa Itu 'Clock Domain'?", "id": "Bagian Sirkuit Yang Gunakan Clock Sama." },
  { "en": "Apa Itu 'Clock Domain Crossing (CDC)'?", "id": "Transfer Data Antar Domain Clock Berbeda." },
  { "en": "Mengapa 'CDC (Clock Domain Crossing)' Sulit?", "id": "Dapat Menyebabkan Metastabilitas." },
  { "en": "Apa Itu 'Sinkronizer'?", "id": "Sirkuit Untuk Mentransfer Sinyal Antar Clock." },
  { "en": "Apa Itu 'Sinkronizer Dua Flip-Flop'?", "id": "Metode Umum Untuk Mengurangi Metastabilitas." },
  { "en": "Apa Itu 'FIFO (First-In, First-Out)'?", "id": "Memori Buffer, Data Masuk Pertama Keluar Pertama." },
  { "en": "Apa Fungsi 'FIFO' Dalam CDC?", "id": "Menjembatani Transfer Data Antar Domain." },
  { "en": "Apa Itu 'Address Decoder'?", "id": "Sirkuit Untuk Memilih Lokasi Memori." },
  { "en": "Apa Itu 'Memory Bank'?", "id": "Sekelompok Chip Memori Yang Bekerja Sama." },
  { "en": "Apa Itu 'Chip Select' Atau 'Chip Enable'?", "id": "Sinyal Untuk Mengaktifkan Chip Tertentu." },
  { "en": "Apa Itu 'Write Enable'?", "id": "Sinyal Kontrol Untuk Operasi Tulis." },
  { "en": "Apa Itu 'Output Enable'?", "id": "Sinyal Kontrol Untuk Mengaktifkan Output." },
  { "en": "Apa Itu 'Static RAM (SRAM)'?", "id": "RAM (Random-Access Memory) Berbasis Latch/Flip-Flop." },
  { "en": "Apa Itu 'Dynamic RAM (DRAM)'?", "id": "RAM (Random-Access Memory) Berbasis Kapasitor." },
  { "en": "Apa Itu 'Refresh Cycle'?", "id": "Proses Mengisi Ulang Kapasitor DRAM." },
  { "en": "Apa Itu 'SDRAM (Synchronous DRAM)'?", "id": "DRAM (Dynamic RAM) Yang Sinkron Dengan Clock." },
  { "en": "Apa Itu 'DDR SDRAM'?", "id": "SDRAM (Synchronous DRAM) Dengan Laju Data Ganda." },
  { "en": "Apa Itu 'Volatile Memory'?", "id": "Memori Yang Datanya Hilang Tanpa Daya." },
  { "en": "Apa Itu 'Non-Volatile Memory'?", "id": "Memori Yang Datanya Tetap Tanpa Daya." },
  { "en": "Apa Itu 'Mask ROM'?", "id": "ROM (Read-Only Memory) Yang Diprogram Saat Fabrikasi." },
  { "en": "Apa Itu 'OTP (One-Time Programmable)'?", "id": "Perangkat Yang Hanya Bisa Diprogram Sekali." },
  { "en": "Apa Itu 'UV EPROM'?", "id": "EPROM (Erasable PROM) Yang Dihapus Sinar UV." },
  { "en": "Apa Itu 'Flash Memory'?", "id": "Memori Non-Volatil Yang Dihapus Secara Elektrik." },
  { "en": "Apa Tipe 'Flash Memory'?", "id": "NAND Dan NOR." },
  { "en": "Flash Mana Yang Lebih Cepat Dibaca?", "id": "Flash Tipe NOR." },
  { "en": "Flash Mana Yang Lebih Padat?", "id": "Flash Tipe NAND." },
  { "en": "Apa Itu 'Sequential Access'?", "id": "Mengakses Data Secara Berurutan." },
  { "en": "Apa Itu 'Random Access'?", "id": "Mengakses Data Langsung Dengan Alamat." },
  { "en": "Apa Itu 'Logic High Voltage (Vih)'?", "id": "Tegangan Input Minimum Untuk Logika 1." },
  { "en": "Apa Itu 'Logic Low Voltage (Vil)'?", "id": "Tegangan Input Maksimum Untuk Logika 0." },
  { "en": "Apa Itu 'Logic High Output Voltage (Voh)'?", "id": "Tegangan Output Minimum Untuk Logika 1." },
  { "en": "Apa Itu 'Logic Low Output Voltage (Vol)'?", "id": "Tegangan Output Maksimum Untuk Logika 0." },
  { "en": "Apa Itu 'Arsitektur Set Instruksi (ISA)'?", "id": "Spesifikasi Instruksi Yang Dipahami Prosesor." },
  { "en": "Apa Kepanjangan 'RISC (Reduced Instruction Set Computer)'?", "id": "Komputer Set Instruksi Yang Disederhanakan." },
  { "en": "Apa Kepanjangan 'CISC (Complex Instruction Set Computer)'?", "id": "Komputer Set Instruksi Kompleks." },
  { "en": "Apa Itu 'Pipelining'?", "id": "Teknik Tumpang Tindih Eksekusi Instruksi." },
  { "en": "Apa Itu 'Program Counter (PC)'?", "id": "Register Penunjuk Alamat Instruksi." },
  { "en": "Apa Itu 'Instruction Register (IR)'?", "id": "Register Penyimpan Instruksi Saat Ini." },
  { "en": "Apa Itu 'Stack'?", "id": "Struktur Memori LIFO (Last-In, First-Out)." },
  { "en": "Apa Itu 'Stack Pointer'?", "id": "Register Penunjuk Puncak Stack." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Itu 'BIOS (Basic Input/Output System)'?", "id": "Firmware Untuk Mem-boot Komputer." },
  { "en": "Apa Itu 'Boolean Algebra Postulate'?", "id": "Aturan Dasar Yang Tidak Perlu Dibuktikan." },
  { "en": "Apa Itu 'Hukum Komutatif'?", "id": "A + B = B + A." },
  { "en": "Apa Itu 'Hukum Asosiatif'?", "id": "A + (B + C) = (A + B) + C." },
  { "en": "Apa Itu 'Hukum Distributif'?", "id": "A(B + C) = AB + AC." },
  { "en": "Apa Itu 'Literal'?", "id": "Variabel Atau Inversi Variabel." },
  { "en": "Apa Itu 'Sintaks' HDL?", "id": "Aturan Penulisan Kode HDL." },
  { "en": "Apa Itu 'Modul' Dalam Verilog?", "id": "Blok Bangunan Dasar Sebuah Desain." },
  { "en": "Apa Itu 'Entitas' Dalam VHDL?", "id": "Mendefinisikan Input Dan Output." },
  { "en": "Apa Itu 'Arsitektur' Dalam VHDL?", "id": "Mendefinisikan Perilaku Entitas." },
  { "en": "Apa Itu 'Testbench'?", "id": "Kode HDL Untuk Mensimulasikan Desain." },
  { "en": "Apa Itu 'Sinyal'?", "id": "Representasi Jalur Atau Koneksi." },
  { "en": "Apa Itu 'Binary Coded Decimal (BCD)'?", "id": "Sistem Pengkodean Desimal Ke Biner." },
  { "en": "Berapa Bit Yang Digunakan 'BCD'?", "id": "Empat Bit Untuk Setiap Digit Desimal." },
  { "en": "Apa Itu 'Gray Code'?", "id": "Kode Biner Yang Hanya Ubah Satu Bit." },
  { "en": "Apa Keuntungan 'Gray Code'?", "id": "Mengurangi Kesalahan Pada Transisi." },
  { "en": "Apa Itu 'ASCII (American Standard Code for Information Interchange)'?", "id": "Kode Standar Untuk Huruf Dan Simbol." },
  { "en": "Apa Itu 'Parity Bit'?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
  { "en": "Bagaimana 'Parity Bit' Bekerja?", "id": "Menjaga Jumlah Bit 1 Ganjil/Genap." },
  { "en": "Apa Itu 'Karnaugh Map (K-Map)'?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu 'Grup' Dalam K-Map?", "id": "Pengelompokan Angka 1 Yang Berdekatan." },
  { "en": "Apa Itu 'Don't Care'?", "id": "Kondisi Output Yang Tidak Relevan." },
  { "en": "Apa Itu 'Rangkaian Kombinasional'?", "id": "Outputnya Hanya Tergantung Input Saat Ini." },
  { "en": "Contoh 'Rangkaian Kombinasional'?", "id": "Encoder, Decoder, Multiplexer, Adder." },
  { "en": "Apa Itu 'Rangkaian Sekuensial'?", "id": "Outputnya Tergantung Input Dan Keadaan." },
  { "en": "Contoh 'Rangkaian Sekuensial'?", "id": "Flip-Flop, Counter, Register." },
  { "en": "Apa Itu 'Flip-Flop'?", "id": "Elemen Memori Yang Menyimpan Satu Bit." },
  { "en": "Apa Tipe-tipe 'Flip-Flop'?", "id": "SR, JK, D, Dan T." },
  { "en": "Apa Itu 'Latch'?", "id": "Mirip Flip-Flop, Tapi Level-sensitive." },
  { "en": "Apa Pemicu 'Latch'?", "id": "Perubahan Level Sinyal Clock (Tinggi/Rendah)." },
  { "en": "Apa Pemicu 'Flip-Flop'?", "id": "Perubahan Tepi Sinyal Clock." },
  { "en": "Apa Itu 'Register'?", "id": "Sekelompok Flip-Flop Untuk Menyimpan Data." },
  { "en": "Apa Itu 'Counter'?", "id": "Rangkaian Yang Menghitung Urutan Biner." },
  { "en": "Apa Itu 'Multiplexer (MUX)'?", "id": "Memilih Satu Input Dari Banyak." },
  { "en": "Apa Itu 'Demultiplexer (DEMUX)'?", "id": "Mengirim Satu Input Ke Banyak Output." },
  { "en": "Apa Itu 'Encoder'?", "id": "Mengubah Sinyal Input Menjadi Kode." },
  { "en": "Apa Itu 'Decoder'?", "id": "Mengubah Kode Menjadi Sinyal Output." },
  { "en": "Apa Itu 'Adder'?", "id": "Rangkaian Untuk Melakukan Penjumlahan Biner." },
  { "en": "Apa Itu 'Subtractor'?", "id": "Rangkaian Untuk Melakukan Pengurangan Biner." },
  { "en": "Apa Itu 'ALU (Arithmetic Logic Unit)'?", "id": "Bagian CPU Untuk Operasi Aritmetika-Logika." },
  { "en": "Apa Itu 'State Machine'?", "id": "Model Desain Untuk Sistem Sekuensial." },
  { "en": "Apa Itu 'Diagram State'?", "id": "Representasi Visual Dari State Machine." },
  { "en": "Apa Itu 'Memori RAM (Random-Access Memory)'?", "id": "Memori Yang Dapat Dibaca Dan Ditulis." },
  { "en": "Apa Sifat 'RAM (Random-Access Memory)'?", "id": "Volatil, Data Hilang Tanpa Listrik." },
  { "en": "Apa Itu 'Memori ROM (Read-Only Memory)'?", "id": "Memori Yang Hanya Dapat Dibaca." },
  { "en": "Apa Sifat 'ROM (Read-Only Memory)'?", "id": "Non-Volatil, Data Tetap Tanpa Listrik." },
  { "en": "Apa Itu 'PLD (Programmable Logic Device)'?", "id": "Perangkat Logika Yang Dapat Dikonfigurasi." },
  { "en": "Apa Itu 'FPGA (Field-Programmable Gate Array)'?", "id": "IC (Integrated Circuit) Dengan Gerbang Terprogram." },
  { "en": "Apa Itu 'HDL (Hardware Description Language)'?", "id": "Bahasa Untuk Mendeskripsikan Sirkuit Digital." },
  { "en": "Sebutkan Contoh 'HDL'?", "id": "VHDL Dan Verilog." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Menguji Desain Sirkuit Di Komputer." },
  { "en": "Apa Itu 'Sintesis'?", "id": "Mengubah Kode HDL Menjadi Gerbang Logika." },
  { "en": "Apa Itu 'Timing Diagram'?", "id": "Grafik Yang Menunjukkan Sinyal Digital." },
  { "en": "Apa Itu 'Propagation Delay'?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Input Stabil Sebelum Clock." },
  { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Input Stabil Setelah Clock." },
  { "en": "Apa Itu 'Metastability'?", "id": "Keadaan Output Yang Tidak Menentu." },
  { "en": "Apa Itu 'Clock Skew'?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu 'Clock Jitter'?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Itu 'Sistem Sinkron'?", "id": "Sistem Yang Bekerja Dengan Satu Clock." },
  { "en": "Apa Itu 'Sistem Asinkron'?", "id": "Sistem Tanpa Clock Terpusat." },
  { "en": "Apa Itu 'ADC (Analog-to-Digital Converter)'?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu 'DAC (Digital-to-Analog Converter)'?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu 'Resolusi'?", "id": "Jumlah Bit Dalam Representasi Digital." },
  { "en": "Apa Itu 'Sampling Rate'?", "id": "Frekuensi Pengambilan Sampel Sinyal Analog." },
  { "en": "Apa Itu 'Antarmuka (Interface)'?", "id": "Titik Hubung Antara Dua Komponen." },
  { "en": "Apa Itu 'Bus'?", "id": "Jalur Komunikasi Bersama." },
  { "en": "Apa Itu 'Protokol'?", "id": "Aturan Untuk Komunikasi Data." },
  { "en": "Apa Itu 'Komunikasi Serial'?", "id": "Transfer Data Satu Bit Sekaligus." },
  { "en": "Apa Itu 'Komunikasi Paralel'?", "id": "Transfer Beberapa Bit Secara Bersamaan." },
  { "en": "Apa Itu 'Tri-State Logic'?", "id": "Logika Dengan Keadaan Impedansi Tinggi." },
  { "en": "Apa Fungsi Keadaan 'Impedansi Tinggi'?", "id": "Memungkinkan Beberapa Perangkat Berbagi Bus." },
  { "en": "Apa Itu 'Pull-up Resistor'?", "id": "Resistor Yang Menarik Sinyal Ke Logika High." },
  { "en": "Apa Itu 'Pull-down Resistor'?", "id": "Resistor Yang Menarik Sinyal Ke Logika Low." },
  { "en": "Apa Itu 'Keluaran Open-Drain'?", "id": "Output Yang Hanya Bisa Menarik Ke Low." },
  { "en": "Apa Itu 'Keluaran Open-Collector'?", "id": "Serupa Open-Drain, Tapi Untuk Logika Bipolar." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Grup Gerbang Berdasarkan Teknologi Pembuatan." },
  { "en": "Sebutkan Contoh 'Keluarga Logika'?", "id": "TTL (Transistor-Transistor Logic), CMOS (Complementary MOS)." },
  { "en": "Apa Itu 'Noise Margin'?", "id": "Toleransi Terhadap Gangguan Sinyal." },
  { "en": "Apa Itu 'Fan-out'?", "id": "Jumlah Input Yang Bisa Digerakkan." },
  { "en": "Apa Itu 'Fan-in'?", "id": "Jumlah Input Pada Sebuah Gerbang." },
  { "en": "Apa Itu 'Floating Input'?", "id": "Input Yang Tidak Terhubung." },
  { "en": "Apa Bahaya 'Floating Input'?", "id": "Menyebabkan Perilaku Rangkaian Tak Terduga." },
  { "en": "Apa Itu 'Debouncing'?", "id": "Menghilangkan Pantulan Dari Saklar Mekanis." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Input Dengan Histeresis Untuk Sinyal Bising." },
  { "en": "Apa Itu 'Sistem Tertanam (Embedded System)'?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Itu 'Mikroprosesor'?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu 'Mikrokontroler'?", "id": "Komputer Kecil Lengkap Dalam Satu Chip." },
  { "en": "Apa Beda 'Mikroprosesor' Dan 'Mikrokontroler'?", "id": "Mikrokontroler Punya Memori Dan Periferal." },
  { "en": "Apa Itu 'Arsitektur Von Neumann'?", "id": "Memori Bersama Untuk Kode Dan Data." },
  { "en": "Apa Itu 'Arsitektur Harvard'?", "id": "Memori Terpisah Untuk Kode Dan Data." },
  { "en": "Apa Itu 'Pipelining'?", "id": "Teknik Untuk Mempercepat Eksekusi Instruksi." },
  { "en": "Apa Itu 'Cache'?", "id": "Memori Kecil Cepat Untuk Data Sering." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Yang Tertanam Di ROM." },
  { "en": "Apa Itu 'Sirkuit Aritmetika-Logika'?", "id": "Nama Lain Dari ALU." },
  { "en": "Apa Itu 'Shift Register'?", "id": "Register Yang Dapat Menggeser Data." },
  { "en": "Apa Kegunaan 'Shift Register'?", "id": "Konversi Antara Data Serial Dan Paralel." },
  { "en": "Apa Itu 'Ring Counter'?", "id": "Shift Register Dengan Umpan Balik." },
  { "en": "Apa Itu 'Mesin Moore'?", "id": "Output Hanya Tergantung Keadaan (State)." },
  { "en": "Apa Itu 'Mesin Mealy'?", "id": "Output Tergantung Keadaan Dan Input." },
  { "en": "Apa Itu 'Sirkuit Terpadu (IC)'?", "id": "Banyak Komponen Dalam Satu Chip Silikon." },
  { "en": "Apa Itu 'Wafer'?", "id": "Lembaran Tipis Silikon Untuk Membuat IC." },
  { "en": "Apa Itu 'Die'?", "id": "Satu Potongan Chip Dari Wafer." },
  { "en": "Apa Itu 'Fotolitografi'?", "id": "Proses Mencetak Pola Sirkuit." },
  { "en": "Apa Itu 'Etching'?", "id": "Proses Menghilangkan Material." },
  { "en": "Apa Itu 'Doping'?", "id": "Menambahkan Impuritas Untuk Mengubah Sifat." },
  { "en": "Apa Itu 'Gerbang Universal'?", "id": "Gerbang (NAND/NOR) Yang Bisa Buat Gerbang Lain." },
  { "en": "Apa Itu 'Buffer Tri-State'?", "id": "Buffer Dengan Keadaan Impedansi Tinggi." },
  { "en": "Apa Beda 'Encoder' Dan 'Multiplexer'?", "id": "Encoder Ubah Ke Kode, Multiplexer Pilih Jalur." },
  { "en": "Apa Beda 'Decoder' Dan 'Demultiplexer'?", "id": "Decoder Aktifkan Output, Demultiplexer Salurkan Data." },
  { "en": "Apa Beda 'Full Adder' Dan 'Half Adder'?", "id": "Full Adder Memproses Input Carry." },
  { "en": "Apa Hubungan 'Flip-Flop' Dan 'Register'?", "id": "Register Adalah Kumpulan Dari Flip-Flop." },
  { "en": "Apa Tujuan Sinyal 'Clock'?", "id": "Mensinkronkan Operasi Rangkaian Sekuensial." },
  { "en": "Apa Output 'Half Adder' Selain 'Sum'?", "id": "Carry-Out." },
  { "en": "Berapa Jalur Pilihan 'MUX (Multiplexer) 4-ke-1'?", "id": "Dua Jalur Pilihan." },
  { "en": "Apa Keadaan Awal 'Register' Saat Dinyalakan?", "id": "Tidak Menentu Atau Acak." },
  { "en": "Apa Itu 'Hukum Komutatif' Boolean?", "id": "Urutan Input Tidak Mempengaruhi Hasil." },
  { "en": "Apa Itu 'Hukum Asosiatif' Boolean?", "id": "Pengelompokan Input Tidak Mempengaruhi Hasil." },
  { "en": "Apa Itu 'Hukum Distributif' Boolean?", "id": "Menjelaskan Hubungan Operasi AND Dan OR." },
  { "en": "Apa Kepanjangan 'DIP (Dual In-line Package)'?", "id": "Paket Dual In-line." },
  { "en": "Apa Ciri Khas Paket 'DIP'?", "id": "Dua Baris Pin Paralel." },
  { "en": "Apa Kepanjangan 'SOP (Small Outline Package)'?", "id": "Paket Garis Kecil." },
  { "en": "Apa Itu 'Disipasi Daya Statis'?", "id": "Daya Terkonsumsi Saat Sirkuit Diam." },
  { "en": "Apa Itu 'Disipasi Daya Dinamis'?", "id": "Daya Terkonsumsi Saat Sirkuit Beralih." },
  { "en": "Kapan 'Disipasi Daya Dinamis' Terjadi?", "id": "Saat Transistor Mengisi Dan Mengosongkan Kapasitor." },
  { "en": "Bagaimana Frekuensi Mempengaruhi 'Daya Dinamis'?", "id": "Frekuensi Tinggi, Daya Lebih Tinggi." },
  { "en": "Apa Itu 'Rangkaian Logika Saklar'?", "id": "Model Gerbang Logika Menggunakan Saklar." },
  { "en": "Saklar Apa Yang Merepresentasikan Gerbang 'AND'?", "id": "Dua Saklar Yang Disusun Seri." },
  { "en": "Saklar Apa Yang Merepresentasikan Gerbang 'OR'?", "id": "Dua Saklar Yang Disusun Paralel." },
  { "en": "Apa Itu 'Master-Slave Flip-Flop'?", "id": "Gabungan Dua Latch Untuk Hindari Masalah." },
  { "en": "Apa Masalah Yang Diatasi 'Master-Slave'?", "id": "Masalah Race Condition." },
  { "en": "Apa Itu 'Waktu Propagasi'?", "id": "Nama Lain Tundaan Propagasi." },
  { "en": "Apa Itu 'Sistem Biner Bertanda'?", "id": "Sistem Yang Bisa Representasikan Bilangan Negatif." },
  { "en": "Bagaimana 'Pengurangan Biner' Dilakukan?", "id": "Dengan Penjumlahan Komplemen Dua." },
  { "en": "Apa Itu 'Komplemen Satu (One's Complement)'?", "id": "Membalik Semua Bit (0 Jadi 1)." },
  { "en": "Apa Itu 'Komplemen Dua (Two's Complement)'?", "id": "Komplemen Satu Ditambah Satu." },
  { "en": "Apa Itu 'Carry Propagate Adder'?", "id": "Nama Lain Untuk Ripple Carry Adder." },
  { "en": "Apa Itu 'Carry Generate'?", "id": "Kondisi Yang Pasti Menghasilkan Carry." },
  { "en": "Apa Itu 'Carry Propagate'?", "id": "Kondisi Yang Meneruskan Carry." },
  { "en": "Apa Itu 'Penjumlah BCD (Binary-Coded Decimal)'?", "id": "Penjumlah Khusus Untuk Kode BCD." },
  { "en": "Apa Itu 'Koreksi Desimal'?", "id": "Penyesuaian Hasil Penjumlahan BCD." },
  { "en": "Apa Itu 'Mode Operasi Asinkron'?", "id": "Operasi Yang Tidak Bergantung Pada Clock." },
  { "en": "Apa Itu 'Counter Modulo'?", "id": "Counter Yang Menghitung Hingga Batas Tertentu." },
  { "en": "Apa Itu 'Truncated Counter'?", "id": "Counter Yang Siklusnya Dihentikan Lebih Awal." },
  { "en": "Apa Itu 'Register Universal'?", "id": "Register Yang Bisa Operasi Geser Kiri-Kanan." },
  { "en": "Apa Itu 'Beban Paralel (Parallel Load)'?", "id": "Memasukkan Semua Bit Data Sekaligus." },
  { "en": "Apa Itu 'Gerbang Logika Tiga Dimensi'?", "id": "Konsep Lanjutan Dari Gerbang Logika." },
  { "en": "Apa Itu 'Sirkuit Terintegrasi (IC)'?", "id": "Komponen Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu 'Skala Integrasi'?", "id": "Ukuran Kepadatan Transistor Pada Chip." },
  { "en": "Apa Kepanjangan 'SSI (Small Scale Integration)'?", "id": "Integrasi Skala Kecil." },
  { "en": "Apa Kepanjangan 'MSI (Medium Scale Integration)'?", "id": "Integrasi Skala Menengah." },
  { "en": "Apa Kepanjangan 'LSI (Large Scale Integration)'?", "id": "Integrasi Skala Besar." },
  { "en": "Apa Kepanjangan 'VLSI (Very Large Scale Integration)'?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Apa Itu 'FPGA (Field-Programmable Gate Array)'?", "id": "IC (Integrated Circuit) Logika Terprogram." },
  { "en": "Apa Itu 'Blok Logika' Dalam FPGA?", "id": "Unit Fungsional Dasar Yang Dikonfigurasi." },
  { "en": "Apa Itu 'Look-Up Table (LUT)'?", "id": "Memori Kecil Implementasi Fungsi Logika." },
  { "en": "Apa Itu 'Matriks Interkoneksi'?", "id": "Jaringan Jalur Penghubung Dalam FPGA." },
  { "en": "Apa Itu 'Bitstream' Konfigurasi?", "id": "Data Untuk Memprogram FPGA." },
  { "en": "Apa Itu 'Memori Konfigurasi'?", "id": "Memori Penyimpan Bitstream Di FPGA." },
  { "en": "Apakah Konfigurasi 'FPGA' Volatil?", "id": "Ya, Biasanya Berbasis SRAM (Static RAM)." },
  { "en": "Apa Itu 'Bootloader' FPGA?", "id": "Sirkuit Yang Memuat Konfigurasi." },
  { "en": "Apa Itu 'Logika Kombinatorial'?", "id": "Nama Lain Untuk Rangkaian Kombinasional." },
  { "en": "Apa Itu 'Logika Sekuensial'?", "id": "Nama Lain Untuk Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Clock Gating'?", "id": "Teknik Menghemat Daya Dengan Matikan Clock." },
  { "en": "Apa Itu 'Power Gating'?", "id": "Teknik Menghemat Daya Dengan Matikan Suplai." },
  { "en": "Apa Itu 'Sintesis Tingkat Tinggi (HLS)'?", "id": "Membuat Desain Digital Dari Bahasa C/C++." },
  { "en": "Apa Itu 'Verifikasi Formal'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Itu 'Clock Tree Synthesis (CTS)'?", "id": "Proses Membangun Jaringan Distribusi Clock." },
  { "en": "Tujuan 'CTS (Clock Tree Synthesis)'?", "id": "Meminimalkan Skew Dan Jitter." },
  { "en": "Apa Itu 'Sinkronisasi'?", "id": "Proses Penyelarasan Dengan Sinyal Clock." },
  { "en": "Apa Itu 'Sistem Digital Multilevel'?", "id": "Sistem Dengan Lebih Dari Dua Level Logika." },
  { "en": "Apa Itu 'Logika Fuzzy'?", "id": "Logika Yang Menangani Nilai Antara 0-1." },
  { "en": "Apa Itu 'Sirkuit Digital Optik'?", "id": "Sirkuit Yang Memproses Sinyal Cahaya." },
  { "en": "Apa Itu 'Clock Recovery'?", "id": "Mengekstrak Sinyal Clock Dari Data." },
  { "en": "Apa Itu 'Data Encoding'?", "id": "Mengubah Data Menjadi Format Transmisi." },
  { "en": "Apa Itu 'Kode Manchester'?", "id": "Encoding Yang Menggabungkan Clock Dan Data." },
  { "en": "Apa Itu 'Transceiver'?", "id": "Perangkat Pengirim (Transmitter) Dan Penerima (Receiver)." },
  { "en": "Apa Itu 'Sistem Toleran Kesalahan'?", "id": "Sistem Yang Tetap Bekerja Meski Ada Gagal." },
  { "en": "Apa Itu 'Redundansi'?", "id": "Menyediakan Komponen Cadangan." },
  { "en": "Apa Itu 'Redundansi Perangkat Keras'?", "id": "Menggunakan Komponen Fisik Cadangan." },
  { "en": "Apa Itu 'Redundansi Informasi'?", "id": "Menambahkan Bit Ekstra (Seperti Paritas)." },
  { "en": "Apa Itu 'Voting Logic'?", "id": "Mengambil Keputusan Berdasarkan Mayoritas." },
  { "en": "Apa Itu 'Sistem Failsafe'?", "id": "Sistem Yang Gagal Dalam Keadaan Aman." },
  { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Saat Enable." },
  { "en": "Apa Itu 'Pengalamatan Memori'?", "id": "Metode Untuk Memilih Lokasi Memori." },
  { "en": "Apa Itu 'Peta Memori'?", "id": "Alokasi Alamat Untuk Perangkat Berbeda." },
  { "en": "Apa Itu 'Waktu Siklus (Cycle Time)'?", "id": "Periode Sinyal Clock." },
  { "en": "Apa Itu 'Laju Transfer Data'?", "id": "Jumlah Data Yang Dipindahkan Per Detik." },
  { "en": "Apa Satuan 'Laju Transfer Data'?", "id": "Bit Per Detik (bps)." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Kapasitas Maksimum Laju Transfer Data." },
  { "en": "Apa Itu 'Latensi'?", "id": "Waktu Tunda Awal Transfer Data." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Berbagi Satu Saluran Untuk Banyak Sinyal." },
  { "en": "Apa Itu 'Demultiplexing'?", "id": "Memisahkan Sinyal Gabungan Kembali." },
  { "en": "Apa Itu 'FDM (Frequency Division Multiplexing)'?", "id": "Berbagi Saluran Berdasarkan Frekuensi." },
  { "en": "Apa Itu 'TDM (Time Division Multiplexing)'?", "id": "Berbagi Saluran Berdasarkan Waktu." },
  { "en": "Apa Itu 'Antarmuka Digital'?", "id": "Titik Sambungan Antara Perangkat Digital." },
  { "en": "Contoh 'Antarmuka Digital'?", "id": "USB (Universal Serial Bus), HDMI, SPI, I2C." },
  { "en": "Apa Kepanjangan 'USB (Universal Serial Bus)'?", "id": "Bus Serial Universal." },
  { "en": "Apa Itu 'Sirkuit Digital Kustom'?", "id": "Sirkuit Yang Didesain Untuk Tugas Tertentu." },
  { "en": "Apa Itu 'ASIC (Application-Specific Integrated Circuit)'?", "id": "Contoh Sirkuit Digital Kustom." },
  { "en": "Apa Itu 'Prosesor Sinyal Digital (DSP)'?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu 'Siklus Tugas 50%'?", "id": "Waktu Sinyal Tinggi Dan Rendah Sama." },
  { "en": "Apa Itu 'Rangkaian Logika Resistor-Transistor (RTL)'?", "id": "Keluarga Logika Awal Menggunakan Resistor-Transistor." },
  { "en": "Apa Itu 'Rangkaian Logika Dioda-Transistor (DTL)'?", "id": "Keluarga Logika Sebelum TTL." },
  { "en": "Apa Itu 'Rangkaian Logika Emitter-Coupled (ECL)'?", "id": "Keluarga Logika Bipolar Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu 'Active Low'?", "id": "Sinyal Aktif Saat Logika Rendah (0)." },
  { "en": "Apa Itu 'Active High'?", "id":"Sinyal Aktif Saat Logika Tinggi (1)." },
  { "en": "Apa Beda 'Encoder' Dan 'Decoder'?", "id": "Encoder Membuat Kode, Decoder Menerjemahkannya." },
  { "en": "Apa Beda 'Multiplexer' Dan 'Demultiplexer'?", "id": "Multiplexer Menggabungkan, Demultiplexer Memisahkan." },
  { "en": "Apa Beda 'SRAM (Static RAM)' Dan 'DRAM (Dynamic RAM)'?", "id": "SRAM (Static RAM) Flip-Flop, DRAM (Dynamic RAM) Kapasitor." },
  { "en": "Apa Beda 'RAM (Random-Access Memory)' Dan 'ROM (Read-Only Memory)'?", "id": "RAM (Random-Access Memory) Volatil, ROM (Read-Only Memory) Non-Volatil." },
  { "en": "Apa Beda 'Counter Sinkron' Dan 'Asinkron'?", "id": "Sinkron Clock Bersamaan, Asinkron Berurutan." },
  { "en": "Apa Fungsi 'Unit Kontrol' Dalam CPU?", "id": "Mengatur Semua Operasi Prosesor." },
  { "en": "Apa Tujuan 'Register Akumulator'?", "id": "Menyimpan Hasil Operasi ALU Sementara." },
  { "en": "Mengapa 'Buffer' Digunakan Dalam Sirkuit?", "id": "Untuk Menguatkan Arus Sinyal Digital." },
  { "en": "Apa Yang Menyebabkan 'Ground Bounce'?", "id": "Perubahan Arus Cepat Pada Jalur Ground." },
  { "en": "Bagaimana Cara Mengubah 'Biner' Ke 'Desimal'?", "id": "Jumlahkan Pangkat Dari Dua." },
  { "en": "Apa Keuntungan Utama 'Kode Gray'?", "id": "Hanya Satu Bit Berubah Setiap Langkah." },
  { "en": "Apa Itu 'Paritas Ganjil'?", "id": "Jumlah Total Bit 1 Selalu Ganjil." },
  { "en": "Apa Itu 'Alamat Memori'?", "id": "Lokasi Unik Untuk Setiap Data." },
  { "en": "Apa Itu 'Siklus Tulis' Memori?", "id": "Urutan Sinyal Untuk Menulis Data." },
  { "en": "Apa Itu 'Siklus Baca' Memori?", "id": "Urutan Sinyal Untuk Membaca Data." },
  { "en": "Apa Perbedaan Antara 'Volatil' Dan 'Non-Volatil'?", "id": "Volatil Butuh Daya Untuk Simpan Data." },
  { "en": "Apa Itu 'Paralelisme Data'?", "id": "Memproses Banyak Data Secara Bersamaan." },
  { "en": "Apa Itu 'Paralelisme Instruksi'?", "id": "Mengeksekusi Banyak Instruksi Secara Bersamaan." },
  { "en": "Apa Itu 'Look-Up Table (LUT)'?", "id": "Memori Kecil Implementasi Fungsi Logika." },
  { "en": "Apa Fungsi 'LUT' Dalam FPGA?", "id": "Sebagai Blok Logika Dasar Yang Diprogram." },
  { "en": "Apa Itu 'Jalur Kritis (Critical Path)'?", "id": "Jalur Sinyal Dengan Penundaan Terlama." },
  { "en": "Mengapa 'Jalur Kritis' Penting?", "id": "Menentukan Frekuensi Clock Maksimum." },
  { "en": "Apa Itu 'Timing Violation'?", "id": "Pelanggaran Aturan Waktu Setup Atau Hold." },
  { "en": "Apa Akibat 'Timing Violation'?", "id": "Dapat Menyebabkan Kesalahan Logika." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Logika Kombinasi Kompleks." },
  { "en": "Apa Itu 'Level Abstraksi'?", "id": "Tingkat Detail Dalam Desain Digital." },
  { "en": "Urutkan Abstraksi Dari Tertinggi Ke Terendah?", "id": "Sistem, Arsitektur, RTL, Gerbang, Transistor." },
  { "en": "Apa Itu 'Level Transistor'?", "id": "Desain Berbasis Saklar Transistor." },
  { "en": "Apa Itu 'Level Gerbang'?", "id": "Desain Berbasis Gerbang Logika." },
  { "en": "Apa Itu 'Level RTL (Register-Transfer Level)'?", "id": "Desain Berbasis Aliran Data Register." },
  { "en": "Apa Itu 'Level Arsitektur'?", "id": "Desain Set Instruksi Dan Organisasi." },
  { "en": "Apa Itu 'Sistem' Dalam Desain Digital?", "id": "Kumpulan Komponen Digital Yang Bekerja Sama." },
  { "en": "Apa Itu 'Power-On Reset (POR)'?", "id": "Sirkuit Yang Mereset Sistem Saat Dinyalakan." },
  { "en": "Apa Tujuan 'Sirkuit POR'?", "id": "Memastikan Sistem Mulai Dari Keadaan Dikenal." },
  { "en": "Apa Itu 'Brown-Out Detector (BOD)'?", "id": "Mendeteksi Jika Tegangan Suplai Turun." },
  { "en": "Apa Itu 'Watchdog Timer'?", "id": "Timer Yang Mereset Jika Sistem Macet." },
  { "en": "Apa Itu 'Sistem Pada Chip (SoC)'?", "id": "Seluruh Sistem Dalam Satu Sirkuit Terpadu." },
  { "en": "Apa Kepanjangan 'SoC (System on Chip)'?", "id": "Sistem Dalam Sebuah Chip." },
  { "en": "Apa Saja Komponen 'SoC'?", "id": "CPU (Central Processing Unit), Memori, Periferal." },
  { "en": "Apa Itu 'Inti IP (Intellectual Property)'?", "id": "Blok Desain Siap Pakai Berlisensi." },
  { "en": "Apa Itu 'Hard Core' IP?", "id": "Blok IP (Intellectual Property) Berupa Layout Fisik." },
  { "en": "Apa Itu 'Soft Core' IP?", "id": "Blok IP (Intellectual Property) Berupa Kode HDL." },
  { "en": "Apa Itu 'Bus On-Chip'?", "id": "Jalur Komunikasi Internal Di Dalam Chip." },
  { "en": "Apa Itu 'Sistem Operasi Waktu Nyata (RTOS)'?", "id": "OS (Operating System) Dengan Respons Waktu Terjamin." },
  { "en": "Apa Itu 'Finite Wordlength Effect'?", "id": "Kesalahan Akibat Representasi Biner Terbatas." },
  { "en": "Apa Itu 'Rounding Error'?", "id": "Kesalahan Akibat Pembulatan Angka." },
  { "en": "Apa Itu 'Truncation Error'?", "id": "Kesalahan Akibat Memotong Angka." },
  { "en": "Apa Itu 'Aritmetika Titik Tetap (Fixed-Point)'?", "id": "Representasi Angka Dengan Posisi Koma Tetap." },
  { "en": "Apa Itu 'Aritmetika Titik Mengambang (Floating-Point)'?", "id": "Representasi Angka Dengan Posisi Koma Mengambang." },
  { "en": "Mana Yang Lebih Cepat, 'Fixed-Point' Atau 'Floating-Point'?", "id": "Fixed-Point Umumnya Lebih Cepat." },
  { "en": "Apa Itu 'Digital Signal Processor (DSP)'?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Operasi Apa Yang Dioptimalkan 'DSP'?", "id": "Operasi MAC (Multiply-Accumulate)." },
  { "en": "Apa Itu 'Saturasi Aritmetika'?", "id": "Membatasi Hasil Ke Nilai Maksimum/Minimum." },
  { "en": "Apa Itu 'Filter Digital'?", "id": "Memproses Sinyal Digital Untuk Ubah Frekuensi." },
  { "en": "Apa Itu 'Filter FIR (Finite Impulse Response)'?", "id": "Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Itu 'Filter IIR (Infinite Impulse Response)'?", "id": "Filter Digital Dengan Umpan Balik." },
  { "en": "Apa Keuntungan 'Filter FIR'?", "id": "Selalu Stabil Dan Punya Fasa Linier." },
  { "en": "Apa Keuntungan 'Filter IIR'?", "id": "Lebih Efisien Secara Komputasi." },
  { "en": "Apa Itu 'Downsampling'?", "id": "Mengurangi Laju Sampling Sinyal." },
  { "en": "Apa Itu 'Upsampling'?", "id": "Meningkatkan Laju Sampling Sinyal." },
  { "en": "Apa Itu 'Aliasing'?", "id": "Distorsi Akibat Laju Sampling Rendah." },
  { "en": "Apa Itu 'Teorema Sampling Nyquist'?", "id": "Laju Sampling Harus Dua Kali Bandwidth." },
  { "en": "Apa Itu 'Binary Phase Shift Keying (BPSK)'?", "id": "Modulasi Digital Mengubah Fasa Biner." },
  { "en": "Apa Itu 'Error Detection Code'?", "id": "Kode Untuk Mendeteksi Kesalahan Transmisi." },
  { "en": "Apa Itu 'Error Correction Code'?", "id": "Kode Untuk Memperbaiki Kesalahan Transmisi." },
  { "en": "Apa Itu 'Kode Hamming'?", "id": "Contoh Sederhana Kode Koreksi Kesalahan." },
  { "en": "Apa Itu 'Jarak Hamming'?", "id": "Jumlah Posisi Bit Yang Berbeda." },
  { "en": "Apa Itu 'Sinkronisasi' Data?", "id": "Memastikan Pengirim Dan Penerima Sejajar." },
  { "en": "Apa Itu 'Bit Start'?", "id": "Bit Penanda Awal Transmisi Asinkron." },
  { "en": "Apa Itu 'Bit Stop'?", "id": "Bit Penanda Akhir Transmisi Asinkron." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Jumlah Perubahan Simbol Per Detik." },
  { "en": "Apa Beda 'Baud Rate' Dan 'Bit Rate'?", "id": "Tidak Selalu Sama, Tergantung Modulasi." },
  { "en": "Apa Itu 'Komunikasi Simplex'?", "id": "Komunikasi Satu Arah Saja." },
  { "en": "Apa Itu 'Komunikasi Half-Duplex'?", "id": "Komunikasi Dua Arah Secara Bergantian." },
  { "en": "Apa Itu 'Komunikasi Full-Duplex'?", "id": "Komunikasi Dua Arah Secara Bersamaan." },
  { "en": "Apa Itu 'Latensi'?", "id": "Waktu Tunda Dalam Transmisi Data." },
  { "en": "Apa Itu 'Throughput'?", "id": "Laju Transfer Data Efektif." },
  { "en": "Apa Itu 'Protokol Jaringan'?", "id": "Aturan Komunikasi Antar Perangkat Jaringan." },
  { "en": "Apa Itu 'Paket' Data?", "id": "Blok Data Yang Dikirim Melalui Jaringan." },
  { "en": "Apa Itu 'Header' Paket?", "id": "Informasi Kontrol Di Awal Paket." },
  { "en": "Apa Itu 'Payload' Paket?", "id": "Data Sebenarnya Yang Dibawa Paket." },
  { "en": "Apa Itu 'Checksum'?", "id": "Nilai Untuk Verifikasi Integritas Data." },
  { "en": "Apa Itu 'CRC (Cyclic Redundancy Check)'?", "id": "Metode Deteksi Kesalahan Yang Kuat." },
  { "en": "Apa Itu 'Sistem File'?", "id": "Cara Mengorganisir Data Di Penyimpanan." },
  { "en": "Apa Itu 'Booting'?", "id": "Proses Memulai Sistem Komputer." },
  { "en": "Apa Itu 'Binary Logic'?", "id": "Logika Yang Hanya Memiliki Dua Keadaan." },
  { "en": "Apa Itu 'Logic Circuit'?", "id": "Nama Lain Untuk Rangkaian Logika." },
  { "en": "Apa Itu 'Sequential Circuit'?", "id": "Nama Lain Untuk Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Combinational Circuit'?", "id": "Nama Lain Untuk Rangkaian Kombinasional." },
  { "en": "Apa Itu 'Boolean Expression'?", "id": "Nama Lain Untuk Ekspresi Boolean." },
  { "en": "Apa Itu 'Logic Diagram'?", "id": "Representasi Grafis Rangkaian Dengan Gerbang." },
  { "en": "Apa Itu 'Karnaugh Map'?", "id": "Alat Bantu Visual Penyederhanaan Logika." },
  { "en": "Apa Itu 'Digital System Design'?", "id": "Proses Merancang Sistem Berbasis Logika Digital." },
  { "en": "Apa Fungsi Jalur 'Select' Pada MUX?", "id": "Memilih Jalur Input Yang Akan Dilewatkan." },
  { "en": "Apa Yang Terjadi Saat 'Counter' Mencapai Batasnya?", "id": "Akan Kembali Ke Hitungan Awal (Reset)." },
  { "en": "Bagaimana Data Dimasukkan Ke 'Register Beban Paralel'?", "id": "Semua Bit Dimasukkan Secara Bersamaan." },
  { "en": "Apa Beda 'Aljabar Boolean' Dan 'Gerbang Logika'?", "id": "Aljabar Teorinya, Gerbang Implementasinya." },
  { "en": "Apa Hubungan 'Tabel Kebenaran' Dan 'Ekspresi Boolean'?", "id": "Keduanya Merepresentasikan Fungsi Logika Yang Sama." },
  { "en": "Apa Itu 'Reset Sinkron'?", "id": "Reset Yang Hanya Aktif Pada Tepi Clock." },
  { "en": "Apa Itu 'Reset Asinkron'?", "id": "Reset Yang Bekerja Kapan Saja." },
  { "en": "Mana Yang Lebih Cepat, 'Reset Sinkron' Atau 'Asinkron'?", "id": "Reset Asinkron Bekerja Lebih Cepat." },
  { "en": "Apa Itu 'Pengkodean One-Hot'?", "id": "Hanya Satu Bit Aktif (Logika 1)." },
  { "en": "Apa Beda 'One-Hot' Dan 'Pengkodean Biner'?", "id": "One-Hot Sederhana, Biner Lebih Efisien." },
  { "en": "Apa Itu 'Active Low'?", "id": "Sinyal Dianggap Aktif Pada Level Rendah." },
  { "en": "Apa Itu 'Active High'?", "id": "Sinyal Dianggap Aktif Pada Level Tinggi." },
  { "en": "Apa Itu 'Kapasitas Memori'?", "id": "Jumlah Total Bit Yang Dapat Disimpan." },
  { "en": "Apa Itu 'Sel Memori'?", "id": "Unit Terkecil Penyimpan Satu Bit." },
  { "en": "Apa Fungsi 'Pin VCC' Pada IC?", "id": "Pin Untuk Suplai Tegangan Positif." },
  { "en": "Apa Fungsi 'Pin GND' Pada IC?", "id": "Pin Untuk Koneksi Ke Ground (Tanah)." },
  { "en": "Apa Itu 'Pinout'?", "id": "Deskripsi Fungsi Setiap Pin Pada IC." },
  { "en": "Apa Itu 'Paket DIP (Dual In-line Package)'?", "id": "Paket IC (Integrated Circuit) Dengan Dua Baris Pin." },
  { "en": "Apa Itu 'Paket SOP (Small Outline Package)'?", "id": "Paket IC (Integrated Circuit) Untuk Pemasangan Permukaan." },
  { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Saat Sinyal Enable Aktif." },
  { "en": "Apa Itu 'Clock Enable'?", "id": "Sinyal Untuk Mengizinkan Clock Bekerja." },
  { "en": "Apa Itu 'Siklus Mesin (Machine Cycle)'?", "id": "Operasi Paling Dasar CPU (Central Processing Unit)." },
  { "en": "Apa Itu 'Operand'?", "id": "Data Yang Dioperasikan Oleh Instruksi." },
  { "en": "Apa Itu 'Opcode (Operation Code)'?", "id": "Bagian Instruksi Yang Menentukan Operasi." },
  { "en": "Apa Itu 'Arsitektur Set Instruksi (ISA)'?", "id": "Kumpulan Instruksi Yang Dipahami Prosesor." },
  { "en": "Apa Itu 'Logika Tiga Keadaan (Tri-State)'?", "id": "Logika Dengan Keadaan High, Low, High-Z." },
  { "en": "Apa Kepanjangan 'High-Z'?", "id": "Impedansi Tinggi (High Impedance)." },
  { "en": "Apa Itu 'Bus Contention'?", "id": "Konflik Saat Dua Output Menggerakkan Bus." },
  { "en": "Bagaimana 'Logika Tri-State' Mencegah Konflik Bus?", "id": "Dengan Menonaktifkan Output (High-Z)." },
  { "en": "Apa Itu 'Gerbang AND Ber-input Tiga'?", "id": "Output 1 Jika Ketiga Inputnya 1." },
  { "en": "Apa Itu 'Don't Care' Dalam Tabel Kebenaran?", "id": "Input Yang Tidak Mempengaruhi Output." },
  { "en": "Apa Simbol Untuk 'Don't Care'?", "id": "Biasanya 'X' Atau '-." },
  { "en": "Apa Itu 'Komplemen' Suatu Bilangan?", "id": "Representasi Negatif Dari Bilangan Biner." },
  { "en": "Apa Itu 'Penjumlah Paralel Biner'?", "id": "Menjumlahkan Semua Bit Secara Bersamaan." },
  { "en": "Apa Itu 'Decoder Biner'?", "id": "Mengubah Input Biner Menjadi Output Tunggal." },
  { "en": "Apa Itu 'Encoder Desimal ke BCD'?", "id": "Mengubah Input Desimal Menjadi Kode BCD." },
  { "en": "Apa Itu 'Shift Left'?", "id": "Menggeser Semua Bit Ke Posisi Kiri." },
  { "en": "Apa Efek Aritmetika 'Shift Left'?", "id": "Sama Dengan Mengalikan Dengan Dua." },
  { "en": "Apa Itu 'Shift Right'?", "id": "Menggeser Semua Bit Ke Posisi Kanan." },
  { "en": "Apa Efek Aritmetika 'Shift Right'?", "id": "Sama Dengan Membagi Dengan Dua." },
  { "en": "Apa Itu 'Rotasi Bit'?", "id": "Menggeser Bit Dengan Umpan Balik." },
  { "en": "Apa Itu 'Volatilitas' Memori?", "id": "Kebutuhan Daya Untuk Menyimpan Data." },
  { "en": "Apa Itu 'Akses Sekuensial'?", "id": "Membaca Data Secara Berurutan." },
  { "en": "Contoh 'Memori Akses Sekuensial'?", "id": "Pita Magnetik." },
  { "en": "Apa Itu 'Akses Acak'?", "id": "Membaca Data Dari Alamat Apapun." },
  { "en": "Apa Itu 'Hirarki Memori'?", "id": "Tingkatan Memori Berdasarkan Kecepatan." },
  { "en": "Urutkan 'Hirarki Memori' Dari Tercepat?", "id": "Register, Cache, RAM, Penyimpanan." },
  { "en": "Apa Itu 'Register File'?", "id": "Kumpulan Register Di Dalam CPU." },
  { "en": "Apa Itu 'Latency' Memori?", "id": "Waktu Tunda Untuk Mengakses Data." },
  { "en": "Apa Itu 'Bandwidth' Memori?", "id": "Laju Transfer Data Dari Memori." },
  { "en": "Apa Itu 'Sel Memori'?", "id": "Unit Fisik Penyimpan Satu Bit." },
  { "en": "Berapa Transistor Dalam 'Sel SRAM'?", "id": "Biasanya Enam Transistor." },
  { "en": "Berapa Komponen Dalam 'Sel DRAM'?", "id": "Satu Transistor Dan Satu Kapasitor." },
  { "en": "Apa Itu 'Array Logika Terprogram (PLA)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND-OR." },
  { "en": "Apa Beda 'PLA' Dan 'PAL'?", "id": "PLA Punya Plane AND Dan OR Terprogram." },
  { "en": "Apa Itu 'Sistem pada Chip (SoC)'?", "id": "Seluruh Sistem Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu 'Sirkuit Terpadu Analog'?", "id": "IC (Integrated Circuit) Untuk Sinyal Analog." },
  { "en": "Contoh 'IC (Integrated Circuit) Analog'?", "id": "Op-Amp (Operational Amplifier), Regulator Tegangan." },
  { "en": "Apa Itu 'Sirkuit Terpadu Sinyal Campuran'?", "id": "IC (Integrated Circuit) Dengan Sirkuit Analog-Digital." },
  { "en": "Contoh 'IC (Integrated Circuit) Sinyal Campuran'?", "id": "ADC (Analog-to-Digital Converter), DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Itu 'Garis Beban (Load Line)'?", "id": "Grafik Arus-Tegangan Untuk Analisis." },
  { "en": "Apa Itu 'Titik Operasi'?", "id": "Titik Kerja Statis Sebuah Transistor." },
  { "en": "Apa Itu 'Batas Logika (Logic Threshold)'?", "id": "Tegangan Peralihan Antara Logika 0/1." },
  { "en": "Apa Itu 'Histeresis'?", "id": "Perbedaan Ambang Batas Naik Dan Turun." },
  { "en": "Apa Fungsi 'Histeresis'?", "id": "Meningkatkan Kekebalan Terhadap Derau." },
  { "en": "Apa Itu 'Komunikasi Asinkron'?", "id": "Komunikasi Tanpa Sinyal Clock Bersama." },
  { "en": "Apa Itu 'Bit Start'?", "id": "Bit Penanda Awal Transmisi Asinkron." },
  { "en": "Apa Itu 'Bit Stop'?", "id": "Bit Penanda Akhir Transmisi Asinkron." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Jumlah Simbol Yang Dikirim Per Detik." },
  { "en": "Apa Itu 'Sinyal Diferensial'?", "id": "Menggunakan Sepasang Kabel Dengan Sinyal Berlawanan." },
  { "en": "Apa Keuntungan 'Sinyal Diferensial'?", "id": "Sangat Tahan Terhadap Gangguan Derau." },
  { "en": "Contoh Antarmuka 'Sinyal Diferensial'?", "id": "RS-485, LVDS (Low-Voltage Differential Signaling)." },
  { "en": "Apa Itu 'Sinyal Single-Ended'?", "id": "Sinyal Diukur Relatif Terhadap Ground." },
  { "en": "Apa Itu 'Boolean Satisfiability (SAT)'?", "id": "Masalah Menemukan Input Untuk Output Benar." },
  { "en": "Apa Itu 'Tautologi'?", "id": "Ekspresi Yang Selalu Benar." },
  { "en": "Apa Itu 'Kontradiksi'?", "id": "Ekspresi Yang Selalu Salah." },
  { "en": "Apa Itu 'Representasi Kanonis'?", "id": "Bentuk Unik Standar Untuk Suatu Fungsi." },
  { "en": "Apa Itu 'Fungsi Incompletely Specified'?", "id": "Fungsi Dengan Beberapa Output Don't Care." },
  { "en": "Apa Itu 'Pengujian Sirkuit'?", "id": "Memastikan Sirkuit Berfungsi Sesuai Desain." },
  { "en": "Apa Itu 'Model Kesalahan (Fault Model)'?", "id": "Asumsi Jenis Kerusakan Fisik." },
  { "en": "Apa Itu 'Stuck-at-0 Fault'?", "id": "Asumsi Jalur Terjebak Di Logika 0." },
  { "en": "Apa Itu 'Stuck-at-1 Fault'?", "id": "Asumsi Jalur Terjebak Di Logika 1." },
  { "en": "Apa Itu 'Test Pattern Generation'?", "id": "Membuat Vektor Uji Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu 'Boundary Scan'?", "id": "Teknik Pengujian Sambungan Antar Chip." },
  { "en": "Apa Itu 'JTAG Port'?", "id": "Port Untuk Akses Boundary Scan." },
  { "en": "Apa Itu 'Sistem Biner Tak Bertanda'?", "id": "Sistem Yang Hanya Mewakili Bilangan Positif." },
  { "en": "Apa Itu 'Ripple Blanking'?", "id": "Mematikan Angka Nol Yang Tidak Perlu." },
  { "en": "Apa Itu 'Seven-Segment Display'?", "id": "Layar Untuk Menampilkan Angka." },
  { "en": "Apa Itu 'Anoda Bersama (Common Anode)'?", "id": "Semua Anoda LED Terhubung Bersama." },
  { "en": "Apa Itu 'Katoda Bersama (Common Cathode)'?", "id": "Semua Katoda LED Terhubung Bersama." },
  { "en": "Apa Itu 'Address Latch'?", "id": "Menyimpan Alamat Untuk Digunakan Nanti." },
  { "en": "Apa Itu 'Data Latch'?", "id": "Menyimpan Data Untuk Digunakan Nanti." },
  { "en": "Apa Itu 'State Reduction'?", "id": "Menyederhanakan Jumlah Keadaan Mesin Keadaan." },
  { "en": "Apa Itu 'Equivalent States'?", "id": "Keadaan Yang Punya Perilaku Sama." },
  { "en": "Apa Itu 'Arsitektur Von Neumann Bottleneck'?", "id": "Keterbatasan Akibat Bus Bersama." },
  { "en": "Mengapa 'Counter Sinkron' Lebih Disukai?", "id": "Karena Tidak Ada Penundaan Propagasi." },
  { "en": "Operasi Logika Apa Yang Dilakukan 'ALU'?", "id": "AND, OR, NOT, XOR." },
  { "en": "Berapa Output Dari 'Encoder 8-ke-3'?", "id": "Tiga Output Biner." },
  { "en": "Bahan Semikonduktor Apa Untuk 'IC'?", "id": "Silikon (Silicon)." },
  { "en": "Apa Itu 'Fotolitografi'?", "id": "Proses Mencetak Pola Sirkuit." },
  { "en": "Berapa 'Level Tegangan CMOS' 5V?", "id": "Rendah 0V, Tinggi 5V." },
  { "en": "Apa Keuntungan 'Keluaran Totem-Pole'?", "id": "Kemampuan Source Dan Sink Arus." },
  { "en": "Apa Itu 'Multivibrator'?", "id": "Sirkuit Yang Menghasilkan Pulsa Digital." },
  { "en": "Apa Itu 'Multivibrator Astabil'?", "id": "Tidak Punya Keadaan Stabil (Osilator)." },
  { "en": "Apa Itu 'Multivibrator Monostabil'?", "id": "Punya Satu Keadaan Stabil (One-Shot)." },
  { "en": "Apa Itu 'Multivibrator Bistabil'?", "id": "Punya Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "Apa Itu 'Alamat Baris (Row Address)' DRAM?", "id": "Alamat Untuk Memilih Baris Memori." },
  { "en": "Apa Itu 'Alamat Kolom (Column Address)' DRAM?", "id": "Alamat Untuk Memilih Kolom Memori." },
  { "en": "Apa Kepanjangan 'RAS (Row Address Strobe)'?", "id": "Strobe Alamat Baris." },
  { "en": "Apa Kepanjangan 'CAS (Column Address Strobe)'?", "id": "Strobe Alamat Kolom." },
  { "en": "Apa Itu 'Latensi CAS'?", "id": "Penundaan Antara Sinyal CAS Dan Data." },
  { "en": "Apa Hubungan 'Bit', 'Byte', Dan 'Word'?", "id": "Word Kumpulan Byte, Byte Kumpulan Bit." },
  { "en": "Bagaimana 'Decoder' Terkait Dengan 'Memori'?", "id": "Decoder Memilih Alamat Memori." },
  { "en": "Bagaimana 'Multiplexer' Terkait Dengan 'Bus'?", "id": "Multiplexer Dapat Memilih Sumber Data Bus." },
  { "en": "Apa Itu 'Floating Point Number'?", "id": "Representasi Bilangan Desimal Biner." },
  { "en": "Apa Itu 'Mantissa'?", "id": "Bagian Angka Signifikan Floating Point." },
  { "en": "Apa Itu 'Exponent'?", "id": "Bagian Pangkat Dari Floating Point." },
  { "en": "Apa Itu 'Gerbang Logika Tiga Input'?", "id": "Gerbang Yang Menerima Tiga Input." },
  { "en": "Apa Itu 'Ekspresi Logika Minimal'?", "id": "Ekspresi Dengan Jumlah Literal Terkecil." },
  { "en": "Apa Itu 'Implicant'?", "id": "Grup Angka 1 Dalam Peta Karnaugh." },
  { "en": "Apa Itu 'Prime Implicant'?", "id": "Implicant Yang Tidak Bisa Diperbesar." },
  { "en": "Apa Itu 'Essential Prime Implicant'?", "id": "Prime Implicant Yang Mencakup Angka 1." },
  { "en": "Apa Itu 'Metode Quine-McCluskey'?", "id": "Metode Tabular Untuk Penyederhanaan Logika." },
  { "en": "Apa Keuntungan 'Metode Quine-McCluskey'?", "id": "Dapat Diotomatisasi Oleh Komputer." },
  { "en": "Apa Itu 'Rangkaian Logika Berjenjang (Multilevel)'?", "id": "Rangkaian Dengan Beberapa Tingkat Gerbang." },
  { "en": "Apa Keuntungan 'Logika Multilevel'?", "id": "Mengurangi Fan-in Dan Penundaan." },
  { "en": "Apa Itu 'Sistem Tertanam (Embedded System)'?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Contoh 'Sistem Tertanam'?", "id": "Microwave, Mesin Cuci, Router." },
  { "en": "Apa Itu 'Sistem Waktu Nyata (Real-Time System)'?", "id": "Sistem Dengan Batasan Waktu Ketat." },
  { "en": "Apa Itu 'Batas Waktu (Deadline)'?", "id": "Batas Waktu Maksimal Untuk Tugas." },
  { "en": "Apa Itu 'Hard Real-Time'?", "id": "Kegagalan Memenuhi Batas Waktu Fatal." },
  { "en": "Apa Itu 'Soft Real-Time'?", "id": "Batas Waktu Terlewat Mengurangi Kualitas." },
  { "en": "Apa Itu 'Watchdog Timer'?", "id": "Timer Pencegah Sistem Macet." },
  { "en": "Bagaimana 'Watchdog Timer' Bekerja?", "id": "Mereset Sistem Jika Tidak Direset." },
  { "en": "Apa Itu 'Gerbang NOT'?", "id": "Nama Lain Untuk Inverter." },
  { "en": "Apa Itu 'Gerbang AND'?", "id": "Gerbang Logika Untuk Operasi Perkalian." },
  { "en": "Apa Itu 'Gerbang OR'?", "id": "Gerbang Logika Untuk Operasi Penjumlahan." },
  { "en": "Apa Itu 'Identitas Boolean'?", "id": "Persamaan Yang Selalu Benar." },
  { "en": "Contoh 'Identitas Boolean'?", "id": "A + 0 = A." },
  { "en": "Apa Itu 'Hukum Idempoten'?", "id": "A + A = A; A * A = A." },
  { "en": "Apa Itu 'Hukum Komplementasi'?", "id": "A + A' = 1." },
  { "en": "Apa Itu 'Hukum Involusi'?", "id": "(A')' = A." },
  { "en": "Apa Itu 'Hukum Penyerapan (Absorption)'?", "id": "A + AB = A." },
  { "en": "Apa Itu 'Sinyal Kontrol'?", "id": "Sinyal Yang Mengatur Operasi Sirkuit." },
  { "en": "Apa Itu 'Jalur Data (Datapath)'?", "id": "Bagian Sirkuit Yang Memproses Data." },
  { "en": "Apa Hubungan 'Unit Kontrol' Dan 'Jalur Data'?", "id": "Unit Kontrol Mengarahkan Jalur Data." },
  { "en": "Apa Itu 'Word' Dalam Komputer?", "id": "Ukuran Bit Alami Prosesor." },
  { "en": "Berapa Bit Satu 'Word'?", "id": "Bisa 16, 32, Atau 64 Bit." },
  { "en": "Apa Itu 'Arsitektur 32-bit'?", "id": "Prosesor Yang Bekerja Dengan Data 32-bit." },
  { "en": "Apa Itu 'Arsitektur 64-bit'?", "id": "Prosesor Yang Bekerja Dengan Data 64-bit." },
  { "en": "Apa Itu 'Memory Address Register (MAR)'?", "id": "Register Penyimpan Alamat Memori." },
  { "en": "Apa Itu 'Memory Data Register (MDR)'?", "id": "Register Buffer Data Dari/Ke Memori." },
  { "en": "Apa Itu 'Counter Sinkron Dapat Diprogram'?", "id": "Counter Yang Bisa Dimulai Dari Nilai Apapun." },
  { "en": "Apa Itu 'Beban Asinkron'?", "id": "Memuat Data Tanpa Menunggu Clock." },
  { "en": "Apa Itu 'Beban Sinkron'?", "id": "Memuat Data Sesuai Dengan Clock." },
  { "en": "Apa Itu 'Decoder 7-Segmen'?", "id": "Mengubah BCD Menjadi Sinyal Layar." },
  { "en": "Apa Itu 'Liquid Crystal Display (LCD)'?", "id": "Layar Digital Berbasis Kristal Cair." },
  { "en": "Apa Itu 'Light Emitting Diode (LED)'?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu 'Rangkaian Logika Kustom'?", "id": "Rangkaian Yang Didesain Untuk Fungsi Unik." },
  { "en": "Apa Itu 'Gerbang Logika Statis'?", "id": "Gerbang Yang Menjaga Outputnya Stabil." },
  { "en": "Apa Itu 'Gerbang Logika Dinamis'?", "id": "Gerbang Yang Gunakan Kapasitor Untuk Logika." },
  { "en": "Apa Keuntungan 'Logika Dinamis'?", "id": "Lebih Cepat Dan Ukuran Lebih Kecil." },
  { "en": "Apa Kerugian 'Logika Dinamis'?", "id": "Lebih Kompleks Dan Butuh Clock." },
  { "en": "Apa Itu 'Charge Sharing'?", "id": "Masalah Dalam Logika Dinamis." },
  { "en": "Apa Itu 'Precharge'?", "id": "Tahap Awal Dalam Operasi Logika Dinamis." },
  { "en": "Apa Itu 'Evaluate'?", "id": "Tahap Kedua Dalam Operasi Logika Dinamis." },
  { "en": "Apa Itu 'Logika Domino'?", "id": "Jenis Logika Dinamis Yang Cepat." },
  { "en": "Apa Itu 'Buffer Inverting'?", "id": "Dua Inverter Disusun Seri." },
  { "en": "Apa Itu 'Sistem Bilangan'?", "id": "Cara Untuk Merepresentasikan Angka." },
  { "en": "Apa Itu 'Basis' Atau 'Radix'?", "id": "Jumlah Simbol Unik Dalam Sistem Bilangan." },
  { "en": "Apa Itu 'Konversi Basis'?", "id": "Mengubah Angka Dari Satu Basis Ke Lainnya." },
  { "en": "Apa Itu 'Desain Digital'?", "id": "Proses Merancang Dan Membangun Sistem Digital." },
  { "en": "Apa Itu 'Sintesis Perilaku (Behavioral)'?", "id": "Sintesis Dari Deskripsi Level Sangat Tinggi." },
  { "en": "Apa Itu 'Abstraksi'?", "id": "Menyembunyikan Detail Kompleksitas." },
  { "en": "Apa Itu 'Hierarki' Dalam Desain?", "id": "Membagi Desain Menjadi Blok-blok Kecil." },
  { "en": "Apa Itu 'Modularitas'?", "id": "Desain Dibangun Dari Modul Independen." },
  { "en": "Apa Keuntungan 'Desain Modular'?", "id": "Mudah Dikelola Dan Digunakan Kembali." },
  { "en": "Apa Itu 'Regularitas'?", "id": "Menggunakan Kembali Blok Yang Sama." },
  { "en": "Apa Itu 'Sistem Biner'?", "id": "Sistem Berbasis Dua Nilai (0 dan 1)." },
  { "en": "Apa Itu 'Sirkuit Digital Terintegrasi'?", "id": "Nama Lain Untuk IC Digital." },
  { "en": "Apa Itu 'Tegangan Suplai Nominal'?", "id": "Tegangan Kerja Yang Direkomendasikan." },
  { "en": "Apa Itu 'Karakteristik Transfer Tegangan'?", "id": "Grafik Vout Terhadap Vin." },
  { "en": "Apa Itu 'Derau (Noise)'?", "id": "Sinyal Tak Diinginkan Pengganggu Logika." },
  { "en": "Apa Itu 'Simbol Logika'?", "id": "Simbol Grafis Untuk Gerbang Logika." },
  { "en": "Apa Itu 'Timing Analysis'?", "id": "Analisis Penundaan Sinyal Dalam Sirkuit." },
  { "en": "Apa Itu 'Verifikasi Fungsional'?", "id": "Memastikan Desain Bekerja Sesuai Fungsi." },
  { "en": "Apa Itu 'Test Case'?", "id": "Satu Set Input Untuk Menguji Fungsi." },
  { "en": "Bagaimana 'FPGA' Menyimpan Konfigurasinya?", "id": "Menggunakan Sel Memori SRAM (Static RAM)." },
  { "en": "Apa Perbedaan 'Mesin Moore' Dan 'Mealy'?", "id": "Output Moore Tergantung State Saja." },
  { "en": "Apa Langkah Pertama Proses 'ADC'?", "id": "Sampling Sinyal Analog." },
  { "en": "Apa Itu 'Current Mirror'?", "id": "Sirkuit Untuk Menyalin Arus Listrik." },
  { "en": "Apa Itu 'Pasangan Diferensial (Differential Pair)'?", "id": "Menguatkan Selisih Antara Dua Sinyal." },
  { "en": "Apa Fungsi 'Pinout Diagram'?", "id": "Menunjukkan Lokasi Dan Fungsi Setiap Pin." },
  { "en": "Apa Fungsi 'Kapasitor Bypass' Dekat IC?", "id": "Menstabilkan Tegangan Suplai Lokal." },
  { "en": "Apa Itu 'Power Distribution Network (PDN)'?", "id": "Jaringan Jalur VCC Dan Ground." },
  { "en": "Apa Itu 'Ground Plane'?", "id": "Lapisan Tembaga Besar Untuk Ground." },
  { "en": "Apa Beda 'Sinyal' Dan 'Variabel' Di VHDL?", "id": "Sinyal Punya Waktu, Variabel Tidak." },
  { "en": "Apa Beda 'Blocking' Dan 'Non-blocking' Di Verilog?", "id": "Blocking Sekuensial, Non-blocking Paralel." },
  { "en": "Apa Itu 'Proses' Di VHDL?", "id": "Blok Kode Sekuensial Yang Sensitif." },
  { "en": "Apa Itu 'Always Block' Di Verilog?", "id": "Blok Kode Yang Dieksekusi Terus-menerus." },
  { "en": "Apa Hubungan 'Frekuensi Clock' Dan 'Periode Clock'?", "id": "Berbanding Terbalik Satu Sama Lain." },
  { "en": "Bagaimana 'Jumlah Bit' Mempengaruhi Jumlah Keadaan?", "id": "N Bit Menghasilkan 2^N Keadaan." },
  { "en": "Apa Hubungan 'Resolusi ADC' Dan 'Error Kuantisasi'?", "id": "Resolusi Tinggi, Error Kuantisasi Kecil." },
  { "en": "Apa Itu 'Bit Sign'?", "id": "Bit Paling Kiri Pada Bilangan Bertanda." },
  { "en": "Angka Berapa Yang Diwakili 'Bit Sign' 0?", "id": "Bilangan Positif." },
  { "en": "Angka Berapa Yang Diwakili 'Bit Sign' 1?", "id": "Bilangan Negatif." },
  { "en": "Apa Itu 'ALU Slice'?", "id": "Satu Bit Dari ALU (Arithmetic Logic Unit) Lengkap." },
  { "en": "Apa Itu 'Lookahead Carry Generator'?", "id": "Sirkuit Khusus Untuk Mempercepat Carry." },
  { "en": "Apa Itu 'Shift-and-Add'?", "id": "Metode Perkalian Biner Secara Serial." },
  { "en": "Apa Itu 'Comparator'?", "id": "Nama Lain Untuk Rangkaian Komparator." },
  { "en": "Apa Itu 'State Minimization'?", "id": "Proses Mengurangi Jumlah Keadaan FSM." },
  { "en": "Mengapa 'State Minimization' Penting?", "id": "Untuk Membuat Rangkaian Lebih Sederhana." },
  { "en": "Apa Itu 'Unused State'?", "id": "Keadaan Yang Tidak Pernah Tercapai." },
  { "en": "Apa Itu 'Counter Sinkron Dengan Beban Paralel'?", "id": "Counter Yang Bisa Dimulai Dari Nilai Awal." },
  { "en": "Apa Itu 'Up/Down Counter'?", "id": "Counter Yang Bisa Menghitung Naik Atau Turun." },
  { "en": "Apa Itu 'BCD Counter'?", "id": "Counter Yang Menghitung Dari 0 Hingga 9." },
  { "en": "Apa Itu 'Register Feedback Geser'?", "id": "Shift Register Dengan Umpan Balik XOR." },
  { "en": "Apa Itu 'Pseudo-Random Number Generator'?", "id": "Rangkaian Penghasil Urutan Angka Tampak Acak." },
  { "en": "Apa Itu 'Arus Suplai Diam (Quiescent)'?", "id": "Arus Yang Dikonsumsi IC Saat Diam." },
  { "en": "Apa Itu 'Slew Rate'?", "id": "Laju Perubahan Maksimum Sinyal Output." },
  { "en": "Apa Itu 'Antarmuka Tiga Kabel (Three-Wire)'?", "id": "Nama Umum Untuk Antarmuka SPI." },
  { "en": "Apa Itu 'Antarmuka Dua Kabel (Two-Wire)'?", "id": "Nama Umum Untuk Antarmuka I2C." },
  { "en": "Apa Itu 'Master' Dalam Komunikasi?", "id": "Perangkat Yang Memulai Komunikasi." },
  { "en": "Apa Itu 'Slave' Dalam Komunikasi?", "id": "Perangkat Yang Merespon Master." },
  { "en": "Bisakah Ada Banyak 'Master' Di Bus I2C?", "id": "Ya, I2C Mendukung Multi-Master." },
  { "en": "Apa Itu 'Clock Stretching'?", "id": "Mekanisme Slave Memperlambat Master." },
  { "en": "Apa Itu 'Acknowledge (ACK)' Bit?", "id": "Sinyal Konfirmasi Penerimaan Data." },
  { "en": "Apa Itu 'Not Acknowledge (NACK)' Bit?", "id": "Sinyal Penolakan Atau Akhir Transmisi." },
  { "en": "Apa Itu 'Bit Rate'?", "id": "Kecepatan Transfer Data Dalam Bit/Detik." },
  { "en": "Apa Itu 'Parity Error'?", "id": "Kesalahan Yang Terdeteksi Oleh Bit Paritas." },
  { "en": "Apa Itu 'Framing Error'?", "id": "Kesalahan Dalam Bit Start Atau Stop." },
  { "en": "Apa Itu 'Sistem Digital'?", "id": "Sistem Yang Menggunakan Sinyal Digital." },
  { "en": "Apa Itu 'Desain Sistem Digital'?", "id": "Proses Merancang Dan Mengimplementasi Sistem." },
  { "en": "Apa Itu 'Siklus Desain'?", "id": "Tahapan Dari Spesifikasi Hingga Implementasi." },
  { "en": "Apa Itu 'Spesifikasi'?", "id": "Deskripsi Detail Fungsi Sistem." },
  { "en": "Apa Itu 'Implementasi'?", "id": "Realisasi Fisik Dari Desain." },
  { "en": "Apa Itu 'Pengujian (Testing)'?", "id": "Proses Verifikasi Implementasi Fisik." },
  { "en": "Apa Beda 'Verifikasi' Dan 'Pengujian'?", "id": "Verifikasi Di Desain, Pengujian Di Hardware." },
  { "en": "Apa Itu 'Sintesis Arsitektur'?", "id": "Menerjemahkan Spesifikasi Menjadi Arsitektur." },
  { "en": "Apa Itu 'Sistem Toleran Kesalahan'?", "id": "Sistem Yang Tahan Terhadap Kegagalan." },
  { "en": "Apa Itu 'Redundansi Perangkat Keras'?", "id": "Menggandakan Komponen Kritis." },
  { "en": "Apa Itu 'Voting Logic'?", "id": "Memilih Output Mayoritas Dari Modul Redundan." },
  { "en": "Apa Itu 'Soft Error'?", "id": "Kesalahan Transien Akibat Partikel Kosmik." },
  { "en": "Apa Itu 'ECC (Error-Correcting Code) Memory'?", "id": "Memori Yang Dapat Memperbaiki Kesalahan." },
  { "en": "Bagaimana 'Kode Hamming' Bekerja?", "id": "Menggunakan Bit Paritas Untuk Lokasi Error." },
  { "en": "Apa Itu 'Checksum'?", "id": "Nilai Dihitung Dari Data Untuk Cek Integritas." },
  { "en": "Apa Itu 'Floating Point Unit (FPU)'?", "id": "Bagian CPU (Central Processing Unit) Untuk Desimal." },
  { "en": "Apa Itu 'Integer Unit'?", "id": "Bagian CPU (Central Processing Unit) Untuk Bilangan Bulat." },
  { "en": "Apa Itu 'Set Instruksi'?", "id": "Kumpulan Perintah Yang Dipahami CPU." },
  { "en": "Apa Itu 'Mode Pengalamatan'?", "id": "Cara Instruksi Menentukan Alamat Data." },
  { "en": "Apa Itu 'Pengalamatan Langsung (Direct)'?", "id": "Alamat Data Ada Dalam Instruksi." },
  { "en": "Apa Itu 'Pengalamatan Tidak Langsung (Indirect)'?", "id": "Instruksi Menunjuk Ke Alamat Data." },
  { "en": "Apa Itu 'Pengalamatan Register'?", "id": "Data Berada Di Dalam Register." },
  { "en": "Apa Itu 'Endianness'?", "id": "Urutan Penyimpanan Byte Dalam Memori." },
  { "en": "Apa Itu 'Big-Endian'?", "id": "Byte Paling Signifikan Disimpan Terlebih Dahulu." },
  { "en": "Apa Itu 'Little-Endian'?", "id": "Byte Paling Tidak Signifikan Disimpan Dulu." },
  { "en": "Apa Itu 'Checksum Internet'?", "id": "Metode Cek Kesalahan Sederhana." },
  { "en": "Apa Itu 'Sistem Operasi (OS)'?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Itu 'Kernel'?", "id": "Inti Dari Sistem Operasi." },
  { "en": "Apa Itu 'Device Driver'?", "id": "Perangkat Lunak Pengontrol Perangkat Keras." },
  { "en": "Apa Itu 'Aplikasi Perangkat Lunak'?", "id": "Program Yang Berjalan Di Atas OS." },
  { "en": "Apa Itu 'Konversi Tipe (Type Casting)'?", "id": "Mengubah Tipe Data Variabel." },
  { "en": "Apa Itu 'Buffer Overflow'?", "id": "Menulis Data Melebihi Batas Buffer." },
  { "en": "Apa Itu 'Stack Overflow'?", "id": "Stack Kehabisan Ruang." },
  { "en": "Apa Itu 'Recursion'?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu 'Base Case' Dalam Rekursi?", "id": "Kondisi Berhenti Untuk Fungsi Rekursif." },
  { "en": "Apa Itu 'Struktur Data'?", "id": "Cara Mengorganisir Data Di Memori." },
  { "en": "Contoh 'Struktur Data'?", "id": "Array, List, Stack, Queue." },
  { "en": "Apa Itu 'Algoritma'?", "id": "Langkah-langkah Untuk Menyelesaikan Masalah." },
  { "en": "Apa Itu 'Kompleksitas Algoritma'?", "id": "Ukuran Sumber Daya Yang Dibutuhkan." },
  { "en": "Apa Itu 'Notasi Big-O'?", "id": "Notasi Untuk Kompleksitas Waktu Terburuk." },
  { "en": "Apa Itu 'Bubble Sort'?", "id": "Algoritma Pengurutan Sederhana Tapi Lambat." },
  { "en": "Apa Itu 'Binary Search'?", "id": "Algoritma Pencarian Efisien Pada Data Terurut." },
  { "en": "Apa Itu 'Hash Table'?", "id": "Struktur Data Untuk Pencarian Cepat." },
  { "en": "Apa Itu 'Hash Collision'?", "id": "Dua Input Menghasilkan Hash Yang Sama." },
  { "en": "Apa Itu 'Pohon Biner'?", "id": "Struktur Data Pohon Dengan Dua Anak." },
  { "en": "Apa Itu 'Graf (Graph)'?", "id": "Struktur Data Simpul Dan Tepi." },
  { "en": "Apa Itu 'Sistem Terdistribusi'?", "id": "Sistem Dengan Komponen Di Komputer Berbeda." },
  { "en": "Apa Itu 'Komputasi Paralel'?", "id": "Menjalankan Banyak Komputasi Secara Bersamaan." },
  { "en": "Mengapa 'Sel SRAM' Disebut Statis?", "id": "Karena Menahan Data Tanpa Perlu Refresh." },
  { "en": "Apa Tahap Dalam 'Pipelining' Klasik?", "id": "Fetch, Decode, Execute, Memory, Writeback." },
  { "en": "Apa Kepanjangan 'VHDL'?", "id": "VHSIC (Very High Speed IC) HDL." },
  { "en": "Apa Itu 'Barrel Shifter'?", "id": "Rangkaian Untuk Menggeser Bit Dengan Cepat." },
  { "en": "Apa Itu 'Carry-Save Adder'?", "id": "Penjumlah Yang Menunda Propagasi Carry." },
  { "en": "Apa Itu 'Wallace Tree'?", "id": "Struktur Cepat Untuk Perkalian Biner." },
  { "en": "Apa Keuntungan 'Pengkodean One-Hot'?", "id": "Logika Dekode Sangat Sederhana." },
  { "en": "Apa Itu 'Interrupt Service Routine (ISR)'?", "id": "Kode Yang Dijalankan Saat Interrupt." },
  { "en": "Apa Itu 'Real-Time Clock (RTC)'?", "id": "Jam Yang Tetap Berjalan Tanpa Daya." },
  { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Antara Jalur Sinyal Berdekatan." },
  { "en": "Apa Itu 'Impedansi' Jalur Transmisi?", "id": "Resistansi Efektif Jalur Frekuensi Tinggi." },
  { "en": "Apa Itu 'Terminasi' Bus?", "id": "Mencegah Pantulan Sinyal Di Ujung Bus." },
  { "en": "Apa Hubungan 'CPU', 'ALU', Dan 'Unit Kontrol'?", "id": "ALU (Arithmetic Logic Unit) Dan Unit Kontrol Bagian CPU." },
  { "en": "Bagaimana 'Sinyal Clock' Dihasilkan?", "id": "Menggunakan Osilator Kristal." },
  { "en": "Apa Peran 'Sistem Operasi' Bagi Hardware?", "id": "Mengelola Dan Mengabstraksi Perangkat Keras." },
  { "en": "Apa Itu 'Word' Dalam Arsitektur Komputer?", "id": "Ukuran Data Alami Sebuah Prosesor." },
  { "en": "Apa Itu 'Alignment' Data?", "id": "Menempatkan Data Pada Alamat Memori Tertentu." },
  { "en": "Apa Itu 'Endianness'?", "id": "Urutan Byte Dalam Representasi Data." },
  { "en": "Apa Itu 'Little-Endian'?", "id": "Byte Paling Tidak Signifikan Disimpan Dulu." },
  { "en": "Apa Itu 'Big-Endian'?", "id": "Byte Paling Signifikan Disimpan Dulu." },
  { "en": "Apa Itu 'Instruksi'?", "id": "Perintah Dasar Untuk Prosesor." },
  { "en": "Apa Itu 'Set Instruksi'?", "id": "Kumpulan Semua Instruksi Yang Dipahami CPU." },
  { "en": "Apa Itu 'Operand'?", "id": "Data Yang Diolah Oleh Instruksi." },
  { "en": "Apa Itu 'Opcode'?", "id": "Kode Biner Yang Merepresentasikan Instruksi." },
  { "en": "Apa Itu 'Prosesor Superscalar'?", "id": "Prosesor Yang Bisa Eksekusi Banyak Instruksi." },
  { "en": "Apa Itu 'Eksekusi Out-of-Order'?", "id": "Eksekusi Instruksi Tidak Sesuai Urutan." },
  { "en": "Apa Itu 'Register Renaming'?", "id": "Teknik Untuk Mengatasi Hazard Data." },
  { "en": "Apa Itu 'Branch Prediction'?", "id": "Menebak Arah Percabangan Kode." },
  { "en": "Apa Itu 'Cache Coherency'?", "id": "Menjaga Konsistensi Data Antar Cache." },
  { "en": "Apa Itu 'Snooping Protocol'?", "id": "Protokol Untuk Menjaga Koherensi Cache." },
  { "en": "Apa Itu 'Cache Miss'?", "id": "Data Yang Dicari Tidak Ada Di Cache." },
  { "en": "Apa Itu 'Cache Hit'?", "id": "Data Yang Dicari Ada Di Cache." },
  { "en": "Apa Itu 'Write-Through' Cache?", "id": "Menulis Langsung Ke RAM Dan Cache." },
  { "en": "Apa Itu 'Write-Back' Cache?", "id": "Menulis Ke RAM Hanya Saat Diperlukan." },
  { "en": "Apa Itu 'Virtual Memory'?", "id": "Mekanisme Memori Yang Gunakan Penyimpanan Sekunder." },
  { "en": "Apa Itu 'Page Table'?", "id": "Tabel Pemetaan Alamat Virtual Ke Fisik." },
  { "en": "Apa Itu 'TLB (Translation Lookaside Buffer)'?", "id": "Cache Untuk Page Table." },
  { "en": "Apa Itu 'Page Fault'?", "id": "Akses Ke Halaman Yang Tidak Ada Di RAM." },
  { "en": "Apa Itu 'Sistem Interkoneksi'?", "id": "Jaringan Komunikasi Di Dalam Chip." },
  { "en": "Apa Itu 'Network-on-Chip (NoC)'?", "id": "Pendekatan Jaringan Untuk Komunikasi On-Chip." },
  { "en": "Apa Itu 'Gerbang Logika Reversibel'?", "id": "Gerbang Yang Inputnya Bisa Direkonstruksi." },
  { "en": "Mengapa 'Logika Reversibel' Penting?", "id": "Untuk Komputasi Kuantum Dan Daya Rendah." },
  { "en": "Apa Itu 'Gerbang Toffoli'?", "id": "Contoh Gerbang Reversibel Yang Universal." },
  { "en": "Apa Itu 'Komputasi Adiabatik'?", "id": "Komputasi Yang Minim Disipasi Panas." },
  { "en": "Apa Itu 'Logika Tiga Nilai (Ternary)'?", "id": "Logika Yang Punya Tiga Nilai." },
  { "en": "Apa Itu 'Sistem Numerik Seimbang'?", "id": "Sistem Angka Dengan Digit Positif-Negatif." },
  { "en": "Apa Itu 'Adder-Subtractor'?", "id": "Rangkaian Yang Bisa Menambah Dan Mengurang." },
  { "en": "Bagaimana Cara Membuat 'Subtractor' Dari 'Adder'?", "id": "Menggunakan Komplemen Dua Dan Gerbang XOR." },
  { "en": "Apa Itu 'Barrel Shifter'?", "id": "Rangkaian Kombinasional Untuk Operasi Geser." },
  { "en": "Apa Itu 'Priority Encoder'?", "id": "Encoder Yang Hanya Proses Input Prioritas Tertinggi." },
  { "en": "Apa Itu 'Decoder Biner ke Gray'?", "id": "Mengubah Kode Biner Ke Kode Gray." },
  { "en": "Apa Itu 'Counter Johnson'?", "id": "Shift Register Dengan Umpan Balik Terbalik." },
  { "en": "Apa Keuntungan 'Johnson Counter'?", "id": "Urutan Bebas Glitch." },
  { "en": "Apa Itu 'Linear Feedback Shift Register (LFSR)'?", "id": "Shift Register Dengan Umpan Balik Linier." },
  { "en": "Apa Kegunaan 'LFSR'?", "id": "Penghasil Angka Acak Semu." },
  { "en": "Apa Itu 'Siklus Batas (Limit Cycle)'?", "id": "Pola Berulang Dalam Sistem Digital." },
  { "en": "Apa Itu 'Verifikasi Properti'?", "id": "Memverifikasi Apakah Desain Memenuhi Properti." },
  { "en": "Apa Itu 'Assertion'?", "id": "Pernyataan Properti Yang Harus Benar." },
  { "en": "Apa Itu 'Simulasi Event-Driven'?", "id": "Simulasi Yang Maju Berdasarkan Kejadian." },
  { "en": "Apa Itu 'Simulasi Cycle-Accurate'?", "id": "Simulasi Yang Akurat Hingga Siklus Clock." },
  { "en": "Apa Itu 'Emulasi Perangkat Keras'?", "id": "Mensimulasikan Desain Di Atas Hardware (FPGA)." },
  { "en": "Apa Itu 'Akselerasi Perangkat Keras'?", "id": "Menggunakan Hardware Khusus Mempercepat Simulasi." },
  { "en": "Apa Itu 'Desain Asinkron'?", "id": "Desain Sirkuit Tanpa Sinyal Clock." },
  { "en": "Apa Keuntungan 'Desain Asinkron'?", "id": "Daya Rendah Dan Tanpa Masalah Clock." },
  { "en": "Apa Kerugian 'Desain Asinkron'?", "id": "Jauh Lebih Sulit Untuk Didesain." },
  { "en": "Apa Itu 'Sirkuit Self-Timed'?", "id": "Jenis Sirkuit Asinkron." },
  { "en": "Apa Itu 'Sinyal Handshake'?", "id": "Sinyal (Request/Acknowledge) Untuk Sinkronisasi." },
  { "en": "Apa Itu 'Arbiter'?", "id": "Sirkuit Untuk Memberi Akses Sumber Daya." },
  { "en": "Apa Itu 'FIFO Asinkron'?", "id": "FIFO (First-In, First-Out) Untuk Menyeberangi Domain Clock." },
  { "en": "Apa Itu 'Grayhill Code'?", "id": "Nama Lain Untuk Kode Gray." },
  { "en": "Apa Itu 'Decimal-to-BCD Encoder'?", "id": "Mengubah 10 Input Desimal Ke 4-bit BCD." },
  { "en": "Apa Itu 'Logic Array Block (LAB)'?", "id": "Blok Logika Dalam Arsitektur CPLD/FPGA." },
  { "en": "Apa Itu 'Antifuse'?", "id": "Elemen Sekali Program Dalam PLD." },
  { "en": "Apa Itu 'Floating Gate'?", "id": "Gerbang Terisolasi Penyimpan Muatan." },
  { "en": "Di Mana 'Floating Gate' Digunakan?", "id": "Di Memori EPROM Dan Flash." },
  { "en": "Apa Itu 'Hot-Electron Injection'?", "id": "Mekanisme Pemrograman Untuk Memori Flash." },
  { "en": "Apa Itu 'Fowler-Nordheim Tunneling'?", "id": "Mekanisme Hapus/Tulis Memori Flash." },
  { "en": "Apa Itu 'Wear Leveling'?", "id": "Distribusi Siklus Tulis Di Memori Flash." },
  { "en": "Apa Itu 'Read Disturb'?", "id": "Membaca Satu Sel Mengganggu Sel Tetangga." },
  { "en": "Apa Itu 'Write Amplification'?", "id": "Data Ditulis Lebih Banyak Dari Sebenarnya." },
  { "en": "Apa Itu 'Garbage Collection' Di SSD?", "id": "Proses Mengatur Ulang Blok Data." },
  { "en": "Apa Itu 'TRIM Command'?", "id": "Perintah OS Ke SSD Tentang Blok Kosong." },
  { "en": "Apa Itu 'NVMe (Non-Volatile Memory Express)'?", "id": "Protokol Cepat Untuk Akses SSD." },
  { "en": "Apa Itu 'Antarmuka SATA (Serial ATA)'?", "id": "Antarmuka Standar Untuk Drive Penyimpanan." },
  { "en": "Apa Itu 'M.2 Form Factor'?", "id": "Faktor Bentuk Kecil Untuk SSD." },
  { "en": "Apa Itu 'U.2 Form Factor'?", "id": "Faktor Bentuk 2.5 Inci Untuk SSD NVMe." },
  { "en": "Apa Itu 'DDR (Double Data Rate)'?", "id": "Transfer Data Di Kedua Tepi Clock." },
  { "en": "Apa Itu 'Registered Memory (RDIMM)'?", "id": "Modul RAM Dengan Register Buffer." },
  { "en": "Di Mana 'RDIMM' Digunakan?", "id": "Di Server Untuk Stabilitas." },
  { "en": "Apa Itu 'Unbuffered Memory (UDIMM)'?", "id": "Modul RAM Tanpa Register Buffer." },
  { "en": "Apa Itu 'ECC (Error-Correcting Code)'?", "id": "Kode Untuk Mendeteksi Dan Memperbaiki Error." },
  { "en": "Apa Itu 'Memory Controller'?", "id": "Sirkuit Pengatur Akses Ke RAM." },
  { "en": "Apa Itu 'Single-Channel Memory'?", "id": "Satu Jalur Data Ke Memori." },
  { "en": "Apa Itu 'Dual-Channel Memory'?", "id": "Dua Jalur Data Paralel Ke Memori." },
  { "en": "Apa Itu 'Quad-Channel Memory'?", "id": "Empat Jalur Data Paralel Ke Memori." },
  { "en": "Apa Itu 'LatenCAS (CL)'?", "id": "Penundaan Waktu Dalam Siklus Memori." },
  { "en": "Mengapa 'Sel SRAM' Lebih Cepat Dari DRAM?", "id": "Karena Tidak Memerlukan Siklus Refresh." },
  { "en": "Apa Itu 'Pipeline Stall' Atau 'Bubble'?", "id": "Penundaan Dalam Aliran Eksekusi Pipeline." },
  { "en": "Apa Beda 'Entitas' Dan 'Arsitektur' Di VHDL?", "id": "Entitas Port, Arsitektur Perilaku." },
  { "en": "Apa Itu 'Multiplier Wallace Tree'?", "id": "Struktur Cepat Untuk Perkalian Biner." },
  { "en": "Apa Itu 'Penjumlah Carry-Save'?", "id": "Menjumlahkan Tiga Angka, Hasilkan Dua." },
  { "en": "Apa Yang Diwakili 'Lingkaran' Dalam Diagram State?", "id": "Keadaan (State) Dari Sistem." },
  { "en": "Apa Yang Diwakili 'Panah' Dalam Diagram State?", "id": "Transisi Antar Keadaan." },
  { "en": "Apa Itu 'Tabel Transisi State'?", "id": "Representasi Tabel Dari Diagram State." },
  { "en": "Apa Itu 'Interrupt Vector Table'?", "id": "Tabel Penunjuk Ke Rutin Interrupt." },
  { "en": "Apa Itu 'Efek Jalur Transmisi'?", "id": "Perilaku Sinyal Pada Frekuensi Tinggi." },
  { "en": "Apa Itu 'Refleksi Sinyal'?", "id": "Pantulan Sinyal Akibat Mismatch Impedansi." },
  { "en": "Apa Itu 'Power Integrity'?", "id": "Menjaga Kualitas Jaringan Suplai Daya." },
  { "en": "Apa Hubungan 'Gerbang Logika' Dan 'Transistor'?", "id": "Gerbang Logika Dibangun Dari Transistor." },
  { "en": "Bagaimana 'Flip-Flop' Membagi Frekuensi?", "id": "Output T-Flip-Flop Setengah Frekuensi Input." },
  { "en": "Apa Peran 'Decoder' Dalam Pengalamatan Memori?", "id": "Mengaktifkan Baris Memori Yang Tepat." },
  { "en": "Apa Itu 'Integer'?", "id": "Representasi Bilangan Bulat." },
  { "en": "Apa Itu 'Floating Point'?", "id": "Representasi Bilangan Dengan Koma Desimal." },
  { "en": "Apa Itu 'Operand'?", "id": "Data Yang Dioperasikan Oleh Instruksi." },
  { "en": "Apa Itu 'Opcode'?", "id": "Kode Operasi Dalam Sebuah Instruksi." },
  { "en": "Apa Itu 'Arsitektur Set Instruksi (ISA)'?", "id": "Kumpulan Instruksi Yang Dipahami Prosesor." },
  { "en": "Apa Itu 'Eksekusi Spekulatif'?", "id": "Mengeksekusi Instruksi Sebelum Dibutuhkan." },
  { "en": "Apa Itu 'Cache L1'?", "id": "Cache Terkecil Dan Tercepat." },
  { "en": "Apa Itu 'Cache L2'?", "id": "Cache Lebih Besar Dari L1." },
  { "en": "Apa Itu 'Cache L3'?", "id": "Cache Terbesar, Dibagi Antar Inti." },
  { "en": "Apa Itu 'Cache Coherence'?", "id": "Menjaga Konsistensi Data Antar Cache." },
  { "en": "Apa Itu 'Write Buffer'?", "id": "Buffer Untuk Menampung Operasi Tulis." },
  { "en": "Apa Itu 'Memory Management Unit (MMU)'?", "id": "Unit Pengelola Memori Virtual." },
  { "en": "Apa Itu 'Halaman Memori (Page)'?", "id": "Blok Memori Virtual Ukuran Tetap." },
  { "en": "Apa Itu 'Swapping'?", "id": "Memindahkan Halaman Antara RAM Dan Disk." },
  { "en": "Apa Itu 'Segmentation' Memori?", "id": "Membagi Memori Menjadi Segmen Logis." },
  { "en": "Apa Itu 'Sistem Interkoneksi Bus'?", "id": "Menggunakan Bus Bersama Untuk Komunikasi." },
  { "en": "Apa Itu 'Sistem Interkoneksi Crossbar'?", "id": "Setiap Input Bisa Terhubung Ke Output." },
  { "en": "Apa Itu 'Synchronous Communication'?", "id": "Komunikasi Yang Disinkronkan Oleh Clock." },
  { "en": "Apa Itu 'Asynchronous Communication'?", "id": "Komunikasi Tanpa Sinyal Clock Bersama." },
  { "en": "Apa Itu 'Parity Ganjil'?", "id": "Memastikan Jumlah Bit 1 Ganjil." },
  { "en": "Apa Itu 'Parity Genap'?", "id": "Memastikan Jumlah Bit 1 Genap." },
  { "en": "Apa Itu 'Hamming Distance'?", "id": "Jumlah Bit Yang Berbeda." },
  { "en": "Apa Itu 'Siklus Instruksi'?", "id": "Langkah-langkah Eksekusi Satu Instruksi." },
  { "en": "Apa Itu 'Register File'?", "id": "Kumpulan Register Di Dalam Prosesor." },
  { "en": "Apa Itu 'Accumulator'?", "id": "Register Khusus Untuk Hasil Aritmetika." },
  { "en": "Apa Itu 'Instruction Pointer'?", "id": "Nama Lain Untuk Program Counter." },
  { "en": "Apa Itu 'Stack Frame'?", "id": "Area Stack Untuk Satu Panggilan Fungsi." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Itu 'Bit Slice Architecture'?", "id": "Membangun CPU Dari ALU Slice." },
  { "en": "Apa Itu 'Microcode'?", "id": "Instruksi Level Rendah Pengontrol CPU." },
  { "en": "Apa Itu 'Horizontal Microcode'?", "id": "Microcode Dengan Kontrol Paralel Tinggi." },
  { "en": "Apa Itu 'Vertical Microcode'?", "id": "Microcode Dengan Pengkodean Lebih Padat." },
  { "en": "Apa Itu 'Prosesor Vektor'?", "id": "Prosesor Yang Beroperasi Pada Array Data." },
  { "en": "Apa Itu 'SIMD (Single Instruction, Multiple Data)'?", "id": "Satu Instruksi Untuk Banyak Data." },
  { "en": "Apa Itu 'MIMD (Multiple Instruction, Multiple Data)'?", "id": "Banyak Instruksi Untuk Banyak Data." },
  { "en": "Apa Itu 'Sistem Multiprocessor'?", "id": "Sistem Dengan Beberapa CPU." },
  { "en": "Apa Itu 'Symmetric Multiprocessing (SMP)'?", "id": "Semua Prosesor Berbagi Memori Yang Sama." },
  { "en": "Apa Itu 'Moore's Law'?", "id": "Jumlah Transistor Berlipat Ganda Periodik." },
  { "en": "Apa Itu 'Amdahl's Law'?", "id": "Hukum Tentang Batas Percepatan Paralel." },
  { "en": "Apa Itu 'Gustafson's Law'?", "id": "Perspektif Lain Tentang Percepatan Paralel." },
  { "en": "Apa Itu 'Benchmark'?", "id": "Program Untuk Mengukur Kinerja." },
  { "en": "Apa Itu 'MIPS (Million Instructions Per Second)'?", "id": "Ukuran Kinerja Kecepatan Prosesor." },
  { "en": "Apa Itu 'FLOPS (Floating-Point Operations Per Second)'?", "id": "Ukuran Kinerja Operasi Floating-Point." },
  { "en": "Apa Itu 'Sistem I/O (Input/Output)'?", "id": "Bagian Komputer Untuk Komunikasi Luar." },
  { "en": "Apa Itu 'Port I/O'?", "id": "Antarmuka Fisik Untuk Perangkat I/O." },
  { "en": "Apa Itu 'Memory-Mapped I/O'?", "id": "Perangkat I/O Dipetakan Ke Alamat Memori." },
  { "en": "Apa Itu 'Port-Mapped I/O'?", "id": "Perangkat I/O Punya Alamat Port Khusus." },
  { "en": "Apa Itu 'Interrupt Controller'?", "id": "Mengelola Sinyal Interrupt Dari Perangkat." },
  { "en": "Apa Itu 'DMA Controller'?", "id": "Mengelola Transfer DMA (Direct Memory Access)." },
  { "en": "Apa Itu 'Bus Master'?", "id": "Perangkat Yang Bisa Memulai Transfer Bus." },
  { "en": "Apa Itu 'Arbitrasi Bus'?", "id": "Proses Menentukan Siapa Yang Kendalikan Bus." },
  { "en": "Apa Itu 'Daisy Chaining'?", "id": "Metode Sederhana Untuk Arbitrasi." },
  { "en": "Apa Itu 'Cache Line'?", "id": "Unit Transfer Data Antara Cache-RAM." },
  { "en": "Apa Itu 'Cache Tag'?", "id": "Menyimpan Informasi Alamat Cache Line." },
  { "en": "Apa Itu 'Valid Bit'?", "id": "Menandakan Apakah Data Cache Valid." },
  { "en": "Apa Itu 'Dirty Bit'?", "id": "Menandakan Data Cache Telah Dimodifikasi." },
  { "en": "Apa Itu 'Cache Mapping'?", "id": "Aturan Penempatan Data Dari RAM Ke Cache." },
  { "en": "Apa Itu 'Direct-Mapped Cache'?", "id": "Setiap Blok RAM Hanya Satu Lokasi Cache." },
  { "en": "Apa Itu 'Fully Associative Cache'?", "id": "Blok RAM Bisa Ditempatkan Di Lokasi Apapun." },
  { "en": "Apa Itu 'Set-Associative Cache'?", "id": "Kompromi Antara Direct Dan Fully Associative." },
  { "en": "Apa Itu 'Replacement Policy'?", "id": "Aturan Memilih Blok Cache Untuk Diganti." },
  { "en": "Apa Itu 'LRU (Least Recently Used)'?", "id": "Mengganti Blok Yang Paling Lama Tak Digunakan." },
  { "en": "Apa Itu 'FIFO (First-In, First-Out)' Cache Replacement?", "id": "Mengganti Blok Yang Paling Pertama Masuk." },
  { "en": "Apa Itu 'Random Replacement'?", "id": "Mengganti Blok Cache Secara Acak." },
  { "en": "Apa Itu 'Unified Cache'?", "id": "Cache Tunggal Untuk Instruksi Dan Data." },
  { "en": "Apa Itu 'Split Cache'?", "id": "Cache Terpisah Untuk Instruksi Dan Data." },
  { "en": "Apa Itu 'Multilevel Cache'?", "id": "Hirarki Cache (L1, L2, L3)." },
  { "en": "Apa Itu 'Write Allocate'?", "id": "Alokasikan Cache Line Saat Operasi Tulis." },
  { "en": "Apa Itu 'No-Write Allocate'?", "id": "Tidak Alokasikan Cache Line Saat Tulis." },
  { "en": "Apa Itu 'Prefetching'?", "id": "Mengambil Data Sebelum Dibutuhkan." },
  { "en": "Apa Itu 'Lockup-Free Cache'?", "id": "Cache Yang Bisa Tangani Banyak Miss." },
  { "en": "Apa Itu 'Virtual Cache'?", "id": "Cache Yang Diindeks Dengan Alamat Virtual." },
  { "en": "Apa Itu 'Physical Cache'?", "id": "Cache Yang Diindeks Dengan Alamat Fisik." },
  { "en": "Apa Saja 'Flag' Yang Dihasilkan ALU?", "id": "Zero, Carry, Overflow, Negative." },
  { "en": "Bagaimana 'Decoder' Memilih Perangkat I/O?", "id": "Dengan Mendekode Alamat Pada Bus." },
  { "en": "Apa Saja Pin Utama Antarmuka 'JTAG'?", "id": "TDI, TDO, TCK, TMS, TRST." },
  { "en": "Apa Itu 'RAS to CAS Delay (tRCD)'?", "id": "Penundaan Antara Aktivasi Baris Dan Kolom." },
  { "en": "Apa Itu 'Precharge' Dalam Siklus DRAM?", "id": "Menutup Baris Memori Yang Sedang Aktif." },
  { "en": "Apa Itu 'Sistem Loop Terbuka'?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu 'Sistem Loop Tertutup'?", "id": "Sistem Kontrol Yang Menggunakan Umpan Balik." },
  { "en": "Apa Itu 'Cyclic Redundancy Check (CRC)'?", "id": "Kode Deteksi Kesalahan Yang Kuat." },
  { "en": "Apa Itu 'Interrupt Latency'?", "id": "Waktu Tunda Respon Terhadap Interrupt." },
  { "en": "Apa Itu 'Interrupt Priority'?", "id": "Tingkat Kepentingan Sebuah Sinyal Interrupt." },
  { "en": "Apa Itu 'Ground Loop'?", "id": "Loop Arus Tak Diinginkan Di Ground." },
  { "en": "Apa Fungsi 'Kapasitor Decoupling'?", "id": "Menyaring Derau Pada Jalur Suplai Daya." },
  { "en": "Apa Hubungan 'Aljabar Boolean' Dan 'Logika Proposisional'?", "id": "Aljabar Boolean Adalah Implementasi Logika Proposisional." },
  { "en": "Bagaimana 'Register' Berbeda Dari 'Memori RAM'?", "id": "Register Jauh Lebih Cepat Dari RAM." },
  { "en": "Apa Peran 'Compiler'?", "id": "Menerjemahkan Kode Sumber Ke Kode Mesin." },
  { "en": "Apa Itu 'Zero Flag (ZF)'?", "id": "Menandakan Hasil Operasi Adalah Nol." },
  { "en": "Apa Itu 'Carry Flag (CF)'?", "id": "Menandakan Adanya Simpanan (Carry-out)." },
  { "en": "Apa Itu 'Overflow Flag (OF)'?", "id": "Menandakan Hasil Melebihi Batas Angka Bertanda." },
  { "en": "Apa Itu 'Sign Flag (SF)'?", "id": "Menandakan Hasil Operasi Adalah Negatif." },
  { "en": "Apa Itu 'Logic Level Shifting'?", "id": "Mengubah Satu Level Tegangan Ke Lainnya." },
  { "en": "Mengapa 'Level Shifting' Dibutuhkan?", "id": "Untuk Komunikasi Antar Keluarga Logika." },
  { "en": "Apa Itu 'Pull-up Resistor Internal'?", "id": "Resistor Pull-up Di Dalam Chip." },
  { "en": "Apa Itu 'Bit-Banging'?", "id": "Implementasi Protokol Serial Dengan Software." },
  { "en": "Apa Kelemahan 'Bit-Banging'?", "id": "Sangat Membebani CPU (Central Processing Unit)." },
  { "en": "Apa Itu 'Hardware Accelerator'?", "id": "Perangkat Keras Khusus Untuk Tugas Tertentu." },
  { "en": "Contoh 'Hardware Accelerator'?", "id": "GPU (Graphics Processing Unit), DSP (Digital Signal Processor)." },
  { "en": "Apa Itu 'Memory-Mapped Register'?", "id": "Register Periferal Yang Diakses Seperti Memori." },
  { "en": "Apa Itu 'Reset Circuit'?", "id": "Sirkuit Yang Menghasilkan Sinyal Reset." },
  { "en": "Apa Itu 'Active-Low Reset'?", "id": "Sistem Direset Saat Sinyal Rendah." },
  { "en": "Apa Itu 'Gated Clock'?", "id": "Sinyal Clock Yang Dikontrol Gerbang Logika." },
  { "en": "Mengapa 'Gated Clock' Dihindari?", "id": "Dapat Menyebabkan Glitch Dan Masalah Waktu." },
  { "en": "Apa Alternatif Untuk 'Gated Clock'?", "id": "Menggunakan Sinyal Clock Enable." },
  { "en": "Apa Itu 'Static Hazard'?", "id": "Potensi Glitch Saat Output Seharusnya Tetap." },
  { "en": "Apa Itu 'Dynamic Hazard'?", "id": "Potensi Glitch Saat Output Seharusnya Berubah." },
  { "en": "Apa Itu 'One-Hot Encoding'?", "id": "Representasi State Dengan Satu Bit Aktif." },
  { "en": "Apa Keuntungan 'One-Hot Encoding'?", "id": "Logika Dekode Sederhana Dan Cepat." },
  { "en": "Apa Kerugian 'One-Hot Encoding'?", "id": "Membutuhkan Lebih Banyak Flip-Flop." },
  { "en": "Apa Itu 'Binary Encoding'?", "id": "Representasi State Menggunakan Kode Biner." },
  { "en": "Apa Itu 'State Transition'?", "id": "Nama Lain Untuk Transisi Keadaan." },
  { "en": "Apa Itu 'Siklus Tunggu (Wait State)'?", "id": "Siklus Clock Kosong Untuk Tunggu Memori." },
  { "en": "Apa Itu 'Burst Mode'?", "id": "Transfer Blok Data Berurutan Dengan Cepat." },
  { "en": "Apa Itu 'Write Combining'?", "id": "Menggabungkan Banyak Operasi Tulis Kecil." },
  { "en": "Apa Itu 'D-Cache'?", "id": "Cache Khusus Untuk Data." },
  { "en": "Apa Itu 'I-Cache'?", "id": "Cache Khusus Untuk Instruksi." },
  { "en": "Apa Itu 'Harvard Architecture'?", "id": "Memisahkan Bus Untuk Data Dan Instruksi." },
  { "en": "Apa Itu 'Modified Harvard Architecture'?", "id": "Arsitektur Harvard Dengan Akses Data." },
  { "en": "Apa Itu 'Von Neumann Architecture'?", "id": "Menggunakan Bus Tunggal Untuk Data-Instruksi." },
  { "en": "Apa Itu 'Bottleneck Von Neumann'?", "id": "Keterbatasan Kinerja Akibat Bus Tunggal." },
  { "en": "Apa Itu 'CISC (Complex Instruction Set Computer)'?", "id": "Arsitektur Dengan Instruksi Kompleks." },
  { "en": "Apa Itu 'RISC (Reduced Instruction Set Computer)'?", "id": "Arsitektur Dengan Instruksi Sederhana." },
  { "en": "Mana Yang Lebih Cepat, 'RISC' Atau 'CISC'?", "id": "RISC (Reduced Instruction Set Computer) Umumnya Lebih Cepat." },
  { "en": "Apa Itu 'VLIW (Very Long Instruction Word)'?", "id": "Arsitektur Dengan Paralelisme Instruksi Eksplisit." },
  { "en": "Apa Itu 'Superscalar'?", "id": "Arsitektur Yang Bisa Eksekusi Banyak Instruksi." },
  { "en": "Apa Itu 'Data Hazard'?", "id": "Konflik Ketergantungan Data Dalam Pipeline." },
  { "en": "Apa Itu 'Control Hazard'?", "id": "Konflik Akibat Instruksi Percabangan." },
  { "en": "Apa Itu 'Structural Hazard'?", "id": "Konflik Akibat Keterbatasan Sumber Daya." },
  { "en": "Apa Itu 'Pipeline Forwarding'?", "id": "Meneruskan Hasil Untuk Mengatasi Hazard." },
  { "en": "Apa Itu 'Instruction Fetch (IF)'?", "id": "Tahap Mengambil Instruksi Dari Memori." },
  { "en": "Apa Itu 'Instruction Decode (ID)'?", "id": "Tahap Menerjemahkan Instruksi." },
  { "en": "Apa Itu 'Execute (EX)'?", "id": "Tahap Eksekusi Operasi Oleh ALU." },
  { "en": "Apa Itu 'Memory Access (MEM)'?", "id": "Tahap Membaca Atau Menulis Ke Memori." },
  { "en": "Apa Itu 'Write Back (WB)'?", "id": "Tahap Menulis Hasil Kembali Ke Register." },
  { "en": "Apa Itu 'Sistem Operasi (OS)'?", "id": "Perangkat Lunak Pengelola Perangkat Keras." },
  { "en": "Apa Itu 'Bootloader'?", "id": "Program Pertama Yang Dijalankan Saat Boot." },
  { "en": "Apa Itu 'Abstraksi Perangkat Keras'?", "id": "Menyembunyikan Detail Kompleks Perangkat Keras." },
  { "en": "Apa Itu 'Antrian Prioritas (Priority Queue)'?", "id": "Struktur Data Berdasarkan Prioritas." },
  { "en": "Apa Itu 'Daftar Tertaut (Linked List)'?", "id": "Struktur Data Dengan Elemen Terhubung." },
  { "en": "Apa Itu 'Pohon (Tree)'?", "id": "Struktur Data Hierarkis." },
  { "en": "Apa Itu 'Simpul (Node)'?", "id": "Elemen Individual Dalam Struktur Data." },
  { "en": "Apa Itu 'Tepi (Edge)'?", "id": "Koneksi Antara Dua Simpul." },
  { "en": "Apa Itu 'Akar (Root)' Pohon?", "id": "Simpul Paling Atas." },
  { "en": "Apa Itu 'Daun (Leaf)' Pohon?", "id": "Simpul Tanpa Anak." },
  { "en": "Apa Itu 'Pohon Pencarian Biner (BST)'?", "id": "Pohon Biner Dengan Aturan Urutan." },
  { "en": "Apa Itu 'Hash Function'?", "id": "Fungsi Yang Memetakan Data Ke Nilai." },
  { "en": "Apa Itu 'Manajemen Memori'?", "id": "Tugas OS (Operating System) Mengelola RAM." },
  { "en": "Apa Itu 'Penjadwalan (Scheduling)'?", "id": "Tugas OS (Operating System) Mengatur Eksekusi Proses." },
  { "en": "Apa Itu 'Proses'?", "id": "Program Yang Sedang Dijalankan." },
  { "en": "Apa Itu 'Thread'?", "id": "Unit Eksekusi Terkecil Dalam Proses." },
  { "en": "Apa Itu 'Multithreading'?", "id": "Menjalankan Banyak Thread Dalam Satu Proses." },
  { "en": "Apa Itu 'Multiprocessing'?", "id": "Menggunakan Beberapa CPU Secara Bersamaan." },
  { "en": "Apa Itu 'Inter-Process Communication (IPC)'?", "id": "Mekanisme Komunikasi Antar Proses." },
  { "en": "Contoh 'IPC (Inter-Process Communication)'?", "id": "Pipes, Sockets, Shared Memory." },
  { "en": "Apa Itu 'Deadlock'?", "id": "Dua Atau Lebih Proses Saling Menunggu." },
  { "en": "Apa Itu 'Sinkronisasi Proses'?", "id": "Mengkoordinasikan Eksekusi Proses." },
  { "en": "Apa Itu 'Mutex'?", "id": "Mekanisme Untuk Mencegah Akses Simultan." },
  { "en": "Apa Itu 'Semaphore'?", "id": "Variabel Untuk Mengontrol Akses Sumber Daya." },
  { "en": "Apa Itu 'Critical Section'?", "id": "Bagian Kode Yang Hanya Boleh Satu Proses." },
  { "en": "Apa Itu 'Sistem File Jurnal'?", "id": "Sistem File Yang Mencatat Perubahan." },
  { "en": "Apa Keuntungan 'Sistem File Jurnal'?", "id": "Lebih Cepat Pulih Dari Kegagalan." },
  { "en": "Apa Itu 'Virtualisasi'?", "id": "Membuat Versi Virtual Dari Sesuatu." },
  { "en": "Apa Itu 'Hypervisor'?", "id": "Perangkat Lunak Yang Membuat Mesin Virtual." },
  { "en": "Apa Itu 'Mesin Virtual (VM)'?", "id": "Emulasi Sistem Komputer Lengkap." },
  { "en": "Apa Saja Operasi Logika Dasar 'ALU'?", "id": "AND, OR, NOT, XOR." },
  { "en": "Bagaimana Cara Kerja 'Decoder 2-ke-4'?", "id": "Dua Input Mengaktifkan Satu Dari Empat Output." },
  { "en": "Apa Tujuan Utama Antarmuka 'JTAG'?", "id": "Untuk Pengujian Dan Debugging Perangkat Keras." },
  { "en": "Apa Beda 'Finite Automata' Dan 'State Machine'?", "id": "Konsep Yang Sama, Beda Terminologi." },
  { "en": "Apa Itu 'System Call'?", "id": "Mekanisme Program Meminta Layanan OS." },
  { "en": "Bagaimana 'Register' Berbeda Dari 'Cache'?", "id": "Register Di Dalam, Cache Di Luar CPU." },
  { "en": "Apa Peran 'Linker' Dalam Kompilasi?", "id": "Menggabungkan File Objek Menjadi Executable." },
  { "en": "Apa Itu 'Siklus Fetch'?", "id": "Tahap Pengambilan Instruksi Dari Memori." },
  { "en": "Apa Itu 'Siklus Execute'?", "id": "Tahap Menjalankan Instruksi Oleh CPU." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Data Dan Komponen Fungsional CPU." },
  { "en": "Apa Itu 'Control Path'?", "id": "Sirkuit Yang Mengontrol Aliran Datapath." },
  { "en": "Apa Itu 'Sinyal Kontrol'?", "id": "Sinyal Yang Dihasilkan Oleh Unit Kontrol." },
  { "en": "Apa Itu 'Micro-operation'?", "id": "Operasi Elementer Yang Dilakukan CPU." },
  { "en": "Apa Itu 'Register Transfer Language (RTL)'?", "id": "Bahasa Simbolik Mendeskripsikan Transfer Register." },
  { "en": "Apa Itu 'Carry Flag'?", "id": "Indikator Terjadinya Carry Out." },
  { "en": "Apa Itu 'Zero Flag'?", "id": "Indikator Hasil Operasi Adalah Nol." },
  { "en": "Apa Itu 'Sign Flag'?", "id": "Indikator Hasil Operasi Adalah Negatif." },
  { "en": "Apa Itu 'Overflow Flag'?", "id": "Indikator Terjadinya Aritmetika Overflow." },
  { "en": "Apa Itu 'Status Register'?", "id": "Register Yang Menyimpan Flag." },
  { "en": "Nama Lain 'Status Register'?", "id": "Flag Register Atau CCR (Condition Code Register)." },
  { "en": "Apa Itu 'Instruksi Percabangan (Branch)'?", "id": "Instruksi Yang Mengubah Alur Program." },
  { "en": "Apa Itu 'Percabangan Bersyarat (Conditional Branch)'?", "id": "Percabangan Yang Bergantung Pada Flag." },
  { "en": "Apa Itu 'Percabangan Tak Bersyarat (Unconditional)'?", "id": "Percabangan Yang Selalu Terjadi." },
  { "en": "Apa Itu 'Subroutine'?", "id": "Blok Kode Yang Bisa Dipanggil." },
  { "en": "Apa Instruksi Untuk Memanggil 'Subroutine'?", "id": "CALL." },
  { "en": "Apa Instruksi Untuk Kembali Dari 'Subroutine'?", "id": "RETURN." },
  { "en": "Bagaimana 'Stack' Digunakan Dalam Panggilan Subroutine?", "id": "Untuk Menyimpan Alamat Kembali." },
  { "en": "Apa Itu 'Interrupt'?", "id": "Sinyal Eksternal Yang Menghentikan Prosesor." },
  { "en": "Apa Itu 'Interrupt Handler'?", "id": "Kode Yang Menangani Kejadian Interrupt." },
  { "en": "Apa Beda 'Interrupt' Dan 'Subroutine' Call?", "id": "Interrupt Asinkron, Subroutine Sinkron." },
  { "en": "Apa Itu 'Interrupt Masking'?", "id": "Mengabaikan Atau Menonaktifkan Interrupt." },
  { "en": "Apa Itu 'Non-Maskable Interrupt (NMI)'?", "id": "Interrupt Prioritas Tinggi, Tidak Bisa Diabaikan." },
  { "en": "Apa Itu 'Direct Memory Access (DMA)'?", "id": "Transfer Data Langsung Antara I/O-Memori." },
  { "en": "Apa Keuntungan 'DMA (Direct Memory Access)'?", "id": "Mengurangi Beban Kerja CPU." },
  { "en": "Apa Itu 'Siklus Stealing'?", "id": "DMA (Direct Memory Access) 'Mencuri' Siklus Bus." },
  { "en": "Apa Itu 'Bus'?", "id": "Jalur Komunikasi Yang Dibagi." },
  { "en": "Apa Tiga Tipe 'Bus' Utama?", "id": "Alamat, Data, Dan Kontrol." },
  { "en": "Apa Itu 'Lebar Bus'?", "id": "Jumlah Jalur Paralel Dalam Bus." },
  { "en": "Apa Itu 'Sinkronisasi Bus'?", "id": "Transfer Data Dikendalikan Oleh Clock." },
  { "en": "Apa Itu 'Bus Asinkron'?", "id": "Transfer Data Dikendalikan Sinyal Handshake." },
  { "en": "Apa Itu 'Arbiter Bus'?", "id": "Mengatur Perangkat Mana Yang Gunakan Bus." },
  { "en": "Apa Itu 'I/O Terisolasi'?", "id": "Ruang Alamat Terpisah Untuk I/O." },
  { "en": "Apa Itu 'I/O Dipetakan Memori'?", "id": "Berbagi Ruang Alamat Dengan Memori." },
  { "en": "Apa Itu 'Port Paralel'?", "id": "Antarmuka Untuk Transfer Data Paralel." },
  { "en": "Apa Itu 'Port Serial'?", "id": "Antarmuka Untuk Transfer Data Serial." },
  { "en": "Apa Itu 'UART (Universal Asynchronous Receiver-Transmitter)'?", "id": "Sirkuit Untuk Komunikasi Serial Asinkron." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Kecepatan Simbol Dalam Komunikasi Serial." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Sekelompok Gerbang Dengan Teknologi Sama." },
  { "en": "Apa Itu 'Noise Immunity'?", "id": "Kemampuan Sirkuit Menolak Gangguan Derau." },
  { "en": "Apa Itu 'Fan-in'?", "id": "Jumlah Input Sebuah Gerbang Logika." },
  { "en": "Apa Itu 'Fan-out'?", "id": "Jumlah Input Yang Bisa Digerakkan Output." },
  { "en": "Apa Itu 'Power Dissipation'?", "id": "Konsumsi Daya Oleh Gerbang Logika." },
  { "en": "Apa Itu 'Propagation Delay'?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu 'Active Pull-up'?", "id": "Struktur Output Menggunakan Transistor Aktif." },
  { "en": "Apa Itu 'Passive Pull-up'?", "id": "Struktur Output Menggunakan Resistor." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Fungsional Kompleks." },
  { "en": "Apa Itu 'Kapasitansi Input'?", "id": "Kapasitansi Yang Dilihat Di Terminal Input." },
  { "en": "Apa Itu 'Kapasitansi Output'?", "id": "Kapasitansi Yang Dilihat Di Terminal Output." },
  { "en": "Apa Itu 'Decoder 1-dari-N'?", "id": "Nama Lain Untuk Decoder." },
  { "en": "Apa Itu 'Enable'?", "id": "Sinyal Untuk Mengaktifkan Fungsi Sirkuit." },
  { "en": "Apa Itu 'Latch'?", "id": "Elemen Memori Sensitif Terhadap Level." },
  { "en": "Apa Itu 'Transparansi' Latch?", "id": "Output Mengikuti Input Saat Enable Aktif." },
  { "en": "Apa Itu 'Gated SR Latch'?", "id": "Latch SR Dengan Input Enable." },
  { "en": "Apa Itu 'Gated D Latch'?", "id": "Latch D Dengan Input Enable." },
  { "en": "Apa Itu 'Clocked Flip-Flop'?", "id": "Flip-Flop Yang Dikendalikan Tepi Clock." },
  { "en": "Apa Itu 'Rising-Edge Triggered'?", "id": "Dipicu Oleh Transisi Rendah-Ke-Tinggi." },
  { "en": "Apa Itu 'Falling-Edge Triggered'?", "id": "Dipicu Oleh Transisi Tinggi-Ke-Rendah." },
  { "en": "Apa Itu 'Characteristic Equation'?", "id": "Persamaan Yang Mendeskripsikan Perilaku Flip-Flop." },
  { "en": "Apa Itu 'Sequential Logic Design'?", "id": "Proses Merancang Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Reduksi State'?", "id": "Menyederhanakan Jumlah State Dalam FSM." },
  { "en": "Apa Itu 'Penandaan State (State Assignment)'?", "id": "Memberi Kode Biner Pada Setiap State." },
  { "en": "Apa Itu 'Register Geser Universal'?", "id": "Bisa Geser Kiri, Kanan, Muat Paralel." },
  { "en": "Apa Itu 'Register'?", "id": "Sekelompok Flip-flop Yang Menyimpan Word." },
  { "en": "Apa Itu 'Counter Mod-N'?", "id": "Counter Yang Punya N Keadaan." },
  { "en": "Apa Itu 'Dekade Counter'?", "id": "Counter Yang Menghitung Sepuluh Keadaan." },
  { "en": "Apa Itu 'Sistem Memori'?", "id": "Organisasi Komponen Memori Dalam Komputer." },
  { "en": "Apa Itu 'Kepadatan Memori'?", "id": "Jumlah Bit Per Satuan Area Chip." },
  { "en": "Apa Itu 'Sel Memori'?", "id": "Sirkuit Dasar Penyimpan Satu Bit." },
  { "en": "Apa Itu 'Array Memori'?", "id": "Susunan Sel Memori Dalam Baris-Kolom." },
  { "en": "Apa Itu 'Row Decoder'?", "id": "Memilih Baris Dalam Array Memori." },
  { "en": "Apa Itu 'Column Decoder'?", "id": "Memilih Kolom Dalam Array Memori." },
  { "en": "Apa Itu 'Sense Amplifier'?", "id": "Mendeteksi Sinyal Lemah Dari Sel Memori." },
  { "en": "Apa Itu 'Programmable Logic Array (PLA)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND-OR." },
  { "en": "Apa Itu 'Programmable Array Logic (PAL)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND." },
  { "en": "Apa Itu 'Complex Programmable Logic Device (CPLD)'?", "id": "Perangkat Logika Kompleks Berbasis Makrosel." },
  { "en": "Apa Itu 'Makrosel'?", "id": "Blok Fungsional Dalam CPLD." },
  { "en": "Apa Itu 'Konfigurasi'?", "id": "Proses Memuat Desain Ke PLD." },
  { "en": "Apa Itu 'Look-Up Table (LUT)'?", "id": "Memori Kecil Yang Implementasikan Fungsi." },
  { "en": "Apa Itu 'Switch Matrix'?", "id": "Matriks Saklar Untuk Routing Sinyal." },
  { "en": "Apa Itu 'Block RAM'?", "id": "Blok Memori Khusus Di Dalam FPGA." },
  { "en": "Apa Itu 'DSP Slice'?", "id": "Blok Khusus Untuk Operasi DSP." },
  { "en": "Apa Itu 'Hard Core Processor'?", "id": "CPU (Central Processing Unit) Fisik Di FPGA." },
  { "en": "Apa Itu 'Soft Core Processor'?", "id": "CPU (Central Processing Unit) Diimplementasi Di Logika." },
  { "en": "Apa Saja Operasi Dasar Aritmetika 'ALU'?", "id": "Tambah, Kurang, Inkremental, Dekremental." },
  { "en": "Bagaimana 'Decoder BCD ke 7-Segmen' Bekerja?", "id": "Mengubah 4-bit BCD (Binary-Coded Decimal) Ke Sinyal Layar." },
  { "en": "Apa Fungsi Pin 'TDI' Pada JTAG?", "id": "Test Data In (Input Data Uji)." },
  { "en": "Apa Fungsi Pin 'TDO' Pada JTAG?", "id": "Test Data Out (Output Data Uji)." },
  { "en": "Apa Fungsi Pin 'TCK' Pada JTAG?", "id": "Test Clock (Sinyal Clock Untuk Uji)." },
  { "en": "Apa Fungsi Pin 'TMS' Pada JTAG?", "id": "Test Mode Select (Memilih Mode Uji)." },
  { "en": "Apa Itu 'Interrupt Controller'?", "id": "Mengelola Prioritas Dan Sinyal Interrupt." },
  { "en": "Apa Itu 'System Call'?", "id": "Cara Program Aplikasi Meminta Layanan OS." },
  { "en": "Apa Itu 'Mesin Virtual (VM)'?", "id": "Emulasi Perangkat Keras Komputer." },
  { "en": "Apa Itu 'Hypervisor'?", "id": "Perangkat Lunak Yang Membuat Dan Menjalankan VM." },
  { "en": "Apa Itu 'Kernel Mode'?", "id": "Mode Operasi CPU (Central Processing Unit) Dengan Hak Penuh." },
  { "en": "Apa Itu 'User Mode'?", "id": "Mode Operasi CPU (Central Processing Unit) Dengan Hak Terbatas." },
  { "en": "Apa Peran 'Linker'?", "id": "Menggabungkan File Objek Dan Pustaka." },
  { "en": "Apa Peran 'Loader'?", "id": "Memuat Program Ke Memori Untuk Eksekusi." },
  { "en": "Apa Itu 'Dynamic Linking'?", "id": "Menautkan Pustaka Saat Program Dijalankan." },
  { "en": "Apa Itu 'Static Linking'?", "id": "Menautkan Pustaka Saat Kompilasi." },
  { "en": "Apa Itu 'Shared Library'?", "id": "Pustaka Yang Dibagi Antar Banyak Program." },
  { "en": "Apa Itu 'Race Condition'?", "id": "Hasil Bergantung Pada Urutan Kejadian." },
  { "en": "Bagaimana 'Metastability' Diselesaikan?", "id": "Menunggu Cukup Lama Hingga Stabil." },
  { "en": "Apa Itu 'Synchronizer'?", "id": "Sirkuit Untuk Menangani Sinyal Asinkron." },
  { "en": "Apa Itu 'Gray Code Counter'?", "id": "Counter Yang Outputnya Dalam Kode Gray." },
  { "en": "Apa Itu 'Decoder Alamat'?", "id": "Memilih Perangkat Berdasarkan Alamat Bus." },
  { "en": "Apa Itu 'Memory-mapped I/O'?", "id": "Perangkat I/O Diperlakukan Seperti Memori." },
  { "en": "Apa Itu 'Boolean Variable'?", "id": "Variabel Yang Hanya Bisa Benar/Salah." },
  { "en": "Apa Itu 'Logika Proposisional'?", "id": "Cabang Logika Yang Berurusan Proposisi." },
  { "en": "Apa Itu 'Tautologi'?", "id": "Ekspresi Yang Selalu Benar." },
  { "en": "Apa Itu 'Kontradiksi'?", "id": "Ekspresi Yang Selalu Salah." },
  { "en": "Apa Itu 'Penjumlahan Biner Setengah (Half Adder)'?", "id": "Menjumlahkan Dua Bit Tanpa Carry-in." },
  { "en": "Apa Itu 'Penjumlahan Biner Penuh (Full Adder)'?", "id": "Menjumlahkan Dua Bit Dengan Carry-in." },
  { "en": "Apa Itu 'Carry-out'?", "id": "Bit Simpanan Hasil Penjumlahan." },
  { "en": "Apa Itu 'Sum Bit'?", "id": "Bit Hasil Penjumlahan." },
  { "en": "Apa Itu 'Penjumlah 4-bit'?", "id": "Rangkaian Yang Menjumlahkan Dua Angka 4-bit." },
  { "en": "Bagaimana Membuat 'Penjumlah 4-bit'?", "id": "Dengan Merangkai Empat Full Adder." },
  { "en": "Apa Itu 'Penjumlah-Pengurang (Adder-Subtractor)'?", "id": "Sirkuit Yang Bisa Menjumlah Atau Mengurang." },
  { "en": "Bagaimana 'Pengurangan' Dilakukan Dengan Penjumlah?", "id": "Menggunakan Komplemen Dua Dari Subtrahend." },
  { "en": "Apa Itu 'Logic Array'?", "id": "Array Gerbang AND Dan OR Terprogram." },
  { "en": "Apa Itu 'Makrosel'?", "id": "Blok Logika Fungsional Dalam PLD." },
  { "en": "Apa Itu 'Jalur Cepat (Fast Path)'?", "id": "Logika Khusus Untuk Carry Cepat." },
  { "en": "Apa Itu 'Arsitektur Slice'?", "id": "Struktur Kolom Dalam FPGA Modern." },
  { "en": "Apa Itu 'Clock Buffer'?", "id": "Buffer Untuk Mendistribusikan Sinyal Clock." },
  { "en": "Apa Itu 'Clock Tree'?", "id": "Jaringan Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Siklus Tunggu (Wait State)'?", "id": "Penundaan Siklus Clock Untuk Sinkronisasi." },
  { "en": "Apa Itu 'Burst Transfer'?", "id": "Transfer Blok Data Berurutan Cepat." },
  { "en": "Apa Itu 'Cache Controller'?", "id": "Sirkuit Logika Pengelola Operasi Cache." },
  { "en": "Apa Itu 'Cache Policy'?", "id": "Aturan Untuk Manajemen Cache." },
  { "en": "Apa Itu 'Cache Coherence Protocol'?", "id": "Protokol Penjaga Konsistensi Antar Cache." },
  { "en": "Apa Itu 'Snooping'?", "id": "Proses Cache Memantau Aktivitas Bus." },
  { "en": "Apa Itu 'Directory-Based Protocol'?", "id": "Protokol Koherensi Cache Tersentralisasi." },
  { "en": "Apa Itu 'False Sharing'?", "id": "Invalidasi Cache Akibat Data Tak Terkait." },
  { "en": "Apa Itu 'Translation Lookaside Buffer (TLB)'?", "id": "Cache Untuk Terjemahan Alamat Memori." },
  { "en": "Apa Itu 'Page Hit'?", "id": "Alamat Virtual Ditemukan Di TLB." },
  { "en": "Apa Itu 'Page Miss'?", "id": "Alamat Virtual Tidak Ditemukan Di TLB." },
  { "en": "Apa Itu 'Virtualisasi Perangkat Keras'?", "id": "Membuat Representasi Virtual Dari Hardware." },
  { "en": "Apa Itu 'Mesin Virtual (VM)'?", "id": "Sistem Komputer Virtual Di Atas Fisik." },
  { "en": "Apa Itu 'Mode Supervisor'?", "id": "Nama Lain Untuk Kernel Mode." },
  { "en": "Apa Itu 'Manajemen Proses'?", "id": "Tugas OS (Operating System) Mengelola Proses." },
  { "en": "Apa Itu 'Penjadwal (Scheduler)'?", "id": "Bagian OS (Operating System) Yang Pilih Proses." },
  { "en": "Apa Itu 'Time Slice'?", "id": "Jatah Waktu CPU Untuk Satu Proses." },
  { "en": "Apa Itu 'Round-Robin Scheduling'?", "id": "Setiap Proses Mendapat Jatah Waktu." },
  { "en": "Apa Itu 'Preemption'?", "id": "Proses Diinterupsi Paksa Oleh OS." },
  { "en": "Apa Itu 'Context Switch'?", "id": "Peralihan CPU (Central Processing Unit) Antar Proses." },
  { "en": "Apa Itu 'Thread Safe'?", "id": "Kode Aman Digunakan Banyak Thread." },
  { "en": "Apa Itu 'Race Condition'?", "id": "Hasil Bergantung Urutan Eksekusi Thread." },
  { "en": "Apa Itu 'Critical Section'?", "id": "Bagian Kode Yang Harus Dijalankan Utuh." },
  { "en": "Apa Itu 'Mutual Exclusion (Mutex)'?", "id": "Memastikan Hanya Satu Thread Masuk." },
  { "en": "Apa Itu 'Semaphore'?", "id": "Variabel Sinkronisasi Untuk Penghitungan." },
  { "en": "Apa Itu 'Produser-Konsumer Problem'?", "id": "Masalah Klasik Sinkronisasi Proses." },
  { "en": "Apa Itu 'Dining Philosophers Problem'?", "id": "Masalah Klasik Deadlock Dan Sinkronisasi." },
  { "en": "Apa Itu 'Fragmentasi Internal'?", "id": "Ruang Terbuang Di Dalam Blok Memori." },
  { "en": "Apa Itu 'Fragmentasi Eksternal'?", "id": "Ruang Terbuang Di Antara Blok Memori." },
  { "en": "Apa Itu 'Compaction' Memori?", "id": "Proses Menggabungkan Blok Memori Kosong." },
  { "en": "Apa Itu 'Paging'?", "id": "Membagi Memori Menjadi Blok Ukuran Sama." },
  { "en": "Apa Itu 'Segmentation'?", "id": "Membagi Memori Menjadi Blok Logis." },
  { "en": "Apa Itu 'File Descriptor'?", "id": "Penunjuk Abstrak Ke Sebuah File." },
  { "en": "Apa Itu 'Inode'?", "id": "Struktur Data Penyimpan Metadata File." },
  { "en": "Apa Itu 'Hard Link'?", "id": "Penunjuk Langsung Ke Inode." },
  { "en": "Apa Itu 'Soft Link (Symbolic Link)'?", "id": "Penunjuk Ke Nama File Lain." },
  { "en": "Apa Itu 'Mount Point'?", "id": "Direktori Titik Kait Sistem File." },
  { "en": "Apa Itu 'Boot Sector'?", "id": "Sektor Pertama Disk Berisi Bootloader." },
  { "en": "Apa Itu 'Master Boot Record (MBR)'?", "id": "Tipe Boot Sector Tradisional." },
  { "en": "Apa Itu 'GUID Partition Table (GPT)'?", "id": "Standar Partisi Modern Pengganti MBR." },
  { "en": "Apa Itu 'Sistem File Jaringan'?", "id": "Sistem File Yang Diakses Lewat Jaringan." },
  { "en": "Apa Kepanjangan 'NFS (Network File System)'?", "id": "Sistem File Jaringan." },
  { "en": "Apa Itu 'Blok Perangkat (Block Device)'?", "id": "Perangkat Yang Transfer Data Per Blok." },
  { "en": "Apa Itu 'Karakter Perangkat (Character Device)'?", "id": "Perangkat Yang Transfer Data Per Karakter." },
  { "en": "Apa Itu 'Device Tree'?", "id": "Deskripsi Perangkat Keras Untuk Kernel." },
  { "en": "Apa Itu 'Modul Kernel'?", "id": "Kode Yang Bisa Dimuat-Lepas Dari Kernel." },
  { "en": "Apa Itu 'Shell'?", "id": "Antarmuka Baris Perintah Ke OS." },
  { "en": "Apa Itu 'GUI (Graphical User Interface)'?", "id": "Antarmuka Pengguna Grafis." },
  { "en": "Apa Itu 'Desktop Environment'?", "id": "Kumpulan GUI (Graphical User Interface) Lengkap." },
  { "en": "Apa Itu 'Register Instruksi'?", "id": "Register Yang Menyimpan Instruksi Saat Ini." },
  { "en": "Apa Itu 'Program Counter'?", "id": "Register Yang Menyimpan Alamat Instruksi Berikutnya." },
  { "en": "Apa Itu 'Decoder Instruksi'?", "id": "Menerjemahkan Opcode Menjadi Sinyal Kontrol." },
  { "en": "Apa Itu 'Siklus Fetch-Decode-Execute'?", "id": "Siklus Dasar Operasi CPU." },
  { "en": "Apa Itu 'Arithmatic Shift'?", "id": "Pergeseran Biner Yang Mempertahankan Tanda." },
  { "en": "Apa Itu 'Logical Shift'?", "id": "Pergeseran Biner Yang Mengisi Dengan Nol." },
  { "en": "Apa Itu 'Rotate Through Carry'?", "id": "Operasi Rotasi Bit Yang Melibatkan Carry." },
  { "en": "Apa Itu 'Floating Point Unit (FPU)'?", "id": "Unit Khusus Untuk Aritmetika Floating-Point." },
  { "en": "Apa Itu 'Representasi IEEE 754'?", "id": "Standar Untuk Representasi Floating-Point." },
  { "en": "Apa Tiga Bagian 'Format IEEE 754'?", "id": "Tanda (Sign), Eksponen, Dan Mantissa." },
  { "en": "Apa Itu 'Normalization'?", "id": "Proses Menstandarkan Format Angka." },
  { "en": "Apa Itu 'Denormalized Number'?", "id": "Angka Sangat Kecil Dekat Nol." },
  { "en": "Apa Itu 'Infinity' Dalam Floating-Point?", "id": "Representasi Nilai Tak Terhingga." },
  { "en": "Apa Itu 'NaN (Not a Number)'?", "id": "Representasi Hasil Operasi Tidak Valid." },
  { "en": "Apa Itu 'Precision'?", "id": "Jumlah Digit Signifikan Dalam Angka." },
  { "en": "Apa Itu 'Single Precision'?", "id": "Format Floating-Point 32-bit." },
  { "en": "Apa Itu 'Double Precision'?", "id": "Format Floating-Point 64-bit." },
  { "en": "Apa Itu 'Pipeline Hazard'?", "id": "Kondisi Yang Mencegah Instruksi Berikutnya." },
  { "en": "Apa Itu 'Data Forwarding'?", "id": "Teknik Mengurangi Stall Akibat Data Hazard." },
  { "en": "Apa Itu 'Pipeline Bubble' Atau 'Stall'?", "id": "Penundaan Dalam Pipeline." },
  { "en": "Apa Itu 'Branch Delay Slot'?", "id": "Instruksi Setelah Branch Yang Selalu Dieksekusi." },
  { "en": "Apa Itu 'Static Branch Prediction'?", "id": "Prediksi Cabang Dilakukan Oleh Compiler." },
  { "en": "Apa Itu 'Dynamic Branch Prediction'?", "id": "Prediksi Cabang Dilakukan Oleh Hardware." },
  { "en": "Apa Itu 'Branch Target Buffer'?", "id": "Cache Yang Menyimpan Alamat Tujuan Branch." },
  { "en": "Apa Itu 'Loop Unrolling'?", "id": "Teknik Optimisasi Mengurangi Overhead Loop." },
  { "en": "Apa Itu 'Out-of-Order Execution'?", "id": "Eksekusi Instruksi Berdasarkan Ketersediaan Data." },
  { "en": "Apa Itu 'Tomasulo Algorithm'?", "id": "Algoritma Untuk Eksekusi Out-of-Order." },
  { "en": "Apa Itu 'Re-order Buffer'?", "id": "Menyimpan Hasil Untuk Commit Sesuai Urutan." },
  { "en": "Apa Itu 'Register Renaming'?", "id": "Menghilangkan Hazard Dengan Mengganti Nama Register." },
  { "en": "Apa Itu 'Speculative Execution'?", "id": "Mengeksekusi Instruksi Sebelum Pasti Dibutuhkan." },
  { "en": "Apa Itu 'Memory Hierarchy'?", "id": "Tingkatan Memori Berdasarkan Kecepatan-Kapasitas." },
  { "en": "Apa Prinsip Dibalik 'Memory Hierarchy'?", "id": "Prinsip Lokalitas Referensi." },
  { "en": "Apa Itu 'Cache Associativity'?", "id": "Fleksibilitas Penempatan Blok Di Cache." },
  { "en": "Apa Itu 'N-Way Set Associative'?", "id": "Blok Bisa Ditempatkan Di N Lokasi." },
  { "en": "Apa Itu 'Cache Replacement Policy'?", "id": "Aturan Memilih Blok Untuk Dikeluarkan." },
  { "en": "Apa Itu 'Write Buffer'?", "id": "Buffer Untuk Menampung Operasi Tulis." },
  { "en": "Apa Itu 'Victim Cache'?", "id": "Cache Kecil Penyimpan Blok Yang Dikeluarkan." },
  { "en": "Apa Itu 'Prefetching'?", "id": "Mengambil Data Sebelum Diminta Secara Eksplisit." },
  { "en": "Apa Itu 'RAID (Redundant Array of Independent Disks)'?", "id": "Menggabungkan Beberapa Disk Fisik." },
  { "en": "Apa Tujuan 'RAID 0'?", "id": "Striping Untuk Peningkatan Kinerja." },
  { "en": "Apa Tujuan 'RAID 1'?", "id": "Mirroring Untuk Redundansi Data." },
  { "en": "Apa Itu 'RAID 5'?", "id": "Striping Dengan Paritas Terdistribusi." },
  { "en": "Apa Itu 'Hot Swapping'?", "id": "Mengganti Komponen Tanpa Mematikan Sistem." },
  { "en": "Apa Itu 'Bus Clock'?", "id": "Clock Yang Mengatur Kecepatan Transfer Bus." },
  { "en": "Apa Itu 'Bus Protocol'?", "id": "Aturan Untuk Komunikasi Di Atas Bus." },
  { "en": "Apa Itu 'Bus Bridge'?", "id": "Menghubungkan Dua Bus Yang Berbeda." },
  { "en": "Apa Itu 'I/O Controller Hub (ICH)'?", "id": "Chipset Pengatur Perangkat I/O Lambat." },
  { "en": "Apa Itu 'Northbridge'?", "id": "Chipset Pengatur Komunikasi Cepat (CPU, RAM)." },
  { "en": "Apa Itu 'Southbridge'?", "id": "Chipset Pengatur Komunikasi Lambat (I/O)." },
  { "en": "Apa Itu 'Chipset'?", "id": "Sekumpulan Sirkuit Terpadu Papan Induk." },
  { "en": "Apa Itu 'Sistem Operasi'?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Itu 'Kernel'?", "id": "Inti Dari Sebuah Sistem Operasi." },
  { "en": "Apa Itu 'Process Control Block (PCB)'?", "id": "Struktur Data Penyimpan Info Proses." },
  { "en": "Apa Itu 'Zombie Process'?", "id": "Proses Selesai Yang Belum Dibersihkan." },
  { "en": "Apa Itu 'Orphan Process'?", "id": "Proses Yang Induknya Telah Berakhir." },
  { "en": "Apa Itu 'System Call Interface'?", "id": "Antarmuka Antara Aplikasi Dan Kernel." },
  { "en": "Apa Itu 'Manajemen Memori'?", "id": "Tugas OS (Operating System) Mengatur Alokasi Memori." },
  { "en": "Apa Itu 'Alokasi Kontigu'?", "id": "Memberi Satu Blok Memori Berurutan." },
  { "en": "Apa Itu 'Alokasi Non-Kontigu'?", "id": "Memberi Blok Memori Yang Tersebar." },
  { "en": "Apa Itu 'File'?", "id": "Kumpulan Informasi Terkait Di Penyimpanan." },
  { "en": "Apa Itu 'Direktori'?", "id": "Struktur Untuk Mengorganisir File." },
  { "en": "Apa Itu 'Sistem File'?", "id": "Metode Penyimpanan Dan Organisasi File." },
  { "en": "Apa Itu 'Metadata'?", "id": "Data Tentang Data (Ukuran, Tanggal)." },
  { "en": "Apa Itu 'Hak Akses File'?", "id": "Izin Baca, Tulis, Eksekusi." },
  { "en": "Apa Itu 'Virtualisasi'?", "id": "Membuat Versi Virtual Dari Sumber Daya." },
  { "en": "Apa Itu 'Hypervisor'?", "id": "Perangkat Lunak Pembuat Dan Pengelola VM." },
  { "en": "Apa Itu 'Virtual Machine Monitor (VMM)'?", "id": "Nama Lain Untuk Hypervisor." },
  { "en": "Apa Itu 'Emulasi'?", "id": "Meniru Perilaku Sistem Lain." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Memodelkan Perilaku Sistem." },
  { "en": "Apa Beda 'Emulasi' Dan 'Simulasi'?", "id": "Emulasi Tiru Fungsi, Simulasi Tiru Perilaku." },
  { "en": "Apa Itu 'Jaringan Komputer'?", "id": "Kumpulan Komputer Yang Saling Terhubung." },
  { "en": "Apa Itu 'Topologi Jaringan'?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
  { "en": "Apa Itu 'Topologi Bus'?", "id": "Semua Perangkat Terhubung Ke Satu Kabel." },
  { "en": "Apa Itu 'Topologi Bintang'?", "id": "Semua Perangkat Terhubung Ke Pusat." },
  { "en": "Apa Itu 'Topologi Cincin'?", "id": "Perangkat Terhubung Dalam Lingkaran." },
  { "en": "Apa Itu 'Topologi Mesh'?", "id": "Banyak Perangkat Saling Terhubung." },
  { "en": "Apa Itu 'LAN (Local Area Network)'?", "id": "Jaringan Area Lokal." },
  { "en": "Apa Itu 'WAN (Wide Area Network)'?", "id": "Jaringan Area Luas." },
  { "en": "Apa Itu 'Internet'?", "id": "Jaringan Global Dari Jaringan." },
  { "en": "Apa Itu 'Model OSI (Open Systems Interconnection)'?", "id": "Model Konseptual Tujuh Lapis Jaringan." },
  { "en": "Apa Itu 'Model TCP/IP'?", "id": "Model Empat Lapis Yang Digunakan Internet." },
  { "en": "Apa Itu 'Encapsulation'?", "id": "Proses Membungkus Data Dengan Header." },
  { "en": "Apa Itu 'Decapsulation'?", "id": "Proses Membuka Bungkusan Header." },
  { "en": "Apa Itu 'Alamat MAC (Media Access Control)'?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
  { "en": "Apa Itu 'Alamat IP (Internet Protocol)'?", "id": "Alamat Logis Perangkat Di Jaringan." },
  { "en": "Apa Itu 'Port Number'?", "id": "Nomor Pengidentifikasi Aplikasi Di Host." },
  { "en": "Apa Itu 'Socket'?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Itu 'Domain Name System (DNS)'?", "id": "Sistem Penamaan Domain." },
  { "en": "Apa Fungsi 'DNS'?", "id": "Menerjemahkan Nama Domain Menjadi Alamat IP." },
  { "en": "Apa Itu 'DHCP (Dynamic Host Configuration Protocol)'?", "id": "Protokol Konfigurasi Host Dinamis." },
  { "en": "Apa Fungsi 'DHCP'?", "id": "Memberi Alamat IP Secara Otomatis." },
  { "en": "Apa Itu 'NAT (Network Address Translation)'?", "id": "Translasi Alamat Jaringan." }



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
