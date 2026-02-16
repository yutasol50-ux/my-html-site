<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#14F195">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="暗号通貨ダッシュボード">
    <link rel="manifest" href="manifest.json">
    <title>暗号通貨 & 経済指標ダッシュボード</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #e8ecf1 100%);
            color: #2c3e50;
            min-height: 100vh;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 10px;
            overflow-x: hidden;
        }

        @media (max-width: 768px) {
            .container {
                padding: 10px 5px;
            }
        }

        header {
            text-align: center;
            padding: 30px 0;
            background: white;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }

        h1 {
            font-size: 2.5em;
            background: linear-gradient(90deg, #27ae60 0%, #2ecc71 50%, #27ae60 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 10px;
            animation: shimmer 3s ease-in-out infinite;
            background-size: 200% auto;
        }

        @keyframes shimmer {
            0%, 100% { background-position: 0% center; }
            50% { background-position: 100% center; }
        }

        .subtitle {
            color: #7f8c8d;
            font-size: 1.1em;
        }

        .nav-buttons {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin-bottom: 30px;
        }

        .nav-btn {
            padding: 15px 40px;
            font-size: 1.2em;
            border: 2px solid #27ae60;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
            background: white;
            color: #27ae60;
            box-shadow: 0 2px 8px rgba(39, 174, 96, 0.2);
        }

        .nav-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(39, 174, 96, 0.3);
            background: #f0fdf4;
        }

        .nav-btn.active {
            background: #27ae60;
            color: white;
            box-shadow: 0 4px 12px rgba(39, 174, 96, 0.4);
        }

        .page {
            display: none;
        }

        .page.active {
            display: block;
            animation: fadeIn 0.5s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Calendar Page - Google Calendar Style */
        .calendar-container {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
            overflow-x: hidden;
        }

        .calendar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .calendar-header h2 {
            font-size: 1.5em;
            color: #2c3e50;
        }

        .calendar-controls {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .calendar-controls button {
            padding: 8px 16px;
            background: white;
            border: 2px solid #27ae60;
            border-radius: 8px;
            color: #27ae60;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 0.9em;
            font-weight: 500;
        }

        .calendar-controls button:hover {
            background: #27ae60;
            color: white;
        }

        /* Timezone info banner */
        .timezone-info {
            background: linear-gradient(135deg, #e8f5e9, #c8e6c9);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            border-left: 4px solid #27ae60;
        }

        .timezone-info h3 {
            color: #27ae60;
            font-size: 1em;
            margin-bottom: 8px;
        }

        .timezone-examples {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 10px;
            font-size: 0.9em;
            color: #2c3e50;
        }

        .timezone-example {
            background: rgba(255, 255, 255, 0.5);
            padding: 8px 12px;
            border-radius: 6px;
        }

        /* Calendar Grid View (Top) */
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
            margin-bottom: 30px;
        }

        .calendar-day-header {
            text-align: center;
            padding: 10px 5px;
            font-weight: bold;
            color: #7f8c8d;
            font-size: 0.9em;
        }

        .calendar-day-cell {
            aspect-ratio: 1;
            background: #f8f9fa;
            border-radius: 8px;
            padding: 8px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            cursor: pointer;
            transition: all 0.2s;
            position: relative;
            border: 1px solid #e0e0e0;
        }

        .calendar-day-cell:hover {
            background: #e8f5e9;
            border-color: #27ae60;
        }

        .calendar-day-cell.today {
            background: #e8f5e9;
            border: 2px solid #27ae60;
            box-shadow: 0 0 0 3px rgba(39, 174, 96, 0.1);
        }

        .calendar-day-cell.other-month {
            opacity: 0.3;
        }

        .calendar-day-number {
            font-size: 0.95em;
            font-weight: 500;
            margin-bottom: 4px;
            color: #2c3e50;
        }

        .calendar-day-cell.today .calendar-day-number {
            color: #27ae60;
            font-weight: bold;
        }

        .event-dots {
            display: flex;
            gap: 3px;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: auto;
        }

        .event-dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
        }

        /* Calendar List View (Bottom) */
        .calendar-events-section {
            border-top: 2px solid #e0e0e0;
            padding-top: 20px;
        }

        .calendar-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .calendar-day-item {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 15px;
            transition: all 0.3s;
            border: 1px solid #e0e0e0;
        }

        .calendar-day-item:hover {
            background: #e8f5e9;
            border-color: #27ae60;
        }

        .day-header {
            display: flex;
            align-items: baseline;
            gap: 10px;
            margin-bottom: 12px;
        }

        .day-number-large {
            font-size: 2em;
            font-weight: bold;
            color: #27ae60;
            min-width: 50px;
        }

        .day-info {
            display: flex;
            flex-direction: column;
        }

        .day-weekday {
            font-size: 0.9em;
            color: #7f8c8d;
        }

        .day-date-full {
            font-size: 0.85em;
            color: #95a5a6;
        }

        .events-list {
            display: flex;
            flex-direction: column;
            gap: 8px;
            margin-left: 60px;
        }

        .event-item {
            padding: 12px;
            border-radius: 8px;
            font-size: 0.95em;
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            transition: all 0.2s;
            border-left: 4px solid;
            background: white;
        }

        .event-item:hover {
            transform: translateX(3px);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }

        .event-title {
            flex: 1;
            font-weight: 500;
            color: #2c3e50;
        }

        .event-time {
            font-size: 0.85em;
            color: #7f8c8d;
            background: #f0f0f0;
            padding: 4px 8px;
            border-radius: 4px;
        }

        .event-impact-badge {
            padding: 4px 10px;
            border-radius: 12px;
            font-size: 0.75em;
            font-weight: bold;
        }

        .event-impact-badge.high {
            background: #fee;
            color: #e74c3c;
        }

        .event-impact-badge.medium {
            background: #fff3cd;
            color: #f39c12;
        }

        .color-picker-modal {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(26, 15, 46, 0.98);
            padding: 20px;
            border-radius: 15px;
            border: 2px solid #14F195;
            z-index: 1000;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
            max-width: 300px;
            width: 90%;
        }

        .color-picker-modal.active {
            display: block;
        }

        .color-options {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-top: 15px;
        }

        .color-option {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            border: 3px solid transparent;
            transition: all 0.2s;
        }

        .color-option:hover {
            transform: scale(1.2);
            border-color: white;
        }

        .modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            z-index: 999;
        }

        .modal-overlay.active {
            display: block;
        }

        @media (max-width: 768px) {
            .calendar-header h2 {
                font-size: 1.2em;
            }
            
            .calendar-controls button {
                padding: 6px 12px;
                font-size: 0.85em;
            }

            .calendar-day-cell {
                padding: 5px;
            }

            .calendar-day-number {
                font-size: 0.85em;
            }

            .event-dot {
                width: 4px;
                height: 4px;
            }

            .events-list {
                margin-left: 0;
            }

            .day-number-large {
                font-size: 1.5em;
                min-width: 40px;
            }
        }

        /* Token Price Page */
        .tokens-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }

        @media (max-width: 768px) {
            .tokens-grid {
                grid-template-columns: 1fr;
                gap: 15px;
            }
        }

        .token-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s;
            cursor: pointer;
        }

        @media (max-width: 768px) {
            .token-card {
                padding: 15px;
            }
        }

        .token-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(20, 241, 149, 0.3);
            border-color: #14F195;
        }

        .token-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .token-name {
            font-size: 1.5em;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .token-symbol {
            font-size: 0.7em;
            color: #888;
        }

        .token-price {
            font-size: 2em;
            font-weight: bold;
            color: #14F195;
            margin-bottom: 10px;
            text-shadow: 0 0 10px rgba(20, 241, 149, 0.3);
        }

        .price-change {
            font-size: 1.1em;
            padding: 5px 10px;
            border-radius: 5px;
            display: inline-block;
        }

        .price-change.positive {
            background: rgba(34, 197, 94, 0.2);
            color: #22c55e;
        }

        .price-change.negative {
            background: rgba(239, 68, 68, 0.2);
            color: #ef4444;
        }

        .metrics {
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .metric-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 8px;
        }

        .metric-label {
            color: #888;
            font-size: 0.95em;
        }

        .metric-value {
            font-weight: bold;
            font-size: 1.1em;
        }

        .metric-value.positive {
            color: #22c55e;
        }

        .metric-value.negative {
            color: #ef4444;
        }

        .vix-indicator {
            background: linear-gradient(135deg, rgba(153, 69, 255, 0.2), rgba(220, 31, 255, 0.2));
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
            border-left: 4px solid #9945FF;
        }

        .vix-label {
            font-size: 0.9em;
            color: #DC1FFF;
            margin-bottom: 5px;
        }

        .vix-value {
            font-size: 1.8em;
            font-weight: bold;
            color: #9945FF;
        }

        .loading {
            text-align: center;
            padding: 40px;
            font-size: 1.2em;
            color: #888;
        }

        .refresh-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            padding: 15px 25px;
            background: linear-gradient(135deg, #14F195, #9945FF);
            border: none;
            border-radius: 50px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(20, 241, 149, 0.4);
            transition: all 0.3s;
        }

        .refresh-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(153, 69, 255, 0.6);
        }

        .last-update {
            text-align: center;
            color: #888;
            margin-top: 20px;
            font-size: 0.9em;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>📊 暗号通貨 & 経済指標ダッシュボード</h1>
            <p class="subtitle">リアルタイム価格・資金フロー・VIX指数を一元管理</p>
            
            <!-- Central Bank Rates -->
            <div style="display: flex; justify-content: center; gap: 30px; margin-top: 20px; flex-wrap: wrap;">
                <div style="background: rgba(255,255,255,0.05); padding: 15px 25px; border-radius: 10px; border-left: 4px solid #14F195;">
                    <div style="font-size: 0.9em; color: #888; margin-bottom: 5px;">🇺🇸 FRB政策金利</div>
                    <div style="font-size: 1.8em; font-weight: bold; color: #14F195;" id="fedRate">4.50%</div>
                    <div style="font-size: 0.75em; color: #666; margin-top: 3px;">更新: 2026/01</div>
                </div>
                <div style="background: rgba(255,255,255,0.05); padding: 15px 25px; border-radius: 10px; border-left: 4px solid #9945FF;">
                    <div style="font-size: 0.9em; color: #888; margin-bottom: 5px;">🇯🇵 日銀政策金利</div>
                    <div style="font-size: 1.8em; font-weight: bold; color: #9945FF;" id="bojRate">0.75%</div>
                    <div style="font-size: 0.75em; color: #666; margin-top: 3px;">更新: 2026/01</div>
                </div>
            </div>
        </header>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showPage('calendar', event)">📅 経済カレンダー</button>
            <button class="nav-btn" onclick="showPage('tokens', event)">💰 トークン価格</button>
            <button class="nav-btn" onclick="showPage('news', event)">📰 ニュース</button>
        </div>

        <!-- Calendar Page -->
        <div id="calendar" class="page active">
            <div class="calendar-container">
                <div class="calendar-header">
                    <h2 id="currentMonth"></h2>
                    <div class="calendar-controls">
                        <button onclick="changeMonth(-1)">← 前月</button>
                        <button onclick="changeMonth(0)">今月</button>
                        <button onclick="changeMonth(1)">次月 →</button>
                    </div>
                </div>
                
                <!-- Timezone Info -->
                <div class="timezone-info">
                    <h3>🕐 タイムゾーン変換 (米国東部時間 → 日本時間)</h3>
                    <div class="timezone-examples">
                        <div class="timezone-example">米国 08:30 → 日本 22:30</div>
                        <div class="timezone-example">米国 14:00 → 日本 04:00 (翌日)</div>
                        <div class="timezone-example">米国 16:00 → 日本 06:00 (翌日)</div>
                    </div>
                </div>
                
                <!-- Grid View (Top) -->
                <div class="calendar-grid" id="calendarGrid"></div>
                
                <!-- List View (Bottom) -->
                <div class="calendar-events-section">
                    <div class="calendar-list" id="calendarList"></div>
                </div>
            </div>
        </div>

        <!-- Color Picker Modal -->
        <div class="modal-overlay" id="modalOverlay" onclick="closeColorPicker()"></div>
        <div class="color-picker-modal" id="colorPickerModal">
            <h3 id="colorPickerTitle" style="margin-bottom: 10px; color: #14F195;">色を選択</h3>
            <div class="color-options">
                <div class="color-option" style="background: #DC1FFF;" onclick="selectColor('#DC1FFF')"></div>
                <div class="color-option" style="background: #9945FF;" onclick="selectColor('#9945FF')"></div>
                <div class="color-option" style="background: #14F195;" onclick="selectColor('#14F195')"></div>
                <div class="color-option" style="background: #F5576C;" onclick="selectColor('#F5576C')"></div>
                <div class="color-option" style="background: #FFA500;" onclick="selectColor('#FFA500')"></div>
                <div class="color-option" style="background: #00D4FF;" onclick="selectColor('#00D4FF')"></div>
                <div class="color-option" style="background: #FFD700;" onclick="selectColor('#FFD700')"></div>
                <div class="color-option" style="background: #FF6B9D;" onclick="selectColor('#FF6B9D')"></div>
            </div>
            <button onclick="closeColorPicker()" style="margin-top: 15px; width: 100%; padding: 10px; background: rgba(255,255,255,0.1); border: 1px solid #14F195; border-radius: 8px; color: white; cursor: pointer;">閉じる</button>
        </div>

        <!-- Token Price Page -->
        <div id="tokens" class="page">
            <div class="tokens-grid" id="tokensGrid">
                <div class="loading">データを読み込み中...</div>
            </div>
            <div class="last-update" id="lastUpdate"></div>
        </div>

        <!-- News Page -->
        <div id="news" class="page">
            <div style="background: white; border-radius: 15px; padding: 20px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);">
                <h2 style="color: #2c3e50; margin-bottom: 20px; border-bottom: 3px solid #27ae60; padding-bottom: 10px;">📰 暗号通貨・経済ニュース</h2>
                
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; margin-bottom: 30px;">
                    <a href="https://coinpost.jp/" target="_blank" style="text-decoration: none;">
                        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border: 2px solid #e0e0e0; transition: all 0.3s; cursor: pointer;">
                            <h3 style="color: #27ae60; margin-bottom: 10px;">📱 CoinPost</h3>
                            <p style="color: #7f8c8d; font-size: 0.9em;">日本最大級の暗号通貨メディア</p>
                        </div>
                    </a>
                    
                    <a href="https://jp.cointelegraph.com/" target="_blank" style="text-decoration: none;">
                        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border: 2px solid #e0e0e0; transition: all 0.3s; cursor: pointer;">
                            <h3 style="color: #27ae60; margin-bottom: 10px;">🌐 CoinTelegraph</h3>
                            <p style="color: #7f8c8d; font-size: 0.9em;">グローバル暗号通貨ニュース</p>
                        </div>
                    </a>
                    
                    <a href="https://www.bloomberg.co.jp/markets" target="_blank" style="text-decoration: none;">
                        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border: 2px solid #e0e0e0; transition: all 0.3s; cursor: pointer;">
                            <h3 style="color: #27ae60; margin-bottom: 10px;">💼 Bloomberg</h3>
                            <p style="color: #7f8c8d; font-size: 0.9em;">金融市場・経済ニュース</p>
                        </div>
                    </a>
                    
                    <a href="https://www.nikkei.com/markets/" target="_blank" style="text-decoration: none;">
                        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border: 2px solid #e0e0e0; transition: all 0.3s; cursor: pointer;">
                            <h3 style="color: #27ae60; margin-bottom: 10px;">📊 日本経済新聞</h3>
                            <p style="color: #7f8c8d; font-size: 0.9em;">マーケット情報</p>
                        </div>
                    </a>
                    
                    <a href="https://cryptonews.com/" target="_blank" style="text-decoration: none;">
                        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border: 2px solid #e0e0e0; transition: all 0.3s; cursor: pointer;">
                            <h3 style="color: #27ae60; margin-bottom: 10px;">🔥 Cryptonews</h3>
                            <p style="color: #7f8c8d; font-size: 0.9em;">Latest Crypto News</p>
                        </div>
                    </a>
                    
                    <a href="https://www.coindesk.com/" target="_blank" style="text-decoration: none;">
                        <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border: 2px solid #e0e0e0; transition: all 0.3s; cursor: pointer;">
                            <h3 style="color: #27ae60; margin-bottom: 10px;">📈 CoinDesk</h3>
                            <p style="color: #7f8c8d; font-size: 0.9em;">Digital Currency News</p>
                        </div>
                    </a>
                </div>
                
                <div style="background: #e8f5e9; padding: 15px; border-radius: 10px; border-left: 4px solid #27ae60;">
                    <p style="color: #2c3e50; font-size: 0.9em; margin: 0;">💡 ヒント: 各リンクをタップすると新しいタブで開きます</p>
                </div>
            </div>
        </div>

        <button class="refresh-btn" onclick="refreshData()">🔄 更新</button>
    </div>

    <script>
        // Current page state
        let currentPage = 'calendar';
        let currentDate = new Date();

        // Sample economic events (abbreviated) with colors and times - 2026 Full Year Calendar
        // Time format: "US_ET" (Eastern Time), auto-converted to JST
        const economicEvents = {
            // February 2026
            '2026-02-18': [
                { title: 'FOMC議事録', impact: 'high', color: '#e74c3c', time: '14:00', timeJST: '04:00+1' },
                { title: '住宅着工', impact: 'medium', color: '#f39c12', time: '08:30', timeJST: '22:30' }
            ],
            '2026-02-19': [
                { title: '小売売上', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' },
                { title: 'PPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-02-20': [
                { title: '失業保険', impact: 'medium', color: '#f39c12', time: '08:30', timeJST: '22:30' },
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-02-25': [
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' },
                { title: 'PCE', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-02-26': [
                { title: '耐久財', impact: 'medium', color: '#f39c12', time: '08:30', timeJST: '22:30' }
            ],
            '2026-02-27': [
                { title: 'PMI', impact: 'medium', color: '#27ae60', time: '09:45', timeJST: '23:45' },
                { title: '新規失業', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // March 2026
            '2026-03-06': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-03-12': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-03-18': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' }
            ],
            '2026-03-26': [
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // April 2026
            '2026-04-03': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-04-10': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-04-29': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' }
            ],
            
            // May 2026
            '2026-05-01': [
                { title: 'PMI', impact: 'medium', color: '#27ae60', time: '09:45', timeJST: '23:45' }
            ],
            '2026-05-08': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-05-13': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-05-28': [
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // June 2026
            '2026-06-05': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-06-10': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-06-17': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' }
            ],
            
            // July 2026
            '2026-07-02': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-07-11': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-07-29': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' },
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // August 2026
            '2026-08-07': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-08-12': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // September 2026
            '2026-09-04': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-09-10': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-09-16': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' }
            ],
            '2026-09-30': [
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // October 2026
            '2026-10-02': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-10-13': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-10-28': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' }
            ],
            '2026-10-29': [
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // November 2026
            '2026-11-06': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-11-12': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            
            // December 2026
            '2026-12-04': [
                { title: '雇用統計', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-12-10': [
                { title: 'CPI', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ],
            '2026-12-16': [
                { title: 'FOMC', impact: 'high', color: '#c0392b', time: '14:00', timeJST: '04:00+1' }
            ],
            '2026-12-22': [
                { title: 'GDP', impact: 'high', color: '#e74c3c', time: '08:30', timeJST: '22:30' }
            ]
        };

        // Available colors for event customization
        const eventColors = [
            '#DC1FFF', '#9945FF', '#14F195', '#F5576C', 
            '#FFA500', '#00D4FF', '#FFD700', '#FF6B9D'
        ];

        // Central Bank Interest Rates (manually updated)
        let centralBankRates = {
            fed: {
                rate: 4.50,
                lastUpdate: '2026-01',
                description: 'Federal Funds Rate'
            },
            boj: {
                rate: 0.75,
                lastUpdate: '2026-01',
                description: 'Uncollateralized Overnight Call Rate'
            }
        };

        // Function to update interest rates display
        function updateInterestRates() {
            document.getElementById('fedRate').textContent = `${centralBankRates.fed.rate.toFixed(2)}%`;
            document.getElementById('bojRate').textContent = `${centralBankRates.boj.rate.toFixed(2)}%`;
        }

        // Token configuration with CoinGecko IDs
        const tokenConfig = [
            {
                symbol: 'BTC',
                name: 'Bitcoin',
                coinGeckoId: 'bitcoin'
            },
            {
                symbol: 'ETH',
                name: 'Ethereum',
                coinGeckoId: 'ethereum'
            },
            {
                symbol: 'SOL',
                name: 'Solana',
                coinGeckoId: 'solana'
            },
            {
                symbol: 'SKR',
                name: 'Seeker',
                coinGeckoId: 'seeker'
            }
        ];

        let tokens = [];

        // Fetch real-time token data from CoinGecko with historical comparisons
        async function fetchTokenData() {
            try {
                const ids = tokenConfig.map(t => t.coinGeckoId).join(',');
                
                // Fetch current prices
                const currentResponse = await fetch(
                    `https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=${ids}&order=market_cap_desc&per_page=10&page=1&sparkline=true&price_change_percentage=24h,7d,30d,200d,1y`
                );
                
                if (!currentResponse.ok) {
                    console.error('API request failed:', currentResponse.status);
                    throw new Error('API request failed');
                }

                const currentData = await currentResponse.json();
                console.log('Fetched data:', currentData);
                
                tokens = currentData.map(coin => {
                    const config = tokenConfig.find(t => t.coinGeckoId === coin.id);
                    
                    if (!config) {
                        console.warn('No config found for:', coin.id);
                        return null;
                    }
                    
                    // Calculate simulated fund flows based on volume
                    const volume24h = coin.total_volume || 0;
                    const priceChange = coin.price_change_percentage_24h || 0;
                    
                    // Estimate inflow/outflow based on volume and price movement
                    let inflow, outflow;
                    if (priceChange > 0) {
                        inflow = volume24h * 0.6;
                        outflow = volume24h * 0.4;
                    } else {
                        inflow = volume24h * 0.4;
                        outflow = volume24h * 0.6;
                    }
                    
                    // Calculate VIX-like volatility indicator from sparkline
                    let volatility = Math.abs(priceChange) * 2 + 15;
                    if (coin.sparkline_in_7d && coin.sparkline_in_7d.price) {
                        const prices = coin.sparkline_in_7d.price;
                        const priceStdDev = calculateStdDev(prices);
                        const avgPrice = prices.reduce((a, b) => a + b, 0) / prices.length;
                        volatility = (priceStdDev / avgPrice) * 100 * 10;
                    }
                    
                    return {
                        symbol: config.symbol,
                        name: config.name,
                        price: coin.current_price,
                        change24h: coin.price_change_percentage_24h || 0,
                        change7d: coin.price_change_percentage_7d_in_currency || 0,
                        change30d: coin.price_change_percentage_30d_in_currency || 0,
                        change6m: coin.price_change_percentage_200d_in_currency || 0,
                        change1y: coin.price_change_percentage_1y_in_currency || 0,
                        inflow: inflow,
                        outflow: outflow,
                        vix: Math.min(50, volatility),
                        volume24h: volume24h,
                        marketCap: coin.market_cap,
                        high24h: coin.high_24h,
                        low24h: coin.low_24h
                    };
                }).filter(t => t !== null);

                console.log('Processed tokens:', tokens);
                return true;
            } catch (error) {
                console.error('Error fetching token data:', error);
                
                // Fallback to demo data if API fails
                tokens = [
                    {
                        symbol: 'BTC',
                        name: 'Bitcoin',
                        price: 96234.50,
                        change24h: 2.34,
                        change7d: 5.12,
                        change30d: -3.45,
                        change6m: 15.23,
                        change1y: 45.67,
                        inflow: 1250000000,
                        outflow: 890000000,
                        vix: 18.5,
                        volume24h: 2140000000
                    },
                    {
                        symbol: 'ETH',
                        name: 'Ethereum',
                        price: 2845.20,
                        change24h: -1.23,
                        change7d: 3.45,
                        change30d: -5.67,
                        change6m: 12.34,
                        change1y: 38.91,
                        inflow: 456000000,
                        outflow: 523000000,
                        vix: 22.3,
                        volume24h: 979000000
                    },
                    {
                        symbol: 'SOL',
                        name: 'Solana',
                        price: 142.85,
                        change24h: 5.67,
                        change7d: 8.23,
                        change30d: -2.11,
                        change6m: 25.45,
                        change1y: 89.12,
                        inflow: 89000000,
                        outflow: 45000000,
                        vix: 28.7,
                        volume24h: 134000000
                    }
                ];
                
                console.log('Using fallback data');
                return false;
            }
        }

        // Helper function to calculate standard deviation
        function calculateStdDev(values) {
            const avg = values.reduce((a, b) => a + b, 0) / values.length;
            const squareDiffs = values.map(value => Math.pow(value - avg, 2));
            const avgSquareDiff = squareDiffs.reduce((a, b) => a + b, 0) / squareDiffs.length;
            return Math.sqrt(avgSquareDiff);
        }

        // Show page
        function showPage(pageName, event) {
            document.querySelectorAll('.page').forEach(page => {
                page.classList.remove('active');
            });
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            document.getElementById(pageName).classList.add('active');
            if (event && event.target) {
                event.target.classList.add('active');
            }
            currentPage = pageName;

            if (pageName === 'calendar') {
                renderCalendar();
            } else if (pageName === 'tokens') {
                // Show loading state
                document.getElementById('tokensGrid').innerHTML = '<div class="loading">最新データを取得中...</div>';
                
                // Fetch and render
                fetchTokenData().then(() => {
                    renderTokens();
                });
            }
        }

        // Calendar functions
        function renderCalendar() {
            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();
            
            document.getElementById('currentMonth').textContent = 
                `${year}年 ${month + 1}月`;

            const firstDay = new Date(year, month, 1);
            const lastDay = new Date(year, month + 1, 0);
            const daysInMonth = lastDay.getDate();
            const startingDayOfWeek = firstDay.getDay();

            const today = new Date();
            const weekdays = ['日', '月', '火', '水', '木', '金', '土'];

            // Render Grid View (Top)
            const calendarGrid = document.getElementById('calendarGrid');
            calendarGrid.innerHTML = '';

            // Day headers
            weekdays.forEach(day => {
                const header = document.createElement('div');
                header.className = 'calendar-day-header';
                header.textContent = day;
                calendarGrid.appendChild(header);
            });

            // Empty cells before first day
            for (let i = 0; i < startingDayOfWeek; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day-cell other-month';
                calendarGrid.appendChild(emptyDay);
            }

            // Days in month
            for (let day = 1; day <= daysInMonth; day++) {
                const date = new Date(year, month, day);
                const dateString = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                const events = economicEvents[dateString] || [];
                
                const isToday = year === today.getFullYear() && 
                               month === today.getMonth() && 
                               day === today.getDate();

                const dayCell = document.createElement('div');
                dayCell.className = `calendar-day-cell ${isToday ? 'today' : ''}`;
                
                const dayNumber = document.createElement('div');
                dayNumber.className = 'calendar-day-number';
                dayNumber.textContent = day;
                dayCell.appendChild(dayNumber);

                // Add event dots
                if (events.length > 0) {
                    const dotsContainer = document.createElement('div');
                    dotsContainer.className = 'event-dots';
                    events.slice(0, 3).forEach(event => {
                        const dot = document.createElement('div');
                        dot.className = 'event-dot';
                        dot.style.background = event.color;
                        dotsContainer.appendChild(dot);
                    });
                    dayCell.appendChild(dotsContainer);
                }

                calendarGrid.appendChild(dayCell);
            }

            // Render List View (Bottom) - Only days with events
            const calendarList = document.getElementById('calendarList');
            calendarList.innerHTML = '';

            for (let day = 1; day <= daysInMonth; day++) {
                const date = new Date(year, month, day);
                const dateString = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                const weekday = weekdays[date.getDay()];
                const events = economicEvents[dateString] || [];
                
                // Only show days with events
                if (events.length === 0) continue;

                const dayItem = document.createElement('div');
                dayItem.className = 'calendar-day-item';

                let eventsHTML = '<div class="events-list">';
                events.forEach((event, index) => {
                    const timeDisplay = event.timeJST ? `<div class="event-time">米${event.time} / 日${event.timeJST}</div>` : '';
                    eventsHTML += `
                        <div class="event-item" onclick="openColorPicker('${dateString}', ${index})" 
                             style="background: ${event.color}20; border-left-color: ${event.color};">
                            <div class="event-title">${event.title}</div>
                            ${timeDisplay}
                            <div class="event-impact-badge ${event.impact}">${event.impact === 'high' ? '高' : '中'}</div>
                        </div>
                    `;
                });
                eventsHTML += '</div>';

                dayItem.innerHTML = `
                    <div class="day-header">
                        <div class="day-number-large">${day}</div>
                        <div class="day-info">
                            <div class="day-weekday">${weekday}曜日</div>
                            <div class="day-date-full">${year}年${month + 1}月${day}日</div>
                        </div>
                    </div>
                    ${eventsHTML}
                `;

                calendarList.appendChild(dayItem);
            }
        }

        // Color picker functionality
        let selectedEventDate = null;
        let selectedEventIndex = null;

        function openColorPicker(dateString, eventIndex) {
            selectedEventDate = dateString;
            selectedEventIndex = eventIndex;
            
            const modal = document.getElementById('colorPickerModal');
            const overlay = document.getElementById('modalOverlay');
            
            modal.classList.add('active');
            overlay.classList.add('active');
            
            document.getElementById('colorPickerTitle').textContent = 
                `色を選択: ${economicEvents[dateString][eventIndex].title}`;
        }

        function closeColorPicker() {
            document.getElementById('colorPickerModal').classList.remove('active');
            document.getElementById('modalOverlay').classList.remove('active');
        }

        function selectColor(color) {
            if (selectedEventDate && selectedEventIndex !== null) {
                economicEvents[selectedEventDate][selectedEventIndex].color = color;
                renderCalendar();
                closeColorPicker();
            }
        }

        function changeMonth(delta) {
            if (delta === 0) {
                currentDate = new Date();
            } else {
                currentDate.setMonth(currentDate.getMonth() + delta);
            }
            renderCalendar();
        }

        // Token functions
        function renderTokens() {
            const tokensGrid = document.getElementById('tokensGrid');
            tokensGrid.innerHTML = '';

            tokens.forEach(token => {
                const netFlow = token.inflow - token.outflow;
                const flowPercentage = token.inflow > 0 ? ((netFlow / token.inflow) * 100).toFixed(2) : 0;

                // Format price based on token (SKR gets 4 decimals, others get 2)
                let priceDisplay;
                if (token.symbol === 'SKR') {
                    priceDisplay = `$${token.price.toLocaleString('en-US', {minimumFractionDigits: 4, maximumFractionDigits: 4})}`;
                } else if (token.price < 1) {
                    priceDisplay = `$${token.price.toLocaleString('en-US', {minimumFractionDigits: 4, maximumFractionDigits: 6})}`;
                } else {
                    priceDisplay = `$${token.price.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`;
                }

                const card = document.createElement('div');
                card.className = 'token-card';
                card.innerHTML = `
                    <div class="token-header">
                        <div class="token-name">
                            ${token.name}
                            <span class="token-symbol">${token.symbol}</span>
                        </div>
                    </div>
                    <div class="token-price">${priceDisplay}</div>
                    
                    <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; margin: 15px 0;">
                        <div class="price-change ${token.change24h >= 0 ? 'positive' : 'negative'}" style="font-size: 0.9em; padding: 5px;">
                            24h: ${token.change24h >= 0 ? '▲' : '▼'} ${Math.abs(token.change24h).toFixed(2)}%
                        </div>
                        <div class="price-change ${token.change7d >= 0 ? 'positive' : 'negative'}" style="font-size: 0.9em; padding: 5px;">
                            7d: ${token.change7d >= 0 ? '▲' : '▼'} ${Math.abs(token.change7d).toFixed(2)}%
                        </div>
                        <div class="price-change ${token.change30d >= 0 ? 'positive' : 'negative'}" style="font-size: 0.9em; padding: 5px;">
                            30d: ${token.change30d >= 0 ? '▲' : '▼'} ${Math.abs(token.change30d).toFixed(2)}%
                        </div>
                        <div class="price-change ${token.change6m >= 0 ? 'positive' : 'negative'}" style="font-size: 0.9em; padding: 5px;">
                            6m: ${token.change6m >= 0 ? '▲' : '▼'} ${Math.abs(token.change6m).toFixed(2)}%
                        </div>
                        <div class="price-change ${token.change1y >= 0 ? 'positive' : 'negative'}" style="font-size: 0.9em; padding: 5px; grid-column: span 2;">
                            1y: ${token.change1y >= 0 ? '▲' : '▼'} ${Math.abs(token.change1y).toFixed(2)}%
                        </div>
                    </div>
                    
                    <div class="metrics">
                        ${token.high24h ? `
                        <div class="metric-row">
                            <span class="metric-label">📈 24h高値</span>
                            <span class="metric-value">$${token.high24h.toLocaleString('en-US', {minimumFractionDigits: 2})}</span>
                        </div>
                        <div class="metric-row">
                            <span class="metric-label">📉 24h安値</span>
                            <span class="metric-value">$${token.low24h.toLocaleString('en-US', {minimumFractionDigits: 2})}</span>
                        </div>
                        ` : ''}
                        <div class="metric-row">
                            <span class="metric-label">💰 24h取引量</span>
                            <span class="metric-value">$${(token.volume24h / 1000000000).toFixed(2)}B</span>
                        </div>
                        <div class="metric-row">
                            <span class="metric-label">💵 資金流入（推定）</span>
                            <span class="metric-value positive">$${(token.inflow / 1000000).toFixed(1)}M</span>
                        </div>
                        <div class="metric-row">
                            <span class="metric-label">💸 資金流出（推定）</span>
                            <span class="metric-value negative">$${(token.outflow / 1000000).toFixed(1)}M</span>
                        </div>
                        <div class="metric-row">
                            <span class="metric-label">📊 純資金フロー</span>
                            <span class="metric-value ${netFlow >= 0 ? 'positive' : 'negative'}">
                                ${netFlow >= 0 ? '+' : ''}$${(netFlow / 1000000).toFixed(1)}M (${flowPercentage >= 0 ? '+' : ''}${flowPercentage}%)
                            </span>
                        </div>
                        ${token.marketCap ? `
                        <div class="metric-row">
                            <span class="metric-label">🏦 時価総額</span>
                            <span class="metric-value">$${(token.marketCap / 1000000000).toFixed(1)}B</span>
                        </div>
                        ` : ''}
                    </div>

                    <div class="vix-indicator">
                        <div class="vix-label">📈 ボラティリティ指数</div>
                        <div class="vix-value">${token.vix.toFixed(1)}</div>
                    </div>
                `;
                tokensGrid.appendChild(card);
            });

            updateLastUpdateTime();
        }

        function updateLastUpdateTime() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('ja-JP');
            document.getElementById('lastUpdate').textContent = 
                `最終更新: ${timeString}`;
        }

        async function refreshData() {
            const btn = document.querySelector('.refresh-btn');
            btn.textContent = '🔄 更新中...';
            btn.disabled = true;

            await fetchTokenData();

            if (currentPage === 'tokens') {
                renderTokens();
            }

            // Show refresh feedback
            btn.textContent = '✓ 更新完了';
            setTimeout(() => {
                btn.textContent = '🔄 更新';
                btn.disabled = false;
            }, 1500);
        }

        // Notification functionality
        async function requestNotificationPermission() {
            if ('Notification' in window && Notification.permission === 'default') {
                const permission = await Notification.requestPermission();
                if (permission === 'granted') {
                    console.log('通知が許可されました');
                    checkUpcomingEvents();
                }
            }
        }

        function checkUpcomingEvents() {
            if (Notification.permission !== 'granted') return;

            const tomorrow = new Date();
            tomorrow.setDate(tomorrow.getDate() + 1);
            const tomorrowStr = tomorrow.toISOString().split('T')[0];

            const upcomingEvents = economicEvents[tomorrowStr];
            if (upcomingEvents && upcomingEvents.length > 0) {
                upcomingEvents.forEach(event => {
                    if (event.impact === 'high') {
                        new Notification(`📊 明日の重要指標`, {
                            body: `${event.title} ${event.timeJST ? `日本時間 ${event.timeJST}` : ''}`,
                            icon: '/icon-192.png',
                            badge: '/icon-192.png'
                        });
                    }
                });
            }
        }

        // Initialize
        renderCalendar();
        
        // Request notification permission
        setTimeout(() => {
            requestNotificationPermission();
        }, 2000);

        // Check for upcoming events daily
        setInterval(checkUpcomingEvents, 86400000); // Every 24 hours
        
        // Fetch initial token data
        (async () => {
            await fetchTokenData();
            if (currentPage === 'tokens') {
                renderTokens();
            }
        })();

        // Auto-refresh every 60 seconds
        setInterval(async () => {
            if (currentPage === 'tokens') {
                await fetchTokenData();
                renderTokens();
            }
        }, 60000);

        // Register service worker for PWA
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('sw.js')
                .then(reg => console.log('Service Worker registered'))
                .catch(err => console.log('Service Worker registration failed'));
        }
    </script>
</body>
</html>
