[telegram-shop-miniapp.html](https://github.com/user-attachments/files/30150500/telegram-shop-miniapp.html)
# hello<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<title>NOVA — магазин</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
  :root{
    --tg-bg: var(--tg-theme-bg-color, #ffffff);
    --tg-text: var(--tg-theme-text-color, #000000);
    --tg-hint: var(--tg-theme-hint-color, #999999);
    --tg-link: var(--tg-theme-link-color, #2481cc);
    --tg-btn: var(--tg-theme-button-color, #2481cc);
    --tg-btn-text: var(--tg-theme-button-text-color, #ffffff);
    --tg-secondary-bg: var(--tg-theme-secondary-bg-color, #f0f0f0);
    --tg-section-bg: var(--tg-theme-section-bg-color, #ffffff);
    --tg-section-header: var(--tg-theme-section-header-text-color, #6d6d72);
    --tg-destructive: var(--tg-theme-destructive-text-color, #e53935);
    --safe-top: env(safe-area-inset-top, 0px);
    --safe-bottom: env(safe-area-inset-bottom, 0px);
  }

  *{ box-sizing:border-box; -webkit-tap-highlight-color: transparent; }

  html,body{
    margin:0; padding:0; height:100%;
    background: var(--tg-secondary-bg);
    color: var(--tg-text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    overscroll-behavior-y: none;
  }

  #app{
    max-width: 520px;
    margin: 0 auto;
    min-height: 100%;
    padding-bottom: calc(96px + var(--safe-bottom));
    position: relative;
  }

  /* ---------- Header ---------- */
  .topbar{
    position: sticky; top:0; z-index:20;
    padding-top: var(--safe-top);
    background: var(--tg-bg);
    border-bottom: 1px solid rgba(0,0,0,0.06);
  }
  .topbar-inner{
    display:flex; align-items:center; justify-content:space-between;
    padding: 12px 16px;
  }
  .brand{
    display:flex; align-items:center; gap:10px;
  }
  .brand-mark{
    width:34px; height:34px; border-radius:10px;
    background: linear-gradient(135deg, var(--tg-btn), color-mix(in srgb, var(--tg-btn) 60%, #7b5cff));
    display:flex; align-items:center; justify-content:center;
    color: var(--tg-btn-text); font-weight:700; font-size:15px;
  }
  .brand-name{ font-size:17px; font-weight:700; letter-spacing:0.2px; }
  .brand-sub{ font-size:12px; color: var(--tg-hint); margin-top:1px; }

  .cart-btn{
    position: relative;
    width:38px; height:38px; border-radius:50%;
    background: var(--tg-secondary-bg);
    border:none; display:flex; align-items:center; justify-content:center;
    color: var(--tg-text); cursor:pointer;
  }
  .cart-badge{
    position:absolute; top:-2px; right:-2px;
    min-width:17px; height:17px; padding:0 4px;
    border-radius:9px; background: var(--tg-destructive);
    color:#fff; font-size:10px; font-weight:700;
    display:flex; align-items:center; justify-content:center;
    display:none;
  }

  .search-row{ padding: 0 16px 12px; }
  .search-box{
    display:flex; align-items:center; gap:8px;
    background: var(--tg-secondary-bg);
    border-radius: 11px; padding: 9px 12px;
  }
  .search-box input{
    border:none; background:transparent; outline:none;
    font-size:15px; color:var(--tg-text); width:100%;
  }
  .search-box svg{ flex-shrink:0; color: var(--tg-hint); }

  .chips{
    display:flex; gap:8px; overflow-x:auto; padding: 0 16px 12px;
    scrollbar-width:none;
  }
  .chips::-webkit-scrollbar{ display:none; }
  .chip{
    flex-shrink:0; padding:7px 14px; border-radius:16px;
    font-size:13.5px; font-weight:500; white-space:nowrap;
    background: var(--tg-secondary-bg); color: var(--tg-text);
    border:none; cursor:pointer; transition: background .15s, color .15s;
  }
  .chip.active{ background: var(--tg-btn); color: var(--tg-btn-text); }

  /* ---------- Screens ---------- */
  .screen{ display:none; animation: fadein .18s ease; }
  .screen.active{ display:block; }
  @keyframes fadein{ from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);} }

  .section-title{
    font-size:13px; font-weight:600; text-transform:uppercase;
    letter-spacing:0.3px; color: var(--tg-section-header);
    padding: 4px 16px 8px;
  }

  /* ---------- Product grid ---------- */
  .grid{
    display:grid; grid-template-columns: 1fr 1fr;
    gap:10px; padding: 0 16px 16px;
  }
  .card{
    background: var(--tg-section-bg); border-radius:14px;
    overflow:hidden; cursor:pointer;
    box-shadow: 0 1px 2px rgba(0,0,0,0.04);
  }
  .card-img{
    width:100%; aspect-ratio: 1 / 1; object-fit:cover;
    background: var(--tg-secondary-bg); display:block;
  }
  .card-body{ padding: 9px 10px 11px; }
  .card-name{ font-size:13.5px; font-weight:600; line-height:1.25; }
  .card-tag{ font-size:11.5px; color: var(--tg-hint); margin-top:1px; }
  .card-row{
    display:flex; align-items:center; justify-content:space-between;
    margin-top:8px;
  }
  .card-price{ font-size:14.5px; font-weight:700; }
  .add-btn{
    width:26px; height:26px; border-radius:50%; border:none;
    background: var(--tg-btn); color: var(--tg-btn-text);
    font-size:16px; line-height:1; cursor:pointer;
    display:flex; align-items:center; justify-content:center;
  }

  .empty{
    text-align:center; padding: 60px 24px; color: var(--tg-hint);
  }
  .empty-emoji{ font-size:40px; margin-bottom:10px; }

  /* ---------- Product detail ---------- */
  .pd-hero{ width:100%; aspect-ratio: 1/1; object-fit:cover; background:var(--tg-secondary-bg); }
  .pd-body{ padding: 16px; }
  .pd-name{ font-size:20px; font-weight:700; }
  .pd-tag{ font-size:13px; color:var(--tg-hint); margin-top:3px; }
  .pd-price{ font-size:22px; font-weight:800; margin-top:12px; }
  .pd-desc{ font-size:14.5px; line-height:1.55; color:var(--tg-text); margin-top:14px; }
  .pd-desc-title{ font-size:13px; font-weight:600; text-transform:uppercase; color:var(--tg-section-header); margin-bottom:6px; }

  .stepper{
    display:flex; align-items:center; gap:14px;
    background: var(--tg-secondary-bg); border-radius:12px;
    padding: 8px 14px; width:fit-content; margin-top:18px;
  }
  .stepper button{
    width:28px; height:28px; border-radius:50%; border:none;
    background: var(--tg-btn); color:var(--tg-btn-text);
    font-size:17px; cursor:pointer;
  }
  .stepper span{ font-size:16px; font-weight:700; min-width:18px; text-align:center; }

  /* ---------- Cart ---------- */
  .cart-item{
    display:flex; gap:12px; padding:12px 16px;
    background: var(--tg-section-bg);
    border-bottom: 1px solid rgba(0,0,0,0.05);
  }
  .cart-item img{ width:60px; height:60px; border-radius:10px; object-fit:cover; flex-shrink:0; background:var(--tg-secondary-bg); }
  .ci-info{ flex:1; min-width:0; }
  .ci-name{ font-size:14.5px; font-weight:600; }
  .ci-tag{ font-size:12px; color:var(--tg-hint); margin-top:1px; }
  .ci-row{ display:flex; align-items:center; justify-content:space-between; margin-top:8px; }
  .ci-price{ font-size:14px; font-weight:700; }
  .ci-step{ display:flex; align-items:center; gap:8px; }
  .ci-step button{
    width:22px; height:22px; border-radius:50%; border:1px solid var(--tg-hint);
    background:transparent; color:var(--tg-text); font-size:13px; cursor:pointer;
  }
  .ci-remove{ background:none; border:none; color:var(--tg-destructive); font-size:12.5px; cursor:pointer; padding:0; margin-top:2px;}

  .summary{
    background: var(--tg-section-bg); margin-top:10px;
  }
  .summary-row{
    display:flex; justify-content:space-between; padding:11px 16px;
    font-size:14.5px; border-bottom:1px solid rgba(0,0,0,0.05);
  }
  .summary-row.total{ font-weight:700; font-size:16px; }
  .summary-row .v-hint{ color: var(--tg-hint); }

  /* ---------- Header nav (product/cart) ---------- */
  .subheader{
    position:sticky; top:0; z-index:20;
    padding-top: var(--safe-top);
    background: var(--tg-bg);
    border-bottom:1px solid rgba(0,0,0,0.06);
    display:flex; align-items:center; gap:10px;
    padding-left:8px; padding-right:16px;
  }
  .back-btn{
    width:36px; height:36px; border:none; background:transparent;
    display:flex; align-items:center; justify-content:center;
    color: var(--tg-link); cursor:pointer;
  }
  .subheader-title{ font-size:16px; font-weight:600; padding:12px 0; }

  /* ---------- Bottom action bar (in-page, mirrors MainButton for browser preview) ---------- */
  .bottom-bar{
    position: fixed; left:0; right:0; bottom:0; z-index:30;
    max-width:520px; margin:0 auto;
    padding: 10px 16px calc(10px + var(--safe-bottom));
    background: var(--tg-bg);
    border-top: 1px solid rgba(0,0,0,0.06);
    display:none;
  }
  .bottom-bar.show{ display:block; }
  .main-btn{
    width:100%; border:none; border-radius:12px;
    background: var(--tg-btn); color: var(--tg-btn-text);
    font-size:16px; font-weight:600; padding:14px;
    display:flex; align-items:center; justify-content:center; gap:8px;
    cursor:pointer;
  }
  .main-btn:active{ opacity:0.85; }

  .toast{
    position:fixed; left:50%; bottom:110px; transform:translateX(-50%);
    background: rgba(20,20,20,0.9); color:#fff; font-size:13.5px;
    padding:9px 16px; border-radius:20px; z-index:50;
    opacity:0; pointer-events:none; transition: opacity .2s, bottom .2s;
  }
  .toast.show{ opacity:1; bottom:120px; }

  .success-screen{
    padding: 60px 24px; text-align:center;
  }
  .success-emoji{ font-size:56px; }
  .success-title{ font-size:20px; font-weight:700; margin-top:14px; }
  .success-text{ font-size:14.5px; color:var(--tg-hint); margin-top:8px; line-height:1.5; }
</style>
</head>
<body>

<div id="app">

  <!-- ===== CATALOG SCREEN ===== -->
  <div class="screen active" id="screen-catalog">
    <div class="topbar">
      <div class="topbar-inner">
        <div class="brand">
          <div class="brand-mark">N</div>
          <div>
            <div class="brand-name">NOVA</div>
            <div class="brand-sub">Одежда и аксессуары</div>
          </div>
        </div>
        <button class="cart-btn" onclick="openCart()">
          🛍️
          <span class="cart-badge" id="cartBadge">0</span>
        </button>
      </div>
      <div class="search-row">
        <div class="search-box">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="7"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
          <input type="text" id="searchInput" placeholder="Поиск товаров" oninput="renderCatalog()">
        </div>
      </div>
      <div class="chips" id="chips"></div>
    </div>
    <div class="section-title" id="gridTitle">Все товары</div>
    <div class="grid" id="grid"></div>
  </div>

  <!-- ===== PRODUCT SCREEN ===== -->
  <div class="screen" id="screen-product">
    <div class="subheader">
      <button class="back-btn" onclick="goTo('catalog')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><polyline points="15 18 9 12 15 6"/></svg>
      </button>
      <div class="subheader-title">Товар</div>
    </div>
    <img class="pd-hero" id="pdImg" src="" alt="">
    <div class="pd-body">
      <div class="pd-name" id="pdName"></div>
      <div class="pd-tag" id="pdTag"></div>
      <div class="pd-price" id="pdPrice"></div>
      <div class="stepper">
        <button onclick="changeQty(-1)">−</button>
        <span id="pdQty">1</span>
        <button onclick="changeQty(1)">+</button>
      </div>
      <div class="pd-desc-title" style="margin-top:20px;">Описание</div>
      <div class="pd-desc" id="pdDesc"></div>
    </div>
  </div>

  <!-- ===== CART SCREEN ===== -->
  <div class="screen" id="screen-cart">
    <div class="subheader">
      <button class="back-btn" onclick="goTo('catalog')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><polyline points="15 18 9 12 15 6"/></svg>
      </button>
      <div class="subheader-title">Корзина</div>
    </div>
    <div id="cartList"></div>
    <div class="summary" id="cartSummary" style="display:none;">
      <div class="summary-row"><span class="v-hint">Товары</span><span id="sumItems"></span></div>
      <div class="summary-row"><span class="v-hint">Доставка</span><span>бесплатно</span></div>
      <div class="summary-row total"><span>Итого</span><span id="sumTotal"></span></div>
    </div>
  </div>

  <!-- ===== SUCCESS SCREEN ===== -->
  <div class="screen" id="screen-success">
    <div class="subheader">
      <div style="width:36px;"></div>
      <div class="subheader-title">Заказ оформлен</div>
    </div>
    <div class="success-screen">
      <div class="success-emoji">✅</div>
      <div class="success-title">Спасибо за заказ!</div>
      <div class="success-text">Мы отправили детали заказа боту.<br>Менеджер свяжется с вами в чате Telegram.</div>
    </div>
  </div>

</div>

<div class="bottom-bar" id="bottomBar">
  <button class="main-btn" id="mainBtn" onclick="handleMainButton()">Оформить заказ</button>
</div>

<div class="toast" id="toast"></div>

<script>
/* =========================================================
   Telegram WebApp init (with safe fallback for browser preview)
   ========================================================= */
const tg = window.Telegram && window.Telegram.WebApp ? window.Telegram.WebApp : {
  ready(){}, expand(){}, close(){}, sendData(){},
  setHeaderColor(){}, setBackgroundColor(){},
  MainButton: { setText(){}, show(){}, hide(){}, onClick(){}, offClick(){}, showProgress(){}, hideProgress(){} },
  BackButton: { show(){}, hide(){}, onClick(){}, offClick(){} },
  HapticFeedback: { impactOccurred(){}, notificationOccurred(){} },
  themeParams: {}, colorScheme: 'light',
  onEvent(){}
};
tg.ready();
tg.expand();
try{ tg.setHeaderColor('secondary_bg_color'); }catch(e){}

/* Apply theme params as CSS vars so it matches the user's Telegram theme */
function applyTheme(){
  const p = tg.themeParams || {};
  const root = document.documentElement.style;
  const map = {
    bg_color:'--tg-theme-bg-color', text_color:'--tg-theme-text-color',
    hint_color:'--tg-theme-hint-color', link_color:'--tg-theme-link-color',
    button_color:'--tg-theme-button-color', button_text_color:'--tg-theme-button-text-color',
    secondary_bg_color:'--tg-theme-secondary-bg-color', section_bg_color:'--tg-theme-section-bg-color',
    section_header_text_color:'--tg-theme-section-header-text-color',
    destructive_text_color:'--tg-theme-destructive-text-color'
  };
  Object.keys(map).forEach(k=>{ if(p[k]) root.setProperty(map[k], p[k]); });
}
applyTheme();
tg.onEvent && tg.onEvent('themeChanged', applyTheme);

/* =========================================================
   Product data — замените на свои товары
   ========================================================= */
const PRODUCTS = [
  { id:1, name:'Оверсайз худи', tag:'Худи · унисекс', price:3990, cat:'Одежда', img:'https://picsum.photos/seed/nova1/500/500', desc:'Плотный хлопковый флис 340 г/м², прямой крой, регулируемый капюшон. Доступны размеры S–XL.' },
  { id:2, name:'Кроссовки Runner', tag:'Обувь', price:6490, cat:'Обувь', img:'https://picsum.photos/seed/nova2/500/500', desc:'Лёгкая амортизирующая подошва, дышащая сетка верха. Подходят для города и пробежек.' },
  { id:3, name:'Кепка классик', tag:'Аксессуары', price:1290, cat:'Аксессуары', img:'https://picsum.photos/seed/nova3/500/500', desc:'Плотная бейсболка с вышитым логотипом, регулируемый ремешок сзади.' },
  { id:4, name:'Джинсы прямые', tag:'Джинсы · унисекс', price:4590, cat:'Одежда', img:'https://picsum.photos/seed/nova4/500/500', desc:'Плотный деним 12 oz, прямой силуэт, средняя посадка.' },
  { id:5, name:'Рюкзак City', tag:'Аксессуары', price:3290, cat:'Аксессуары', img:'https://picsum.photos/seed/nova5/500/500', desc:'Водоотталкивающая ткань, отделение под ноутбук 15", объём 18 л.' },
  { id:6, name:'Футболка Basic', tag:'Футболки · унисекс', price:1590, cat:'Одежда', img:'https://picsum.photos/seed/nova6/500/500', desc:'100% хлопок плотностью 180 г/м², свободный крой, усиленные швы.' },
  { id:7, name:'Носки х3', tag:'Аксессуары', price:790, cat:'Аксессуары', img:'https://picsum.photos/seed/nova7/500/500', desc:'Набор из трёх пар, хлопок с эластаном, усиленная пятка.' },
  { id:8, name:'Сникеры Low', tag:'Обувь', price:5990, cat:'Обувь', img:'https://picsum.photos/seed/nova8/500/500', desc:'Минималистичный дизайн, кожзам премиум-класса, резиновая подошва.' },
];

const CATEGORIES = ['Все', ...Array.from(new Set(PRODUCTS.map(p=>p.cat)))];

/* =========================================================
   State
   ========================================================= */
let cart = {};          // { productId: qty }
let activeCategory = 'Все';
let currentProductId = null;
let currentScreen = 'catalog';

/* =========================================================
   Render: catalog
   ========================================================= */
function renderChips(){
  const el = document.getElementById('chips');
  el.innerHTML = CATEGORIES.map(c =>
    `<button class="chip ${c===activeCategory?'active':''}" onclick="setCategory('${c}')">${c}</button>`
  ).join('');
}

function setCategory(c){
  activeCategory = c;
  renderChips();
  renderCatalog();
  tg.HapticFeedback && tg.HapticFeedback.impactOccurred('light');
}

function renderCatalog(){
  const q = (document.getElementById('searchInput').value || '').trim().toLowerCase();
  let items = PRODUCTS.filter(p => activeCategory==='Все' || p.cat===activeCategory);
  if(q) items = items.filter(p => p.name.toLowerCase().includes(q));

  document.getElementById('gridTitle').textContent = activeCategory==='Все' ? 'Все товары' : activeCategory;

  const grid = document.getElementById('grid');
  if(items.length===0){
    grid.style.display='none';
    grid.parentElement.querySelector('.section-title').style.display='none';
    if(!document.getElementById('emptyState')){
      const empty = document.createElement('div');
      empty.id='emptyState';
      empty.className='empty';
      empty.innerHTML = `<div class="empty-emoji">🔍</div>Ничего не найдено`;
      grid.parentElement.appendChild(empty);
    }
    return;
  }
  const existingEmpty = document.getElementById('emptyState');
  if(existingEmpty) existingEmpty.remove();
  grid.style.display='grid';
  grid.parentElement.querySelector('.section-title').style.display='block';

  grid.innerHTML = items.map(p => `
    <div class="card" onclick="openProduct(${p.id})">
      <img class="card-img" src="${p.img}" alt="${p.name}">
      <div class="card-body">
        <div class="card-name">${p.name}</div>
        <div class="card-tag">${p.tag}</div>
        <div class="card-row">
          <div class="card-price">${fmt(p.price)}</div>
          <button class="add-btn" onclick="event.stopPropagation(); addToCart(${p.id}, 1); bumpToast('Добавлено в корзину');">+</button>
        </div>
      </div>
    </div>
  `).join('');
}

/* =========================================================
   Render: product detail
   ========================================================= */
function openProduct(id){
  currentProductId = id;
  const p = PRODUCTS.find(x=>x.id===id);
  document.getElementById('pdImg').src = p.img;
  document.getElementById('pdName').textContent = p.name;
  document.getElementById('pdTag').textContent = p.tag;
  document.getElementById('pdPrice').textContent = fmt(p.price);
  document.getElementById('pdDesc').textContent = p.desc;
  document.getElementById('pdQty').textContent = '1';
  goTo('product');
}

function changeQty(delta){
  const el = document.getElementById('pdQty');
  let v = parseInt(el.textContent,10) + delta;
  if(v<1) v=1;
  el.textContent = v;
  tg.HapticFeedback && tg.HapticFeedback.impactOccurred('light');
}

/* =========================================================
   Cart logic
   ========================================================= */
function addToCart(id, qty){
  cart[id] = (cart[id]||0) + qty;
  updateCartBadge();
}

function removeFromCart(id){
  delete cart[id];
  updateCartBadge();
  renderCart();
}

function setQtyInCart(id, qty){
  if(qty<=0){ removeFromCart(id); return; }
  cart[id]=qty;
  updateCartBadge();
  renderCart();
}

function cartCount(){
  return Object.values(cart).reduce((a,b)=>a+b,0);
}

function cartTotal(){
  return Object.entries(cart).reduce((sum,[id,qty])=>{
    const p = PRODUCTS.find(x=>x.id==id);
    return sum + (p ? p.price*qty : 0);
  },0);
}

function updateCartBadge(){
  const badge = document.getElementById('cartBadge');
  const n = cartCount();
  badge.textContent = n;
  badge.style.display = n>0 ? 'flex' : 'none';
}

function renderCart(){
  const list = document.getElementById('cartList');
  const entries = Object.entries(cart);
  if(entries.length===0){
    list.innerHTML = `<div class="empty"><div class="empty-emoji">🛍️</div>Корзина пуста<br><span style="font-size:13px;">Добавьте товары из каталога</span></div>`;
    document.getElementById('cartSummary').style.display='none';
    return;
  }
  document.getElementById('cartSummary').style.display='block';
  list.innerHTML = entries.map(([id,qty])=>{
    const p = PRODUCTS.find(x=>x.id==id);
    return `
    <div class="cart-item">
      <img src="${p.img}" alt="${p.name}">
      <div class="ci-info">
        <div class="ci-name">${p.name}</div>
        <div class="ci-tag">${p.tag}</div>
        <div class="ci-row">
          <div class="ci-price">${fmt(p.price*qty)}</div>
          <div class="ci-step">
            <button onclick="setQtyInCart(${p.id}, ${qty-1})">−</button>
            <span>${qty}</span>
            <button onclick="setQtyInCart(${p.id}, ${qty+1})">+</button>
          </div>
        </div>
        <button class="ci-remove" onclick="removeFromCart(${p.id})">Удалить</button>
      </div>
    </div>`;
  }).join('');

  document.getElementById('sumItems').textContent = fmt(cartTotal());
  document.getElementById('sumTotal').textContent = fmt(cartTotal());
}

/* =========================================================
   Navigation + MainButton sync
   ========================================================= */
function goTo(screen){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById('screen-'+screen).classList.add('active');
  currentScreen = screen;
  window.scrollTo(0,0);

  if(screen==='cart') renderCart();

  syncMainButton();
  syncBackButton();
}

function openCart(){ goTo('cart'); }

function syncBackButton(){
  if(currentScreen==='catalog'){
    tg.BackButton.hide();
  } else {
    tg.BackButton.show();
  }
}
tg.BackButton.onClick && tg.BackButton.onClick(()=>{
  if(currentScreen==='product') goTo('catalog');
  else if(currentScreen==='cart') goTo('catalog');
  else if(currentScreen==='success') goTo('catalog');
});

function syncMainButton(){
  const bar = document.getElementById('bottomBar');
  const btn = document.getElementById('mainBtn');

  if(currentScreen==='product'){
    const qty = parseInt(document.getElementById('pdQty').textContent,10);
    const p = PRODUCTS.find(x=>x.id===currentProductId);
    const label = `Добавить · ${fmt(p.price*qty)}`;
    btn.textContent = label;
    bar.classList.add('show');
    tg.MainButton.setText(label);
    tg.MainButton.show();
  } else if(currentScreen==='cart' && cartCount()>0){
    const label = `Оформить заказ · ${fmt(cartTotal())}`;
    btn.textContent = label;
    bar.classList.add('show');
    tg.MainButton.setText(label);
    tg.MainButton.show();
  } else {
    bar.classList.remove('show');
    tg.MainButton.hide();
  }
}

function handleMainButton(){
  if(currentScreen==='product'){
    const qty = parseInt(document.getElementById('pdQty').textContent,10);
    addToCart(currentProductId, qty);
    bumpToast('Добавлено в корзину');
    tg.HapticFeedback && tg.HapticFeedback.notificationOccurred('success');
    goTo('catalog');
  } else if(currentScreen==='cart'){
    submitOrder();
  }
}
tg.MainButton.onClick && tg.MainButton.onClick(handleMainButton);

function submitOrder(){
  const order = {
    items: Object.entries(cart).map(([id,qty])=>{
      const p = PRODUCTS.find(x=>x.id==id);
      return { id:p.id, name:p.name, price:p.price, qty };
    }),
    total: cartTotal()
  };
  try{ tg.sendData(JSON.stringify(order)); }catch(e){}
  tg.HapticFeedback && tg.HapticFeedback.notificationOccurred('success');
  cart = {};
  updateCartBadge();
  goTo('success');
  setTimeout(()=>{ try{ tg.close(); }catch(e){} }, 1800);
}

/* =========================================================
   Utilities
   ========================================================= */
function fmt(n){
  return n.toLocaleString('ru-RU') + ' ₽';
}

let toastTimer;
function bumpToast(msg){
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(()=>t.classList.remove('show'), 1500);
}

/* =========================================================
   Init
   ========================================================= */
renderChips();
renderCatalog();
updateCartBadge();
syncBackButton();
</script>
</body>
</html>
