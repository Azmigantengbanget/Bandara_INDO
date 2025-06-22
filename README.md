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


    { "en": "Bandar Udara Internasional Soekarno-Hatta?", "id": "Tangerang (Banten)." },
    { "en": "Bandar Udara Internasional I Gusti Ngurah Rai?", "id": "Badung (Bali)." },
    { "en": "Bandar Udara Internasional Juanda?", "id": "Sidoarjo (Jawa Timur)." },
    { "en": "Bandar Udara Internasional Kualanamu?", "id": "Deli Serdang (Sumatera Utara)." },
    { "en": "Bandar Udara Internasional Sultan Hasanuddin?", "id": "Maros (Sulawesi Selatan)." },
    { "en": "Bandar Udara Internasional Yogyakarta?", "id": "Kulon Progo (DI Yogyakarta)." },
    { "en": "Bandar Udara Internasional Sultan Aji Muhammad Sulaiman Sepinggan?", "id": "Balikpapan (Kalimantan Timur)." },
    { "en": "Bandar Udara Internasional Minangkabau?", "id": "Padang Pariaman (Sumatera Barat)." },
    { "en": "Bandar Udara Internasional Sam Ratulangi?", "id": "Manado (Sulawesi Utara)." },
    { "en": "Bandar Udara Internasional Sultan Mahmud Badaruddin II?", "id": "Palembang (Sumatera Selatan)." },
    { "en": "Bandar Udara Halim Perdanakusuma?", "id": "Jakarta Timur (DKI Jakarta)." },
    { "en": "Bandar Udara Internasional Jenderal Ahmad Yani?", "id": "Semarang (Jawa Tengah)." },
    { "en": "Bandar Udara Internasional Adi Soemarmo?", "id": "Surakarta (Jawa Tengah)." },
    { "en": "Bandar Udara Internasional Husein Sastranegara?", "id": "Bandung (Jawa Barat)." },
    { "en": "Bandar Udara Internasional Kertajati?", "id": "Majalengka (Jawa Barat)." },
    { "en": "Bandar Udara Internasional Lombok (Zainuddin Abdul Madjid)?", "id": "Lombok Tengah (Nusa Tenggara Barat)." },
    { "en": "Bandar Udara Internasional Hang Nadim?", "id": "Batam (Kepulauan Riau)." },
    { "en": "Bandar Udara Internasional Sultan Syarif Kasim II?", "id": "Pekanbaru (Riau)." },
    { "en": "Bandar Udara Internasional Supadio?", "id": "Pontianak (Kalimantan Barat)." },
    { "en": "Bandar Udara Internasional Syamsudin Noor?", "id": "Banjarmasin (Kalimantan Selatan)." },
    { "en": "Bandar Udara Internasional Sentani?", "id": "Jayapura (Papua)." },
    { "en": "Bandar Udara Internasional Frans Kaisiepo?", "id": "Biak Numfor (Papua)." },
    { "en": "Bandar Udara Internasional El Tari?", "id": "Kupang (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Internasional Pattimura?", "id": "Ambon (Maluku)." },
    { "en": "Bandar Udara Internasional Sultan Iskandar Muda?", "id": "Banda Aceh (Aceh)." },
    { "en": "Bandar Udara Internasional Adisutjipto?", "id": "Sleman (DI Yogyakarta)." },
    { "en": "Bandar Udara Internasional Radin Inten II?", "id": "Lampung Selatan (Lampung)." },
    { "en": "Bandar Udara Sultan Thaha?", "id": "Jambi (Jambi)." },
    { "en": "Bandar Udara Fatmawati Soekarno?", "id": "Bengkulu (Bengkulu)." },
    { "en": "Bandar Udara Depati Amir?", "id": "Pangkal Pinang (Kepulauan Bangka Belitung)." },
    { "en": "Bandar Udara H.A.S. Hanandjoeddin?", "id": "Tanjung Pandan (Kepulauan Bangka Belitung)." },
    { "en": "Bandar Udara Raja Haji Fisabilillah?", "id": "Tanjung Pinang (Kepulauan Riau)." },
    { "en": "Bandar Udara Abdulrachman Saleh?", "id": "Malang (Jawa Timur)." },
    { "en": "Bandar Udara Banyuwangi?", "id": "Banyuwangi (Jawa Timur)." },
    { "en": "Bandar Udara Notohadinegoro?", "id": "Jember (Jawa Timur)." },
    { "en": "Bandar Udara Tunggul Wulung?", "id": "Cilacap (Jawa Tengah)." },
    { "en": "Bandar Udara Komodo?", "id": "Labuan Bajo (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Mutiara SIS Al-Jufri?", "id": "Palu (Sulawesi Tengah)." },
    { "en": "Bandar Udara Haluoleo?", "id": "Kendari (Sulawesi Tenggara)." },
    { "en": "Bandar Udara Jalaluddin?", "id": "Gorontalo (Gorontalo)." },
    { "en": "Bandar Udara Tampa Padang?", "id": "Mamuju (Sulawesi Barat)." },
    { "en": "Bandar Udara Sultan Babullah?", "id": "Ternate (Maluku Utara)." },
    { "en": "Bandar Udara Tjilik Riwut?", "id": "Palangkaraya (Kalimantan Tengah)." },
    { "en": "Bandar Udara Aji Pangeran Tumenggung Pranoto?", "id": "Samarinda (Kalimantan Timur)." },
    { "en": "Bandar Udara Internasional Juwata?", "id": "Tarakan (Kalimantan Utara)." },
    { "en": "Bandar Udara Mopah?", "id": "Merauke (Papua Selatan)." },
    { "en": "Bandar Udara Internasional Mozes Kilangin?", "id": "Timika (Papua Tengah)." },
    { "en": "Bandar Udara Wamena?", "id": "Wamena (Papua Pegunungan)." },
    { "en": "Bandar Udara Rendani?", "id": "Manokwari (Papua Barat)." },
    { "en": "Bandar Udara Domine Eduard Osok?", "id": "Sorong (Papua Barat Daya)." },
    { "en": "Bandar Udara Ferdinand Lumban Tobing?", "id": "Sibolga (Tapanuli Tengah)." },
    { "en": "Bandar Udara Internasional Sisingamangaraja XII?", "id": "Silangit (Tapanuli Utara)." },
    { "en": "Bandar Udara Pinang Kampai?", "id": "Dumai (Riau)." },
    { "en": "Bandar Udara Depati Parbo?", "id": "Kerinci (Jambi)." },
    { "en": "Bandar Udara Atung Bungsu?", "id": "Pagar Alam (Sumatera Selatan)." },
    { "en": "Bandar Udara Cakrabhuwana?", "id": "Cirebon (Jawa Barat)." },
    { "en": "Bandar Udara Trunojoyo?", "id": "Sumenep (Jawa Timur)." },
    { "en": "Bandar Udara Sultan Muhammad Salahuddin?", "id": "Bima (Nusa Tenggara Barat)." },
    { "en": "Bandar Udara H. Hasan Aroeboesman?", "id": "Ende (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Frans Seda?", "id": "Maumere (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Kasiguncu?", "id": "Poso (Sulawesi Tengah)." },
    { "en": "Bandar Udara Syukuran Aminuddin Amir?", "id": "Luwuk (Sulawesi Tengah)." },
    { "en": "Bandar Udara Betoambari?", "id": "Bau-Bau (Sulawesi Tenggara)." },
    { "en": "Bandar Udara Gusti Sjamsir Alam?", "id": "Kotabaru (Kalimantan Selatan)." },
    { "en": "Bandar Udara Kalimarau?", "id": "Berau (Kalimantan Timur)." },
    { "en": "Bandar Udara Robert Atty Bessing?", "id": "Malinau (Kalimantan Utara)." },
    { "en": "Bandar Udara Malikus Saleh?", "id": "Lhokseumawe (Aceh)." },
    { "en": "Bandar Udara Aek Godang?", "id": "Padang Sidempuan (Sumatera Utara)." },
    { "en": "Bandar Udara Douw Aturure?", "id": "Nabire (Papua Tengah)." },
    { "en": "Bandar Udara Siboru?", "id": "Fakfak (Papua Barat)." },
    { "en": "Bandar Udara Marinda?", "id": "Raja Ampat (Papua Barat Daya)." },
    { "en": "Bandar Udara Dewadaru?", "id": "Karimunjawa, Jepara (Jawa Tengah)." },
    { "en": "Bandar Udara H. Asan?", "id": "Sampit, Kotawaringin Timur (Kalimantan Tengah)." },
    { "en": "Bandar Udara Matahora?", "id": "Wakatobi (Sulawesi Tenggara)." },
    { "en": "Bandar Udara Mau Hau?", "id": "Waingapu, Sumba Timur (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Karel Sadsuitubun?", "id": "Langgur, Maluku Tenggara (Maluku)." },
    { "en": "Bandar Udara Mathilda Batlayeri?", "id": "Saumlaki, Kepulauan Tanimbar (Maluku)." },
    { "en": "Bandar Udara Dabo?", "id": "Singkep, Lingga (Kepulauan Riau)." },
    { "en": "Bandar Udara Oksibil?", "id": "Oksibil, Pegunungan Bintang (Papua Pegunungan)." },
    { "en": "Bandar Udara Nop Goliat?", "id": "Dekai, Yahukimo (Papua Pegunungan)." },
    { "en": "Bandar Udara Ewer?", "id": "Asmat (Papua Selatan)." },
    { "en": "Bandar Udara D. C. Saudale?", "id": "Rote Ndao (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Gewayantana?", "id": "Larantuka, Flores Timur (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Soa?", "id": "Bajawa, Ngada (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Alor?", "id": "Alor (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara H. Aroeppala?", "id": "Kepulauan Selayar (Sulawesi Selatan)." },
    { "en": "Bandar Udara Pogogul?", "id": "Buol (Sulawesi Tengah)." },
    { "en": "Bandar Udara Andi Jemma?", "id": "Masamba, Luwu Utara (Sulawesi Selatan)." },
    { "en": "Bandar Udara Pitu?", "id": "Morotai (Maluku Utara)." },
    { "en": "Bandar Udara Naha?", "id": "Tahuna, Kepulauan Sangihe (Sulawesi Utara)." },
    { "en": "Bandar Udara Melonguane?", "id": "Kepulauan Talaud (Sulawesi Utara)." },
    { "en": "Bandar Udara Pangsuma?", "id": "Putussibau, Kapuas Hulu (Kalimantan Barat)." },
    { "en": "Bandar Udara Rahadi Oesman?", "id": "Ketapang (Kalimantan Barat)." },
    { "en": "Bandar Udara Long Apung?", "id": "Malinau (Kalimantan Utara)." },
    { "en": "Bandar Udara Torea?", "id": "Fakfak (Papua Barat)." },
    { "en": "Bandar Udara Utarom?", "id": "Kaimana (Papua Barat)." },
    { "en": "Bandar Udara Letung?", "id": "Jemaja, Kepulauan Anambas (Kepulauan Riau)." },
    { "en": "Bandar Udara Cut Nyak Dhien?", "id": "Nagan Raya (Aceh)." },
    { "en": "Bandar Udara Budiarto?", "id": "Curug, Tangerang (Banten)." },
    { "en": "Bandar Udara Wirasaba?", "id": "Purbalingga (Jawa Tengah)." },
    { "en": "Bandar Udara Iswahyudi?", "id": "Madiun (Jawa Timur)." },
    { "en": "Bandar Udara Bua?", "id": "Luwu (Sulawesi Selatan)." },
    { "en": "Bandar Udara Sangia Nibandera?", "id": "Kolaka (Sulawesi Tenggara)." },
    { "en": "Bandar Udara Tambolaka?", "id": "Sumba Barat Daya (Nusa Tenggara Timur)." },
    { "en": "Bandar Udara Warukin?", "id": "Tabalong (Kalimantan Selatan)." },
    { "en": "Bandar Udara H. Muhammad Sidik?", "id": "Barito Utara (Kalimantan Tengah)." },
    { "en": "Bandar Udara Weda Bay?", "id": "Halmahera Tengah (Maluku Utara)." },
    { "en": "Bandar Udara Emalamo?", "id": "Sanana, Kepulauan Sula (Maluku Utara)." },
    { "en": "Bandar Udara Rar Gwamar?", "id": "Dobo, Kepulauan Aru (Maluku)." },
    { "en": "Bandar Udara Namrole?", "id": "Buru Selatan (Maluku)." },
    { "en": "Bandar Udara Mukomuko?", "id": "Mukomuko (Bengkulu)." },
    { "en": "Bandar Udara Pasir Pangaraian?", "id": "Rokan Hulu (Riau)." },
    { "en": "Bandar Udara Lasondre?", "id": "Nias Selatan (Sumatera Utara)." },
    { "en": "Bandar Udara Atang Sendjaja?", "id": "Semplak, Bogor (Jawa Barat)." },
    { "en": "Bandar Udara Sugapa?", "id": "Intan Jaya (Papua Tengah)." },
    { "en": "Bandar Udara Tiom?", "id": "Lanny Jaya (Papua Pegunungan)." },
    { "en": "Bandar Udara Ilaga?", "id": "Puncak (Papua Tengah)." },
    { "en": "Bandar Udara Sumarorong?", "id": "Mamasa (Sulawesi Barat)." },
    { "en": "Bandar Udara Toronda?", "id": "Tojo Una-Una (Sulawesi Tengah)." },
    { "en": "Bandar Udara Soroako?", "id": "Luwu Timur (Sulawesi Selatan)." },
    { "en": "Bandar Udara Nusawiru?", "id": "Pangandaran (Jawa Barat)." },
    { "en": "Bandar Udara Pasirian?", "id": "Lumajang (Jawa Timur)." },
    { "en": "Bandar Udara Beringin?", "id": "Muara Teweh (Kalimantan Tengah)." },
    { "en": "Bandar Udara Senipah?", "id": "Kutai Kartanegara (Kalimantan Timur)." },
    { "en": "Bandar Udara Karubaga?", "id": "Karubaga, Tolikara (Papua Pegunungan)." },
    { "en": "Bandar Udara Enarotali?", "id": "Enarotali, Paniai (Papua Tengah)." },
    { "en": "Bandar Udara Raden Sadjad?", "id": "Ranai, Natuna (Kepulauan Riau)." },
    { "en": "Bandar Udara Tanah Merah?", "id": "Tanah Merah, Boven Digoel (Papua Selatan)." },
    { "en": "Bandar Udara Rokot?", "id": "Sipora, Kepulauan Mentawai (Sumatera Barat)." },
    { "en": "Bandar Udara Melalan?", "id": "Melak, Kutai Barat (Kalimantan Timur)." },
    { "en": "Bandar Udara Datah Dawai?", "id": "Long Pahangai, Mahakam Ulu (Kalimantan Timur)." },
    { "en": "Bandar Udara Gading?", "id": "Playen, Gunungkidul (DI Yogyakarta)." },
    { "en": "Bandar Udara Buli?", "id": "Maba, Halmahera Timur (Maluku Utara)." },
    { "en": "Bandar Udara Taliabu?", "id": "Bobong, Pulau Taliabu (Maluku Utara)." },
    { "en": "Bandar Udara Kepi?", "id": "Kepi, Mappi (Papua Selatan)." },
    { "en": "Bandar Udara Arara?", "id": "Arara, Teluk Bintuni (Papua Barat)." },
    { "en": "Bandar Udara Japura?", "id": "Rengat, Indragiri Hulu (Riau)." },
    { "en": "Bandar Udara Abdulrachman Saleh (militer)?", "id": "Pakis, Malang (Jawa Timur)." }


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
