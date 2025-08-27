<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cuestionario sobre Hipertensión Arterial Sistémica</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Energetic & Playful -->
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f8f9fa; }
        .bg-vibrant { background-color: #419D78; color: white; }
        .text-vibrant { color: #419D78; }
        .border-vibrant { border-color: #419D78; }
        .button-primary { background-color: #FF8C61; color: white; transition: background-color 0.3s ease; }
        .button-primary:hover { background-color: #E0A458; }
        .correct { background-color: #419D78; color: white; }
        .incorrect { background-color: #FF6B6B; color: white; }
        .feedback-container { padding: 1rem; border-radius: 0.5rem; }
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
            <div class="flex justify-between mt-6">
                <button id="prev-button" class="button-primary font-bold py-2 px-6 rounded-lg shadow-md" style="display: none;">Anterior</button>
                <button id="next-button" class="button-primary font-bold py-2 px-6 rounded-lg shadow-md" style="display: none;">Siguiente</button>
            </div>
        </div>

        <!-- Sección de Resultados -->
        <div id="results-section" class="text-center" style="display: none;">
            <h2 class="text-3xl font-bold text-vibrant mb-4">Resultados</h2>
            <p id="score-text" class="text-2xl font-semibold mb-2"></p>
            <p id="feedback-text" class="text-gray-600 mb-6"></p>
            <button id="restart-button" class="button-primary font-bold py-2 px-6 rounded-lg shadow-md">Volver a Intentarlo</button>
            
            <!-- Sección de Retroalimentación de Respuestas Incorrectas -->
            <div id="incorrect-answers-section" class="mt-8 text-left" style="display: none;">
                <h3 class="text-2xl font-bold text-vibrant mb-4">Revisión de Respuestas Incorrectas</h3>
                <div id="incorrect-answers-list" class="space-y-6">
                    <!-- El detalle de las preguntas incorrectas se generará aquí -->
                </div>
            </div>
        </div>
    </div>

    <script>
        const questions = [
            { question: "¿Qué tipo de documento es el PRONAM?", answers: ["Un artículo de investigación.", "Un libro de texto.", "Un instrumento resumido y basado en evidencia científica.", "Un protocolo de investigación."], correct: 2 },
            { question: "Según el documento, la HAS es un factor de riesgo importante para:", answers: ["Enfermedades infecciosas.", "Enfermedades cardiovasculares (CV).", "Enfermedades respiratorias.", "Enfermedades autoinmunes."], correct: 1 },
            { question: "¿Cuál es la prevalencia estimada de HAS a nivel mundial, según el estudio de la Carga Global de la Enfermedad 2019?", answers: ["15%", "20%", "31%", "45%"], correct: 2 },
            { question: "En México, la prevalencia de personas con HAS fue del:", answers: ["17.7%", "57.4%", "29.9%", "82.5%"], correct: 2 },
            { question: "El 90% de los enfermos con HAS presentan la forma:", answers: ["Secundaria.", "Relacionada con el estilo de vida.", "Esencial o primaria.", "Congénita."], correct: 2 },
            { question: "La hipertensión secundaria representa el:", answers: ["5% de los casos.", "10% de los casos.", "25% de los casos.", "50% de los casos."], correct: 1 },
            { question: "¿Cuál de los siguientes no es un daño a órgano blanco a largo plazo por HAS?", answers: ["Hipertrofia ventricular.", "Retinopatía hipertensiva.", "Atrofia muscular.", "Esclerosis glomerular."], correct: 2 },
            { question: "Según el documento, ¿cuál de los siguientes es un posible desenlace de la HAS si no se trata adecuadamente?", answers: ["Dermatitis.", "Insuficiencia cardíaca.", "Cataratas.", "Artritis."], correct: 1 },
            { question: "¿Con qué frecuencia se recomienda medir la PA en menores de 40 años sin factores de riesgo?", answers: ["Al menos 1 vez al año.", "Al menos cada 3 años.", "Una vez cada 5 años.", "Solo si presentan síntomas."], correct: 1 },
            { question: "¿Con qué frecuencia se recomienda medir la PA en mayores de 40 años?", answers: ["Al menos 1 vez al año.", "Al menos cada 3 años.", "Solo una vez en la vida.", "Solo si presentan síntomas."], correct: 0 },
            { question: "Antes de medir la PA, el paciente debe haber evitado el uso de estimulantes (cafeína y tabaco) al menos:", answers: ["10 minutos antes.", "30 minutos antes.", "1 hora antes.", "2 horas antes."], correct: 1 },
            { question: "Para la medición de PA en consultorio, se deben promediar las lecturas de las:", answers: ["Primeras dos mediciones.", "Últimas dos mediciones.", "Todas las mediciones.", "Solo la primera medición."], correct: 1 },
            { question: "¿Qué diferencia en la medición de PA entre ambos brazos justifica tomar más registros?", answers: ["Menor a 5 mmHg.", "Mayor a 10 mmHg.", "Mayor a 20 mmHg.", "Cualquier diferencia."], correct: 1 },
            { question: "¿Cuál es la caída de PA que define la hipotensión ortostática al ponerse de pie?", answers: ["PAS de 10 mmHg y PAD de 5 mmHg.", "PAS de 20 mmHg y PAD de 10 mmHg.", "PAS de 5 mmHg y PAD de 10 mmHg.", "PAS de 30 mmHg y PAD de 15 mmHg."], correct: 1 },
            { question: "Para la medición de PA en domicilio, se recomienda tomar los registros:", answers: ["Una vez al día.", "Solo por la mañana.", "2 veces por día (mañana y noche) de al menos 3 de 7 días/semana.", "Una vez a la semana."], correct: 2 },
            { question: "Según la Tabla 1, una PA sistólica de 135 mmHg en consultorio se clasifica como:", answers: ["Normal.", "Limítrofe.", "HAS.", "No hay suficiente información."], correct: 1 },
            { question: "Un paciente con HAS en MAPA durante la noche tiene una PA sistólica mayor a:", answers: ["140 mmHg.", "135 mmHg.", "130 mmHg.", "120 mmHg."], correct: 3 },
            { question: "La 'HAS de bata blanca' se define como:", answers: ["PA > 140 en consultorio y < 140 en casa.", "PA < 140 en consultorio y > 140 en casa.", "PA > 130 en consultorio.", "PA < 120 en casa."], correct: 0 },
            { question: "Según el documento, un examen general de orina (EGO) se busca:", answers: ["Hemoglobina.", "Glucosa.", "Microalbuminuria.", "Ácido úrico."], correct: 2 },
            { question: "¿Qué es el índice de Sokolow-Lyon para el crecimiento ventricular izquierdo?", answers: ["S en V1 + R en V5 > 35 mm.", "R en aVL > 20 mm.", "S en V3 + R en aVL > 28 mm.", "Masa del VI/talla > 50."], correct: 0 },
            { question: "¿Qué valor del índice tobillo/brazo indica daño a órgano blanco?", answers: ["< 0.9.", "> 1.1.", "1.0.", "Entre 0.9 y 1.1."], correct: 0 },
            { question: "El riesgo alto de HAS se asocia con:", answers: ["Diabetes tipo 2, DOB y más de 2 factores de riesgo.", "Enfermedad cardiovascular establecida.", "ERC.", "Solo un factor de riesgo."], correct: 0 },
            { question: "¿Cuál es la meta de riesgo crítico según la calculadora HEARTS?", answers: ["< 5%.", "10-20%.", "> 30%.", "5-10%."], correct: 2 },
            { question: "Las medidas no farmacológicas para el tratamiento de la HAS aplican a:", answers: ["Pacientes mayores de 60 años.", "Pacientes con riesgo alto.", "Todo paciente con HAS.", "Solo pacientes con sobrepeso."], correct: 2 },
            { question: "¿Cuántos minutos de ejercicio aeróbico se recomiendan por semana en HAS?", answers: ["150 min de moderada intensidad o 75 min de alta intensidad.", "30 min de baja intensidad.", "60 min de alta intensidad.", "200 min de moderada intensidad."], correct: 0 },
            { question: "La dieta baja en sal recomendada es de:", answers: ["2-5 g al día.", "5-10 g al día.", "Más de 10 g al día.", "Menos de 1 g al día."], correct: 0 },
            { question: "¿Qué tipo de bebidas se deben evitar, según el documento?", answers: ["Bebidas azucaradas y energéticas.", "Solo bebidas energéticas.", "Solo jugos naturales.", "Agua simple."], correct: 0 },
            { question: "La meta de peso es una reducción del:", answers: ["5-10% del peso inicial.", "15-20% del peso inicial.", "20-25% del peso inicial.", "No hay una meta específica."], correct: 0 },
            { question: "Según el algoritmo de tratamiento, si la PA es ≥140/90 mmHg, ¿qué tipo de terapia se recomienda?", answers: ["Monoterapia.", "Terapia dual.", "No requiere tratamiento.", "Solo medidas no farmacológicas."], correct: 1 },
            { question: "La monoterapia puede ser considerada en:", answers: ["Jóvenes.", "Pacientes con riesgo alto.", "Adultos mayores frágiles.", "Pacientes con cardiopatía isquémica."], correct: 2 },
            { question: "La urgencia hipertensiva se define con una PA de:", answers: ["≥160/100 mmHg.", "≥180/110 mmHg sin daño agudo a órganos.", "≥180/110 mmHg con daño a órganos.", "Cualquier PA elevada."], correct: 1 },
            { question: "En una emergencia hipertensiva, el objetivo es reducir la PA en:", answers: ["10% en 1-2 horas.", "25% en 1-2 horas.", "50% en 24 horas.", "75% en 12 horas."], correct: 1 },
            { question: "Según el documento, la meta de control de PA en jóvenes (18-40 años) es:", answers: ["<140/90 mmHg.", "<130/80 mmHg.", "<120/70 mmHg.", "<110/60 mmHg."], correct: 1 },
            { question: "Para adultos mayores de 85 años y frágiles, el tratamiento inicial es:", answers: ["Monoterapia.", "Terapia dual.", "Triple terapia.", "No se recomienda tratamiento farmacológico."], correct: 0 },
            { question: "En pacientes con diabetes, ¿qué se debe evitar en el tratamiento de la HAS?", answers: ["Calcioantagonistas.", "IECA/ARA.", "Diuréticos tiazídicos.", "Betabloqueadores."], correct: 2 },
            { question: "Para la cardiopatía isquémica crónica, ¿cuál es la combinación de tratamiento de primera elección?", answers: ["IECA/ARA + CaA.", "IECA/ARA + BB/CaA.", "Diurético + Espironolactona.", "BB + IECA/ARA."], correct: 1 },
            { question: "¿Cuál es un criterio para referir a un paciente a un especialista?", answers: ["Un solo factor de riesgo.", "Edad mayor a 60 años.", "Hipertensión arterial resistente.", "Presión arterial normal."], correct: 2 },
            { question: "Una urgencia hipertensiva requiere:", answers: ["Reducción de PA en 24-48 horas.", "Reducción inmediata de PA.", "Hospitalización inmediata.", "Solo un cambio de estilo de vida."], correct: 0 },
            { question: "En pacientes con cardiopatía isquémica, se debe considerar agregar:", answers: ["Ácido acetilsalicílico.", "Estatinas.", "Espironolactona si no se llega a la meta.", "Diuréticos."], correct: 2 },
            { question: "Según la tabla de dosis, la combinación de Olmesartán/Hidroclorotiazida se presenta en dosis de:", answers: ["16/12.5 mg.", "80/12.5 mg.", "300/12.5 mg.", "20/12.5 mg y 40/12.5 mg."], correct: 3 },
            { question: "¿Cuál es la masa del VI/superficie corporal que define el crecimiento ventricular izquierdo en hombres?", answers: ["< 95 g/m².", "≥ 115 g/m².", "≥ 150 g/m².", "No está especificado."], correct: 1 },
            { question: "Una TFG < 60 mL/min/m² es un indicador de:", answers: ["Daño renal moderado.", "Hipertensión enmascarada.", "Hipertensión sistólica pura.", "Hipotensión ortostática."], correct: 0 },
            { question: "¿Cuál es la meta de HbA1c en pacientes con HAS y diabetes?", answers: ["< 6.5%.", "< 7%.", "< 7.5%.", "< 8%."], correct: 1 },
            { question: "El índice cardiotorácico mayor a 0.5 es un indicador de:", answers: ["Hipertensión arterial.", "Cardiomegalia.", "Aterosclerosis.", "Retinopatía."], correct: 1 },
            { question: "La hipertensión sistólica pura se define por:", answers: ["PA sistólica > 140 y diastólica > 90.", "PA sistólica > 140 y diastólica < 90.", "PA sistólica < 140 y diastólica > 90.", "PA sistólica < 120 y diastólica < 79."], correct: 1 },
            { question: "Para los pacientes con sobrepeso, obesidad, diabetes y síndrome metabólico, la meta de control de PA es:", answers: ["< 140/90 mmHg.", "< 130/80 mmHg.", "< 120/70 mmHg.", "< 110/60 mmHg."], correct: 1 },
            { question: "La terapia dual se recomienda para:", answers: ["Evitar los efectos secundarios de las dosis altas.", "Solo en pacientes con riesgo bajo.", "Solo si la monoterapia falla.", "Solo si hay daño a órgano blanco."], correct: 0 },
            { question: "Según la tabla de dosis, ¿qué combinación de fármacos tiene la presentación de 5/10 mg?", answers: ["Candesartán/Hidroclorotiazida.", "Irbesartán/Amlodipino.", "Perindopril/Amlodipino.", "Olmesartán/Hidroclorotiazida."], correct: 2 },
            { question: "El cálculo del perfil lipídico incluye:", answers: ["Sodio, potasio y calcio.", "Colesterol total, c-LDL, c-HDL, triglicéridos.", "Glucosa y ácido úrico.", "Biometría hemática completa."], correct: 1 },
            { question: "El documento recomienda la toma de agua simple de:", answers: ["1 litro al día.", "1.5 a 2 litros al día.", "3 litros al día.", "No especifica."], correct: 1 }
        ];

        // Elementos del DOM
        const quizContainer = document.getElementById('quiz-container');
        const resultsSection = document.getElementById('results-section');
        const questionText = document.getElementById('question-text');
        const answersContainer = document.getElementById('answers-container');
        const nextButton = document.getElementById('next-button');
        const prevButton = document.getElementById('prev-button');
        const restartButton = document.getElementById('restart-button');
        const scoreText = document.getElementById('score-text');
        const feedbackText = document.getElementById('feedback-text');
        const incorrectAnswersSection = document.getElementById('incorrect-answers-section');
        const incorrectAnswersList = document.getElementById('incorrect-answers-list');

        // Variables de estado
        let currentQuestionIndex = 0;
        let score = 0;
        let userAnswers = new Array(questions.length).fill(null);

        // Función para mostrar la pregunta actual
        function showQuestion() {
            answersContainer.innerHTML = '';
            nextButton.textContent = (currentQuestionIndex === questions.length - 1) ? 'Finalizar' : 'Siguiente';
            
            const currentQuestion = questions[currentQuestionIndex];
            questionText.textContent = `${currentQuestionIndex + 1}. ${currentQuestion.question}`;

            currentQuestion.answers.forEach((answer, index) => {
                const button = document.createElement('button');
                button.textContent = answer;
                button.classList.add('w-full', 'py-3', 'px-4', 'rounded-lg', 'text-left', 'border', 'border-gray-300', 'bg-gray-50', 'hover:bg-gray-200', 'transition', 'duration-200');
                
                button.addEventListener('click', () => selectAnswer(button, index));
                answersContainer.appendChild(button);
            });
            
            // Si la pregunta ya ha sido respondida, restaurar la selección
            if (userAnswers[currentQuestionIndex] !== null) {
                Array.from(answersContainer.children)[userAnswers[currentQuestionIndex]].classList.add('bg-vibrant', 'text-white');
            }

            // Ocultar/mostrar botones de navegación
            prevButton.style.display = (currentQuestionIndex === 0) ? 'none' : 'block';
            nextButton.style.display = (userAnswers[currentQuestionIndex] !== null) ? 'block' : 'none';
        }

        // Función para manejar la selección de una respuesta
        function selectAnswer(selectedButton, selectedIndex) {
            userAnswers[currentQuestionIndex] = selectedIndex;

            // Eliminar clases de selección de todos los botones
            Array.from(answersContainer.children).forEach(button => {
                button.classList.remove('bg-vibrant', 'text-white');
            });

            // Añadir clase de selección al botón actual
            selectedButton.classList.add('bg-vibrant', 'text-white');
            nextButton.style.display = 'block';
        }

        // Función para pasar a la siguiente pregunta o mostrar los resultados
        function nextQuestion() {
            if (currentQuestionIndex < questions.length - 1) {
                currentQuestionIndex++;
                showQuestion();
            } else {
                showResults();
            }
        }

        // Función para ir a la pregunta anterior
        function prevQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                showQuestion();
            }
        }

        // Función para mostrar los resultados finales
        function showResults() {
            quizContainer.style.display = 'none';
            resultsSection.style.display = 'block';
            score = 0;
            userAnswers.forEach((answer, index) => {
                if (answer === questions[index].correct) {
                    score++;
                }
            });
            
            const percentage = (score / questions.length) * 100;
            scoreText.textContent = `Obtuviste ${score} de ${questions.length} respuestas correctas (${percentage.toFixed(2)}%).`;
            
            if (score === questions.length) {
                feedbackText.textContent = '¡Excelente trabajo! Has demostrado un dominio sobresaliente del tema.';
            } else if (score >= questions.length * 0.75) {
                feedbackText.textContent = 'Muy buen trabajo. Sigue así para dominar el tema por completo.';
            } else if (score >= questions.length / 2) {
                feedbackText.textContent = 'Buen intento. Revisa el material para mejorar tu puntuación.';
            } else {
                feedbackText.textContent = 'Sigue estudiando el protocolo. ¡El conocimiento es la mejor herramienta!';
            }
            
            showIncorrectAnswers();
        }

        // Función para mostrar las respuestas incorrectas con retroalimentación
        function showIncorrectAnswers() {
            incorrectAnswersList.innerHTML = '';
            let hasIncorrectAnswers = false;

            questions.forEach((question, index) => {
                if (userAnswers[index] !== question.correct) {
                    hasIncorrectAnswers = true;
                    
                    const feedbackItem = document.createElement('div');
                    feedbackItem.classList.add('bg-gray-100', 'p-4', 'rounded-lg', 'shadow');
                    
                    const questionP = document.createElement('p');
                    questionP.classList.add('font-semibold', 'text-lg', 'text-gray-800', 'mb-2');
                    questionP.textContent = `Pregunta ${index + 1}: ${question.question}`;
                    
                    const yourAnswerP = document.createElement('p');
                    yourAnswerP.classList.add('text-sm', 'text-gray-600', 'mb-1');
                    yourAnswerP.innerHTML = `<span class="font-bold text-red-500">Tu respuesta:</span> ${question.answers[userAnswers[index]]}`;
                    
                    const correctAnswerP = document.createElement('p');
                    correctAnswerP.classList.add('text-sm', 'text-gray-600');
                    correctAnswerP.innerHTML = `<span class="font-bold text-green-600">Respuesta correcta:</span> ${question.answers[question.correct]}`;

                    feedbackItem.appendChild(questionP);
                    feedbackItem.appendChild(yourAnswerP);
                    feedbackItem.appendChild(correctAnswerP);
                    incorrectAnswersList.appendChild(feedbackItem);
                }
            });

            if (hasIncorrectAnswers) {
                incorrectAnswersSection.style.display = 'block';
            } else {
                incorrectAnswersSection.style.display = 'none';
            }
        }

        // Función para reiniciar el cuestionario
        function restartQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            userAnswers = new Array(questions.length).fill(null);
            quizContainer.style.display = 'block';
            resultsSection.style.display = 'none';
            incorrectAnswersSection.style.display = 'none'; // Asegurarse de ocultar la sección
            showQuestion();
        }

        // Event Listeners
        nextButton.addEventListener('click', nextQuestion);
        prevButton.addEventListener('click', prevQuestion);
        restartButton.addEventListener('click', restartQuiz);

        // Iniciar el cuestionario al cargar la página
        showQuestion();
    </script>
</body>
</html>
