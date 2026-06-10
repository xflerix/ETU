
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>English Trainer Ultimate</title>
<style>
:root {
  --bg: #0f172a;
  --card: #1e293b;
  --text: #f8fafc;
  --accent: #3b82f6;
  --ok: #10b981;
  --err: #ef4444;
  --muted: #334155;
  --r: 20px;
  --t: all 0.2s cubic-bezier(0.4,0,0.2,1);
}
body.light {
  --bg: #f1f5f9; --card: #ffffff; --text: #0f172a;
  --accent: #2563eb; --ok: #059669; --err: #dc2626; --muted: #e2e8f0;
}
*{box-sizing:border-box;margin:0;padding:0;}
body {
  font-family:'Segoe UI',Roboto,sans-serif;
  background:var(--bg); color:var(--text);
  text-align:center; padding:10px;
  min-height:100vh; display:flex; flex-direction:column;
  justify-content:center; align-items:center;
  transition:var(--t); overflow-x:hidden;
}
.container{max-width:600px;width:100%;margin:0 auto;}

/* HEADER */
header{display:flex;justify-content:space-between;align-items:center;margin-bottom:15px;padding:0 5px;width:100%;}
h1{font-size:24px;font-weight:800;background:linear-gradient(135deg,#3b82f6,#8b5cf6);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.header-actions{display:flex;gap:8px;}
.icon-btn{background:var(--card);color:var(--text);border:1px solid var(--muted);padding:10px;border-radius:50%;cursor:pointer;font-size:18px;line-height:1;box-shadow:0 4px 6px -1px rgba(0,0,0,.1);transition:var(--t);}
.icon-btn:hover{transform:scale(1.1);}

/* CARD */
.card{background:var(--card);border-radius:var(--r);padding:24px;margin-bottom:20px;box-shadow:0 10px 25px -5px rgba(0,0,0,.15);transition:var(--t);position:relative;overflow:hidden;width:100%;}
.screen{display:none;animation:fadeIn .3s ease;}
.screen.active{display:block;}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

/* ── ГЛАВНОЕ МЕНЮ ── */
.menu-logo{font-size:64px;margin-bottom:10px;animation:float 3s ease-in-out infinite;display:block;}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-10px)}}
.menu-title{font-size:28px;font-weight:900;margin-bottom:5px;}
.user-badge{display:inline-block;background:linear-gradient(135deg,#f59e0b,#d97706);color:#fff;padding:4px 14px;border-radius:20px;font-size:12px;font-weight:800;text-transform:uppercase;letter-spacing:.5px;margin-bottom:20px;}
.menu-stats{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-bottom:20px;}
.menu-stat-item{background:var(--muted);padding:15px;border-radius:15px;font-size:14px;font-weight:600;}
.menu-stat-item strong{display:block;font-size:22px;color:var(--accent);margin-top:5px;}
.daily-goal{background:rgba(59,130,246,.1);border:1px dashed var(--accent);border-radius:15px;padding:15px;margin-bottom:20px;}
.daily-goal-title{font-size:14px;font-weight:700;margin-bottom:5px;display:flex;justify-content:space-between;}
.menu-btns{display:flex;flex-direction:column;gap:10px;}

/* ── ВЫБОР РЕЖИМА (стартовый экран) ── */
#modeSelectScreen .mode-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-top:18px;}
.mode-card{background:var(--muted);border-radius:16px;padding:22px 14px;cursor:pointer;transition:var(--t);border:2px solid transparent;font-weight:700;font-size:15px;}
.mode-card:hover{border-color:var(--accent);transform:translateY(-3px);}
.mode-card .mc-icon{font-size:36px;display:block;margin-bottom:8px;}
.mode-card .mc-desc{font-size:12px;opacity:.7;font-weight:500;margin-top:4px;}

/* ── СПРИНТ: ВЫБОР ДЛИНЫ ── */
.sprint-opts{display:flex;gap:10px;justify-content:center;margin:16px 0;}
.sprint-opt-btn{background:var(--muted);color:var(--text);border:2px solid transparent;padding:14px 20px;border-radius:14px;font-size:18px;font-weight:800;cursor:pointer;transition:var(--t);}
.sprint-opt-btn:hover,.sprint-opt-btn.active{background:var(--accent);color:#fff;}

/* ── ИГРОВОЙ ЭКРАН ── */
.game-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.pause-btn{background:var(--muted);color:var(--text);border:none;border-radius:10px;padding:8px 14px;font-weight:700;font-size:14px;cursor:pointer;transition:var(--t);}
.pause-btn:hover{background:var(--accent);color:#fff;}
.stats-bar{display:flex;gap:8px;flex:1;justify-content:flex-end;}
.mini-stat{background:var(--muted);padding:6px 12px;border-radius:10px;font-size:12px;font-weight:700;}
.mini-stat span{color:var(--accent);}

/* прогресс-бар */
.prog-wrap{background:var(--muted);height:8px;border-radius:4px;overflow:hidden;margin-bottom:14px;}
.prog-bar{height:100%;width:0%;background:linear-gradient(90deg,#3b82f6,#8b5cf6);border-radius:4px;transition:width .3s cubic-bezier(.4,0,.2,1);}

/* Спринт: счётчик слов */
.sprint-counter{font-size:13px;font-weight:700;opacity:.7;margin-bottom:6px;text-align:right;}

/* режим ввода */
.g-mode-selector{display:flex;gap:8px;margin-bottom:12px;}
.g-mode-btn{flex:1;padding:10px;font-size:13px;border-radius:12px;background:var(--muted);color:var(--text);cursor:pointer;font-weight:700;transition:var(--t);border:2px solid transparent;}
.g-mode-btn.active{background:var(--accent);color:#fff;}

/* таймер */
.timer-box{font-size:18px;font-weight:bold;color:#f59e0b;margin-bottom:8px;display:flex;align-items:center;justify-content:center;gap:5px;}

/* слово */
.word-display{font-size:32px;font-weight:800;margin:12px 0;min-height:48px;letter-spacing:-.5px;display:flex;align-items:center;justify-content:center;gap:10px;}
.speaker-btn{background:none;border:none;cursor:pointer;font-size:24px;padding:5px;border-radius:50%;transition:var(--t);}
.speaker-btn:hover{background:var(--muted);transform:scale(1.1);}

/* ввод */
.input-wrapper{position:relative;width:100%;margin-bottom:16px;}
input[type=text]{width:100%;padding:16px;font-size:18px;font-weight:600;border-radius:12px;border:2px solid var(--muted);background:var(--bg);color:var(--text);outline:none;text-align:center;transition:var(--t);}
input[type=text]:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(59,130,246,.15);}

/* варианты */
.options-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:16px;}
.option-btn{background:var(--muted);color:var(--text);border:2px solid transparent;padding:16px 12px;font-size:16px;font-weight:600;border-radius:12px;cursor:pointer;transition:var(--t);}
.option-btn:hover{background:var(--accent);color:#fff;transform:translateY(-2px);}
.option-btn.correct-choice{background:var(--ok)!important;color:#fff!important;}
.option-btn.incorrect-choice{background:var(--err)!important;color:#fff!important;}

/* кнопки действий */
.actions{display:flex;gap:10px;width:100%;}
button{padding:14px;font-size:16px;font-weight:700;border:none;border-radius:12px;cursor:pointer;transition:var(--t);display:flex;align-items:center;justify-content:center;gap:8px;}
.btn-primary{flex:2;background:var(--accent);color:#fff;min-width:120px;box-shadow:0 4px 6px -1px rgba(59,130,246,.2);}
.btn-primary:hover{filter:brightness(1.1);}
.btn-secondary{flex:1;background:var(--muted);color:var(--text);}
.btn-secondary:hover{filter:brightness(1.1);}
.btn-hint{background:#f59e0b;color:#fff;}
.btn-hint:hover{background:#d97706;}
.btn-danger{background:var(--err);color:#fff;}
@media(max-width:480px){.actions{flex-direction:column;gap:8px;}.actions button{width:100%;flex:none;padding:16px;}}

/* результат */
.result{font-size:18px;font-weight:700;min-height:28px;margin-top:12px;transition:var(--t);}
.ok-text{color:var(--ok);} .err-text{color:var(--err);}

/* анимации */
.shake{animation:shake .5s ease;}
@keyframes shake{0%,100%{transform:translateX(0)}20%,60%{transform:translateX(-8px)}40%,80%{transform:translateX(8px)}}
.flash-ok{animation:flash-ok .35s ease;}
@keyframes flash-ok{0%{box-shadow:0 0 0 0 rgba(16,185,129,.5)}50%{box-shadow:0 0 20px 10px rgba(16,185,129,.3)}100%{box-shadow:0 0 0 0 rgba(16,185,129,0)}}

/* XP pop */
.xp-pop{position:fixed;pointer-events:none;font-size:20px;font-weight:900;color:#10b981;text-shadow:0 2px 8px rgba(0,0,0,.4);animation:xp-fly .9s ease forwards;z-index:9999;}
@keyframes xp-fly{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-70px) scale(1.4)}}

/* ── ПАУЗА / ОВЕРЛЕЙ ── */
.overlay{display:none;position:fixed;inset:0;background:rgba(15,23,42,.88);backdrop-filter:blur(8px);z-index:100;justify-content:center;align-items:center;animation:fadeIn .3s ease;}
.overlay.active{display:flex;}
.pause-card{background:var(--card);border-radius:var(--r);padding:30px;max-width:420px;width:90%;box-shadow:0 20px 25px -5px rgba(0,0,0,.3);border:1px solid var(--muted);max-height:90vh;overflow-y:auto;}
.pause-title{font-size:24px;font-weight:800;margin-bottom:18px;}
.pause-menu-actions{display:flex;flex-direction:column;gap:10px;margin-top:16px;}
.pause-menu-actions button{width:100%;}
select{width:100%;padding:12px;border:1px solid var(--muted);border-radius:12px;background:var(--card);color:var(--text);font-size:13px;font-weight:600;outline:none;cursor:pointer;transition:var(--t);}
select:focus{border-color:var(--accent);}
.settings-row{display:flex;flex-direction:column;gap:6px;text-align:left;margin-bottom:12px;}
.settings-row label{font-size:13px;font-weight:700;opacity:.8;}

/* ── ЭКРАН РЕЗУЛЬТАТОВ СПРИНТА ── */
#resultsScreen .result-hero{font-size:72px;margin-bottom:10px;}
#resultsScreen .result-title{font-size:26px;font-weight:900;margin-bottom:20px;}
.result-stats{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-bottom:24px;}
.result-stat{background:var(--muted);padding:16px;border-radius:14px;font-size:13px;font-weight:600;}
.result-stat strong{display:block;font-size:24px;color:var(--accent);margin-top:4px;}
.result-verdict{background:rgba(59,130,246,.1);border:1px solid var(--accent);border-radius:14px;padding:14px;margin-bottom:20px;font-size:15px;font-weight:700;line-height:1.5;}

/* ── МОИ СЛОВА ── */
#myWordsScreen .mw-list{max-height:220px;overflow-y:auto;margin-bottom:14px;}
.mw-row{background:var(--muted);border-radius:10px;padding:10px 12px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center;font-size:14px;font-weight:600;}
.mw-add-row{display:flex;gap:8px;margin-bottom:10px;}
.mw-add-row input{flex:1;padding:10px;font-size:14px;}
.mw-add-btn{background:var(--accent);color:#fff;border:none;border-radius:10px;padding:10px 14px;font-weight:700;cursor:pointer;font-size:14px;white-space:nowrap;}
.mw-del-btn{background:transparent;border:1px solid var(--err);color:var(--err);border-radius:7px;padding:3px 8px;font-size:11px;cursor:pointer;font-weight:700;}
.mw-del-btn:hover{background:var(--err);color:#fff;}

/* ── ТАБЫ ── */
.tabs{display:flex;border-bottom:2px solid var(--muted);margin-bottom:14px;}
.tab{flex:1;padding:12px;cursor:pointer;font-weight:700;opacity:.6;border-bottom:3px solid transparent;transition:var(--t);}
.tab.active{opacity:1;border-color:var(--accent);}
.tab-content{display:none;max-height:240px;overflow-y:auto;text-align:left;padding-left:4px;}
.tab-content.active{display:block;}
ul{list-style:none;padding:0;}
li{padding:8px 12px;background:var(--muted);border-radius:8px;margin-bottom:8px;font-size:14px;font-weight:600;display:flex;justify-content:space-between;align-items:center;}
.err-ru{color:var(--err);} .ok-en{color:var(--ok);}
.empty-state{text-align:center;opacity:.5;font-style:italic;padding:20px 0;}

/* ── КНОПКИ СНИЗУ ── */
.clean-btn{background:transparent;border:1px solid var(--err);color:var(--err);padding:6px 10px;border-radius:8px;font-size:11px;cursor:pointer;font-weight:bold;}
.clean-btn:hover{background:var(--err);color:#fff;}
.reset-btn{background:transparent;border:1px solid var(--err);color:var(--err);padding:8px 14px;border-radius:10px;cursor:pointer;font-weight:700;font-size:12px;margin-top:14px;}
.reset-btn:hover{background:var(--err);color:#fff;}
</style>
</head>
<body onclick="unlockAudio()">

<div class="container">
  <header>
    <h1>📚 ETU</h1>
    <div class="header-actions">
      <button class="icon-btn" onclick="toggleSound();event.stopPropagation();" id="soundBtn" title="Звук">🔊</button>
      <button class="icon-btn" onclick="toggleTheme();event.stopPropagation();" id="themeBtn" title="Тема">🌙</button>
    </div>
  </header>

  <!-- ═══ ГЛАВНОЕ МЕНЮ ═══ -->
  <div id="menuScreen" class="card screen active">
    <span class="menu-logo">🦁</span>
    <div class="menu-title">English Trainer</div>
    <div class="user-badge" id="userRank">Новичок</div>
    <div class="menu-stats">
      <div class="menu-stat-item">⭐ Опыт (XP)<strong id="menuXp">0</strong></div>
      <div class="menu-stat-item">🏅 Уровень<strong id="menuLevel">1</strong></div>
      <div class="menu-stat-item">🔥 Макс. серия<strong id="menuStreak">0</strong></div>
      <div class="menu-stat-item">🎯 Точность<strong id="menuAccuracy">0%</strong></div>
    </div>
    <div class="daily-goal">
      <div class="daily-goal-title">
        <span>📅 Дневная цель (50 XP):</span>
        <span id="dailyGoalText">0 / 50 XP</span>
      </div>
      <div class="prog-wrap" style="margin-bottom:0;height:6px;">
        <div class="prog-bar" id="dailyGoalBar"></div>
      </div>
    </div>
    <div class="menu-btns">
      <button class="btn-primary" onclick="showModeSelect()" style="font-size:18px;padding:16px;">🚀 Начать тренировку</button>
      <button class="btn-secondary" onclick="showScreen('myWordsScreen')">📝 Мои слова</button>
    </div>
  </div>

  <!-- ═══ ВЫБОР РЕЖИМА ═══ -->
  <div id="modeSelectScreen" class="card screen">
    <div style="font-size:22px;font-weight:900;margin-bottom:6px;">Выбери режим</div>
    <div style="opacity:.6;font-size:14px;margin-bottom:4px;">Как хочешь тренироваться?</div>
    <div class="mode-grid">
      <div class="mode-card" onclick="startEndless()">
        <span class="mc-icon">♾️</span>
        Бесконечный
        <div class="mc-desc">Слова без остановки</div>
      </div>
      <div class="mode-card" onclick="showSprintSelect()">
        <span class="mc-icon">⚡</span>
        Спринт
        <div class="mc-desc">N слов — итог в конце</div>
      </div>
    </div>
    <div id="sprintLenRow" style="display:none;margin-top:18px;">
      <div style="font-size:14px;font-weight:700;margin-bottom:10px;opacity:.8;">Сколько слов?</div>
      <div class="sprint-opts">
        <button class="sprint-opt-btn active" onclick="setSprintLen(10,this)">10</button>
        <button class="sprint-opt-btn" onclick="setSprintLen(20,this)">20</button>
        <button class="sprint-opt-btn" onclick="setSprintLen(30,this)">30</button>
      </div>
      <button class="btn-primary" style="width:100%;margin-top:10px;" onclick="startSprint()">🏁 Поехали!</button>
    </div>
    <button class="btn-secondary" style="width:100%;margin-top:14px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ ТРЕНИРОВКА ═══ -->
  <div id="gameScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="pauseGame()">⏸️ Пауза</button>
      <div class="stats-bar">
        <div class="mini-stat">⭐ <span id="gameXp">0</span> XP</div>
        <div class="mini-stat">🔥 <span id="gameStreak">0</span></div>
        <div class="mini-stat">🎯 <span id="gameAccuracy">0%</span></div>
      </div>
    </div>

    <div class="prog-wrap"><div class="prog-bar" id="xpBar"></div></div>
    <div id="sprintCounterRow" class="sprint-counter" style="display:none;">Слово <span id="sprintCurrent">1</span> из <span id="sprintTotal">10</span></div>
    <div class="prog-wrap" id="sprintProgWrap" style="display:none;margin-bottom:10px;"><div class="prog-bar" id="sprintBar" style="background:linear-gradient(90deg,#10b981,#3b82f6);"></div></div>

    <div class="g-mode-selector">
      <button class="g-mode-btn active" id="modeInputBtn" onclick="setGameMode('write')">⌨️ Ввод</button>
      <button class="g-mode-btn" id="modeTestBtn" onclick="setGameMode('test')">🔘 Тест</button>
    </div>

    <div class="timer-box">⏱️ <span id="timer">15</span></div>

    <div class="word-display">
      <span id="word"></span>
      <button class="speaker-btn" onclick="speakCurrent()" id="speakBtn" style="display:none;">🔊</button>
    </div>

    <div id="inputSection">
      <div class="input-wrapper">
        <input type="text" id="answer" placeholder="Введите ответ…" autocomplete="off">
      </div>
    </div>
    <div id="testSection" style="display:none;">
      <div class="options-grid" id="optionsGrid"></div>
    </div>

    <div class="actions">
      <button class="btn-secondary btn-hint" onclick="getHint()" id="hintBtn">💡 −5XP</button>
      <button class="btn-secondary" onclick="skipWord()" id="skipBtn">⏭️ Пропуск</button>
      <button class="btn-primary" onclick="checkAnswer()" id="checkBtn">✅ Проверить</button>
    </div>
    <div class="result" id="result"></div>
  </div>

  <!-- ═══ РЕЗУЛЬТАТЫ СПРИНТА ═══ -->
  <div id="resultsScreen" class="card screen">
    <div class="result-hero" id="resultsEmoji">🏆</div>
    <div class="result-title" id="resultsTitle">Спринт завершён!</div>
    <div class="result-stats">
      <div class="result-stat">✅ Правильно<strong id="resCorrect">0</strong></div>
      <div class="result-stat">❌ Ошибки<strong id="resErrors">0</strong></div>
      <div class="result-stat">⭐ XP заработано<strong id="resXp">0</strong></div>
      <div class="result-stat">🔥 Макс. серия<strong id="resStreak">0</strong></div>
    </div>
    <div class="result-verdict" id="resultsVerdict"></div>
    <div style="display:flex;flex-direction:column;gap:10px;">
      <button class="btn-primary" onclick="startSprint()">🔄 Ещё раз</button>
      <button class="btn-secondary" onclick="showScreen('menuScreen');updateMenu();">🏠 В меню</button>
    </div>
  </div>

  <!-- ═══ МОИ СЛОВА ═══ -->
  <div id="myWordsScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:14px;">📝 Мои слова</div>
    <div style="font-size:13px;opacity:.7;margin-bottom:14px;text-align:left;">Добавь свои пары слов. Они появятся в категории «Мои слова».</div>
    <div class="mw-add-row">
      <input type="text" id="mwRu" placeholder="Русский" style="flex:1;">
      <input type="text" id="mwEn" placeholder="English" style="flex:1;">
      <button class="mw-add-btn" onclick="addMyWord()">+ Добавить</button>
    </div>
    <div class="mw-list" id="mwList"></div>
    <button class="btn-secondary" style="width:100%;margin-top:6px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ ДОСТИЖЕНИЯ И ОШИБКИ ═══ -->
  <div class="card" id="dashboardCard">
    <div class="tabs">
      <div class="tab active" onclick="switchTab('tab-ach')" id="achBtn">🏆 Достижения</div>
      <div class="tab" onclick="switchTab('tab-mis')" id="misBtn">❌ Ошибки</div>
    </div>
    <div class="tab-content active" id="tab-ach"><ul id="achievements"></ul></div>
    <div class="tab-content" id="tab-mis">
      <ul id="mistakes"></ul>
      <button class="clean-btn" onclick="clearMistakes()">Очистить ошибки</button>
    </div>
  </div>

  <div style="text-align:center;">
    <button class="reset-btn" onclick="confirmReset()">Сбросить весь прогресс</button>
  </div>
</div>

<!-- ═══ ОВЕРЛЕЙ ПАУЗЫ ═══ -->
<div class="overlay" id="pauseOverlay">
  <div class="pause-card">
    <div class="pause-title">⏸️ Пауза</div>
    <div class="settings-row">
      <label>Направление перевода:</label>
      <select id="mode" onchange="savePrefs();">
        <option value="ru-en">🇷🇺 → 🇬🇧 Русский → Английский</option>
        <option value="en-ru">🇬🇧 → 🇷🇺 Английский → Русский</option>
        <option value="mixed">🔀 Смешанный</option>
      </select>
    </div>
    <div class="settings-row">
      <label>Категория:</label>
      <select id="category" onchange="savePrefs();">
        <option value="all">🌍 Все категории</option>
        <option value="countries">🌎 Страны</option>
        <option value="family">👨‍👩‍👧 Семья</option>
        <option value="animals">🐶 Животные</option>
        <option value="things">📱 Вещи</option>
        <option value="personal">🎒 Личные вещи</option>
        <option value="colors">🎨 Цвета</option>
        <option value="numbers">🔢 Числа</option>
        <option value="food">🍕 Еда</option>
        <option value="professions">👨‍⚕️ Профессии</option>
        <option value="verbs">💪 Глаголы</option>
        <option value="myWords">⭐ Мои слова</option>
      </select>
    </div>
    <div class="pause-menu-actions">
      <button class="btn-primary" onclick="resumeGame()">▶️ Продолжить</button>
      <button class="btn-secondary" onclick="goToMainMenu()">🏠 В главное меню</button>
    </div>
  </div>
</div>

<script>
// ═══════════════════════════════════════════
//  ДАННЫЕ
// ═══════════════════════════════════════════
const categories = {
  countries:[
    {ru:"Аргентина",en:"Argentina"},{ru:"Австралия",en:"Australia"},{ru:"Австрия",en:"Austria"},
    {ru:"Бразилия",en:"Brazil"},{ru:"Канада",en:"Canada"},{ru:"Китай",en:"China"},
    {ru:"Чехия",en:"Czech Republic"},{ru:"Египет",en:"Egypt"},{ru:"Франция",en:"France"},
    {ru:"Германия",en:"Germany"},{ru:"Греция",en:"Greece"},{ru:"Индия",en:"India"},
    {ru:"Япония",en:"Japan"},{ru:"Мексика",en:"Mexico"},{ru:"Нидерланды",en:"Netherlands"},
    {ru:"Польша",en:"Poland"},{ru:"Португалия",en:"Portugal"},{ru:"Россия",en:"Russia"},
    {ru:"Испания",en:"Spain"},{ru:"Швейцария",en:"Switzerland"},{ru:"Таиланд",en:"Thailand"},
    {ru:"Турция",en:"Turkey"},{ru:"Великобритания",en:"United Kingdom"},{ru:"США",en:"United States"},
    {ru:"Южная Корея",en:"South Korea"},{ru:"ОАЭ",en:"United Arab Emirates"},
    {ru:"Сингапур",en:"Singapore"},{ru:"Монголия",en:"Mongolia"},{ru:"Норвегия",en:"Norway"},
    {ru:"Швеция",en:"Sweden"},{ru:"Финляндия",en:"Finland"},{ru:"Дания",en:"Denmark"},
    {ru:"Бельгия",en:"Belgium"},{ru:"Венгрия",en:"Hungary"},{ru:"Румыния",en:"Romania"}
  ],
  family:[
    {ru:"тётя",en:"aunt"},{ru:"брат",en:"brother"},{ru:"зять / шурин",en:"brother-in-law"},
    {ru:"дети",en:"children"},{ru:"двоюродный брат / сестра",en:"cousin"},{ru:"дочь",en:"daughter"},
    {ru:"отец / папа",en:"father / dad"},{ru:"дедушка",en:"grandfather"},{ru:"бабушка",en:"grandmother"},
    {ru:"внук",en:"grandson"},{ru:"внучка",en:"granddaughter"},{ru:"муж",en:"husband"},
    {ru:"мать / мама",en:"mother / mom"},{ru:"племянник",en:"nephew"},{ru:"племянница",en:"niece"},
    {ru:"родители",en:"parents"},{ru:"сестра",en:"sister"},{ru:"сын",en:"son"},
    {ru:"дядя",en:"uncle"},{ru:"жена",en:"wife"},{ru:"сводный брат",en:"stepbrother"},
    {ru:"мачеха",en:"stepmother"},{ru:"сводная сестра",en:"stepsister"}
  ],
  animals:[
    {ru:"кошка",en:"cat"},{ru:"курица",en:"chicken"},{ru:"корова",en:"cow"},
    {ru:"собака",en:"dog"},{ru:"осёл",en:"donkey"},{ru:"рыба",en:"fish"},
    {ru:"морская свинка",en:"guinea pig"},{ru:"хомяк",en:"hamster"},{ru:"лошадь",en:"horse"},
    {ru:"попугай",en:"parrot"},{ru:"свинья",en:"pig"},{ru:"кролик",en:"rabbit"},
    {ru:"овца",en:"sheep"},{ru:"змея",en:"snake"},{ru:"черепаха",en:"tortoise"},
    {ru:"лев",en:"lion"},{ru:"тигр",en:"tiger"},{ru:"слон",en:"elephant"},
    {ru:"медведь",en:"bear"},{ru:"волк",en:"wolf"},{ru:"лиса",en:"fox"},
    {ru:"обезьяна",en:"monkey"},{ru:"жираф",en:"giraffe"},{ru:"зебра",en:"zebra"},
    {ru:"крокодил",en:"crocodile"},{ru:"дельфин",en:"dolphin"},{ru:"кит",en:"whale"}
  ],
  things:[
    {ru:"яблоко",en:"apple"},{ru:"бутылка воды",en:"bottle of water"},{ru:"фотоаппарат",en:"camera"},
    {ru:"мобильный телефон",en:"cell phone / mobile phone"},{ru:"монеты",en:"coins"},
    {ru:"наушники",en:"earphones"},{ru:"ключи",en:"keys"},{ru:"ноутбук",en:"laptop"},
    {ru:"блокнот",en:"notebook"},{ru:"ручка",en:"pen"},{ru:"карандаш",en:"pencil"},
    {ru:"сэндвич",en:"sandwich"},{ru:"планшет",en:"tablet"},{ru:"кошелёк",en:"wallet"},
    {ru:"зарядное устройство",en:"charger"},{ru:"наушники",en:"headphones"},
    {ru:"пульт дистанционного управления",en:"remote control"},{ru:"лампа",en:"lamp"}
  ],
  personal:[
    {ru:"книга / роман",en:"book / novel"},{ru:"словарь",en:"dictionary"},
    {ru:"очки",en:"glasses"},{ru:"расчёска",en:"hairbrush"},{ru:"удостоверение личности",en:"ID card"},
    {ru:"журнал",en:"magazine"},{ru:"карта",en:"map"},{ru:"зеркало",en:"mirror"},
    {ru:"ожерелье",en:"necklace"},{ru:"газета",en:"newspaper"},{ru:"паспорт",en:"passport"},
    {ru:"ежедневник",en:"planner / diary"},{ru:"солнцезащитные очки",en:"sunglasses"},
    {ru:"зубная щётка",en:"toothbrush"},{ru:"зонт",en:"umbrella"},{ru:"часы",en:"watch"}
  ],
  colors:[
    {ru:"красный",en:"red"},{ru:"синий / голубой",en:"blue"},{ru:"зелёный",en:"green"},
    {ru:"жёлтый",en:"yellow"},{ru:"оранжевый",en:"orange"},{ru:"фиолетовый",en:"purple"},
    {ru:"розовый",en:"pink"},{ru:"белый",en:"white"},{ru:"чёрный",en:"black"},
    {ru:"серый",en:"grey / gray"},{ru:"коричневый",en:"brown"},{ru:"бежевый",en:"beige"},
    {ru:"золотой",en:"gold"},{ru:"серебряный",en:"silver"},{ru:"бирюзовый",en:"turquoise"},
    {ru:"малиновый",en:"crimson"},{ru:"лазурный",en:"azure"},{ru:"индиго",en:"indigo"}
  ],
  numbers:[
    {ru:"один",en:"one"},{ru:"два",en:"two"},{ru:"три",en:"three"},{ru:"четыре",en:"four"},
    {ru:"пять",en:"five"},{ru:"шесть",en:"six"},{ru:"семь",en:"seven"},{ru:"восемь",en:"eight"},
    {ru:"девять",en:"nine"},{ru:"десять",en:"ten"},{ru:"одиннадцать",en:"eleven"},
    {ru:"двенадцать",en:"twelve"},{ru:"двадцать",en:"twenty"},{ru:"тридцать",en:"thirty"},
    {ru:"сорок",en:"forty"},{ru:"пятьдесят",en:"fifty"},{ru:"сто",en:"hundred"},
    {ru:"тысяча",en:"thousand"},{ru:"первый",en:"first"},{ru:"второй",en:"second"},
    {ru:"третий",en:"third"},{ru:"последний",en:"last"},{ru:"половина",en:"half"}
  ],
  food:[
    {ru:"хлеб",en:"bread"},{ru:"масло",en:"butter"},{ru:"сыр",en:"cheese"},
    {ru:"яйцо",en:"egg"},{ru:"молоко",en:"milk"},{ru:"мясо",en:"meat"},
    {ru:"курица",en:"chicken"},{ru:"рыба",en:"fish"},{ru:"рис",en:"rice"},
    {ru:"макароны",en:"pasta"},{ru:"суп",en:"soup"},{ru:"салат",en:"salad"},
    {ru:"пицца",en:"pizza"},{ru:"гамбургер",en:"burger"},{ru:"торт",en:"cake"},
    {ru:"печенье",en:"cookie"},{ru:"мороженое",en:"ice cream"},{ru:"сок",en:"juice"},
    {ru:"кофе",en:"coffee"},{ru:"чай",en:"tea"},{ru:"вода",en:"water"},
    {ru:"яблоко",en:"apple"},{ru:"банан",en:"banana"},{ru:"клубника",en:"strawberry"},
    {ru:"апельсин",en:"orange"},{ru:"виноград",en:"grapes"},{ru:"помидор",en:"tomato"},
    {ru:"картофель",en:"potato"},{ru:"морковь",en:"carrot"},{ru:"лук",en:"onion"}
  ],
  professions:[
    {ru:"врач",en:"doctor"},{ru:"учитель",en:"teacher"},{ru:"инженер",en:"engineer"},
    {ru:"программист",en:"programmer"},{ru:"юрист",en:"lawyer"},{ru:"повар",en:"cook / chef"},
    {ru:"водитель",en:"driver"},{ru:"полицейский",en:"police officer"},{ru:"пожарный",en:"firefighter"},
    {ru:"медсестра",en:"nurse"},{ru:"архитектор",en:"architect"},{ru:"дизайнер",en:"designer"},
    {ru:"журналист",en:"journalist"},{ru:"актёр",en:"actor"},{ru:"певец",en:"singer"},
    {ru:"спортсмен",en:"athlete"},{ru:"военный",en:"soldier"},{ru:"фермер",en:"farmer"},
    {ru:"пилот",en:"pilot"},{ru:"стюардесса",en:"flight attendant"},{ru:"официант",en:"waiter"},
    {ru:"продавец",en:"salesperson"},{ru:"менеджер",en:"manager"},{ru:"директор",en:"director"}
  ],
  verbs:[
    {ru:"идти / ходить",en:"go / walk"},{ru:"бежать",en:"run"},{ru:"есть",en:"eat"},
    {ru:"пить",en:"drink"},{ru:"спать",en:"sleep"},{ru:"работать",en:"work"},
    {ru:"учиться",en:"study / learn"},{ru:"читать",en:"read"},{ru:"писать",en:"write"},
    {ru:"слушать",en:"listen"},{ru:"говорить",en:"speak / talk"},{ru:"смотреть",en:"watch / look"},
    {ru:"играть",en:"play"},{ru:"петь",en:"sing"},{ru:"танцевать",en:"dance"},
    {ru:"плавать",en:"swim"},{ru:"готовить",en:"cook"},{ru:"покупать",en:"buy"},
    {ru:"продавать",en:"sell"},{ru:"думать",en:"think"},{ru:"знать",en:"know"},
    {ru:"любить",en:"love / like"},{ru:"хотеть",en:"want"},{ru:"мочь",en:"can"},
    {ru:"помогать",en:"help"},{ru:"открывать",en:"open"},{ru:"закрывать",en:"close"},
    {ru:"давать",en:"give"},{ru:"брать",en:"take"},{ru:"находить",en:"find"}
  ],
  myWords: []
};

// ═══════════════════════════════════════════
//  ПРОГРЕСС
// ═══════════════════════════════════════════
let xp       = parseInt(localStorage.getItem("xp"))       || 0;
let correct  = parseInt(localStorage.getItem("correct"))  || 0;
let total    = parseInt(localStorage.getItem("total"))    || 0;
let maxStreak= parseInt(localStorage.getItem("maxStreak"))|| 0;
let streak   = parseInt(localStorage.getItem("streak"))   || 0;
let soundEnabled = localStorage.getItem("soundEnabled") !== "false";

// Дневной XP
const TODAY = new Date().toDateString();
const savedDay = localStorage.getItem("etu_day");
let dailyXp = savedDay === TODAY ? (parseInt(localStorage.getItem("etu_dailyXp"))||0) : 0;
if (savedDay !== TODAY) {
  localStorage.setItem("etu_day", TODAY);
  localStorage.setItem("etu_dailyXp", 0);
}

// Мои слова
try { categories.myWords = JSON.parse(localStorage.getItem("myWords")) || []; } catch(e) {}

let earnedAchievements = [];
try { earnedAchievements = JSON.parse(localStorage.getItem("achievements")) || []; } catch(e) {}
let mistakes = [];
try { mistakes = JSON.parse(localStorage.getItem("mistakes")) || []; } catch(e) {}

// ═══════════════════════════════════════════
//  СОСТОЯНИЕ ИГРЫ
// ═══════════════════════════════════════════
let timer = 15, interval;
let currentWordObj, correctAnswerString, currentLanguage = 'ru';
let gameMode = localStorage.getItem("gameMode") || "write";
let isPaused = false, isAnswering = false, isGameActive = false;

// Спринт
let isSprint = false, sprintLen = 10, sprintIdx = 0;
let sprintCorrect = 0, sprintErrors = 0, sprintXpEarned = 0, sprintMaxStreak = 0;

// Умный повтор: отслеживаем ошибки для приоритета
let wordWeights = {};
try { wordWeights = JSON.parse(localStorage.getItem("wordWeights")) || {}; } catch(e) {}

// ═══════════════════════════════════════════
//  ДОСТИЖЕНИЯ
// ═══════════════════════════════════════════
const achievementList = {
  "ach_10":      "🎯 Знаток: 10 правильных ответов",
  "ach_50":      "🏆 Магистр: 50 правильных ответов",
  "ach_100":     "👑 Лингвист: 100 правильных ответов",
  "ach_250":     "🧠 Эрудит: 250 правильных ответов",
  "ach_str10":   "🔥 Серия 10",
  "ach_str25":   "⚡ Неудержимый: серия 25",
  "ach_str50":   "🌪️ Ураган: серия 50",
  "ach_lvl5":    "⭐ Уровень 5",
  "ach_lvl10":   "💎 Уровень 10",
  "ach_sprint":  "🏁 Первый спринт завершён",
  "ach_perfect": "✨ Идеальный спринт без ошибок",
  "ach_myword":  "📝 Добавил своё слово",
};

// ═══════════════════════════════════════════
//  АУДИО
// ═══════════════════════════════════════════
let audioCtx = null;
function unlockAudio() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  if (audioCtx.state === 'suspended') audioCtx.resume();
}
function beep(freq, type, dur, vol=0.08) {
  if (!soundEnabled || !audioCtx) return;
  try {
    const o = audioCtx.createOscillator(), g = audioCtx.createGain();
    o.connect(g); g.connect(audioCtx.destination);
    o.frequency.value = freq; o.type = type;
    g.gain.setValueAtTime(vol, audioCtx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + dur);
    o.start(); o.stop(audioCtx.currentTime + dur);
  } catch(e) {}
}
// Правильный ответ — восходящий аккорд
function playCorrect() {
  beep(523, 'sine', .1);
  setTimeout(()=>beep(659, 'sine', .12), 70);
  setTimeout(()=>beep(784, 'sine', .18), 140);
}
// Ошибка — нисходящий двойной сигнал
function playWrong() {
  beep(300, 'sawtooth', .15, .1);
  setTimeout(()=>beep(200, 'sawtooth', .25, .12), 150);
}
// Серия! — бодрый аккорд
function playStreak() {
  beep(659, 'sine', .08); setTimeout(()=>beep(784, 'sine', .08), 60);
  setTimeout(()=>beep(987, 'sine', .08), 120); setTimeout(()=>beep(1047, 'sine', .18), 180);
}
// Достижение — торжественный звук
function playAchievement() {
  [523,659,784,1047].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.25,.12),i*80));
}
// Отсчёт таймера <= 5 — тик
function playTick() { beep(880, 'square', .05, .04); }
// Подсказка
function playHint() { beep(440, 'triangle', .15, .07); }
// Конец спринта
function playSprint() {
  [523,587,659,698,784,880,988,1047].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.2,.1),i*90));
}
// Начало игры
function playStart() {
  beep(440,'sine',.1); setTimeout(()=>beep(550,'sine',.1),100); setTimeout(()=>beep(660,'sine',.2),200);
}
// Нажатие кнопки
function playClick() { beep(600,'square',.05,.04); }

function speak(text) {
  if ('speechSynthesis' in window) {
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.lang = "en-US"; u.rate = 0.95;
    speechSynthesis.speak(u);
  }
}
function speakCurrent() {
  unlockAudio();
  if (currentWordObj && currentLanguage === 'en') speak(currentWordObj.en.split('/')[0].trim());
}

// ═══════════════════════════════════════════
//  УТИЛИТЫ
// ═══════════════════════════════════════════
function norm(str) {
  if (!str) return [];
  return str.toLowerCase().replace(/ё/g,'е').replace(/\([^)]*\)/g,'')
    .split(/[\/,;+]/).map(s=>s.trim()).filter(Boolean);
}
function verify(input, correct) {
  return norm(input).some(t => norm(correct).includes(t));
}
function getPool() {
  const cat = document.getElementById("category").value;
  if (cat === "all") return Object.values(categories).flat();
  return categories[cat] || [];
}

// Умный выбор слова: ошибочные слова выпадают чаще
function pickWord(pool) {
  // Создаём массив с весами
  const weighted = [];
  pool.forEach(w => {
    const key = w.en;
    const weight = (wordWeights[key] || 0) + 1; // минимум 1
    for (let i = 0; i < weight; i++) weighted.push(w);
  });
  let candidate = weighted[Math.floor(Math.random() * weighted.length)];
  // Не повторять то же слово дважды подряд
  let tries = 0;
  while (tries < 20 && pool.length > 1 && currentWordObj && candidate.en === currentWordObj.en) {
    candidate = weighted[Math.floor(Math.random() * weighted.length)];
    tries++;
  }
  return candidate;
}

function toggleBtns(disabled) {
  ['answer','skipBtn','hintBtn','checkBtn'].forEach(id=>{
    const el = document.getElementById(id);
    if (el) el.disabled = disabled;
  });
}

// ═══════════════════════════════════════════
//  СОХРАНЕНИЕ
// ═══════════════════════════════════════════
function save() {
  localStorage.setItem("xp", xp);
  localStorage.setItem("correct", correct);
  localStorage.setItem("total", total);
  localStorage.setItem("streak", streak);
  if (streak > maxStreak) { maxStreak = streak; localStorage.setItem("maxStreak", maxStreak); }
  localStorage.setItem("achievements", JSON.stringify(earnedAchievements));
  localStorage.setItem("mistakes", JSON.stringify(mistakes));
  localStorage.setItem("soundEnabled", soundEnabled);
  localStorage.setItem("wordWeights", JSON.stringify(wordWeights));
  localStorage.setItem("etu_dailyXp", dailyXp);
}
function savePrefs() {
  localStorage.setItem("etu_mode", document.getElementById("mode").value);
  localStorage.setItem("etu_category", document.getElementById("category").value);
}
function loadPrefs() {
  const m = localStorage.getItem("etu_mode"), c = localStorage.getItem("etu_category");
  if (m) document.getElementById("mode").value = m;
  if (c) document.getElementById("category").value = c;
}

// ═══════════════════════════════════════════
//  ЭКРАНЫ
// ═══════════════════════════════════════════
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}
function goToMainMenu() {
  clearInterval(interval);
  isPaused = false; isAnswering = false; isGameActive = false;
  document.getElementById("pauseOverlay").classList.remove("active");
  updateMenu();
  showScreen("menuScreen");
  document.getElementById("dashboardCard").style.display = "block";
}
function showModeSelect() {
  playClick();
  document.getElementById("sprintLenRow").style.display = "none";
  showScreen("modeSelectScreen");
}
function showSprintSelect() {
  playClick();
  document.getElementById("sprintLenRow").style.display = "block";
}

// ═══════════════════════════════════════════
//  МЕНЮ / ДАШБОРД
// ═══════════════════════════════════════════
function getRank(x) {
  if (x<100) return "🌱 Новичок";
  if (x<300) return "💪 Энтузиаст";
  if (x<700) return "🔥 Продвинутый";
  if (x<1500) return "🏆 Мастер слов";
  if (x<3000) return "🧙 Пророк";
  return "⚡ Бог Английского";
}
function updateMenu() {
  document.getElementById("menuXp").textContent = xp;
  document.getElementById("menuLevel").textContent = Math.floor(xp/100)+1;
  document.getElementById("menuStreak").textContent = maxStreak;
  document.getElementById("menuAccuracy").textContent = (total ? Math.round(correct/total*100) : 0)+"%";
  document.getElementById("userRank").textContent = getRank(xp);
  document.getElementById("dailyGoalText").textContent = `${dailyXp} / 50 XP`;
  document.getElementById("dailyGoalBar").style.width = Math.min(dailyXp/50*100,100)+"%";
}

// ═══════════════════════════════════════════
//  ИГРА — ЗАПУСК
// ═══════════════════════════════════════════
function startEndless() {
  playStart();
  isSprint = false;
  isGameActive = true;
  document.getElementById("sprintCounterRow").style.display = "none";
  document.getElementById("sprintProgWrap").style.display = "none";
  document.getElementById("dashboardCard").style.display = "none";
  showScreen("gameScreen");
  nextWord();
}

let sprintLenSelected = 10;
function setSprintLen(n, btn) {
  sprintLenSelected = n;
  document.querySelectorAll('.sprint-opt-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}
function startSprint() {
  playStart();
  isSprint = true;
  sprintLen = sprintLenSelected || 10;
  sprintIdx = 0; sprintCorrect = 0; sprintErrors = 0; sprintXpEarned = 0; sprintMaxStreak = 0;
  isGameActive = true;
  document.getElementById("sprintCounterRow").style.display = "block";
  document.getElementById("sprintProgWrap").style.display = "block";
  document.getElementById("dashboardCard").style.display = "none";
  showScreen("gameScreen");
  nextWord();
}

// ═══════════════════════════════════════════
//  РЕЖИМ ВВОДА
// ═══════════════════════════════════════════
function setGameMode(mode) {
  gameMode = mode;
  localStorage.setItem("gameMode", mode);
  document.getElementById("modeInputBtn").classList.toggle("active", mode==='write');
  document.getElementById("modeTestBtn").classList.toggle("active", mode==='test');
  document.getElementById("inputSection").style.display = mode==='write'?'block':'none';
  document.getElementById("testSection").style.display = mode==='test'?'block':'none';
  document.getElementById("checkBtn").style.display = mode==='write'?'flex':'none';
  if (isGameActive) nextWord();
}

// ═══════════════════════════════════════════
//  СЛОВО
// ═══════════════════════════════════════════
function nextWord() {
  if (isPaused) return;
  if (isSprint && sprintIdx >= sprintLen) { showResults(); return; }

  isAnswering = false;
  toggleBtns(false);

  const pool = getPool();
  if (!pool.length) { alertPop("В категории нет слов!"); return; }

  currentWordObj = pickWord(pool);
  const dir = document.getElementById("mode").value;

  if (dir === "ru-en" || (dir === "mixed" && Math.random() < .5)) {
    document.getElementById("word").textContent = currentWordObj.ru;
    correctAnswerString = currentWordObj.en;
    currentLanguage = 'ru';
    document.getElementById("speakBtn").style.display = 'none';
  } else {
    document.getElementById("word").textContent = currentWordObj.en;
    correctAnswerString = currentWordObj.ru;
    currentLanguage = 'en';
    document.getElementById("speakBtn").style.display = 'inline-block';
  }

  document.getElementById("answer").value = "";
  document.getElementById("result").innerHTML = "";
  document.getElementById("result").className = "result";

  if (isSprint) {
    document.getElementById("sprintCurrent").textContent = sprintIdx + 1;
    document.getElementById("sprintTotal").textContent = sprintLen;
    document.getElementById("sprintBar").style.width = (sprintIdx / sprintLen * 100) + "%";
  }

  if (gameMode === 'test') genOptions();
  if (gameMode === 'write' && window.innerWidth > 768) document.getElementById("answer").focus();

  resetTimer();
}

// ═══════════════════════════════════════════
//  ТЕСТ: ВАРИАНТЫ
// ═══════════════════════════════════════════
function genOptions() {
  const pool = getPool();
  const key = currentLanguage === 'ru' ? 'en' : 'ru';
  const correct = currentWordObj[key];
  let opts = [correct];
  const uniq = [...new Set(pool.map(w=>w[key]))].filter(v=>v!==correct);
  while (opts.length < Math.min(4, uniq.length+1)) {
    const r = uniq[Math.floor(Math.random()*uniq.length)];
    if (!opts.includes(r)) opts.push(r);
  }
  opts.sort(()=>Math.random()-.5);
  const grid = document.getElementById("optionsGrid");
  grid.innerHTML = "";
  opts.forEach(opt => {
    const btn = document.createElement("button");
    btn.className = "option-btn";
    btn.textContent = opt.charAt(0).toUpperCase() + opt.slice(1);
    btn.onclick = () => { unlockAudio(); selectOpt(opt, btn); };
    grid.appendChild(btn);
  });
}
function selectOpt(val, btn) {
  if (isAnswering) return;
  isAnswering = true;
  clearInterval(interval);
  toggleBtns(true);
  const ok = verify(val, correctAnswerString);
  document.querySelectorAll(".option-btn").forEach(b => {
    b.style.pointerEvents = "none";
    if (verify(b.textContent, correctAnswerString)) b.classList.add("correct-choice");
  });
  if (!ok && btn) btn.classList.add("incorrect-choice");
  ok ? onCorrect() : onWrong();
}

// ═══════════════════════════════════════════
//  ПРОВЕРКА
// ═══════════════════════════════════════════
function checkAnswer() {
  unlockAudio();
  if (gameMode !== 'write' || isAnswering) return;
  const val = document.getElementById("answer").value.trim();
  if (!val) return;
  isAnswering = true;
  clearInterval(interval);
  toggleBtns(true);
  verify(val, correctAnswerString) ? onCorrect() : onWrong();
}

function onCorrect() {
  total++; correct++; streak++;
  if (isSprint) { sprintCorrect++; sprintMaxStreak = Math.max(sprintMaxStreak, streak); }

  // Снижаем вес слова (хорошо знаем)
  const key = currentWordObj.en;
  wordWeights[key] = Math.max((wordWeights[key]||0) - 1, 0);

  const mult = streak >= 15 ? 3 : streak >= 5 ? 2 : 1;
  const earned = 10 * mult;
  xp += earned; dailyXp += earned;
  if (isSprint) sprintXpEarned += earned;

  const res = document.getElementById("result");
  res.className = "result ok-text";
  res.innerHTML = `✅ Правильно! +${earned} XP${mult>1?` <b>(×${mult} 🔥)</b>`:''}`;

  // Анимация XP
  spawnXpPop(earned);

  document.getElementById("gameScreen").classList.add("flash-ok");
  setTimeout(()=>document.getElementById("gameScreen").classList.remove("flash-ok"), 350);

  if (mult > 1) playStreak(); else playCorrect();
  if (currentLanguage === 'ru') speak(currentWordObj.en.split('/')[0].trim());

  if (streak > maxStreak) maxStreak = streak;
  update(); save(); checkAchievements();

  if (isSprint) {
    sprintIdx++;
    setTimeout(nextWord, sprintIdx >= sprintLen ? 600 : 250);
  } else {
    setTimeout(nextWord, 250);
  }
}

function onWrong() {
  total++; streak = 0;
  if (isSprint) sprintErrors++;

  // Увеличиваем вес слова (знаем плохо)
  const key = currentWordObj.en;
  wordWeights[key] = (wordWeights[key]||0) + 2;

  const res = document.getElementById("result");
  res.className = "result err-text";
  res.innerHTML = `❌ Ошибка. Верно: <strong>${correctAnswerString}</strong>`;

  document.getElementById("gameScreen").classList.add("shake");
  setTimeout(()=>document.getElementById("gameScreen").classList.remove("shake"), 500);

  playWrong();

  const isEN = currentLanguage === 'en';
  const mstr = isEN ? `${currentWordObj.en} — ${currentWordObj.ru}` : `${currentWordObj.ru} — ${currentWordObj.en}`;
  if (!mistakes.includes(mstr)) { mistakes.push(mstr); renderMistakes(); }

  update(); save(); checkAchievements();

  if (isSprint) {
    sprintIdx++;
    setTimeout(nextWord, sprintIdx >= sprintLen ? 600 : 1200);
  } else {
    setTimeout(nextWord, 1500);
  }
}

// ═══════════════════════════════════════════
//  ПОДСКАЗКА / ПРОПУСК
// ═══════════════════════════════════════════
function getHint() {
  unlockAudio();
  if (isAnswering) return;
  if (xp < 5) { alertPop("Нужно минимум 5 XP для подсказки!"); return; }
  xp -= 5; playHint(); update(); save();
  if (gameMode === 'write') {
    const inp = document.getElementById("answer");
    const clean = norm(correctAnswerString)[0];
    if (clean) { inp.value = clean.substring(0, Math.max(inp.value.length+1, 1)); inp.focus(); }
  } else {
    let removed = 0;
    document.querySelectorAll(".option-btn").forEach(btn => {
      if (removed < 1 && !verify(btn.textContent, correctAnswerString) && btn.style.opacity !== "0.3") {
        btn.style.opacity = "0.3"; btn.style.pointerEvents = "none"; removed++;
      }
    });
  }
}
function skipWord() {
  if (isAnswering) return;
  playClick();
  if (isSprint) { sprintIdx++; }
  nextWord();
}

// ═══════════════════════════════════════════
//  ТАЙМЕР
// ═══════════════════════════════════════════
function resetTimer() {
  clearInterval(interval);
  timer = 15;
  const el = document.getElementById("timer");
  el.textContent = timer; el.style.color = "#f59e0b";
  interval = setInterval(() => {
    if (isPaused || isAnswering) return;
    timer--;
    el.textContent = timer;
    if (timer <= 5) { el.style.color = "#ef4444"; playTick(); }
    if (timer <= 0) {
      clearInterval(interval);
      streak = 0; update(); save();
      if (isSprint) sprintIdx++;
      nextWord();
    }
  }, 1000);
}

// ═══════════════════════════════════════════
//  ПАУЗА
// ═══════════════════════════════════════════
function pauseGame() {
  clearInterval(interval); isPaused = true;
  document.getElementById("pauseOverlay").classList.add("active");
}
function resumeGame() {
  isPaused = false; savePrefs();
  document.getElementById("pauseOverlay").classList.remove("active");
  // Обновить слово с новыми настройками
  nextWord();
}

// ═══════════════════════════════════════════
//  КЛАВИША ESCAPE
// ═══════════════════════════════════════════
document.addEventListener("keydown", e => {
  if (e.key === "Escape") {
    if (document.getElementById("pauseOverlay").classList.contains("active")) resumeGame();
    else if (isGameActive) pauseGame();
  }
  if (e.key === "Enter") { unlockAudio(); checkAnswer(); }
});

// ═══════════════════════════════════════════
//  ОБНОВЛЕНИЕ UI
// ═══════════════════════════════════════════
function update() {
  document.getElementById("gameXp").textContent = xp;
  document.getElementById("gameStreak").textContent = streak;
  document.getElementById("gameAccuracy").textContent = (total ? Math.round(correct/total*100):0)+"%";
  document.getElementById("xpBar").style.width = (xp%100)+"%";
  updateMenu();
}

// ═══════════════════════════════════════════
//  РЕЗУЛЬТАТЫ СПРИНТА
// ═══════════════════════════════════════════
function showResults() {
  clearInterval(interval);
  isGameActive = false;
  playSprint();

  document.getElementById("resCorrect").textContent = sprintCorrect;
  document.getElementById("resErrors").textContent = sprintErrors;
  document.getElementById("resXp").textContent = sprintXpEarned;
  document.getElementById("resStreak").textContent = sprintMaxStreak;

  const pct = sprintLen > 0 ? Math.round(sprintCorrect/sprintLen*100) : 0;
  let emoji = "😅", title = "Неплохое начало!", verdict = "";
  if (pct === 100) { emoji="🏆"; title="Идеально! 🎉"; triggerAch("ach_perfect"); }
  else if (pct >= 80) { emoji="🌟"; title="Отличный результат!"; }
  else if (pct >= 60) { emoji="👍"; title="Хорошая работа!"; }
  else if (pct >= 40) { emoji="💪"; title="Продолжай тренироваться!"; }
  else { emoji="😤"; title="Не сдавайся!"; }

  verdict = `Правильных ответов: <b>${sprintCorrect} из ${sprintLen}</b> (${pct}%)<br>
Заработано XP: <b>+${sprintXpEarned}</b> • Лучшая серия: <b>${sprintMaxStreak}</b>`;

  document.getElementById("resultsEmoji").textContent = emoji;
  document.getElementById("resultsTitle").textContent = title;
  document.getElementById("resultsVerdict").innerHTML = verdict;

  triggerAch("ach_sprint");
  showScreen("resultsScreen");
  document.getElementById("dashboardCard").style.display = "none";
}

// ═══════════════════════════════════════════
//  ДОСТИЖЕНИЯ
// ═══════════════════════════════════════════
function checkAchievements() {
  const lvl = Math.floor(xp/100)+1;
  if (correct>=10)  triggerAch("ach_10");
  if (correct>=50)  triggerAch("ach_50");
  if (correct>=100) triggerAch("ach_100");
  if (correct>=250) triggerAch("ach_250");
  if (streak>=10)   triggerAch("ach_str10");
  if (streak>=25)   triggerAch("ach_str25");
  if (streak>=50)   triggerAch("ach_str50");
  if (lvl>=5)       triggerAch("ach_lvl5");
  if (lvl>=10)      triggerAch("ach_lvl10");
  renderAchievements();
}
function triggerAch(id) {
  if (!earnedAchievements.includes(id) && achievementList[id]) {
    earnedAchievements.push(id);
    save();
    playAchievement();
    alertPop(`🎉 Достижение: "${achievementList[id]}"!`);
  }
}
function renderAchievements() {
  const c = document.getElementById("achievements");
  if (!earnedAchievements.length) {
    c.innerHTML = `<li class="empty-state">Нет достижений. Продолжай тренироваться!</li>`; return;
  }
  c.innerHTML = earnedAchievements.map(id=>`<li><span>${achievementList[id]||id}</span> 👑</li>`).join("");
}

// ═══════════════════════════════════════════
//  ОШИБКИ
// ═══════════════════════════════════════════
function renderMistakes() {
  const c = document.getElementById("mistakes");
  if (!mistakes.length) {
    c.innerHTML = `<li class="empty-state">Ошибок нет. Хорошая работа!</li>`; return;
  }
  c.innerHTML = mistakes.map((m,i)=>{
    const [a,b] = m.split(" — ");
    return `<li>
      <div><span class="err-ru">${a||''}</span> ➜ <span class="ok-en">${b||''}</span></div>
      <button class="clean-btn" style="margin:0;padding:4px 8px;font-size:11px;" onclick="studyMistake('${(a||'').replace(/'/g,"\\'")}','${(b||'').replace(/'/g,"\\'")}',${i})">Учить</button>
    </li>`;
  }).join("");
}
function studyMistake(src, tgt, idx) {
  unlockAudio();
  isGameActive = true; isSprint = false;
  showScreen("gameScreen");
  document.getElementById("dashboardCard").style.display = "none";
  document.getElementById("sprintCounterRow").style.display = "none";
  document.getElementById("sprintProgWrap").style.display = "none";
  currentWordObj = { ru: currentLanguage==='ru'?src:tgt, en: currentLanguage==='ru'?tgt:src };
  document.getElementById("word").textContent = src;
  correctAnswerString = tgt;
  document.getElementById("answer").value = "";
  document.getElementById("result").innerHTML = "💪 Режим отработки ошибки!";
  document.getElementById("result").className = "result";
  if (gameMode === 'test') genOptions();
  mistakes.splice(idx, 1); save(); renderMistakes();
  isAnswering = false; toggleBtns(false); resetTimer();
}
function clearMistakes() { mistakes = []; save(); renderMistakes(); }

// ═══════════════════════════════════════════
//  МОИ СЛОВА
// ═══════════════════════════════════════════
function renderMyWords() {
  const list = document.getElementById("mwList");
  if (!categories.myWords.length) {
    list.innerHTML = `<div class="empty-state" style="padding:16px;font-size:14px;">Пока нет слов. Добавь первое!</div>`; return;
  }
  list.innerHTML = categories.myWords.map((w,i)=>
    `<div class="mw-row">
      <span>🇷🇺 <b>${w.ru}</b> → 🇬🇧 <b>${w.en}</b></span>
      <button class="mw-del-btn" onclick="deleteMyWord(${i})">✕</button>
    </div>`
  ).join("");
}
function addMyWord() {
  const ru = document.getElementById("mwRu").value.trim();
  const en = document.getElementById("mwEn").value.trim();
  if (!ru || !en) { alertPop("Введи оба слова!"); return; }
  categories.myWords.push({ru,en});
  localStorage.setItem("myWords", JSON.stringify(categories.myWords));
  document.getElementById("mwRu").value = "";
  document.getElementById("mwEn").value = "";
  playCorrect();
  triggerAch("ach_myword");
  renderMyWords();
}
function deleteMyWord(i) {
  categories.myWords.splice(i,1);
  localStorage.setItem("myWords", JSON.stringify(categories.myWords));
  renderMyWords();
}
// Слушаем Enter в полях добавления слов
document.getElementById("mwEn").addEventListener("keydown", e=>{ if(e.key==="Enter") addMyWord(); });

// ═══════════════════════════════════════════
//  ТАБЫ
// ═══════════════════════════════════════════
function switchTab(id) {
  ['tab-ach','tab-mis'].forEach(t=>document.getElementById(t).classList.remove('active'));
  ['achBtn','misBtn'].forEach(b=>document.getElementById(b).classList.remove('active'));
  document.getElementById(id).classList.add('active');
  document.getElementById(id==='tab-ach'?'achBtn':'misBtn').classList.add('active');
}

// ═══════════════════════════════════════════
//  ТЕМА / ЗВУК
// ═══════════════════════════════════════════
function toggleTheme() {
  document.body.classList.toggle("light");
  const l = document.body.classList.contains("light");
  localStorage.setItem("etu_theme", l?"light":"dark");
  document.getElementById("themeBtn").textContent = l?"☀️":"🌙";
}
function loadTheme() {
  const t = localStorage.getItem("etu_theme");
  if (t==="light") { document.body.classList.add("light"); document.getElementById("themeBtn").textContent="☀️"; }
}
function toggleSound() {
  unlockAudio(); soundEnabled = !soundEnabled;
  localStorage.setItem("soundEnabled", soundEnabled);
  document.getElementById("soundBtn").textContent = soundEnabled?"🔊":"🔇";
  if (soundEnabled) playClick();
}

// ═══════════════════════════════════════════
//  XP ПОП-УП
// ═══════════════════════════════════════════
function spawnXpPop(amount) {
  const el = document.getElementById("gameXp");
  if (!el) return;
  const rect = el.getBoundingClientRect();
  const pop = document.createElement("div");
  pop.className = "xp-pop";
  pop.textContent = `+${amount} XP`;
  pop.style.left = (rect.left + rect.width/2 - 30)+"px";
  pop.style.top  = (rect.top + window.scrollY - 10)+"px";
  document.body.appendChild(pop);
  setTimeout(()=>pop.remove(), 950);
}

// ═══════════════════════════════════════════
//  УВЕДОМЛЕНИЯ
// ═══════════════════════════════════════════
function alertPop(msg) {
  const d = document.createElement("div");
  Object.assign(d.style, {
    position:"fixed",top:"20px",left:"50%",transform:"translateX(-50%)",
    background:"var(--accent)",color:"#fff",padding:"13px 22px",borderRadius:"12px",
    fontWeight:"bold",boxShadow:"0 10px 15px -3px rgba(0,0,0,.3)",zIndex:"99999",
    transition:"all .4s ease",whiteSpace:"nowrap",fontSize:"15px"
  });
  d.textContent = msg;
  document.body.appendChild(d);
  setTimeout(()=>{ d.style.opacity="0"; setTimeout(()=>d.remove(),400); }, 3000);
}

// ═══════════════════════════════════════════
//  СБРОС
// ═══════════════════════════════════════════
function confirmReset() {
  if (!confirm("Сбросить весь прогресс, XP, достижения и ошибки? Это необратимо.")) return;
  localStorage.clear();
  xp=correct=total=maxStreak=streak=dailyXp=0;
  earnedAchievements=[]; mistakes=[]; wordWeights={};
  categories.myWords = [];
  save(); goToMainMenu(); update();
  alertPop("Прогресс сброшен!");
}

// ═══════════════════════════════════════════
//  ИНИЦИАЛИЗАЦИЯ
// ═══════════════════════════════════════════
loadTheme();
document.getElementById("soundBtn").textContent = soundEnabled?"🔊":"🔇";
loadPrefs();
setGameMode(gameMode);
updateMenu();
update();
renderAchievements();
renderMistakes();
renderMyWords();
goToMainMenu();
</script>
</body>
</html>
