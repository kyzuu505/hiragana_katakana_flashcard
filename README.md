<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Flashcard Hiragana & Katakana</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #f2f2f2;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.container {
    text-align: center;
}
.card {
    width: 200px;
    height: 200px;
    background: white;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 64px;
    cursor: pointer;
    margin-bottom: 20px;
}
.buttons button {
    padding: 10px 15px;
    margin: 5px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    background: #4CAF50;
    color: white;
}
.buttons button:hover {
    background: #43a047;
}
</style>
</head>

<body>

<div class="container">
    <h2 id="mode">Hiragana</h2>

    <div class="card" onclick="flipCard()">
        <span id="cardText">あ</span>
    </div>

    <div class="buttons">
        <button onclick="nextCard()">Next</button>
        <button onclick="setMode('hiragana')">Hiragana</button>
        <button onclick="setMode('katakana')">Katakana</button>
    </div>
</div>

<script>
const hiragana = [
    {jp:"あ", ro:"a"}, {jp:"い", ro:"i"}, {jp:"う", ro:"u"},
    {jp:"え", ro:"e"}, {jp:"お", ro:"o"},
    {jp:"か", ro:"ka"}, {jp:"き", ro:"ki"}, {jp:"く", ro:"ku"},
    {jp:"け", ro:"ke"}, {jp:"こ", ro:"ko"}
];

const katakana = [
    {jp:"ア", ro:"a"}, {jp:"イ", ro:"i"}, {jp:"ウ", ro:"u"},
    {jp:"エ", ro:"e"}, {jp:"オ", ro:"o"},
    {jp:"カ", ro:"ka"}, {jp:"キ", ro:"ki"}, {jp:"ク", ro:"ku"},
    {jp:"ケ", ro:"ke"}, {jp:"コ", ro:"ko"}
];

let mode = "hiragana";
let current = 0;
let showRomaji = false;

function updateCard() {
    const data = mode === "hiragana" ? hiragana : katakana;
    document.getElementById("cardText").innerText =
        showRomaji ? data[current].ro : data[current].jp;
}

function flipCard() {
    showRomaji = !showRomaji;
    updateCard();
}

function nextCard() {
    const data = mode === "hiragana" ? hiragana : katakana;
    current = Math.floor(Math.random() * data.length);
    showRomaji = false;
    updateCard();
}

function setMode(newMode) {
    mode = newMode;
    current = 0;
    showRomaji = false;
    document.getElementById("mode").innerText =
        newMode === "hiragana" ? "Hiragana" : "Katakana";
    updateCard();
}
</script>

</body>
</html>
