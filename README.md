# 🤖📞🔍 Mi Primer API Request Usando Postman

Actividad práctica con Postman

## 🎯 **Aprendizaje esperado**

Al finalizar esta actividad, el estudiante será capaz de:

1. **Comprender los conceptos fundamentales de una API**, solicitudes HTTP, endpoints y respuestas JSON.
2. **Entender cómo se establece la comunicación entre cliente y servidor** mediante el uso de métodos HTTP.
3. **Ejecutar solicitudes GET en Postman** e interpretar adecuadamente la información recibida.
4. **Configurar una solicitud con un *Pre-request Script*** para realizar múltiples peticiones internas de forma programada.
5. **Generar visualizaciones dinámicas** utilizando Postman Visualizer para sintetizar información.
6. **Comparar datos provenientes de distintas fuentes** de manera automatizada y estructurada.
7. **Analizar los resultados para tomar decisiones informadas**, en este caso, determinar el precio más conveniente entre diferentes supermercados.

---

## 📘 1. Conceptos fundamentales antes de comenzar

Antes de interactuar con Postman, es importante comprender las piezas clave que intervienen en la comunicación entre sistemas:

### ✅ **¿Qué es una API?**

Una **API (Application Programming Interface)** es un conjunto de reglas que permite que dos sistemas se comuniquen entre sí.
A través de una API, un cliente (por ejemplo, Postman) puede solicitar datos o ejecutar funciones en un servidor.
En esta práctica, trataremos cada supermercado como un “servidor” que expone información a través de una API.

---

### ✅ **¿Qué es un Endpoint?**

Un **endpoint** es una dirección específica (URL) dentro de una API a la que el cliente puede enviar solicitudes.
Cada endpoint sirve para obtener o manipular un recurso concreto.
En esta actividad:

* `/superA` devuelve el precio del producto en Super A
* `/superB` devuelve el precio del producto en Super B
* `/superC` devuelve el precio del producto en Super C

Un endpoint es, en esencia, **la puerta de entrada a un dato concreto**.

---

### ✅ **¿Qué es HTTP?**

**HTTP (Hypertext Transfer Protocol)** es el protocolo que utilizan los navegadores y las aplicaciones para comunicarse con servidores en internet.
Permite enviar solicitudes (requests) y recibir respuestas (responses).
Los métodos más comunes son:

* **GET** → solicitar información
* **POST** → enviar datos
* **PUT / PATCH** → actualizar información
* **DELETE** → eliminar información

En esta práctica únicamente utilizaremos **GET**.

---

### ✅ **¿Qué es JSON?**

**JSON (JavaScript Object Notation)** es un formato estándar para intercambiar datos entre sistemas.
Se caracteriza por ser:

* Ligero
* Legible por humanos
* Fácil de interpretar por máquinas

Ejemplo utilizado en esta actividad:

```json
{
  "store": "Super A",
  "product": "Roles de canela (6 pzas)",
  "price": 54.90
}
```

Postman mostrará todas las respuestas en este formato.

---

### ✅ 1. Contexto de la actividad

Para simular una situación real, utilizaremos **tres APIs** que representan diferentes supermercados.
Cada una devuelve el precio de un mismo producto:

✅ **Roles de canela (6 piezas)**

Tu tarea será consultar cada endpoint y luego comparar automáticamente los tres precios utilizando un *Pre-request Script* y el **Visualizer** de Postman.

---

## 🛠️ 2. Abrir Postman

1. Accede a **[https://web.postman.co](https://web.postman.co)** o abre la app de escritorio.
2. Inicia sesión en tu Workspace personal.

---

## 📁 3. Crear una colección (opcional pero recomendado)

1. En la barra izquierda selecciona **Collections**.
2. Clic en **New Collection**.
3. Asigna un nombre:

**Comparación de precios — Roles de canela**

---

## 🧾 4. Crear las solicitudes de cada supermercado

Configura una solicitud **GET** para cada supermercado dentro de la colección:

### ✅ Super A

`https://937b5598-8007-477d-aa9a-eb93e2d20af1.mock.pstmn.io/superA`

### ✅ Super B

`https://937b5598-8007-477d-aa9a-eb93e2d20af1.mock.pstmn.io/superB`

### ✅ Super C

`https://937b5598-8007-477d-aa9a-eb93e2d20af1.mock.pstmn.io/superC`

Nombres sugeridos:

* `Consulta Super A`
* `Consulta Super B`
* `Consulta Super C`

---

## 🔍 5. Ejecutar cada solicitud

1. Selecciona cada solicitud.
2. Clic en **Send**.
3. Verifica que la respuesta sea similar a:

```json
{
  "store": "Super A",
  "product": "Roles de canela (6 pzas)",
  "price": 54.90
}
```

Repite para las tres.

---

## 📊 6. Crear la solicitud de comparación

1. Crear un nuevo request en la colección llamado:
   **Comparación de precios**
2. Colocar cualquier URL válida (solo para enviar), por ejemplo:
   `https://postman-echo.com/get`

---

## 🧩 7. Agregar el Script (solo en *Pre-request Script*)

En la solicitud **Comparación de precios**:

✅ Abre **Pre-request Script**
✅ Pega:

```
// Enhanced comparative price visualization for “Roles de canela (6 pzas)”
// Features: custom bar colors, rounded corners, value labels, subtitle, larger fonts, drop shadow, animation, robust error handling.

const endpoints = [
  {
    label: "Super A",
    url: "Insertar aquí la url del endpoint del super A"
  },
  {
    label: "Super B",
    url: "Insertar aquí la url del endpoint del super B"
  },
  {
    label: "Super C",
    url: "Insertar aquí la url del endpoint del super C"
  }
];

const barColors = ["#43a047", "#fb8c00", "#1e88e5"];
const borderColors = ["#388e3c", "#ef6c00", "#1565c0"];
const chartTitle = "Comparación de precios";
const chartSubtitle = "Roles de canela (6 pzas)";
const fontSize = 18;
const subtitleFontSize = 15;
const valueFontSize = 16;
const shadowColor = "rgba(40,40,40,0.10)";

function fetchAllPrices() {
  return Promise.all(
    endpoints.map((ep) =>
      new Promise((resolve) => {
        pm.sendRequest({ url: ep.url, method: "GET" }, (err, res) => {
          let price = 0;
          let note = null;
          if (err || !res || !res.json) {
            note = `No se pudo obtener precio de ${ep.label}`;
          } else {
            try {
              const data = res.json();
              price = Number(data.price) || 0;
              if (!data.price) note = `Precio faltante en ${ep.label}`;
            } catch (e) {
              note = `Respuesta inválida de ${ep.label}`;
            }
          }
          resolve({ label: ep.label, price, note });
        });
      })
    )
  );
}

fetchAllPrices().then((results) => {
  const labels = results.map((r) => r.label);
  const prices = results.map((r) => r.price);
  const notes = results.filter((r) => r.note).map((r) => r.note);

  const template = `
  <div style="max-width:540px;margin:0 auto;">
    <div style="background:#fff;border-radius:18px;box-shadow:0 6px 24px ${shadowColor};padding:28px 18px 18px 18px;">
      <div style="text-align:center;margin-bottom:2px;">
        <span style="font-size:${fontSize + 2}px;font-weight:700;">${chartTitle}</span><br>
        <span style="font-size:${subtitleFontSize}px;color:#666;">${chartSubtitle}</span>
      </div>
      <canvas id="priceChart" height="110"></canvas>
      <div id="footnote" style="font-size:13px;color:#888;margin-top:10px;text-align:center;"></div>
    </div>
  </div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
  <script>
  (function() {
    pm.getData(function(err, data) {
      var ctx = document.getElementById('priceChart').getContext('2d');
      // Custom plugin for rounded bars and value labels
      const roundedBar = {
        id: 'roundedBar',
        afterDatasetsDraw(chart) {
          const {ctx, chartArea, data: chartData, scales} = chart;
          chart.getDatasetMeta(0).data.forEach((bar, i) => {
            ctx.save();
            ctx.font = 'bold ${valueFontSize}px sans-serif';
            ctx.textAlign = 'center';
            ctx.fillStyle = '#222';
            const val = data.prices[i];
            const y = bar.y - 10;
            ctx.fillText(val === 0 ? 'N/A' : ('$' + val.toFixed(2) + ' MXN'), bar.x, y);
            ctx.restore();
          });
        }
      };
      Chart.register(roundedBar);
      var chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: data.labels,
          datasets: [{
            label: 'Precio (MXN)',
            data: data.prices,
            backgroundColor: data.colors.bar,
            borderColor: data.colors.border,
            borderWidth: 2,
            borderRadius: 14,
            maxBarThickness: 60,
            minBarLength: 2,
            hoverBackgroundColor: data.colors.bar,
            hoverBorderColor: data.colors.border
          }]
        },
        options: {
          responsive: true,
          animation: {
            duration: 1100,
            easing: 'easeOutQuart'
          },
          plugins: {
            legend: { display: false },
            title: { display: false },
            tooltip: {
              callbacks: {
                label: function(context) {
                  return context.parsed.y === 0 ? 'N/A' : ('$' + context.parsed.y.toFixed(2) + ' MXN');
                }
              },
              backgroundColor: '#fff',
              titleColor: '#222',
              bodyColor: '#222',
              borderColor: '#bbb',
              borderWidth: 1,
              bodyFont: { size: ${fontSize} },
              titleFont: { size: ${fontSize + 1} }
            }
          },
          scales: {
            y: {
              beginAtZero: true,
              ticks: {
                font: { size: ${fontSize} },
                color: '#444',
                callback: function(value) { return '$' + value; }
              },
              title: {
                display: true,
                text: 'Precio (MXN)',
                font: { size: ${fontSize + 1}, weight: 'bold' },
                color: '#222'
              },
              grid: { color: '#eee' }
            },
            x: {
              ticks: {
                font: { size: ${fontSize} },
                color: '#444'
              },
              title: {
                display: true,
                text: 'Supermercado',
                font: { size: ${fontSize + 1}, weight: 'bold' },
                color: '#222'
              },
              grid: { display: false }
            }
          }
        },
        plugins: ['roundedBar']
      });
      if (data.footnote) {
        document.getElementById('footnote').innerText = data.footnote;
      }
    });
  })();
  </script>
  `;

  function constructVisualizerPayload() {
    return {
      labels,
      prices,
      colors: {
        bar: barColors,
        border: borderColors
      },
      title: chartTitle,
      subtitle: chartSubtitle,
      footnote: notes.length ? notes.join(". ") : null
    };
  }

  pm.visualizer.set(template, constructVisualizerPayload());
});
```

Guardar.

---

## 📈 8. Ejecutar y visualizar

1. Abrir **Comparación de precios**.
2. Hacer clic en **Send**.
3. Abrir la pestaña **Visualizer**.

Deberá aparecer una tabla con:

✅ Super A, Super B, Super C
✅ Precios
✅ Fila más económica resaltada en verde
✅ Mensaje indicando el supermercado ganador

---

## ✅ 9. Evidencia a entregar (solo 1 elemento)

El alumno únicamente debe **subir al repositorio**:

### 📌 **1 imagen del Visualizer con su tabla comparativa**

(nombre sugerido: `evidencia_tunombre.png`)
