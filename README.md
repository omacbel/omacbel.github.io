# omacbel.github.io
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Vape Shop</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
  :root {
    --smoke: #1a1a2e;
    --deep: #16213e;
    --card: #0f3460;
    --accent: #e94560;
    --accent2: #533483;
    --glow: #e9456033;
    --text: #eaeaea;
    --muted: #8892a4;
    --success: #00d9a3;
    --radius: 16px;
    --font: 'Segoe UI', system-ui, sans-serif;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

  body {
    font-family: var(--font);
    background: var(--smoke);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── SCREENS ── */
  .screen { display: none; min-height: 100vh; padding-bottom: 80px; }
  .screen.active { display: block; }

  /* ── HEADER ── */
  .header {
    background: linear-gradient(135deg, var(--deep) 0%, var(--card) 100%);
    padding: 16px 20px;
    position: sticky; top: 0; z-index: 100;
    border-bottom: 1px solid #ffffff11;
    backdrop-filter: blur(10px);
  }
  .header-top { display: flex; align-items: center; justify-content: space-between; }
  .logo { font-size: 22px; font-weight: 800; letter-spacing: -0.5px; }
  .logo span { color: var(--accent); }
  .cart-btn {
    background: var(--glow);
    border: 1px solid var(--accent);
    color: var(--accent);
    border-radius: 50px;
    padding: 8px 16px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    display: flex; align-items: center; gap: 6px;
    transition: all 0.2s;
  }
  .cart-btn:hover { background: var(--accent); color: white; }
  .cart-count {
    background: var(--accent);
    color: white;
    border-radius: 50%;
    width: 20px; height: 20px;
    font-size: 11px;
    display: flex; align-items: center; justify-content: center;
    font-weight: 800;
  }
  .back-btn {
    background: none; border: none; color: var(--text);
    font-size: 20px; cursor: pointer; padding: 4px;
    display: flex; align-items: center; gap: 8px;
  }
  .back-btn span { font-size: 16px; font-weight: 600; }

  /* ── HERO ── */
  .hero {
    background: linear-gradient(135deg, var(--card) 0%, var(--accent2) 100%);
    margin: 16px;
    border-radius: var(--radius);
    padding: 28px 24px;
    position: relative;
    overflow: hidden;
  }
  .hero::before {
    content: '💨';
    position: absolute; right: 20px; top: 50%; transform: translateY(-50%);
    font-size: 64px; opacity: 0.3;
    animation: float 3s ease-in-out infinite;
  }
  @keyframes float { 0%,100% { transform: translateY(-50%) rotate(-5deg); } 50% { transform: translateY(-60%) rotate(5deg); } }
  .hero h2 { font-size: 22px; font-weight: 800; margin-bottom: 6px; }
  .hero p { color: #ffffffaa; font-size: 14px; }

  /* ── SECTION TITLE ── */
  .section-title {
    font-size: 18px; font-weight: 700;
    padding: 20px 20px 12px;
    display: flex; align-items: center; gap: 8px;
  }

  /* ── LOCATION CARDS ── */
  .locations-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 12px; padding: 0 16px;
  }
  .loc-card {
    background: var(--card);
    border: 1px solid #ffffff11;
    border-radius: var(--radius);
    padding: 20px 16px;
    cursor: pointer;
    transition: all 0.2s;
    text-align: center;
  }
  .loc-card:hover, .loc-card.selected {
    border-color: var(--accent);
    box-shadow: 0 0 20px var(--glow);
    transform: translateY(-2px);
  }
  .loc-card .icon { font-size: 32px; margin-bottom: 8px; }
  .loc-card .name { font-size: 14px; font-weight: 600; }
  .loc-card .type { font-size: 11px; color: var(--muted); margin-top: 3px; }

  /* ── CATEGORY TABS ── */
  .cats-scroll {
    display: flex; gap: 8px;
    overflow-x: auto; padding: 0 16px 8px;
    scrollbar-width: none;
  }
  .cats-scroll::-webkit-scrollbar { display: none; }
  .cat-tab {
    background: var(--card);
    border: 1px solid #ffffff11;
    border-radius: 50px;
    padding: 8px 16px;
    font-size: 13px; font-weight: 600;
    cursor: pointer; white-space: nowrap;
    transition: all 0.2s; flex-shrink: 0;
  }
  .cat-tab.active {
    background: var(--accent);
    border-color: var(--accent);
    color: white;
  }

  /* ── PRODUCTS GRID ── */
  .products-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 12px; padding: 12px 16px;
  }
  .product-card {
    background: var(--card);
    border: 1px solid #ffffff11;
    border-radius: var(--radius);
    overflow: hidden;
    transition: all 0.2s;
    position: relative;
  }
  .product-card:hover { transform: translateY(-2px); box-shadow: 0 8px 24px #00000044; }
  .product-img {
    width: 100%; aspect-ratio: 1;
    background: linear-gradient(135deg, var(--deep), var(--accent2));
    display: flex; align-items: center; justify-content: center;
    font-size: 48px;
    object-fit: cover;
  }
  .product-img img { width: 100%; height: 100%; object-fit: cover; }
  .product-info { padding: 10px 12px; }
  .product-name { font-size: 13px; font-weight: 700; margin-bottom: 4px; line-height: 1.3; }
  .product-desc { font-size: 11px; color: var(--muted); margin-bottom: 8px; line-height: 1.4; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
  .product-footer { display: flex; align-items: center; justify-content: space-between; }
  .product-price { font-size: 15px; font-weight: 800; color: var(--accent); }
  .add-btn {
    background: var(--accent);
    border: none; color: white;
    width: 28px; height: 28px;
    border-radius: 50%;
    font-size: 18px; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: all 0.15s;
    line-height: 1;
  }
  .add-btn:active { transform: scale(0.85); }
  .out-of-stock {
    position: absolute; top: 8px; right: 8px;
    background: #00000088;
    color: #ff6b6b;
    font-size: 10px; font-weight: 700;
    padding: 3px 8px; border-radius: 50px;
  }
  .qty-control {
    display: flex; align-items: center; gap: 6px;
  }
  .qty-btn {
    background: var(--deep);
    border: none; color: var(--text);
    width: 24px; height: 24px;
    border-radius: 50%; font-size: 14px;
    cursor: pointer; display: flex; align-items: center; justify-content: center;
  }
  .qty-num { font-size: 13px; font-weight: 700; min-width: 14px; text-align: center; }

  /* ── CART ── */
  .cart-items { padding: 0 16px; }
  .cart-item {
    background: var(--card);
    border: 1px solid #ffffff11;
    border-radius: var(--radius);
    padding: 14px 16px;
    margin-bottom: 10px;
    display: flex; align-items: center; gap: 12px;
  }
  .cart-item-emoji { font-size: 32px; flex-shrink: 0; }
  .cart-item-info { flex: 1; min-width: 0; }
  .cart-item-name { font-size: 14px; font-weight: 700; }
  .cart-item-price { font-size: 13px; color: var(--accent); margin-top: 2px; }
  .cart-item-qty {
    display: flex; align-items: center; gap: 8px; flex-shrink: 0;
  }
  .cart-total {
    background: linear-gradient(135deg, var(--card), var(--accent2));
    border-radius: var(--radius);
    margin: 16px;
    padding: 20px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .cart-total-label { font-size: 14px; color: var(--muted); }
  .cart-total-sum { font-size: 24px; font-weight: 900; color: var(--success); }

  /* ── CHECKOUT ── */
  .checkout-block {
    background: var(--card);
    border-radius: var(--radius);
    margin: 16px;
    padding: 20px;
  }
  .checkout-title { font-size: 16px; font-weight: 700; margin-bottom: 14px; }
  .delivery-options { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 16px; }
  .delivery-opt {
    background: var(--deep);
    border: 2px solid #ffffff11;
    border-radius: 12px;
    padding: 14px 10px;
    text-align: center; cursor: pointer;
    transition: all 0.2s;
  }
  .delivery-opt.selected { border-color: var(--accent); background: var(--glow); }
  .delivery-opt .d-icon { font-size: 28px; margin-bottom: 6px; }
  .delivery-opt .d-label { font-size: 13px; font-weight: 600; }

  .input-group { margin-top: 14px; }
  .input-label { font-size: 13px; color: var(--muted); margin-bottom: 6px; }
  .input-field {
    width: 100%;
    background: var(--deep);
    border: 1px solid #ffffff22;
    border-radius: 10px;
    padding: 12px 14px;
    color: var(--text);
    font-size: 15px;
    font-family: var(--font);
    outline: none;
    transition: border-color 0.2s;
  }
  .input-field:focus { border-color: var(--accent); }

  .pickup-info {
    background: var(--deep);
    border-radius: 12px;
    padding: 14px;
    margin-top: 14px;
  }
  .pickup-info .contact { display: flex; align-items: center; gap: 10px; margin-top: 8px; }
  .pickup-info .contact a { color: var(--success); font-weight: 600; text-decoration: none; }

  /* ── BOTTOM BTN ── */
  .bottom-action {
    position: fixed; bottom: 0; left: 0; right: 0;
    padding: 12px 16px;
    background: linear-gradient(to top, var(--smoke) 60%, transparent);
    z-index: 200;
  }
  .action-btn {
    width: 100%;
    background: linear-gradient(135deg, var(--accent) 0%, #c03045 100%);
    color: white;
    border: none;
    border-radius: 14px;
    padding: 16px;
    font-size: 16px; font-weight: 800;
    cursor: pointer;
    letter-spacing: 0.3px;
    box-shadow: 0 4px 20px var(--glow);
    transition: all 0.2s;
  }
  .action-btn:active { transform: scale(0.97); }
  .action-btn:disabled { opacity: 0.5; cursor: not-allowed; }
  .action-btn.success { background: linear-gradient(135deg, var(--success) 0%, #00a87a 100%); }

  /* ── EMPTY ── */
  .empty-state {
    text-align: center; padding: 60px 20px;
    color: var(--muted);
  }
  .empty-state .icon { font-size: 64px; margin-bottom: 16px; }
  .empty-state p { font-size: 16px; }

  /* ── TOAST ── */
  .toast {
    position: fixed; bottom: 100px; left: 50%; transform: translateX(-50%);
    background: var(--success);
    color: #001a12;
    padding: 10px 20px;
    border-radius: 50px;
    font-size: 14px; font-weight: 700;
    z-index: 9999;
    opacity: 0; transition: opacity 0.3s;
    white-space: nowrap;
  }
  .toast.show { opacity: 1; }

  /* ── LOADER ── */
  .loader {
    display: flex; justify-content: center; padding: 40px;
  }
  .spinner {
    width: 36px; height: 36px;
    border: 3px solid #ffffff22;
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.7s linear infinite;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  .divider { height: 1px; background: #ffffff11; margin: 8px 0; }
</style>
</head>
<body>

<!-- SCREEN 1: Выбор города -->
<div id="screen-location" class="screen active">
  <div class="header">
    <div class="header-top">
      <div class="logo">Vape<span>Shop</span></div>
    </div>
  </div>
  <div class="hero">
    <h2>Выберите ваш город</h2>
    <p>Ассортимент и доставка зависят от локации</p>
  </div>
  <div class="section-title">🏙 Города</div>
  <div class="locations-grid" id="cities-grid"></div>
  <div class="section-title">🌾 Сёла и деревни</div>
  <div class="locations-grid" id="villages-grid"></div>
  <div style="height:80px"></div>
  <div class="bottom-action">
    <button class="action-btn" id="btn-choose-location" disabled onclick="goToCatalog()">Выбрать →</button>
  </div>
</div>

<!-- SCREEN 2: Каталог -->
<div id="screen-catalog" class="screen">
  <div class="header">
    <div class="header-top">
      <button class="back-btn" onclick="goBack('screen-location')">← <span id="location-label">Город</span></button>
      <button class="cart-btn" onclick="showCart()">
        🛒 <span class="cart-count" id="cart-count">0</span>
      </button>
    </div>
    <div class="cats-scroll" id="cats-tabs" style="margin-top:12px"></div>
  </div>
  <div id="products-container"></div>
</div>

<!-- SCREEN 3: Корзина -->
<div id="screen-cart" class="screen">
  <div class="header">
    <div class="header-top">
      <button class="back-btn" onclick="goBack('screen-catalog')">← <span>Корзина</span></button>
    </div>
  </div>

  <div id="cart-body"></div>

  <div id="checkout-section" style="display:none">
    <div class="checkout-block">
      <div class="checkout-title">📦 Способ получения</div>
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
        <div class="input-group">
          <div class="input-label">Адрес доставки</div>
          <textarea class="input-field" id="address-input" rows="3"
            placeholder="Улица, дом, квартира, подъезд, этаж..."></textarea>
        </div>
      </div>

      <div id="pickup-info" style="display:none">
        <div class="pickup-info">
          <div style="font-size:13px; color:var(--muted)">Свяжитесь с менеджером для самовывоза:</div>
          <div id="manager-contacts"></div>
        </div>
      </div>
    </div>
  </div>

  <div style="height:80px"></div>
  <div class="bottom-action" id="cart-actions">
    <button class="action-btn" id="btn-checkout" onclick="handleCheckout()">Оформить заказ</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const tg = window.Telegram.WebApp;
tg.ready();
tg.expand();

// Цвета в зависимости от темы Telegram
if (tg.colorScheme === 'light') {
  document.documentElement.style.setProperty('--smoke', '#f0f0f5');
  document.documentElement.style.setProperty('--deep', '#e0e0ea');
  document.documentElement.style.setProperty('--card', '#ffffff');
  document.documentElement.style.setProperty('--text', '#111111');
  document.documentElement.style.setProperty('--muted', '#666688');
}

// ── STATE ──────────────────────────────────────────────────────
let state = {
  locations: [],
  catalog: [],
  selectedLocation: null,
  activeCategory: null,
  cart: {},      // {productId: {product, qty}}
  deliveryType: 'delivery',
  managers: []
};

// ── INIT ───────────────────────────────────────────────────────
async function init() {
  const base = window.location.origin;
  try {
    const [locsRes, catRes] = await Promise.all([
      fetch(`${base}/api/locations`),
      fetch(`${base}/api/catalog`)
    ]);
    state.locations = await locsRes.json();
    state.catalog = await catRes.json();
  } catch(e) {
    // Fallback demo data for testing
    state.locations = [
      {id:1, name:'Москва', type:'city'},
      {id:2, name:'Санкт-Петербург', type:'city'},
      {id:3, name:'Сергиев Посад', type:'city'},
      {id:4, name:'Пример-Деревня', type:'village'},
    ];
    state.catalog = [
      {id:1, name:'Жижи для под-систем', emoji:'💧', products:[
        {id:1, name:'BLUEBERRY ICE 30ml', description:'Черника со льдом, 6мг', price:450, image_url:'', in_stock:true},
        {id:2, name:'MANGO TANGO 30ml', description:'Сочный манго, 3мг', price:490, image_url:'', in_stock:true},
        {id:3, name:'STRAWBERRY MILK', description:'Клубника с молоком', price:420, image_url:'', in_stock:false},
        {id:4, name:'MENTHOL BREEZE', description:'Ледяной ментол', price:380, image_url:'', in_stock:true},
      ]},
      {id:2, name:'Жижи для под-модов', emoji:'🔥', products:[
        {id:5, name:'DESERT SHIP 60ml', description:'Карамель табак', price:750, image_url:'', in_stock:true},
        {id:6, name:'FRESH MINT 60ml', description:'Свежая мята', price:720, image_url:'', in_stock:true},
      ]},
      {id:3, name:'Расходники (испарители)', emoji:'⚙️', products:[
        {id:7, name:'SMOK Nord Coil 0.6Ω', description:'Мешевой испаритель', price:180, image_url:'', in_stock:true},
        {id:8, name:'UWELL Caliburn G2', description:'Картридж 2.5мл', price:350, image_url:'', in_stock:true},
      ]},
      {id:4, name:'Вата / Сетки', emoji:'🧵', products:[
        {id:9, name:'Cotton Bacon Prime', description:'Премиум хлопок 10 полосок', price:290, image_url:'', in_stock:true},
        {id:10, name:'SS316L Mesh 100 mesh', description:'Нержавеющая сетка 1м', price:210, image_url:'', in_stock:true},
      ]},
    ];
  }
  renderLocations();
}

// ── LOCATIONS ──────────────────────────────────────────────────
function renderLocations() {
  const cities = state.locations.filter(l => l.type === 'city');
  const villages = state.locations.filter(l => l.type === 'village');

  document.getElementById('cities-grid').innerHTML =
    cities.map(loc => locationCard(loc)).join('') || '<p style="padding:0 20px;color:var(--muted)">Нет городов</p>';
  document.getElementById('villages-grid').innerHTML =
    villages.map(loc => locationCard(loc)).join('') || '<p style="padding:0 20px;color:var(--muted)">Нет населённых пунктов</p>';
}

function locationCard(loc) {
  const icons = {'city': '🏙', 'village': '🌾'};
  const types = {'city': 'Город', 'village': 'Село / Деревня'};
  return `<div class="loc-card" id="loc-${loc.id}" onclick="selectLocation(${loc.id})">
    <div class="icon">${icons[loc.type] || '📍'}</div>
    <div class="name">${loc.name}</div>
    <div class="type">${types[loc.type] || ''}</div>
  </div>`;
}

function selectLocation(id) {
  document.querySelectorAll('.loc-card').forEach(c => c.classList.remove('selected'));
  document.getElementById(`loc-${id}`).classList.add('selected');
  state.selectedLocation = state.locations.find(l => l.id === id);
  document.getElementById('btn-choose-location').disabled = false;
}

function goToCatalog() {
  if (!state.selectedLocation) return;
  document.getElementById('location-label').textContent = state.selectedLocation.name;
  showScreen('screen-catalog');
  renderCatalog();
}

// ── CATALOG ────────────────────────────────────────────────────
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
  if (n.includes('ice') || n.includes('mint') || n.includes('menthol')) return '🧊';
  if (n.includes('mango') || n.includes('peach') || n.includes('tropical')) return '🥭';
  if (n.includes('strawberry') || n.includes('клубник')) return '🍓';
  if (n.includes('blueberry') || n.includes('черник')) return '🫐';
  if (n.includes('milk') || n.includes('молок')) return '🥛';
  if (n.includes('cotton') || n.includes('вата')) return '🧵';
  if (n.includes('coil') || n.includes('испар') || n.includes('mesh') || n.includes('сетк')) return '⚙️';
  if (n.includes('60ml') || n.includes('мод')) return '🔥';
  return '💧';
}

function renderProducts() {
  const container = document.getElementById('products-container');
  const cat = state.catalog.find(c => c.id === state.activeCategory);
  if (!cat) { container.innerHTML = ''; return; }

  if (!cat.products.length) {
    container.innerHTML = `<div class="empty-state"><div class="icon">📦</div><p>Товаров в этой категории пока нет</p></div>`;
    return;
  }

  container.innerHTML = `<div class="products-grid">${cat.products.map(p => productCard(p)).join('')}</div>`;
}

function productCard(p) {
  const inCart = state.cart[p.id];
  const emoji = getProductEmoji(p.name);

  const imgContent = p.image_url
    ? `<img src="${p.image_url}" alt="${p.name}" onerror="this.parentNode.innerHTML='${emoji}'">`
    : emoji;

  const actionHtml = !p.in_stock
    ? `<button class="add-btn" disabled style="opacity:0.4;background:#555">✕</button>`
    : inCart
    ? `<div class="qty-control">
        <button class="qty-btn" onclick="changeQty(${p.id},-1,event)">−</button>
        <span class="qty-num">${inCart.qty}</span>
        <button class="qty-btn" onclick="changeQty(${p.id},1,event)" style="background:var(--accent)">+</button>
       </div>`
    : `<button class="add-btn" onclick="addToCart(${p.id},event)">+</button>`;

  return `<div class="product-card" id="pcard-${p.id}">
    <div class="product-img">${imgContent}</div>
    ${!p.in_stock ? '<div class="out-of-stock">Нет в наличии</div>' : ''}
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

// ── CART LOGIC ─────────────────────────────────────────────────
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
  showToast(`✅ ${p.name} добавлен`);
  rerenderProductAction(id);
}

function changeQty(id, delta, e) {
  e?.stopPropagation();
  if (!state.cart[id]) return;
  state.cart[id].qty += delta;
  if (state.cart[id].qty <= 0) delete state.cart[id];
  updateCartUI();
  rerenderProductAction(id);
}

function rerenderProductAction(id) {
  const el = document.getElementById(`action-${id}`);
  if (!el) return;
  const p = findProduct(id);
  if (!p) return;
  const inCart = state.cart[id];
  el.innerHTML = !p.in_stock
    ? `<button class="add-btn" disabled style="opacity:0.4;background:#555">✕</button>`
    : inCart
    ? `<div class="qty-control">
        <button class="qty-btn" onclick="changeQty(${id},-1,event)">−</button>
        <span class="qty-num">${inCart.qty}</span>
        <button class="qty-btn" onclick="changeQty(${id},1,event)" style="background:var(--accent)">+</button>
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

// ── CART SCREEN ────────────────────────────────────────────────
function showCart() {
  showScreen('screen-cart');
  renderCart();
}

function renderCart() {
  const cartItems = Object.values(state.cart);
  const body = document.getElementById('cart-body');
  const checkout = document.getElementById('checkout-section');
  const actions = document.getElementById('cart-actions');

  if (!cartItems.length) {
    body.innerHTML = `<div class="empty-state"><div class="icon">🛒</div><p>Корзина пуста</p></div>`;
    checkout.style.display = 'none';
    actions.innerHTML = `<button class="action-btn" onclick="goBack('screen-catalog')">← Перейти к товарам</button>`;
    return;
  }

  const total = getCartTotal();
  body.innerHTML = `
    <div class="cart-items">
      ${cartItems.map(i => `
        <div class="cart-item">
          <div class="cart-item-emoji">${getProductEmoji(i.product.name)}</div>
          <div class="cart-item-info">
            <div class="cart-item-name">${i.product.name}</div>
            <div class="cart-item-price">${i.product.price} ₽ × ${i.qty} = ${i.product.price * i.qty} ₽</div>
          </div>
          <div class="cart-item-qty">
            <button class="qty-btn" onclick="cartChangeQty(${i.product.id},-1)">−</button>
            <span class="qty-num">${i.qty}</span>
            <button class="qty-btn" onclick="cartChangeQty(${i.product.id},1)" style="background:var(--accent)">+</button>
          </div>
        </div>
      `).join('')}
    </div>
    <div class="cart-total">
      <div>
        <div class="cart-total-label">Итого</div>
        <div class="cart-total-sum">${total} ₽</div>
      </div>
      <div style="font-size:32px">💰</div>
    </div>
  `;

  checkout.style.display = 'block';
  renderManagerContacts();
  actions.innerHTML = `<button class="action-btn" id="btn-checkout" onclick="submitOrder()">🛒 Оформить заказ</button>`;
}

function cartChangeQty(id, delta) {
  if (!state.cart[id]) return;
  state.cart[id].qty += delta;
  if (state.cart[id].qty <= 0) delete state.cart[id];
  updateCartUI();
  renderCart();
  rerenderProductAction(id);
}

// ── DELIVERY ───────────────────────────────────────────────────
function selectDelivery(type) {
  state.deliveryType = type;
  document.getElementById('opt-delivery').classList.toggle('selected', type === 'delivery');
  document.getElementById('opt-pickup').classList.toggle('selected', type === 'pickup');
  document.getElementById('delivery-form').style.display = type === 'delivery' ? 'block' : 'none';
  document.getElementById('pickup-info').style.display = type === 'pickup' ? 'block' : 'none';
}

function renderManagerContacts() {
  // Managers would be loaded from API in production
  // For now show generic message
  const el = document.getElementById('manager-contacts');
  el.innerHTML = `<div class="contact" style="margin-top:8px;font-size:13px;color:var(--muted)">
    После оформления заказа бот пришлёт контакт менеджера для самовывоза
  </div>`;
}

// ── ORDER SUBMIT ───────────────────────────────────────────────
function submitOrder() {
  const items = getCartItems();
  if (!items.length) return showToast('❌ Корзина пуста!');
  if (!state.selectedLocation) return showToast('❌ Выберите город!');

  const deliveryType = state.deliveryType;
  let address = '';

  if (deliveryType === 'delivery') {
    address = document.getElementById('address-input').value.trim();
    if (!address) {
      showToast('❌ Введите адрес доставки!');
      document.getElementById('address-input').focus();
      return;
    }
  }

  const payload = {
    location_id: state.selectedLocation.id,
    delivery_type: deliveryType,
    address: address,
    items: items,
    total: getCartTotal()
  };

  const btn = document.getElementById('btn-checkout');
  btn.disabled = true;
  btn.textContent = 'Отправляем...';

  try {
    tg.sendData(JSON.stringify(payload));
    // Clear cart
    state.cart = {};
    updateCartUI();
    btn.textContent = '✅ Заказ отправлен!';
    btn.classList.add('success');
    setTimeout(() => {
      tg.close();
    }, 1500);
  } catch(e) {
    btn.disabled = false;
    btn.textContent = '🛒 Оформить заказ';
    showToast('❌ Ошибка отправки. Попробуйте снова.');
  }
}

// ── NAVIGATION ─────────────────────────────────────────────────
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

function goBack(to) {
  showScreen(to);
  if (to === 'screen-catalog') renderProducts();
}

// ── TOAST ──────────────────────────────────────────────────────
let toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2000);
}

// ── handleCheckout alias ───────────────────────────────────────
function handleCheckout() { submitOrder(); }

// ── BOOT ───────────────────────────────────────────────────────
init();
</script>
</body>
</html>
