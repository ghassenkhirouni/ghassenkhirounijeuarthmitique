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
      background: #0078ff;
      color: white;
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
</body>
</html>
