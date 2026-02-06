# hiragana_katakana_flashcard

<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Flashcard Hiragana & Katakana</title>
  <style>
    body {
      font-family: Arial, Helvetica, sans-serif;
      background: linear-gradient(to bottom, #f0f4f8, #d9e2ec);
      margin: 0;
      padding: 20px;
      color: #333;
      min-height: 100vh;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
      margin-bottom: 24px;
    }
    .container {
      max-width: 700px;
      margin: 0 auto;
    }
    .tabs {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin-bottom: 24px;
    }
    .tab-btn {
      padding: 10px 24px;
      font-size: 1.1rem;
      cursor: pointer;
      border: none;
      background: #bdc3c7;
      color: white;
      border-radius: 10px 10px 0 0;
      transition: all 0.22s ease;
    }
    .tab-btn.active {
      background: #3498db;
    }
    .tab-btn:hover:not(.active) {
      background: #2980b9;
    }

    /* Card Flip Container */
    .card-outer {
      perspective: 1000px;
      margin-bottom: 24px;
      min-height: 340px;
      width: 100%;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
    }
    .card {
      background: white;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.12);
      text-align: center;
      position: relative;
      transition: transform 0.6s ease;
      transform-style: preserve-3d;
      user-select: none;
      height: 340px;
      min-height: 300px;
      display: flex;
    }
    .card.flipped {
      transform: rotateY(180deg);
    }
    .card-front, .card-back {
      position: absolute;
      inset: 0;
      backface-visibility: hidden;
      border-radius: 16px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 24px;
      box-sizing: border-box;
    }
    .card-front {
      background: white;
    }
    .card-back {
      background: #f9fafb;
      transform: rotateY(180deg);
    }

    .character {
      font-size: clamp(5rem, 18vw, 8rem);
      margin: 8px 0;
      font-weight: bold;
      color: #2c3e50;
      line-height: 1;
    }
    .romaji {
      font-size: clamp(2.5rem, 10vw, 3.8rem);
      color: #e74c3c;
      margin: 8px 0;
      font-weight: 600;
    }
    .small-info {
      font-size: 1.1rem;
      color: #7f8c8d;
      margin-top: 12px;
      text-align: center;
      max-width: 90%;
    }

    .buttons {
      display: flex;
      justify-content: center;
      gap: 16px;
      flex-wrap: wrap;
      margin: 24px 0;
    }
    button {
      padding: 12px 28px;
      font-size: 1.1rem;
      font-weight: 500;
      cursor: pointer;
      border: none;
      border-radius: 10px;
      background: #3498db;
      color: white;
      transition: all 0.22s ease;
      touch-action: manipulation;
    }
    button:hover:not(:disabled) {
      background: #2980b9;
      transform: translateY(-1px);
    }
    button:disabled {
      background: #95a5a6;
      cursor: not-allowed;
    }
    .answer-btn {
      background: #27ae60;
    }
    .answer-btn:hover:not(:disabled) {
      background: #219653;
    }

    #quiz-area {
      margin-top: 24px;
    }
    .quiz-options {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(110px, 1fr));
      gap: 16px;
      margin-top: 20px;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
    }
    .option-btn {
      padding: 18px 12px;
      font-size: clamp(1.6rem, 6vw, 2.1rem);
      background: #ecf0f1;
      color: #2c3e50;
      border: 2px solid #bdc3c7;
      border-radius: 12px;
      cursor: pointer;
      transition: all 0.18s ease;
      touch-action: manipulation;
    }
    .option-btn:hover:not(.correct):not(.wrong) {
      background: #d5dbdb;
      border-color: #95a5a6;
    }
    .correct {
      background: #2ecc71 !important;
      color: white !important;
      border-color: #27ae60 !important;
    }
    .wrong {
      background: #e74c3c !important;
      color: white !important;
      border-color: #c0392b !important;
    }

    #score-area {
      text-align: center;
      font-size: 1.35rem;
      font-weight: 500;
      margin: 16px 0;
      color: #2c3e50;
      line-height: 1.4;
    }

    #mode-title {
      font-size: 1.4rem;
      margin-bottom: 16px;
      color: #2c3e50;
    }

    footer {
      text-align: center;
      margin-top: 48px;
      color: #7f8c8d;
      font-size: 0.95rem;
      padding-bottom: 20px;
    }

    .hidden {
      display: none;
    }

    @media (max-width: 480px) {
      .card {
        height: auto;
        min-height: 280px;
        padding: 16px;
      }
      .character { font-size: clamp(4.5rem, 22vw, 6.5rem); }
      .romaji { font-size: clamp(2.2rem, 12vw, 3rem); }
      .buttons { gap: 12px; }
      button { padding: 10px 20px; font-size: 1rem; }
    }
  </style>
</head>
<body>

<div class="container">
  <h1>Flashcard Hiragana & Katakana</h1>

  <div class="tabs">
    <button class="tab-btn active" data-mode="learn">Belajar</button>
    <button class="tab-btn" data-mode="flash">Flashcard</button>
    <button class="tab-btn" data-mode="quiz">Quiz</button>
  </div>

  <div class="card-outer">
    <div class="card" id="card">
      <div class="card-front">
        <div id="mode-title"></div>
        <div class="character" id="main-char">あ</div>
        <div class="small-info" id="info">Pilih mode di atas • Klik kartu untuk flip (Flashcard/Quiz)</div>
      </div>
      <div class="card-back">
        <div class="character" id="main-char-back"></div>
        <div class="romaji" id="main-romaji-back"></div>
        <div class="small-info">Klik kartu lagi atau tombol Selanjutnya</div>
      </div>
    </div>
  </div>

  <div id="quiz-area" class="hidden">
    <div style="font-size:1.5rem; margin:20px 0; text-align:center;">Apa romaji-nya?</div>
    <div class="quiz-options" id="quiz-options"></div>
  </div>

  <div id="score-area"></div>

  <div class="buttons">
    <button id="prev">Sebelumnya</button>
    <button id="show-answer" class="answer-btn hidden">Lihat Jawaban</button>
    <button id="next">Selanjutnya</button>
    <button id="random">Acak</button>
  </div>

  <footer>
    Dibuat untuk belajar Hiragana & Katakana • 46 dasar + dakuten • Progress disimpan otomatis
  </footer>
</div>

<script>
// Data Kana (lengkapi sesuai kebutuhan, ini sudah termasuk contoh dakuten)
const kanaData = [
  {hiragana: "あ", katakana: "ア", romaji: "a"},
  {hiragana: "い", katakana: "イ", romaji: "i"},
  {hiragana: "う", katakana: "ウ", romaji: "u"},
  {hiragana: "え", katakana: "エ", romaji: "e"},
  {hiragana: "お", katakana: "オ", romaji: "o"},
  {hiragana: "か", katakana: "カ", romaji: "ka"},
  {hiragana: "き", katakana: "キ", romaji: "ki"},
  {hiragana: "く", katakana: "ク", romaji: "ku"},
  {hiragana: "け", katakana: "ケ", romaji: "ke"},
  {hiragana: "こ", katakana: "コ", romaji: "ko"},
  {hiragana: "さ", katakana: "サ", romaji: "sa"},
  {hiragana: "し", katakana: "シ", romaji: "shi"},
  {hiragana: "す", katakana: "ス", romaji: "su"},
  {hiragana: "せ", katakana: "セ", romaji: "se"},
  {hiragana: "そ", katakana: "ソ", romaji: "so"},
  {hiragana: "た", katakana: "タ", romaji: "ta"},
  {hiragana: "ち", katakana: "チ", romaji: "chi"},
  {hiragana: "つ", katakana: "ツ", romaji: "tsu"},
  {hiragana: "て", katakana: "テ", romaji: "te"},
  {hiragana: "と", katakana: "ト", romaji: "to"},
  {hiragana: "な", katakana: "ナ", romaji: "na"},
  {hiragana: "に", katakana: "ニ", romaji: "ni"},
  {hiragana: "ぬ", katakana: "ヌ", romaji: "nu"},
  {hiragana: "ね", katakana: "ネ", romaji: "ne"},
  {hiragana: "の", katakana: "ノ", romaji: "no"},
  {hiragana: "は", katakana: "ハ", romaji: "ha"},
  {hiragana: "ひ", katakana: "ヒ", romaji: "hi"},
  {hiragana: "ふ", katakana: "フ", romaji: "fu"},
  {hiragana: "へ", katakana: "ヘ", romaji: "he"},
  {hiragana: "ほ", katakana: "ホ", romaji: "ho"},
  {hiragana: "ま", katakana: "マ", romaji: "ma"},
  {hiragana: "み", katakana: "ミ", romaji: "mi"},
  {hiragana: "む", katakana: "ム", romaji: "mu"},
  {hiragana: "め", katakana: "メ", romaji: "me"},
  {hiragana: "も", katakana: "モ", romaji: "mo"},
  {hiragana: "や", katakana: "ヤ", romaji: "ya"},
  {hiragana: "ゆ", katakana: "ユ", romaji: "yu"},
  {hiragana: "よ", katakana: "ヨ", romaji: "yo"},
  {hiragana: "ら", katakana: "ラ", romaji: "ra"},
  {hiragana: "り", katakana: "リ", romaji: "ri"},
  {hiragana: "る", katakana: "ル", romaji: "ru"},
  {hiragana: "れ", katakana: "レ", romaji: "re"},
  {hiragana: "ろ", katakana: "ロ", romaji: "ro"},
  {hiragana: "わ", katakana: "ワ", romaji: "wa"},
  {hiragana: "を", katakana: "ヲ", romaji: "wo"},
  {hiragana: "ん", katakana: "ン", romaji: "n"},
  // Dakuten & handakuten contoh (bisa ditambah semua)
  {hiragana: "が", katakana: "ガ", romaji: "ga"},
  {hiragana: "ぎ", katakana: "ギ", romaji: "gi"},
  {hiragana: "ぐ", katakana: "グ", romaji: "gu"},
  {hiragana: "げ", katakana: "ゲ", romaji: "ge"},
  {hiragana: "ご", katakana: "ゴ", romaji: "go"},
  {hiragana: "ざ", katakana: "ザ", romaji: "za"},
  {hiragana: "じ", katakana: "ジ", romaji: "ji"},
  {hiragana: "ず", katakana: "ズ", romaji: "zu"},
  {hiragana: "ぜ", katakana: "ゼ", romaji: "ze"},
  {hiragana: "ぞ", katakana: "ゾ", romaji: "zo"},
  {hiragana: "だ", katakana: "ダ", romaji: "da"},
  {hiragana: "ぢ", katakana: "ヂ", romaji: "ji"},
  {hiragana: "づ", katakana: "ヅ", romaji: "zu"},
  {hiragana: "で", katakana: "デ", romaji: "de"},
  {hiragana: "ど", katakana: "ド", romaji: "do"},
  {hiragana: "ば", katakana: "バ", romaji: "ba"},
  {hiragana: "び", katakana: "ビ", romaji: "bi"},
  {hiragana: "ぶ", katakana: "ブ", romaji: "bu"},
  {hiragana: "べ", katakana: "ベ", romaji: "be"},
  {hiragana: "ぼ", katakana: "ボ", romaji: "bo"},
  {hiragana: "ぱ", katakana: "パ", romaji: "pa"},
  {hiragana: "ぴ", katakana: "ピ", romaji: "pi"},
  {hiragana: "ぷ", katakana: "プ", romaji: "pu"},
  {hiragana: "ぺ", katakana: "ペ", romaji: "pe"},
  {hiragana: "ぽ", katakana: "ポ", romaji: "po"},
];

let currentIndex = 0;
let currentMode = "learn";
let isFlipped = false;

let score = { correct: 0, wrong: 0, seen: new Set() };

// ─── Load & Save Progress ───
function loadProgress() {
  const saved = localStorage.getItem("kanaQuizProgress");
  if (saved) {
    const data = JSON.parse(saved);
    score.correct = data.correct || 0;
    score.wrong   = data.wrong   || 0;
    if (data.seen) score.seen = new Set(data.seen);
  }
  updateScoreDisplay();
}

function saveProgress() {
  localStorage.setItem("kanaQuizProgress", JSON.stringify({
    correct: score.correct,
    wrong: score.wrong,
    seen: Array.from(score.seen)
  }));
}

function updateScoreDisplay() {
  const total = score.correct + score.wrong;
  const percent = total > 0 ? ((score.correct / total) * 100).toFixed(1) : 0;
  document.getElementById("score-area").innerHTML = `
    Skor: <b>\( {score.correct}</b> benar • <b> \){score.wrong}</b> salah<br>
    Akurasi: <b>\( {percent}%</b> ( \){score.correct}/${total})
  `;
}

// ─── DOM Elements ───
const card = document.getElementById("card");
const mainChar = document.getElementById("main-char");
const mainCharBack = document.getElementById("main-char-back");
const mainRomajiBack = document.getElementById("main-romaji-back");
const info = document.getElementById("info");
const showAnswerBtn = document.getElementById("show-answer");
const quizArea = document.getElementById("quiz-area");
const quizOptions = document.getElementById("quiz-options");
const modeTitle = document.getElementById("mode-title");

// ─── Functions ───
function flipCard(flip) {
  isFlipped = flip;
  card.classList.toggle("flipped", flip);
  card.style.cursor = (currentMode === "flash" || currentMode === "quiz") ? "pointer" : "default";
}

function updateCard() {
  const item = kanaData[currentIndex];
  const showHiragana = Math.random() > 0.5;

  flipCard(false);

  modeTitle.textContent = currentMode === "learn" ? "Mode Belajar" :
                          currentMode === "flash" ? "Mode Flashcard" :
                          "Mode Quiz";

  if (currentMode === "learn") {
    mainChar.textContent = `${item.hiragana} / ${item.katakana}`;
    mainCharBack.textContent = `${item.hiragana} / ${item.katakana}`;
    mainRomajiBack.textContent = item.romaji;
    info.textContent = "Hiragana & Katakana + Romaji";
    showAnswerBtn.classList.add("hidden");
    quizArea.classList.add("hidden");
    flipCard(true); // langsung tampilkan jawaban
  }
  else if (currentMode === "flash") {
    if (isFlipped) {
      mainCharBack.textContent = `${item.hiragana} / ${item.katakana}`;
      mainRomajiBack.textContent = item.romaji;
      info.textContent = "Klik kartu atau tombol Selanjutnya";
      showAnswerBtn.classList.add("hidden");
    } else {
      mainChar.textContent = showHiragana ? item.hiragana : item.katakana;
      info.textContent = "Klik kartu untuk melihat jawaban";
      showAnswerBtn.classList.remove("hidden");
    }
    quizArea.classList.add("hidden");
  }
  else if (currentMode === "quiz") {
    mainChar.textContent = showHiragana ? item.hiragana : item.katakana;
    info.textContent = "";
    showAnswerBtn.classList.add("hidden");
    quizArea.classList.remove("hidden");
    generateQuizOptions(item.romaji);
    score.seen.add(currentIndex);
    saveProgress();
  }
}

function generateQuizOptions(correct) {
  quizOptions.innerHTML = "";
  let options = [correct];

  while (options.length < 4) {
    const rand = kanaData[Math.floor(Math.random() * kanaData.length)].romaji;
    if (!options.includes(rand)) options.push(rand);
  }

  options.sort(() => Math.random() - 0.5);

  options.forEach(opt => {
    const btn = document.createElement("button");
    btn.className = "option-btn";
    btn.textContent = opt;
    btn.onclick = () => {
      const isCorrect = opt === correct;
      btn.classList.add(isCorrect ? "correct" : "wrong");

      if (isCorrect) score.correct++;
      else score.wrong++;

      updateScoreDisplay();
      saveProgress();

      setTimeout(() => {
        flipCard(true);
        const item = kanaData[currentIndex];
        mainCharBack.textContent = `${item.hiragana} / ${item.katakana}`;
        mainRomajiBack.textContent = correct;
      }, 700);
    };
    quizOptions.appendChild(btn);
  });
}

// ─── Navigation ───
function nextCard() {
  currentIndex = (currentIndex + 1) % kanaData.length;
  flipCard(false);
  updateCard();
}

function prevCard() {
  currentIndex = (currentIndex - 1 + kanaData.length) % kanaData.length;
  flipCard(false);
  updateCard();
}

function randomCard() {
  currentIndex = Math.floor(Math.random() * kanaData.length);
  flipCard(false);
  updateCard();
}

// ─── Event Listeners ───
document.querySelectorAll(".tab-btn").forEach(btn => {
  btn.addEventListener("click", () => {
    document.querySelectorAll(".tab-btn").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
    currentMode = btn.dataset.mode;
    flipCard(false);
    updateCard();
  });
});

card.addEventListener("click", () => {
  if (currentMode === "flash" || currentMode === "quiz") {
    flipCard(!isFlipped);
  }
});

document.getElementById("next").onclick = nextCard;
document.getElementById("prev").onclick = prevCard;
document.getElementById("random").onclick = randomCard;
document.getElementById("show-answer").onclick = () => flipCard(true);

// ─── Init ───
loadProgress();
updateCard();
</script>
</body>
</html>
