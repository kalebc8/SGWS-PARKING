# SGWS-PARKING
[index.html](https://github.com/user-attachments/files/28071410/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SGWS Lakeland — Driver Parking</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600&family=DM+Mono:wght@500&display=swap');
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'DM Sans', system-ui, sans-serif; background: #f0ede6; min-height: 100vh; }
  header { background: #1a1a2e; color: white; padding: 14px 20px; }
  header .logo { font-size: 11px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: #a0aec0; }
  header h1 { font-size: 16px; font-weight: 600; }
  .container { max-width: 720px; margin: 0 auto; padding: 20px 16px; }

  /* TABS */
  .tab-bar { display: flex; gap: 8px; margin-bottom: 1.25rem; flex-wrap: wrap; }
  .tab { padding: 8px 16px; border: 1.5px solid #cbd5e0; border-radius: 8px; background: white; color: #4a5568; cursor: pointer; font-size: 13px; font-weight: 500; transition: all .15s; }
  .tab.active { background: #1a1a2e; color: white; border-color: #1a1a2e; }
  .panel { display: none; }
  .panel.active { display: block; }

  /* SEARCH */
  .search-box { background: white; border-radius: 12px; padding: 16px; margin-bottom: 16px; box-shadow: 0 1px 4px rgba(0,0,0,0.08); }
  .search-box label { font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: #718096; display: block; margin-bottom: 8px; }
  .search-row { display: flex; gap: 8px; }
  .search-row input { flex: 1; padding: 10px 14px; border: 1.5px solid #e2e8f0; border-radius: 8px; font-size: 18px; font-family: 'DM Mono', monospace; font-weight: 500; outline: none; transition: border .15s; }
  .search-row input:focus { border-color: #4299e1; }
  .search-row button { padding: 10px 18px; background: #1a1a2e; color: white; border: none; border-radius: 8px; font-size: 14px; font-weight: 500; cursor: pointer; }
  .result-box { border-radius: 12px; overflow: hidden; margin-bottom: 16px; box-shadow: 0 1px 4px rgba(0,0,0,0.1); display: none; }
  .result-header { padding: 16px 20px; color: white; display: flex; align-items: center; gap: 14px; }
  .result-header .sec-circle { width: 52px; height: 52px; border-radius: 50%; background: rgba(255,255,255,0.2); display: flex; align-items: center; justify-content: center; font-size: 20px; font-weight: 700; flex-shrink: 0; }
  .result-header .info h2 { font-size: 22px; font-weight: 700; }
  .result-header .info p { font-size: 13px; opacity: 0.85; margin-top: 2px; }
  .result-body { background: white; padding: 16px 20px; }
  .result-body h3 { font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: #718096; margin-bottom: 6px; }
  .result-body p { font-size: 14px; color: #2d3748; line-height: 1.5; }
  .not-found { background: white; border-radius: 12px; padding: 20px; text-align: center; color: #718096; font-size: 14px; margin-bottom: 16px; display: none; }

  /* MAP */
  .map-card { background: white; border-radius: 12px; overflow: hidden; box-shadow: 0 1px 4px rgba(0,0,0,0.08); margin-bottom: 16px; }
  .map-card-header { padding: 12px 16px; background: #1a1a2e; color: white; font-size: 12px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; }
  .map-card svg { display: block; width: 100%; }
  .legend { display: flex; flex-wrap: wrap; gap: 8px; padding: 12px 16px; border-top: 1px solid #f0ede6; }
  .legend-item { display: flex; align-items: center; gap: 5px; font-size: 11px; padding: 4px 8px; border-radius: 20px; border: 1.5px solid; cursor: pointer; }
  .legend-dot { width: 8px; height: 8px; border-radius: 50%; }
  .zone { cursor: pointer; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.55} }
  .highlight-pulse { animation: pulse 1.2s ease-in-out 3; }

  /* QR CODES */
  .qr-controls { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 16px; align-items: center; }
  .qr-controls select { padding: 8px 12px; border: 1.5px solid #e2e8f0; border-radius: 8px; font-size: 13px; background: white; outline: none; }
  .qr-controls button { padding: 8px 16px; background: #1a1a2e; color: white; border: none; border-radius: 8px; font-size: 13px; font-weight: 500; cursor: pointer; }
  .qr-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 14px; }
  .qr-card {
    background: white;
    border-radius: 10px;
    padding: 14px 12px 10px;
    text-align: center;
    border: 4px solid #ccc;
    box-shadow: 0 1px 4px rgba(0,0,0,0.08);
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .qr-card .truck-label { font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; margin-bottom: 2px; }
  .qr-card .truck-num { font-size: 26px; font-weight: 700; font-family: 'DM Mono', monospace; line-height: 1; margin-bottom: 8px; }
  .qr-card .qr-img { margin-bottom: 8px; }
  .qr-card .qr-img canvas, .qr-card .qr-img img { border-radius: 4px; }
  .qr-card .sec-label { font-size: 11px; font-weight: 600; padding: 3px 10px; border-radius: 20px; color: white; }
  .qr-note { font-size: 12px; color: #718096; margin-bottom: 12px; }

  .info-strip { background: white; border-radius: 12px; padding: 12px 16px; font-size: 12px; color: #718096; line-height: 1.6; box-shadow: 0 1px 4px rgba(0,0,0,0.06); margin-top: 16px; }

  @media print {
    header, .tab-bar, .qr-controls, .info-strip { display: none !important; }
    body { background: white; }
    .container { padding: 0; max-width: 100%; }
    .qr-grid { grid-template-columns: repeat(4, 1fr); gap: 10px; }
    .panel { display: block !important; }
    #panel-lookup, #panel-map { display: none !important; }
  }
</style>
</head>
<body>
<header>
  <div class="logo">Southern Glazer's Wine & Spirits</div>
  <h1>Lakeland Driver Parking</h1>
</header>
<div class="container">

  <div class="tab-bar">
    <button class="tab active" onclick="showTab('lookup',this)">🔍 Truck Lookup</button>
    <button class="tab" onclick="showTab('map',this)">🗺 Yard Map</button>
    <button class="tab" onclick="showTab('qr',this)">📱 QR Codes</button>
  </div>

  <!-- ── LOOKUP ── -->
  <div class="panel active" id="panel-lookup">
    <div class="search-box">
      <label>Find your parking section</label>
      <div class="search-row">
        <input type="text" id="truck-input" placeholder="Truck #" inputmode="numeric" oninput="lookup(this.value)">
        <button onclick="lookup(document.getElementById('truck-input').value)">Go</button>
      </div>
    </div>
    <div class="result-box" id="result-box">
      <div class="result-header" id="result-header">
        <div class="sec-circle" id="result-circle"></div>
        <div class="info"><h2 id="result-title"></h2><p id="result-sub"></p></div>
      </div>
      <div class="result-body"><h3>Where to park</h3><p id="result-desc"></p></div>
    </div>
    <div class="not-found" id="not-found">Truck not found. Check the number or ask your supervisor.</div>
  </div>

  <!-- ── MAP ── -->
  <div class="panel" id="panel-map">
    <div class="map-card">
      <div class="map-card-header">Yard Map — 4440 Old Tampa Hwy</div>
      <svg viewBox="0 0 680 400" xmlns="http://www.w3.org/2000/svg" id="yard-svg">
        <rect width="680" height="400" fill="#d6d1c0"/>
        <ellipse cx="52" cy="46" rx="40" ry="26" fill="#6b8f47" opacity="0.75"/>
        <ellipse cx="102" cy="38" rx="32" ry="22" fill="#5a7a3a" opacity="0.75"/>
        <ellipse cx="34" cy="70" rx="26" ry="18" fill="#6b8f47" opacity="0.65"/>
        <rect x="0" y="0" width="680" height="16" fill="#b8b2a0"/>
        <rect x="0" y="384" width="680" height="16" fill="#a0998a"/>
        <rect x="284" y="0" width="112" height="20" fill="#d6d1c0"/>
        <text x="340" y="13" text-anchor="middle" fill="#888" font-size="9" font-family="DM Sans,sans-serif">ENTRY / EXIT</text>
        <rect x="4" y="318" width="672" height="66" rx="3" fill="#6e6e6e"/>
        <text x="340" y="360" text-anchor="middle" fill="#ccc" font-size="11" font-family="DM Sans,sans-serif">WAREHOUSE / LOADING DOCKS</text>
        <rect x="14" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="31" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="48" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="65" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="82" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="120" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="137" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="154" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="171" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="220" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="237" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="254" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="420" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="437" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="454" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="530" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="547" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="564" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="581" y="326" width="13" height="22" rx="1" fill="#444"/>
        <rect x="618" y="326" width="13" height="22" rx="1" fill="#444"/><rect x="635" y="326" width="13" height="22" rx="1" fill="#444"/>
        <!-- Fuel island far right -->
        <rect x="600" y="155" width="70" height="95" rx="4" fill="#e8e2d0" stroke="#aaa" stroke-width="1.5" stroke-dasharray="4 3"/>
        <text x="635" y="196" text-anchor="middle" fill="#777" font-size="9" font-family="DM Sans,sans-serif">FUEL</text>
        <text x="635" y="208" text-anchor="middle" fill="#777" font-size="9" font-family="DM Sans,sans-serif">ISLAND</text>
        <text x="635" y="220" text-anchor="middle" fill="#999" font-size="8" font-family="DM Sans,sans-serif">← entry</text>
        <!-- Generator bottom center-right -->
        <rect x="490" y="252" width="100" height="60" rx="4" fill="#8a8a8a" opacity="0.85"/>
        <text x="540" y="279" text-anchor="middle" fill="#ddd" font-size="9" font-family="DM Sans,sans-serif">OFFICE /</text>
        <text x="540" y="291" text-anchor="middle" fill="#ddd" font-size="9" font-family="DM Sans,sans-serif">GENERATOR</text>
        <!-- S3 Yellow -->
        <g id="zone-3" class="zone" onclick="highlightZone(3)">
          <rect x="135" y="18" width="110" height="76" rx="6" fill="#FBBF24" fill-opacity="0.32" stroke="#D97706" stroke-width="2.5"/>
          <rect x="144" y="26" width="10" height="34" rx="2" fill="#D97706" opacity="0.5"/><rect x="158" y="26" width="10" height="34" rx="2" fill="#D97706" opacity="0.5"/>
          <rect x="172" y="26" width="10" height="34" rx="2" fill="#D97706" opacity="0.5"/><rect x="186" y="26" width="10" height="34" rx="2" fill="#D97706" opacity="0.5"/>
          <rect x="200" y="26" width="10" height="34" rx="2" fill="#D97706" opacity="0.5"/>
          <text x="190" y="74" text-anchor="middle" fill="#92400E" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S3</text>
          <text x="190" y="86" text-anchor="middle" fill="#92400E" font-size="9" font-family="DM Sans,sans-serif">Guard West · 9</text>
        </g>
        <!-- S4 Green -->
        <g id="zone-4" class="zone" onclick="highlightZone(4)">
          <rect x="252" y="18" width="82" height="76" rx="6" fill="#22C55E" fill-opacity="0.28" stroke="#16A34A" stroke-width="2.5"/>
          <rect x="260" y="26" width="10" height="34" rx="2" fill="#16A34A" opacity="0.5"/><rect x="274" y="26" width="10" height="34" rx="2" fill="#16A34A" opacity="0.5"/>
          <rect x="288" y="26" width="10" height="34" rx="2" fill="#16A34A" opacity="0.5"/><rect x="302" y="26" width="10" height="34" rx="2" fill="#16A34A" opacity="0.5"/>
          <text x="293" y="74" text-anchor="middle" fill="#14532D" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S4</text>
          <text x="293" y="86" text-anchor="middle" fill="#14532D" font-size="9" font-family="DM Sans,sans-serif">Guard East · 4</text>
        </g>
        <!-- S2 Red -->
        <g id="zone-2" class="zone" onclick="highlightZone(2)">
          <rect x="342" y="18" width="124" height="110" rx="6" fill="#EF4444" fill-opacity="0.22" stroke="#DC2626" stroke-width="2.5"/>
          <rect x="350" y="26" width="10" height="40" rx="2" fill="#DC2626" opacity="0.45"/><rect x="364" y="26" width="10" height="40" rx="2" fill="#DC2626" opacity="0.45"/>
          <rect x="378" y="26" width="10" height="40" rx="2" fill="#DC2626" opacity="0.45"/><rect x="392" y="26" width="10" height="40" rx="2" fill="#DC2626" opacity="0.45"/>
          <rect x="406" y="26" width="10" height="40" rx="2" fill="#DC2626" opacity="0.45"/><rect x="420" y="26" width="10" height="40" rx="2" fill="#DC2626" opacity="0.45"/>
          <rect x="350" y="74" width="10" height="32" rx="2" fill="#DC2626" opacity="0.3"/><rect x="364" y="74" width="10" height="32" rx="2" fill="#DC2626" opacity="0.3"/>
          <rect x="378" y="74" width="10" height="32" rx="2" fill="#DC2626" opacity="0.3"/><rect x="392" y="74" width="10" height="32" rx="2" fill="#DC2626" opacity="0.3"/>
          <text x="404" y="108" text-anchor="middle" fill="#7F1D1D" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S2</text>
          <text x="404" y="120" text-anchor="middle" fill="#7F1D1D" font-size="9" font-family="DM Sans,sans-serif">West Side · 12</text>
        </g>
        <!-- S7 Blue -->
        <g id="zone-7" class="zone" onclick="highlightZone(7)">
          <rect x="474" y="18" width="118" height="130" rx="6" fill="#3B82F6" fill-opacity="0.22" stroke="#2563EB" stroke-width="2.5"/>
          <rect x="482" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/><rect x="495" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="508" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/><rect x="521" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="534" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/><rect x="547" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="560" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/><rect x="573" y="26" width="9" height="44" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="482" y="78" width="9" height="40" rx="2" fill="#2563EB" opacity="0.3"/><rect x="495" y="78" width="9" height="40" rx="2" fill="#2563EB" opacity="0.3"/>
          <rect x="508" y="78" width="9" height="40" rx="2" fill="#2563EB" opacity="0.3"/><rect x="521" y="78" width="9" height="40" rx="2" fill="#2563EB" opacity="0.3"/>
          <rect x="534" y="78" width="9" height="40" rx="2" fill="#2563EB" opacity="0.3"/>
          <text x="533" y="132" text-anchor="middle" fill="#1E3A8A" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S7</text>
          <text x="533" y="144" text-anchor="middle" fill="#1E3A8A" font-size="9" font-family="DM Sans,sans-serif">Fuel Island E · 18</text>
        </g>
        <!-- S5 Cyan -->
        <g id="zone-5" class="zone" onclick="highlightZone(5)">
          <rect x="18" y="108" width="168" height="128" rx="6" fill="#06B6D4" fill-opacity="0.2" stroke="#0891B2" stroke-width="2.5"/>
          <rect x="26" y="116" width="10" height="40" rx="2" fill="#0891B2" opacity="0.45"/><rect x="40" y="116" width="10" height="40" rx="2" fill="#0891B2" opacity="0.45"/>
          <rect x="54" y="116" width="10" height="40" rx="2" fill="#0891B2" opacity="0.45"/>
          <rect x="26" y="164" width="10" height="36" rx="2" fill="#0891B2" opacity="0.32"/><rect x="40" y="164" width="10" height="36" rx="2" fill="#0891B2" opacity="0.32"/>
          <rect x="54" y="164" width="10" height="36" rx="2" fill="#0891B2" opacity="0.32"/><rect x="68" y="164" width="10" height="36" rx="2" fill="#0891B2" opacity="0.32"/>
          <text x="102" y="218" text-anchor="middle" fill="#164E63" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S5</text>
          <text x="102" y="230" text-anchor="middle" fill="#164E63" font-size="9" font-family="DM Sans,sans-serif">Hand Trucks · 7</text>
        </g>
        <!-- S6 Orange -->
        <g id="zone-6" class="zone" onclick="highlightZone(6)">
          <rect x="18" y="244" width="168" height="68" rx="6" fill="#F97316" fill-opacity="0.22" stroke="#EA580C" stroke-width="2.5"/>
          <rect x="26" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/><rect x="40" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/>
          <rect x="54" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/><rect x="68" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/>
          <rect x="82" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/><rect x="96" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/>
          <rect x="110" y="252" width="10" height="28" rx="2" fill="#EA580C" opacity="0.45"/>
          <text x="102" y="293" text-anchor="middle" fill="#7C2D12" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S6</text>
          <text x="102" y="305" text-anchor="middle" fill="#7C2D12" font-size="9" font-family="DM Sans,sans-serif">Checkers Doc · 7</text>
        </g>
        <!-- S8 Pink centered between S2 and S7 -->
        <g id="zone-8" class="zone" onclick="highlightZone(8)">
          <rect x="355" y="136" width="154" height="108" rx="6" fill="#D946EF" fill-opacity="0.2" stroke="#C026D3" stroke-width="2.5"/>
          <rect x="363" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/><rect x="376" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="389" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/><rect x="402" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="415" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/><rect x="428" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="441" y="144" width="9" height="42" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="363" y="193" width="9" height="36" rx="2" fill="#C026D3" opacity="0.28"/><rect x="376" y="193" width="9" height="36" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="389" y="193" width="9" height="36" rx="2" fill="#C026D3" opacity="0.28"/><rect x="402" y="193" width="9" height="36" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="415" y="193" width="9" height="36" rx="2" fill="#C026D3" opacity="0.28"/>
          <text x="432" y="222" text-anchor="middle" fill="#701A75" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S8</text>
          <text x="432" y="234" text-anchor="middle" fill="#701A75" font-size="9" font-family="DM Sans,sans-serif">Middle Yard · 36</text>
        </g>
        <!-- S1 Lime between S8 and generator -->
        <g id="zone-1" class="zone" onclick="highlightZone(1)">
          <rect x="355" y="252" width="128" height="60" rx="6" fill="#84CC16" fill-opacity="0.32" stroke="#65A30D" stroke-width="2.5"/>
          <rect x="363" y="260" width="9" height="24" rx="2" fill="#65A30D" opacity="0.5"/><rect x="376" y="260" width="9" height="24" rx="2" fill="#65A30D" opacity="0.5"/>
          <rect x="389" y="260" width="9" height="24" rx="2" fill="#65A30D" opacity="0.5"/><rect x="402" y="260" width="9" height="24" rx="2" fill="#65A30D" opacity="0.5"/>
          <rect x="415" y="260" width="9" height="24" rx="2" fill="#65A30D" opacity="0.5"/>
          <text x="419" y="286" text-anchor="middle" fill="#365314" font-size="13" font-weight="600" font-family="DM Sans,sans-serif">S1</text>
          <text x="419" y="298" text-anchor="middle" fill="#365314" font-size="9" font-family="DM Sans,sans-serif">Generator · 9</text>
        </g>
      </svg>
      <div class="legend" id="legend"></div>
    </div>
    <div class="info-strip">
      <strong>Tip:</strong> Click any section on the map to highlight it and see which trucks are assigned there.
    </div>
  </div>

  <!-- ── QR CODES ── -->
  <div class="panel" id="panel-qr">
    <div class="qr-controls">
      <select id="sec-filter" onchange="buildQR()">
        <option value="all">All Sections</option>
        <option value="1">Section 1 — Generator</option>
        <option value="2">Section 2 — West Side</option>
        <option value="3">Section 3 — Guard West</option>
        <option value="4">Section 4 — Guard East</option>
        <option value="5">Section 5 — Hand Trucks</option>
        <option value="6">Section 6 — Checkers Doc</option>
        <option value="7">Section 7 — Fuel Island E</option>
        <option value="8">Section 8 — Middle Yard</option>
      </select>
      <button onclick="window.print()">🖨 Print QR Codes</button>
    </div>
    <p class="qr-note">Each QR code is color-bordered to match its section. Scan to open the parking map for that truck.</p>
    <div class="qr-grid" id="qr-grid"></div>
  </div>

  <div class="info-strip" id="footer-strip" style="margin-top:16px">
    <strong>Questions?</strong> See your supervisor or yard manager. This map is for driver self-parking only.
  </div>
</div>

<script>
const GITHUB_USER = 'kalebc8';
const REPO_NAME = 'SGWS-PARKING'; // UPDATE THIS after deploying to GitHub Pages
const BASE_URL = `https://${GITHUB_USER}.github.io/${REPO_NAME}/`;

const SECTIONS = {
  1: { name:"In Front of Generator", short:"Generator", color:"#65A30D", bg:"#ECFCCB", text:"#365314",
       trucks:[1226,1227,1228,1229,1230,3450,3451,3452,3453],
       desc:"Bottom right, lime green zone. Park in front of the generator — between Section 8 above and the generator building." },
  2: { name:"First 12 Spots West Side", short:"West Side", color:"#DC2626", bg:"#FEE2E2", text:"#7F1D1D",
       trucks:[59,19,9005,9006,9007,9221,9222,9223,9461,9699,9449,9961],
       desc:"Top center, red zone. First 12 spots on the west side of the yard." },
  3: { name:"Fence by Guard West Side", short:"Guard West", color:"#D97706", bg:"#FEF3C7", text:"#92400E",
       trucks:[5542,5543,5544,5545,5546,5541,5571,5572,5573],
       desc:"Top left, yellow zone. Along the fence by the guard shack on the west side." },
  4: { name:"Fence by Guard East Side", short:"Guard East", color:"#16A34A", bg:"#DCFCE7", text:"#14532D",
       trucks:[3333,3334,3335,3336],
       desc:"Top left, green zone — right of Section 3. 4 spots along the guard fence, east side." },
  5: { name:"By Hand Trucks Facing Fence", short:"Hand Trucks", color:"#0891B2", bg:"#CFFAFE", text:"#164E63",
       trucks:[3203,3204,3205,3206,3208,3209,4738],
       desc:"Left middle, cyan zone. By the hand trucks area, facing the fence." },
  6: { name:"Facing Checkers Doc", short:"Checkers Doc", color:"#EA580C", bg:"#FFEDD5", text:"#7C2D12",
       trucks:[45,111,140,222,251,554,559],
       desc:"Left lower, orange zone. Facing the Checkers Doc area." },
  7: { name:"East Side Fence Fuel Island", short:"Fuel Island E", color:"#2563EB", bg:"#DBEAFE", text:"#1E3A8A",
       trucks:[2121,3596,3903,3904,3905,3906,3907,4739,4740,5921,5922,5923,5924,5925,5926,7642],
       desc:"Top right, blue zone. Large double row along the east side fence near the fuel island." },
  8: { name:"Section 8 Middle Yard Staggered", short:"Middle Yard", color:"#C026D3", bg:"#FAE8FF", text:"#701A75",
       trucks:[2204,2205,2206,2553,2555,2819,2820,3192,3194,3195,4135,4136,4200,4431,4432,4433,5167,5188,5234,5235,5236,5264,5281,6051,6052,6053,7264,7705,8092,8093,8094,8110,8111,8703,8704,8705],
       desc:"Center right, pink zone. Parked Staggered leaving room to walk between trucks." }
};

const truckMap = {};
Object.entries(SECTIONS).forEach(([s,d]) => d.trucks.forEach(t => truckMap[String(t)] = +s));

// TABS
function showTab(name, btn) {
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.getElementById('panel-'+name).classList.add('active');
  btn.classList.add('active');
  if (name === 'qr') buildQR();
}

// LOOKUP
function lookup(val) {
  const num = val.trim().replace(/^#/,'');
  const rb = document.getElementById('result-box');
  const nf = document.getElementById('not-found');
  rb.style.display='none'; nf.style.display='none';
  if (!num) return;
  const sec = truckMap[num];
  if (!sec) { nf.style.display='block'; return; }
  const s = SECTIONS[sec];
  document.getElementById('result-header').style.background = s.color;
  document.getElementById('result-circle').textContent = 'S'+sec;
  document.getElementById('result-title').textContent = 'Truck #'+num+' → Section '+sec;
  document.getElementById('result-sub').textContent = s.name;
  document.getElementById('result-desc').textContent = s.desc;
  rb.style.display='block';
}

// MAP HIGHLIGHT
function highlightZone(sec) {
  document.querySelectorAll('[id^="zone-"]').forEach(g => {
    const r = g.querySelector('rect');
    if (r) { r.setAttribute('fill-opacity','0.12'); r.setAttribute('stroke-opacity','0.35'); }
  });
  const el = document.getElementById('zone-'+sec);
  if (el) {
    const r = el.querySelector('rect');
    r.setAttribute('fill-opacity','0.6'); r.setAttribute('stroke-opacity','1');
    el.classList.add('highlight-pulse');
    setTimeout(()=>el.classList.remove('highlight-pulse'),3600);
  }
}

// LEGEND
const leg = document.getElementById('legend');
Object.entries(SECTIONS).forEach(([s,d]) => {
  const item = document.createElement('div');
  item.className='legend-item';
  item.style.borderColor=d.color; item.style.color=d.text;
  item.innerHTML=`<div class="legend-dot" style="background:${d.color}"></div>S${s}: ${d.short} · ${d.trucks.length}`;
  item.onclick=()=>highlightZone(+s);
  leg.appendChild(item);
});

// QR CODES
let qrBuilt = false;
function buildQR() {
  const filter = document.getElementById('sec-filter').value;
  const grid = document.getElementById('qr-grid');
  grid.innerHTML = '';
  qrBuilt = false;

  const entries = [];
  Object.entries(SECTIONS).forEach(([sec, d]) => {
    if (filter !== 'all' && filter !== sec) return;
    d.trucks.forEach(t => entries.push({ truck: t, sec: +sec, d }));
  });

  if (!entries.length) {
    grid.innerHTML = '<p style="color:#718096;font-size:13px;padding:8px">No trucks assigned to this section yet.</p>';
    return;
  }

  entries.forEach(({ truck, sec, d }) => {
    const card = document.createElement('div');
    card.className = 'qr-card';
    card.style.borderColor = d.color;
    card.innerHTML = `
      <div class="truck-label" style="color:${d.color}">Truck</div>
      <div class="truck-num" style="color:${d.color}">${truck}</div>
      <div class="qr-img" id="qr-${truck}"></div>
      <div class="sec-label" style="background:${d.color}">Section ${sec}: ${d.short}</div>`;
    grid.appendChild(card);
  });

  // Generate QR codes after DOM is ready
  setTimeout(() => {
    entries.forEach(({ truck, sec, d }) => {
      const el = document.getElementById('qr-'+truck);
      if (el && window.QRCode) {
        new QRCode(el, {
          text: `${BASE_URL}?truck=${truck}`,
          width: 120,
          height: 120,
          colorDark: d.color,
          colorLight: '#ffffff',
          correctLevel: QRCode.CorrectLevel.M
        });
      }
    });
  }, 100);
}

// Handle ?truck= URL param (for when drivers scan QR)
const urlTruck = new URLSearchParams(location.search).get('truck');
if (urlTruck) {
  document.getElementById('truck-input').value = urlTruck;
  lookup(urlTruck);
  // Also highlight the zone on the map
  const sec = truckMap[urlTruck];
  if (sec) setTimeout(() => highlightZone(sec), 300);
}
</script>
</body>
</html>
