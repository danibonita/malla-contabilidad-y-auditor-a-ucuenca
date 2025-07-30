<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Malla Interactiva - Contabilidad</title>
  <style>
    body { font-family: sans-serif; margin: 2em; }
    .materia { margin-bottom: 8px; }
    .habilitada { color: green; font-weight: bold; }
    .bloqueada { color: gray; }
  </style>
</head>
<body>
  <h1>Malla Interactiva - Contabilidad y Auditoría</h1>
  <p>Selecciona las materias que ya aprobaste:</p>
  <div id="materias-aprobadas"></div>
  <h2>Materias que puedes tomar:</h2>
  <ul id="disponibles"></ul>

  <script>
    const materias = [
      { id: "cont_gen", nombre: "Contabilidad General", prereqs: [] },
      { id: "calc_diff", nombre: "Cálculo Diferencial", prereqs: [] },
      { id: "leg_merc", nombre: "Legislación Mercantil y Societaria", prereqs: [] },
      { id: "cont_com", nombre: "Contabilidad Comercial", prereqs: ["cont_gen"] },
      { id: "calc_int", nombre: "Cálculo Integral", prereqs: ["calc_diff"] },
      { id: "leg_trib", nombre: "Legislación Tributaria", prereqs: ["leg_merc"] },
      { id: "costos_bas", nombre: "Contabilidad Costos Básica", prereqs: ["cont_com"] },
      { id: "financiera", nombre: "Contabilidad Financiera", prereqs: ["cont_com"] },
      { id: "costos_adv", nombre: "Contabilidad Costos Avanzada", prereqs: ["costos_bas"] },
      { id: "control_interno", nombre: "Control Interno y Gestión de Riesgos", prereqs: ["costos_bas"] },
      { id: "auditoria_gestion", nombre: "Auditoría de Gestión", prereqs: ["control_interno"] }
      // Puedes seguir agregando más materias aquí...
    ];

    const contenedor = document.getElementById("materias-aprobadas");
    const lista = document.getElementById("disponibles");

    materias.forEach(m => {
      const div = document.createElement("div");
      div.className = "materia";
      div.innerHTML = `<input type="checkbox" id="${m.id}"> <label for="${m.id}">${m.nombre}</label>`;
      contenedor.appendChild(div);
    });

    contenedor.addEventListener("change", actualizar);

    function actualizar() {
      const aprobadas = materias.filter(m => document.getElementById(m.id).checked).map(m => m.id);
      lista.innerHTML = "";
      materias.forEach(m => {
        if (aprobadas.includes(m.id)) return;
        const puedeTomar = m.prereqs.every(req => aprobadas.includes(req));
        const li = document.createElement("li");
        li.textContent = m.nombre;
        li.className = puedeTomar ? "habilitada" : "bloqueada";
        lista.appendChild(li);
      });
    }
  </script>
</body>
</html>
