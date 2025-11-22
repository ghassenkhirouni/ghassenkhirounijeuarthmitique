<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu de Calcul Mental</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background: #f0f0f0;
      margin: 0;
    }
    .container {
      text-align: center;
      background: white;
      padding: 2rem;
      border-radius: 15px;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
      transition: background 0.3s;
    }
    input {
      padding: 0.5rem;
      font-size: 1.2rem;
      width: 100px;
      text-align: center;
    }
    button {
      margin-left: 10px;
      padding: 0.5rem 1rem;
      font-size: 1rem;
      border: none;
      border-radius: 10px;
      background: #0078ff;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background: #005fcc;
    }
    .good { background-color: #c8f7c5 !important; }
    .bad { background-color: #f7c5c5 !important; }
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
</body>
</html>
