# EthnoMed-Deliverable
Community Network Map
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Seattle Afghan Resources / منابع افغان‌ها در سیاتل</title>
<link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;500;700&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<link href="https://api.mapbox.com/mapbox-gl-js/v3.3.0/mapbox-gl.css" rel="stylesheet">
<script src="https://api.mapbox.com/mapbox-gl-js/v3.3.0/mapbox-gl.js"></script>
<style>
  :root {
    --bg: #0f1117;
    --surface: #1a1d27;
    --surface2: #22263a;
    --accent: #c8a96e;
    --accent2: #7eb8b0;
    --text: #f0ece4;
    --text-muted: #8a8fa8;
    --border: rgba(200, 169, 110, 0.2);
    --sidebar-w: 360px;
    --font-latin: 'DM Sans', sans-serif;
    --font-farsi: 'Vazirmatn', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: var(--font-latin);
    background: var(--bg);
    color: var(--text);
    height: 100vh;
    overflow: hidden;
    display: flex;
  }

  body.farsi {
    font-family: var(--font-farsi);
    direction: rtl;
  }

  /* ── Sidebar ── */
  #sidebar {
    width: var(--sidebar-w);
    min-width: var(--sidebar-w);
    background: var(--surface);
    display: flex;
    flex-direction: column;
    border-right: 1px solid var(--border);
    z-index: 10;
    overflow: hidden;
  }

  body.farsi #sidebar {
    border-right: none;
    border-left: 1px solid var(--border);
  }

  /* Header */
  #header {
    padding: 20px 20px 16px;
    border-bottom: 1px solid var(--border);
    background: var(--surface);
  }

  .lang-toggle {
    display: flex;
    gap: 6px;
    margin-bottom: 16px;
    background: var(--bg);
    border-radius: 8px;
    padding: 4px;
  }

  .lang-btn {
    flex: 1;
    padding: 6px 12px;
    border: none;
    border-radius: 6px;
    background: transparent;
    color: var(--text-muted);
    cursor: pointer;
    font-size: 13px;
    font-weight: 500;
    font-family: inherit;
    transition: all 0.2s;
  }

  .lang-btn.active {
    background: var(--accent);
    color: #0f1117;
  }

  .logo-area h1 {
    font-size: 15px;
    font-weight: 600;
    color: var(--text);
    line-height: 1.4;
  }

  .logo-area .subtitle {
    font-size: 12px;
    color: var(--text-muted);
    margin-top: 2px;
  }

  .logo-area .farsi-title {
    font-family: var(--font-farsi);
    font-size: 14px;
    color: var(--accent);
    margin-top: 4px;
    direction: rtl;
  }

  /* Filters */
  #filters {
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
  }

  .filter-label {
    font-size: 10px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--text-muted);
    margin-bottom: 10px;
  }

  body.farsi .filter-label {
    letter-spacing: 0;
    font-size: 11px;
  }

  .filter-chips {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }

  .chip {
    padding: 5px 11px;
    border-radius: 20px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--text-muted);
    font-size: 12px;
    cursor: pointer;
    font-family: inherit;
    transition: all 0.2s;
    white-space: nowrap;
  }

  .chip.active {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(200,169,110,0.08);
  }

  .chip:hover { border-color: var(--accent2); color: var(--accent2); }

  /* Resource list */
  #resource-list {
    flex: 1;
    overflow-y: auto;
    padding: 12px 12px;
  }

  #resource-list::-webkit-scrollbar { width: 4px; }
  #resource-list::-webkit-scrollbar-track { background: transparent; }
  #resource-list::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }

  .resource-card {
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px;
    margin-bottom: 8px;
    cursor: pointer;
    transition: all 0.2s;
    background: var(--bg);
  }

  .resource-card:hover, .resource-card.active {
    border-color: var(--accent);
    background: rgba(200,169,110,0.05);
  }

  .card-top {
    display: flex;
    align-items: flex-start;
    gap: 10px;
  }

  .card-icon {
    width: 34px;
    height: 34px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
  }

  .card-body { flex: 1; min-width: 0; }

  .card-name {
    font-size: 13px;
    font-weight: 600;
    color: var(--text);
    line-height: 1.3;
  }

  .card-name-farsi {
    font-family: var(--font-farsi);
    font-size: 12px;
    color: var(--accent);
    margin-top: 2px;
    direction: rtl;
    text-align: right;
  }

  body.farsi .card-name-farsi {
    font-size: 13px;
    color: var(--text);
    font-weight: 600;
  }
  body.farsi .card-name {
    font-size: 11px;
    color: var(--text-muted);
  }

  .card-category {
    font-size: 10px;
    color: var(--text-muted);
    margin-top: 3px;
    display: flex;
    align-items: center;
    gap: 4px;
  }

  .card-detail {
    margin-top: 8px;
    font-size: 11.5px;
    color: var(--text-muted);
    line-height: 1.5;
  }

  body.farsi .card-detail {
    font-family: var(--font-farsi);
    font-size: 12px;
    text-align: right;
  }

  .card-tags {
    margin-top: 8px;
    display: flex;
    flex-wrap: wrap;
    gap: 4px;
  }

  .tag {
    font-size: 10px;
    padding: 2px 7px;
    border-radius: 4px;
    background: rgba(126,184,176,0.12);
    color: var(--accent2);
    border: 1px solid rgba(126,184,176,0.2);
  }

  .card-phone {
    margin-top: 8px;
    font-size: 11px;
    color: var(--accent);
    display: flex;
    align-items: center;
    gap: 4px;
  }

  /* Footer */
  #footer {
    padding: 12px 20px;
    border-top: 1px solid var(--border);
    font-size: 11px;
    color: var(--text-muted);
    text-align: center;
  }

  /* Map */
  #map-container {
    flex: 1;
    position: relative;
  }

  #map { width: 100%; height: 100%; }

  /* Popup */
  .mapboxgl-popup-content {
    background: var(--surface) !important;
    border: 1px solid var(--border) !important;
    border-radius: 10px !important;
    padding: 14px 16px !important;
    color: var(--text) !important;
    font-family: var(--font-latin);
    max-width: 260px;
    box-shadow: 0 8px 32px rgba(0,0,0,0.5) !important;
  }

  .mapboxgl-popup-tip { display: none !important; }

  .popup-name {
    font-size: 13px;
    font-weight: 600;
    margin-bottom: 2px;
  }

  .popup-farsi {
    font-family: var(--font-farsi);
    font-size: 12px;
    color: var(--accent);
    margin-bottom: 8px;
    direction: rtl;
  }

  .popup-desc {
    font-size: 11.5px;
    color: var(--text-muted);
    line-height: 1.5;
    margin-bottom: 8px;
  }

  .popup-phone {
    font-size: 11px;
    color: var(--accent);
    margin-bottom: 6px;
  }

  .popup-btn {
    display: block;
    text-align: center;
    padding: 6px 12px;
    border-radius: 6px;
    background: rgba(200,169,110,0.12);
    border: 1px solid var(--border);
    color: var(--accent);
    text-decoration: none;
    font-size: 11px;
    cursor: pointer;
    font-family: inherit;
    width: 100%;
  }

  .popup-btn:hover { background: rgba(200,169,110,0.2); }

  /* Map controls hint */
  .map-hint {
    position: absolute;
    bottom: 30px;
    right: 20px;
    background: rgba(26,29,39,0.9);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 8px 12px;
    font-size: 11px;
    color: var(--text-muted);
    pointer-events: none;
  }

  /* Category colors */
  .cat-legal    { background: rgba(200,169,110,0.15); color: #c8a96e; }
  .cat-health   { background: rgba(126,184,176,0.15); color: #7eb8b0; }
  .cat-housing  { background: rgba(180,140,200,0.15); color: #b48cc8; }
  .cat-community{ background: rgba(240,160,100,0.15); color: #f0a064; }
  .cat-education{ background: rgba(100,180,130,0.15); color: #64b482; }
  .cat-resettlement{ background: rgba(160,200,240,0.15); color: #a0c8f0; }

  /* Count badge */
  .count-badge {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background: var(--accent);
    color: #0f1117;
    font-size: 10px;
    font-weight: 700;
    min-width: 18px;
    height: 18px;
    border-radius: 9px;
    padding: 0 5px;
    margin-left: 6px;
  }

  body.farsi .count-badge { margin-left: 0; margin-right: 6px; }

  .empty-state {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-muted);
    font-size: 13px;
  }

  /* Mobile */
  @media (max-width: 700px) {
    body { flex-direction: column; }
    #sidebar { width: 100%; min-width: unset; height: 50vh; border-right: none; border-bottom: 1px solid var(--border); }
    #map-container { height: 50vh; }
  }
</style>
</head>
<body>

<div id="sidebar">
  <div id="header">
    <div class="lang-toggle">
      <button class="lang-btn active" onclick="setLang('en')" id="btn-en">English</button>
      <button class="lang-btn" onclick="setLang('fa')" id="btn-fa" style="font-family: 'Vazirmatn', sans-serif;">فارسی / دری</button>
    </div>
    <div class="logo-area">
      <h1 class="t-en">Afghan Community Resources</h1>
      <div class="subtitle t-en">Greater Seattle Area · King County</div>
      <div class="farsi-title t-fa" style="display:none">منابع جامعه افغانستانی سیاتل</div>
    </div>
  </div>

  <div id="filters">
    <div class="filter-label t-en">Filter by service</div>
    <div class="filter-label t-fa" style="display:none">فیلتر بر اساس خدمات</div>
    <div class="filter-chips">
      <button class="chip active" data-cat="all" onclick="filterCat('all')">
        <span class="t-en">All</span><span class="t-fa" style="display:none">همه</span>
      </button>
      <button class="chip" data-cat="resettlement" onclick="filterCat('resettlement')">
        <span class="t-en">Resettlement</span><span class="t-fa" style="display:none">اسکان مجدد</span>
      </button>
      <button class="chip" data-cat="legal" onclick="filterCat('legal')">
        <span class="t-en">Legal</span><span class="t-fa" style="display:none">حقوقی</span>
      </button>
      <button class="chip" data-cat="health" onclick="filterCat('health')">
        <span class="t-en">Health</span><span class="t-fa" style="display:none">صحت</span>
      </button>
      <button class="chip" data-cat="community" onclick="filterCat('community')">
        <span class="t-en">Community</span><span class="t-fa" style="display:none">جامعه</span>
      </button>
      <button class="chip" data-cat="education" onclick="filterCat('education')">
        <span class="t-en">Education</span><span class="t-fa" style="display:none">آموزش</span>
      </button>
    </div>
  </div>

  <div id="resource-list"></div>

  <div id="footer">
    <span class="t-en">Last updated May 2026 · Verify hours before visiting</span>
    <span class="t-fa" style="display:none;font-family:'Vazirmatn',sans-serif">آخرین بروزرسانی: می ۲۰۲۶ · قبل از مراجعه ساعات کاری را تأیید کنید</span>
  </div>
</div>

<div id="map-container">
  <div id="map"></div>
  <div class="map-hint t-en">Click a marker to see details</div>
  <div class="map-hint t-fa" style="display:none;font-family:'Vazirmatn',sans-serif">روی نقطه کلیک کنید</div>
</div>

<script>
// ── Data ──────────────────────────────────────────────────────────────────
const resources = [
  {
    id: 1,
    name: "Kabul Seattle Community Services",
    nameFa: "خدمات جامعه کابل سیاتل",
    category: "community",
    icon: "🤝",
    lat: 47.3684, lng: -122.2304,
    address: "1209 Central Ave S, Suite 140, Kent, WA 98032",
    phone: "(206) 850-1034",
    website: "https://kabulseattle.org",
    descEn: "Afghan-led nonprofit offering family programs, youth resilience, and English classes for women.",
    descFa: "سازمان غیرانتفاعی افغانی با برنامه‌های خانوادگی، جوانان و کلاس‌های انگلیسی برای خانم‌ها.",
    tags: ["Dari", "Family", "Youth"]
  },
  {
    id: 2,
    name: "IRC — International Rescue Committee",
    nameFa: "کمیته بین‌المللی نجات",
    category: "resettlement",
    icon: "🏠",
    lat: 47.4314, lng: -122.3193,
    address: "1200 S 192nd St, SeaTac, WA 98148",
    phone: "(206) 623-2105",
    website: "https://www.rescue.org/united-states/seattle-wa",
    descEn: "Full resettlement services: employment, legal aid, cash assistance, English classes.",
    descFa: "خدمات کامل اسکان: اشتغال، کمک حقوقی، کمک نقدی، کلاس‌های انگلیسی.",
    tags: ["Dari interpreters", "Employment", "Legal"]
  },
  {
    id: 3,
    name: "ReWA — Refugee Women's Alliance",
    nameFa: "اتحادیه زنان پناهنده",
    category: "health",
    icon: "💜",
    lat: 47.5677, lng: -122.2961,
    address: "4008 Martin Luther King Jr Way S, Seattle, WA 98108",
    phone: "(206) 721-0243",
    website: "https://www.rewa.org",
    descEn: "The only DV program in the region with advocates fluent in Dari, Farsi and Pashtu.",
    descFa: "تنها برنامه خشونت خانگی در منطقه با مشاورین صحبت‌کننده به دری، فارسی و پشتو.",
    tags: ["Dari", "Farsi", "Pashtu", "Women", "DV"]
  },
  {
    id: 4,
    name: "World Relief Western Washington",
    nameFa: "امداد جهانی واشنگتن غربی",
    category: "resettlement",
    icon: "🌍",
    lat: 47.3879, lng: -122.2969,
    address: "23835 Pacific Hwy S, Suite 100, Kent, WA 98032",
    phone: "(253) 277-1121",
    website: "https://worldrelief.org/western-wa",
    descEn: "Holistic resettlement: housing, employment, English classes, community support.",
    descFa: "اسکان جامع: مسکن، اشتغال، کلاس‌های انگلیسی، حمایت اجتماعی.",
    tags: ["Housing", "Employment", "ESL"]
  },
  {
    id: 5,
    name: "Jewish Family Service of Seattle",
    nameFa: "خدمات خانواده یهودیان سیاتل",
    category: "resettlement",
    icon: "🍎",
    lat: 47.6156, lng: -122.3119,
    address: "1601 16th Ave, Seattle, WA 98122",
    phone: "(206) 461-3240",
    website: "https://www.jfsseattle.org",
    descEn: "Food bank, immigration services, and resettlement support for Afghan families.",
    descFa: "بانک غذا، خدمات مهاجرتی و حمایت اسکان برای خانواده‌های افغانستانی.",
    tags: ["Food", "Immigration", "Refugees"]
  },
  {
    id: 6,
    name: "AACO — Afghan-American Community Org.",
    nameFa: "سازمان جامعه افغانی-امریکایی",
    category: "community",
    icon: "🦅",
    lat: 47.6062, lng: -122.3321,
    address: "Seattle, WA (online + events)",
    phone: null,
    website: "https://aa-co.org",
    descEn: "Education, civic engagement, scholarships, and Dari/Farsi rights guides.",
    descFa: "آموزش، مشارکت مدنی، بورسیه‌ها و راهنماهای حقوقی به دری/فارسی.",
    tags: ["Dari", "Farsi", "Advocacy", "Education"]
  },
  {
    id: 7,
    name: "WA Dept. of Health — Afghan Resources",
    nameFa: "وزارت صحت واشنگتن",
    category: "health",
    icon: "🏥",
    lat: 47.5001, lng: -122.4500,
    address: "Online resource hub",
    phone: null,
    website: "https://doh.wa.gov",
    descEn: "Health fact sheets in Dari and Farsi: TB, vaccines, WIC, COVID-19.",
    descFa: "برگه‌های صحی به دری و فارسی: سل، واکسین‌ها، WIC، کووید-۱۹.",
    tags: ["Dari", "Farsi", "Vaccines", "WIC"]
  },
  {
    id: 8,
    name: "Afghan Health Initiative",
    nameFa: "ابتکار صحت افغانستانی",
    category: "health",
    icon: "❤️",
    lat: 47.3800, lng: -122.2350,
    address: "King County, WA",
    phone: "(253) 237-6214",
    website: "https://www.afghanhealth.org",
    descEn: "Addresses health disparities in the Afghan community. Food fund and eviction prevention.",
    descFa: "رسیدگی به نابرابری‌های صحی در جامعه افغانستانی. صندوق غذا و جلوگیری از اخراج.",
    tags: ["Health", "Food", "Afghan-led"]
  }
];

// ── State ─────────────────────────────────────────────────────────────────
let currentLang = 'en';
let currentCat = 'all';
let activeId = null;
let markers = {};
let popup = null;
let map;

// ── Language ──────────────────────────────────────────────────────────────
function setLang(lang) {
  currentLang = lang;
  document.body.classList.toggle('farsi', lang === 'fa');
  document.documentElement.lang = lang === 'fa' ? 'fa' : 'en';
  document.documentElement.dir = lang === 'fa' ? 'rtl' : 'ltr';

  document.querySelectorAll('.t-en').forEach(el => el.style.display = lang === 'en' ? '' : 'none');
  document.querySelectorAll('.t-fa').forEach(el => el.style.display = lang === 'fa' ? '' : 'none');

  document.getElementById('btn-en').classList.toggle('active', lang === 'en');
  document.getElementById('btn-fa').classList.toggle('active', lang === 'fa');

  renderList();
}

// ── Filter ────────────────────────────────────────────────────────────────
function filterCat(cat) {
  currentCat = cat;
  document.querySelectorAll('.chip').forEach(c => c.classList.toggle('active', c.dataset.cat === cat));
  renderList();
  updateMarkers();
}

function filteredResources() {
  if (currentCat === 'all') return resources;
  return resources.filter(r => r.category === currentCat);
}

// ── Sidebar list ──────────────────────────────────────────────────────────
const catColors = {
  legal: 'cat-legal', health: 'cat-health', housing: 'cat-housing',
  community: 'cat-community', education: 'cat-education', resettlement: 'cat-resettlement'
};

const catLabelEn = {
  legal:'Legal', health:'Health', housing:'Housing',
  community:'Community', education:'Education', resettlement:'Resettlement'
};
const catLabelFa = {
  legal:'حقوقی', health:'صحت', housing:'مسکن',
  community:'جامعه', education:'آموزش', resettlement:'اسکان'
};

function renderList() {
  const list = document.getElementById('resource-list');
  const items = filteredResources();
  const lang = currentLang;

  if (items.length === 0) {
    list.innerHTML = `<div class="empty-state">${lang==='fa'?'نتیجه‌ای یافت نشد':'No results found'}</div>`;
    return;
  }

  list.innerHTML = items.map(r => `
    <div class="resource-card ${activeId===r.id?'active':''}" onclick="selectResource(${r.id})">
      <div class="card-top">
        <div class="card-icon ${catColors[r.category]}">${r.icon}</div>
        <div class="card-body">
          <div class="card-name">${lang==='fa' ? r.nameFa : r.name}</div>
          <div class="card-name-farsi">${lang==='fa' ? r.name : r.nameFa}</div>
          <div class="card-category">
            <span>${lang==='fa' ? catLabelFa[r.category] : catLabelEn[r.category]}</span>
          </div>
        </div>
      </div>
      <div class="card-detail">${lang==='fa' ? r.descFa : r.descEn}</div>
      <div class="card-tags">${r.tags.map(t=>`<span class="tag">${t}</span>`).join('')}</div>
      ${r.phone ? `<div class="card-phone">📞 ${r.phone}</div>` : ''}
    </div>
  `).join('');
}

// ── Map ───────────────────────────────────────────────────────────────────
// NOTE: Replace with your actual Mapbox public token
const MAPBOX_TOKEN = 'pk.eyJ1IjoiYW11YzIiLCJhIjoiY21vdDYwMDh4MDFhdTJxbzlkcmd0amdhNCJ9.Z7tPVbHinU_VjmcGPRvLdA';

function initMap() {
  mapboxgl.accessToken = MAPBOX_TOKEN;

  map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/dark-v11',
    center: [-122.29, 47.47],
    zoom: 9.5,
    attributionControl: false
  });

  map.addControl(new mapboxgl.NavigationControl(), 'top-right');
  map.addControl(new mapboxgl.AttributionControl({ compact: true }));

  map.on('load', () => {
    addMarkers();
    renderList();
  });
}

function markerColor(cat) {
  const cols = {
    legal:'#c8a96e', health:'#7eb8b0', housing:'#b48cc8',
    community:'#f0a064', education:'#64b482', resettlement:'#a0c8f0'
  };
  return cols[cat] || '#c8a96e';
}

function addMarkers() {
  resources.forEach(r => {
    const el = document.createElement('div');
    el.style.cssText = `
      width:36px; height:36px; border-radius:50%;
      background:${markerColor(r.category)};
      display:flex; align-items:center; justify-content:center;
      font-size:16px; cursor:pointer;
      border:2px solid rgba(255,255,255,0.2);
      box-shadow:0 2px 8px rgba(0,0,0,0.5);
      transition:transform 0.2s;
    `;
    el.textContent = r.icon;
    el.addEventListener('mouseenter', () => el.style.transform = 'scale(1.2)');
    el.addEventListener('mouseleave', () => { if(activeId!==r.id) el.style.transform='scale(1)'; });
    el.addEventListener('click', () => selectResource(r.id));

    const marker = new mapboxgl.Marker({ element: el, anchor: 'center' })
      .setLngLat([r.lng, r.lat])
      .addTo(map);

    markers[r.id] = { marker, el };
  });
}

function updateMarkers() {
  const visible = new Set(filteredResources().map(r => r.id));
  resources.forEach(r => {
    const el = markers[r.id]?.el;
    if (el) el.style.opacity = visible.has(r.id) ? '1' : '0.15';
  });
}

function selectResource(id) {
  activeId = id;
  const r = resources.find(x => x.id === id);
  if (!r) return;

  renderList();

  // Scroll card into view
  const cards = document.querySelectorAll('.resource-card');
  const filtered = filteredResources();
  const idx = filtered.findIndex(x => x.id === id);
  if (idx >= 0 && cards[idx]) cards[idx].scrollIntoView({ behavior: 'smooth', block: 'nearest' });

  // Fly to
  map.flyTo({ center: [r.lng, r.lat], zoom: 13, duration: 800 });

  // Pulse marker
  if (markers[id]?.el) markers[id].el.style.transform = 'scale(1.3)';

  // Show popup
  if (popup) popup.remove();
  const lang = currentLang;

  popup = new mapboxgl.Popup({ closeButton: true, maxWidth: '280px', offset: 20 })
    .setLngLat([r.lng, r.lat])
    .setHTML(`
      <div class="popup-name">${lang==='fa' ? r.nameFa : r.name}</div>
      <div class="popup-farsi">${lang==='fa' ? r.name : r.nameFa}</div>
      <div class="popup-desc">${lang==='fa' ? r.descFa : r.descEn}</div>
      ${r.phone ? `<div class="popup-phone">📞 ${r.phone}</div>` : ''}
      <a class="popup-btn" href="${r.website}" target="_blank" rel="noopener">
        ${lang==='fa' ? 'مشاهده وبسایت ↗' : 'Visit website ↗'}
      </a>
    `)
    .addTo(map);

  popup.on('close', () => {
    activeId = null;
    if (markers[id]?.el) markers[id].el.style.transform = 'scale(1)';
    renderList();
  });
}

// ── Init ──────────────────────────────────────────────────────────────────
initMap();
</script>
</body>
</html>
