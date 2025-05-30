<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego de Semiótica: Verdadero o Falso</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.min.js"></script>
    <style>
        body {
            font-family: "Inter", sans-serif;
            background-color: #e0f7fa; /* Un color de fondo azul verdoso muy claro y agradable */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh; /* Asegura que ocupe toda la altura de la ventana */
            margin: 0;
            padding: 20px; /* Espaciado general */
            box-sizing: border-box;
        }
        .game-container {
            background-color: #ffffff;
            border-radius: 1.5rem; /* Bordes redondeados */
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06); /* Sombra más sutil y minimalista */
            padding: 2.5rem; /* Buen padding */
            max-width: 600px; /* Ancho máximo para mejor lectura */
            width: 100%;
            text-align: center;
            display: flex;
            flex-direction: column;
            gap: 1.5rem; /* Espacio entre elementos */
        }
        .question-text {
            font-size: 1.5rem; /* Texto de pregunta más grande */
            font-weight: 600; /* Semibold */
            color: #2d3748; /* Color de texto oscuro */
            margin-bottom: 1.5rem;
        }
        .option-button {
            background-color: #e0f2fe; /* Color de botón azul claro por defecto */
            color: #1e40af; /* Texto de botón azul oscuro */
            padding: 1rem 1.5rem;
            border-radius: 0.75rem; /* Bordes redondeados */
            cursor: pointer;
            transition: all 0.2s ease-in-out;
            font-weight: 600; /* Texto más negrita */
            border: 2px solid transparent; /* Borde para el hover */
        }
        .option-button:hover {
            background-color: #bfdbfe; /* Un poco más oscuro al pasar el ratón */
            border-color: #60a5fa; /* Borde azul al pasar el ratón */
        }
        .option-button.correct {
            background-color: #48bb78; /* Verde para correcto */
            color: white;
            border-color: #38a169;
        }
        .option-button.incorrect {
            background-color: #f56565; /* Rojo para incorrecto */
            color: white;
            border-color: #e53e3e;
        }
        .feedback-message {
            font-size: 1.125rem; /* Texto de feedback */
            font-weight: 500;
            margin-top: 1rem;
        }
        .next-button, .restart-button {
            background-color: #4299e1; /* Azul para botones de acción */
            color: white;
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem;
            cursor: pointer;
            transition: background-color 0.2s ease-in-out;
            font-weight: 600;
            margin-top: 1.5rem;
            border: none;
        }
        .next-button:hover, .restart-button:hover {
            background-color: #3182ce; /* Azul más oscuro al pasar el ratón */
        }
        .score-display {
            font-size: 1.75rem; /* Tamaño de la puntuación */
            font-weight: 700;
            color: #2d3748;
            margin-top: 2rem;
        }
        .explanation-text {
            font-size: 0.95rem;
            color: #4a5568;
            margin-top: 1rem;
            line-height: 1.5;
            text-align: left; /* Alineado a la izquierda para mejor lectura */
            padding: 0 1rem; /* Pequeño padding horizontal */
        }
        .review-section {
            margin-top: 2rem;
            text-align: left;
        }
        .review-item {
            margin-bottom: 1rem;
            padding: 1rem;
            background-color: #fef2f2; /* Fondo rojo muy claro para ítems de repaso */
            border-radius: 0.75rem;
            box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
        }
        .review-item p {
            margin-bottom: 0.5rem;
        }
        .review-item .question-text-review {
            font-weight: 600;
            color: #b91c1c; /* Rojo oscuro para la pregunta de repaso */
        }
        .review-item .correct-answer-text {
            font-weight: 500;
            color: #dc2626; /* Rojo para la respuesta correcta */
        }
        .review-item .explanation-text-review {
            font-size: 0.9rem;
            color: #4a5568;
        }
        .rating-section {
            margin-top: 2.5rem;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .stars-container {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        .star-button {
            background: none;
            border: none;
            font-size: 2rem;
            cursor: pointer;
            color: #cbd5e0; /* Gris claro por defecto */
            transition: color 0.2s ease-in-out;
        }
        .star-button.filled {
            color: #f6ad55; /* Dorado */
        }
        .congratulations-message {
            font-size: 2rem;
            font-weight: 700;
            color: #38a169; /* Verde fuerte */
            margin-top: 2rem;
        }
    </style>
</head>
<body>
    <div id="gameContainer" class="game-container">
        <h1 class="text-3xl font-bold text-gray-800 mb-6">Juego: Semiótica para Principiantes - ¿Verdadero o Falso?</h1>
        <p class="text-gray-600 mb-8">Lee cada afirmación y decide si es **Verdadera** o **Falsa**. ¡Luego, haz clic para descubrir la respuesta!</p>

        <div id="welcomeScreen" class="text-center">
            <h2 class="text-2xl font-bold text-gray-800 mb-4">¡Bienvenido al Juego de Semiótica!</h2>
            <p class="text-lg text-gray-700 mb-6">Pon a prueba tus conocimientos sobre los signos y el significado.</p>
            <button id="startButton" class="next-button">Comenzar Juego</button>
        </div>

        <div id="questionDisplay" class="hidden">
            <p id="questionCounter" class="text-sm text-gray-500 mb-2"></p>
            <p id="currentScore" class="text-sm text-gray-500 mb-4"></p>
            <p id="questionText" class="question-text"></p>
            <div class="flex flex-col gap-4 mt-4">
                <button id="optionA" class="option-button" data-value="true">A. Verdadero</button>
                <button id="optionB" class="option-button" data-value="false">B. Falso</button>
            </div>
            <div id="feedback" class="feedback-message mt-4"></div>
            <p id="explanation" class="explanation-text hidden"></p>
            <button id="nextButton" class="next-button hidden">Siguiente Pregunta</button>
        </div>

        <div id="scoreDisplay" class="hidden">
            <p class="score-display">¡Juego Terminado!</p>
            <p id="finalScore" class="text-xl font-semibold text-gray-700"></p>
            <p id="congratulations" class="congratulations-message hidden"></p>
            <div id="reviewSection" class="review-section"></div>

            <div class="rating-section">
                <p class="text-lg font-semibold text-gray-800">¿Qué te pareció el juego?</p>
                <div id="starsContainer" class="stars-container">
                    <button class="star-button" data-rating="1">★</button>
                    <button class="star-button" data-rating="2">★</button>
                    <button class="star-button" data-rating="3">★</button>
                    <button class="star-button" data-rating="4">★</button>
                    <button class="star-button" data-rating="5">★</button>
                </div>
                <p id="ratingFeedback" class="text-md text-gray-600 mt-2"></p>
            </div>

            <button id="restartButton" class="restart-button">Reiniciar Juego</button>
        </div>
    </div>

    <script>
        // Array de objetos que contiene las preguntas, opciones y la respuesta correcta.
        // Se han añadido 2 preguntas nuevas para un total de 8.
        const questions = [
            {
                question: "Según Ferdinand de Saussure, la relación entre el significante y el significado es natural e inherente, no una convención social.",
                correctAnswer: false, // Falso
                explanation: "Saussure postuló que la relación entre el significante (la imagen acústica) y el significado (el concepto) es **arbitraria**, es decir, no hay una conexión natural, sino que es una convención social y cultural."
            },
            {
                question: "La 'langue' de Saussure se refiere al acto individual y concreto de habla o escritura.",
                correctAnswer: false, // Falso
                explanation: "La 'langue' es el sistema abstracto y colectivo de la lengua, el conjunto de reglas y convenciones compartidas. El acto individual de habla es la 'parole'."
            },
            {
                question: "La lingüística diacrónica estudia la lengua en un momento específico, sin considerar su evolución histórica.",
                correctAnswer: false, // Falso
                explanation: "La lingüística **sincrónica** estudia la lengua en un momento específico. La lingüística **diacrónica** estudia la evolución y los cambios de la lengua a lo largo del tiempo."
            },
            {
                question: "En la tríada de Peirce, el 'Representamen' es el efecto o la comprensión que el signo produce en la mente de quien lo interpreta.",
                correctAnswer: false, // Falso
                explanation: "El 'Representamen' es el signo en sí mismo, la forma perceptible. El efecto o la comprensión es el 'Interpretante'."
            },
            {
                question: "El 'Objeto Dinámico' de Peirce es la representación del objeto tal como es presentado por el signo.",
                correctAnswer: false, // Falso
                explanation: "El 'Objeto Dinámico' es el objeto real, independiente del signo. La representación del objeto presentada por el signo es el 'Objeto Inmediato'."
            },
            {
                question: "Según Roland Barthes, un 'mito' es un sistema de significación de segundo orden que toma un signo existente y lo convierte en un nuevo significante para transmitir un significado cultural o ideológico.",
                correctAnswer: true, // Verdadero
                explanation: "Barthes analizó cómo los mitos funcionan como 'metasistemas' que construyen significados adicionales sobre signos ya establecidos, a menudo para naturalizar ideologías."
            },
            {
                question: "La notación S/s de Saussure se utiliza para representar la distinción entre 'lengua' (langue) y 'habla' (parole).",
                correctAnswer: false, // Falso
                explanation: "La notación S/s de Saussure representa la relación entre el **Significante (S)** y el **Significado (s)**, los dos componentes inseparables del signo lingüístico."
            },
            {
                question: "Según la perspectiva saussureana, el 'yo' (la subjetividad) es una entidad fija e independiente del sistema lingüístico y su enunciación.",
                correctAnswer: false, // Falso
                explanation: "Para la semiótica influenciada por Saussure, el 'yo' es una categoría lingüística definida por el acto de enunciación. No es una entidad fija, sino que se construye y se posiciona a través del lenguaje."
            }
        ];

        // Variables de estado del juego
        let currentQuestionIndex = 0;
        let score = 0;
        let answeredThisQuestion = false; // Bandera para evitar múltiples respuestas por pregunta
        let shuffledQuestions = []; // Array para almacenar las preguntas mezcladas
        let incorrectAnswers = []; // Array para almacenar las preguntas incorrectas para revisión

        // Referencias a elementos del DOM
        const welcomeScreen = document.getElementById('welcomeScreen');
        const startButton = document.getElementById('startButton');
        const questionDisplay = document.getElementById('questionDisplay');
        const questionCounter = document.getElementById('questionCounter');
        const currentScore = document.getElementById('currentScore');
        const questionText = document.getElementById('questionText');
        const optionA = document.getElementById('optionA');
        const optionB = document.getElementById('optionB');
        const feedback = document.getElementById('feedback');
        const explanation = document.getElementById('explanation');
        const nextButton = document.getElementById('nextButton');
        const scoreDisplay = document.getElementById('scoreDisplay');
        const finalScore = document.getElementById('finalScore');
        const congratulationsMessage = document.getElementById('congratulations'); // Mensaje de felicitaciones
        const reviewSection = document.getElementById('reviewSection');
        const restartButton = document.getElementById('restartButton');
        const starsContainer = document.getElementById('starsContainer'); // Contenedor de estrellas
        const ratingFeedback = document.getElementById('ratingFeedback'); // Feedback de calificación

        // Inicializar sintetizadores de Tone.js para los sonidos
        // Sonido de Correcto + Aplausos (simulado con un acorde mayor ascendente)
        const correctPolySynth = new Tone.PolySynth(Tone.Synth, {
            oscillator: { type: "triangle" },
            envelope: { attack: 0.05, decay: 0.3, sustain: 0.1, release: 0.5 },
        }).toDestination();

        // Sonido de Incorrecto + Tristeza (simulado con un acorde menor descendente o disonante)
        const incorrectPolySynth = new Tone.PolySynth(Tone.Synth, {
            oscillator: { type: "sine" },
            envelope: { attack: 0.05, decay: 0.5, sustain: 0.1, release: 1 },
        }).toDestination();

        // Función para reproducir sonido de respuesta correcta (acorde de C mayor)
        function playCorrectSound() {
            correctPolySynth.triggerAttackRelease(["C5", "E5", "G5"], "0.5");
        }

        // Función para reproducir sonido de respuesta incorrecta (acorde de C menor invertido o disonante)
        function playIncorrectSound() {
            incorrectPolySynth.triggerAttackRelease(["C3", "G#2", "D2"], "0.8"); // Sonido más disonante y descendente
        }

        // Función para mezclar un array (algoritmo de Fisher-Yates)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]]; // Intercambia elementos
            }
        }

        // Función para cargar una pregunta
        function loadQuestion() {
            // Reinicia el estado para la nueva pregunta
            answeredThisQuestion = false;
            feedback.textContent = '';
            explanation.classList.add('hidden');
            nextButton.classList.add('hidden');
            optionA.classList.remove('correct', 'incorrect');
            optionB.classList.remove('correct', 'incorrect');
            optionA.disabled = false; // Habilita los botones
            optionB.disabled = false;

            // Muestra la pregunta actual del array mezclado
            const q = shuffledQuestions[currentQuestionIndex];
            questionText.textContent = q.question;

            // Actualiza el contador de preguntas y la puntuación en tiempo real
            questionCounter.textContent = `Pregunta ${currentQuestionIndex + 1} de ${shuffledQuestions.length}`;
            currentScore.textContent = `Puntuación: ${score}`;
        }

        // Función para verificar la respuesta del usuario
        function checkAnswer(selectedAnswer) {
            // Si ya se respondió esta pregunta, no hacer nada
            if (answeredThisQuestion) return;
            answeredThisQuestion = true;

            const q = shuffledQuestions[currentQuestionIndex];
            const isCorrect = (selectedAnswer === q.correctAnswer);

            // Aplica la clase CSS correspondiente a la opción seleccionada
            if (selectedAnswer) { // Si el usuario eligió "Verdadero"
                optionA.classList.add(isCorrect ? 'correct' : 'incorrect');
                optionB.classList.add(isCorrect ? 'incorrect' : 'correct'); // La otra opción se marca como incorrecta
            } else { // Si el usuario eligió "Falso"
                optionB.classList.add(isCorrect ? 'correct' : 'incorrect');
                optionA.classList.add(isCorrect ? 'incorrect' : 'correct'); // La otra opción se marca como incorrecta
            }


            if (isCorrect) {
                score++;
                feedback.textContent = '¡Correcto!';
                feedback.style.color = '#48bb78'; // Verde
                playCorrectSound(); // Reproduce sonido de correcto
            } else {
                feedback.textContent = 'Incorrecto.';
                feedback.style.color = '#f56565'; // Rojo
                playIncorrectSound(); // Reproduce sonido de incorrecto
                // Añade la pregunta incorrecta al array para revisión
                incorrectAnswers.push({
                    question: q.question,
                    explanation: q.explanation,
                    correctAnswer: q.correctAnswer ? 'Verdadero' : 'Falso' // Para mostrar la respuesta correcta
                });
            }
            // Actualiza la puntuación en tiempo real después de responder
            currentScore.textContent = `Puntuación: ${score}`;

            // Muestra la explicación y el botón de siguiente
            explanation.textContent = q.explanation;
            explanation.classList.remove('hidden');
            nextButton.classList.remove('hidden');
            optionA.disabled = true; // Deshabilita los botones después de responder
            optionB.disabled = true;
        }

        // Función para mostrar el resultado final del juego
        function showResults() {
            questionDisplay.classList.add('hidden');
            scoreDisplay.classList.remove('hidden');
            finalScore.textContent = `Tu puntuación final es: ${score} de ${shuffledQuestions.length}`;

            // Muestra mensaje de felicitaciones si todo es correcto
            if (score === shuffledQuestions.length) {
                congratulationsMessage.textContent = '¡Felicitaciones! ¡Has respondido todas las preguntas correctamente!';
                congratulationsMessage.classList.remove('hidden');
            } else {
                congratulationsMessage.classList.add('hidden');
            }


            // Limpia la sección de revisión antes de añadir nuevos elementos
            reviewSection.innerHTML = '';
            if (incorrectAnswers.length > 0) {
                const reviewTitle = document.createElement('h3');
                reviewTitle.className = 'text-2xl font-bold text-gray-800 mb-4';
                reviewTitle.textContent = 'Preguntas a Repasar:';
                reviewSection.appendChild(reviewTitle);

                incorrectAnswers.forEach((item, index) => {
                    const reviewItem = document.createElement('div');
                    reviewItem.className = 'review-item';
                    reviewItem.innerHTML = `
                        <p class="question-text-review">Pregunta ${index + 1}:</p>
                        <p class="text-gray-800 mb-2">${item.question}</p>
                        <p class="correct-answer-text">Respuesta Correcta: ${item.correctAnswer}</p>
                        <p class="explanation-text-review mt-1">${item.explanation}</p>
                    `;
                    reviewSection.appendChild(reviewItem);
                });
            } else {
                // Si no hay incorrectas, y no felicitaciones, no mostrar mensaje de repaso
                if (score !== shuffledQuestions.length) { // Solo si no es 100% y no hay errores
                    const perfectMessage = document.createElement('p');
                    perfectMessage.className = 'text-xl font-semibold text-green-700';
                    perfectMessage.textContent = '¡Excelente! No tienes preguntas que repasar. ¡Buen trabajo!';
                    reviewSection.appendChild(perfectMessage);
                }
            }

            // Reinicia el estado de las estrellas de calificación
            ratingFeedback.textContent = '';
            document.querySelectorAll('.star-button').forEach(star => star.classList.remove('filled'));
        }

        // Función para reiniciar el juego
        function restartGame() {
            currentQuestionIndex = 0;
            score = 0;
            incorrectAnswers = []; // Reinicia las preguntas incorrectas
            scoreDisplay.classList.add('hidden');
            welcomeScreen.classList.remove('hidden'); // Vuelve a la pantalla de bienvenida
            congratulationsMessage.classList.add('hidden'); // Oculta felicitaciones al reiniciar
        }

        // --- Event Listeners ---

        // Iniciar el juego
        startButton.addEventListener('click', () => {
            // Es necesario iniciar el contexto de audio de Tone.js con una interacción del usuario
            Tone.start();
            welcomeScreen.classList.add('hidden'); // Oculta la pantalla de bienvenida
            questionDisplay.classList.remove('hidden'); // Muestra la pantalla de preguntas
            shuffledQuestions = [...questions]; // Copia el array original
            shuffleArray(shuffledQuestions); // Mezcla las preguntas
            loadQuestion();
        });

        // Manejar clics en las opciones
        optionA.addEventListener('click', () => checkAnswer(true)); // Botón "Verdadero"
        optionB.addEventListener('click', () => checkAnswer(false)); // Botón "Falso"

        // Pasar a la siguiente pregunta o mostrar resultados
        nextButton.addEventListener('click', () => {
            currentQuestionIndex++;
            if (currentQuestionIndex < shuffledQuestions.length) {
                loadQuestion();
            } else {
                showResults();
            }
        });

        // Reiniciar el juego
        restartButton.addEventListener('click', restartGame);

        // Calificación del juego (estrellas)
        starsContainer.addEventListener('click', (event) => {
            if (event.target.classList.contains('star-button')) {
                const rating = parseInt(event.target.dataset.rating);
                document.querySelectorAll('.star-button').forEach(star => {
                    if (parseInt(star.dataset.rating) <= rating) {
                        star.classList.add('filled');
                    } else {
                        star.classList.remove('filled');
                    }
                });
                ratingFeedback.textContent = `¡Gracias por tu calificación de ${rating} estrella${rating > 1 ? 's' : ''}!`;
            }
        });
    </script>
</body>
</html>
