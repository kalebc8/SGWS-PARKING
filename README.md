# SGWS-PARKING
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
  .tab-bar { display: flex; gap: 8px; margin-bottom: 1.25rem; flex-wrap: wrap; }
  .tab { padding: 8px 16px; border: 1.5px solid #cbd5e0; border-radius: 8px; background: white; color: #4a5568; cursor: pointer; font-size: 13px; font-weight: 500; transition: all .15s; }
  .tab.active { background: #1a1a2e; color: white; border-color: #1a1a2e; }
  .panel { display: none; }
  .panel.active { display: block; }
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
  .map-card { background: white; border-radius: 12px; overflow: hidden; box-shadow: 0 1px 4px rgba(0,0,0,0.08); margin-bottom: 16px; }
  .map-card-header { padding: 12px 16px; background: #1a1a2e; color: white; font-size: 12px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; }
  .map-card svg { display: block; width: 100%; }
  .legend { display: flex; flex-wrap: wrap; gap: 8px; padding: 12px 16px; border-top: 1px solid #f0ede6; }
  .legend-item { display: flex; align-items: center; gap: 5px; font-size: 11px; padding: 4px 8px; border-radius: 20px; border: 1.5px solid; cursor: pointer; }
  .legend-dot { width: 8px; height: 8px; border-radius: 50%; }
  .zone { cursor: pointer; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.55} }
  .highlight-pulse { animation: pulse 1.2s ease-in-out 3; }
  .qr-controls { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 16px; align-items: center; }
  .qr-controls select { padding: 8px 12px; border: 1.5px solid #e2e8f0; border-radius: 8px; font-size: 13px; background: white; outline: none; }
  .qr-controls button { padding: 8px 16px; background: #1a1a2e; color: white; border: none; border-radius: 8px; font-size: 13px; font-weight: 500; cursor: pointer; }
  .qr-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 14px; }
  .qr-card { background: white; border-radius: 10px; padding: 14px 12px 10px; text-align: center; border: 4px solid #ccc; box-shadow: 0 1px 4px rgba(0,0,0,0.08); display: flex; flex-direction: column; align-items: center; }
  .qr-card .truck-label { font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; margin-bottom: 2px; }
  .qr-card .truck-num { font-size: 26px; font-weight: 700; font-family: 'DM Mono', monospace; line-height: 1; margin-bottom: 8px; }
  .qr-card .qr-img { margin-bottom: 8px; }
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
 
  <!-- LOOKUP -->
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
 
  <!-- MAP -->
  <div class="panel" id="panel-map">
    <div class="map-card">
      <div class="map-card-header">Yard Map — 4440 Old Tampa Hwy, Lakeland FL</div>
      <svg id="yard-svg" width="100%" viewBox="0 0 680 480" role="img" xmlns="http://www.w3.org/2000/svg">
        <title>SGWS Lakeland yard parking map</title>
        <desc>Yard map with 8 color-coded parking sections</desc>
        <defs>
          <marker id="arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
            <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </marker>
        </defs>
        <path d="M 15 118 Q 15 104 28 104 L 108 104 L 108 44 Q 108 32 120 32 L 652 32 Q 664 32 664 44 L 664 368 Q 664 380 652 380 L 28 380 Q 15 380 15 368 Z" fill="#cec9b5" stroke="#b0aa96" stroke-width="1"/>
        <ellipse cx="50" cy="72" rx="36" ry="22" fill="#7a9a5a" opacity="0.8"/>
        <ellipse cx="86" cy="56" rx="28" ry="18" fill="#6a8a4a" opacity="0.8"/>
        <ellipse cx="28" cy="96" rx="20" ry="13" fill="#7a9a5a" opacity="0.7"/>
        <path d="M555 8 Q610 5 655 10 L660 32 L550 32 Z" fill="#5a8a3a" opacity="0.55"/>
        <line x1="15" y1="380" x2="664" y2="380" stroke="#e8c000" stroke-width="3"/>
        <rect x="15" y="380" width="649" height="56" fill="#5a5a5a"/>
        <rect x="15" y="380" width="108" height="56" fill="#6a3a8a" opacity="0.95"/>
        <rect x="22" y="386" width="10" height="20" rx="1" fill="#9b59d0"/>
        <rect x="35" y="386" width="10" height="20" rx="1" fill="#9b59d0"/>
        <rect x="48" y="386" width="10" height="20" rx="1" fill="#cc3333"/>
        <rect x="61" y="386" width="10" height="20" rx="1" fill="#9b59d0"/>
        <rect x="74" y="386" width="10" height="20" rx="1" fill="#cc3333"/>
        <rect x="87" y="386" width="10" height="20" rx="1" fill="#9b59d0"/>
        <rect x="100" y="386" width="10" height="20" rx="1" fill="#9b59d0"/>
        <text font-family="DM Sans,sans-serif" font-size="11" x="69" y="422" text-anchor="middle" fill="white" font-weight="600">RETURN DOCKS</text>
        <rect x="145" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="158" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="171" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="210" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="223" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="260" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="273" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="330" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="343" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="390" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="450" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="510" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="523" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="570" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="583" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="620" y="386" width="9" height="18" rx="1" fill="#444"/>
        <rect x="633" y="386" width="9" height="18" rx="1" fill="#444"/><rect x="646" y="386" width="9" height="18" rx="1" fill="#444"/>
        <text font-family="DM Sans,sans-serif" font-size="11" x="420" y="422" text-anchor="middle" fill="#aaa">LOADING DOCKS</text>
        <rect x="20" y="130" width="50" height="80" rx="6" fill="#E91E8C" fill-opacity="0.22" stroke="#C2185B" stroke-width="2" stroke-dasharray="4 3"/>
        <text font-family="DM Sans,sans-serif" font-size="11" x="45" y="158" text-anchor="middle" fill="#880E4F">HAND</text>
        <text font-family="DM Sans,sans-serif" font-size="11" x="45" y="172" text-anchor="middle" fill="#880E4F">CART</text>
        <text font-family="DM Sans,sans-serif" font-size="11" x="45" y="186" text-anchor="middle" fill="#880E4F">STN</text>
        <rect x="618" y="148" width="46" height="74" rx="5" fill="#1565C0" fill-opacity="0.22" stroke="#1565C0" stroke-width="2"/>
        <text font-family="DM Sans,sans-serif" font-size="11" x="641" y="178" text-anchor="middle" fill="#0D47A1">FUEL</text>
        <text font-family="DM Sans,sans-serif" font-size="11" x="641" y="192" text-anchor="middle" fill="#0D47A1">ISLAND</text>
        <rect x="430" y="346" width="140" height="34" rx="3" fill="#222" opacity="0.92"/>
        <line x1="430" y1="354" x2="570" y2="354" stroke="#444" stroke-width="1"/>
        <line x1="430" y1="361" x2="570" y2="361" stroke="#444" stroke-width="1"/>
        <line x1="430" y1="368" x2="570" y2="368" stroke="#444" stroke-width="1"/>
        <line x1="430" y1="375" x2="570" y2="375" stroke="#444" stroke-width="1"/>
        <text font-family="DM Sans,sans-serif" font-size="11" x="500" y="367" text-anchor="middle" fill="white" font-weight="600">GENERATOR</text>
        <rect x="190" y="130" width="190" height="155" rx="8" fill="#888" fill-opacity="0.18" stroke="#cc2222" stroke-width="2" stroke-dasharray="6 3"/>
        <line x1="200" y1="140" x2="370" y2="275" stroke="#cc2222" stroke-width="1.5" opacity="0.35"/>
        <line x1="370" y1="140" x2="200" y2="275" stroke="#cc2222" stroke-width="1.5" opacity="0.35"/>
        <text font-family="DM Sans,sans-serif" font-size="11" x="280" y="216" text-anchor="middle" fill="#991111" font-weight="500">NO PARKING</text>
        <!-- S3 -->
        <g id="zone-3" class="zone" onclick="highlightZone(3)">
          <rect x="120" y="44" width="108" height="60" rx="5" fill="#FBBF24" fill-opacity="0.32" stroke="#D97706" stroke-width="2"/>
          <rect x="127" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.55"/><rect x="141" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.55"/>
          <rect x="155" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.55"/><rect x="169" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.55"/>
          <rect x="183" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.55"/><rect x="197" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.5"/>
          <rect x="211" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.5"/><rect x="225" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.5"/>
          <rect x="239" y="50" width="10" height="36" rx="2" fill="#D97706" opacity="0.45"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="174" y="95" text-anchor="middle" fill="#92400E">S3 · 9 trucks</text>
        </g>
        <!-- S4 -->
        <g id="zone-4" class="zone" onclick="highlightZone(4)">
          <rect x="234" y="44" width="56" height="60" rx="5" fill="#22C55E" fill-opacity="0.28" stroke="#16A34A" stroke-width="2"/>
          <rect x="241" y="50" width="10" height="36" rx="2" fill="#16A34A" opacity="0.55"/><rect x="255" y="50" width="10" height="36" rx="2" fill="#16A34A" opacity="0.55"/>
          <rect x="269" y="50" width="10" height="36" rx="2" fill="#16A34A" opacity="0.5"/><rect x="283" y="50" width="10" height="36" rx="2" fill="#16A34A" opacity="0.5"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="262" y="93" text-anchor="middle" fill="#14532D">S4 · 4</text>
        </g>
        <!-- EXIT -->
        <line x1="340" y1="44" x2="340" y2="18" stroke="#1a1a2e" stroke-width="2.5" marker-end="url(#arr)"/>
        <rect x="320" y="20" width="40" height="16" rx="3" fill="#cec9b5" opacity="0.95"/>
        <text font-family="DM Sans,sans-serif" font-size="11" font-weight="600" x="340" y="32" text-anchor="middle" fill="#1a1a2e">EXIT</text>
        <!-- S2 -->
        <g id="zone-2" class="zone" onclick="highlightZone(2)">
          <rect x="424" y="44" width="94" height="60" rx="5" fill="#EF4444" fill-opacity="0.2" stroke="#DC2626" stroke-width="2"/>
          <rect x="430" y="50" width="10" height="36" rx="2" fill="#DC2626" opacity="0.45"/><rect x="444" y="50" width="10" height="36" rx="2" fill="#DC2626" opacity="0.45"/>
          <rect x="458" y="50" width="10" height="36" rx="2" fill="#DC2626" opacity="0.45"/><rect x="472" y="50" width="10" height="36" rx="2" fill="#DC2626" opacity="0.45"/>
          <rect x="486" y="50" width="10" height="36" rx="2" fill="#DC2626" opacity="0.45"/><rect x="500" y="50" width="10" height="36" rx="2" fill="#DC2626" opacity="0.4"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="471" y="93" text-anchor="middle" fill="#7F1D1D">S2 · 12 trucks</text>
        </g>
        <!-- S7 -->
        <g id="zone-7" class="zone" onclick="highlightZone(7)">
          <rect x="524" y="44" width="140" height="60" rx="5" fill="#3B82F6" fill-opacity="0.2" stroke="#2563EB" stroke-width="2"/>
          <rect x="530" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/><rect x="544" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="558" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/><rect x="572" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="586" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/><rect x="600" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="614" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/><rect x="628" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.45"/>
          <rect x="642" y="50" width="10" height="36" rx="2" fill="#2563EB" opacity="0.4"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="594" y="93" text-anchor="middle" fill="#1E3A8A">S7 · 18 trucks</text>
        </g>
        <!-- S5 -->
        <g id="zone-5" class="zone" onclick="highlightZone(5)">
          <rect x="60" y="196" width="122" height="52" rx="5" fill="#06B6D4" fill-opacity="0.2" stroke="#0891B2" stroke-width="2"/>
          <rect x="67" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.5"/><rect x="81" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.5"/>
          <rect x="95" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.5"/><rect x="109" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.5"/>
          <rect x="123" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.5"/><rect x="137" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.45"/>
          <rect x="151" y="203" width="10" height="36" rx="2" fill="#0891B2" opacity="0.45"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="121" y="224" text-anchor="middle" fill="#164E63">S5 · 7 trucks</text>
        </g>
        <!-- S6 -->
        <g id="zone-6" class="zone" onclick="highlightZone(6)">
          <rect x="60" y="254" width="122" height="52" rx="5" fill="#F97316" fill-opacity="0.22" stroke="#EA580C" stroke-width="2"/>
          <rect x="67" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.5"/><rect x="81" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.5"/>
          <rect x="95" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.5"/><rect x="109" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.5"/>
          <rect x="123" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.5"/><rect x="137" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.45"/>
          <rect x="151" y="261" width="10" height="36" rx="2" fill="#EA580C" opacity="0.45"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="121" y="282" text-anchor="middle" fill="#7C2D12">S6 · 7 trucks</text>
        </g>
        <!-- S8 -->
        <g id="zone-8" class="zone" onclick="highlightZone(8)">
          <rect x="390" y="138" width="216" height="118" rx="5" fill="#D946EF" fill-opacity="0.18" stroke="#C026D3" stroke-width="2"/>
          <rect x="397" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="411" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="425" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="439" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="453" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="467" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="481" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="495" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="509" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="523" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="537" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="551" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="565" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/><rect x="579" y="145" width="10" height="44" rx="2" fill="#C026D3" opacity="0.4"/>
          <rect x="404" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/><rect x="418" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="432" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/><rect x="446" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="460" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/><rect x="474" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="488" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/><rect x="502" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="516" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/><rect x="530" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="544" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/><rect x="558" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <rect x="572" y="197" width="10" height="42" rx="2" fill="#C026D3" opacity="0.28"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="498" y="188" text-anchor="middle" fill="#701A75">S8</text>
          <text font-family="DM Sans,sans-serif" font-size="11" x="498" y="202" text-anchor="middle" fill="#701A75">36 trucks · staggered</text>
        </g>
        <!-- S1 -->
        <g id="zone-1" class="zone" onclick="highlightZone(1)">
          <rect x="430" y="296" width="140" height="46" rx="5" fill="#84CC16" fill-opacity="0.32" stroke="#65A30D" stroke-width="2"/>
          <rect x="437" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.6"/><rect x="452" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.6"/>
          <rect x="467" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.6"/><rect x="482" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.6"/>
          <rect x="497" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.6"/><rect x="512" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.55"/>
          <rect x="527" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.55"/><rect x="542" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.55"/>
          <rect x="557" y="302" width="10" height="32" rx="2" fill="#65A30D" opacity="0.5"/>
          <text font-family="DM Sans,sans-serif" font-size="12" font-weight="500" x="500" y="317" text-anchor="middle" fill="#365314">S1 · 9 trucks</text>
        </g>
        <!-- ENTER -->
        <rect x="600" y="264" width="60" height="24" rx="5" fill="#166534"/>
        <text font-family="DM Sans,sans-serif" font-size="11" font-weight="600" x="630" y="280" text-anchor="middle" fill="white">ENTER →</text>
        <line x1="598" y1="276" x2="560" y2="276" stroke="#166534" stroke-width="3" marker-end="url(#arr)"/>
      </svg>
      <div class="legend" id="map-legend"></div>
    </div>
    <div class="info-strip">Click any section on the map to highlight it.</div>
  </div>
 
  <!-- QR CODES -->
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
        <option value="7">Section 7 — East Fence</option>
        <option value="8">Section 8 — Middle Yard</option>
      </select>
      <button onclick="window.print()">🖨 Print QR Codes</button>
    </div>
    <p class="qr-note">Each QR code is color-bordered to match its section. Scan to open the parking map.</p>
    <div class="qr-grid" id="qr-grid"></div>
  </div>
 
  <div class="info-strip">
    <strong>Questions?</strong> See your supervisor or yard manager. This map is for driver self-parking only.
  </div>
</div>
 
<script>
const GITHUB_USER = 'kalebc8';
const REPO_NAME = 'SGWS-PARKING';
const BASE_URL = `https://${GITHUB_USER}.github.io/${REPO_NAME}/`;
 
const SECTIONS = {
  1: { name:"In Front of Generator", short:"Generator", color:"#65A30D", bg:"#ECFCCB", text:"#365314",
       trucks:[1226,1227,1228,1229,1230,3450,3451,3452,3453],
       desc:"Bottom right, lime green zone. Single row above the generator, facing the warehouse." },
  2: { name:"First 12 Spots West Side", short:"West Side", color:"#DC2626", bg:"#FEE2E2", text:"#7F1D1D",
       trucks:[59,19,9005,9006,9007,9221,9222,9223,9461,9699,9449,9961],
       desc:"Top right, red zone. Single row to the left of Section 7, hugging the top fence." },
  3: { name:"Fence by Guard West Side", short:"Guard West", color:"#D97706", bg:"#FEF3C7", text:"#92400E",
       trucks:[5542,5543,5544,5545,5546,5541,5571,5572,5573],
       desc:"Top center, yellow zone. Single row inside the yard along the top." },
  4: { name:"Fence by Guard East Side", short:"Guard East", color:"#16A34A", bg:"#DCFCE7", text:"#14532D",
       trucks:[3333,3334,3335,3336],
       desc:"Top center, green zone. 4 spots to the right of Section 3." },
  5: { name:"By Hand Trucks Facing Fence", short:"Hand Trucks", color:"#0891B2", bg:"#CFFAFE", text:"#164E63",
       trucks:[3203,3204,3205,3206,3208,3209,4738],
       desc:"Left side, cyan zone. Single row above Section 6." },
  6: { name:"Facing Checkers Doc", short:"Checkers Doc", color:"#EA580C", bg:"#FFEDD5", text:"#7C2D12",
       trucks:[45,111,140,222,251,554,559],
       desc:"Left side, orange zone. Single row directly below Section 5." },
  7: { name:"East Side Fence", short:"East Fence", color:"#2563EB", bg:"#DBEAFE", text:"#1E3A8A",
       trucks:[2121,3596,3903,3904,3905,3906,3907,4739,4740,5921,5922,5923,5924,5925,5926,7642],
       desc:"Top right corner, blue zone. Single row hugging the right fence." },
  8: { name:"Middle Yard Staggered", short:"Middle Yard", color:"#C026D3", bg:"#FAE8FF", text:"#701A75",
       trucks:[2204,2205,2206,2553,2555,2819,2820,3192,3194,3195,4135,4136,4200,4431,4432,4433,5167,5188,5234,5235,5236,5264,5281,6051,6052,6053,7264,7705,8092,8093,8094,8110,8111,8703,8704,8705],
       desc:"Center right, pink zone. Double row staggered — leave room to walk between trucks." }
};
 
const truckMap = {};
Object.entries(SECTIONS).forEach(([s,d]) => d.trucks.forEach(t => truckMap[String(t)] = +s));
 
function showTab(name, btn) {
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.getElementById('panel-'+name).classList.add('active');
  btn.classList.add('active');
  if (name === 'qr') buildQR();
}
 
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
  highlightZone(sec);
}
 
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
 
// Build legend
const leg = document.getElementById('map-legend');
Object.entries(SECTIONS).forEach(([s,d]) => {
  const item = document.createElement('div');
  item.className='legend-item';
  item.style.borderColor=d.color; item.style.color=d.text;
  item.innerHTML=`<div class="legend-dot" style="background:${d.color}"></div>S${s}: ${d.short}`;
  item.onclick=()=>highlightZone(+s);
  leg.appendChild(item);
});
 
function buildQR() {
  const filter = document.getElementById('sec-filter').value;
  const grid = document.getElementById('qr-grid');
  grid.innerHTML = '';
  const entries = [];
  Object.entries(SECTIONS).forEach(([sec,d]) => {
    if (filter !== 'all' && filter !== sec) return;
    d.trucks.forEach(t => entries.push({ truck:t, sec:+sec, d }));
  });
  if (!entries.length) {
    grid.innerHTML = '<p style="color:#718096;font-size:13px;padding:8px">No trucks assigned yet.</p>';
    return;
  }
  entries.forEach(({truck,sec,d}) => {
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
  setTimeout(() => {
    entries.forEach(({truck,sec,d}) => {
      const el = document.getElementById('qr-'+truck);
      if (el && window.QRCode) {
        new QRCode(el, {
          text: `${BASE_URL}?truck=${truck}`,
          width: 120, height: 120,
          colorDark: d.color, colorLight: '#ffffff',
          correctLevel: QRCode.CorrectLevel.M
        });
      }
    });
  }, 100);
}
 
const urlTruck = new URLSearchParams(location.search).get('truck');
if (urlTruck) {
  document.getElementById('truck-input').value = urlTruck;
  lookup(urlTruck);
  const sec = truckMap[urlTruck];
  if (sec) setTimeout(() => highlightZone(sec), 300);
}
</script>
</body>
</html>
