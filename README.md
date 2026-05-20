<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>⚔️ BRAWL CLICKER — Telegram Mini App ⚔️</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            min-height: 100vh;
            background: var(--tg-theme-bg-color, #0a0a0a);
            font-family: 'Segoe UI', 'Cinema', 'Impact', 'Franklin Gothic Heavy', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 0.5rem;
            transition: background 0.2s;
        }

        .game-container {
            max-width: 950px;
            width: 100%;
            background: var(--tg-theme-secondary-bg-color, rgba(15, 10, 10, 0.95));
            backdrop-filter: blur(3px);
            border-radius: 2rem;
            padding: 0.8rem 1rem 1.2rem;
            box-shadow: 0 0 30px rgba(180, 30, 30, 0.2), inset 0 1px 2px rgba(255,80,80,0.1);
            border: 1px solid #8b2c2c;
        }

        .top-bar {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 0.4rem;
            background: var(--tg-theme-secondary-bg-color, #0a0a0a);
            padding: 0.4rem 0.6rem;
            border-radius: 2rem;
            margin-bottom: 1rem;
            border: 1px solid #b33;
        }
        .stat-pill {
            background: rgba(26, 16, 16, 0.7);
            border-radius: 2rem;
            padding: 0.2rem 0.6rem;
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.75rem;
            font-weight: bold;
            color: #ffcc77;
            border: 1px solid #a44;
            font-family: monospace;
            white-space: nowrap;
        }
        .energy-bar-bg {
            width: 50px;
            height: 6px;
            background: #3a1a1a;
            border-radius: 10px;
            overflow: hidden;
            display: inline-block;
            margin-left: 4px;
        }
        .energy-fill { height: 100%; width: 100%; background: #ff5555; border-radius: 10px; }
        .time-text { font-family: monospace; }

        .tabs {
            display: flex;
            gap: 0.5rem;
            margin-bottom: 1rem;
            background: #0f0f0f;
            padding: 0.2rem;
            border-radius: 3rem;
            border: 1px solid #9a3a3a;
        }
        .tab-btn {
            flex: 1;
            background: #220000;
            border: none;
            padding: 0.5rem 0.2rem;
            font-weight: 800;
            font-size: 0.8rem;
            border-radius: 2rem;
            color: #ffaa77;
            cursor: pointer;
            text-transform: uppercase;
        }
        .tab-btn.active {
            background: linear-gradient(95deg, #cc3300, #991111);
            color: #ffefc0;
            box-shadow: 0 0 12px #ff4444;
        }
        .tab-content { display: none; animation: fadeRise 0.2s ease; }
        .tab-content.active-tab { display: block; }
        @keyframes fadeRise { from { opacity: 0; transform: translateY(6px);} to { opacity: 1; transform: translateY(0);} }

        .double-click-area {
            display: flex;
            justify-content: center;
            gap: 1.2rem;
            flex-wrap: wrap;
            margin: 0.8rem 0 1.2rem;
        }
        .click-button {
            display: flex;
            flex-direction: column;
            align-items: center;
            cursor: pointer;
            background: transparent;
            border: none;
            transition: transform 0.07s;
            padding: 0.3rem;
        }
        .click-button:active { transform: scale(0.95); }
        .click-icon {
            width: 85px;
            height: 85px;
            object-fit: contain;
            filter: drop-shadow(2px 4px 8px rgba(0,0,0,0.5));
            margin-bottom: 6px;
        }
        .click-label {
            background: #000000aa;
            border-radius: 2rem;
            padding: 4px 10px;
            font-weight: bold;
            color: #ffcc88;
            font-size: 0.7rem;
            backdrop-filter: blur(4px);
            border: 1px solid #ffaa55;
        }

        .resources-panel {
            display: flex;
            justify-content: space-between;
            gap: 0.8rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
        }
        .resource-card {
            background: #1f1111;
            border-radius: 1.5rem;
            padding: 0.3rem 0.6rem;
            flex: 1;
            text-align: center;
            border: 1px solid #aa5555;
        }
        .resource-label { font-size: 0.65rem; color: #ffaa77; }
        .resource-value { font-size: 1.2rem; font-weight: 800; color: #ffcc66; line-height: 1; }

        .upgrades-title {
            font-size: 0.9rem;
            font-weight: 800;
            margin: 0.6rem 0 0.5rem;
            color: #ffaa66;
            border-left: 5px solid #cc3333;
            padding-left: 12px;
        }
        .upgrade-grid { display: flex; flex-direction: column; gap: 0.6rem; }
        .upgrade-card {
            background: #0e0808;
            border-radius: 1.2rem;
            padding: 0.5rem 0.8rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 6px;
            border: 1px solid #aa3a3a;
        }
        .upgrade-info { flex: 2; }
        .upgrade-name { font-weight: 800; font-size: 0.8rem; color: #ffb37c; }
        .upgrade-desc { font-size: 0.6rem; color: #b99; }
        .upgrade-stats { font-size: 0.65rem; font-weight: 600; color: #ffaa66; }
        .buy-btn {
            background: linear-gradient(95deg, #aa2222, #660000);
            border: none;
            padding: 4px 10px;
            border-radius: 2rem;
            font-weight: 800;
            font-size: 0.7rem;
            color: #ffe5c8;
            cursor: pointer;
            width: 100%;
        }
        .buy-btn:disabled { opacity: 0.5; cursor: not-allowed; }

        .profile-card {
            background: #0f0808;
            border-radius: 1.5rem;
            padding: 0.8rem;
            border: 2px solid #b44;
        }
        .profile-stat {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 0.6rem 0;
            background: #1a1010;
            padding: 0.4rem 0.8rem;
            border-radius: 2rem;
            flex-wrap: wrap;
            gap: 8px;
            color: white;
            font-family: 'Segoe UI', sans-serif;
        }
        .edit-name-input {
            background: #2a1a1a;
            border: 1px solid #ffaa66;
            border-radius: 2rem;
            padding: 3px 10px;
            color: #ffefc0;
            outline: none;
        }
        .save-name-btn, .exchange-btn {
            background: #2a4a2a;
            border: none;
            padding: 4px 12px;
            border-radius: 2rem;
            font-weight: bold;
            color: #ffefb0;
            cursor: pointer;
        }
        .exchange-group { display: flex; gap: 0.8rem; flex-wrap: wrap; justify-content: center; margin-top: 0.8rem; }
        .reset-btn {
            background: #440000;
            border: 1px solid #ff6666;
            color: #ffaa88;
            width: 100%;
            margin-top: 1rem;
            padding: 8px;
            border-radius: 3rem;
            font-weight: 800;
            cursor: pointer;
        }
        footer { font-size: 0.5rem; text-align: center; color: #5e4a4a; margin-top: 0.8rem; }

        @media (max-width: 600px) {
            .click-icon { width: 70px; height: 70px; }
            .resource-value { font-size: 1rem; }
            .stat-pill { font-size: 0.65rem; }
        }
    </style>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
</head>
<body>
<div class="game-container" id="gameContainer">
    <div class="top-bar">
        <div class="stat-pill">👤 <span id="playerNameDisplay">Викинг</span></div>
        <div class="stat-pill">⚡ Энергия <span id="energyValue">100</span><div class="energy-bar-bg"><div class="energy-fill" id="energyFill"></div></div></div>
        <div class="stat-pill">🪵 <span id="woodAmount">0</span></div>
        <div class="stat-pill">🍎 <span id="foodAmount">0</span></div>
        <div class="stat-pill">💰 <span id="rublesAmount">0.00</span></div>
        <div class="stat-pill time-text">🕒 <span id="currentTime">--:--:--</span></div>
    </div>

    <div class="tabs">
        <button class="tab-btn active" data-tab="tab-game">⚔️ ИГРА</button>
        <button class="tab-btn" data-tab="tab-profile">🏰 ЛИЧНЫЙ КАБИНЕТ</button>
    </div>

    <div id="tab-game" class="tab-content active-tab">
        <div class="double-click-area">
            <div class="click-button" id="woodClickBtn">
                <img class="click-icon" src="https://i.pinimg.com/736x/ff/a3/b0/ffa3b0266b4222e251340d81238d7858.jpg" alt="Топор" onerror="this.src='https://cdn-icons-png.flaticon.com/512/2936/2936886.png';">
                <div class="click-label">РУБИТЬ</div>
            </div>
            <div class="click-button" id="foodClickBtn">
                <img class="click-icon" src="https://img.magnific.com/premium-vector/chicken-cutting-board-with-picture-chicken-it_608055-185.jpg?semt=ais_hybrid" alt="Еда" onerror="this.src='https://cdn-icons-png.flaticon.com/512/2970/2970612.png';">
                <div class="click-label">СОБИРАТЬ</div>
            </div>
        </div>

        <div class="resources-panel">
            <div class="resource-card"><div class="resource-label">🪓 Сила рубки</div><div class="resource-value" id="woodPowerStat">1</div></div>
            <div class="resource-card"><div class="resource-label">🍎 Сила сбора</div><div class="resource-value" id="foodPowerStat">1</div></div>
            <div class="resource-card"><div class="resource-label">📈 Пассив/сек</div><div class="resource-value" id="resourcesPerSec">0 / 0</div></div>
        </div>

        <div class="upgrades-title">🏆 УЛУЧШЕНИЯ</div>
        <div class="upgrade-grid">
            <div class="upgrade-card"><div class="upgrade-info"><div class="upgrade-name">🪓 КРЕПКИЙ ТОПОР</div><div class="upgrade-stats">Уровень: <strong id="woodPowerLevel">1</strong> | Стоимость: <span id="woodPowerCost">100</span> 🪵</div></div><div class="upgrade-action"><button class="buy-btn" id="buyWoodPowerBtn">КУПИТЬ</button></div></div>
            <div class="upgrade-card"><div class="upgrade-info"><div class="upgrade-name">🍎 БЫСТРЫЙ ПОИСК</div><div class="upgrade-stats">Уровень: <strong id="foodPowerLevel">1</strong> | Стоимость: <span id="foodPowerCost">100</span> 🍎</div></div><div class="upgrade-action"><button class="buy-btn" id="buyFoodPowerBtn">КУПИТЬ</button></div></div>
            <div class="upgrade-card"><div class="upgrade-info"><div class="upgrade-name">🌲 ЛЕСНОЙ ДУХ</div><div class="upgrade-stats">Уровень: <strong id="woodPassiveLevel">0</strong> | Доход: <span id="woodPassiveIncome">0</span>/сек | Стоимость: <span id="woodPassiveCost">100</span> 🪵</div></div><div class="upgrade-action"><button class="buy-btn" id="buyWoodPassiveBtn">КУПИТЬ</button></div></div>
            <div class="upgrade-card"><div class="upgrade-info"><div class="upgrade-name">🏹 БЫСТРАЯ ОХОТА</div><div class="upgrade-stats">Уровень: <strong id="foodPassiveLevel">0</strong> | Доход: <span id="foodPassiveIncome">0</span>/сек | Стоимость: <span id="foodPassiveCost">100</span> 🍎</div></div><div class="upgrade-action"><button class="buy-btn" id="buyFoodPassiveBtn">КУПИТЬ</button></div></div>
        </div>
    </div>

    <div id="tab-profile" class="tab-content">
        <div class="profile-card">
            <div class="profile-stat"><span>🏷️ Имя викинга:</span><strong id="profileNameDisplay">Викинг</strong><input type="text" id="newNameInput" class="edit-name-input" placeholder="Новое имя" maxlength="20"><button id="saveNameBtn" class="save-name-btn">СОХР.</button></div>
            <div class="profile-stat"><span>💵 КОШЕЛЕК:</span><strong id="rublesProfile">0.00</strong> ₽</div>
            <div class="exchange-group"><button class="exchange-btn" id="exchangeWoodBtn">🪵 1000 древесины → 1 ₽</button><button class="exchange-btn" id="exchangeFoodBtn">🍎 1000 еды → 1 ₽</button></div>
            <div style="font-size:0.65rem; text-align:center; margin-top:8px; color:#ccaa77;">Рубли — универсальная валюта</div>
        </div>
    </div>

    <button class="reset-btn" id="resetGameBtn">🔥 СБРОСИТЬ ВСЁ</button>
    <footer>⚡ Энергия восстанавливается 0.5/сек. Улучшения удваивают стоимость.</footer>
</div>

<script>
    // --- Telegram WebApp инициализация ---
    let tg = window.Telegram?.WebApp;
    if (tg) {
        tg.expand(); // разворачиваем на весь экран
        tg.enableClosingConfirmation();
        // применяем тему Telegram к контейнеру
        const container = document.getElementById('gameContainer');
        if (tg.themeParams.bg_color) container.style.backgroundColor = tg.themeParams.bg_color;
        if (tg.themeParams.text_color) document.body.style.color = tg.themeParams.text_color;
    }

    // ---------- ИГРОВЫЕ ПЕРЕМЕННЫЕ ----------
    let wood = 0, food = 0, rubles = 0, playerName = "Викинг";
    let woodPower = 1, woodPowerCost = 100;
    let foodPower = 1, foodPowerCost = 100;
    let woodPassiveLevel = 0, woodPassiveCost = 100;
    let foodPassiveLevel = 0, foodPassiveCost = 100;
    let totalWoodMined = 0, totalFoodCollected = 0;
    let currentEnergy = 100;
    const MAX_ENERGY = 100;

    // DOM
    const woodSpan = document.getElementById('woodAmount');
    const foodSpan = document.getElementById('foodAmount');
    const rublesSpan = document.getElementById('rublesAmount');
    const rublesProfileSpan = document.getElementById('rublesProfile');
    const woodPowerStatSpan = document.getElementById('woodPowerStat');
    const foodPowerStatSpan = document.getElementById('foodPowerStat');
    const resourcesPerSecSpan = document.getElementById('resourcesPerSec');
    const energyValueSpan = document.getElementById('energyValue');
    const energyFill = document.getElementById('energyFill');
    const woodPowerLevelSpan = document.getElementById('woodPowerLevel');
    const woodPowerCostSpan = document.getElementById('woodPowerCost');
    const foodPowerLevelSpan = document.getElementById('foodPowerLevel');
    const foodPowerCostSpan = document.getElementById('foodPowerCost');
    const woodPassiveLevelSpan = document.getElementById('woodPassiveLevel');
    const woodPassiveIncomeSpan = document.getElementById('woodPassiveIncome');
    const woodPassiveCostSpan = document.getElementById('woodPassiveCost');
    const foodPassiveLevelSpan = document.getElementById('foodPassiveLevel');
    const foodPassiveIncomeSpan = document.getElementById('foodPassiveIncome');
    const foodPassiveCostSpan = document.getElementById('foodPassiveCost');
    const playerNameDisplay = document.getElementById('playerNameDisplay');
    const profileNameDisplay = document.getElementById('profileNameDisplay');
    const newNameInput = document.getElementById('newNameInput');
    const saveNameBtn = document.getElementById('saveNameBtn');
    const exchangeWoodBtn = document.getElementById('exchangeWoodBtn');
    const exchangeFoodBtn = document.getElementById('exchangeFoodBtn');
    const resetBtn = document.getElementById('resetGameBtn');
    const woodClickBtn = document.getElementById('woodClickBtn');
    const foodClickBtn = document.getElementById('foodClickBtn');
    const buyWoodPowerBtn = document.getElementById('buyWoodPowerBtn');
    const buyFoodPowerBtn = document.getElementById('buyFoodPowerBtn');
    const buyWoodPassiveBtn = document.getElementById('buyWoodPassiveBtn');
    const buyFoodPassiveBtn = document.getElementById('buyFoodPassiveBtn');

    function formatNumber(num) {
        if (num >= 1e12) return (num/1e12).toFixed(1)+'T';
        if (num >= 1e9) return (num/1e9).toFixed(1)+'B';
        if (num >= 1e6) return (num/1e6).toFixed(1)+'M';
        if (num >= 1e3) return (num/1e3).toFixed(1)+'K';
        if (num < 1 && num > 0) return num.toFixed(2);
        return Math.floor(num * 100) / 100;
    }

    function getWoodPassivePerSec() { return woodPassiveLevel * 0.1; }
    function getFoodPassivePerSec() { return foodPassiveLevel * 0.1; }

    function updateUI() {
        woodSpan.innerText = formatNumber(wood);
        foodSpan.innerText = formatNumber(food);
        rublesSpan.innerText = rubles.toFixed(2);
        rublesProfileSpan.innerText = rubles.toFixed(2);
        woodPowerStatSpan.innerText = woodPower;
        foodPowerStatSpan.innerText = foodPower;
        let wPass = getWoodPassivePerSec(), fPass = getFoodPassivePerSec();
        resourcesPerSecSpan.innerText = `${formatNumber(wPass)} / ${formatNumber(fPass)}`;
        woodPowerLevelSpan.innerText = woodPower;
        woodPowerCostSpan.innerText = formatNumber(woodPowerCost);
        foodPowerLevelSpan.innerText = foodPower;
        foodPowerCostSpan.innerText = formatNumber(foodPowerCost);
        woodPassiveLevelSpan.innerText = woodPassiveLevel;
        woodPassiveIncomeSpan.innerText = formatNumber(wPass);
        woodPassiveCostSpan.innerText = formatNumber(woodPassiveCost);
        foodPassiveLevelSpan.innerText = foodPassiveLevel;
        foodPassiveIncomeSpan.innerText = formatNumber(fPass);
        foodPassiveCostSpan.innerText = formatNumber(foodPassiveCost);
        buyWoodPowerBtn.disabled = wood < woodPowerCost;
        buyFoodPowerBtn.disabled = food < foodPowerCost;
        buyWoodPassiveBtn.disabled = wood < woodPassiveCost;
        buyFoodPassiveBtn.disabled = food < foodPassiveCost;
        exchangeWoodBtn.disabled = wood < 1000;
        exchangeFoodBtn.disabled = food < 1000;
        updateEnergyUI();
    }

    function updateEnergyUI() {
        energyValueSpan.innerText = Math.floor(currentEnergy);
        let percent = (currentEnergy / MAX_ENERGY) * 100;
        energyFill.style.width = percent + "%";
    }

    function spendEnergy(amount) {
        if (currentEnergy >= amount) { currentEnergy -= amount; updateEnergyUI(); return true; }
        return false;
    }

    function regenEnergy() {
        if (currentEnergy < MAX_ENERGY) { currentEnergy = Math.min(MAX_ENERGY, currentEnergy + 0.5); updateEnergyUI(); }
    }

    function handleWoodClick() {
        if (!spendEnergy(1)) { flashMessage("⚠️ Нет энергии!", true); return; }
        let gain = woodPower;
        wood += gain; totalWoodMined += gain;
        updateUI(); saveGame();
        createFloatingPlus(gain, '🪵');
        if (navigator.vibrate) navigator.vibrate(20);
    }

    function handleFoodClick() {
        if (!spendEnergy(1)) { flashMessage("⚠️ Нет энергии!", true); return; }
        let gain = foodPower;
        food += gain; totalFoodCollected += gain;
        updateUI(); saveGame();
        createFloatingPlus(gain, '🍎');
        if (navigator.vibrate) navigator.vibrate(20);
    }

    function buyWoodPower() { if (wood >= woodPowerCost) { wood -= woodPowerCost; woodPower++; woodPowerCost = Math.floor(woodPowerCost * 2); updateUI(); saveGame(); flashMessage(`🪓 Крепкий топор ${woodPower}`); } else flashMessage("Не хватает древесины!", true); }
    function buyFoodPower() { if (food >= foodPowerCost) { food -= foodPowerCost; foodPower++; foodPowerCost = Math.floor(foodPowerCost * 2); updateUI(); saveGame(); flashMessage(`🍎 Быстрый поиск ${foodPower}`); } else flashMessage("Не хватает еды!", true); }
    function buyWoodPassive() { if (wood >= woodPassiveCost) { wood -= woodPassiveCost; woodPassiveLevel++; woodPassiveCost = Math.floor(woodPassiveCost * 2); updateUI(); saveGame(); flashMessage(`🌲 Лесной дух +0.1/сек`); } else flashMessage("Не хватает древесины!", true); }
    function buyFoodPassive() { if (food >= foodPassiveCost) { food -= foodPassiveCost; foodPassiveLevel++; foodPassiveCost = Math.floor(foodPassiveCost * 2); updateUI(); saveGame(); flashMessage(`🏹 Быстрая охота +0.1/сек`); } else flashMessage("Не хватает еды!", true); }
    function exchangeWoodToRubles() { if (wood >= 1000) { let cnt = Math.floor(wood/1000); rubles += cnt; wood -= cnt*1000; updateUI(); saveGame(); flashMessage(`💰 +${cnt} руб.`); } else flashMessage("❌ Нужно 1000 древесины!", true); }
    function exchangeFoodToRubles() { if (food >= 1000) { let cnt = Math.floor(food/1000); rubles += cnt; food -= cnt*1000; updateUI(); saveGame(); flashMessage(`💰 +${cnt} руб.`); } else flashMessage("❌ Нужно 1000 еды!", true); }
    function changePlayerName() { let newName = newNameInput.value.trim(); if (newName === "") { flashMessage("Имя не может быть пустым!", true); return; } playerName = newName; playerNameDisplay.innerText = playerName; profileNameDisplay.innerText = playerName; newNameInput.value = ""; saveGame(); flashMessage(`Имя → ${playerName}`); }
    function resetGame() { if (confirm("Сбросить всё?")) { localStorage.clear(); location.reload(); } }

    function startPassiveIncome() {
        setInterval(() => {
            let wGain = getWoodPassivePerSec(), fGain = getFoodPassivePerSec();
            if (wGain > 0 || fGain > 0) {
                wood += wGain; food += fGain; totalWoodMined += wGain; totalFoodCollected += fGain;
                updateUI(); saveGame();
            }
        }, 1000);
    }

    function startEnergyRegen() { setInterval(() => regenEnergy(), 1000); }
    function updateClock() { document.getElementById('currentTime').innerText = new Date().toLocaleTimeString('ru-RU'); }
    function saveGame() { localStorage.setItem('brawlClickerTG', JSON.stringify({ wood, food, rubles, playerName, woodPower, woodPowerCost, foodPower, foodPowerCost, woodPassiveLevel, woodPassiveCost, foodPassiveLevel, foodPassiveCost, totalWoodMined, totalFoodCollected, currentEnergy })); }
    function loadGame() {
        let s = localStorage.getItem('brawlClickerTG');
        if (!s) return;
        try {
            let d = JSON.parse(s);
            wood = d.wood ?? 0; food = d.food ?? 0; rubles = d.rubles ?? 0; playerName = d.playerName ?? "Викинг";
            woodPower = d.woodPower ?? 1; woodPowerCost = d.woodPowerCost ?? 100;
            foodPower = d.foodPower ?? 1; foodPowerCost = d.foodPowerCost ?? 100;
            woodPassiveLevel = d.woodPassiveLevel ?? 0; woodPassiveCost = d.woodPassiveCost ?? 100;
            foodPassiveLevel = d.foodPassiveLevel ?? 0; foodPassiveCost = d.foodPassiveCost ?? 100;
            totalWoodMined = d.totalWoodMined ?? 0; totalFoodCollected = d.totalFoodCollected ?? 0;
            currentEnergy = (d.currentEnergy !== undefined) ? d.currentEnergy : 100;
            if (currentEnergy > MAX_ENERGY) currentEnergy = MAX_ENERGY;
            playerNameDisplay.innerText = playerName; profileNameDisplay.innerText = playerName;
            updateUI();
        } catch(e) {}
    }

    function flashMessage(text, isErr=false) {
        let div = document.createElement('div');
        div.innerText = text;
        div.style.cssText = `position:fixed; bottom:30%; left:50%; transform:translateX(-50%); background:${isErr?'#991111dd':'#226622dd'}; padding:6px 16px; border-radius:40px; color:#ffefb0; font-weight:bold; z-index:2000; font-size:0.8rem; backdrop-filter:blur(6px); border:1px solid gold; white-space:nowrap;`;
        document.body.appendChild(div);
        setTimeout(()=>div.remove(),1500);
    }
    function createFloatingPlus(val, icon) {
        let div = document.createElement('div');
        div.innerText = `+${val} ${icon}`;
        div.style.cssText = `position:fixed; left:50%; top:55%; transform:translate(-50%,-50%); font-size:1.2rem; font-weight:800; color:#ffdd88; text-shadow:0 0 4px red; pointer-events:none; z-index:2000; animation:floatUp 0.6s forwards;`;
        if(!document.querySelector('#floatStyle')) {
            let st = document.createElement('style');
            st.id = 'floatStyle';
            st.innerText = `@keyframes floatUp{0%{opacity:1; transform:translate(-50%,-50%) scale(0.7);}100%{opacity:0; transform:translate(-50%,-120%) scale(1.2);}}`;
            document.head.appendChild(st);
        }
        document.body.appendChild(div);
        setTimeout(()=>div.remove(),600);
    }

    function initTabs() {
        let btns = document.querySelectorAll('.tab-btn');
        let contents = document.querySelectorAll('.tab-content');
        btns.forEach(btn => btn.addEventListener('click', () => {
            let tabId = btn.dataset.tab;
            btns.forEach(b=>b.classList.remove('active'));
            btn.classList.add('active');
            contents.forEach(c=>c.classList.remove('active-tab'));
            document.getElementById(tabId).classList.add('active-tab');
        }));
    }

    function init() {
        loadGame();
        startPassiveIncome();
        startEnergyRegen();
        setInterval(updateClock, 1000);
        updateClock();
        updateUI();
        initTabs();
        woodClickBtn.addEventListener('click', handleWoodClick);
        foodClickBtn.addEventListener('click', handleFoodClick);
        buyWoodPowerBtn.addEventListener('click', buyWoodPower);
        buyFoodPowerBtn.addEventListener('click', buyFoodPower);
        buyWoodPassiveBtn.addEventListener('click', buyWoodPassive);
        buyFoodPassiveBtn.addEventListener('click', buyFoodPassive);
        exchangeWoodBtn.addEventListener('click', exchangeWoodToRubles);
        exchangeFoodBtn.addEventListener('click', exchangeFoodToRubles);
        resetBtn.addEventListener('click', resetGame);
        saveNameBtn.addEventListener('click', changePlayerName);
        newNameInput.addEventListener('keypress', (e) => { if(e.key === 'Enter') changePlayerName(); });
        if (tg) tg.ready();
    }
    init();
</script>
</body>
</html>
