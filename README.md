# Anderson-ENT-.
<!DOCTYPE html>
<html>
<head>
    <title>Anderson-ENT | Химия Биология ЕНТ</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
        }
        .card {
            background: white;
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .subject {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 10px;
        }
        .chemistry { color: #e74c3c; }
        .biology { color: #2ecc71; }
        .anatomy { color: #3498db; }
        button {
            background: #667eea;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 25px;
            font-size: 16px;
            margin: 5px;
            cursor: pointer;
        }
        .quiz-question {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 15px;
            margin-top: 15px;
        }
        .option {
            background: white;
            padding: 10px;
            margin: 8px 0;
            border-radius: 10px;
            cursor: pointer;
            border: 1px solid #ddd;
        }
        .option:hover { background: #e0e0e0; }
        .correct { background: #2ecc71; color: white; }
        .wrong { background: #e74c3c; color: white; }
        .progress-bar {
            background: #e0e0e0;
            border-radius: 25px;
            height: 25px;
            overflow: hidden;
            margin: 10px 0;
        }
        .progress-fill {
            background: linear-gradient(90deg, #4CAF50, #8BC34A);
            width: 0%;
            height: 100%;
            border-radius: 25px;
            transition: width 0.3s;
        }
        .score { font-size: 18px; font-weight: bold; text-align: center; }
    </style>
</head>
<body>
<div class="container">
    <div class="card">
        <h1>🦷 Anderson-ENT</h1>
        <p style="text-align:center">Подготовка к ЕНТ | Стоматология</p>
    </div>

    <div class="card">
        <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
        <div class="score">⭐ Баллы: <span id="score">0</span> / 140</div>
    </div>

    <div class="card">
        <div class="subject chemistry">🧪 ХИМИЯ</div>
        <button onclick="startQuiz('chemistry1')">Органика (+10 баллов)</button>
        <button onclick="startQuiz('chemistry2')">Неорганика (+10 баллов)</button>
        <div id="quiz-chemistry1" class="quiz-question" style="display:none"></div>
        <div id="quiz-chemistry2" class="quiz-question" style="display:none"></div>
    </div>

    <div class="card">
        <div class="subject biology">🧬 БИОЛОГИЯ</div>
        <button onclick="startQuiz('biology1')">Цитология (+10 баллов)</button>
        <button onclick="startQuiz('biology2')">Генетика (+10 баллов)</button>
        <div id="quiz-biology1" class="quiz-question" style="display:none"></div>
        <div id="quiz-biology2" class="quiz-question" style="display:none"></div>
    </div>

    <div class="card">
        <div class="subject anatomy">🦷 АНАТОМИЯ (стоматологу)</div>
        <button onclick="startQuiz('anatomy1')">Череп и зубы (+10 баллов)</button>
        <div id="quiz-anatomy1" class="quiz-question" style="display:none"></div>
    </div>
</div>

<script>
    const quizzes = {
        chemistry1: { q: "Общая формула алканов?", opts: ["CₙH₂ₙ", "CₙH₂ₙ₊₂", "CₙH₂ₙ₋₂"], correct: 1 },
        chemistry2: { q: "Какое вещество является основанием?", opts: ["HCl", "H₂SO₄", "NaOH"], correct: 2 },
        biology1: { q: "Какой органоид отвечает за энергию (АТФ)?", opts: ["Рибосомы", "Митохондрии", "Ядро"], correct: 1 },
        biology2: { q: "Второй закон Менделя — это расщепление?", opts: ["1:1", "3:1", "9:3:3:1"], correct: 1 },
        anatomy1: { q: "Сколько зубов у взрослого человека?", opts: ["28", "30", "32"], correct: 2 }
    };
    
    let totalScore = localStorage.getItem('score') ? parseInt(localStorage.getItem('score')) : 0;
    let completed = localStorage.getItem('completed') ? JSON.parse(localStorage.getItem('completed')) : [];
    
    function updateUI() {
        let percent = (totalScore / 140) * 100;
        document.getElementById('progressFill').style.width = percent + '%';
        document.getElementById('score').innerText = totalScore;
        localStorage.setItem('score', totalScore);
        localStorage.setItem('completed', JSON.stringify(completed));
    }
    
    function startQuiz(id) {
        let div = document.getElementById('quiz-' + id);
        if (completed.includes(id)) {
            div.innerHTML = '<p>✅ Урок уже пройден! +10 баллов получено.</p>';
            div.style.display = 'block';
            return;
        }
        let q = quizzes[id];
        let html = `<p><strong>❓ ${q.q}</strong></p>`;
        q.opts.forEach((opt, i) => {
            html += `<div class="option" onclick="checkAnswer('${id}', ${i}, ${q.correct})">${opt}</div>`;
        });
        div.innerHTML = html;
        div.style.display = 'block';
    }
    
    function checkAnswer(id, selected, correct) {
        if (completed.includes(id)) return;
        let div = document.getElementById('quiz-' + id);
        if (selected === correct) {
            div.innerHTML = '<p style="color:green">✅ Правильно! +10 баллов!</p>';
            totalScore += 10;
            completed.push(id);
            updateUI();
        } else {
            let rightAnswer = quizzes[id].opts[correct];
            div.innerHTML = `<p style="color:red">❌ Неправильно. Правильный ответ: ${rightAnswer}</p>`;
        }
    }
    
    updateUI();
</script>
</body>
</html>
