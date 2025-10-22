<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gioco di Vocabolario Italiano</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #008C45 0%, #F4F5F0 50%, #CD212A 100%);
            min-height: 100vh;
            padding: 20px;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #008C45, #CD212A);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #008C45, #CD212A);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 140, 69, 0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #008C45;
            box-shadow: 0 4px 15px rgba(0, 140, 69, 0.2);
        }

        .word-bank h3 {
            color: #008C45;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #008C45, #CD212A);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(0, 140, 69, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 140, 69, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #CD212A;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #CD212A;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 150px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #008C45;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(0, 140, 69, 0.2);
        }

        .option.selected {
            background: #008C45;
            color: white;
            border-color: #008C45;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #008C45;
            font-size: 1.2em;
        }

        .match-item {
            padding: 15px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #CD212A;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(205, 33, 42, 0.2);
        }

        .match-item.selected {
            background: #fff5f5;
            border-color: #CD212A;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #008C45, #CD212A);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 140, 69, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 140, 69, 0.4);
        }

        .reset-btn {
            background: linear-gradient(135deg, #f44336, #ff7961);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 10px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(244, 67, 54, 0.3);
        }

        .reset-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(244, 67, 54, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #008C45, #CD212A);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(0, 140, 69, 0.3);
            z-index: 1000;
        }

        .icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        /* Vocabulary List Styles */
        .vocabulary-list {
            padding: 20px;
        }

        .vocabulary-item {
            margin-bottom: 25px;
            padding: 20px;
            border-radius: 10px;
            background: #f8f9fa;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .vocabulary-item h3 {
            color: #CD212A;
            margin-bottom: 10px;
            font-size: 1.4em;
            border-bottom: 2px solid #008C45;
            padding-bottom: 8px;
        }

        .vocabulary-item p {
            margin-bottom: 8px;
            line-height: 1.6;
        }

        .example {
            font-style: italic;
            color: #555;
            padding-left: 15px;
            border-left: 3px solid #008C45;
            margin-top: 10px;
        }

        .italian-flag {
            display: inline-block;
            width: 20px;
            height: 15px;
            background: linear-gradient(90deg, #008C45 33%, #F4F5F0 33%, #F4F5F0 66%, #CD212A 66%);
            margin-right: 8px;
            border: 1px solid #ddd;
            vertical-align: middle;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="score">Punteggio: <span id="score">0</span>/12</div>
    
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-book icon"></i> Gioco di Vocabolario Italiano</h1>
            <p>Pratica il tuo vocabolario italiano!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('vocabulary')">Lista Vocabolario</button>
            <button class="nav-btn" onclick="showSection('fill-gaps')">Completa le Frasi</button>
            <button class="nav-btn" onclick="showSection('matching')">Abbina le Definizioni</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Scelta Multipla</button>
        </div>

        <!-- Vocabulary List Section -->
        <div id="vocabulary" class="game-section active">
            <h2 style="text-align: center; margin-bottom: 20px; color: #008C45;">Spiegazioni del Vocabolario</h2>
            
            <div class="vocabulary-list">
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>facile</h3>
                    <p><strong>Definizione:</strong> Easy, simple</p>
                    <p><strong>Uso:</strong> Describes something that is not difficult</p>
                    <p class="example"><strong>Esempio:</strong> "Questo esercizio è molto facile." (This exercise is very easy.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>secondo me</h3>
                    <p><strong>Definizione:</strong> In my opinion</p>
                    <p><strong>Uso:</strong> Used to express personal opinion</p>
                    <p class="example"><strong>Esempio:</strong> "Secondo me, il film era bellissimo." (In my opinion, the movie was beautiful.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>vivere</h3>
                    <p><strong>Definizione:</strong> To live</p>
                    <p><strong>Uso:</strong> Describes the act of living or residing somewhere</p>
                    <p class="example"><strong>Esempio:</strong> "Voglio vivere in Italia." (I want to live in Italy.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>gradi</h3>
                    <p><strong>Definizione:</strong> Degrees (temperature), grades</p>
                    <p><strong>Uso:</strong> Used for temperature or academic grades</p>
                    <p class="example"><strong>Esempio:</strong> "Oggi ci sono 25 gradi." (Today it's 25 degrees.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>vacanza</h3>
                    <p><strong>Definizione:</strong> Vacation, holiday</p>
                    <p><strong>Uso:</strong> Period of time for rest or travel</p>
                    <p class="example"><strong>Esempio:</strong> "Andiamo in vacanza al mare." (We're going on vacation to the seaside.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>quindi</h3>
                    <p><strong>Definizione:</strong> Therefore, so</p>
                    <p><strong>Uso:</strong> Used to show consequence or conclusion</p>
                    <p class="example"><strong>Esempio:</strong> "Piove, quindi resto a casa." (It's raining, so I'm staying home.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>perché (as because)</h3>
                    <p><strong>Definizione:</strong> Because</p>
                    <p><strong>Uso:</strong> Explains the reason for something</p>
                    <p class="example"><strong>Esempio:</strong> "Studio italiano perché mi piace la cultura." (I study Italian because I like the culture.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>preferito</h3>
                    <p><strong>Definizione:</strong> Favorite, preferred</p>
                    <p><strong>Uso:</strong> Describes something liked best</p>
                    <p class="example"><strong>Esempio:</strong> "La pizza è il mio cibo preferito." (Pizza is my favorite food.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>penna</h3>
                    <p><strong>Definizione:</strong> Pen</p>
                    <p><strong>Uso:</strong> Writing instrument</p>
                    <p class="example"><strong>Esempio:</strong> "Ho bisogno di una penna per scrivere." (I need a pen to write.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>affollata</h3>
                    <p><strong>Definizione:</strong> Crowded</p>
                    <p><strong>Uso:</strong> Describes a place with many people</p>
                    <p class="example"><strong>Esempio:</strong> "La spiaggia era molto affollata." (The beach was very crowded.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>per quanto tempo</h3>
                    <p><strong>Definizione:</strong> For how long</p>
                    <p><strong>Uso:</strong> Asks about duration of time</p>
                    <p class="example"><strong>Esempio:</strong> "Per quanto tempo resti in Italia?" (For how long are you staying in Italy?)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>volo</h3>
                    <p><strong>Definizione:</strong> Flight</p>
                    <p><strong>Uso:</strong> Air travel journey</p>
                    <p class="example"><strong>Esempio:</strong> "Il volo per Roma dura due ore." (The flight to Rome takes two hours.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>biglietti</h3>
                    <p><strong>Definizione:</strong> Tickets</p>
                    <p><strong>Uso:</strong> For transportation or events</p>
                    <p class="example"><strong>Esempio:</strong> "Ho comprato i biglietti per il concerto." (I bought tickets for the concert.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>l'uno (as each)</h3>
                    <p><strong>Definizione:</strong> Each, one another</p>
                    <p><strong>Uso:</strong> Refers to individual items or reciprocal action</p>
                    <p class="example"><strong>Esempio:</strong> "I ragazzi si aiutano l'uno l'altro." (The boys help each other.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>benestare</h3>
                    <p><strong>Definizione:</strong> Approval, consent</p>
                    <p><strong>Uso:</strong> Formal permission or agreement</p>
                    <p class="example"><strong>Esempio:</strong> "Ho bisogno del tuo benestare per procedere." (I need your approval to proceed.)</p>
                </div>
            </div>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section">
            <div class="word-bank">
                <h3><i class="fas fa-list-ul icon"></i> Banca Parole - Scegli tra queste parole:</h3>
                <div class="word-options">
                    <span class="word-option">facile</span>
                    <span class="word-option">secondo me</span>
                    <span class="word-option">vivere</span>
                    <span class="word-option">gradi</span>
                    <span class="word-option">vacanza</span>
                    <span class="word-option">quindi</span>
                    <span class="word-option">perché</span>
                    <span class="word-option">preferito</span>
                    <span class="word-option">penna</span>
                    <span class="word-option">affollata</span>
                    <span class="word-option">per quanto tempo</span>
                    <span class="word-option">volo</span>
                    <span class="word-option">biglietti</span>
                    <span class="word-option">l'uno</span>
                    <span class="word-option">benestare</span>
                </div>
            </div>

            <div id="fill-gaps-container">
                <!-- Sentences will be dynamically inserted here -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Controlla Risposte</button>
            <button class="reset-btn" onclick="resetFillBlanks()">Ricomincia</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #008C45;">Abbina le parole con le loro definizioni</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Parole Italiane</h4>
                    <div class="match-item" data-word="facile" onclick="selectMatch(this)">facile</div>
                    <div class="match-item" data-word="secondo me" onclick="selectMatch(this)">secondo me</div>
                    <div class="match-item" data-word="vivere" onclick="selectMatch(this)">vivere</div>
                    <div class="match-item" data-word="gradi" onclick="selectMatch(this)">gradi</div>
                    <div class="match-item" data-word="vacanza" onclick="selectMatch(this)">vacanza</div>
                    <div class="match-item" data-word="quindi" onclick="selectMatch(this)">quindi</div>
                    <div class="match-item" data-word="perché" onclick="selectMatch(this)">perché</div>
                    <div class="match-item" data-word="preferito" onclick="selectMatch(this)">preferito</div>
                    <div class="match-item" data-word="penna" onclick="selectMatch(this)">penna</div>
                    <div class="match-item" data-word="affollata" onclick="selectMatch(this)">affollata</div>
                    <div class="match-item" data-word="per quanto tempo" onclick="selectMatch(this)">per quanto tempo</div>
                    <div class="match-item" data-word="volo" onclick="selectMatch(this)">volo</div>
                    <div class="match-item" data-word="biglietti" onclick="selectMatch(this)">biglietti</div>
                    <div class="match-item" data-word="l'uno" onclick="selectMatch(this)">l'uno</div>
                    <div class="match-item" data-word="benestare" onclick="selectMatch(this)">benestare</div>
                </div>
                <div class="match-column">
                    <h4>Definizioni</h4>
                    <div id="meanings-container">
                        <!-- Meanings will be dynamically inserted here -->
                    </div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
            <button class="check-btn" onclick="checkMatching()">Controlla Abbinamenti</button>
            <button class="reset-btn" onclick="resetMatching()">Ricomincia</button>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Domanda 1: Cosa significa "facile"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Difficile</div>
                    <div class="option" onclick="selectOption(this, true)">Facile, semplice</div>
                    <div class="option" onclick="selectOption(this, false)">Interessante</div>
                    <div class="option" onclick="selectOption(this, false)">Noioso</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 2: Quando usiamo "secondo me"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Per descrivere fatti</div>
                    <div class="option" onclick="selectOption(this, true)">Per esprimere opinioni personali</div>
                    <div class="option" onclick="selectOption(this, false)">Per fare domande</div>
                    <div class="option" onclick="selectOption(this, false)">Per dare ordini</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 3: Cosa significa "vivere"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Studiare</div>
                    <div class="option" onclick="selectOption(this, true)">Vivere, abitare</div>
                    <div class="option" onclick="selectOption(this, false)">Viaggiare</div>
                    <div class="option" onclick="selectOption(this, false)">Lavorare</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 4: Cosa sono i "gradi"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Monete</div>
                    <div class="option" onclick="selectOption(this, true)">Gradi di temperatura o voti</div>
                    <div class="option" onclick="selectOption(this, false)">Libri</div>
                    <div class="option" onclick="selectOption(this, false)">Città</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 5: Cosa significa "vacanza"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Lavoro</div>
                    <div class="option" onclick="selectOption(this, true)">Vacanza, ferie</div>
                    <div class="option" onclick="selectOption(this, false)">Scuola</div>
                    <div class="option" onclick="selectOption(this, false)">Casa</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Domanda 6: Quando usiamo "quindi"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Per fare domande</div>
                    <div class="option" onclick="selectOption(this, true)">Per mostrare conseguenza</div>
                    <div class="option" onclick="selectOption(this, false)">Per esprimere dubbi</div>
                    <div class="option" onclick="selectOption(this, false)">Per descrivere il passato</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Domanda 7: Cosa significa "perché" (as because)?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Quando</div>
                    <div class="option" onclick="selectOption(this, true)">Perché (motivo)</div>
                    <div class="option" onclick="selectOption(this, false)">Dove</div>
                    <div class="option" onclick="selectOption(this, false)">Chi</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Domanda 8: Cosa significa "preferito"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Nuovo</div>
                    <div class="option" onclick="selectOption(this, true)">Preferito, favorito</div>
                    <div class="option" onclick="selectOption(this, false)">Vecchio</div>
                    <div class="option" onclick="selectOption(this, false)">Strano</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Domanda 9: Cos'è una "penna"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Un libro</div>
                    <div class="option" onclick="selectOption(this, true)">Uno strumento per scrivere</div>
                    <div class="option" onclick="selectOption(this, false)">Un computer</div>
                    <div class="option" onclick="selectOption(this, false)">Una sedia</div>
                </div>
                <div class="feedback" id="mc-feedback-9" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Domanda 10: Cosa significa "affollata"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Vuota</div>
                    <div class="option" onclick="selectOption(this, true)">Affollata, piena di gente</div>
                    <div class="option" onclick="selectOption(this, false)">Pulita</div>
                    <div class="option" onclick="selectOption(this, false)">Sporca</div>
                </div>
                <div class="feedback" id="mc-feedback-10" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Domanda 11: Cosa chiediamo con "per quanto tempo"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Il prezzo</div>
                    <div class="option" onclick="selectOption(this, true)">La durata del tempo</div>
                    <div class="option" onclick="selectOption(this, false)">La distanza</div>
                    <div class="option" onclick="selectOption(this, false)">La quantità</div>
                </div>
                <div class="feedback" id="mc-feedback-11" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Domanda 12: Cos'è un "volo"?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Un treno</div>
                    <div class="option" onclick="selectOption(this, true)">Un viaggio in aereo</div>
                    <div class="option" onclick="selectOption(this, false)">Una nave</div>
                    <div class="option" onclick="selectOption(this, false)">Un'automobile</div>
                </div>
                <div class="feedback" id="mc-feedback-12" style="display: none;"></div>
            </div>
            
            <button class="reset-btn" onclick="resetMultipleChoice()">Ricomincia</button>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Track correct answers for each section
        let fillBlanksCorrect = 0;
        let matchingCorrect = 0;
        let multipleChoiceCorrect = 0;
        
        // Definitions for the matching game
        const definitions = [
            { meaning: "facile", text: "Easy, simple" },
            { meaning: "secondo me", text: "In my opinion" },
            { meaning: "vivere", text: "To live" },
            { meaning: "gradi", text: "Degrees (temperature), grades" },
            { meaning: "vacanza", text: "Vacation, holiday" },
            { meaning: "quindi", text: "Therefore, so" },
            { meaning: "perché", text: "Because" },
            { meaning: "preferito", text: "Favorite, preferred" },
            { meaning: "penna", text: "Pen" },
            { meaning: "affollata", text: "Crowded" },
            { meaning: "per quanto tempo", text: "For how long" },
            { meaning: "volo", text: "Flight" },
            { meaning: "biglietti", text: "Tickets" },
            { meaning: "l'uno", text: "Each, one another" },
            { meaning: "benestare", text: "Approval, consent" }
        ];

        // Sentences for the fill-in-the-blanks game
        const sentences = [
            { text: "Questo esercizio è molto <input type='text' class='fill-blank' data-answer='facile' placeholder='risposta'>.", answer: "facile" },
            { text: "<input type='text' class='fill-blank' data-answer='secondo me' placeholder='risposta'>, il film era bellissimo.", answer: "secondo me" },
            { text: "Voglio <input type='text' class='fill-blank' data-answer='vivere' placeholder='risposta'> in Italia.", answer: "vivere" },
            { text: "Oggi ci sono 25 <input type='text' class='fill-blank' data-answer='gradi' placeholder='risposta'>.", answer: "gradi" },
            { text: "Andiamo in <input type='text' class='fill-blank' data-answer='vacanza' placeholder='risposta'> al mare.", answer: "vacanza" },
            { text: "Piove, <input type='text' class='fill-blank' data-answer='quindi' placeholder='risposta'> resto a casa.", answer: "quindi" },
            { text: "Studio italiano <input type='text' class='fill-blank' data-answer='perché' placeholder='risposta'> mi piace la cultura.", answer: "perché" },
            { text: "La pizza è il mio cibo <input type='text' class='fill-blank' data-answer='preferito' placeholder='risposta'>.", answer: "preferito" },
            { text: "Ho bisogno di una <input type='text' class='fill-blank' data-answer='penna' placeholder='risposta'> per scrivere.", answer: "penna" },
            { text: "La spiaggia era molto <input type='text' class='fill-blank' data-answer='affollata' placeholder='risposta'>.", answer: "affollata" },
            { text: "<input type='text' class='fill-blank' data-answer='per quanto tempo' placeholder='risposta'> resti in Italia?", answer: "per quanto tempo" },
            { text: "Il <input type='text' class='fill-blank' data-answer='volo' placeholder='risposta'> per Roma dura due ore.", answer: "volo" },
            { text: "Ho comprato i <input type='text' class='fill-blank' data-answer='biglietti' placeholder='risposta'> per il concerto.", answer: "biglietti" },
            { text: "I ragazzi si aiutano <input type='text' class='fill-blank' data-answer='l&apos;uno' placeholder='risposta'> l'altro.", answer: "l'uno" },
            { text: "Ho bisogno del tuo <input type='text' class='fill-blank' data-answer='benestare' placeholder='risposta'> per procedere.", answer: "benestare" }
        ];

        // Function to shuffle array (Fisher-Yates algorithm)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Initialize the fill-in-the-blanks game with shuffled sentences
        function initFillBlanks() {
            const container = document.getElementById('fill-gaps-container');
            container.innerHTML = '';
            
            // Shuffle the sentences
            const shuffledSentences = shuffleArray([...sentences]);
            
            // Create and append the sentence elements
            shuffledSentences.forEach((sentence, index) => {
                const div = document.createElement('div');
                div.className = 'question';
                div.innerHTML = `
                    <h3>Domanda ${index + 1}:</h3>
                    <p>${sentence.text}</p>
                    <div class="feedback" id="feedback-${index + 1}" style="display: none;"></div>
                `;
                container.appendChild(div);
            });
        }

        // Initialize the matching game with shuffled definitions
        function initMatchingGame() {
            const meaningsContainer = document.getElementById('meanings-container');
            meaningsContainer.innerHTML = '';
            
            // Shuffle the definitions
            const shuffledDefinitions = shuffleArray([...definitions]);
            
            // Create and append the definition elements
            shuffledDefinitions.forEach(def => {
                const div = document.createElement('div');
                div.className = 'match-item';
                div.setAttribute('data-meaning', def.meaning);
                div.onclick = function() { selectMatch(this); };
                div.textContent = def.text;
                meaningsContainer.appendChild(div);
            });
        }

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If showing fill-in-the-blanks section, reinitialize with shuffled sentences
            if (sectionId === 'fill-gaps') {
                initFillBlanks();
            }
            
            // If showing matching section, reinitialize with shuffled definitions
            if (sectionId === 'matching') {
                initMatchingGame();
                // Reset matching game state
                document.querySelectorAll('.match-item').forEach(item => {
                    item.classList.remove('selected', 'matched');
                });
                selectedWord = null;
                selectedMeaning = null;
                matchedPairs = [];
                document.getElementById('matching-feedback').style.display = 'none';
            }
            
            // Update score display
            updateScore();
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Show feedback for each question
                const feedback = document.getElementById(`feedback-${index+1}`);
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Corretto!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Sbagliato. La risposta corretta è: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            fillBlanksCorrect = correctCount;
            updateScore();
        }

        function resetFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            blanks.forEach(blank => {
                blank.value = '';
                blank.classList.remove('correct', 'incorrect');
            });
            
            const feedbacks = document.querySelectorAll('#fill-gaps .feedback');
            feedbacks.forEach(feedback => {
                feedback.style.display = 'none';
            });
            
            fillBlanksCorrect = 0;
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Abbinamento corretto!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === definitions.length) {
                    feedback.textContent = '🎉 Congratulazioni! Hai abbinato tutti i termini correttamente!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Abbinamento sbagliato. Riprova.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            matchingCorrect = matchedPairs.length;
            updateScore();
        }

        function checkMatching() {
            const allMatches = document.querySelectorAll('.match-item[data-word]');
            let allCorrect = true;
            
            allMatches.forEach(item => {
                if (!item.classList.contains('matched')) {
                    allCorrect = false;
                }
            });
            
            const feedback = document.getElementById('matching-feedback');
            if (allCorrect) {
                feedback.textContent = '🎉 Eccellente! Tutti gli abbinamenti sono corretti!';
                feedback.className = 'feedback correct';
            } else {
                const unmatchedCount = definitions.length - matchedPairs.length;
                feedback.textContent = `Hai ${unmatchedCount} coppia${unmatchedCount !== 1 ? 'e' : ''} non abbinate. Continua a provare!`;
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }

        function resetMatching() {
            document.querySelectorAll('.match-item').forEach(item => {
                item.classList.remove('selected', 'matched');
            });
            
            initMatchingGame();
            
            selectedWord = null;
            selectedMeaning = null;
            matchedPairs = [];
            
            document.getElementById('matching-feedback').style.display = 'none';
            
            matchingCorrect = 0;
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
                opt.classList.remove('correct');
                opt.classList.remove('incorrect');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Corretto!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Sbagliato. Riprova.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            
            // Count correct answers in multiple choice
            multipleChoiceCorrect = document.querySelectorAll('#multiple-choice .option.correct.selected').length;
            updateScore();
        }

        function resetMultipleChoice() {
            document.querySelectorAll('#multiple-choice .option').forEach(option => {
                option.classList.remove('selected', 'correct', 'incorrect');
            });
            
            document.querySelectorAll('#multiple-choice .feedback').forEach(feedback => {
                feedback.style.display = 'none';
            });
            
            multipleChoiceCorrect = 0;
            updateScore();
        }

        function updateScore() {
            // Total score
            score = fillBlanksCorrect + matchingCorrect + multipleChoiceCorrect;
            document.getElementById('score').textContent = score;
        }
        
        // Initialize the page
        window.onload = function() {
            initFillBlanks();
            initMatchingGame();
        }
    </script>
</body>
</html>
