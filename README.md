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
        </div>
    </div>

    <script>
        const questions = [
            { question: "¿Cuál es el rango de presión arterial sistólica (PAS) que define la Hipertensión Arterial Sistémica (HAS) en consultorio?", answers: ["Menor de 120 mmHg", "120-139 mmHg", "≥140 mmHg", "130-139 mmHg"], correct: 2 },
            { question: "¿Cuál de los siguientes es un paso fundamental para una medición correcta de la presión arterial, según el protocolo?", answers: ["Medir solo en un brazo.", "Fumar 10 minutos antes de la medición.", "Utilizar un brazalete que sea del tamaño adecuado.", "Sentarse erguido en la silla."], correct: 2 },
            { question: "¿Cuál es la combinación de fármacos recomendada para iniciar la terapia dual en un paciente con HAS?", answers: ["Betabloqueador + Diurético", "IECA/ARA + Calcioantagonista", "Diurético + Espironolactona", "IECA/ARA + Betabloqueador"], correct: 1 },
            { question: "¿Qué tipo de daño se asocia con la hipertensión en el cerebro?", answers: ["Hipertrofia ventricular", "Esclerosis glomerular", "Retinopatía hipertensiva", "Enfermedad vascular cerebral (EVC)"], correct: 3 },
            { question: "Según el protocolo, ¿cuál es la meta de presión arterial para la mayoría de los pacientes?", answers: ["Menos de 140/90 mmHg", "Menos de 130/80 mmHg", "Menos de 120/70 mmHg", "Menos de 110/60 mmHg"], correct: 1 },
            { question: "¿Qué factor de riesgo cardiovascular NO es modificable?", answers: ["Tabaquismo", "Dislipidemia", "Diabetes Mellitus", "Genética"], correct: 3 },
            { question: "¿Qué significa la abreviatura IECA?", answers: ["Inhibidor de la Enzima Convertidora de Angiotensina", "Inhibidor de la Enzima Cardíaca Activa", "Inhibidor del Eje Cardiovascular Arterial", "Inhibidor de la Excreción de Calcio Arterial"], correct: 0 },
            { question: "¿Cuál es el tratamiento no farmacológico inicial recomendado para todos los pacientes con HAS?", answers: ["Medicación inmediata", "Dieta DASH", "Ejercicio de alta intensidad", "Cirugía"], correct: 1 },
            { question: "¿Qué órgano es comúnmente afectado por la hipertensión a largo plazo?", answers: ["Páncreas", "Hígado", "Riñones", "Tiroides"], correct: 2 },
            { question: "¿Qué indica una presión arterial de 130/85 mmHg?", answers: ["Normal", "Hipertensión Grado 1", "Hipertensión Grado 2", "Presión arterial elevada"], correct: 3 },
            { question: "¿Cuál es el objetivo de la terapia antihipertensiva?", answers: ["Eliminar por completo el medicamento.", "Alcanzar metas de presión arterial y prevenir daño a órganos diana.", "Curar la enfermedad.", "Reducir el riesgo de infarto."], correct: 1 },
            { question: "¿Qué riesgo se asocia con el tabaquismo en pacientes hipertensos?", answers: ["Disminución del riesgo cardiovascular", "Aumento del riesgo cardiovascular", "Mejora de la función renal", "Disminución del riesgo de EVC"], correct: 1 },
            { question: "¿Cuál es la primera línea de tratamiento para la HAS no complicada?", answers: ["Terapia triple", "Terapia cuádruple", "Terapia dual", "Monoterapia"], correct: 3 },
            { question: "¿Qué tipo de fármaco es el Losartán?", answers: ["Diurético", "Betabloqueador", "Inhibidor de la ECA", "Antagonista del receptor de angiotensina (ARA)"], correct: 3 },
            { question: "¿Qué es la monitorización ambulatoria de la presión arterial (MAPA)?", answers: ["Medición de PA en consultorio.", "Medición de PA en el hospital.", "Medición de PA en casa.", "Medición de PA durante 24 horas."], correct: 3 },
            { question: "¿Qué es la dislipidemia?", answers: ["Nivel alto de azúcar en sangre.", "Nivel anormal de lípidos en sangre.", "Inflamación de las articulaciones.", "Enfermedad del corazón."], correct: 1 },
            { question: "¿Qué tipo de ejercicio se recomienda para pacientes con HAS?", answers: ["Ejercicio aeróbico moderado", "Ejercicio anaeróbico de alta intensidad", "Levantamiento de pesas extremo", "Ejercicios de equilibrio"], correct: 0 },
            { question: "¿Cuál es la medida de PA que indica 'Hipertensión de bata blanca'?", answers: ["PA elevada en casa, normal en consultorio.", "PA normal en casa, elevada en consultorio.", "PA elevada en ambos entornos.", "PA normal en ambos entornos."], correct: 1 },
            { question: "¿Qué alimento se debe limitar en una dieta para HAS?", answers: ["Frutas y verduras", "Alimentos ricos en potasio", "Sodio (sal)", "Proteínas"], correct: 2 },
            { question: "¿Cuál es el primer paso para el diagnóstico de HAS?", answers: ["Iniciar tratamiento farmacológico.", "Confirmar las lecturas de PA elevadas en múltiples visitas.", "Realizar una tomografía cerebral.", "Medir la HbA1C."], correct: 1 },
            { question: "¿Qué es la hipertensión arterial sistólica aislada?", answers: ["PAS normal, PAD elevada.", "PAS elevada, PAD normal.", "PAS y PAD elevadas.", "Ninguna de las anteriores."], correct: 1 },
            { question: "¿Qué es un calcioantagonista (CaA)?", answers: ["Un fármaco que bloquea los canales de calcio.", "Un fármaco que reduce el colesterol.", "Un fármaco que aumenta el sodio.", "Un fármaco que diluye la sangre."], correct: 0 },
            { question: "¿Cuál es la principal causa de muerte en pacientes con HAS no controlada?", answers: ["Insuficiencia renal", "Accidente cerebrovascular", "Enfermedad coronaria", "Diabetes"], correct: 2 },
            { question: "¿Qué es el daño a órgano diana (DOD)?", answers: ["Un efecto secundario de la medicación.", "Daño a órganos vitales causado por la HAS.", "Una enfermedad genética.", "Un tipo de arritmia."], correct: 1 },
            { question: "¿Qué prueba diagnóstica se utiliza para evaluar el corazón en un paciente con HAS?", answers: ["Electrocardiograma (ECG)", "Ecocardiograma", "Ambas", "Ninguna de las anteriores"], correct: 2 },
            { question: "¿Qué tipo de diurético se usa comúnmente en la terapia de combinación para HAS?", answers: ["Diuréticos de asa", "Diuréticos tiazídicos", "Diuréticos ahorradores de potasio", "Ninguno de los anteriores."], correct: 1 },
            { question: "¿Qué es la Hipertensión Arterial Sistémica?", answers: ["Una presión arterial baja.", "Una condición crónica de PA elevada.", "Una infección del sistema circulatorio.", "Un tipo de diabetes."], correct: 1 },
            { question: "¿Cuál es un factor de riesgo para el desarrollo de HAS?", answers: ["Consumo excesivo de sal", "Actividad física regular", "Dieta rica en fibra", "Peso bajo"], correct: 0 },
            { question: "¿Qué significa la abreviatura 'AAS' en el glosario del protocolo?", answers: ["Ácido ascórbico", "Ácido acetilsalicílico", "Anticoagulante arterial sistémico", "Antagonista de la angiotensina sistémica"], correct: 1 },
            { question: "¿Qué tipo de medicación se usa para tratar la HAS en pacientes con diabetes tipo 2?", answers: ["IECA/ARA", "Diuréticos", "Betabloqueadores", "Todos los anteriores"], correct: 0 },
            { question: "¿Qué se debe hacer si un paciente presenta una crisis hipertensiva?", answers: ["Enviar a urgencias de inmediato.", "Ajustar la dosis de medicación en casa.", "Beber mucha agua.", "Hacer ejercicio intenso."], correct: 0 },
            { question: "¿Cuál es un síntoma común de la hipertensión arterial?", answers: ["Dolor de cabeza", "Mareos", "Visión borrosa", "La mayoría son asintomáticos"], correct: 3 },
            { question: "¿Qué es el 'fenómeno de la bata blanca'?", answers: ["PA normal en el hospital.", "PA elevada por la ansiedad en el consultorio médico.", "PA baja en casa.", "Ninguna de las anteriores."], correct: 1 },
            { question: "¿Cuál es la importancia de la educación del paciente en el manejo de la HAS?", answers: ["No es importante.", "Solo es importante para pacientes jóvenes.", "Es fundamental para la adherencia al tratamiento.", "Solo para pacientes sin medicación."], correct: 2 },
            { question: "¿Cuál es el principal objetivo del tratamiento de la HAS?", answers: ["Reducir el peso del paciente.", "Alcanzar una PA de 100/60 mmHg.", "Prevenir eventos cardiovasculares.", "Curar la enfermedad."], correct: 2 },
            { question: "¿Qué es la 'hipertensión enmascarada'?", answers: ["PA normal en consultorio, pero elevada en casa.", "PA elevada en consultorio, pero normal en casa.", "PA normal en todo momento.", "PA elevada solo por la noche."], correct: 0 },
            { question: "¿Cuál es una complicación común de la HAS no controlada?", answers: ["Cáncer de pulmón", "Accidente cerebrovascular isquémico", "Artritis", "Cataratas"], correct: 1 },
            { question: "¿Qué es un betabloqueador (BB)?", answers: ["Un fármaco que reduce la frecuencia cardíaca y la PA.", "Un fármaco que dilata los vasos sanguíneos.", "Un fármaco que aumenta la frecuencia cardíaca.", "Un fármaco que reduce el colesterol."], correct: 0 },
            { question: "¿Qué tipo de examen se utiliza para evaluar el daño renal en pacientes con HAS?", answers: ["Examen general de orina (EGO)", "Tomografía computarizada", "Resonancia magnética", "Radiografía de tórax"], correct: 0 },
            { question: "¿Cuál es el papel del alcohol en el desarrollo de la HAS?", answers: ["Previene la HAS.", "Aumenta el riesgo de HAS.", "No tiene efecto.", "Solo afecta a las mujeres."], correct: 1 },
            { question: "¿Qué es la Hipertensión Arterial Sistémica (HAS) de riesgo alto?", answers: ["PAC entre 130-139, PAD entre 85-89 y 3 factores de riesgo.", "PAC entre 140-159, PAD entre 90-99 y 2 factores de riesgo.", "PAC >160, PAD >100 con daño a órgano blanco.", "Cualquier PA elevada."], correct: 2 },
            { question: "¿Qué es la 'presión arterial media'?", answers: ["El promedio de PA sistólica y diastólica.", "La PA en la mitad del día.", "La PA en el consultorio.", "La PA en la mañana."], correct: 0 },
            { question: "¿Qué es una dieta baja en sodio?", answers: ["Una dieta que contiene más de 5 gramos de sal por día.", "Una dieta que limita la sal a menos de 5 gramos por día.", "Una dieta que excluye todos los alimentos.", "Una dieta rica en sodio."], correct: 1 },
            { question: "¿Qué significa la abreviatura EVC?", answers: ["Enfermedad venosa coronaria.", "Enfermedad vascular cerebral.", "Examen vascular cardíaco.", "Enfermedad viral crónica."], correct: 1 },
            { question: "¿Qué es la PA 'normal alta'?", answers: ["130-139/85-89 mmHg", "120-129/80-84 mmHg", "140-159/90-99 mmHg", "Menor de 120/80 mmHg"], correct: 1 },
            { question: "¿Qué medicamento está contraindicado en mujeres embarazadas con HAS?", answers: ["Labetalol", "Metildopa", "Losartán", "Nifedipino"], correct: 2 },
            { question: "¿Qué factor de riesgo cardiovascular NO es modificable?", answers: ["Edad", "Obesidad", "Tabaquismo", "Dieta"], correct: 0 },
            { question: "¿Qué significa la abreviatura 'IAM'?", answers: ["Insuficiencia arterial media", "Infarto agudo de miocardio", "Inflamación arterial muscular", "Índice de arteria media"], correct: 1 },
            { question: "¿Qué medida de PA se usa para diagnosticar HAS?", answers: ["Una sola medición de PA.", "Dos mediciones de PA en la misma visita.", "Promedio de 2 o más mediciones en 2 o más visitas.", "Una medición de PA por la noche."], correct: 2 },
            { question: "¿Qué es la retinopatía hipertensiva?", answers: ["Daño a la retina causado por la HAS.", "Daño a la córnea causado por la diabetes.", "Inflamación del iris.", "Presión arterial alta en el ojo."], correct: 0 }
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
        }

        // Función para reiniciar el cuestionario
        function restartQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            userAnswers = new Array(questions.length).fill(null);
            quizContainer.style.display = 'block';
            resultsSection.style.display = 'none';
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
