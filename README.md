<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Key Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            width: 100%;
            height: 100vh;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            overflow: hidden;
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Stars Background */
        #starContainer {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            pointer-events: none;
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        /* Main Content */
        .container {
            position: relative;
            z-index: 10;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            padding: 60px 40px;
            border-radius: 20px;
            text-align: center;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
            max-width: 500px;
            width: 90%;
        }

        h1 {
            color: #00d4ff;
            font-size: 2.5em;
            margin-bottom: 30px;
            text-shadow: 0 0 10px rgba(0, 212, 255, 0.5);
        }

        .key-section {
            background: rgba(0, 0, 0, 0.3);
            padding: 25px;
            border-radius: 15px;
            margin: 20px 0;
            border: 2px solid #00d4ff;
        }

        .key-label {
            color: #00d4ff;
            font-size: 0.9em;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 10px;
            opacity: 0.8;
        }

        .key-display {
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid #00d4ff;
            color: #00ff00;
            padding: 15px;
            border-radius: 8px;
            font-family: 'Courier New', monospace;
            font-size: 1.3em;
            letter-spacing: 2px;
            word-break: break-all;
            margin: 10px 0;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .key-display.expired {
            color: #ff4444;
            border-color: #ff4444;
        }

        .status {
            color: #ffaa00;
            font-size: 0.85em;
            margin-top: 10px;
            font-weight: bold;
        }

        .status.active {
            color: #00ff00;
        }

        .status.expired {
            color: #ff4444;
        }

        .timer {
            color: #00d4ff;
            font-size: 0.95em;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid rgba(0, 212, 255, 0.3);
        }

        .timer-label {
            opacity: 0.7;
            font-size: 0.85em;
        }

        .timer-value {
            font-size: 1.2em;
            font-weight: bold;
            font-family: 'Courier New', monospace;
        }

        .daily-text {
            background: rgba(0, 0, 0, 0.3);
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            border-left: 4px solid #00d4ff;
            color: #ffffff;
            font-size: 1.1em;
            line-height: 1.6;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .refresh-btn {
            background: linear-gradient(135deg, #00d4ff, #0099cc);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 8px;
            font-size: 1em;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: bold;
        }

        .refresh-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(0, 212, 255, 0.4);
        }

        .refresh-btn:active {
            transform: translateY(0);
        }

        .warning {
            background: rgba(255, 68, 68, 0.2);
            border: 2px solid #ff4444;
            color: #ff6666;
            padding: 15px;
            border-radius: 8px;
            margin-top: 20px;
            font-weight: bold;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }

        .next-update {
            color: #ffaa00;
            font-size: 0.9em;
            margin-top: 15px;
        }
    </style>
</head>
<body>
    <div id="starContainer"></div>

    <div class="container">
        <h1>🔐 KEY GENERATOR</h1>

        <!-- Current Key Section -->
        <div class="key-section">
            <div class="key-label">Current Key</div>
            <div class="key-display" id="keyDisplay">modderZx-free-LOADING</div>
            <div class="status" id="keyStatus">Generating...</div>
            <div class="timer">
                <div class="timer-label">Key expires in:</div>
                <div class="timer-value" id="expiryTimer">--:--:--</div>
            </div>
        </div>

        <!-- Daily Text Section -->
        <div class="key-section">
            <div class="key-label">Daily Message (Updates Every 24h)</div>
            <div class="daily-text" id="dailyText">Loading daily message...</div>
            <div class="next-update">
                <div class="timer-label">Next update in:</div>
                <div class="timer-value" id="textTimer">--:--:--</div>
            </div>
        </div>

        <!-- Key Refresh Timer -->
        <div class="timer" style="margin-top: 20px;">
            <div class="timer-label">Next key refresh in:</div>
            <div class="timer-value" id="refreshTimer">--:--:--</div>
        </div>

        <!-- Expired Warning -->
        <div id="expiredWarning" style="display: none;">
            <div class="warning">⚠️ KEY EXPIRED ⚠️<br>Please refresh the page to generate a new key</div>
        </div>

        <button class="refresh-btn" onclick="location.reload()">Refresh Page</button>
    </div>

    <script>
        // ===== CONFIGURATION =====
        const KEY_PREFIX = 'modderZx-free-';
        const KEY_VALIDITY_HOURS = 3;
        const KEY_REFRESH_HOURS = 5;
        const TEXT_UPDATE_HOURS = 24;

        // ===== STAR ANIMATION =====
        function createStars() {
            const starContainer = document.getElementById('starContainer');
            const starCount = 100;

            for (let i = 0; i < starCount; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.width = Math.random() * 3 + 'px';
                star.style.height = star.style.width;
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.animationDelay = Math.random() * 3 + 's';
                star.style.animationDuration = (Math.random() * 2 + 2) + 's';
                starContainer.appendChild(star);
            }
        }

        // ===== KEY GENERATION =====
        function generateRandomString(length = 5) {
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let result = '';
            for (let i = 0; i < length; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return result;
        }

        function generateKey() {
            const randomPart = generateRandomString(5);
            return KEY_PREFIX + randomPart;
        }

        // ===== DAILY TEXT ROTATION =====
        const dailyTexts = [
            "Welcome to the world of endless possibilities! 🌟",
            "Every key is unique, just like you! ✨",
            "Your access is secured with advanced encryption. 🔒",
            "Remember to keep your keys safe and secure! 🛡️",
            "Today is a great day to unlock new opportunities! 🚀",
            "Thank you for using our service! 💫",
            "Your key is valid and ready to use! ⚡",
            "Stay secure, stay awesome! 🎯",
            "The future is encrypted! 🔐",
            "You have the power to unlock! 💪"
        ];

        function getDailyText() {
            const today = new Date();
            const dayOfYear = Math.floor((today - new Date(today.getFullYear(), 0, 0)) / 86400000);
            return dailyTexts[dayOfYear % dailyTexts.length];
        }

        // ===== LOCAL STORAGE MANAGEMENT =====
        function initializeKey() {
            const stored = localStorage.getItem('keyData');
            
            if (stored) {
                const data = JSON.parse(stored);
                const createdTime = new Date(data.createdAt).getTime();
                const now = new Date().getTime();
                const elapsedHours = (now - createdTime) / (1000 * 60 * 60);

                if (elapsedHours < KEY_VALIDITY_HOURS) {
                    return data;
                }
            }

            // Generate new key
            const newKey = {
                key: generateKey(),
                createdAt: new Date().toISOString(),
                isExpired: false
            };

            localStorage.setItem('keyData', JSON.stringify(newKey));
            return newKey;
        }

        function getLastTextUpdate() {
            const stored = localStorage.getItem('lastTextUpdate');
            if (stored) {
                return new Date(stored);
            }
            // If no record, set it to 24 hours ago so it shows current text
            const yesterday = new Date(Date.now() - 24 * 60 * 60 * 1000);
            localStorage.setItem('lastTextUpdate', yesterday.toISOString());
            return yesterday;
        }

        // ===== UPDATE DISPLAYS =====
        function updateKeyDisplay() {
            const keyData = JSON.parse(localStorage.getItem('keyData'));
            const createdTime = new Date(keyData.createdAt).getTime();
            const now = new Date().getTime();
            const elapsedMillis = now - createdTime;
            const validityMillis = KEY_VALIDITY_HOURS * 60 * 60 * 1000;

            const keyDisplay = document.getElementById('keyDisplay');
            const keyStatus = document.getElementById('keyStatus');
            const expiredWarning = document.getElementById('expiredWarning');

            if (elapsedMillis > validityMillis) {
                keyDisplay.textContent = keyData.key;
                keyDisplay.classList.add('expired');
                keyStatus.textContent = '❌ EXPIRED';
                keyStatus.classList.remove('active');
                keyStatus.classList.add('expired');
                expiredWarning.style.display = 'block';
                keyData.isExpired = true;
                localStorage.setItem('keyData', JSON.stringify(keyData));
            } else {
                keyDisplay.textContent = keyData.key;
                keyDisplay.classList.remove('expired');
                keyStatus.textContent = '✅ ACTIVE';
                keyStatus.classList.add('active');
                keyStatus.classList.remove('expired');
                expiredWarning.style.display = 'none';
            }

            updateExpiryTimer();
        }

        function updateExpiryTimer() {
            const keyData = JSON.parse(localStorage.getItem('keyData'));
            const createdTime = new Date(keyData.createdAt).getTime();
            const expiryTime = createdTime + (KEY_VALIDITY_HOURS * 60 * 60 * 1000);
            const now = new Date().getTime();
            const remaining = Math.max(0, expiryTime - now);

            const hours = Math.floor(remaining / (1000 * 60 * 60));
            const minutes = Math.floor((remaining % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((remaining % (1000 * 60)) / 1000);

            document.getElementById('expiryTimer').textContent = 
                `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
        }

        function updateRefreshTimer() {
            const keyData = JSON.parse(localStorage.getItem('keyData'));
            const createdTime = new Date(keyData.createdAt).getTime();
            const refreshTime = createdTime + (KEY_REFRESH_HOURS * 60 * 60 * 1000);
            const now = new Date().getTime();
            const remaining = Math.max(0, refreshTime - now);

            const hours = Math.floor(remaining / (1000 * 60 * 60));
            const minutes = Math.floor((remaining % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((remaining % (1000 * 60)) / 1000);

            document.getElementById('refreshTimer').textContent = 
                `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
        }

        function updateDailyText() {
            const lastUpdate = getLastTextUpdate();
            const now = new Date();
            const elapsedMillis = now - lastUpdate;
            const updateIntervalMillis = TEXT_UPDATE_HOURS * 60 * 60 * 1000;

            if (elapsedMillis > updateIntervalMillis) {
                localStorage.setItem('lastTextUpdate', now.toISOString());
            }

            document.getElementById('dailyText').textContent = getDailyText();
            updateTextTimer();
        }

        function updateTextTimer() {
            const lastUpdate = new Date(localStorage.getItem('lastTextUpdate'));
            const nextUpdate = new Date(lastUpdate.getTime() + (TEXT_UPDATE_HOURS * 60 * 60 * 1000));
            const now = new Date();
            const remaining = Math.max(0, nextUpdate - now);

            const hours = Math.floor(remaining / (1000 * 60 * 60));
            const minutes = Math.floor((remaining % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((remaining % (1000 * 60)) / 1000);

            document.getElementById('textTimer').textContent = 
                `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
        }

        // ===== INITIALIZATION =====
        document.addEventListener('DOMContentLoaded', function() {
            createStars();
            initializeKey();
            updateKeyDisplay();
            updateRefreshTimer();
            updateDailyText();

            // Update timers every second
            setInterval(updateExpiryTimer, 1000);
            setInterval(updateRefreshTimer, 1000);
            setInterval(updateTextTimer, 1000);
            setInterval(updateKeyDisplay, 1000); // Check if key expired

            // Update key every 5 hours
            setInterval(() => {
                initializeKey();
                updateKeyDisplay();
                updateRefreshTimer();
            }, KEY_REFRESH_HOURS * 60 * 60 * 1000);

            // Update text every 24 hours
            setInterval(() => {
                updateDailyText();
            }, TEXT_UPDATE_HOURS * 60 * 60 * 1000);
        });
    </script>
</body>
</html># ModderZx-get-key
ModderZx ative get key 3hr
