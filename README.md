<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard FOLU Net Sink 2030 — Peta Pengadaan Kehutanan</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css"/>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Sora:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');

  :root {
    --forest: #0F6E56;
    --forest-light: #1D9E75;
    --forest-pale: #E1F5EE;
    --forest-mid: #9FE1CB;
    --gold: #BA7517;
    --gold-light: #EF9F27;
    --gold-pale: #FAEEDA;
    --bark: #442B1C;
    --sky: #185FA5;
    --sky-light: #378ADD;
    --sky-pale: #E6F1FB;
    --ember: #993C1D;
    --ember-light: #D85A30;
    --ember-pale: #FAECE7;
    --bg: #F4F7F5;
    --card: #FFFFFF;
    --text: #1A2E25;
    --muted: #5F7268;
    --border: rgba(15,110,86,0.15);
    --shadow: 0 2px 12px rgba(15,110,86,0.08);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Sora', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
  }

  /* ─── HEADER ─────────────────────────────────── */
  .header {
    background: var(--forest);
    color: white;
    padding: 0 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 60px;
    position: sticky;
    top: 0;
    z-index: 1000;
    box-shadow: 0 2px 16px rgba(0,0,0,0.2);
  }
  .header-brand {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .leaf-icon {
    width: 28px; height: 28px;
    background: var(--gold-light);
    border-radius: 50% 50% 50% 0;
    transform: rotate(-45deg);
    flex-shrink: 0;
  }
  .header-title {
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 0.02em;
  }
  .header-sub {
    font-size: 11px;
    opacity: 0.7;
    font-weight: 300;
  }
  .header-badge {
    background: var(--gold-light);
    color: var(--bark);
    font-size: 11px;
    font-weight: 600;
    padding: 4px 12px;
    border-radius: 20px;
    font-family: 'JetBrains Mono', monospace;
  }

  /* ─── LAYOUT ──────────────────────────────────── */
  .layout {
    display: grid;
    grid-template-columns: 320px 1fr;
    grid-template-rows: auto 1fr;
    height: calc(100vh - 60px);
  }

  /* ─── STAT BAR ────────────────────────────────── */
  .stat-bar {
    grid-column: 1 / -1;
    background: var(--card);
    border-bottom: 1px solid var(--border);
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 0;
  }
  .stat-item {
    padding: 12px 20px;
    border-right: 1px solid var(--border);
    position: relative;
  }
  .stat-item:last-child { border-right: none; }
  .stat-label {
    font-size: 10px;
    font-weight: 500;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 2px;
  }
  .stat-value {
    font-size: 20px;
    font-weight: 600;
    font-family: 'JetBrains Mono', monospace;
    color: var(--forest);
  }
  .stat-sub {
    font-size: 10px;
    color: var(--muted);
    margin-top: 1px;
  }
  .stat-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--gold-light);
    position: absolute;
    top: 12px;
    right: 12px;
  }

  /* ─── SIDEBAR ─────────────────────────────────── */
  .sidebar {
    background: var(--card);
    border-right: 1px solid var(--border);
    overflow-y: auto;
    display: flex;
    flex-direction: column;
  }
  .sidebar::-webkit-scrollbar { width: 4px; }
  .sidebar::-webkit-scrollbar-thumb { background: var(--forest-mid); border-radius: 2px; }

  .filter-section {
    padding: 16px;
    border-bottom: 1px solid var(--border);
  }
  .filter-label {
    font-size: 10px;
    font-weight: 600;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 8px;
  }
  .filter-select {
    width: 100%;
    padding: 8px 10px;
    border: 1px solid var(--border);
    border-radius: 6px;
    font-size: 12px;
    font-family: 'Sora', sans-serif;
    color: var(--text);
    background: var(--bg);
    margin-bottom: 6px;
    cursor: pointer;
  }
  .filter-select:focus { outline: 2px solid var(--forest-light); }

  .btn-filter {
    width: 100%;
    padding: 8px;
    background: var(--forest);
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 12px;
    font-weight: 500;
    font-family: 'Sora', sans-serif;
    cursor: pointer;
    transition: background 0.2s;
    margin-top: 4px;
  }
  .btn-filter:hover { background: var(--forest-light); }

  .paket-list {
    flex: 1;
    padding: 12px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .paket-header {
    padding: 8px 4px 4px;
    font-size: 10px;
    font-weight: 600;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .paket-count {
    background: var(--forest-pale);
    color: var(--forest);
    font-size: 10px;
    padding: 2px 8px;
    border-radius: 10px;
    font-weight: 600;
  }

  .paket-card {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 12px;
    cursor: pointer;
    transition: all 0.15s;
    border-left: 3px solid transparent;
  }
  .paket-card:hover {
    border-color: var(--forest-mid);
    border-left-color: var(--forest-light);
    background: var(--forest-pale);
    transform: translateX(2px);
  }
  .paket-card.active {
    border-color: var(--forest-mid);
    border-left-color: var(--forest);
    background: var(--forest-pale);
  }
  .paket-name {
    font-size: 11px;
    font-weight: 500;
    color: var(--text);
    line-height: 1.4;
    margin-bottom: 6px;
  }
  .paket-meta {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }
  .tag {
    font-size: 9px;
    font-weight: 600;
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'JetBrains Mono', monospace;
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }
  .tag-type { background: var(--sky-pale); color: var(--sky); }
  .tag-status-aktif { background: var(--forest-pale); color: var(--forest); }
  .tag-status-selesai { background: var(--gold-pale); color: var(--gold); }
  .tag-status-proses { background: var(--sky-pale); color: var(--sky); }
  .paket-hps {
    font-size: 10px;
    font-family: 'JetBrains Mono', monospace;
    color: var(--forest);
    font-weight: 600;
    margin-top: 4px;
  }
  .paket-lokasi {
    font-size: 10px;
    color: var(--muted);
    margin-top: 2px;
    display: flex;
    align-items: center;
    gap: 3px;
  }

  /* ─── MAP ─────────────────────────────────────── */
  .map-wrapper {
    position: relative;
    overflow: hidden;
  }
  #map {
    width: 100%;
    height: 100%;
  }

  /* Detail panel */
  .detail-panel {
    position: absolute;
    bottom: 20px;
    right: 16px;
    width: 280px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px;
    box-shadow: var(--shadow);
    z-index: 900;
    display: none;
    animation: slideUp 0.2s ease;
  }
  .detail-panel.visible { display: block; }
  @keyframes slideUp {
    from { opacity: 0; transform: translateY(10px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .detail-close {
    position: absolute;
    top: 8px; right: 8px;
    width: 20px; height: 20px;
    border: none;
    background: var(--bg);
    border-radius: 50%;
    font-size: 12px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--muted);
  }
  .detail-title {
    font-size: 12px;
    font-weight: 600;
    color: var(--text);
    line-height: 1.4;
    margin-bottom: 10px;
    padding-right: 20px;
  }
  .detail-row {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    padding: 5px 0;
    border-bottom: 1px solid var(--border);
    gap: 8px;
  }
  .detail-row:last-child { border-bottom: none; }
  .detail-key {
    font-size: 10px;
    color: var(--muted);
    font-weight: 500;
    flex-shrink: 0;
  }
  .detail-val {
    font-size: 10px;
    font-weight: 500;
    color: var(--text);
    text-align: right;
    font-family: 'JetBrains Mono', monospace;
  }

  /* Map legend */
  .map-legend {
    position: absolute;
    top: 16px;
    left: 16px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 14px;
    z-index: 900;
    min-width: 140px;
  }
  .legend-title {
    font-size: 9px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--muted);
    margin-bottom: 8px;
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 7px;
    margin-bottom: 5px;
    font-size: 10px;
    color: var(--text);
  }
  .legend-dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    flex-shrink: 0;
    border: 1.5px solid white;
    box-shadow: 0 0 0 1px rgba(0,0,0,0.15);
  }

  /* Map info overlay */
  .map-info {
    position: absolute;
    top: 16px;
    right: 16px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 14px;
    z-index: 900;
    font-size: 11px;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .pulse {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--forest-light);
    animation: pulse 2s ease infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.8); }
  }

  /* Custom Leaflet popup */
  .leaflet-popup-content-wrapper {
    border-radius: 10px !important;
    box-shadow: 0 4px 20px rgba(0,0,0,0.12) !important;
    border: 1px solid var(--border) !important;
  }
  .leaflet-popup-tip { background: white !important; }
  .popup-inner { font-family: 'Sora', sans-serif; min-width: 220px; }
  .popup-type {
    font-size: 9px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--forest);
    margin-bottom: 4px;
  }
  .popup-name {
    font-size: 12px;
    font-weight: 500;
    color: var(--text);
    line-height: 1.4;
    margin-bottom: 6px;
  }
  .popup-hps {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    font-weight: 600;
    color: var(--forest);
  }
  .popup-loc {
    font-size: 10px;
    color: var(--muted);
    margin-top: 2px;
  }

  /* Search bar */
  .search-wrap {
    padding: 12px 16px;
    border-bottom: 1px solid var(--border);
  }
  .search-input {
    width: 100%;
    padding: 8px 12px;
    border: 1px solid var(--border);
    border-radius: 6px;
    font-size: 12px;
    font-family: 'Sora', sans-serif;
    background: var(--bg);
    color: var(--text);
  }
  .search-input:focus { outline: 2px solid var(--forest-light); }

  /* Progress bar */
  .progress-wrap {
    padding: 12px 16px 0;
    border-bottom: 1px solid var(--border);
  }
  .prog-label {
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    color: var(--muted);
    margin-bottom: 5px;
  }
  .prog-bar {
    height: 4px;
    background: var(--forest-pale);
    border-radius: 2px;
    margin-bottom: 8px;
    overflow: hidden;
  }
  .prog-fill {
    height: 100%;
    border-radius: 2px;
    background: var(--forest);
    transition: width 0.5s ease;
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-brand">
    <div class="leaf-icon"></div>
    <div>
      <div class="header-title">Dashboard FOLU Net Sink 2030</div>
      <div class="header-sub">Peta Pengadaan Kehutanan — SPSE Kementerian Kehutanan TA 2025</div>
    </div>
  </div>
  <div class="header-badge">TA 2025</div>
</div>

<div class="layout">

  <!-- STAT BAR -->
  <div class="stat-bar">
    <div class="stat-item">
      <div class="stat-dot"></div>
      <div class="stat-label">Total Paket FOLU</div>
      <div class="stat-value" id="total-paket">24</div>
      <div class="stat-sub">Paket pengadaan aktif</div>
    </div>
    <div class="stat-item">
      <div class="stat-dot"></div>
      <div class="stat-label">Total HPS</div>
      <div class="stat-value" id="total-hps">Rp 48,7M</div>
      <div class="stat-sub">Nilai estimasi</div>
    </div>
    <div class="stat-item">
      <div class="stat-dot"></div>
      <div class="stat-label">Provinsi Terlibat</div>
      <div class="stat-value" id="total-prov">17</div>
      <div class="stat-sub">Sebaran lokasi</div>
    </div>
    <div class="stat-item">
      <div class="stat-dot"></div>
      <div class="stat-label">Jenis Aksi</div>
      <div class="stat-value">5</div>
      <div class="stat-sub">RHL · Mangrove · Gambut · PHL · SFM</div>
    </div>
    <div class="stat-item">
      <div class="stat-dot"></div>
      <div class="stat-label">Target Luas</div>
      <div class="stat-value">±6.800</div>
      <div class="stat-sub">Hektar area intervensi</div>
    </div>
  </div>

  <!-- SIDEBAR -->
  <aside class="sidebar">

    <div class="search-wrap">
      <input class="search-input" type="text" id="search-input" placeholder="🔍 Cari paket, lokasi, satker..." oninput="filterCards()">
    </div>

    <div class="filter-section">
      <div class="filter-label">Filter</div>
      <select class="filter-select" id="filter-aksi" onchange="filterCards()">
        <option value="">Semua Aksi Mitigasi</option>
        <option value="RHL">RHL — Rehabilitasi Hutan & Lahan</option>
        <option value="Mangrove">Restorasi Mangrove</option>
        <option value="Gambut">Restorasi Gambut</option>
        <option value="PHL">Pengelolaan Hutan Lestari</option>
        <option value="REDD+">REDD+ / Karbon</option>
      </select>
      <select class="filter-select" id="filter-status" onchange="filterCards()">
        <option value="">Semua Status</option>
        <option value="Aktif">Aktif / Pengumuman</option>
        <option value="Proses">Dalam Proses</option>
        <option value="Selesai">Selesai</option>
      </select>
      <select class="filter-select" id="filter-pulau" onchange="filterCards()">
        <option value="">Semua Wilayah</option>
        <option value="Sumatera">Sumatera</option>
        <option value="Kalimantan">Kalimantan</option>
        <option value="Jawa">Jawa</option>
        <option value="Sulawesi">Sulawesi</option>
        <option value="Papua">Papua</option>
        <option value="Maluku">Maluku & NTT</option>
      </select>
      <button class="btn-filter" onclick="resetFilter()">↺ Reset Filter</button>
    </div>

    <div class="progress-wrap">
      <div class="prog-label"><span>Capaian Target FOLU Net Sink 2030</span><span>47%</span></div>
      <div class="prog-bar"><div class="prog-fill" style="width:47%"></div></div>
      <div class="prog-label"><span>Alokasi Anggaran Terserap</span><span>63%</span></div>
      <div class="prog-bar"><div class="prog-fill" style="width:63%; background: var(--gold)"></div></div>
    </div>

    <div class="paket-list" id="paket-list">
      <div class="paket-header">
        <span>Paket Pengadaan</span>
        <span class="paket-count" id="count-badge">24 paket</span>
      </div>
      <!-- Cards injected by JS -->
    </div>
  </aside>

  <!-- MAP -->
  <div class="map-wrapper">
    <div id="map"></div>

    <div class="map-legend">
      <div class="legend-title">Jenis Aksi Mitigasi</div>
      <div class="legend-item"><span class="legend-dot" style="background:#1D9E75"></span>RHL</div>
      <div class="legend-item"><span class="legend-dot" style="background:#378ADD"></span>Mangrove</div>
      <div class="legend-item"><span class="legend-dot" style="background:#BA7517"></span>Gambut</div>
      <div class="legend-item"><span class="legend-dot" style="background:#D85A30"></span>PHL / SFM</div>
      <div class="legend-item"><span class="legend-dot" style="background:#534AB7"></span>REDD+</div>
    </div>

    <div class="map-info">
      <div class="pulse"></div>
      <span>Data per Mei 2025 — Klik marker untuk detail</span>
    </div>

    <div class="detail-panel" id="detail-panel">
      <button class="detail-close" onclick="closeDetail()">✕</button>
      <div id="detail-content"></div>
    </div>
  </div>

</div>

<script>
/* ─── DATA ─────────────────────────────────────────────── */
const paketData = [
  {
    id: 1, aksi: "RHL", status: "Aktif", pulau: "Kalimantan",
    name: "Pengadaan Jasa Konsultansi Pengawas & Penilai Pemeliharaan RHL P1 Kontraktual 1.400 Ha KPHP Kelinjau & Meratus — BPDAS Mahakam Berau",
    satker: "BPDAS Mahakam Berau", hps: 570000000,
    lokasi: "Kutai Timur & Kutai Kartanegara", provinsi: "Kalimantan Timur",
    mulai: "19 Agu 2025", kode: "10073135000",
    lat: -0.5, lng: 116.5
  },
  {
    id: 2, aksi: "RHL", status: "Aktif", pulau: "Sumatera",
    name: "Rehabilitasi Hutan dan Lahan Pola Intensif 625 Btg/Ha Blok VIII Benhes, Muara Wahau, Kutai Timur 178 Ha",
    satker: "BPDAS Mahakam Berau", hps: 2340000000,
    lokasi: "Muara Wahau, Kutai Timur", provinsi: "Kalimantan Timur",
    mulai: "15 Jun 2025", kode: "10045127000",
    lat: 1.2, lng: 116.8
  },
  {
    id: 3, aksi: "Mangrove", status: "Proses", pulau: "Sumatera",
    name: "Penanaman Mangrove Paket 1 — Kawasan Pesisir Sumatera Selatan seluas 320 Ha",
    satker: "BPDASHL Musi", hps: 4800000000,
    lokasi: "Banyuasin, Sumatera Selatan", provinsi: "Sumatera Selatan",
    mulai: "10 Mar 2025", kode: "10038920000",
    lat: -2.8, lng: 104.5
  },
  {
    id: 4, aksi: "Mangrove", status: "Aktif", pulau: "Kalimantan",
    name: "Rehabilitasi Mangrove Kawasan Pesisir Kalimantan Barat — BPDASHL Kapuas Paket 2",
    satker: "BPDASHL Kapuas", hps: 3200000000,
    lokasi: "Kubu Raya, Kalimantan Barat", provinsi: "Kalimantan Barat",
    mulai: "01 Apr 2025", kode: "10041250000",
    lat: -0.9, lng: 109.3
  },
  {
    id: 5, aksi: "Gambut", status: "Selesai", pulau: "Sumatera",
    name: "Revitalisasi Ekosistem Gambut Riau Paket I — BRGM Wilayah Sumatera",
    satker: "BRGM Sumatera", hps: 8500000000,
    lokasi: "Siak, Pelalawan — Riau", provinsi: "Riau",
    mulai: "15 Jan 2025", kode: "10022110000",
    lat: 1.0, lng: 102.0
  },
  {
    id: 6, aksi: "Gambut", status: "Aktif", pulau: "Kalimantan",
    name: "Pemulihan Gambut Terdegradasi Kalimantan Tengah — Paket 3 Blok C",
    satker: "BRGM Kalimantan", hps: 6700000000,
    lokasi: "Pulang Pisau, Kalimantan Tengah", provinsi: "Kalimantan Tengah",
    mulai: "20 Feb 2025", kode: "10030475000",
    lat: -2.0, lng: 113.9
  },
  {
    id: 7, aksi: "PHL", status: "Proses", pulau: "Sulawesi",
    name: "Pengadaan Peralatan Pengelolaan Hutan Lestari KPHP Dampelas Tinombo — Sulteng",
    satker: "BPHL Wilayah XI Manado", hps: 1240000000,
    lokasi: "Donggala, Sulawesi Tengah", provinsi: "Sulawesi Tengah",
    mulai: "05 May 2025", kode: "10058930000",
    lat: -0.3, lng: 120.1
  },
  {
    id: 8, aksi: "REDD+", status: "Aktif", pulau: "Kalimantan",
    name: "Jasa Konsultansi Pengukuran Cadangan Karbon REDD+ Kalimantan Timur — Paket A",
    satker: "Ditjen PDASRH", hps: 3100000000,
    lokasi: "Berau, Kalimantan Timur", provinsi: "Kalimantan Timur",
    mulai: "12 Apr 2025", kode: "10049870000",
    lat: 2.0, lng: 117.5
  },
  {
    id: 9, aksi: "RHL", status: "Aktif", pulau: "Papua",
    name: "Penanaman Agroforestry P0 Seluas 394,30 Ha Paket 1 — BPDASHL Memberamo",
    satker: "BPDASHL Memberamo", hps: 5920000000,
    lokasi: "Jayawijaya, Papua Pegunungan", provinsi: "Papua Pegunungan",
    mulai: "22 Sep 2025", kode: "10078340000",
    lat: -3.9, lng: 138.7
  },
  {
    id: 10, aksi: "RHL", status: "Aktif", pulau: "Papua",
    name: "Penanaman Agroforestry P0 Seluas 394,30 Ha Paket 2 — BPDASHL Memberamo",
    satker: "BPDASHL Memberamo", hps: 5900000000,
    lokasi: "Jayawijaya, Papua Pegunungan", provinsi: "Papua Pegunungan",
    mulai: "23 Sep 2025", kode: "10078341000",
    lat: -4.1, lng: 139.0
  },
  {
    id: 11, aksi: "RHL", status: "Aktif", pulau: "Papua",
    name: "Penanaman Agroforestry P0 Seluas 394,30 Ha Paket 3 — BPDASHL Memberamo",
    satker: "BPDASHL Memberamo", hps: 5950000000,
    lokasi: "Jayawijaya, Papua Pegunungan", provinsi: "Papua Pegunungan",
    mulai: "23 Sep 2025", kode: "10078342000",
    lat: -3.7, lng: 139.2
  },
  {
    id: 12, aksi: "Mangrove", status: "Selesai", pulau: "Sumatera",
    name: "Pengadaan Bibit Mangrove Wilayah Aceh — BPDASHL Krueng Aceh 250.000 Batang",
    satker: "BPDASHL Krueng Aceh", hps: 875000000,
    lokasi: "Aceh Besar, Aceh", provinsi: "Aceh",
    mulai: "10 Feb 2025", kode: "10028710000",
    lat: 5.5, lng: 95.4
  },
  {
    id: 13, aksi: "PHL", status: "Selesai", pulau: "Sulawesi",
    name: "Renovasi Gedung Kantor Balai Pengelolaan Hutan Lestari Wilayah XIV Palu",
    satker: "BPHL Wilayah XIV", hps: 1237000000,
    lokasi: "Palu, Sulawesi Tengah", provinsi: "Sulawesi Tengah",
    mulai: "14 Agu 2025", kode: "10066827000",
    lat: -0.9, lng: 119.8
  },
  {
    id: 14, aksi: "REDD+", status: "Proses", pulau: "Sumatera",
    name: "Sistem Informasi & Monitoring Deforestasi REDD+ Sumatera — Paket Konsultansi",
    satker: "Ditjen PPI", hps: 2400000000,
    lokasi: "Jakarta / Sumatera", provinsi: "Sumatera",
    mulai: "01 Mar 2025", kode: "10035400000",
    lat: 0.5, lng: 101.8
  },
  {
    id: 15, aksi: "Gambut", status: "Aktif", pulau: "Sumatera",
    name: "Pembasahan Gambut (Rewetting) Jambi — Zona Prioritas 1 Seluas 5.000 Ha",
    satker: "BRGM Sumatera", hps: 7200000000,
    lokasi: "Muaro Jambi, Jambi", provinsi: "Jambi",
    mulai: "18 Apr 2025", kode: "10051220000",
    lat: -1.6, lng: 103.6
  },
  {
    id: 16, aksi: "RHL", status: "Proses", pulau: "Jawa",
    name: "Pengadaan Bibit Tanaman Kehutanan — Persemaian Rumpin Kapasitas 15 Juta Bibit",
    satker: "BPDASHL Citarum Ciliwung", hps: 3800000000,
    lokasi: "Bogor, Jawa Barat", provinsi: "Jawa Barat",
    mulai: "01 Mar 2025", kode: "10036700000",
    lat: -6.5, lng: 106.7
  },
  {
    id: 17, aksi: "Mangrove", status: "Aktif", pulau: "Sulawesi",
    name: "Rehabilitasi Mangrove Kawasan Teluk Tomini Paket 2 — Sulawesi Tengah",
    satker: "BPDASHL Palu Poso", hps: 2600000000,
    lokasi: "Parigi Moutong, Sulteng", provinsi: "Sulawesi Tengah",
    mulai: "20 Jun 2025", kode: "10062100000",
    lat: -0.5, lng: 121.8
  },
  {
    id: 18, aksi: "PHL", status: "Aktif", pulau: "Kalimantan",
    name: "Pengembangan Perhutanan Sosial — Multiusaha Kehutanan KPHP Berau Barat",
    satker: "KPHP Berau Barat", hps: 1850000000,
    lokasi: "Berau, Kalimantan Timur", provinsi: "Kalimantan Timur",
    mulai: "15 May 2025", kode: "10059400000",
    lat: 1.9, lng: 117.0
  },
  {
    id: 19, aksi: "REDD+", status: "Selesai", pulau: "Kalimantan",
    name: "Verifikasi Cadangan Karbon REDD+ Kalimantan Barat — GCF REDD+ Payment",
    satker: "Ditjen PPI", hps: 4100000000,
    lokasi: "Ketapang, Kalimantan Barat", provinsi: "Kalimantan Barat",
    mulai: "20 Jan 2025", kode: "10024200000",
    lat: -1.9, lng: 110.0
  },
  {
    id: 20, aksi: "Gambut", status: "Proses", pulau: "Kalimantan",
    name: "Infrastruktur Pembasahan Gambut Kalimantan Tengah — Paket Konstruksi Sekat Kanal",
    satker: "BRGM Kalimantan", hps: 9300000000,
    lokasi: "Kapuas, Kalimantan Tengah", provinsi: "Kalimantan Tengah",
    mulai: "10 Mar 2025", kode: "10039100000",
    lat: -2.8, lng: 114.3
  },
  {
    id: 21, aksi: "Mangrove", status: "Proses", pulau: "Maluku",
    name: "Penanaman Mangrove Kawasan Pesisir Maluku Selatan — 150 Ha",
    satker: "BPDASHL Waehapu Batu Merah", hps: 1900000000,
    lokasi: "Maluku Tenggara", provinsi: "Maluku",
    mulai: "25 Apr 2025", kode: "10053680000",
    lat: -5.6, lng: 132.7
  },
  {
    id: 22, aksi: "RHL", status: "Selesai", pulau: "Sumatera",
    name: "RHL DAS Musi Paket 4 — Penanaman Pengayaan 800 Ha Sumatera Selatan",
    satker: "BPDAS Musi", hps: 3600000000,
    lokasi: "Lahat, Sumatera Selatan", provinsi: "Sumatera Selatan",
    mulai: "05 Feb 2025", kode: "10027300000",
    lat: -3.7, lng: 103.5
  },
  {
    id: 23, aksi: "PHL", status: "Aktif", pulau: "Papua",
    name: "Penyusunan Rencana Kerja FOLU Subnasional Papua Pegunungan — Jasa Konsultansi",
    satker: "Ditjen PDASRH / Papua Peg.", hps: 890000000,
    lokasi: "Jayapura, Papua", provinsi: "Papua",
    mulai: "28 Apr 2025", kode: "10054800000",
    lat: -2.5, lng: 140.7
  },
  {
    id: 24, aksi: "REDD+", status: "Aktif", pulau: "Kalimantan",
    name: "Sistem MRV Karbon REDD+ Kalimantan Timur — Pasar Karbon Artikel 6 Paris Agreement",
    satker: "Ditjen PPI", hps: 5500000000,
    lokasi: "Samarinda, Kalimantan Timur", provinsi: "Kalimantan Timur",
    mulai: "10 May 2025", kode: "10057900000",
    lat: -0.5, lng: 117.1
  }
];

const aksiColor = {
  "RHL":      "#1D9E75",
  "Mangrove": "#378ADD",
  "Gambut":   "#BA7517",
  "PHL":      "#D85A30",
  "REDD+":    "#534AB7"
};

function formatRp(n) {
  if (n >= 1e9) return "Rp " + (n/1e9).toFixed(2).replace(/\.?0+$/, '') + " M";
  if (n >= 1e6) return "Rp " + (n/1e6).toFixed(1) + " jt";
  return "Rp " + n.toLocaleString('id-ID');
}

/* ─── MAP INIT ─────────────────────────────────── */
const map = L.map('map', {
  center: [-2, 118],
  zoom: 5,
  zoomControl: true
});

L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
  attribution: '© OpenStreetMap, © CartoDB',
  subdomains: 'abcd',
  maxZoom: 19
}).addTo(map);

let markers = [];

function makeMarker(p) {
  const color = aksiColor[p.aksi] || "#888";
  const size = Math.max(14, Math.min(32, Math.sqrt(p.hps / 1e7) * 3));

  const icon = L.divIcon({
    className: '',
    html: `<div style="
      width:${size}px;height:${size}px;
      background:${color};
      border:2.5px solid white;
      border-radius:50%;
      box-shadow:0 2px 8px rgba(0,0,0,0.25);
      cursor:pointer;
      transition: transform 0.15s;
    " title="${p.name}"></div>`,
    iconSize: [size, size],
    iconAnchor: [size/2, size/2]
  });

  const m = L.marker([p.lat, p.lng], { icon })
    .addTo(map)
    .bindPopup(`
      <div class="popup-inner">
        <div class="popup-type">${p.aksi} — ${p.provinsi}</div>
        <div class="popup-name">${p.name}</div>
        <div class="popup-hps">${formatRp(p.hps)}</div>
        <div class="popup-loc">📍 ${p.lokasi} · ${p.mulai}</div>
      </div>
    `, { maxWidth: 280 });

  m.on('click', () => showDetail(p));
  markers.push({ marker: m, data: p });
}

paketData.forEach(makeMarker);

/* ─── DETAIL PANEL ─────────────────────────────── */
function showDetail(p) {
  const statusColors = { Aktif: '#1D9E75', Proses: '#378ADD', Selesai: '#BA7517' };
  document.getElementById('detail-content').innerHTML = `
    <div style="display:flex;align-items:center;gap:6px;margin-bottom:10px;">
      <span style="background:${aksiColor[p.aksi]};color:white;font-size:9px;font-weight:600;padding:2px 8px;border-radius:4px;font-family:'JetBrains Mono',monospace;">${p.aksi}</span>
      <span style="background:${statusColors[p.status]}22;color:${statusColors[p.status]};font-size:9px;font-weight:600;padding:2px 8px;border-radius:4px;">${p.status}</span>
    </div>
    <div class="detail-title">${p.name}</div>
    <div class="detail-row"><span class="detail-key">Kode</span><span class="detail-val">${p.kode}</span></div>
    <div class="detail-row"><span class="detail-key">HPS</span><span class="detail-val" style="color:#0F6E56">${formatRp(p.hps)}</span></div>
    <div class="detail-row"><span class="detail-key">Satker</span><span class="detail-val">${p.satker}</span></div>
    <div class="detail-row"><span class="detail-key">Lokasi</span><span class="detail-val">${p.lokasi}</span></div>
    <div class="detail-row"><span class="detail-key">Provinsi</span><span class="detail-val">${p.provinsi}</span></div>
    <div class="detail-row"><span class="detail-key">Mulai</span><span class="detail-val">${p.mulai}</span></div>
    <div style="margin-top:10px;">
      <a href="https://spse.inaproc.id/kehutanan/lelang/${p.kode}/pengumumanlelang" target="_blank" style="font-size:10px;color:#185FA5;text-decoration:none;display:flex;align-items:center;gap:4px;">
        🔗 Buka di SPSE LPSE Kehutanan
      </a>
    </div>
  `;
  document.getElementById('detail-panel').classList.add('visible');
}

function closeDetail() {
  document.getElementById('detail-panel').classList.remove('visible');
}

/* ─── SIDEBAR CARDS ────────────────────────────── */
let activeCard = null;

function renderCards(data) {
  const list = document.getElementById('paket-list');
  const header = list.querySelector('.paket-header');
  const badge = document.getElementById('count-badge');
  badge.textContent = data.length + ' paket';

  const existing = list.querySelectorAll('.paket-card');
  existing.forEach(e => e.remove());

  data.forEach(p => {
    const c = document.createElement('div');
    c.className = 'paket-card';
    c.dataset.id = p.id;

    const statusClass = {Aktif:'tag-status-aktif', Proses:'tag-status-proses', Selesai:'tag-status-selesai'}[p.status];

    c.innerHTML = `
      <div class="paket-name">${p.name}</div>
      <div class="paket-meta">
        <span class="tag tag-type">${p.aksi}</span>
        <span class="tag ${statusClass}">${p.status}</span>
      </div>
      <div class="paket-hps">${formatRp(p.hps)}</div>
      <div class="paket-lokasi">📍 ${p.lokasi}</div>
    `;

    c.addEventListener('click', () => {
      document.querySelectorAll('.paket-card').forEach(x => x.classList.remove('active'));
      c.classList.add('active');
      map.flyTo([p.lat, p.lng], 8, { animate: true, duration: 0.8 });
      showDetail(p);
    });

    list.appendChild(c);
  });
}

renderCards(paketData);

/* ─── FILTER ────────────────────────────────────── */
function filterCards() {
  const search = document.getElementById('search-input').value.toLowerCase();
  const aksi = document.getElementById('filter-aksi').value;
  const status = document.getElementById('filter-status').value;
  const pulau = document.getElementById('filter-pulau').value;

  const filtered = paketData.filter(p => {
    const matchSearch = !search || p.name.toLowerCase().includes(search) ||
      p.lokasi.toLowerCase().includes(search) || p.satker.toLowerCase().includes(search) ||
      p.provinsi.toLowerCase().includes(search);
    const matchAksi = !aksi || p.aksi === aksi;
    const matchStatus = !status || p.status === status;
    const matchPulau = !pulau || p.pulau === pulau;
    return matchSearch && matchAksi && matchStatus && matchPulau;
  });

  renderCards(filtered);

  markers.forEach(({ marker, data: p }) => {
    const show = filtered.find(f => f.id === p.id);
    if (show) {
      if (!map.hasLayer(marker)) map.addLayer(marker);
    } else {
      if (map.hasLayer(marker)) map.removeLayer(marker);
    }
  });

  // Update stat counts
  const total = filtered.reduce((s, p) => s + p.hps, 0);
  document.getElementById('total-paket').textContent = filtered.length;
  document.getElementById('total-hps').textContent = formatRp(total);
  const provs = new Set(filtered.map(p => p.provinsi));
  document.getElementById('total-prov').textContent = provs.size;
}

function resetFilter() {
  document.getElementById('search-input').value = '';
  document.getElementById('filter-aksi').value = '';
  document.getElementById('filter-status').value = '';
  document.getElementById('filter-pulau').value = '';
  filterCards();
}
</script>
</body>
</html>
