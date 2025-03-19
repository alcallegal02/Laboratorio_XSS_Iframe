# Laboratorio sobre XSS con iFrames

## Objetivo
El objetivo de este laboratorio es entender cómo los iframes pueden manipular el DOM de la página principal, identificar vulnerabilidades y aplicar medidas de seguridad para proteger la página.

---

## Parte 1: Preparando el entorno

### Paso 1: Crea la página principal
Se creó un archivo `index.html` que incluye un título, un mensaje en un párrafo y un iframe que carga `desconocido.html`.

### Paso 2: Crea la página desconocido.html
Se creó un archivo `desconocido.html` que intenta modificar el DOM de la página principal (`index.html`) usando JavaScript. Si la modificación falla, se muestra un mensaje de error en la consola.

---

## Parte 2: Descubre la vulnerabilidad

### Preguntas y respuestas

1. **¿El iframe pudo modificar el DOM de la página principal? Explica tu respuesta.**
   - **Respuesta:** Sí, el iframe pudo modificar el DOM de la página principal. Esto ocurre porque el iframe y la página principal comparten el mismo origen, lo que permite que el iframe acceda y modifique el DOM de la página principal.

2. **Si hubo errores en la consola, ¿qué dicen?**
   - **Respuesta:** No hubo errores en la consola porque el iframe y la página principal comparten el mismo origen, permitiendo la modificación del DOM sin problemas.

3. **Prueba ejecutar en la consola:**
   ```javascript
   document.getElementById("iframePrueba").contentWindow.document.body.innerHTML;
  ´´´
  
   **¿Qué ocurre?**
- **Respuesta:** Se obtiene el contenido HTML del cuerpo del iframe. Esto es posible porque el iframe y la página principal comparten el mismo origen.

---

**Si hay error, ¿por qué crees que sucede?**
- **Respuesta:** No hubo error porque el iframe y la página principal comparten el mismo origen, lo que permite el acceso al contenido del iframe.

---

**Prueba modificar el iframe desde la página principal con este código:**
```javascript
document.getElementById("iframePrueba").contentWindow.document.body.style.backgroundColor = "blue";
```

**¿Funcionó?**

- **Respuesta:** Sí, funcionó. El fondo del iframe cambió a azul porque la página principal tiene acceso al DOM del iframe al compartir el mismo origen.

---

## Parte 3: Protege tu sitio

### Medidas de seguridad implementadas

1. **Evita que iframes externos accedan a tu DOM:**
   - Se agregó la cabecera `Content-Security-Policy: frame-ancestors 'self';` en `index.html` para restringir que solo los iframes del mismo origen puedan incrustarse en la página.

2. **Bloquea iframes completamente:**
   - Se agregó la cabecera `X-Frame-Options: DENY` en `index.html` para evitar que la página sea incrustada en iframes de cualquier origen.

3. **Usa `window.postMessage()` para comunicarte de forma segura:**
   - En `desconocido.html`, se reemplazó el código que intentaba modificar el DOM directamente por el uso de `window.parent.postMessage()` para enviar mensajes de forma segura desde el iframe a la página principal.

---

## Reflexión final

### Preguntas y respuestas

1. **¿Cómo pueden los iframes ser usados en ataques como clickjacking?**
   - **Respuesta:** Los iframes pueden ser usados en ataques de clickjacking al incrustar una página maliciosa en un iframe transparente sobre otra página legítima. Esto engaña al usuario para que haga clic en elementos de la página legítima sin darse cuenta, permitiendo que el atacante realice acciones no deseadas.

2. **¿Por qué es importante la Same-Origin Policy (SOP)?**
   - **Respuesta:** La Same-Origin Policy (SOP) es importante porque restringe cómo un documento o script cargado desde un origen puede interactuar con recursos de otro origen. Esto previene que scripts maliciosos accedan a datos sensibles o manipulen el DOM de otras páginas, protegiendo la seguridad y privacidad del usuario.

3. **¿Cómo `window.postMessage()` puede ayudar a hacer intercambios de datos entre sitios seguros?**
   - **Respuesta:** `window.postMessage()` permite la comunicación segura entre ventanas o iframes de diferentes orígenes al enviar mensajes que solo pueden ser recibidos por el destinatario específico. Esto evita que terceros intercepten o manipulen los datos, asegurando que la comunicación sea segura y controlada.
