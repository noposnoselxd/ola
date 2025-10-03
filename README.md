<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Evaluación de Desempeño</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #1e1e1e;
      color: #f0f0f0;
      padding: 20px;
    }
    h2 {
      color: #aa8811;
    }
    input, button {
      padding: 8px;
      margin: 5px 0;
      width: 100%;
    }
    .hidden {
      display: none;
    }
    .resultados {
      margin-top: 20px;
      padding: 15px;
      background-color: #2e2e2e;
      border: 1px solid #444;
    }
  </style>
</head>
<body>

  <h2>Acceso de Empleado</h2>
  <div id="loginForm">
    <label>Usuario:</label>
    <input type="text" id="username">
    <label>Contraseña:</label>
    <input type="password" id="password">
    <button onclick="login()">Iniciar sesión</button>
    <p id="loginError" style="color: red;"></p>
  </div>

  <div id="mainApp" class="hidden">
    <h2>Evaluación de Desempeño</h2>
    <label>ID de Empleado:</label>
    <input type="text" id="matricula">

    <label>(Evaluación general 0-10):</label>
    <input type="number" id="calificacionUnica" min="0" max="10">

    <h3>Evaluaciones por competencia:</h3>
    <div id="calificacionesInputs">
      <!-- Se generan dinámicamente -->
    </div>

    <button onclick="calcular()">Calcular</button>

    <div class="resultados hidden" id="resultados">
      <h3>Resultados:</h3>
      <p id="resultadoCalificacion"></p>
      <p id="promedio"></p>
      <p id="reprobadas"></p>
    </div>
  </div>

  <script>
    const correctUsername = "Josue17";
    const correctPassword = "161223";
    let intentos = 0;
    const maxIntentos = 3;

    const competencias = [
      "Comunicación efectiva",
      "Resolución de problemas",
      "Trabajo en equipo",
      "Gestión del tiempo",
      "Dominio tecnológico",
      "Liderazgo",
      "Orientación a resultados",
      "Adaptabilidad al cambio"
    ];

    function login() {
      const user = document.getElementById("username").value;
      const pass = document.getElementById("password").value;

      if (user === correctUsername && pass === correctPassword) {
        document.getElementById("loginForm").classList.add("hidden");
        document.getElementById("mainApp").classList.remove("hidden");
        generarInputs();
      } else {
        intentos++;
        document.getElementById("loginError").textContent = `Intento ${intentos} de ${maxIntentos}: Credenciales incorrectas.`;
        if (intentos >= maxIntentos) {
          alert("Acceso denegado. Se han agotado los intentos.");
          document.getElementById("username").disabled = true;
          document.getElementById("password").disabled = true;
        }
      }
    }

    function generarInputs() {
      const container = document.getElementById("calificacionesInputs");
      competencias.forEach((competencia, index) => {
        const label = document.createElement("label");
        label.textContent = competencia + ":";
        const input = document.createElement("input");
        input.type = "number";
        input.min = 0;
        input.max = 10;
        input.id = `competencia${index}`;
        input.placeholder = "Evaluación (0-10)";
        container.appendChild(label);
        container.appendChild(input);
      });
    }

    function evaluarDesempeño(calif) {
      if (calif < 6) return "Desempeño Insuficiente";
      if (calif == 6) return "Nivel I (Insuficiente)";
      if (calif == 7 || calif == 8) return "Nivel S (Satisfactorio)";
      if (calif == 9) return "Nivel B (Bueno)";
      if (calif == 10) return "Nivel E (Excelente)";
      return "Valor inválido";
    }

    function calcular() {
      const califUnica = parseFloat(document.getElementById("calificacionUnica").value);
      const resultadoCalif = evaluarDesempeño(califUnica);
      const empleadoID = document.getElementById("matricula").value;

      let evaluaciones = [];

      for (let i = 0; i < competencias.length; i++) {
        const val = parseFloat(document.getElementById(`competencia${i}`).value);
        if (!isNaN(val) && val >= 0 && val <= 10) {
          evaluaciones.push(val);
        } else {
          alert(`La evaluación en "${competencias[i]}" no es válida.`);
          return;
        }
      }

      const promedio = evaluaciones.reduce((a, b) => a + b, 0) / evaluaciones.length;
      const bajoRendimiento = evaluaciones.filter(c => c < 6).length;

      document.getElementById("resultadoCalificacion").textContent = `Resultado general: ${resultadoCalif}`;
      document.getElementById("promedio").textContent = `Desempeño promedio: ${promedio.toFixed(2)}`;
      document.getElementById("reprobadas").textContent = `Áreas a mejorar: ${bajoRendimiento}`;

      document.getElementById("resultados").classList.remove("hidden");
    }
  </script>

</body>
</html>
