<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sports Fantasy Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html, body {
            overflow: hidden;
            height: 100vh;
            width: 100vw;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        body::before {
            content: '';
            position: fixed;
            left: 0;
            bottom: 0;
            width: 280px;
            height: 280px;
            background-image: url('https://cdn.prod.website-files.com/64b5f8bfc12b3ec8aef889d7/657b15a8ce62dd978dff60df_demon.png');
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
            opacity: 0.85;
            z-index: 0;
            pointer-events: none;
        }

        body::after {
            content: '';
            position: fixed;
            right: 0;
            bottom: 0;
            width: 280px;
            height: 280px;
            background-image: url('https://cdn.prod.website-files.com/64b5f8bfc12b3ec8aef889d7/657b15e705a1737153b425b3_happy-little-gobby.png');
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
            opacity: 0.85;
            z-index: 0;
            pointer-events: none;
        }

        .main-container {
            display: flex;
            gap: 12px;
            position: fixed;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            z-index: 1;
            height: 90vh;
            width: 1050px;
            flex-direction: column;
        }

        .header {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin-bottom: 15px;
        }

        .header p {
            color: rgba(255, 255, 255, 0.95);
            font-size: 28px;
            font-weight: 600;
            margin: 0;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
            letter-spacing: 0.5px;
        }

        .columns-wrapper {
            display: flex;
            gap: 12px;
            height: 100%;
        }

        .left-column {
            width: 300px;
            height: 100%;
            overflow-y: auto;
        }

        .middle-column {
            width: 450px;
            height: 100%;
            overflow-y: auto;
        }

        .right-column {
            width: 300px;
            height: 100%;
            overflow-y: auto;
        }

        .container {
            background: white;
            border-radius: 8px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
            padding: 20px;
            height: fit-content;
            max-height: 90vh;
            overflow-y: auto;
        }

        .middle-column .container {
            max-height: none;
            overflow-y: visible;
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .middle-column .container.show {
            display: block;
            opacity: 1;
        }

        .breakdown-header {
            color: #667eea;
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 15px;
            text-align: center;
            border-bottom: 2px solid #667eea;
            padding-bottom: 12px;
        }

        .breakdown-item {
            display: flex;
            justify-content: space-between;
            font-size: 13px;
            color: #333;
            margin-bottom: 10px;
            padding-bottom: 10px;
            border-bottom: 1px solid #e0e0e0;
        }

        .breakdown-item:last-of-type {
            border-bottom: none;
            margin-bottom: 12px;
            padding-bottom: 0;
        }

        .breakdown-label {
            font-weight: 500;
            flex: 1;
            font-size: 13px;
        }

        .breakdown-calc {
            color: #999;
            font-size: 11px;
            margin: 0 8px;
        }

        .breakdown-value {
            font-weight: 700;
            color: #667eea;
            min-width: 45px;
            text-align: right;
            font-size: 13px;
        }

        .breakdown-value.negative {
            color: #dc3545;
        }

        .total-score-section {
            background-color: #e8f5e9;
            padding: 12px;
            border-left: 4px solid #28a745;
            border-radius: 4px;
            margin-top: 12px;
        }

        .total-score-label {
            color: #333;
            font-weight: 600;
            font-size: 13px;
            margin-bottom: 6px;
        }

        .total-score-value {
            color: #28a745;
            font-weight: 700;
            font-size: 28px;
        }

        .copy-feedback {
            color: #28a745;
            font-size: 11px;
            text-align: center;
            font-weight: 600;
            margin-top: 8px;
            display: none;
        }

        .copy-feedback.show {
            display: block;
        }

        .logo-header {
            display: flex;
            justify-content: center;
            margin-bottom: 15px;
        }

        .logo-header img {
            max-width: 100%;
            height: auto;
            max-height: 50px;
        }

        .form-group {
            margin-bottom: 18px;
        }

        .radio-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .radio-item {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        input[type="radio"] {
            cursor: pointer;
            width: 16px;
            height: 16px;
            accent-color: #667eea;
        }

        .radio-item label {
            margin: 0;
            font-weight: 400;
            cursor: pointer;
            color: #666;
            font-size: 16px;
        }

        .message-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .message h3 {
            color: #667eea;
            font-size: 17px;
            margin: 0;
            font-weight: 700;
        }

        .reset-btn {
            padding: 6px 10px;
            background-color: #667eea;
            color: white;
            border: none;
            border-radius: 4px;
            font-size: 11px;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s;
            white-space: nowrap;
        }

        .reset-btn:hover {
            background-color: #5568d3;
        }

        .reset-btn:active {
            background-color: #4552b8;
        }

        .stat-row {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 10px;
        }

        .stat-label {
            flex: 1;
            color: #333;
            font-size: 13px;
            font-weight: 500;
        }

        .stat-value {
            color: #999;
            font-size: 12px;
            min-width: 60px;
            text-align: center;
            font-weight: 500;
        }

        .stat-input {
            width: 85px;
            padding: 7px;
            border: 2px solid #e0e0e0;
            border-radius: 4px;
            font-size: 13px;
            color: #333;
        }

        .stat-input:focus {
            outline: none;
            border-color: #667eea;
        }

        .stat-row:last-child {
            margin-bottom: 0;
        }

        .negative .stat-value {
            color: #dc3545;
        }

        .radio-group-baseball,
        .radio-group-football {
            display: flex;
            flex-direction: column;
            gap: 8px;
            margin-bottom: 15px;
        }

        .radio-item-baseball,
        .radio-item-football {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        input[type="radio"].baseball-type,
        input[type="radio"].football-type {
            cursor: pointer;
            width: 16px;
            height: 16px;
            accent-color: #667eea;
        }

        .radio-item-baseball label,
        .radio-item-football label {
            margin: 0;
            font-weight: 400;
            cursor: pointer;
            color: #666;
            font-size: 15px;
        }

        .action-btn {
            width: 100%;
            padding: 10px;
            background-color: #667eea;
            color: white;
            border: none;
            border-radius: 4px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s;
            margin-top: 15px;
        }

        .action-btn:hover {
            background-color: #5568d3;
        }

        .action-btn:active {
            background-color: #4552b8;
        }

        .hitter-stats,
        .pitcher-stats,
        .mma-stats,
        .tennis-stats,
        .football-offensive-stats,
        .football-kicker-stats {
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .hitter-stats.show,
        .pitcher-stats.show,
        .mma-stats.show,
        .tennis-stats.show,
        .football-offensive-stats.show,
        .football-kicker-stats.show {
            display: block;
            opacity: 1;
        }

        .stats-container {
            min-height: auto;
            position: relative;
        }

        .results-empty {
            color: #999;
            font-size: 13px;
            text-align: center;
            padding: 30px 15px;
        }

        @media (max-width: 1200px) {
            .main-container {
                position: static;
                transform: none;
                flex-direction: column;
                align-items: center;
                justify-content: center;
                height: auto;
                width: 100%;
            }

            .columns-wrapper {
                flex-direction: column;
                align-items: center;
            }

            .left-column,
            .middle-column,
            .right-column {
                width: 100%;
                max-width: 100%;
                height: auto;
                overflow-y: visible;
            }

            html, body {
                overflow: auto;
                height: auto;
                width: auto;
            }
        }
    </style>
</head>
<body>
    <div class="main-container">
        <!-- Header -->
        <div class="header">
            <p>Fantasy Score Calculator</p>
        </div>

        <!-- Columns Wrapper -->
        <div class="columns-wrapper">
            <!-- LEFT COLUMN: Sports Selection -->
            <div class="left-column">
                <div class="container">
                    <div class="logo-header">
                        <img src="https://cdn.prod.website-files.com/64b5f8bfc12b3ec8aef889d7/68a89edaf3b822b2116ea1f7_prizepicks_full-logo.svg" alt="PrizePicks Logo">
                    </div>

                    <form id="sportsForm">
                        <div class="form-group">
                            <div class="radio-group">
                                <div class="radio-item">
                                    <input type="radio" id="radio-basketball" name="favorite-sport" value="basketball">
                                    <label for="radio-basketball">🏀 Basketball</label>
                                </div>
                                <div class="radio-item">
                                    <input type="radio" id="radio-baseball" name="favorite-sport" value="baseball">
                                    <label for="radio-baseball">⚾ Baseball</label>
                                </div>
                                <div class="radio-item">
                                    <input type="radio" id="radio-tennis" name="favorite-sport" value="tennis">
                                    <label for="radio-tennis">🎾 Tennis</label>
                                </div>
                                <div class="radio-item">
                                    <input type="radio" id="radio-mma" name="favorite-sport" value="mma">
                                    <label for="radio-mma">🥊 MMA</label>
                                </div>
                                <div class="radio-item">
                                    <input type="radio" id="radio-football" name="favorite-sport" value="football">
                                    <label for="radio-football">🏈 Football</label>
                                </div>
                            </div>
                        </div>
                    </form>
                </div>
            </div>

            <!-- MIDDLE COLUMN: Fantasy Score Forms -->
            <div class="middle-column">
                <!-- Basketball -->
                <div class="container" id="basketballMessage">
                    <div class="message-header">
                        <h3>Basketball Fantasy Score</h3>
                        <button type="button" class="reset-btn" id="resetBtn">🔄 Reset</button>
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">Points = 1.0</span>
                        <span class="stat-value">1.0</span>
                        <input type="number" class="stat-input" data-multiplier="1.0" data-name="Points" placeholder="">
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">Rebounds = 1.2</span>
                        <span class="stat-value">1.2</span>
                        <input type="number" class="stat-input" data-multiplier="1.2" data-name="Rebounds" placeholder="">
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">Assists = 1.5</span>
                        <span class="stat-value">1.5</span>
                        <input type="number" class="stat-input" data-multiplier="1.5" data-name="Assists" placeholder="">
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">Steals = 3.0</span>
                        <span class="stat-value">3.0</span>
                        <input type="number" class="stat-input" data-multiplier="3.0" data-name="Steals" placeholder="">
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">Blocks = 3.0</span>
                        <span class="stat-value">3.0</span>
                        <input type="number" class="stat-input" data-multiplier="3.0" data-name="Blocks" placeholder="">
                    </div>
                    <div class="stat-row negative">
                        <span class="stat-label">Turnovers = -1.0</span>
                        <span class="stat-value">-1.0</span>
                        <input type="number" class="stat-input" data-multiplier="-1.0" data-name="Turnovers" placeholder="">
                    </div>

                    <button type="button" class="action-btn" id="actionBtn">👁️ Show & Copy Result</button>
                </div>

                <!-- Baseball -->
                <div class="container" id="baseballMessage">
                    <div class="message-header">
                        <h3>Baseball Fantasy Score</h3>
                        <button type="button" class="reset-btn" id="baseballResetBtn">🔄 Reset</button>
                    </div>
                    <div class="radio-group-baseball">
                        <div class="radio-item-baseball">
                            <input type="radio" id="radio-hitter" name="baseball-type" value="hitter" class="baseball-type">
                            <label for="radio-hitter">Hitter/Batter</label>
                        </div>
                        <div class="radio-item-baseball">
                            <input type="radio" id="radio-pitcher" name="baseball-type" value="pitcher" class="baseball-type">
                            <label for="radio-pitcher">Pitcher</label>
                        </div>
                    </div>

                    <div class="stats-container">
                        <!-- Hitter Stats -->
                        <div class="hitter-stats" id="hitterStats">
                            <div class="stat-row">
                                <span class="stat-label">Single = 3</span>
                                <span class="stat-value">3</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="3" data-name="Single" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Double = 5</span>
                                <span class="stat-value">5</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="5" data-name="Double" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Triple = 8</span>
                                <span class="stat-value">8</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="8" data-name="Triple" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Home Run = 10</span>
                                <span class="stat-value">10</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="10" data-name="Home Run" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Run = 2</span>
                                <span class="stat-value">2</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="2" data-name="Run" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">RBI = 2</span>
                                <span class="stat-value">2</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="2" data-name="RBI" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Walk (BB) = 2</span>
                                <span class="stat-value">2</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="2" data-name="Walk (BB)" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Hit By Pitch (HBP) = 2</span>
                                <span class="stat-value">2</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="2" data-name="Hit By Pitch (HBP)" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Stolen Base = 5</span>
                                <span class="stat-value">5</span>
                                <input type="number" class="stat-input hitter-input" data-multiplier="5" data-name="Stolen Base" placeholder="">
                            </div>

                            <button type="button" class="action-btn" id="hitterActionBtn">👁️ Show & Copy Result</button>
                        </div>

                        <!-- Pitcher Stats -->
                        <div class="pitcher-stats" id="pitcherStats">
                            <div class="stat-row">
                                <span class="stat-label">Win = 6</span>
                                <span class="stat-value">6</span>
                                <input type="number" class="stat-input pitcher-input" data-multiplier="6" data-name="Win" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Quality Start = 4</span>
                                <span class="stat-value">4</span>
                                <input type="number" class="stat-input pitcher-input" data-multiplier="4" data-name="Quality Start" placeholder="">
                            </div>
                            <div class="stat-row negative">
                                <span class="stat-label">Earned Run = -3</span>
                                <span class="stat-value">-3</span>
                                <input type="number" class="stat-input pitcher-input" data-multiplier="-3" data-name="Earned Run" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Strikeout = 3</span>
                                <span class="stat-value">3</span>
                                <input type="number" class="stat-input pitcher-input" data-multiplier="3" data-name="Strikeout" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Out = 1</span>
                                <span class="stat-value">1</span>
                                <input type="number" class="stat-input pitcher-input" data-multiplier="1" data-name="Out" placeholder="">
                            </div>

                            <button type="button" class="action-btn" id="pitcherActionBtn">👁️ Show & Copy Result</button>
                        </div>
                    </div>
                </div>

                <!-- Tennis -->
                <div class="container" id="tennisMessage">
                    <div class="message-header">
                        <h3>Tennis Fantasy Score</h3>
                        <button type="button" class="reset-btn" id="tennisResetBtn">🔄 Reset</button>
                    </div>

                    <div class="tennis-stats" id="tennisStats">
                        <div class="stat-row">
                            <span class="stat-label">Match Played = 10</span>
                            <span class="stat-value">10</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="10" data-name="Match Played" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Game Win = 1</span>
                            <span class="stat-value">1</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="1" data-name="Game Win" placeholder="">
                        </div>
                        <div class="stat-row negative">
                            <span class="stat-label">Game Loss = -1</span>
                            <span class="stat-value">-1</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="-1" data-name="Game Loss" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Set Won = 3</span>
                            <span class="stat-value">3</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="3" data-name="Set Won" placeholder="">
                        </div>
                        <div class="stat-row negative">
                            <span class="stat-label">Set Loss = -3</span>
                            <span class="stat-value">-3</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="-3" data-name="Set Loss" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Ace = 0.5</span>
                            <span class="stat-value">0.5</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="0.5" data-name="Ace" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Double Fault = 0.5</span>
                            <span class="stat-value">0.5</span>
                            <input type="number" class="stat-input tennis-input" data-multiplier="0.5" data-name="Double Fault" placeholder="">
                        </div>

                        <button type="button" class="action-btn" id="tennisActionBtn">👁️ Show & Copy Result</button>
                    </div>
                </div>

                <!-- MMA -->
                <div class="container" id="mmaMessage">
                    <div class="message-header">
                        <h3>MMA Fantasy Score</h3>
                        <button type="button" class="reset-btn" id="mmaResetBtn">🔄 Reset</button>
                    </div>

                    <div class="mma-stats" id="mmaStats">
                        <div class="stat-row">
                            <span class="stat-label">Significant Strikes = 0.5</span>
                            <span class="stat-value">0.5</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="0.5" data-name="Significant Strikes" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Takedowns = 1.5</span>
                            <span class="stat-value">1.5</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="1.5" data-name="Takedowns" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Knockdowns = 2</span>
                            <span class="stat-value">2</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="2" data-name="Knockdowns" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Control Time (sec) = 0.1</span>
                            <span class="stat-value">0.1</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="0.1" data-name="Control Time" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Win Bonus = 10</span>
                            <span class="stat-value">10</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="10" data-name="Win Bonus" placeholder="">
                        </div>
                        <div class="stat-row">
                            <span class="stat-label">Performance Bonus = 5</span>
                            <span class="stat-value">5</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="5" data-name="Performance Bonus" placeholder="">
                        </div>
                        <div class="stat-row negative">
                            <span class="stat-label">Penalties = -2</span>
                            <span class="stat-value">-2</span>
                            <input type="number" class="stat-input mma-input" data-multiplier="-2" data-name="Penalties" placeholder="">
                        </div>

                        <button type="button" class="action-btn" id="mmaActionBtn">👁️ Show & Copy Result</button>
                    </div>
                </div>

                <!-- Football -->
                <div class="container" id="footballMessage">
                    <div class="message-header">
                        <h3>Football Fantasy Score</h3>
                        <button type="button" class="reset-btn" id="footballResetBtn">🔄 Reset</button>
                    </div>
                    <div class="radio-group-football">
                        <div class="radio-item-football">
                            <input type="radio" id="radio-offensive" name="football-type" value="offensive" class="football-type">
                            <label for="radio-offensive">Offensive Player</label>
                        </div>
                        <div class="radio-item-football">
                            <input type="radio" id="radio-kicker" name="football-type" value="kicker" class="football-type">
                            <label for="radio-kicker">Kicker</label>
                        </div>
                    </div>

                    <div class="stats-container">
                        <!-- Offensive Stats -->
                        <div class="football-offensive-stats" id="footballOffensiveStats">
                            <div class="stat-row">
                                <span class="stat-label">Passing Yards = 0.04</span>
                                <span class="stat-value">0.04</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="0.04" data-name="Passing Yards" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Passing TDs = 4</span>
                                <span class="stat-value">4</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="4" data-name="Passing TDs" placeholder="">
                            </div>
                            <div class="stat-row negative">
                                <span class="stat-label">Interceptions = -1</span>
                                <span class="stat-value">-1</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="-1" data-name="Interceptions" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Rushing Yards = 0.1</span>
                                <span class="stat-value">0.1</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="0.1" data-name="Rushing Yards" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Rushing TDs = 6</span>
                                <span class="stat-value">6</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="6" data-name="Rushing TDs" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Receptions = 1</span>
                                <span class="stat-value">1</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="1" data-name="Receptions" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Receiving Yards = 0.1</span>
                                <span class="stat-value">0.1</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="0.1" data-name="Receiving Yards" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Receiving TDs = 6</span>
                                <span class="stat-value">6</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="6" data-name="Receiving TDs" placeholder="">
                            </div>
                            <div class="stat-row negative">
                                <span class="stat-label">Fumbles Lost = -1</span>
                                <span class="stat-value">-1</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="-1" data-name="Fumbles Lost" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">2-Pt Conversion = 2</span>
                                <span class="stat-value">2</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="2" data-name="2-Point Conversions" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Fumble Recovery TD = 6</span>
                                <span class="stat-value">6</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="6" data-name="Offensive Fumble Recovery TDs" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">Return TDs = 6</span>
                                <span class="stat-value">6</span>
                                <input type="number" class="stat-input football-offensive-input" data-multiplier="6" data-name="Kick/Punt/Field Goal Return TDs" placeholder="">
                            </div>

                            <button type="button" class="action-btn" id="footballOffensiveActionBtn">👁️ Show & Copy Result</button>
                        </div>

                        <!-- Kicker Stats -->
                        <div class="football-kicker-stats" id="footballKickerStats">
                            <div class="stat-row">
                                <span class="stat-label">FG 0-39 yards = 3</span>
                                <span class="stat-value">3</span>
                                <input type="number" class="stat-input football-kicker-input" data-multiplier="3" data-name="Field Goals (0–39 yards)" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">FG 40-49 yards = 4</span>
                                <span class="stat-value">4</span>
                                <input type="number" class="stat-input football-kicker-input" data-multiplier="4" data-name="Field Goals (40–49 yards)" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">FG 50+ yards = 5</span>
                                <span class="stat-value">5</span>
                                <input type="number" class="stat-input football-kicker-input" data-multiplier="5" data-name="Field Goals (50+ yards)" placeholder="">
                            </div>
                            <div class="stat-row">
                                <span class="stat-label">PAT Made = 1</span>
                                <span class="stat-value">1</span>
                                <input type="number" class="stat-input football-kicker-input" data-multiplier="1" data-name="PAT Made" placeholder="">
                            </div>
                            <div class="stat-row negative">
                                <span class="stat-label">Missed FG = -1</span>
                                <span class="stat-value">-1</span>
                                <input type="number" class="stat-input football-kicker-input" data-multiplier="-1" data-name="Missed FG" placeholder="">
                            </div>
                            <div class="stat-row negative">
                                <span class="stat-label">Missed PAT = -1</span>
                                <span class="stat-value">-1</span>
                                <input type="number" class="stat-input football-kicker-input" data-multiplier="-1" data-name="Missed PAT" placeholder="">
                            </div>

                            <button type="button" class="action-btn" id="footballKickerActionBtn">👁️ Show & Copy Result</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- RIGHT COLUMN: Results Display -->
            <div class="right-column" id="resultsModal" style="display: none;">
                <div class="container">
                    <div id="resultsContent"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const radioButtons = document.querySelectorAll('input[name="favorite-sport"]');
        const basketballMessage = document.getElementById('basketballMessage');
        const baseballMessage = document.getElementById('baseballMessage');
        const tennisMessage = document.getElementById('tennisMessage');
        const mmaMessage = document.getElementById('mmaMessage');
        const footballMessage = document.getElementById('footballMessage');
        const resultsModal = document.getElementById('resultsModal');
        const resultsContent = document.getElementById('resultsContent');
        
        const statInputs = document.querySelectorAll('.stat-input:not(.hitter-input):not(.pitcher-input):not(.mma-input):not(.tennis-input):not(.football-offensive-input):not(.football-kicker-input)');
        const hitterInputs = document.querySelectorAll('.hitter-input');
        const pitcherInputs = document.querySelectorAll('.pitcher-input');
        const mmaInputs = document.querySelectorAll('.mma-input');
        const tennisInputs = document.querySelectorAll('.tennis-input');
        const footballOffensiveInputs = document.querySelectorAll('.football-offensive-input');
        const footballKickerInputs = document.querySelectorAll('.football-kicker-input');

        const resetBtn = document.getElementById('resetBtn');
        const baseballResetBtn = document.getElementById('baseballResetBtn');
        const mmaResetBtn = document.getElementById('mmaResetBtn');
        const tennisResetBtn = document.getElementById('tennisResetBtn');
        const footballResetBtn = document.getElementById('footballResetBtn');
        const actionBtn = document.getElementById('actionBtn');
        const hitterRadio = document.getElementById('radio-hitter');
        const pitcherRadio = document.getElementById('radio-pitcher');
        const offensiveRadio = document.getElementById('radio-offensive');
        const kickerRadio = document.getElementById('radio-kicker');
        const hitterStats = document.getElementById('hitterStats');
        const pitcherStats = document.getElementById('pitcherStats');
        const mmaStats = document.getElementById('mmaStats');
        const tennisStats = document.getElementById('tennisStats');
        const footballOffensiveStats = document.getElementById('footballOffensiveStats');
        const footballKickerStats = document.getElementById('footballKickerStats');
        const hitterActionBtn = document.getElementById('hitterActionBtn');
        const pitcherActionBtn = document.getElementById('pitcherActionBtn');
        const mmaActionBtn = document.getElementById('mmaActionBtn');
        const tennisActionBtn = document.getElementById('tennisActionBtn');
        const footballOffensiveActionBtn = document.getElementById('footballOffensiveActionBtn');
        const footballKickerActionBtn = document.getElementById('footballKickerActionBtn');

        radioButtons.forEach(radio => {
            radio.addEventListener('change', function() {
                console.log('Sport Selected:', this.value);
                
                // Reset all input fields
                statInputs.forEach(input => { input.value = ''; });
                hitterInputs.forEach(input => { input.value = ''; });
                pitcherInputs.forEach(input => { input.value = ''; });
                mmaInputs.forEach(input => { input.value = ''; });
                tennisInputs.forEach(input => { input.value = ''; });
                footballOffensiveInputs.forEach(input => { input.value = ''; });
                footballKickerInputs.forEach(input => { input.value = ''; });

                // Reset radio buttons
                hitterRadio.checked = false;
                pitcherRadio.checked = false;
                offensiveRadio.checked = false;
                kickerRadio.checked = false;

                // Reset all containers
                document.querySelectorAll('.middle-column .container').forEach(c => c.classList.remove('show'));
                hitterStats.classList.remove('show');
                pitcherStats.classList.remove('show');
                mmaStats.classList.remove('show');
                tennisStats.classList.remove('show');
                footballOffensiveStats.classList.remove('show');
                footballKickerStats.classList.remove('show');

                // Hide results modal
                resultsModal.style.display = 'none';

                // Show selected sport container
                if (this.value === 'basketball') {
                    basketballMessage.classList.add('show');
                } else if (this.value === 'baseball') {
                    baseballMessage.classList.add('show');
                } else if (this.value === 'tennis') {
                    tennisMessage.classList.add('show');
                    tennisStats.classList.add('show');
                } else if (this.value === 'mma') {
                    mmaMessage.classList.add('show');
                    mmaStats.classList.add('show');
                } else if (this.value === 'football') {
                    footballMessage.classList.add('show');
                }
            });
        });

        hitterRadio.addEventListener('change', function() {
            if (this.checked) {
                hitterStats.classList.add('show');
                pitcherStats.classList.remove('show');
                hitterInputs.forEach(input => { input.value = ''; });
            }
        });

        pitcherRadio.addEventListener('change', function() {
            if (this.checked) {
                pitcherStats.classList.add('show');
                hitterStats.classList.remove('show');
                pitcherInputs.forEach(input => { input.value = ''; });
            }
        });

        offensiveRadio.addEventListener('change', function() {
            if (this.checked) {
                footballOffensiveStats.classList.add('show');
                footballKickerStats.classList.remove('show');
                footballOffensiveInputs.forEach(input => { input.value = ''; });
            }
        });

        kickerRadio.addEventListener('change', function() {
            if (this.checked) {
                footballKickerStats.classList.add('show');
                footballOffensiveStats.classList.remove('show');
                footballKickerInputs.forEach(input => { input.value = ''; });
            }
        });

        resetBtn.addEventListener('click', function(e) {
            e.preventDefault();
            statInputs.forEach(input => { input.value = ''; });
            resultsModal.style.display = 'none';
        });

        baseballResetBtn.addEventListener('click', function(e) {
            e.preventDefault();
            hitterRadio.checked = false;
            pitcherRadio.checked = false;
            hitterStats.classList.remove('show');
            pitcherStats.classList.remove('show');
            hitterInputs.forEach(input => { input.value = ''; });
            pitcherInputs.forEach(input => { input.value = ''; });
            resultsModal.style.display = 'none';
        });

        mmaResetBtn.addEventListener('click', function(e) {
            e.preventDefault();
            mmaInputs.forEach(input => { input.value = ''; });
            resultsModal.style.display = 'none';
        });

        tennisResetBtn.addEventListener('click', function(e) {
            e.preventDefault();
            tennisInputs.forEach(input => { input.value = ''; });
            resultsModal.style.display = 'none';
        });

        footballResetBtn.addEventListener('click', function(e) {
            e.preventDefault();
            offensiveRadio.checked = false;
            kickerRadio.checked = false;
            footballOffensiveStats.classList.remove('show');
            footballKickerStats.classList.remove('show');
            footballOffensiveInputs.forEach(input => { input.value = ''; });
            footballKickerInputs.forEach(input => { input.value = ''; });
            resultsModal.style.display = 'none';
        });

        // Store current inputs and title for copy function
        let currentInputs = [];
        let currentTitle = '';

        // Display results function with breakdown
        function displayResults(inputs, title) {
            let html = '';
            let total = 0;
            let hasResults = false;

            currentInputs = inputs;
            currentTitle = title;

            html += `<div class="breakdown-header">📊 ${title} Breakdown:</div>`;

            inputs.forEach(input => {
                const value = parseFloat(input.value);
                if (value && !isNaN(value)) {
                    const multiplier = parseFloat(input.dataset.multiplier);
                    const name = input.dataset.name;
                    const calculation = value * multiplier;
                    total += calculation;
                    
                    const isNegative = calculation < 0;
                    html += `<div class="breakdown-item">
                        <span class="breakdown-label">${name}</span>
                        <span class="breakdown-calc">${value} × ${multiplier}</span>
                        <span class="breakdown-value ${isNegative ? 'negative' : ''}">${calculation.toFixed(1)}</span>
                    </div>`;
                    hasResults = true;
                }
            });

            if (hasResults) {
                html += `<div class="total-score-section">
                    <div class="total-score-label">Total Fantasy Score</div>
                    <div class="total-score-value">${total.toFixed(1)}</div>
                </div>`;
                html += `<div class="copy-feedback" id="copyFeedback">✓ Copied to clipboard!</div>`;
                resultsContent.innerHTML = html;
                resultsModal.style.display = 'block';
                
                // Auto-copy to clipboard
                copyBreakdown();
            }
        }

        // Copy breakdown to clipboard
        function copyBreakdown() {
            let breakdownText = `${currentTitle} Breakdown:\n\n`;
            
            currentInputs.forEach(input => {
                const value = parseFloat(input.value);
                if (value && !isNaN(value)) {
                    const multiplier = parseFloat(input.dataset.multiplier);
                    const name = input.dataset.name;
                    const calculation = value * multiplier;
                    breakdownText += `${name}: ${value} × ${multiplier} = ${calculation.toFixed(1)}\n`;
                }
            });

            // Get total from display
            const totalEl = document.querySelector('.total-score-value');
            if (totalEl) {
                breakdownText += `\nTotal Fantasy Score: ${totalEl.textContent}`;
            }

            navigator.clipboard.writeText(breakdownText).then(() => {
                const feedback = document.getElementById('copyFeedback');
                if (feedback) {
                    feedback.classList.add('show');
                    setTimeout(() => {
                        feedback.classList.remove('show');
                    }, 2000);
                }
            });
        }

        // Button click handlers
        actionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(statInputs, 'Basketball Fantasy Score');
        });

        hitterActionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(hitterInputs, 'Baseball (Hitter) Fantasy Score');
        });

        pitcherActionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(pitcherInputs, 'Baseball (Pitcher) Fantasy Score');
        });

        mmaActionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(mmaInputs, 'MMA Fantasy Score');
        });

        tennisActionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(tennisInputs, 'Tennis Fantasy Score');
        });

        footballOffensiveActionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(footballOffensiveInputs, 'Football (Offensive Player) Fantasy Score');
        });

        footballKickerActionBtn.addEventListener('click', function(e) {
            e.preventDefault();
            displayResults(footballKickerInputs, 'Football (Kicker) Fantasy Score');
        });
    </script>
</body>
</html>
