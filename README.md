<?php
if (isset($_GET['steam_api']) && $_GET['steam_api'] == 1 && isset($_GET['steam_id'])) {
    header('Content-Type: application/json');
    $steam_id = preg_replace('/[^0-9]/', '', $_GET['steam_id']);
    
    $api_key = '5F1E3C30157303F443325231597C2C25';
    if (empty($api_key) || $api_key == 'YOUR_STEAM_API_KEY') {
        echo json_encode(['error' => 'Please fill in a valid Steam API Key in config.']);
        exit;
    }
    $url = "https://api.steampowered.com/ISteamUser/GetPlayerSummaries/v2/?key={$api_key}&steamids={$steam_id}";
    
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
    curl_setopt($ch, CURLOPT_TIMEOUT, 10);
    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    $curlError = curl_error($ch);
    curl_close($ch);
    
    if ($response === false) {
        echo json_encode(['error' => "cURL error: " . $curlError]);
        exit;
    }
    if ($httpCode !== 200) {
        echo json_encode(['error' => "HTTP error code: $httpCode"]);
        exit;
    }
    echo $response;
    exit;
}

$site_title = "MirkCOG";
$avatar_url = "user.jpg";
$main_id = "MirkCOG";

$steam_status = [
    'enable'       => true,
    'api_key'      => '5F1E3C30157303F443325231597C2C25',
    'steam_id'     => '76561199866924921',
    'refresh_sec'  => 15,
];

$uid_nav_list = [
    ['Steam',     'icon',  'fa-brands fa-steam','https://steamcommunity.com/profiles/76561199866924921/'],
    ['YouTube',   'icon',  'fa-brands fa-youtube','https://www.youtube.com/@cogx3469'],
    ['Gamesense', 'image',  '1.png','https://gamesense.pub/forums/profile.php?id=4732'],
    ['Melatonin', 'image', 'melatonin.png','https://melatonin.win/members/mirkcog.666/'],
];

$link_buttons = [
    ['CS2 Setting', 'xx'],
    ['Melatonin Helper', 'xx'],
    ['Melatonin Legit Config', 'xx'],
];

$music = [
    'enable'   => true,
    'file'     => 'Crash Adams - Hotel Party.mp3',
    'cover'    => 'Hotel Party.jfif',
    'title'    => 'Hotel Party',
];
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title id="dynamicTitle">MirkCOG</title>
    <link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
                * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        #starCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: #030510;
            z-index: -1;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            font-family: 'Segoe UI', Arial, sans-serif;
            perspective: 800px;
            overflow-x: hidden;
        }

        .glass-panel {
            max-width: 950px;
            width: 100%;
            background: rgba(20, 22, 27, 0.55);
            backdrop-filter: blur(16px);
            border-radius: 48px;
            border: 1px solid rgba(255, 255, 255, 0.25);
            box-shadow: 0 25px 40px rgba(0, 0, 0, 0.3), inset 0 1px 1px rgba(255, 255, 255, 0.1);
            padding: 35px 25px 45px 25px;
            transition: transform 0.1s ease-out;
            transform-style: preserve-3d;
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        
        .glass-panel > * {
            width: auto;
            max-width: 100%;
            flex-shrink: 0;
        }

        .avatar {
            width: 90px;
            height: 90px;
            border-radius: 50%;
            margin-bottom: 20px;
            object-fit: cover;
            border: 2px solid rgba(255, 255, 255, 0.5);
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .avatar:hover {
            transform: scale(1.05);
            box-shadow: 0 0 18px rgba(255, 255, 255, 0.4);
        }
        .main-id {
            font-size: 3rem;
            font-weight: 900;
            margin-bottom: 25px;
            letter-spacing: 2px;
            background: linear-gradient(135deg, #ffffff, #ccccff, #aaccff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 12px rgba(0, 160, 255, 0.3);
        }
        .steam-card {
            background: rgba(25, 35, 55, 0.75);
            backdrop-filter: blur(14px);
            border-radius: 28px;
            border: 1px solid rgba(255, 255, 255, 0.25);
            padding: 12px 28px;
            margin-bottom: 25px;
            display: inline-flex;
            align-items: center;
            gap: 20px;
            flex-wrap: wrap;
            justify-content: center;
            transition: all 0.25s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        .steam-card:hover {
            background: rgba(45, 60, 85, 0.85);
            border-color: rgba(90, 150, 220, 0.6);
            transform: translateY(-3px);
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.3);
        }
        /* 音乐卡片 - 与 Steam 卡片样式一致 */
        .music-card {
            background: rgba(25, 35, 55, 0.75);
            backdrop-filter: blur(14px);
            border-radius: 28px;
            border: 1px solid rgba(255, 255, 255, 0.25);
            padding: 12px 28px;
            margin-bottom: 25px;
            display: inline-flex;
            align-items: center;
            justify-content: space-between;
            gap: 20px;
            flex-wrap: wrap;
            transition: all 0.25s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            width: auto;
            max-width: 100%;
        }
        .music-card:hover {
            background: rgba(45, 60, 85, 0.85);
            border-color: rgba(90, 150, 220, 0.6);
            transform: translateY(-3px);
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.3);
        }
        .steam-avatar {
            width: 56px;
            height: 56px;
            border-radius: 50%;
            background: #2c2f33;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        .steam-avatar img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            object-fit: cover;
        }
        .online-dot {
            position: absolute;
            bottom: 2px;
            right: 2px;
            width: 14px;
            height: 14px;
            border-radius: 50%;
            border: 2px solid #1e1f22;
        }
        .online-dot.online  { background-color: #23a55a; }
        .online-dot.offline { background-color: #747f8d; }
        .online-dot.ingame  { background-color: #67c1f5; }

        .steam-info {
            text-align: center;
        }
        .steam-name {
            font-size: 1.35rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
        }
        .steam-badge {
            background: #171a21;
            padding: 2px 10px;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: normal;
            color: #67c1f5;
        }
        .steam-status-text {
            font-size: 0.85rem;
            color: #e0e3e8;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            margin-top: 6px;
            line-height: 1;
        }
        .steam-status-text i,
        .steam-status-text span {
            line-height: 1;
        }
        .game-name {
            background: rgba(0, 0, 0, 0.5);
            padding: 4px 12px;
            border-radius: 40px;
            display: inline-block;
            line-height: 1.2;
            vertical-align: middle;
        }
        .error-message {
            color: #ffaaaa;
        }
        .uid-nav-block {
            width: 100%;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            gap: 18px 24px;
            margin-bottom: 30px;
        }
        .uid-item {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            width: 100px;
            padding: 14px 6px;
            border-radius: 20px;
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(4px);
            border: 1px solid rgba(255, 255, 255, 0.15);
            text-decoration: none;
            color: #ffffff;
            transition: all 0.25s cubic-bezier(0.2, 0.9, 0.4, 1.1);
        }
        .uid-item i {
            font-size: 2rem;
            margin-bottom: 8px;
        }
        .uid-img {
            width: 36px;
            height: 36px;
            object-fit: contain;
            margin-bottom: 8px;
        }
        .uid-item span {
            font-size: 0.75rem;
            font-weight: 500;
            opacity: 0.9;
            white-space: nowrap;
        }
        .uid-item:hover {
            background: rgba(255, 255, 255, 0.2);
            border-color: rgba(255, 255, 255, 0.5);
            transform: translateY(-8px);
        }

        /* 文字链接按钮组样式（置于最下方） */
        .link-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 16px;
            margin-top: 10px;
            width: 100%;
        }
        .link-buttons a {
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(4px);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 40px;
            padding: 8px 20px;
            color: #fff;
            text-decoration: none;
            font-size: 0.9rem;
            font-weight: 500;
            transition: all 0.25s ease;
            letter-spacing: 0.5px;
        }
        .link-buttons a:hover {
            background: rgba(255, 255, 255, 0.2);
            border-color: rgba(255, 255, 255, 0.5);
            transform: translateY(-2px);
        }

        /* 音乐内部元素 */
        .music-left {
            display: flex;
            align-items: center;
            gap: 12px;
            flex-shrink: 1;
            min-width: 0;
        }
        .music-mini-cover {
            width: 44px;
            height: 44px;
            border-radius: 12px;
            object-fit: cover;
            background: #2c2f33;
            flex-shrink: 0;
        }
        .music-mini-info {
            text-align: left;
            white-space: nowrap;
        }
        .music-mini-title {
            font-size: 1rem;
            font-weight: 600;
            color: #fff;
            letter-spacing: 0.3px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .music-mini-icon {
            color: #6c8eff;
            font-size: 0.85rem;
        }
        .volume-control-inline {
            display: flex;
            align-items: center;
            gap: 8px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 30px;
            padding: 4px 12px;
            flex-shrink: 0;
        }
        .volume-control-inline i {
            font-size: 0.9rem;
            color: #b8c7ff;
            cursor: pointer;
            width: 24px;
            text-align: center;
        }
        .volume-slider {
            -webkit-appearance: none;
            width: 90px;
            height: 3px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 5px;
            outline: none;
            cursor: pointer;
        }
        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #ffffff;
            border: none;
            box-shadow: 0 0 4px #6c8eff;
            cursor: pointer;
        }
        .volume-percent {
            min-width: 38px;
            text-align: right;
            color: #ddd;
            font-size: 0.7rem;
        }
        @media (max-width: 680px) {
            .main-id {
                font-size: 2rem;
            }
            .steam-card, .music-card {
                padding: 10px 20px;
            }
            .steam-avatar {
                width: 48px;
                height: 48px;
            }
            .uid-item {
                width: 80px;
                padding: 10px 4px;
            }
            .uid-item i {
                font-size: 1.5rem;
            }
            .music-left {
                flex-wrap: wrap;
                justify-content: center;
            }
            .volume-control-inline {
                margin-left: 0;
                margin-top: 6px;
            }
            .volume-slider {
                width: 70px;
            }
            .music-mini-info {
                white-space: normal;
            }
            .link-buttons a {
                padding: 6px 16px;
                font-size: 0.8rem;
            }
        }
    </style>
</head>
<body>

<canvas id="starCanvas"></canvas>

<div class="glass-panel" id="tiltPanel">
    <?php if(!empty($avatar_url)): ?>
        <img src="<?php echo htmlspecialchars($avatar_url); ?>" alt="Avatar" class="avatar"
             onerror="this.onerror=null; this.src='https://via.placeholder.com/90?text=Avatar';">
    <?php endif; ?>

    <h1 class="main-id"><?php echo htmlspecialchars($main_id); ?></h1>

    <?php if($steam_status['enable'] && !empty($steam_status['api_key']) && !empty($steam_status['steam_id'])): ?>
    <div class="steam-card" id="steamCard">
        <div class="steam-avatar">
            <img id="steamAvatar" src="https://steamcdn-a.akamaihd.net/steamcommunity/public/images/avatars/00/00.jpg" alt="Steam Avatar">
            <div id="steamOnlineDot" class="online-dot offline"></div>
        </div>
        <div class="steam-info">
            <div class="steam-name">
                <span id="steamPersonaName">Steam User</span>
                <span class="steam-badge"><i class="fa-brands fa-steam"></i> Steam</span>
            </div>
            <div class="steam-status-text">
                <i class="fa-solid fa-circle-info"></i>
                <span id="steamStatusText" class="game-name">Fetching status...</span>
            </div>
        </div>
    </div>
    <?php endif; ?>

    <?php if($music['enable'] && !empty($music['file'])): ?>
    <div class="music-card">
        <div class="music-left">
            <?php if(!empty($music['cover']) && file_exists($music['cover'])): ?>
                <img src="<?php echo htmlspecialchars($music['cover']); ?>" class="music-mini-cover" alt="cover">
            <?php else: ?>
                <div class="music-mini-cover" style="background: #2c2f33; display: flex; align-items: center; justify-content: center;">
                    <i class="fas fa-music" style="color: #aaa; font-size: 20px;"></i>
                </div>
            <?php endif; ?>
            <div class="music-mini-info">
                <div class="music-mini-title">
                    <i class="fas fa-headphones music-mini-icon"></i>
                    <span><?php echo htmlspecialchars($music['title']); ?></span>
                </div>
            </div>
        </div>
        <div class="volume-control-inline">
            <i class="fas fa-volume-down" id="volumeIcon"></i>
            <input type="range" id="volumeSlider" class="volume-slider" min="0" max="1" step="0.01" value="0.20">
            <span id="volumePercent" class="volume-percent">20%</span>
        </div>
    </div>
    <audio id="hiddenAudio" src="<?php echo htmlspecialchars($music['file']); ?>" loop preload="auto"></audio>
    <?php endif; ?>

    <!-- 原有带图标的社交链接块 -->
    <div class="uid-nav-block">
        <?php foreach($uid_nav_list as $item):
            $name = htmlspecialchars($item[0]);
            $type = $item[1];
            $value = htmlspecialchars($item[2]);
            $link = htmlspecialchars($item[3]);
        ?>
            <a href="<?php echo $link; ?>" target="_blank" rel="noopener noreferrer" class="uid-item">
                <?php if($type === 'icon'): ?>
                    <i class="<?php echo $value; ?>"></i>
                <?php elseif($type === 'image'): ?>
                    <img src="<?php echo $value; ?>" alt="<?php echo $name; ?>" class="uid-img"
                         onerror="this.onerror=null; this.src='https://via.placeholder.com/36?text=Logo';">
                <?php endif; ?>
                <span><?php echo $name; ?></span>
            </a>
        <?php endforeach; ?>
    </div>

    <!-- 纯文字按钮组（最下方） -->
    <div class="link-buttons">
        <?php foreach($link_buttons as $btn): ?>
            <a href="<?php echo htmlspecialchars($btn[1]); ?>" target="_blank" rel="noopener noreferrer"><?php echo htmlspecialchars($btn[0]); ?></a>
        <?php endforeach; ?>
    </div>
</div>

<script>
    (function() {
        const starConfig = {
            starCount: window.innerWidth < 768 ? 80 : 150,
            starMaxSize: 2.2,
            starMinSize: 0.4,
            moveSpeed: 0.22,
            lineDistance: 150,
            mouseFollowRange: 200,
            flickerSpeed: 0.009,
            starColor: '255,255,255',
            lineColor: '210,220,255'
        };
        const canvas = document.getElementById('starCanvas');
        const ctx = canvas.getContext('2d');
        let stars = [];
        let mouseX = null, mouseY = null;

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }

        function initStars() {
            stars = [];
            for(let i = 0; i < starConfig.starCount; i++) {
                stars.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    size: Math.random() * (starConfig.starMaxSize - starConfig.starMinSize) + starConfig.starMinSize,
                    speedX: (Math.random() - 0.5) * starConfig.moveSpeed,
                    speedY: (Math.random() - 0.5) * starConfig.moveSpeed,
                    opacity: Math.random(),
                    flickerDir: Math.random() > 0.5 ? 1 : -1
                });
            }
        }

        function drawStars() {
            ctx.fillStyle = 'rgba(3, 5, 16, 0.12)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            stars.forEach((star, idx) => {
                star.opacity += star.flickerDir * starConfig.flickerSpeed;
                if(star.opacity > 1) star.flickerDir = -1;
                if(star.opacity < 0.2) star.flickerDir = 1;
                star.x += star.speedX;
                star.y += star.speedY;
                if(star.x < 0 || star.x > canvas.width) star.speedX *= -1;
                if(star.y < 0 || star.y > canvas.height) star.speedY *= -1;
                if(mouseX !== null && mouseY !== null) {
                    let dx = star.x - mouseX, dy = star.y - mouseY;
                    let dist = Math.hypot(dx, dy);
                    if(dist < starConfig.mouseFollowRange) {
                        let force = (starConfig.mouseFollowRange - dist) / starConfig.mouseFollowRange;
                        star.x += dx * force * 0.03;
                        star.y += dy * force * 0.03;
                    }
                }
                ctx.beginPath();
                ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2);
                ctx.fillStyle = `rgba(${starConfig.starColor}, ${star.opacity * 0.9})`;
                ctx.fill();
                for(let j = idx + 1; j < stars.length; j++) {
                    let s2 = stars[j];
                    let dx = star.x - s2.x, dy = star.y - s2.y;
                    let dist = Math.hypot(dx, dy);
                    if(dist < starConfig.lineDistance) {
                        let alpha = (1 - dist / starConfig.lineDistance) * 0.25;
                        ctx.beginPath();
                        ctx.moveTo(star.x, star.y);
                        ctx.lineTo(s2.x, s2.y);
                        ctx.strokeStyle = `rgba(${starConfig.lineColor}, ${alpha})`;
                        ctx.lineWidth = 0.6;
                        ctx.stroke();
                    }
                }
            });
            requestAnimationFrame(drawStars);
        }
        window.addEventListener('mousemove', (e) => { mouseX = e.clientX; mouseY = e.clientY; });
        window.addEventListener('mouseout', () => { mouseX = null; mouseY = null; });
        window.addEventListener('resize', () => { resizeCanvas(); initStars(); });
        resizeCanvas();
        initStars();
        drawStars();
    })();

    (function() {
        const fullText = "MirkCOG";
        const typeInterval = 1200;
        const holdTime = 5000;
        let currentIndex = 0;
        let timer = null;
        let isHolding = false;
        const titleElem = document.querySelector('title');
        if (!titleElem) return;

        function stopAnimation() {
            if (timer) {
                clearTimeout(timer);
                timer = null;
            }
        }

        function startTyping() {
            stopAnimation();
            currentIndex = 0;
            isHolding = false;
            titleElem.innerText = "";
            stepTyping();
        }

        function stepTyping() {
            if (isHolding) return;
            if (currentIndex < fullText.length) {
                currentIndex++;
                titleElem.innerText = fullText.substring(0, currentIndex);
                timer = setTimeout(stepTyping, typeInterval);
            } else {
                isHolding = true;
                timer = setTimeout(() => {
                    startTyping();
                }, holdTime);
            }
        }
        startTyping();
    })();

    <?php if($steam_status['enable'] && !empty($steam_status['api_key']) && !empty($steam_status['steam_id'])): ?>
    const STEAM_ID = "<?php echo addslashes($steam_status['steam_id']); ?>";
    const REFRESH_SEC = <?php echo intval($steam_status['refresh_sec']); ?>;

    async function fetchSteamStatus() {
        const statusSpan = document.getElementById('steamStatusText');
        const dotElem = document.getElementById('steamOnlineDot');
        try {
            const response = await fetch('?steam_api=1&steam_id=' + STEAM_ID);
            const data = await response.json();
            if (data.error) {
                statusSpan.innerHTML = '⚠️ ' + data.error;
                statusSpan.classList.add('error-message');
                dotElem.className = 'online-dot offline';
                return;
            }
            statusSpan.classList.remove('error-message');
            if (data.response && data.response.players && data.response.players.length > 0) {
                const player = data.response.players[0];
                const avatarUrl = player.avatarfull || player.avatarmedium || player.avatar;
                if (avatarUrl) document.getElementById('steamAvatar').src = avatarUrl;
                document.getElementById('steamPersonaName').innerText = player.personaname || 'Steam User';
                const personastate = player.personastate;
                const gameextrainfo = player.gameextrainfo || '';

                if (gameextrainfo) {
                    statusSpan.innerHTML = `🎮 Playing ${gameextrainfo}`;
                    dotElem.className = 'online-dot ingame';
                } else {
                    switch(personastate) {
                        case 0: statusSpan.innerHTML = '⚪ Offline'; dotElem.className = 'online-dot offline'; break;
                        case 1: statusSpan.innerHTML = '🟢 Online'; dotElem.className = 'online-dot online'; break;
                        case 2: statusSpan.innerHTML = '🔴 Busy'; dotElem.className = 'online-dot online'; break;
                        case 3: statusSpan.innerHTML = '🌙 Away'; dotElem.className = 'online-dot online'; break;
                        case 4: statusSpan.innerHTML = '😴 Snooze'; dotElem.className = 'online-dot online'; break;
                        default: statusSpan.innerHTML = '💬 Online'; dotElem.className = 'online-dot online';
                    }
                }
            } else {
                statusSpan.innerHTML = '❓ User not found (Check ID or public profile)';
                statusSpan.classList.add('error-message');
                dotElem.className = 'online-dot offline';
            }
        } catch (err) {
            console.error(err);
            statusSpan.innerHTML = '🌐 Network error: cannot reach proxy';
            statusSpan.classList.add('error-message');
            dotElem.className = 'online-dot offline';
        }
    }
    fetchSteamStatus();
    setInterval(fetchSteamStatus, REFRESH_SEC * 1000);
    <?php endif; ?>

    (function() {
        const panel = document.getElementById('tiltPanel');
        if (!panel) return;
        const maxRotateX = 5;
        const maxRotateY = 5;
        
        function handleMouseMove(e) {
            const rect = panel.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            const offsetX = (e.clientX - centerX) / (rect.width / 2);
            const offsetY = (e.clientY - centerY) / (rect.height / 2);
            const clampedX = Math.min(Math.max(offsetX, -1), 1);
            const clampedY = Math.min(Math.max(offsetY, -1), 1);
            const rotY = clampedX * maxRotateY;
            const rotX = -clampedY * maxRotateX;
            panel.style.transform = `rotateX(${rotX}deg) rotateY(${rotY}deg)`;
        }
        
        function resetTilt() {
            panel.style.transform = 'rotateX(0deg) rotateY(0deg)';
        }
        
        document.addEventListener('mousemove', handleMouseMove);
        document.addEventListener('mouseleave', resetTilt);
        window.addEventListener('mouseout', (e) => {
            if (e.relatedTarget === null) resetTilt();
        });
    })();

    <?php if($music['enable'] && !empty($music['file'])): ?>
    (function() {
        const audio = document.getElementById('hiddenAudio');
        const volumeSlider = document.getElementById('volumeSlider');
        const volumePercent = document.getElementById('volumePercent');
        const volumeIcon = document.getElementById('volumeIcon');
        if (!audio) return;

        let savedVolume = localStorage.getItem('musicVolume');
        if (savedVolume !== null && !isNaN(parseFloat(savedVolume))) {
            savedVolume = Math.min(1, Math.max(0, parseFloat(savedVolume)));
        } else {
            savedVolume = 0.20;
        }
        audio.volume = savedVolume;
        if (volumeSlider) volumeSlider.value = savedVolume;
        if (volumePercent) volumePercent.innerText = Math.round(savedVolume * 100) + '%';
        
        function updateVolumeIcon(vol) {
            if (!volumeIcon) return;
            if (vol <= 0) volumeIcon.className = 'fas fa-volume-mute';
            else if (vol < 0.5) volumeIcon.className = 'fas fa-volume-down';
            else volumeIcon.className = 'fas fa-volume-up';
        }
        updateVolumeIcon(savedVolume);

        if (volumeSlider) {
            volumeSlider.addEventListener('input', function(e) {
                let val = parseFloat(e.target.value);
                audio.volume = val;
                if (volumePercent) volumePercent.innerText = Math.round(val * 100) + '%';
                updateVolumeIcon(val);
                localStorage.setItem('musicVolume', val);
            });
        }

        if (volumeIcon) {
            volumeIcon.style.cursor = 'pointer';
            volumeIcon.addEventListener('click', function() {
                if (audio.volume > 0) {
                    localStorage.setItem('musicVolume_beforeMute', audio.volume);
                    audio.volume = 0;
                    if (volumeSlider) volumeSlider.value = 0;
                    if (volumePercent) volumePercent.innerText = '0%';
                    updateVolumeIcon(0);
                } else {
                    let lastVol = localStorage.getItem('musicVolume_beforeMute');
                    let newVol = (lastVol && !isNaN(parseFloat(lastVol))) ? parseFloat(lastVol) : 0.20;
                    audio.volume = newVol;
                    if (volumeSlider) volumeSlider.value = newVol;
                    if (volumePercent) volumePercent.innerText = Math.round(newVol * 100) + '%';
                    updateVolumeIcon(newVol);
                    localStorage.setItem('musicVolume', newVol);
                }
            });
        }
        
        function attemptAutoPlay() {
            audio.play().then(() => {
                console.log('Audio playing with volume:', audio.volume);
            }).catch((err) => {
                console.log('Auto-play blocked, waiting for user interaction');
                const startOnInteraction = function() {
                    audio.play().catch(e => console.warn('Still blocked:', e));
                    document.removeEventListener('click', startOnInteraction);
                    document.removeEventListener('touchstart', startOnInteraction);
                };
                document.addEventListener('click', startOnInteraction);
                document.addEventListener('touchstart', startOnInteraction);
            });
        }
        
        window.addEventListener('load', attemptAutoPlay);
    })();
    <?php endif; ?>
</script>
</body>
</html>
