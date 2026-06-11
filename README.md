
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
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
button, .mode-card, .diff-option, .letter-tile, .answer-tile, .option-btn, .icon-btn {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  min-height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding: 16px 12px 40px;
  padding-left: max(12px, env(safe-area-inset-left));
  padding-right: max(12px, env(safe-area-inset-right));
  padding-bottom: max(40px, env(safe-area-inset-bottom));
  overflow-x: hidden;
  transition: background .4s, color .3s;
  -webkit-tap-highlight-color: transparent;
  touch-action: manipulation;
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

/* Сложность дневной цели */
.daily-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
.daily-title { font-size: 13px; font-weight: 700; }
.daily-change-btn {
  background: rgba(79,142,255,.15); color: var(--accent);
  border: 1px solid rgba(79,142,255,.3); padding: 3px 10px;
  border-radius: 8px; font-size: 11px; font-weight: 800;
  cursor: pointer; transition: var(--t); letter-spacing: .3px;
}
.daily-change-btn:hover { background: rgba(79,142,255,.28); }
.daily-difficulty-badge {
  display: inline-flex; align-items: center; gap: 4px;
  font-size: 11px; font-weight: 800; padding: 2px 9px;
  border-radius: 8px; margin-bottom: 6px;
}
.daily-progress-row { display: flex; justify-content: space-between; font-size: 12px; font-weight: 700; margin-bottom: 6px; }
.daily-xp-done { color: var(--ok); }
.daily-xp-goal { color: var(--text2); }
.daily-complete-banner {
  background: linear-gradient(135deg, rgba(34,216,143,.15), rgba(79,142,255,.1));
  border: 1px solid rgba(34,216,143,.4); border-radius: 10px;
  padding: 6px 12px; font-size: 12px; font-weight: 800; color: var(--ok);
  text-align: center; margin-top: 8px; display: none;
}

/* Модальное окно выбора сложности */
.diff-modal-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(8,12,20,.88); backdrop-filter: blur(14px);
  z-index: 200; justify-content: center; align-items: center;
  animation: fadeIn .25s ease;
}
.diff-modal-overlay.open { display: flex; }
.diff-modal {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: 24px; padding: 28px 24px; max-width: 400px; width: 94%;
  backdrop-filter: blur(20px); box-shadow: 0 30px 60px rgba(0,0,0,.5);
  animation: screenIn .3s ease;
}
.diff-modal-title { font-size: 22px; font-weight: 900; margin-bottom: 4px; }
.diff-modal-sub { font-size: 13px; color: var(--text2); margin-bottom: 20px; }
.diff-options { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
.diff-option {
  display: flex; align-items: center; gap: 14px;
  background: var(--muted2); border: 2px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px;
  cursor: pointer; transition: var(--t);
}
.diff-option:hover { border-color: var(--accent); transform: translateX(4px); }
.diff-option.selected { border-color: var(--accent); background: rgba(79,142,255,.12); }
.diff-option-icon { font-size: 28px; flex-shrink: 0; }
.diff-option-info { flex: 1; text-align: left; }
.diff-option-name { font-size: 15px; font-weight: 800; }
.diff-option-desc { font-size: 12px; color: var(--text2); margin-top: 2px; }
.diff-option-xp {
  font-size: 14px; font-weight: 900; padding: 4px 10px;
  border-radius: 8px; white-space: nowrap; flex-shrink: 0;
}
.diff-modal-close {
  width: 100%; padding: 13px; background: var(--muted); color: var(--text);
  border: 1px solid var(--card-border); border-radius: var(--r2);
  font-weight: 700; font-size: 15px; cursor: pointer; transition: var(--t);
}
.diff-modal-close:hover { border-color: var(--accent); }

/* Цветовые схемы для уровней */
.diff-easy   { background: rgba(34,216,143,.12) !important; }
.diff-easy.selected, .diff-easy:hover { border-color: var(--ok) !important; }
.diff-easy .diff-option-xp { background: rgba(34,216,143,.2); color: var(--ok); }
.diff-medium { background: rgba(79,142,255,.08) !important; }
.diff-medium.selected, .diff-medium:hover { border-color: var(--accent) !important; }
.diff-medium .diff-option-xp { background: rgba(79,142,255,.2); color: var(--accent); }
.diff-hard   { background: rgba(255,194,71,.08) !important; }
.diff-hard.selected, .diff-hard:hover { border-color: var(--warn) !important; }
.diff-hard .diff-option-xp { background: rgba(255,194,71,.2); color: var(--warn); }
.diff-ultra  { background: rgba(255,79,109,.08) !important; }
.diff-ultra.selected, .diff-ultra:hover { border-color: var(--err) !important; }
.diff-ultra .diff-option-xp { background: rgba(255,79,109,.2); color: var(--err); }

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
  z-index: 99999; white-space: nowrap; animation: notifIn .4s ease; max-width: calc(100vw - 32px);
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

/* ════════════════════════════════════
   ПРОИЗНОШЕНИЕ
════════════════════════════════════ */
.pronun-input-row {
  display: flex; gap: 8px; margin-bottom: 14px;
}
.pronun-input-row input {
  flex: 1; font-size: 17px; text-align: left;
}
.pronun-speak-btn {
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; border: none; border-radius: var(--r2);
  padding: 0 20px; font-size: 22px; cursor: pointer;
  flex-shrink: 0; transition: var(--t);
  display: flex; align-items: center; justify-content: center;
}
.pronun-speak-btn:hover { filter: brightness(1.15); transform: scale(1.05); }
.pronun-speak-btn:active { transform: scale(0.97); }
.pronun-speak-btn.speaking { animation: speakPulse .6s ease-in-out infinite alternate; }
@keyframes speakPulse {
  from { box-shadow: 0 0 0 0 rgba(79,142,255,.4); }
  to   { box-shadow: 0 0 0 12px rgba(79,142,255,0); }
}
.pronun-voice-row {
  display: flex; gap: 8px; align-items: center; margin-bottom: 14px; flex-wrap: wrap;
}
.pronun-voice-row label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; white-space: nowrap; }
.pronun-voice-row select { flex: 1; min-width: 140px; font-size: 13px; }
.pronun-speed-row {
  display: flex; gap: 8px; align-items: center; margin-bottom: 14px;
}
.pronun-speed-row label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; white-space: nowrap; }
.pronun-speed-row input[type=range] {
  flex: 1; accent-color: var(--accent);
  background: transparent; border: none; padding: 0; outline: none; box-shadow: none;
  height: 6px; cursor: pointer;
}
.pronun-speed-val { font-size: 13px; font-weight: 800; color: var(--accent); width: 32px; text-align: right; }
.pronun-history {
  max-height: 160px; overflow-y: auto; margin-top: 6px;
}
.pronun-history::-webkit-scrollbar { width: 4px; }
.pronun-history::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }
.pronun-hist-item {
  display: flex; align-items: center; justify-content: space-between;
  padding: 8px 12px; background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: 10px; margin-bottom: 6px; font-size: 14px; font-weight: 600; cursor: pointer;
  transition: var(--t);
}
.pronun-hist-item:hover { border-color: var(--accent); background: rgba(79,142,255,.08); }
.pronun-hist-repeat { font-size: 16px; color: var(--accent); }
.pronun-no-support {
  background: rgba(255,79,109,.1); border: 1px solid rgba(255,79,109,.3);
  border-radius: var(--r2); padding: 12px 16px; font-size: 13px; font-weight: 700;
  color: var(--err); text-align: center; margin-bottom: 14px;
}
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

/* ════════════════════════════════════
   АДАПТИВНАЯ ВЁРСТКА
════════════════════════════════════ */

/* ── Базовые улучшения для мобильных ── */
@media (max-width: 480px) {
  body { padding: 10px 8px 60px; }

  .container { max-width: 100%; }

  /* Шапка */
  header { margin-bottom: 12px; padding: 0; }
  .logo { font-size: 18px; gap: 6px; }
  .logo-icon { font-size: 22px; }
  .icon-btn { padding: 8px 10px; font-size: 14px; border-radius: 10px; }
  .header-actions { gap: 6px; }

  /* Карточки */
  .card { padding: 16px 14px; border-radius: 16px; margin-bottom: 12px; }

  /* Главное меню */
  .avatar { width: 52px; height: 52px; font-size: 24px; border-radius: 14px; }
  .menu-name { font-size: 17px; }
  .menu-stats { grid-template-columns: 1fr 1fr; gap: 8px; }
  .stat-card .s-val { font-size: 20px; }

  /* Сетка режимов */
  .mode-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
  .mode-card { padding: 14px 8px; font-size: 13px; border-radius: 12px; }
  .mode-card .mc-icon { font-size: 28px; margin-bottom: 6px; }
  .mode-card .mc-sub { font-size: 10px; }

  /* Спринт кнопки */
  .sprint-opts { gap: 6px; }
  .sprint-opt-btn { padding: 10px 14px; font-size: 16px; border-radius: 12px; }

  /* Игровой экран */
  .game-header { gap: 6px; flex-wrap: wrap; }
  .pause-btn { padding: 7px 10px; font-size: 12px; }
  .stats-bar { gap: 4px; }
  .mini-stat { padding: 5px 8px; font-size: 11px; border-radius: 8px; }

  /* Слово */
  .word-display { font-size: 26px; margin: 10px 0; min-height: 44px; gap: 8px; }

  /* Переключатель режима */
  .g-mode-selector { gap: 4px; }
  .g-mode-btn { padding: 8px 6px; font-size: 11px; }

  /* Инпут */
  input[type=text] { padding: 13px 14px; font-size: 16px; border-radius: 12px; }

  /* Варианты ответов */
  .options-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
  .option-btn { padding: 12px 8px; font-size: 13px; border-radius: 12px; }

  /* Кнопки действий */
  .actions { flex-direction: column; gap: 8px; }
  .actions button { width: 100%; flex: none; padding: 13px; font-size: 14px; }

  /* Флэшкарты */
  .flashcard-wrap { height: 160px; margin: 10px 0; }
  .fc-face { font-size: 22px; padding: 14px; }
  .fc-progress-row { gap: 6px; }
  .fc-prog-item { padding: 6px; font-size: 11px; border-radius: 8px; }
  .flashcard-actions { gap: 8px; }
  .fc-btn-know, .fc-btn-unknown { padding: 12px; font-size: 13px; border-radius: 12px; }

  /* Анаграмма */
  .letter-tile, .answer-tile { width: 36px; height: 42px; font-size: 16px; border-radius: 8px; }
  .anagram-word { font-size: 18px; }
  .anagram-letters, .anagram-answer { gap: 6px; }

  /* Результаты */
  .result-hero { font-size: 60px; }
  .result-title { font-size: 22px; }
  .result-pct { font-size: 44px; }
  .result-stats { grid-template-columns: 1fr 1fr; gap: 8px; }
  .result-stat { padding: 10px; }
  .result-stat strong { font-size: 20px; }

  /* Достижения/ошибки */
  .stats-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
  .stats-cell .sc-val { font-size: 20px; }
  .rank-levels { gap: 6px; }
  .rank-item { font-size: 12px; padding: 6px 10px; }

  /* Мои слова */
  .mw-add-row { flex-direction: column; gap: 8px; }
  .mw-add-row input { width: 100%; }
  .mw-add-btn { width: 100%; padding: 12px; }

  /* Произношение */
  .pronun-input-row { gap: 6px; }
  .pronun-speak-btn { padding: 0 14px; font-size: 20px; }
  .pronun-voice-row { flex-direction: column; align-items: flex-start; gap: 6px; }
  .pronun-voice-row select { width: 100%; }
  .pronun-speed-row { flex-wrap: wrap; }

  /* Модальное окно */
  .diff-modal { padding: 20px 16px; border-radius: 18px; }
  .diff-modal-title { font-size: 18px; }
  .diff-option { gap: 10px; padding: 12px 12px; border-radius: 12px; }
  .diff-option-icon { font-size: 22px; }
  .diff-option-name { font-size: 13px; }
  .diff-option-xp { font-size: 12px; padding: 3px 8px; }

  /* Попап достижения */
  .ach-pop { bottom: 16px; right: 12px; left: 12px; max-width: none; font-size: 12px; padding: 12px 14px; }

  /* Уведомление */
  .notif { font-size: 13px; padding: 10px 18px; white-space: normal; text-align: center; max-width: 90vw; }

  /* Пауза */
  .pause-card { padding: 20px 16px; border-radius: 18px; }
  .pause-title { font-size: 18px; }

  /* Серия-баннер */
  .streak-banner { font-size: 36px; }

  /* Дневная цель */
  .daily-box { padding: 12px 12px; }
  .daily-row { font-size: 12px; }
  .daily-complete-banner { font-size: 11px; padding: 5px 10px; }

  /* Табы */
  .tab { padding: 8px 6px; font-size: 12px; }
  li { padding: 8px 10px; font-size: 12px; }

  /* Таймер */
  .timer-box { font-size: 14px; }
}

/* ── Средние планшеты (481–768px) ── */
@media (min-width: 481px) and (max-width: 768px) {
  body { padding: 14px 16px 50px; }

  .container { max-width: 540px; }

  .logo { font-size: 20px; }
  .card { padding: 20px 18px; }

  .avatar { width: 58px; height: 58px; font-size: 27px; }
  .menu-name { font-size: 18px; }

  .mode-grid { grid-template-columns: 1fr 1fr; gap: 10px; }
  .mode-card { padding: 16px 12px; }

  .word-display { font-size: 30px; }

  .flashcard-wrap { height: 180px; }
  .fc-face { font-size: 24px; }

  .result-pct { font-size: 48px; }
  .result-hero { font-size: 70px; }

  .mw-add-row { flex-wrap: wrap; }
  .mw-add-btn { flex-shrink: 0; }

  .ach-pop { right: 14px; max-width: 300px; }
}

/* ── Большие планшеты (769–1024px) ── */
@media (min-width: 769px) and (max-width: 1024px) {
  body { padding: 20px 24px 50px; }
  .container { max-width: 580px; }
  .card { padding: 26px 22px; }
  .word-display { font-size: 36px; }
}

/* ── Десктоп (1025px+) ── */
@media (min-width: 1025px) {
  body { padding: 28px 24px 60px; }
  .container { max-width: 640px; }
  .card { padding: 28px 26px; }
  .word-display { font-size: 38px; }
  .mode-grid { grid-template-columns: 1fr 1fr; }
  .card:hover { box-shadow: 0 12px 40px rgba(0,0,0,0.25); }
}

/* ── Широкие экраны (1440px+) ── */
@media (min-width: 1440px) {
  .container { max-width: 680px; }
  body { padding: 36px 24px 70px; }
}

/* ── Ориентация: альбомная на маленьких устройствах ── */
@media (max-width: 768px) and (orientation: landscape) {
  body { padding: 8px 12px 30px; }
  .card { padding: 14px 16px; margin-bottom: 10px; }
  .menu-top { gap: 12px; }
  .avatar { width: 44px; height: 44px; font-size: 20px; }
  .flashcard-wrap { height: 140px; }
  .word-display { font-size: 24px; margin: 8px 0; min-height: 38px; }
  .timer-ring { width: 32px; height: 32px; }
  .mode-grid { grid-template-columns: repeat(4, 1fr); gap: 8px; }
  .mode-card .mc-icon { font-size: 22px; margin-bottom: 4px; }
  .result-hero { font-size: 48px; margin-bottom: 6px; }
  .result-pct { font-size: 36px; margin-bottom: 10px; }
}

/* ── Тёмная тема системы ── */
@media (prefers-color-scheme: light) {
  body:not(.light):not(.dark-forced) {
    /* Уважаем явный выбор пользователя */
  }
}

/* ── Уменьшение анимаций ── */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
  .bg-orb { animation: none !important; }
  .level-bar::after { animation: none !important; }
}

/* ── Тонкие экраны (SE, fold) ── */
@media (max-width: 360px) {
  body { padding: 8px 6px 50px; }
  .logo { font-size: 16px; }
  .logo-icon { font-size: 18px; }
  .icon-btn { padding: 7px 8px; font-size: 13px; }
  .card { padding: 14px 10px; border-radius: 14px; }
  .word-display { font-size: 22px; }
  input[type=text] { font-size: 14px; padding: 11px 10px; }
  .letter-tile, .answer-tile { width: 32px; height: 38px; font-size: 14px; }
  .mode-grid { gap: 6px; }
  .mode-card { padding: 12px 6px; font-size: 12px; }
  .mode-card .mc-icon { font-size: 24px; }
  .stat-card .s-val { font-size: 18px; }
  .stats-grid { grid-template-columns: 1fr 1fr; }
  .result-pct { font-size: 38px; }
  .result-title { font-size: 18px; }
  .diff-option { padding: 10px 10px; gap: 8px; }
  .diff-option-icon { font-size: 18px; }
}
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
      ETU by BV <span style="opacity:.5;font-weight:400;font-size:14px">v3</span>
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
      <div class="daily-header">
        <span class="daily-title">📅 Дневная цель</span>
        <button class="daily-change-btn" onclick="openDiffModal();event.stopPropagation();">⚙️ Сложность</button>
      </div>
      <div class="daily-difficulty-badge" id="dailyDiffBadge">🌱 Легко</div>
      <div class="daily-progress-row">
        <span class="daily-xp-done" id="dailyXpDone">0 XP набрано</span>
        <span class="daily-xp-goal" id="dailyGoalText">цель: 50 XP</span>
      </div>
      <div class="prog-wrap" style="margin-bottom:0;height:10px;">
        <div class="prog-bar" id="dailyGoalBar"></div>
      </div>
      <div class="daily-complete-banner" id="dailyCompleteBanner">🎉 Дневная цель выполнена! Отличная работа!</div>
    </div>

    <div class="quote-box" id="quoteBox">
      <em id="quoteText">Loading...</em>
    </div>

    <div class="menu-btns">
      <button class="btn-primary" onclick="showModeSelect()" style="font-size:17px;padding:16px;">🚀 Начать тренировку</button>
      <button class="btn-secondary" onclick="showScreen('pronounceScreen');initPronunScreen();">🗣️ Произношение</button>
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


  <!-- ═══ ПРОИЗНОШЕНИЕ ═══ -->
  <div id="pronounceScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:6px;">🗣️ Произношение</div>
    <div style="font-size:13px;color:var(--text2);margin-bottom:16px;">Введи слово, букву или фразу — и услышишь, как это звучит по-английски.</div>

    <div id="pronunNoSupport" class="pronun-no-support" style="display:none;">
      ⚠️ Твой браузер не поддерживает синтез речи. Попробуй Chrome или Edge.
    </div>

    <div class="pronun-input-row">
      <input type="text" id="pronunInput" placeholder="Введи слово или букву…" autocomplete="off" spellcheck="false" onkeydown="if(event.key==='Enter') pronounceWord()">
      <button class="pronun-speak-btn" id="pronunSpeakBtn" onclick="pronounceWord()" title="Произнести">🔊</button>
    </div>

    <div class="pronun-voice-row">
      <label>Голос</label>
      <select id="pronunVoice"></select>
    </div>

    <div class="pronun-speed-row">
      <label>Скорость</label>
      <input type="range" id="pronunSpeed" min="0.5" max="2" step="0.1" value="1" oninput="document.getElementById('pronunSpeedVal').textContent=parseFloat(this.value).toFixed(1)+'×'">
      <span class="pronun-speed-val" id="pronunSpeedVal">1.0×</span>
    </div>

    <div class="pronun-speed-row" style="margin-bottom:6px;">
      <label>Тональность</label>
      <input type="range" id="pronunPitch" min="0.5" max="2" step="0.1" value="1" oninput="document.getElementById('pronunPitchVal').textContent=parseFloat(this.value).toFixed(1)+'×'">
      <span class="pronun-speed-val" id="pronunPitchVal">1.0×</span>
    </div>

    <div class="divider"></div>
    <div class="section-title">История 🕐</div>
    <div class="pronun-history" id="pronunHistory">
      <div class="empty-state" id="pronunHistEmpty">Ещё ничего не произносилось.</div>
    </div>

    <button class="btn-secondary" style="width:100%;margin-top:14px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

</div><!-- /container -->

<!-- ═══ МОДАЛЬНОЕ ОКНО СЛОЖНОСТИ ═══ -->
<div class="diff-modal-overlay" id="diffModalOverlay" onclick="if(event.target===this)closeDiffModal()">
  <div class="diff-modal">
    <div class="diff-modal-title">🎯 Дневная цель</div>
    <div class="diff-modal-sub">Выбери нагрузку — XP сбрасываются каждый день</div>
    <div class="diff-options">
      <div class="diff-option diff-easy" id="diff-easy" onclick="selectDifficulty('easy')">
        <span class="diff-option-icon">🌱</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Лёгкая</div>
          <div class="diff-option-desc">Для начинающих и занятых людей</div>
        </div>
        <span class="diff-option-xp">30 XP</span>
      </div>
      <div class="diff-option diff-medium" id="diff-medium" onclick="selectDifficulty('medium')">
        <span class="diff-option-icon">⚡</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Обычная</div>
          <div class="diff-option-desc">Оптимальный ежедневный прогресс</div>
        </div>
        <span class="diff-option-xp">75 XP</span>
      </div>
      <div class="diff-option diff-hard" id="diff-hard" onclick="selectDifficulty('hard')">
        <span class="diff-option-icon">🔥</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Усиленная</div>
          <div class="diff-option-desc">Серьёзный подход к изучению</div>
        </div>
        <span class="diff-option-xp">150 XP</span>
      </div>
      <div class="diff-option diff-ultra" id="diff-ultra" onclick="selectDifficulty('ultra')">
        <span class="diff-option-icon">💀</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Ультра</div>
          <div class="diff-option-desc">Только для настоящих чемпионов</div>
        </div>
        <span class="diff-option-xp">300 XP</span>
      </div>
    </div>
    <button class="diff-modal-close" onclick="closeDiffModal()">Закрыть</button>
  </div>
</div>

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
        <option value="jobs">💼 Профессии (Jobs)</option>
        <option value="workplaces">🏭 Места работы</option>
        <option value="dailyRoutines">🌅 Распорядок дня</option>
        <option value="activities">📅 Занятия и дни недели</option>
        <option value="aroundTown">🏙️ По городу</option>
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
  // Theme 9 — Jobs
  jobs:[
    {ru:"уборщик / уборщица",en:"cleaner"},{ru:"водитель",en:"driver"},
    {ru:"продавец-консультант",en:"sales assistant"},{ru:"парикмахер",en:"hairdresser"},
    {ru:"шеф-повар",en:"chef"},{ru:"садовник",en:"gardener"},
    {ru:"ветеринар",en:"vet"},{ru:"актёр",en:"actor"},
    {ru:"врач",en:"doctor"},{ru:"медсестра / медбрат",en:"nurse"},
    {ru:"стоматолог",en:"dentist"},{ru:"полицейский",en:"police officer"},
    {ru:"пожарный",en:"firefighter"},{ru:"фермер",en:"farmer"},
    {ru:"строитель",en:"construction worker / builder"},{ru:"художник",en:"artist"},
    {ru:"администратор / секретарь",en:"receptionist"},{ru:"механик",en:"mechanic"},
    {ru:"инженер",en:"engineer"},{ru:"учёный",en:"scientist"},
    {ru:"учитель",en:"teacher"},{ru:"бизнесвумен",en:"businesswoman"},
    {ru:"бизнесмен",en:"businessman"},{ru:"официант",en:"waiter"},
    {ru:"официантка",en:"waitress"},{ru:"электрик",en:"electrician"},
    {ru:"пилот",en:"pilot"},{ru:"судья",en:"judge"}
  ],
  // Theme 10 — Workplaces / Work with
  workplaces:[
    {ru:"ферма",en:"farm"},{ru:"офис",en:"office"},
    {ru:"театр",en:"theater / theatre"},{ru:"школа",en:"school"},
    {ru:"лаборатория",en:"laboratory"},{ru:"ресторан",en:"restaurant"},
    {ru:"строительная площадка",en:"construction site"},{ru:"больница",en:"hospital"},
    {ru:"животные",en:"animals"},{ru:"дети",en:"children"},
    {ru:"пациенты",en:"patients"},{ru:"растения",en:"plants"},
    {ru:"еда",en:"food"},{ru:"люди",en:"people"}
  ],
  // Theme 11 — Daily Routines / Times of Day
  dailyRoutines:[
    {ru:"просыпаться",en:"wake up"},{ru:"вставать",en:"get up"},
    {ru:"принимать душ",en:"take a shower / have a shower"},
    {ru:"принимать ванну",en:"take a bath / have a bath"},
    {ru:"расчесывать волосы",en:"brush your hair"},
    {ru:"завтракать",en:"have breakfast / eat breakfast"},
    {ru:"идти на работу",en:"go to work"},{ru:"идти в школу",en:"go to school"},
    {ru:"покупать продукты",en:"buy groceries"},{ru:"идти домой",en:"go home"},
    {ru:"готовить ужин",en:"cook dinner"},{ru:"ужинать",en:"have dinner / eat dinner"},
    {ru:"гладить рубашку",en:"iron a shirt"},{ru:"одеваться",en:"get dressed"},
    {ru:"чистить зубы",en:"brush your teeth"},{ru:"умываться",en:"wash your face"},
    {ru:"начинать работу",en:"start work"},{ru:"обедать",en:"have lunch / eat lunch"},
    {ru:"заканчивать работу",en:"finish work"},{ru:"уходить с работы",en:"leave work"},
    {ru:"убирать со стола",en:"clear the table"},
    {ru:"мыть посуду",en:"do the dishes / wash the dishes"},
    {ru:"выгуливать собаку",en:"walk the dog"},{ru:"ложиться спать",en:"go to bed"},
    {ru:"день",en:"day"},{ru:"ночь",en:"night"},
    {ru:"рассвет",en:"dawn"},{ru:"утро",en:"morning"},
    {ru:"после обеда",en:"afternoon"},{ru:"сумерки",en:"dusk"},
    {ru:"вечер",en:"evening"},{ru:"поздний вечер",en:"late evening"}
  ],
  // Theme 14 — Activities / Days of the Week
  activities:[
    {ru:"понедельник",en:"Monday"},{ru:"вторник",en:"Tuesday"},
    {ru:"среда",en:"Wednesday"},{ru:"четверг",en:"Thursday"},
    {ru:"пятница",en:"Friday"},{ru:"суббота",en:"Saturday"},
    {ru:"воскресенье",en:"Sunday"},{ru:"выходные",en:"weekend"},
    {ru:"ходить в спортзал",en:"go to the gym"},
    {ru:"ходить плавать",en:"go swimming"},
    {ru:"играть в теннис",en:"play tennis"},
    {ru:"играть в футбол",en:"play soccer"},
    {ru:"читать газету",en:"read the newspaper"},
    {ru:"принимать ванну",en:"take a bath"}
  ],
  // Theme 20 — Around Town
  aroundTown:[
    {ru:"деревня",en:"village"},{ru:"небольшой город",en:"town"},
    {ru:"крупный город",en:"city"},{ru:"больница",en:"hospital"},
    {ru:"полицейский участок",en:"police station"},{ru:"автовокзал",en:"bus station"},
    {ru:"автобусная остановка",en:"bus stop"},{ru:"железнодорожный вокзал",en:"train station"},
    {ru:"аэропорт",en:"airport"},{ru:"школа",en:"school"},
    {ru:"фабрика / завод",en:"factory"},{ru:"супермаркет",en:"supermarket"},
    {ru:"магазин",en:"store / shop"},{ru:"аптека",en:"pharmacy"},
    {ru:"банк",en:"bank"},{ru:"почтовое отделение",en:"post office"},
    {ru:"библиотека",en:"library"},{ru:"музей",en:"museum"},
    {ru:"мэрия / городская администрация",en:"town hall"},{ru:"замок",en:"castle"},
    {ru:"офисное здание",en:"office building"},{ru:"парк",en:"park"},
    {ru:"здесь",en:"here"},{ru:"мост",en:"bridge"},
    {ru:"плавательный бассейн",en:"swimming pool"},{ru:"ресторан",en:"restaurant"},
    {ru:"кафе",en:"cafe"},{ru:"там",en:"there"},
    {ru:"бар",en:"bar"},{ru:"кинотеатр",en:"movie theater / cinema"},
    {ru:"театр",en:"theater / theatre"},{ru:"отель",en:"hotel"},
    {ru:"рядом",en:"near"},{ru:"церковь",en:"church"},
    {ru:"мечеть",en:"mosque"},{ru:"синагога",en:"synagogue"},
    {ru:"храм",en:"temple"},{ru:"далеко",en:"far"}
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

// ════════════════════════════════════
//  СИСТЕМА СЛОЖНОСТИ ДНЕВНОЙ ЦЕЛИ
// ════════════════════════════════════
const DAILY_GOALS = {
  easy:   { xp: 30,  name: "Лёгкая",   icon: "🌱", color: "var(--ok)",   bg: "rgba(34,216,143,.15)" },
  medium: { xp: 75,  name: "Обычная",  icon: "⚡",  color: "var(--accent)",bg: "rgba(79,142,255,.15)" },
  hard:   { xp: 150, name: "Усиленная",icon: "🔥",  color: "var(--warn)",  bg: "rgba(255,194,71,.15)" },
  ultra:  { xp: 300, name: "Ультра",   icon: "💀",  color: "var(--err)",   bg: "rgba(255,79,109,.15)" },
};
let dailyDifficulty = localStorage.getItem("etu_difficulty") || "easy";

function openDiffModal() {
  playClick();
  document.getElementById("diffModalOverlay").classList.add("open");
  // Подсветить текущий
  ['easy','medium','hard','ultra'].forEach(d => {
    document.getElementById("diff-"+d).classList.toggle("selected", d === dailyDifficulty);
  });
}
function closeDiffModal() {
  document.getElementById("diffModalOverlay").classList.remove("open");
}
function selectDifficulty(d) {
  dailyDifficulty = d;
  localStorage.setItem("etu_difficulty", d);
  ['easy','medium','hard','ultra'].forEach(k => {
    document.getElementById("diff-"+k).classList.toggle("selected", k === d);
  });
  playCorrect();
  updateMenu();
  setTimeout(closeDiffModal, 350);
  alertPop(`Цель: ${DAILY_GOALS[d].icon} ${DAILY_GOALS[d].name} — ${DAILY_GOALS[d].xp} XP`);
}

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
  document.getElementById("dailyGoalText").textContent = `цель: ${DAILY_GOALS[dailyDifficulty].xp} XP`;
  document.getElementById("dailyXpDone").textContent = `${dailyXp} XP набрано`;
  const pctDaily = Math.min(dailyXp / DAILY_GOALS[dailyDifficulty].xp * 100, 100);
  document.getElementById("dailyGoalBar").style.width = pctDaily + "%";
  const d = DAILY_GOALS[dailyDifficulty];
  const badge = document.getElementById("dailyDiffBadge");
  badge.textContent = `${d.icon} ${d.name}`;
  badge.style.background = d.bg; badge.style.color = d.color;
  const done = dailyXp >= DAILY_GOALS[dailyDifficulty].xp;
  document.getElementById("dailyCompleteBanner").style.display = done ? "block" : "none";
  document.getElementById("dailyGoalBar").style.background = done
    ? "linear-gradient(90deg, #22d88f, #4f8eff)"
    : "linear-gradient(90deg, #4f8eff, #a78bfa)";

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
  document.getElementById("stDailyXp").textContent = `${dailyXp} / ${DAILY_GOALS[dailyDifficulty].xp}`;
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
  const savedDiff = localStorage.getItem("etu_difficulty");
  const savedTheme = localStorage.getItem("etu_theme");
  localStorage.clear();
  if (savedDiff) localStorage.setItem("etu_difficulty", savedDiff);
  if (savedTheme) localStorage.setItem("etu_theme", savedTheme);
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
//  ПРОИЗНОШЕНИЕ
// ════════════════════════════════════
let pronunHistory = [];
let pronunVoicesLoaded = false;

function initPronunScreen() {
  if (!('speechSynthesis' in window)) {
    document.getElementById('pronunNoSupport').style.display = 'block';
    return;
  }
  loadPronunVoices();
  renderPronunHistory();
  setTimeout(() => document.getElementById('pronunInput').focus(), 200);
}

function loadPronunVoices() {
  const sel = document.getElementById('pronunVoice');
  const populate = () => {
    const voices = speechSynthesis.getVoices();
    const engVoices = voices.filter(v => v.lang.startsWith('en'));
    sel.innerHTML = '';
    if (engVoices.length === 0) {
      const opt = document.createElement('option');
      opt.textContent = 'По умолчанию'; opt.value = '';
      sel.appendChild(opt);
    } else {
      engVoices.forEach((v, i) => {
        const opt = document.createElement('option');
        opt.value = i;
        opt.textContent = `${v.name} (${v.lang})`;
        sel.appendChild(opt);
      });
    }
    pronunVoicesLoaded = true;
  };
  if (speechSynthesis.getVoices().length) { populate(); }
  else { speechSynthesis.onvoiceschanged = populate; }
}

function pronounceWord(text) {
  if (!('speechSynthesis' in window)) return;
  const input = text || document.getElementById('pronunInput').value.trim();
  if (!input) { alertPop('Введи слово или букву!'); return; }

  speechSynthesis.cancel();

  const utt = new SpeechSynthesisUtterance(input);
  const sel = document.getElementById('pronunVoice');
  const voices = speechSynthesis.getVoices().filter(v => v.lang.startsWith('en'));
  if (voices.length && sel.value !== '') {
    utt.voice = voices[parseInt(sel.value)] || voices[0];
  }
  utt.rate  = parseFloat(document.getElementById('pronunSpeed').value);
  utt.pitch = parseFloat(document.getElementById('pronunPitch').value);
  utt.lang  = 'en-US';

  const btn = document.getElementById('pronunSpeakBtn');
  btn.classList.add('speaking');
  utt.onend = () => btn.classList.remove('speaking');
  utt.onerror = () => btn.classList.remove('speaking');

  speechSynthesis.speak(utt);

  // сохранить в историю (без дублей подряд)
  if (!pronunHistory.length || pronunHistory[0] !== input) {
    pronunHistory.unshift(input);
    if (pronunHistory.length > 20) pronunHistory.pop();
    renderPronunHistory();
  }
}

function renderPronunHistory() {
  const wrap = document.getElementById('pronunHistory');
  const empty = document.getElementById('pronunHistEmpty');
  if (!pronunHistory.length) {
    empty.style.display = 'block';
    // remove old items
    wrap.querySelectorAll('.pronun-hist-item').forEach(el => el.remove());
    return;
  }
  empty.style.display = 'none';
  wrap.querySelectorAll('.pronun-hist-item').forEach(el => el.remove());
  pronunHistory.forEach(word => {
    const div = document.createElement('div');
    div.className = 'pronun-hist-item';
    div.innerHTML = `<span>${word}</span><span class="pronun-hist-repeat">🔊</span>`;
    div.onclick = () => {
      document.getElementById('pronunInput').value = word;
      pronounceWord(word);
    };
    wrap.appendChild(div);
  });
}


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
