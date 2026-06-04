<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Pokémon Card Roller | Rarity Roll</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(145deg, #1a2a3a 0%, #0f1a24 100%);
            font-family: 'Segoe UI', 'Poppins', system-ui, -apple-system, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        /* Main container */
        .app-container {
            max-width: 650px;
            width: 100%;
            background: rgba(30, 40, 55, 0.75);
            backdrop-filter: blur(14px);
            border-radius: 48px;
            box-shadow: 0 25px 45px rgba(0,0,0,0.4), 0 0 0 1px rgba(255,215,0,0.2);
            overflow: hidden;
            transition: all 0.2s;
        }

        /* Header */
        .header {
            background: linear-gradient(105deg, #2c3e4e, #1e2f3c);
            padding: 1rem 1.8rem;
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            flex-wrap: wrap;
            gap: 10px;
            border-bottom: 2px solid #f5b642;
        }

        .title h1 {
            font-size: 1.6rem;
            font-weight: 800;
            background: linear-gradient(135deg, #FFD966, #FFB347);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .user-info {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .username-display {
            color: #FFD966;
            font-weight: bold;
        }

        .logout-btn {
            background: none;
            border: 1px solid #ff8888;
            color: #ff8888;
            padding: 5px 12px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 0.8rem;
        }

        /* Tabs */
        .tabs {
            display: flex;
            background: #1e2f3c;
        }

        .tab-btn {
            flex: 1;
            background: none;
            border: none;
            padding: 12px;
            font-weight: bold;
            color: #aaa;
            cursor: pointer;
            transition: 0.2s;
        }

        .tab-btn.active {
            background: #2c3e4e;
            color: #FFD966;
            border-bottom: 2px solid #FFB347;
        }

        /* Panels */
        .panel {
            padding: 1.8rem;
            display: none;
        }

        .panel.active {
            display: block;
        }

        /* Roller panel */
        .odds-bar {
            background: #0f1a24;
            border-radius: 40px;
            padding: 12px 18px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 8px;
            font-size: 0.75rem;
        }

        .odds-item {
            color: #ddd;
        }

        .odds-value {
            color: #FFD966;
            font-weight: bold;
        }

        .card-display {
            background: #1e2f3c;
            border-radius: 32px;
            padding: 20px;
            text-align: center;
            margin-bottom: 20px;
            transition: 0.2s;
            box-shadow: inset 0 0 15px rgba(0,0,0,0.3), 0 5px 15px rgba(0,0,0,0.2);
        }

        .card-image {
            width: 180px;
            height: 250px;
            object-fit: contain;
            margin: 0 auto;
            background: #2c3e4e;
            border-radius: 20px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }

        .card-name {
            margin-top: 15px;
            font-size: 1.3rem;
            font-weight: bold;
            color: white;
        }

        .card-rarity {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 40px;
            font-size: 0.7rem;
            font-weight: bold;
            margin-top: 8px;
        }

        .roll-btn {
            background: linear-gradient(105deg, #FFB347, #FF8C00);
            border: none;
            width: 100%;
            padding: 14px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 1.1rem;
            color: #1a2a3a;
            cursor: pointer;
            transition: 0.2s;
        }

        .roll-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        /* Admin panel */
        .admin-section {
            margin-bottom: 25px;
            background: #1e2f3c;
            border-radius: 24px;
            padding: 18px;
        }

        .admin-title {
            color: #FFB347;
            margin-bottom: 12px;
        }

        .slider-group {
            margin-bottom: 15px;
        }

        .slider-group label {
            display: block;
            color: #ddd;
            font-size: 0.8rem;
            margin-bottom: 5px;
        }

        input[type="range"] {
            width: 100%;
        }

        .force-select {
            margin-top: 15px;
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }

        .force-btn {
            background: #2c3e4e;
            border: none;
            padding: 6px 12px;
            border-radius: 40px;
            color: white;
            cursor: pointer;
        }

        .force-btn.active {
            background: #FFB347;
            color: #1a2a3a;
        }

        .save-rates {
            background: #FF8C00;
            border: none;
            padding: 8px 15px;
            border-radius: 40px;
            font-weight: bold;
            margin-top: 12px;
            cursor: pointer;
        }

        /* Login panel */
        .login-form, .register-form {
            background: #1e2f3c;
            border-radius: 24px;
            padding: 1.5rem;
            margin-bottom: 20px;
        }

        .login-form input, .register-form input {
            width: 100%;
            padding: 12px;
            margin: 8px 0;
            background: #0f1a24;
            border: 1px solid #2c3e4e;
            border-radius: 30px;
            color: white;
        }

        .login-btn, .register-btn {
            background: #FFB347;
            border: none;
            width: 100%;
            padding: 10px;
            border-radius: 30px;
            font-weight: bold;
            margin-top: 10px;
            cursor: pointer;
        }

        .error-msg {
            color: #ff8888;
            font-size: 0.8rem;
            margin-top: 8px;
        }

        /* Utility */
        .hidden {
            display: none;
        }
    </style>
</head>
<body>

<div class="app-container" id="app">
    <div class="header">
        <div class="title">
            <h1>⚡ Pokémon Card Roller</h1>
        </div>
        <div class="user-info" id="userInfo">
            <span class="username-display" id="usernameSpan">Guest</span>
            <button class="logout-btn" id="logoutBtn" style="display: none;">Logout</button>
        </div>
    </div>

    <!-- Unauthenticated panel (login/register) -->
    <div id="authPanel" class="panel active">
        <div class="login-form">
            <h3 style="color:white;">Login</h3>
            <input type="text" id="loginUsername" placeholder="Username">
            <input type="password" id="loginPassword" placeholder="Password">
            <button class="login-btn" id="doLogin">Login</button>
            <div id="loginError" class="error-msg"></div>
        </div>
        <div class="register-form">
            <h3 style="color:white;">Register</h3>
            <input type="text" id="regUsername" placeholder="Username">
            <input type="password" id="regPassword" placeholder="Password">
            <button class="register-btn" id="doRegister">Create Account</button>
            <div id="regError" class="error-msg"></div>
        </div>
    </div>

    <!-- Main app panel (after login) -->
    <div id="mainPanel" class="panel" style="display: none;">
        <div class="tabs">
            <button class="tab-btn active" data-tab="roller">🎲 ROLLER</button>
            <button class="tab-btn" data-tab="inventory">📦 INVENTORY</button>
            <button class="tab-btn" data-tab="admin" id="adminTabBtn" style="display: none;">⚙️ ADMIN</button>
        </div>

        <!-- Roller tab -->
        <div id="rollerTab" class="panel active">
            <div class="odds-bar" id="oddsDisplay">
                <!-- dynamic odds will be shown here -->
            </div>
            <div class="card-display" id="cardDisplay">
                <img id="cardImage" class="card-image" src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/25.png" alt="Card">
                <div class="card-name" id="cardName">Pikachu</div>
                <div class="card-rarity" id="cardRarity" style="background: #aaa;">Common</div>
            </div>
            <button class="roll-btn" id="rollBtn">🎲 ROLL CARD 🎲</button>
        </div>

        <!-- Inventory tab -->
        <div id="inventoryTab" class="panel" style="display: none;">
            <div id="inventoryList" style="max-height: 400px; overflow-y: auto; display: flex; flex-wrap: wrap; gap: 12px; justify-content: center;">
                <!-- cards will appear here -->
            </div>
        </div>

        <!-- Admin tab -->
        <div id="adminTab" class="panel" style="display: none;">
            <div class="admin-section">
                <h3 class="admin-title">🎮 Drop Rate Control</h3>
                <div class="slider-group">
                    <label>Common (<span id="commonVal">60</span>%)</label>
                    <input type="range" id="commonRate" min="0" max="100" step="1">
                </div>
                <div class="slider-group">
                    <label>Rare (<span id="rareVal">25</span>%)</label>
                    <input type="range" id="rareRate" min="0" max="100" step="1">
                </div>
                <div class="slider-group">
                    <label>Epic (<span id="epicVal">10</span>%)</label>
                    <input type="range" id="epicRate" min="0" max="100" step="1">
                </div>
                <div class="slider-group">
                    <label>Legendary (<span id="legendaryVal">4</span>%)</label>
                    <input type="range" id="legendaryRate" min="0" max="100" step="1">
                </div>
                <div class="slider-group">
                    <label>Mythical (<span id="mythicalVal">1</span>%)</label>
                    <input type="range" id="mythicalRate" min="0" max="100" step="1">
                </div>
                <button class="save-rates" id="saveRatesBtn">💾 Save Rates</button>
            </div>
            <div class="admin-section">
                <h3 class="admin-title">🔮 Force Next Card (Override Rarity)</h3>
                <div class="force-select" id="forceButtons">
                    <button class="force-btn" data-rarity="Common">Common</button>
                    <button class="force-btn" data-rarity="Rare">Rare</button>
                    <button class="force-btn" data-rarity="Epic">Epic</button>
                    <button class="force-btn" data-rarity="Legendary">Legendary</button>
                    <button class="force-btn" data-rarity="Mythical">Mythical</button>
                    <button class="force-btn" data-rarity="None">Clear force</button>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // ---------- Pokémon card database (real cards with images) ----------
    const cardsDB = {
        Common: [
            { name: "Bulbasaur", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/1.png" },
            { name: "Charmander", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/4.png" },
            { name: "Squirtle", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/7.png" },
            { name: "Pikachu", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/25.png" },
            { name: "Jigglypuff", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/39.png" },
            { name: "Psyduck", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/54.png" },
            { name: "Growlithe", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/58.png" },
            { name: "Machop", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/66.png" }
        ],
        Rare: [
            { name: "Haunter", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/93.png" },
            { name: "Eevee", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/133.png" },
            { name: "Snorlax", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/143.png" },
            { name: "Dragonair", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/148.png" },
            { name: "Marill", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/183.png" },
            { name: "Lapras", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/131.png" }
        ],
        Epic: [
            { name: "Gengar", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/94.png" },
            { name: "Arcanine", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/59.png" },
            { name: "Vaporeon", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/134.png" },
            { name: "Jolteon", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/135.png" },
            { name: "Flareon", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/136.png" },
            { name: "Tyranitar", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/248.png" }
        ],
        Legendary: [
            { name: "Mewtwo", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/150.png" },
            { name: "Lugia", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/249.png" },
            { name: "Ho-Oh", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/250.png" },
            { name: "Rayquaza", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/384.png" },
            { name: "Dialga", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/483.png" },
            { name: "Palkia", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/484.png" }
        ],
        Mythical: [
            { name: "Mew", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/151.png" },
            { name: "Celebi", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/251.png" },
            { name: "Jirachi", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/385.png" },
            { name: "Darkrai", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/491.png" },
            { name: "Shaymin", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/492.png" },
            { name: "Arceus", img: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/493.png" }
        ]
    };

    // Rarity color mapping
    const rarityColors = {
        Common: "#6c757d",
        Rare: "#007bff",
        Epic: "#9b59b6",
        Legendary: "#f39c12",
        Mythical: "#e84393"
    };

    // ---------- User management (localStorage) ----------
    let currentUser = null;
    let users = JSON.parse(localStorage.getItem("pokemon_users")) || {};
    let forcedNextRarity = null;

    // Default drop rates (sum to 100)
    let dropRates = {
        Common: 60,
        Rare: 25,
        Epic: 10,
        Legendary: 4,
        Mythical: 1
    };

    // Admin list (hardcoded admin usernames)
    const adminList = ["admin", "owner", "damon"];

    // Save user data (inventory)
    function saveUserData() {
        if (currentUser) {
            users[currentUser.username].inventory = currentUser.inventory;
            users[currentUser.username].dropRates = dropRates;
            localStorage.setItem("pokemon_users", JSON.stringify(users));
        }
    }

    function loadUserData() {
        if (!currentUser) return;
        const stored = users[currentUser.username];
        if (stored) {
            currentUser.inventory = stored.inventory || [];
            if (stored.dropRates) dropRates = stored.dropRates;
        } else {
            currentUser.inventory = [];
        }
        // Update odds display
        updateOddsDisplay();
        if (currentUser.isAdmin) document.getElementById("adminTabBtn").style.display = "block";
        else document.getElementById("adminTabBtn").style.display = "none";
        updateInventoryUI();
    }

    // Odds display
    function updateOddsDisplay() {
        const oddsDiv = document.getElementById("oddsDisplay");
        oddsDiv.innerHTML = `
            <span class="odds-item">⚪ Common: <span class="odds-value">${dropRates.Common}%</span></span>
            <span class="odds-item">🔵 Rare: <span class="odds-value">${dropRates.Rare}%</span></span>
            <span class="odds-item">🟣 Epic: <span class="odds-value">${dropRates.Epic}%</span></span>
            <span class="odds-item">🟠 Legendary: <span class="odds-value">${dropRates.Legendary}%</span></span>
            <span class="odds-item">💖 Mythical: <span class="odds-value">${dropRates.Mythical}%</span></span>
        `;
        // Update admin sliders if visible
        const sliders = ["commonRate", "rareRate", "epicRate", "legendaryRate", "mythicalRate"];
        if (document.getElementById("commonRate")) {
            document.getElementById("commonRate").value = dropRates.Common;
            document.getElementById("commonVal").innerText = dropRates.Common;
            document.getElementById("rareRate").value = dropRates.Rare;
            document.getElementById("rareVal").innerText = dropRates.Rare;
            document.getElementById("epicRate").value = dropRates.Epic;
            document.getElementById("epicVal").innerText = dropRates.Epic;
            document.getElementById("legendaryRate").value = dropRates.Legendary;
            document.getElementById("legendaryVal").innerText = dropRates.Legendary;
            document.getElementById("mythicalRate").value = dropRates.Mythical;
            document.getElementById("mythicalVal").innerText = dropRates.Mythical;
        }
    }

    // Roll logic
    function getRandomCard() {
        // Check forced next rarity (admin override)
        if (forcedNextRarity && forcedNextRarity !== "None") {
            const rarity = forcedNextRarity;
            forcedNextRarity = null;
            const pool = cardsDB[rarity];
            if (pool && pool.length) {
                const card = pool[Math.floor(Math.random() * pool.length)];
                return { rarity, card };
            }
        }
        // Weighted random based on dropRates
        const total = dropRates.Common + dropRates.Rare + dropRates.Epic + dropRates.Legendary + dropRates.Mythical;
        let rand = Math.random() * total;
        if (rand < dropRates.Common) return { rarity: "Common", card: cardsDB.Common[Math.floor(Math.random() * cardsDB.Common.length)] };
        rand -= dropRates.Common;
        if (rand < dropRates.Rare) return { rarity: "Rare", card: cardsDB.Rare[Math.floor(Math.random() * cardsDB.Rare.length)] };
        rand -= dropRates.Rare;
        if (rand < dropRates.Epic) return { rarity: "Epic", card: cardsDB.Epic[Math.floor(Math.random() * cardsDB.Epic.length)] };
        rand -= dropRates.Epic;
        if (rand < dropRates.Legendary) return { rarity: "Legendary", card: cardsDB.Legendary[Math.floor(Math.random() * cardsDB.Legendary.length)] };
        return { rarity: "Mythical", card: cardsDB.Mythical[Math.floor(Math.random() * cardsDB.Mythical.length)] };
    }

    async function rollCard() {
        const btn = document.getElementById("rollBtn");
        btn.disabled = true;
        btn.innerText = "🌀 ROLLING...";
        // Simulate animation delay
        await new Promise(r => setTimeout(r, 600));
        const { rarity, card } = getRandomCard();
        // Update display
        document.getElementById("cardName").innerText = card.name;
        document.getElementById("cardImage").src = card.img;
        const raritySpan = document.getElementById("cardRarity");
        raritySpan.innerText = rarity;
        raritySpan.style.background = rarityColors[rarity];
        // Add to inventory
        currentUser.inventory.push({ name: card.name, img: card.img, rarity, timestamp: Date.now() });
        saveUserData();
        updateInventoryUI();
        btn.disabled = false;
        btn.innerText = "🎲 ROLL CARD 🎲";
    }

    // Inventory UI
    function updateInventoryUI() {
        const container = document.getElementById("inventoryList");
        if (!container) return;
        container.innerHTML = "";
        const inv = currentUser.inventory.slice().reverse();
        inv.forEach(item => {
            const cardDiv = document.createElement("div");
            cardDiv.style.background = "#1e2f3c";
            cardDiv.style.borderRadius = "16px";
            cardDiv.style.padding = "8px";
            cardDiv.style.textAlign = "center";
            cardDiv.style.width = "100px";
            const img = document.createElement("img");
            img.src = item.img;
            img.style.width = "80px";
            img.style.height = "110px";
            img.style.objectFit = "contain";
            const nameSpan = document.createElement("div");
            nameSpan.innerText = item.name;
            nameSpan.style.color = "white";
            nameSpan.style.fontSize = "0.7rem";
            const raritySpan = document.createElement("div");
            raritySpan.innerText = item.rarity;
            raritySpan.style.fontSize = "0.6rem";
            raritySpan.style.color = "#FFD966";
            cardDiv.appendChild(img);
            cardDiv.appendChild(nameSpan);
            cardDiv.appendChild(raritySpan);
            container.appendChild(cardDiv);
        });
    }

    // Admin: save rates
    function saveDropRates() {
        dropRates.Common = parseInt(document.getElementById("commonRate").value);
        dropRates.Rare = parseInt(document.getElementById("rareRate").value);
        dropRates.Epic = parseInt(document.getElementById("epicRate").value);
        dropRates.Legendary = parseInt(document.getElementById("legendaryRate").value);
        dropRates.Mythical = parseInt(document.getElementById("mythicalRate").value);
        // Normalize to sum 100 (optional but keep)
        let sum = dropRates.Common + dropRates.Rare + dropRates.Epic + dropRates.Legendary + dropRates.Mythical;
        if (sum !== 100) {
            // Adjust mythical to fix sum
            dropRates.Mythical += (100 - sum);
            if (dropRates.Mythical < 0) dropRates.Mythical = 0;
            document.getElementById("mythicalRate").value = dropRates.Mythical;
            document.getElementById("mythicalVal").innerText = dropRates.Mythical;
        }
        updateOddsDisplay();
        saveUserData();
        alert("Drop rates saved!");
    }

    // Admin force next rarity
    function setupForceButtons() {
        const btns = document.querySelectorAll(".force-btn");
        btns.forEach(btn => {
            btn.addEventListener("click", () => {
                const rarity = btn.getAttribute("data-rarity");
                if (rarity === "None") forcedNextRarity = null;
                else forcedNextRarity = rarity;
                // visual active
                btns.forEach(b => b.classList.remove("active"));
                if (rarity !== "None") btn.classList.add("active");
                alert(`Next card forced to ${rarity || "random"}`);
            });
        });
    }

    // Login/Register
    function login() {
        const username = document.getElementById("loginUsername").value.trim();
        const password = document.getElementById("loginPassword").value;
        if (!username || !password) { document.getElementById("loginError").innerText = "Fill all fields"; return; }
        const userData = users[username];
        if (!userData || userData.password !== password) {
            document.getElementById("loginError").innerText = "Invalid credentials";
            return;
        }
        currentUser = {
            username: username,
            password: userData.password,
            inventory: userData.inventory || [],
            isAdmin: adminList.includes(username.toLowerCase())
        };
        loadUserData();
        showMainApp();
    }

    function register() {
        const username = document.getElementById("regUsername").value.trim();
        const password = document.getElementById("regPassword").value;
        if (!username || !password) { document.getElementById("regError").innerText = "Fill all fields"; return; }
        if (users[username]) { document.getElementById("regError").innerText = "Username taken"; return; }
        users[username] = { password: password, inventory: [], dropRates: { Common:60, Rare:25, Epic:10, Legendary:4, Mythical:1 } };
        localStorage.setItem("pokemon_users", JSON.stringify(users));
        document.getElementById("regError").innerText = "Account created! Please login.";
    }

    function logout() {
        currentUser = null;
        forcedNextRarity = null;
        document.getElementById("authPanel").style.display = "block";
        document.getElementById("mainPanel").style.display = "none";
        document.getElementById("usernameSpan").innerText = "Guest";
        document.getElementById("logoutBtn").style.display = "none";
        // clear inputs
        document.getElementById("loginUsername").value = "";
        document.getElementById("loginPassword").value = "";
        document.getElementById("regUsername").value = "";
        document.getElementById("regPassword").value = "";
    }

    function showMainApp() {
        document.getElementById("authPanel").style.display = "none";
        document.getElementById("mainPanel").style.display = "block";
        document.getElementById("usernameSpan").innerText = currentUser.username;
        document.getElementById("logoutBtn").style.display = "block";
        updateInventoryUI();
        updateOddsDisplay();
        // default first card
        const firstCard = cardsDB.Common[0];
        document.getElementById("cardName").innerText = firstCard.name;
        document.getElementById("cardImage").src = firstCard.img;
        document.getElementById("cardRarity").innerText = "Common";
        document.getElementById("cardRarity").style.background = rarityColors.Common;
    }

    // Tab switching
    function initTabs() {
        const tabs = document.querySelectorAll(".tab-btn");
        tabs.forEach(btn => {
            btn.addEventListener("click", () => {
                const tabName = btn.getAttribute("data-tab");
                document.querySelectorAll(".panel").forEach(panel => panel.classList.remove("active"));
                if (tabName === "roller") document.getElementById("rollerTab").classList.add("active");
                else if (tabName === "inventory") document.getElementById("inventoryTab").classList.add("active");
                else if (tabName === "admin") document.getElementById("adminTab").classList.add("active");
                tabs.forEach(b => b.classList.remove("active"));
                btn.classList.add("active");
            });
        });
    }

    // Event binding
    document.getElementById("doLogin").addEventListener("click", login);
    document.getElementById("doRegister").addEventListener("click", register);
    document.getElementById("logoutBtn").addEventListener("click", logout);
    document.getElementById("rollBtn").addEventListener("click", rollCard);
    if (document.getElementById("saveRatesBtn")) {
        document.getElementById("saveRatesBtn").addEventListener("click", saveDropRates);
        setupForceButtons();
    }
    initTabs();

    // If already logged in? check localStorage for last session? not needed.
    // Start with auth panel visible.
</script>
</body>
</html>
