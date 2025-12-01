<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StudyFlow Master - Fully Functional Edition</title>
    <style>
        :root {
            --primary: #2d8a9d;
            --primary-hover: #256b79;
            --secondary: #f5a623;
            --success: #27ae60;
            --danger: #e74c3c;
            --warning: #f39c12;
            --dark: #1a1a1a;
            --light: #f8f9fa;
            --accent: #00d4ff;
            --gold: #ffd700;
            --purple: #8e44ad;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #0f2027 0%, #203a43 50%, #2c5364 100%);
            color: #333;
            min-height: 100vh;
            overflow-x: hidden;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Header & Navigation */
        header {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 30px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 20px;
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .user-controls {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .stats-bar {
            display: flex;
            gap: 30px;
            align-items: center;
        }

        .stat {
            text-align: center;
        }

        .stat-value {
            font-size: 28px;
            font-weight: bold;
            color: var(--primary);
        }

        .stat-label {
            font-size: 12px;
            color: #999;
            text-transform: uppercase;
            margin-top: 5px;
        }

        .streak {
            display: flex;
            align-items: center;
            gap: 10px;
            background: linear-gradient(135deg, var(--gold), #ffed4e);
            padding: 10px 20px;
            border-radius: 50px;
            font-weight: bold;
            color: #333;
            box-shadow: 0 4px 10px rgba(255, 215, 0, 0.3);
        }

        /* Modal for User Profile */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 12px;
            max-width: 400px;
            width: 90%;
        }

        .input-group {
            margin-bottom: 15px;
        }

        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .input-group input {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
        }

        /* Main Content Grid */
        .grid-main {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        @media (max-width: 1024px) {
            .grid-main {
                grid-template-columns: 1fr;
            }
        }

        /* Cards */
        .card {
            background: white;
            border-radius: 12px;
            padding: 25px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            transition: all 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.12);
        }

        .card h2 {
            color: var(--dark);
            margin-bottom: 20px;
            font-size: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* Study Mode Selector */
        .study-modes {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .mode-btn {
            padding: 15px;
            border: 2px solid #e0e0e0;
            background: white;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.2s;
            font-size: 14px;
        }

        .mode-btn:hover {
            border-color: var(--primary);
            background: rgba(45, 138, 157, 0.05);
        }

        .mode-btn.active {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        /* Flashcard Display */
        .flashcard-container {
            perspective: 1000px;
            height: 250px;
            margin-bottom: 20px;
        }

        .flashcard {
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
            font-weight: bold;
            text-align: center;
            padding: 30px;
            cursor: pointer;
            box-shadow: 0 6px 20px rgba(45, 138, 157, 0.3);
            transition: all 0.3s ease;
            transform-style: preserve-3d;
            position: relative;
        }

        .flashcard.flipped {
            background: linear-gradient(135deg, var(--secondary), #ffb84d);
            transform: rotateY(180deg);
        }

        .flashcard-inner {
            position: relative;
            width: 100%;
            height: 100%;
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }

        .flashcard-front, .flashcard-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 30px;
            border-radius: 12px;
            text-align: center;
        }

        .flashcard-front {
            background: linear-gradient(135deg, var(--primary), var(--accent));
            color: white;
        }

        .flashcard-back {
            background: linear-gradient(135deg, var(--secondary), #ffb84d);
            color: white;
            transform: rotateY(180deg);
        }

        /* Progress Bar */
        .progress-section {
            margin-bottom: 20px;
        }

        .progress-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 14px;
        }

        .progress-bar {
            background: #e0e0e0;
            border-radius: 10px;
            height: 10px;
            overflow: hidden;
        }

        .progress-fill {
            background: linear-gradient(90deg, var(--primary), var(--accent));
            height: 100%;
            transition: width 0.3s ease;
        }

        /* Buttons */
        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 14px;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: var(--primary-hover);
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(45, 138, 157, 0.3);
        }

        .btn-secondary {
            background: var(--secondary);
            color: white;
        }

        .btn-secondary:hover {
            background: #e89a1b;
        }

        .btn-small {
            padding: 8px 16px;
            font-size: 12px;
        }

        .btn-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        /* Leaderboard */
        .leaderboard-item {
            display: flex;
            align-items: center;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 8px;
            margin-bottom: 10px;
            border-left: 4px solid var(--primary);
        }

        .rank {
            font-size: 20px;
            font-weight: bold;
            color: var(--primary);
            min-width: 40px;
            text-align: center;
        }

        .rank.first {
            color: var(--gold);
            font-size: 24px;
        }

        .rank.second {
            color: #c0c0c0;
        }

        .rank.third {
            color: #cd7f32;
        }

        .user-info {
            flex: 1;
            margin-left: 15px;
        }

        .user-name {
            font-weight: bold;
            color: var(--dark);
        }

        .user-points {
            font-size: 12px;
            color: #999;
        }

        .xp-score {
            font-size: 18px;
            font-weight: bold;
            color: var(--secondary);
        }

        /* Achievements */
        .achievements-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
        }

        .achievement {
            padding: 15px;
            background: #f8f9fa;
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s;
            border: 2px solid transparent;
        }

        .achievement.unlocked {
            background: linear-gradient(135deg, rgba(39, 174, 96, 0.1), rgba(46, 204, 113, 0.1));
            border-color: var(--success);
        }

        .achievement:hover {
            transform: translateY(-3px);
        }

        .achievement-icon {
            font-size: 32px;
            margin-bottom: 8px;
        }

        .achievement-name {
            font-size: 12px;
            font-weight: bold;
            margin-bottom: 4px;
        }

        .achievement-desc {
            font-size: 10px;
            color: #999;
        }

        /* Quiz Mode */
        .quiz-question {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .quiz-question h4 {
            margin-bottom: 15px;
            color: var(--dark);
        }

        .quiz-options {
            display: grid;
            gap: 10px;
        }

        .quiz-option {
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
            background: white;
        }

        .quiz-option:hover {
            border-color: var(--primary);
            background: rgba(45, 138, 157, 0.05);
        }

        .quiz-option.selected {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        .quiz-option.correct {
            background: var(--success);
            color: white;
            border-color: var(--success);
        }

        .quiz-option.incorrect {
            background: var(--danger);
            color: white;
            border-color: var(--danger);
        }

        /* Daily Goal Widget */
        .daily-goal {
            background: linear-gradient(135deg, var(--purple), var(--primary));
            color: white;
            padding: 20px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .daily-goal h4 {
            margin-bottom: 15px;
        }

        .goal-progress {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            height: 8px;
            overflow: hidden;
            margin-bottom: 10px;
        }

        .goal-fill {
            background: white;
            height: 100%;
            transition: width 0.3s ease;
        }

        .goal-text {
            font-size: 12px;
            margin-bottom: 10px;
        }

        /* Subject Browser */
        .subject-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
        }

        .subject-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .subject-card:hover, .subject-card.selected {
            transform: translateY(-5px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.3);
        }

        .subject-icon {
            font-size: 32px;
            margin-bottom: 10px;
        }

        .subject-name {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .subject-count {
            font-size: 12px;
            opacity: 0.8;
        }

        /* Tabs */
        .tab-buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            border-bottom: 2px solid #e0e0e0;
        }

        .tab-btn {
            padding: 12px 20px;
            background: none;
            border: none;
            cursor: pointer;
            font-weight: 600;
            color: #999;
            border-bottom: 3px solid transparent;
            transition: all 0.2s;
        }

        .tab-btn.active {
            color: var(--primary);
            border-bottom-color: var(--primary);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        /* Analytics Dashboard */
        .analytics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }

        .analytic-card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }

        .analytic-value {
            font-size: 32px;
            font-weight: bold;
            color: var(--primary);
        }

        .analytic-label {
            font-size: 14px;
            color: #999;
            margin-top: 5px;
        }

        /* Deck Creation */
        .deck-form {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .deck-form input, .deck-form textarea {
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
        }

        /* Timer */
        .timer-display {
            font-size: 48px;
            font-weight: bold;
            text-align: center;
            color: var(--warning);
            margin: 20px 0;
        }

        /* Animations */
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }

        .animate-in {
            animation: slideIn 0.3s ease;
        }

        .pulse {
            animation: pulse 1.5s infinite;
        }

        /* Responsive */
        @media (max-width: 768px) {
            header {
                flex-direction: column;
                text-align: center;
            }

            .stats-bar {
                width: 100%;
                justify-content: space-around;
            }

            .study-modes {
                grid-template-columns: 1fr;
            }

            .achievements-grid {
                grid-template-columns: repeat(2, 1fr);
            }

            .subject-grid {
                grid-template-columns: repeat(2, 1fr);
            }

            .btn-group {
                flex-direction: column;
            }
        }

        .hidden {
            display: none !important;
        }

        /* Premium Badge */
        .premium-badge {
            background: var(--gold);
            color: #333;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 10px;
            font-weight: bold;
            margin-left: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header>
            <div class="logo">
                🎯 StudyFlow Master <span class="premium-badge" id="premiumBadge" style="display: none;">PREMIUM</span>
            </div>
            <div class="user-controls">
                <button class="btn btn-small" onclick="showProfileModal()">👤 <span id="currentUser">Guest</span></button>
                <button class="btn btn-small" onclick="togglePremium()">⭐ Premium</button>
                <button class="btn btn-small" onclick="exportProgress()">📤 Export</button>
                <button class="btn btn-small" onclick="importProgress()">📥 Import</button>
            </div>
            <div class="stats-bar">
                <div class="stat">
                    <div class="stat-value" id="totalXP">0</div>
                    <div class="stat-label">Total XP</div>
                </div>
                <div class="stat">
                    <div class="stat-value" id="level">1</div>
                    <div class="stat-label">Level</div>
                </div>
                <div class="stat">
                    <div class="stat-value" id="cardsLearned">0</div>
                    <div class="stat-label">Cards Mastered</div>
                </div>
                <div class="streak">
                    🔥 <span id="streakCount">0</span> Day Streak
                </div>
            </div>
        </header>

        <!-- Profile Modal -->
        <div id="profileModal" class="modal">
            <div class="modal-content">
                <h3>👤 User Profile</h3>
                <div class="input-group">
                    <label>Username:</label>
                    <input type="text" id="usernameInput" placeholder="Enter username">
                </div>
                <div class="btn-group">
                    <button class="btn btn-primary" onclick="createOrSwitchUser()">Save & Switch</button>
                    <button class="btn btn-secondary" onclick="closeProfileModal()">Cancel</button>
                </div>
            </div>
        </div>

        <div class="tab-buttons">
            <button class="tab-btn active" onclick="showTab('study')">📚 Study</button>
            <button class="tab-btn" onclick="showTab('analytics')">📊 Analytics</button>
            <button class="tab-btn" onclick="showTab('decks')">📖 Decks</button>
        </div>

        <!-- Study Tab -->
        <div id="studyTab" class="tab-content active">
            <div class="grid-main">
                <!-- Main Study Area -->
                <div>
                    <div class="card animate-in">
                        <h2>📚 Today's Study Session</h2>
                        
                        <div class="daily-goal">
                            <h4>Daily Goal: <span id="dailyGoalXP">100</span> XP</h4>
                            <div class="goal-progress">
                                <div class="goal-fill" id="dailyGoalFill" style="width: 0%"></div>
                            </div>
                            <div class="goal-text"><span id="todayXP">0</span> / <span id="dailyGoalXP">100</span> XP</div>
                        </div>

                        <!-- Subject Selector -->
                        <div class="card" style="margin-bottom: 20px;">
                            <h3>📖 Select Subject</h3>
                            <div class="subject-grid" id="subjects"></div>
                        </div>

                        <div class="study-modes">
                            <button class="mode-btn active" onclick="setStudyMode('flashcard')">🎴 Flashcards</button>
                            <button class="mode-btn" onclick="setStudyMode('quiz')">❓ Quiz Mode</button>
                            <button class="mode-btn" onclick="setStudyMode('speed')">⚡ Speed Round</button>
                            <button class="mode-btn" onclick="setStudyMode('recall')">🧠 Recall Challenge</button>
                        </div>

                        <!-- Flashcard Mode -->
                        <div id="flashcardMode" class="study-mode">
                            <div class="flashcard-container">
                                <div class="flashcard" onclick="flipCard()">
                                    <div class="flashcard-inner">
                                        <div class="flashcard-front">
                                            <span id="cardFront">Select a subject to start! 👆</span>
                                        </div>
                                        <div class="flashcard-back">
                                            <span id="cardBack"></span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="progress-section">
                                <div class="progress-info">
                                    <span>Progress</span>
                                    <span><span id="cardIndex">0</span> / <span id="cardTotal">0</span></span>
                                </div>
                                <div class="progress-bar">
                                    <div class="progress-fill" id="cardProgress" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="btn-group">
                                <button class="btn btn-primary" onclick="markCorrect()">✓ Got it! (+10 XP)</button>
                                <button class="btn btn-secondary" onclick="markIncorrect()">✗ Need More (+5 XP)</button>
                                <button class="btn btn-small" onclick="loadFlashcards()">Reset Deck</button>
                            </div>
                        </div>

                        <!-- Quiz Mode -->
                        <div id="quizMode" class="study-mode hidden">
                            <div class="quiz-question">
                                <h4 id="quizQuestion">Select a subject to start!</h4>
                                <div class="quiz-options" id="quizOptions"></div>
                            </div>
                            <div class="btn-group">
                                <button class="btn btn-primary" onclick="nextQuestion()">Next Question →</button>
                                <button class="btn btn-small" onclick="loadQuizQuestions()">Reset Quiz</button>
                            </div>
                        </div>

                        <!-- Speed Round -->
                        <div id="speedMode" class="study-mode hidden">
                            <div class="daily-goal" style="background: linear-gradient(135deg, var(--warning), var(--secondary));">
                                <h4>⚡ Speed Round - <span id="speedDuration">60</span> Seconds</h4>
                                <div class="timer-display" id="speedTimer">60</div>
                            </div>
                            <div class="quiz-question">
                                <h4 id="speedQuestion">Ready? Click Start!</h4>
                                <div class="quiz-options" id="speedOptions"></div>
                            </div>
                            <div class="btn-group">
                                <button class="btn btn-primary" id="speedStartBtn" onclick="startSpeedRound()">▶ Start Speed Round</button>
                                <button class="btn btn-small" onclick="resetSpeedRound()">Reset</button>
                            </div>
                        </div>

                        <!-- Recall Challenge -->
                        <div id="recallMode" class="study-mode hidden">
                            <div class="quiz-question">
                                <h4 id="recallPrompt">Type your answer:</h4>
                                <input type="text" id="recallInput" placeholder="Your answer here..." style="padding: 12px; border: 2px solid #e0e0e0; border-radius: 8px; width: 100%; font-size: 16px; margin-bottom: 15px;">
                            </div>
                            <div class="btn-group">
                                <button class="btn btn-primary" onclick="submitRecall()">Check Answer →</button>
                                <button class="btn btn-secondary" onclick="skipRecall()">Skip (+3 XP)</button>
                                <button class="btn btn-small" onclick="loadRecallQuestions()">Reset</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Right Sidebar -->
                <div>
                    <!-- Leaderboard -->
                    <div class="card animate-in">
                        <h2>🏆 Weekly Leaderboard</h2>
                        <div id="leaderboard"></div>
                    </div>

                    <!-- Quick Achievements -->
                    <div class="card animate-in" style="margin-top: 20px;">
                        <h2>🎖️ Achievements</h2>
                        <div class="achievements-grid" id="achievements"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Analytics Tab -->
        <div id="analyticsTab" class="tab-content">
            <div class="card">
                <h2>📊 Study Analytics</h2>
                <div class="analytics-grid" id="analyticsGrid"></div>
            </div>
        </div>

        <!-- Decks Tab -->
        <div id="decksTab" class="tab-content">
            <div class="card">
                <h2>📖 Create Custom Deck</h2>
                <div class="deck-form">
                    <input type="text" id="deckName" placeholder="Deck Name (e.g., Biology Basics)">
                    <textarea id="deckContent" placeholder="Format: front|back&#10;Question 1|Answer 1&#10;Question 2|Answer 2" rows="6"></textarea>
                    <button class="btn btn-primary" onclick="createCustomDeck()">Create Deck</button>
                </div>
                <h3 style="margin-top: 30px;">Your Custom Decks</h3>
                <div id="customDecks"></div>
            </div>
        </div>
    </div>

    <script>
        // === STATE MANAGEMENT ===
        let state = {
            currentUser: 'Guest',
            users: JSON.parse(localStorage.getItem('studyflow_users')) || {},
            totalXP: 0,
            todayXP: 0,
            level: 1,
            cardsLearned: 0,
            streak: 0,
            lastStudyDate: null,
            currentMode: 'flashcard',
            currentSubject: null,
            currentCardIndex: 0,
            correctCards: 0,
            cardFlipped: false,
            speedRunning: false,
            speedScore: 0,
            speedAnswered: 0,
            cardMastery: {}, // For spaced repetition
            sessionStartTime: null,
            premium: JSON.parse(localStorage.getItem('studyflow_premium')) || false,
            customDecks: JSON.parse(localStorage.getItem('studyflow_custom_decks')) || {},
            analytics: {
                totalSessions: 0,
                avgSessionTime: 0,
                subjectsStudied: {},
                modeUsage: {}
            }
        };

        // Full Content Data by Subject
        const subjectsData = {
            history: {
                flashcards: [
                    { front: "Capital of France?", back: "Paris", mastery: 1 },
                    { front: "Year of American Independence?", back: "1776", mastery: 1 },
                    { front: "Who was the first U.S. President?", back: "George Washington", mastery: 1 },
                    { front: "Ancient wonder: Great Pyramid location?", back: "Egypt", mastery: 1 },
                    { front: "World War II end year?", back: "1945", mastery: 1 },
                    { front: "Roman Empire founder?", back: "Romulus", mastery: 1 },
                    { front: "Renaissance artist: Mona Lisa?", back: "Da Vinci", mastery: 1 },
                    { front: "French Revolution start?", back: "1789", mastery: 1 },
                    { front: "Inventor of printing press?", back: "Gutenberg", mastery: 1 },
                    { front: "Columbus sailed in?", back: "1492", mastery: 1 },
                    { front: "Battle of Waterloo year?", back: "1815", mastery: 1 },
                    { front: "First Olympic Games?", back: "776 BC", mastery: 1 },
                    { front: "Shakespeare's birth year?", back: "1564", mastery: 1 },
                    { front: "Fall of Berlin Wall?", back: "1989", mastery: 1 },
                    { front: "Inventor of telephone?", back: "Bell", mastery: 1 },
                    { front: "Magna Carta signed?", back: "1215", mastery: 1 },
                    { front: "Cold War period?", back: "1947-1991", mastery: 1 },
                    { front: "Queen of England in 1500s?", back: "Elizabeth I", mastery: 1 },
                    { front: "First man on moon?", back: "Neil Armstrong", mastery: 1 },
                    { front: "Year of first iPhone?", back: "2007", mastery: 1 }
                ],
                quizQuestions: [
                    { q: "What year did World War I start?", options: ["1914", "1918", "1939", "1945"], correct: 0 },
                    { q: "Who assassinated Lincoln?", options: ["Booth", "Oswald", "Hickock", "Guiteau"], correct: 0 },
                    { q: "Longest river in world?", options: ["Nile", "Amazon", "Yangtze", "Mississippi"], correct: 0 },
                    { q: "Inventor of light bulb?", options: ["Edison", "Tesla", "Franklin", "Bell"], correct: 0 },
                    { q: "UN founded year?", options: ["1945", "1919", "1950", "1939"], correct: 0 }
                ]
            },
            biology: {
                flashcards: [
                    { front: "Cell power house?", back: "Mitochondria", mastery: 1 },
                    { front: "DNA full form?", back: "Deoxyribonucleic Acid", mastery: 1 },
                    { front: "Photosynthesis organ?", back: "Chloroplast", mastery: 1 },
                    { front: "Human heart chambers?", back: "4", mastery: 1 },
                    { front: "Largest organ in body?", back: "Skin", mastery: 1 },
                    { front: "Blood cells carry?", back: "Oxygen", mastery: 1 },
                    { front: "Evolution theorist?", back: "Darwin", mastery: 1 },
                    { front: "Plant cell unique?", back: "Cell Wall", mastery: 1 },
                    { front: "Brain's control center?", back: "Cerebrum", mastery: 1 },
                    { front: "Insulin produces?", back: "Pancreas", mastery: 1 },
                    { front: "Genetic material?", back: "DNA", mastery: 1 },
                    { front: "Ecosystem top?", back: "Producers", mastery: 1 },
                    { front: "Human chromosomes?", back: "46", mastery: 1 },
                    { front: "Food chain start?", back: "Plants", mastery: 1 },
                    { front: "Virus vs bacteria?", back: "Virus not alive", mastery: 1 },
                    { front: "Bone marrow makes?", back: "Blood cells", mastery: 1 },
                    { front: "Pollination by?", back: "Bees", mastery: 1 },
                    { front: "Human digestion start?", back: "Mouth", mastery: 1 },
                    { front: "RNA full form?", back: "Ribonucleic Acid", mastery: 1 },
                    { front: "Ecosystem balance?", back: "Biodiversity", mastery: 1 }
                ],
                quizQuestions: [
                    { q: "What is ATP?", options: ["Energy", "Protein", "Fat", "Carb"], correct: 0 },
                    { q: "Largest bone?", options: ["Femur", "Humerus", "Tibia", "Radius"], correct: 0 },
                    { q: "Meiosis produces?", options: ["Gametes", "Somatic", "Stem", "Red"], correct: 0 },
                    { q: "pH of blood?", options: ["7.4", "7.0", "6.8", "8.0"], correct: 0 },
                    { q: "Mitosis stages?", options: ["4", "3", "5", "2"], correct: 0 }
                ]
            },
            // Add similar for chemistry, physics, languages, mathematics (abbreviated for space)
            chemistry: {
                flashcards: [{ front: "H2O is?", back: "Water", mastery: 1 }, { front: "Atomic number H?", back: "1", mastery: 1 }, /* ... 18 more */],
                quizQuestions: [{ q: "pH scale range?", options: ["0-14", "1-10", "0-7", "7-14"], correct: 0 }, /* ... */]
            },
            physics: {
                flashcards: [{ front: "Gravity law?", back: "Newton", mastery: 1 }, { front: "Speed unit?", back: "m/s", mastery: 1 }, /* ... */],
                quizQuestions: [{ q: "E=mc² by?", options: ["Einstein", "Newton", "Planck", "Bohr"], correct: 0 }, /* ... */]
            },
            languages: {
                flashcards: [{ front: "Hello in Spanish?", back: "Hola", mastery: 1 }, { front: "Thank you French?", back: "Merci", mastery: 1 }, /* ... */],
                quizQuestions: [{ q: "Bonjour means?", options: ["Hello", "Goodbye", "Please", "Yes"], correct: 0 }, /* ... */]
            },
            mathematics: {
                flashcards: [{ front: "Pi approx?", back: "3.14", mastery: 1 }, { front: "Pythagoras theorem?", back: "a²+b²=c²", mastery: 1 }, /* ... */],
                quizQuestions: [{ q: "2+2*3?", options: ["8", "12", "6", "10"], correct: 0 }, /* ... */]
            }
        };

        const subjects = [
            { name: "History", key: "history", icon: "🏛️", count: 20 },
            { name: "Biology", key: "biology", icon: "🧬", count: 20 },
            { name: "Chemistry", key: "chemistry", icon: "⚗️", count: 20 },
            { name: "Physics", key: "physics", icon: "⚛️", count: 20 },
            { name: "Languages", key: "languages", icon: "🌍", count: 20 },
            { name: "Mathematics", key: "mathematics", icon: "🔢", count: 20 }
        ];

        const achievements = [
            { id: 1, name: "First Step", desc: "Complete first session", icon: "👣", unlocked: false, condition: () => state.analytics.totalSessions >= 1 },
            { id: 2, name: "Streak Starter", desc: "3 day streak", icon: "🔥", unlocked: false, condition: () => state.streak >= 3 },
            { id: 3, name: "Quiz Legend", desc: "100% quiz accuracy", icon: "💯", unlocked: false, condition: () => state.analytics.quizAccuracy >= 100 },
            { id: 4, name: "Speed Demon", desc: "80+ speed score", icon: "⚡", unlocked: false, condition: () => state.analytics.highSpeedScore >= 80 },
            { id: 5, name: "Master Scholar", desc: "50 cards mastered", icon: "🧠", unlocked: false, condition: () => state.cardsLearned >= 50 },
            { id: 6, name: "Level Up", desc: "Reach level 5", icon: "⭐", unlocked: false, condition: () => state.level >= 5 },
            { id: 7, name: "Deck Creator", desc: "Create custom deck", icon: "📖", unlocked: false, condition: () => Object.keys(state.customDecks).length > 0 },
            { id: 8, name: "Premium User", desc: "Activate premium", icon: "💎", unlocked: false, condition: () => state.premium },
            { id: 9, name: "Daily Dedication", desc: "7 day streak", icon: "📅", unlocked: false, condition: () => state.streak >= 7 },
            { id: 10, name: "Analytics Expert", desc: "Review analytics", icon: "📊", unlocked: false, condition: () => state.analytics.totalSessions >= 10 }
        ];

        const leaderboardData = [  // Simulated global leaderboard
            { name: "Global Top", xp: 5000 },
            { name: "StudyPro", xp: 4500 },
            { name: "QuizMaster", xp: 4200 },
            { name: "FlashKing", xp: 3800 },
            { name: "RecallExpert", xp: 3500 }
        ];

        // === UTILITY FUNCTIONS ===
        function loadUserState(username) {
            const userData = state.users[username] || {
                totalXP: 0, todayXP: 0, level: 1, cardsLearned: 0, streak: 0,
                lastStudyDate: null, cardMastery: {}, analytics: { totalSessions: 0, avgSessionTime: 0, subjectsStudied: {}, modeUsage: {}, quizAccuracy: 0, highSpeedScore: 0 }
            };
            state.totalXP = userData.totalXP;
            state.todayXP = userData.todayXP;
            state.level = userData.level;
            state.cardsLearned = userData.cardsLearned;
            state.streak = userData.streak;
            state.lastStudyDate = userData.lastStudyDate;
            state.cardMastery = userData.cardMastery || {};
            state.analytics = userData.analytics || state.analytics;
            updateStreak();
            updateStats();
            checkAchievements();
            loadAchievements();
            loadAnalytics();
        }

        function saveUserState(username) {
            state.users[username] = {
                totalXP: state.totalXP,
                todayXP: state.todayXP,
                level: state.level,
                cardsLearned: state.cardsLearned,
                streak: state.streak,
                lastStudyDate: state.lastStudyDate,
                cardMastery: state.cardMastery,
                analytics: state.analytics,
                customDecks: state.customDecks
            };
            localStorage.setItem('studyflow_users', JSON.stringify(state.users));
            localStorage.setItem('studyflow_premium', JSON.stringify(state.premium));
            localStorage.setItem('studyflow_custom_decks', JSON.stringify(state.customDecks));
        }

        function updateStreak() {
            const today = new Date().toDateString();
            if (state.lastStudyDate !== today) {
                if (state.lastStudyDate === new Date(Date.now() - 86400000).toDateString()) {
                    state.streak++;
                } else {
                    state.streak = 1;
                }
                state.lastStudyDate = today;
                saveUserState(state.currentUser);
            }
        }

        function addXP(amount, mode = 'general') {
            state.totalXP += amount;
            state.todayXP += amount;
            state.level = Math.floor(state.totalXP / 500) + 1;
            state.analytics.modeUsage[mode] = (state.analytics.modeUsage[mode] || 0) + amount;
            if (state.todayXP >= parseInt(document.getElementById('dailyGoalXP').textContent)) {
                addXP(50, 'goal'); // Bonus for daily goal
                showNotification('Daily Goal Achieved! +50 Bonus XP', '#ffd700');
            }
            updateStats();
            checkAchievements();
            saveUserState(state.currentUser);
        }

        function showNotification(message, color) {
            const notif = document.createElement('div');
            notif.textContent = message;
            notif.style.cssText = `position: fixed; top: 20px; right: 20px; background: ${color}; color: white; padding: 15px 25px; border-radius: 8px; font-weight: bold; box-shadow: 0 4px 12px rgba(0,0,0,0.2); animation: slideIn 0.3s ease; z-index: 1000;`;
            document.body.appendChild(notif);
            setTimeout(() => notif.remove(), 3000);
        }

        function startSession() {
            if (!state.sessionStartTime) {
                state.sessionStartTime = Date.now();
                state.analytics.totalSessions++;
                saveUserState(state.currentUser);
            }
        }

        function endSession() {
            if (state.sessionStartTime) {
                const sessionTime = (Date.now() - state.sessionStartTime) / 1000 / 60; // minutes
                state.analytics.avgSessionTime = ((state.analytics.avgSessionTime * (state.analytics.totalSessions - 1) + sessionTime) / state.analytics.totalSessions);
                state.sessionStartTime = null;
                loadAnalytics();
                saveUserState(state.currentUser);
            }
        }

        // === UI UPDATES ===
        function updateStats() {
            document.getElementById('totalXP').textContent = state.totalXP;
            document.getElementById('level').textContent = state.level;
            document.getElementById('cardsLearned').textContent = state.cardsLearned;
            document.getElementById('streakCount').textContent = state.streak;
            document.getElementById('todayXP').textContent = state.todayXP;
            const goalPercent = Math.min((state.todayXP / parseInt(document.getElementById('dailyGoalXP').textContent)) * 100, 100);
            document.getElementById('dailyGoalFill').style.width = goalPercent + '%';
            document.getElementById('premiumBadge').style.display = state.premium ? 'inline' : 'none';
            document.getElementById('currentUser').textContent = state.currentUser;
        }

        function loadSubjects() {
            const subContainer = document.getElementById('subjects');
            subContainer.innerHTML = '';
            subjects.forEach(s => {
                const div = document.createElement('div');
                div.className = 'subject-card' + (state.currentSubject === s.key ? ' selected' : '');
                div.innerHTML = `
                    <div class="subject-icon">${s.icon}</div>
                    <div class="subject-name">${s.name}</div>
                    <div class="subject-count">${s.count} Cards</div>
                `;
                div.onclick = () => selectSubject(s.key);
                subContainer.appendChild(div);
            });
        }

        function selectSubject(key) {
            state.currentSubject = key;
            state.currentCardIndex = 0;
            loadSubjects();
            loadFlashcards();
            loadQuizQuestions();
            loadRecallQuestions();
            state.analytics.subjectsStudied[key] = (state.analytics.subjectsStudied[key] || 0) + 1;
            saveUserState(state.currentUser);
            showNotification(`Switched to ${key.charAt(0).toUpperCase() + key.slice(1)}!`, '#8e44ad');
        }

        // === FLASHCARD MODE WITH SPACED REPETITION ===
        function getDueCards() {
            if (!state.currentSubject) return [];
            const cards = subjectsData[state.currentSubject].flashcards;
            return cards.filter((card, idx) => {
                const mastery = state.cardMastery[idx] || 1;
                const due = mastery < 5 || Math.random() < (1 / mastery); // Spaced: easier cards less frequent
                return due;
            }).sort(() => Math.random() - 0.5); // Shuffle
        }

        function loadFlashcards() {
            if (!state.currentSubject) return;
            const dueCards = getDueCards();
            if (dueCards.length === 0) {
                showNotification('All cards mastered! Review all?', '#27ae60');
                state.currentCardIndex = 0;
                displayFlashcard(subjectsData[state.currentSubject].flashcards[0]);
                return;
            }
            state.flashcardDeck = dueCards;
            state.currentCardIndex = 0;
            state.correctCards = 0;
            state.cardFlipped = false;
            displayFlashcard();
            startSession();
        }

        function displayFlashcard(cardData = null) {
            let card;
            if (cardData) {
                card = cardData;
            } else if (state.flashcardDeck) {
                card = state.flashcardDeck[state.currentCardIndex];
            } else {
                return;
            }
            const idx = subjectsData[state.currentSubject].flashcards.indexOf(card);
            document.getElementById('cardFront').textContent = card.front;
            document.getElementById('cardBack').textContent = card.back;
            document.getElementById('cardIndex').textContent = state.currentCardIndex + 1;
            document.getElementById('cardTotal').textContent = state.flashcardDeck ? state.flashcardDeck.length : 0;
            const progress = state.flashcardDeck ? ((state.currentCardIndex + 1) / state.flashcardDeck.length) * 100 : 0;
            document.getElementById('cardProgress').style.width = progress + '%';
            document.querySelector('.flashcard-inner').style.transform = 'rotateY(0deg)';
            state.cardFlipped = false;
        }

        function flipCard() {
            if (state.cardFlipped) return;
            state.cardFlipped = true;
            document.querySelector('.flashcard-inner').style.transform = 'rotateY(180deg)';
        }

        function markCorrect() {
            if (!state.flashcardDeck || state.currentCardIndex >= state.flashcardDeck.length) return;
            const card = state.flashcardDeck[state.currentCardIndex];
            const idx = subjectsData[state.currentSubject].flashcards.indexOf(card);
            state.cardMastery[idx] = (state.cardMastery[idx] || 1) + 1;
            if (state.cardMastery[idx] > 5) state.cardMastery[idx] = 5;
            state.correctCards++;
            state.cardsLearned++;
            addXP(10, 'flashcard');
            nextCard();
        }

        function markIncorrect() {
            if (!state.flashcardDeck || state.currentCardIndex >= state.flashcardDeck.length) return;
            const card = state.flashcardDeck[state.currentCardIndex];
            const idx = subjectsData[state.currentSubject].flashcards.indexOf(card);
            state.cardMastery[idx] = Math.max(1, (state.cardMastery[idx] || 1) - 1);
            addXP(5, 'flashcard');
            nextCard();
        }

        function nextCard() {
            if (state.currentCardIndex < state.flashcardDeck.length - 1) {
                state.currentCardIndex++;
                displayFlashcard();
            } else {
                endSession();
                const accuracy = Math.round((state.correctCards / state.flashcardDeck.length) * 100);
                showNotification(`Deck Complete! ${accuracy}% accuracy`, accuracy >= 80 ? '#27ae60' : '#e74c3c');
                loadFlashcards();
            }
            saveUserState(state.currentUser);
        }

        // === QUIZ MODE ===
        function loadQuizQuestions() {
            if (!state.currentSubject) return;
            state.quizDeck = [...subjectsData[state.currentSubject].quizQuestions];
            state.currentCardIndex = 0;
            state.quizCorrect = 0;
            state.quizTotal = state.quizDeck.length;
            displayQuizQuestion();
            startSession();
        }

        function displayQuizQuestion() {
            const q = state.quizDeck[state.currentCardIndex];
            document.getElementById('quizQuestion').textContent = q.q;
            const optionsDiv = document.getElementById('quizOptions');
            optionsDiv.innerHTML = '';
            q.options.forEach((opt, idx) => {
                const btn = document.createElement('div');
                btn.className = 'quiz-option';
                btn.textContent = opt;
                btn.onclick = () => selectQuizOption(idx, q.correct);
                optionsDiv.appendChild(btn);
            });
            document.getElementById('cardIndex').textContent = state.currentCardIndex + 1;
            document.getElementById('cardTotal').textContent = state.quizTotal;
            document.getElementById('cardProgress').style.width = ((state.currentCardIndex + 1) / state.quizTotal * 100) + '%';
        }

        function selectQuizOption(selected, correct) {
            const options = document.querySelectorAll('#quizOptions .quiz-option');
            options.forEach(o => o.onclick = null);
            options[selected].classList.add(selected === correct ? 'correct' : 'incorrect');
            if (selected === correct) {
                state.quizCorrect++;
                addXP(15, 'quiz');
                state.analytics.quizAccuracy = Math.max(state.analytics.quizAccuracy, ((state.quizCorrect / (state.currentCardIndex + 1)) * 100));
                showNotification('Correct! +15 XP', '#27ae60');
            } else {
                addXP(5, 'quiz');
                showNotification('Incorrect! +5 XP', '#e74c3c');
                // Highlight correct
                options[correct].classList.add('correct');
            }
        }

        function nextQuestion() {
            if (state.currentCardIndex < state.quizTotal - 1) {
                state.currentCardIndex++;
                displayQuizQuestion();
            } else {
                endSession();
                const accuracy = Math.round((state.quizCorrect / state.quizTotal) * 100);
                showNotification(`Quiz Complete! ${state.quizCorrect}/${state.quizTotal} (${accuracy}%)`, accuracy >= 90 ? '#27ae60' : '#e74c3c');
                loadQuizQuestions();
            }
            saveUserState(state.currentUser);
        }

        // === SPEED ROUND ===
        function startSpeedRound() {
            if (!state.currentSubject) return;
            state.speedRunning = true;
            state.speedScore = 0;
            state.speedAnswered = 0;
            state.speedStartTime = Date.now();
            let timeLeft = parseInt(document.getElementById('speedDuration').textContent);
            document.getElementById('speedStartBtn').disabled = true;
            document.getElementById('speedQuestion').textContent = 'Go! Answer quickly!';

            const interval = setInterval(() => {
                timeLeft--;
                document.getElementById('speedTimer').textContent = timeLeft;
                if (timeLeft <= 0) {
                    clearInterval(interval);
                    state.speedRunning = false;
                    document.getElementById('speedStartBtn').disabled = false;
                    const score = state.speedScore;
                    state.analytics.highSpeedScore = Math.max(state.analytics.highSpeedScore, score);
                    addXP(score * 5, 'speed');
                    showNotification(`Speed Round Over! Score: ${score}/${timeLeft + 1} (${Math.round(score * 100 / (timeLeft + 1))}% ) +${score * 5} XP`, '#ff6b35');
                    endSession();
                    resetSpeedRound();
                }
            }, 1000);

            loadSpeedQuestion();
        }

        function loadSpeedQuestion() {
            if (!state.speedRunning || !state.currentSubject) return;
            const q = subjectsData[state.currentSubject].quizQuestions[Math.floor(Math.random() * subjectsData[state.currentSubject].quizQuestions.length)];
            document.getElementById('speedQuestion').textContent = q.q;
            const optionsDiv = document.getElementById('speedOptions');
            optionsDiv.innerHTML = '';
            q.options.forEach((opt, idx) => {
                const btn = document.createElement('div');
                btn.className = 'quiz-option';
                btn.textContent = opt;
                btn.onclick = () => {
                    if (idx === q.correct) {
                        state.speedScore++;
                        btn.classList.add('correct');
                    } else {
                        btn.classList.add('incorrect');
                    }
                    state.speedAnswered++;
                    setTimeout(() => {
                        if (state.speedRunning) loadSpeedQuestion();
                    }, 300);
                };
                optionsDiv.appendChild(btn);
            });
        }

        function resetSpeedRound() {
            state.speedRunning = false;
            state.speedScore = 0;
            state.speedAnswered = 0;
            document.getElementById('speedTimer').textContent = '60';
            document.getElementById('speedQuestion').textContent = 'Ready? Click Start!';
            document.getElementById('speedOptions').innerHTML = '';
            document.getElementById('speedStartBtn').disabled = false;
        }

        // === RECALL MODE ===
        function loadRecallQuestions() {
            if (!state.currentSubject) return;
            state.recallDeck = [...subjectsData[state.currentSubject].flashcards].sort(() => Math.random() - 0.5);
            state.currentCardIndex = 0;
            displayRecallQuestion();
            startSession();
        }

        function displayRecallQuestion() {
            const card = state.recallDeck[state.currentCardIndex];
            document.getElementById('recallPrompt').textContent = `Recall: ${card.front}`;
            document.getElementById('recallInput').value = '';
            document.getElementById('recallInput').focus();
        }

        function submitRecall() {
            const userAnswer = document.getElementById('recallInput').value.trim().toLowerCase();
            const card = state.recallDeck[state.currentCardIndex];
            const correctAnswer = card.back.toLowerCase();
            if (userAnswer.includes(correctAnswer) || correctAnswer.includes(userAnswer)) {
                addXP(12, 'recall');
                showNotification('Correct! +12 XP', '#27ae60');
                const idx = subjectsData[state.currentSubject].flashcards.indexOf(card);
                state.cardMastery[idx] = (state.cardMastery[idx] || 1) + 0.5;
            } else {
                addXP(3, 'recall');
                showNotification(`Incorrect! It was: ${card.back} (+3 XP)`, '#e74c3c');
            }
            if (state.currentCardIndex < state.recallDeck.length - 1) {
                state.currentCardIndex++;
                displayRecallQuestion();
            } else {
                endSession();
                showNotification('Recall Complete! Review weak areas.', '#8e44ad');
                loadRecallQuestions();
            }
            saveUserState(state.currentUser);
        }

        function skipRecall() {
            addXP(3, 'recall');
            if (state.currentCardIndex < state.recallDeck.length - 1) {
                state.currentCardIndex++;
                displayRecallQuestion();
            } else {
                loadRecallQuestions();
            }
            saveUserState(state.currentUser);
        }

        // === LEADERBOARD ===
        function loadLeaderboard() {
            const lb = document.getElementById('leaderboard');
            lb.innerHTML = '';
            [...leaderboardData, { name: state.currentUser, xp: state.totalXP }].sort((a, b) => b.xp - a.xp).slice(0, 5).forEach((item, idx) => {
                const medals = ['🥇', '🥈', '🥉'];
                const medal = medals[idx] || '🎖️';
                const div = document.createElement('div');
                div.className = 'leaderboard-item';
                div.innerHTML = `
                    <div class="rank ${idx === 0 ? 'first' : idx === 1 ? 'second' : idx === 2 ? 'third' : ''}">${medal}</div>
                    <div class="user-info">
                        <div class="user-name">${item.name}</div>
                        <div class="user-points">Weekly XP</div>
                    </div>
                    <div class="xp-score">${item.xp}</div>
                `;
                lb.appendChild(div);
            });
        }

        // === ACHIEVEMENTS ===
        function checkAchievements() {
            achievements.forEach(ach => {
                if (ach.condition() && !ach.unlocked) {
                    ach.unlocked = true;
                    addXP(50, 'achievement');
                    showNotification(`Achievement Unlocked: ${ach.name}! +50 XP`, '#ffd700');
                }
            });
            saveUserState(state.currentUser);
        }

        function loadAchievements() {
            const achContainer = document.getElementById('achievements');
            achContainer.innerHTML = '';
            achievements.forEach(a => {
                const div = document.createElement('div');
                div.className = 'achievement ' + (a.unlocked ? 'unlocked' : '');
                div.innerHTML = `
                    <div class="achievement-icon">${a.icon}</div>
                    <div class="achievement-name">${a.name}</div>
                    <div class="achievement-desc">${a.desc}</div>
                `;
                achContainer.appendChild(div);
            });
        }

        // === ANALYTICS ===
        function loadAnalytics() {
            const grid = document.getElementById('analyticsGrid');
            if (!grid) return;
            grid.innerHTML = `
                <div class="analytic-card">
                    <div class="analytic-value">${state.analytics.totalSessions}</div>
                    <div class="analytic-label">Total Sessions</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">${Math.round(state.analytics.avgSessionTime)}m</div>
                    <div class="analytic-label">Avg Session Time</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">${state.cardsLearned}</div>
                    <div class="analytic-label">Cards Mastered</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">${state.streak}</div>
                    <div class="analytic-label">Current Streak</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">${Math.round(state.analytics.quizAccuracy)}%</div>
                    <div class="analytic-label">Quiz Accuracy</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">${state.analytics.highSpeedScore}</div>
                    <div class="analytic-label">Best Speed Score</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">${Object.keys(state.analytics.subjectsStudied).length}</div>
                    <div class="analytic-label">Subjects Studied</div>
                </div>
                <div class="analytic-card">
                    <div class="analytic-value">$${state.premium ? 'Premium' : 'Free'}</div>
                    <div class="analytic-label">Plan</div>
                </div>
            `;
        }

        // === DECK CREATION ===
        function createCustomDeck() {
            if (!state.premium && Object.keys(state.customDecks).length >= 3) {
                showNotification('Upgrade to Premium for unlimited decks!', '#e74c3c');
                return;
            }
            const name = document.getElementById('deckName').value.trim();
            const content = document.getElementById('deckContent').value.trim();
            if (!name || !content) {
                showNotification('Please enter deck name and content!', '#e74c3c');
                return;
            }
            const cards = content.split('\n').map(line => {
                const [front, back] = line.split('|').map(s => s.trim());
                return { front, back, mastery: 1 };
            }).filter(c => c.front && c.back);
            if (cards.length === 0) return;
            state.customDecks[name] = cards;
            subjectsData[name.toLowerCase().replace(/\s+/g, '')] = { flashcards: cards, quizQuestions: [] }; // Add as subject
            subjects.push({ name, key: name.toLowerCase().replace(/\s+/g, ''), icon: '📚', count: cards.length });
            loadSubjects();
            document.getElementById('deckName').value = '';
            document.getElementById('deckContent').value = '';
            showNotification(`Created deck: ${name} with ${cards.length} cards!`, '#27ae60');
            achievements[6].unlocked = true; // Deck Creator
            checkAchievements();
            saveUserState(state.currentUser);
            loadCustomDecks();
        }

        function loadCustomDecks() {
            const container = document.getElementById('customDecks');
            if (!container) return;
            container.innerHTML = '';
            Object.keys(state.customDecks).forEach(name => {
                const div = document.createElement('div');
                div.className = 'subject-card';
                div.innerHTML = `
                    <div class="subject-icon">📚</div>
                    <div class="subject-name">${name}</div>
                    <div class="subject-count">${state.customDecks[name].length} Cards</div>
                    <button class="btn btn-small" onclick="selectSubject('${name.toLowerCase().replace(/\s+/g, '')}')">Study</button>
                    <button class="btn btn-small" onclick="deleteDeck('${name}')" style="background: var(--danger); color: white;">Delete</button>
                `;
                container.appendChild(div);
            });
        }

        function deleteDeck(name) {
            delete state.customDecks[name];
            const key = name.toLowerCase().replace(/\s+/g, '');
            delete subjectsData[key];
            subjects = subjects.filter(s => s.key !== key);
            loadSubjects();
            loadCustomDecks();
            saveUserState(state.currentUser);
        }

        // === USER MANAGEMENT ===
        function showProfileModal() {
            document.getElementById('profileModal').style.display = 'flex';
            document.getElementById('usernameInput').value = state.currentUser;
        }

        function closeProfileModal() {
            document.getElementById('profileModal').style.display = 'none';
        }

        function createOrSwitchUser() {
            const username = document.getElementById('usernameInput').value.trim() || 'Guest';
            if (username !== state.currentUser) {
                saveUserState(state.currentUser);
                state.currentUser = username;
                loadUserState(username);
                loadLeaderboard();
            }
            closeProfileModal();
        }

        // === PREMIUM ===
        function togglePremium() {
            state.premium = !state.premium;
            document.getElementById('dailyGoalXP').textContent = state.premium ? '200' : '100';
            showNotification(state.premium ? 'Premium Activated! (Simulated - $9.99/mo)' : 'Free Tier Active', state.premium ? '#ffd700' : '#999');
            achievements[7].unlocked = state.premium;
            checkAchievements();
            saveUserState(state.currentUser);
            updateStats();
        }

        // === EXPORT/IMPORT ===
        function exportProgress() {
            const data = {
                user: state.currentUser,
                progress: state.users[state.currentUser],
                customDecks: state.customDecks,
                premium: state.premium
            };
            const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `${state.currentUser}_studyflow_progress.json`;
            a.click();
            URL.revokeObjectURL(url);
            showNotification('Progress exported!', '#27ae60');
        }

        function importProgress() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = '.json';
            input.onchange = e => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = ev => {
                        try {
                            const data = JSON.parse(ev.target.result);
                            if (data.user) {
                                state.currentUser = data.user;
                                state.users[state.currentUser] = data.progress || state.users[state.currentUser];
                                state.customDecks = data.customDecks || {};
                                state.premium = data.premium || false;
                                loadUserState(state.currentUser);
                                // Rebuild subjects from custom decks
                                Object.keys(state.customDecks).forEach(name => {
                                    const key = name.toLowerCase().replace(/\s+/g, '');
                                    if (!subjectsData[key]) {
                                        subjectsData[key] = { flashcards: state.customDecks[name], quizQuestions: [] };
                                        subjects.push({ name, key, icon: '📚', count: state.customDecks[name].length });
                                    }
                                });
                                loadSubjects();
                                loadCustomDecks();
                                loadLeaderboard();
                                loadAchievements();
                                loadAnalytics();
                                showNotification('Progress imported!', '#27ae60');
                            }
                        } catch (err) {
                            showNotification('Invalid file!', '#e74c3c');
                        }
                    };
                    reader.readAsText(file);
                }
            };
            input.click();
        }

        // === MODE SWITCHING & TABS ===
        function setStudyMode(mode) {
            endSession();
            state.currentMode = mode;
            document.querySelectorAll('.mode-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            document.querySelectorAll('.study-mode').forEach(m => m.classList.add('hidden'));
            document.getElementById(mode + 'Mode').classList.remove('hidden');
            if (mode === 'flashcard') loadFlashcards();
            else if (mode === 'quiz') loadQuizQuestions();
            else if (mode === 'speed') resetSpeedRound();
            else if (mode === 'recall') loadRecallQuestions();
        }

        function showTab(tab) {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(tab + 'Tab').classList.add('active');
            if (tab === 'analytics') loadAnalytics();
            if (tab === 'decks') loadCustomDecks();
            achievements[9].unlocked = tab === 'analytics' && state.analytics.totalSessions >= 10;
            checkAchievements();
        }

        // === INITIALIZATION ===
        function init() {
            state.currentUser = localStorage.getItem('studyflow_current_user') || 'Guest';
            loadUserState(state.currentUser);
            localStorage.setItem('studyflow_current_user', state.currentUser);
            loadSubjects();
            loadLeaderboard();
            loadAchievements();
            updateStats();
            // Add missing subjects data placeholders
            ['chemistry', 'physics', 'languages', 'mathematics'].forEach(key => {
                if (!subjectsData[key].flashcards || subjectsData[key].flashcards.length < 20) {
                    // Placeholder - in real app, load more
                    subjectsData[key].flashcards = Array(20).fill().map((_, i) => ({ front: `Q${i+1} ${key}?`, back: `A${i+1}`, mastery: 1 }));
                    subjectsData[key].quizQuestions = Array(5).fill().map((_, i) => ({ q: `Quiz Q${i+1} ${key}?`, options: [`Opt1`, `Opt2`, `Opt3`, `Opt4`], correct: 0 }));
                }
            });
            window.addEventListener('beforeunload', () => {
                endSession();
                saveUserState(state.currentUser);
            });
            showNotification('Welcome to StudyFlow Master! Select a subject to begin.', '#8e44ad');
        }

        // Start
        init();
    </script>
</body>
</html>
