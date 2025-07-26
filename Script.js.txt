let karutaList = [];
let currentIndex = 0;
let score = 0;

const yomifudaDiv = document.getElementById("yomifuda");
const torifudaContainer = document.getElementById("torifuda-container");
const resultDiv = document.getElementById("result");
const scoreDiv = document.getElementById("score");
const nextBtn = document.getElementById("next-btn");

fetch("karuta_data.json")
  .then(response => response.json())
  .then(data => {
    karutaList = data;
    shuffleArray(karutaList);
    showNextCard();
  });

function showNextCard() {
  resultDiv.textContent = "";
  if (currentIndex >= karutaList.length) {
    yomifudaDiv.textContent = "ゲーム終了！";
    torifudaContainer.innerHTML = "";
    nextBtn.style.display = "none";
    return;
  }

  const card = karutaList[currentIndex];
  yomifudaDiv.textContent = card.yomifuda;

  let choices = [card];
  while (choices.length < 3) {
    const random = karutaList[Math.floor(Math.random() * karutaList.length)];
    if (!choices.includes(random)) choices.push(random);
  }

  shuffleArray(choices);

  torifudaContainer.innerHTML = "";
  choices.forEach(c => {
    const div = document.createElement("div");
    div.className = "torifuda";
    div.textContent = c.torifuda;
    div.onclick = () => {
      if (c.id === card.id) {
        resultDiv.textContent = "✅ 正解！";
        score++;
        scoreDiv.textContent = `得点: ${score}`;
      } else {
        resultDiv.textContent = "❌ 不正解...";
      }
      currentIndex++;
    };
    torifudaContainer.appendChild(div);
  });
}

nextBtn.onclick = () => {
  showNextCard();
};

function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}