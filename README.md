<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Тест 1 — Базовый: Present / Past / Future</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Segoe UI', Arial, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 20px;
    color: #333;
  }
  .container { max-width: 750px; margin: 0 auto; }
  header { text-align: center; color: #fff; margin-bottom: 25px; }
  header h1 { font-size: 1.9em; margin-bottom: 8px; }
  header p { opacity: 0.9; }
  .card {
    background: #fff;
    border-radius: 16px;
    padding: 30px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  }
  .question {
    margin-bottom: 22px;
    padding-bottom: 18px;
    border-bottom: 1px solid #eee;
  }
  .question:last-of-type { border-bottom: none; }
  .q-text { font-weight: 600; margin-bottom: 12px; font-size: 1.05em; }
  .q-num { color: #764ba2; }
  .options { display: flex; flex-direction: column; gap: 8px; }
  .option {
    display: flex; align-items: center;
    padding: 10px 14px;
    border: 2px solid #e0e0e0;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.2s;
    user-select: none;
  }
  .option:hover { border-color: #667eea; background: #f5f7ff; }
  .option input { margin-right: 10px; accent-color: #764ba2; }
  .option.correct { border-color: #2ecc71 !important; background: #eafaf1 !important; }
  .option.wrong { border-color: #e74c3c !important; background: #fdeaea !important; }
  .option.disabled { cursor: default; }
  .explanation {
    margin-top: 10px;
    padding: 10px 14px;
    background: #f0f4ff;
    border-left: 4px solid #667eea;
    border-radius: 6px;
    font-size: 0.92em;
    color: #444;
    display: none;
  }
  .explanation.show { display: block; }
  .btn {
    display: block;
    width: 100%;
    padding: 15px;
    margin-top: 20px;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: #fff;
    border: none;
    border-radius: 12px;
    font-size: 1.1em;
    font-weight: 700;
    cursor: pointer;
    transition: 0.3s;
  }
  .btn:hover { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(118,75,162,0.4); }
  .result {
    margin-top: 20px;
    padding: 20px;
    border-radius: 12px;
    text-align: center;
    font-size: 1.2em;
    font-weight: 700;
    display: none;
  }
  .result.show { display: block; }
  .result.excellent { background: #eafaf1; color: #27ae60; }
  .result.good { background: #fff8e1; color: #f39c12; }
  .result.bad { background: #fdeaea; color: #e74c3c; }
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>⏰ Тест 1 — Базовый</h1>
    <p>Present · Past · Future</p>
  </header>

  <div class="card">
    <div id="questions"></div>
    <button class="btn" id="checkBtn">Проверить ответы</button>
    <div class="result" id="result"></div>
  </div>
</div>

<script>
const questions = [
  { q: "She ___ to school every day.", opts: ["go", "goes", "went"], correct: 1, exp: "Present Simple — регулярное действие (every day)." },
  { q: "They ___ a film yesterday.", opts: ["watch", "watches", "watched"], correct: 2, exp: "Past Simple — указатель yesterday." },
  { q: "I ___ my homework tomorrow.", opts: ["will do", "did", "do"], correct: 0, exp: "Future Simple — указатель tomorrow." },
  { q: "He ___ TV at the moment.", opts: ["watch", "is watching", "watched"], correct: 1, exp: "Present Continuous — действие сейчас (at the moment)." },
  { q: "We ___ to the park last Sunday.", opts: ["go", "will go", "went"], correct: 2, exp: "Past Simple — указатель last Sunday." },
  { q: "Look! The cat ___ on the sofa.", opts: ["sleeps", "is sleeping", "slept"], correct: 1, exp: "Present Continuous — действие происходит прямо сейчас (Look!)." },
  { q: "Next week we ___ a new project.", opts: ["start", "started", "will start"], correct: 2, exp: "Future Simple — указатель next week." },
  { q: "She ___ her keys yesterday.", opts: ["loses", "lost", "will lose"], correct: 1, exp: "Past Simple — указатель yesterday." },
  { q: "I usually ___ coffee in the morning.", opts: ["drink", "drank", "will drink"], correct: 0, exp: "Present Simple — привычка (usually)." },
  { q: "They ___ a party next Friday.", opts: ["have", "had", "are going to have"], correct: 2, exp: "Future — план на будущее (be going to)." }
];

const container = document.getElementById('questions');

questions.forEach((item, i) => {
  const div = document.createElement('div');
  div.className = 'question';
  div.innerHTML = `
    <div class="q-text"><span class="q-num">${i + 1}.</span> ${item.q}</div>
    <div class="options">
      ${item.opts.map((opt, j) => `
        <label class="option" id="opt-${i}-${j}">
          <input type="radio" name="q${i}" value="${j}">
          ${opt}
        </label>
      `).join('')}
    </div>
    <div class="explanation" id="exp-${i}">${item.exp}</div>
  `;
  container.appendChild(div);
});

document.getElementById('checkBtn').addEventListener('click', function () {
  let score = 0;

  questions.forEach((item, i) => {
    document.getElementById(`exp-${i}`).classList.add('show');
    const chosen = document.querySelector(`input[name="q${i}"]:checked`);
    const selected = chosen ? parseInt(chosen.value) : -1;

    item.opts.forEach((_, j) => {
      const label = document.getElementById(`opt-${i}-${j}`);
      label.querySelector('input').disabled = true;
      label.classList.add('disabled');
      if (j === item.correct) label.classList.add('correct');
      else if (j === selected) label.classList.add('wrong');
    });

    if (selected === item.correct) score++;
  });

  const result = document.getElementById('result');
  result.classList.add('show');

  if (score === questions.length) {
    result.className = 'result show excellent';
    result.textContent = `🏆 Правильно: ${score} из ${questions.length}. Отлично!`;
  } else if (score >= 7) {
    result.className = 'result show good';
    result.textContent = `👍 Правильно: ${score} из ${questions.length}. Хороший результат!`;
  } else {
    result.className = 'result show bad';
    result.textContent = `😔 Правильно: ${score} из ${questions.length}. Стоит повторить.`;
  }

  this.textContent = 'Пройти заново';
  this.onclick = () => location.reload();
});
</script>
</body>
</html>
