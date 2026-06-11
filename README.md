
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>English Trainer Ultimate v3</title>
<style>
/* ════════════════════════════════════
   ПЕРЕМЕННЫЕ И БАЗА
════════════════════════════════════ */
:root {
  --bg: #080c14;
  --bg2: #0d1525;
  --card: rgba(255,255,255,0.06);
  --card-border: rgba(255,255,255,0.10);
  --glass: rgba(255,255,255,0.04);
  --text: #f0f4ff;
  --text2: #8899bb;
  --accent: #4f8eff;
  --accent2: #7c5cfc;
  --ok: #22d88f;
  --err: #ff4f6d;
  --warn: #ffc247;
  --muted: rgba(255,255,255,0.08);
  --muted2: rgba(255,255,255,0.05);
  --r: 20px;
  --r2: 14px;
  --t: all 0.22s cubic-bezier(0.4,0,0.2,1);
  --glow-accent: 0 0 30px rgba(79,142,255,0.25);
  --glow-ok: 0 0 30px rgba(34,216,143,0.3);
  --glow-err: 0 0 30px rgba(255,79,109,0.3);
}
body.light {
  --bg: #f0f4ff;
  --bg2: #e4ebfa;
  --card: rgba(255,255,255,0.85);
  --card-border: rgba(100,130,200,0.2);
  --glass: rgba(255,255,255,0.6);
  --text: #0f1a35;
  --text2: #5566aa;
  --accent: #2563eb;
  --accent2: #6d28d9;
  --ok: #059669;
  --err: #dc2626;
  --warn: #d97706;
  --muted: rgba(0,0,0,0.06);
  --muted2: rgba(0,0,0,0.04);
}

*, *::before, *::after { box-sizing:border-box; margin:0; padding:0; }

body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding: 16px 12px 40px;
  overflow-x: hidden;
  transition: background .4s, color .3s;
}

/* Анимированный фон */
.bg-orbs {
  position: fixed; inset: 0; pointer-events: none; z-index: 0; overflow: hidden;
}
.bg-orb {
  position: absolute; border-radius: 50%; filter: blur(80px); opacity: 0.12;
  animation: orbFloat linear infinite;
}
.bg-orb:nth-child(1){ width:600px;height:600px;background:#4f8eff;top:-200px;left:-100px;animation-duration:20s; }
.bg-orb:nth-child(2){ width:500px;height:500px;background:#7c5cfc;bottom:-150px;right:-100px;animation-duration:26s;animation-direction:reverse; }
.bg-orb:nth-child(3){ width:300px;height:300px;background:#22d88f;top:40%;left:60%;animation-duration:32s; }
@keyframes orbFloat {
  0%,100%{ transform:translate(0,0) scale(1); }
  33%{ transform:translate(40px,-30px) scale(1.05); }
  66%{ transform:translate(-20px,40px) scale(0.97); }
}
body.light .bg-orb { opacity:0.07; }

.container { max-width:620px; width:100%; position:relative; z-index:1; }

/* ════════════════════════════════════
   ШАПКА
════════════════════════════════════ */
header {
  display: flex; justify-content: space-between; align-items: center;
  margin-bottom: 18px; padding: 0 2px;
}
.logo {
  font-size: 22px; font-weight: 900; letter-spacing: -0.5px;
  background: linear-gradient(135deg, #4f8eff, #a78bfa);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  display: flex; align-items: center; gap: 8px;
}
.logo-icon { font-size: 28px; -webkit-text-fill-color: initial; }
.header-actions { display: flex; gap: 8px; }
.icon-btn {
  background: var(--card); color: var(--text);
  border: 1px solid var(--card-border); padding: 9px 11px;
  border-radius: 12px; cursor: pointer; font-size: 16px; line-height: 1;
  backdrop-filter: blur(10px); transition: var(--t);
}
.icon-btn:hover { transform: scale(1.08); border-color: var(--accent); }

/* ════════════════════════════════════
   КАРТОЧКИ / ЭКРАНЫ
════════════════════════════════════ */
.card {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: var(--r); padding: 24px; margin-bottom: 16px;
  backdrop-filter: blur(20px); box-shadow: 0 8px 32px rgba(0,0,0,0.2);
  position: relative; overflow: hidden; width: 100%;
  transition: var(--t);
}
.screen { display: none; animation: screenIn .35s cubic-bezier(.4,0,.2,1); }
.screen.active { display: block; }
@keyframes screenIn {
  from { opacity:0; transform:translateY(14px) scale(0.98); }
  to   { opacity:1; transform:translateY(0) scale(1); }
}

/* ════════════════════════════════════
   ГЛАВНОЕ МЕНЮ
════════════════════════════════════ */
.menu-top {
  display: flex; align-items: center; gap: 16px; margin-bottom: 20px;
}
.avatar {
  width: 64px; height: 64px; border-radius: 18px;
  background: linear-gradient(135deg, #4f8eff, #7c5cfc);
  display: flex; align-items: center; justify-content: center;
  font-size: 30px; flex-shrink: 0; animation: pulse 3s ease-in-out infinite;
}
@keyframes pulse { 0%,100%{box-shadow:0 0 0 0 rgba(79,142,255,.4)} 50%{box-shadow:0 0 0 10px rgba(79,142,255,0)} }
.menu-info { text-align: left; flex: 1; }
.menu-name { font-size: 20px; font-weight: 800; margin-bottom: 4px; }
.rank-badge {
  display: inline-block; padding: 3px 12px; border-radius: 20px;
  font-size: 12px; font-weight: 800; letter-spacing: .5px;
  background: linear-gradient(135deg, #ffc247, #ff8c42); color: #000;
}

/* Уровень и XP */
.level-bar-wrap {
  background: var(--muted); border-radius: 8px; overflow: hidden;
  height: 10px; margin: 10px 0 4px; position: relative;
}
.level-bar {
  height: 100%; border-radius: 8px;
  background: linear-gradient(90deg, #4f8eff, #a78bfa);
  transition: width .5s cubic-bezier(.4,0,.2,1);
  position: relative;
}
.level-bar::after {
  content:''; position:absolute; inset:0; border-radius:8px;
  background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,.3) 50%, transparent 100%);
  background-size: 200% 100%; animation: shimmer 2s infinite;
}
@keyframes shimmer { from{background-position:200% 0} to{background-position:-200% 0} }
.level-txt { font-size: 12px; color: var(--text2); font-weight: 600; margin-bottom: 14px; text-align: right; }

.menu-stats {
  display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 16px;
}
.stat-card {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 12px 14px; text-align: left;
}
.stat-card .s-label { font-size: 11px; color: var(--text2); font-weight: 700; text-transform: uppercase; letter-spacing: .5px; }
.stat-card .s-val { font-size: 24px; font-weight: 900; color: var(--accent); margin-top: 2px; }

/* Дневная цель */
.daily-box {
  background: linear-gradient(135deg, rgba(79,142,255,.1), rgba(124,92,252,.1));
  border: 1px dashed rgba(79,142,255,.4);
  border-radius: var(--r2); padding: 14px 16px; margin-bottom: 16px;
}
.daily-row { display: flex; justify-content: space-between; font-size: 13px; font-weight: 700; margin-bottom: 8px; }
.daily-xp { color: var(--accent); }

/* Цитата дня */
.quote-box {
  background: var(--muted2); border-left: 3px solid var(--accent2);
  border-radius: 0 var(--r2) var(--r2) 0; padding: 12px 14px;
  margin-bottom: 16px; font-style: italic; font-size: 13px; color: var(--text2);
  line-height: 1.5;
}
.quote-box strong { color: var(--text); font-style: normal; }

.menu-btns { display: flex; flex-direction: column; gap: 10px; }

/* ════════════════════════════════════
   ВЫБОР РЕЖИМА
════════════════════════════════════ */
.mode-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 16px; }
.mode-card {
  background: var(--muted2); border: 2px solid var(--card-border);
  border-radius: var(--r2); padding: 20px 14px;
  cursor: pointer; transition: var(--t);
  font-weight: 700; font-size: 15px; text-align: center;
}
.mode-card:hover {
  border-color: var(--accent); transform: translateY(-4px);
  box-shadow: var(--glow-accent);
}
.mode-card .mc-icon { font-size: 36px; display: block; margin-bottom: 8px; }
.mode-card .mc-sub { font-size: 11px; color: var(--text2); font-weight: 500; margin-top: 4px; }
.mode-card.new-badge::after {
  content: 'NEW'; position: absolute; top: 8px; right: 8px;
  background: var(--ok); color: #000; font-size: 9px; font-weight: 900;
  padding: 2px 6px; border-radius: 6px; letter-spacing: .5px;
}
.mode-card { position: relative; }

/* Спринт опции */
.sprint-opts { display: flex; gap: 10px; justify-content: center; margin: 14px 0; }
.sprint-opt-btn {
  background: var(--muted); color: var(--text);
  border: 2px solid transparent; padding: 12px 20px;
  border-radius: 14px; font-size: 18px; font-weight: 800;
  cursor: pointer; transition: var(--t);
}
.sprint-opt-btn:hover, .sprint-opt-btn.active {
  background: var(--accent); color: #fff; border-color: var(--accent);
}

/* ════════════════════════════════════
   ИГРОВОЙ ЭКРАН
════════════════════════════════════ */
.game-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; gap: 8px; }
.pause-btn {
  background: var(--muted); color: var(--text); border: 1px solid var(--card-border);
  border-radius: 10px; padding: 8px 14px; font-weight: 700; font-size: 13px;
  cursor: pointer; transition: var(--t); white-space: nowrap;
}
.pause-btn:hover { background: var(--accent); color: #fff; border-color: var(--accent); }
.stats-bar { display: flex; gap: 6px; flex: 1; justify-content: flex-end; flex-wrap: wrap; }
.mini-stat {
  background: var(--muted); padding: 6px 10px; border-radius: 10px;
  font-size: 12px; font-weight: 700; white-space: nowrap;
}
.mini-stat span { color: var(--accent); }

/* Прогресс */
.prog-wrap { background: var(--muted); height: 7px; border-radius: 4px; overflow: hidden; margin-bottom: 12px; }
.prog-bar {
  height: 100%; width: 0; border-radius: 4px;
  background: linear-gradient(90deg, #4f8eff, #a78bfa);
  transition: width .35s cubic-bezier(.4,0,.2,1);
}
.prog-bar.sprint { background: linear-gradient(90deg, #22d88f, #4f8eff); }

.sprint-counter { font-size: 12px; font-weight: 700; color: var(--text2); margin-bottom: 6px; text-align: right; }

/* Переключатель режима */
.g-mode-selector { display: flex; gap: 6px; margin-bottom: 14px; background: var(--muted2); border-radius: 14px; padding: 4px; }
.g-mode-btn {
  flex: 1; padding: 9px; font-size: 12px; border-radius: 10px;
  background: transparent; color: var(--text2); cursor: pointer;
  font-weight: 700; transition: var(--t); border: none;
}
.g-mode-btn.active { background: var(--accent); color: #fff; }

/* Таймер */
.timer-box {
  font-size: 16px; font-weight: 800; color: var(--warn);
  margin-bottom: 10px; display: flex; align-items: center; justify-content: center; gap: 6px;
}
.timer-ring {
  width: 40px; height: 40px; position: relative; flex-shrink: 0;
}
.timer-ring svg { transform: rotate(-90deg); }
.timer-ring circle {
  fill: none; stroke-width: 3;
  stroke-linecap: round;
  transition: stroke-dashoffset .9s linear;
}
.timer-ring .bg-ring { stroke: var(--muted); stroke-dasharray: 100; stroke-dashoffset: 0; }
.timer-ring .fg-ring {
  stroke: var(--warn); stroke-dasharray: 100; stroke-dashoffset: 0;
  transition: stroke-dashoffset 1s linear, stroke .5s;
}
.timer-num { font-size: 14px; font-weight: 900; position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; }

/* Слово */
.word-display {
  font-size: 34px; font-weight: 900; margin: 14px 0;
  min-height: 52px; letter-spacing: -.5px;
  display: flex; align-items: center; justify-content: center; gap: 10px;
  text-shadow: 0 2px 20px rgba(79,142,255,.15);
}
.speaker-btn {
  background: var(--muted); border: none; cursor: pointer;
  font-size: 20px; padding: 8px; border-radius: 50%; transition: var(--t);
}
.speaker-btn:hover { background: var(--accent); transform: scale(1.1); }

/* Ввод */
.input-wrapper { position: relative; width: 100%; margin-bottom: 14px; }
input[type=text] {
  width: 100%; padding: 15px 18px; font-size: 18px; font-weight: 600;
  border-radius: var(--r2); border: 2px solid var(--card-border);
  background: var(--muted2); color: var(--text); outline: none; text-align: center;
  transition: var(--t); backdrop-filter: blur(10px);
}
input[type=text]:focus {
  border-color: var(--accent);
  box-shadow: 0 0 0 4px rgba(79,142,255,.12);
}
input[type=text].input-correct { border-color: var(--ok); box-shadow: 0 0 0 4px rgba(34,216,143,.12); }
input[type=text].input-wrong { border-color: var(--err); box-shadow: 0 0 0 4px rgba(255,79,109,.12); }

/* Варианты */
.options-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 14px; }
.option-btn {
  background: var(--muted); color: var(--text);
  border: 2px solid var(--card-border); padding: 15px 12px;
  font-size: 15px; font-weight: 600; border-radius: var(--r2);
  cursor: pointer; transition: var(--t); line-height: 1.3;
}
.option-btn:hover { border-color: var(--accent); background: rgba(79,142,255,.12); transform: translateY(-2px); }
.option-btn.correct-choice { background: rgba(34,216,143,.2) !important; border-color: var(--ok) !important; color: var(--ok) !important; }
.option-btn.incorrect-choice { background: rgba(255,79,109,.2) !important; border-color: var(--err) !important; color: var(--err) !important; }

/* Кнопки действий */
.actions { display: flex; gap: 8px; width: 100%; }
button { padding: 13px; font-size: 15px; font-weight: 700; border: none; border-radius: var(--r2); cursor: pointer; transition: var(--t); display: flex; align-items: center; justify-content: center; gap: 7px; }
.btn-primary {
  flex: 2; background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; min-width: 110px;
  box-shadow: 0 4px 16px rgba(79,142,255,.25);
}
.btn-primary:hover { filter: brightness(1.12); transform: translateY(-1px); }
.btn-primary:active { transform: translateY(0); }
.btn-secondary { flex: 1; background: var(--muted); color: var(--text); border: 1px solid var(--card-border); }
.btn-secondary:hover { background: var(--muted2); border-color: var(--accent); }
.btn-hint { background: rgba(255,194,71,.15); color: var(--warn); border: 1px solid rgba(255,194,71,.3); }
.btn-hint:hover { background: rgba(255,194,71,.25); }
.btn-danger { background: rgba(255,79,109,.15); color: var(--err); border: 1px solid rgba(255,79,109,.3); }
.btn-danger:hover { background: rgba(255,79,109,.25); }

@media(max-width:480px){
  .actions { flex-direction:column; gap:8px; }
  .actions button { width:100%; flex:none; padding:14px; }
}

/* Результат ответа */
.result {
  font-size: 16px; font-weight: 700; min-height: 26px;
  margin-top: 12px; transition: var(--t); text-align: center;
  line-height: 1.4;
}
.ok-text { color: var(--ok); }
.err-text { color: var(--err); }

/* ════════════════════════════════════
   РЕЖИМ ФЛЭШКАРТЫ
════════════════════════════════════ */
.flashcard-wrap {
  perspective: 1000px; height: 200px; margin: 16px 0; cursor: pointer;
}
.flashcard {
  width: 100%; height: 100%; position: relative;
  transform-style: preserve-3d; transition: transform .5s cubic-bezier(.4,0,.2,1);
}
.flashcard.flipped { transform: rotateY(180deg); }
.fc-face {
  position: absolute; inset: 0;
  backface-visibility: hidden; border-radius: var(--r);
  display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 8px;
  font-weight: 800; font-size: 28px; padding: 20px;
  border: 1px solid var(--card-border);
}
.fc-front {
  background: linear-gradient(135deg, rgba(79,142,255,.15), rgba(124,92,252,.15));
  border-color: rgba(79,142,255,.3);
}
.fc-back {
  background: linear-gradient(135deg, rgba(34,216,143,.1), rgba(79,142,255,.1));
  border-color: rgba(34,216,143,.3);
  transform: rotateY(180deg);
}
.fc-hint { font-size: 12px; font-weight: 600; color: var(--text2); margin-top: 4px; }
.fc-lang { font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--text2); font-weight: 700; }

.flashcard-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 12px; }
.fc-btn-know {
  background: rgba(34,216,143,.15); color: var(--ok);
  border: 1px solid rgba(34,216,143,.3); border-radius: var(--r2);
  padding: 14px; font-weight: 800; cursor: pointer; transition: var(--t);
}
.fc-btn-know:hover { background: rgba(34,216,143,.25); transform: translateY(-2px); }
.fc-btn-unknown {
  background: rgba(255,79,109,.1); color: var(--err);
  border: 1px solid rgba(255,79,109,.25); border-radius: var(--r2);
  padding: 14px; font-weight: 800; cursor: pointer; transition: var(--t);
}
.fc-btn-unknown:hover { background: rgba(255,79,109,.2); transform: translateY(-2px); }
.fc-counter { font-size: 13px; color: var(--text2); font-weight: 700; text-align: center; margin-bottom: 8px; }
.fc-progress-row { display: flex; gap: 8px; margin-bottom: 12px; }
.fc-prog-item { flex: 1; padding: 8px; border-radius: 10px; font-size: 12px; font-weight: 700; text-align: center; }
.fc-prog-know { background: rgba(34,216,143,.1); color: var(--ok); }
.fc-prog-unkn { background: rgba(255,79,109,.1); color: var(--err); }
.fc-prog-left { background: var(--muted2); color: var(--text2); }

/* ════════════════════════════════════
   РЕЖИМ АНАГРАММА
════════════════════════════════════ */
.anagram-q { font-size: 16px; color: var(--text2); font-weight: 700; margin-bottom: 10px; }
.anagram-word { font-size: 22px; font-weight: 900; margin-bottom: 16px; color: var(--text); }
.anagram-answer {
  display: flex; gap: 8px; flex-wrap: wrap; justify-content: center;
  min-height: 52px; padding: 10px; background: var(--muted2);
  border: 2px dashed var(--card-border); border-radius: var(--r2);
  margin-bottom: 12px; cursor: pointer;
}
.anagram-letters {
  display: flex; gap: 8px; flex-wrap: wrap; justify-content: center; margin-bottom: 14px;
}
.letter-tile {
  background: var(--muted); border: 1px solid var(--card-border);
  border-radius: 10px; width: 42px; height: 48px;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; font-weight: 800; cursor: pointer; transition: var(--t);
  user-select: none;
}
.letter-tile:hover { border-color: var(--accent); background: rgba(79,142,255,.12); transform: translateY(-3px); }
.letter-tile.used { opacity: 0.3; pointer-events: none; }
.answer-tile {
  background: linear-gradient(135deg, rgba(79,142,255,.2), rgba(124,92,252,.2));
  border: 1px solid rgba(79,142,255,.4);
  border-radius: 10px; width: 42px; height: 48px;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; font-weight: 800; cursor: pointer; transition: var(--t);
  color: var(--accent);
}
.answer-tile:hover { opacity: 0.7; transform: scale(0.95); }
.anagram-hint-row { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
.anagram-hint-word { font-size: 13px; color: var(--text2); font-weight: 600; letter-spacing: 1px; }

/* ════════════════════════════════════
   РЕЗУЛЬТАТЫ СПРИНТА
════════════════════════════════════ */
.result-hero { font-size: 80px; margin-bottom: 10px; animation: heroIn .5s cubic-bezier(.4,0,.2,1); }
@keyframes heroIn { from{transform:scale(.5) rotate(-10deg);opacity:0} to{transform:scale(1) rotate(0);opacity:1} }
.result-title { font-size: 28px; font-weight: 900; margin-bottom: 8px; }
.result-pct {
  font-size: 56px; font-weight: 900; margin-bottom: 16px;
  background: linear-gradient(135deg, #4f8eff, #a78bfa);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
}
.result-stats { display: grid; grid-template-columns: repeat(2,1fr); gap: 10px; margin-bottom: 20px; }
.result-stat {
  background: var(--muted2); border: 1px solid var(--card-border);
  padding: 14px; border-radius: var(--r2); font-size: 12px; font-weight: 700;
  color: var(--text2); text-transform: uppercase; letter-spacing: .5px;
}
.result-stat strong { display:block; font-size:26px; color:var(--accent); margin-top:3px; letter-spacing:-1px; }
.result-verdict {
  background: linear-gradient(135deg, rgba(79,142,255,.08), rgba(124,92,252,.08));
  border: 1px solid rgba(79,142,255,.2);
  border-radius: var(--r2); padding: 14px 16px;
  margin-bottom: 20px; font-size: 14px; font-weight: 700; line-height: 1.6;
}

/* ════════════════════════════════════
   МОИ СЛОВА
════════════════════════════════════ */
.mw-list { max-height: 260px; overflow-y: auto; margin-bottom: 14px; }
.mw-list::-webkit-scrollbar { width: 4px; }
.mw-list::-webkit-scrollbar-track { background: var(--muted2); border-radius: 2px; }
.mw-list::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }
.mw-row {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: 12px; padding: 10px 14px; margin-bottom: 8px;
  display: flex; justify-content: space-between; align-items: center;
  font-size: 14px; font-weight: 600;
}
.mw-add-row { display: flex; gap: 8px; margin-bottom: 12px; }
.mw-add-row input { flex: 1; padding: 10px 12px; font-size: 14px; }
.mw-add-btn {
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; border: none; border-radius: var(--r2);
  padding: 10px 16px; font-weight: 700; cursor: pointer; font-size: 14px; white-space: nowrap;
}
.mw-del-btn {
  background: transparent; border: 1px solid rgba(255,79,109,.4); color: var(--err);
  border-radius: 8px; padding: 3px 9px; font-size: 11px; cursor: pointer; font-weight: 700;
}
.mw-del-btn:hover { background: rgba(255,79,109,.15); }

/* ════════════════════════════════════
   СТАТИСТИКА
════════════════════════════════════ */
.stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 16px; }
.stats-cell {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px;
}
.stats-cell .sc-icon { font-size: 24px; margin-bottom: 6px; }
.stats-cell .sc-label { font-size: 11px; color: var(--text2); font-weight: 700; text-transform: uppercase; letter-spacing: .5px; }
.stats-cell .sc-val { font-size: 26px; font-weight: 900; color: var(--text); margin-top: 2px; }
.accuracy-visual {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 16px; margin-bottom: 12px;
}
.acc-bar-wrap { background: var(--muted); height: 12px; border-radius: 6px; overflow: hidden; margin-top: 8px; }
.acc-bar-ok { height: 100%; background: linear-gradient(90deg, var(--ok), #4f8eff); border-radius: 6px; transition: width .8s ease; }

/* Ранг-прогресс */
.rank-progress {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 16px; margin-bottom: 12px;
}
.rank-levels { display: flex; justify-content: space-between; flex-wrap: wrap; gap: 8px; }
.rank-item {
  display: flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 700;
  padding: 8px 12px; border-radius: 10px; background: var(--muted);
}
.rank-item.earned { background: rgba(79,142,255,.12); border: 1px solid rgba(79,142,255,.3); }
.rank-item.current { background: rgba(34,216,143,.12); border: 1px solid rgba(34,216,143,.4); color: var(--ok); }

/* ════════════════════════════════════
   ТАБЫ
════════════════════════════════════ */
.tabs { display: flex; gap: 4px; background: var(--muted2); padding: 4px; border-radius: 14px; margin-bottom: 14px; }
.tab {
  flex: 1; padding: 10px; cursor: pointer; font-weight: 700; font-size: 13px;
  border-radius: 10px; transition: var(--t); color: var(--text2); text-align: center;
}
.tab.active { background: var(--card); color: var(--text); box-shadow: 0 2px 8px rgba(0,0,0,.15); }
.tab-content { display: none; max-height: 260px; overflow-y: auto; text-align: left; padding: 2px; }
.tab-content.active { display: block; }
.tab-content::-webkit-scrollbar { width: 4px; }
.tab-content::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }

ul { list-style: none; padding: 0; }
li {
  padding: 10px 14px; background: var(--muted2);
  border: 1px solid var(--card-border); border-radius: 12px;
  margin-bottom: 8px; font-size: 13px; font-weight: 600;
  display: flex; justify-content: space-between; align-items: center;
  transition: var(--t);
}
li:hover { border-color: rgba(79,142,255,.3); }
.err-ru { color: var(--err); }
.ok-en { color: var(--ok); }
.empty-state { text-align: center; color: var(--text2); font-style: italic; padding: 24px 0; font-size: 14px; }
.ach-icon { font-size: 20px; }

/* ════════════════════════════════════
   ПАУЗА / ОВЕРЛЕЙ
════════════════════════════════════ */
.overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(8,12,20,.85); backdrop-filter: blur(12px);
  z-index: 100; justify-content: center; align-items: center;
  animation: fadeIn .3s ease;
}
.overlay.active { display: flex; }
@keyframes fadeIn { from{opacity:0} to{opacity:1} }
.pause-card {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: 24px; padding: 28px; max-width: 440px; width: 92%;
  backdrop-filter: blur(20px); box-shadow: 0 30px 60px rgba(0,0,0,.4);
  max-height: 90vh; overflow-y: auto; animation: screenIn .3s ease;
}
.pause-title { font-size: 22px; font-weight: 900; margin-bottom: 16px; }
.pause-menu-actions { display: flex; flex-direction: column; gap: 10px; margin-top: 14px; }
.pause-menu-actions button { width: 100%; }

select {
  width: 100%; padding: 12px 14px; border: 1px solid var(--card-border);
  border-radius: 12px; background: var(--muted2); color: var(--text);
  font-size: 13px; font-weight: 600; outline: none; cursor: pointer;
  transition: var(--t); backdrop-filter: blur(10px);
}
select:focus { border-color: var(--accent); }
.settings-row { display: flex; flex-direction: column; gap: 6px; text-align: left; margin-bottom: 12px; }
.settings-row label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; }

/* Кнопки управления */
.clean-btn {
  background: transparent; border: 1px solid rgba(255,79,109,.4); color: var(--err);
  padding: 7px 12px; border-radius: 10px; font-size: 11px; cursor: pointer; font-weight: 800;
}
.clean-btn:hover { background: rgba(255,79,109,.12); }
.reset-btn {
  background: transparent; border: 1px solid rgba(255,79,109,.3); color: var(--err);
  padding: 9px 16px; border-radius: 12px; cursor: pointer; font-weight: 700; font-size: 12px;
  margin-top: 14px; display: inline-block;
}
.reset-btn:hover { background: rgba(255,79,109,.12); }

/* ════════════════════════════════════
   АНИМАЦИИ ОТВЕТОВ
════════════════════════════════════ */
.shake { animation: shake .45s ease; }
@keyframes shake { 0%,100%{transform:translateX(0)} 20%,60%{transform:translateX(-8px)} 40%,80%{transform:translateX(8px)} }
.flash-ok { animation: flashOk .35s ease; }
@keyframes flashOk {
  0%  { box-shadow:0 8px 32px rgba(0,0,0,.2); }
  50% { box-shadow:0 0 40px 15px rgba(34,216,143,.25); }
  100%{ box-shadow:0 8px 32px rgba(0,0,0,.2); }
}

/* XP поп */
.xp-pop {
  position: fixed; pointer-events: none; font-size: 20px; font-weight: 900;
  color: var(--ok); text-shadow: 0 2px 10px rgba(0,0,0,.5);
  animation: xpFly .9s ease forwards; z-index: 9999;
}
@keyframes xpFly { 0%{opacity:1;transform:translateY(0) scale(1)} 100%{opacity:0;transform:translateY(-80px) scale(1.5)} }

/* Уведомление */
.notif {
  position: fixed; top: 20px; left: 50%; transform: translateX(-50%);
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; padding: 12px 24px; border-radius: 16px;
  font-weight: 800; font-size: 14px; box-shadow: 0 10px 30px rgba(79,142,255,.3);
  z-index: 99999; white-space: nowrap; animation: notifIn .4s ease;
}
@keyframes notifIn { from{opacity:0;transform:translateX(-50%) translateY(-10px)} to{opacity:1;transform:translateX(-50%) translateY(0)} }

/* Достижение поп */
.ach-pop {
  position: fixed; bottom: 30px; right: 20px;
  background: linear-gradient(135deg, rgba(34,216,143,.15), rgba(79,142,255,.15));
  border: 1px solid rgba(34,216,143,.4);
  backdrop-filter: blur(16px);
  color: var(--text); padding: 14px 18px; border-radius: 16px;
  font-weight: 700; font-size: 13px; box-shadow: 0 10px 30px rgba(0,0,0,.3);
  z-index: 99999; max-width: 280px; animation: achIn .4s ease;
}
@keyframes achIn { from{opacity:0;transform:translateX(20px)} to{opacity:1;transform:translateX(0)} }
.ach-pop .ach-pop-title { color: var(--ok); font-size: 11px; text-transform: uppercase; letter-spacing: .8px; margin-bottom: 4px; }

/* Серия-баннер */
.streak-banner {
  position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%);
  font-size: 48px; font-weight: 900; pointer-events: none; z-index: 9998;
  animation: streakPop .6s ease forwards;
  text-shadow: 0 4px 20px rgba(255,194,71,.5);
}
@keyframes streakPop {
  0%  { opacity:0; transform:translate(-50%,-50%) scale(.5); }
  30% { opacity:1; transform:translate(-50%,-50%) scale(1.1); }
  70% { opacity:1; transform:translate(-50%,-50%) scale(1); }
  100%{ opacity:0; transform:translate(-50%,-60%) scale(.9); }
}

/* Разделитель */
.divider { height: 1px; background: var(--card-border); margin: 16px 0; }
.section-title { font-size: 13px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .8px; margin-bottom: 12px; }

/* Полоска ввода анаграммы */
.anagram-blank {
  display: flex; gap: 4px; justify-content: center; margin-bottom: 12px; flex-wrap: wrap;
}
.blank-slot {
  width: 28px; height: 4px; border-radius: 2px;
  background: var(--card-border);
}
.blank-slot.filled { background: var(--accent); }
</style>
</head>
<body onclick="unlockAudio()">

<!-- Фоновые сферы -->
<div class="bg-orbs">
  <div class="bg-orb"></div>
  <div class="bg-orb"></div>
  <div class="bg-orb"></div>
</div>

<div class="container">
  <!-- ═══ ШАПКА ═══ -->
  <header>
    <div class="logo">
      <span class="logo-icon">📚</span>
      ETU <span style="opacity:.5;font-weight:400;font-size:14px">v3</span>
    </div>
    <div class="header-actions">
      <button class="icon-btn" onclick="showStatsScreen();event.stopPropagation();" title="Статистика">📊</button>
      <button class="icon-btn" onclick="toggleSound();event.stopPropagation();" id="soundBtn" title="Звук">🔊</button>
      <button class="icon-btn" onclick="toggleTheme();event.stopPropagation();" id="themeBtn" title="Тема">🌙</button>
    </div>
  </header>

  <!-- ═══ ГЛАВНОЕ МЕНЮ ═══ -->
  <div id="menuScreen" class="card screen active">
    <div class="menu-top">
      <div class="avatar" id="menuAvatar">🦁</div>
      <div class="menu-info">
        <div class="menu-name">English Trainer</div>
        <div class="rank-badge" id="userRank">Новичок</div>
      </div>
    </div>

    <div style="display:flex;justify-content:space-between;font-size:13px;font-weight:700;margin-bottom:4px;">
      <span style="color:var(--text2)">Уровень <span id="menuLevel" style="color:var(--accent)">1</span></span>
      <span style="color:var(--text2)"><span id="menuXp">0</span> / <span id="nextLvlXp">100</span> XP</span>
    </div>
    <div class="level-bar-wrap"><div class="level-bar" id="levelBar"></div></div>
    <div class="level-txt" id="levelTxt"></div>

    <div class="menu-stats">
      <div class="stat-card">
        <div class="s-label">🔥 Макс. серия</div>
        <div class="s-val" id="menuStreak">0</div>
      </div>
      <div class="stat-card">
        <div class="s-label">🎯 Точность</div>
        <div class="s-val" id="menuAccuracy">—</div>
      </div>
    </div>

    <div class="daily-box">
      <div class="daily-row">
        <span>📅 Дневная цель</span>
        <span class="daily-xp" id="dailyGoalText">0 / 50 XP</span>
      </div>
      <div class="prog-wrap" style="margin-bottom:0;height:8px;">
        <div class="prog-bar" id="dailyGoalBar"></div>
      </div>
    </div>

    <div class="quote-box" id="quoteBox">
      <em id="quoteText">Loading...</em>
    </div>

    <div class="menu-btns">
      <button class="btn-primary" onclick="showModeSelect()" style="font-size:17px;padding:16px;">🚀 Начать тренировку</button>
      <button class="btn-secondary" onclick="showScreen('myWordsScreen')">📝 Мои слова</button>
    </div>
  </div>

  <!-- ═══ ВЫБОР РЕЖИМА ═══ -->
  <div id="modeSelectScreen" class="card screen">
    <div style="font-size:22px;font-weight:900;margin-bottom:4px;">Выбери режим</div>
    <div style="color:var(--text2);font-size:13px;margin-bottom:4px;">Как будем тренироваться?</div>

    <div class="mode-grid">
      <div class="mode-card" onclick="startEndless()">
        <span class="mc-icon">♾️</span>
        Бесконечный
        <div class="mc-sub">Слова без остановки</div>
      </div>
      <div class="mode-card" onclick="showSprintSelect()">
        <span class="mc-icon">⚡</span>
        Спринт
        <div class="mc-sub">N слов — итог в конце</div>
      </div>
      <div class="mode-card new-badge" onclick="startFlashcards()">
        <span class="mc-icon">🃏</span>
        Флэшкарты
        <div class="mc-sub">Переверни и запомни</div>
      </div>
      <div class="mode-card new-badge" onclick="startAnagram()">
        <span class="mc-icon">🧩</span>
        Анаграмма
        <div class="mc-sub">Составь слово из букв</div>
      </div>
    </div>

    <div id="sprintLenRow" style="display:none;margin-top:16px;">
      <div style="font-size:13px;font-weight:800;margin-bottom:10px;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;">Количество слов</div>
      <div class="sprint-opts">
        <button class="sprint-opt-btn active" onclick="setSprintLen(10,this)">10</button>
        <button class="sprint-opt-btn" onclick="setSprintLen(20,this)">20</button>
        <button class="sprint-opt-btn" onclick="setSprintLen(30,this)">30</button>
      </div>
      <button class="btn-primary" style="width:100%;margin-top:12px;" onclick="startSprint()">🏁 Поехали!</button>
    </div>

    <button class="btn-secondary" style="width:100%;margin-top:14px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ ТРЕНИРОВКА ═══ -->
  <div id="gameScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="pauseGame()">⏸ Пауза</button>
      <div class="stats-bar">
        <div class="mini-stat">⭐ <span id="gameXp">0</span></div>
        <div class="mini-stat">🔥 <span id="gameStreak">0</span></div>
        <div class="mini-stat">🎯 <span id="gameAccuracy">—</span></div>
      </div>
    </div>

    <div class="prog-wrap"><div class="prog-bar" id="xpBar"></div></div>
    <div id="sprintCounterRow" class="sprint-counter" style="display:none;">
      Слово <span id="sprintCurrent">1</span> из <span id="sprintTotal">10</span>
    </div>
    <div class="prog-wrap" id="sprintProgWrap" style="display:none;">
      <div class="prog-bar sprint" id="sprintBar"></div>
    </div>

    <div class="g-mode-selector">
      <button class="g-mode-btn active" id="modeInputBtn" onclick="setGameMode('write')">⌨️ Ввод</button>
      <button class="g-mode-btn" id="modeTestBtn" onclick="setGameMode('test')">🔘 Тест</button>
    </div>

    <div class="timer-box">
      <div class="timer-ring">
        <svg viewBox="0 0 36 36" width="40" height="40">
          <circle class="bg-ring" cx="18" cy="18" r="15.9" stroke-dasharray="100" stroke-dashoffset="0"/>
          <circle class="fg-ring" id="timerCircle" cx="18" cy="18" r="15.9"/>
        </svg>
        <div class="timer-num" id="timer">15</div>
      </div>
    </div>

    <div class="word-display">
      <span id="word"></span>
      <button class="speaker-btn" onclick="speakCurrent()" id="speakBtn" style="display:none;">🔊</button>
    </div>

    <div id="inputSection">
      <div class="input-wrapper">
        <input type="text" id="answer" placeholder="Введите ответ…" autocomplete="off" spellcheck="false">
      </div>
    </div>
    <div id="testSection" style="display:none;">
      <div class="options-grid" id="optionsGrid"></div>
    </div>

    <div class="actions">
      <button class="btn-secondary btn-hint" onclick="getHint()" id="hintBtn">💡 −5XP</button>
      <button class="btn-secondary" onclick="skipWord()" id="skipBtn">⏭ Пропуск</button>
      <button class="btn-primary" onclick="checkAnswer()" id="checkBtn">✅ Проверить</button>
    </div>
    <div class="result" id="result"></div>
  </div>

  <!-- ═══ ФЛЭШКАРТЫ ═══ -->
  <div id="flashcardScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="exitFlashcards()">← Выйти</button>
      <div class="stats-bar">
        <div class="mini-stat">✅ <span id="fcKnow">0</span></div>
        <div class="mini-stat">❌ <span id="fcUnknown">0</span></div>
      </div>
    </div>

    <div class="fc-counter" id="fcCounter">Карточка 1 из 20</div>
    <div class="fc-progress-row">
      <div class="fc-prog-item fc-prog-know">✅ Знаю: <span id="fcKnowCount">0</span></div>
      <div class="fc-prog-item fc-prog-unkn">❌ Учить: <span id="fcUnknCount">0</span></div>
      <div class="fc-prog-item fc-prog-left">📋 Осталось: <span id="fcLeftCount">0</span></div>
    </div>

    <div class="prog-wrap">
      <div class="prog-bar sprint" id="fcBar"></div>
    </div>

    <div class="flashcard-wrap" onclick="flipCard()">
      <div class="flashcard" id="flashcard">
        <div class="fc-face fc-front">
          <div class="fc-lang">🇷🇺 Русский</div>
          <div id="fcFrontWord">слово</div>
          <div class="fc-hint">Нажми чтобы перевернуть 👆</div>
        </div>
        <div class="fc-face fc-back">
          <div class="fc-lang">🇬🇧 English</div>
          <div id="fcBackWord">word</div>
          <div class="fc-hint">Ты знаешь это слово?</div>
        </div>
      </div>
    </div>

    <div class="flashcard-actions">
      <button class="fc-btn-unknown" onclick="fcAnswer(false)">❌ Учить</button>
      <button class="fc-btn-know" onclick="fcAnswer(true)">✅ Знаю!</button>
    </div>
  </div>

  <!-- ═══ РЕЗУЛЬТАТ ФЛЭШКАРТ ═══ -->
  <div id="flashcardResultScreen" class="card screen">
    <div class="result-hero" id="fcResultEmoji">🃏</div>
    <div class="result-title" id="fcResultTitle">Раунд завершён!</div>
    <div class="result-stats">
      <div class="result-stat">✅ Знаю<strong id="fcResKnow">0</strong></div>
      <div class="result-stat">📚 Учить<strong id="fcResUnknown">0</strong></div>
    </div>
    <div class="result-verdict" id="fcVerdict"></div>
    <div style="display:flex;flex-direction:column;gap:10px;">
      <button class="btn-primary" onclick="startFlashcards()">🔄 Ещё раз</button>
      <button class="btn-secondary" onclick="fcRepeatUnknown()" id="fcRepeatBtn">📚 Повторить ошибки</button>
      <button class="btn-secondary" onclick="showScreen('menuScreen');updateMenu();">🏠 В меню</button>
    </div>
  </div>

  <!-- ═══ АНАГРАММА ═══ -->
  <div id="anagramScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="exitAnagram()">← Выйти</button>
      <div class="stats-bar">
        <div class="mini-stat">⭐ <span id="agXp">0</span></div>
        <div class="mini-stat">🔥 <span id="agStreak">0</span></div>
      </div>
    </div>

    <div class="prog-wrap"><div class="prog-bar sprint" id="agBar"></div></div>
    <div class="sprint-counter" id="agCounter" style="text-align:right;margin-bottom:6px;">Слово 1 из 10</div>

    <div class="timer-box">⏱️ <span id="agTimer" style="font-size:20px;font-weight:900;">20</span></div>

    <div class="anagram-q">Составь слово по переводу:</div>
    <div class="anagram-word" id="agWord">слово</div>

    <div class="anagram-blank" id="agBlank"></div>

    <div class="anagram-answer" id="agAnswer" onclick="anagramAnswerClick(event)"></div>

    <div class="anagram-letters" id="agLetters"></div>

    <div class="actions">
      <button class="btn-secondary btn-hint" onclick="agHint()" id="agHintBtn">💡 Подсказка</button>
      <button class="btn-secondary" onclick="agSkip()">⏭ Пропуск</button>
      <button class="btn-primary" onclick="agCheck()">✅ Проверить</button>
    </div>
    <div class="result" id="agResult"></div>
  </div>

  <!-- ═══ РЕЗУЛЬТАТЫ СПРИНТА ═══ -->
  <div id="resultsScreen" class="card screen">
    <div class="result-hero" id="resultsEmoji">🏆</div>
    <div class="result-title" id="resultsTitle">Спринт завершён!</div>
    <div class="result-pct" id="resultsPct">0%</div>
    <div class="result-stats">
      <div class="result-stat">✅ Правильно<strong id="resCorrect">0</strong></div>
      <div class="result-stat">❌ Ошибки<strong id="resErrors">0</strong></div>
      <div class="result-stat">⭐ XP<strong id="resXp">0</strong></div>
      <div class="result-stat">🔥 Серия<strong id="resStreak">0</strong></div>
    </div>
    <div class="result-verdict" id="resultsVerdict"></div>
    <div style="display:flex;flex-direction:column;gap:10px;">
      <button class="btn-primary" onclick="startSprint()">🔄 Ещё раз</button>
      <button class="btn-secondary" onclick="showScreen('menuScreen');updateMenu();">🏠 В меню</button>
    </div>
  </div>

  <!-- ═══ МОИ СЛОВА ═══ -->
  <div id="myWordsScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:12px;">📝 Мои слова</div>
    <div style="font-size:13px;color:var(--text2);margin-bottom:14px;">Добавь свои пары — они войдут в категорию «Мои слова».</div>
    <div class="mw-add-row">
      <input type="text" id="mwRu" placeholder="🇷🇺 Русский">
      <input type="text" id="mwEn" placeholder="🇬🇧 English">
      <button class="mw-add-btn" onclick="addMyWord()">+ Добавить</button>
    </div>
    <div class="mw-list" id="mwList"></div>
    <button class="btn-secondary" style="width:100%;margin-top:6px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ СТАТИСТИКА ═══ -->
  <div id="statsScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:16px;">📊 Статистика</div>

    <div class="stats-grid">
      <div class="stats-cell">
        <div class="sc-icon">⭐</div>
        <div class="sc-label">Всего XP</div>
        <div class="sc-val" id="stXp">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">📖</div>
        <div class="sc-label">Уровень</div>
        <div class="sc-val" id="stLevel">1</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">✅</div>
        <div class="sc-label">Правильно</div>
        <div class="sc-val" id="stCorrect">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">📝</div>
        <div class="sc-label">Всего ответов</div>
        <div class="sc-val" id="stTotal">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">🔥</div>
        <div class="sc-label">Макс. серия</div>
        <div class="sc-val" id="stStreak">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">📅</div>
        <div class="sc-label">XP сегодня</div>
        <div class="sc-val" id="stDailyXp">0</div>
      </div>
    </div>

    <div class="accuracy-visual">
      <div class="section-title">Точность ответов</div>
      <div style="display:flex;justify-content:space-between;font-size:22px;font-weight:900;margin-bottom:6px;">
        <span id="stAccPct">0%</span>
        <span style="color:var(--text2);font-size:14px;font-weight:700;align-self:flex-end" id="stAccDetail"></span>
      </div>
      <div class="acc-bar-wrap">
        <div class="acc-bar-ok" id="stAccBar" style="width:0%"></div>
      </div>
    </div>

    <div class="rank-progress">
      <div class="section-title">Путь к мастерству</div>
      <div class="rank-levels" id="rankLevels"></div>
    </div>

    <button class="btn-secondary" style="width:100%;margin-bottom:10px;" onclick="showScreen('menuScreen')">← Назад</button>
    <div style="text-align:center;">
      <button class="reset-btn" onclick="confirmReset()">⚠️ Сбросить прогресс</button>
    </div>
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

</div><!-- /container -->

<!-- ═══ ПАУЗА ═══ -->
<div class="overlay" id="pauseOverlay">
  <div class="pause-card">
    <div class="pause-title">⏸ Пауза</div>
    <div class="settings-row">
      <label>Направление перевода</label>
      <select id="mode" onchange="savePrefs();">
        <option value="ru-en">🇷🇺 → 🇬🇧 Русский → Английский</option>
        <option value="en-ru">🇬🇧 → 🇷🇺 Английский → Русский</option>
        <option value="mixed">🔀 Смешанный</option>
      </select>
    </div>
    <div class="settings-row">
      <label>Категория</label>
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
        <option value="phrases">💬 Фразы</option>
        <option value="myWords">⭐ Мои слова</option>
      </select>
    </div>
    <div class="pause-menu-actions">
      <button class="btn-primary" onclick="resumeGame()">▶️ Продолжить</button>
      <button class="btn-secondary" onclick="goToMainMenu()">🏠 Главное меню</button>
    </div>
  </div>
</div>

<script>
// ════════════════════════════════════
//  ДАННЫЕ — СЛОВАРИ
// ════════════════════════════════════
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
    {ru:"зарядное устройство",en:"charger"},{ru:"пульт управления",en:"remote control"},{ru:"лампа",en:"lamp"}
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
  // Новая категория — полезные фразы
  phrases:[
    {ru:"Как дела?",en:"How are you?"},{ru:"Всё хорошо",en:"I'm fine / I'm good"},
    {ru:"Спасибо",en:"Thank you"},{ru:"Пожалуйста",en:"You're welcome / Please"},
    {ru:"Извини",en:"Sorry / Excuse me"},{ru:"Не понимаю",en:"I don't understand"},
    {ru:"Повтори, пожалуйста",en:"Please repeat that"},{ru:"Сколько это стоит?",en:"How much is it?"},
    {ru:"Где находится...?",en:"Where is...?"},{ru:"Помогите мне",en:"Help me"},
    {ru:"Я не знаю",en:"I don't know"},{ru:"Приятно познакомиться",en:"Nice to meet you"},
    {ru:"До свидания",en:"Goodbye / See you"},{ru:"Доброе утро",en:"Good morning"},
    {ru:"Добрый вечер",en:"Good evening"},{ru:"Спокойной ночи",en:"Good night"},
    {ru:"Удачи!",en:"Good luck!"},{ru:"Поздравляю!",en:"Congratulations!"},
    {ru:"Счастливого пути",en:"Have a safe trip"},{ru:"Будь здоров",en:"Bless you / Stay healthy"}
  ],
  myWords:[]
};

// ════════════════════════════════════
//  ЦИТАТЫ
// ════════════════════════════════════
const quotes = [
  { text:"The limits of my language are the limits of my world.", author:"Ludwig Wittgenstein" },
  { text:"One language sets you in a corridor for life. Two languages open every door.", author:"Frank Smith" },
  { text:"To have another language is to possess a second soul.", author:"Charlemagne" },
  { text:"Language is the road map of a culture.", author:"Rita Mae Brown" },
  { text:"The more languages you know, the more you are human.", author:"Tomáš Masaryk" },
  { text:"Learning a language is the discovery of a new world.", author:"Unknown" },
  { text:"Every word learned is a step forward.", author:"Unknown" },
  { text:"Practice makes perfect — especially with language.", author:"Unknown" },
];

// ════════════════════════════════════
//  ПРОГРЕСС
// ════════════════════════════════════
let xp        = parseInt(localStorage.getItem("xp"))        || 0;
let correct   = parseInt(localStorage.getItem("correct"))   || 0;
let total     = parseInt(localStorage.getItem("total"))     || 0;
let maxStreak = parseInt(localStorage.getItem("maxStreak")) || 0;
let streak    = parseInt(localStorage.getItem("streak"))    || 0;
let soundEnabled = localStorage.getItem("soundEnabled") !== "false";

const TODAY = new Date().toDateString();
const savedDay = localStorage.getItem("etu_day");
let dailyXp = savedDay === TODAY ? (parseInt(localStorage.getItem("etu_dailyXp"))||0) : 0;
if (savedDay !== TODAY) {
  localStorage.setItem("etu_day", TODAY);
  localStorage.setItem("etu_dailyXp", 0);
}

try { categories.myWords = JSON.parse(localStorage.getItem("myWords")) || []; } catch(e){}

let earnedAchievements = [];
try { earnedAchievements = JSON.parse(localStorage.getItem("achievements")) || []; } catch(e){}
let mistakes = [];
try { mistakes = JSON.parse(localStorage.getItem("mistakes")) || []; } catch(e){}
let wordWeights = {};
try { wordWeights = JSON.parse(localStorage.getItem("wordWeights")) || {}; } catch(e){}

// ════════════════════════════════════
//  СОСТОЯНИЕ
// ════════════════════════════════════
let timerVal = 15, interval;
let currentWordObj, correctAnswerString, currentLanguage = 'ru';
let gameMode = localStorage.getItem("gameMode") || "write";
let isPaused = false, isAnswering = false, isGameActive = false;
let isSprint = false, sprintLen = 10, sprintIdx = 0;
let sprintCorrect = 0, sprintErrors = 0, sprintXpEarned = 0, sprintMaxStreak = 0;

// Флэшкарты
let fcPool = [], fcIdx = 0, fcKnown = 0, fcUnknown = 0, fcUnknownCards = [];

// Анаграмма
let agPool = [], agIdx = 0, agWord = "", agTarget = "", agInterval;
let agTimer = 20, agCorrect = 0, agErrors = 0, agXpEarned = 0, agStreak = 0;
const AG_TOTAL = 10;

// ════════════════════════════════════
//  ДОСТИЖЕНИЯ
// ════════════════════════════════════
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
  "ach_flash":   "🃏 Первый раунд флэшкарт",
  "ach_anagram": "🧩 Первая анаграмма",
  "ach_phrases": "💬 Знаток фраз",
};

// ════════════════════════════════════
//  АУДИО
// ════════════════════════════════════
let audioCtx = null;
function unlockAudio() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  if (audioCtx.state === 'suspended') audioCtx.resume();
}
function beep(freq, type, dur, vol=0.07) {
  if (!soundEnabled || !audioCtx) return;
  try {
    const o = audioCtx.createOscillator(), g = audioCtx.createGain();
    o.connect(g); g.connect(audioCtx.destination);
    o.frequency.value = freq; o.type = type;
    g.gain.setValueAtTime(vol, audioCtx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + dur);
    o.start(); o.stop(audioCtx.currentTime + dur);
  } catch(e){}
}
function playCorrect(){ beep(523,'sine',.1); setTimeout(()=>beep(659,'sine',.12),70); setTimeout(()=>beep(784,'sine',.2),140); }
function playWrong(){ beep(300,'sawtooth',.15,.1); setTimeout(()=>beep(200,'sawtooth',.25,.12),150); }
function playStreak(){ beep(659,'sine',.08); setTimeout(()=>beep(784,'sine',.08),60); setTimeout(()=>beep(987,'sine',.08),120); setTimeout(()=>beep(1047,'sine',.2),180); }
function playAchievement(){ [523,659,784,1047].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.25,.12),i*80)); }
function playTick(){ beep(880,'square',.05,.04); }
function playHint(){ beep(440,'triangle',.15,.07); }
function playSprint(){ [523,587,659,698,784,880,988,1047].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.2,.1),i*90)); }
function playStart(){ beep(440,'sine',.1); setTimeout(()=>beep(550,'sine',.1),100); setTimeout(()=>beep(660,'sine',.2),200); }
function playClick(){ beep(600,'square',.05,.04); }
function playFlip(){ beep(800,'sine',.08,.05); }

function speak(text) {
  if ('speechSynthesis' in window) {
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.lang = "en-US"; u.rate = 0.9;
    speechSynthesis.speak(u);
  }
}
function speakCurrent() {
  unlockAudio();
  if (currentWordObj && currentLanguage === 'en') speak(currentWordObj.en.split('/')[0].trim());
}

// ════════════════════════════════════
//  УТИЛИТЫ
// ════════════════════════════════════
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
function pickWord(pool) {
  const weighted = [];
  pool.forEach(w => {
    const wt = (wordWeights[w.en]||0) + 1;
    for (let i=0;i<wt;i++) weighted.push(w);
  });
  let c = weighted[Math.floor(Math.random()*weighted.length)];
  let tries = 0;
  while (tries++ < 20 && pool.length > 1 && currentWordObj && c.en === currentWordObj.en) {
    c = weighted[Math.floor(Math.random()*weighted.length)];
  }
  return c;
}
function getLevel(x){ return Math.floor(x/100)+1; }
function getNextLvlXp(x){ return (Math.floor(x/100)+1)*100; }

// ════════════════════════════════════
//  СОХРАНЕНИЕ
// ════════════════════════════════════
function save() {
  localStorage.setItem("xp", xp);
  localStorage.setItem("correct", correct);
  localStorage.setItem("total", total);
  localStorage.setItem("streak", streak);
  if (streak > maxStreak){ maxStreak = streak; localStorage.setItem("maxStreak", maxStreak); }
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

// ════════════════════════════════════
//  РАНГИ
// ════════════════════════════════════
const ranks = [
  {min:0,   label:"🌱 Новичок",   avatar:"🌱"},
  {min:100, label:"💪 Энтузиаст", avatar:"💪"},
  {min:300, label:"🔥 Продвинутый",avatar:"🔥"},
  {min:700, label:"🏆 Мастер слов",avatar:"🏆"},
  {min:1500,label:"🧙 Эрудит",    avatar:"🧙"},
  {min:3000,label:"⚡ Легенда",   avatar:"⚡"},
];
function getRankObj(x){ return [...ranks].reverse().find(r=>x>=r.min) || ranks[0]; }

// ════════════════════════════════════
//  ЭКРАНЫ
// ════════════════════════════════════
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}
function goToMainMenu() {
  clearInterval(interval);
  clearInterval(agInterval);
  isPaused=false; isAnswering=false; isGameActive=false;
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
function showStatsScreen() {
  playClick();
  updateStats();
  showScreen("statsScreen");
  document.getElementById("dashboardCard").style.display = "none";
}

// ════════════════════════════════════
//  МЕНЮ / ДАШБОРД
// ════════════════════════════════════
function updateMenu() {
  const lvl = getLevel(xp);
  const nextXp = getNextLvlXp(xp);
  const pct = ((xp % 100) / 100 * 100);
  document.getElementById("menuXp").textContent = xp;
  document.getElementById("menuLevel").textContent = lvl;
  document.getElementById("nextLvlXp").textContent = nextXp;
  document.getElementById("levelBar").style.width = pct + "%";
  document.getElementById("levelTxt").textContent = `${100 - (xp%100)} XP до уровня ${lvl+1}`;
  document.getElementById("menuStreak").textContent = maxStreak;
  document.getElementById("menuAccuracy").textContent = total ? Math.round(correct/total*100)+"%" : "—";
  const rank = getRankObj(xp);
  document.getElementById("userRank").textContent = rank.label;
  document.getElementById("menuAvatar").textContent = rank.avatar;
  document.getElementById("dailyGoalText").textContent = `${dailyXp} / 50 XP`;
  document.getElementById("dailyGoalBar").style.width = Math.min(dailyXp/50*100,100)+"%";

  // Цитата
  const q = quotes[new Date().getDate() % quotes.length];
  document.getElementById("quoteText").innerHTML = `"${q.text}" <strong>— ${q.author}</strong>`;
}

function updateStats() {
  document.getElementById("stXp").textContent = xp;
  document.getElementById("stLevel").textContent = getLevel(xp);
  document.getElementById("stCorrect").textContent = correct;
  document.getElementById("stTotal").textContent = total;
  document.getElementById("stStreak").textContent = maxStreak;
  document.getElementById("stDailyXp").textContent = dailyXp;
  const acc = total ? Math.round(correct/total*100) : 0;
  document.getElementById("stAccPct").textContent = acc + "%";
  document.getElementById("stAccDetail").textContent = `${correct} из ${total}`;
  document.getElementById("stAccBar").style.width = acc + "%";

  // Ранг лестница
  const c = document.getElementById("rankLevels");
  c.innerHTML = ranks.map(r=>{
    const earned = xp >= r.min;
    const current = getRankObj(xp).min === r.min;
    return `<div class="rank-item ${earned?'earned':''} ${current?'current':''}">${r.label}</div>`;
  }).join("");
}

// ════════════════════════════════════
//  ИГРА — ЗАПУСК
// ════════════════════════════════════
function startEndless() {
  playStart(); isSprint=false; isGameActive=true;
  document.getElementById("sprintCounterRow").style.display="none";
  document.getElementById("sprintProgWrap").style.display="none";
  document.getElementById("dashboardCard").style.display="none";
  showScreen("gameScreen"); nextWord();
}

let sprintLenSelected = 10;
function setSprintLen(n, btn) {
  sprintLenSelected = n;
  document.querySelectorAll('.sprint-opt-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}
function startSprint() {
  playStart(); isSprint=true;
  sprintLen=sprintLenSelected||10; sprintIdx=0;
  sprintCorrect=0; sprintErrors=0; sprintXpEarned=0; sprintMaxStreak=0;
  isGameActive=true;
  document.getElementById("sprintCounterRow").style.display="block";
  document.getElementById("sprintProgWrap").style.display="block";
  document.getElementById("dashboardCard").style.display="none";
  showScreen("gameScreen"); nextWord();
}

// ════════════════════════════════════
//  РЕЖИМ ВВОДА
// ════════════════════════════════════
function setGameMode(mode) {
  gameMode = mode; localStorage.setItem("gameMode", mode);
  document.getElementById("modeInputBtn").classList.toggle("active", mode==='write');
  document.getElementById("modeTestBtn").classList.toggle("active", mode==='test');
  document.getElementById("inputSection").style.display = mode==='write'?'block':'none';
  document.getElementById("testSection").style.display = mode==='test'?'block':'none';
  document.getElementById("checkBtn").style.display = mode==='write'?'flex':'none';
  if (isGameActive) nextWord();
}

// ════════════════════════════════════
//  СЛОВО
// ════════════════════════════════════
function nextWord() {
  if (isPaused) return;
  if (isSprint && sprintIdx >= sprintLen) { showResults(); return; }
  isAnswering=false; toggleBtns(false);
  const pool = getPool();
  if (!pool.length){ alertPop("В категории нет слов!"); return; }
  currentWordObj = pickWord(pool);
  const dir = document.getElementById("mode").value;
  if (dir==="ru-en" || (dir==="mixed" && Math.random()<.5)) {
    document.getElementById("word").textContent = currentWordObj.ru;
    correctAnswerString = currentWordObj.en; currentLanguage='ru';
    document.getElementById("speakBtn").style.display='none';
  } else {
    document.getElementById("word").textContent = currentWordObj.en;
    correctAnswerString = currentWordObj.ru; currentLanguage='en';
    document.getElementById("speakBtn").style.display='inline-flex';
  }
  document.getElementById("answer").value="";
  document.getElementById("answer").className="";
  document.getElementById("result").innerHTML="";
  document.getElementById("result").className="result";
  if (isSprint){
    document.getElementById("sprintCurrent").textContent = sprintIdx+1;
    document.getElementById("sprintTotal").textContent = sprintLen;
    document.getElementById("sprintBar").style.width = (sprintIdx/sprintLen*100)+"%";
  }
  if (gameMode==='test') genOptions();
  if (gameMode==='write' && window.innerWidth>768) document.getElementById("answer").focus();
  resetTimer();
}

// ════════════════════════════════════
//  ТЕСТ: ВАРИАНТЫ
// ════════════════════════════════════
function genOptions() {
  const pool = getPool();
  const key = currentLanguage==='ru'?'en':'ru';
  const corr = currentWordObj[key];
  let opts = [corr];
  const uniq = [...new Set(pool.map(w=>w[key]))].filter(v=>v!==corr);
  while (opts.length < Math.min(4, uniq.length+1)){
    const r = uniq[Math.floor(Math.random()*uniq.length)];
    if (!opts.includes(r)) opts.push(r);
  }
  opts.sort(()=>Math.random()-.5);
  const grid = document.getElementById("optionsGrid");
  grid.innerHTML="";
  opts.forEach(opt=>{
    const btn = document.createElement("button");
    btn.className="option-btn";
    btn.textContent = opt.charAt(0).toUpperCase()+opt.slice(1);
    btn.onclick=()=>{ unlockAudio(); selectOpt(opt,btn); };
    grid.appendChild(btn);
  });
}
function selectOpt(val, btn) {
  if (isAnswering) return;
  isAnswering=true; clearInterval(interval); toggleBtns(true);
  const ok = verify(val, correctAnswerString);
  document.querySelectorAll(".option-btn").forEach(b=>{
    b.style.pointerEvents="none";
    if (verify(b.textContent, correctAnswerString)) b.classList.add("correct-choice");
  });
  if (!ok && btn) btn.classList.add("incorrect-choice");
  ok ? onCorrect() : onWrong();
}

// ════════════════════════════════════
//  ПРОВЕРКА
// ════════════════════════════════════
function checkAnswer() {
  unlockAudio();
  if (gameMode!=='write'||isAnswering) return;
  const val = document.getElementById("answer").value.trim();
  if (!val) return;
  isAnswering=true; clearInterval(interval); toggleBtns(true);
  const ok = verify(val, correctAnswerString);
  document.getElementById("answer").classList.add(ok?'input-correct':'input-wrong');
  ok ? onCorrect() : onWrong();
}

function onCorrect() {
  total++; correct++; streak++;
  if (isSprint){ sprintCorrect++; sprintMaxStreak = Math.max(sprintMaxStreak,streak); }
  const key = currentWordObj.en;
  wordWeights[key] = Math.max((wordWeights[key]||0)-1, 0);
  const mult = streak>=15?3:streak>=5?2:1;
  const earned = 10*mult;
  xp+=earned; dailyXp+=earned;
  if (isSprint) sprintXpEarned+=earned;
  const res = document.getElementById("result");
  res.className="result ok-text";
  res.innerHTML = `✅ Правильно! +${earned} XP${mult>1?` <b>(×${mult} 🔥)</b>`:''}`;
  spawnXpPop(earned);
  document.getElementById("gameScreen").classList.add("flash-ok");
  setTimeout(()=>document.getElementById("gameScreen").classList.remove("flash-ok"),350);
  if (mult>1){ playStreak(); showStreakBanner(streak); }
  else playCorrect();
  if (currentLanguage==='ru') speak(currentWordObj.en.split('/')[0].trim());
  if (streak>maxStreak) maxStreak=streak;
  update(); save(); checkAchievements();
  if (isSprint){ sprintIdx++; setTimeout(nextWord, sprintIdx>=sprintLen?600:250); }
  else setTimeout(nextWord,250);
}
function onWrong() {
  total++; streak=0;
  if (isSprint) sprintErrors++;
  const key = currentWordObj.en;
  wordWeights[key]=(wordWeights[key]||0)+2;
  const res = document.getElementById("result");
  res.className="result err-text";
  res.innerHTML = `❌ Ошибка. Верно: <strong>${correctAnswerString}</strong>`;
  document.getElementById("gameScreen").classList.add("shake");
  setTimeout(()=>document.getElementById("gameScreen").classList.remove("shake"),500);
  playWrong();
  const isEN = currentLanguage==='en';
  const mstr = isEN?`${currentWordObj.en} — ${currentWordObj.ru}`:`${currentWordObj.ru} — ${currentWordObj.en}`;
  if (!mistakes.includes(mstr)){ mistakes.push(mstr); renderMistakes(); }
  update(); save(); checkAchievements();
  if (isSprint){ sprintIdx++; setTimeout(nextWord,sprintIdx>=sprintLen?600:1200); }
  else setTimeout(nextWord,1500);
}

// ════════════════════════════════════
//  ПОДСКАЗКА / ПРОПУСК
// ════════════════════════════════════
function getHint() {
  unlockAudio(); if (isAnswering) return;
  if (xp<5){ alertPop("Нужно минимум 5 XP для подсказки!"); return; }
  xp-=5; playHint(); update(); save();
  if (gameMode==='write'){
    const inp = document.getElementById("answer");
    const clean = norm(correctAnswerString)[0];
    if (clean){ inp.value=clean.substring(0,Math.max(inp.value.length+1,1)); inp.focus(); }
  } else {
    let removed=0;
    document.querySelectorAll(".option-btn").forEach(btn=>{
      if (removed<1&&!verify(btn.textContent,correctAnswerString)&&btn.style.opacity!=="0.3"){
        btn.style.opacity="0.3"; btn.style.pointerEvents="none"; removed++;
      }
    });
  }
}
function skipWord() {
  if (isAnswering) return;
  playClick(); if (isSprint) sprintIdx++; nextWord();
}

// ════════════════════════════════════
//  ТАЙМЕР
// ════════════════════════════════════
const TIMER_MAX = 15;
function resetTimer() {
  clearInterval(interval); timerVal=TIMER_MAX;
  const el = document.getElementById("timer");
  const circle = document.getElementById("timerCircle");
  el.textContent = timerVal;
  if (circle){ circle.style.stroke="var(--warn)"; circle.style.strokeDashoffset="0"; }
  interval = setInterval(()=>{
    if (isPaused||isAnswering) return;
    timerVal--;
    el.textContent=timerVal;
    const offset = (1-timerVal/TIMER_MAX)*100;
    if (circle){ circle.style.strokeDashoffset=offset; }
    if (timerVal<=5){ if (circle) circle.style.stroke="var(--err)"; playTick(); }
    if (timerVal<=0){
      clearInterval(interval); streak=0; update(); save();
      if (isSprint) sprintIdx++;
      nextWord();
    }
  },1000);
}

// ════════════════════════════════════
//  ПАУЗА
// ════════════════════════════════════
function pauseGame() {
  clearInterval(interval); isPaused=true;
  document.getElementById("pauseOverlay").classList.add("active");
}
function resumeGame() {
  isPaused=false; savePrefs();
  document.getElementById("pauseOverlay").classList.remove("active");
  nextWord();
}

function toggleBtns(disabled) {
  ['answer','skipBtn','hintBtn','checkBtn'].forEach(id=>{
    const el=document.getElementById(id); if(el) el.disabled=disabled;
  });
}

// ════════════════════════════════════
//  UPDATE UI
// ════════════════════════════════════
function update() {
  document.getElementById("gameXp").textContent=xp;
  document.getElementById("gameStreak").textContent=streak;
  document.getElementById("gameAccuracy").textContent=(total?Math.round(correct/total*100):0)+"%";
  document.getElementById("xpBar").style.width=(xp%100)+"%";
  updateMenu();
}

// ════════════════════════════════════
//  РЕЗУЛЬТАТЫ СПРИНТА
// ════════════════════════════════════
function showResults() {
  clearInterval(interval); isGameActive=false; playSprint();
  document.getElementById("resCorrect").textContent=sprintCorrect;
  document.getElementById("resErrors").textContent=sprintErrors;
  document.getElementById("resXp").textContent=sprintXpEarned;
  document.getElementById("resStreak").textContent=sprintMaxStreak;
  const pct = sprintLen>0?Math.round(sprintCorrect/sprintLen*100):0;
  document.getElementById("resultsPct").textContent=pct+"%";
  let emoji="😅", title="Не сдавайся!", verdict="";
  if (pct===100){emoji="🏆";title="Идеально! 🎉";triggerAch("ach_perfect");}
  else if(pct>=80){emoji="🌟";title="Отличный результат!";}
  else if(pct>=60){emoji="👍";title="Хорошая работа!";}
  else if(pct>=40){emoji="💪";title="Продолжай тренироваться!";}
  verdict=`Правильных: <b>${sprintCorrect} из ${sprintLen}</b> (${pct}%)<br>Заработано: <b>+${sprintXpEarned} XP</b> • Серия: <b>${sprintMaxStreak}</b>`;
  document.getElementById("resultsEmoji").textContent=emoji;
  document.getElementById("resultsTitle").textContent=title;
  document.getElementById("resultsVerdict").innerHTML=verdict;
  triggerAch("ach_sprint");
  showScreen("resultsScreen");
  document.getElementById("dashboardCard").style.display="none";
}

// ════════════════════════════════════
//  ФЛЭШКАРТЫ
// ════════════════════════════════════
function startFlashcards(customPool) {
  playStart();
  const pool = customPool || getPool().sort(()=>Math.random()-.5).slice(0, 20);
  fcPool = pool; fcIdx=0; fcKnown=0; fcUnknown=0; fcUnknownCards=[];
  document.getElementById("dashboardCard").style.display="none";
  showScreen("flashcardScreen");
  renderFlashcard();
  triggerAch("ach_flash");
}
function renderFlashcard() {
  if (fcIdx >= fcPool.length){ showFlashcardResult(); return; }
  const w = fcPool[fcIdx];
  const card = document.getElementById("flashcard");
  card.classList.remove("flipped");
  document.getElementById("fcFrontWord").textContent = w.ru;
  document.getElementById("fcBackWord").textContent = w.en;
  document.getElementById("fcCounter").textContent = `Карточка ${fcIdx+1} из ${fcPool.length}`;
  document.getElementById("fcBar").style.width = (fcIdx/fcPool.length*100)+"%";
  document.getElementById("fcKnowCount").textContent = fcKnown;
  document.getElementById("fcUnknCount").textContent = fcUnknown;
  document.getElementById("fcLeftCount").textContent = fcPool.length - fcIdx;
}
function flipCard() {
  playFlip(); unlockAudio();
  document.getElementById("flashcard").classList.toggle("flipped");
}
function fcAnswer(known) {
  unlockAudio();
  if (known){ fcKnown++; playCorrect(); }
  else { fcUnknown++; fcUnknownCards.push(fcPool[fcIdx]); playWrong(); }
  fcIdx++;
  setTimeout(renderFlashcard, 200);
}
function showFlashcardResult() {
  const total = fcKnown + fcUnknown;
  const pct = total?Math.round(fcKnown/total*100):0;
  let emoji="📚", title="Продолжай!";
  if(pct===100){emoji="🎉";title="Все слова знаешь!";}
  else if(pct>=80){emoji="🌟";title="Отлично!";}
  else if(pct>=60){emoji="👍";title="Хорошо!";}
  document.getElementById("fcResultEmoji").textContent=emoji;
  document.getElementById("fcResultTitle").textContent=title;
  document.getElementById("fcResKnow").textContent=fcKnown;
  document.getElementById("fcResUnknown").textContent=fcUnknown;
  document.getElementById("fcVerdict").innerHTML=`Знаешь <b>${fcKnown}</b> из <b>${total}</b> (${pct}%)${fcUnknownCards.length>0?`<br>Рекомендуем повторить: <b>${fcUnknownCards.length}</b> слов`:'.'}`;
  const repBtn = document.getElementById("fcRepeatBtn");
  repBtn.style.display = fcUnknownCards.length>0?'flex':'none';
  showScreen("flashcardResultScreen");
  xp += fcKnown*3; dailyXp += fcKnown*3; save(); updateMenu();
}
function fcRepeatUnknown() {
  if (fcUnknownCards.length>0) startFlashcards(fcUnknownCards);
}
function exitFlashcards() { goToMainMenu(); }

// ════════════════════════════════════
//  АНАГРАММА
// ════════════════════════════════════
function startAnagram() {
  playStart();
  const pool = getPool().filter(w=>w.en.split('/')[0].trim().length<=10 && !w.en.includes(' '));
  if (pool.length < 5){ alertPop("Нет подходящих слов! Выбери категорию с простыми словами."); return; }
  agPool = pool.sort(()=>Math.random()-.5).slice(0, AG_TOTAL);
  agIdx=0; agCorrect=0; agErrors=0; agXpEarned=0; agStreak=0;
  document.getElementById("dashboardCard").style.display="none";
  showScreen("anagramScreen");
  nextAnagram();
  triggerAch("ach_anagram");
}
function nextAnagram() {
  clearInterval(agInterval);
  if (agIdx >= agPool.length){ showAnagramResult(); return; }
  const w = agPool[agIdx];
  agWord = w.ru;
  agTarget = w.en.split('/')[0].trim().toLowerCase();
  document.getElementById("agWord").textContent = agWord;
  document.getElementById("agCounter").textContent = `Слово ${agIdx+1} из ${AG_TOTAL}`;
  document.getElementById("agBar").style.width=(agIdx/AG_TOTAL*100)+"%";
  document.getElementById("agResult").innerHTML="";
  document.getElementById("agResult").className="result";
  document.getElementById("agXp").textContent=xp;
  document.getElementById("agStreak").textContent=agStreak;
  renderAnagramLetters();
  agTimer=20; agTick();
  agInterval = setInterval(()=>{
    agTimer--;
    document.getElementById("agTimer").textContent=agTimer;
    if(agTimer<=5) playTick();
    if(agTimer<=0){ clearInterval(agInterval); agOnWrong(); }
  },1000);
}
function renderAnagramLetters(){
  const shuffled = agTarget.split('').sort(()=>Math.random()-.5);
  const lettersEl = document.getElementById("agLetters");
  const answerEl = document.getElementById("agAnswer");
  const blankEl = document.getElementById("agBlank");
  lettersEl.innerHTML = shuffled.map((l,i)=>
    `<div class="letter-tile" data-letter="${l}" data-idx="${i}" onclick="agAddLetter(this)">${l.toUpperCase()}</div>`
  ).join("");
  answerEl.innerHTML="";
  blankEl.innerHTML = agTarget.split('').map(()=>`<div class="blank-slot"></div>`).join("");
}
function agAddLetter(tile) {
  if (tile.classList.contains('used')) return;
  playClick();
  tile.classList.add('used');
  const answerEl = document.getElementById("agAnswer");
  const div = document.createElement("div");
  div.className="answer-tile";
  div.textContent=tile.dataset.letter.toUpperCase();
  div.dataset.srcIdx=tile.dataset.idx;
  answerEl.appendChild(div);
  updateAnagramBlank();
}
function anagramAnswerClick(e){
  const tile = e.target.closest('.answer-tile');
  if (!tile) return;
  playClick();
  const srcIdx = tile.dataset.srcIdx;
  document.querySelector(`.letter-tile[data-idx="${srcIdx}"]`)?.classList.remove('used');
  tile.remove();
  updateAnagramBlank();
}
function updateAnagramBlank(){
  const filled = document.getElementById("agAnswer").children.length;
  document.querySelectorAll(".blank-slot").forEach((s,i)=>{
    s.classList.toggle('filled', i<filled);
  });
}
function agHint(){
  playHint();
  const answerEl = document.getElementById("agAnswer");
  const pos = answerEl.children.length;
  if (pos >= agTarget.length) return;
  const nextChar = agTarget[pos];
  const tile = [...document.querySelectorAll('.letter-tile')].find(t=>!t.classList.contains('used')&&t.dataset.letter===nextChar);
  if(tile) agAddLetter(tile);
  xp=Math.max(0,xp-3); update(); save();
}
function agSkip(){
  clearInterval(agInterval);
  agErrors++; agStreak=0;
  document.getElementById("agResult").innerHTML=`⏭ Пропущено. Ответ: <strong>${agTarget}</strong>`;
  document.getElementById("agResult").className="result err-text";
  agIdx++;
  setTimeout(nextAnagram, 1200);
}
function agCheck(){
  unlockAudio();
  const answer = [...document.getElementById("agAnswer").children].map(t=>t.textContent.toLowerCase()).join('');
  if (!answer){ alertPop("Составь слово из букв!"); return; }
  clearInterval(agInterval);
  if (answer===agTarget){
    agCorrect++; agStreak++;
    const earned=10+(agStreak>=5?10:0); agXpEarned+=earned;
    xp+=earned; dailyXp+=earned;
    document.getElementById("agResult").innerHTML=`✅ Верно! +${earned} XP`;
    document.getElementById("agResult").className="result ok-text";
    spawnXpPop(earned);
    if(agStreak>=5) playStreak(); else playCorrect();
    speak(agTarget);
  } else {
    agErrors++; agStreak=0;
    document.getElementById("agResult").innerHTML=`❌ Неверно. Правильно: <strong>${agTarget}</strong>`;
    document.getElementById("agResult").className="result err-text";
    playWrong();
  }
  document.getElementById("agXp").textContent=xp;
  document.getElementById("agStreak").textContent=agStreak;
  update(); save(); checkAchievements();
  agIdx++;
  setTimeout(nextAnagram, 1300);
}
function agTick(){ document.getElementById("agTimer").textContent=agTimer; }
function agOnWrong(){
  agErrors++; agStreak=0;
  document.getElementById("agResult").innerHTML=`⏱ Время вышло! Ответ: <strong>${agTarget}</strong>`;
  document.getElementById("agResult").className="result err-text";
  playWrong();
  agIdx++;
  setTimeout(nextAnagram, 1200);
}
function showAnagramResult(){
  const pct = AG_TOTAL>0?Math.round(agCorrect/AG_TOTAL*100):0;
  let emoji="🧩",title="Анаграмма завершена!";
  if(pct===100){emoji="🏆";title="Все слова угаданы!";}
  else if(pct>=70){emoji="🌟";title="Отлично!";}
  sprintCorrect=agCorrect; sprintErrors=agErrors; sprintXpEarned=agXpEarned; sprintMaxStreak=agStreak;
  sprintLen=AG_TOTAL;
  document.getElementById("resCorrect").textContent=agCorrect;
  document.getElementById("resErrors").textContent=agErrors;
  document.getElementById("resXp").textContent=agXpEarned;
  document.getElementById("resStreak").textContent=agStreak;
  document.getElementById("resultsPct").textContent=pct+"%";
  document.getElementById("resultsEmoji").textContent=emoji;
  document.getElementById("resultsTitle").textContent=title;
  document.getElementById("resultsVerdict").innerHTML=`Угадано: <b>${agCorrect} из ${AG_TOTAL}</b> (${pct}%)<br>Заработано: <b>+${agXpEarned} XP</b>`;
  playSprint();
  showScreen("resultsScreen");
  document.getElementById("dashboardCard").style.display="none";
}
function exitAnagram(){
  clearInterval(agInterval); goToMainMenu();
}

// ════════════════════════════════════
//  ДОСТИЖЕНИЯ
// ════════════════════════════════════
function checkAchievements() {
  const lvl=getLevel(xp);
  if(correct>=10)  triggerAch("ach_10");
  if(correct>=50)  triggerAch("ach_50");
  if(correct>=100) triggerAch("ach_100");
  if(correct>=250) triggerAch("ach_250");
  if(streak>=10)   triggerAch("ach_str10");
  if(streak>=25)   triggerAch("ach_str25");
  if(streak>=50)   triggerAch("ach_str50");
  if(lvl>=5)       triggerAch("ach_lvl5");
  if(lvl>=10)      triggerAch("ach_lvl10");
  renderAchievements();
}
function triggerAch(id) {
  if (!earnedAchievements.includes(id) && achievementList[id]){
    earnedAchievements.push(id); save(); playAchievement();
    showAchPop(achievementList[id]);
  }
}
function renderAchievements() {
  const c=document.getElementById("achievements");
  if(!earnedAchievements.length){
    c.innerHTML=`<li class="empty-state">Нет достижений. Продолжай тренироваться!</li>`;return;
  }
  c.innerHTML=earnedAchievements.map(id=>`<li><span>${achievementList[id]||id}</span></li>`).join("");
}

// ════════════════════════════════════
//  ОШИБКИ
// ════════════════════════════════════
function renderMistakes() {
  const c=document.getElementById("mistakes");
  if(!mistakes.length){ c.innerHTML=`<li class="empty-state">Ошибок нет. Хорошая работа!</li>`;return; }
  c.innerHTML=mistakes.map((m,i)=>{
    const [a,b]=m.split(" — ");
    return `<li>
      <div><span class="err-ru">${a||''}</span> → <span class="ok-en">${b||''}</span></div>
      <button class="clean-btn" onclick="studyMistake('${(a||'').replace(/'/g,"\\'")}','${(b||'').replace(/'/g,"\\'")}',${i})">Учить</button>
    </li>`;
  }).join("");
}
function studyMistake(src,tgt,idx) {
  unlockAudio(); isGameActive=true; isSprint=false;
  showScreen("gameScreen");
  document.getElementById("dashboardCard").style.display="none";
  document.getElementById("sprintCounterRow").style.display="none";
  document.getElementById("sprintProgWrap").style.display="none";
  currentWordObj={ru:currentLanguage==='ru'?src:tgt, en:currentLanguage==='ru'?tgt:src};
  document.getElementById("word").textContent=src;
  correctAnswerString=tgt;
  document.getElementById("answer").value="";
  document.getElementById("result").innerHTML="💪 Режим отработки ошибки!";
  document.getElementById("result").className="result";
  if(gameMode==='test') genOptions();
  mistakes.splice(idx,1); save(); renderMistakes();
  isAnswering=false; toggleBtns(false); resetTimer();
}
function clearMistakes(){ mistakes=[];save();renderMistakes(); }

// ════════════════════════════════════
//  МОИ СЛОВА
// ════════════════════════════════════
function renderMyWords() {
  const list=document.getElementById("mwList");
  if(!categories.myWords.length){
    list.innerHTML=`<div class="empty-state" style="padding:16px;font-size:14px;">Пока нет слов. Добавь первое!</div>`;return;
  }
  list.innerHTML=categories.myWords.map((w,i)=>
    `<div class="mw-row">
      <span>🇷🇺 <b>${w.ru}</b> → 🇬🇧 <b>${w.en}</b></span>
      <button class="mw-del-btn" onclick="deleteMyWord(${i})">✕</button>
    </div>`
  ).join("");
}
function addMyWord() {
  const ru=document.getElementById("mwRu").value.trim();
  const en=document.getElementById("mwEn").value.trim();
  if(!ru||!en){ alertPop("Введи оба слова!"); return; }
  categories.myWords.push({ru,en});
  localStorage.setItem("myWords",JSON.stringify(categories.myWords));
  document.getElementById("mwRu").value="";
  document.getElementById("mwEn").value="";
  playCorrect(); triggerAch("ach_myword"); renderMyWords();
}
function deleteMyWord(i){
  categories.myWords.splice(i,1);
  localStorage.setItem("myWords",JSON.stringify(categories.myWords));
  renderMyWords();
}
document.getElementById("mwEn").addEventListener("keydown",e=>{ if(e.key==="Enter") addMyWord(); });

// ════════════════════════════════════
//  ТАБЫ
// ════════════════════════════════════
function switchTab(id) {
  ['tab-ach','tab-mis'].forEach(t=>document.getElementById(t).classList.remove('active'));
  ['achBtn','misBtn'].forEach(b=>document.getElementById(b).classList.remove('active'));
  document.getElementById(id).classList.add('active');
  document.getElementById(id==='tab-ach'?'achBtn':'misBtn').classList.add('active');
}

// ════════════════════════════════════
//  ТЕМА / ЗВУК
// ════════════════════════════════════
function toggleTheme() {
  document.body.classList.toggle("light");
  const l=document.body.classList.contains("light");
  localStorage.setItem("etu_theme",l?"light":"dark");
  document.getElementById("themeBtn").textContent=l?"☀️":"🌙";
}
function loadTheme() {
  const t=localStorage.getItem("etu_theme");
  if(t==="light"){ document.body.classList.add("light"); document.getElementById("themeBtn").textContent="☀️"; }
}
function toggleSound() {
  unlockAudio(); soundEnabled=!soundEnabled;
  localStorage.setItem("soundEnabled",soundEnabled);
  document.getElementById("soundBtn").textContent=soundEnabled?"🔊":"🔇";
  if(soundEnabled) playClick();
}

// ════════════════════════════════════
//  ПОПАПЫ
// ════════════════════════════════════
function spawnXpPop(amount) {
  const el=document.getElementById("gameXp");
  if(!el) return;
  const rect=el.getBoundingClientRect();
  const pop=document.createElement("div");
  pop.className="xp-pop"; pop.textContent=`+${amount} XP`;
  pop.style.left=(rect.left+rect.width/2-30)+"px";
  pop.style.top=(rect.top+window.scrollY-10)+"px";
  document.body.appendChild(pop);
  setTimeout(()=>pop.remove(),950);
}
function showAchPop(text) {
  const pop=document.createElement("div");
  pop.className="ach-pop";
  pop.innerHTML=`<div class="ach-pop-title">🎉 Достижение получено!</div>${text}`;
  document.body.appendChild(pop);
  setTimeout(()=>{ pop.style.opacity="0"; pop.style.transition="opacity .5s"; setTimeout(()=>pop.remove(),500); },3000);
}
function showStreakBanner(n) {
  if(n===5||n===10||n===15||n===20||n===25||n===50){
    const el=document.createElement("div");
    el.className="streak-banner";
    el.textContent=`🔥 ${n} серия!`;
    document.body.appendChild(el);
    setTimeout(()=>el.remove(),700);
  }
}
function alertPop(msg) {
  const d=document.createElement("div");
  d.className="notif"; d.textContent=msg;
  document.body.appendChild(d);
  setTimeout(()=>{ d.style.opacity="0"; d.style.transition="opacity .4s"; setTimeout(()=>d.remove(),400); },3000);
}

// ════════════════════════════════════
//  СБРОС
// ════════════════════════════════════
function confirmReset() {
  if(!confirm("Сбросить весь прогресс, XP, достижения и ошибки?")) return;
  localStorage.clear();
  xp=correct=total=maxStreak=streak=dailyXp=0;
  earnedAchievements=[]; mistakes=[]; wordWeights=[];
  categories.myWords=[];
  save(); goToMainMenu(); update();
  alertPop("Прогресс сброшен!");
}

// ════════════════════════════════════
//  КЛАВИШИ
// ════════════════════════════════════
document.addEventListener("keydown",e=>{
  if(e.key==="Escape"){
    if(document.getElementById("pauseOverlay").classList.contains("active")) resumeGame();
    else if(isGameActive) pauseGame();
  }
  if(e.key==="Enter"){ unlockAudio(); checkAnswer(); }
});

// ════════════════════════════════════
//  ИНИЦИАЛИЗАЦИЯ
// ════════════════════════════════════
loadTheme();
document.getElementById("soundBtn").textContent=soundEnabled?"🔊":"🔇";
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
