# Chatbot Informática – UNSAAC

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Dayan77uu/chatboot/blob/main/Chatbot_Informatica.ipynb)

Un asistente virtual que responde las preguntas más frecuentes de estudiantes y docentes de la Escuela Profesional de Ingeniería Informática y de Sistemas (EPIIS) de la UNSAAC.

Proyecto del curso **IF651 – Inteligencia Artificial**.

Repositorio: https://github.com/Dayan77uu/chatboot

---

## ¿Qué problema resuelve?

Cada semestre, la Escuela recibe muchas veces las mismas preguntas: qué cursos llevar, cómo hacer un trámite, cuándo son las tutorías, qué becas hay, cómo titularse, etc. Responderlas una por una toma tiempo tanto a los estudiantes como al personal de la Escuela.

Este chatbot busca resolver eso: el estudiante escribe su pregunta en lenguaje natural (como si le escribiera a una persona) y el sistema responde automáticamente, con información real y actualizada de la Escuela.

## ¿Qué puede responder?

El chatbot cubre 15 temas distintos:

- Información general (misión, visión, historia, autoridades, acreditación)
- Docentes (quiénes son, dedicación, especialidad)
- Cursos (por semestre, por área, créditos, prerrequisitos)
- Horarios de clases
- Trámites administrativos (matrícula, certificados, constancias, traslados, convalidaciones — con costos y plazos oficiales según el TUPA de la universidad)
- Tutorías (cómo funcionan, tipos, cómo cambiar de tutor)
- Prácticas preprofesionales
- Temas de tesis e investigación
- Titulación (modalidades, requisitos, costos)
- Información para ingresantes
- Servicios universitarios (salud, comedor, becas, deportes, movilidad, convenios)
- Datos de contacto
- Saludos y despedidas

Además, puede evaluar si un estudiante cumple los requisitos para:

- Iniciar sus prácticas preprofesionales
- Tramitar el bachillerato
- Saber qué modalidad de titulación le corresponde
- Saber cuántos créditos puede matricular según su situación académica

Para estos casos especiales, el chatbot no solo busca un dato, sino que **razona** combinando varias condiciones a la vez (por ejemplo: para el bachillerato hay que cumplir cinco requisitos distintos al mismo tiempo, no solo uno). El sistema explica además *por qué* llegó a esa conclusión, paso por paso.

## ¿Cómo funciona, en simple?

1. El estudiante escribe una pregunta.
2. El sistema "limpia" el texto (minúsculas, sin tildes, sin signos de puntuación).
3. Convierte la pregunta en una representación numérica que un modelo de aprendizaje automático puede entender.
4. Un modelo entrenado (Random Forest) adivina de qué tema trata la pregunta y qué tan seguro está de su respuesta.
5. Si no está seguro, le pide al estudiante que reformule la pregunta en vez de arriesgarse a contestar mal.
6. Si está seguro, busca la respuesta en una base de datos con la información oficial de la Escuela (cursos, docentes, trámites, etc.), o —si la pregunta requiere combinar varios datos a la vez— usa un módulo especial de razonamiento.
7. Devuelve la respuesta al estudiante.

## ¿Con qué está construido?

- **Un banco de preguntas de ejemplo** (corpus): 1,931 preguntas reales organizadas en 15 temas, que sirven para "enseñarle" al sistema a reconocer de qué está hablando el usuario.
- **Un modelo de clasificación** que aprende de esas preguntas para identificar el tema de cualquier consulta nueva.
- **Una base de datos** con la información real de la Escuela: cursos, docentes, horarios, trámites (con costos y plazos oficiales), becas, servicios, etc.
- **Un módulo de razonamiento** que combina varios datos para responder preguntas más complejas, como la elegibilidad para trámites académicos.

Todo el proyecto está organizado en un único cuaderno de **Google Colab**, programado en Python siguiendo el paradigma de **Programación Orientada a Objetos** (cada tema tiene su propia clase, ordenada y fácil de mantener).

## ¿Qué tan bien funciona?

En las pruebas realizadas, el sistema acierta el tema de la pregunta en aproximadamente el **83% de los casos**. Se probó con preguntas representativas de cada uno de los 15 temas, incluyendo los casos de razonamiento más complejos (prácticas, bachillerato, titulación y regularidad académica), y la gran mayoría de las respuestas fueron correctas.

## ¿Cómo se usa?

1. Abrir el archivo `.ipynb` en Google Colab (puedes usar el botón "Abrir en Colab" de arriba).
2. Ejecutar todas las celdas en orden (de arriba hacia abajo).
3. Al llegar a la última celda, el chatbot queda disponible para conversar: el estudiante escribe su pregunta y el sistema responde. Para terminar la conversación, basta con escribir "salir".

## Algunas preguntas de ejemplo

- "¿Cuál es la misión de la Escuela?"
- "¿qué cursos hay en el sexto semestre?"
- "¿cómo homologo una asignatura con nombre distinto?"
- "tengo 175 créditos y seguro vigente, ¿puedo iniciar prácticas?"
- "ingresé en 2013, tengo bachiller y 4 años de experiencia, ¿qué modalidad de titulación tengo?"

## Equipo

Proyecto desarrollado por CASTILLA VARGAS, Dayana Angela 210409 - SANCHEZ CHACON, Elbert Cesar 200341- HALANOCCA SURCO, Jhon Kevin 215277 - CABEZA HUILLCA, Flavio Antony 211265, como parte del curso de Inteligencia Artificial.

## Notas de desarrollo

- Se detectó y corrigió un error en la versión inicial del notebook: la clase `GestorPracticas` estaba referenciada en el constructor de `EscuelaBot` pero nunca había sido definida (su lógica había sido absorbida por `GestorElegibilidad` en esta versión del proyecto). Se corrigió haciendo que `self.practicas` apunte a la instancia de `GestorElegibilidad`, que es el módulo que realmente resuelve esa intención.
- Se hicieron pruebas adicionales para comprobar el mecanismo de "pedir reformular la pregunta" cuando el modelo no está seguro de la respuesta. En la práctica, este modelo entrenado rara vez baja de ese límite de confianza, incluso con preguntas sin sentido o totalmente ajenas al tema de la Escuela: la confianza mínima encontrada en las pruebas fue de 0.38, por encima del límite configurado (0.20). Queda como una observación a considerar para una futura mejora del sistema.

## Limitaciones conocidas

- El chatbot solo responde con información que tiene cargada en su base de datos; no inventa datos cuando no está seguro, y en esos casos recomienda consultar directamente con la Escuela.
- Algunas preguntas muy parecidas entre sí (por ejemplo, distintos tipos de trámites de matrícula) a veces se confunden entre ellas, ya que comparten muchas palabras similares.
- El sistema está pensado como un prototipo académico, no como un servicio oficial de la universidad.
