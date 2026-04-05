<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Vape Shop</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
/* ══════════════════════════════════════════
   RETRO DARK — CRT / SYNTHWAVE / NEON NOIR
══════════════════════════════════════════ */
:root {
  --bg:        #080810;
  --bg2:       #0d0d1a;
  --panel:     #0f0f20;
  --border:    #ff003c55;
  --border2:   #00ffe455;
  --neon-r:    #ff003c;
  --neon-b:    #00ffe4;
  --neon-y:    #ffe600;
  --neon-p:    #cc00ff;
  --text:      #e8e8f0;
  --muted:     #6677aa;
  --success:   #00ffe4;
  --glow-r:    #ff003c44;
  --glow-b:    #00ffe422;
  --scan:      rgba(0,0,0,0.08);
  --radius:    4px;
  --font:      'Segoe UI', system-ui, sans-serif;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

html { scroll-behavior: smooth; }

body {
  font-family: var(--font);
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  overflow-x: hidden;
  /* CRT scanlines */
  background-image:
    repeating-linear-gradient(
      0deg,
      var(--scan) 0px,
      var(--scan) 1px,
      transparent 1px,
      transparent 4px
    );
}

/* Noise overlay */
body::before {
  content: '';
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  opacity: 0.03;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 256px;
}

/* ── SCREENS ── */
.screen { display: none; min-height: 100vh; padding-bottom: 90px; position: relative; z-index: 1; }
.screen.active { display: block; }

/* ── HEADER ── */
.header {
  background: var(--bg2);
  border-bottom: 1px solid var(--neon-r);
  box-shadow: 0 0 20px var(--glow-r);
  padding: 14px 18px;
  position: sticky; top: 0; z-index: 100;
}
.header-top { display: flex; align-items: center; justify-content: space-between; }

.logo {
  font-size: 20px; font-weight: 900; letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--neon-r);
  text-shadow: 0 0 10px var(--neon-r), 0 0 30px var(--neon-r);
  display: flex; align-items: center; gap: 8px;
}
.logo-sub {
  font-size: 9px; letter-spacing: 5px; color: var(--muted);
  font-weight: 400; display: block; margin-top: -2px;
}

.cart-btn {
  background: transparent;
  border: 1px solid var(--neon-r);
  color: var(--neon-r);
  border-radius: var(--radius);
  padding: 7px 14px;
  font-size: 13px; font-weight: 700;
  cursor: pointer;
  display: flex; align-items: center; gap: 6px;
  letter-spacing: 1px;
  box-shadow: 0 0 10px var(--glow-r), inset 0 0 8px var(--glow-r);
  transition: all 0.2s;
}
.cart-btn:active { box-shadow: 0 0 20px var(--neon-r); }
.cart-count {
  background: var(--neon-r);
  color: #000;
  border-radius: 2px;
  width: 18px; height: 18px;
  font-size: 11px;
  display: flex; align-items: center; justify-content: center;
  font-weight: 900;
}

.back-btn {
  background: none; border: none; color: var(--neon-b);
  font-size: 14px; font-weight: 700; cursor: pointer; padding: 4px;
  display: flex; align-items: center; gap: 6px;
  letter-spacing: 1px; text-transform: uppercase;
  text-shadow: 0 0 8px var(--neon-b);
}

/* ── HERO / BANNER ── */
.hero {
  margin: 16px;
  border: 1px solid var(--neon-r);
  border-radius: var(--radius);
  padding: 24px 20px;
  background: linear-gradient(135deg, #0a0012 0%, #0d001a 50%, #000a12 100%);
  box-shadow: 0 0 30px var(--glow-r), inset 0 0 40px #00000088;
  position: relative; overflow: hidden;
}
.hero::before {
  content: '';
  position: absolute; inset: 0;
  background: repeating-linear-gradient(
    90deg, transparent, transparent 40px,
    rgba(255,0,60,0.03) 40px, rgba(255,0,60,0.03) 41px
  );
}
.hero::after {
  content: '◈';
  position: absolute; right: 16px; top: 50%; transform: translateY(-50%);
  font-size: 56px; opacity: 0.1; color: var(--neon-r);
  animation: pulse-glow 2s ease-in-out infinite;
}
@keyframes pulse-glow {
  0%,100% { opacity: 0.08; text-shadow: 0 0 20px var(--neon-r); }
  50%      { opacity: 0.18; text-shadow: 0 0 40px var(--neon-r); }
}
.hero-tag {
  font-size: 9px; letter-spacing: 4px; color: var(--neon-r);
  text-transform: uppercase; margin-bottom: 8px;
  text-shadow: 0 0 8px var(--neon-r);
}
.hero h2 {
  font-size: 20px; font-weight: 900; letter-spacing: 1px;
  text-shadow: 0 0 12px var(--neon-b);
  color: var(--text);
}
.hero p { color: var(--muted); font-size: 13px; margin-top: 5px; }

/* ── SECTION TITLE ── */
.section-title {
  font-size: 11px; font-weight: 700; letter-spacing: 4px;
  text-transform: uppercase; color: var(--neon-b);
  text-shadow: 0 0 8px var(--neon-b);
  padding: 20px 18px 10px;
  display: flex; align-items: center; gap: 10px;
}
.section-title::after {
  content: ''; flex: 1; height: 1px;
  background: linear-gradient(to right, var(--neon-b), transparent);
  opacity: 0.4;
}

/* ── LOCATION CARDS ── */
.locations-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 10px; padding: 0 16px;
}
.loc-card {
  background: var(--panel);
  border: 1px solid #ffffff11;
  border-radius: var(--radius);
  padding: 18px 12px;
  cursor: pointer;
  transition: all 0.2s;
  text-align: center;
  position: relative; overflow: hidden;
}
.loc-card::before {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 1px;
  background: linear-gradient(to right, transparent, var(--neon-r), transparent);
  opacity: 0; transition: opacity 0.2s;
}
.loc-card:hover::before, .loc-card.selected::before { opacity: 1; }
.loc-card.selected {
  border-color: var(--neon-r);
  box-shadow: 0 0 16px var(--glow-r), inset 0 0 20px #ff003c08;
}
.loc-card .icon { font-size: 28px; margin-bottom: 8px; filter: drop-shadow(0 0 6px var(--neon-r)); }
.loc-card .name { font-size: 13px; font-weight: 700; letter-spacing: 0.5px; }
.loc-card .type { font-size: 10px; color: var(--muted); margin-top: 3px; letter-spacing: 2px; text-transform: uppercase; }

/* ── CATEGORY TABS ── */
.cats-scroll {
  display: flex; gap: 6px;
  overflow-x: auto; padding: 8px 16px 6px;
  scrollbar-width: none;
}
.cats-scroll::-webkit-scrollbar { display: none; }
.cat-tab {
  background: var(--panel);
  border: 1px solid #ffffff15;
  border-radius: 2px;
  padding: 7px 14px;
  font-size: 12px; font-weight: 700;
  cursor: pointer; white-space: nowrap;
  transition: all 0.2s; flex-shrink: 0;
  letter-spacing: 1px; text-transform: uppercase;
  color: var(--muted);
}
.cat-tab.active {
  background: var(--neon-r);
  border-color: var(--neon-r);
  color: #000;
  box-shadow: 0 0 12px var(--neon-r);
}

/* ── PRODUCTS GRID ── */
.products-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 10px; padding: 10px 16px;
}
.product-card {
  background: var(--panel);
  border: 1px solid #ffffff0d;
  border-radius: var(--radius);
  overflow: hidden;
  transition: all 0.2s;
  position: relative;
}
.product-card:hover {
  border-color: var(--neon-r);
  box-shadow: 0 0 12px var(--glow-r);
}
.product-img {
  width: 100%; aspect-ratio: 1;
  background: #05050f;
  display: flex; align-items: center; justify-content: center;
  font-size: 44px;
  position: relative; overflow: hidden;
}
.product-img img {
  width: 100%; height: 100%; object-fit: cover;
  filter: saturate(0.8) contrast(1.1);
}
/* CRT grid on image */
.product-img::after {
  content: '';
  position: absolute; inset: 0;
  background: repeating-linear-gradient(
    0deg, rgba(0,0,0,0.1) 0px, rgba(0,0,0,0.1) 1px, transparent 1px, transparent 3px
  );
  pointer-events: none;
}
.product-info { padding: 9px 11px 11px; }
.product-name { font-size: 12px; font-weight: 700; margin-bottom: 3px; line-height: 1.3; letter-spacing: 0.3px; }
.product-desc { font-size: 10px; color: var(--muted); margin-bottom: 7px; line-height: 1.4; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
.product-footer { display: flex; align-items: center; justify-content: space-between; }
.product-price { font-size: 14px; font-weight: 900; color: var(--neon-r); text-shadow: 0 0 6px var(--neon-r); }
.add-btn {
  background: var(--neon-r);
  border: none; color: #000;
  width: 26px; height: 26px;
  border-radius: 2px;
  font-size: 17px; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-weight: 900;
  box-shadow: 0 0 8px var(--neon-r);
  transition: all 0.15s; line-height: 1;
}
.add-btn:active { transform: scale(0.85); }
.out-of-stock {
  position: absolute; top: 0; left: 0; right: 0;
  background: #000000cc;
  color: var(--neon-r);
  font-size: 9px; font-weight: 700; letter-spacing: 3px;
  text-transform: uppercase;
  padding: 4px 8px; text-align: center;
  border-bottom: 1px solid var(--neon-r);
}
.qty-control { display: flex; align-items: center; gap: 5px; }
.qty-btn {
  background: var(--bg2);
  border: 1px solid #ffffff22; color: var(--text);
  width: 22px; height: 22px;
  border-radius: 2px; font-size: 13px;
  cursor: pointer; display: flex; align-items: center; justify-content: center;
  font-weight: 900;
}
.qty-num { font-size: 12px; font-weight: 900; min-width: 14px; text-align: center; color: var(--neon-b); }

/* ── CART ── */
.cart-items { padding: 0 16px; }
.cart-item {
  background: var(--panel);
  border: 1px solid #ffffff0d;
  border-left: 2px solid var(--neon-r);
  border-radius: var(--radius);
  padding: 12px 14px;
  margin-bottom: 8px;
  display: flex; align-items: center; gap: 11px;
}
.cart-item-emoji { font-size: 28px; flex-shrink: 0; }
.cart-item-info { flex: 1; min-width: 0; }
.cart-item-name { font-size: 13px; font-weight: 700; }
.cart-item-price { font-size: 11px; color: var(--neon-r); margin-top: 2px; font-weight: 700; }
.cart-item-qty { display: flex; align-items: center; gap: 6px; flex-shrink: 0; }

.cart-total {
  border: 1px solid var(--neon-r);
  border-radius: var(--radius);
  margin: 14px 16px;
  padding: 18px;
  background: var(--panel);
  box-shadow: 0 0 20px var(--glow-r);
  display: flex; align-items: center; justify-content: space-between;
}
.cart-total-label { font-size: 10px; letter-spacing: 4px; text-transform: uppercase; color: var(--muted); }
.cart-total-sum { font-size: 26px; font-weight: 900; color: var(--neon-r); text-shadow: 0 0 12px var(--neon-r); }

/* ── CHECKOUT ── */
.checkout-block {
  background: var(--panel);
  border: 1px solid #ffffff0d;
  border-radius: var(--radius);
  margin: 14px 16px;
  padding: 18px;
}
.checkout-title {
  font-size: 10px; font-weight: 700; letter-spacing: 4px;
  text-transform: uppercase; color: var(--neon-b);
  text-shadow: 0 0 8px var(--neon-b);
  margin-bottom: 14px;
}

.delivery-options { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 14px; }
.delivery-opt {
  background: var(--bg2);
  border: 1px solid #ffffff15;
  border-radius: var(--radius);
  padding: 14px 8px;
  text-align: center; cursor: pointer;
  transition: all 0.2s;
}
.delivery-opt.selected {
  border-color: var(--neon-r);
  box-shadow: 0 0 12px var(--glow-r);
  background: #0d0008;
}
.delivery-opt .d-icon { font-size: 26px; margin-bottom: 5px; }
.delivery-opt .d-label {
  font-size: 10px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
  color: var(--muted);
}
.delivery-opt.selected .d-label { color: var(--neon-r); text-shadow: 0 0 6px var(--neon-r); }

.input-label {
  font-size: 9px; letter-spacing: 3px; text-transform: uppercase;
  color: var(--muted); margin-bottom: 6px; display: block;
}
.input-field {
  width: 100%;
  background: var(--bg2);
  border: 1px solid #ffffff18;
  border-radius: var(--radius);
  padding: 11px 13px;
  color: var(--text);
  font-size: 14px;
  font-family: var(--font);
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
  margin-bottom: 10px;
}
.input-field:focus {
  border-color: var(--neon-r);
  box-shadow: 0 0 10px var(--glow-r);
}
.input-field::placeholder { color: var(--muted); }

.pickup-info {
  background: var(--bg2);
  border: 1px solid var(--neon-b);
  border-radius: var(--radius);
  padding: 13px;
  margin-top: 10px;
  box-shadow: 0 0 10px var(--glow-b);
}
.pickup-info .pi-label {
  font-size: 9px; letter-spacing: 3px; text-transform: uppercase;
  color: var(--neon-b); text-shadow: 0 0 6px var(--neon-b);
  margin-bottom: 8px;
}

/* ── BOTTOM ACTION ── */
.bottom-action {
  position: fixed; bottom: 0; left: 0; right: 0;
  padding: 12px 16px 16px;
  background: linear-gradient(to top, var(--bg) 70%, transparent);
  z-index: 200;
}
.action-btn {
  width: 100%;
  background: var(--neon-r);
  color: #000;
  border: none;
  border-radius: var(--radius);
  padding: 15px;
  font-size: 13px; font-weight: 900;
  cursor: pointer;
  letter-spacing: 3px; text-transform: uppercase;
  box-shadow: 0 0 20px var(--neon-r), 0 0 40px var(--glow-r);
  transition: all 0.2s;
}
.action-btn:active { transform: scale(0.97); box-shadow: 0 0 30px var(--neon-r); }
.action-btn:disabled { opacity: 0.35; cursor: not-allowed; box-shadow: none; }
.action-btn.success {
  background: var(--neon-b);
  box-shadow: 0 0 20px var(--neon-b), 0 0 40px var(--glow-b);
  color: #000;
}
.action-btn.secondary {
  background: transparent;
  border: 1px solid var(--neon-b);
  color: var(--neon-b);
  box-shadow: 0 0 10px var(--glow-b);
}

/* ── EMPTY STATE ── */
.empty-state {
  text-align: center; padding: 60px 20px;
  color: var(--muted);
}
.empty-state .icon { font-size: 56px; margin-bottom: 14px; opacity: 0.5; }
.empty-state p { font-size: 13px; letter-spacing: 2px; text-transform: uppercase; }

/* ── TOAST ── */
.toast {
  position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%);
  background: var(--panel);
  border: 1px solid var(--neon-b);
  color: var(--neon-b);
  box-shadow: 0 0 16px var(--glow-b);
  padding: 9px 20px;
  border-radius: var(--radius);
  font-size: 12px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
  z-index: 9999;
  opacity: 0; transition: opacity 0.25s;
  white-space: nowrap;
}
.toast.show { opacity: 1; }

/* ── DIVIDER ── */
.divider {
  height: 1px;
  background: linear-gradient(to right, transparent, var(--neon-r), transparent);
  opacity: 0.2; margin: 8px 0;
}

/* ── BADGE ── */
.badge {
  display: inline-block;
  background: var(--neon-r);
  color: #000;
  font-size: 9px; font-weight: 900; letter-spacing: 1px;
  padding: 2px 6px; border-radius: 2px;
  text-transform: uppercase;
}
</style>
</head>
<body>

<!-- ══ SCREEN 1: Выбор города ══ -->
<div id="screen-location" class="screen active">
  <div class="header">
    <div class="header-top">
      <div class="logo">
        ◈ VAPE
        <span class="logo-sub">SELECT LOCATION</span>
      </div>
    </div>
  </div>

  <div class="hero">
    <div class="hero-tag">// system boot</div>
    <h2>Выберите ваш город</h2>
    <p>Ассортимент и доставка зависят от локации</p>
  </div>

  <div class="section-title">🏙 Города</div>
  <div class="locations-grid" id="cities-grid"></div>

  <div class="section-title">🌾 Сёла и деревни</div>
  <div class="locations-grid" id="villages-grid"></div>

  <div style="height:90px"></div>
  <div class="bottom-action">
    <button class="action-btn" id="btn-choose-location" disabled onclick="goToCatalog()">
      ▶ Войти в магазин
    </button>
  </div>
</div>

<!-- ══ SCREEN 2: Каталог ══ -->
<div id="screen-catalog" class="screen">
  <div class="header">
    <div class="header-top">
      <button class="back-btn" onclick="goBack('screen-location')">◄ <span id="location-label">Город</span></button>
      <button class="cart-btn" onclick="showCart()">
        ◈ КОРЗИНА <span class="cart-count" id="cart-count">0</span>
      </button>
    </div>
    <div class="cats-scroll" id="cats-tabs" style="margin-top:10px"></div>
  </div>
  <div id="products-container"></div>
</div>

<!-- ══ SCREEN 3: Корзина ══ -->
<div id="screen-cart" class="screen">
  <div class="header">
    <div class="header-top">
      <button class="back-btn" onclick="goBack('screen-catalog')">◄ Назад</button>
      <div class="logo" style="font-size:14px">◈ КОРЗИНА</div>
    </div>
  </div>

  <div id="cart-body"></div>

  <div id="checkout-section" style="display:none">
    <div class="checkout-block">
      <div class="checkout-title">// Способ получения</div>
      <div class="delivery-options">
        <div class="delivery-opt selected" id="opt-delivery" onclick="selectDelivery('delivery')">
          <div class="d-icon">🚚</div>
          <div class="d-label">Доставка</div>
        </div>
        <div class="delivery-opt" id="opt-pickup" onclick="selectDelivery('pickup')">
          <div class="d-icon">🏪</div>
          <div class="d-label">Самовывоз</div>
        </div>
      </div>

      <div id="delivery-form">
        <label class="input-label">Номер телефона</label>
        <input class="input-field" id="phone-input" type="tel"
          placeholder="+7 (___) ___-__-__" inputmode="tel"
          oninput="formatPhone(this)">

        <label class="input-label">Адрес доставки</label>
        <textarea class="input-field" id="address-input" rows="3"
          placeholder="Улица, дом, квартира, подъезд, этаж..."></textarea>
      </div>

      <div id="pickup-info" style="display:none">
        <div class="pickup-info">
          <div class="pi-label">// Контакт для самовывоза</div>
          <div id="manager-contacts"></div>
        </div>
      </div>
    </div>
  </div>

  <div style="height:90px"></div>
  <div class="bottom-action" id="cart-actions"></div>
</div>

<div class="toast" id="toast"></div>

<script>
const tg = window.Telegram.WebApp;
tg.ready();
tg.expand();

// API base — укажите ваш сервер (ngrok/VPS)
// При GitHub Pages Mini App нужен отдельный сервер для API!
const API_BASE = 'http://127.0.0.1:4040';  // ← замените на ваш публичный URL

// ── STATE ──
let state = {
  locations: [],
  catalog:   [],
  selectedLocation: null,
  activeCategory: null,
  cart: {},
  deliveryType: 'delivery',
};

// ── INIT ──
async function init() {
  try {
    const locsRes = await fetch(`${API_BASE}/api/locations`);
    if (locsRes.ok) state.locations = await locsRes.json();
    // Каталог загружается позже — после выбора локации
  } catch(e) {
    console.warn('API недоступен, используем демо-данные:', e);
  }

  // Демо если API недоступен
  if (!state.locations.length) {
    state.locations = [
      {id:1,name:'Москва',type:'city'},
      {id:2,name:'Санкт-Петербург',type:'city'},
      {id:3,name:'Сергиев Посад',type:'city'},
      {id:4,name:'Пример-Деревня',type:'village'},
    ];
  }
  if (!state.catalog.length) {
    state.catalog = [
      {id:1,name:'Жижи под-систем',emoji:'💧',products:[
        {id:1,name:'BLUEBERRY ICE 30ml',description:'Черника со льдом, 6мг',price:450,image_url:'',in_stock:true},
        {id:2,name:'MANGO TANGO 30ml',description:'Сочный манго, 3мг',price:490,image_url:'',in_stock:true},
        {id:3,name:'STRAWBERRY MILK',description:'Клубника с молоком',price:420,image_url:'',in_stock:false},
        {id:4,name:'MENTHOL BREEZE',description:'Ледяной ментол',price:380,image_url:'',in_stock:true},
      ]},
      {id:2,name:'Жижи под-мод',emoji:'🔥',products:[
        {id:5,name:'DESERT SHIP 60ml',description:'Карамельный табак',price:750,image_url:'',in_stock:true},
        {id:6,name:'FRESH MINT 60ml',description:'Свежая мята',price:720,image_url:'',in_stock:true},
      ]},
      {id:3,name:'Испарители',emoji:'⚙️',products:[
        {id:7,name:'SMOK Nord 0.6Ω',description:'Мешевой испаритель',price:180,image_url:'',in_stock:true},
        {id:8,name:'Caliburn G2',description:'Картридж 2.5мл',price:350,image_url:'',in_stock:true},
      ]},
      {id:4,name:'Вата / Сетки',emoji:'🧵',products:[
        {id:9,name:'Cotton Bacon Prime',description:'Премиум хлопок',price:290,image_url:'',in_stock:true},
        {id:10,name:'SS316L Mesh',description:'Нержавеющая сетка 1м',price:210,image_url:'',in_stock:true},
      ]},
    ];
  }

  renderLocations();
}

// ── LOCATIONS ──
function renderLocations() {
  const cities   = state.locations.filter(l => l.type === 'city');
  const villages = state.locations.filter(l => l.type === 'village');
  document.getElementById('cities-grid').innerHTML =
    cities.map(locationCard).join('') || '<p style="padding:0 18px;color:var(--muted);font-size:12px">Нет городов</p>';
  document.getElementById('villages-grid').innerHTML =
    villages.map(locationCard).join('') || '<p style="padding:0 18px;color:var(--muted);font-size:12px">Нет населённых пунктов</p>';
}

function locationCard(loc) {
  const icons = {city:'🏙',village:'🌾'};
  const types = {city:'ГОРОД',village:'СЕЛО / ДЕР.'};
  return `<div class="loc-card" id="loc-${loc.id}" onclick="selectLocation(${loc.id})">
    <div class="icon">${icons[loc.type]||'📍'}</div>
    <div class="name">${loc.name}</div>
    <div class="type">${types[loc.type]||''}</div>
  </div>`;
}

function selectLocation(id) {
  document.querySelectorAll('.loc-card').forEach(c => c.classList.remove('selected'));
  document.getElementById(`loc-${id}`).classList.add('selected');
  state.selectedLocation = state.locations.find(l => l.id === id);
  document.getElementById('btn-choose-location').disabled = false;
}

async function goToCatalog() {
  if (!state.selectedLocation) return;
  document.getElementById('location-label').textContent = state.selectedLocation.name;
  showScreen('screen-catalog');

  // Загружаем каталог для выбранной локации
  const btn = document.getElementById('btn-choose-location');
  btn.disabled = true;
  btn.textContent = '// загрузка...';

  try {
    const res = await fetch(`${API_BASE}/api/catalog?location_id=${state.selectedLocation.id}`);
    if (res.ok) {
      const data = await res.json();
      // Фильтруем пустые категории
      state.catalog = data.filter(c => c.products && c.products.length > 0);
    }
  } catch(e) {
    console.warn('Не удалось загрузить каталог для локации:', e);
  }

  btn.disabled = false;
  btn.textContent = '▶ Войти в магазин';
  renderCatalog();
}

// ── CATALOG ──
function renderCatalog() {
  const tabs = document.getElementById('cats-tabs');
  tabs.innerHTML = state.catalog.map((cat, i) =>
    `<div class="cat-tab ${i===0?'active':''}" id="tab-${cat.id}" onclick="filterCat(${cat.id})">${cat.emoji} ${cat.name}</div>`
  ).join('');
  state.activeCategory = state.catalog[0]?.id || null;
  renderProducts();
}

function filterCat(catId) {
  document.querySelectorAll('.cat-tab').forEach(t => t.classList.remove('active'));
  document.getElementById(`tab-${catId}`).classList.add('active');
  state.activeCategory = catId;
  renderProducts();
}

function getProductEmoji(name) {
  const n = name.toLowerCase();
  if (n.includes('ice')||n.includes('mint')||n.includes('menthol')) return '🧊';
  if (n.includes('mango')||n.includes('peach'))  return '🥭';
  if (n.includes('strawberry')||n.includes('клубник')) return '🍓';
  if (n.includes('blueberry')||n.includes('черник')) return '🫐';
  if (n.includes('milk')||n.includes('молок'))   return '🥛';
  if (n.includes('cotton')||n.includes('вата'))   return '🧵';
  if (n.includes('coil')||n.includes('mesh')||n.includes('испар')||n.includes('сетк')) return '⚙️';
  if (n.includes('60ml')||n.includes('desert'))  return '🔥';
  return '💧';
}

function resolveImageUrl(raw) {
  if (!raw) return null;
  if (raw.startsWith('tg://photo/')) {
    const fileId = raw.replace('tg://photo/', '');
    return `${API_BASE}/api/photo/${fileId}`;
  }
  if (raw.startsWith('http')) return raw;
  return null;
}

function renderProducts() {
  const container = document.getElementById('products-container');
  const cat = state.catalog.find(c => c.id === state.activeCategory);
  if (!cat) { container.innerHTML = ''; return; }
  if (!cat.products.length) {
    container.innerHTML = `<div class="empty-state"><div class="icon">📦</div><p>Товаров нет</p></div>`;
    return;
  }
  container.innerHTML = `<div class="products-grid">${cat.products.map(productCard).join('')}</div>`;
}

function productCard(p) {
  const inCart = state.cart[p.id];
  const emoji  = getProductEmoji(p.name);
  const imgUrl = resolveImageUrl(p.image_url);

  const imgHtml = imgUrl
    ? `<img src="${imgUrl}" alt="${p.name}" onerror="this.style.display='none';this.parentNode.dataset.fb='1'">
       <span style="display:none;font-size:44px" class="fb-emoji">${emoji}</span>`
    : `<span style="font-size:44px">${emoji}</span>`;

  const actionHtml = !p.in_stock
    ? `<button class="add-btn" disabled style="opacity:0.3;background:#333;box-shadow:none">✕</button>`
    : inCart
    ? `<div class="qty-control">
        <button class="qty-btn" onclick="changeQty(${p.id},-1,event)">−</button>
        <span class="qty-num">${inCart.qty}</span>
        <button class="qty-btn" onclick="changeQty(${p.id},1,event)" style="border-color:var(--neon-r);color:var(--neon-r)">+</button>
       </div>`
    : `<button class="add-btn" onclick="addToCart(${p.id},event)">+</button>`;

  return `<div class="product-card" id="pcard-${p.id}">
    <div class="product-img">${imgHtml}</div>
    ${!p.in_stock ? '<div class="out-of-stock">// нет в наличии</div>' : ''}
    <div class="product-info">
      <div class="product-name">${p.name}</div>
      ${p.description ? `<div class="product-desc">${p.description}</div>` : ''}
      <div class="product-footer">
        <div class="product-price">${p.price} ₽</div>
        <div id="action-${p.id}">${actionHtml}</div>
      </div>
    </div>
  </div>`;
}

// ── CART LOGIC ──
function findProduct(id) {
  for (const cat of state.catalog)
    for (const p of cat.products)
      if (p.id === id) return p;
  return null;
}

function addToCart(id, e) {
  e?.stopPropagation();
  const p = findProduct(id);
  if (!p || !p.in_stock) return;
  state.cart[id] = { product: p, qty: 1 };
  updateCartUI();
  showToast(`+ ${p.name}`);
  rerenderProductAction(id);
}

function changeQty(id, delta, e) {
  e?.stopPropagation();
  if (!state.cart[id]) return;
  state.cart[id].qty += delta;
  if (state.cart[id].qty <= 0) delete state.cart[id];
  updateCartUI();
  // Перерисовываем кнопку — включая восстановление кнопки + при qty=0
  rerenderProductAction(id);
}

function cartChangeQtyAndSync(id, delta) {
  if (!state.cart[id]) return;
  state.cart[id].qty += delta;
  if (state.cart[id].qty <= 0) delete state.cart[id];
  updateCartUI();
  renderCart();
  // Синхронизируем кнопку в каталоге если DOM существует
  rerenderProductAction(id);
}

function rerenderProductAction(id) {
  const el = document.getElementById(`action-${id}`);
  if (!el) return;
  const p = findProduct(id);
  if (!p) return;
  const inCart = state.cart[id];
  el.innerHTML = !p.in_stock
    ? `<button class="add-btn" disabled style="opacity:0.3;background:#333;box-shadow:none">✕</button>`
    : inCart
    ? `<div class="qty-control">
        <button class="qty-btn" onclick="changeQty(${id},-1,event)">−</button>
        <span class="qty-num">${inCart.qty}</span>
        <button class="qty-btn" onclick="changeQty(${id},1,event)" style="border-color:var(--neon-r);color:var(--neon-r)">+</button>
       </div>`
    : `<button class="add-btn" onclick="addToCart(${id},event)">+</button>`;
}

function updateCartUI() {
  const total = Object.values(state.cart).reduce((s, i) => s + i.qty, 0);
  document.getElementById('cart-count').textContent = total;
}

function getCartItems() {
  return Object.values(state.cart).map(i => ({
    id: i.product.id,
    name: i.product.name,
    price: i.product.price,
    qty: i.qty
  }));
}

function getCartTotal() {
  return Object.values(state.cart).reduce((s, i) => s + i.product.price * i.qty, 0);
}

// ── CART SCREEN ──
function showCart() {
  showScreen('screen-cart');
  renderCart();
}

function renderCart() {
  const cartItems = Object.values(state.cart);
  const body      = document.getElementById('cart-body');
  const checkout  = document.getElementById('checkout-section');
  const actions   = document.getElementById('cart-actions');

  if (!cartItems.length) {
    body.innerHTML = `<div class="empty-state"><div class="icon">◈</div><p>Корзина пуста</p></div>`;
    checkout.style.display = 'none';
    actions.innerHTML = `<button class="action-btn secondary" onclick="goBack('screen-catalog')">◄ К товарам</button>`;
    return;
  }

  const total = getCartTotal();
  body.innerHTML = `
    <div class="cart-items" style="margin-top:14px">
      ${cartItems.map(i => `
        <div class="cart-item">
          <div class="cart-item-emoji">${getProductEmoji(i.product.name)}</div>
          <div class="cart-item-info">
            <div class="cart-item-name">${i.product.name}</div>
            <div class="cart-item-price">${i.product.price} × ${i.qty} = ${i.product.price * i.qty} ₽</div>
          </div>
          <div class="cart-item-qty">
            <button class="qty-btn" onclick="cartChangeQty(${i.product.id},-1)">−</button>
            <span class="qty-num">${i.qty}</span>
            <button class="qty-btn" onclick="cartChangeQty(${i.product.id},1)" style="border-color:var(--neon-r);color:var(--neon-r)">+</button>
          </div>
        </div>`).join('')}
    </div>
    <div class="cart-total">
      <div>
        <div class="cart-total-label">Итого к оплате</div>
        <div class="cart-total-sum">${total} ₽</div>
      </div>
      <div style="font-size:28px;opacity:0.6">◈</div>
    </div>`;

  checkout.style.display = 'block';
  actions.innerHTML = `<button class="action-btn" onclick="submitOrder()">▶ Оформить заказ</button>`;
}

function cartChangeQty(id, delta) {
  if (!state.cart[id]) return;
  state.cart[id].qty += delta;
  if (state.cart[id].qty <= 0) delete state.cart[id];
  updateCartUI();
  renderCart();
  rerenderProductAction(id);
}

// ── DELIVERY ──
function selectDelivery(type) {
  state.deliveryType = type;
  document.getElementById('opt-delivery').classList.toggle('selected', type === 'delivery');
  document.getElementById('opt-pickup').classList.toggle('selected', type === 'pickup');
  document.getElementById('delivery-form').style.display = type === 'delivery' ? 'block' : 'none';
  document.getElementById('pickup-info').style.display   = type === 'pickup'   ? 'block' : 'none';

  if (type === 'pickup') {
    document.getElementById('manager-contacts').innerHTML =
      `<div style="font-size:13px;color:var(--text);margin-top:6px">
        После оформления бот пришлёт контакт менеджера для самовывоза.
       </div>`;
  }
}

// ── PHONE FORMAT ──
function formatPhone(input) {
  let v = input.value.replace(/\D/g, '');
  if (v.startsWith('8')) v = '7' + v.slice(1);
  if (!v.startsWith('7')) v = '7' + v;
  v = v.slice(0, 11);
  let formatted = '+7';
  if (v.length > 1) formatted += ' (' + v.slice(1, 4);
  if (v.length >= 4) formatted += ') ' + v.slice(4, 7);
  if (v.length >= 7) formatted += '-' + v.slice(7, 9);
  if (v.length >= 9) formatted += '-' + v.slice(9, 11);
  input.value = formatted;
}

// ── SUBMIT ──
function submitOrder() {
  const items = getCartItems();
  if (!items.length) return showToast('Корзина пуста!');
  if (!state.selectedLocation) return showToast('Выберите город!');

  const deliveryType = state.deliveryType;
  let address = '';
  let phone   = '';

  if (deliveryType === 'delivery') {
    phone   = document.getElementById('phone-input').value.trim();
    address = document.getElementById('address-input').value.trim();

    if (!phone || phone.replace(/\D/g,'').length < 11) {
      showToast('Введите номер телефона!');
      document.getElementById('phone-input').focus();
      return;
    }
    if (!address) {
      showToast('Введите адрес доставки!');
      document.getElementById('address-input').focus();
      return;
    }
  }

  const payload = {
    location_id:   state.selectedLocation.id,
    delivery_type: deliveryType,
    address:        deliveryType === 'delivery' ? `${phone} | ${address}` : '',
    phone:          phone,
    items:          items,
    total:          getCartTotal()
  };

  const btn = document.querySelector('#cart-actions .action-btn');
  if (btn) { btn.disabled = true; btn.textContent = '// ОТПРАВКА...'; }

  try {
    tg.sendData(JSON.stringify(payload));
    state.cart = {};
    updateCartUI();
    if (btn) { btn.classList.add('success'); btn.textContent = '✓ ЗАКАЗ ПРИНЯТ'; }
    setTimeout(() => tg.close(), 1500);
  } catch(e) {
    if (btn) { btn.disabled = false; btn.textContent = '▶ Оформить заказ'; }
    showToast('Ошибка отправки!');
  }
}

// ── NAVIGATION ──
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

function goBack(to) {
  showScreen(to);
  if (to === 'screen-catalog') {
    // Полная перерисовка каталога — исправляет баг с кнопкой +
    renderProducts();
  }
}

// ── TOAST ──
let toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = '// ' + msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2200);
}

init();

// Авто-обновление каталога каждые 30 секунд — изменения видны всем без перезахода
setInterval(async () => {
  if (!state.selectedLocation) return;
  try {
    const res = await fetch(`${API_BASE}/api/catalog?location_id=${state.selectedLocation.id}&_t=${Date.now()}`);
    if (res.ok) {
      const data = await res.json();
      const updated = data.filter(c => c.products && c.products.length > 0);
      // Обновляем только если что-то изменилось
      if (JSON.stringify(updated) !== JSON.stringify(state.catalog)) {
        state.catalog = updated;
        const activeScreen = document.querySelector('.screen.active')?.id;
        if (activeScreen === 'screen-catalog') renderCatalog();
      }
    }
  } catch(e) {}
}, 30000);

// Авто-обновление локаций каждую минуту
setInterval(async () => {
  try {
    const res = await fetch(`${API_BASE}/api/locations?_t=${Date.now()}`);
    if (res.ok) {
      const data = await res.json();
      if (JSON.stringify(data) !== JSON.stringify(state.locations)) {
        state.locations = data;
        const activeScreen = document.querySelector('.screen.active')?.id;
        if (activeScreen === 'screen-location') renderLocations();
      }
    }
  } catch(e) {}
}, 60000);
</script>
</body>
</html>
