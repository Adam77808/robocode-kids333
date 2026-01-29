<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🌊 RoboCode Kids 3 - Aventure Océan</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root { --ocean-dark: #0c2d48; --ocean-mid: #145da0; --ocean-light: #2e8bc0; --sand: #f5d78e; --coral: #ff6b6b; --seaweed: #2ecc71; --fish-orange: #ff9f43; --fish-purple: #a06cd5; --white: #ffffff; }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Fredoka', sans-serif; background: linear-gradient(180deg, var(--ocean-light) 0%, var(--ocean-mid) 50%, var(--ocean-dark) 100%); min-height: 100vh; color: var(--white); }
        .bubbles { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 0; pointer-events: none; }
        .bubble { position: absolute; bottom: -50px; background: rgba(255,255,255,0.3); border-radius: 50%; animation: rise 8s infinite ease-in; }
        @keyframes rise { 0% { transform: translateY(0); opacity: 0.6; } 100% { transform: translateY(-100vh); opacity: 0; } }
        header { background: rgba(12,45,72,0.95); padding: 1rem 2rem; position: relative; z-index: 100; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 1rem; border-bottom: 3px solid var(--sand); }
        .logo { display: flex; align-items: center; gap: 0.75rem; }
        .logo-icon { width: 55px; height: 55px; background: linear-gradient(135deg, var(--ocean-light), var(--seaweed)); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.6rem; animation: float 3s infinite ease-in-out; }
        @keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
        .logo h1 { font-size: 1.4rem; color: var(--sand); }
        .logo h1 span { font-size: 0.8rem; display: block; color: var(--ocean-light); }
        .score-badge { background: linear-gradient(135deg, var(--sand), var(--fish-orange)); padding: 0.5rem 1.2rem; border-radius: 50px; font-weight: 600; color: var(--ocean-dark); }
        .container { position: relative; z-index: 10; max-width: 1300px; margin: 0 auto; padding: 1.5rem; }
        .main-menu { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 1.2rem; }
        .menu-card { background: rgba(255,255,255,0.1); border: 2px solid rgba(255,255,255,0.15); border-radius: 20px; padding: 1.5rem; cursor: pointer; transition: all 0.3s; position: relative; overflow: hidden; }
        .menu-card::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 4px; }
        .menu-card:hover { transform: translateY(-6px); border-color: rgba(255,255,255,0.3); }
        .menu-card.variables::before { background: linear-gradient(90deg, var(--fish-orange), var(--sand)); }
        .menu-card.functions::before { background: linear-gradient(90deg, var(--fish-purple), var(--coral)); }
        .menu-card.debug::before { background: linear-gradient(90deg, var(--coral), var(--fish-orange)); }
        .menu-card.creator::before { background: linear-gradient(90deg, var(--seaweed), var(--ocean-light)); }
        .menu-card.blocks::before { background: linear-gradient(90deg, var(--ocean-light), var(--fish-purple)); }
        .menu-card.sensors::before { background: linear-gradient(90deg, var(--sand), var(--seaweed)); }
        .card-icon { width: 60px; height: 60px; border-radius: 15px; display: flex; align-items: center; justify-content: center; font-size: 2rem; margin-bottom: 0.8rem; background: rgba(255,255,255,0.1); }
        .menu-card h2 { font-size: 1.2rem; margin-bottom: 0.4rem; }
        .menu-card p { color: rgba(255,255,255,0.7); font-size: 0.85rem; }
        .difficulty { display: inline-flex; gap: 0.2rem; margin-top: 0.6rem; font-size: 0.75rem; }
        .star-icon { color: var(--sand); }
        .star-icon.empty { color: rgba(255,255,255,0.3); }
        .game-screen { display: none; background: rgba(255,255,255,0.08); border: 2px solid rgba(255,255,255,0.1); border-radius: 20px; padding: 1.5rem; }
        .game-screen.active { display: block; }
        .game-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1.2rem; flex-wrap: wrap; gap: 0.8rem; }
        .game-title { display: flex; align-items: center; gap: 0.8rem; }
        .game-title h2 { font-size: 1.4rem; }
        .back-btn { background: rgba(255,255,255,0.1); border: 2px solid rgba(255,255,255,0.2); padding: 0.5rem 1rem; border-radius: 50px; cursor: pointer; font-family: 'Fredoka', sans-serif; font-size: 0.9rem; color: var(--white); }
        .back-btn:hover { background: rgba(255,255,255,0.2); }
        .level-info { background: rgba(255,255,255,0.1); padding: 0.4rem 0.8rem; border-radius: 8px; font-size: 0.85rem; }
        .action-btn { padding: 0.6rem 1.2rem; border: none; border-radius: 10px; font-family: 'Fredoka', sans-serif; font-size: 0.95rem; cursor: pointer; transition: all 0.2s; }
        .action-btn:hover { transform: scale(1.05); }
        .action-btn.primary { background: linear-gradient(135deg, var(--seaweed), var(--ocean-light)); color: white; }
        .action-btn.secondary { background: rgba(255,255,255,0.1); color: white; border: 2px solid rgba(255,255,255,0.2); }
        .var-grid { display: grid; gap: 3px; }
        .var-cell { aspect-ratio: 1; border-radius: 5px; display: flex; align-items: center; justify-content: center; font-size: 1.2rem; background: rgba(255,255,255,0.05); transition: all 0.3s; }
        .var-cell.sub { background: var(--ocean-light); }
        .var-mission { background: rgba(245,215,142,0.1); border: 2px solid rgba(245,215,142,0.3); border-radius: 10px; padding: 0.8rem; margin-bottom: 0.8rem; }
        .var-mission h4 { color: var(--sand); margin-bottom: 0.3rem; font-size: 0.95rem; }
        .var-display { background: rgba(0,0,0,0.3); border-radius: 10px; padding: 0.8rem; margin-bottom: 0.8rem; text-align: center; }
        .var-display h4 { font-size: 0.8rem; color: rgba(255,255,255,0.7); margin-bottom: 0.3rem; }
        .var-value { font-size: 2rem; font-weight: 700; color: var(--sand); }
        .var-controls { display: grid; grid-template-columns: repeat(3, 1fr); gap: 0.4rem; margin-bottom: 0.8rem; }
        .var-btn { aspect-ratio: 1; border: none; border-radius: 8px; font-size: 1.1rem; cursor: pointer; background: rgba(255,255,255,0.1); color: white; }
        .var-btn:hover { background: rgba(255,255,255,0.2); }
        .var-btn.arrow { background: linear-gradient(135deg, var(--ocean-light), var(--seaweed)); }
        .var-btn.empty { background: transparent; cursor: default; }
        .game-container { display: grid; grid-template-columns: 1fr 280px; gap: 1.5rem; align-items: start; }
        @media (max-width: 750px) { .game-container { grid-template-columns: 1fr; } }
        .grid-wrapper { background: rgba(0,0,0,0.2); padding: 1rem; border-radius: 14px; }
        .panel { background: rgba(255,255,255,0.05); border-radius: 14px; padding: 1.2rem; }
        .func-creator { background: rgba(160,108,213,0.2); border: 2px solid rgba(160,108,213,0.4); border-radius: 10px; padding: 0.8rem; margin-bottom: 0.8rem; }
        .func-creator h4 { color: var(--fish-purple); margin-bottom: 0.5rem; font-size: 0.95rem; }
        .func-name-input { width: 100%; padding: 0.4rem; border: none; border-radius: 6px; font-family: 'Fredoka', sans-serif; margin-bottom: 0.5rem; }
        .func-commands { display: flex; flex-wrap: wrap; gap: 0.3rem; min-height: 35px; background: rgba(0,0,0,0.2); padding: 0.4rem; border-radius: 6px; margin-bottom: 0.5rem; }
        .func-cmd { background: rgba(255,255,255,0.15); padding: 0.2rem 0.4rem; border-radius: 3px; font-size: 0.85rem; }
        .func-arrows { display: flex; gap: 0.4rem; justify-content: center; margin-bottom: 0.5rem; }
        .func-arrow-btn { width: 40px; height: 40px; border: none; border-radius: 8px; font-size: 1.1rem; cursor: pointer; background: linear-gradient(135deg, var(--ocean-light), var(--fish-purple)); color: white; }
        .func-list { display: flex; flex-direction: column; gap: 0.4rem; }
        .func-item { background: rgba(160,108,213,0.2); padding: 0.4rem 0.6rem; border-radius: 6px; cursor: pointer; display: flex; justify-content: space-between; font-size: 0.85rem; }
        .func-item:hover { background: rgba(160,108,213,0.4); }
        .func-item-name { font-weight: 600; color: var(--fish-purple); }
        .main-program { background: rgba(0,0,0,0.2); border-radius: 10px; padding: 0.8rem; margin-top: 0.8rem; }
        .main-program h4 { font-size: 0.8rem; color: rgba(255,255,255,0.7); margin-bottom: 0.4rem; }
        .program-display { display: flex; flex-wrap: wrap; gap: 0.2rem; min-height: 40px; }
        .program-item { background: rgba(255,255,255,0.1); padding: 0.2rem 0.4rem; border-radius: 3px; font-size: 0.8rem; }
        .program-item.function { background: rgba(160,108,213,0.3); color: var(--fish-purple); font-weight: 600; }
        .debug-container { max-width: 750px; margin: 0 auto; }
        .debug-mission { background: rgba(255,107,107,0.1); border: 2px solid rgba(255,107,107,0.3); border-radius: 10px; padding: 1rem; margin-bottom: 1rem; }
        .debug-mission h3 { color: var(--coral); margin-bottom: 0.4rem; }
        .debug-layout { display: grid; grid-template-columns: auto 1fr; gap: 1.5rem; align-items: start; }
        @media (max-width: 650px) { .debug-layout { grid-template-columns: 1fr; } }
        .code-line { display: flex; align-items: center; gap: 0.5rem; padding: 0.4rem; background: rgba(0,0,0,0.2); border-radius: 6px; margin-bottom: 0.4rem; }
        .line-number { background: rgba(255,255,255,0.1); padding: 0.2rem 0.4rem; border-radius: 3px; font-size: 0.75rem; }
        .line-content { flex: 1; font-size: 0.9rem; }
        .line-actions { display: flex; gap: 0.2rem; }
        .line-action-btn { background: rgba(255,255,255,0.1); border: none; padding: 0.2rem 0.4rem; border-radius: 3px; cursor: pointer; font-size: 0.75rem; color: white; }
        .debug-controls { display: flex; gap: 0.5rem; margin-top: 0.8rem; flex-wrap: wrap; }
        .tool-selector { display: grid; grid-template-columns: repeat(2, 1fr); gap: 0.4rem; margin-bottom: 1rem; }
        .tool-btn { padding: 0.6rem; border: 2px solid transparent; border-radius: 8px; cursor: pointer; font-family: 'Fredoka', sans-serif; font-size: 0.8rem; background: rgba(255,255,255,0.1); color: white; display: flex; flex-direction: column; align-items: center; gap: 0.2rem; }
        .tool-btn .tool-icon { font-size: 1.3rem; }
        .tool-btn.active { border-color: var(--sand); background: rgba(245,215,142,0.2); }
        .creator-grid { display: grid; gap: 2px; }
        .creator-cell { aspect-ratio: 1; border-radius: 3px; cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 0.9rem; background: rgba(255,255,255,0.05); }
        .creator-cell:hover { background: rgba(255,255,255,0.15); }
        .test-area { background: rgba(0,0,0,0.2); border-radius: 10px; padding: 0.8rem; margin-top: 0.8rem; }
        .test-area h4 { font-size: 0.8rem; color: rgba(255,255,255,0.7); margin-bottom: 0.5rem; }
        .test-commands { display: flex; flex-wrap: wrap; gap: 0.2rem; min-height: 35px; background: rgba(255,255,255,0.05); padding: 0.4rem; border-radius: 6px; }
        .blocks-container { display: grid; grid-template-columns: 200px 1fr 220px; gap: 1rem; align-items: start; }
        @media (max-width: 850px) { .blocks-container { grid-template-columns: 1fr; } }
        .blocks-palette { background: rgba(255,255,255,0.05); border-radius: 14px; padding: 0.8rem; }
        .blocks-palette h4 { font-size: 0.85rem; color: rgba(255,255,255,0.7); margin-bottom: 0.5rem; }
        .palette-section { margin-bottom: 0.8rem; }
        .palette-section-title { font-size: 0.75rem; color: var(--sand); margin-bottom: 0.3rem; }
        .block-item { padding: 0.5rem 0.6rem; border-radius: 6px; margin-bottom: 0.3rem; cursor: pointer; font-size: 0.8rem; }
        .block-item.move { background: var(--ocean-light); }
        .block-item.loop { background: var(--fish-purple); }
        .blocks-workspace { background: rgba(0,0,0,0.2); border-radius: 14px; padding: 0.8rem; min-height: 300px; }
        .workspace-area { background: rgba(255,255,255,0.03); border: 2px dashed rgba(255,255,255,0.2); border-radius: 10px; min-height: 220px; padding: 0.8rem; }
        .workspace-block { padding: 0.5rem 0.8rem; border-radius: 6px; margin-bottom: 0.3rem; display: flex; justify-content: space-between; align-items: center; }
        .workspace-block .remove-block { background: rgba(0,0,0,0.2); border: none; color: white; width: 20px; height: 20px; border-radius: 50%; cursor: pointer; font-size: 0.7rem; }
        .blocks-preview { background: rgba(255,255,255,0.05); border-radius: 14px; padding: 0.8rem; }
        .preview-grid { display: grid; gap: 2px; }
        .preview-cell { aspect-ratio: 1; border-radius: 3px; display: flex; align-items: center; justify-content: center; font-size: 0.85rem; background: rgba(255,255,255,0.05); }
        .sensor-readings { background: rgba(0,0,0,0.3); border-radius: 10px; padding: 0.8rem; margin-bottom: 0.8rem; }
        .sensor-readings h4 { font-size: 0.85rem; color: rgba(255,255,255,0.7); margin-bottom: 0.5rem; }
        .reading-row { display: flex; justify-content: space-between; align-items: center; padding: 0.3rem 0; border-bottom: 1px solid rgba(255,255,255,0.1); font-size: 0.85rem; }
        .reading-row:last-child { border-bottom: none; }
        .reading-value { padding: 0.15rem 0.5rem; border-radius: 15px; font-size: 0.75rem; font-weight: 600; }
        .reading-value.blocked { background: var(--coral); }
        .reading-value.clear { background: var(--seaweed); }
        .sensor-rules { background: rgba(245,215,142,0.1); border: 2px solid rgba(245,215,142,0.3); border-radius: 10px; padding: 0.8rem; margin-bottom: 0.8rem; }
        .sensor-rules h4 { color: var(--sand); margin-bottom: 0.3rem; font-size: 0.9rem; }
        .rule-item { font-size: 0.8rem; padding: 0.2rem 0; }
        .sensor-controls { display: flex; flex-direction: column; gap: 0.4rem; }
        .feedback-modal { display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); z-index: 1000; align-items: center; justify-content: center; padding: 1rem; }
        .feedback-modal.active { display: flex; }
        .feedback-content { background: linear-gradient(135deg, var(--ocean-mid), var(--ocean-dark)); border: 2px solid rgba(255,255,255,0.2); padding: 2rem; border-radius: 20px; text-align: center; max-width: 350px; width: 100%; }
        .feedback-icon { font-size: 3.5rem; margin-bottom: 0.8rem; }
        .feedback-content h3 { font-size: 1.6rem; margin-bottom: 0.4rem; color: var(--sand); }
        .feedback-content p { color: rgba(255,255,255,0.8); margin-bottom: 1.2rem; }
        .feedback-btn { background: linear-gradient(135deg, var(--seaweed), var(--ocean-light)); color: white; border: none; padding: 0.8rem 1.8rem; border-radius: 50px; font-family: 'Fredoka', sans-serif; font-size: 1rem; cursor: pointer; }
    </style>
</head>
<body>
    <div class="bubbles" id="bubblesContainer"></div>
    <header>
        <div class="logo"><div class="logo-icon">🐠</div><h1>RoboCode Kids 3<span>Aventure Océan</span></h1></div>
        <div class="score-badge">🐚 <span id="totalScore">0</span> pts</div>
    </header>
    <div class="container">
        <div id="mainMenu" class="main-menu">
            <div class="menu-card variables" onclick="startGame('variables')"><div class="card-icon">🎒</div><h2>Le Sac à Perles</h2><p>Ramasse des perles et compte avec une variable !</p><div class="difficulty"><span class="star-icon">⭐⭐</span><span class="star-icon empty">⭐</span></div></div>
            <div class="menu-card functions" onclick="startGame('functions')"><div class="card-icon">📦</div><h2>Super Pouvoirs</h2><p>Crée tes propres commandes réutilisables !</p><div class="difficulty"><span class="star-icon">⭐⭐⭐</span></div></div>
            <div class="menu-card debug" onclick="startGame('debug')"><div class="card-icon">🔧</div><h2>Docteur Bug</h2><p>Trouve et corrige les erreurs du programme !</p><div class="difficulty"><span class="star-icon">⭐⭐</span><span class="star-icon empty">⭐</span></div></div>
            <div class="menu-card creator" onclick="startGame('creator')"><div class="card-icon">🎨</div><h2>Créateur</h2><p>Dessine ton propre niveau océan !</p><div class="difficulty"><span class="star-icon">⭐</span><span class="star-icon empty">⭐⭐</span></div></div>
            <div class="menu-card blocks" onclick="startGame('blocks')"><div class="card-icon">🧱</div><h2>Blocs Magiques</h2><p>Programme avec des blocs colorés !</p><div class="difficulty"><span class="star-icon">⭐⭐⭐</span></div></div>
            <div class="menu-card sensors" onclick="startGame('sensors')"><div class="card-icon">📡</div><h2>Les Capteurs</h2><p>Le sous-marin détecte et réagit tout seul !</p><div class="difficulty"><span class="star-icon">⭐⭐⭐</span></div></div>
        </div>
        <div id="variablesScreen" class="game-screen">
            <div class="game-header"><div class="game-title"><span style="font-size:1.8rem;">🎒</span><h2>Le Sac à Perles</h2></div><div class="level-info">Niv. <span id="varLevel">1</span>/3</div><button class="back-btn" onclick="backToMenu()">← Retour</button></div>
            <div class="game-container"><div class="grid-wrapper"><div class="var-grid" id="varGrid"></div></div>
                <div class="panel"><div class="var-mission"><h4>🎯 Mission</h4><p id="varMissionText"></p></div><div class="var-display"><h4>🎒 Perles</h4><div class="var-value" id="pearlCount">0</div></div>
                    <div class="var-controls"><div class="var-btn empty"></div><button class="var-btn arrow" onclick="moveSubmarine('up')">⬆️</button><div class="var-btn empty"></div><button class="var-btn arrow" onclick="moveSubmarine('left')">⬅️</button><div class="var-btn empty"></div><button class="var-btn arrow" onclick="moveSubmarine('right')">➡️</button><div class="var-btn empty"></div><button class="var-btn arrow" onclick="moveSubmarine('down')">⬇️</button><div class="var-btn empty"></div></div>
                    <button class="action-btn secondary" onclick="resetVarLevel()" style="width:100%;">🔄 Recommencer</button></div></div></div>
        <div id="functionsScreen" class="game-screen">
            <div class="game-header"><div class="game-title"><span style="font-size:1.8rem;">📦</span><h2>Super Pouvoirs</h2></div><div class="level-info">Niv. <span id="funcLevel">1</span>/3</div><button class="back-btn" onclick="backToMenu()">← Retour</button></div>
            <div class="game-container"><div class="grid-wrapper"><div class="var-mission"><h4>🎯 Mission</h4><p id="funcMissionText"></p></div><div class="var-grid" id="funcGrid"></div></div>
                <div class="panel"><div class="func-creator"><h4>✨ Créer fonction</h4><input type="text" class="func-name-input" id="funcNameInput" placeholder="Nom"><div class="func-commands" id="funcCommands"></div><div class="func-arrows"><button class="func-arrow-btn" onclick="addToFunction('⬆️')">⬆️</button><button class="func-arrow-btn" onclick="addToFunction('⬇️')">⬇️</button><button class="func-arrow-btn" onclick="addToFunction('⬅️')">⬅️</button><button class="func-arrow-btn" onclick="addToFunction('➡️')">➡️</button></div><div style="display:flex;gap:0.4rem;"><button class="action-btn primary" onclick="saveFunction()" style="flex:1;">💾</button><button class="action-btn secondary" onclick="clearFunction()">🗑️</button></div></div>
                    <div style="font-size:0.8rem;color:rgba(255,255,255,0.7);margin-bottom:0.3rem;">📚 Mes fonctions</div><div class="func-list" id="funcList"></div>
                    <div class="main-program"><h4>📝 Programme</h4><div class="program-display" id="mainProgram"></div><div style="display:flex;gap:0.4rem;margin-top:0.5rem;"><button class="action-btn primary" onclick="runFuncProgram()" style="flex:1;">▶️</button><button class="action-btn secondary" onclick="clearMainProgram()">🗑️</button></div></div></div></div></div>
        <div id="debugScreen" class="game-screen">
            <div class="game-header"><div class="game-title"><span style="font-size:1.8rem;">🔧</span><h2>Docteur Bug</h2></div><div class="level-info">Bug <span id="debugLevel">1</span>/4</div><button class="back-btn" onclick="backToMenu()">← Retour</button></div>
            <div class="debug-container"><div class="debug-mission"><h3>🐛 Problème</h3><p id="debugMissionText"></p></div>
                <div class="debug-layout"><div class="grid-wrapper"><div class="var-grid" id="debugGrid"></div></div><div class="panel"><h4 style="color:var(--coral);margin-bottom:0.8rem;">📝 Code à corriger</h4><div id="debugCodeLines"></div><div class="debug-controls"><button class="action-btn primary" onclick="testDebugCode()">▶️ Tester</button><button class="action-btn secondary" onclick="resetDebugLevel()">🔄</button></div></div></div></div></div>
        <div id="creatorScreen" class="game-screen">
            <div class="game-header"><div class="game-title"><span style="font-size:1.8rem;">🎨</span><h2>Créateur</h2></div><div class="score-badge" style="background:linear-gradient(135deg,var(--sand),var(--fish-orange));">🏆 Score: <span id="creatorScore">0</span> pts</div><button class="back-btn" onclick="backToMenu()">← Retour</button></div>
            <div class="game-container"><div class="grid-wrapper"><div class="var-mission" style="margin-bottom:0.8rem;"><h4>🎯 Crée ton niveau !</h4><p>1. Choisis un élément dans les outils<br>2. Clique sur une case pour le placer<br>3. Programme le chemin avec les flèches !</p><p style="margin-top:0.5rem;font-size:0.8rem;color:var(--sand);">🔮 Perle = +1 pt | 💎 Trésor = +2 pts | 🪨 Rocher = -1 pt</p></div><div class="creator-grid" id="creatorGrid"></div></div>
                <div class="panel"><h4 style="margin-bottom:0.5rem;">🛠️ Choisis un élément</h4>
                    <div class="tool-selector"><button class="tool-btn active" onclick="selectTool('empty')" id="tool-empty"><span class="tool-icon">🌊</span>Eau</button><button class="tool-btn" onclick="selectTool('wall')" id="tool-wall"><span class="tool-icon">🪨</span>Rocher</button><button class="tool-btn" onclick="selectTool('sub')" id="tool-sub"><span class="tool-icon">🚤</span>Sous-marin</button><button class="tool-btn" onclick="selectTool('treasure')" id="tool-treasure"><span class="tool-icon">💎</span>Trésor</button><button class="tool-btn" onclick="selectTool('pearl')" id="tool-pearl"><span class="tool-icon">🔮</span>Perle</button><button class="tool-btn" onclick="selectTool('seaweed')" id="tool-seaweed"><span class="tool-icon">🌿</span>Algue</button></div>
                    <button class="action-btn secondary" onclick="clearCreatorGrid()" style="width:100%;margin-bottom:0.8rem;">🗑️ Tout effacer</button>
                    <div class="test-area"><h4>🎮 Programme le chemin</h4><p style="font-size:0.75rem;color:rgba(255,255,255,0.6);margin-bottom:0.4rem;">Clique sur les flèches puis "Lancer"</p><div class="test-commands" id="testCommands"></div><div style="display:grid;grid-template-columns:repeat(4,1fr);gap:0.3rem;margin-top:0.5rem;"><button class="var-btn arrow" onclick="addTestCommand('⬆️')">⬆️</button><button class="var-btn arrow" onclick="addTestCommand('⬇️')">⬇️</button><button class="var-btn arrow" onclick="addTestCommand('⬅️')">⬅️</button><button class="var-btn arrow" onclick="addTestCommand('➡️')">➡️</button></div><div style="display:flex;gap:0.3rem;margin-top:0.4rem;"><button class="action-btn primary" onclick="runTestCommands()" style="flex:1;">▶️ Lancer</button><button class="action-btn secondary" onclick="clearTestCommands()">🗑️</button></div></div></div></div></div>
        <div id="blocksScreen" class="game-screen">
            <div class="game-header"><div class="game-title"><span style="font-size:1.8rem;">🧱</span><h2>Blocs Magiques</h2></div><div class="level-info">Niv. <span id="blocksLevel">1</span>/3</div><button class="back-btn" onclick="backToMenu()">← Retour</button></div>
            <div class="blocks-container">
                <div class="blocks-palette"><h4>📦 Blocs</h4><div class="palette-section"><div class="palette-section-title">Mouvements</div><div class="block-item move" onclick="addBlock('move','⬆️')">⬆️ Haut</div><div class="block-item move" onclick="addBlock('move','⬇️')">⬇️ Bas</div><div class="block-item move" onclick="addBlock('move','⬅️')">⬅️ Gauche</div><div class="block-item move" onclick="addBlock('move','➡️')">➡️ Droite</div></div><div class="palette-section"><div class="palette-section-title">Boucles</div><div class="block-item loop" onclick="addBlock('loop','2')">🔁 Répéter 2x</div><div class="block-item loop" onclick="addBlock('loop','3')">🔁 Répéter 3x</div><div class="block-item loop" onclick="addBlock('loop-end','')">] Fin</div></div></div>
                <div class="blocks-workspace"><div style="display:flex;justify-content:space-between;margin-bottom:0.5rem;"><h4>📝 Programme</h4><button class="action-btn secondary" onclick="clearBlocks()" style="padding:0.3rem 0.6rem;font-size:0.8rem;">🗑️</button></div><div class="workspace-area" id="blocksWorkspace"></div><button class="action-btn primary" onclick="runBlocks()" style="width:100%;margin-top:0.8rem;">▶️ Exécuter</button></div>
                <div class="blocks-preview"><div class="var-mission"><h4>🎯 Mission</h4><p id="blocksMissionText"></p></div><div style="background:rgba(0,0,0,0.2);padding:0.5rem;border-radius:10px;"><div class="preview-grid" id="blocksGrid"></div></div></div></div></div>
        <div id="sensorsScreen" class="game-screen">
            <div class="game-header"><div class="game-title"><span style="font-size:1.8rem;">📡</span><h2>Les Capteurs</h2></div><div class="level-info">Niv. <span id="sensorsLevel">1</span>/3</div><button class="back-btn" onclick="backToMenu()">← Retour</button></div>
            <div class="game-container"><div class="grid-wrapper"><div class="var-mission"><h4>🎯 Mission</h4><p id="sensorsMissionText"></p></div><div class="var-grid" id="sensorsGrid"></div></div>
                <div class="panel"><div class="sensor-readings"><h4>📡 Que voit le sous-marin ?</h4><div class="reading-row"><span>⬆️ Haut</span><span class="reading-value clear" id="sensorUp">?</span></div><div class="reading-row"><span>⬇️ Bas</span><span class="reading-value clear" id="sensorDown">?</span></div><div class="reading-row"><span>⬅️ Gauche</span><span class="reading-value clear" id="sensorLeft">?</span></div><div class="reading-row"><span>➡️ Droite</span><span class="reading-value clear" id="sensorRight">?</span></div></div>
                    <div class="sensor-rules"><h4>🧭 Choisis la bonne direction !</h4><p style="font-size:0.75rem;color:rgba(255,255,255,0.7);">Utilise les capteurs pour éviter les rochers.</p></div>
                    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:0.4rem;margin:0.8rem 0;"><div></div><button class="var-btn arrow" onclick="moveSensorsManual('up')">⬆️</button><div></div><button class="var-btn arrow" onclick="moveSensorsManual('left')">⬅️</button><div></div><button class="var-btn arrow" onclick="moveSensorsManual('right')">➡️</button><div></div><button class="var-btn arrow" onclick="moveSensorsManual('down')">⬇️</button><div></div></div>
                    <button class="action-btn secondary" onclick="resetSensorsLevel()" style="width:100%;">🔄 Recommencer</button></div></div></div>
    </div>
    <div class="feedback-modal" id="feedbackModal"><div class="feedback-content"><div class="feedback-icon" id="feedbackIcon">🎉</div><h3 id="feedbackTitle">Bravo !</h3><p id="feedbackText">Réussi !</p><button class="feedback-btn" onclick="closeFeedback()">Continuer</button></div></div>
<script>
let totalScore=0;function generateBubbles(){const c=document.getElementById('bubblesContainer');for(let i=0;i<12;i++){const b=document.createElement('div');b.className='bubble';b.style.left=Math.random()*100+'%';b.style.width=Math.random()*25+10+'px';b.style.height=b.style.width;b.style.animationDelay=Math.random()*8+'s';b.style.animationDuration=(Math.random()*4+6)+'s';c.appendChild(b);}}
function startGame(g){document.getElementById('mainMenu').style.display='none';document.getElementById(g+'Screen').classList.add('active');if(g==='variables')initVariables();else if(g==='functions')initFunctions();else if(g==='debug')initDebug();else if(g==='creator')initCreator();else if(g==='blocks')initBlocks();else if(g==='sensors')initSensors();}
function backToMenu(){document.querySelectorAll('.game-screen').forEach(s=>s.classList.remove('active'));document.getElementById('mainMenu').style.display='grid';}
function updateScore(p){totalScore+=p;document.getElementById('totalScore').textContent=totalScore;}
function showFeedback(ok,t,txt,cb){document.getElementById('feedbackIcon').textContent=ok?'🎉':'🐙';document.getElementById('feedbackTitle').textContent=t;document.getElementById('feedbackText').textContent=txt;document.getElementById('feedbackModal').classList.add('active');window.fbCb=cb;}
function closeFeedback(){document.getElementById('feedbackModal').classList.remove('active');if(window.fbCb)window.fbCb();}
function sleep(ms){return new Promise(r=>setTimeout(r,ms));}

let varLevel=0,varPos={x:0,y:0},pearlCount=0;
const varLevelsOrig=[{grid:[['S','.','🔮','.','.'],['.','.','.','.','.'],['.','🔮','.','🔮','.'],['.','.','.','.','.'],['.','.','🏠','.','.']],start:{x:0,y:0},base:{x:2,y:4},target:3,mission:"Ramasse 3 perles et va à la base 🏠"},{grid:[['.', '.', '🔮', '.', 'S'],['.', '🪨', '🪨', '.', '.'],['🔮', '.', '🔮', '.', '🔮'],['.', '🪨', '.', '🪨', '.'],['🏠', '.', '🔮', '.', '.']],start:{x:4,y:0},base:{x:0,y:4},target:5,mission:"5 perles en évitant les rochers !"},{grid:[['🏠', '.', '🔮', '🪨', '🔮'],['.', '🪨', '.', '.', '.'],['🔮', '.', '🔮', '🪨', '🔮'],['.', '.', '🪨', '.', '.'],['🔮', '.', '🔮', '.', 'S']],start:{x:4,y:4},base:{x:0,y:0},target:7,mission:"Défi ! 7 perles et retour !"}];
let currentVarGrid=[];
function initVariables(){varLevel=0;loadVarLevel();}
function loadVarLevel(){const l=varLevelsOrig[varLevel];varPos={...l.start};pearlCount=0;currentVarGrid=l.grid.map(row=>[...row]);document.getElementById('pearlCount').textContent='0';document.getElementById('varLevel').textContent=varLevel+1;document.getElementById('varMissionText').textContent=l.mission;renderVarGrid();}
function renderVarGrid(){const g=document.getElementById('varGrid');g.style.gridTemplateColumns=`repeat(${currentVarGrid[0].length},1fr)`;g.innerHTML='';for(let y=0;y<currentVarGrid.length;y++)for(let x=0;x<currentVarGrid[y].length;x++){const c=document.createElement('div');c.className='var-cell';if(x===varPos.x&&y===varPos.y){c.classList.add('sub');c.textContent='🚤';}else if(currentVarGrid[y][x]==='🪨'){c.style.background='var(--ocean-dark)';c.textContent='🪨';}else if(currentVarGrid[y][x]==='🔮'){c.style.background='var(--sand)';c.textContent='🔮';}else if(currentVarGrid[y][x]==='🏠'){c.style.background='var(--seaweed)';c.textContent='🏠';}g.appendChild(c);}}
function moveSubmarine(dir){let nx=varPos.x,ny=varPos.y;if(dir==='up')ny--;else if(dir==='down')ny++;else if(dir==='left')nx--;else if(dir==='right')nx++;if(nx<0||nx>=currentVarGrid[0].length||ny<0||ny>=currentVarGrid.length||currentVarGrid[ny][nx]==='🪨')return;varPos={x:nx,y:ny};if(currentVarGrid[ny][nx]==='🔮'){pearlCount++;document.getElementById('pearlCount').textContent=pearlCount;currentVarGrid[ny][nx]='.';}if(currentVarGrid[ny][nx]==='🏠'&&pearlCount>=varLevelsOrig[varLevel].target){updateScore(20+varLevel*10);varLevel++;if(varLevel<varLevelsOrig.length)showFeedback(true,'Super!','Niveau suivant!',loadVarLevel);else showFeedback(true,'Champion!','Terminé!',()=>{varLevel=0;loadVarLevel();});}renderVarGrid();}
function resetVarLevel(){loadVarLevel();}

let funcLevel=0,funcPos={x:0,y:0},currentFuncCommands=[],savedFunctions=[],mainProgramCommands=[];
const funcLevels=[{grid:[['S','.','.','.','.','💎']],start:{x:0,y:0},goal:{x:5,y:0},mission:"Crée une fonction AVANCE5 (5x droite) puis utilise-la !"},{grid:[['S','.','.','.'],['.','.','.','.'],['.','.','.','.'],['.','.','.','.'],['💎','.','.','.']], start:{x:0,y:0},goal:{x:0,y:4},mission:"Crée une fonction BAS4 (4x bas) puis utilise-la !"},{grid:[['S','.','.'],['.','.','.'],['.','.','.'],['💎','.','.']], start:{x:0,y:0},goal:{x:0,y:3},mission:"Crée DESCENTE (3x bas) et utilise-la !"}];
function initFunctions(){funcLevel=0;savedFunctions=[];loadFuncLevel();}
function loadFuncLevel(){const l=funcLevels[funcLevel];funcPos={...l.start};currentFuncCommands=[];mainProgramCommands=[];document.getElementById('funcLevel').textContent=funcLevel+1;document.getElementById('funcMissionText').textContent=l.mission;document.getElementById('funcCommands').innerHTML='';document.getElementById('mainProgram').innerHTML='';document.getElementById('funcNameInput').value='';renderFuncGrid();renderFuncList();}
function renderFuncGrid(){const l=funcLevels[funcLevel],g=document.getElementById('funcGrid');g.style.gridTemplateColumns=`repeat(${l.grid[0].length},1fr)`;g.innerHTML='';for(let y=0;y<l.grid.length;y++)for(let x=0;x<l.grid[y].length;x++){const c=document.createElement('div');c.className='var-cell';if(x===funcPos.x&&y===funcPos.y){c.classList.add('sub');c.textContent='🚤';}else if(l.grid[y][x]==='💎'){c.style.background='var(--sand)';c.textContent='💎';}g.appendChild(c);}}
function addToFunction(cmd){currentFuncCommands.push(cmd);const s=document.createElement('span');s.className='func-cmd';s.textContent=cmd;document.getElementById('funcCommands').appendChild(s);}
function clearFunction(){currentFuncCommands=[];document.getElementById('funcCommands').innerHTML='';document.getElementById('funcNameInput').value='';}
function saveFunction(){const n=document.getElementById('funcNameInput').value.trim().toUpperCase();if(!n||!currentFuncCommands.length)return;savedFunctions.push({name:n,commands:[...currentFuncCommands]});clearFunction();renderFuncList();}
function renderFuncList(){const l=document.getElementById('funcList');l.innerHTML='';savedFunctions.forEach(f=>{const d=document.createElement('div');d.className='func-item';d.onclick=()=>addFunctionToProgram(f.name);d.innerHTML=`<span class="func-item-name">${f.name}</span><span style="font-size:0.75rem;color:rgba(255,255,255,0.6);">${f.commands.join('')}</span>`;l.appendChild(d);});}
function addFunctionToProgram(name){mainProgramCommands.push({type:'function',name});renderMainProgram();}
function renderMainProgram(){const c=document.getElementById('mainProgram');c.innerHTML='';mainProgramCommands.forEach(cmd=>{const s=document.createElement('span');s.className='program-item'+(cmd.type==='function'?' function':'');s.textContent=cmd.type==='function'?cmd.name:cmd;c.appendChild(s);});}
function clearMainProgram(){mainProgramCommands=[];document.getElementById('mainProgram').innerHTML='';}
async function runFuncProgram(){const l=funcLevels[funcLevel];funcPos={...l.start};renderFuncGrid();let all=[];for(const cmd of mainProgramCommands){if(cmd.type==='function'){const f=savedFunctions.find(x=>x.name===cmd.name);if(f)all.push(...f.commands);}else all.push(cmd);}for(const cmd of all){let dx=0,dy=0;if(cmd==='⬆️')dy=-1;else if(cmd==='⬇️')dy=1;else if(cmd==='⬅️')dx=-1;else if(cmd==='➡️')dx=1;const nx=funcPos.x+dx,ny=funcPos.y+dy;if(nx>=0&&nx<l.grid[0].length&&ny>=0&&ny<l.grid.length)funcPos={x:nx,y:ny};renderFuncGrid();await sleep(250);}if(funcPos.x===l.goal.x&&funcPos.y===l.goal.y){updateScore(30);funcLevel++;if(funcLevel<funcLevels.length)showFeedback(true,'Excellent!','Suivant!',loadFuncLevel);else showFeedback(true,'Génie!','Tu maîtrises!',()=>{funcLevel=0;loadFuncLevel();});}else showFeedback(false,'Pas encore...','Vérifie!',null);}

let debugLevel=0,currentDebugCode=[];
const debugLevels=[{grid:[['S','.','💎']],start:{x:0,y:0},goal:{x:2,y:0},code:['➡️','⬆️'],mission:"Le sous-marin monte au lieu d'aller à droite ! Corrige la 2ème commande."},{grid:[['S','.'],['.','💎']],start:{x:0,y:0},goal:{x:1,y:1},code:['➡️'],mission:"Il manque une commande pour descendre au trésor !"},{grid:[['S','.','💎']],start:{x:0,y:0},goal:{x:2,y:0},code:['⬅️','➡️','➡️'],mission:"La 1ère commande est fausse ! Il va dans le mauvais sens."},{grid:[['S'],['.'],['💎']],start:{x:0,y:0},goal:{x:0,y:2},code:['⬇️','➡️'],mission:"La 2ème commande est fausse ! Il doit continuer à descendre."}];
function initDebug(){debugLevel=0;loadDebugLevel();}
function loadDebugLevel(){const l=debugLevels[debugLevel];currentDebugCode=[...l.code];document.getElementById('debugLevel').textContent=debugLevel+1;document.getElementById('debugMissionText').textContent=l.mission;renderDebugGrid();renderDebugCode();}
function renderDebugGrid(){const l=debugLevels[debugLevel],g=document.getElementById('debugGrid');g.style.gridTemplateColumns=`repeat(${l.grid[0].length},1fr)`;g.innerHTML='';for(let y=0;y<l.grid.length;y++)for(let x=0;x<l.grid[y].length;x++){const c=document.createElement('div');c.className='var-cell';c.id=`debug-${x}-${y}`;if(x===l.start.x&&y===l.start.y){c.classList.add('sub');c.textContent='🚤';}else if(l.grid[y][x]==='💎'){c.style.background='var(--sand)';c.textContent='💎';}else if(l.grid[y][x]==='🪨'){c.style.background='var(--ocean-dark)';c.textContent='🪨';}g.appendChild(c);}}
function renderDebugCode(){const c=document.getElementById('debugCodeLines');c.innerHTML='';currentDebugCode.forEach((line,i)=>{const d=document.createElement('div');d.className='code-line';d.innerHTML=`<span class="line-number">${i+1}</span><span class="line-content">${line}</span><div class="line-actions"><button class="line-action-btn" onclick="changeDebugLine(${i},'up')">⬆️</button><button class="line-action-btn" onclick="changeDebugLine(${i},'down')">⬇️</button><button class="line-action-btn" onclick="changeDebugLine(${i},'left')">⬅️</button><button class="line-action-btn" onclick="changeDebugLine(${i},'right')">➡️</button><button class="line-action-btn" onclick="addDebugLine(${i})">+</button><button class="line-action-btn" onclick="removeDebugLine(${i})">🗑️</button></div>`;c.appendChild(d);});}
function changeDebugLine(i,dir){const a={up:'⬆️',down:'⬇️',left:'⬅️',right:'➡️'};currentDebugCode[i]=a[dir];renderDebugCode();}
function addDebugLine(i){currentDebugCode.splice(i+1,0,'➡️');renderDebugCode();}
function removeDebugLine(i){if(currentDebugCode.length>1){currentDebugCode.splice(i,1);renderDebugCode();}}
async function testDebugCode(){const l=debugLevels[debugLevel];let pos={...l.start};renderDebugGrid();await sleep(200);for(const cmd of currentDebugCode){let dx=0,dy=0;if(cmd==='⬆️')dy=-1;else if(cmd==='⬇️')dy=1;else if(cmd==='⬅️')dx=-1;else if(cmd==='➡️')dx=1;const nx=pos.x+dx,ny=pos.y+dy;if(nx<0||nx>=l.grid[0].length||ny<0||ny>=l.grid.length){showFeedback(false,'Crash!','Sorti!',null);return;}if(l.grid[ny][nx]==='🪨'){showFeedback(false,'Boum!','Rocher!',null);return;}const old=document.getElementById(`debug-${pos.x}-${pos.y}`);old.classList.remove('sub');old.textContent='';pos={x:nx,y:ny};const nw=document.getElementById(`debug-${pos.x}-${pos.y}`);nw.classList.add('sub');nw.textContent='🚤';await sleep(350);}if(pos.x===l.goal.x&&pos.y===l.goal.y){updateScore(25);debugLevel++;if(debugLevel<debugLevels.length)showFeedback(true,'Corrigé!','Suivant!',loadDebugLevel);else showFeedback(true,'Expert!','Tous bugs corrigés!',()=>{debugLevel=0;loadDebugLevel();});}else showFeedback(false,'Pas encore...','Vérifie!',null);}
function resetDebugLevel(){loadDebugLevel();}

const CREATOR_SIZE=6;let creatorGrid=[],currentTool='empty',testCommands=[],creatorSubPos=null,creatorTreasurePos=null,creatorScore=0;
function initCreator(){creatorGrid=Array(CREATOR_SIZE).fill(null).map(()=>Array(CREATOR_SIZE).fill('none'));creatorSubPos=null;creatorTreasurePos=null;creatorScore=0;testCommands=[];document.getElementById('creatorScore').textContent='0';renderCreatorGrid();document.getElementById('testCommands').innerHTML='';}
function renderCreatorGrid(){const g=document.getElementById('creatorGrid');g.style.gridTemplateColumns=`repeat(${CREATOR_SIZE},1fr)`;g.innerHTML='';for(let y=0;y<CREATOR_SIZE;y++)for(let x=0;x<CREATOR_SIZE;x++){const c=document.createElement('div');c.className='creator-cell';c.onclick=()=>paintCell(x,y);const t=creatorGrid[y][x];if(t==='wall'){c.style.background='var(--ocean-dark)';c.textContent='🪨';}else if(t==='sub'){c.style.background='var(--ocean-light)';c.textContent='🚤';}else if(t==='treasure'){c.style.background='var(--sand)';c.textContent='💎';}else if(t==='pearl'){c.textContent='🔮';}else if(t==='seaweed'){c.style.background='var(--seaweed)';c.textContent='🌿';}else if(t==='empty'){c.style.background='rgba(46,139,192,0.4)';c.textContent='🌊';}g.appendChild(c);}}
function selectTool(tool){currentTool=tool;document.querySelectorAll('.tool-btn').forEach(b=>b.classList.remove('active'));document.getElementById('tool-'+tool).classList.add('active');}
function paintCell(x,y){if(currentTool==='sub'&&creatorSubPos)creatorGrid[creatorSubPos.y][creatorSubPos.x]='none';if(currentTool==='treasure'&&creatorTreasurePos)creatorGrid[creatorTreasurePos.y][creatorTreasurePos.x]='none';if((currentTool==='empty'||currentTool==='none')&&creatorGrid[y][x]==='sub')creatorSubPos=null;if((currentTool==='empty'||currentTool==='none')&&creatorGrid[y][x]==='treasure')creatorTreasurePos=null;creatorGrid[y][x]=currentTool;if(currentTool==='sub')creatorSubPos={x,y};if(currentTool==='treasure')creatorTreasurePos={x,y};renderCreatorGrid();}
function clearCreatorGrid(){initCreator();}
function addTestCommand(cmd){testCommands.push(cmd);const s=document.createElement('span');s.className='program-item';s.textContent=cmd;document.getElementById('testCommands').appendChild(s);}
function testCreatorLevel(){if(!creatorSubPos){showFeedback(false,'Oups!','Place un sous-marin 🚤 sur la grille !',null);return;}showFeedback(true,'C\'est parti !','Programme le chemin avec les flèches.',null);}
function clearTestCommands(){testCommands=[];document.getElementById('testCommands').innerHTML='';}
async function runTestCommands(){if(!creatorSubPos){showFeedback(false,'Oups!','Place un sous-marin 🚤 d\'abord !',null);return;}creatorScore=0;document.getElementById('creatorScore').textContent='0';let pos={...creatorSubPos};for(const cmd of testCommands){let dx=0,dy=0;if(cmd==='⬆️')dy=-1;else if(cmd==='⬇️')dy=1;else if(cmd==='⬅️')dx=-1;else if(cmd==='➡️')dx=1;const nx=pos.x+dx,ny=pos.y+dy;if(nx>=0&&nx<CREATOR_SIZE&&ny>=0&&ny<CREATOR_SIZE&&creatorGrid[ny][nx]!=='wall'){const oldCell=creatorGrid[ny][nx];creatorGrid[pos.y][pos.x]='none';pos={x:nx,y:ny};if(oldCell==='pearl'){creatorScore+=1;document.getElementById('creatorScore').textContent=creatorScore;}else if(oldCell==='treasure'){creatorScore+=2;document.getElementById('creatorScore').textContent=creatorScore;creatorTreasurePos=null;}creatorGrid[ny][nx]='sub';creatorSubPos=pos;renderCreatorGrid();await sleep(250);}else if(nx>=0&&nx<CREATOR_SIZE&&ny>=0&&ny<CREATOR_SIZE&&creatorGrid[ny][nx]==='wall'){creatorScore-=1;document.getElementById('creatorScore').textContent=creatorScore;}}testCommands=[];document.getElementById('testCommands').innerHTML='';if(creatorScore>0)showFeedback(true,'Bravo!','Tu as marqué '+creatorScore+' points !',null);}

let blocksLevel=0,workspaceBlocks=[];
const blocksLevels=[{grid:[['S','.','.','💎']],start:{x:0,y:0},goal:{x:3,y:0},mission:"3x Droite!"},{grid:[['S','.','.','.'],['.','.','.','.'],['.','.','.','💎']],start:{x:0,y:0},goal:{x:3,y:2},mission:"Utilise une boucle!"},{grid:[['S','.','🪨','💎'],['.','.','.','.']], start:{x:0,y:0},goal:{x:3,y:0},mission:"Contourne!"}];
function initBlocks(){blocksLevel=0;workspaceBlocks=[];loadBlocksLevel();}
function loadBlocksLevel(){const l=blocksLevels[blocksLevel];workspaceBlocks=[];document.getElementById('blocksLevel').textContent=blocksLevel+1;document.getElementById('blocksMissionText').textContent=l.mission;document.getElementById('blocksWorkspace').innerHTML='';renderBlocksGrid();}
function renderBlocksGrid(){const l=blocksLevels[blocksLevel],g=document.getElementById('blocksGrid');g.style.gridTemplateColumns=`repeat(${l.grid[0].length},1fr)`;g.innerHTML='';for(let y=0;y<l.grid.length;y++)for(let x=0;x<l.grid[y].length;x++){const c=document.createElement('div');c.className='preview-cell';c.id=`blocks-${x}-${y}`;if(x===l.start.x&&y===l.start.y){c.style.background='var(--ocean-light)';c.textContent='🚤';}else if(l.grid[y][x]==='💎'){c.style.background='var(--sand)';c.textContent='💎';}else if(l.grid[y][x]==='🪨'){c.style.background='var(--ocean-dark)';c.textContent='🪨';}g.appendChild(c);}}
function addBlock(type,text){workspaceBlocks.push({type,text});renderWorkspace();}
function removeBlock(i){workspaceBlocks.splice(i,1);renderWorkspace();}
function renderWorkspace(){const c=document.getElementById('blocksWorkspace');c.innerHTML='';workspaceBlocks.forEach((b,i)=>{const d=document.createElement('div');d.className='workspace-block';if(b.type==='move')d.style.background='var(--ocean-light)';else if(b.type==='loop'||b.type==='loop-end')d.style.background='var(--fish-purple)';d.innerHTML=`<span>${b.type==='loop'?'🔁 '+b.text+'x [':b.type==='loop-end'?'] Fin':b.text}</span><button class="remove-block" onclick="removeBlock(${i})">×</button>`;c.appendChild(d);});}
function clearBlocks(){workspaceBlocks=[];document.getElementById('blocksWorkspace').innerHTML='';}
async function runBlocks(){const l=blocksLevels[blocksLevel];let pos={...l.start};renderBlocksGrid();await sleep(200);let cmds=[],i=0;while(i<workspaceBlocks.length){const b=workspaceBlocks[i];if(b.type==='loop'){const rep=parseInt(b.text)||2;let lc=[],depth=1;i++;while(i<workspaceBlocks.length&&depth>0){if(workspaceBlocks[i].type==='loop')depth++;else if(workspaceBlocks[i].type==='loop-end')depth--;if(depth>0)lc.push(workspaceBlocks[i]);i++;}for(let r=0;r<rep;r++)cmds.push(...lc);}else if(b.type!=='loop-end'){cmds.push(b);i++;}else i++;}for(const cmd of cmds){if(cmd.type!=='move')continue;let dx=0,dy=0;if(cmd.text==='⬆️')dy=-1;else if(cmd.text==='⬇️')dy=1;else if(cmd.text==='⬅️')dx=-1;else if(cmd.text==='➡️')dx=1;const nx=pos.x+dx,ny=pos.y+dy;if(nx>=0&&nx<l.grid[0].length&&ny>=0&&ny<l.grid.length&&l.grid[ny][nx]!=='🪨'){const old=document.getElementById(`blocks-${pos.x}-${pos.y}`);old.style.background='';old.textContent='';pos={x:nx,y:ny};const nw=document.getElementById(`blocks-${pos.x}-${pos.y}`);nw.style.background='var(--ocean-light)';nw.textContent='🚤';await sleep(300);}}if(pos.x===l.goal.x&&pos.y===l.goal.y){updateScore(30);blocksLevel++;if(blocksLevel<blocksLevels.length)showFeedback(true,'Super!','Suivant!',loadBlocksLevel);else showFeedback(true,'Architecte!','Tu maîtrises!',()=>{blocksLevel=0;loadBlocksLevel();});}else showFeedback(false,'Pas encore...','Vérifie!',null);}

let sensorsLevel=0,sensorsPos={x:0,y:0},sensorsMoves=0;
const sensorsLevels=[{grid:[['S','.','.','💎']],start:{x:0,y:0},goal:{x:3,y:0},mission:"Atteins le trésor ! (pas de piège)"},{grid:[['S','.','🪨','💎'],['.','.','.','.']], start:{x:0,y:0},goal:{x:3,y:0},mission:"Attention au rocher ! Regarde les capteurs."},{grid:[['S','.','🪨','.'],['🪨','.','.','.'],['.','🪨','.','💎']], start:{x:0,y:0},goal:{x:3,y:2},mission:"Labyrinthe ! Les capteurs t'aident."}];
function initSensors(){sensorsLevel=0;loadSensorsLevel();}
function loadSensorsLevel(){const l=sensorsLevels[sensorsLevel];sensorsPos={...l.start};sensorsMoves=0;document.getElementById('sensorsLevel').textContent=sensorsLevel+1;document.getElementById('sensorsMissionText').textContent=l.mission;renderSensorsGrid();updateSensorReadings();}
function renderSensorsGrid(){const l=sensorsLevels[sensorsLevel],g=document.getElementById('sensorsGrid');g.style.gridTemplateColumns=`repeat(${l.grid[0].length},1fr)`;g.innerHTML='';for(let y=0;y<l.grid.length;y++)for(let x=0;x<l.grid[y].length;x++){const c=document.createElement('div');c.className='var-cell';c.id=`sensor-${x}-${y}`;if(x===sensorsPos.x&&y===sensorsPos.y){c.classList.add('sub');c.textContent='🚤';}else if(l.grid[y][x]==='💎'){c.style.background='var(--sand)';c.textContent='💎';}else if(l.grid[y][x]==='🪨'){c.style.background='var(--ocean-dark)';c.textContent='🪨';}g.appendChild(c);}}
function updateSensorReadings(){const l=sensorsLevels[sensorsLevel];[{id:'sensorUp',dx:0,dy:-1},{id:'sensorDown',dx:0,dy:1},{id:'sensorLeft',dx:-1,dy:0},{id:'sensorRight',dx:1,dy:0}].forEach(d=>{const cx=sensorsPos.x+d.dx,cy=sensorsPos.y+d.dy,e=document.getElementById(d.id);if(cx<0||cx>=l.grid[0].length||cy<0||cy>=l.grid.length){e.textContent='Mur';e.className='reading-value blocked';}else if(l.grid[cy][cx]==='🪨'){e.textContent='Rocher!';e.className='reading-value blocked';}else if(l.grid[cy]&&l.grid[cy][cx]==='💎'){e.textContent='Trésor!';e.className='reading-value clear';e.style.background='var(--sand)';}else{e.textContent='Libre';e.className='reading-value clear';e.style.background='';}});}
function moveSensorsManual(dir){const l=sensorsLevels[sensorsLevel];let dx=0,dy=0;if(dir==='up')dy=-1;else if(dir==='down')dy=1;else if(dir==='left')dx=-1;else if(dir==='right')dx=1;const nx=sensorsPos.x+dx,ny=sensorsPos.y+dy;if(nx<0||nx>=l.grid[0].length||ny<0||ny>=l.grid.length){showFeedback(false,'Attention!','Tu sors de la zone !',null);return;}if(l.grid[ny][nx]==='🪨'){showFeedback(false,'Boum!','Tu as foncé dans un rocher ! Regarde les capteurs.',null);sensorsMoves++;return;}const old=document.getElementById(`sensor-${sensorsPos.x}-${sensorsPos.y}`);old.classList.remove('sub');old.textContent='';sensorsPos={x:nx,y:ny};sensorsMoves++;const nw=document.getElementById(`sensor-${sensorsPos.x}-${sensorsPos.y}`);nw.classList.add('sub');nw.textContent='🚤';updateSensorReadings();if(sensorsPos.x===l.goal.x&&sensorsPos.y===l.goal.y){updateScore(30);sensorsLevel++;if(sensorsLevel<sensorsLevels.length)showFeedback(true,'Bravo!','Trésor trouvé en '+sensorsMoves+' coups !',loadSensorsLevel);else showFeedback(true,'Champion!','Tu maîtrises les capteurs !',()=>{sensorsLevel=0;loadSensorsLevel();});}}
function resetSensorsLevel(){loadSensorsLevel();}
window.onload=generateBubbles;
</script>
</body>
</html>
