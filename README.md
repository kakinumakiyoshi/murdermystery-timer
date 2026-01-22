[index.html](https://github.com/user-attachments/files/24796750/index.html)
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>マダミスタイマー</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            width: 100%;
            max-width: 400px;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: 24px;
            padding: 40px 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        h1 {
            color: #fff;
            text-align: center;
            margin-bottom: 30px;
            font-size: 24px;
            font-weight: 300;
            letter-spacing: 2px;
        }

        .timer-display {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 16px;
            padding: 40px 20px;
            margin-bottom: 30px;
            text-align: center;
        }

        .time {
            font-size: 64px;
            font-weight: 700;
            color: #00d4ff;
            font-variant-numeric: tabular-nums;
            text-shadow: 0 0 20px rgba(0, 212, 255, 0.5);
            font-family: 'Arial Black', 'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Yu Gothic', sans-serif;
        }

        .status {
            color: #aaa;
            font-size: 14px;
            margin-top: 10px;
            letter-spacing: 1px;
        }

        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .time-btn {
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.2), rgba(0, 150, 199, 0.2));
            border: 1px solid rgba(0, 212, 255, 0.3);
            color: #00d4ff;
            padding: 20px;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 300;
            cursor: pointer;
            transition: all 0.3s ease;
            letter-spacing: 1px;
        }

        .time-btn:hover {
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.3), rgba(0, 150, 199, 0.3));
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 212, 255, 0.3);
        }

        .time-btn:active {
            transform: translateY(0);
        }

        .control-btns {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .control-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: #fff;
            padding: 16px;
            border-radius: 12px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
            letter-spacing: 1px;
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        .control-btn.stop {
            background: rgba(255, 68, 68, 0.2);
            border-color: rgba(255, 68, 68, 0.3);
            color: #ff6b6b;
        }

        .control-btn.stop:hover {
            background: rgba(255, 68, 68, 0.3);
        }

        .running .timer-display {
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 0 0 rgba(0, 212, 255, 0); }
            50% { box-shadow: 0 0 0 8px rgba(0, 212, 255, 0.1); }
        }

        .finished .time {
            color: #ff6b6b;
            animation: blink 0.5s ease-in-out infinite;
        }

        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }

        .fullscreen-btn {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #888;
            padding: 12px;
            border-radius: 12px;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s ease;
            letter-spacing: 1px;
            margin-top: 12px;
            width: 100%;
        }

        .fullscreen-btn:hover {
            background: rgba(255, 255, 255, 0.1);
            color: #aaa;
        }

        /* Fullscreen mode styles */
        .fullscreen-mode {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            z-index: 9999;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .fullscreen-mode .time {
            font-size: 120px;
            margin-bottom: 20px;
            font-weight: 700;
            font-family: 'Arial Black', 'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Yu Gothic', sans-serif;
        }

        .fullscreen-mode .status {
            font-size: 24px;
            margin-bottom: 40px;
        }

        .exit-fullscreen {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: #fff;
            padding: 16px 32px;
            border-radius: 12px;
            font-size: 18px;
            cursor: pointer;
            transition: all 0.3s ease;
            letter-spacing: 1px;
            margin-top: 40px;
        }

        .exit-fullscreen:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        @media (max-width: 600px) {
            .fullscreen-mode .time {
                font-size: 80px;
            }
            .fullscreen-mode .status {
                font-size: 18px;
            }
        }
    </style>
</head>
<body>
    <div class="container" id="container">
        <h1>TIMER</h1>
        
        <div class="timer-display">
            <div class="time" id="timeDisplay">00:00</div>
            <div class="status" id="status">待機中</div>
        </div>

        <div class="buttons-grid">
            <button class="time-btn" onclick="startTimer(1)">1分</button>
            <button class="time-btn" onclick="startTimer(5)">5分</button>
            <button class="time-btn" onclick="startTimer(15)">15分</button>
            <button class="time-btn" onclick="startTimer(20)">20分</button>
            <button class="time-btn" onclick="startTimer(25)">25分</button>
            <button class="time-btn" onclick="startTimer(30)">30分</button>
        </div>

        <div class="control-btns">
            <button class="control-btn" onclick="pauseTimer()">一時停止</button>
            <button class="control-btn stop" onclick="stopTimer()">リセット</button>
        </div>

        <button class="fullscreen-btn" onclick="toggleFullscreen()">⛶ 全画面表示</button>
        <button class="fullscreen-btn" onclick="testSound()" style="margin-top: 8px; background: rgba(0, 212, 255, 0.1); border-color: rgba(0, 212, 255, 0.3); color: #00d4ff;">🔊 音テスト</button>
    </div>

    <div id="fullscreenDisplay" class="fullscreen-mode" style="display: none;">
        <div class="time" id="fullscreenTime">00:00</div>
        <div class="status" id="fullscreenStatus">待機中</div>
        <button class="exit-fullscreen" onclick="toggleFullscreen()">戻る</button>
    </div>

    <audio id="alarmSound" preload="auto">
        <source src="data:audio/wav;base64,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" type="audio/wav">
    </audio>

    <script>
        let countdown;
        let totalSeconds = 0;
        let isPaused = false;
        let remainingSeconds = 0;
        let isFullscreen = false;

        function startTimer(minutes) {
            stopTimer();
            totalSeconds = minutes * 60;
            remainingSeconds = totalSeconds;
            isPaused = false;
            
            document.getElementById('container').classList.remove('finished');
            document.getElementById('container').classList.add('running');
            document.getElementById('status').textContent = 'カウント中';
            
            updateDisplay();
            runTimer();
        }

        function runTimer() {
            countdown = setInterval(() => {
                if (!isPaused) {
                    remainingSeconds--;
                    updateDisplay();
                    
                    if (remainingSeconds <= 0) {
                        timerFinished();
                    }
                }
            }, 1000);
        }

        function updateDisplay() {
            const minutes = Math.floor(remainingSeconds / 60);
            const seconds = remainingSeconds % 60;
            const timeString = `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
            
            document.getElementById('timeDisplay').textContent = timeString;
            document.getElementById('fullscreenTime').textContent = timeString;
        }

        function pauseTimer() {
            if (countdown && !isPaused) {
                isPaused = true;
                const statusText = '一時停止中';
                document.getElementById('status').textContent = statusText;
                document.getElementById('fullscreenStatus').textContent = statusText;
                document.getElementById('container').classList.remove('running');
                document.getElementById('fullscreenDisplay').classList.remove('running');
            } else if (countdown && isPaused) {
                isPaused = false;
                const statusText = 'カウント中';
                document.getElementById('status').textContent = statusText;
                document.getElementById('fullscreenStatus').textContent = statusText;
                document.getElementById('container').classList.add('running');
                document.getElementById('fullscreenDisplay').classList.add('running');
            }
        }

        function stopTimer() {
            clearInterval(countdown);
            countdown = null;
            isPaused = false;
            remainingSeconds = 0;
            totalSeconds = 0;
            const timeString = '00:00';
            const statusText = '待機中';
            
            document.getElementById('timeDisplay').textContent = timeString;
            document.getElementById('fullscreenTime').textContent = timeString;
            document.getElementById('status').textContent = statusText;
            document.getElementById('fullscreenStatus').textContent = statusText;
            document.getElementById('container').classList.remove('running', 'finished');
            document.getElementById('fullscreenDisplay').classList.remove('running', 'finished');
        }

        function timerFinished() {
            clearInterval(countdown);
            const statusText = '終了！';
            
            document.getElementById('container').classList.remove('running');
            document.getElementById('container').classList.add('finished');
            document.getElementById('fullscreenDisplay').classList.remove('running');
            document.getElementById('fullscreenDisplay').classList.add('finished');
            document.getElementById('status').textContent = statusText;
            document.getElementById('fullscreenStatus').textContent = statusText;
            
            // Play alarm sound
            playAlarm();
            
            if (navigator.vibrate) {
                navigator.vibrate([200, 100, 200, 100, 200, 100, 200, 100, 200]);
            }
        }

        function playAlarm() {
            const audio = document.getElementById('alarmSound');
            audio.volume = 1.0;
            
            // Play the beep 3 times
            let count = 0;
            function playBeep() {
                if (count < 3) {
                    audio.currentTime = 0;
                    audio.play().catch(e => console.log('Play failed:', e));
                    count++;
                    setTimeout(playBeep, 600);
                }
            }
            playBeep();
        }

        // Test sound function
        function testSound() {
            const audio = document.getElementById('alarmSound');
            audio.volume = 1.0;
            audio.currentTime = 0;
            audio.play()
                .then(() => {
                    alert('音が鳴りました！タイマーでも鳴るはずです。');
                })
                .catch(e => {
                    alert('音が鳴りませんでした。iPhoneの音量を確認してください。エラー: ' + e.message);
                });
        }

        // Initialize audio on first interaction
        let audioReady = false;
        document.addEventListener('click', function initAudio() {
            if (!audioReady) {
                const audio = document.getElementById('alarmSound');
                audio.load();
                audioReady = true;
            }
        }, { once: true });

        function toggleFullscreen() {
            isFullscreen = !isFullscreen;
            const fullscreenDisplay = document.getElementById('fullscreenDisplay');
            const container = document.getElementById('container');
            
            if (isFullscreen) {
                fullscreenDisplay.style.display = 'flex';
                container.style.display = 'none';
                
                // Sync status
                document.getElementById('fullscreenStatus').textContent = 
                    document.getElementById('status').textContent;
                
                // Copy running/finished state
                if (document.getElementById('container').classList.contains('running')) {
                    fullscreenDisplay.classList.add('running');
                }
                if (document.getElementById('container').classList.contains('finished')) {
                    fullscreenDisplay.classList.add('finished');
                }
            } else {
                fullscreenDisplay.style.display = 'none';
                container.style.display = 'block';
            }
        }

    </script>
</body>
</html>
