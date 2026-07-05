<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>ปฏิทินของฉัน · Google Calendar</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,600;9..144,700&family=IBM+Plex+Sans+Thai:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --paper: #EDE7DA;
    --paper-dark: #E2DACB;
    --ink: #232320;
    --ink-soft: #55524A;
    --muted: #8B8577;
    --pine: #2F5233;
    --pine-soft: #3F6A45;
    --brick: #C0392B;
    --line: rgba(35,35,32,0.14);
    --radius: 18px;
  }

  * { box-sizing: border-box; }

  html, body {
    margin: 0;
    padding: 0;
    background: var(--paper-dark);
    color: var(--ink);
    font-family: 'IBM Plex Sans Thai', sans-serif;
    -webkit-font-smoothing: antialiased;
  }

  body {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    padding: 20px 14px 60px;
    background-image:
      radial-gradient(circle at 1px 1px, rgba(35,35,32,0.06) 1px, transparent 1px);
    background-size: 14px 14px;
  }

  .sheet {
    width: 100%;
    max-width: 760px;
  }

  /* ---------- Ring binding header ---------- */
  .binding {
    display: flex;
    justify-content: space-evenly;
    padding: 0 18px;
    margin-bottom: -14px;
    position: relative;
    z-index: 2;
  }
  .ring {
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background: var(--paper-dark);
    border: 3px solid var(--ink);
    box-shadow: inset 0 2px 3px rgba(0,0,0,0.35);
  }

  .card {
    background: var(--paper);
    border-radius: var(--radius);
    padding: 34px 22px 22px;
    box-shadow: 0 18px 40px rgba(35,35,32,0.18), 0 2px 0 rgba(35,35,32,0.06);
    position: relative;
    overflow: hidden;
  }
  .card::before{
    content:"";
    position:absolute;
    top:0; left:0; right:0;
    height: 8px;
    background: var(--pine);
  }

  /* ---------- Hero: torn month tab + big date ---------- */
  .hero {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    gap: 16px;
    padding: 14px 4px 22px;
    border-bottom: 2px dashed var(--line);
    margin-bottom: 20px;
  }

  .hero-left { display:flex; align-items:flex-end; gap:16px; }

  .day-badge {
    background: var(--ink);
    color: var(--paper);
    border-radius: 12px;
    padding: 6px 14px 10px;
    text-align: center;
    min-width: 74px;
    transform: rotate(-2deg);
    box-shadow: 3px 4px 0 rgba(0,0,0,0.15);
  }
  .day-badge .num {
    font-family: 'Fraunces', serif;
    font-weight: 700;
    font-size: 40px;
    line-height: 1;
    display:block;
  }
  .day-badge .weekday {
    font-size: 11px;
    letter-spacing: .12em;
    opacity: .8;
  }

  .month-title {
    font-family: 'IBM Plex Sans Thai', sans-serif;
    font-weight: 700;
    font-size: 22px;
    color: var(--ink);
    margin: 0 0 2px;
  }
  .month-title .eng {
    display:block;
    font-family: 'Fraunces', serif;
    font-style: italic;
    font-weight: 400;
    font-size: 14px;
    color: var(--muted);
    margin-top: 2px;
  }

  .gear-btn {
    border: 1.5px solid var(--ink);
    background: transparent;
    color: var(--ink);
    border-radius: 999px;
    width: 40px;
    height: 40px;
    font-size: 18px;
    cursor: pointer;
    flex-shrink: 0;
    transition: background .15s, color .15s, transform .15s;
  }
  .gear-btn:hover { background: var(--ink); color: var(--paper); }
  .gear-btn:active { transform: scale(0.92); }

  /* ---------- Calendar embed area ---------- */
  .cal-wrap {
    border-radius: 12px;
    overflow: hidden;
    border: 1px solid var(--line);
    background: #fff;
    position: relative;
  }
  .cal-scale-outer {
    width: 100%;
    overflow: hidden;
    position: relative;
  }
  .cal-scale-inner {
    transform-origin: top left;
  }
  iframe#gcal {
    display: block;
    border: 0;
  }

  .empty-state {
    padding: 46px 20px;
    text-align: center;
    color: var(--ink-soft);
  }
  .empty-state .pin {
    width: 34px; height: 34px;
    border-radius: 50%;
    background: var(--brick);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    font-size: 18px;
    margin-bottom: 14px;
    box-shadow: 0 3px 6px rgba(192,57,43,0.4);
  }
  .empty-state h3 {
    font-family: 'IBM Plex Sans Thai', sans-serif;
    margin: 0 0 8px;
    font-size: 17px;
    color: var(--ink);
  }
  .empty-state p { margin: 0 0 18px; font-size: 14px; line-height: 1.6; }
  .empty-state button {
    background: var(--pine);
    color: #fff;
    border: 0;
    padding: 11px 22px;
    border-radius: 999px;
    font-family: 'IBM Plex Sans Thai', sans-serif;
    font-weight: 600;
    font-size: 14px;
    cursor: pointer;
  }
  .empty-state button:active { transform: scale(0.97); }

  /* footer note strip */
  .note-strip {
    margin-top: 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 12.5px;
    color: var(--muted);
    padding: 0 4px;
  }
  .note-strip .dot { width:6px; height:6px; border-radius:50%; background: var(--pine-soft); flex-shrink:0; }

  details.howto {
    margin-top: 18px;
    border-top: 2px dashed var(--line);
    padding-top: 16px;
  }
  details.howto summary {
    cursor: pointer;
    font-weight: 600;
    font-size: 14.5px;
    color: var(--pine);
    list-style: none;
    display:flex;
    align-items:center;
    gap:8px;
  }
  details.howto summary::-webkit-details-marker{ display:none; }
  details.howto summary::before {
    content: "▸";
    transition: transform .15s;
  }
  details.howto[open] summary::before { transform: rotate(90deg); }
  .howto ol {
    margin: 14px 0 4px;
    padding-left: 20px;
    font-size: 13.5px;
    line-height: 1.9;
    color: var(--ink-soft);
  }
  .howto code {
    background: var(--paper-dark);
    padding: 2px 6px;
    border-radius: 5px;
    font-size: 12.5px;
  }

  /* ---------- Modal ---------- */
  .overlay {
    position: fixed; inset: 0;
    background: rgba(35,35,32,0.55);
    display: none;
    align-items: flex-end;
    justify-content: center;
    z-index: 50;
    padding: 0;
  }
  .overlay.open { display: flex; }
  @media (min-width: 640px) {
    .overlay { align-items: center; }
  }

  .modal {
    background: var(--paper);
    width: 100%;
    max-width: 480px;
    border-radius: 20px 20px 0 0;
    padding: 24px 22px 28px;
    box-shadow: 0 -10px 40px rgba(0,0,0,0.3);
  }
  @media (min-width: 640px) {
    .modal { border-radius: 18px; }
  }

  .modal h2 {
    font-family: 'IBM Plex Sans Thai', sans-serif;
    font-size: 18px;
    margin: 0 0 6px;
  }
  .modal p.sub {
    font-size: 13px;
    color: var(--muted);
    margin: 0 0 18px;
    line-height: 1.7;
  }
  .modal label {
    display:block;
    font-size: 13px;
    font-weight: 600;
    margin-bottom: 6px;
  }
  .modal input[type=text] {
    width: 100%;
    padding: 12px 14px;
    border-radius: 10px;
    border: 1.5px solid var(--line);
    font-size: 14px;
    font-family: 'IBM Plex Sans Thai', sans-serif;
    background: #fff;
    color: var(--ink);
  }
  .modal input[type=text]:focus {
    outline: none;
    border-color: var(--pine);
  }
  .field-row { margin-bottom: 16px; }
  .modal-actions {
    display: flex;
    gap: 10px;
    margin-top: 4px;
  }
  .modal-actions button {
    flex: 1;
    padding: 12px;
    border-radius: 10px;
    border: 0;
    font-family: 'IBM Plex Sans Thai', sans-serif;
    font-weight: 600;
    font-size: 14px;
    cursor: pointer;
  }
  .btn-save { background: var(--pine); color: #fff; }
  .btn-cancel { background: transparent; color: var(--ink-soft); border: 1.5px solid var(--line) !important; }
  .btn-save:active, .btn-cancel:active { transform: scale(0.98); }

  .toast {
    position: fixed;
    bottom: 20px; left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: var(--ink);
    color: var(--paper);
    padding: 12px 20px;
    border-radius: 999px;
    font-size: 13.5px;
    opacity: 0;
    pointer-events: none;
    transition: opacity .25s, transform .25s;
    z-index: 60;
  }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }
</style>
</head>
<body>

  <div class="sheet">
    <div class="binding">
      <div class="ring"></div><div class="ring"></div><div class="ring"></div>
      <div class="ring"></div><div class="ring"></div><div class="ring"></div>
    </div>

    <div class="card">
      <div class="hero">
        <div class="hero-left">
          <div class="day-badge">
            <span class="num" id="heroDay">--</span>
            <span class="weekday" id="heroWeekday">-</span>
          </div>
          <div>
            <p class="month-title" id="heroMonthTh">กำลังโหลด…</p>
            <span class="eng" id="heroMonthEn"></span>
          </div>
        </div>
        <button class="gear-btn" id="openSettings" title="ตั้งค่าปฏิทิน" aria-label="ตั้งค่าปฏิทิน">⚙</button>
      </div>

      <div class="cal-wrap" id="calWrap">
        <div class="empty-state" id="emptyState">
          <div class="pin">★</div>
          <h3>ยังไม่ได้เชื่อมปฏิทิน</h3>
          <p>ใส่อีเมล หรือ Calendar ID ของ Google Calendar<br>ที่ตั้งค่าแชร์แบบสาธารณะไว้แล้ว เพื่อแสดงผลที่นี่</p>
          <button id="emptyStateBtn">เชื่อมปฏิทินตอนนี้</button>
        </div>
        <div class="cal-scale-outer" id="calScaleOuter" style="display:none;">
          <div class="cal-scale-inner" id="calScaleInner">
            <iframe id="gcal" width="800" height="600" frameborder="0" scrolling="no"></iframe>
          </div>
        </div>
      </div>

      <div class="note-strip" id="noteStrip" style="display:none;">
        <span class="dot"></span>
        <span>ปัดซ้าย/ขวาภายในปฏิทินเพื่อเปลี่ยนเดือน · แตะกิจกรรมเพื่อดูรายละเอียด</span>
      </div>

      <details class="howto">
        <summary>วิธีหา Calendar ID จาก Google Calendar</summary>
        <ol>
          <li>เปิด <code>calendar.google.com</code> บนคอมพิวเตอร์</li>
          <li>เลือกปฏิทินในเมนูซ้าย แล้วกดจุดสามจุด → <code>การตั้งค่าและการแชร์</code></li>
          <li>เลื่อนไปที่หัวข้อ <code>สิทธิ์การเข้าถึงสำหรับกิจกรรมสาธารณะ</code> แล้วติ๊ก <code>ทำให้ปฏิทินนี้เผยแพร่ต่อสาธารณะ</code></li>
          <li>เลื่อนลงไปที่หัวข้อ <code>รวมปฏิทิน</code> จะเห็น <code>Calendar ID</code> (มักจะเป็นอีเมลของคุณ หรือรหัสลงท้ายด้วย <code>@group.calendar.google.com</code>)</li>
          <li>คัดลอก Calendar ID มาวางในช่องตั้งค่าของหน้านี้ได้เลย</li>
        </ol>
      </details>
    </div>
  </div>

  <div class="overlay" id="overlay">
    <div class="modal">
      <h2>ตั้งค่าปฏิทิน</h2>
      <p class="sub">ใส่ Calendar ID ของปฏิทิน Google ที่แชร์แบบสาธารณะไว้แล้ว ระบบจะจำค่านี้ไว้ในเบราว์เซอร์นี้</p>
      <div class="field-row">
        <label for="calIdInput">Calendar ID หรืออีเมล Google</label>
        <input type="text" id="calIdInput" placeholder="เช่น somchai@gmail.com">
      </div>
      <div class="modal-actions">
        <button class="btn-cancel" id="cancelBtn">ยกเลิก</button>
        <button class="btn-save" id="saveBtn">บันทึกและแสดงผล</button>
      </div>
    </div>
  </div>

  <div class="toast" id="toast"></div>

<script>
  const THAI_MONTHS = ["มกราคม","กุมภาพันธ์","มีนาคม","เมษายน","พฤษภาคม","มิถุนายน","กรกฎาคม","สิงหาคม","กันยายน","ตุลาคม","พฤศจิกายน","ธันวาคม"];
  const THAI_WEEKDAYS = ["อาทิตย์","จันทร์","อังคาร","พุธ","พฤหัสบดี","ศุกร์","เสาร์"];
  const ENG_MONTHS = ["January","February","March","April","May","June","July","August","September","October","November","December"];

  const STORAGE_KEY = "gcal_view_calendar_id";
  const DEFAULT_CALENDAR_ID = "fo.welladee@gmail.com";

  const els = {
    heroDay: document.getElementById('heroDay'),
    heroWeekday: document.getElementById('heroWeekday'),
    heroMonthTh: document.getElementById('heroMonthTh'),
    heroMonthEn: document.getElementById('heroMonthEn'),
    emptyState: document.getElementById('emptyState'),
    emptyStateBtn: document.getElementById('emptyStateBtn'),
    calScaleOuter: document.getElementById('calScaleOuter'),
    calScaleInner: document.getElementById('calScaleInner'),
    noteStrip: document.getElementById('noteStrip'),
    gcal: document.getElementById('gcal'),
    overlay: document.getElementById('overlay'),
    openSettings: document.getElementById('openSettings'),
    cancelBtn: document.getElementById('cancelBtn'),
    saveBtn: document.getElementById('saveBtn'),
    calIdInput: document.getElementById('calIdInput'),
    toast: document.getElementById('toast'),
  };

  function renderHero() {
    const now = new Date();
    els.heroDay.textContent = now.getDate();
    els.heroWeekday.textContent = THAI_WEEKDAYS[now.getDay()];
    els.heroMonthTh.textContent = `${THAI_MONTHS[now.getMonth()]} ${now.getFullYear() + 543}`;
    els.heroMonthEn.textContent = `${ENG_MONTHS[now.getMonth()]} ${now.getFullYear()}`;
  }

  function buildEmbedUrl(calId) {
    const params = new URLSearchParams({
      src: calId,
      ctz: "Asia/Bangkok",
      mode: "MONTH",
      showTitle: "0",
      showNav: "1",
      showPrint: "0",
      showTabs: "0",
      showCalendars: "0",
      showTz: "0",
    });
    return `https://calendar.google.com/calendar/embed?${params.toString()}`;
  }

  const BASE_W = 800;
  const BASE_H = 600;

  function fitScale() {
    const outer = els.calScaleOuter;
    if (outer.style.display === "none") return;
    const containerWidth = outer.parentElement.clientWidth;
    const scale = containerWidth / BASE_W;
    els.calScaleInner.style.transform = `scale(${scale})`;
    outer.style.height = `${BASE_H * scale}px`;
  }

  function showCalendar(calId) {
    els.gcal.src = buildEmbedUrl(calId);
    els.emptyState.style.display = "none";
    els.calScaleOuter.style.display = "block";
    els.noteStrip.style.display = "flex";
    requestAnimationFrame(fitScale);
  }

  function showEmpty() {
    els.emptyState.style.display = "block";
    els.calScaleOuter.style.display = "none";
    els.noteStrip.style.display = "none";
  }

  function openModal() {
    const saved = localStorage.getItem(STORAGE_KEY) || DEFAULT_CALENDAR_ID;
    els.calIdInput.value = saved;
    els.overlay.classList.add('open');
    setTimeout(() => els.calIdInput.focus(), 50);
  }
  function closeModal() { els.overlay.classList.remove('open'); }

  function showToast(msg) {
    els.toast.textContent = msg;
    els.toast.classList.add('show');
    setTimeout(() => els.toast.classList.remove('show'), 2200);
  }

  els.openSettings.addEventListener('click', openModal);
  els.emptyStateBtn.addEventListener('click', openModal);
  els.cancelBtn.addEventListener('click', closeModal);
  els.overlay.addEventListener('click', (e) => { if (e.target === els.overlay) closeModal(); });

  els.saveBtn.addEventListener('click', () => {
    const val = els.calIdInput.value.trim();
    if (!val) { showToast("กรุณาใส่ Calendar ID ก่อนบันทึก"); return; }
    localStorage.setItem(STORAGE_KEY, val);
    showCalendar(val);
    closeModal();
    showToast("เชื่อมปฏิทินเรียบร้อย");
  });

  window.addEventListener('resize', fitScale);

  renderHero();
  const saved = localStorage.getItem(STORAGE_KEY) || DEFAULT_CALENDAR_ID;
  showCalendar(saved);
</script>

</body>
</html>
