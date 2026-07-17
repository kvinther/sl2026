<!DOCTYPE html>
<html lang="da">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SL2026 – Ugeskema, Svendborg Gruppe</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: system-ui, -apple-system, sans-serif;
      background: #f5f4f0;
      color: #1a1a18;
      padding: 24px 16px;
    }

    h1 {
      font-size: 20px;
      font-weight: 500;
      margin-bottom: 4px;
    }

    .subtitle {
      font-size: 13px;
      color: #666;
      margin-bottom: 20px;
    }

    .legend {
      display: flex;
      gap: 14px;
      flex-wrap: wrap;
      margin-bottom: 20px;
    }

    .leg {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 12px;
      color: #555;
    }

    .leg-dot {
      width: 12px;
      height: 12px;
      border-radius: 3px;
      flex-shrink: 0;
    }

    .scroll {
      overflow-x: auto;
    }

    #cal {
      display: grid;
      grid-template-columns: 52px repeat(9, minmax(85px, 1fr));
      min-width: 845px;
      position: relative;
      background: #fff;
      border-radius: 10px;
      border: 0.5px solid #ddd;
      overflow: hidden;
    }

    .hdr {
      font-size: 12px;
      font-weight: 500;
      color: #555;
      padding: 6px;
      text-align: center;
      border-bottom: 1px solid #ddd;
      background: #fff;
      position: sticky;
      top: 0;
      z-index: 10;
    }

    .time-cell {
      font-size: 11px;
      color: #aaa;
      text-align: right;
      padding-right: 6px;
      display: flex;
      align-items: flex-start;
      justify-content: flex-end;
      border-right: 0.5px solid #eee;
    }

    .bg-cell {
      border-right: 0.5px solid #eee;
      border-bottom: 0.5px solid #eee;
    }

    .event {
      position: absolute;
      left: 2px;
      right: 2px;
      border-radius: 5px;
      padding: 2px 5px;
      font-size: 10px;
      overflow: hidden;
      cursor: default;
      z-index: 5;
      line-height: 1.35;
    }

    .event:hover {
      z-index: 20;
      filter: brightness(0.93);
    }

    .etitle {
      font-weight: 600;
    }

    .etime {
      font-size: 9px;
      opacity: 0.8;
    }

    .c-all {
      background: #B5D4F4;
      color: #0C447C;
      border: 0.5px solid #85B7EB;
    }

    .c-klan {
      background: #9FE1CB;
      color: #085041;
      border: 0.5px solid #5DCAA5;
    }

    .c-trop {
      background: #FAC775;
      color: #633806;
      border: 0.5px solid #EF9F27;
    }

    .c-junior {
      background: #F4C0D1;
      color: #72243E;
      border: 0.5px solid #ED93B1;
    }

    .c-ulv {
      background: #C0DD97;
      color: #27500A;
      border: 0.5px solid #97C459;
    }

    .c-mix {
      background: #CECBF6;
      color: #3C3489;
      border: 0.5px solid #AFA9EC;
    }

    @media (prefers-color-scheme: dark) {
      body {
        background: #1a1a18;
        color: #e8e6e0;
      }

      #cal {
        background: #242422;
        border-color: #444;
      }

      .hdr {
        background: #242422;
        color: #aaa;
        border-color: #444;
      }

      .time-cell {
        color: #666;
        border-color: #333;
      }

      .bg-cell {
        border-color: #333;
      }

      .c-all {
        background: #0C447C;
        color: #B5D4F4;
        border-color: #185FA5;
      }

      .c-klan {
        background: #085041;
        color: #9FE1CB;
        border-color: #0F6E56;
      }

      .c-trop {
        background: #633806;
        color: #FAC775;
        border-color: #854F0B;
      }

      .c-junior {
        background: #72243E;
        color: #F4C0D1;
        border-color: #993556;
      }

      .c-ulv {
        background: #27500A;
        color: #C0DD97;
        border-color: #3B6D11;
      }

      .c-mix {
        background: #3C3489;
        color: #CECBF6;
        border-color: #534AB7;
      }

      .c-fri {
        background: #0f91ae;
        color: #CECBF6;
        border-color: #534AB7;
      }

      .leg {
        color: #aaa;
      }

      .subtitle {
        color: #888;
      }
    }
  </style>
</head>

<body>
  <h1>SL2026 – Ugeskema</h1>
  <p class="subtitle">KFUM Svendborg Gruppe 18.–26. juli 2026</p>

  <div class="legend">
    <div class="leg">
      <div class="leg-dot c-all"></div>Alle deltagere
    </div>
    <div class="leg">
      <div class="leg-dot c-klan"></div>Klan (Kristian/Alexander, Heidi, Louise, Lucas, Lærke, Michael og Rikke)
    </div>
    <div class="leg">
      <div class="leg-dot c-trop"></div>Trop (Asta, Dicte, Freja, Mille, Mira, Sofia og Tobias)
    </div>
    <div class="leg">
      <div class="leg-dot c-junior"></div>Junior (Anna Clara, Ida, Isabella, Jo, Siri, Vilma og Kristian)
    </div>
    <div class="leg">
      <div class="leg-dot c-ulv"></div>Ulv (Katrine, Ronja og Lucia)
    </div>
    <div class="leg">
      <div class="leg-dot c-mix"></div>Blandet
    </div>
  </div>

  <div class="scroll">
    <div id="cal"></div>
  </div>

  <script>
    const DAYS = ['Lør 18/7', 'Søn 19/7', 'Man 20/7', 'Tir 21/7', 'Ons 22/7', 'Tor 23/7', 'Fre 24/7', 'Lør 25/7', 'Søn 26/7'];
    const DAYS_NUM = DAYS.length;
    const START_H = 8, END_H = 23, SLOT_H = 48;
    const TOTAL = END_H - START_H;

    const events = [
      { day: 0, start: '8:00', end: '9:00', title: 'Afrejse fra SG Byen', loc: 'lejren', color: 'c-all' },
      { day: 0, start: '10:15', end: '11:00', title: 'Ankomst og gå til lejren', loc: 'lejren', color: 'c-all' },
      { day: 0, start: '11:00', end: '17:00', title: 'Lejretablering', loc: 'lejren', color: 'c-all' },
      { day: 1, start: '10:00', end: '11:00', title: 'Sustainable Foods (junior+trop)', loc: 'lejren', color: 'c-mix' },
      { day: 1, start: '12:00', end: '17:00', title: 'Udforskning af lejren (junior+trop)', loc: 'lejren', color: 'c-mix' },
      { day: 1, start: '19:30', end: '20:45', title: '🔥 Åbningslejrbål 🔥', loc: 'lejren', color: 'c-all' },
      { day: 2, start: '08:00', end: '', title: 'Klan afgang til Roskilde <b>8:50</b><br/>Hjemme igen 14:30', loc: 'roskilde', color: 'c-klan' },
      { day: 2, start: '09:00', end: '12:00', title: 'Internetspejd tal/chat med andre spejdere (junior+trop)', loc: 'lejren', color: 'c-mix' },
      { day: 2, start: '10:00', end: '11:30', title: 'Domkirken lofter', loc: 'roskilde', color: 'c-klan', offset: 1 },
      { day: 2, start: '12:00', end: '12:30', title: 'Bueskydningskamp', loc: 'roskilde', color: 'c-klan', offset: 1 },
      { day: 2, start: '15:00', end: '16:00', title: 'Ordonnansløb', loc: 'lejren', color: 'c-trop' },
      { day: 2, start: '16:10', end: '16:30', title: 'Friendship Award', loc: 'lejren', color: 'c-junior' },
      { day: 3, start: '09:00', end: '16:00', title: 'Underlejrdag – Skoven', loc: 'lejren', color: 'c-mix' },
      { day: 3, start: '13:00', end: '16:00', title: 'Sejltur – Skjoldungerne', loc: 'veddelev', color: 'c-junior', offset: 1 },
      { day: 3, start: '14:30', end: '15:15', title: 'Escaperoom', loc: 'lejren', color: 'c-trop', offset: 1 },
      { day: 4, start: '08:45', end: '12:00', title: 'Robinson', loc: 'lejren', color: 'c-trop' },
      { day: 4, start: '09:00', end: '12:00', title: 'Lorte-Løbet', loc: 'lejren', color: 'c-junior', offset: 1 },
      { day: 4, start: '11:00', end: '12:00', title: 'Siri skal døbes!', loc: 'lejren', color: 'c-all', offset: 1 },
      { day: 4, start: '13:00', end: '17:30', title: 'Besøgsdag', loc: 'lejren', color: 'c-all' },
      { day: 4, start: '13:30', end: '16:30', title: 'Fællesskabets Magi', loc: 'lejren', color: 'c-klan', offset: 1 },
      { day: 5, start: '10:30', end: '14:30', title: 'Naturforsker for en dag', loc: 'hoje-taastrup', color: 'c-ulv' },
      { day: 5, start: '13:00', end: '15:00', title: 'Svømmehallen 10+', loc: 'swimming-pool', color: 'c-mix', offset: 1 },
      { day: 5, start: '19:00', end: '22:00', title: 'GoNatløbet (junior+trop)', loc: 'lejren', color: 'c-mix' },
      { day: 6, start: '09:00', end: '13:00', title: 'Patruljekonkurrence 1946', loc: 'lejren', color: 'c-trop' },
      { day: 7, start: '09:30', end: '11:30', title: 'Flimmer 5-kamp', loc: 'lejren', color: 'c-ulv' },
      { day: 7, start: '19:00', end: '20:30', title: '🔥 Afslutningslejrbål 🔥', loc: 'lejren', color: 'c-all' },
      { day: 8, start: '09:00', end: '13:30', title: 'Nedpakning og gåtur til busterminalen', loc: 'lejren', color: 'c-all' },
      { day: 8, start: '14:00', end: '16:15', title: 'Hjemrejse med bus', loc: 'lejren', color: 'c-all' },
      /* Aftensmad: 17:30-18:30 */
      { day: 0, start: '17:30', end: '18:30', title: '🌮 Quesadillas', loc: 'lejren', color: 'c-all' },
      { day: 1, start: '17:30', end: '18:30', title: '🌮 Mac & Cheese med sommergrønt', loc: 'lejren', color: 'c-all' },
      { day: 2, start: '17:30', end: '18:30', title: '🌮 Bygotto', loc: 'lejren', color: 'c-all' },
      { day: 3, start: '17:30', end: '18:30', title: '🌮 Anicia Bolo', loc: 'lejren', color: 'c-all' },
      { day: 4, start: '17:30', end: '18:30', title: '🌮 Vegedeller med kartoffelsalat', loc: 'lejren', color: 'c-all' },
      { day: 5, start: '17:30', end: '18:30', title: '🌮 Chili Sin Carne', loc: 'lejren', color: 'c-all' },
      { day: 6, start: '17:30', end: '18:30', title: '🌮 Hallomiburger med gnavegrønt', loc: 'lejren', color: 'c-all' },
      { day: 7, start: '17:30', end: '18:30', title: '🌮 Rugbrød', loc: 'lejren', color: 'c-all' },
    ];

    function ts(t) {
      const [h, m] = t.split(':').map(Number);
      return (h - START_H) + m / 60;
    }

    const cal = document.getElementById('cal');

    const corner = document.createElement('div');
    corner.className = 'hdr';
    cal.appendChild(corner);
    DAYS.forEach((d, i) => {
      const h = document.createElement('div');
      h.className = 'hdr';
      h.style.gridColumn = i + 2;
      h.textContent = d;
      cal.appendChild(h);
    });

    for (let s = 0; s < TOTAL; s++) {
      const h = START_H + s;
      const tc = document.createElement('div');
      tc.className = 'time-cell';
      tc.style.cssText = `grid-column:1; grid-row:${s + 2}; height:${SLOT_H}px;`;
      tc.textContent = h + ':00';
      cal.appendChild(tc);
      for (let d = 0; d < DAYS_NUM; d++) {
        const cell = document.createElement('div');
        cell.className = 'bg-cell';
        cell.style.cssText = `grid-column:${d + 2}; grid-row:${s + 2}; height:${SLOT_H}px;`;
        cal.appendChild(cell);
      }
    }

    const containers = Array.from({ length: DAYS_NUM }, (_, d) => {
      const c = document.createElement('div');
      c.style.cssText = `grid-column:${d + 2}; grid-row:2/${TOTAL + 2}; position:relative; z-index:2; pointer-events:none; height:${TOTAL * SLOT_H}px;`;
      cal.appendChild(c);
      return c;
    });

    events.forEach(ev => {
      const top = ts(ev.start) * SLOT_H;
      const height = Math.max((ts(ev.end) - ts(ev.start)) * SLOT_H - 2, 30);
      const el = document.createElement('div');
      el.className = 'event ' + ev.color;
      el.style.top = top + 'px';
      el.style.height = height + 'px';
      if (ev.offset) { el.style.left = (2 + ev.offset * 30) + '%'; el.style.zIndex = 5 + ev.offset; }
      const dur = ts(ev.end) - ts(ev.start);
      el.innerHTML = `<div class="etitle">${ev.title}</div><div class="etime">${ev.start}–${ev.end}</div>`;
      el.title = `${ev.title}\n${ev.start}–${ev.end} @ ${ev.loc}`;
      containers[ev.day].appendChild(el);
    });
  </script>
</body>

</html>