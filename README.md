<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cuestionario sobre Hipertensión Arterial Sistémica</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Energetic & Playful -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f8f9fa; }
        .bg-vibrant { background-color: #419D78; color: white; }
        .text-vibrant { color: #419D78; }
        .border-vibrant { border-color: #419D78; }
        .button-primary { background-color: #FF8C61; color: white; transition: background-color 0.3s ease; }
        .button-primary:hover { background-color: #E0A458; }
        .correct { background-color: #419D78; color: white; }
        .incorrect { background-color: #FF6B6B; color: white; }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

    <div class="max-w-xl w-full bg-white p-8 rounded-xl shadow-lg border-t-8 border-vibrant">
        <!-- Contenedor del Cuestionario -->
        <div id="quiz-container">
            <h1 class="text-3xl font-bold text-center text-vibrant mb-2">Cuestionario: HAS</h1>
            <p class="text-center text-gray-600 mb-6">Evalúa tus conocimientos sobre el protocolo PRONAM.</p>

            <!-- Sección de Pregunta -->
            <div id="question-section" class="mb-6">
                <p id="question-text" class="text-xl font-semibold mb-4"></p>
                <div id="answers-container" class="space-y-3">
                    <!-- Los botones de respuesta se generarán aquí con JavaScript -->
                </div>
            </div>
            
            <!-- Botones de Navegación -->
            <div class="flex justify-end mt-6">
                <button id="next-button" class="button-primary font-bold py-2 px-6 rounded-lg shadow-md" style="display: none;">Siguiente</button>
            </div>
        </div>

        <!-- Sección de Resultados -->
        <div id="results-section" class="text-center" style="display: none;">
            <h2 class="text-3xl font-bold text-vibrant mb-4">Resultados</h2>
            <p id="score-text" class="text-2xl font-semibold mb-2"></p>
            <p id="feedback-text" class="text-gray-600 mb-6"></p>
            <button id="restart-button" class="button-primary font-bold py-2 px-6 rounded-lg shadow-md">Volver a Intentarlo</button>
        </div>
    </div>

    <script>
        // Array de objetos para las preguntas y respuestas
        const questions = [
            {
                question: "¿Cuál es el rango de presión arterial sistólica (PAS) que define la Hipertensión Arterial Sistémica (HAS) en consultorio?",
                answers: ["Menor de 120 mmHg", "120-139 mmHg", "≥140 mmHg", "130-139 mmHg"],
                correct: 2 
            },
            {
                question: "¿Cuál de los siguientes es un paso fundamental para una medición correcta de la presión arterial, según el protocolo?",
                answers: ["Medir solo en un brazo.", "Fumar 10 minutos antes de la medición.", "Utilizar un brazalete que sea del tamaño adecuado.", "Sentarse erguido en la silla."],
                correct: 2
            },
            {
                question: "¿Cuál es la combinación de fármacos recomendada para iniciar la terapia dual en un paciente con HAS?",
                answers: ["Betabloqueador + Diurético", "IECA/ARA + Calcioantagonista", "Diurético + Espironolactona", "IECA/ARA + Betabloqueador"],
                correct: 1
            },
            {
                question: "¿Qué tipo de daño se asocia con la hipertensión en el cerebro?",
                answers: ["Hipertrofia ventricular", "Esclerosis glomerular", "Retinopatía hipertensiva", "Enfermedad vascular cerebral (EVC)"],
                correct: 3
            },
            {
                question: "Según el protocolo, ¿cuál es la meta de presión arterial para la mayoría de los pacientes?",
                answers: ["Menos de 140/90 mmHg", "Menos de 130/80 mmHg", "Menos de 120/70 mmHg", "Menos de 110/60 mmHg"],
                correct: 1
            }
        ];

        // Elementos del DOM
        const quizContainer = document.getElementById('quiz-container');
        const resultsSection = document.getElementById('results-section');
        const questionText = document.getElementById('question-text');
        const answersContainer = document.getElementById('answers-container');
        const nextButton = document.getElementById('next-button');
        const restartButton = document.getElementById('restart-button');
        const scoreText = document.getElementById('score-text');
        const feedbackText = document.getElementById('feedback-text');

        // Variables de estado
        let currentQuestionIndex = 0;
        let score = 0;

        // Función para mostrar la pregunta actual
        function showQuestion() {
            // Reiniciar la visualización
            answersContainer.innerHTML = '';
            nextButton.style.display = 'none';

            // Obtener la pregunta actual
            const currentQuestion = questions[currentQuestionIndex];
            questionText.textContent = `${currentQuestionIndex + 1}. ${currentQuestion.question}`;

            // Crear botones para cada respuesta
            currentQuestion.answers.forEach((answer, index) => {
                const button = document.createElement('button');
                button.textContent = answer;
                button.classList.add('w-full', 'py-3', 'px-4', 'rounded-lg', 'text-left', 'border', 'border-gray-300', 'bg-gray-50', 'hover:bg-gray-200', 'transition', 'duration-200');
                
                button.addEventListener('click', () => selectAnswer(button, index));
                answersContainer.appendChild(button);
            });
        }

        // Función para manejar la selección de una respuesta
        function selectAnswer(selectedButton, selectedIndex) {
            // Deshabilitar todos los botones después de la selección
            Array.from(answersContainer.children).forEach(button => {
                button.disabled = true;
            });

            const currentQuestion = questions[currentQuestionIndex];
            
            // Verificar si la respuesta es correcta
            if (selectedIndex === currentQuestion.correct) {
                score++;
                selectedButton.classList.add('correct');
            } else {
                selectedButton.classList.add('incorrect');
                // Mostrar la respuesta correcta
                answersContainer.children[currentQuestion.correct].classList.add('correct');
            }
            
            nextButton.style.display = 'block';
        }

        // Función para pasar a la siguiente pregunta o mostrar los resultados
        function nextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < questions.length) {
                showQuestion();
            } else {
                showResults();
            }
        }

        // Función para mostrar los resultados finales
        function showResults() {
            quizContainer.style.display = 'none';
            resultsSection.style.display = 'block';
            scoreText.textContent = `Obtuviste ${score} de ${questions.length} respuestas correctas.`;
            
            if (score === questions.length) {
                feedbackText.textContent = '¡Excelente trabajo! Has demostrado un dominio sobresaliente del tema.';
            } else if (score >= questions.length / 2) {
                feedbackText.textContent = 'Buen trabajo. Sigue practicando para dominar el tema por completo.';
            } else {
                feedbackText.textContent = 'Sigue estudiando el protocolo. ¡El conocimiento es la mejor herramienta!';
            }
        }

        // Función para reiniciar el cuestionario
        function restartQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            quizContainer.style.display = 'block';
            resultsSection.style.display = 'none';
            showQuestion();
        }

        // Event Listeners
        nextButton.addEventListener('click', nextQuestion);
        restartButton.addEventListener('click', restartQuiz);

        // Iniciar el cuestionario al cargar la página
        showQuestion();
    </script>
</body>
</html>
