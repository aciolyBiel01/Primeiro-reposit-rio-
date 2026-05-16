<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Monster Tracker - Sistema de Captura</title>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', 'Poppins', 'Inter', sans-serif;
            background: linear-gradient(135deg, #0a0e1a 0%, #0f1525 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        /* Background com grid e glow */
        body::before {
            content: '';
            position: fixed;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(rgba(0, 255, 255, 0.05) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 255, 255, 0.05) 1px, transparent 1px);
            background-size: 40px 40px;
            pointer-events: none;
        }

        /* Container Principal */
        .container {
            max-width: 1400px;
            width: 100%;
            display: flex;
            gap: 30px;
            flex-wrap: wrap;
            position: relative;
            z-index: 1;
        }

        /* Painel de Controle */
        .control-panel {
            flex: 1;
            min-width: 320px;
            background: rgba(20, 28, 40, 0.85);
            backdrop-filter: blur(20px);
            border-radius: 32px;
            padding: 32px 28px;
            border: 1px solid rgba(0, 255, 255, 0.2);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4), 0 0 20px rgba(0, 255, 255, 0.1);
            transition: all 0.3s ease;
        }

        .control-panel:hover {
            border-color: rgba(0, 255, 255, 0.4);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5), 0 0 30px rgba(0, 255, 255, 0.2);
        }

        /* Título */
        .title {
            font-size: 28px;
            font-weight: 700;
            background: linear-gradient(135deg, #00ffff, #8a2be2);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 32px;
            text-align: center;
            letter-spacing: -0.5px;
        }

        /* Barra de Pesquisa */
        .search-wrapper {
            margin-bottom: 24px;
        }

        .search-box {
            width: 100%;
            padding: 16px 20px;
            background: rgba(10, 14, 26, 0.9);
            border: 2px solid rgba(0, 255, 255, 0.3);
            border-radius: 60px;
            color: #fff;
            font-size: 16px;
            transition: all 0.3s ease;
            outline: none;
        }

        .search-box:focus {
            border-color: #00ffff;
            box-shadow: 0 0 15px rgba(0, 255, 255, 0.3);
        }

        /* Botão Enter */
        .enter-btn {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, #00ffff, #0088ff);
            border: none;
            border-radius: 60px;
            color: #0a0e1a;
            font-weight: bold;
            font-size: 18px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-bottom: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .enter-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0, 255, 255, 0.3);
            filter: brightness(1.05);
        }

        /* Slider Container */
        .slider-container {
            margin-bottom: 28px;
        }

        .slider-label {
            display: flex;
            justify-content: space-between;
            color: #00ffff;
            margin-bottom: 12px;
            font-weight: 500;
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            -webkit-appearance: none;
            background: linear-gradient(90deg, #00ffff, #8a2be2);
            border-radius: 5px;
            outline: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            background: #00ffff;
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 0 10px #00ffff;
            transition: 0.2s;
        }

        /* Toggle Switches */
        .toggle-container {
            margin-bottom: 28px;
        }

        .toggle-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 16px 0;
            border-bottom: 1px solid rgba(0, 255, 255, 0.1);
        }

        .toggle-info {
            display: flex;
            align-items: center;
            gap: 12px;
            color: #e0e0e0;
        }

        .toggle-info i {
            font-size: 20px;
            color: #00ffff;
        }

        /* Switch personalizado */
        .switch {
            position: relative;
            display: inline-block;
            width: 52px;
            height: 28px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider-round {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #2a2f3f;
            transition: 0.3s;
            border-radius: 28px;
        }

        .slider-round:before {
            position: absolute;
            content: "";
            height: 22px;
            width: 22px;
            left: 3px;
            bottom: 3px;
            background-color: white;
            transition: 0.3s;
            border-radius: 50%;
        }

        input:checked + .slider-round {
            background: linear-gradient(135deg, #00ffff, #0088ff);
        }

        input:checked + .slider-round:before {
            transform: translateX(24px);
        }

        /* Ambiente de Teste */
        .test-environment {
            flex: 2;
            min-width: 500px;
            background: rgba(20, 28, 40, 0.85);
            backdrop-filter: blur(20px);
            border-radius: 32px;
            padding: 24px;
            border: 1px solid rgba(0, 255, 255, 0.2);
            transition: all 0.3s ease;
        }

        /* Canvas do Mapa */
        .map-canvas {
            background: radial-gradient(circle at 20% 30%, #1a2340, #0a0e1a);
            border-radius: 24px;
            position: relative;
            height: 500px;
            border: 2px solid rgba(0, 255, 255, 0.2);
            overflow: hidden;
            margin-bottom: 20px;
        }

        #gameCanvas {
            width: 100%;
            height: 100%;
            display: block;
        }

        /* Status e Logs */
        .info-panel {
            background: rgba(0, 0, 0, 0.5);
            border-radius: 20px;
            padding: 16px;
            margin-top: 16px;
        }

        .status {
            display: flex;
            justify-content: space-between;
            margin-bottom: 12px;
            color: #00ffff;
            font-size: 14px;
        }

        .log-area {
            background: rgba(0, 0, 0, 0.6);
            border-radius: 12px;
            padding: 12px;
            height: 100px;
            overflow-y: auto;
            font-size: 12px;
            font-family: monospace;
            color: #88ffcc;
        }

        /* Responsividade */
        @media (max-width: 900px) {
            .container {
                flex-direction: column;
            }
            .test-environment {
                min-width: auto;
            }
        }

        /* Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #1a1f2e;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #00ffff;
            border-radius: 10px;
        }
    </style>
</head>
<body>
<div class="container">
    <!-- Painel de Controle -->
    <div class="control-panel">
        <div class="title">
            <i class="fas fa-dragon"></i> MONSTER TRACKER
        </div>

        <!-- Barra de Pesquisa -->
        <div class="search-wrapper">
            <input type="text" id="searchInput" class="search-box" placeholder="🔍 Digite o nome da criatura...">
        </div>
        <button id="enterBtn" class="enter-btn">
            <i class="fas fa-arrow-right"></i> ENTER
        </button>

        <!-- Slider de Velocidade -->
        <div class="slider-container">
            <div class="slider-label">
                <span><i class="fas fa-tachometer-alt"></i> Meeting Speed</span>
                <span id="speedValue">3.0s</span>
            </div>
            <input type="range" id="speedSlider" min="3" max="15" step="0.5" value="8">
        </div>

        <!-- Auto Battle -->
        <div class="toggle-container">
            <div class="toggle-item">
                <div class="toggle-info">
                    <i class="fas fa-robot"></i>
                    <span>Auto Battle</span>
                </div>
                <label class="switch">
                    <input type="checkbox" id="autoBattleToggle">
                    <span class="slider-round"></span>
                </label>
            </div>

            <!-- Defense Mode -->
            <div class="toggle-item">
                <div class="toggle-info">
                    <i class="fas fa-shield-alt"></i>
                    <span>Defense Mode (Dano reduzido a 0)</span>
                </div>
                <label class="switch">
                    <input type="checkbox" id="defenseToggle">
                    <span class="slider-round"></span>
                </label>
            </div>
        </div>
    </div>

    <!-- Ambiente de Teste Offline -->
    <div class="test-environment">
        <div class="map-canvas">
            <canvas id="gameCanvas" width="800" height="500"></canvas>
        </div>
        <div class="info-panel">
            <div class="status">
                <span><i class="fas fa-map-marker-alt"></i> Posição: <span id="posX">0</span>, <span id="posY">0</span></span>
                <span><i class="fas fa-trophy"></i> EXP: <span id="expTotal">0</span> | Moedas: <span id="coinTotal">0</span></span>
                <span><i class="fas fa-hourglass-half"></i> Status: <span id="battleStatus">Ocioso</span></span>
            </div>
            <div class="log-area" id="logArea">
                [Sistema] Bem-vindo, Treinador!<br>
                [Sistema] Modo offline ativado.
            </div>
        </div>
    </div>
</div>

<script>
    // ==================== CONFIGURAÇÕES DO AMBIENTE ====================
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    
    // Lista de criaturas com coordenadas fixas no mapa
    const creatures = [
        { name: "Pikachu", x: 150, y: 120, icon: "⚡" },
        { name: "Charizard", x: 600, y: 80, icon: "🔥" },
        { name: "Squirtle", x: 400, y: 380, icon: "💧" },
        { name: "Bulbasaur", x: 680, y: 400, icon: "🌿" },
        { name: "Mewtwo", x: 50, y: 420, icon: "🧬" },
        { name: "Gengar", x: 300, y: 200, icon: "👻" }
    ];
    
    // Personagem do jogador
    let player = { x: 400, y: 250, targetX: null, targetY: null, isMoving: false, moveStartTime: 0, moveDuration: 0 };
    
    // Sistema de batalha
    let isInBattle = false;
    let battleEndTime = 0;
    let expTotal = 0;
    let coinTotal = 0;
    
    // Estados dos modos
    let autoBattleEnabled = false;
    let defenseModeEnabled = false;
    let autoBattleInterval = null;
    
    // Velocidade (tempo de encontro em segundos)
    let meetingSpeed = 8; // segundos
    
    // Elementos DOM
    const searchInput = document.getElementById('searchInput');
    const enterBtn = document.getElementById('enterBtn');
    const speedSlider = document.getElementById('speedSlider');
    const speedValue = document.getElementById('speedValue');
    const autoBattleToggle = document.getElementById('autoBattleToggle');
    const defenseToggle = document.getElementById('defenseToggle');
    const posXSpan = document.getElementById('posX');
    const posYSpan = document.getElementById('posY');
    const expTotalSpan = document.getElementById('expTotal');
    const coinTotalSpan = document.getElementById('coinTotal');
    const battleStatusSpan = document.getElementById('battleStatus');
    const logArea = document.getElementById('logArea');
    
    // Função para adicionar log
    function addLog(message) {
        const logEntry = document.createElement('div');
        logEntry.innerHTML = `[${new Date().toLocaleTimeString()}] ${message}`;
        logArea.appendChild(logEntry);
        logArea.scrollTop = logArea.scrollHeight;
        // Limitar logs
        while (logArea.children.length > 50) logArea.removeChild(logArea.firstChild);
    }
    
    // Atualizar UI de posição
    function updateUI() {
        posXSpan.textContent = Math.floor(player.x);
        posYSpan.textContent = Math.floor(player.y);
        expTotalSpan.textContent = expTotal;
        coinTotalSpan.textContent = coinTotal;
    }
    
    // Teleportar para criatura
    function teleportToCreature(creatureName) {
        const creature = creatures.find(c => c.name.toLowerCase() === creatureName.toLowerCase());
        if (!creature) {
            addLog(`❌ Criatura "${creatureName}" não encontrada!`);
            return false;
        }
        
        // Interrompe movimento atual
        player.isMoving = false;
        player.targetX = null;
        player.targetY = null;
        
        // Teleporta instantaneamente
        player.x = creature.x;
        player.y = creature.y;
        addLog(`✨ Teleportado para ${creature.name} na posição (${creature.x}, ${creature.y})`);
        updateUI();
        draw();
        return true;
    }
    
    // Função para iniciar batalha (simulada)
    function startBattle(creature) {
        if (isInBattle) {
            addLog(`⚠️ Já em batalha! Aguarde o término.`);
            return false;
        }
        
        isInBattle = true;
        battleStatusSpan.textContent = "Em Batalha";
        addLog(`⚔️ BATALHA INICIADA contra ${creature.name}!`);
        
        // Simula duração da batalha (2 segundos base + aleatório)
        const battleDuration = 2000 + Math.random() * 1000;
        
        setTimeout(() => {
            // Concede recompensas simuladas
            const expGain = Math.floor(Math.random() * 50) + 30;
            const coinGain = Math.floor(Math.random() * 40) + 20;
            
            // Defense Mode: redução total de dano (apenas visual, mas aplicamos bônus)
            if (defenseModeEnabled) {
                addLog(`🛡️ Defense Mode ativo! Você não sofreu dano!`);
            }
            
            expTotal += expGain;
            coinTotal += coinGain;
            updateUI();
            
            addLog(`🏆 VITÓRIA! +${expGain} EXP e +${coinGain} moedas!`);
            battleStatusSpan.textContent = "Ocioso";
            isInBattle = false;
            
            // Se auto battle estiver ativo e não houver batalha pendente, procurar próxima
            if (autoBattleEnabled && !isInBattle) {
                setTimeout(() => findNearestBattle(), 500);
            }
            
            draw();
        }, battleDuration);
        
        draw();
        return true;
    }
    
    // Verificar colisão com criaturas (encontro)
    function checkCreatureEncounter() {
        if (isInBattle) return;
        
        for (let creature of creatures) {
            const dx = player.x - creature.x;
            const dy = player.y - creature.y;
            const distance = Math.sqrt(dx*dx + dy*dy);
            
            if (distance < 30) { // Raio de encontro
                addLog(`🔍 Você encontrou ${creature.name}!`);
                startBattle(creature);
                break;
            }
        }
    }
    
    // Movimentação suave do personagem
    function updateMovement(currentTime) {
        if (!player.isMoving || !player.targetX || !player.targetY) return false;
        
        const elapsed = (currentTime - player.moveStartTime) / 1000;
        let t = Math.min(1, elapsed / player.moveDuration);
        
        // Easing suave
        const easeInOut = t < 0.5 ? 2 * t * t : 1 - Math.pow(-2 * t + 2, 2) / 2;
        
        player.x = player.startX + (player.targetX - player.startX) * easeInOut;
        player.y = player.startY + (player.targetY - player.startY) * easeInOut;
        
        if (t >= 1) {
            player.x = player.targetX;
            player.y = player.targetY;
            player.isMoving = false;
            player.targetX = null;
            player.targetY = null;
            updateUI();
            checkCreatureEncounter(); // Ao final do movimento, verifica encontro
            return true;
        }
        updateUI();
        return false;
    }
    
    // Mover para coordenada com animação
    function moveTo(x, y) {
        if (isInBattle) {
            addLog(`❌ Não pode se mover durante a batalha!`);
            return false;
        }
        
        player.startX = player.x;
        player.startY = player.y;
        player.targetX = x;
        player.targetY = y;
        player.moveDuration = meetingSpeed;
        player.moveStartTime = performance.now() / 1000;
        player.isMoving = true;
        addLog(`🚶 Movendo para (${Math.floor(x)}, ${Math.floor(y)}) - Velocidade: ${meetingSpeed}s`);
        return true;
    }
    
    // Encontrar a batalha mais próxima (criatura mais perto)
    function findNearestBattle() {
        if (isInBattle) {
            addLog(`⏳ Aguardando fim da batalha para buscar próximo alvo...`);
            return;
        }
        
        let nearest = null;
        let minDist = Infinity;
        
        for (let creature of creatures) {
            const dx = player.x - creature.x;
            const dy = player.y - creature.y;
            const dist = Math.sqrt(dx*dx + dy*dy);
            if (dist < minDist) {
                minDist = dist;
                nearest = creature;
            }
        }
        
        if (nearest) {
            addLog(`🤖 Auto Battle: Indo para ${nearest.name} a ${Math.floor(minDist)}px de distância`);
            moveTo(nearest.x, nearest.y);
        } else {
            addLog(`⚠️ Nenhuma criatura encontrada no mapa!`);
        }
    }
    
    // Loop de animação e auto battle contínuo
    let animationId = null;
    let lastTimestamp = 0;
    
    function animate(timestamp) {
        if (!animationId) return;
        
        const now = timestamp / 1000;
        if (player.isMoving) {
            updateMovement(now);
            draw();
        }
        
        // Auto Battle: a cada frame, se ativo e sem batalha e sem movimento, procura nova batalha
        if (autoBattleEnabled && !isInBattle && !player.isMoving) {
            // Evita spam de chamadas
            if (!window._autoBattleCooldown) {
                window._autoBattleCooldown = true;
                findNearestBattle();
                setTimeout(() => { window._autoBattleCooldown = false; }, 1000);
            }
        }
        
        draw();
        requestAnimationFrame(animate);
    }
    
    // Desenhar o ambiente com estilo neon
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        // Grid futurista
        ctx.strokeStyle = 'rgba(0, 255, 255, 0.15)';
        ctx.lineWidth = 1;
        for (let i = 0; i < canvas.width; i += 40) {
            ctx.beginPath();
            ctx.moveTo(i, 0);
            ctx.lineTo(i, canvas.height);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(0, i);
            ctx.lineTo(canvas.width, i);
            ctx.stroke();
        }
        
        // Desenhar Criaturas
        creatures.forEach(c => {
            // Glow effect
            ctx.shadowBlur = 15;
            ctx.shadowColor = '#00ffff';
            ctx.font = 'bold 32px "Segoe UI"';
            ctx.fillStyle = '#ffffff';
            ctx.fillText(c.icon, c.x - 15, c.y + 10);
            ctx.font = '12px monospace';
            ctx.fillStyle = '#00ffff';
            ctx.fillText(c.name, c.x - 25, c.y - 10);
            
            // Círculo ao redor
            ctx.shadowBlur = 5;
            ctx.strokeStyle = 'rgba(0, 255, 255, 0.5)';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.arc(c.x, c.y, 22, 0, Math.PI * 2);
            ctx.stroke();
        });
        
        // Desenhar Jogador
        ctx.shadowBlur = 20;
        ctx.shadowColor = '#ff00ff';
        ctx.font = '36px "Segoe UI"';
        ctx.fillStyle = '#ffcc44';
        ctx.fillText('🧙', player.x - 18, player.y + 12);
        
        // Indicador de batalha
        if (isInBattle) {
            ctx.font = 'bold 20px monospace';
            ctx.fillStyle = '#ff4444';
            ctx.shadowBlur = 10;
            ctx.fillText('⚔️ BATTLE ⚔️', canvas.width/2 - 80, 40);
        }
        
        // Resetar shadow
        ctx.shadowBlur = 0;
    }
    
    // Inicializar eventos
    function init() {
        // Botão Enter / Pesquisa
        enterBtn.addEventListener('click', () => {
            const query = searchInput.value.trim();
            if (query) {
                teleportToCreature(query);
            } else {
                addLog(`⚠️ Digite o nome de uma criatura!`);
            }
        });
        
        searchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') enterBtn.click();
        });
        
        // Slider de velocidade
        speedSlider.addEventListener('input', (e) => {
            meetingSpeed = parseFloat(e.target.value);
            speedValue.textContent = meetingSpeed.toFixed(1) + 's';
            addLog(`⚙️ Velocidade de encontro alterada para ${meetingSpeed} segundos`);
        });
        
        // Auto Battle Toggle
        autoBattleToggle.addEventListener('change', (e) => {
            autoBattleEnabled = e.target.checked;
            if (autoBattleEnabled && !isInBattle && !player.isMoving) {
                findNearestBattle();
            }
            addLog(`${autoBattleEnabled ? '✅ Auto Battle ATIVADO' : '❌ Auto Battle DESATIVADO'}`);
        });
        
        // Defense Mode Toggle
        defenseToggle.addEventListener('change', (e) => {
            defenseModeEnabled = e.target.checked;
            addLog(`🛡️ Defense Mode ${defenseModeEnabled ? 'ATIVADO - Dano reduzido a 0' : 'DESATIVADO'}`);
        });
        
        // Iniciar animação
        animationId = true;
        draw();
        requestAnimationFrame(animate);
        
        addLog(`🎮 Sistema inicializado. Use a busca para teleportar até as criaturas!`);
        addLog(`📋 Criaturas disponíveis: ${creatures.map(c => c.name).join(', ')}`);
    }
    
    init();
</script>
</body>
</html>
