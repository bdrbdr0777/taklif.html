<!doctype html>
<html lang="uz">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Uchrashuv taklifi</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Gloock&family=Nunito+Sans:wght@400;600;700&display=swap">
<style>
  :root {
    --bg: #0b1030; --bg-2: #2a1a55;
    --glow-a: #ff7aa8; --glow-b: #ffcf6b; --glow-c: #4aa3ff;
    --fg: #fff6f8; --fg-soft: #d9d3f0;
    --ha: #ffc857; --ha-fg: #2b1a00;
    --panel: rgba(255,255,255,0.08); --line: rgba(255,255,255,0.22);
    --font-display: "Gloock", Georgia, "Times New Roman", serif;
    --font-body: "Nunito Sans", system-ui, -apple-system, "Segoe UI", sans-serif;
    color-scheme: dark;
  }
  html, body { height: 100%; }
  body {
    margin: 0; background: var(--bg); color: var(--fg);
    font-family: var(--font-body); font-size: 18px; line-height: 1.5;
    padding-block: 24px;
    padding-left: max(16px, env(safe-area-inset-left, 0px));
    padding-right: max(16px, env(safe-area-inset-right, 0px));
    box-sizing: border-box; overflow-x: hidden; overflow-y: auto; position: relative;
  }
  .sky {
    position: fixed; inset: 0; z-index: 0;
    background:
      radial-gradient(120% 80% at 50% 110%, var(--bg-2) 0%, transparent 60%),
      linear-gradient(180deg, #060a22 0%, var(--bg) 45%, #3a2169 100%);
  }
  .blob { position: absolute; border-radius: 50%; filter: blur(70px); opacity: 0.5; animation: drift 18s ease-in-out infinite alternate; }
  .blob.a { width: 46vmax; height: 46vmax; left: -12vmax; top: -10vmax; background: var(--glow-a); }
  .blob.b { width: 40vmax; height: 40vmax; right: -14vmax; bottom: -8vmax; background: var(--glow-c); animation-duration: 22s; }
  .blob.c { width: 28vmax; height: 28vmax; left: 38%; bottom: -12vmax; background: var(--glow-b); opacity: 0.35; animation-duration: 26s; }
  @keyframes drift { from { transform: translate3d(0,0,0) scale(1); } to { transform: translate3d(5vmax,4vmax,0) scale(1.12); } }
  .floaters { position: fixed; inset: 0; z-index: 1; pointer-events: none; overflow: hidden; }
  .floater { position: absolute; bottom: -40px; opacity: 0; animation: rise linear infinite; will-change: transform, opacity; }
  @keyframes rise {
    0% { transform: translate3d(0,0,0) rotate(0deg); opacity: 0; }
    10% { opacity: 0.7; }
    90% { opacity: 0.5; }
    100% { transform: translate3d(var(--sway, 30px), -110vh, 0) rotate(40deg); opacity: 0; }
  }

  main { position: relative; z-index: 2; min-height: 100%; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; }
  .step { display: none; flex-direction: column; align-items: center; gap: 28px; width: 100%; max-width: 560px; min-width: 0; }
  body[data-step="ask"] .step-ask,
  body[data-step="pick"] .step-pick,
  body[data-step="done"] .step-done { display: flex; }

  .eyebrow { margin: 0; font-size: 0.8rem; letter-spacing: 0.22em; text-transform: uppercase; color: var(--fg-soft); }
  h1, h2 { margin: 0; font-family: var(--font-display); font-weight: 400; line-height: 1.06; text-wrap: balance; text-shadow: 0 6px 40px rgba(255,122,168,0.45); }
  h1 { font-size: clamp(2.5rem, 10.5vw, 5.2rem); max-width: 13ch; }
  h2 { font-size: clamp(2rem, 8vw, 3.6rem); max-width: 16ch; }
  .sub { margin: 0; max-width: 34ch; color: var(--fg-soft); text-wrap: balance; min-height: 1.5em; }

  .choices { display: flex; align-items: center; justify-content: center; gap: 20px; flex-wrap: wrap; }
  button { font: inherit; font-weight: 700; border: 0; border-radius: 999px; cursor: pointer; padding: 14px 40px; min-width: 120px; transition: transform 0.25s ease, box-shadow 0.25s ease, left 0.18s ease-out, top 0.18s ease-out; }
  button:focus-visible, input:focus-visible { outline: 3px solid #fff; outline-offset: 3px; }

  .primary { background: var(--ha); color: var(--ha-fg); font-size: 1.1rem; box-shadow: 0 10px 30px rgba(255,200,87,0.45); }
  .primary:hover { box-shadow: 0 14px 40px rgba(255,200,87,0.7); }
  #ha { transform: scale(var(--grow, 1)); }
  .ghost { background: transparent; color: var(--fg); border: 1px solid var(--line); }
  #yoq { background: rgba(255,255,255,0.14); color: var(--fg); border: 1px solid rgba(255,255,255,0.28); backdrop-filter: blur(6px); touch-action: none; z-index: 5; }
  #yoq.fled { position: fixed; }
  #yoq-slot { display: inline-block; }

  .form { width: 100%; display: flex; flex-direction: column; gap: 26px; text-align: left; }
  .field { display: flex; flex-direction: column; gap: 10px; min-width: 0; }
  label { font-size: 0.78rem; letter-spacing: 0.18em; text-transform: uppercase; color: var(--fg-soft); }
  input[type="date"], input[type="time"], input[type="text"] {
    font: inherit; color: var(--fg); background: var(--panel); border: 1px solid var(--line);
    border-radius: 14px; padding: 12px 14px; min-height: 48px; width: 100%; box-sizing: border-box; min-width: 0;
    -webkit-appearance: none; appearance: none;
  }
  input::placeholder { color: rgba(217,211,240,0.6); }
  .chips { display: flex; flex-wrap: wrap; gap: 8px; }
  .chip { min-width: 0; padding: 9px 15px; font-weight: 600; font-size: 0.95rem; background: var(--panel); color: var(--fg); border: 1px solid var(--line); }
  .chip.on { background: var(--ha); color: var(--ha-fg); border-color: var(--ha); }
  .hint { margin: 0; min-height: 1.5em; color: #ffd3de; text-align: center; font-size: 0.95rem; }
  .form .primary { align-self: center; }

  .summary { margin: 0; width: 100%; display: flex; flex-direction: column; gap: 10px; }
  .summary div { display: flex; justify-content: space-between; align-items: baseline; gap: 16px; padding: 14px 18px; background: var(--panel); border: 1px solid var(--line); border-radius: 16px; text-align: left; }
  .summary dt { font-size: 0.78rem; letter-spacing: 0.18em; text-transform: uppercase; color: var(--fg-soft); flex: none; }
  .summary dd { margin: 0; font-weight: 700; text-align: right; overflow-wrap: anywhere; min-width: 0; }

  .burst { position: fixed; z-index: 4; pointer-events: none; animation: pop 1.6s ease-out forwards; }
  @keyframes pop {
    0% { transform: translate3d(0,0,0) scale(0.4); opacity: 1; }
    100% { transform: translate3d(var(--dx), var(--dy), 0) scale(1.2) rotate(var(--rot, 0deg)); opacity: 0; }
  }
  @media (prefers-reduced-motion: reduce) {
    .blob, .floater { animation: none; }
    .floater { display: none; }
    button { transition: none; }
  }
</style>
</head>
<body data-step="ask">
<div class="sky" aria-hidden="true">
  <div class="blob a"></div>
  <div class="blob b"></div>
  <div class="blob c"></div>
</div>
<div class="floaters" id="floaters" aria-hidden="true"></div>

<main>
  <section class="step step-ask">
    <p class="eyebrow">Марьям, sendan bitta savolim bor</p>
    <h1>Men bilan uchrashuvga chiqasanmi?</h1>
    <p class="sub" id="sub">Iltimos, to'g'ri javob ber.</p>
    <div class="choices">
      <button id="ha" class="primary" type="button">Ha</button>
      <span id="yoq-slot"><button id="yoq" type="button" tabindex="-1">Yo'q</button></span>
    </div>
  </section>

  <section class="step step-pick">
    <p class="eyebrow">Ajoyib, rahmat</p>
    <h2>Qachon va qayerga borishni o'zing tanla</h2>
    <form id="form" class="form" novalidate>
      <div class="field">
        <label for="date">Qaysi kuni</label>
        <div class="chips" id="dayChips"></div>
        <input type="date" id="date">
      </div>
      <div class="field">
        <label for="time">Soat nechida</label>
        <div class="chips" id="timeChips"></div>
        <input type="time" id="time">
      </div>
      <div class="field">
        <label for="place">Qayerga borasan</label>
        <div class="chips" id="placeChips"></div>
        <input type="text" id="place" placeholder="Yoki o'zing yoz" maxlength="60" autocomplete="off">
      </div>
      <p class="hint" id="hint" aria-live="polite"></p>
      <button type="submit" class="primary">Tayyor</button>
    </form>
  </section>

  <section class="step step-done">
    <p class="eyebrow">Kelishdik</p>
    <h2>Uchrashuv tayyor, Марьям!</h2>
    <dl class="summary">
      <div><dt>Kun</dt><dd id="sDay"></dd></div>
      <div><dt>Soat</dt><dd id="sTime"></dd></div>
      <div><dt>Joy</dt><dd id="sPlace"></dd></div>
    </dl>
    <p class="sub">Tanlovingni menga yuborib qo'y.</p>
    <div class="choices">
      <button id="send" class="primary" type="button">Javobni yuborish</button>
      <button id="edit" class="ghost" type="button">O'zgartirish</button>
    </div>
    <p class="sub" id="sendMsg" aria-live="polite"></p>
  </section>
</main>

<script>
(function () {
  var heart = '<svg width="100%" height="100%" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/></svg>';
  var $ = function (id) { return document.getElementById(id); };
  var ha = $('ha'), yoq = $('yoq'), slot = $('yoq-slot'), sub = $('sub');
  var dateEl = $('date'), timeEl = $('time'), placeEl = $('place');

  var WEEK = ['Yakshanba', 'Dushanba', 'Seshanba', 'Chorshanba', 'Payshanba', 'Juma', 'Shanba'];
  var MONTH = ['yanvar', 'fevral', 'mart', 'aprel', 'may', 'iyun', 'iyul', 'avgust', 'sentabr', 'oktabr', 'noyabr', 'dekabr'];

  function pad(n) { return (n < 10 ? '0' : '') + n; }
  function iso(d) { return d.getFullYear() + '-' + pad(d.getMonth() + 1) + '-' + pad(d.getDate()); }
  function parseIso(s) { var p = s.split('-'); return new Date(+p[0], +p[1] - 1, +p[2]); }
  function longDate(s) { var d = parseIso(s); return WEEK[d.getDay()] + ', ' + d.getDate() + '-' + MONTH[d.getMonth()]; }

  function setStep(name) {
    document.body.setAttribute('data-step', name);
    window.scrollTo(0, 0);
    document.body.scrollTop = 0;
  }

  /* ---------- "Yo'q" tugmasi qochadi ---------- */
  var lines = [
    "Iltimos, to'g'ri javob ber.",
    "Bu tugma qiyin ekan, a?",
    "Yana urinib ko'rasanmi?",
    "Ushlab bo'lmaydi.",
    "Ha tugmasi ancha qulay ko'rinadi.",
    "Faqat bitta tugma bosiladi."
  ];
  var dodges = 0;

  function rect(el) { return el.getBoundingClientRect(); }

  function flee(px, py) {
    if (!yoq.classList.contains('fled')) {
      var r = rect(yoq);
      slot.style.width = r.width + 'px';
      slot.style.height = r.height + 'px';
      yoq.classList.add('fled');
    }
    var w = yoq.offsetWidth, h = yoq.offsetHeight, m = 16;
    var maxX = Math.max(m, window.innerWidth - w - m);
    var maxY = Math.max(m, window.innerHeight - h - m);
    var haR = rect(ha), best = null, bestScore = -1;
    for (var i = 0; i < 24; i++) {
      var x = m + Math.random() * (maxX - m);
      var y = m + Math.random() * (maxY - m);
      var d = (px == null) ? 200 : Math.hypot(x + w / 2 - px, y + h / 2 - py);
      var overlap = !(x + w < haR.left - 12 || x > haR.right + 12 || y + h < haR.top - 12 || y > haR.bottom + 12);
      if (overlap) d *= 0.1;
      if (d > bestScore) { bestScore = d; best = { x: x, y: y }; }
    }
    yoq.style.left = best.x + 'px';
    yoq.style.top = best.y + 'px';
    dodges++;
    sub.textContent = lines[dodges % lines.length];
    ha.style.setProperty('--grow', Math.min(1 + dodges * 0.06, 1.6));
  }

  function onApproach(e) {
    var p = e.touches && e.touches[0] ? e.touches[0] : e;
    flee(p.clientX, p.clientY);
  }

  yoq.addEventListener('pointerenter', onApproach);
  yoq.addEventListener('pointerdown', function (e) { e.preventDefault(); onApproach(e); });
  yoq.addEventListener('touchstart', function (e) { e.preventDefault(); onApproach(e); }, { passive: false });
  yoq.addEventListener('mousedown', function (e) { e.preventDefault(); onApproach(e); });
  yoq.addEventListener('focus', function () { flee(null, null); yoq.blur(); });
  yoq.addEventListener('click', function (e) { e.preventDefault(); e.stopPropagation(); onApproach(e); });

  document.addEventListener('pointermove', function (e) {
    if (document.body.getAttribute('data-step') !== 'ask') return;
    var r = rect(yoq);
    if (!yoq.classList.contains('fled')) {
      var near = e.clientX > r.left - 40 && e.clientX < r.right + 40 && e.clientY > r.top - 40 && e.clientY < r.bottom + 40;
      if (near) flee(e.clientX, e.clientY);
      return;
    }
    if (Math.hypot(e.clientX - (r.left + r.width / 2), e.clientY - (r.top + r.height / 2)) < 90) flee(e.clientX, e.clientY);
  });

  window.addEventListener('resize', function () {
    if (!yoq.classList.contains('fled')) return;
    var r = rect(yoq);
    yoq.style.left = Math.min(r.left, Math.max(16, window.innerWidth - r.width - 16)) + 'px';
    yoq.style.top = Math.min(r.top, Math.max(16, window.innerHeight - r.height - 16)) + 'px';
  });

  /* ---------- yurakchalar ---------- */
  var reduce = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  if (!reduce) {
    var floaters = $('floaters');
    for (var i = 0; i < 16; i++) {
      var f = document.createElement('div');
      f.className = 'floater';
      var s = 12 + Math.random() * 22;
      f.style.width = s + 'px';
      f.style.height = s + 'px';
      f.style.left = (Math.random() * 100) + '%';
      f.style.animationDuration = (12 + Math.random() * 14) + 's';
      f.style.animationDelay = (-Math.random() * 20) + 's';
      f.style.setProperty('--sway', (Math.random() * 80 - 40) + 'px');
      f.style.color = Math.random() > 0.5 ? '#ff7aa8' : '#ffcf6b';
      f.innerHTML = heart;
      floaters.appendChild(f);
    }
  }

  function burst() {
    var cx = window.innerWidth / 2, cy = window.innerHeight / 2;
    var colors = ['#ff7aa8', '#ffcf6b', '#ffffff', '#ff9ac1'];
    for (var i = 0; i < 36; i++) {
      var b = document.createElement('div');
      b.className = 'burst';
      var s = 14 + Math.random() * 26;
      var ang = Math.random() * Math.PI * 2;
      var dist = 120 + Math.random() * Math.min(window.innerWidth, 500) * 0.6;
      b.style.width = s + 'px';
      b.style.height = s + 'px';
      b.style.left = cx + 'px';
      b.style.top = cy + 'px';
      b.style.setProperty('--dx', Math.cos(ang) * dist + 'px');
      b.style.setProperty('--dy', Math.sin(ang) * dist + 'px');
      b.style.setProperty('--rot', (Math.random() * 90 - 45) + 'deg');
      b.style.color = colors[i % 4];
      b.innerHTML = heart;
      document.body.appendChild(b);
      (function (el) { setTimeout(function () { el.remove(); }, 1800); })(b);
    }
  }

  /* ---------- tanlash ---------- */
  var today = new Date();
  today.setHours(0, 0, 0, 0);
  dateEl.min = iso(today);

  function makeChips(containerId, items, onPick) {
    var box = $(containerId);
    items.forEach(function (it) {
      var b = document.createElement('button');
      b.type = 'button';
      b.className = 'chip';
      b.textContent = it.label;
      b.setAttribute('data-value', it.value);
      b.addEventListener('click', function () { onPick(it.value); sync(); });
      box.appendChild(b);
    });
  }

  var days = [];
  for (var k = 1; k <= 5; k++) {
    var dd = new Date(today.getFullYear(), today.getMonth(), today.getDate() + k);
    var name = k === 1 ? 'Ertaga' : (k === 2 ? 'Indin' : WEEK[dd.getDay()]);
    days.push({ value: iso(dd), label: name + ' · ' + dd.getDate() + '-' + MONTH[dd.getMonth()] });
  }
  makeChips('dayChips', days, function (v) { dateEl.value = v; });

  var times = ['11:00', '13:00', '15:00', '17:00', '19:00', '20:00'].map(function (t) { return { value: t, label: t }; });
  makeChips('timeChips', times, function (v) { timeEl.value = v; });

  var places = ['Kafe', 'Restoran', 'Park', 'Kino', 'Sohil', 'Shirinliklar', 'Sayr qilish'].map(function (p) { return { value: p, label: p }; });
  makeChips('placeChips', places, function (v) { placeEl.value = v; });

  function sync() {
    var pairs = [['dayChips', dateEl.value], ['timeChips', timeEl.value], ['placeChips', placeEl.value.trim()]];
    pairs.forEach(function (p) {
      var chips = $(p[0]).children;
      for (var i = 0; i < chips.length; i++) {
        chips[i].classList.toggle('on', chips[i].getAttribute('data-value') === p[1]);
      }
    });
  }
  [dateEl, timeEl, placeEl].forEach(function (el) {
    el.addEventListener('input', sync);
    el.addEventListener('change', sync);
  });

  ha.addEventListener('click', function () {
    yoq.hidden = true;
    setStep('pick');
    burst();
  });

  var choice = { day: '', time: '', place: '' };

  $('form').addEventListener('submit', function (e) {
    e.preventDefault();
    var missing = [];
    if (!dateEl.value) missing.push('kunni');
    if (!timeEl.value) missing.push('soatni');
    if (!placeEl.value.trim()) missing.push('joyni');
    var hint = $('hint');
    if (missing.length) {
      hint.textContent = 'Avval ' + missing.join(', ') + ' tanla.';
      return;
    }
    hint.textContent = '';
    choice.day = longDate(dateEl.value);
    choice.time = timeEl.value;
    choice.place = placeEl.value.trim();
    $('sDay').textContent = choice.day;
    $('sTime').textContent = choice.time;
    $('sPlace').textContent = choice.place;
    $('sendMsg').textContent = '';
    setStep('done');
    burst();
  });

  $('edit').addEventListener('click', function () { setStep('pick'); });

  function message() {
    return 'Salom! Men uchrashuvga roziman. Kun: ' + choice.day + '. Soat: ' + choice.time + '. Joy: ' + choice.place + '.';
  }

  function copyText(text) {
    var msg = $('sendMsg');
    function fallback() {
      var ta = document.createElement('textarea');
      ta.value = text;
      ta.style.position = 'fixed';
      ta.style.opacity = '0';
      document.body.appendChild(ta);
      ta.focus();
      ta.select();
      var ok = false;
      try { ok = document.execCommand('copy'); } catch (err) { ok = false; }
      ta.remove();
      msg.textContent = ok ? 'Nusxalandi. Endi menga Telegramda yuborib qo\'y.' : 'Nusxalab bo\'lmadi. Tanlovingni o\'zing yozib yubor.';
    }
    if (navigator.clipboard && navigator.clipboard.writeText) {
      navigator.clipboard.writeText(text).then(function () {
        msg.textContent = 'Nusxalandi. Endi menga Telegramda yuborib qo\'y.';
      }, fallback);
    } else {
      fallback();
    }
  }

  $('send').addEventListener('click', function () {
    var text = message();
    if (navigator.share) {
      navigator.share({ text: text }).catch(function () {});
    } else {
      copyText(text);
    }
  });
})();
</script>
</body>
</html>
