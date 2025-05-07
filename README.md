<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>乗法公式ゲーム (ax±by)² / (ax±by)(cx±dy)</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    input[type="text"] { width: 300px; font-size: 16px; }
    button { font-size: 16px; margin: 5px 5px 10px 0; }
  </style>
</head>
<body>
  <h1>乗法公式ゲーム</h1>
  <div id="question"></div>
  <input type="text" id="answer" placeholder="x²+xy+y² みたいに入力">
  <br>
  <div>
    <button onclick="addTerm('x²')">x²</button>
    <button onclick="addTerm('xy')">xy</button>
    <button onclick="addTerm('y²')">y²</button>
    <button onclick="addTerm('+')">+</button>
    <button onclick="addTerm('-')">-</button>
    <button onclick="clearInput()">消す</button>
  </div>
  <button onclick="checkAnswer()">答える！</button>
  <p id="result"></p>
  <p id="score">0問正解中</p>

  <script>
    let questionCount = 0;
    let correctCount = 0;
    let correctExpansion = "";

    function generateQuestion() {
      const type = Math.random() < 0.5 ? "square" : "binomial";
      let questionText = "";
      let a = randInt(1, 9);
      let b = randInt(1, 9);
      let sign1 = Math.random() < 0.5 ? "+" : "-";

      if (type === "square") {
        questionText = `Q${questionCount + 1}: ( ${a}x ${sign1} ${b}y )² を展開して！`;
        correctExpansion = expandSquare(a, b, sign1);
      } else {
        let c = randInt(1, 9);
        let d = randInt(1, 9);
        let sign2 = Math.random() < 0.5 ? "+" : "-";
        questionText = `Q${questionCount + 1}: ( ${a}x ${sign1} ${b}y )( ${c}x ${sign2} ${d}y ) を展開して！`;
        correctExpansion = expandBinomial(a, b, c, d, sign1, sign2);
      }

      document.getElementById("question").textContent = questionText;
    }

    function expandSquare(a, b, sign) {
      const ab2 = 2 * a * b;
      const op = sign === "+" ? "+" : "-";
      return `${a * a}x²${op}${ab2}xy+${b * b}y²`;
    }

    function expandBinomial(a, b, c, d, sign1, sign2) {
      const ac = a * c;
      const bd = b * d;
      const ad = a * d * (sign2 === "+" ? 1 : -1);
      const bc = b * c * (sign1 === "+" ? 1 : -1);
      const xy = ad + bc;
      const op1 = xy >= 0 ? "+" : "-";
      const op2 = (bd * (sign1 === sign2 ? 1 : -1)) >= 0 ? "+" : "-";
      return `${ac}x²${op1}${Math.abs(xy)}xy${op2}${Math.abs(bd)}y²`;
    }

    function randInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    function checkAnswer() {
      const userAnswer = document.getElementById("answer").value.replace(/\s+/g, "");
      const result = document.getElementById("result");

      if (userAnswer === correctExpansion) {
        result.textContent = "正解！";
        correctCount++;
      } else {
        result.textContent = `不正解。正解は ${correctExpansion}`;
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
      if (confirm(`お疲れさま！\n正答数：${correctCount}/10\n正答率：${accuracy.toFixed(1)}%\nもう一度挑戦しますか？`)) {
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

    function addTerm(term) {
      const input = document.getElementById("answer");
      input.value += term;
      input.focus();
    }

    function clearInput() {
      document.getElementById("answer").value = "";
      document.getElementById("answer").focus();
    }

    generateQuestion();
  </script>
</body>
</html>
