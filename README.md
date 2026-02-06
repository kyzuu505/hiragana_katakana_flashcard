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
    }
    h1 { text-align: center; color: #2c3e50; }
    .container { max-width: 700px; margin: 0 auto; }
    .tabs {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin-bottom: 20px;
    }
    .tab-btn {
      padding: 10px 20px;
      font-size: 1.1rem;
      cursor: pointer;
      border: none;
      background: #bdc3c7;
      color: white;
      border-radius: 8px 8px 0 0;
      transition: all 0.2s;
    }
    .tab-btn.active { background: #3498db; }
    .tab-btn:hover { background: #2980b9; }

    /* Card Flip Container */
    .card-outer {
      perspective: 1000px;
      margin-bottom: 20px;
      min-height: 320px;
    }
    .card {
      background: white;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.15);
      padding: 40px;
      text-align: center;
      height: 320px;
      position: relative;
      transition: transform 0.6s;
      transform-style: preserve-3d;
      user-select: none;
      cursor: pointer;
    }
    .card.flipped {
      transform: rotateY(180deg);
    }
    .card-front, .card-back {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      border-radius: 16px;
    }
    .card-front {
      background: white;
    }
    .card-back {
      background: #f8f9fa;
      transform: rotateY(180deg);
    }
    .character {
      font-size: 8rem;
      margin: 10px 0;
      font-weight: bold;
      color: #2c3e50;
    }
    .romaji {
      font-size: 3.5rem;
      color: #e74c3c;
      margin: 10px 0;
    }
    .small-info {
      font-size: 1.1rem;
      color: #7f8c8d;
      margin-top: 10px;
    }

    .buttons {
      display: flex;
      justify-content: center;
      gap: 15px;
      flex-wrap: wrap;
      margin: 20px 0;
    }
    button {
      padding: 12px 24px;
      font-size: 1.1rem;
      cursor: pointer;
      border: none;
      border-radius: 8px;
      background: #3498db;
      color: white;
      transition: 0.2s;
    }
    button:hover { background: #2980b9; }
    button:disabled { background: #95a5a6; cursor: not-allowed; }
    .answer-btn { background: #27ae60; }
    .answer-btn:hover { background: #219653; }

    #quiz-area { margin-top: 20px; }
    .quiz-options {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
      gap: 15px;
      margin-top: 20px;
    }
    .option-btn {
      padding: 20px;
      font-size: 2rem;
      background: #ecf0f1;
      color: #2c3e50;
      border: 2px solid #bdc3c7;
      border-radius: 12px;
      cursor: pointer;
    }
    .option-btn:hover { background: #bdc3c7; }
    .correct { background: #2ecc71 !important; color: white !important; }
    .wrong   { background: #e74c3c !important; color: white !important; }

    #score-area {
      text-align: center;
      font-size: 1.3rem;
      margin: 15px 0;
      color: #2c3e50;
    }

    footer {
      text-align: center;
      margin-top: 40px;
      color: #7f8c8d;
      font-size: 0.9rem;
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
        <div id="mode-title" style="font-size:1.4rem; margin-bottom:20px;"></div>
        <div class="character" id="main-char">あ</div>
        <div class="small-info" id="info">Klik kartu untuk flip (mode flashcard)</div>
      </div>
      <div class="card-back">
        <div class="character" id="main-char-back"></div>
        <div class="romaji" id="main-romaji-back"></div>
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
    Dibuat untuk belajar Hiragana & Katakana • Basic 46 + dakuten • 2026
  </footer>
</div>

<script>
// ──────────────────────────────────────────────
const kanaData = [
  {hiragana: "あ", katakana: "ア", romaji: "a"},
  {hiragana: "い", katakana: "イ", romaji: "i"},
  {hiragana: "う", katakana: "ウ", romaji: "u"},
  {hiragana: "え", katakana: "エ", romaji: "e"},
  {hiragana: "お", katakana: "オ", romaji: "o"},
  {hiragana: "か", katakana: "カ", romaji: "ka"},
  // ... (sama seperti daftar kamu, saya singkat di sini)
  {hiragana: "が", katakana: "ガ", romaji: "ga"},
  {hiragana: "ざ", katakana: "ザ", romaji: "za"},
  {hiragana: "だ", katakana: "ダ", romaji: "da"},
  {hiragana: "ば", katakana: "バ", romaji: "ba"},
  // tambahkan sisanya sesuai kebutuhan
];

// ──────────────────────────────────────────────
let currentIndex = 0;
let currentMode = "learn";
let isFlipped = false;
let score = { correct: 0, wrong: 0, seen: new Set() };

// Load progress dari localStorage
function loadProgress() {
  const saved = localStorage.getItem("kanaQuizProgress");
  if (saved) {
    const data = JSON.parse(saved);
    score.correct = data.correct || 0;
    score.wrong   = data.wrong   || 0;
    score.seen    = new Set(data.seen || []);
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

// ──────────────────────────────────────────────
const card = document.getElementById("card");
const mainCharFront = document.getElementById("main-char");
const info = document.getElementById("info");
const showAnswerBtn = document.getElementById("show-answer");
const quizArea = document.getElementById("quiz-area");
const quizOptions = document.getElementById("quiz-options");
const modeTitle = document.getElementById("mode-title");
const mainCharBack = document.getElementById("main-char-back");
const mainRomajiBack = document.getElementById("main-romaji-back");

function flipCard(shouldFlip) {
  isFlipped = shouldFlip;
  card.classList.toggle("flipped", shouldFlip);
}

function updateCard() {
  const item = kanaData[currentIndex];
  const isHiragana = Math.random() > 0.5;

  flipCard(false); // reset flip

  modeTitle.textContent = currentMode === "learn" ? "Mode Belajar" :
                          currentMode === "flash" ? "Mode Flashcard" : "Mode Quiz";

  if (currentMode === "learn") {
    mainCharFront.textContent = `\( {item.hiragana} / \){item.katakana}`;
    mainCharBack.textContent = `\( {item.hiragana} / \){item.katakana}`;
    mainRomajiBack.textContent = item.romaji;
    info.textContent = "Hiragana & Katakana + Romaji";
    showAnswerBtn.classList.add("hidden");
    quizArea.classList.add("hidden");
    flipCard(true); // langsung tampilkan back (jawaban)
  }
  else if (currentMode === "flash") {
    if (isFlipped) {
      mainCharBack.textContent = `\( {item.hiragana} / \){item.katakana}`;
      mainRomajiBack.textContent = item.romaji;
      info.textContent = "Klik kartu atau 'Selanjutnya'";
      showAnswerBtn.classList.add("hidden");
    } else {
      mainCharFront.textContent = isHiragana ? item.hiragana : item.katakana;
      info.textContent = "Klik kartu untuk melihat jawaban";
      showAnswerBtn.classList.remove("hidden");
    }
    quizArea.classList.add("hidden");
  }
  else if (currentMode === "quiz") {
    mainCharFront.textContent = isHiragana ? item.hiragana : item.katakana;
    info.textContent = "";
    showAnswerBtn.classList.add("hidden");
    quizArea.classList.remove("hidden");
    generateQuizOptions(item.romaji);
    score.seen.add(currentIndex);
    saveProgress();
  }
}

function generateQuizOptions(correctRomaji) {
  quizOptions.innerHTML = "";
  let options = [correctRomaji];
  
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
      const isCorrect = opt === correctRomaji;
      btn.classList.add(isCorrect ? "correct" : "wrong");

      if (isCorrect) {
        score.correct++;
      } else {
        score.wrong++;
      }
      updateScoreDisplay();
      saveProgress();

      // Tampilkan jawaban benar di back side
      setTimeout(() => {
        flipCard(true);
        mainCharBack.textContent = kanaData[currentIndex].hiragana + " / " + kanaData[currentIndex].katakana;
        mainRomajiBack.textContent = correctRomaji;
      }, 700);
    };
    quizOptions.appendChild(btn);
  });
}

// ──────────────────────────────────────────────
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

// ──────────────────────────────────────────────
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

// Mulai
loadProgress();
updateCard();
</script>
</body>
</html>
