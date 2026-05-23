[index.html](https://github.com/user-attachments/files/28172213/index.html)
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#1a1a2e">
<meta name="format-detection" content="telephone=no">
<meta name="apple-mobile-web-app-title" content="만세 1919">
<meta name="application-name" content="만세 1919">
  <title>만세 1919</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html,
    body {
      width: 100%;
      height: 100%;
      overflow: hidden;
    }

    body {
      position: fixed;
      inset: 0;
      background: #1a1a2e;
      color: #fff;
      font-family: -apple-system, BlinkMacSystemFont, "Noto Sans KR", "Segoe UI", sans-serif;
      touch-action: manipulation;
      user-select: none;
      -webkit-touch-callout: none;
      -webkit-tap-highlight-color: transparent;
      -webkit-user-select: none;
      overscroll-behavior: none;
    }

    button,
    input {
      font: inherit;
    }

    #game-container {
      position: relative;
      width: 100vw;
      height: 100vh;
      height: 100dvh;
      height: var(--app-height, 100dvh);
      min-height: -webkit-fill-available;
      overflow: hidden;
      background:
        radial-gradient(circle at 20% 18%, rgba(233, 69, 96, 0.18), transparent 32%),
        linear-gradient(135deg, #1a1a2e 0%, #16213e 52%, #0f3460 100%);
    }

    canvas {
      display: block;
      width: 100%;
      height: 100%;
      cursor: crosshair;
      touch-action: none;
    }

    .btn {
      min-height: 46px;
      padding: 0 30px;
      border: 0;
      border-radius: 8px;
      background: #e94560;
      color: #fff;
      font-size: 1.05rem;
      font-weight: 800;
      cursor: pointer;
      box-shadow: 0 10px 24px rgba(233, 69, 96, 0.28);
      transition: transform 0.15s ease, background 0.15s ease;
    }

    .btn:hover {
      background: #ff6077;
    }

    .btn:active {
      transform: scale(0.96);
    }

    .btn.secondary {
      background: #2a9d8f;
      box-shadow: 0 10px 24px rgba(42, 157, 143, 0.24);
    }

    #start-screen,
    #clear-overlay,
    #ending-screen {
      position: absolute;
      inset: 0;
      z-index: 100;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
      left: 0;
      top: 0;
      padding:
        max(28px, env(safe-area-inset-top))
        max(28px, env(safe-area-inset-right))
        max(28px, env(safe-area-inset-bottom))
        max(28px, env(safe-area-inset-left));
      text-align: center;
    }

    #start-screen {
      background: linear-gradient(135deg, #1a1a2e 0%, #16213e 52%, #0f3460 100%);
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;
      touch-action: pan-y;
      transition: opacity 0.35s ease;
    }

    #start-screen h1 {
      margin-bottom: 10px;
      color: #e94560;
      font-size: clamp(3rem, 12vw, 6rem);
      letter-spacing: 0;
      line-height: 1;
      text-shadow: 0 0 22px rgba(233, 69, 96, 0.58);
    }

    #start-screen .subtitle {
      margin-bottom: 34px;
      color: #d7dfef;
      font-size: clamp(1rem, 4vw, 1.35rem);
      word-break: keep-all;
    }

    .start-panel {
      width: min(720px, 94vw);
      margin-bottom: 24px;
      padding: clamp(16px, 3vw, 24px);
      border: 1px solid rgba(255, 255, 255, 0.16);
      border-radius: 8px;
      background: rgba(10, 15, 30, 0.56);
      box-shadow: 0 18px 50px rgba(0, 0, 0, 0.28);
      text-align: left;
    }

    .start-panel h2 {
      margin-bottom: 8px;
      color: #ffd700;
      font-size: clamp(1.15rem, 4vw, 1.45rem);
      text-align: center;
    }

    .start-panel p {
      color: #e5edf8;
      font-size: clamp(0.9rem, 2.8vw, 1rem);
      line-height: 1.58;
      word-break: keep-all;
    }

    .intro-list {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin-top: 14px;
    }

    .intro-item {
      min-height: 82px;
      padding: 12px;
      border-radius: 8px;
      background: rgba(255, 255, 255, 0.08);
    }

    .intro-item strong {
      display: block;
      margin-bottom: 6px;
      color: #ffd700;
      font-size: 0.9rem;
    }

    .intro-item span {
      display: block;
      color: #d7dfef;
      font-size: 0.82rem;
      line-height: 1.38;
      word-break: keep-all;
    }

    #game-ui {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 10;
      display: none;
      width: 100%;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      padding:
        max(10px, env(safe-area-inset-top))
        max(20px, env(safe-area-inset-right))
        10px
        max(20px, env(safe-area-inset-left));
      background: rgba(0, 0, 0, 0.52);
      border-bottom: 1px solid rgba(255, 255, 255, 0.09);
      pointer-events: none;
    }

    #stage-label {
      color: #ffd700;
      font-size: 0.9rem;
      font-weight: 900;
    }

    #score {
      color: #fff;
      font-weight: 900;
    }

    #hp-bar-wrap {
      position: absolute;
      top: calc(max(10px, env(safe-area-inset-top)) + 38px);
      left: max(20px, env(safe-area-inset-left));
      right: max(20px, env(safe-area-inset-right));
      z-index: 10;
      display: none;
      height: 18px;
      overflow: hidden;
      border: 1px solid #555;
      border-radius: 6px;
      background: #333;
      pointer-events: none;
    }

    #hp-bar {
      width: 100%;
      height: 100%;
      border-radius: 6px;
      background: linear-gradient(90deg, #e94560, #ff6b6b);
      transition: width 0.15s ease, background 0.15s ease;
    }

    #combo {
      position: absolute;
      top: calc(max(10px, env(safe-area-inset-top)) + 66px);
      right: max(20px, env(safe-area-inset-right));
      z-index: 12;
      color: #ffd700;
      font-size: 1.35rem;
      font-weight: 900;
      opacity: 0;
      text-shadow: 2px 2px 0 #000;
      transition: opacity 0.15s ease;
      pointer-events: none;
    }

    #speech {
      position: absolute;
      top: calc(max(10px, env(safe-area-inset-top)) + 66px);
      left: max(20px, env(safe-area-inset-left));
      z-index: 12;
      display: none;
      max-width: min(430px, 62vw);
      padding: 8px 14px;
      border-radius: 8px;
      background: rgba(255, 255, 255, 0.96);
      color: #1f2937;
      font-size: 0.88rem;
      font-weight: 800;
      line-height: 1.45;
      pointer-events: none;
    }

    #weapon-bar {
      position: absolute;
      bottom: max(15px, calc(env(safe-area-inset-bottom) + 12px));
      left: 50%;
      z-index: 12;
      display: none;
      gap: 10px;
      transform: translateX(-50%);
      padding: 8px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 8px;
      background: rgba(0, 0, 0, 0.58);
    }

    .weapon-slot {
      width: 58px;
      height: 58px;
      border: 2px solid #555;
      border-radius: 8px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background: rgba(255, 255, 255, 0.06);
      cursor: pointer;
      transition: transform 0.15s ease, border-color 0.15s ease, background 0.15s ease;
    }

    .weapon-slot.active {
      border-color: #e94560;
      background: rgba(233, 69, 96, 0.28);
      box-shadow: 0 0 0 3px rgba(233, 69, 96, 0.16);
    }

    .weapon-slot:active {
      transform: scale(0.95);
    }

    .weapon-slot .emoji {
      font-size: 1.4rem;
      line-height: 1;
    }

    .weapon-slot .label {
      margin-top: 4px;
      color: #b6c0d0;
      font-size: 0.58rem;
      font-weight: 700;
    }

    .damage-number {
      position: absolute;
      z-index: 25;
      color: #ffd700;
      font-size: 1.55rem;
      font-weight: 900;
      text-shadow: 2px 2px 0 #000, 0 0 12px rgba(255, 215, 0, 0.5);
      pointer-events: none;
      animation: dmgPop 0.9s ease-out forwards;
    }

    .damage-number.critical {
      color: #ff3333;
      font-size: 2.2rem;
      text-shadow: 2px 2px 0 #000, 0 0 16px rgba(255, 0, 0, 0.7);
    }

    @keyframes dmgPop {
      0% {
        opacity: 1;
        transform: translate(-50%, -50%) scale(1.2);
      }
      32% {
        opacity: 1;
        transform: translate(-50%, -90%) scale(1.45);
      }
      100% {
        opacity: 0;
        transform: translate(-50%, -180%) scale(0.6);
      }
    }

    #clear-overlay {
      z-index: 60;
      display: none;
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;
      touch-action: pan-y;
      background: rgba(0, 0, 0, 0.86);
    }

    #clear-overlay h2 {
      margin-bottom: 14px;
      color: #ffd700;
      font-size: clamp(1.8rem, 6vw, 3rem);
    }

    #clear-overlay .fact {
      max-width: 620px;
      margin-bottom: 24px;
      color: #e3e8f5;
      font-size: 0.98rem;
      line-height: 1.72;
      word-break: keep-all;
    }

    #ending-screen {
      display: none;
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;
      touch-action: pan-y;
      background: linear-gradient(135deg, #0f3460 0%, #16213e 52%, #1a1a2e 100%);
    }

    #ending-screen h1 {
      margin-bottom: 6px;
      color: #ffd700;
      font-size: clamp(2.15rem, 7vw, 4rem);
      line-height: 1.08;
      text-shadow: 0 0 20px rgba(255, 215, 0, 0.8), 0 0 40px rgba(255, 215, 0, 0.36);
      animation: pulse 1.5s ease-in-out infinite;
    }

    .rank-badge {
      margin: 8px 0;
      color: #ff6b6b;
      font-size: clamp(1.25rem, 4vw, 1.75rem);
      font-weight: 900;
      text-shadow: 0 0 15px rgba(255, 107, 107, 0.55);
    }

    .score-final {
      margin: 4px 0;
      color: #fff;
      font-size: 1rem;
      font-weight: 800;
    }

    .grade-box {
      margin: 12px 0 16px;
      padding: 8px 24px;
      border: 3px solid currentColor;
      border-radius: 8px;
      font-size: 2rem;
      font-weight: 900;
      animation: gradeGlow 2s ease-in-out infinite alternate;
    }

    .grade-S { color: #ffd700; background: rgba(255, 215, 0, 0.15); }
    .grade-A { color: #ff6b6b; background: rgba(255, 107, 107, 0.15); }
    .grade-B { color: #4ecdc4; background: rgba(78, 205, 196, 0.15); }
    .grade-C { color: #95afc0; background: rgba(149, 175, 192, 0.15); }

    #ending-flag {
      position: absolute;
      top: 50%;
      left: 50%;
      width: min(360px, 80vw);
      opacity: 0.08;
      transform: translate(-50%, -50%);
      animation: flagWave 4s ease-in-out infinite;
      pointer-events: none;
    }

    #nickname-entry {
      z-index: 2;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      margin-top: 10px;
      flex-wrap: wrap;
    }

    #nickname-input {
      width: 180px;
      height: 46px;
      border: 1px solid rgba(255, 255, 255, 0.22);
      border-radius: 8px;
      padding: 0 12px;
      background: rgba(255, 255, 255, 0.94);
      color: #151923;
      font-weight: 900;
    }

    #register-status {
      width: 100%;
      min-height: 22px;
      color: #ffd700;
      font-size: 0.86rem;
      font-weight: 800;
    }

    #leaderboard {
      z-index: 2;
      width: min(640px, 92vw);
      margin-top: 12px;
      padding: 12px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 8px;
      background: rgba(255, 255, 255, 0.04);
      color: #fff;
    }

    #leaderboard h3 {
      margin-bottom: 8px;
      color: #ffd700;
      font-size: 1rem;
      text-align: left;
    }

    .lb-row {
      display: grid;
      grid-template-columns: 44px 1fr 80px 86px;
      align-items: center;
      gap: 8px;
      padding: 7px 8px;
      border-radius: 6px;
      opacity: 0;
      transform: translateY(6px);
      transition: opacity 0.22s ease, transform 0.22s ease;
    }

    .lb-row.show {
      opacity: 1;
      transform: translateY(0);
    }

    .lb-row.current {
      background: rgba(233, 69, 96, 0.18);
    }

    .lb-rank {
      font-size: 1.05rem;
      font-weight: 900;
    }

    .lb-name {
      color: #fff;
      font-weight: 800;
      text-align: left;
    }

    .lb-grade {
      color: #cdd8e7;
      font-size: 0.76rem;
      text-align: left;
    }

    .lb-score,
    .lb-time {
      text-align: right;
      font-size: 0.9rem;
      font-weight: 800;
    }

    #ending-replay-btn {
      z-index: 2;
      margin-top: 14px;
    }

    .firework {
      position: absolute;
      width: 6px;
      height: 6px;
      border-radius: 50%;
      pointer-events: none;
      animation: fireworkBurst 1.3s ease-out forwards;
    }

    .debris-particle {
      position: absolute;
      width: 4px;
      height: 4px;
      background: #888;
      pointer-events: none;
      animation: debrisFall 3s linear forwards;
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.04); }
    }

    @keyframes gradeGlow {
      0% { box-shadow: 0 0 10px currentColor; }
      100% { box-shadow: 0 0 30px currentColor, 0 0 58px currentColor; }
    }

    @keyframes flagWave {
      0%, 100% { transform: translate(-50%, -50%) rotate(-2deg); }
      50% { transform: translate(-50%, -50%) rotate(2deg); }
    }

    @keyframes fireworkBurst {
      0% { opacity: 1; transform: translate(0, 0) scale(1); }
      100% { opacity: 0; transform: translate(var(--tx), var(--ty)) scale(0); }
    }

    @keyframes debrisFall {
      0% { opacity: 1; transform: translateY(0) rotate(0deg); }
      100% { opacity: 0; transform: translateY(100vh) rotate(720deg); }
    }

    @media (max-width: 560px) {
      #start-screen {
        justify-content: flex-start;
        padding-top: max(22px, env(safe-area-inset-top));
        overflow-y: auto;
      }

      #start-screen h1 {
        font-size: clamp(2.6rem, 13vw, 4.2rem);
      }

      #start-screen .subtitle {
        margin-bottom: 18px;
      }

      .start-panel {
        margin-bottom: 18px;
      }

      .intro-list {
        grid-template-columns: 1fr;
      }

      .intro-item {
        min-height: 0;
      }

      #stage-label {
        max-width: 68vw;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }

      #speech {
        max-width: 58vw;
        font-size: 0.78rem;
      }

      .weapon-slot {
        width: 52px;
        height: 52px;
      }

      .lb-row {
        grid-template-columns: 36px 1fr 66px 76px;
        gap: 5px;
        font-size: 0.82rem;
      }
    }

    @media (max-height: 680px) {
      #start-screen,
      #ending-screen,
      #clear-overlay {
        justify-content: flex-start;
      }

      #start-screen h1 {
        font-size: clamp(2.35rem, 10vw, 3.6rem);
      }

      #start-screen .subtitle {
        margin-bottom: 14px;
      }

      .start-panel {
        margin-bottom: 14px;
        padding: 14px;
      }

      .intro-item {
        min-height: 68px;
        padding: 9px;
      }

      #ending-screen h1 {
        font-size: clamp(2rem, 8vw, 3rem);
      }

      .grade-box {
        margin: 8px 0 10px;
        padding: 6px 18px;
        font-size: 1.55rem;
      }

      #leaderboard {
        margin-top: 8px;
        padding: 9px;
      }

      #ending-replay-btn {
        margin-top: 10px;
      }
    }

    @media (orientation: landscape) and (max-height: 520px) {
      #start-screen h1 {
        font-size: 2.4rem;
      }

      .start-panel {
        width: min(780px, 92vw);
      }

      .intro-list {
        grid-template-columns: repeat(3, 1fr);
      }

      #weapon-bar {
        transform: translateX(-50%) scale(0.9);
        transform-origin: bottom center;
      }

      #speech {
        max-width: 52vw;
      }
    }
  </style>
</head>
<body>
  <div id="game-container">
    <canvas id="game-canvas"></canvas>

    <div id="game-ui">
      <span id="stage-label">STAGE 1</span>
      <span id="score">0점</span>
    </div>

    <div id="hp-bar-wrap"><div id="hp-bar"></div></div>
    <div id="combo"></div>
    <div id="speech"></div>
    <div id="weapon-bar"></div>

    <div id="start-screen">
      <h1>만세 1919</h1>
      <p class="subtitle">독립의 함성을 울려라!</p>
      <div class="start-panel">
        <h2>작은 손으로 독립의 길을 열어요</h2>
        <p>
          1919년, 사람들은 태극기를 들고 독립을 외쳤습니다.
          목표를 눌러 작전을 성공시키고 마지막에 대한독립만세를 외쳐요.
        </p>
        <div class="intro-list">
          <div class="intro-item">
            <strong>목표</strong>
            <span>4개의 작전을 차례대로 성공시켜요.</span>
          </div>
          <div class="intro-item">
            <strong>방법</strong>
            <span>가운데를 누르고 아래 무기를 바꿔요.</span>
          </div>
          <div class="intro-item">
            <strong>읽기</strong>
            <span>끝날 때마다 짧은 이야기가 나와요.</span>
          </div>
        </div>
      </div>
      <button class="btn" id="start-btn">게임 시작</button>
    </div>

    <div id="clear-overlay">
      <h2 id="clear-title"></h2>
      <p class="fact" id="clear-fact"></p>
      <button class="btn" id="clear-btn">다음 스테이지</button>
    </div>

    <div id="ending-screen">
      <img class="taegeukgi-bg" id="ending-flag" alt="">
      <h1>대한독립만세!</h1>
      <div class="rank-badge" id="ending-rank">독립운동 마스터</div>
      <p class="score-final" id="ending-score"></p>
      <p class="score-final" id="ending-combo"></p>
      <p class="score-final" id="ending-time"></p>
      <div class="grade-box" id="ending-grade"></div>

      <div id="nickname-entry">
        <input id="nickname-input" maxlength="5" placeholder="닉네임">
        <button id="save-score-btn" class="btn">순위 등록</button>
        <p id="register-status"></p>
      </div>

      <div id="leaderboard"></div>
      <button class="btn secondary" id="ending-replay-btn">한 번 더 만세!</button>
    </div>
  </div>

  <script>
    const TAEGEUKGI_SVG = `<svg xmlns="http://www.w3.org/2000/svg" width="900" height="600" viewBox="-36 -24 72 48"><path fill="#fff" d="M-36-24h72v48h-72z"/><g transform="rotate(-56.31)"><g id="b"><path id="a" stroke="#000" stroke-width="2" d="M-6-25H6m-12 3H6m-12 3H6"/><use href="#a" y="44"/></g><path stroke="#fff" d="M0 17v10"/><circle r="12" fill="#cd2e3a"/><path fill="#0047a0" d="M0-12A6 6 0 0 0 0 0a6 6 0 0 1 0 12 12 12 0 0 1 0-24Z"/></g><g transform="rotate(-123.69)"><use href="#b"/><path stroke="#fff" d="M0-23.5v3M0 17v3.5m0 3v3"/></g></svg>`;
    const taegeukgiImg = new Image();
    taegeukgiImg.src = "data:image/svg+xml;base64," + btoa(unescape(encodeURIComponent(TAEGEUKGI_SVG)));

    const STAGES = [
      {
        name: "조선총독부",
        description: "일제 식민통치의 상징, 조선총독부를 무너뜨려라!",
        hp: 980,
        shape: "building",
        characterName: "임대수",
        speeches: ["대한의 독립을 위하여!", "이 건물을 무너뜨리겠다!", "우리 땅에서 물러가라!"],
        historicalFact: "조선총독부 청사는 일제 식민통치의 상징 건물로, 1926년 경복궁 앞에 세워졌습니다. 1995년 철거가 시작되며 경복궁 복원이 진행되었습니다.",
      },
      {
        name: "태극기 목판 인쇄",
        description: "목판을 눌러 태극기를 인쇄하라!",
        hp: 860,
        shape: "woodblock",
        characterName: "이수욱",
        speeches: ["한 장 더! 태극기를 찍어라!", "이 깃발이 독립의 씨앗이다!", "비밀리에, 그러나 힘차게!", "종이마다 독립의 뜻을 새기자!"],
        historicalFact: "일제강점기 독립운동가들은 비밀리에 태극기를 제작하여 만세운동에 사용하였습니다. 목판 인쇄 등의 방법으로 수많은 태극기가 만들어졌습니다.",
      },
      {
        name: "태극기 게양",
        description: "일장기를 끌어내리고 태극기를 높이 올려라!",
        hp: 820,
        shape: "flag_ceremony",
        characterName: "가네코 후미코",
        speeches: ["일장기를 끌어내려라!", "태극기를 높이 올리자!", "이 땅의 깃발은 태극기다!", "대한의 하늘에 태극기를!"],
        historicalFact: "가네코 후미코는 일본인이면서도 조선의 독립운동에 헌신한 인물입니다. 박열과 함께 일제에 저항하며 식민지 지배의 부당함을 알렸습니다.",
      },
      {
        name: "동양척식주식회사",
        description: "수탈의 본거지를 무너뜨려라!",
        hp: 1120,
        shape: "company_building",
        characterName: "홍일섭",
        speeches: ["빼앗긴 땅을 되찾자!", "수탈의 근거지를 부숴라!", "우리 땅 우리 손으로!", "만세! 만세! 대한독립만세!"],
        historicalFact: "동양척식주식회사는 일제가 한국의 토지와 자원을 수탈하기 위해 1908년 설립한 국책회사입니다. 한국 농민의 토지를 빼앗아 일본인에게 넘기는 식민 착취의 핵심 기구였습니다.",
      },
    ];

    const WEAPONS = [
      { name: "주먹", damage: 4, speed: 1.35, emoji: "✊", particleColor: "#ffaa00" },
      { name: "돌멩이", damage: 8, speed: 1.1, emoji: "●", particleColor: "#9aa0a6" },
      { name: "태극기 봉", damage: 13, speed: 0.95, emoji: "🇰🇷", particleColor: "#e94560" },
      { name: "폭탄", damage: 24, speed: 0.55, emoji: "💣", particleColor: "#ff4400" },
    ];

    const LEADERBOARD_KEY = "manse1919_leaderboard";
    const canvas = document.getElementById("game-canvas");
    const ctx = canvas.getContext("2d");

    const gameState = {
      isPlaying: false,
      currentStage: 0,
      currentWeapon: 0,
      score: 0,
      combo: 0,
      maxCombo: 0,
      lastHitTime: 0,
      targetHP: 0,
      targetMaxHP: 0,
      shakeIntensity: 0,
      particles: [],
      cracks: [],
      startedAt: 0,
      _loopStarted: false,
      _lastTime: performance.now(),
      _endingTimers: [],
    };

    let pendingLeaderboardEntry = null;
    let runtimeLeaderboard = [];

    function resizeCanvas() {
      const vv = window.visualViewport;
      const width = vv ? vv.width : window.innerWidth;
      const height = vv ? vv.height : window.innerHeight;
      const ratio = window.devicePixelRatio || 1;
      document.documentElement.style.setProperty("--app-height", height + "px");
      canvas.width = Math.floor(width * ratio);
      canvas.height = Math.floor(height * ratio);
      canvas.style.width = width + "px";
      canvas.style.height = height + "px";
      ctx.setTransform(ratio, 0, 0, ratio, 0, 0);
    }

    function vw() {
      return canvas.width / (window.devicePixelRatio || 1);
    }

    function vh() {
      return canvas.height / (window.devicePixelRatio || 1);
    }

    window.addEventListener("resize", resizeCanvas);
    if (window.visualViewport) window.visualViewport.addEventListener("resize", resizeCanvas);
    resizeCanvas();

    async function requestWakeLock() {
      try {
        if ("wakeLock" in navigator) await navigator.wakeLock.request("screen");
      } catch (error) {
        return null;
      }
    }

    function startGame() {
      requestWakeLock();
      const startScreen = document.getElementById("start-screen");
      startScreen.style.opacity = "0";
      setTimeout(() => {
        startScreen.style.display = "none";
        clearEndingEffects();
        gameState.score = 0;
        gameState.combo = 0;
        gameState.maxCombo = 0;
        gameState.startedAt = Date.now();
        gameState.currentWeapon = 0;
        initStage(0);
      }, 300);
    }

    function initStage(index) {
      const stage = STAGES[index];
      gameState.currentStage = index;
      gameState.isPlaying = true;
      gameState.targetHP = stage.hp;
      gameState.targetMaxHP = stage.hp;
      gameState.combo = 0;
      gameState.lastHitTime = 0;
      gameState.shakeIntensity = 0;
      gameState.particles = [];
      gameState.cracks = [];
      gameState.currentWeapon = 0;

      document.getElementById("game-ui").style.display = "flex";
      document.getElementById("hp-bar-wrap").style.display = "block";
      document.getElementById("weapon-bar").style.display = "flex";
      document.getElementById("clear-overlay").style.display = "none";
      document.getElementById("ending-screen").style.display = "none";
      document.getElementById("stage-label").textContent = `STAGE ${index + 1} - ${stage.name}`;
      updateScore();
      updateHPBar();
      updateComboDisplay();
      buildWeaponBar();
      showSpeech(stage.speeches[0]);

      if (!gameState._loopStarted) {
        gameState._loopStarted = true;
        gameState._lastTime = performance.now();
        requestAnimationFrame(gameLoop);
      }
    }

    function buildWeaponBar() {
      const bar = document.getElementById("weapon-bar");
      bar.innerHTML = "";
      WEAPONS.forEach((weapon, index) => {
        const slot = document.createElement("button");
        slot.type = "button";
        slot.className = "weapon-slot" + (index === gameState.currentWeapon ? " active" : "");
        slot.dataset.idx = String(index);
        slot.title = weapon.name;
        const emoji = document.createElement("span");
        emoji.className = "emoji";
        emoji.textContent = weapon.emoji;
        const label = document.createElement("span");
        label.className = "label";
        label.textContent = weapon.name;
        slot.append(emoji, label);
        slot.addEventListener("click", () => selectWeapon(index));
        bar.appendChild(slot);
      });
    }

    function selectWeapon(index) {
      gameState.currentWeapon = index;
      document.querySelectorAll(".weapon-slot").forEach((slot, i) => {
        slot.classList.toggle("active", i === index);
      });
    }

    function gameLoop(now) {
      const dt = Math.min((now - gameState._lastTime) / 1000, 0.05);
      gameState._lastTime = now;
      if (gameState.isPlaying) {
        update(dt);
        render();
      }
      requestAnimationFrame(gameLoop);
    }

    function update(dt) {
      for (let i = gameState.particles.length - 1; i >= 0; i--) {
        const p = gameState.particles[i];
        p.x += p.vx;
        p.y += p.vy;
        p.vy += p.gravity || 0.26;
        p.life -= dt;
        p.rotation += p.rotationSpeed || 0;
        if (p.life <= 0) gameState.particles.splice(i, 1);
      }

      if (gameState.shakeIntensity > 0) {
        gameState.shakeIntensity *= 0.85;
        if (gameState.shakeIntensity < 0.05) gameState.shakeIntensity = 0;
      }

      if (gameState.combo > 0 && Date.now() - gameState.lastHitTime > 3200) {
        gameState.combo = 0;
        updateComboDisplay();
      }
    }

    function render() {
      ctx.clearRect(0, 0, vw(), vh());
      ctx.save();
      if (gameState.shakeIntensity > 0) {
        ctx.translate(
          (Math.random() - 0.5) * gameState.shakeIntensity * 14,
          (Math.random() - 0.5) * gameState.shakeIntensity * 14
        );
      }
      drawBackground();
      drawTarget();
      drawCharacter();
      drawParticles();
      drawCracks();
      ctx.restore();
    }

    function drawBackground() {
      const width = vw();
      const height = vh();
      const grad = ctx.createLinearGradient(0, 0, 0, height);
      grad.addColorStop(0, "#16213e");
      grad.addColorStop(1, "#1a1a2e");
      ctx.fillStyle = grad;
      ctx.fillRect(0, 0, width, height);

      ctx.fillStyle = "#2a2a3a";
      ctx.fillRect(0, height * 0.75, width, height * 0.25);
      ctx.fillStyle = "#333345";
      ctx.fillRect(0, height * 0.75, width, 3);

      ctx.fillStyle = "rgba(255,255,255,0.06)";
      ctx.font = `900 ${Math.min(54, width * 0.075)}px sans-serif`;
      ctx.textAlign = "center";
      ctx.fillText(STAGES[gameState.currentStage].name, width / 2, height * 0.65);
    }

    function drawTarget() {
      const stage = STAGES[gameState.currentStage];
      const cx = vw() / 2;
      const cy = vh() / 2 - 30;
      const damage = 1 - gameState.targetHP / gameState.targetMaxHP;

      if (stage.shape === "building") drawBuilding(cx, cy, damage);
      if (stage.shape === "woodblock") drawWoodblock(cx, cy, damage);
      if (stage.shape === "flag_ceremony") drawFlagCeremony(cx, cy, damage);
      if (stage.shape === "company_building") drawCompanyBuilding(cx, cy, damage);
    }

    function drawBuilding(cx, cy, damage) {
      const w = Math.min(290, vw() * 0.62);
      const h = w * 0.78;
      const x = cx - w / 2;
      const y = cy - h / 2 - 20;

      ctx.fillStyle = "#5f6368";
      ctx.fillRect(x, y + h * 0.18, w, h * 0.82);
      ctx.fillStyle = "#3d4148";
      ctx.beginPath();
      ctx.moveTo(x - 18, y + h * 0.18);
      ctx.lineTo(cx, y);
      ctx.lineTo(x + w + 18, y + h * 0.18);
      ctx.closePath();
      ctx.fill();

      ctx.fillStyle = "#2e3035";
      ctx.fillRect(x - 10, y + h * 0.17, w + 20, 12);
      ctx.fillStyle = "#a4161a";
      ctx.beginPath();
      ctx.arc(cx, y + h * 0.42, w * 0.09, 0, Math.PI * 2);
      ctx.fill();

      for (let row = 0; row < 4; row++) {
        for (let col = 0; col < 5; col++) {
          const wx = x + w * 0.12 + col * w * 0.18;
          const wy = y + h * 0.3 + row * h * 0.145;
          const broken = damage > 0.25 && ((row * 5 + col) / 20) < damage * 0.85;
          ctx.fillStyle = broken ? "#15171b" : "#8ecae6";
          ctx.fillRect(wx, wy, w * 0.09, h * 0.085);
        }
      }

      if (damage > 0.18) drawDamageOverlay(x, y, w, h, damage);
      if (damage < 0.7) drawJapanFlag(cx - 10, y - 15, 28, 19);
      drawCanvasLabel("조선총독부", cx, y + h + 28);
    }

    function drawWoodblock(cx, cy, damage) {
      const printedCount = Math.floor(damage * 30);
      ctx.fillStyle = "#5c3d2e";
      ctx.fillRect(cx - 135, cy + 42, 270, 80);
      ctx.strokeStyle = "#3d2617";
      ctx.lineWidth = 3;
      ctx.strokeRect(cx - 135, cy + 42, 270, 80);
      ctx.fillStyle = "#4a2e1e";
      ctx.fillRect(cx - 124, cy + 120, 15, 44);
      ctx.fillRect(cx + 109, cy + 120, 15, 44);

      ctx.fillStyle = "#6b4226";
      ctx.fillRect(cx - 82, cy - 62, 164, 104);
      ctx.strokeStyle = "#3d2617";
      ctx.lineWidth = 4;
      ctx.strokeRect(cx - 82, cy - 62, 164, 104);
      ctx.globalAlpha = 0.45;
      ctx.drawImage(taegeukgiImg, cx - 55, cy - 45, 110, 73);
      ctx.globalAlpha = 1;

      ctx.fillStyle = "#111";
      ctx.fillRect(cx + 102, cy - 20, 25, 30);
      ctx.beginPath();
      ctx.arc(cx + 114, cy - 20, 14, Math.PI, 0);
      ctx.fill();

      for (let i = 0; i < Math.min(30 - printedCount, 6); i++) {
        ctx.fillStyle = "#f5f0e0";
        ctx.fillRect(cx - 130 + i * 1.5, cy - 122 - i * 2, 55, 37);
      }

      for (let i = 0; i < Math.min(printedCount, 24); i++) {
        const px = cx + 20 + (i % 6) * 23;
        const py = cy - 146 + Math.floor(i / 6) * 23;
        ctx.drawImage(taegeukgiImg, px, py, 21, 14);
      }

      if (Date.now() - gameState.lastHitTime < 180) {
        ctx.fillStyle = "#f5f0e0";
        ctx.fillRect(cx - 55, cy - 45, 110, 73);
        ctx.drawImage(taegeukgiImg, cx - 55, cy - 45, 110, 73);
      }

      drawCanvasLabel(`태극기 ${printedCount} / 30장`, cx, cy + 94);
    }

    function drawFlagCeremony(cx, cy, damage) {
      const topY = cy - 205;
      const bottomY = cy + 50;
      const range = bottomY - topY;
      const flagW = 112;
      const flagH = 74;

      ctx.fillStyle = "#888";
      ctx.fillRect(cx - 5, cy - 214, 10, 356);
      ctx.fillStyle = "#666";
      ctx.fillRect(cx - 42, cy + 142, 84, 20);
      ctx.fillRect(cx - 25, cy + 122, 50, 20);
      ctx.fillStyle = "#daa520";
      ctx.beginPath();
      ctx.arc(cx, cy - 220, 8, 0, Math.PI * 2);
      ctx.fill();

      ctx.strokeStyle = "#aaa";
      ctx.lineWidth = 2;
      ctx.beginPath();
      ctx.moveTo(cx + 6, topY);
      ctx.lineTo(cx + 6, bottomY + 10);
      ctx.stroke();

      if (damage <= 0.5) {
        const progress = damage / 0.5;
        const flagY = topY + progress * range;
        drawJapanFlag(cx + 12, flagY, flagW, flagH);
        drawCanvasLabel(`일장기를 끌어내려라 ${Math.floor(progress * 100)}%`, cx, cy + 112, "#ff8d8d");
      } else {
        const progress = (damage - 0.5) / 0.5;
        const flagY = bottomY - progress * range;
        ctx.drawImage(taegeukgiImg, cx + 12, flagY, flagW, flagH);
        drawCanvasLabel(`태극기를 높이 올려라 ${Math.floor(progress * 100)}%`, cx, cy + 112, "#ffd700");
        if (progress > 0.8) {
          const grad = ctx.createRadialGradient(cx + 65, flagY + 34, 10, cx + 65, flagY + 34, 150);
          grad.addColorStop(0, `rgba(255,215,0,${(progress - 0.8) * 1.6})`);
          grad.addColorStop(1, "rgba(255,215,0,0)");
          ctx.fillStyle = grad;
          ctx.fillRect(cx - 110, flagY - 60, 330, 210);
        }
      }
    }

    function drawCompanyBuilding(cx, cy, damage) {
      const w = Math.min(285, vw() * 0.64);
      const x = cx - w / 2;
      const y = cy - 150;
      ctx.fillStyle = "#4a3828";
      ctx.fillRect(x + 14, y + 250, w - 28, 30);
      ctx.fillStyle = "#5c4033";
      ctx.fillRect(x + 28, y + 130, w - 56, 125);
      ctx.fillStyle = "#4a3328";
      ctx.fillRect(x + 28, y + 10, w - 56, 125);
      ctx.fillStyle = "#3a2218";
      ctx.fillRect(x + 16, y - 15, w - 32, 30);
      ctx.fillStyle = "#2a1a10";
      ctx.fillRect(x + 8, y - 18, w - 16, 6);

      if (damage < 0.42) {
        ctx.fillStyle = "#1a1008";
        ctx.fillRect(cx - 82, y - 10, 164, 22);
        ctx.fillStyle = "#daa520";
        ctx.font = "900 12px sans-serif";
        ctx.textAlign = "center";
        ctx.fillText("동양척식주식회사", cx, y + 5);
      } else {
        ctx.save();
        ctx.translate(cx + 24, y + 268);
        ctx.rotate(0.55);
        ctx.fillStyle = "#1a1008";
        ctx.fillRect(-48, -7, 96, 14);
        ctx.restore();
      }

      for (let floor = 0; floor < 2; floor++) {
        for (let col = 0; col < 4; col++) {
          const wx = x + 52 + col * (w - 104) / 3;
          const wy = floor === 0 ? y + 25 : y + 145;
          const broken = damage > 0.28 && ((col + floor * 4) / 8) < damage * 0.9;
          ctx.fillStyle = broken ? "#0b0b0b" : "#1a3a5a";
          ctx.fillRect(wx, wy, 30, 38);
          ctx.strokeStyle = "#8b7355";
          ctx.lineWidth = 2;
          ctx.strokeRect(wx, wy, 30, 38);
          if (!broken) {
            ctx.beginPath();
            ctx.moveTo(wx + 15, wy);
            ctx.lineTo(wx + 15, wy + 38);
            ctx.moveTo(wx, wy + 19);
            ctx.lineTo(wx + 30, wy + 19);
            ctx.stroke();
          }
        }
      }

      ctx.fillStyle = "#1a0e05";
      ctx.fillRect(cx - 24, y + 190, 48, 65);
      ctx.fillStyle = "#daa520";
      ctx.fillRect(cx - 18, y + 196, 36, 3);

      if (damage > 0.3) drawFlyingPapers(cx, cy, damage);
      if (damage > 0.58) drawSmoke(cx, y - 16, damage);
      if (damage > 0.18) drawDamageOverlay(x, y - 25, w, 300, damage * 0.86);
      drawCanvasLabel("동양척식주식회사", cx, y + 310);
    }

    function drawDamageOverlay(x, y, w, h, damage) {
      ctx.fillStyle = `rgba(0,0,0,${damage * 0.28})`;
      ctx.fillRect(x, y, w, h);
      if (damage > 0.35) {
        ctx.strokeStyle = "rgba(10,10,10,0.85)";
        ctx.lineWidth = 2;
        for (let i = 0; i < Math.floor(damage * 7); i++) {
          const sx = x + 20 + ((i * 47) % Math.max(50, w - 40));
          const sy = y + 20 + ((i * 71) % Math.max(50, h - 40));
          ctx.beginPath();
          ctx.moveTo(sx, sy);
          ctx.lineTo(sx + 18, sy + 23);
          ctx.lineTo(sx - 8, sy + 44);
          ctx.stroke();
        }
      }
      if (damage > 0.68) drawSmoke(x + w / 2, y + 20, damage);
    }

    function drawFlyingPapers(cx, cy, damage) {
      const count = Math.floor((damage - 0.2) * 18);
      for (let i = 0; i < count; i++) {
        const px = cx - 110 + ((i * 37) % 220);
        const py = cy - 190 - ((i * 23) % 84);
        ctx.save();
        ctx.translate(px, py);
        ctx.rotate((i % 6 - 3) * 0.2);
        ctx.fillStyle = i % 3 === 0 ? "#fff8dc" : "#f5f0e0";
        ctx.fillRect(-5, -7, 12, 16);
        ctx.fillStyle = "rgba(0,0,0,0.25)";
        ctx.fillRect(-3, -4, 8, 1);
        ctx.fillRect(-3, 0, 8, 1);
        ctx.fillRect(-3, 4, 8, 1);
        ctx.restore();
      }
    }

    function drawSmoke(cx, cy, damage) {
      for (let i = 0; i < Math.floor(damage * 8); i++) {
        const sx = cx - 80 + ((i * 31) % 160);
        const sy = cy - ((i * 19) % 48);
        const sr = 14 + ((i * 7) % 18);
        ctx.globalAlpha = 0.12 + (i % 4) * 0.03;
        ctx.fillStyle = "#555";
        ctx.beginPath();
        ctx.arc(sx, sy, sr, 0, Math.PI * 2);
        ctx.fill();
      }
      ctx.globalAlpha = 1;
    }

    function drawJapanFlag(x, y, w, h) {
      ctx.fillStyle = "#fff";
      ctx.fillRect(x, y, w, h);
      ctx.fillStyle = "#cc0000";
      ctx.beginPath();
      ctx.arc(x + w / 2, y + h / 2, Math.min(w, h) * 0.28, 0, Math.PI * 2);
      ctx.fill();
      ctx.strokeStyle = "#ccc";
      ctx.lineWidth = 1;
      ctx.strokeRect(x, y, w, h);
    }

    function drawCanvasLabel(text, x, y, color = "#ffd700") {
      ctx.fillStyle = color;
      ctx.font = "900 15px sans-serif";
      ctx.textAlign = "center";
      ctx.fillText(text, x, y);
    }

    function drawCharacter() {
      const stage = STAGES[gameState.currentStage];
      const x = Math.max(64, vw() * 0.15);
      const y = vh() * 0.7;
      ctx.fillStyle = "#fff";
      ctx.fillRect(x - 12, y - 30, 24, 42);
      ctx.fillStyle = "#f0d0a0";
      ctx.beginPath();
      ctx.arc(x, y - 42, 14, 0, Math.PI * 2);
      ctx.fill();
      ctx.fillStyle = "#222";
      ctx.fillRect(x - 19, y - 58, 38, 6);
      ctx.fillRect(x - 8, y - 66, 16, 10);
      ctx.font = "23px sans-serif";
      ctx.textAlign = "center";
      ctx.fillText(WEAPONS[gameState.currentWeapon].emoji, x + 26, y - 15);
      ctx.fillStyle = "#ffd700";
      ctx.font = "900 13px sans-serif";
      ctx.fillText(stage.characterName, x, y + 30);
    }

    function drawParticles() {
      for (const p of gameState.particles) {
        ctx.save();
        ctx.globalAlpha = Math.max(0, p.life * 1.4);
        ctx.translate(p.x, p.y);
        ctx.rotate(p.rotation || 0);
        ctx.fillStyle = p.color;
        if (p.type === "ring") {
          const size = p.size * (1 - p.life);
          ctx.strokeStyle = p.color;
          ctx.lineWidth = 3 + p.life * 4;
          ctx.beginPath();
          ctx.arc(0, 0, size, 0, Math.PI * 2);
          ctx.stroke();
        } else if (p.type === "smoke") {
          ctx.beginPath();
          ctx.arc(0, 0, p.size, 0, Math.PI * 2);
          ctx.fill();
        } else if (p.type === "debris") {
          ctx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size);
        } else {
          ctx.fillRect(-p.size / 2, -p.size / 4, p.size, p.size / 2);
        }
        ctx.restore();
      }
      ctx.globalAlpha = 1;
    }

    function drawCracks() {
      ctx.strokeStyle = "rgba(20,20,20,0.82)";
      ctx.lineWidth = 2;
      for (const crack of gameState.cracks) {
        ctx.beginPath();
        ctx.moveTo(crack.points[0].x, crack.points[0].y);
        for (let i = 1; i < crack.points.length; i++) ctx.lineTo(crack.points[i].x, crack.points[i].y);
        ctx.stroke();
      }
    }

    function attack(x, y) {
      if (!gameState.isPlaying) return;
      const weapon = WEAPONS[gameState.currentWeapon];
      const now = Date.now();
      const cooldown = 1000 / (weapon.speed * 8);
      if (now - gameState.lastHitTime < cooldown) return;

      const cx = vw() / 2;
      const cy = vh() / 2 - 30;
      const hitTarget = x > cx - 165 && x < cx + 165 && y > cy - 220 && y < cy + 220;

      if (!hitTarget) {
        gameState.combo = 0;
        updateComboDisplay();
        createMissParticles(x, y);
        return;
      }

      gameState.combo = now - gameState.lastHitTime < 3200 ? gameState.combo + 1 : 1;
      gameState.lastHitTime = now;
      gameState.maxCombo = Math.max(gameState.maxCombo, gameState.combo);

      const critical = Math.random() < 0.1;
      const comboBonus = 1 + Math.min(gameState.combo, 20) * 0.03;
      let damage = Math.round(weapon.damage * comboBonus);
      if (critical) damage *= 2;
      gameState.targetHP = Math.max(0, gameState.targetHP - damage);
      gameState.score += damage * (critical ? 2 : 1);

      const stage = STAGES[gameState.currentStage];
      const progress = 1 - gameState.targetHP / gameState.targetMaxHP;
      let customText = "";
      if (stage.shape === "woodblock") customText = "찍기!";
      if (stage.shape === "flag_ceremony") customText = progress <= 0.5 ? "내려라!" : "올려라!";

      updateScore();
      updateHPBar();
      updateComboDisplay();
      showDamageNumber(x, y, damage, critical, customText);
      createHitParticles(x, y, stage.shape === "woodblock" ? { ...weapon, particleColor: "#111" } : weapon);

      if ((stage.shape === "building" || stage.shape === "company_building") && Math.random() < 0.44) createCrack(x, y);
      gameState.shakeIntensity = critical ? 5 : 1.8;
      canvas.style.filter = critical ? "brightness(1.8)" : "brightness(1.28)";
      setTimeout(() => { canvas.style.filter = ""; }, critical ? 110 : 55);
      gameState.particles.push({ x, y, vx: 0, vy: 0, gravity: 0, life: 0.42, size: critical ? 62 : 36, color: critical ? "rgba(255,50,50,0.62)" : "rgba(255,255,255,0.42)", type: "ring" });

      if (Math.random() < 0.12) showSpeech(stage.speeches[Math.floor(Math.random() * stage.speeches.length)]);
      if (gameState.combo > 0 && gameState.combo % 10 === 0) comboBurst();
      if (gameState.targetHP <= 0) stageClear();
    }

    function createHitParticles(x, y, weapon) {
      const colors = [weapon.particleColor, "#ffd700", "#fff", "#888", "#ff6b6b"];
      for (let i = 0; i < 16; i++) {
        const angle = Math.random() * Math.PI * 2;
        const speed = 3 + Math.random() * 8;
        gameState.particles.push({
          x, y,
          vx: Math.cos(angle) * speed,
          vy: Math.sin(angle) * speed - 3,
          life: 0.7 + Math.random() * 0.6,
          size: 3 + Math.random() * 8,
          color: colors[Math.floor(Math.random() * colors.length)],
          type: Math.random() < 0.42 ? "debris" : "spark",
          rotation: Math.random() * Math.PI,
          rotationSpeed: (Math.random() - 0.5) * 0.5,
        });
      }
      if (weapon.name === "폭탄") {
        for (let i = 0; i < 18; i++) {
          const angle = Math.random() * Math.PI * 2;
          const speed = 4 + Math.random() * 6;
          gameState.particles.push({
            x, y,
            vx: Math.cos(angle) * speed,
            vy: Math.sin(angle) * speed - 4,
            life: 1 + Math.random() * 0.5,
            size: 10 + Math.random() * 14,
            color: Math.random() < 0.5 ? "#ff4400" : "#ff8800",
            type: "smoke",
            rotation: 0,
            rotationSpeed: 0,
          });
        }
        gameState.particles.push({ x, y, vx: 0, vy: 0, gravity: 0, life: 0.5, size: 80, color: "rgba(255,100,0,0.5)", type: "ring" });
      }
    }

    function createMissParticles(x, y) {
      for (let i = 0; i < 4; i++) {
        gameState.particles.push({
          x, y,
          vx: (Math.random() - 0.5) * 3,
          vy: (Math.random() - 0.5) * 3,
          life: 0.4,
          size: 3,
          color: "#777",
          type: "spark",
        });
      }
    }

    function createCrack(x, y) {
      const points = [{ x, y }];
      let px = x;
      let py = y;
      for (let i = 0; i < 4 + Math.floor(Math.random() * 3); i++) {
        px += (Math.random() - 0.5) * 44;
        py += 5 + Math.random() * 16;
        points.push({ x: px, y: py });
      }
      gameState.cracks.push({ points });
      if (gameState.cracks.length > 28) gameState.cracks.shift();
    }

    function comboBurst() {
      const cx = vw() / 2;
      const cy = vh() / 2 - 30;
      gameState.shakeIntensity = 6;
      gameState.particles.push({ x: cx, y: cy, vx: 0, vy: 0, gravity: 0, life: 0.62, size: 130, color: "rgba(255,215,0,0.5)", type: "ring" });
      for (let i = 0; i < 24; i++) createHitParticles(cx + (Math.random() - 0.5) * 240, cy + (Math.random() - 0.5) * 220, WEAPONS[gameState.currentWeapon]);
    }

    function showDamageNumber(x, y, damage, critical, customText) {
      const el = document.createElement("div");
      el.className = "damage-number" + (critical ? " critical" : "");
      el.textContent = (critical ? "폭발 " : "") + (customText ? customText + " " : "") + "-" + damage;
      el.style.left = x + "px";
      el.style.top = y + "px";
      document.getElementById("game-container").appendChild(el);
      setTimeout(() => el.remove(), 920);
    }

    function updateScore() {
      document.getElementById("score").textContent = gameState.score + "점";
    }

    function updateHPBar() {
      const ratio = gameState.targetHP / gameState.targetMaxHP;
      const hpBar = document.getElementById("hp-bar");
      hpBar.style.width = ratio * 100 + "%";
      if (ratio > 0.5) hpBar.style.background = "linear-gradient(90deg, #e94560, #ff6b6b)";
      else if (ratio > 0.25) hpBar.style.background = "linear-gradient(90deg, #ff8c00, #ffa500)";
      else hpBar.style.background = "linear-gradient(90deg, #ff0, #ffd700)";
    }

    function updateComboDisplay() {
      const el = document.getElementById("combo");
      if (gameState.combo > 1) {
        el.textContent = `COMBO x${gameState.combo}`;
        el.style.opacity = "1";
        el.style.fontSize = Math.min(32, 18 + gameState.combo) + "px";
      } else {
        el.style.opacity = "0";
      }
    }

    function showSpeech(text) {
      const el = document.getElementById("speech");
      el.textContent = text;
      el.style.display = "block";
      clearTimeout(el._timer);
      el._timer = setTimeout(() => {
        el.style.display = "none";
      }, 3300);
    }

    function stageClear() {
      gameState.isPlaying = false;
      gameState.shakeIntensity = 4;
      const cx = vw() / 2;
      const cy = vh() / 2;
      for (let i = 0; i < 36; i++) {
        setTimeout(() => createHitParticles(cx + (Math.random() - 0.5) * 280, cy + (Math.random() - 0.5) * 260, { name: "", particleColor: "#ffd700" }), i * 18);
      }

      setTimeout(() => {
        const stage = STAGES[gameState.currentStage];
        const messages = {
          building: "조선총독부 철거 완료!",
          woodblock: "태극기 인쇄 성공!",
          flag_ceremony: "태극기 게양 완료!",
          company_building: "동양척식주식회사 철거 완료!",
        };
        document.getElementById("clear-title").textContent = messages[stage.shape] || `${stage.name} 완료!`;
        document.getElementById("clear-fact").textContent = stage.historicalFact;
        const button = document.getElementById("clear-btn");
        if (gameState.currentStage < STAGES.length - 1) {
          button.textContent = "다음 스테이지";
          button.onclick = () => initStage(gameState.currentStage + 1);
        } else {
          button.textContent = "결과 보기";
          button.onclick = showEnding;
        }
        document.getElementById("clear-overlay").style.display = "flex";
      }, 900);
    }

    function showEnding() {
      gameState.isPlaying = false;
      document.getElementById("clear-overlay").style.display = "none";
      document.getElementById("game-ui").style.display = "none";
      document.getElementById("hp-bar-wrap").style.display = "none";
      document.getElementById("weapon-bar").style.display = "none";
      document.getElementById("combo").style.opacity = "0";
      document.getElementById("speech").style.display = "none";

      const score = gameState.score;
      const combo = gameState.maxCombo;
      let grade = "C등급";
      let gradeClass = "grade-C";
      let rankTitle = "만세 견습생";
      if (score >= 5200 || combo >= 38) {
        grade = "S등급";
        gradeClass = "grade-S";
        rankTitle = "전설의 독립운동가";
      } else if (score >= 3600 || combo >= 28) {
        grade = "A등급";
        gradeClass = "grade-A";
        rankTitle = "독립운동 마스터";
      } else if (score >= 2300 || combo >= 18) {
        grade = "B등급";
        gradeClass = "grade-B";
        rankTitle = "독립 작전 성공";
      }

      const clearTimeMs = Math.max(1, Date.now() - gameState.startedAt);
      document.getElementById("ending-rank").textContent = rankTitle;
      document.getElementById("ending-score").textContent = `총 점수: ${score}점`;
      document.getElementById("ending-combo").textContent = `최대 콤보: ${combo}`;
      document.getElementById("ending-time").textContent = `클리어 시간: ${formatClearTime(clearTimeMs)}`;
      document.getElementById("ending-flag").src = taegeukgiImg.src;

      const gradeEl = document.getElementById("ending-grade");
      gradeEl.textContent = grade;
      gradeEl.className = "grade-box " + gradeClass;

      pendingLeaderboardEntry = { score, combo, grade, clearTimeMs, date: Date.now() };
      document.getElementById("nickname-entry").style.display = "flex";
      document.getElementById("nickname-input").value = "";
      document.getElementById("register-status").textContent = "닉네임을 입력하고 순위 등록을 누르세요. 비워두면 익명으로 저장됩니다.";
      document.getElementById("ending-screen").style.display = "flex";
      buildLeaderboard();
      startEndingEffects();
    }

    function sanitizeNickname(name) {
      return String(name || "").trim().slice(0, 5).replace(/[<>]/g, "");
    }

    function formatClearTime(ms) {
      const total = Math.floor(ms / 1000);
      const min = Math.floor(total / 60);
      const sec = total % 60;
      const cs = Math.floor((ms % 1000) / 10);
      return `${min}:${String(sec).padStart(2, "0")}.${String(cs).padStart(2, "0")}`;
    }

    function sortLeaderboard(entries) {
      return entries.sort((a, b) => {
        if (b.score !== a.score) return b.score - a.score;
        if ((a.clearTimeMs || 0) !== (b.clearTimeMs || 0)) return (a.clearTimeMs || 0) - (b.clearTimeMs || 0);
        return b.date - a.date;
      });
    }

    function getLeaderboard() {
      try {
        const stored = JSON.parse(localStorage.getItem(LEADERBOARD_KEY) || "[]");
        runtimeLeaderboard = stored;
        return stored;
      } catch (error) {
        return runtimeLeaderboard;
      }
    }

    function saveLeaderboard(entries) {
      runtimeLeaderboard = entries;
      try {
        localStorage.setItem(LEADERBOARD_KEY, JSON.stringify(entries));
        return true;
      } catch (error) {
        return false;
      }
    }

    function submitLeaderboard() {
      const status = document.getElementById("register-status");
      if (!pendingLeaderboardEntry) {
        status.textContent = "이번 기록은 이미 처리됐습니다.";
        return;
      }
      const nickname = sanitizeNickname(document.getElementById("nickname-input").value) || "익명";
      const saved = { ...pendingLeaderboardEntry, nickname };
      const entries = getLeaderboard();
      entries.push(saved);
      const top = sortLeaderboard(entries).slice(0, 5);
      const savedToStorage = saveLeaderboard(top);
      pendingLeaderboardEntry = null;
      document.getElementById("nickname-entry").style.display = "none";
      status.textContent = savedToStorage
        ? `${nickname} 이름으로 순위표에 등록됐습니다.`
        : `${nickname} 기록을 화면에 표시했습니다. 이 브라우저에서는 저장이 제한될 수 있어요.`;
      buildLeaderboard(saved.date);
    }

    function buildLeaderboard(currentDate = null) {
      const container = document.getElementById("leaderboard");
      container.innerHTML = "";
      const title = document.createElement("h3");
      title.textContent = "순위표 TOP 5";
      container.appendChild(title);

      const top = sortLeaderboard(getLeaderboard()).slice(0, 5);
      if (!top.length) {
        const empty = document.createElement("p");
        empty.textContent = "아직 등록된 기록이 없습니다.";
        empty.style.color = "#d8e0ec";
        empty.style.textAlign = "left";
        container.appendChild(empty);
        return;
      }

      let currentIndex = -1;
      top.forEach((entry, index) => {
        if (currentDate !== null && entry.date === currentDate) currentIndex = index;
        const row = document.createElement("div");
        row.className = "lb-row" + (entry.date === currentDate ? " current" : "");
        const rank = document.createElement("div");
        rank.className = "lb-rank";
        rank.textContent = index === 0 ? "1위" : index === 1 ? "2위" : index === 2 ? "3위" : `${index + 1}위`;
        const info = document.createElement("div");
        const name = document.createElement("div");
        name.className = "lb-name";
        name.textContent = entry.nickname || "익명";
        const grade = document.createElement("div");
        grade.className = "lb-grade";
        grade.textContent = entry.grade || "";
        info.append(name, grade);
        const score = document.createElement("div");
        score.className = "lb-score";
        score.textContent = `${entry.score || 0}점`;
        const time = document.createElement("div");
        time.className = "lb-time";
        time.textContent = formatClearTime(entry.clearTimeMs || 0);
        row.append(rank, info, score, time);
        container.appendChild(row);
        setTimeout(() => row.classList.add("show"), 20 + index * 60);
      });

      if (currentDate !== null) {
        const note = document.createElement("p");
        note.style.marginTop = "8px";
        note.style.color = "#ffd700";
        note.textContent = currentIndex >= 0 ? `이번 기록: ${currentIndex + 1}위!` : "이번 기록은 TOP 5 밖이에요.";
        container.appendChild(note);
      }
    }

    function startEndingEffects() {
      clearEndingEffects();
      const fireworkTimer = setInterval(createFirework, 420);
      const debrisTimer = setInterval(createDebris, 160);
      gameState._endingTimers.push(fireworkTimer, debrisTimer);
      for (let i = 0; i < 8; i++) setTimeout(createFirework, i * 100);
    }

    function clearEndingEffects() {
      gameState._endingTimers.forEach(clearInterval);
      gameState._endingTimers = [];
      document.querySelectorAll(".firework,.debris-particle").forEach((el) => el.remove());
    }

    function createFirework() {
      const container = document.getElementById("ending-screen");
      if (container.style.display === "none") return;
      const colors = ["#ffd700", "#ff6b6b", "#4ecdc4", "#fff", "#cd2e3a", "#0047a0", "#ffa502"];
      const cx = Math.random() * window.innerWidth;
      const cy = Math.random() * window.innerHeight * 0.68;
      for (let i = 0; i < 12; i++) {
        const spark = document.createElement("div");
        spark.className = "firework";
        spark.style.left = cx + "px";
        spark.style.top = cy + "px";
        spark.style.background = colors[Math.floor(Math.random() * colors.length)];
        const angle = (Math.PI * 2 / 12) * i;
        const dist = 44 + Math.random() * 64;
        spark.style.setProperty("--tx", Math.cos(angle) * dist + "px");
        spark.style.setProperty("--ty", Math.sin(angle) * dist + "px");
        container.appendChild(spark);
        setTimeout(() => spark.remove(), 1800);
      }
    }

    function createDebris() {
      const container = document.getElementById("ending-screen");
      if (container.style.display === "none") return;
      const piece = document.createElement("div");
      piece.className = "debris-particle";
      piece.style.left = Math.random() * 100 + "%";
      piece.style.top = "-10px";
      piece.style.background = ["#666", "#888", "#aaa", "#555"][Math.floor(Math.random() * 4)];
      piece.style.width = 3 + Math.random() * 5 + "px";
      piece.style.height = 3 + Math.random() * 5 + "px";
      container.appendChild(piece);
      setTimeout(() => piece.remove(), 3100);
    }

    function replay() {
      clearEndingEffects();
      document.getElementById("ending-screen").style.display = "none";
      const start = document.getElementById("start-screen");
      start.style.opacity = "1";
      start.style.display = "flex";
      ctx.clearRect(0, 0, vw(), vh());
    }

    document.addEventListener("touchmove", (event) => {
      if (event.target.closest("#start-screen") || event.target.closest("#ending-screen") || event.target.closest("#clear-overlay")) return;
      event.preventDefault();
    }, { passive: false });
    document.addEventListener("touchend", (event) => {
      const target = event.target;
      if (target.tagName === "BUTTON" || target.tagName === "INPUT" || target.closest(".weapon-slot") || target.closest("#ending-screen") || target.closest("#clear-overlay") || target.closest("#start-screen")) return;
      event.preventDefault();
    }, { passive: false });

    function handleCanvasPoint(clientX, clientY) {
      const rect = canvas.getBoundingClientRect();
      attack(clientX - rect.left, clientY - rect.top);
    }

    if (window.PointerEvent) {
      canvas.addEventListener("pointerdown", (event) => {
        if (!gameState.isPlaying) return;
        if (event.pointerType === "mouse" && event.button !== 0) return;
        event.preventDefault();
        handleCanvasPoint(event.clientX, event.clientY);
      }, { passive: false });
    } else {
      canvas.addEventListener("click", (event) => {
        handleCanvasPoint(event.clientX, event.clientY);
      });

      canvas.addEventListener("touchstart", (event) => {
        if (!gameState.isPlaying) return;
        event.preventDefault();
        const touch = event.touches[0];
        handleCanvasPoint(touch.clientX, touch.clientY);
      }, { passive: false });
    }

    document.addEventListener("keydown", (event) => {
      const value = Number(event.key);
      if (value >= 1 && value <= WEAPONS.length) selectWeapon(value - 1);
    });

    document.getElementById("start-btn").addEventListener("click", startGame);
    document.getElementById("save-score-btn").addEventListener("click", submitLeaderboard);
    document.getElementById("ending-replay-btn").addEventListener("click", replay);
  </script>
</body>
</html>
