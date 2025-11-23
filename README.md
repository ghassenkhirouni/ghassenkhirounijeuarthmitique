<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu de Calcul Mental</title>
  <style>
    :root {
      --bg: #0f172a;
      --card: #0b1220;
      --accent: #06b6d4;
      --good: #16a34a;
      --bad: #ef4444;
      --muted: #94a3b8;
    }

    *{
      box-sizing: border-box;
      font-family: Inter, ui-sans-serif, system-ui, Segoe UI, Roboto, "Helvetica Neue", Arial;
    }

    html,
    body {
      height: 100%;
      margin: 0;
      background: linear-gradient(180deg, #07122a 0%, #07102a 100%);
      color: #e6eef7;
    }

    .wrap {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
    }

    .card {
      background: var(--card);
      width: 100%;
      max-width: 760px;
      border-radius: 14px;
      padding: 22px;
      box-shadow: 0 10px 30px rgba(2, 6, 23, 0.6);
    }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    h1 {
      font-size: 1.25rem;
      margin: 0;
    }

    .controls {
      display: flex;
      gap: 8px;
      align-items: center;
    }

    select,
    input[type=range] {
      background: transparent;
      border: 1px solid rgba(255, 255, 255, 0.06);
      color: inherit;
      padding: 6px;
      border-radius: 8px;
    }

    .game {
      display: grid;
      grid-template-columns: 1fr 220px;
      gap: 18px;
    }

    @media (max-width: 720px) {
      .game {
        grid-template-columns: 1fr;
      }
      .right {
        order: 2;
      }
    }

    .left {
      padding: 18px;
      border-radius: 10px;
      background: linear-gradient(180deg, rgba(255,255,255,0.02), transparent);
    }

    .question {
      font-size: 2.1rem;
      font-weight: 700;
      margin: 6px 0;
    }

    .meta {
      display: flex;
      gap: 12px;
      align-items: center;
      margin-bottom: 12px;
    }

    .answer-row {
      display: flex;
      gap: 8px;
      align-items: center;
    }

    input.answer {
      flex: 1;
      padding: 12px;
      border-radius: 10px;
      border: 1px solid rgba(255,255,255,0.06);
      font-size: 1.1rem;
    }

    button.btn {
      padding: 10px 14px;
      border-radius: 10px;
      border: 0;
      background: var(--accent);
      color: #0a7792;
      font-weight: 700;
      cursor: pointer;
    }

    button.secondary {
      background: transparent;
      border: 1px solid rgba(255,255,255,0.06);
      color: var(--muted);
    }

    .right {
      padding: 18px;
      border-radius: 10px;
      background: linear-gradient(180deg, rgba(255,255,255,0.02), transparent);
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .stat {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px;
      border-radius: 8px;
      background: rgba(255,255,255,0.01);
    }

    .feedback {
      height: 8px;
      border-radius: 8px;
      overflow: hidden;
      background: rgba(255,255,255,0.02);
    }

    .feedback > i {
      display: block;
      height: 100%;
      width: 0%;
      transition: width 250ms ease;
    }

    .small {
      font-size: 0.85rem;
      color: var(--muted);
    }

    .hint {
      font-size: 0.9rem;
      color: var(--muted);
      margin-top: 8px;
    }

    .pulse-good {
      animation: pulse-good 550ms ease;
    }

    .pulse-bad {
      animation: pulse-bad 550ms ease;
    }

    @keyframes pulse-good {
      0%   { box-shadow: 0 0 0 0 rgba(22,163,74,0.5); }
      100% { box-shadow: 0 0 0 18px rgba(22,163,74,0); }
    }

    @keyframes pulse-bad {
      0%   { box-shadow: 0 0 0 0 rgba(239,68,68,0.5); }
      100% { box-shadow: 0 0 0 18px rgba(239,68,68,0); }
    }

    footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 14px;
      color: var(--muted);
      font-size: 0.9rem;
    }

  </style>
</head>

<body>
  <div class="wrap">
    <div class="card" role="application">
      <header>
        <h1>Jeu de calcul mental</h1>
        <div class="controls">
          <label class="small">Difficulté
            <select id="difficulty">
              <option value="easy">Facile</option>
              <option value="medium" selected>Moyen</option>
              <option value="hard">Difficile</option>
            </select>
          </label>
          <button id="startBtn" class="btn">Démarrer</button>
          <button id="resetBtn" class="secondary">Reset</button>
        </div>
      </header>

      <div class="game">
        <section class="left">
          <div class="meta">
            <div class="small">Temps par question: <strong id="timePerQ">10</strong>s</div>
            <div class="small">Questions posées: <span id="asked">0</span></div>
            <div class="small">Réussites: <span id="streak">0</span></div>
          </div>

          <div id="questionCard">
            <div class="question" id="question">Appuyez sur Démarrer pour commencer</div>
            <div class="answer-row">
              <input id="answerInput" class="answer" inputmode="numeric" autocomplete="off" placeholder="Entrez votre réponse" />
              <button id="submitBtn" class="btn">OK</button>
            </div>
            <div class="hint small" id="hint">Utilisez Entrée pour valider. Les divisions donnent des entiers.</div>
            <div class="feedback" style="margin-top:12px"><i id="bar"></i></div>
          </div>
        </section>

        <aside class="right">
          <div class="stat"><div>Score</div><div id="score">0</div></div>
          <div class="stat"><div>Meilleur score (local)</div><div id="best">0</div></div>
          <div class="stat"><div>Temps restant</div><div id="timeLeft">--</div></div>
          <div class="stat"><div>Dernier résultat</div><div id="last">—</div></div>

          <div style="margin-top:6px">
            <label class="small">Temps/question (sec)</label>
            <input id="timeRange" type="range" min="3" max="20" value="10" />
          </div>

          <div style="display:flex;gap:8px;margin-top:8px">
            <button id="skipBtn" class="secondary">Passer</button>
            <button id="toggleSound" class="secondary">Son: ON</button>
          </div>

        </aside>
      </div>

      <footer>
        <div>MI2a</div>
        <div>&copy; Khairouni Ghassen </div>
      </footer>
    </div>
  </div>
  <script>
    const startBtn = document.getElementById('startBtn');
    const resetBtn = document.getElementById('resetBtn');
    const submitBtn = document.getElementById('submitBtn');
    const skipBtn = document.getElementById('skipBtn');
    const questionEl = document.getElementById('question');
    const answerInput = document.getElementById('answerInput');
    const scoreEl = document.getElementById('score');
    const bestEl = document.getElementById('best');
    const lastEl = document.getElementById('last');
    const timeLeftEl = document.getElementById('timeLeft');
    const bar = document.getElementById('bar');
    const askedEl = document.getElementById('asked');
    const streakEl = document.getElementById('streak');
    const difficultySel = document.getElementById('difficulty');
    const timeRange = document.getElementById('timeRange');
    const timePerQEl = document.getElementById('timePerQ');
    const toggleSoundBtn = document.getElementById('toggleSound');

    let state = {
      running: false,
      score: 0,
      best: Number(localStorage.getItem('jeucalc_best')||0),
      asked: 0,
      streak: 0,
      currentAnswer: null,
      intervalId: null,
      perQuestion: Number(timeRange.value),
      timeLeft: 0,
      sound: true
    };

    bestEl.textContent = state.best;
    timePerQEl.textContent = state.perQuestion;
    timeLeftEl.textContent = '--';

    function randInt(a,b){return Math.floor(Math.random()*(b-a+1))+a}

    function generate(difficulty){
      const opPool = ['+','-','*','/'];
      let a,b,op;
      if(difficulty==='easy'){
        op = opPool[randInt(0,1)];
        a = randInt(1,20);
        b = randInt(1,20);
      } else if(difficulty==='medium'){
        op = opPool[randInt(0,2)];
        a = randInt(2,50);
        b = randInt(1,12);
      } else {
        op = opPool[randInt(0,3)];
        a = randInt(10,150);
        b = randInt(2,25);
      }

      if(op === '/'){
        b = randInt(2,12);
        const prod = a * b;
        a = prod; 
      }

      const expr = `${a} ${op} ${b}`;
      let ans = eval(expr);
      if(op === '/') ans = Math.trunc(ans);
      return {question:expr,answer:ans};
    }

    function startGame(){
      if(state.running) return;
      state.running = true;
      state.score = 0; state.asked=0; state.streak=0;
      updateUI();
      nextQuestion();
    }

    function resetGame(){
      clearInterval(state.intervalId);
      state.running = false;
      state.score = 0; state.asked=0; state.streak=0; state.currentAnswer=null; state.timeLeft='--';
      bar.style.width='0%';
      questionEl.textContent = 'Appuyez sur Démarrer pour recommencer';
      updateUI();
    }

    function updateUI(){
      scoreEl.textContent = state.score;
      bestEl.textContent = state.best;
      askedEl.textContent = state.asked;
      streakEl.textContent = state.streak;
      timePerQEl.textContent = state.perQuestion;
      timeRange.value = state.perQuestion;
    }

    function tick(){
      if(typeof state.timeLeft !== 'number') return;
      state.timeLeft -= 0.25; // t250msick every 
      if(state.timeLeft <= 0){
        wrongAnswer('Temps écoulé');
        return;
      }
      timeLeftEl.textContent = Math.ceil(state.timeLeft)+'s';
      const pct = Math.max(0,((state.perQuestion-state.timeLeft)/state.perQuestion)*100);
      bar.style.width = `${pct}%`;
    }

    function scheduleTicker(){
      clearInterval(state.intervalId);
      state.intervalId = setInterval(tick,250);
    }

    function nextQuestion(){
      const dif = difficultySel.value;
      const {question,answer} = generate(dif);
      state.currentAnswer = answer;
      questionEl.textContent = question;
      answerInput.value='';
      answerInput.focus();
      state.timeLeft = state.perQuestion;
      timeLeftEl.textContent = state.perQuestion+'s';
      bar.style.width = '0%';
      scheduleTicker();
    }

    function correctAnswer(){
      state.score += 10;
      state.asked += 1;
      state.streak += 1;
      lastEl.textContent = 'Correct ✓';

      finishRound();
    }

    function wrongAnswer(reason='Mauvais'){ 
      state.asked += 1;
      state.streak = 0;
      lastEl.textContent = reason + ' ✗';

      finishRound();
    }

    function finishRound(){
      clearInterval(state.intervalId);
      bar.style.width='100%';
      setTimeout(()=>{
        if(state.score > state.best){ state.best = state.score; localStorage.setItem('jeucalc_best', state.best); }
        updateUI();
        if(state.running) nextQuestion();
      }, 350);
    }
    startBtn.addEventListener('click', ()=>{
      startGame();
    });

    resetBtn.addEventListener('click', ()=>{
      resetGame();
    });

    submitBtn.addEventListener('click', ()=>{
      if(!state.running) return;
      const val = answerInput.value.trim();
      if(val==='') return;
      const num = Number(val);
      if(Number.isNaN(num)) { wrongAnswer('Entrée invalide'); return; }
      if(num === state.currentAnswer) correctAnswer(); else wrongAnswer('Incorrect');
    });
 
    skipBtn.addEventListener('click', ()=>{
      if(!state.running) return;
      wrongAnswer('Passée');
    });

    (function(){
      const b = Number(localStorage.getItem('jeucalc_best')||0);
      state.best = b; bestEl.textContent = b;
    })();

    setTimeout(()=>{
      questionEl.style.transition='transform 400ms ease';
      questionEl.style.transform='translateY(0)';
    },200);

  </script>
</body>
</html>
