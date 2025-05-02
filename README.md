<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>乗法公式ゲーム (ax±by)²</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    input[type="text"] { width: 300px; font-size: 16px; }
    button { font-size: 16px; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>乗法公式ゲーム (ax±by)²</h1>
  <div id="question"></div>
  <input type="text" id="answer" placeholder="x²+xy+y² みたいに入力">
  <br>
  <button onclick="checkAnswer()">答える！</button>
  <p id="result"></p>
  <p id="score">0問正解中</p>

  <script>
    let questionCount = 0;
    let correctCount = 0;
    let a, b, plus, correctExpansion;

    function generateQuestion() {
      a = Math.floor(Math.random() * 9) + 1;
      b = Math.floor(Math.random() * 9) + 1;
      plus = Math.random() < 0.5;

      const operator = plus ? "+" : "-";
      document.getElementById("question").textContent =
        `Q${questionCount + 1}: ( ${a}x ${operator} ${b}y )² を展開して！`;

      if (plus) {
        correctExpansion = `${a*a}x²+${2*a*b}xy+${b*b}y²`;
      } else {
        correctExpansion = `${a*a}x²-${2*a*b}xy+${b*b}y²`;
      }
    }

    function checkAnswer() {
      const userAnswer = document.getElementById("answer").value.replace(/\s+/g, "");
      const result = document.getElementById("result");

      if (userAnswer === correctExpansion) {
        result.textContent = "正解！ 🎉";
        correctCount++;
      } else {
        result.textContent = `不正解 😢 正解は ${correctExpansion}`;
      }

      questionCount++;
      document.getElementById("score").textContent = `${questionCount}問中 ${correctCount}問正解`;

      if (questionCount >= 10) {
        showResult();
      } else {
        document.getElementById("answer").value = "";
        generateQuestion();
      }
    }

    function showResult() {
      const accuracy = (correctCount / 10) * 100;
      if (confirm(`お疲れさま！終わりたい場合はサイトを閉じてください。\n正答数：${correctCount}/10\n正答率：${accuracy.toFixed(1)}%\nもう一度挑戦しますか？`)) {
        questionCount = 0;
        correctCount = 0;
        document.getElementById("answer").value = "";
        document.getElementById("result").textContent = "";
        document.getElementById("score").textContent = "0問正解中";
        generateQuestion();
      } else {
        alert("また遊んでね！");
      }
    }

    generateQuestion();
  </script>
</body>
</html>
