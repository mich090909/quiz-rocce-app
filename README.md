<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Roccioso Interattivo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f9ff; /* Light blue background */
            color: #1e3a8a; /* Dark blue text */
        }
        .kahoot-container {
            max-width: 900px;
            margin: 2rem auto;
            padding: 1.5rem;
            background-color: #ffffff;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border: 2px solid #bfdbfe; /* Light blue border */
        }
        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            font-weight: 600;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease-in-out;
            border: none;
        }
        .btn-primary {
            background-color: #3b82f6; /* Blue */
            color: white;
        }
        .btn-primary:hover:not(:disabled) {
            background-color: #2563eb;
        }
        .btn-primary:disabled {
            background-color: #9ca3af; /* Gray for disabled */
            cursor: not-allowed;
        }
        .btn-secondary {
            background-color: #60a5fa; /* Lighter blue */
            color: white;
        }
        .btn-secondary:hover {
            background-color: #3b82f6;
        }
        .btn-outline {
            background-color: transparent;
            border: 2px solid #3b82f6;
            color: #3b82f6;
        }
        .btn-outline:hover {
            background-color: #3b82f6;
            color: white;
        }
        .answer-btn {
            display: flex;
            align-items: center;
            justify-content: flex-start; /* Align text to the left */
            width: 100%;
            padding: 1rem; /* Increased padding for better touch */
            margin-bottom: 0.75rem;
            font-size: 1rem; /* Slightly larger font */
            border-radius: 8px;
            border: 2px solid transparent;
            transition: all 0.2s ease-in-out;
        }
        .answer-btn.selected {
            border-color: #1e3a8a; /* Dark blue border for selected */
            transform: scale(1.02);
        }
        .answer-btn.correct {
            background-color: #10b981 !important; /* Green */
            color: white !important;
            border-color: #059669 !important;
        }
        .answer-btn.incorrect {
            background-color: #ef4444 !important; /* Red */
            color: white !important;
            border-color: #dc2626 !important;
        }
        .answer-btn:disabled {
            opacity: 0.7;
            cursor: not-allowed;
        }
        .timer {
            font-size: 1.5rem;
            font-weight: 700;
            color: #ef4444; /* Red for timer */
        }
        .question-header {
            background-color: #dbeafe; /* Very light blue */
            padding: 1rem;
            border-radius: 8px;
            margin-bottom: 1rem;
        }
        .progress-bar-container {
            width: 100%;
            background-color: #e0e0e0;
            border-radius: 9999px;
            height: 10px;
            margin-top: 0.5rem;
            overflow: hidden;
        }
        .progress-bar {
            height: 100%;
            /* background-color set by JS */
            width: 0%;
            border-radius: 9999px;
            transition: width 0.2s linear, background-color 0.5s ease;
        }

        /* Colors for answer buttons */
        .answer-colors {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 0.75rem;
        }
        .answer-btn-red { background-color: #fee2e2; color: #991b1b; } .answer-btn-red:hover:not(:disabled) { background-color: #fecaca; }
        .answer-btn-blue { background-color: #dbeafe; color: #1e40af; } .answer-btn-blue:hover:not(:disabled) { background-color: #bfdbfe; }
        .answer-btn-green { background-color: #d1fae5; color: #065f46; } .answer-btn-green:hover:not(:disabled) { background-color: #a7f3d0; }
        .answer-btn-yellow { background-color: #fef3c7; color: #92400e; } .answer-btn-yellow:hover:not(:disabled) { background-color: #fde68a; }

        .icon-shape {
            width: 30px;
            height: 30px;
            margin-right: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 1rem;
            border-radius: 4px;
        }
        .shape-triangle { background-color: #ef4444; clip-path: polygon(50% 0%, 0% 100%, 100% 100%); } /* Red */
        .shape-diamond { background-color: #3b82f6; clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%); } /* Blue */
        .shape-circle { background-color: #f59e0b; border-radius: 50%; } /* Yellow */
        .shape-square { background-color: #10b981; } /* Green */

        #allResultsContainer li {
            border-bottom: 1px solid #e5e7eb; /* Tailwind gray-200 */
        }
        #allResultsContainer li:last-child {
            border-bottom: none;
        }
        .input-field {
            width: 100%;
            padding: 0.75rem 1rem;
            border: 1px solid #d1d5db;
            border-radius: 6px;
            margin-bottom: 1rem;
            font-size: 1rem;
        }
        .input-field:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.3);
        }
        .error-message {
            color: #ef4444; /* Red for error messages */
            font-size: 0.875rem;
            margin-top: -0.5rem;
            margin-bottom: 1rem;
        }

    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

    <div class="kahoot-container w-full">

        <div id="userInfo" class="text-xs text-gray-500 mb-2 text-right">User ID: <span id="userIdDisplay">Caricamento...</span></div>

        <div id="startScreen" class="text-center">
            <h1 class="text-4xl font-bold mb-4 text-blue-700">Quiz Roccioso! <i class="fas fa-mountain text-yellow-500"></i></h1>
            <p class="text-lg mb-6 text-gray-600">Metti alla prova la tua conoscenza del mondo delle rocce!</p>
            
            <input type="text" id="studentNameInput" class="input-field max-w-md mx-auto" placeholder="Inserisci il tuo nome...">
            <div id="nameError" class="error-message hidden">Per favore, inserisci il tuo nome per iniziare.</div>

            <button id="startButton" class="btn btn-primary text-xl px-8 py-4" disabled>Avvia il Gioco!</button>
            <button id="showAllResultsButton" class="btn btn-outline mt-4 block mx-auto">Visualizza Tutti i Risultati</button>
        </div>

        <div id="gameScreen" class="hidden">
            <div class="question-header">
                <div class="flex justify-between items-center mb-2">
                    <span id="questionCounter" class="text-sm font-semibold text-blue-600">Domanda 1/15</span>
                    <div class="timer" id="timerDisplay">30</div>
                </div>
                <h2 id="questionText" class="text-xl font-semibold text-center">Testo della domanda...</h2>
                <div class="progress-bar-container">
                    <div id="timeProgressBar" class="progress-bar"></div> </div>
            </div>

            <div id="answerOptions" class="answer-colors mt-6">
                </div>
            <button id="nextQuestionButton" class="btn btn-secondary w-full mt-6 hidden">Prossima Domanda</button>
        </div>

        <div id="endScreen" class="hidden text-center">
            <h2 class="text-3xl font-bold mb-4 text-blue-700">Quiz Terminato! <i class="fas fa-trophy text-yellow-500"></i></h2>
            <p id="playerNameEnd" class="text-xl font-semibold mb-1"></p>
            <p id="finalScore" class="text-2xl mb-1"></p>
            <p id="correctAnswersSummary" class="text-lg mb-4"></p>
            <p id="feedbackMessage" class="text-lg mb-6"></p>
            <button id="restartButton" class="btn btn-primary text-lg">Rigioca</button>
        </div>

        <div id="allResultsScreen" class="hidden mt-8">
            <h2 class="text-3xl font-bold mb-6 text-center text-blue-700">Risultati Complessivi Studenti <i class="fas fa-clipboard-list"></i></h2>
            <div id="allResultsContainer" class="space-y-3 bg-gray-50 p-4 rounded-lg max-h-96 overflow-y-auto">
                </div>
            <button id="backToStartButton" class="btn btn-secondary mt-6 w-full">Torna alla Home</button>
        </div>

    </div>

    <script type="module">
        // Import Firebase SDK modules
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged, setPersistence, browserLocalPersistence } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, getDocs, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Firebase configuration (provided by Canvas environment)
        const firebaseConfigJson = typeof __firebase_config !== 'undefined' ? __firebase_config : '{}';
        let firebaseConfig = {};
        try {
            firebaseConfig = JSON.parse(firebaseConfigJson);
            if (Object.keys(firebaseConfig).length === 0) {
                 console.warn("Firebase config is empty. Using placeholder for local testing. Firestore features will not work.");
                 // Example placeholder: firebaseConfig = { apiKey: "AIza...", authDomain: "...", projectId: "..." };
            }
        } catch (e) {
            console.error("Error parsing Firebase config:", e);
            // Handle error or use a default config for local testing
        }
        
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-quiz-app-id'; // App ID from Canvas

        // Firebase app, auth, and db instances
        let app;
        let auth;
        let db;
        let currentUserId = null; // To store the current user's ID
        let currentStudentName = ""; // Variable to store student's name

        // Initialize Firebase and handle authentication
        async function initializeFirebase() {
            try {
                // Check if Firebase config is available
                if (Object.keys(firebaseConfig).length === 0) {
                    console.error("Firebase configuration is missing. App functionality requiring Firebase will be limited.");
                    document.getElementById('userIdDisplay').textContent = 'Firebase non configurato';
                    return Promise.resolve(null); // Resolve with null if config is missing
                }
                app = initializeApp(firebaseConfig);
                auth = getAuth(app);
                db = getFirestore(app);
                setLogLevel('debug'); // Enable Firestore debug logging

                // Optional: Persist authentication state
                await setPersistence(auth, browserLocalPersistence);

                // Listen for authentication state changes
                return new Promise((resolve) => {
                    onAuthStateChanged(auth, async (user) => {
                        if (user) {
                            // User is signed in
                            console.log("User is signed in:", user.uid);
                            currentUserId = user.uid;
                        } else {
                            // User is signed out, attempt to sign in
                            console.log("User is signed out. Attempting to sign in...");
                            try {
                                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                                    // Sign in with custom token if available (Canvas environment)
                                    await signInWithCustomToken(auth, __initial_auth_token);
                                    console.log("Signed in with custom token.");
                                } else {
                                    // Fallback to anonymous sign-in
                                    await signInAnonymously(auth);
                                    console.log("Signed in anonymously.");
                                }
                                currentUserId = auth.currentUser ? auth.currentUser.uid : crypto.randomUUID();
                            } catch (error) {
                                console.error("Error during sign-in:", error);
                                currentUserId = crypto.randomUUID(); // Fallback to a random ID
                            }
                        }
                        // Display User ID (or N/A if not available)
                        document.getElementById('userIdDisplay').textContent = currentUserId || 'N/A';
                        resolve(currentUserId);
                    });
                });
            } catch (error) {
                console.error("Firebase initialization error:", error);
                currentUserId = crypto.randomUUID(); // Fallback to a random ID
                document.getElementById('userIdDisplay').textContent = currentUserId + ' (Firebase init failed)';
                return Promise.resolve(currentUserId); // Still resolve, but with a fallback ID
            }
        }
        
        // Call initializeFirebase when the script loads
        initializeFirebase().then(uid => {
            console.log("Firebase initialized, User ID:", uid);
            // Any logic that depends on Firebase being initialized can go here
        });

        // Quiz questions data
        const questions = [
             { question: "Cos'è un minerale secondo la definizione geologica?", options: ["Un aggregato di rocce diverse.","Un corpo inorganico, naturale, solido, con composizione chimica definita e struttura cristallina.","Qualsiasi sostanza solida trovata sulla Terra.","Un liquido solidificato rapidamente."], answer: 1 },
             { question: "Qual è l'unità strutturale fondamentale di tutti i minerali silicati?", options: ["La molecola d'acqua (H2O).","Il cubo di salgemma (NaCl).","Il tetraedro silicio-ossigeno (SiO4)4-.","Un atomo di carbonio."], answer: 2 },
             { question: "Le rocce magmatiche intrusive (o plutoniche) si formano da:", options: ["Solidificazione rapida della lava in superficie.","Solidificazione lenta del magma in profondità.","Accumulo di ceneri vulcaniche.","Trasformazione di rocce preesistenti."], answer: 1 },
             { question: "Una struttura porfirica in una roccia magmatica indica:", options: ["Un raffreddamento estremamente rapido e solo vetro.","Un raffreddamento molto lento con cristalli tutti grandi.","Un raffreddamento in due o più fasi (lento in profondità, poi più rapido).","Una roccia formata da frammenti di altre rocce."], answer: 2 },
             { question: "Il granito è l'equivalente intrusivo di quale roccia effusiva?", options: ["Basalto", "Riolite", "Andesite", "Gabbro"], answer: 1 },
             { question: "Quale processo NON è una tappa fondamentale del processo sedimentario?", options: ["Degradazione meteorica ed erosione", "Trasporto", "Fusione parziale", "Sedimentazione e Diagenesi"], answer: 2 },
             { question: "Le rocce clastiche sono classificate principalmente in base a:", options: ["Colore dei clasti", "Dimensione dei clasti", "Presenza di fossili", "Composizione chimica del cemento"], answer: 1 },
             { question: "Il salgemma e il gesso sono esempi di rocce sedimentarie formatesi per:", options: ["Accumulo di scheletri di organismi.","Precipitazione da acque in evaporazione (evaporiti).","Cementazione di sabbie vulcaniche.","Metamorfismo di rocce argillose."], answer: 1 },
             { question: "Il metamorfismo avviene quando una roccia preesistente (protolito) subisce trasformazioni:", options: ["Allo stato fuso, per poi solidificare.","Allo stato solido, per variazioni di T e/o P.","Solo per azione dell'acqua superficiale.","Esclusivamente per frantumazione meccanica."], answer: 1 },
             { question: "Quale tipo di metamorfismo è tipicamente associato alla formazione delle catene montuose (orogenesi)?", options: ["Metamorfismo di Contatto", "Metamorfismo Dinamico", "Metamorfismo Regionale", "Metamorfismo da Impatto"], answer: 2 },
             { question: "La scistosità è una struttura tipica delle rocce metamorfiche che indica:", options: ["Presenza di grossi cristalli equidimensionali.","Facile divisibilità in lastre per orientazione di minerali lamellari.","Completa fusione e ricristallizzazione.","Presenza di numerosi fossili."], answer: 1 },
             { question: "Il marmo deriva dal metamorfismo di quale roccia preesistente?", options: ["Arenaria", "Argilla", "Basalto", "Calcare o dolomia"], answer: 3 },
             { question: "Il ciclo litogenetico descrive come:", options: ["Solo le rocce magmatiche si trasformano.","I materiali della litosfera vengano continuamente trasformati e riciclati.","Le montagne si formano e vengono erose.","I minerali si formano esclusivamente da magma."], answer: 1 },
             { question: "I minerali silicati sialici (o felsici) sono generalmente:", options: ["Ricchi di Fe e Mg, di colore scuro e densità alta.","Ricchi di Si e Al, di colore chiaro e densità bassa.","Composti solo da silicio.","Trovati solo nelle rocce sedimentarie."], answer: 1 },
             { question: "Cosa sono le migmatiti?", options: ["Rocce sedimentarie molto fini.","Un tipo di roccia magmatica intrusiva.","Rocce formate da fusione parziale (ultrametamorfismo), con parti fuse e non fuse.","Rocce formate da impatti meteoritici."], answer: 2 }
        ];

        // DOM element references
        const startScreen = document.getElementById('startScreen');
        const gameScreen = document.getElementById('gameScreen');
        const endScreen = document.getElementById('endScreen');
        const allResultsScreen = document.getElementById('allResultsScreen');
        const studentNameInput = document.getElementById('studentNameInput');
        const nameErrorElement = document.getElementById('nameError');
        const startButton = document.getElementById('startButton');
        const questionCounterElement = document.getElementById('questionCounter');
        const questionTextElement = document.getElementById('questionText');
        const answerOptionsElement = document.getElementById('answerOptions');
        const nextQuestionButton = document.getElementById('nextQuestionButton');
        const playerNameEndElement = document.getElementById('playerNameEnd');
        const finalScoreElement = document.getElementById('finalScore');
        const correctAnswersSummaryElement = document.getElementById('correctAnswersSummary');
        const feedbackMessageElement = document.getElementById('feedbackMessage');
        const restartButton = document.getElementById('restartButton');
        const timerDisplayElement = document.getElementById('timerDisplay');
        const timeProgressBarElement = document.getElementById('timeProgressBar');
        const showAllResultsButton = document.getElementById('showAllResultsButton');
        const backToStartButton = document.getElementById('backToStartButton');

        // Game state variables
        let currentQuestionIndex = 0;
        let score = 0;
        let correctAnswersCount = 0; 
        let timer; // Timer interval ID
        const MAX_TIME_PER_QUESTION = 30; // seconds for each question
        let timeLeft = MAX_TIME_PER_QUESTION; 
        const answerButtonClasses = ['answer-btn-red', 'answer-btn-blue', 'answer-btn-yellow', 'answer-btn-green'];
        const answerShapes = ['shape-triangle', 'shape-diamond', 'shape-square', 'shape-circle']; 

        // Event listener for student name input
        studentNameInput.addEventListener('input', () => {
            if (studentNameInput.value.trim() !== "") {
                startButton.disabled = false; // Enable start button if name is entered
                nameErrorElement.classList.add('hidden'); // Hide error message
            } else {
                startButton.disabled = true; // Disable start button if name is empty
            }
        });

        // Event listener for start button
        startButton.addEventListener('click', () => {
            currentStudentName = studentNameInput.value.trim();
            if (currentStudentName === "") {
                nameErrorElement.classList.remove('hidden'); // Show error if name is empty
                startButton.disabled = true;
                return;
            }
            nameErrorElement.classList.add('hidden');
            startGame();
        });

        // Event listener for next question button
        nextQuestionButton.addEventListener('click', loadNextQuestion);
        
        // Event listener for restart button
        restartButton.addEventListener('click', () => {
            studentNameInput.value = ""; // Clear name input
            startButton.disabled = true; // Disable start button
            // Switch UI to the start screen
            endScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
            // startGame() will be called by the startButton click logic after name input
        });

        // Event listener for showing all results
        showAllResultsButton.addEventListener('click', async () => {
            // Hide other screens and show the results screen
            startScreen.classList.add('hidden');
            gameScreen.classList.add('hidden');
            endScreen.classList.add('hidden');
            allResultsScreen.classList.remove('hidden');
            await loadAndDisplayAllResults(); // Load and display results from Firestore
        });

        // Event listener for going back to the start screen from results
        backToStartButton.addEventListener('click', () => {
            allResultsScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
        });

        // Function to start the game
        function startGame() {
            currentQuestionIndex = 0;
            score = 0;
            correctAnswersCount = 0; 
            // Switch UI to game screen
            startScreen.classList.add('hidden');
            endScreen.classList.add('hidden');
            allResultsScreen.classList.add('hidden');
            gameScreen.classList.remove('hidden');
            nextQuestionButton.classList.add('hidden'); // Hide next button initially
            displayQuestion(); // Display the first question
        }

        // Function to display the current question
        function displayQuestion() {
            resetTimer(); // Reset and start the timer for the new question
            const currentQuestion = questions[currentQuestionIndex];
            questionCounterElement.textContent = `Domanda ${currentQuestionIndex + 1}/${questions.length}`;
            questionTextElement.textContent = currentQuestion.question;
            answerOptionsElement.innerHTML = ''; // Clear previous answer options

            // Create and append answer buttons
            currentQuestion.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.classList.add('btn', 'answer-btn', answerButtonClasses[index % answerButtonClasses.length]);
                
                const shapeDiv = document.createElement('div');
                shapeDiv.classList.add('icon-shape', answerShapes[index % answerShapes.length]);
                button.appendChild(shapeDiv);

                const textSpan = document.createElement('span');
                textSpan.textContent = option;
                button.appendChild(textSpan);
                
                button.addEventListener('click', () => selectAnswer(index, timeLeft)); // Pass timeLeft to calculate score
                answerOptionsElement.appendChild(button);
            });
            startTimer(); // Start the countdown timer
        }

        // Function to handle answer selection
        function selectAnswer(selectedIndex, timeRemaining) {
            clearInterval(timer); // Stop the timer for the current question
            const currentQuestion = questions[currentQuestionIndex];
            const buttons = answerOptionsElement.querySelectorAll('.answer-btn');
            
            let questionPoints = 0;
            if (selectedIndex === currentQuestion.answer) {
                correctAnswersCount++;
                // Calculate points: 500 base + up to 500 time bonus
                const basePoints = 500;
                const timeBonus = Math.round(500 * (timeRemaining / MAX_TIME_PER_QUESTION));
                questionPoints = basePoints + timeBonus;
                score += questionPoints;
            }

            // Visual feedback for selected, correct, and incorrect answers
            buttons.forEach((btn, idx) => {
                btn.disabled = true; // Disable all answer buttons
                if (idx === currentQuestion.answer) {
                    btn.classList.add('correct'); // Highlight correct answer
                }
                if (idx === selectedIndex && idx !== currentQuestion.answer) {
                    btn.classList.add('incorrect'); // Highlight incorrect selection
                }
                 if (idx === selectedIndex) {
                    btn.classList.add('selected'); // Mark the selected button
                }
            });
            
            nextQuestionButton.classList.remove('hidden'); // Show the next question button
        }

        // Function to load the next question or end the game
        function loadNextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < questions.length) {
                displayQuestion();
                nextQuestionButton.classList.add('hidden'); // Hide next button for the new question
                 // Reset button states for the new question
                 const buttons = answerOptionsElement.querySelectorAll('.answer-btn');
                 buttons.forEach(btn => {
                    btn.classList.remove('correct', 'incorrect', 'selected');
                    btn.disabled = false;
                 });
            } else {
                showEndScreen(); // If no more questions, show the end screen
            }
        }

        // Function to display the end screen with results
        async function showEndScreen() {
            gameScreen.classList.add('hidden');
            endScreen.classList.remove('hidden');
            playerNameEndElement.textContent = `Complimenti, ${currentStudentName}!`;
            finalScoreElement.textContent = `Punteggio Finale: ${score} punti`;
            correctAnswersSummaryElement.textContent = `Risposte Corrette: ${correctAnswersCount}/${questions.length}`;
            
            // Provide feedback based on performance
            let message = "";
            const percentageCorrect = (correctAnswersCount / questions.length) * 100;
            if (percentageCorrect >= 90) message = "Eccezionale! Sei un vero geologo! 🌋";
            else if (percentageCorrect >= 70) message = "Ottimo lavoro! Conosci bene le rocce! 💎";
            else if (percentageCorrect >= 50) message = "Buon tentativo! Continua a studiare! 📚";
            else message = "Non mollare! Riprova e impara di più! 💪";
            feedbackMessageElement.textContent = message;

            await saveScoreToFirestore(); // Save the score to Firestore
        }

        // Function to save the score to Firestore
        async function saveScoreToFirestore() {
            // Ensure Firestore DB and User ID are available
            if (!db || !currentUserId) {
                console.error("Firestore DB or User ID not available for saving score. Ensure Firebase is initialized correctly.");
                // Optionally, inform the user that the score could not be saved.
                return;
            }
            // Define the path for the results collection in Firestore
            // Public path allows all authenticated users to write (as per typical Kahoot-like game)
            // Ensure your Firestore rules allow this. For this example, using a public path.
            const resultsCollectionPath = `/artifacts/${appId}/public/data/quizResults`; 
            try {
                await addDoc(collection(db, resultsCollectionPath), {
                    studentName: currentStudentName,
                    userId: currentUserId,
                    score: score, // This now includes time bonus
                    correctAnswersCount: correctAnswersCount,
                    totalQuestions: questions.length,
                    percentage: parseFloat(((correctAnswersCount / questions.length) * 100).toFixed(2)),
                    timestamp: new Date().toISOString() // ISO string for easy sorting
                });
                console.log("Score saved to Firestore.");
            } catch (error) {
                console.error("Error saving score to Firestore:", error);
                // Optionally, inform the user about the error.
            }
        }

        // Function to load and display all results from Firestore
        async function loadAndDisplayAllResults() {
            const allResultsContainer = document.getElementById('allResultsContainer');
            allResultsContainer.innerHTML = '<p class="text-center py-4">Caricamento risultati...</p>'; // Loading message

            if (!db) {
                allResultsContainer.innerHTML = '<p class="text-center text-red-500 py-4">Errore: Database non inizializzato. Assicurati che la configurazione di Firebase sia corretta.</p>';
                return;
            }

            const resultsCollectionPath = `/artifacts/${appId}/public/data/quizResults`;
            try {
                const querySnapshot = await getDocs(collection(db, resultsCollectionPath));
                if (querySnapshot.empty) {
                    allResultsContainer.innerHTML = '<p class="text-center py-4">Nessun risultato ancora registrato.</p>';
                    return;
                }

                let resultsHTML = '<ul class="divide-y divide-gray-200">';
                const resultsData = querySnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));
                
                // Sort results: by score (descending), then by timestamp (newest first for ties)
                resultsData.sort((a, b) => {
                    if (b.score !== a.score) {
                        return b.score - a.score;
                    }
                    return new Date(b.timestamp) - new Date(a.timestamp);
                });

                // Build HTML for each result entry
                resultsData.forEach(docData => {
                    resultsHTML += `
                        <li class="py-3 px-1 hover:bg-gray-100 rounded-md">
                            <p class="text-md font-semibold text-blue-700">${docData.studentName || 'Nome non specificato'}</p>
                            <p class="text-sm text-gray-700">Punteggio: ${docData.score} punti</p>
                            <p class="text-sm text-gray-600">Corrette: ${docData.correctAnswersCount}/${docData.totalQuestions} (${docData.percentage}%)</p>
                            <p class="text-xs text-gray-500">User ID: ${docData.userId}</p>
                            <p class="text-xs text-gray-500">Data: ${new Date(docData.timestamp).toLocaleString('it-IT', { dateStyle: 'short', timeStyle: 'short' })}</p>
                        </li>`;
                });
                resultsHTML += '</ul>';
                allResultsContainer.innerHTML = resultsHTML;

            } catch (error) {
                console.error("Error fetching all results:", error);
                allResultsContainer.innerHTML = `<p class="text-center text-red-500 py-4">Errore nel caricamento dei risultati: ${error.message}. Controlla la console per dettagli.</p>`;
            }
        }

        // Function to start the question timer
        function startTimer() {
            timeLeft = MAX_TIME_PER_QUESTION;
            timerDisplayElement.textContent = timeLeft;
            timeProgressBarElement.style.width = '100%';
            timeProgressBarElement.style.backgroundColor = '#10b981'; // Start with green

            timer = setInterval(() => {
                timeLeft--;
                timerDisplayElement.textContent = timeLeft;
                const progressWidth = (timeLeft / MAX_TIME_PER_QUESTION) * 100;
                timeProgressBarElement.style.width = `${progressWidth}%`;

                // Change progress bar color based on time left
                if (timeLeft <= MAX_TIME_PER_QUESTION * 0.33) timeProgressBarElement.style.backgroundColor = '#ef4444'; // Red
                else if (timeLeft <= MAX_TIME_PER_QUESTION * 0.66) timeProgressBarElement.style.backgroundColor = '#f59e0b'; // Yellow
                else timeProgressBarElement.style.backgroundColor = '#10b981'; // Green


                if (timeLeft <= 0) {
                    clearInterval(timer); // Stop timer when time is up
                    handleTimeOut(); // Handle timeout
                }
            }, 1000);
        }
        
        // Function to handle question timeout
        function handleTimeOut() {
            const buttons = answerOptionsElement.querySelectorAll('.answer-btn');
            buttons.forEach(btn => btn.disabled = true); // Disable answer buttons
            
            const currentQuestion = questions[currentQuestionIndex];
            // Highlight the correct answer even on timeout
            if (buttons.length > currentQuestion.answer && buttons[currentQuestion.answer]) { 
                 buttons[currentQuestion.answer].classList.add('correct');
            }

            questionTextElement.textContent = "Tempo scaduto! La risposta corretta era evidenziata.";
            nextQuestionButton.classList.remove('hidden'); // Show next button
             // No points are awarded for a timeout
        }

        // Function to reset the timer
        function resetTimer() {
            clearInterval(timer);
        }

    </script>
</body>
</html>
